---
title: DbContext yapılandırma EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 53b38f288cd45e66d68ebcc3b6066646d59b0262
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136185"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="4a826-102">DbContext yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4a826-102">Configuring a DbContext</span></span>

<span data-ttu-id="4a826-103">Bu makalede, belirli bir EF Core sağlayıcısı ve isteğe bağlı davranışları kullanarak bir veritabanına bağlanmak için `DbContextOptions` aracılığıyla bir `DbContext` yapılandırmaya yönelik temel desenler gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="4a826-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="4a826-104">Tasarım zamanı DbContext yapılandırması</span><span class="sxs-lookup"><span data-stu-id="4a826-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="4a826-105">[Geçiş](xref:core/managing-schemas/migrations/index) gibi EF Core tasarım zamanı araçlarının, uygulamanın varlık türleriyle ilgili ayrıntıları ve bir veritabanı şemasına nasıl eşlendikleri hakkında bilgi toplamak için bir `DbContext` türünün çalışma örneğini bulması ve oluşturabilmeleri gerekir.</span><span class="sxs-lookup"><span data-stu-id="4a826-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="4a826-106">Bu işlem, araç, çalışma zamanında nasıl yapılandırılacağından benzer şekilde yapılandırılacak şekilde `DbContext` kolayca oluşturabileceğiniz sürece otomatik hale gelir.</span><span class="sxs-lookup"><span data-stu-id="4a826-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="4a826-107">`DbContext` gerekli yapılandırma bilgilerini sağlayan herhangi bir desen çalışma zamanında çalışsa da, tasarım zamanında `DbContext` kullanılması gereken araçlar yalnızca sınırlı sayıda desenle çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="4a826-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="4a826-108">Bunlar [Tasarım zamanı bağlam oluşturma](xref:core/miscellaneous/cli/dbcontext-creation) bölümünde daha ayrıntılı olarak ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="4a826-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="4a826-109">DbContextOptions 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4a826-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="4a826-110">`DbContext` herhangi bir çalışmayı gerçekleştirmek için `DbContextOptions` bir örneğine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4a826-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="4a826-111">`DbContextOptions` örnek, şu şekilde yapılandırma bilgilerini taşır:</span><span class="sxs-lookup"><span data-stu-id="4a826-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="4a826-112">Kullanılacak veritabanı sağlayıcısı, genellikle `UseSqlServer` veya `UseSqlite`gibi bir yöntemi çağırarak seçilir.</span><span class="sxs-lookup"><span data-stu-id="4a826-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="4a826-113">Bu uzantı yöntemleri, `Microsoft.EntityFrameworkCore.SqlServer` veya `Microsoft.EntityFrameworkCore.Sqlite`gibi karşılık gelen sağlayıcı paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4a826-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="4a826-114">Yöntemler `Microsoft.EntityFrameworkCore` ad alanında tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4a826-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="4a826-115">Genellikle yukarıda bahsedilen sağlayıcı seçim yöntemine bir bağımsız değişken olarak geçirilen veritabanı örneğinin gerekli bağlantı dizesi veya tanımlayıcısı</span><span class="sxs-lookup"><span data-stu-id="4a826-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="4a826-116">Tüm sağlayıcı düzeyi isteğe bağlı davranış seçicileri, genellikle sağlayıcı seçim yöntemine yapılan çağrının içinde de zincirleme</span><span class="sxs-lookup"><span data-stu-id="4a826-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="4a826-117">Genel EF Core davranış seçicileri, genellikle sağlayıcı Seçicisi yönteminden sonra veya önce zincirleme</span><span class="sxs-lookup"><span data-stu-id="4a826-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="4a826-118">Aşağıdaki örnek, `DbContextOptions` SQL Server sağlayıcıyı, `connectionString` değişkeninde yer alan bir bağlantıyı, sağlayıcı düzeyi komut zaman aşımını ve tüm sorguları varsayılan olarak `DbContext` [hiçbir izlemede](xref:core/querying/tracking#no-tracking-queries) yürütülen bir EF Core davranış seçicisini kullanacak şekilde yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="4a826-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="4a826-119">Yukarıda bahsedilen sağlayıcı seçici yöntemleri ve diğer davranış Seçicisi yöntemleri `DbContextOptions` veya sağlayıcıya özgü seçenek sınıflarında uzantı yöntemleridir.</span><span class="sxs-lookup"><span data-stu-id="4a826-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="4a826-120">Bu uzantı yöntemlerine erişebilmek için, kapsamda bir ad alanına (genellikle `Microsoft.EntityFrameworkCore`) sahip olmanız ve projeye ek paket bağımlılıklarını eklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="4a826-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="4a826-121">`DbContextOptions`, `OnConfiguring` yöntemi geçersiz kılınarak veya bir Oluşturucu bağımsız değişkeni aracılığıyla `DbContext` sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="4a826-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="4a826-122">Her ikisi de kullanılırsa `OnConfiguring` en son uygulanır ve Oluşturucu bağımsız değişkenine sağlanan seçeneklerin üzerine yazabilir.</span><span class="sxs-lookup"><span data-stu-id="4a826-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="4a826-123">Oluşturucu bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="4a826-123">Constructor argument</span></span>

