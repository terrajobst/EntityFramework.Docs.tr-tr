---
title: Entity Framework sağlayıcıları - EF6
author: divega
ms.date: 2018-06-27
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
ms.openlocfilehash: 98dc34591e8b2724401810a13f363382dce53218
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997924"
---
# <a name="entity-framework-6-providers"></a><span data-ttu-id="f06db-102">Entity Framework 6 sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="f06db-102">Entity Framework 6 Providers</span></span>
> [!NOTE]
> <span data-ttu-id="f06db-103">**EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="f06db-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="f06db-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="f06db-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="f06db-105">Entity Framework, açık kaynaklı lisans ve EF6 altında artık geliştirilmektedir ve yukarıda .NET Framework'ün bir parçası olarak gönderilmeyecek.</span><span class="sxs-lookup"><span data-stu-id="f06db-105">The Entity Framework is now being developed under an open-source license and EF6 and above will not be shipped as part of the .NET Framework.</span></span> <span data-ttu-id="f06db-106">Bu, birçok avantaj sunar ancak ayrıca EF sağlayıcıları karşı EF6 derlemelerin yeniden oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f06db-106">This has many advantages but also requires that EF providers be rebuilt against the EF6 assemblies.</span></span> <span data-ttu-id="f06db-107">Başka bir deyişle, bunlar yeniden kadar EF sağlayıcıları EF5 ve aşağıda EF6 ile çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="f06db-107">This means that EF providers for EF5 and below will not work with EF6 until they have been rebuilt.</span></span>

## <a name="which-providers-are-available-for-ef6"></a><span data-ttu-id="f06db-108">Hangi sağlayıcıları EF6 için kullanılabilir mi?</span><span class="sxs-lookup"><span data-stu-id="f06db-108">Which providers are available for EF6?</span></span>

<span data-ttu-id="f06db-109">Farkında duyuyoruz sağlayıcıları, yeniden EF6 içerir:</span><span class="sxs-lookup"><span data-stu-id="f06db-109">Providers we are aware of that have been rebuilt for EF6 include:</span></span>

