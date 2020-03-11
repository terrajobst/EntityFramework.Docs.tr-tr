---
title: DbContext-EF6 ile çalışma
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416324"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="1e207-102">DbContext ile Çalışma</span><span class="sxs-lookup"><span data-stu-id="1e207-102">Working with DbContext</span></span>

<span data-ttu-id="1e207-103">.NET nesnelerini kullanarak verileri sorgulamak, eklemek, güncelleştirmek ve silmek için, önce modelinizde tanımlanan varlıkları ve ilişkileri bir veritabanındaki tablolarla eşleyen [bir model oluşturmanız](~/ef6/modeling/index.md) gerekir. Entity Framework</span><span class="sxs-lookup"><span data-stu-id="1e207-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="1e207-104">Bir modelinize sahip olduğunuzda, uygulamanızın etkileşimde bulunduğu birincil sınıf `System.Data.Entity.DbContext` (genellikle bağlam sınıfı olarak adlandırılır).</span><span class="sxs-lookup"><span data-stu-id="1e207-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="1e207-105">Bir modelle ilişkili bir DbContext kullanarak şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1e207-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="1e207-106">Sorguları yazma ve yürütme</span><span class="sxs-lookup"><span data-stu-id="1e207-106">Write and execute queries</span></span>   
- <span data-ttu-id="1e207-107">Sorgu sonuçlarını varlık nesneleri olarak gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="1e207-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="1e207-108">Bu nesnelerde yapılan değişiklikleri izleme</span><span class="sxs-lookup"><span data-stu-id="1e207-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="1e207-109">Nesne değişikliklerini veritabanında geri Sürdür</span><span class="sxs-lookup"><span data-stu-id="1e207-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="1e207-110">Nesneleri bellekten UI denetimlerine bağlama</span><span class="sxs-lookup"><span data-stu-id="1e207-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="1e207-111">Bu sayfa, bağlam sınıfının nasıl yönetilebileceğine ilişkin yönergeler sunar.</span><span class="sxs-lookup"><span data-stu-id="1e207-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="1e207-112">DbContext türetilmiş sınıfı tanımlama</span><span class="sxs-lookup"><span data-stu-id="1e207-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="1e207-113">Bağlam ile çalışmanın önerilen yolu, DbContext 'ten türetilen bir sınıf tanımlamaktır ve bağlamda belirtilen varlıkların koleksiyonlarını temsil eden DbSet özelliklerini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="1e207-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="1e207-114">EF Designer ile çalışıyorsanız, bağlam sizin için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1e207-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="1e207-115">Code First ile çalışıyorsanız, genellikle bağlamı yazarsınız.</span><span class="sxs-lookup"><span data-stu-id="1e207-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="1e207-116">Bir bağlama sahip olduktan sonra, bu özellikler aracılığıyla bağlam içindeki `Attach` `Add` (`Remove`) veya bağlama () varlıklarını sorgular.</span><span class="sxs-lookup"><span data-stu-id="1e207-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="1e207-117">Bağlam nesnesindeki bir `DbSet` özelliğine erişilmesi, belirtilen türdeki tüm varlıkları döndüren bir başlangıç sorgusunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="1e207-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="1e207-118">Yalnızca bir özelliğe erişmenin sorguyu yürütmeyeceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1e207-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="1e207-119">Şu durumlarda bir sorgu yürütülür:</span><span class="sxs-lookup"><span data-stu-id="1e207-119">A query is executed when:</span></span>  

