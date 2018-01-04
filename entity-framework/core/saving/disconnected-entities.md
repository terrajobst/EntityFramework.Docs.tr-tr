---
title: "Bağlantısı kesilmiş varlıkları - EF çekirdek"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0ea02876b9594d54c971a7b70fcf7ce591e56ba0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="disconnected-entities"></a><span data-ttu-id="59dbc-102">Bağlantısı kesilmiş varlıklar</span><span class="sxs-lookup"><span data-stu-id="59dbc-102">Disconnected entities</span></span>

<span data-ttu-id="59dbc-103">DbContext örnek veritabanı tarafından döndürülen varlıkları otomatik olarak izler.</span><span class="sxs-lookup"><span data-stu-id="59dbc-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="59dbc-104">Bu varlıklar için yapılan değişiklikler sonra SaveChanges olarak adlandırılır ve veritabanı gerektiğinde güncelleştirileceği zaman algılanacak.</span><span class="sxs-lookup"><span data-stu-id="59dbc-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="59dbc-105">Bkz: [temel Kaydet](basic.md) ve [ilgili verileri](related-data.md) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="59dbc-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="59dbc-106">Ancak, bazen varlıklar bir bağlam örneğini kullanarak ve farklı bir örneği kullanılarak kaydedilmiş seçmeleri istenir.</span><span class="sxs-lookup"><span data-stu-id="59dbc-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="59dbc-107">Bu genellikle "bağlantısız" senaryolarda burada varlıkları sorgulanan, istemciye gönderilen, değişiklik, bir sunucuya geri gönderilen ve sonra kaydedilmiş bir web uygulaması gibi gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="59dbc-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="59dbc-108">Bu durumda, ikinci bağlam (güncelleştirilmelidir) mevcut veya yeni varlıklardır olup olmadığını bilmek (eklenecek) gereksinimlerini örneği.</span><span class="sxs-lookup"><span data-stu-id="59dbc-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="59dbc-109">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) github'da.</span><span class="sxs-lookup"><span data-stu-id="59dbc-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="59dbc-110">Yeni varlıklar tanımlama</span><span class="sxs-lookup"><span data-stu-id="59dbc-110">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="59dbc-111">İstemci yeni varlıklar tanımlar</span><span class="sxs-lookup"><span data-stu-id="59dbc-111">Client identifies new entities</span></span>

<span data-ttu-id="59dbc-112">İstemci sunucuya varlık yeni veya mevcut olup olmadığını bildirir uğraşmanız basit durumdur.</span><span class="sxs-lookup"><span data-stu-id="59dbc-112">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="59dbc-113">Örneğin, genellikle yeni bir varlık eklemek için var olan bir varlığı güncelleştirmek için istekten farklı isteğidir.</span><span class="sxs-lookup"><span data-stu-id="59dbc-113">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="59dbc-114">Bu bölüm geri kalanı durumları kapsayan Burada, diğer herhangi bir yolla eklemek veya güncelleştirmek karar vermek gerekli.</span><span class="sxs-lookup"><span data-stu-id="59dbc-114">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="59dbc-115">Otomatik olarak oluşturulan anahtarlarla</span><span class="sxs-lookup"><span data-stu-id="59dbc-115">With auto-generated keys</span></span>

<span data-ttu-id="59dbc-116">Otomatik olarak oluşturulan bir anahtarın değeri, genellikle bir varlık eklenmesi veya güncelleştirilmesi gerekip gerekmediğini belirlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="59dbc-116">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="59dbc-117">Anahtar olmayan varsa varlık yeni olmalıdır ve gereken ekleme (yani hala bulunduğu CLR varsayılan değeri null, sıfır, vb.), ayarlanmış bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="59dbc-117">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="59dbc-118">Diğer taraftan, anahtar değeri ayarlarsanız sonra zaten daha önce kaydedilmiş gerekir ve şimdi güncelleştirilmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="59dbc-118">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="59dbc-119">Diğer bir deyişle, anahtar değeri varsa, ardından varlık, istemciye gönderilen sorgulandı ve şimdi güncelleştirilmesi döndürülmesini.</span><span class="sxs-lookup"><span data-stu-id="59dbc-119">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="59dbc-120">Varlık türü bilinen bir unset anahtarı için denetimi kolaydır:</span><span class="sxs-lookup"><span data-stu-id="59dbc-120">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="59dbc-121">Ancak, EF ayrıca herhangi bir varlık türü ve anahtar türü için bunu yapmanın yerleşik bir yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="59dbc-121">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="59dbc-122">Varlık bağlamı tarafından izlenen hemen varlık Added durumunda olsa bile anahtarları ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="59dbc-122">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="59dbc-123">Bu varlıkları ve her biri, bu tür olduğu gibi TrackGraph API'si kullanılırken yapmanız gerekenler karar grafiği çapraz geçiş yapan zaman yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="59dbc-123">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="59dbc-124">Anahtar değeri yalnızca aşağıda gösterildiği şekilde kullanılmalıdır _önce_ varlık izlemek için bir çağrı yapılır.</span><span class="sxs-lookup"><span data-stu-id="59dbc-124">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="59dbc-125">Diğer anahtarları</span><span class="sxs-lookup"><span data-stu-id="59dbc-125">With other keys</span></span>

