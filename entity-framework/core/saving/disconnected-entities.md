---
title: Bağlantısız Varlıklar - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
uid: core/saving/disconnected-entities
ms.openlocfilehash: 421531e68ac98c0553938f1c24892701f22fef3c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417599"
---
# <a name="disconnected-entities"></a><span data-ttu-id="c7fbf-102">Bağlantısı kesilen varlıklar</span><span class="sxs-lookup"><span data-stu-id="c7fbf-102">Disconnected entities</span></span>

<span data-ttu-id="c7fbf-103">DbContext örneği, veritabanından döndürülen varlıkları otomatik olarak izler.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="c7fbf-104">SaveChanges çağrıldığında bu varlıklarda yapılan değişiklikler algılanır ve veritabanı gerektiği gibi güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="c7fbf-105">Ayrıntılar için [Temel Kaydet](basic.md) ve [İlgili Verilere](related-data.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="c7fbf-106">Ancak, bazen varlıklar tek bir bağlam örneği kullanılarak sorgulanır ve sonra farklı bir örnek kullanılarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="c7fbf-107">Bu genellikle varlıkların sorgulandığı, istemciye gönderildiği, değiştirildiği, istekte sunucuya geri gönderildiği ve sonra kaydedildiği bir web uygulaması gibi "bağlantısı kesilmiş" senaryolarda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="c7fbf-108">Bu durumda, ikinci bağlam örneğinin varlıkların yeni mi yoksa varolan mı (güncelleştirilmelidir) olup olmadığını bilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

<!-- markdownlint-disable MD028 -->
> [!TIP]
> <span data-ttu-id="c7fbf-109">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) GitHub'da görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-109">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="c7fbf-110">EF Core, belirli bir birincil anahtar değeri olan herhangi bir varlığın yalnızca bir örneğini izleyebilir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="c7fbf-111">Bu bir sorun olmasını önlemenin en iyi yolu, her bir çalışma birimi için bağlamın boş başlaması, ona bağlı varlıkların olması, bu varlıkların kaydedilmesi ve bağlamın bertaraf edilip atılması gibi kısa ömürlü bir bağlam kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>
<!-- markdownlint-enable MD028 -->

## <a name="identifying-new-entities"></a><span data-ttu-id="c7fbf-112">Yeni varlıkların tanımlanması</span><span class="sxs-lookup"><span data-stu-id="c7fbf-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="c7fbf-113">İstemci yeni varlıkları tanımlar</span><span class="sxs-lookup"><span data-stu-id="c7fbf-113">Client identifies new entities</span></span>

<span data-ttu-id="c7fbf-114">Ele almak için en basit durumda istemci varlığı yeni veya varolan olup olmadığını sunucuya bildirir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="c7fbf-115">Örneğin, genellikle yeni bir varlık ekleme isteği varolan bir varlığı güncelleştirme isteğifarklıdır.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="c7fbf-116">Bu bölümün geri kalanı, eklemek veya güncelleştirmek için başka bir şekilde belirlemek için gerekli durumlarda kapsar.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="c7fbf-117">Otomatik oluşturulan anahtarlarla</span><span class="sxs-lookup"><span data-stu-id="c7fbf-117">With auto-generated keys</span></span>

<span data-ttu-id="c7fbf-118">Otomatik olarak oluşturulan anahtarın değeri genellikle bir varlığın eklenmesi veya güncelleştirilmesi gerekip gerekmediğini belirlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="c7fbf-119">Anahtar ayarlanmadıysa (diğer bir deyişle, hala null, sıfır, vb CLR varsayılan değerine sahip), o zaman varlık yeni olmalı ve ekleme ihtiyacı vardır.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-119">If the key has not been set (that is, it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="c7fbf-120">Diğer taraftan, anahtar değeri ayarlanmışsa, daha önce kaydedilmiş olması gerekir ve şimdi güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="c7fbf-121">Başka bir deyişle, anahtarın bir değeri varsa, varlık sorgulandı, istemciye gönderildi ve şimdi güncellenmek üzere geri geldi.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-121">In other words, if the key has a value, then the entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="c7fbf-122">Varlık türü bilindiğinde ayarlanmamış anahtarı denetlemek kolaydır:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="c7fbf-123">Ancak, EF de herhangi bir varlık türü ve anahtar türü için bunu yapmak için yerleşik bir şekilde vardır:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="c7fbf-124">Varlıklar bağlam tarafından izlenir izlenmez, varlık Eklenen durumda olsa bile anahtarlar ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="c7fbf-125">Bu, varlıkların bir grafiğini dolaşırken ve TrackGraph API'sını kullanırken olduğu gibi her biriyle ne yapacağınız konusunda karar verirken yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="c7fbf-126">Anahtar değeri, varlığı izlemek için herhangi bir arama yapılmadan _önce_ yalnızca burada gösterilen şekilde kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="c7fbf-127">Diğer tuşlarla</span><span class="sxs-lookup"><span data-stu-id="c7fbf-127">With other keys</span></span>