- <span data-ttu-id="1e207-120">`foreach` (C#) veya `For Each` (Visual Basic) ifadesiyle numaralandırılır.</span><span class="sxs-lookup"><span data-stu-id="1e207-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="1e207-121">`ToArray`, `ToDictionary`veya `ToList`gibi bir koleksiyon işlemi tarafından numaralandırılır.</span><span class="sxs-lookup"><span data-stu-id="1e207-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="1e207-122">`First` veya `Any` gibi LINQ işleçleri sorgunun en dıştaki bölümünde belirtilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1e207-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="1e207-123">Aşağıdaki yöntemlerden biri çağrılır: belirtilen anahtara sahip bir varlık bağlamda zaten yüklü değilse `Load` uzantı yöntemi, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`ve `DbSet<T>.Find`.</span><span class="sxs-lookup"><span data-stu-id="1e207-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="1e207-124">Ömür</span><span class="sxs-lookup"><span data-stu-id="1e207-124">Lifetime</span></span>  

<span data-ttu-id="1e207-125">Bağlam yaşam süresi, örnek oluşturulduğunda başlar ve örnek atıldığında ya da atık toplanmış olduğunda sona erer.</span><span class="sxs-lookup"><span data-stu-id="1e207-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="1e207-126">Bağlam denetimlerine ait tüm kaynakların bloğun sonunda atıldığından emin olmak istiyorsanız **kullanarak** kullanın.</span><span class="sxs-lookup"><span data-stu-id="1e207-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="1e207-127">**Using**kullandığınızda, derleyici otomatik olarak bir try/finally bloğu oluşturur ve **finally** bloğunda Dispose çağırır.</span><span class="sxs-lookup"><span data-stu-id="1e207-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="1e207-128">Bağlam ömrü üzerinde karar verirken bazı genel yönergeler aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="1e207-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="1e207-129">Web uygulamalarıyla çalışırken, istek başına bir bağlam örneği kullanın.</span><span class="sxs-lookup"><span data-stu-id="1e207-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="1e207-130">Windows Presentation Foundation (WPF) veya Windows Forms çalışırken, form başına bir bağlam örneği kullanın.</span><span class="sxs-lookup"><span data-stu-id="1e207-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="1e207-131">Bu, içeriğin sağladığı değişiklik izleme işlevini kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="1e207-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="1e207-132">Bağlam örneği bir bağımlılık ekleme kapsayıcısı tarafından oluşturulduysa, genellikle kapsayıcının, bağlamı atma sorumluluğunda olur.</span><span class="sxs-lookup"><span data-stu-id="1e207-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="1e207-133">Bağlam uygulama kodunda oluşturulduysa, artık gerekli olmadığında bağlamı atmayı unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1e207-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="1e207-134">Uzun süre çalışan bağlamla çalışırken aşağıdakileri göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="1e207-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="1e207-135">Daha fazla nesne ve başvuruları belleğe yüklerken, içeriğin bellek tüketimi hızla artırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1e207-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="1e207-136">Bu durum performans sorunlarına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1e207-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="1e207-137">Bağlam, iş parçacığı açısından güvenli değildir, bu nedenle aynı anda üzerinde çalışan birden çok iş parçacığı arasında paylaşılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="1e207-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="1e207-138">Bir özel durum bağlamın kurtarılamaz durumda olmasına neden olursa tüm uygulama sonlandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1e207-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="1e207-139">Eşzamanlılık ile ilgili sorunlara çalıştırmanın olasılığı, verilerin sorgulandığı ve güncelleştirildiği saat arasındaki boşluk olarak artar.</span><span class="sxs-lookup"><span data-stu-id="1e207-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="1e207-140">Bağlantılar</span><span class="sxs-lookup"><span data-stu-id="1e207-140">Connections</span></span>  

<span data-ttu-id="1e207-141">Varsayılan olarak, bağlam veritabanıyla bağlantıları yönetir.</span><span class="sxs-lookup"><span data-stu-id="1e207-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="1e207-142">Bağlam, gerektiğinde bağlantıları açar ve kapatır.</span><span class="sxs-lookup"><span data-stu-id="1e207-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="1e207-143">Örneğin, bağlam bir sorguyu yürütmek için bir bağlantı açar ve ardından tüm sonuç kümeleri işlendiğinde bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="1e207-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="1e207-144">Bağlantı açılıp kapanışında daha fazla denetime sahip olmak istediğiniz durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="1e207-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="1e207-145">Örneğin, SQL Server Compact ile çalışırken, performansı artırmak için uygulamanın kullanım ömrü boyunca veritabanına ayrı bir açık bağlantı sağlamak önerilir.</span><span class="sxs-lookup"><span data-stu-id="1e207-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="1e207-146">`Connection` özelliğini kullanarak bu işlemi el ile yönetebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1e207-146">You can manage this process manually by using the `Connection` property.</span></span>  
