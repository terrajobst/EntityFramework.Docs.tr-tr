---
title: EF Core - DbContext yapılandırma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 316d363d4a1b8a909efc1c32b492280c0d16cb4e
ms.sourcegitcommit: 960e42a01b3a2f76da82e074f64f52252a8afecc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65405214"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="c54ed-102">DbContext yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c54ed-102">Configuring a DbContext</span></span>

<span data-ttu-id="c54ed-103">Yapılandırma temel düzenlerden Bu makale bir `DbContext` aracılığıyla bir `DbContextOptions` belirli bir EF Core sağlayıcısını ve isteğe bağlı davranışları kullanarak bir veritabanına bağlanmak için.</span><span class="sxs-lookup"><span data-stu-id="c54ed-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="c54ed-104">Tasarım zamanında DbContext yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c54ed-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="c54ed-105">EF Core tasarım zamanı araçlarını gibi [geçişler](xref:core/managing-schemas/migrations/index) bulmak ve bir çalışma örneğini oluşturmak gereken bir `DbContext` uygulamanın varlık türleri ve bunların veritabanı şemasına nasıl eşleneceğine hakkındaki ayrıntıları toplamak için türü.</span><span class="sxs-lookup"><span data-stu-id="c54ed-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="c54ed-106">Aracı bir kolayca oluşturabilir sürece bu işlem otomatik olabilir `DbContext` şekilde, benzer şekilde nasıl, çalışma zamanında yapılandırılır için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="c54ed-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="c54ed-107">While için gerekli yapılandırma bilgileri sağlayan herhangi bir desen `DbContext` çalışma zamanında, kullanılmasını araçları çalışabilir bir `DbContext` tasarım zamanında yalnızca sınırlı sayıda desenleri ile çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="c54ed-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="c54ed-108">Bunlar daha ayrıntılı olarak ele alınmaktadır [tasarım zamanı bağlam oluşturma](xref:core/miscellaneous/cli/dbcontext-creation) bölümü.</span><span class="sxs-lookup"><span data-stu-id="c54ed-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="c54ed-109">DbContextOptions yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c54ed-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="c54ed-110">`DbContext` bir örneğine sahip olması `DbContextOptions` herhangi bir çalışmayı gerçekleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="c54ed-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="c54ed-111">`DbContextOptions` Örneği yapılandırma bilgileri gibi yapar:</span><span class="sxs-lookup"><span data-stu-id="c54ed-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="c54ed-112">Veritabanı sağlayıcısı kullanmak için genellikle seçili bir yöntemi çağırarak `UseSqlServer` veya `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="c54ed-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="c54ed-113">Bu uzantı yöntemleri gibi ilgili sağlayıcı paketi gereken `Microsoft.EntityFrameworkCore.SqlServer` veya `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="c54ed-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="c54ed-114">Yöntemleri tanımlanan `Microsoft.EntityFrameworkCore` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="c54ed-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="c54ed-115">Herhangi bir gerekli bağlantı dizesini veya veritabanı örneğinin tanımlayıcı genellikle geçirilen bağımsız değişken olarak yukarıda belirtilen sağlayıcısı seçme yöntemi</span><span class="sxs-lookup"><span data-stu-id="c54ed-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="c54ed-116">Genellikle sağlayıcısı seçme yöntemi çağrısı içinde da zincirleme tüm sağlayıcısı düzeyi isteğe bağlı davranışını seçiciler</span><span class="sxs-lookup"><span data-stu-id="c54ed-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="c54ed-117">Sağlayıcı Seçici yöntemi önce veya sonra genellikle zincirleme tüm genel EF Core davranışı seçiciler</span><span class="sxs-lookup"><span data-stu-id="c54ed-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="c54ed-118">Aşağıdaki örnek yapılandırır `DbContextOptions` ve SQL Server sağlayıcıyı kullanmak için bir bağlantı bulunan `connectionString` değişkeni, bir sağlayıcı düzeyi komut zaman aşımı ve içinde yürütülen tüm sorguları yapan bir EF Core davranışı Seçici `DbContext` [Hayır izleme](xref:core/querying/tracking#no-tracking-queries) varsayılan olarak:</span><span class="sxs-lookup"><span data-stu-id="c54ed-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="c54ed-119">Sağlayıcı Seçici ve yukarıda belirtilen diğer davranışı Seçici yöntemleri üzerinde genişletme yöntemleri olan `DbContextOptions` veya sağlayıcıya özgü seçenek sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c54ed-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="c54ed-120">Bir ad alanı sağlamak için ihtiyacınız olan bu genişletme yöntemlerini erişimi için (genellikle `Microsoft.EntityFrameworkCore`), kapsam ve projede ek paket bağımlılıklarını içerir.</span><span class="sxs-lookup"><span data-stu-id="c54ed-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="c54ed-121">`DbContextOptions` İçin sağlanan `DbContext` kılarak `OnConfiguring` yöntem veya oluşturucu bağımsız değişkeni aracılığıyla harici olarak.</span><span class="sxs-lookup"><span data-stu-id="c54ed-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="c54ed-122">Her ikisi de kullanılıyorsa `OnConfiguring` son uygulanan ve oluşturucu bağımsız değişkeni için sağlanan seçenekleri geçersiz kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c54ed-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="c54ed-123">Oluşturucu bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="c54ed-123">Constructor argument</span></span>

<span data-ttu-id="c54ed-124">Bağlam Kod Oluşturucu ile:</span><span class="sxs-lookup"><span data-stu-id="c54ed-124">Context code with constructor:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="c54ed-125">DbContext, temel oluşturucu, genel olmayan sürümü de kabul eder `DbContextOptions`, ancak genel olmayan sürümüyle önerilmez birden çok içerik türü olan uygulamalar için.</span><span class="sxs-lookup"><span data-stu-id="c54ed-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="c54ed-126">Oluşturucu bağımsız değişkeninden başlatmak için uygulama kodu:</span><span class="sxs-lookup"><span data-stu-id="c54ed-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="c54ed-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="c54ed-127">OnConfiguring</span></span>

<span data-ttu-id="c54ed-128">Bağlam koduyla `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="c54ed-128">Context code with `OnConfiguring`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

