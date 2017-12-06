---
title: "DbContext - EF çekirdek yapılandırma"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="ed649-102">Bir yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ed649-102">Configuring a DbContext</span></span>

<span data-ttu-id="ed649-103">Bu makalede yapılandırma desenleri gösterilmektedir bir `DbContext` ile `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="ed649-103">This article shows patterns for configuring a `DbContext` with `DbContextOptions`.</span></span> <span data-ttu-id="ed649-104">Seçenekler, öncelikle seçin ve veri deposunu yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ed649-104">Options are primarily used to select and configure the data store.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="ed649-105">DbContextOptions yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ed649-105">Configuring DbContextOptions</span></span>

<span data-ttu-id="ed649-106">`DbContext`bir örneği olmalıdır `DbContextOptions` yürütmek için.</span><span class="sxs-lookup"><span data-stu-id="ed649-106">`DbContext` must have an instance of `DbContextOptions` in order to execute.</span></span> <span data-ttu-id="ed649-107">Bu geçersiz kılma tarafından yapılandırılabilir `OnConfiguring`, ya da dışarıdan oluşturucu bağımsız değişkeni sağlanan.</span><span class="sxs-lookup"><span data-stu-id="ed649-107">This can be configured by overriding `OnConfiguring`, or supplied externally via a constructor argument.</span></span>

<span data-ttu-id="ed649-108">Her ikisi de kullanılıyorsa, `OnConfiguring` ADDITIVE ve can olduğu anlamına gelir sağlanan seçeneklerin, yürütülen üzerine yazma seçeneklerini oluşturucu bağımsız değişkeni için sağlanan.</span><span class="sxs-lookup"><span data-stu-id="ed649-108">If both are used, `OnConfiguring` is executed on the supplied options, meaning it is additive and can overwrite  options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="ed649-109">Oluşturucu bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="ed649-109">Constructor argument</span></span>

<span data-ttu-id="ed649-110">Bağlam koduyla Oluşturucusu</span><span class="sxs-lookup"><span data-stu-id="ed649-110">Context code with constructor</span></span>

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
> <span data-ttu-id="ed649-111">Temel DbContext oluşturucusunun genel olmayan sürümünü de kabul eder `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="ed649-111">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`.</span></span> <span data-ttu-id="ed649-112">Genel olmayan sürümünü kullanarak birden fazla bağlam türü olan uygulamalar için önerilmez.</span><span class="sxs-lookup"><span data-stu-id="ed649-112">Using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="ed649-113">Uygulama kodu oluşturucu bağımsız değişkenden başlatmak için</span><span class="sxs-lookup"><span data-stu-id="ed649-113">Application code to initialize from constructor argument</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="ed649-114">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="ed649-114">OnConfiguring</span></span>

> [!WARNING]  
> <span data-ttu-id="ed649-115">`OnConfiguring`Son oluşur ve üzerine yazma seçeneklerini dı veya oluşturucusu elde.</span><span class="sxs-lookup"><span data-stu-id="ed649-115">`OnConfiguring` occurs last and can overwrite options obtained from DI or the constructor.</span></span> <span data-ttu-id="ed649-116">Bu yaklaşım kendisini (tam veritabanı hedef sürece) sınama için ödünç değil.</span><span class="sxs-lookup"><span data-stu-id="ed649-116">This approach does not lend itself to testing (unless you target the full database).</span></span>

<span data-ttu-id="ed649-117">Bağlam koduyla `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="ed649-117">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="ed649-118">Uygulama kodu ile başlatmak için `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="ed649-118">Application code to initialize with `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="ed649-119">DbContext ile bağımlılık ekleme kullanılarak</span><span class="sxs-lookup"><span data-stu-id="ed649-119">Using DbContext with dependency injection</span></span>

