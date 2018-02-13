---
title: "Bağlantısı kesilmiş varlıkları - EF çekirdek"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0b145217d40027c4b8e4746e9c5651652a28c9eb
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2018
---
# <a name="disconnected-entities"></a><span data-ttu-id="d618f-102">Bağlantısı kesilmiş varlıklar</span><span class="sxs-lookup"><span data-stu-id="d618f-102">Disconnected entities</span></span>

<span data-ttu-id="d618f-103">DbContext örnek veritabanı tarafından döndürülen varlıkları otomatik olarak izler.</span><span class="sxs-lookup"><span data-stu-id="d618f-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="d618f-104">Bu varlıklar için yapılan değişiklikler sonra SaveChanges olarak adlandırılır ve veritabanı gerektiğinde güncelleştirileceği zaman algılanacak.</span><span class="sxs-lookup"><span data-stu-id="d618f-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="d618f-105">Bkz: [temel Kaydet](basic.md) ve [ilgili verileri](related-data.md) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="d618f-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="d618f-106">Ancak, bazen varlıklar bir bağlam örneğini kullanarak ve farklı bir örneği kullanılarak kaydedilmiş seçmeleri istenir.</span><span class="sxs-lookup"><span data-stu-id="d618f-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="d618f-107">Bu genellikle "bağlantısız" senaryolarda burada varlıkları sorgulanan, istemciye gönderilen, değişiklik, bir sunucuya geri gönderilen ve sonra kaydedilmiş bir web uygulaması gibi gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="d618f-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="d618f-108">Bu durumda, ikinci bağlam (güncelleştirilmelidir) mevcut veya yeni varlıklardır olup olmadığını bilmek (eklenecek) gereksinimlerini örneği.</span><span class="sxs-lookup"><span data-stu-id="d618f-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="d618f-109">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) github'da.</span><span class="sxs-lookup"><span data-stu-id="d618f-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="d618f-110">EF çekirdek yalnızca bir örneğini belirtilen birincil anahtar değerine sahip herhangi bir varlığa izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d618f-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="d618f-111">Bu bağlamda boş başlatır gibi kısa süreli bir bağlam her--iş birimi için kullanmak üzere bir sorun olduğu durumda önlemek için en iyi yolu ekli varlıklar bu varlıkların ve bağlam atıldı ve atılan kaydeder sahiptir.</span><span class="sxs-lookup"><span data-stu-id="d618f-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="d618f-112">Yeni varlıklar tanımlama</span><span class="sxs-lookup"><span data-stu-id="d618f-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="d618f-113">İstemci yeni varlıklar tanımlar</span><span class="sxs-lookup"><span data-stu-id="d618f-113">Client identifies new entities</span></span>

<span data-ttu-id="d618f-114">İstemci sunucuya varlık yeni veya mevcut olup olmadığını bildirir uğraşmanız basit durumdur.</span><span class="sxs-lookup"><span data-stu-id="d618f-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="d618f-115">Örneğin, genellikle yeni bir varlık eklemek için var olan bir varlığı güncelleştirmek için istekten farklı isteğidir.</span><span class="sxs-lookup"><span data-stu-id="d618f-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="d618f-116">Bu bölüm geri kalanı durumları kapsayan Burada, diğer herhangi bir yolla eklemek veya güncelleştirmek karar vermek gerekli.</span><span class="sxs-lookup"><span data-stu-id="d618f-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="d618f-117">Otomatik olarak oluşturulan anahtarlarla</span><span class="sxs-lookup"><span data-stu-id="d618f-117">With auto-generated keys</span></span>

<span data-ttu-id="d618f-118">Otomatik olarak oluşturulan bir anahtarın değeri, genellikle bir varlık eklenmesi veya güncelleştirilmesi gerekip gerekmediğini belirlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d618f-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="d618f-119">Anahtar olmayan varsa varlık yeni olmalıdır ve gereken ekleme (yani hala bulunduğu CLR varsayılan değeri null, sıfır, vb.), ayarlanmış bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="d618f-119">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="d618f-120">Diğer taraftan, anahtar değeri ayarlarsanız sonra zaten daha önce kaydedilmiş gerekir ve şimdi güncelleştirilmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="d618f-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="d618f-121">Diğer bir deyişle, anahtar değeri varsa, ardından varlık, istemciye gönderilen sorgulandı ve şimdi güncelleştirilmesi döndürülmesini.</span><span class="sxs-lookup"><span data-stu-id="d618f-121">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="d618f-122">Varlık türü bilinen bir unset anahtarı için denetimi kolaydır:</span><span class="sxs-lookup"><span data-stu-id="d618f-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="d618f-123">Ancak, EF ayrıca herhangi bir varlık türü ve anahtar türü için bunu yapmanın yerleşik bir yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="d618f-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="d618f-124">Varlık bağlamı tarafından izlenen hemen varlık Added durumunda olsa bile anahtarları ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="d618f-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="d618f-125">Bu varlıkları ve her biri, bu tür olduğu gibi TrackGraph API'si kullanılırken yapmanız gerekenler karar grafiği çapraz geçiş yapan zaman yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="d618f-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="d618f-126">Anahtar değeri yalnızca aşağıda gösterildiği şekilde kullanılmalıdır _önce_ varlık izlemek için bir çağrı yapılır.</span><span class="sxs-lookup"><span data-stu-id="d618f-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="d618f-127">Diğer anahtarları</span><span class="sxs-lookup"><span data-stu-id="d618f-127">With other keys</span></span>

