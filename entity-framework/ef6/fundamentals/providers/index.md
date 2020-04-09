---
title: Taraf Çerçeve Sağlayıcıları - EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
uid: ef6/fundamentals/providers/index
ms.openlocfilehash: 661398e7d6037875ce0cdb15c221a729d1f0c7d8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419365"
---
# <a name="entity-framework-6-providers"></a><span data-ttu-id="b9cfd-102">Varlık Çerçevesi 6 Sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="b9cfd-102">Entity Framework 6 Providers</span></span>
> [!NOTE]
> <span data-ttu-id="b9cfd-103">**EF6 Yalnızca -** Bu sayfada tartışılan özellikler, API'ler vb. Entity Framework 6'da tanıtıldı.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="b9cfd-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="b9cfd-105">Varlık Çerçevesi şu anda açık kaynak lisansı altında geliştirilmektedir ve EF6 ve üzeri .NET Çerçevesi'nin bir parçası olarak sevk edilmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-105">The Entity Framework is now being developed under an open-source license and EF6 and above will not be shipped as part of the .NET Framework.</span></span> <span data-ttu-id="b9cfd-106">Bunun birçok avantajı vardır, ancak EF sağlayıcılarının EF6 montajlarına karşı yeniden inşa edilmesi de gerekir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-106">This has many advantages but also requires that EF providers be rebuilt against the EF6 assemblies.</span></span> <span data-ttu-id="b9cfd-107">Bu, EF5 ve altındaki EF sağlayıcılarının yeniden oluşturulana kadar EF6 ile çalışmayacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-107">This means that EF providers for EF5 and below will not work with EF6 until they have been rebuilt.</span></span>

## <a name="which-providers-are-available-for-ef6"></a><span data-ttu-id="b9cfd-108">EF6 için hangi sağlayıcılar kullanılabilir?</span><span class="sxs-lookup"><span data-stu-id="b9cfd-108">Which providers are available for EF6?</span></span>

<span data-ttu-id="b9cfd-109">EF6 için yeniden inşa edildiğini bildiğimiz sağlayıcılar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="b9cfd-109">Providers we are aware of that have been rebuilt for EF6 include:</span></span>

