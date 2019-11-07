---
title: Entity Framework sağlayıcıları-EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
uid: ef6/fundamentals/providers/index
ms.openlocfilehash: 661398e7d6037875ce0cdb15c221a729d1f0c7d8
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656211"
---
# <a name="entity-framework-6-providers"></a><span data-ttu-id="92927-102">Entity Framework 6 sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="92927-102">Entity Framework 6 Providers</span></span>
> [!NOTE]
> <span data-ttu-id="92927-103">**Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="92927-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="92927-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="92927-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="92927-105">Entity Framework artık açık kaynaklı bir lisans altında geliştirilmektedir ve EF6 bir .NET Framework parçası olarak, yukarıdaki ve üzeri olarak gönderilmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="92927-105">The Entity Framework is now being developed under an open-source license and EF6 and above will not be shipped as part of the .NET Framework.</span></span> <span data-ttu-id="92927-106">Bunun birçok avantajı vardır, ancak EF6 derlemelerinde EF sağlayıcılarının yeniden oluşturulmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="92927-106">This has many advantages but also requires that EF providers be rebuilt against the EF6 assemblies.</span></span> <span data-ttu-id="92927-107">Bu, EF5 ve aşağıdaki için EF sağlayıcılarının yeniden oluşturulana kadar EF6 ile çalışmadıkları anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="92927-107">This means that EF providers for EF5 and below will not work with EF6 until they have been rebuilt.</span></span>

## <a name="which-providers-are-available-for-ef6"></a><span data-ttu-id="92927-108">EF6 için hangi sağlayıcılar kullanılabilir?</span><span class="sxs-lookup"><span data-stu-id="92927-108">Which providers are available for EF6?</span></span>

<span data-ttu-id="92927-109">EF6 şunlar için yeniden derlendiğimiz sağlayıcılar:</span><span class="sxs-lookup"><span data-stu-id="92927-109">Providers we are aware of that have been rebuilt for EF6 include:</span></span>