<span data-ttu-id="d618f-128">Başka bir mekanizma anahtarı değerlerini otomatik olarak oluşturulmaz yeni varlıklar belirlemek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d618f-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="d618f-129">Bu iki genel yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="d618f-129">There are two general approaches to this:</span></span>
 * <span data-ttu-id="d618f-130">Varlık için sorgu</span><span class="sxs-lookup"><span data-stu-id="d618f-130">Query for the entity</span></span>
 * <span data-ttu-id="d618f-131">İstemciden bir bayrak geçirin</span><span class="sxs-lookup"><span data-stu-id="d618f-131">Pass a flag from the client</span></span>

<span data-ttu-id="d618f-132">Varlık için sorgu için hemen Find yöntemi kullanın:</span><span class="sxs-lookup"><span data-stu-id="d618f-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="d618f-133">Bir istemciden bir bayrak geçirme için tam kod göstermek için bu belgenin kapsamı dışındadır.</span><span class="sxs-lookup"><span data-stu-id="d618f-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="d618f-134">Bir web uygulamasını, genellikle farklı eylemler için farklı istekleri yapan veya istek bazı durumda geçirme ardından denetleyicide ayıklanıyor anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="d618f-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="d618f-135">Tek varlıkları kaydetme</span><span class="sxs-lookup"><span data-stu-id="d618f-135">Saving single entities</span></span>

<span data-ttu-id="d618f-136">Bir INSERT veya update gereklidir ve sonra uygun şekilde ekleme veya güncelleştirme kullanılabilir olup olmadığını bilinen ise:</span><span class="sxs-lookup"><span data-stu-id="d618f-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="d618f-137">Bununla birlikte, varlık otomatik olarak oluşturulan anahtar değerlerini kullanıyorsa, güncelleştirme yöntemi için her iki durumda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d618f-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="d618f-138">Update yöntemi güncelleştirmesi değil Ekle varlık normal olarak işaretler.</span><span class="sxs-lookup"><span data-stu-id="d618f-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="d618f-139">Bununla birlikte, varlık otomatik olarak oluşturulan bir anahtar varsa ve varlık bunun yerine otomatik olarak için işaretlenmiş sonra herhangi bir anahtar değer ayarlandı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d618f-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="d618f-140">Bu davranış EF çekirdek 2. 0'sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="d618f-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="d618f-141">Önceki sürümleri için her zaman açık olarak ekleme veya güncelleştirme seçmek gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d618f-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="d618f-142">Varlık anahtarları otomatik olarak oluşturulan kullanmayan sonra uygulama varlık eklenen veya güncelleştirilen karar vermeniz gerekir: Örneğin:</span><span class="sxs-lookup"><span data-stu-id="d618f-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="d618f-143">Burada adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="d618f-143">The steps here are:</span></span>
* <span data-ttu-id="d618f-144">Bul döndürür diyoruz şekilde veritabanı zaten bu Kimliğe sahip blog içermiyor. ardından null eklerseniz, ekleme için işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="d618f-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="d618f-145">Bul bir varlık döndürürse, veritabanında var ve bağlam şimdi varolan varlık izleme</span><span class="sxs-lookup"><span data-stu-id="d618f-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="d618f-146">Ardından SetValues istemciden gelen değişiklikleri bu varlık üzerinde tüm özelliklerinin değerlerini ayarlamak için kullanırız.</span><span class="sxs-lookup"><span data-stu-id="d618f-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="d618f-147">SetValues çağrısı gerektiğinde güncelleştirilecek varlık işaretler.</span><span class="sxs-lookup"><span data-stu-id="d618f-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="d618f-148">Yalnızca SetValues izlenen varlık de için farklı değerlere sahip özellikleri değiştirilemez olarak işaretlenmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="d618f-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="d618f-149">Bu güncelleştirme gönderildiğinde, aslında değişmiş olan sütunları güncelleştirilecek anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="d618f-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="d618f-150">(Ve hiçbir şey değiştiyse, ardından güncelleştirme hiç gönderilecek.)</span><span class="sxs-lookup"><span data-stu-id="d618f-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="d618f-151">Grafikleri ile çalışma</span><span class="sxs-lookup"><span data-stu-id="d618f-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="d618f-152">Kimlik çözümleme</span><span class="sxs-lookup"><span data-stu-id="d618f-152">Identity resolution</span></span>