<span data-ttu-id="4a826-124">Oluşturucunuzu aşağıdaki şekilde `DbContextOptions` kabul edebilir:</span><span class="sxs-lookup"><span data-stu-id="4a826-124">Your constructor can simply accept a `DbContextOptions` as follows:</span></span>

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
> <span data-ttu-id="4a826-125">DbContext 'in temel Oluşturucusu Ayrıca `DbContextOptions`genel olmayan sürümünü de kabul eder, ancak birden çok bağlam türüne sahip uygulamalar için genel olmayan sürüm kullanılması önerilmez.</span><span class="sxs-lookup"><span data-stu-id="4a826-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="4a826-126">Uygulamanız artık bir bağlamı örneklediğinde `DbContextOptions` şu şekilde geçirebilir:</span><span class="sxs-lookup"><span data-stu-id="4a826-126">Your application can now pass the `DbContextOptions` when instantiating a context, as follows:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="4a826-127">Onyapılandırıyor</span><span class="sxs-lookup"><span data-stu-id="4a826-127">OnConfiguring</span></span>

<span data-ttu-id="4a826-128">Ayrıca, bağlamı içinde `DbContextOptions` de başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4a826-128">You can also initialize the `DbContextOptions` within the context itself.</span></span> <span data-ttu-id="4a826-129">Temel yapılandırma için bu tekniği kullanabilseniz de, genellikle bir veritabanı bağlantı dizesi gibi, dışarıdaki bazı yapılandırma ayrıntılarını almanız gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="4a826-129">While you can use this technique for basic configuration, you will typically still need to get certain configuration details from the outside, e.g. a database connection string.</span></span> <span data-ttu-id="4a826-130">Bu, bir yapılandırma API 'SI veya başka yollarla yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="4a826-130">This can be done with a configuration API or any other means.</span></span>

<span data-ttu-id="4a826-131">Bağlam içinde `DbContextOptions` başlatmak için `OnConfiguring` yöntemini geçersiz kılın ve belirtilen `DbContextOptionsBuilder`Yöntemleri çağırın:</span><span class="sxs-lookup"><span data-stu-id="4a826-131">To initialize `DbContextOptions` within the context, override the `OnConfiguring` method and call the methods on the provided `DbContextOptionsBuilder`:</span></span>

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

<span data-ttu-id="4a826-132">Bir uygulama, oluşturucusuna hiçbir şey geçirmeden bu bağlamı basit bir şekilde oluşturabilir:</span><span class="sxs-lookup"><span data-stu-id="4a826-132">An application can simply instantiate such a context without passing anything to its constructor:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="4a826-133">Testler tam veritabanını hedeflemez değilse, bu yaklaşım kendisini teste vermez.</span><span class="sxs-lookup"><span data-stu-id="4a826-133">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="4a826-134">Bağımlılık ekleme ile DbContext kullanma</span><span class="sxs-lookup"><span data-stu-id="4a826-134">Using DbContext with dependency injection</span></span>