*   <span data-ttu-id="92927-110">**Microsoft SQL Server sağlayıcı**</span><span class="sxs-lookup"><span data-stu-id="92927-110">**Microsoft SQL Server provider**</span></span>
    *   <span data-ttu-id="92927-111">[Entity Framework açık kaynak kodu tabanı](https://github.com/aspnet/EntityFramework6) tarafından oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="92927-111">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="92927-112">[EntityFramework NuGet paketinin](https://nuget.org/packages/EntityFramework) parçası olarak sevk edildi</span><span class="sxs-lookup"><span data-stu-id="92927-112">Shipped as part of the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework)</span></span>
*   <span data-ttu-id="92927-113">**Microsoft SQL Server Compact sürümü sağlayıcısı**</span><span class="sxs-lookup"><span data-stu-id="92927-113">**Microsoft SQL Server Compact Edition provider**</span></span>
    *   <span data-ttu-id="92927-114">[Entity Framework açık kaynak kodu tabanı](https://github.com/aspnet/EntityFramework6) tarafından oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="92927-114">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="92927-115">[EntityFramework. SqlServerCompact NuGet paketinde](https://nuget.org/packages/EntityFramework.SqlServerCompact) sevk edildi</span><span class="sxs-lookup"><span data-stu-id="92927-115">Shipped in the [EntityFramework.SqlServerCompact NuGet package](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span></span>
*   [<span data-ttu-id="92927-116">**Devart dotConnect veri sağlayıcıları**</span><span class="sxs-lookup"><span data-stu-id="92927-116">**Devart dotConnect Data Providers**</span></span>](https://www.devart.com/dotconnect/)
    *   <span data-ttu-id="92927-117">Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 ve SQL Server gibi çeşitli veritabanları için [Devart](https://www.devart.com/) 'ten üçüncü taraf sağlayıcılar vardır</span><span class="sxs-lookup"><span data-stu-id="92927-117">There are third-party providers from [Devart](https://www.devart.com/) for a variety of databases including Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2, and SQL Server</span></span>
*   [<span data-ttu-id="92927-118">**CData yazılım sağlayıcıları**</span><span class="sxs-lookup"><span data-stu-id="92927-118">**CData Software providers**</span></span>](https://www.cdata.com/ado/)
    *   <span data-ttu-id="92927-119">Salesforce, Azure Table Storage, MySql ve çok daha fazlasını içeren çeşitli veri depoları için [CDATA yazılımından](https://www.cdata.com/ado/) üçüncü taraf sağlayıcılar vardır</span><span class="sxs-lookup"><span data-stu-id="92927-119">There are third-party providers from [CData Software](https://www.cdata.com/ado/) for a variety of data stores including Salesforce, Azure Table Storage, MySql, and many more</span></span>
*   <span data-ttu-id="92927-120">**Firebird sağlayıcısı**</span><span class="sxs-lookup"><span data-stu-id="92927-120">**Firebird provider**</span></span>
    *   <span data-ttu-id="92927-121">[NuGet paketi](https://www.nuget.org/packages/EntityFramework.Firebird/) olarak kullanılabilir</span><span class="sxs-lookup"><span data-stu-id="92927-121">Available as a [NuGet Package](https://www.nuget.org/packages/EntityFramework.Firebird/)</span></span>
*   <span data-ttu-id="92927-122">**Visual Fox Pro sağlayıcısı**</span><span class="sxs-lookup"><span data-stu-id="92927-122">**Visual Fox Pro provider**</span></span>
    *   <span data-ttu-id="92927-123">[NuGet paketi](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/) olarak kullanılabilir</span><span class="sxs-lookup"><span data-stu-id="92927-123">Available as a [NuGet package](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span></span>
*   <span data-ttu-id="92927-124">**MySQL**</span><span class="sxs-lookup"><span data-stu-id="92927-124">**MySQL**</span></span>
    *   [<span data-ttu-id="92927-125">Entity Framework için MySQL Bağlayıcısı/ağı</span><span class="sxs-lookup"><span data-stu-id="92927-125">MySQL Connector/NET for Entity Framework</span></span>](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   <span data-ttu-id="92927-126">**PostgreSQL**</span><span class="sxs-lookup"><span data-stu-id="92927-126">**PostgreSQL**</span></span>
    *   <span data-ttu-id="92927-127">Npgsql bir [NuGet paketi](https://www.nuget.org/packages/EntityFramework6.Npgsql/) olarak kullanılabilir</span><span class="sxs-lookup"><span data-stu-id="92927-127">Npgsql is available as a [NuGet package](https://www.nuget.org/packages/EntityFramework6.Npgsql/)</span></span>
*   <span data-ttu-id="92927-128">**Oracle**</span><span class="sxs-lookup"><span data-stu-id="92927-128">**Oracle**</span></span>
    *   <span data-ttu-id="92927-129">ODP.NET, bir [NuGet paketi](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/) olarak kullanılabilir</span><span class="sxs-lookup"><span data-stu-id="92927-129">ODP.NET is available as a [NuGet package](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span></span>

<span data-ttu-id="92927-130">Bu listede yer alan, belirli bir sağlayıcı için işlev düzeyini veya desteğin olduğunu göstermez, yalnızca bir EF6 derlemesi kullanılabilir hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="92927-130">Note that inclusion in this list does not indicate the level of functionality or support for a given provider, only that a build for EF6 has been made available.</span></span>

## <a name="registering-ef-providers"></a><span data-ttu-id="92927-131">EF sağlayıcılarını kaydetme</span><span class="sxs-lookup"><span data-stu-id="92927-131">Registering EF providers</span></span>

<span data-ttu-id="92927-132">Entity Framework 6 EF sağlayıcılarından itibaren, kod tabanlı yapılandırma veya uygulamanın yapılandırma dosyası kullanılarak kayıt yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="92927-132">Starting with Entity Framework 6 EF providers can be registered using either code-based configuration or in the application’s config file.</span></span>

### <a name="config-file-registration"></a><span data-ttu-id="92927-133">Yapılandırma dosyası kaydı</span><span class="sxs-lookup"><span data-stu-id="92927-133">Config file registration</span></span>

<span data-ttu-id="92927-134">App. config veya Web. config dosyasındaki EF sağlayıcısı 'nın kaydı şu biçimdedir:</span><span class="sxs-lookup"><span data-stu-id="92927-134">Registration of the EF provider in app.config or web.config has the following format:</span></span>


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

<span data-ttu-id="92927-135">Bu durum genellikle, NuGet 'den EF sağlayıcısı yüklüyse, NuGet paketinin bu kaydı yapılandırma dosyasına otomatik olarak eklemesini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="92927-135">Note that often if the EF provider is installed from NuGet, then the NuGet package will automatically add this registration to the config file.</span></span> <span data-ttu-id="92927-136">NuGet paketini uygulamanız için başlangıç projesi olmayan bir projeye yüklerseniz, kaydı başlangıç projeniz için yapılandırma dosyasına kopyalamanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="92927-136">If you install the NuGet package into a project that is not the startup project for your app, then you may need to copy the registration into the config file for your startup project.</span></span>

<span data-ttu-id="92927-137">Bu kayıtta "invariantName", bir ADO.NET sağlayıcısı tanımlamak için kullanılan sabit addır.</span><span class="sxs-lookup"><span data-stu-id="92927-137">The “invariantName” in this registration is the same invariant name used to identify an ADO.NET provider.</span></span> <span data-ttu-id="92927-138">Bu, bir DbProviderFactory kaydında "sabit" özniteliği olarak ve bir bağlantı dizesi kaydında "providerName" özniteliği olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="92927-138">This can be found as the “invariant” attribute in a DbProviderFactories registration and as the “providerName” attribute in a connection string registration.</span></span> <span data-ttu-id="92927-139">Kullanılacak sabit ad, sağlayıcıya yönelik belgelere de eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="92927-139">The invariant name to use should also be included in documentation for the provider.</span></span> <span data-ttu-id="92927-140">Sabit adlara örnek olarak, SQL Server için "System. Data. SqlClient" ve SQL Server Compact için "System. Data. SqlServerCe. 4.0" verilebilir.</span><span class="sxs-lookup"><span data-stu-id="92927-140">Examples of invariant names are “System.Data.SqlClient” for SQL Server and “System.Data.SqlServerCe.4.0” for SQL Server Compact.</span></span>

<span data-ttu-id="92927-141">Bu kayıtta "tür", "System. Data. Entity. Core. Common. DbProviderServices" öğesinden türetilen sağlayıcı türünün derleme nitelikli adıdır.</span><span class="sxs-lookup"><span data-stu-id="92927-141">The “type” in this registration is the assembly-qualified name of the provider type that derives from “System.Data.Entity.Core.Common.DbProviderServices”.</span></span> <span data-ttu-id="92927-142">Örneğin, SQL Compact için kullanılacak dize "System. Data. Entity. SqlServerCompact. SqlCeProviderServices, EntityFramework. SqlServerCompact" dir.</span><span class="sxs-lookup"><span data-stu-id="92927-142">For example, the string to use for SQL Compact is “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span></span> <span data-ttu-id="92927-143">Burada kullanılacak tür, sağlayıcının belgelerine eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="92927-143">The type to use here should be included in documentation for the provider.</span></span>

### <a name="code-based-registration"></a><span data-ttu-id="92927-144">Kod tabanlı kayıt</span><span class="sxs-lookup"><span data-stu-id="92927-144">Code-based registration</span></span>

<span data-ttu-id="92927-145">Entity Framework 6 ' dan itibaren EF için uygulama genelinde yapılandırma, kodda belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="92927-145">Starting with Entity Framework 6 application-wide configuration for EF can be specified in code.</span></span> <span data-ttu-id="92927-146">Tam Ayrıntılar için bkz. _[kod tabanlı yapılandırma Entity Framework](https://msdn.microsoft.com/data/jj680699)_ .</span><span class="sxs-lookup"><span data-stu-id="92927-146">For full details see _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_.</span></span> <span data-ttu-id="92927-147">Kod tabanlı yapılandırma kullanarak EF sağlayıcısını kaydetmek için normal yol, System. Data. Entity. DbConfiguration ' tan türetilen yeni bir sınıf oluşturmak ve bunu DbContext sınıfınız ile aynı derlemeye yerleştirkullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="92927-147">The normal way to register an EF provider using code-based configuration is to create a new class that derives from System.Data.Entity.DbConfiguration and place it in the same assembly as your DbContext class.</span></span> <span data-ttu-id="92927-148">DbConfiguration sınıfınız daha sonra sağlayıcıyı yapıcısına kaydetmelidir.</span><span class="sxs-lookup"><span data-stu-id="92927-148">Your DbConfiguration class should then register the provider in its constructor.</span></span> <span data-ttu-id="92927-149">Örneğin, SQL Compact sağlayıcısını kaydetmek için DbConfiguration sınıfı şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="92927-149">For example, to register the SQL Compact provider the DbConfiguration class looks like this:</span></span>

``` csharp
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetProviderServices(
                SqlCeProviderServices.ProviderInvariantName,
                SqlCeProviderServices.Instance);
        }
    }
```

<span data-ttu-id="92927-150">Bu kodda, "SqlCeProviderServices. ProviderInvariantName", SQL Server Compact sağlayıcı sabit adı dizesi ("System. Data. SqlServerCe. 4.0") ve SqlCeProviderServices. Instance için SQL Compact 'in Singleton örneğini döndürür EF sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="92927-150">In this code “SqlCeProviderServices.ProviderInvariantName” is a convenience for the SQL Server Compact provider invariant name string (“System.Data.SqlServerCe.4.0”) and SqlCeProviderServices.Instance returns the singleton instance of the SQL Compact EF provider.</span></span>

## <a name="what-if-the-provider-i-need-isnt-available"></a><span data-ttu-id="92927-151">Yapmam gereken sağlayıcı yoksa ne olacak?</span><span class="sxs-lookup"><span data-stu-id="92927-151">What if the provider I need isn’t available?</span></span>

<span data-ttu-id="92927-152">Sağlayıcı, EF 'in önceki sürümleri için kullanılabilirse, sağlayıcının sahibine başvurmanız ve bir EF6 sürümü oluşturmasını isteyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="92927-152">If the provider is available for previous versions of EF, then we encourage you to contact the owner of the provider and ask them to create an EF6 version.</span></span> <span data-ttu-id="92927-153">[EF6 sağlayıcı modelinin belgelerine](~/ef6/fundamentals/providers/provider-model.md)bir başvuru eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="92927-153">You should include a reference to the [documentation for the EF6 provider model](~/ef6/fundamentals/providers/provider-model.md).</span></span>

## <a name="can-i-write-a-provider-myself"></a><span data-ttu-id="92927-154">Bir sağlayıcıyı kendim yazabilir miyim?</span><span class="sxs-lookup"><span data-stu-id="92927-154">Can I write a provider myself?</span></span>

<span data-ttu-id="92927-155">Tamamen bir EF sağlayıcısı oluşturmak, ancak önemsiz bir şekilde düşünülmemelidir.</span><span class="sxs-lookup"><span data-stu-id="92927-155">It is certainly possible to create an EF provider yourself although it should not be considered a trivial undertaking.</span></span> <span data-ttu-id="92927-156">EF6 sağlayıcı modeliyle ilgili yukarıdaki bağlantının başlaması iyi bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="92927-156">The the link above about the EF6 provider model is a good place to start.</span></span> <span data-ttu-id="92927-157">Ayrıca, bir başlangıç noktası veya başvuru olarak [EF açık kaynak kod](https://github.com/aspnet/EntityFramework6) tabanına dahil SQL Server ve SQL CE sağlayıcısı için kodu kullanmayı yararlı bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="92927-157">You may also find it useful to use the code for the SQL Server and SQL CE provider included in the [EF open source codebase](https://github.com/aspnet/EntityFramework6) as a starting point or for reference.</span></span>

<span data-ttu-id="92927-158">EF6 ile başlayan EF sağlayıcısı, temel alınan ADO.NET sağlayıcısına daha sıkı bir şekilde bağlanmış olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="92927-158">Note that starting with EF6 the EF provider is less tightly coupled to the underlying ADO.NET provider.</span></span> <span data-ttu-id="92927-159">Bu, ADO.NET sınıfları yazmaya veya kaydırmaya gerek kalmadan bir EF sağlayıcısı yazmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="92927-159">This makes it easier to write an EF provider without needing to write or wrap the ADO.NET classes.</span></span>
