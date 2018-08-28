---
title: EF Core 1.0 RC1'den RC2 - EF Core yükseltme
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 83b98fda5ac9491994b5b3fb333c9951ec01188a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996903"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="5afc5-102">EF Core 1.0 RC1'den 1.0 RC2'ye yükseltme</span><span class="sxs-lookup"><span data-stu-id="5afc5-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="5afc5-103">Bu makalede RC1 paketler RC2 ile oluşturulmuş bir uygulamada taşımak için yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="5afc5-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="5afc5-104">Paket adları ve sürümler</span><span class="sxs-lookup"><span data-stu-id="5afc5-104">Package Names and Versions</span></span>

<span data-ttu-id="5afc5-105">RC1 ile RC2 arasında "İçin Entity Framework Core" "Entity Framework' 7" değiştirdik.</span><span class="sxs-lookup"><span data-stu-id="5afc5-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="5afc5-106">Daha fazla değişiklik nedenleri hakkında [Scott Hanselman tarafından Bu gönderiyi](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="5afc5-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="5afc5-107">Bu değişiklik nedeniyle, bizim paket adları değiştirildi `EntityFramework.*` için `Microsoft.EntityFrameworkCore.*` ve bizim sürümlerinden `7.0.0-rc1-final` için `1.0.0-rc2-final` (veya `1.0.0-preview1-final` araçları için).</span><span class="sxs-lookup"><span data-stu-id="5afc5-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="5afc5-108">**Tamamen RC1 paketlerini kaldırın ve ardından RC2 yüklemek ihtiyacınız olanları.**</span><span class="sxs-lookup"><span data-stu-id="5afc5-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="5afc5-109">Bazı yaygın paketler için eşleme aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5afc5-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="5afc5-110">RC1'de paket</span><span class="sxs-lookup"><span data-stu-id="5afc5-110">RC1 Package</span></span>                                               | <span data-ttu-id="5afc5-111">RC2'yi eşdeğerine</span><span class="sxs-lookup"><span data-stu-id="5afc5-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="5afc5-112">EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="5afc5-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="5afc5-113">Microsoft.EntityFrameworkCore.SqlServer 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="5afc5-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="5afc5-114">EntityFramework.SQLite 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="5afc5-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="5afc5-115">Microsoft.EntityFrameworkCore.Sqlite 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="5afc5-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="5afc5-116">EntityFramework7.Npgsql 3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="5afc5-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="5afc5-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="5afc5-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="5afc5-118">EntityFramework.SqlServerCompact35 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="5afc5-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="5afc5-119">EntityFrameworkCore.SqlServerCompact35 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="5afc5-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="5afc5-120">EntityFramework.SqlServerCompact40 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="5afc5-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="5afc5-121">EntityFrameworkCore.SqlServerCompact40 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="5afc5-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="5afc5-122">EntityFramework.InMemory 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="5afc5-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="5afc5-123">Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="5afc5-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="5afc5-124">EntityFramework.IBMDataServer 7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="5afc5-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="5afc5-125">RC2 için henüz kullanılamıyor</span><span class="sxs-lookup"><span data-stu-id="5afc5-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="5afc5-126">EntityFramework.Commands 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="5afc5-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="5afc5-127">Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="5afc5-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="5afc5-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="5afc5-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="5afc5-129">Microsoft.EntityFrameworkCore.SqlServer.Design 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="5afc5-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="5afc5-130">Ad Alanları</span><span class="sxs-lookup"><span data-stu-id="5afc5-130">Namespaces</span></span>