<span data-ttu-id="d618f-153">Yukarıda belirtildiği gibi EF çekirdek yalnızca bir örneğini belirtilen birincil anahtar değerine sahip herhangi bir varlığa izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d618f-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="d618f-154">Grafiklerle çalışırken grafiği ideal olarak bu değişmeyen saklanır ve bağlam yalnızca bir birim çalışma için kullanılması gereken şekilde oluşturulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d618f-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="d618f-155">Ardından grafiği çoğaltmaları içeriyorsa, bu grafiği tek birden çok örneği birleştirilecek EF göndermeden işlemek gerekli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="d618f-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="d618f-156">Bu yani sağlamlaştırmak çoğaltmaları mümkün olan en kısa sürede çakışma çözümü önlemek için uygulama ardışık düzeninizde yapılmalıdır örnekleri çakışan değerler ve ilişkileri, sahip olduğu Önemsiz olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="d618f-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="d618f-157">Tüm yeni/tüm var olan varlıkları</span><span class="sxs-lookup"><span data-stu-id="d618f-157">All new/all existing entities</span></span>

<span data-ttu-id="d618f-158">Grafikleri ile çalışmaya ilişkin bir örnek ekleme veya blog ilişkili gönderileri kendi koleksiyonunu birlikte güncelleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="d618f-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="d618f-159">Grafik alanındaki tüm varlıkları eklenecek ya da tüm güncelleştirilmesi, ardından aynı tek varlıklar için yukarıda açıklandığı gibi işlemidir.</span><span class="sxs-lookup"><span data-stu-id="d618f-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="d618f-160">Örneğin, bir grafik bloglar ve şöyle oluşturulan gönderileri:</span><span class="sxs-lookup"><span data-stu-id="d618f-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="d618f-161">Bu gibi eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="d618f-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="d618f-162">Ekle çağrısı blog ve eklenecek tüm gönderileri işaretler.</span><span class="sxs-lookup"><span data-stu-id="d618f-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="d618f-163">Bir grafik alanındaki tüm varlıkları güncelleştirilmesi gerekiyorsa, benzer şekilde, daha sonra güncelleştirme kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d618f-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="d618f-164">Blog ve tüm gönderileri güncelleştirilmesi için işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="d618f-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="d618f-165">Yeni ve var olan varlıkları karışımı</span><span class="sxs-lookup"><span data-stu-id="d618f-165">Mix of new and existing entities</span></span>

<span data-ttu-id="d618f-166">Grafik ekleme gerektiren varlıkları ve güncelleştirme gerektirenler bir karışımını içeriyorsa bile otomatik olarak oluşturulan anahtarlarla güncelleştirme yeniden ekler ve güncelleştirmeler için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d618f-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="d618f-167">Güncelleştirme grafiği, blog veya diğer tüm varlıkları güncelleştirmesi işaretlenmiş sırada bir anahtar değer kümesi yoksa, ekleme için post herhangi bir varlığa işaretler.</span><span class="sxs-lookup"><span data-stu-id="d618f-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="d618f-168">Olarak daha önce otomatik olarak oluşturulan anahtarları kullanmadığınızda bir sorgu ve bazı işleme kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d618f-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="d618f-169">Siler işleme</span><span class="sxs-lookup"><span data-stu-id="d618f-169">Handling deletes</span></span>

<span data-ttu-id="d618f-170">Delete itibaren işlemek hassas olabilir, silinmesi gerektiğini genellikle bir varlık olmaması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="d618f-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="d618f-171">Bu işlem için bir yol varlık gerçekte silinmesini yerine silinmiş olarak işaretlenmiş şekilde "yumuşak siler" kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="d618f-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="d618f-172">Siler ve ardından güncelleştirmeleri ile aynı olur.</span><span class="sxs-lookup"><span data-stu-id="d618f-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="d618f-173">Kullanarak yazılım siler uygulanabilir [sorgu filtreleri](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="d618f-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="d618f-174">Silme işlemi için true, genel bir desen uzantı sorgu deseni temelde grafik diff nedir gerçekleştirmek için kullanmaktır Örneğin:</span><span class="sxs-lookup"><span data-stu-id="d618f-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="d618f-175">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="d618f-175">TrackGraph</span></span>

<span data-ttu-id="d618f-176">Dahili olarak, Ekle, ekleme ve güncelleştirme olup olmadığını, (eklemek için) eklenen (güncelleştirmek için) değiştirilen işaretlenmelidir ilişkin her bir varlık için Unchanged yapılan belirleme ile grafik geçişi kullanın (hiçbir şey yapma), veya silinen (silmek için).</span><span class="sxs-lookup"><span data-stu-id="d618f-176">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="d618f-177">Bu mekanizma TrackGraph API sunulur.</span><span class="sxs-lookup"><span data-stu-id="d618f-177">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="d618f-178">Örneğin, istemci bir grafik varlıkların geri gönderdiğinde, nasıl işleneceğini belirten her varlığın bazı bayrağını ayarlar varsayalım.</span><span class="sxs-lookup"><span data-stu-id="d618f-178">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="d618f-179">TrackGraph sonra bu bayrak işlemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d618f-179">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="d618f-180">Bayrakları yalnızca varlık örneğin kolaylık olması için bir parçası olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="d618f-180">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="d618f-181">Genelde bayrakları bir DTO veya istekte bulunan başka bir duruma parçası olacaktır.</span><span class="sxs-lookup"><span data-stu-id="d618f-181">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
