---
title: RC2 - EF çekirdek EF çekirdek 1.0 RC1 ' yükseltme
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: e76886729248101ccac024568cf9abcd945fca33
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678633"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="718e4-102">EF çekirdek 1.0 RC1 1.0 RC2'ye yükseltme</span><span class="sxs-lookup"><span data-stu-id="718e4-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="718e4-103">Bu makalede RC2 için RC1 paketleri ile oluşturulmuş bir uygulama taşınması için yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="718e4-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="718e4-104">Paket adları ve sürümleri</span><span class="sxs-lookup"><span data-stu-id="718e4-104">Package Names and Versions</span></span>

<span data-ttu-id="718e4-105">RC1 ve RC2 arasında biz "İçin Entity Framework Çekirdek" "Entity Framework' 7" değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="718e4-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="718e4-106">Daha fazla bilgiyi değişikliği nedenleri hakkında [bu posta Scott Hanselman yoluyla](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="718e4-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="718e4-107">Bu değişikliği nedeniyle bizim paket adları değiştirildi `EntityFramework.*` için `Microsoft.EntityFrameworkCore.*` ve bizim sürümlerden `7.0.0-rc1-final` için `1.0.0-rc2-final` (veya `1.0.0-preview1-final` araçları için).</span><span class="sxs-lookup"><span data-stu-id="718e4-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="718e4-108">**Tamamen RC1 paketlerini kaldırın ve ardından RC2'yi yüklemeniz gerekir olanları.**</span><span class="sxs-lookup"><span data-stu-id="718e4-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="718e4-109">Burada, bazı ortak paketler için eşleme verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="718e4-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="718e4-110">RC1 paketi</span><span class="sxs-lookup"><span data-stu-id="718e4-110">RC1 Package</span></span>                                               | <span data-ttu-id="718e4-111">RC2 eşdeğeri</span><span class="sxs-lookup"><span data-stu-id="718e4-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="718e4-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="718e4-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="718e4-113">Microsoft.EntityFrameworkCore.SqlServer 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="718e4-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="718e4-114">EntityFramework.SQLite 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="718e4-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="718e4-115">Microsoft.EntityFrameworkCore.Sqlite 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="718e4-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="718e4-116">EntityFramework7.Npgsql 3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="718e4-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="718e4-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="718e4-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="718e4-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="718e4-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="718e4-119">EntityFrameworkCore.SqlServerCompact35 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="718e4-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="718e4-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="718e4-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="718e4-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="718e4-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="718e4-122">EntityFramework.InMemory 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="718e4-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="718e4-123">Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="718e4-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="718e4-124">EntityFramework.IBMDataServer 7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="718e4-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="718e4-125">RC2 için henüz kullanılamıyor</span><span class="sxs-lookup"><span data-stu-id="718e4-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="718e4-126">EntityFramework.Commands 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="718e4-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="718e4-127">Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="718e4-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="718e4-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="718e4-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="718e4-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="718e4-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="718e4-130">Ad Alanları</span><span class="sxs-lookup"><span data-stu-id="718e4-130">Namespaces</span></span>

