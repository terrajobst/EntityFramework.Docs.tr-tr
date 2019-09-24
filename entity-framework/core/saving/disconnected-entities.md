---
title: Bağlantısı kesilen varlıklar-EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
uid: core/saving/disconnected-entities
ms.openlocfilehash: 070f2ad396ec21858096c29413ac80bdf8547328
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197804"
---
# <a name="disconnected-entities"></a><span data-ttu-id="630a4-102">Bağlantısı kesilen varlıklar</span><span class="sxs-lookup"><span data-stu-id="630a4-102">Disconnected entities</span></span>

<span data-ttu-id="630a4-103">DbContext örneği veritabanından döndürülen varlıkları otomatik olarak izler.</span><span class="sxs-lookup"><span data-stu-id="630a4-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="630a4-104">Bu varlıklarda yapılan değişiklikler, SaveChanges çağrıldığında ve veritabanı gerektiği şekilde güncelleniyorsa algılanır.</span><span class="sxs-lookup"><span data-stu-id="630a4-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="630a4-105">Ayrıntılar için bkz. [temel kaydetme](basic.md) ve [ilgili veriler](related-data.md) .</span><span class="sxs-lookup"><span data-stu-id="630a4-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="630a4-106">Ancak, bazen varlıklar bir bağlam örneği kullanılarak sorgulanır ve sonra farklı bir örnek kullanılarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="630a4-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="630a4-107">Bu genellikle varlıkların sorgulandığı, istemciye gönderildiği, değiştirildiği, bir istekte sunucuya geri gönderildiği ve ardından kaydedildiği bir Web uygulaması gibi "bağlantısı kesik" senaryolarda meydana gelir.</span><span class="sxs-lookup"><span data-stu-id="630a4-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="630a4-108">Bu durumda, ikinci bağlam örneğinin varlıkların yeni (eklenmelidir) mi yoksa mevcut mi (güncelleştirilmesi gerekir) olduğunu bilmeleri gerekir.</span><span class="sxs-lookup"><span data-stu-id="630a4-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="630a4-109">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="630a4-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="630a4-110">EF Core, belirli bir birincil anahtar değeri olan herhangi bir varlığın yalnızca bir örneğini izleyebilir.</span><span class="sxs-lookup"><span data-stu-id="630a4-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="630a4-111">Bu sorunu önlemenin en iyi yolu, her iş birimi için, içeriğin boş başlaması, kendisine iliştirilmiş varlıklar olması, bu varlıkları kaydettiğinden ve sonra bağlamın atılacağını ve atılmasına yönelik kısa süreli bir bağlam kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="630a4-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="630a4-112">Yeni varlıkları tanımlama</span><span class="sxs-lookup"><span data-stu-id="630a4-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="630a4-113">İstemci yeni varlıkları tanımlar</span><span class="sxs-lookup"><span data-stu-id="630a4-113">Client identifies new entities</span></span>

<span data-ttu-id="630a4-114">En basit durum, istemcinin, varlığın yeni mi yoksa mevcut mi olduğunu bildirir.</span><span class="sxs-lookup"><span data-stu-id="630a4-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="630a4-115">Örneğin, genellikle yeni bir varlık ekleme isteği, var olan bir varlığı güncelleştirmek için istekten farklıdır.</span><span class="sxs-lookup"><span data-stu-id="630a4-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="630a4-116">Bu bölümün geri kalanında, ekleme veya güncelleştirme yapıp etmeksizin başka bir şekilde belirlenmesi gereken durumlar ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="630a4-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="630a4-117">Otomatik olarak oluşturulan anahtarlarla</span><span class="sxs-lookup"><span data-stu-id="630a4-117">With auto-generated keys</span></span>