*   <span data-ttu-id="f06db-110">**Microsoft SQL Server sağlayıcısı**</span><span class="sxs-lookup"><span data-stu-id="f06db-110">**Microsoft SQL Server provider**</span></span>
    *   <span data-ttu-id="f06db-111">Oluşturulmuş [Entity Framework açık kaynak kod tabanı](http://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="f06db-111">Built from the [Entity Framework open source code base](http://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="f06db-112">Parçası olarak gönderildiğini [EntityFramework NuGet paketi](http://nuget.org/packages/EntityFramework)</span><span class="sxs-lookup"><span data-stu-id="f06db-112">Shipped as part of the [EntityFramework NuGet package](http://nuget.org/packages/EntityFramework)</span></span>
*   <span data-ttu-id="f06db-113">**Microsoft SQL Server Compact Edition sağlayıcısı**</span><span class="sxs-lookup"><span data-stu-id="f06db-113">**Microsoft SQL Server Compact Edition provider**</span></span>
    *   <span data-ttu-id="f06db-114">Oluşturulmuş [Entity Framework açık kaynak kod tabanı](http://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="f06db-114">Built from the [Entity Framework open source code base](http://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="f06db-115">İçinde sevk [EntityFramework.SqlServerCompact NuGet paketi](http://nuget.org/packages/EntityFramework.SqlServerCompact)</span><span class="sxs-lookup"><span data-stu-id="f06db-115">Shipped in the [EntityFramework.SqlServerCompact NuGet package](http://nuget.org/packages/EntityFramework.SqlServerCompact)</span></span>
*   [<span data-ttu-id="f06db-116">**Devart dotConnect veri sağlayıcıları**</span><span class="sxs-lookup"><span data-stu-id="f06db-116">**Devart dotConnect Data Providers**</span></span>](http://www.devart.com/dotconnect/)
    *   <span data-ttu-id="f06db-117">Üçüncü taraf sağlayıcılarından vardır [Devart](http://www.devart.com/) Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 ve SQL Server dahil olmak üzere çeşitli</span><span class="sxs-lookup"><span data-stu-id="f06db-117">There are third-party providers from [Devart](http://www.devart.com/) for a variety of databases including Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2, and SQL Server</span></span>
*   [<span data-ttu-id="f06db-118">**CData yazılım sağlayıcıları**</span><span class="sxs-lookup"><span data-stu-id="f06db-118">**CData Software providers**</span></span>](http://www.cdata.com/ado/)
    *   <span data-ttu-id="f06db-119">Üçüncü taraf sağlayıcılarından vardır [CData yazılım](http://www.cdata.com/ado/) veri depoları, Salesforce, Azure Table Storage, MySql ve çok daha fazlası dahil olmak üzere çeşitli</span><span class="sxs-lookup"><span data-stu-id="f06db-119">There are third-party providers from [CData Software](http://www.cdata.com/ado/) for a variety of data stores including Salesforce, Azure Table Storage, MySql, and many more</span></span>
*   <span data-ttu-id="f06db-120">**Firebird sağlayıcısı**</span><span class="sxs-lookup"><span data-stu-id="f06db-120">**Firebird provider**</span></span>
    *   <span data-ttu-id="f06db-121">Olarak kullanılabilir bir [NuGet paketi](http://www.nuget.org/packages/FirebirdSql.Data.FirebirdClient/)</span><span class="sxs-lookup"><span data-stu-id="f06db-121">Available as a [NuGet Package](http://www.nuget.org/packages/FirebirdSql.Data.FirebirdClient/)</span></span>
*   <span data-ttu-id="f06db-122">**Görsel Fox Pro sağlayıcısı**</span><span class="sxs-lookup"><span data-stu-id="f06db-122">**Visual Fox Pro provider**</span></span>
    *   <span data-ttu-id="f06db-123">Olarak kullanılabilir bir [NuGet paketi](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span><span class="sxs-lookup"><span data-stu-id="f06db-123">Available as a [NuGet package](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span></span>
*   <span data-ttu-id="f06db-124">**MySQL**</span><span class="sxs-lookup"><span data-stu-id="f06db-124">**MySQL**</span></span>
    *   [<span data-ttu-id="f06db-125">MySQL Connector/Net</span><span class="sxs-lookup"><span data-stu-id="f06db-125">MySQL Connector/Net</span></span>](http://dev.mysql.com/downloads/connector/net/)
*   <span data-ttu-id="f06db-126">**PostgreSQL**</span><span class="sxs-lookup"><span data-stu-id="f06db-126">**PostgreSQL**</span></span>
    *   <span data-ttu-id="f06db-127">Npgsql olarak kullanılabilir bir [NuGet paketi](http://www.nuget.org/packages/Npgsql.EF6/)</span><span class="sxs-lookup"><span data-stu-id="f06db-127">Npgsql is available as a [NuGet package](http://www.nuget.org/packages/Npgsql.EF6/)</span></span>
*   <span data-ttu-id="f06db-128">**Oracle**</span><span class="sxs-lookup"><span data-stu-id="f06db-128">**Oracle**</span></span>
    *   <span data-ttu-id="f06db-129">ODP.NET olarak kullanılabilir bir [NuGet paketi](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span><span class="sxs-lookup"><span data-stu-id="f06db-129">ODP.NET is available as a [NuGet package](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span></span>

<span data-ttu-id="f06db-130">Bu listesine eklenmek üzere işlevsellik veya yalnızca, bir derleme EF6 için kullanılabilir duruma getirilen belirli bir sağlayıcı için destek düzeyini göstermez unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f06db-130">Note that inclusion in this list does not indicate the level of functionality or support for a given provider, only that a build for EF6 has been made available.</span></span>

## <a name="registering-ef-providers"></a><span data-ttu-id="f06db-131">EF sağlayıcılar kaydediliyor</span><span class="sxs-lookup"><span data-stu-id="f06db-131">Registering EF providers</span></span>

<span data-ttu-id="f06db-132">Entity Framework 6 EF sağlayıcılardan başlayarak kaydedilebilir ya da kod tabanlı yapılandırma kullanarak veya uygulamanın yapılandırma dosyasında.</span><span class="sxs-lookup"><span data-stu-id="f06db-132">Starting with Entity Framework 6 EF providers can be registered using either code-based configuration or in the application’s config file.</span></span>

### <a name="config-file-registration"></a><span data-ttu-id="f06db-133">Yapılandırma dosyası kayıt</span><span class="sxs-lookup"><span data-stu-id="f06db-133">Config file registration</span></span>

<span data-ttu-id="f06db-134">App.config veya web.config EF sağlayıcı kaydı, aşağıdaki biçime sahiptir:</span><span class="sxs-lookup"><span data-stu-id="f06db-134">Registration of the EF provider in app.config or web.config has the following format:</span></span>


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

<span data-ttu-id="f06db-135">Nuget'ten EF sağlayıcısı yüklüyse, NuGet paketi yapılandırma dosyasına otomatik olarak bu kayıt ekleyeceksiniz sonra genellikle unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f06db-135">Note that often if the EF provider is installed from NuGet, then the NuGet package will automatically add this registration to the config file.</span></span> <span data-ttu-id="f06db-136">Uygulamanız için başlangıç projesi olmayan bir proje içinde NuGet paketi yüklerseniz, başlangıç projeniz için yapılandırma dosyası kayıt kopyalamamız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="f06db-136">If you install the NuGet package into a project that is not the startup project for your app, then you may need to copy the registration into the config file for your startup project.</span></span>

<span data-ttu-id="f06db-137">Bu kayıt "Invariantname", bir ADO.NET sağlayıcısı tanımlamak için kullanılan aynı sabit adıdır.</span><span class="sxs-lookup"><span data-stu-id="f06db-137">The “invariantName” in this registration is the same invariant name used to identify an ADO.NET provider.</span></span> <span data-ttu-id="f06db-138">Bu, "sabit" özniteliği DbProviderFactories kayıt ve bağlantı dizesini kayıt "providerName" özniteliği olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="f06db-138">This can be found as the “invariant” attribute in a DbProviderFactories registration and as the “providerName” attribute in a connection string registration.</span></span> <span data-ttu-id="f06db-139">Değişmez adını kullanmak için Ayrıca sağlayıcının belgelerine eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="f06db-139">The invariant name to use should also be included in documentation for the provider.</span></span> <span data-ttu-id="f06db-140">SQL Server için "System.Data.SqlClient" ve "System.Data.SqlServerCe.4.0" SQL Server Compact için sabit adları örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="f06db-140">Examples of invariant names are “System.Data.SqlClient” for SQL Server and “System.Data.SqlServerCe.4.0” for SQL Server Compact.</span></span>

<span data-ttu-id="f06db-141">Bu kayıt "type" bütünleştirilmiş kodla nitelenen "System.Data.Entity.Core.Common.DbProviderServices" türetilen sağlayıcı türü adıdır.</span><span class="sxs-lookup"><span data-stu-id="f06db-141">The “type” in this registration is the assembly-qualified name of the provider type that derives from “System.Data.Entity.Core.Common.DbProviderServices”.</span></span> <span data-ttu-id="f06db-142">Örneğin, SQL Compact için kullanılacak dize "System.Data.Entity.SqlServerCompact.SqlCeProviderServices EntityFramework.SqlServerCompact" olur.</span><span class="sxs-lookup"><span data-stu-id="f06db-142">For example, the string to use for SQL Compact is “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span></span> <span data-ttu-id="f06db-143">Burada kullanmak üzere türü sağlayıcının belgelerine eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="f06db-143">The type to use here should be included in documentation for the provider.</span></span>

### <a name="code-based-registration"></a><span data-ttu-id="f06db-144">Kod tabanlı kayıt</span><span class="sxs-lookup"><span data-stu-id="f06db-144">Code-based registration</span></span>

<span data-ttu-id="f06db-145">EF için birçok farklı uygulama yapılandırma Entity Framework 6 ile başlayarak, kod içinde belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="f06db-145">Starting with Entity Framework 6 application-wide configuration for EF can be specified in code.</span></span> <span data-ttu-id="f06db-146">Tam Ayrıntılar için bkz.  _[Entity Framework Code-Based yapılandırma](https://msdn.microsoft.com/en-us/data/jj680699)_.</span><span class="sxs-lookup"><span data-stu-id="f06db-146">For full details see _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/en-us/data/jj680699)_.</span></span> <span data-ttu-id="f06db-147">Kod tabanlı yapılandırma kullanarak bir EF sağlayıcısını kaydetmek için normal System.Data.Entity.DbConfiguration türeyen yeni bir sınıf oluşturun ve aynı bütünleştirilmiş kodun DbContext sınıfınıza olarak yerleştirme yoludur.</span><span class="sxs-lookup"><span data-stu-id="f06db-147">The normal way to register an EF provider using code-based configuration is to create a new class that derives from System.Data.Entity.DbConfiguration and place it in the same assembly as your DbContext class.</span></span> <span data-ttu-id="f06db-148">DbConfiguration sınıfınıza ardından oluşturucusunda sağlayıcısını kaydetmeniz.</span><span class="sxs-lookup"><span data-stu-id="f06db-148">Your DbConfiguration class should then register the provider in its constructor.</span></span> <span data-ttu-id="f06db-149">Örneğin, SQL Compact kaydettirmek için sağlayıcı DbConfiguration sınıfı şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="f06db-149">For example, to register the SQL Compact provider the DbConfiguration class looks like this:</span></span>

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

<span data-ttu-id="f06db-150">Bu kodu içinde "SqlCeProviderServices.ProviderInvariantName" SQL Server Compact sağlayıcı değişmez adı dize ("System.Data.SqlServerCe.4.0") için bir kolaylık ve SqlCeProviderServices.Instance SQL Compact tekil örneğini döndürür. EF sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="f06db-150">In this code “SqlCeProviderServices.ProviderInvariantName” is a convenience for the SQL Server Compact provider invariant name string (“System.Data.SqlServerCe.4.0”) and SqlCeProviderServices.Instance returns the singleton instance of the SQL Compact EF provider.</span></span>

## <a name="what-if-the-provider-i-need-isnt-available"></a><span data-ttu-id="f06db-151">İhtiyacım olursa ne sağlayıcı yapabilirim kullanılamıyor?</span><span class="sxs-lookup"><span data-stu-id="f06db-151">What if the provider I need isn’t available?</span></span>

<span data-ttu-id="f06db-152">Sağlayıcı EF'ın önceki sürümleri için kullanılabilir haldeyse, ardından sağlayıcı sahibine başvurun ve EF6 sürüm oluşturma isteyin geçirmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="f06db-152">If the provider is available for previous versions of EF, then we encourage you to contact the owner of the provider and ask them to create an EF6 version.</span></span> <span data-ttu-id="f06db-153">Bir başvuru içermelidir [EF6 sağlayıcı modeli için belgeleri](~/ef6/fundamentals/providers/provider-model.md).</span><span class="sxs-lookup"><span data-stu-id="f06db-153">You should include a reference to the [documentation for the EF6 provider model](~/ef6/fundamentals/providers/provider-model.md).</span></span>

## <a name="can-i-write-a-provider-myself"></a><span data-ttu-id="f06db-154">Ben bir sağlayıcı kendim yazmak?</span><span class="sxs-lookup"><span data-stu-id="f06db-154">Can I write a provider myself?</span></span>

<span data-ttu-id="f06db-155">Bu, kesinlikle önemsiz erişilmediğinden değerlendirilmemelidir rağmen bir EF sağlayıcısı kendiniz oluşturmanız mümkündür.</span><span class="sxs-lookup"><span data-stu-id="f06db-155">It is certainly possible to create an EF provider yourself although it should not be considered a trivial undertaking.</span></span> <span data-ttu-id="f06db-156">EF6 sağlayıcı modeli hakkında bilgi için yukarıdaki bağlantıya başlatmak için iyi bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="f06db-156">The the link above about the EF6 provider model is a good place to start.</span></span> <span data-ttu-id="f06db-157">Ayrıca SQL Server ve SQL CE sağlayıcısı dahil kodu kullanmak faydalı [EF açık kaynak kod tabanı](https://github.com/aspnet/EntityFramework6) bir başlangıç noktası olarak veya başvuru.</span><span class="sxs-lookup"><span data-stu-id="f06db-157">You may also find it useful to use the code for the SQL Server and SQL CE provider included in the [EF open source codebase](https://github.com/aspnet/EntityFramework6) as a starting point or for reference.</span></span>

<span data-ttu-id="f06db-158">EF sağlayıcısı EF6'ile başlayan daha az sıkı bir şekilde temel alınan ADO.NET sağlayıcısı için katı olarak birleştirilmiş olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f06db-158">Note that starting with EF6 the EF provider is less tightly coupled to the underlying ADO.NET provider.</span></span> <span data-ttu-id="f06db-159">Bu, yazma ya da ADO.NET sınıfları sarmalamak gerek kalmadan bir EF sağlayıcı yazma kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="f06db-159">This makes it easier to write an EF provider without needing to write or wrap the ADO.NET classes.</span></span>