<span data-ttu-id="59dbc-126">Başka bir mekanizma anahtarı değerlerini otomatik olarak oluşturulmaz yeni varlıklar belirlemek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="59dbc-126">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="59dbc-127">Bu iki genel yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="59dbc-127">There are two general approaches to this:</span></span>
 * <span data-ttu-id="59dbc-128">Varlık için sorgu</span><span class="sxs-lookup"><span data-stu-id="59dbc-128">Query for the entity</span></span>
 * <span data-ttu-id="59dbc-129">İstemciden bir bayrak geçirin</span><span class="sxs-lookup"><span data-stu-id="59dbc-129">Pass a flag from the client</span></span>

<span data-ttu-id="59dbc-130">Varlık için sorgu için hemen Find yöntemi kullanın:</span><span class="sxs-lookup"><span data-stu-id="59dbc-130">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="59dbc-131">Bir istemciden bir bayrak geçirme için tam kod göstermek için bu belgenin kapsamı dışındadır.</span><span class="sxs-lookup"><span data-stu-id="59dbc-131">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="59dbc-132">Bir web uygulamasını, genellikle farklı eylemler için farklı istekleri yapan veya istek bazı durumda geçirme ardından denetleyicide ayıklanıyor anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="59dbc-132">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="59dbc-133">Tek varlıkları kaydetme</span><span class="sxs-lookup"><span data-stu-id="59dbc-133">Saving single entities</span></span>

<span data-ttu-id="59dbc-134">Bir INSERT veya update gereklidir ve sonra uygun şekilde ekleme veya güncelleştirme kullanılabilir olup olmadığını bilinen ise:</span><span class="sxs-lookup"><span data-stu-id="59dbc-134">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="59dbc-135">Bununla birlikte, varlık otomatik olarak oluşturulan anahtar değerlerini kullanıyorsa, güncelleştirme yöntemi için her iki durumda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="59dbc-135">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="59dbc-136">Update yöntemi güncelleştirmesi değil Ekle varlık normal olarak işaretler.</span><span class="sxs-lookup"><span data-stu-id="59dbc-136">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="59dbc-137">Bununla birlikte, varlık otomatik olarak oluşturulan bir anahtar varsa ve varlık bunun yerine otomatik olarak için işaretlenmiş sonra herhangi bir anahtar değer ayarlandı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="59dbc-137">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="59dbc-138">Bu davranış EF çekirdek 2. 0'sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="59dbc-138">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="59dbc-139">Önceki sürümleri için her zaman açık olarak ekleme veya güncelleştirme seçmek gereklidir.</span><span class="sxs-lookup"><span data-stu-id="59dbc-139">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="59dbc-140">Varlık anahtarları otomatik olarak oluşturulan kullanmayan sonra uygulama varlık eklenen veya güncelleştirilen karar vermeniz gerekir: Örneğin:</span><span class="sxs-lookup"><span data-stu-id="59dbc-140">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="59dbc-141">Burada adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="59dbc-141">The steps here are:</span></span>
* <span data-ttu-id="59dbc-142">Bul döndürür diyoruz şekilde veritabanı zaten bu Kimliğe sahip blog içermiyor. ardından null eklerseniz, ekleme için işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="59dbc-142">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="59dbc-143">Bul bir varlık döndürürse, veritabanında var ve bağlam şimdi varolan varlık izleme</span><span class="sxs-lookup"><span data-stu-id="59dbc-143">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="59dbc-144">Ardından SetValues istemciden gelen değişiklikleri bu varlık üzerinde tüm özelliklerinin değerlerini ayarlamak için kullanırız.</span><span class="sxs-lookup"><span data-stu-id="59dbc-144">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="59dbc-145">SetValues çağrısı gerektiğinde güncelleştirilecek varlık işaretler.</span><span class="sxs-lookup"><span data-stu-id="59dbc-145">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="59dbc-146">Yalnızca SetValues izlenen varlık de için farklı değerlere sahip özellikleri değiştirilemez olarak işaretlenmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="59dbc-146">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="59dbc-147">Bu güncelleştirme gönderildiğinde, aslında değişmiş olan sütunları güncelleştirilecek anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="59dbc-147">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="59dbc-148">(Ve hiçbir şey değiştiyse, ardından güncelleştirme hiç gönderilecek.)</span><span class="sxs-lookup"><span data-stu-id="59dbc-148">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="59dbc-149">Grafikleri ile çalışma</span><span class="sxs-lookup"><span data-stu-id="59dbc-149">Working with graphs</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="59dbc-150">Tüm yeni/tüm var olan varlıkları</span><span class="sxs-lookup"><span data-stu-id="59dbc-150">All new/all existing entities</span></span>