<span data-ttu-id="718e4-131">Ad alanları değiştirildi paket adları ile birlikte `Microsoft.Data.Entity.*` için `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="718e4-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="718e4-132">Bu değişiklikle bulun ve değiştirin işleyebilir `using Microsoft.Data.Entity` ile `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="718e4-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="718e4-133">Adlandırma kuralı değişiklikleri tablosu</span><span class="sxs-lookup"><span data-stu-id="718e4-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="718e4-134">Biz sürdü RC2 önemli bir işlev değişiklik adını kullanılmasıydır `DbSet<TEntity>` özelliği tablo adı olarak belirli bir varlık için eşleneceği, yalnızca sınıf adını yerine.</span><span class="sxs-lookup"><span data-stu-id="718e4-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="718e4-135">Daha fazla bilgiyi bu değişiklik hakkında [ilgili duyuru sorunu](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="718e4-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="718e4-136">Varolan RC1 uygulamalar için aşağıdaki kod başlangıcına ekleme olan öneririz, `OnModelCreating` yöntemi RC1 adlandırma stratejisi tutmak için:</span><span class="sxs-lookup"><span data-stu-id="718e4-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="718e4-137">Yeni adlandırma stratejisi benimsemeyi istiyorsanız, başarılı bir şekilde yükseltme adımları ve ardından kod kaldırma ve tablo uygulamak için bir geçiş oluşturma rest Tamamlanıyor yeniden adlandırır öneririz.</span><span class="sxs-lookup"><span data-stu-id="718e4-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="718e4-138">AddDbContext / haline değiştirir (yalnızca ASP.NET Core projeler)</span><span class="sxs-lookup"><span data-stu-id="718e4-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="718e4-139">Uygulama hizmeti sağlayıcısı - Entity Framework Hizmetleri eklemek gerekiyordu RC1 içinde `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="718e4-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="718e4-140">RC2'yi çağrıları kaldırabilirsiniz `AddEntityFramework()`, `AddSqlServer()`vb..:</span><span class="sxs-lookup"><span data-stu-id="718e4-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="718e4-141">Bağlam seçeneklerini alır ve temel oluşturucuya aktaran türetilmiş içeriğiniz, bir oluşturucu eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="718e4-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="718e4-142">Biz bunları arka planda snuck korkutucu Sihirli bazıları kaldırılmış olduğundan bu gereklidir:</span><span class="sxs-lookup"><span data-stu-id="718e4-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="718e4-143">Bir IServiceProvider geçirme</span><span class="sxs-lookup"><span data-stu-id="718e4-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="718e4-144">Geçirir RC1 kodunuz varsa bir `IServiceProvider` bağlamına bu artık e taşınmıştır `DbContextOptions`, ayrı Oluşturucu parametresini olmak yerine.</span><span class="sxs-lookup"><span data-stu-id="718e4-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="718e4-145">Kullanım `DbContextOptionsBuilder.UseInternalServiceProvider(...)` hizmet sağlayıcısı ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="718e4-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="718e4-146">Sınama</span><span class="sxs-lookup"><span data-stu-id="718e4-146">Testing</span></span>

