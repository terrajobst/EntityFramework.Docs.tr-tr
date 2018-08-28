---
title: DbContext - EF6 ile çalışma
author: divega
ms.date: 2016-10-23
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: f95f503c4e40e65503d5af0c1b686d0055728bfe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998152"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="b097c-102">DbContext ile çalışma</span><span class="sxs-lookup"><span data-stu-id="b097c-102">Working with DbContext</span></span>

<span data-ttu-id="b097c-103">Entity Framework sorgulama, ekleme, güncelleştirme ve .NET nesneleri kullanarak verileri silme olarak kullanabilmek için öncelikle gerekir [Model oluşturma](~/ef6/modeling/index.md) varlıkları ve modelinizi bir veritabanındaki tablolar için tanımlanan ilişkileri eşler.</span><span class="sxs-lookup"><span data-stu-id="b097c-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="b097c-104">Bir modeli oluşturduktan sonra uygulamanızı etkileşim birincil sınıf, `System.Data.Entity.DbContext` (genellikle bağlam sınıfını adlandırılır).</span><span class="sxs-lookup"><span data-stu-id="b097c-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="b097c-105">Bir Modeli'ne için ilişkili bir DbContext kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b097c-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="b097c-106">Yazma ve sorgular yürütme</span><span class="sxs-lookup"><span data-stu-id="b097c-106">Write and execute queries</span></span>   
- <span data-ttu-id="b097c-107">Sorgu sonuçları varlık nesnesi depolanabildiği</span><span class="sxs-lookup"><span data-stu-id="b097c-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="b097c-108">Bu nesnelere yapılan değişiklikleri izleme</span><span class="sxs-lookup"><span data-stu-id="b097c-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="b097c-109">Nesne değişikliklerini geri veritabanında kalıcı hale</span><span class="sxs-lookup"><span data-stu-id="b097c-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="b097c-110">Nesneleri bellekte bir UI denetimine bağlama</span><span class="sxs-lookup"><span data-stu-id="b097c-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="b097c-111">Bu sayfa, bağlam sınıfını yönetme hakkında rehberlik sağlar.</span><span class="sxs-lookup"><span data-stu-id="b097c-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="b097c-112">DbContext türetilmiş bir sınıf tanımlama</span><span class="sxs-lookup"><span data-stu-id="b097c-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="b097c-113">Bağlamı ile çalışmak için önerilen yöntem bağlamında belirtilen varlık koleksiyonları temsil eden olan DB özellikleri gösterir ve DbContext türetilen bir sınıf tanımlamaktır.</span><span class="sxs-lookup"><span data-stu-id="b097c-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="b097c-114">EF Designer ile çalışıyorsanız, bağlam sizin için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b097c-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="b097c-115">Code First ile çalışıyorsanız, tipik olarak bağlam kendi yazdığınız.</span><span class="sxs-lookup"><span data-stu-id="b097c-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="b097c-116">Bir bağlam aldıktan sonra sorgulama, ekleme (kullanarak `Add` veya `Attach` yöntemleri) veya kaldırma (kullanarak `Remove`) bağlamında bu özellikleri aracılığıyla varlıklar.</span><span class="sxs-lookup"><span data-stu-id="b097c-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="b097c-117">Erişim bir `DbSet` bir bağlam nesnesi özelliği belirtilen türün tüm varlıklarını döndüren başlayan bir sorgu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="b097c-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="b097c-118">Yalnızca bir özellik erişim sorgu yürütülmez unutmayın.</span><span class="sxs-lookup"><span data-stu-id="b097c-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="b097c-119">Bir sorgu yürütüldüğü zaman:</span><span class="sxs-lookup"><span data-stu-id="b097c-119">A query is executed when:</span></span>  

