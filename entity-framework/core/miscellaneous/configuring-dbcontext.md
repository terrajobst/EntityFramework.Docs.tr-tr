---
title: DbContext - EF çekirdek yapılandırma
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 6980acd53b0a74055af7a1e04b476f4625c327c9
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152396"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="97217-102">Bir yapılandırma</span><span class="sxs-lookup"><span data-stu-id="97217-102">Configuring a DbContext</span></span>

<span data-ttu-id="97217-103">Bu makalede yapılandırma temel düzenlerden gösterilmektedir bir `DbContext` aracılığıyla bir `DbContextOptions` belirli EF çekirdek sağlayıcısını ve isteğe bağlı davranışları kullanarak bir veritabanına bağlanmak için.</span><span class="sxs-lookup"><span data-stu-id="97217-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="97217-104">Tasarım zamanı DbContext yapılandırma</span><span class="sxs-lookup"><span data-stu-id="97217-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="97217-105">EF çekirdek tasarım zamanı araçları gibi [geçişler](xref:core/managing-schemas/migrations/index) bulmak ve bir çalışma örneği oluşturmak gereken bir `DbContext` uygulamanın varlık türlerini ve bunların bir veritabanı şeması nasıl eşleneceğine ayrıntılarını toplama amacıyla türü.</span><span class="sxs-lookup"><span data-stu-id="97217-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="97217-106">Aracı kolayca oluşturabilir sürece bu işlemi otomatik olabilir `DbContext` , benzer şekilde nasıl çalışma zamanında yapılandırılması için yapılandırılır şekilde.</span><span class="sxs-lookup"><span data-stu-id="97217-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="97217-107">While gerekli yapılandırma bilgileri sağlar düzeni `DbContext` çalışma zamanında, kullanmak için gerekli araçları çalışabilir bir `DbContext` tasarım zamanında yalnızca sınırlı sayıda desenler ile çalışabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97217-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="97217-108">Bunlar daha ayrıntılı olarak ele alınmıştır [tasarım zamanı bağlam oluşturma](xref:core/miscellaneous/cli/dbcontext-creation) bölümü.</span><span class="sxs-lookup"><span data-stu-id="97217-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="97217-109">DbContextOptions yapılandırma</span><span class="sxs-lookup"><span data-stu-id="97217-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="97217-110">`DbContext` bir örneği olmalıdır `DbContextOptions` herhangi bir iş gerçekleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="97217-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="97217-111">`DbContextOptions` Örneği yapılandırma bilgilerini aşağıdaki gibi yapar:</span><span class="sxs-lookup"><span data-stu-id="97217-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="97217-112">Genellikle seçili kullanmak için veritabanı sağlayıcısı gibi bir yöntemini çağırarak `UseSqlServer` veya `UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="97217-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`</span></span>
- <span data-ttu-id="97217-113">Tüm gerekli bağlantı dizesi veya veritabanı örneğinin tanıtıcısı genellikle geçirilen bağımsız değişken olarak yukarıda belirtilen sağlayıcı seçimi yöntemi</span><span class="sxs-lookup"><span data-stu-id="97217-113">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="97217-114">Sağlayıcı seçimi yöntem çağrısı içinde genellikle de zincirleme tüm sağlayıcısı düzeyi isteğe bağlı davranışını seçiciler</span><span class="sxs-lookup"><span data-stu-id="97217-114">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="97217-115">Sağlayıcı Seçici yöntemi önce veya sonra genellikle zincirleme tüm genel EF çekirdek davranışı seçiciler</span><span class="sxs-lookup"><span data-stu-id="97217-115">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="97217-116">Aşağıdaki örnek yapılandırır `DbContextOptions` SQL Server sağlayıcısı kullanmak için bir bağlantı içinde yer alan `connectionString` değişkeni, sağlayıcı düzeyi komut zaman aşımı ve içinde yürütülen tüm sorguları yapar EF çekirdek davranışı Seçici `DbContext` [Hayır izleme](xref:core/querying/tracking#no-tracking-queries) varsayılan olarak:</span><span class="sxs-lookup"><span data-stu-id="97217-116">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="97217-117">Sağlayıcı Seçici ve yukarıda belirtilen diğer davranış Seçici yöntemleri olan genişletme yöntemleri `DbContextOptions` veya sağlayıcıya özgü seçenek sınıfları.</span><span class="sxs-lookup"><span data-stu-id="97217-117">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="97217-118">Erişilebilmesi için bir ad alanı içerecek şekilde gerekebilir bu uzantı yöntemleri (genellikle `Microsoft.EntityFrameworkCore`) içinde kapsam ve ek paket bağımlılıklarını projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="97217-118">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="97217-119">`DbContextOptions` İçin sağlanan `DbContext` kılarak `OnConfiguring` yöntemi veya oluşturucu bağımsız değişkeni aracılığıyla harici olarak.</span><span class="sxs-lookup"><span data-stu-id="97217-119">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="97217-120">Her ikisi de kullanılıyorsa, `OnConfiguring` son olarak uygulanır ve oluşturucu bağımsız değişkeni için sağlanan seçenekleri geçersiz kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97217-120">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="97217-121">Oluşturucu bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="97217-121">Constructor argument</span></span>

<span data-ttu-id="97217-122">Bağlam Kod Oluşturucusu ile:</span><span class="sxs-lookup"><span data-stu-id="97217-122">Context code with constructor:</span></span>

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
> <span data-ttu-id="97217-123">Temel DbContext oluşturucusunun genel olmayan sürümünü de kabul eder `DbContextOptions`, ancak genel olmayan sürümüyle önerilmez birden fazla bağlam türü olan uygulamalar için.</span><span class="sxs-lookup"><span data-stu-id="97217-123">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="97217-124">Oluşturucu bağımsız değişkenden başlatmak için uygulama kodu:</span><span class="sxs-lookup"><span data-stu-id="97217-124">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="97217-125">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="97217-125">OnConfiguring</span></span>

<span data-ttu-id="97217-126">Bağlam koduyla `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="97217-126">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="97217-127">Uygulama kodu başlatmak için bir `DbContext` kullanan `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="97217-127">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="97217-128">Tam veritabanı testleri hedef sürece bu yaklaşım kendisini test için ödünç değil.</span><span class="sxs-lookup"><span data-stu-id="97217-128">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="97217-129">DbContext ile bağımlılık ekleme kullanılarak</span><span class="sxs-lookup"><span data-stu-id="97217-129">Using DbContext with dependency injection</span></span>

<span data-ttu-id="97217-130">EF çekirdek destekleyen kullanarak `DbContext` bir bağımlılık ekleme kapsayıcısını ile.</span><span class="sxs-lookup"><span data-stu-id="97217-130">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="97217-131">DbContext türü kullanarak hizmet kapsayıcısı eklenebilir `AddDbContext<TContext>` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="97217-131">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="97217-132">`AddDbContext<TContext>` hem sizin DbContext türü, yapacak `TContext`ve karşılık gelen `DbContextOptions<TContext>` hizmet kapsayıcısından yerleştirme için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="97217-132">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="97217-133">Bkz: [daha fazla okuma](#more-reading) aşağıda bağımlılık ekleme hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="97217-133">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="97217-134">Ekleme `Dbcontext` bağımlılık ekleme için:</span><span class="sxs-lookup"><span data-stu-id="97217-134">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="97217-135">Bu ekleme gerektiren bir [oluşturucu bağımsız değişkeni](#constructor-argument) kabul eder, DbContext türü `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="97217-135">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="97217-136">Bağlam kodu:</span><span class="sxs-lookup"><span data-stu-id="97217-136">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="97217-137">Uygulama kodunda (ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="97217-137">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="97217-138">Uygulama kodu (ServiceProvider, daha az yaygın kullanarak doğrudan):</span><span class="sxs-lookup"><span data-stu-id="97217-138">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="97217-139">Daha fazla okuma</span><span class="sxs-lookup"><span data-stu-id="97217-139">More reading</span></span>

* <span data-ttu-id="97217-140">Okuma [Başlarken ASP.NET Core üzerinde](../get-started/aspnetcore/index.md) EF ASP.NET Core ile kullanma hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="97217-140">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="97217-141">Okuma [bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) dı kullanma hakkında daha fazla bilgi edinmek için.</span><span class="sxs-lookup"><span data-stu-id="97217-141">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="97217-142">Okuma [test](testing/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="97217-142">Read [Testing](testing/index.md) for more information.</span></span>
