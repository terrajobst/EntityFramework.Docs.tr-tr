---
title: Bağlantısı kesilmiş varlıklar - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: a81b0a26fe98dcc1ddedc11aba2673338c8991e8
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37948983"
---
# <a name="disconnected-entities"></a><span data-ttu-id="2b803-102">Bağlantısı kesilmiş varlıklar</span><span class="sxs-lookup"><span data-stu-id="2b803-102">Disconnected entities</span></span>

<span data-ttu-id="2b803-103">DbContext örneği veritabanından döndürülen varlıkları otomatik olarak izler.</span><span class="sxs-lookup"><span data-stu-id="2b803-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="2b803-104">Bu varlıklar için yapılan değişiklikler SaveChanges çağrılır ve gerektiği şekilde veritabanını güncelleştirilecek algılanır.</span><span class="sxs-lookup"><span data-stu-id="2b803-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="2b803-105">Bkz: [temel Kaydet](basic.md) ve [ilgili verileri](related-data.md) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="2b803-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="2b803-106">Ancak, bazen varlıklar bir bağlam örneğini kullanma ve ardından farklı bir örneği kullanılarak kaydedilmiş sorgulanır.</span><span class="sxs-lookup"><span data-stu-id="2b803-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="2b803-107">Bu genellikle "bağlantısız" senaryolarında burada varlıkları sorgulanan, istemciye gönderilen, değişiklik, bir istek sunucuya geri gönderilen ve ardından kaydedilebilir bir web uygulaması gibi gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="2b803-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="2b803-108">Bu durumda, ikinci bağlam (güncelleştirme) mevcut veya yeni varlıklar olup olmadığını bilmek (eklenmesi gereken) gereksinimlerini örneği.</span><span class="sxs-lookup"><span data-stu-id="2b803-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="2b803-109">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="2b803-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="2b803-110">EF Core, yalnızca belirli bir birincil anahtar değeri ile herhangi bir varlık örneği takip edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b803-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="2b803-111">Bu bağlam boş olarak başlar, kısa süreli bir bağlam her birim iş için kullanılacak bir sorun olduğu durumda önlemek için en iyi yolu olarak bağlanmış varlıklar kişilikleri ve bu bağlam atıldı ve atılan kaydeder sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2b803-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="2b803-112">Yeni varlıklar tanımlayan</span><span class="sxs-lookup"><span data-stu-id="2b803-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="2b803-113">İstemci yeni varlıklar tanımlayan</span><span class="sxs-lookup"><span data-stu-id="2b803-113">Client identifies new entities</span></span>

<span data-ttu-id="2b803-114">İstemci sunucuya varlık yeni veya mevcut olup olmadığını bildirir uğraşmanız basit durumdur.</span><span class="sxs-lookup"><span data-stu-id="2b803-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="2b803-115">Örneğin, genellikle yeni bir varlık eklemek için mevcut bir varlığı güncelleştirmek için istekte farklı isteğidir.</span><span class="sxs-lookup"><span data-stu-id="2b803-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="2b803-116">Bu bölümün geri kalanında durumları kapsayan burada da eklemek veya güncelleştirmek başka bir şekilde karar vermek gerekli.</span><span class="sxs-lookup"><span data-stu-id="2b803-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="2b803-117">Otomatik olarak oluşturulan anahtarları</span><span class="sxs-lookup"><span data-stu-id="2b803-117">With auto-generated keys</span></span>

