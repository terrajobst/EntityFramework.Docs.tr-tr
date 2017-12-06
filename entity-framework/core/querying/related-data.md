---
title: "Yükleme ilgili verileri - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: cd26bd2e6f85083f73d97b1356d0ba38f53e0b8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="loading-related-data"></a><span data-ttu-id="78d3f-102">İlgili verileri yükleniyor</span><span class="sxs-lookup"><span data-stu-id="78d3f-102">Loading Related Data</span></span>

<span data-ttu-id="78d3f-103">Entity Framework Çekirdek, gezinti özellikleri ilgili varlıklar yüklemek için modelinizde kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="78d3f-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="78d3f-104">İlgili verileri yüklemek için kullanılan üç ortak O/RM desenler vardır.</span><span class="sxs-lookup"><span data-stu-id="78d3f-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="78d3f-105">**İstekli yükleme** ilgili verileri ilk sorgu bir parçası olarak veritabanından yüklenen anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="78d3f-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="78d3f-106">**Açık yükleme** ilgili verileri veritabanından açıkça daha sonraki bir zamanda yüklenip yüklenmediğine anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="78d3f-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="78d3f-107">**Yavaş Yükleniyor** gezinti özelliği erişildiğinde ilgili verileri veritabanından saydam yüklenen anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="78d3f-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span> <span data-ttu-id="78d3f-108">Yavaş yükleniyor henüz EF çekirdek ile mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="78d3f-108">Lazy loading is not yet possible with EF Core.</span></span>

> [!TIP]  
> <span data-ttu-id="78d3f-109">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) github'da.</span><span class="sxs-lookup"><span data-stu-id="78d3f-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="78d3f-110">istekli yükleniyor</span><span class="sxs-lookup"><span data-stu-id="78d3f-110">Eager loading</span></span>

<span data-ttu-id="78d3f-111">Kullanabileceğiniz `Include` yöntemi sorgu sonuçlarında dahil edilecek ilgili verileri belirtin.</span><span class="sxs-lookup"><span data-stu-id="78d3f-111">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="78d3f-112">Aşağıdaki örnekte, döndürülen sonuçlarda bloglar olacaktır kendi `Posts` özelliği ile ilgili gönderileri doldurulur.</span><span class="sxs-lookup"><span data-stu-id="78d3f-112">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="78d3f-113">Entity Framework Çekirdek otomatik olarak düzeltme yukarı Gezinti özellikleri bağlam örneğine önceden yüklenmiş herhangi bir varlık için.</span><span class="sxs-lookup"><span data-stu-id="78d3f-113">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="78d3f-114">Bu nedenle bir gezinme özelliği için veri açıkça içerme olsa bile, özellik hala bazıları doldurulmuş veya tüm ilgili varlıklar daha önce yüklenen.</span><span class="sxs-lookup"><span data-stu-id="78d3f-114">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="78d3f-115">Tek bir sorguda birden çok ilişki ilgili verileri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="78d3f-115">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="78d3f-116">Birden çok düzeyi de dahil olmak üzere</span><span class="sxs-lookup"><span data-stu-id="78d3f-116">Including multiple levels</span></span>

