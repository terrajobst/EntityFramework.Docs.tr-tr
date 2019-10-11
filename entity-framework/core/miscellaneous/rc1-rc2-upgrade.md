---
title: EF Core 1,0 RC1 'den RC2 'ye yükseltme EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 887b7cd539b9c0f5a680398f5039757420228710
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181284"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="24b4e-102">EF Core 1,0 RC1 'ten 1,0 RC2 'ye yükseltme</span><span class="sxs-lookup"><span data-stu-id="24b4e-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="24b4e-103">Bu makalede, RC1 paketleriyle derlenmiş bir uygulamayı RC2 'ye taşımaya yönelik rehberlik sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="24b4e-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="24b4e-104">Paket adları ve sürümleri</span><span class="sxs-lookup"><span data-stu-id="24b4e-104">Package Names and Versions</span></span>

<span data-ttu-id="24b4e-105">RC1 ve RC2 arasında "Entity Framework 7" iken "Entity Framework Core" olarak değiştiriyoruz.</span><span class="sxs-lookup"><span data-stu-id="24b4e-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="24b4e-106">[Scott Hanselman tarafından bu postadaki](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx)değişikliğin nedenleri hakkında daha fazla bilgi edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="24b4e-106">You can read more about the reasons for the change in [this post by Scott Hanselman](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="24b4e-107">Bu değişiklik nedeniyle, paket adlarımız `EntityFramework.*` ' dan `Microsoft.EntityFrameworkCore.*` ' e ve sürümlerimize `7.0.0-rc1-final` ' den `1.0.0-rc2-final` ' e (ya da araç için `1.0.0-preview1-final`) değişti.</span><span class="sxs-lookup"><span data-stu-id="24b4e-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="24b4e-108">**RC1 paketlerini tamamen kaldırmanız ve ardından RC2 'yi yüklemeniz gerekir.**</span><span class="sxs-lookup"><span data-stu-id="24b4e-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="24b4e-109">Bazı ortak paketlere yönelik eşleme aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="24b4e-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="24b4e-110">RC1 paketi</span><span class="sxs-lookup"><span data-stu-id="24b4e-110">RC1 Package</span></span>                                               | <span data-ttu-id="24b4e-111">RC2 eşdeğeri</span><span class="sxs-lookup"><span data-stu-id="24b4e-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="24b4e-112">EntityFramework. MicrosoftSqlServer 7.0.0-RC1-son</span><span class="sxs-lookup"><span data-stu-id="24b4e-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="24b4e-113">Microsoft. EntityFrameworkCore. SqlServer 1.0.0-RC2-final</span><span class="sxs-lookup"><span data-stu-id="24b4e-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="24b4e-114">EntityFramework. SQLite 7.0.0-RC1-son</span><span class="sxs-lookup"><span data-stu-id="24b4e-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="24b4e-115">Microsoft. EntityFrameworkCore. SQLite 1.0.0-RC2-final</span><span class="sxs-lookup"><span data-stu-id="24b4e-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="24b4e-116">EntityFramework7. Npgsql 3.1.0-RC1-3</span><span class="sxs-lookup"><span data-stu-id="24b4e-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="24b4e-117">NpgSql. EntityFrameworkCore. Postgres <to be advised></span><span class="sxs-lookup"><span data-stu-id="24b4e-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="24b4e-118">EntityFramework. SqlServerCompact35 7.0.0-RC1-final</span><span class="sxs-lookup"><span data-stu-id="24b4e-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="24b4e-119">EntityFrameworkCore. SqlServerCompact35 1.0.0-RC2-final</span><span class="sxs-lookup"><span data-stu-id="24b4e-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="24b4e-120">EntityFramework. SqlServerCompact40 7.0.0-RC1-final</span><span class="sxs-lookup"><span data-stu-id="24b4e-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="24b4e-121">EntityFrameworkCore. SqlServerCompact40 1.0.0-RC2-final</span><span class="sxs-lookup"><span data-stu-id="24b4e-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="24b4e-122">EntityFramework. InMemory 7.0.0-RC1-son</span><span class="sxs-lookup"><span data-stu-id="24b4e-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="24b4e-123">Microsoft. EntityFrameworkCore. InMemory 1.0.0-RC2-final</span><span class="sxs-lookup"><span data-stu-id="24b4e-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="24b4e-124">EntityFramework. IBMDataServer 7.0.0-Beta1</span><span class="sxs-lookup"><span data-stu-id="24b4e-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="24b4e-125">Henüz RC2 için kullanılamaz</span><span class="sxs-lookup"><span data-stu-id="24b4e-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="24b4e-126">EntityFramework. Commands 7.0.0-RC1-final</span><span class="sxs-lookup"><span data-stu-id="24b4e-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="24b4e-127">Microsoft. EntityFrameworkCore. Tools 1.0.0-preview1 'i-final</span><span class="sxs-lookup"><span data-stu-id="24b4e-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="24b4e-128">EntityFramework. MicrosoftSqlServer. Design 7.0.0-RC1-final</span><span class="sxs-lookup"><span data-stu-id="24b4e-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="24b4e-129">Microsoft. EntityFrameworkCore. SqlServer. Design 1.0.0-RC2-final</span><span class="sxs-lookup"><span data-stu-id="24b4e-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="24b4e-130">Ad Alanları</span><span class="sxs-lookup"><span data-stu-id="24b4e-130">Namespaces</span></span>