<span data-ttu-id="c7fbf-128">Anahtar değerler otomatik olarak oluşturulmadığında yeni varlıkları tanımlamak için başka bir mekanizma gerekir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="c7fbf-129">Bunun iki genel yaklaşımı vardır:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-129">There are two general approaches to this:</span></span>

* <span data-ttu-id="c7fbf-130">Varlık için sorgu</span><span class="sxs-lookup"><span data-stu-id="c7fbf-130">Query for the entity</span></span>
* <span data-ttu-id="c7fbf-131">İstemciden bayrak geçir</span><span class="sxs-lookup"><span data-stu-id="c7fbf-131">Pass a flag from the client</span></span>

<span data-ttu-id="c7fbf-132">Varlığı sorgulamak için Bul yöntemini kullanman:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="c7fbf-133">Bir istemciden bayrak geçirmek için tam kodu göstermek için bu belgenin kapsamı dışındadır.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="c7fbf-134">Bir web uygulamasında, genellikle farklı eylemler için farklı isteklerde bulunmak veya istekteki bazı durumları geçmek ve denetleyicide ayıklamak anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="c7fbf-135">Tek varlıkları kaydetme</span><span class="sxs-lookup"><span data-stu-id="c7fbf-135">Saving single entities</span></span>

<span data-ttu-id="c7fbf-136">Bir ekleme veya güncelleştirme gerekip gerekmediği biliniyorsa, Ekle veya Güncelleştir uygun şekilde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="c7fbf-137">Ancak, varlık otomatik olarak oluşturulan anahtar değerlerini kullanıyorsa, güncelleştirme yöntemi her iki durum için de kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="c7fbf-138">Güncelleştirme yöntemi normalde ekleme değil, güncelleştirme için varlık işaretler.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="c7fbf-139">Ancak, varlığın otomatik olarak oluşturulan bir anahtarı varsa ve anahtar değeri ayarlanmadıysa, varlık eklemek için otomatik olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="c7fbf-140">Bu davranış EF Core 2.0'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="c7fbf-141">Önceki sürümler için her zaman açıkça Ekle veya Güncelleştir'i seçmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="c7fbf-142">Varlık otomatik olarak oluşturulan anahtarları kullanmıyorsa, uygulama varlığın eklenmesi mi yoksa güncelleştirilmesi mi gerektiğine karar vermelidir: Örneğin:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="c7fbf-143">Burada adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-143">The steps here are:</span></span>

* <span data-ttu-id="c7fbf-144">Bul döndürür null dönerse, veritabanı zaten bu kimliği içeren blog içermez, bu yüzden ekleme için işaretle diyoruz.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="c7fbf-145">Find bir varlığı döndürürse, veritabanında var olur ve bağlam şimdi varolan varlığı takip ediyor</span><span class="sxs-lookup"><span data-stu-id="c7fbf-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="c7fbf-146">Daha sonra, bu varlıktaki tüm özelliklerin değerlerini istemciden gelenlere ayarlamak için SetValues'i kullanırız.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="c7fbf-147">SetValues çağrısı, gerektiğinde güncelleştirilecek varlığı işaretler.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="c7fbf-148">SetDeğerleri yalnızca izlenen varlıktakilerle farklı değerlere sahip özellikleri değiştirilmiş olarak işaretler.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="c7fbf-149">Bu, güncelleştirme gönderildiğinde yalnızca gerçekten değiştirilen sütunların güncelleştirileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="c7fbf-150">(Ve hiçbir şey değişmediyse, o zaman hiçbir güncelleştirme gönderilmez.)</span><span class="sxs-lookup"><span data-stu-id="c7fbf-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="c7fbf-151">Grafiklerle çalışma</span><span class="sxs-lookup"><span data-stu-id="c7fbf-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="c7fbf-152">Kimlik çözünürlüğü</span><span class="sxs-lookup"><span data-stu-id="c7fbf-152">Identity resolution</span></span>