<span data-ttu-id="78d3f-117">Birden çok düzeyinden birini kullanarak ilgili verileri içerecek şekilde ilişkileri ayrıntıya girebilirsiniz `ThenInclude` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="78d3f-117">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="78d3f-118">Aşağıdaki örnek, tüm blogları, kendi ilgili gönderileri ve her posta yazarı yükler.</span><span class="sxs-lookup"><span data-stu-id="78d3f-118">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="78d3f-119">Birden fazla çağrı zincir `ThenInclude` daha ilgili verileri düzeylerini dahil olmak üzere devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="78d3f-119">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="78d3f-120">Bu aynı sorguda birden çok düzeyi ve birden çok kök ilgili verileri içerecek şekilde tüm birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78d3f-120">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="78d3f-121">Dahil varlıklar biri için birden çok ilgili varlık eklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78d3f-121">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="78d3f-122">Örneğin, sorgulanırken `Blog`s, eklediğiniz `Posts` ve ardından her ikisi de dahil etmek istediğiniz `Author` ve `Tags` , `Posts`.</span><span class="sxs-lookup"><span data-stu-id="78d3f-122">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="78d3f-123">Bunu yapmak için her dahil kökte başlayan yolunu belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="78d3f-123">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="78d3f-124">Örneğin, `Blog -> Posts -> Author` ve `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="78d3f-124">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="78d3f-125">Bu, çoğu durumda EF birleştirmek yedekli birleştirmeler SQL oluştururken birleştirmeler alırsınız gelmez.</span><span class="sxs-lookup"><span data-stu-id="78d3f-125">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a><span data-ttu-id="78d3f-126">Göz ardı içerir</span><span class="sxs-lookup"><span data-stu-id="78d3f-126">Ignored includes</span></span>

<span data-ttu-id="78d3f-127">Böylece artık sorgu ile başlamıştır varlık türünün örneklerini döndürür sorgu değiştirirseniz, INCLUDE işleçleri göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="78d3f-127">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="78d3f-128">INCLUDE işleçleri aşağıdaki örnekte, temel alan `Blog`, ancak ardından `Select` işleci anonim bir tür döndürmek için sorguyu değiştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="78d3f-128">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="78d3f-129">Bu durumda, INCLUDE işleçleri hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="78d3f-129">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="78d3f-130">Varsayılan olarak, bir uyarı EF çekirdek oturum zaman dahil işleçleri yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="78d3f-130">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="78d3f-131">Bkz: [günlüğü](../miscellaneous/logging.md) günlük çıktısı görüntüleme hakkında daha fazla bilgi.</span><span class="sxs-lookup"><span data-stu-id="78d3f-131">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="78d3f-132">Ekle işleci throw veya hiçbir şey yapma göz ardı davranışı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78d3f-132">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="78d3f-133">Bu genellikle, içeriğiniz - seçeneklerini ayarlama zaman yapılır `DbContext.OnConfiguring`, veya `Startup.cs` ASP.NET Core kullanıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="78d3f-133">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="78d3f-134">Açık yükleniyor</span><span class="sxs-lookup"><span data-stu-id="78d3f-134">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="78d3f-135">Bu özellik EF çekirdek 1.1 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="78d3f-135">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="78d3f-136">Bir gezinti özelliği aracılığıyla açıkça yükleyebilir `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="78d3f-136">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="78d3f-137">İlgili varlıklar döndürüyor ayrı bir sorgu yürüterek bir gezinti özelliği de açıkça yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78d3f-137">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="78d3f-138">Değişiklik izleme etkinleştirildi, varsa bir varlık yüklenirken EF çekirdek otomatik olarak zaten yüklenmiş herhangi bir varlık başvurmak için yeni yüklenen entitiy Gezinti özelliklerini ayarlamak ve başvurmak için daha önce yüklenmiş varlıklar Gezinti özelliklerini ayarlama Yeni yüklenen varlık.</span><span class="sxs-lookup"><span data-stu-id="78d3f-138">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="78d3f-139">İlgili varlıklar sorgulama</span><span class="sxs-lookup"><span data-stu-id="78d3f-139">Querying related entities</span></span>

<span data-ttu-id="78d3f-140">Ayrıca bir gezinti özelliği içeriğini temsil eden bir LINQ Sorgu alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78d3f-140">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="78d3f-141">Bu belleğe yüklemeden ilgili varlıklar üzerinde bir toplama işleci çalıştırma gibi şeyler olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="78d3f-141">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="78d3f-142">Ayrıca, hangi ilgili varlıklar belleğe yüklenen filtre uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78d3f-142">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="78d3f-143">yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="78d3f-143">Lazy loading</span></span>

<span data-ttu-id="78d3f-144">Yavaş yükleniyor EF çekirdek tarafından henüz desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="78d3f-144">Lazy loading is not yet supported by EF Core.</span></span> <span data-ttu-id="78d3f-145">Görüntüleyebileceğiniz [bizim biriktirme listesi üzerinde öğe yavaş yüklenirken](https://github.com/aspnet/EntityFramework/issues/3797) bu özellik izlemek için.</span><span class="sxs-lookup"><span data-stu-id="78d3f-145">You can view the [lazy loading item on our backlog](https://github.com/aspnet/EntityFramework/issues/3797) to track this feature.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="78d3f-146">İlgili tüm verileri ve seri hale getirme</span><span class="sxs-lookup"><span data-stu-id="78d3f-146">Related data and serialization</span></span>

<span data-ttu-id="78d3f-147">EF çekirdek işlem otomatik olarak düzeltme yukarı Gezinti özellikleri, döngü ile nesne grafiğinde düşebilir olduğundan.</span><span class="sxs-lookup"><span data-stu-id="78d3f-147">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="78d3f-148">Örneğin, bir blog ve yükleme gönderileri gönderileri koleksiyonu başvuruda bulunan bir blog nesnesinde sonuçlanır ilişkilidir.</span><span class="sxs-lookup"><span data-stu-id="78d3f-148">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="78d3f-149">Bu gönderileri her bir başvuru geri blog sahip olur.</span><span class="sxs-lookup"><span data-stu-id="78d3f-149">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="78d3f-150">Bazı serileştirme çerçeveler gibi döngüleri izin vermez.</span><span class="sxs-lookup"><span data-stu-id="78d3f-150">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="78d3f-151">Örneğin, bir döngü encoutered ise Json.NET şu özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="78d3f-151">For example, Json.NET will throw the following exception if a cycle is encoutered.</span></span>

> <span data-ttu-id="78d3f-152">Newtonsoft.Json.JsonSerializationException: Kendi kendine başvuran döngü 'Blog' özelliği için 'MyApplication.Models.Blog' türüyle algılandı.</span><span class="sxs-lookup"><span data-stu-id="78d3f-152">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="78d3f-153">ASP.NET Core kullanıyorsanız, nesne grafiğinde bulduğu döngüleri yoksaymak için Json.NET yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78d3f-153">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="78d3f-154">Bu yapılır `ConfigureServices(...)` yönteminde `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="78d3f-154">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