<span data-ttu-id="2b803-118">Otomatik olarak oluşturulan bir anahtarın değeri, genellikle bir varlık eklenmesi veya güncelleştirilmesi gerekip gerekmediğini belirlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2b803-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="2b803-119">Anahtarı ayarlanmamış olması halinde (diğer bir deyişle, hala CLR varsayılan değeri null, sıfır, vb.) olan varlık yeni ve gereken ekleme.</span><span class="sxs-lookup"><span data-stu-id="2b803-119">If the key has not been set (that is, it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="2b803-120">Öte yandan, anahtar değerini ayarlarsanız sonra zaten daha önce kaydedilmiş olması gerekir ve artık güncelleştirilmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="2b803-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="2b803-121">Diğer bir deyişle, anahtar varsa, bir değer ve varlık, istemciye gönderilen sorgulandı dönüp kodu artık güncelleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="2b803-121">In other words, if the key has a value, then the entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="2b803-122">Varlık türü bilinen bir unset anahtarı için denetimi daha kolaydır:</span><span class="sxs-lookup"><span data-stu-id="2b803-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="2b803-123">Ancak, EF ayrıca herhangi bir varlık türü ve anahtar türü için bunu yapmak için yerleşik bir yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="2b803-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="2b803-124">Varlıkları bağlam tarafından izlenen hemen sonra varlık Added durumda olsa bile anahtarları ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2b803-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="2b803-125">Bu, bir grafik varlıkları ve her biri, bu tür olduğu gibi TrackGraph API'sini kullanarak ne yapılması gerektiğine karar verme geçiş sırasında yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="2b803-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="2b803-126">Anahtar değeri yalnızca burada gösterilen şekilde kullanılmalıdır _önce_ varlık izlemek için bir çağrı yapılır.</span><span class="sxs-lookup"><span data-stu-id="2b803-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="2b803-127">İle diğer anahtarlar</span><span class="sxs-lookup"><span data-stu-id="2b803-127">With other keys</span></span>

<span data-ttu-id="2b803-128">Başka bir mekanizma, anahtar değerlerini otomatik olarak oluşturulmaz yeni varlıklar belirlemek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="2b803-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="2b803-129">Bu iki genel yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="2b803-129">There are two general approaches to this:</span></span>
 * <span data-ttu-id="2b803-130">Varlık için sorgu</span><span class="sxs-lookup"><span data-stu-id="2b803-130">Query for the entity</span></span>
 * <span data-ttu-id="2b803-131">İstemciden bir bayrak geçirin</span><span class="sxs-lookup"><span data-stu-id="2b803-131">Pass a flag from the client</span></span>

<span data-ttu-id="2b803-132">Varlık için sorgu için hemen bulma yöntemi kullanın:</span><span class="sxs-lookup"><span data-stu-id="2b803-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="2b803-133">Bir istemciden bir bayrak geçirmek için tam kod göstermek için bu belgenin kapsamı dışındadır.</span><span class="sxs-lookup"><span data-stu-id="2b803-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="2b803-134">Bir web uygulamasında bu genellikle farklı eylemler için farklı istek yapma veya bazı istek durumuna geçirmeden sonra denetleyicide ayıklama anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2b803-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="2b803-135">Tek varlık kaydediliyor</span><span class="sxs-lookup"><span data-stu-id="2b803-135">Saving single entities</span></span>

<span data-ttu-id="2b803-136">Bir ekleme veya güncelleştirme gerektiği ve ardından uygun şekilde ekleme veya güncelleştirme kullanılabilir olup olmadığını bilinen varsa:</span><span class="sxs-lookup"><span data-stu-id="2b803-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="2b803-137">Ancak, anahtar değerlerini otomatik olarak oluşturulan varlık kullanıyorsa, güncelleştirme yöntemi için her iki durumda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2b803-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="2b803-138">Güncelleştirme yöntemi, update, INSERT değil varlığın normal olarak işaretler.</span><span class="sxs-lookup"><span data-stu-id="2b803-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="2b803-139">Ancak, varlık otomatik olarak oluşturulmuş bir anahtar varsa ve varlık bunun yerine otomatik olarak için işaretlenmiş sonra anahtar değer ayarlandı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2b803-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="2b803-140">Bu davranış EF Core 2.0 sürümünde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2b803-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="2b803-141">Önceki sürümler için her zaman açıkça ekleme veya güncelleştirme seçmek gereklidir.</span><span class="sxs-lookup"><span data-stu-id="2b803-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="2b803-142">Varlık anahtarları otomatik olarak oluşturulan kullanmayan sonra uygulamanın varlık eklenmesi veya güncelleştirilmesi karar vermeniz gerekir: Örnek:</span><span class="sxs-lookup"><span data-stu-id="2b803-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="2b803-143">Buradaki adımları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2b803-143">The steps here are:</span></span>
* <span data-ttu-id="2b803-144">Bul döndürür dediğimiz bu nedenle veritabanı zaten bu Kimliğe sahip blog içermiyor sonra null eklerseniz ekleme için işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="2b803-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="2b803-145">Bul varlığın döndürürse, veritabanında var ve bağlam artık mevcut varlık izleme</span><span class="sxs-lookup"><span data-stu-id="2b803-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="2b803-146">Ardından SetValues Bu istemciden gelen değişiklikleri varlığa tüm özelliklerin değerlerini ayarlamak için kullanırız.</span><span class="sxs-lookup"><span data-stu-id="2b803-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="2b803-147">Gerektiği şekilde güncelleştirilecek varlık SetValues çağrı işaretler.</span><span class="sxs-lookup"><span data-stu-id="2b803-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="2b803-148">Yalnızca SetValues izlenen varlık öğesindekilerle farklı değerlere sahip özellikleri değiştirilmiş olarak işaretlenmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="2b803-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="2b803-149">Bu, gerçekten değişmiş olan sütunları güncelleştirme gönderildiğinde, güncelleştirilecek anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2b803-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="2b803-150">(Ve hiçbir şey değişmediyse, ardından hiçbir güncelleştirme hiç gönderilir.)</span><span class="sxs-lookup"><span data-stu-id="2b803-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="2b803-151">Grafikler ile çalışma</span><span class="sxs-lookup"><span data-stu-id="2b803-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="2b803-152">Kimlik çözümlemesi</span><span class="sxs-lookup"><span data-stu-id="2b803-152">Identity resolution</span></span>

<span data-ttu-id="2b803-153">Yukarıda belirtildiği gibi EF Core yalnızca belirli bir birincil anahtar değeri ile herhangi bir varlık örneği takip edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b803-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="2b803-154">Grafiklerle çalışırken graf ideal olarak bu sabit tutulur ve içeriği tek bir birim iş için kullanılması gereken şekilde oluşturulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2b803-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="2b803-155">Ardından grafiğin, yinelenenleri içeriyorsa, grafik EF birden çok örneği tek bir araya getirmek için göndermeden önce işlemek için gerekli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2b803-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="2b803-156">Bu şekilde yinelemeleri sağlamlaştırmak olabildiğince çabuk çakışma önlemek için uygulama ardışık düzeninizde yapılmalıdır örnekleri çakışan değerler ve ilişkileri, sahip olduğu Önemsiz olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="2b803-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="2b803-157">Tüm yeni/tüm var olan varlıkları</span><span class="sxs-lookup"><span data-stu-id="2b803-157">All new/all existing entities</span></span>

<span data-ttu-id="2b803-158">Grafikler ile çalışmaya ilişkin bir örnek ekleme veya blog ilişkili gönderileri kendi koleksiyonunu birlikte güncelleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="2b803-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="2b803-159">Graftaki tüm varlıkları eklenmesi gereken ya da tüm güncelleştirilmelidir ardından aynı tek varlıklar için yukarıda açıklandığı gibi işlemidir.</span><span class="sxs-lookup"><span data-stu-id="2b803-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="2b803-160">Örneğin, bir grafiğini blog ve gönderi şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="2b803-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="2b803-161">şu şekilde eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="2b803-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="2b803-162">Eklenecek çağrı blog ve tüm gönderileri eklenecek işaretler.</span><span class="sxs-lookup"><span data-stu-id="2b803-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="2b803-163">Bir grafikteki tüm varlıkları güncelleştirilmesi gerekiyorsa, benzer şekilde, daha sonra güncelleştirme kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2b803-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="2b803-164">Güncelleştirilecek blog ve tüm gönderileri işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="2b803-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="2b803-165">Yeni ve var olan varlıkları karışımı</span><span class="sxs-lookup"><span data-stu-id="2b803-165">Mix of new and existing entities</span></span>

<span data-ttu-id="2b803-166">Graf ekleme gerektiren varlıkları ve bu güncelleştirme gerektiren bir karışımını içeriyorsa bile otomatik olarak oluşturulan anahtarları ile güncelleştirme yeniden eklemeler ve güncelleştirmeler için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2b803-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="2b803-167">Güncelleştirme, grafik, blog veya post, diğer tüm varlıklar için güncelleştirme olarak işaretlenir ancak bir anahtar değer kümesi olmaması durumunda ekleme için herhangi bir varlık olarak işaretler.</span><span class="sxs-lookup"><span data-stu-id="2b803-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="2b803-168">Olarak daha önce otomatik olarak oluşturulan anahtarları, kullanılmadığında bir sorgu ve bazı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2b803-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="2b803-169">İşleme siler</span><span class="sxs-lookup"><span data-stu-id="2b803-169">Handling deletes</span></span>

<span data-ttu-id="2b803-170">Delete beri işlemek zor olabilir, silinmesi gerektiğini genellikle bir varlığın olmaması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2b803-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="2b803-171">Bu ayarı kullanarak çıkılacağını bir varlık gerçekten silinmesini yerine silinmiş olarak işaretlenmiş şekilde "geçici silme" kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="2b803-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="2b803-172">Siler ve ardından güncelleştirmeleri ile aynı olur.</span><span class="sxs-lookup"><span data-stu-id="2b803-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="2b803-173">Geçici silme kullanarak uygulanabilir [sorgu filtreleri](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="2b803-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="2b803-174">Silme işlemi için true, aslında graf farkı nedir gerçekleştirmek için sorgu deseninin bir uzantı kullanmak için yaygın bir düzen tutulmasıdır. Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2b803-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="2b803-175">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="2b803-175">TrackGraph</span></span>

<span data-ttu-id="2b803-176">Dahili olarak, Ekle, ekleme ve güncelleştirme olup olmadığını onu (eklemek için) eklenen (güncelleştirmek için) değiştirilen işaretlenmelidir seçeceğine her varlık için Unchanged yapılan bir belirleme ile grafik çapraz geçişi kullanın (hiçbir şey yapma), veya silinen (silmek için).</span><span class="sxs-lookup"><span data-stu-id="2b803-176">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="2b803-177">Bu mekanizma TrackGraph API aracılığıyla kullanıma sunulur.</span><span class="sxs-lookup"><span data-stu-id="2b803-177">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="2b803-178">Örneğin, istemci bir grafik varlıkları geri gönderdiğinde, her varlığın nasıl işleneceğini belirten bazı bayrağını ayarlar varsayalım.</span><span class="sxs-lookup"><span data-stu-id="2b803-178">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="2b803-179">TrackGraph, ardından bu bayrağı işlemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2b803-179">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="2b803-180">Bayraklar yalnızca varlık örneğin kolaylık olması için bir parçası olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="2b803-180">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="2b803-181">Genelde bayrakları bir DTO veya istekte bulunan başka bir duruma parçası olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2b803-181">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
