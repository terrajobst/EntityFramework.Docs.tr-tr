---
title: EF Core - DbContext yapılandırma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: f5a9ae17471391442170d8c40264e4db6922cb08
ms.sourcegitcommit: 39080d38e1adea90db741257e60dc0e7ed08aa82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980008"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="4eb6f-102">DbContext yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4eb6f-102">Configuring a DbContext</span></span>

<span data-ttu-id="4eb6f-103">Yapılandırma temel düzenlerden Bu makale bir `DbContext` aracılığıyla bir `DbContextOptions` belirli bir EF Core sağlayıcısını ve isteğe bağlı davranışları kullanarak bir veritabanına bağlanmak için.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="4eb6f-104">Tasarım zamanında DbContext yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4eb6f-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="4eb6f-105">EF Core tasarım zamanı araçlarını gibi [geçişler](xref:core/managing-schemas/migrations/index) bulmak ve bir çalışma örneğini oluşturmak gereken bir `DbContext` uygulamanın varlık türleri ve bunların veritabanı şemasına nasıl eşleneceğine hakkındaki ayrıntıları toplamak için türü.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="4eb6f-106">Aracı bir kolayca oluşturabilir sürece bu işlem otomatik olabilir `DbContext` şekilde, benzer şekilde nasıl, çalışma zamanında yapılandırılır için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="4eb6f-107">While için gerekli yapılandırma bilgileri sağlayan herhangi bir desen `DbContext` çalışma zamanında, kullanılmasını araçları çalışabilir bir `DbContext` tasarım zamanında yalnızca sınırlı sayıda desenleri ile çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="4eb6f-108">Bunlar daha ayrıntılı olarak ele alınmaktadır [tasarım zamanı bağlam oluşturma](xref:core/miscellaneous/cli/dbcontext-creation) bölümü.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="4eb6f-109">DbContextOptions yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4eb6f-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="4eb6f-110">`DbContext` bir örneğine sahip olması `DbContextOptions` herhangi bir çalışmayı gerçekleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="4eb6f-111">`DbContextOptions` Örneği yapılandırma bilgileri gibi yapar:</span><span class="sxs-lookup"><span data-stu-id="4eb6f-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="4eb6f-112">Veritabanı sağlayıcısı kullanmak için genellikle seçili bir yöntemi çağırarak `UseSqlServer` veya `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="4eb6f-113">Bu uzantı yöntemleri gibi ilgili sağlayıcı paketi gereken `Microsoft.EntityFrameworkCore.SqlServer` veya `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="4eb6f-114">Yöntemleri tanımlanan `Microsoft.EntityFrameworkCore` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="4eb6f-115">Herhangi bir gerekli bağlantı dizesini veya veritabanı örneğinin tanımlayıcı genellikle geçirilen bağımsız değişken olarak yukarıda belirtilen sağlayıcısı seçme yöntemi</span><span class="sxs-lookup"><span data-stu-id="4eb6f-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="4eb6f-116">Genellikle sağlayıcısı seçme yöntemi çağrısı içinde da zincirleme tüm sağlayıcısı düzeyi isteğe bağlı davranışını seçiciler</span><span class="sxs-lookup"><span data-stu-id="4eb6f-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="4eb6f-117">Sağlayıcı Seçici yöntemi önce veya sonra genellikle zincirleme tüm genel EF Core davranışı seçiciler</span><span class="sxs-lookup"><span data-stu-id="4eb6f-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="4eb6f-118">Aşağıdaki örnek yapılandırır `DbContextOptions` ve SQL Server sağlayıcıyı kullanmak için bir bağlantı bulunan `connectionString` değişkeni, bir sağlayıcı düzeyi komut zaman aşımı ve içinde yürütülen tüm sorguları yapan bir EF Core davranışı Seçici `DbContext` [Hayır izleme](xref:core/querying/tracking#no-tracking-queries) varsayılan olarak:</span><span class="sxs-lookup"><span data-stu-id="4eb6f-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="4eb6f-119">Sağlayıcı Seçici ve yukarıda belirtilen diğer davranışı Seçici yöntemleri üzerinde genişletme yöntemleri olan `DbContextOptions` veya sağlayıcıya özgü seçenek sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="4eb6f-120">Bir ad alanı sağlamak için ihtiyacınız olan bu genişletme yöntemlerini erişimi için (genellikle `Microsoft.EntityFrameworkCore`), kapsam ve projede ek paket bağımlılıklarını içerir.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="4eb6f-121">`DbContextOptions` İçin sağlanan `DbContext` kılarak `OnConfiguring` yöntem veya oluşturucu bağımsız değişkeni aracılığıyla harici olarak.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="4eb6f-122">Her ikisi de kullanılıyorsa `OnConfiguring` son uygulanan ve oluşturucu bağımsız değişkeni için sağlanan seçenekleri geçersiz kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="4eb6f-123">Oluşturucu bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="4eb6f-123">Constructor argument</span></span>

<span data-ttu-id="4eb6f-124">Bağlam Kod Oluşturucu ile:</span><span class="sxs-lookup"><span data-stu-id="4eb6f-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="4eb6f-125">DbContext, temel oluşturucu, genel olmayan sürümü de kabul eder `DbContextOptions`, ancak genel olmayan sürümüyle önerilmez birden çok içerik türü olan uygulamalar için.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="4eb6f-126">Oluşturucu bağımsız değişkeninden başlatmak için uygulama kodu:</span><span class="sxs-lookup"><span data-stu-id="4eb6f-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="4eb6f-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="4eb6f-127">OnConfiguring</span></span>

<span data-ttu-id="4eb6f-128">Bağlam koduyla `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="4eb6f-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="4eb6f-129">Uygulama kodu başlatmak için bir `DbContext` kullanan `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="4eb6f-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="4eb6f-130">Testlerin tam bir veritabanı hedef sürece bu yaklaşımın kendisi test için uygun olmayan.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="4eb6f-131">DbContext bağımlılık ekleme ile kullanma</span><span class="sxs-lookup"><span data-stu-id="4eb6f-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="4eb6f-132">EF Core destekler kullanarak `DbContext` bağımlılık ekleme kapsayıcısına sahip.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="4eb6f-133">DbContext türünüz kullanarak hizmet kapsayıcıya eklenebilir `AddDbContext<TContext>` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="4eb6f-134">`AddDbContext<TContext>` Her iki DbContext türünüzü hale getirecek `TContext`ve karşılık gelen `DbContextOptions<TContext>` hizmet kapsayıcısından yerleştirme için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-134">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="4eb6f-135">Bkz: [daha fazla okuma](#more-reading) aşağıdaki bağımlılık ekleme hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="4eb6f-136">Ekleme `Dbcontext` bağımlılık ekleme:</span><span class="sxs-lookup"><span data-stu-id="4eb6f-136">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="4eb6f-137">Bu ekleme gerektirir bir [oluşturucu bağımsız değişkeni](#constructor-argument) kabul eden DbContext türünüz için `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="4eb6f-138">Bağlam kodu:</span><span class="sxs-lookup"><span data-stu-id="4eb6f-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="4eb6f-139">Uygulama kodunda (ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="4eb6f-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="4eb6f-140">Uygulama kodu (doğrudan kullanarak daha az ortak ServiceProvider):</span><span class="sxs-lookup"><span data-stu-id="4eb6f-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="4eb6f-141">Daha fazla okuma</span><span class="sxs-lookup"><span data-stu-id="4eb6f-141">More reading</span></span>

* <span data-ttu-id="4eb6f-142">Okuma [Başlarken üzerinde ASP.NET Core](../get-started/aspnetcore/index.md) EF ASP.NET Core ile kullanma hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-142">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="4eb6f-143">Okuma [bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) DI kullanma hakkında daha fazla bilgi edinmek için.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-143">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="4eb6f-144">Okuma [test](testing/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="4eb6f-144">Read [Testing](testing/index.md) for more information.</span></span>
