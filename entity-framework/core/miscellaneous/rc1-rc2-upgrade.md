---
title: EF Core 1,0 RC1 'den RC2 'ye yükseltme EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 887b7cd539b9c0f5a680398f5039757420228710
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416612"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="dd752-102">EF Core 1,0 RC1 'ten 1,0 RC2 'ye yükseltme</span><span class="sxs-lookup"><span data-stu-id="dd752-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="dd752-103">Bu makalede, RC1 paketleriyle derlenmiş bir uygulamayı RC2 'ye taşımaya yönelik rehberlik sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="dd752-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="dd752-104">Paket adları ve sürümleri</span><span class="sxs-lookup"><span data-stu-id="dd752-104">Package Names and Versions</span></span>

<span data-ttu-id="dd752-105">RC1 ve RC2 arasında "Entity Framework 7" iken "Entity Framework Core" olarak değiştiriyoruz.</span><span class="sxs-lookup"><span data-stu-id="dd752-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="dd752-106">[Scott Hanselman tarafından bu postadaki](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx)değişikliğin nedenleri hakkında daha fazla bilgi edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dd752-106">You can read more about the reasons for the change in [this post by Scott Hanselman](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="dd752-107">Bu değişiklik nedeniyle, paket adlarımız `EntityFramework.*` 'den `Microsoft.EntityFrameworkCore.*` ve sürümlerimize `7.0.0-rc1-final` `1.0.0-rc2-final` (ya da araç için `1.0.0-preview1-final`) olarak değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="dd752-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="dd752-108">**RC1 paketlerini tamamen kaldırmanız ve ardından RC2 'yi yüklemeniz gerekir.**</span><span class="sxs-lookup"><span data-stu-id="dd752-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="dd752-109">Bazı ortak paketlere yönelik eşleme aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="dd752-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="dd752-110">RC1 paketi</span><span class="sxs-lookup"><span data-stu-id="dd752-110">RC1 Package</span></span>                                               | <span data-ttu-id="dd752-111">RC2 eşdeğeri</span><span class="sxs-lookup"><span data-stu-id="dd752-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="dd752-112">EntityFramework. MicrosoftSqlServer 7.0.0-RC1-son</span><span class="sxs-lookup"><span data-stu-id="dd752-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="dd752-113">Microsoft. EntityFrameworkCore. SqlServer 1.0.0-RC2-final</span><span class="sxs-lookup"><span data-stu-id="dd752-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="dd752-114">EntityFramework. SQLite 7.0.0-RC1-son</span><span class="sxs-lookup"><span data-stu-id="dd752-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="dd752-115">Microsoft. EntityFrameworkCore. SQLite 1.0.0-RC2-final</span><span class="sxs-lookup"><span data-stu-id="dd752-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="dd752-116">EntityFramework7. Npgsql 3.1.0-RC1-3</span><span class="sxs-lookup"><span data-stu-id="dd752-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="dd752-117">NpgSql. EntityFrameworkCore. Postgres <to be advised></span><span class="sxs-lookup"><span data-stu-id="dd752-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="dd752-118">EntityFramework. SqlServerCompact35 7.0.0-RC1-final</span><span class="sxs-lookup"><span data-stu-id="dd752-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="dd752-119">EntityFrameworkCore. SqlServerCompact35 1.0.0-RC2-final</span><span class="sxs-lookup"><span data-stu-id="dd752-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="dd752-120">EntityFramework. SqlServerCompact40 7.0.0-RC1-final</span><span class="sxs-lookup"><span data-stu-id="dd752-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="dd752-121">EntityFrameworkCore. SqlServerCompact40 1.0.0-RC2-final</span><span class="sxs-lookup"><span data-stu-id="dd752-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="dd752-122">EntityFramework. InMemory 7.0.0-RC1-son</span><span class="sxs-lookup"><span data-stu-id="dd752-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="dd752-123">Microsoft. EntityFrameworkCore. InMemory 1.0.0-RC2-final</span><span class="sxs-lookup"><span data-stu-id="dd752-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="dd752-124">EntityFramework. IBMDataServer 7.0.0-Beta1</span><span class="sxs-lookup"><span data-stu-id="dd752-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="dd752-125">Henüz RC2 için kullanılamaz</span><span class="sxs-lookup"><span data-stu-id="dd752-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="dd752-126">EntityFramework. Commands 7.0.0-RC1-final</span><span class="sxs-lookup"><span data-stu-id="dd752-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="dd752-127">Microsoft. EntityFrameworkCore. Tools 1.0.0-preview1 'i-final</span><span class="sxs-lookup"><span data-stu-id="dd752-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="dd752-128">EntityFramework. MicrosoftSqlServer. Design 7.0.0-RC1-final</span><span class="sxs-lookup"><span data-stu-id="dd752-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="dd752-129">Microsoft. EntityFrameworkCore. SqlServer. Design 1.0.0-RC2-final</span><span class="sxs-lookup"><span data-stu-id="dd752-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="dd752-130">Ad Alanları</span><span class="sxs-lookup"><span data-stu-id="dd752-130">Namespaces</span></span>

<span data-ttu-id="dd752-131">Paket adlarıyla birlikte `Microsoft.Data.Entity.*` ad alanları `Microsoft.EntityFrameworkCore.*`olarak değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="dd752-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="dd752-132">Bu değişikliği, `using Microsoft.EntityFrameworkCore``using Microsoft.Data.Entity` Bul/Değiştir ile işleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dd752-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="dd752-133">Tablo adlandırma kuralı değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="dd752-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="dd752-134">RC2 'de yaptığımız önemli bir işlevsel değişiklik, belirli bir varlık için `DbSet<TEntity>` özelliğinin adını yalnızca sınıf adı yerine, eşlendiği tablo adı olarak kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="dd752-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="dd752-135">[İlgili duyuru sorununu](https://github.com/aspnet/Announcements/issues/167)bu değişiklik hakkında daha fazla bilgi edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dd752-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="dd752-136">Mevcut RC1 uygulamaları için, RC1 adlandırma stratejisini korumak üzere `OnModelCreating` yönteminizin başlangıcına aşağıdaki kodu eklemeniz önerilir:</span><span class="sxs-lookup"><span data-stu-id="dd752-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="dd752-137">Yeni adlandırma stratejisini benimsemek istiyorsanız, Yükseltme adımlarının geri kalanını başarıyla tamamladık ve sonra kodu kaldırarak ve tablo yeniden adlandırmalarını uygulamak için bir geçiş oluşturmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="dd752-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="dd752-138">AddDbContext/Startup.cs Changes (yalnızca ASP.NET Core projeleri)</span><span class="sxs-lookup"><span data-stu-id="dd752-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="dd752-139">RC1 'de, uygulama hizmeti sağlayıcısına Entity Framework Hizmetleri eklemeniz gerekiyordu-`Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="dd752-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="dd752-140">RC2 'de `AddEntityFramework()`, `AddSqlServer()`, vb. çağrıları kaldırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="dd752-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="dd752-141">Ayrıca, bağlam seçeneklerini alan ve bunları temel oluşturucuya ileten türetilmiş bağlamına bir Oluşturucu eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="dd752-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="dd752-142">Bu, tüm Sary Magic 'in arka planda yer aldığı bir kısmını kaldırdığımız için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="dd752-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="dd752-143">IServiceProvider 'a geçirme</span><span class="sxs-lookup"><span data-stu-id="dd752-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="dd752-144">Bir `IServiceProvider` bağlama sahip olan RC1 kodunuz varsa, bu artık ayrı bir oluşturucu parametresi yerine `DbContextOptions`' a taşınır.</span><span class="sxs-lookup"><span data-stu-id="dd752-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="dd752-145">Hizmet sağlayıcısını ayarlamak için `DbContextOptionsBuilder.UseInternalServiceProvider(...)` kullanın.</span><span class="sxs-lookup"><span data-stu-id="dd752-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="dd752-146">Test Etme</span><span class="sxs-lookup"><span data-stu-id="dd752-146">Testing</span></span>

<span data-ttu-id="dd752-147">Bunu yapmanın en yaygın senaryosu, test edilirken bir InMemory veritabanının kapsamını denetmektir.</span><span class="sxs-lookup"><span data-stu-id="dd752-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="dd752-148">Bunu RC2 ile yapmakla ilgili bir örnek için güncelleştirilmiş [Test](testing/index.md) makalesine bakın.</span><span class="sxs-lookup"><span data-stu-id="dd752-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="dd752-149">Uygulama hizmeti sağlayıcısından Iç Hizmetleri çözme (yalnızca ASP.NET Core projeleri)</span><span class="sxs-lookup"><span data-stu-id="dd752-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="dd752-150">Bir ASP.NET Core uygulamanız varsa ve uygulama hizmeti sağlayıcısından iç Hizmetleri çözümlemek istiyorsanız, bunu yapılandırmanıza izin veren `AddDbContext` aşırı yüklemesi vardır:</span><span class="sxs-lookup"><span data-stu-id="dd752-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> <span data-ttu-id="dd752-151">İç EF hizmetlerini uygulama hizmeti sağlayıcınızda birleştirmek için bir nedeniniz yoksa, EF 'in kendi hizmetlerini dahili olarak yönetmesine izin vermesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="dd752-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="dd752-152">Bunu yapmak isteyebileceğiniz başlıca neden, uygulama hizmeti sağlayıcınızı EF 'in dahili olarak kullandığı hizmetleri değiştirmek için kullanmaktır</span><span class="sxs-lookup"><span data-stu-id="dd752-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="dd752-153">DNX komutları = > .NET CLı (yalnızca ASP.NET Core projeleri)</span><span class="sxs-lookup"><span data-stu-id="dd752-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="dd752-154">Daha önce ASP.NET 5 projeleri için `dnx ef` komutlarını kullandıysanız, bunlar artık `dotnet ef` komutlara taşınmıştır.</span><span class="sxs-lookup"><span data-stu-id="dd752-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="dd752-155">Aynı komut söz dizimi hala geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="dd752-155">The same command syntax still applies.</span></span> <span data-ttu-id="dd752-156">Sözdizimi bilgileri için `dotnet ef --help` kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dd752-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="dd752-157">DNX 'in .NET CLı tarafından değiştirilmeleri nedeniyle, komutların kayıtlı olduğu Yöntem RC2 'de değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="dd752-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="dd752-158">Komutlar artık `project.json`bir `tools` bölümüne kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="dd752-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="dd752-159">Visual Studio kullanıyorsanız artık ASP.NET Core projelerine yönelik EF komutlarını çalıştırmak için Package Manager konsolunu kullanabilirsiniz (Bu, RC1 'de desteklenmez).</span><span class="sxs-lookup"><span data-stu-id="dd752-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="dd752-160">Bunu yapmak için, `project.json` `tools` bölümündeki komutları kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="dd752-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="dd752-161">Paket Yöneticisi komutları PowerShell 5 gerektirir</span><span class="sxs-lookup"><span data-stu-id="dd752-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="dd752-162">Visual Studio 'da Paket Yöneticisi konsolundaki Entity Framework komutlarını kullanırsanız, PowerShell 5 ' in yüklü olduğundan emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="dd752-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="dd752-163">Bu, sonraki sürümde kaldırılacak geçici bir gereksinimdir (daha fazla ayrıntı için bkz. [sorun #5327](https://github.com/aspnet/EntityFramework/issues/5327) ).</span><span class="sxs-lookup"><span data-stu-id="dd752-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="dd752-164">Project. JSON içinde "Imports" kullanma</span><span class="sxs-lookup"><span data-stu-id="dd752-164">Using "imports" in project.json</span></span>

<span data-ttu-id="dd752-165">EF Core bağımlılıklarından bazıları henüz .NET Standard desteklemez.</span><span class="sxs-lookup"><span data-stu-id="dd752-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="dd752-166">.NET Standard ve .NET Core projelerindeki EF Core, geçici bir geçici çözüm olarak Project. json ' a "Imports" eklenmesini gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="dd752-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="dd752-167">EF eklendiğinde, NuGet geri yükleme şu hata iletisini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="dd752-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="dd752-168">Geçici çözüm olarak, "Portable-net451 + Win8" taşınabilir profilini el ile içeri aktarırsınız.</span><span class="sxs-lookup"><span data-stu-id="dd752-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="dd752-169">Bu, NuGet 'in bu ikili dosyaları, .NET Standard ile eşleşen bu ikilileri, uyumlu bir çerçeve olarak kabul etmeye zorlar, ancak olmasa da.</span><span class="sxs-lookup"><span data-stu-id="dd752-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="dd752-170">"Portable-net451 + Win8 100", .NET Standard ile uyumlu olmasa da, PCL 'den .NET Standard geçişe yeterince uyumlu olur.</span><span class="sxs-lookup"><span data-stu-id="dd752-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="dd752-171">EF 'in bağımlılıkları sonunda .NET Standard 'e yükselttiğinizde içeri aktarmalar kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="dd752-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="dd752-172">Dizi sözdiziminde "Imports" öğesine birden çok çerçeve eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="dd752-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="dd752-173">Projenize ek kitaplıklar eklerseniz diğer içeri aktarmalar da gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="dd752-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="dd752-174">Bkz. [sorun #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="dd752-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