<span data-ttu-id="c7fbf-153">Yukarıda belirtildiği gibi, EF Core yalnızca belirli bir birincil anahtar değeri olan herhangi bir varlığın bir örneğini izleyebilir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="c7fbf-154">Grafiklerle çalışırken grafik ideal olarak bu değişmezin korunması ve bağlamın yalnızca bir çalışma birimi için kullanılması gibi oluşturulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="c7fbf-155">Grafik yinelemeler içeriyorsa, birden çok örneği tek bir örnekte birleştirmek için grafiği EF'ye göndermeden önce işlemek gerekir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="c7fbf-156">Örneklerin çakışan değerler eve sahip olduğu bu önemsiz olmayabilir, bu nedenle çakışma çözümünü önlemek için uygulama ardışık bilgisayarınızda yinelenenleri en kısa sürede birleştirme yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="c7fbf-157">Tüm yeni/tüm mevcut varlıklar</span><span class="sxs-lookup"><span data-stu-id="c7fbf-157">All new/all existing entities</span></span>

<span data-ttu-id="c7fbf-158">Grafiklerle çalışmanın bir örneği, bir blogu ilişkili gönderikoleksiyonuyla birlikte eklemek veya güncelleştirmektir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="c7fbf-159">Grafikteki tüm varlıklar eklenmelidir veya tümü güncelleştirilmelidir, işlem tek varlıklar için yukarıda açıklandığı gibi aynıdır.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="c7fbf-160">Örneğin, aşağıdaki gibi oluşturulan blogların ve gönderilerin grafiği:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="c7fbf-161">şu şekilde eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="c7fbf-162">Ekle'ye çağrı, blogu ve eklenecek tüm gönderileri işaretler.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="c7fbf-163">Aynı şekilde, grafikteki tüm varlıkların güncellenmesi gerekiyorsa, Güncelleştirme kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="c7fbf-164">Blog ve tüm gönderileri güncellenmek üzere işaretlenecektir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="c7fbf-165">Yeni ve mevcut varlıkların karışımı</span><span class="sxs-lookup"><span data-stu-id="c7fbf-165">Mix of new and existing entities</span></span>

<span data-ttu-id="c7fbf-166">Otomatik olarak oluşturulan anahtarlarla, grafik ekleme ve güncelleştirme gerektiren varlıkların bir karışımını içerse bile, Güncelleştirme hem ekler hem de güncelleştirmeler için yeniden kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="c7fbf-167">Güncelleştirme, önemli bir değer kümesi yoksa, diğer tüm varlıklar güncelleştirme için işaretlenmiş ise ekleme için grafik, blog veya gönderideki herhangi bir varlığı işaretler.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="c7fbf-168">Daha önce olduğu gibi, otomatik olarak oluşturulan anahtarları kullanmadığınızda, bir sorgu ve bazı işleme kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="c7fbf-169">İşleme siler</span><span class="sxs-lookup"><span data-stu-id="c7fbf-169">Handling deletes</span></span>

<span data-ttu-id="c7fbf-170">Silme, genellikle bir varlığın yokluğu silinmesi gerektiği anlamına geldiğinden, işlemek zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="c7fbf-171">Bununla başa çıkmanın bir yolu, varlığın gerçekten silinmek yerine silinmiş olarak işaretlendiğini "yumuşak silmeler" kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="c7fbf-172">Siler sonra güncelleştirmeleri aynı olur.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="c7fbf-173">Yumuşak [silmesorgu filtreleri](xref:core/querying/filters)kullanılarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="c7fbf-174">Gerçek siler için, ortak bir desen aslında bir grafik diff ne gerçekleştirmek için sorgu deseni bir uzantısı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff.</span></span> <span data-ttu-id="c7fbf-175">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-175">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="c7fbf-176">Trackgraph</span><span class="sxs-lookup"><span data-stu-id="c7fbf-176">TrackGraph</span></span>

<span data-ttu-id="c7fbf-177">Dahili olarak, Ekle, Ekle ve Güncelleştir, her varlık için eklenen (eklemek için), Değiştirilen (güncelleştirmek için), Değiştirilmeden (hiçbir şey yapmamak) veya Silinmiş (silinecek) olarak işaretlenip işaretlenmemesi gerektiği konusunda yapılan bir kararlılıkla grafik-traversal kullanın.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-177">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="c7fbf-178">Bu mekanizma TrackGraph API ile ortaya çıkarır.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-178">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="c7fbf-179">Örneğin, istemci varlıkların bir grafik geri gönderdiğinde, nasıl işlenmesi gerektiğini belirten her varlık üzerinde bazı bayrak ayarlar varsayalım.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-179">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="c7fbf-180">TrackGraph daha sonra bu bayrağı işlemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="c7fbf-180">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="c7fbf-181">Bayraklar yalnızca örneğin basitliği için varlığın bir parçası olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-181">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="c7fbf-182">Genellikle bayraklar, isteğe dahil edilmiş bir DTO'nun veya başka bir durum durumunun bir parçası olur.</span><span class="sxs-lookup"><span data-stu-id="c7fbf-182">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