<span data-ttu-id="718e4-147">Bunu yapmak için en yaygın senaryo test edilirken bir Inmemory veritabanı kapsamını denetlemek için oluştu.</span><span class="sxs-lookup"><span data-stu-id="718e4-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="718e4-148">Güncelleştirilmiş bkz [test](testing/index.md) makale RC2 ile bunu bir örnek.</span><span class="sxs-lookup"><span data-stu-id="718e4-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="718e4-149">Uygulama hizmeti sağlayıcısı (yalnızca ASP.NET Core projeleri) iç Hizmetleri'nden çözme</span><span class="sxs-lookup"><span data-stu-id="718e4-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="718e4-150">Bir ASP.NET Core uygulama varsa ve uygulama servis sağlayıcı iç Hizmetleri'nden çözümlemek için EF istediğiniz bir aşırı yüklemesini yoktur `AddDbContext` , böylece bunu yapılandırmak:</span><span class="sxs-lookup"><span data-stu-id="718e4-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="718e4-151">Uygulama hizmet sağlayıcınıza iç EF Hizmetleri birleştirmek için bir nedeniniz yoksa dahili olarak kendi hizmetleri yönetmek EF izin vererek öneririz.</span><span class="sxs-lookup"><span data-stu-id="718e4-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="718e4-152">Bunu yapmak isteyebilirsiniz ana nedeni, uygulama hizmet sağlayıcınıza EF dahili olarak kullandığı hizmetleri değiştirmek için kullanmaktır</span><span class="sxs-lookup"><span data-stu-id="718e4-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="718e4-153">DNX komutları = > .NET CLI (yalnızca ASP.NET Core projeler)</span><span class="sxs-lookup"><span data-stu-id="718e4-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="718e4-154">Daha önce kullandıysanız `dnx ef` ASP.NET 5 projeleri için komutları, bunlar artık taşınmış için `dotnet ef` komutları.</span><span class="sxs-lookup"><span data-stu-id="718e4-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="718e4-155">Aynı komut sözdizimini hala geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="718e4-155">The same command syntax still applies.</span></span> <span data-ttu-id="718e4-156">Kullanabileceğiniz `dotnet ef --help` sözdizimi bilgi.</span><span class="sxs-lookup"><span data-stu-id="718e4-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="718e4-157">Komutları kayıtlı şekilde .NET CLI tarafından değiştirilen DNX nedeniyle RC2 değişti.</span><span class="sxs-lookup"><span data-stu-id="718e4-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="718e4-158">İçinde artık komutları kayıtlı bir `tools` bölümüne `project.json`:</span><span class="sxs-lookup"><span data-stu-id="718e4-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="718e4-159">Visual Studio kullanıyorsanız (Bu RC1 desteklenmiyordu) ASP.NET Core projeleri için EF komutları çalıştırmak için Paket Yöneticisi konsolu şimdi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="718e4-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="718e4-160">Hala komutlar kaydetmeniz gerekir `tools` bölümünü `project.json` Bunu yapmak için.</span><span class="sxs-lookup"><span data-stu-id="718e4-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="718e4-161">Paket Yöneticisi komutları PowerShell 5 gerektirir</span><span class="sxs-lookup"><span data-stu-id="718e4-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="718e4-162">Visual Studio'da Paket Yöneticisi konsolunda Entity Framework komutları kullanıyorsanız PowerShell 5 yüklü emin olmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="718e4-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="718e4-163">Bu, sonraki sürümde kaldırılacak, geçici bir gereksinimdir (bkz [sorun #5327](https://github.com/aspnet/EntityFramework/issues/5327) daha fazla ayrıntı için).</span><span class="sxs-lookup"><span data-stu-id="718e4-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="718e4-164">Project.json içinde "Imports" kullanma</span><span class="sxs-lookup"><span data-stu-id="718e4-164">Using "imports" in project.json</span></span>

<span data-ttu-id="718e4-165">EF çekirdek'ın bağımlılıkları bazıları .NET standart henüz desteklemez.</span><span class="sxs-lookup"><span data-stu-id="718e4-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="718e4-166">EF çekirdek .NET standart ve .NET Core projelerinde "project.json geçici bir çözüm olarak içeri aktarır" ekleme gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="718e4-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="718e4-167">EF eklerken NuGet restore bu hata iletisini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="718e4-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="718e4-168">Taşınabilir profil "taşınabilir net451 + olduğu win8" el ile almak için geçici bir çözüm değildir.</span><span class="sxs-lookup"><span data-stu-id="718e4-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="718e4-169">Olmadıkları olsa bile bu zorlar bu eşleşen bu ikili dosyalarını işlemek için NuGet standart .NET ile uyumlu bir çerçeve olarak sağlayın.</span><span class="sxs-lookup"><span data-stu-id="718e4-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="718e4-170">"Taşınabilir net451 + olduğu win8" % 100 standart .NET ile uyumlu olmamasına rağmen geçiş PCL .NET standart için yeterince uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="718e4-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="718e4-171">EF'ın bağımlılıkları sonunda .NET standart yükselttiğinizde içeri aktarmalar kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="718e4-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="718e4-172">Birden çok çerçeveyi "içeri aktarmalar için" dizi sözdiziminde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="718e4-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="718e4-173">Diğer içeri aktarmalar ek kitaplıklarını projenize eklemek durumlarda gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="718e4-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="718e4-174">Bkz: [#5176 sorun](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="718e4-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
