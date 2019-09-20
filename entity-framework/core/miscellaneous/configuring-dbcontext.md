---
title: DbContext yapılandırma EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 734acad86e364abbfd1522fe79d4a847b1acfb52
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149027"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="6b000-102">DbContext yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6b000-102">Configuring a DbContext</span></span>

<span data-ttu-id="6b000-103">Bu makalede belirli bir EF Core sağlayıcısı ve isteğe `DbContext` bağlı davranışları `DbContextOptions` kullanarak bir veritabanına bağlanmak için bir ile yapılandırma için temel desenler gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6b000-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="6b000-104">Tasarım zamanı DbContext yapılandırması</span><span class="sxs-lookup"><span data-stu-id="6b000-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="6b000-105">EF Core tasarım zamanı araçlarının, uygulamanın varlık türleriyle ilgili ayrıntıları ve bir veritabanı şemasına nasıl eşlendikleri hakkında bilgi toplamak için `DbContext` bir türün çalışma örneğini bulması ve oluşturabilmeleri [gerekir.](xref:core/managing-schemas/migrations/index)</span><span class="sxs-lookup"><span data-stu-id="6b000-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="6b000-106">Bu işlem, araç, `DbContext` çalışma zamanında nasıl yapılandırılacağından benzer şekilde yapılandırılacak şekilde kolayca oluşturabileceğiniz sürece otomatik olabilir.</span><span class="sxs-lookup"><span data-stu-id="6b000-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="6b000-107">İçin gerekli yapılandırma bilgilerini sağlayan herhangi bir `DbContext` `DbContext` desen çalışma zamanında çalışsa da, tasarım zamanı için kullanılması gereken araçlar yalnızca sınırlı sayıda desenle çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="6b000-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="6b000-108">Bunlar [Tasarım zamanı bağlam oluşturma](xref:core/miscellaneous/cli/dbcontext-creation) bölümünde daha ayrıntılı olarak ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="6b000-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="6b000-109">DbContextOptions 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6b000-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="6b000-110">`DbContext`herhangi bir çalışmayı gerçekleştirmek için `DbContextOptions` bir örneğine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6b000-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="6b000-111">Örnek `DbContextOptions` , şu şekilde yapılandırma bilgilerini taşır:</span><span class="sxs-lookup"><span data-stu-id="6b000-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="6b000-112">Kullanılacak veritabanı sağlayıcısı, genellikle `UseSqlServer` veya `UseSqlite`gibi bir yöntemi çağırarak seçilir.</span><span class="sxs-lookup"><span data-stu-id="6b000-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="6b000-113">Bu uzantı yöntemleri, `Microsoft.EntityFrameworkCore.SqlServer` veya `Microsoft.EntityFrameworkCore.Sqlite`gibi karşılık gelen sağlayıcı paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6b000-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="6b000-114">Yöntemler `Microsoft.EntityFrameworkCore` ad alanında tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6b000-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="6b000-115">Genellikle yukarıda bahsedilen sağlayıcı seçim yöntemine bir bağımsız değişken olarak geçirilen veritabanı örneğinin gerekli bağlantı dizesi veya tanımlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6b000-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="6b000-116">Tüm sağlayıcı düzeyi isteğe bağlı davranış seçicileri, genellikle sağlayıcı seçim yöntemine yapılan çağrının içinde de zincirleme</span><span class="sxs-lookup"><span data-stu-id="6b000-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="6b000-117">Genel EF Core davranış seçicileri, genellikle sağlayıcı Seçicisi yönteminden sonra veya önce zincirleme</span><span class="sxs-lookup"><span data-stu-id="6b000-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="6b000-118">Aşağıdaki örnek, SQL Server sağlayıcıyı `DbContextOptions` , `connectionString` değişkende yer alan bir bağlantıyı, sağlayıcı düzeyi komut zaman aşımını ve `DbContext` tüm sorguları ' de yürütülen bir EF Core davranış seçicisini kullanacak şekilde yapılandırır. Varsayılan olarak [izleme yok](xref:core/querying/tracking#no-tracking-queries) :</span><span class="sxs-lookup"><span data-stu-id="6b000-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="6b000-119">Yukarıda bahsedilen sağlayıcı Seçicisi yöntemleri ve diğer davranış Seçicisi yöntemleri, uzantı yöntemleri `DbContextOptions` veya sağlayıcıya özgü seçenek sınıflarıdır.</span><span class="sxs-lookup"><span data-stu-id="6b000-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="6b000-120">Bu uzantı yöntemlerine erişebilmek için, kapsamda bir ad alanına (genellikle `Microsoft.EntityFrameworkCore`) sahip olmanız ve ek paket bağımlılıklarını projeye eklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="6b000-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="6b000-121">, Yöntemi geçersiz kılarak `DbContext` veya dışarıdan bir Oluşturucu bağımsız değişkeni aracılığıyla sağlanabilir. `OnConfiguring` `DbContextOptions`</span><span class="sxs-lookup"><span data-stu-id="6b000-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="6b000-122">Her ikisi de kullanılırsa, `OnConfiguring` en son uygulanır ve Oluşturucu bağımsız değişkenine sağlanan seçeneklerin üzerine yazabilir.</span><span class="sxs-lookup"><span data-stu-id="6b000-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="6b000-123">Oluşturucu bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="6b000-123">Constructor argument</span></span>