<span data-ttu-id="5afc5-131">Paket adları ile birlikte ad değiştirildi `Microsoft.Data.Entity.*` için `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="5afc5-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="5afc5-132">Bu değişiklikle birlikte Bul/Değiştir işleyebilir `using Microsoft.Data.Entity` ile `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="5afc5-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="5afc5-133">Adlandırma kuralı değişikliklerinin tablo</span><span class="sxs-lookup"><span data-stu-id="5afc5-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="5afc5-134">RC2'deki attığımız önemli işlevsel bir değişiklik adını kullanılmasıydır `DbSet<TEntity>` özelliği tablo adını olarak belirli bir varlık için eşleneceği, yalnızca sınıf adı yerine.</span><span class="sxs-lookup"><span data-stu-id="5afc5-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="5afc5-135">Daha fazla bilgi bu değişiklik hakkında [ilgili duyuru sorunu](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="5afc5-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="5afc5-136">Mevcut RC1 uygulamalar için aşağıdaki kodu ekleyerek başlangıcına kadar olan öneririz, `OnModelCreating` RC1 adlandırma stratejisi tutmak yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5afc5-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="5afc5-137">Yeni adlandırma stratejisi benimsemek isterseniz başarıyla yeniden adlandırır ve yükseltme adımları ve ardından kod kaldırma tablo uygulamak için bir geçiş oluşturmaya rest Tamamlanıyor öneririz.</span><span class="sxs-lookup"><span data-stu-id="5afc5-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="5afc5-138">AddDbContext / Startup.cs değişiklikler (yalnızca ASP.NET Core projeleri için)</span><span class="sxs-lookup"><span data-stu-id="5afc5-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="5afc5-139">RC1'de uygulama hizmet sağlayıcısı - Entity Framework Hizmetleri eklemek zorunda `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="5afc5-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="5afc5-140">RC2'deki çağrıları kaldırabilirsiniz `AddEntityFramework()`, `AddSqlServer()`vb..:</span><span class="sxs-lookup"><span data-stu-id="5afc5-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="5afc5-141">Bağlamı seçenekleri alır ve bunları temel oluşturucuya geçirir, türetilmiş bağlamı, bir oluşturucu eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5afc5-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="5afc5-142">Bunları arka planda snuck scary Sihirli bazıları kaldırdık çünkü bu gereklidir:</span><span class="sxs-lookup"><span data-stu-id="5afc5-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="5afc5-143">İçinde bir IServiceProvider geçirme</span><span class="sxs-lookup"><span data-stu-id="5afc5-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="5afc5-144">Geçirir RC1 kodunuz varsa bir `IServiceProvider` bağlamına, bunu şimdi e taşınmıştır `DbContextOptions`, ayrı Oluşturucu parametresi olan yerine.</span><span class="sxs-lookup"><span data-stu-id="5afc5-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="5afc5-145">Kullanım `DbContextOptionsBuilder.UseInternalServiceProvider(...)` hizmet sağlayıcısı ayarlanamadı.</span><span class="sxs-lookup"><span data-stu-id="5afc5-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="5afc5-146">Sınama</span><span class="sxs-lookup"><span data-stu-id="5afc5-146">Testing</span></span>

<span data-ttu-id="5afc5-147">Bunu yapmak için en yaygın senaryo test ederken Inmemory veritabanı kapsamını denetlemek için oluştu.</span><span class="sxs-lookup"><span data-stu-id="5afc5-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="5afc5-148">Güncelleştirilmiş bkz [test](testing/index.md) makale RC2 ile bunu bir örnek.</span><span class="sxs-lookup"><span data-stu-id="5afc5-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="5afc5-149">Uygulama hizmeti sağlayıcısı (yalnızca ASP.NET Core projeleri için) iç hizmetlerinden çözümleme</span><span class="sxs-lookup"><span data-stu-id="5afc5-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="5afc5-150">Bir ASP.NET Core uygulaması varsa ve uygulama servis sağlayıcısından iç hizmetlerin çözümlenmesi için EF istediğiniz bir aşırı yüklemesini yoktur `AddDbContext` bu yapılandırmanıza olanak tanır:</span><span class="sxs-lookup"><span data-stu-id="5afc5-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="5afc5-151">Uygulama hizmet sağlayıcınıza iç EF Hizmetleri birleştirmek için bir nedeniniz yoksa dahili olarak kendi Servisleri yönetilecek EF izin vererek öneririz.</span><span class="sxs-lookup"><span data-stu-id="5afc5-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="5afc5-152">Bunu yapmak isteyebileceğiniz temel nedeni uygulama hizmet sağlayıcınıza EF dahili olarak kullandığı hizmetleri değiştirilecek kullanmaktır</span><span class="sxs-lookup"><span data-stu-id="5afc5-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="5afc5-153">DNX komutlarını = > .NET CLI (yalnızca ASP.NET Core projeleri için)</span><span class="sxs-lookup"><span data-stu-id="5afc5-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="5afc5-154">Daha önce kullandıysanız `dnx ef` komutları ASP.NET 5 projeleri için bunlar artık taşınmış için `dotnet ef` komutları.</span><span class="sxs-lookup"><span data-stu-id="5afc5-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="5afc5-155">Aynı komut sözdizimi hala geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5afc5-155">The same command syntax still applies.</span></span> <span data-ttu-id="5afc5-156">Kullanabileceğiniz `dotnet ef --help` söz dizimi bilgi.</span><span class="sxs-lookup"><span data-stu-id="5afc5-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="5afc5-157">.NET CLI tarafından değiştirilen DNX nedeniyle, RC2'deki komutları kayıtlı şekilde değiştirdi.</span><span class="sxs-lookup"><span data-stu-id="5afc5-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="5afc5-158">İçinde artık komutları kayıtlı bir `tools` konusundaki `project.json`:</span><span class="sxs-lookup"><span data-stu-id="5afc5-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> <span data-ttu-id="5afc5-159">Visual Studio kullanıyorsanız, Paket Yöneticisi konsolu artık (Bu RC1'de desteklenmiyordu) ASP.NET Core projeleri için EF komutlarını çalıştırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5afc5-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="5afc5-160">Komutlar kaydetmek yine `tools` bölümünü `project.json` Bunu yapmak için.</span><span class="sxs-lookup"><span data-stu-id="5afc5-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="5afc5-161">Paket Yöneticisi komutları PowerShell 5 gerektirir</span><span class="sxs-lookup"><span data-stu-id="5afc5-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="5afc5-162">Entity Framework komutlar Visual Studio'da Paket Yöneticisi konsolu kullanıyorsanız, PowerShell 5'in yüklü olduğundan emin olmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="5afc5-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="5afc5-163">Bu, sonraki sürümde kaldırılacak, geçici bir gereksinimdir (bkz [sorun #5327](https://github.com/aspnet/EntityFramework/issues/5327) daha fazla ayrıntı için).</span><span class="sxs-lookup"><span data-stu-id="5afc5-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="5afc5-164">Project.json içinde "içeri aktarmalar" kullanma</span><span class="sxs-lookup"><span data-stu-id="5afc5-164">Using "imports" in project.json</span></span>

<span data-ttu-id="5afc5-165">EF Core'nın bağımlılıkları bazıları .NET Standard henüz desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="5afc5-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="5afc5-166">EF Core .NET Standard ve .NET Core projelerinde, "project.json geçici bir çözüm olarak alır" ekleyerek gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="5afc5-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="5afc5-167">NuGet geri yükleme EF eklerken, bu hata iletisini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="5afc5-167">When adding EF, NuGet restore will display this error message:</span></span>

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

<span data-ttu-id="5afc5-168">Taşınabilir profil "taşınabilir net451 + win8" el ile içeri aktarmak için çözüm olabilir.</span><span class="sxs-lookup"><span data-stu-id="5afc5-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="5afc5-169">Olmadıkları olsa bile bu zorlar bu eşleşen bu ikili dosyalarını işlemek için NuGet ile .NET Standard uyumlu çerçeve sağlar.</span><span class="sxs-lookup"><span data-stu-id="5afc5-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="5afc5-170">"Taşınabilir net451 + win8" %100 .NET Standard ile uyumlu olmamasına karşın, geçiş PCL .NET Standard için yeteri kadar uyumlu.</span><span class="sxs-lookup"><span data-stu-id="5afc5-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="5afc5-171">EF'ın bağımlılıkları sonunda .NET Standard yükselttiğinizde içeri aktarmalar kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="5afc5-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="5afc5-172">Birden çok çerçeveyi "Alınanlar için" dizi sözdiziminde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5afc5-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="5afc5-173">Diğer içeri aktarmalar ek kitaplıklar projenize eklediğiniz durumlarda gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="5afc5-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="5afc5-174">Bkz: [#5176 sorun](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="5afc5-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