<span data-ttu-id="ed649-120">EF destekleyen kullanarak `DbContext` bir bağımlılık ekleme kapsayıcısını ile.</span><span class="sxs-lookup"><span data-stu-id="ed649-120">EF supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="ed649-121">DbContext türü kullanarak hizmet kapsayıcısı eklenebilir `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="ed649-121">Your DbContext type can be added to the service container by using `AddDbContext<TContext>`.</span></span>

<span data-ttu-id="ed649-122">`AddDbContext`hem sizin DbContext türü, yapacak `TContext`, ve `DbContextOptions<TContext>` hizmet kapsayıcısından yerleştirme için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ed649-122">`AddDbContext` will make both your DbContext type, `TContext`, and `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="ed649-123">Bkz: [daha fazla okuma](#more-reading) aşağıda bağımlılık ekleme hakkında bilgi için.</span><span class="sxs-lookup"><span data-stu-id="ed649-123">See [more reading](#more-reading) below for information on dependency injection.</span></span>

<span data-ttu-id="ed649-124">Bağımlılık ekleme dbcontext ekleme</span><span class="sxs-lookup"><span data-stu-id="ed649-124">Adding dbcontext to dependency injection</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="ed649-125">Bu ekleme gerektiren bir [oluşturucu bağımsız değişkeni](#constructor-argument) kabul eder, DbContext türü `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="ed649-125">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions`.</span></span>

<span data-ttu-id="ed649-126">Bağlam kodu:</span><span class="sxs-lookup"><span data-stu-id="ed649-126">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="ed649-127">Uygulama kodunda (ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="ed649-127">Application code (in ASP.NET Core):</span></span>

``` csharp
public MyController(BloggingContext context)
```

<span data-ttu-id="ed649-128">Uygulama kodu (ServiceProvider, daha az yaygın kullanarak doğrudan):</span><span class="sxs-lookup"><span data-stu-id="ed649-128">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a><span data-ttu-id="ed649-129">Kullanma`IDesignTimeDbContextFactory<TContext>`</span><span class="sxs-lookup"><span data-stu-id="ed649-129">Using `IDesignTimeDbContextFactory<TContext>`</span></span>

<span data-ttu-id="ed649-130">Yukarıdaki seçeneklerin alternatif olarak, bir uygulaması, ayrıca sağlayabilen `IDesignTimeDbContextFactory<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="ed649-130">As an alternative to the options above, you may also provide an implementation of `IDesignTimeDbContextFactory<TContext>`.</span></span> <span data-ttu-id="ed649-131">EF araçları bu Fabrika, DbContext örneği oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed649-131">EF tools can use this factory to create an instance of your DbContext.</span></span> <span data-ttu-id="ed649-132">Geçişler gibi belirli tasarım zamanı deneyimleri etkinleştirmek için gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="ed649-132">This may be required in order to enable specific design-time experiences such as migrations.</span></span>

<span data-ttu-id="ed649-133">Genel varsayılan bir oluşturucu yok bağlam türleri için tasarım zamanı hizmetlerini etkinleştirmek için bu arabirimi uygular.</span><span class="sxs-lookup"><span data-stu-id="ed649-133">Implement this interface to enable design-time services for context types that do not have a public default constructor.</span></span> <span data-ttu-id="ed649-134">Tasarım zamanı Hizmetleri türetilmiş bağlamı olarak aynı bütünleştirilmiş kodda olan bu arabirim uygulamaları otomatik olarak bulur.</span><span class="sxs-lookup"><span data-stu-id="ed649-134">Design-time services will automatically discover implementations of this interface that are in the same assembly as the derived context.</span></span>

<span data-ttu-id="ed649-135">Örnek:</span><span class="sxs-lookup"><span data-stu-id="ed649-135">Example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a><span data-ttu-id="ed649-136">Daha fazla okuma</span><span class="sxs-lookup"><span data-stu-id="ed649-136">More reading</span></span>

* <span data-ttu-id="ed649-137">Okuma [Başlarken ASP.NET Core üzerinde](../get-started/aspnetcore/index.md) EF ASP.NET Core ile kullanma hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="ed649-137">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="ed649-138">Okuma [bağımlılık ekleme](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) dı kullanma hakkında daha fazla bilgi edinmek için.</span><span class="sxs-lookup"><span data-stu-id="ed649-138">Read [Dependency Injection](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) to learn more about using DI.</span></span>
* <span data-ttu-id="ed649-139">Okuma [test](testing/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="ed649-139">Read [Testing](testing/index.md) for more information.</span></span>