<span data-ttu-id="630a4-118">Otomatik olarak oluşturulan bir anahtarın değeri, genellikle bir varlığın eklenmesi veya güncelleştirilmesi gerekip gerekmediğini belirlemede kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="630a4-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="630a4-119">Anahtar ayarlanmamışsa (diğer bir deyişle, hala CLR varsayılan değeri null, sıfır vb.), varlık yeni ve ekleme gerekiyor olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="630a4-119">If the key has not been set (that is, it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="630a4-120">Diğer taraftan, anahtar değeri ayarlandıysa, daha önce kaydedilmiş ve şimdi güncelleştirilmesi gerekiyor olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="630a4-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="630a4-121">Diğer bir deyişle, anahtarın bir değeri varsa, varlık sorgulanmıştı, istemciye gönderilir ve şimdi güncelleştirilmesini geri gelir.</span><span class="sxs-lookup"><span data-stu-id="630a4-121">In other words, if the key has a value, then the entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="630a4-122">Varlık türü bilindiğinde, bir ayarı kontrol etmek kolaydır:</span><span class="sxs-lookup"><span data-stu-id="630a4-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="630a4-123">Bununla birlikte, EF aynı zamanda herhangi bir varlık türü ve anahtar türü için yerleşik bir yönteme sahiptir:</span><span class="sxs-lookup"><span data-stu-id="630a4-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="630a4-124">Varlık eklenen durumda olsa bile, varlıklar bağlam tarafından izlendiğinde, anahtarlar hemen ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="630a4-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="630a4-125">Bu, bir varlık grafiğinde geçiş yaparken ve her biriyle ne yapacağına karar verirken (TrackGraph API 'SI kullanılırken olduğu gibi) yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="630a4-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="630a4-126">Anahtar değeri yalnızca varlığı izlemek için herhangi bir çağrı _yapılmadan önce_ burada gösterildiği gibi kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="630a4-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="630a4-127">Diğer anahtarlarla</span><span class="sxs-lookup"><span data-stu-id="630a4-127">With other keys</span></span>

<span data-ttu-id="630a4-128">Anahtar değerleri otomatik olarak oluşturulmadığında yeni varlıkları tanımlamak için başka bir mekanizma gerekir.</span><span class="sxs-lookup"><span data-stu-id="630a4-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="630a4-129">Bunun için iki genel yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="630a4-129">There are two general approaches to this:</span></span>
 * <span data-ttu-id="630a4-130">Varlık için sorgu</span><span class="sxs-lookup"><span data-stu-id="630a4-130">Query for the entity</span></span>
 * <span data-ttu-id="630a4-131">İstemciden bir bayrak geçirin</span><span class="sxs-lookup"><span data-stu-id="630a4-131">Pass a flag from the client</span></span>

<span data-ttu-id="630a4-132">Varlığı sorgulamak için Find metodunu kullanmanız yeterlidir:</span><span class="sxs-lookup"><span data-stu-id="630a4-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="630a4-133">Bir istemciden bayrak geçirmenin tam kodunu göstermek için bu belgenin kapsamı dışındadır.</span><span class="sxs-lookup"><span data-stu-id="630a4-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="630a4-134">Bir Web uygulamasında, genellikle farklı eylemler için farklı istekler yapılması veya istekte bir durum iletilmesi ve sonra denetleyiciye ayıklanması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="630a4-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="630a4-135">Tek varlıkları kaydetme</span><span class="sxs-lookup"><span data-stu-id="630a4-135">Saving single entities</span></span>