- <span data-ttu-id="b097c-120">Tarafından numaralandırılmış bir `foreach` (C#) veya `For Each` deyimi (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="b097c-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="b097c-121">Bir toplama işlemi tarafından gibi numaralandırılana `ToArray`, `ToDictionary`, veya `ToList`.</span><span class="sxs-lookup"><span data-stu-id="b097c-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="b097c-122">Gibi LINQ işleçleri `First` veya `Any` en dıştaki sorgunun parçası belirtilir.</span><span class="sxs-lookup"><span data-stu-id="b097c-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="b097c-123">Aşağıdaki yöntemlerden birini çağrılır: `Load` genişletme yöntemi `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, ve `DbSet<T>.Find`, belirtilen anahtara sahip bir varlık zaten yüklenmiş bir bağlamda bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="b097c-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="b097c-124">Ömür</span><span class="sxs-lookup"><span data-stu-id="b097c-124">Lifetime</span></span>  

<span data-ttu-id="b097c-125">Örneği oluşturulduğunda ve örnek ya da elden atık olarak toplanmış veya sona erer bağlamı ömrünü başlar.</span><span class="sxs-lookup"><span data-stu-id="b097c-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="b097c-126">Kullanım **kullanarak** bağlam bloğunun sonunda çıkarılması için denetimleri tüm kaynakları istiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="b097c-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="b097c-127">Kullanırken **kullanarak**, derleyici bir try/finally bloğu otomatik olarak oluşturur ve dispose işlemini çağırır **son** blok.</span><span class="sxs-lookup"><span data-stu-id="b097c-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="b097c-128">Bağlam ömrü hakkında karar verirken bazı genel yönergeler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="b097c-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="b097c-129">Web uygulamaları ile çalışırken, istek başına bir bağlam örneği kullanın.</span><span class="sxs-lookup"><span data-stu-id="b097c-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="b097c-130">Windows Presentation Foundation (WPF) veya Windows Forms ile çalışırken, bir formda bir bağlam örneği kullanın.</span><span class="sxs-lookup"><span data-stu-id="b097c-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="b097c-131">Bu değişiklik izleme işlevselliği kullanmanıza olanak tanır, bağlam sağlar.</span><span class="sxs-lookup"><span data-stu-id="b097c-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="b097c-132">Bağlam örneği bir bağımlılık ekleme kapsayıcısını tarafından oluşturduysanız, genellikle bağlamı atmayı kapsayıcının sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="b097c-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="b097c-133">Uygulama kodunda bağlamı oluşturduysanız, artık gerekli olmadığında bağlamında atmayı unutmayın.</span><span class="sxs-lookup"><span data-stu-id="b097c-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="b097c-134">Uzun süre çalışan bağlamı ile çalışırken aşağıdakileri göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="b097c-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="b097c-135">Daha fazla nesne ve başvurularını belleğe yük bağlamı bellek tüketimi hızlı bir şekilde artırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b097c-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="b097c-136">Bu durum, performans sorunlarına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="b097c-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="b097c-137">Bağlam, iş parçacığı açısından güvenli değildir, bu nedenle, iş üzerindeki eşzamanlı olarak yapılması birden çok iş parçacığı arasında Paylaşılmaması gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="b097c-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="b097c-138">Bir özel durum bağlamı kurtarılamaz bir duruma gelmesine neden olursa, tüm uygulama sonlandırabilir.</span><span class="sxs-lookup"><span data-stu-id="b097c-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="b097c-139">Verilerin ne zaman sorgulanabilir ve güncelleştirilmiş saat arasındaki boşluk büyüdükçe eşzamanlılık ile ilgili bir sorunla karşılaşırsanız çalıştırma olasılığını artırın.</span><span class="sxs-lookup"><span data-stu-id="b097c-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="b097c-140">Bağlantıları</span><span class="sxs-lookup"><span data-stu-id="b097c-140">Connections</span></span>  

<span data-ttu-id="b097c-141">Varsayılan olarak, içerik veritabanı bağlantılarını yönetir.</span><span class="sxs-lookup"><span data-stu-id="b097c-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="b097c-142">Bağlamı açar ve gerektiğinde bağlantıları kapatır.</span><span class="sxs-lookup"><span data-stu-id="b097c-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="b097c-143">Örneğin, içeriği bir sorgu yürütmek için bir bağlantı açar ve tüm sonuç kümelerindeki işlendiğinde ardından bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="b097c-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="b097c-144">Bağlantıyı açar ve kapandığında üzerinde daha fazla denetime sahip olmasını istediğiniz zaman durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="b097c-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="b097c-145">Örneğin, SQL Server Compact ile çalışırken, genellikle veritabanı ayrı açık bağlantı ömrü boyunca uygulama performansını korumak için önerilir.</span><span class="sxs-lookup"><span data-stu-id="b097c-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="b097c-146">Kullanarak bu işlemi el ile yönetebilirsiniz `Connection` özelliği.</span><span class="sxs-lookup"><span data-stu-id="b097c-146">You can manage this process manually by using the `Connection` property.</span></span>  