<span data-ttu-id="24b4e-131">Paket adlarıyla birlikte ad alanları `Microsoft.Data.Entity.*` ' dan `Microsoft.EntityFrameworkCore.*` ' e değişmiştir.</span><span class="sxs-lookup"><span data-stu-id="24b4e-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="24b4e-132">Bu değişikliği, `using Microsoft.Data.Entity` ' ın bir Bul/Değiştir ile `using Microsoft.EntityFrameworkCore` ile işleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="24b4e-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="24b4e-133">Tablo adlandırma kuralı değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="24b4e-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="24b4e-134">RC2 'de yaptığımız önemli bir işlevsel değişiklik, belirli bir varlık için yalnızca sınıf adı yerine, eşlendiği tablo adı olarak `DbSet<TEntity>` özelliğinin adını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="24b4e-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="24b4e-135">[İlgili duyuru sorununu](https://github.com/aspnet/Announcements/issues/167)bu değişiklik hakkında daha fazla bilgi edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="24b4e-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="24b4e-136">Mevcut RC1 uygulamaları için, RC1 adlandırma stratejisini korumak üzere `OnModelCreating` yönteminizin başlangıcına aşağıdaki kodu eklemeniz önerilir:</span><span class="sxs-lookup"><span data-stu-id="24b4e-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="24b4e-137">Yeni adlandırma stratejisini benimsemek istiyorsanız, Yükseltme adımlarının geri kalanını başarıyla tamamladık ve sonra kodu kaldırarak ve tablo yeniden adlandırmalarını uygulamak için bir geçiş oluşturmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="24b4e-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="24b4e-138">AddDbContext/Startup.cs Changes (yalnızca ASP.NET Core projeleri)</span><span class="sxs-lookup"><span data-stu-id="24b4e-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="24b4e-139">RC1 'de, uygulama hizmeti sağlayıcısına Entity Framework Hizmetleri eklemeniz gerekiyordu-`Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="24b4e-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="24b4e-140">RC2 'de `AddEntityFramework()`, `AddSqlServer()` vb. çağrıları kaldırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="24b4e-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="24b4e-141">Ayrıca, bağlam seçeneklerini alan ve bunları temel oluşturucuya ileten türetilmiş bağlamına bir Oluşturucu eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="24b4e-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="24b4e-142">Bu, tüm Sary Magic 'in arka planda yer aldığı bir kısmını kaldırdığımız için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="24b4e-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="24b4e-143">IServiceProvider 'a geçirme</span><span class="sxs-lookup"><span data-stu-id="24b4e-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="24b4e-144">Bağlam `IServiceProvider` ' ı geçiren RC1 kodunuz varsa, bu artık ayrı bir oluşturucu parametresi yerine `DbContextOptions` ' e taşınır.</span><span class="sxs-lookup"><span data-stu-id="24b4e-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="24b4e-145">Hizmet sağlayıcısını ayarlamak için `DbContextOptionsBuilder.UseInternalServiceProvider(...)` kullanın.</span><span class="sxs-lookup"><span data-stu-id="24b4e-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="24b4e-146">Test Etme</span><span class="sxs-lookup"><span data-stu-id="24b4e-146">Testing</span></span>

<span data-ttu-id="24b4e-147">Bunu yapmanın en yaygın senaryosu, test edilirken bir InMemory veritabanının kapsamını denetmektir.</span><span class="sxs-lookup"><span data-stu-id="24b4e-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="24b4e-148">Bunu RC2 ile yapmakla ilgili bir örnek için güncelleştirilmiş [Test](testing/index.md) makalesine bakın.</span><span class="sxs-lookup"><span data-stu-id="24b4e-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="24b4e-149">Uygulama hizmeti sağlayıcısından Iç Hizmetleri çözme (yalnızca ASP.NET Core projeleri)</span><span class="sxs-lookup"><span data-stu-id="24b4e-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="24b4e-150">Bir ASP.NET Core uygulamanız varsa ve uygulama hizmeti sağlayıcısından iç Hizmetleri çözümlemek istiyorsanız, bunu yapılandırmanıza izin veren `AddDbContext` ' ın bir aşırı yüklemesi vardır:</span><span class="sxs-lookup"><span data-stu-id="24b4e-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> <span data-ttu-id="24b4e-151">İç EF hizmetlerini uygulama hizmeti sağlayıcınızda birleştirmek için bir nedeniniz yoksa, EF 'in kendi hizmetlerini dahili olarak yönetmesine izin vermesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="24b4e-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="24b4e-152">Bunu yapmak isteyebileceğiniz başlıca neden, uygulama hizmeti sağlayıcınızı EF 'in dahili olarak kullandığı hizmetleri değiştirmek için kullanmaktır</span><span class="sxs-lookup"><span data-stu-id="24b4e-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="24b4e-153">DNX komutları = > .NET CLı (yalnızca ASP.NET Core projeleri)</span><span class="sxs-lookup"><span data-stu-id="24b4e-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="24b4e-154">Daha önce ASP.NET 5 projeleri için `dnx ef` komutlarını kullandıysanız, bunlar artık `dotnet ef` komutlarına taşınmıştır.</span><span class="sxs-lookup"><span data-stu-id="24b4e-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="24b4e-155">Aynı komut söz dizimi hala geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="24b4e-155">The same command syntax still applies.</span></span> <span data-ttu-id="24b4e-156">Sözdizimi bilgileri için `dotnet ef --help` kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="24b4e-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="24b4e-157">DNX 'in .NET CLı tarafından değiştirilmeleri nedeniyle, komutların kayıtlı olduğu Yöntem RC2 'de değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="24b4e-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="24b4e-158">Komutlar artık `project.json` ' deki bir `tools` bölümüne kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="24b4e-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="24b4e-159">Visual Studio kullanıyorsanız artık ASP.NET Core projelerine yönelik EF komutlarını çalıştırmak için Package Manager konsolunu kullanabilirsiniz (Bu, RC1 'de desteklenmez).</span><span class="sxs-lookup"><span data-stu-id="24b4e-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="24b4e-160">Bunu yapmak için yine de `project.json` ' in `tools` bölümünde komutları kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="24b4e-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="24b4e-161">Paket Yöneticisi komutları PowerShell 5 gerektirir</span><span class="sxs-lookup"><span data-stu-id="24b4e-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="24b4e-162">Visual Studio 'da Paket Yöneticisi konsolundaki Entity Framework komutlarını kullanırsanız, PowerShell 5 ' in yüklü olduğundan emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="24b4e-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="24b4e-163">Bu, sonraki sürümde kaldırılacak geçici bir gereksinimdir (daha fazla ayrıntı için bkz. [sorun #5327](https://github.com/aspnet/EntityFramework/issues/5327) ).</span><span class="sxs-lookup"><span data-stu-id="24b4e-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="24b4e-164">Project. JSON içinde "Imports" kullanma</span><span class="sxs-lookup"><span data-stu-id="24b4e-164">Using "imports" in project.json</span></span>

<span data-ttu-id="24b4e-165">EF Core bağımlılıklarından bazıları henüz .NET Standard desteklemez.</span><span class="sxs-lookup"><span data-stu-id="24b4e-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="24b4e-166">.NET Standard ve .NET Core projelerindeki EF Core, geçici bir geçici çözüm olarak Project. json ' a "Imports" eklenmesini gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="24b4e-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="24b4e-167">EF eklendiğinde, NuGet geri yükleme şu hata iletisini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="24b4e-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="24b4e-168">Geçici çözüm olarak, "Portable-net451 + Win8" taşınabilir profilini el ile içeri aktarırsınız.</span><span class="sxs-lookup"><span data-stu-id="24b4e-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="24b4e-169">Bu, NuGet 'in bu ikili dosyaları, .NET Standard ile eşleşen bu ikilileri, uyumlu bir çerçeve olarak kabul etmeye zorlar, ancak olmasa da.</span><span class="sxs-lookup"><span data-stu-id="24b4e-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="24b4e-170">"Portable-net451 + Win8 100", .NET Standard ile uyumlu olmasa da, PCL 'den .NET Standard geçişe yeterince uyumlu olur.</span><span class="sxs-lookup"><span data-stu-id="24b4e-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="24b4e-171">EF 'in bağımlılıkları sonunda .NET Standard 'e yükselttiğinizde içeri aktarmalar kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="24b4e-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="24b4e-172">Dizi sözdiziminde "Imports" öğesine birden çok çerçeve eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="24b4e-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="24b4e-173">Projenize ek kitaplıklar eklerseniz diğer içeri aktarmalar da gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="24b4e-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="24b4e-174">Bkz. [sorun #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="24b4e-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