<span data-ttu-id="630a4-136">Bir ekleme veya güncelleştirme gerekip gerekmediğini bilindiğinde, ekleme veya güncelleştirme uygun şekilde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="630a4-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="630a4-137">Ancak, varlık otomatik üretilen anahtar değerlerini kullanıyorsa, her iki durumda da güncelleştirme yöntemi kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="630a4-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="630a4-138">Update yöntemi, normal olarak güncelleştirme için varlığı, INSERT değil olarak işaretler.</span><span class="sxs-lookup"><span data-stu-id="630a4-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="630a4-139">Ancak, varlık otomatik olarak oluşturulmuş bir anahtara sahipse ve anahtar değeri ayarlanmamışsa, bu durumda varlık otomatik olarak ekleme için işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="630a4-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="630a4-140">Bu davranış EF Core 2,0 ' de tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="630a4-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="630a4-141">Daha önceki sürümlerde, açıkça Ekle veya Güncelleştir ' i seçmek için her zaman gereklidir.</span><span class="sxs-lookup"><span data-stu-id="630a4-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="630a4-142">Varlık otomatik olarak oluşturulan anahtarları kullanmıyor ise, uygulamanın varlığın eklenip eklenmeyeceğine veya güncelleştirilmesine karar vermelidir: Örneğin:</span><span class="sxs-lookup"><span data-stu-id="630a4-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="630a4-143">Buradaki adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="630a4-143">The steps here are:</span></span>
* <span data-ttu-id="630a4-144">Find null döndürürse, veritabanı bu KIMLIĞE sahip blogu zaten içermez, bu nedenle ekleme öğesini çağırıyoruz.</span><span class="sxs-lookup"><span data-stu-id="630a4-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="630a4-145">Bul bir varlık döndürürse, veritabanında bulunur ve bağlam artık var olan varlığı izliyor</span><span class="sxs-lookup"><span data-stu-id="630a4-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="630a4-146">Daha sonra bu varlıktaki tüm özelliklerin değerlerini istemciden gelen değerlere ayarlamak için SetValues kullanılır.</span><span class="sxs-lookup"><span data-stu-id="630a4-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="630a4-147">SetValues çağrısı, varlığı gerektiği şekilde güncelleştiren şekilde işaretleyecek.</span><span class="sxs-lookup"><span data-stu-id="630a4-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="630a4-148">SetValues yalnızca, izlenen varlıktaki farklı değerlere sahip özellikleri değiştirilmiş olarak işaretlenecektir.</span><span class="sxs-lookup"><span data-stu-id="630a4-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="630a4-149">Bu, güncelleştirme gönderildiğinde yalnızca gerçekten değiştirilen sütunlar güncelleştirilecek anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="630a4-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="630a4-150">(Ve hiçbir şey değiştirilmezse hiçbir güncelleştirme gönderilmez.)</span><span class="sxs-lookup"><span data-stu-id="630a4-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="630a4-151">Grafiklerle çalışma</span><span class="sxs-lookup"><span data-stu-id="630a4-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="630a4-152">Kimlik çözümlemesi</span><span class="sxs-lookup"><span data-stu-id="630a4-152">Identity resolution</span></span>

<span data-ttu-id="630a4-153">Yukarıda belirtildiği gibi, EF Core yalnızca belirli bir birincil anahtar değeri olan herhangi bir varlığın bir örneğini izleyebilir.</span><span class="sxs-lookup"><span data-stu-id="630a4-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="630a4-154">Grafiklerle çalışırken, grafik ideal bir şekilde oluşturulmalıdır ve bağlam yalnızca bir iş birimi için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="630a4-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="630a4-155">Grafik yinelenen öğeler içeriyorsa, birden çok örneği birleştirmek için bir grafik için EF 'e göndermeden önce grafiğin işlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="630a4-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="630a4-156">Bu, örneklerin çakışan değerler ve ilişkilerin bulunduğu önemsiz olmayabilir, bu nedenle yinelenenleri birleştirme, uygulama ardışık düzeninde çakışma çözümünden kaçınmak için mümkün olan en kısa sürede yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="630a4-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="630a4-157">Tüm yeni/tüm mevcut varlıklar</span><span class="sxs-lookup"><span data-stu-id="630a4-157">All new/all existing entities</span></span>