<span data-ttu-id="6b000-124">Oluşturucuya sahip bağlam kodu:</span><span class="sxs-lookup"><span data-stu-id="6b000-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="6b000-125">DbContext 'in temel Oluşturucusu, genel `DbContextOptions`olmayan sürümünü de kabul eder, ancak birden çok bağlam türüne sahip uygulamalar için genel olmayan sürüm kullanılması önerilmez.</span><span class="sxs-lookup"><span data-stu-id="6b000-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="6b000-126">Oluşturucu bağımsız değişkeninden başlatılacak uygulama kodu:</span><span class="sxs-lookup"><span data-stu-id="6b000-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="6b000-127">Onyapılandırıyor</span><span class="sxs-lookup"><span data-stu-id="6b000-127">OnConfiguring</span></span>

<span data-ttu-id="6b000-128">Bağlam kodu `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="6b000-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="6b000-129">Öğesini başlatmak `DbContext` `OnConfiguring`için kullanılan uygulama kodu:</span><span class="sxs-lookup"><span data-stu-id="6b000-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="6b000-130">Testler tam veritabanını hedeflemez değilse, bu yaklaşım kendisini teste vermez.</span><span class="sxs-lookup"><span data-stu-id="6b000-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="6b000-131">Bağımlılık ekleme ile DbContext kullanma</span><span class="sxs-lookup"><span data-stu-id="6b000-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="6b000-132">EF Core, bağımlılık `DbContext` ekleme kapsayıcısı ile kullanmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="6b000-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="6b000-133">DbContext türü, `AddDbContext<TContext>` yöntemi kullanılarak hizmet kapsayıcısına eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="6b000-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="6b000-134">`AddDbContext<TContext>`hem DbContext türü, `TContext` `DbContextOptions<TContext>` hem de hizmet kapsayıcısından ekleme için kullanılabilir duruma gelir.</span><span class="sxs-lookup"><span data-stu-id="6b000-134">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="6b000-135">Bağımlılık ekleme hakkında ek bilgi için aşağıda [daha fazla okuma](#more-reading) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="6b000-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="6b000-136">`DbContext` To bağımlılığı ekleme:</span><span class="sxs-lookup"><span data-stu-id="6b000-136">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="6b000-137">Bu, kabul `DbContextOptions<TContext>`eden DbContext türüne bir [Oluşturucu bağımsız değişkeni](#constructor-argument) eklenmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6b000-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="6b000-138">Bağlam kodu:</span><span class="sxs-lookup"><span data-stu-id="6b000-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="6b000-139">Uygulama kodu (ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="6b000-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="6b000-140">Uygulama kodu (doğrudan ServiceProvider kullanılarak, daha az ortak):</span><span class="sxs-lookup"><span data-stu-id="6b000-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="6b000-141">DbContext iş parçacığı sorunlarından kaçınma</span><span class="sxs-lookup"><span data-stu-id="6b000-141">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="6b000-142">Entity Framework Core, aynı `DbContext` örnekte çalıştırılan birden çok paralel işlemi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="6b000-142">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="6b000-143">Bu, zaman uyumsuz sorguların paralel yürütmesini ve birden çok iş parçacığından açık olan eşzamanlı kullanımı içerir.</span><span class="sxs-lookup"><span data-stu-id="6b000-143">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="6b000-144">Bu nedenle, `await` her zaman zaman uyumsuz çağrılar hemen, `DbContext` paralel olarak yürütülen işlemler için ayrı örnekler kullanır.</span><span class="sxs-lookup"><span data-stu-id="6b000-144">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="6b000-145">EF Core aynı anda bir `DbContext` örneği kullanma girişimi algıladığında şöyle bir ileti `InvalidOperationException` görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="6b000-145">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span> 

> <span data-ttu-id="6b000-146">Önceki bir işlem tamamlanmadan önce bu bağlamda ikinci bir işlem başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="6b000-146">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="6b000-147">Bu genellikle aynı DbContext örneğini kullanan farklı iş parçacıklarından kaynaklanır, ancak örnek üyelerin iş parçacığı güvenli olduğu garanti edilmez.</span><span class="sxs-lookup"><span data-stu-id="6b000-147">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="6b000-148">Eşzamanlı erişim algılanmadıysa, tanımsız davranışa, uygulama kilitlenmelerine ve veri bozulmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="6b000-148">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="6b000-149">Aynı `DbContext` örnekte yanlışlıkla eşzamanlı erişime neden olabilecek yaygın hatalar vardır:</span><span class="sxs-lookup"><span data-stu-id="6b000-149">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="6b000-150">Aynı DbContext üzerinde başka bir işlem başlatmadan önce zaman uyumsuz bir işlemin tamamlanmasını beklemek</span><span class="sxs-lookup"><span data-stu-id="6b000-150">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="6b000-151">Zaman uyumsuz yöntemler, EF Core engellenmeyen bir şekilde veritabanına erişen işlemler başlatmasını etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="6b000-151">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="6b000-152">Ancak bir arayan, bu yöntemlerin birinin tamamlanmasını beklemez ve üzerinde `DbContext`başka işlemler gerçekleştirmeye devam etmez, durumu `DbContext` (ve büyük olasılıkla) bozulmuş olabilir.</span><span class="sxs-lookup"><span data-stu-id="6b000-152">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="6b000-153">Her zaman zaman uyumsuz yöntemleri hemen EF Core.</span><span class="sxs-lookup"><span data-stu-id="6b000-153">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="6b000-154">Bağımlılık ekleme aracılığıyla birden çok iş parçacığında DbContext örneklerini örtülü olarak paylaşma</span><span class="sxs-lookup"><span data-stu-id="6b000-154">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="6b000-155">Genişletme [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) yöntemi, varsayılan `DbContext` olarak [kapsamlı yaşam süresine](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) sahip türleri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="6b000-155">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="6b000-156">Bu, belirli bir zamanda her istemci isteğini yürüten tek bir iş parçacığı olduğundan ve her istek ayrı bir bağımlılık ekleme kapsamı (ve dolayısıyla ayrı `DbContext` bir bağımlılık ekleme kapsamı) aldığından, ASP.NET Core uygulamalardaki eşzamanlı erişim sorunlarından güvenlidir. örnek).</span><span class="sxs-lookup"><span data-stu-id="6b000-156">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="6b000-157">Ancak birden çok iş parçacığını paralel olarak yürüten herhangi bir kod, `DbContext` örneklere hiçbir zaman eş zamanlı olarak erişilmemesini sağlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="6b000-157">However any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="6b000-158">Bağımlılık ekleme 'yi kullanarak bu, her bir `IServiceScopeFactory` `DbContext` `AddDbContext` işparçacığıiçinbağlamıkapsamolarakkaydederek(kullanarak)yadageçiciolarakkaydederekeldeedilebilir.`ServiceLifetime` parametresi).</span><span class="sxs-lookup"><span data-stu-id="6b000-158">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="6b000-159">Daha fazla okuma</span><span class="sxs-lookup"><span data-stu-id="6b000-159">More reading</span></span>

* <span data-ttu-id="6b000-160">Dı kullanma hakkında daha fazla bilgi edinmek için [bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) konusunu okuyun.</span><span class="sxs-lookup"><span data-stu-id="6b000-160">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="6b000-161">Daha fazla bilgi için [testi](testing/index.md) okuyun.</span><span class="sxs-lookup"><span data-stu-id="6b000-161">Read [Testing](testing/index.md) for more information.</span></span>