<span data-ttu-id="c54ed-129">Uygulama kodu başlatmak için bir `DbContext` kullanan `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="c54ed-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="c54ed-130">Testlerin tam bir veritabanı hedef sürece bu yaklaşımın kendisi test için uygun olmayan.</span><span class="sxs-lookup"><span data-stu-id="c54ed-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="c54ed-131">DbContext bağımlılık ekleme ile kullanma</span><span class="sxs-lookup"><span data-stu-id="c54ed-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="c54ed-132">EF Core destekler kullanarak `DbContext` bağımlılık ekleme kapsayıcısına sahip.</span><span class="sxs-lookup"><span data-stu-id="c54ed-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="c54ed-133">DbContext türünüz kullanarak hizmet kapsayıcıya eklenebilir `AddDbContext<TContext>` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c54ed-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="c54ed-134">`AddDbContext<TContext>` Her iki DbContext türünüzü hale getirecek `TContext`ve karşılık gelen `DbContextOptions<TContext>` hizmet kapsayıcısından yerleştirme için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c54ed-134">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="c54ed-135">Bkz: [daha fazla okuma](#more-reading) aşağıdaki bağımlılık ekleme hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c54ed-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="c54ed-136">Ekleme `Dbcontext` bağımlılık ekleme:</span><span class="sxs-lookup"><span data-stu-id="c54ed-136">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="c54ed-137">Bu ekleme gerektirir bir [oluşturucu bağımsız değişkeni](#constructor-argument) kabul eden DbContext türünüz için `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="c54ed-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="c54ed-138">Bağlam kodu:</span><span class="sxs-lookup"><span data-stu-id="c54ed-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="c54ed-139">Uygulama kodunda (ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="c54ed-139">Application code (in ASP.NET Core):</span></span>

``` csharp
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

<span data-ttu-id="c54ed-140">Uygulama kodu (doğrudan kullanarak daha az ortak ServiceProvider):</span><span class="sxs-lookup"><span data-stu-id="c54ed-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="c54ed-141">DbContext iş parçacığı oluşturma sorunları önleme</span><span class="sxs-lookup"><span data-stu-id="c54ed-141">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="c54ed-142">Entity Framework Core aynı çalıştırılan birden çok paralel işlemleri desteklemez `DbContext` örneği.</span><span class="sxs-lookup"><span data-stu-id="c54ed-142">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="c54ed-143">Bu, hem zaman uyumsuz sorgu Paralel yürütme hem de açık eş zamanlı kullanım birden çok iş parçacığından içerir.</span><span class="sxs-lookup"><span data-stu-id="c54ed-143">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="c54ed-144">Bu nedenle, her zaman `await` zaman uyumsuz hemen çağırır veya ayrı kullanın `DbContext` örnekleri, paralel olarak yürütülen işlemler için.</span><span class="sxs-lookup"><span data-stu-id="c54ed-144">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="c54ed-145">EF Core kullanma girişimi algıladığında bir `DbContext` göreceğiniz eşzamanlı olarak, örnek bir `InvalidOperationException` şöyle bir ileti ile:</span><span class="sxs-lookup"><span data-stu-id="c54ed-145">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span> 

> <span data-ttu-id="c54ed-146">Bu bağlamdaki bir önceki işlemi tamamlandı önce ikinci bir işlem başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="c54ed-146">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="c54ed-147">Bu genellikle farklı iş parçacıkları tarafından DbContext aynı örneği kullanarak, ancak örnek üyeleri iş parçacığı güvenli olduğu garanti edilmez neden olur.</span><span class="sxs-lookup"><span data-stu-id="c54ed-147">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="c54ed-148">Eş zamanlı erişim algılanmayan gittiğinde, tanımsız davranış, uygulama kilitlenmesi ve veri bozulmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="c54ed-148">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="c54ed-149">Aynı inadvernetly neden eş zamanlı erişim için yaygın hatalar vardır `DbContext` örneği:</span><span class="sxs-lookup"><span data-stu-id="c54ed-149">There are common mistakes that can inadvernetly cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="c54ed-150">Aynı Dbcontext'e üzerinde herhangi bir işlem başlatmadan önce zaman uyumsuz bir işlemin tamamlanmasını beklemek unutmak</span><span class="sxs-lookup"><span data-stu-id="c54ed-150">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="c54ed-151">EF Core, engelleyici olmayan bir yolla veritabanına erişen işlemler başlatmak zaman uyumsuz yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="c54ed-151">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="c54ed-152">Ancak bir çağıranın tamamlandığında, aşağıdaki yöntemlerden birini await değil ve diğer işlemleri gerçekleştirmeye devam eder, `DbContext`, durumu `DbContext` olabilir (ve büyük olasılıkla olabilir) bozuk.</span><span class="sxs-lookup"><span data-stu-id="c54ed-152">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="c54ed-153">Her zaman hemen EF Core zaman uyumsuz yöntemler bekler.</span><span class="sxs-lookup"><span data-stu-id="c54ed-153">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="c54ed-154">Örtük olarak bağımlılık ekleme aracılığıyla birden çok iş parçacığı arasında DbContext örnekleri paylaşma</span><span class="sxs-lookup"><span data-stu-id="c54ed-154">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="c54ed-155">[ `AddDbContext` ](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) Genişletme yöntemi kaydeder `DbContext` ile türleri bir [kapsamlı ömrü](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="c54ed-155">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="c54ed-156">Bu yalnızca bir iş parçacığı belirli bir zamanda her istemci isteği yürütmeden olduğundan ve her isteğin ayrı bağımlılık ekleme kapsamı aldığından ASP.NET Core uygulamaları eş zamanlı erişim sorunları karşı (ve bu nedenle ayrı `DbContext` Örnek).</span><span class="sxs-lookup"><span data-stu-id="c54ed-156">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="c54ed-157">Birden çok iş parçacığı içinde paralell açıkça yürütülen herhangi bir kod, ancak emin olmalısınız `DbContext` örnekleri olmayan hiç olmadığı kadar accesed eşzamanlı olarak.</span><span class="sxs-lookup"><span data-stu-id="c54ed-157">However any code that explicitly executes multiple threads in paralell should ensure that `DbContext` instances aren't ever accesed concurrently.</span></span>

<span data-ttu-id="c54ed-158">Bağımlılık ekleme kullanılarak, bu iki bağlam kapsamlı ve oluşturma kapsamı olarak kaydederek gerçekleştirilebilir (kullanarak `IServiceScopeFactory`) kaydederek veya her bir iş parçacığı için `DbContext` geçici olarak (aşırı yüklemesini kullanarak `AddDbContext` bir alır`ServiceLifetime` parametresi).</span><span class="sxs-lookup"><span data-stu-id="c54ed-158">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="c54ed-159">Daha fazla okuma</span><span class="sxs-lookup"><span data-stu-id="c54ed-159">More reading</span></span>

* <span data-ttu-id="c54ed-160">Okuma [Başlarken üzerinde ASP.NET Core](../get-started/aspnetcore/index.md) EF ASP.NET Core ile kullanma hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c54ed-160">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="c54ed-161">Okuma [bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) DI kullanma hakkında daha fazla bilgi edinmek için.</span><span class="sxs-lookup"><span data-stu-id="c54ed-161">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="c54ed-162">Okuma [test](testing/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c54ed-162">Read [Testing](testing/index.md) for more information.</span></span>