<span data-ttu-id="630a4-158">Grafiklerle çalışmaya bir örnek, bir blogu ilişkili gönderilerin koleksiyonuyla birlikte ekleme veya güncelleştirme.</span><span class="sxs-lookup"><span data-stu-id="630a4-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="630a4-159">Grafikteki tüm varlıkların eklenmesi veya tümünün güncellenmesi gerekiyorsa, işlem yukarıda açıklanan tek varlıklar için aynı olur.</span><span class="sxs-lookup"><span data-stu-id="630a4-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="630a4-160">Örneğin, aşağıdaki gibi oluşturulan blogların ve gönderilerin bir grafiği:</span><span class="sxs-lookup"><span data-stu-id="630a4-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="630a4-161">şöyle eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="630a4-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="630a4-162">Eklenecek çağrı blogunu ve eklenecek tüm gönderileri işaretleyecek.</span><span class="sxs-lookup"><span data-stu-id="630a4-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="630a4-163">Benzer şekilde, bir grafikteki tüm varlıkların güncelleştirilmesi gerekiyorsa, güncelleştirme kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="630a4-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="630a4-164">Blog ve tüm gönderimler güncellenmeyecek şekilde işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="630a4-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="630a4-165">Yeni ve mevcut varlıkların karışımı</span><span class="sxs-lookup"><span data-stu-id="630a4-165">Mix of new and existing entities</span></span>

<span data-ttu-id="630a4-166">Otomatik olarak oluşturulan anahtarlarla, grafik ekleme ve güncelleştirme gerektiren varlıkların bir karışımını içerse bile, güncelleştirme her iki ekleme ve güncelleştirme için de kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="630a4-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="630a4-167">Güncelleştirme, bir anahtar değer kümesi yoksa, diğer tüm varlıklar güncelleştirme için işaretlenirken grafik, blog veya gönderi içindeki herhangi bir varlığı, ekleme için bir anahtar değeri ayarlanmamışsa işaretler.</span><span class="sxs-lookup"><span data-stu-id="630a4-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="630a4-168">Daha önce olduğu gibi, otomatik olarak oluşturulan anahtarlar kullanılmazsa bir sorgu ve bir işlem kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="630a4-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="630a4-169">Silmeleri işleme</span><span class="sxs-lookup"><span data-stu-id="630a4-169">Handling deletes</span></span>

<span data-ttu-id="630a4-170">Genellikle bir varlık yokluğu, silinmesi gerektiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="630a4-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="630a4-171">Bunu yapmanın bir yolu, varlığın gerçekten silinmesi yerine silinmiş olarak işaretlenmesi için "geçici silme" kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="630a4-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="630a4-172">Sonra siler, güncelleştirmelerle aynı olur.</span><span class="sxs-lookup"><span data-stu-id="630a4-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="630a4-173">Geçici silme, [sorgu filtreleri](xref:core/querying/filters)kullanılarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="630a4-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="630a4-174">Doğru silme işlemleri için genel bir model, temel bir grafik farkı olan bir sorgu deseninin uzantısını kullanmaktır. Örneğin:</span><span class="sxs-lookup"><span data-stu-id="630a4-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="630a4-175">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="630a4-175">TrackGraph</span></span>

<span data-ttu-id="630a4-176">Dahili, ekleme, Iliştirme ve güncelleştirme, her varlık için eklenen (ekleme), değiştirme (güncelleştirme), değiştirilmemiş (hiçbir şey yapma) veya silinmiş (silmek için) olarak işaretlenme gibi bir belirleme ile grafik çapraz geçişi kullanın.</span><span class="sxs-lookup"><span data-stu-id="630a4-176">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="630a4-177">Bu mekanizma, TrackGraph API 'SI aracılığıyla sunulur.</span><span class="sxs-lookup"><span data-stu-id="630a4-177">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="630a4-178">Örneğin, istemci bir varlık grafiğini geri gönderdiğinde, her bir varlıkta nasıl işleneceğini gösteren bir bayrak ayarladığını varsayalım.</span><span class="sxs-lookup"><span data-stu-id="630a4-178">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="630a4-179">TrackGraph, daha sonra bu bayrağı işlemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="630a4-179">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="630a4-180">Bayraklar, örneğin basitliği için varlığın bir parçası olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="630a4-180">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="630a4-181">Genellikle bayraklar bir DTO veya istekte bulunan başka bir durumun parçası olur.</span><span class="sxs-lookup"><span data-stu-id="630a4-181">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