*   <span data-ttu-id="b9cfd-110">**Microsoft SQL Server sağlayıcısı**</span><span class="sxs-lookup"><span data-stu-id="b9cfd-110">**Microsoft SQL Server provider**</span></span>
    *   <span data-ttu-id="b9cfd-111">[Varlık Çerçevesi açık kaynak kod tabanından oluşturulmuştur](https://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="b9cfd-111">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="b9cfd-112">[EntityFramework NuGet paketinin](https://nuget.org/packages/EntityFramework) bir parçası olarak sevk edildi</span><span class="sxs-lookup"><span data-stu-id="b9cfd-112">Shipped as part of the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework)</span></span>
*   <span data-ttu-id="b9cfd-113">**Microsoft SQL Server Compact Edition sağlayıcısı**</span><span class="sxs-lookup"><span data-stu-id="b9cfd-113">**Microsoft SQL Server Compact Edition provider**</span></span>
    *   <span data-ttu-id="b9cfd-114">[Varlık Çerçevesi açık kaynak kod tabanından oluşturulmuştur](https://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="b9cfd-114">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="b9cfd-115">[EntityFramework.SqlServerCompact NuGet paketinde](https://nuget.org/packages/EntityFramework.SqlServerCompact) gönderildi</span><span class="sxs-lookup"><span data-stu-id="b9cfd-115">Shipped in the [EntityFramework.SqlServerCompact NuGet package](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span></span>
*   [<span data-ttu-id="b9cfd-116">**Devart dotConnect Veri Sağlayıcıları**</span><span class="sxs-lookup"><span data-stu-id="b9cfd-116">**Devart dotConnect Data Providers**</span></span>](https://www.devart.com/dotconnect/)
    *   <span data-ttu-id="b9cfd-117">Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 ve SQL Server dahil olmak üzere çeşitli veritabanları için [Devart'ın](https://www.devart.com/) üçüncü taraf sağlayıcıları vardır</span><span class="sxs-lookup"><span data-stu-id="b9cfd-117">There are third-party providers from [Devart](https://www.devart.com/) for a variety of databases including Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2, and SQL Server</span></span>
*   [<span data-ttu-id="b9cfd-118">**CData Yazılım sağlayıcıları**</span><span class="sxs-lookup"><span data-stu-id="b9cfd-118">**CData Software providers**</span></span>](https://www.cdata.com/ado/)
    *   <span data-ttu-id="b9cfd-119">Salesforce, Azure Table Storage, MySql ve daha birçok veri deposu için [CData Software'in](https://www.cdata.com/ado/) üçüncü taraf sağlayıcıları vardır</span><span class="sxs-lookup"><span data-stu-id="b9cfd-119">There are third-party providers from [CData Software](https://www.cdata.com/ado/) for a variety of data stores including Salesforce, Azure Table Storage, MySql, and many more</span></span>
*   <span data-ttu-id="b9cfd-120">**Firebird sağlayıcısı**</span><span class="sxs-lookup"><span data-stu-id="b9cfd-120">**Firebird provider**</span></span>
    *   <span data-ttu-id="b9cfd-121">[NuGet Paketi](https://www.nuget.org/packages/EntityFramework.Firebird/) olarak kullanılabilir</span><span class="sxs-lookup"><span data-stu-id="b9cfd-121">Available as a [NuGet Package](https://www.nuget.org/packages/EntityFramework.Firebird/)</span></span>
*   <span data-ttu-id="b9cfd-122">**Görsel Fox Pro sağlayıcısı**</span><span class="sxs-lookup"><span data-stu-id="b9cfd-122">**Visual Fox Pro provider**</span></span>
    *   <span data-ttu-id="b9cfd-123">[NuGet paketi](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/) olarak kullanılabilir</span><span class="sxs-lookup"><span data-stu-id="b9cfd-123">Available as a [NuGet package](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span></span>
*   <span data-ttu-id="b9cfd-124">**MySQL**</span><span class="sxs-lookup"><span data-stu-id="b9cfd-124">**MySQL**</span></span>
    *   [<span data-ttu-id="b9cfd-125">Entity Framework için MySQL Bağlayıcısı/NET</span><span class="sxs-lookup"><span data-stu-id="b9cfd-125">MySQL Connector/NET for Entity Framework</span></span>](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   <span data-ttu-id="b9cfd-126">**PostgreSQL**</span><span class="sxs-lookup"><span data-stu-id="b9cfd-126">**PostgreSQL**</span></span>
    *   <span data-ttu-id="b9cfd-127">Npgsql [NuGet paketi](https://www.nuget.org/packages/EntityFramework6.Npgsql/) olarak kullanılabilir</span><span class="sxs-lookup"><span data-stu-id="b9cfd-127">Npgsql is available as a [NuGet package](https://www.nuget.org/packages/EntityFramework6.Npgsql/)</span></span>
*   <span data-ttu-id="b9cfd-128">**Oracle**</span><span class="sxs-lookup"><span data-stu-id="b9cfd-128">**Oracle**</span></span>
    *   <span data-ttu-id="b9cfd-129">ODP.NET [NuGet paketi](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/) olarak kullanılabilir</span><span class="sxs-lookup"><span data-stu-id="b9cfd-129">ODP.NET is available as a [NuGet package](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span></span>

<span data-ttu-id="b9cfd-130">Bu listeye eklenmesinin belirli bir sağlayıcı için işlevsellik düzeyini veya destek düzeyini göstermediğini, yalnızca EF6 için bir yapının kullanıma sunulduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-130">Note that inclusion in this list does not indicate the level of functionality or support for a given provider, only that a build for EF6 has been made available.</span></span>

## <a name="registering-ef-providers"></a><span data-ttu-id="b9cfd-131">EF sağlayıcılarının kaydedilmesi</span><span class="sxs-lookup"><span data-stu-id="b9cfd-131">Registering EF providers</span></span>

<span data-ttu-id="b9cfd-132">Entity Framework 6 EF sağlayıcıları ndan başlayarak kod tabanlı yapılandırma kullanılarak veya uygulamanın config dosyasına kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-132">Starting with Entity Framework 6 EF providers can be registered using either code-based configuration or in the application’s config file.</span></span>

### <a name="config-file-registration"></a><span data-ttu-id="b9cfd-133">Config dosya kaydı</span><span class="sxs-lookup"><span data-stu-id="b9cfd-133">Config file registration</span></span>

<span data-ttu-id="b9cfd-134">EF sağlayıcısının app.config veya web.config'e kaydolması aşağıdaki biçimdedir:</span><span class="sxs-lookup"><span data-stu-id="b9cfd-134">Registration of the EF provider in app.config or web.config has the following format:</span></span>


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

<span data-ttu-id="b9cfd-135">EF sağlayıcısı NuGet'den yüklüyse, NuGet paketinin bu kaydı otomatik olarak config dosyasına ekleyeceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-135">Note that often if the EF provider is installed from NuGet, then the NuGet package will automatically add this registration to the config file.</span></span> <span data-ttu-id="b9cfd-136">NuGet paketini uygulamanızın başlangıç projesi olmayan bir projeye yüklerseniz, başlangıç projeniz için kaydı config dosyasına kopyalamanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-136">If you install the NuGet package into a project that is not the startup project for your app, then you may need to copy the registration into the config file for your startup project.</span></span>

<span data-ttu-id="b9cfd-137">Bu kayıttaki "değişmez Ad", ADO.NET sağlayıcısını tanımlamak için kullanılan değişmez adla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-137">The “invariantName” in this registration is the same invariant name used to identify an ADO.NET provider.</span></span> <span data-ttu-id="b9cfd-138">Bu, Bir DbProviderFactorys kaydında "değişmez" özniteliği ve bağlantı dize kaydındaki "providerName" özniteliği olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-138">This can be found as the “invariant” attribute in a DbProviderFactories registration and as the “providerName” attribute in a connection string registration.</span></span> <span data-ttu-id="b9cfd-139">Kullanılacak değişmez ad da sağlayıcı için belgelere eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-139">The invariant name to use should also be included in documentation for the provider.</span></span> <span data-ttu-id="b9cfd-140">Değişmez adlara örnek olarak SQL Server için "System.Data.SqlClientClient" ve SQL Server Compact için "System.Data.SqlServerCe.4.0" verilebilir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-140">Examples of invariant names are “System.Data.SqlClient” for SQL Server and “System.Data.SqlServerCe.4.0” for SQL Server Compact.</span></span>

<span data-ttu-id="b9cfd-141">Bu kayıttaki "tür" "System.Data.Entity.Core.Common.DbProviderServices" adresinden türeyen sağlayıcı türünün montaja uygun adıdır.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-141">The “type” in this registration is the assembly-qualified name of the provider type that derives from “System.Data.Entity.Core.Common.DbProviderServices”.</span></span> <span data-ttu-id="b9cfd-142">Örneğin, SQL Compact için kullanılacak dize "System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact" dır.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-142">For example, the string to use for SQL Compact is “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span></span> <span data-ttu-id="b9cfd-143">Burada kullanılacak tür sağlayıcı için belgelere eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-143">The type to use here should be included in documentation for the provider.</span></span>

### <a name="code-based-registration"></a><span data-ttu-id="b9cfd-144">Kod tabanlı kayıt</span><span class="sxs-lookup"><span data-stu-id="b9cfd-144">Code-based registration</span></span>

<span data-ttu-id="b9cfd-145">VARLıK Framework 6 ile başlayan EF için uygulama çapında yapılandırma kod olarak belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-145">Starting with Entity Framework 6 application-wide configuration for EF can be specified in code.</span></span> <span data-ttu-id="b9cfd-146">Tüm ayrıntılar için Entity _[Framework Code Tabanlı Yapılandırma'ya](https://msdn.microsoft.com/data/jj680699)_ bakın.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-146">For full details see _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_.</span></span> <span data-ttu-id="b9cfd-147">Kod tabanlı yapılandırmakullanarak bir EF sağlayıcısını kaydetmenin normal yolu, System.Data.Entity.DbConfiguration'dan türetilen yeni bir sınıf oluşturmak ve onu DbContext sınıfınızla aynı derlemeye yerleştirmektir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-147">The normal way to register an EF provider using code-based configuration is to create a new class that derives from System.Data.Entity.DbConfiguration and place it in the same assembly as your DbContext class.</span></span> <span data-ttu-id="b9cfd-148">DbConfiguration sınıfınız sağlayıcıyı oluşturucusuna kaydettirmelidir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-148">Your DbConfiguration class should then register the provider in its constructor.</span></span> <span data-ttu-id="b9cfd-149">Örneğin, SQL Compact sağlayıcısını kaydetmek için DbConfiguration sınıfı aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="b9cfd-149">For example, to register the SQL Compact provider the DbConfiguration class looks like this:</span></span>

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

<span data-ttu-id="b9cfd-150">Bu kodda "SqlCeProviderServices.ProviderInvariantName" SQL Server Compact sağlayıcı değişmez ad dizesi ("System.Data.SqlServerCe.4.0") ve SqlCeProviderServices.Instance sql Compact EF sağlayıcısının singleton örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-150">In this code “SqlCeProviderServices.ProviderInvariantName” is a convenience for the SQL Server Compact provider invariant name string (“System.Data.SqlServerCe.4.0”) and SqlCeProviderServices.Instance returns the singleton instance of the SQL Compact EF provider.</span></span>

## <a name="what-if-the-provider-i-need-isnt-available"></a><span data-ttu-id="b9cfd-151">İhtiyacım olan sağlayıcı kullanılamıyorsa ne olur?</span><span class="sxs-lookup"><span data-stu-id="b9cfd-151">What if the provider I need isn’t available?</span></span>

<span data-ttu-id="b9cfd-152">Sağlayıcı EF'nin önceki sürümleri için kullanılabilirse, sağlayıcının sahibiyle iletişime geçmenizi ve onlardan bir EF6 sürümü oluşturmalarını istemenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-152">If the provider is available for previous versions of EF, then we encourage you to contact the owner of the provider and ask them to create an EF6 version.</span></span> <span data-ttu-id="b9cfd-153">[EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md)için belgelere bir başvuru eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-153">You should include a reference to the [documentation for the EF6 provider model](~/ef6/fundamentals/providers/provider-model.md).</span></span>

## <a name="can-i-write-a-provider-myself"></a><span data-ttu-id="b9cfd-154">Ben de bir sağlayıcı yazabilir miyim?</span><span class="sxs-lookup"><span data-stu-id="b9cfd-154">Can I write a provider myself?</span></span>

<span data-ttu-id="b9cfd-155">Önemsiz bir girişim olarak kabul edilmemelidir rağmen kesinlikle bir EF sağlayıcı kendiniz oluşturmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-155">It is certainly possible to create an EF provider yourself although it should not be considered a trivial undertaking.</span></span> <span data-ttu-id="b9cfd-156">EF6 sağlayıcı modeli hakkında yukarıdaki bağlantı başlamak için iyi bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-156">The the link above about the EF6 provider model is a good place to start.</span></span> <span data-ttu-id="b9cfd-157">Ayrıca, [EF açık kaynak kod tabanında](https://github.com/aspnet/EntityFramework6) yer alan SQL Server ve SQL CE sağlayıcısının kodunu başlangıç noktası olarak veya başvuru için kullanmayı da yararlı bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-157">You may also find it useful to use the code for the SQL Server and SQL CE provider included in the [EF open source codebase](https://github.com/aspnet/EntityFramework6) as a starting point or for reference.</span></span>

<span data-ttu-id="b9cfd-158">EF6'dan başlayarak EF sağlayıcısının altta yatan ADO.NET sağlayıcısıyla daha az sıkı bir şekilde birleştiğinde olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-158">Note that starting with EF6 the EF provider is less tightly coupled to the underlying ADO.NET provider.</span></span> <span data-ttu-id="b9cfd-159">Bu, ADO.NET sınıflarını yazmaya veya sarmaya gerek kalmadan bir EF sağlayıcısı yazmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="b9cfd-159">This makes it easier to write an EF provider without needing to write or wrap the ADO.NET classes.</span></span>