<span data-ttu-id="4a826-135">EF Core, bağımlılık ekleme kapsayıcısı ile `DbContext` kullanımını destekler.</span><span class="sxs-lookup"><span data-stu-id="4a826-135">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="4a826-136">DbContext türü, `AddDbContext<TContext>` yöntemi kullanılarak hizmet kapsayıcısına eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="4a826-136">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="4a826-137">`AddDbContext<TContext>` hem DbContext türü, `TContext`hem de karşılık gelen `DbContextOptions<TContext>` hizmet kapsayıcısından ekleme için kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="4a826-137">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="4a826-138">Bağımlılık ekleme hakkında ek bilgi için aşağıda [daha fazla okuma](#more-reading) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="4a826-138">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="4a826-139">`DbContext` bağımlılığı ekleme:</span><span class="sxs-lookup"><span data-stu-id="4a826-139">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="4a826-140">Bu, `DbContextOptions<TContext>`kabul eden DbContext türüne bir [Oluşturucu bağımsız değişkeni](#constructor-argument) eklenmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4a826-140">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="4a826-141">Bağlam kodu:</span><span class="sxs-lookup"><span data-stu-id="4a826-141">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="4a826-142">Uygulama kodu (ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="4a826-142">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="4a826-143">Uygulama kodu (doğrudan ServiceProvider kullanılarak, daha az ortak):</span><span class="sxs-lookup"><span data-stu-id="4a826-143">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="4a826-144">DbContext iş parçacığı sorunlarından kaçınma</span><span class="sxs-lookup"><span data-stu-id="4a826-144">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="4a826-145">Entity Framework Core, aynı `DbContext` örneğinde çalışan birden çok paralel işlemi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="4a826-145">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="4a826-146">Bu, zaman uyumsuz sorguların paralel yürütmesini ve birden çok iş parçacığından açık olan eşzamanlı kullanımı içerir.</span><span class="sxs-lookup"><span data-stu-id="4a826-146">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="4a826-147">Bu nedenle, zaman uyumsuz çağrıları hemen `await` veya paralel olarak yürütülen işlemler için ayrı `DbContext` örnekleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="4a826-147">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="4a826-148">EF Core eşzamanlı olarak bir `DbContext` örneği kullanma girişimi algıladığında şöyle bir ileti içeren bir `InvalidOperationException` görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="4a826-148">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span>

> <span data-ttu-id="4a826-149">Önceki bir işlem tamamlanmadan önce bu bağlamda ikinci bir işlem başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="4a826-149">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="4a826-150">Bu genellikle aynı DbContext örneğini kullanan farklı iş parçacıklarından kaynaklanır, ancak örnek üyelerin iş parçacığı güvenli olduğu garanti edilmez.</span><span class="sxs-lookup"><span data-stu-id="4a826-150">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="4a826-151">Eşzamanlı erişim algılanmadıysa, tanımsız davranışa, uygulama kilitlenmelerine ve veri bozulmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="4a826-151">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="4a826-152">Aynı `DbContext` örneğinde eşzamanlı erişime yanlışlıkla neden olabilecek yaygın hatalar vardır:</span><span class="sxs-lookup"><span data-stu-id="4a826-152">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="4a826-153">Aynı DbContext üzerinde başka bir işlem başlatmadan önce zaman uyumsuz bir işlemin tamamlanmasını beklemek</span><span class="sxs-lookup"><span data-stu-id="4a826-153">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="4a826-154">Zaman uyumsuz yöntemler, EF Core engellenmeyen bir şekilde veritabanına erişen işlemler başlatmasını etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="4a826-154">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="4a826-155">Ancak bir arayan, bu yöntemlerin birinin tamamlanmasını beklemez ve `DbContext`başka işlemler gerçekleştirmeye devam etmez, `DbContext` durumu (ve büyük olasılıkla) bozulmuş olabilir.</span><span class="sxs-lookup"><span data-stu-id="4a826-155">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span>

<span data-ttu-id="4a826-156">Her zaman zaman uyumsuz yöntemleri hemen EF Core.</span><span class="sxs-lookup"><span data-stu-id="4a826-156">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="4a826-157">Bağımlılık ekleme aracılığıyla birden çok iş parçacığında DbContext örneklerini örtülü olarak paylaşma</span><span class="sxs-lookup"><span data-stu-id="4a826-157">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="4a826-158">[`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) uzantısı yöntemi, varsayılan olarak [kapsamlı yaşam süresine](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) sahip `DbContext` türlerini kaydeder.</span><span class="sxs-lookup"><span data-stu-id="4a826-158">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span>

<span data-ttu-id="4a826-159">Belirli bir zamanda her istemci isteğini yürüten tek bir iş parçacığı olduğundan ve her istek ayrı bir bağımlılık ekleme kapsamı (ve dolayısıyla ayrı bir `DbContext` örneği) aldığından, çoğu ASP.NET Core uygulamalarda eşzamanlı erişim sorunlarından daha güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="4a826-159">This is safe from concurrent access issues in most ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span> <span data-ttu-id="4a826-160">Blazor sunucusu barındırma modeli için, Blazor Kullanıcı devresini sürdürmek için bir mantıksal istek kullanılır ve bu nedenle varsayılan ekleme kapsamı kullanılırsa Kullanıcı devresi başına yalnızca bir kapsamlı DbContext örneği kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4a826-160">For Blazor Server hosting model, one logical request is used for maintaining the Blazor user circuit, and thus only one scoped DbContext instance is available per user circuit if the default injection scope is used.</span></span>

<span data-ttu-id="4a826-161">Birden çok iş parçacığını paralel olarak doğrudan yürüten tüm kodlar `DbContext` örneklerine aynı anda erişilmemesini sağlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="4a826-161">Any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="4a826-162">Bağımlılık ekleme 'yi kullanarak bu, bağlamı kapsam olarak kaydederek ve her bir iş parçacığı için kapsam oluşturarak (`IServiceScopeFactory`kullanarak) ya da `DbContext` geçici olarak kaydederek (`ServiceLifetime` parametresi alan `AddDbContext` aşırı yüklemesini kullanarak) elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4a826-162">Using dependency injection, this can be achieved by either registering the context as scoped, and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="4a826-163">Daha fazla okuma</span><span class="sxs-lookup"><span data-stu-id="4a826-163">More reading</span></span>

- <span data-ttu-id="4a826-164">Dı kullanma hakkında daha fazla bilgi edinmek için [bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) konusunu okuyun.</span><span class="sxs-lookup"><span data-stu-id="4a826-164">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
- <span data-ttu-id="4a826-165">Daha fazla bilgi için [testi](testing/index.md) okuyun.</span><span class="sxs-lookup"><span data-stu-id="4a826-165">Read [Testing](testing/index.md) for more information.</span></span>