<span data-ttu-id="59dbc-151">Grafikleri ile çalışmaya ilişkin bir örnek ekleme veya blog ilişkili gönderileri kendi koleksiyonunu birlikte güncelleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="59dbc-151">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="59dbc-152">Grafik alanındaki tüm varlıkları eklenecek ya da tüm güncelleştirilmesi, ardından aynı tek varlıklar için yukarıda açıklandığı gibi işlemidir.</span><span class="sxs-lookup"><span data-stu-id="59dbc-152">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="59dbc-153">Örneğin, bir grafik bloglar ve şöyle oluşturulan gönderileri:</span><span class="sxs-lookup"><span data-stu-id="59dbc-153">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="59dbc-154">Bu gibi eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="59dbc-154">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="59dbc-155">Ekle çağrısı blog ve eklenecek tüm gönderileri işaretler.</span><span class="sxs-lookup"><span data-stu-id="59dbc-155">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="59dbc-156">Bir grafik alanındaki tüm varlıkları güncelleştirilmesi gerekiyorsa, benzer şekilde, daha sonra güncelleştirme kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="59dbc-156">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="59dbc-157">Blog ve tüm gönderileri güncelleştirilmesi için işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="59dbc-157">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="59dbc-158">Yeni ve var olan varlıkları karışımı</span><span class="sxs-lookup"><span data-stu-id="59dbc-158">Mix of new and existing entities</span></span>

<span data-ttu-id="59dbc-159">Grafik ekleme gerektiren varlıkları ve güncelleştirme gerektirenler bir karışımını içeriyorsa bile otomatik olarak oluşturulan anahtarlarla güncelleştirme yeniden ekler ve güncelleştirmeler için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="59dbc-159">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="59dbc-160">Güncelleştirme grafiği, blog veya diğer tüm varlıkları güncelleştirmesi işaretlenmiş sırada bir anahtar değer kümesi yoksa, ekleme için post herhangi bir varlığa işaretler.</span><span class="sxs-lookup"><span data-stu-id="59dbc-160">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="59dbc-161">Olarak daha önce otomatik olarak oluşturulan anahtarları kullanmadığınızda bir sorgu ve bazı işleme kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="59dbc-161">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="59dbc-162">Siler işleme</span><span class="sxs-lookup"><span data-stu-id="59dbc-162">Handling deletes</span></span>

<span data-ttu-id="59dbc-163">Delete itibaren işlemek hassas olabilir, silinmesi gerektiğini genellikle bir varlık olmaması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="59dbc-163">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="59dbc-164">Bu işlem için bir yol varlık gerçekte silinmesini yerine silinmiş olarak işaretlenmiş şekilde "yumuşak siler" kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="59dbc-164">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="59dbc-165">Siler ve ardından güncelleştirmeleri ile aynı olur.</span><span class="sxs-lookup"><span data-stu-id="59dbc-165">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="59dbc-166">Kullanarak yazılım siler uygulanabilir [sorgu filtreleri](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="59dbc-166">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="59dbc-167">Silme işlemi için true, genel bir desen uzantı sorgu deseni temelde grafik diff nedir gerçekleştirmek için kullanmaktır Örneğin:</span><span class="sxs-lookup"><span data-stu-id="59dbc-167">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="59dbc-168">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="59dbc-168">TrackGraph</span></span>

<span data-ttu-id="59dbc-169">Dahili olarak, Ekle, ekleme ve güncelleştirme olup olmadığını, (eklemek için) eklenen (güncelleştirmek için) değiştirilen işaretlenmelidir ilişkin her bir varlık için Unchanged yapılan belirleme ile grafik geçişi kullanın (hiçbir şey yapma), veya silinen (silmek için).</span><span class="sxs-lookup"><span data-stu-id="59dbc-169">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="59dbc-170">Bu mekanizma TrackGraph API sunulur.</span><span class="sxs-lookup"><span data-stu-id="59dbc-170">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="59dbc-171">Örneğin, istemci bir grafik varlıkların geri gönderdiğinde, nasıl işleneceğini belirten her varlığın bazı bayrağını ayarlar varsayalım.</span><span class="sxs-lookup"><span data-stu-id="59dbc-171">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="59dbc-172">TrackGraph sonra bu bayrak işlemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="59dbc-172">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="59dbc-173">Bayrakları yalnızca varlık örneğin kolaylık olması için bir parçası olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="59dbc-173">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="59dbc-174">Genelde bayrakları bir DTO veya istekte bulunan başka bir duruma parçası olacaktır.</span><span class="sxs-lookup"><span data-stu-id="59dbc-174">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
