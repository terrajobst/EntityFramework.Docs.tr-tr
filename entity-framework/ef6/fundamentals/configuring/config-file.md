---
title: Yapılandırma dosyası ayarları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 86389e4a3a3bac46e2a4cf2da648a4b19e29f3c3
ms.sourcegitcommit: 299011fc4bd576eed58a4274f967639fa13fec53
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886561"
---
# <a name="configuration-file-settings"></a><span data-ttu-id="96908-102">Yapılandırma dosyası ayarları</span><span class="sxs-lookup"><span data-stu-id="96908-102">Configuration File Settings</span></span>
<span data-ttu-id="96908-103">Entity Framework, yapılandırma dosyasından bir dizi ayar belirtilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="96908-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="96908-104">Genel EF ' de bir ' yapılandırma üzerinden kural ' prensibi: bu gönderide ele alınan tüm ayarların varsayılan bir davranışı vardır; yalnızca varsayılan değer gereksinimlerinizi karşılamıyorsa ayarı değiştirme konusunda endişelenmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="96908-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="96908-105">Kod tabanlı alternatif</span><span class="sxs-lookup"><span data-stu-id="96908-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="96908-106">Bu ayarların tümü kod kullanılarak da uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="96908-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="96908-107">EF6 ' den itibaren, koddan yapılandırma uygulamanın merkezi bir yolunu sağlayan [kod tabanlı yapılandırma](code-based.md)ekledik.</span><span class="sxs-lookup"><span data-stu-id="96908-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="96908-108">EF6 ' dan önce yapılandırma yine koddan uygulanabilir, ancak farklı bölgeleri yapılandırmak için çeşitli API 'Ler kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="96908-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="96908-109">Yapılandırma dosyası seçeneği, kodunuzun güncelleştirilmesi gerekmeden dağıtım sırasında bu ayarların kolayca değiştirilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="96908-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="96908-110">Entity Framework yapılandırma bölümü</span><span class="sxs-lookup"><span data-stu-id="96908-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="96908-111">EF 4.1 ile başlayarak, yapılandırma dosyasının **appSettings** bölümünü kullanarak bir bağlam için veritabanı başlatıcısı ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96908-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="96908-112">EF 4,3 ' de, yeni ayarları işlemek için özel **EntityFramework** bölümünü kullanıma sunduk.</span><span class="sxs-lookup"><span data-stu-id="96908-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="96908-113">Entity Framework, eski biçimi kullanarak veritabanı başlatıcıları kümesini tanımaya devam eder, ancak mümkün olduğunda yeni biçime geçmeyi öneririz.</span><span class="sxs-lookup"><span data-stu-id="96908-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="96908-114">EntityFramework NuGet paketini yüklediğinizde **EntityFramework** bölümü projenizin yapılandırma dosyasına otomatik olarak eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="96908-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a><span data-ttu-id="96908-115">Bağlantı Dizeleri</span><span class="sxs-lookup"><span data-stu-id="96908-115">Connection Strings</span></span>  

<span data-ttu-id="96908-116">[Bu sayfada](~/ef6/fundamentals/configuring/connection-strings.md) , yapılandırma dosyasındaki bağlantı dizeleri de dahil olmak üzere, kullanılacak veritabanını Entity Framework nasıl belirlediği hakkında daha fazla bilgi verilmektedir.</span><span class="sxs-lookup"><span data-stu-id="96908-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="96908-117">Bağlantı dizeleri standart **ConnectionString** öğesine gider ve **EntityFramework** bölümünü gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="96908-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="96908-118">Code First tabanlı modeller normal ADO.NET bağlantı dizelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="96908-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="96908-119">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="96908-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="96908-120">EF Designer tabanlı modeller özel EF bağlantı dizeleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="96908-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="96908-121">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="96908-121">For example:</span></span>  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient;
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="96908-122">Kod tabanlı yapılandırma türü (EF6 Onsürümleri)</span><span class="sxs-lookup"><span data-stu-id="96908-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="96908-123">EF6 ile başlayarak, uygulamanızda [kod tabanlı yapılandırma](code-based.md) için kullanmak üzere, EF Için DBConfiguration belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96908-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="96908-124">Çoğu durumda, EF otomatik olarak DbConfiguration 'ı bulacaktır bu ayarı belirtmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="96908-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="96908-125">Yapılandırma dosyanızda DbConfiguration 'ı ne zaman belirtmeniz gerektiği hakkında ayrıntılı bilgi için [kod tabanlı yapılandırmanın](code-based.md) **hareketli DBConfiguration** bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="96908-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="96908-126">Bir DbConfiguration türü ayarlamak için **Codeconfigurationtype** öğesinde derleme nitelikli tür adını belirtirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96908-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="96908-127">Bütünleştirilmiş kod nitelikli adı ad alanı nitelenmiş addır, ardından virgül ve türün bulunduğu derleme.</span><span class="sxs-lookup"><span data-stu-id="96908-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="96908-128">İsteğe bağlı olarak, derleme sürümünü, kültürü ve ortak anahtar belirtecini de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96908-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="96908-129">EF veritabanı sağlayıcıları (EF6 sürümleri)</span><span class="sxs-lookup"><span data-stu-id="96908-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="96908-130">EF6 'ten önce, bir veritabanı sağlayıcısının Entity Framework özgü bölümlerinin çekirdek ADO.NET sağlayıcısı 'nın bir parçası olarak eklenmesi gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="96908-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="96908-131">EF6 ile başlayarak, EF 'e özgü parçalar artık yönetilir ve ayrı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="96908-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="96908-132">Normalde, sağlayıcıları kendiniz kaydetmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="96908-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="96908-133">Bu işlem genellikle sağlayıcı tarafından yüklendiğinde yapılır.</span><span class="sxs-lookup"><span data-stu-id="96908-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="96908-134">Sağlayıcılar, **EntityFramework** bölümünün **sağlayıcılar** alt bölümü altına bir **sağlayıcı** öğesi eklenerek kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="96908-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="96908-135">Sağlayıcı girişi için iki gerekli öznitelik vardır:</span><span class="sxs-lookup"><span data-stu-id="96908-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="96908-136">**InvariantName** , bu EF sağlayıcının hedeflediği Core ADO.net sağlayıcısını tanımlar</span><span class="sxs-lookup"><span data-stu-id="96908-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="96908-137">**tür** , EF sağlayıcı uygulamasının derleme nitelikli tür adıdır</span><span class="sxs-lookup"><span data-stu-id="96908-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="96908-138">Bütünleştirilmiş kod nitelikli adı ad alanı nitelenmiş addır, ardından virgül ve türün bulunduğu derleme.</span><span class="sxs-lookup"><span data-stu-id="96908-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="96908-139">İsteğe bağlı olarak, derleme sürümünü, kültürü ve ortak anahtar belirtecini de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96908-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="96908-140">Bir örnek olarak, Entity Framework yüklediğinizde varsayılan SQL Server sağlayıcıyı kaydetmek için oluşturulan girdidir.</span><span class="sxs-lookup"><span data-stu-id="96908-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="96908-141">Yakalayıcılar (EF 6.1 ve sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="96908-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="96908-142">EF 6.1 ile başlayarak, yapılandırma dosyasına dinleyici kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96908-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="96908-143">Yakalayıcılar, veritabanı sorguları yürütme, bağlantılar açma vb. gibi belirli işlemleri gerçekleştirdiğinde, ek mantık çalıştırmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="96908-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="96908-144">Dinleyici oluşturma, **EntityFramework** bölümünün alt bölümünün altına bir **yakalayıcısı** öğesi eklenerek kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="96908-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="96908-145">Örneğin, aşağıdaki yapılandırma, tüm veritabanı işlemlerini konsola kaydedecek yerleşik **Databasegünlükçü** yakalayıcısını kaydeder.</span><span class="sxs-lookup"><span data-stu-id="96908-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="96908-146">Veritabanı Işlemlerini bir dosyaya kaydetme (EF 6.1 ve sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="96908-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="96908-147">Bir sorunu ayıklamanıza yardımcı olması için, mevcut bir uygulamaya günlük eklemek istediğinizde özellikle de yapılandırma dosyası aracılığıyla kayıt yaptırıcılar yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="96908-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="96908-148">**Databasegünlükçü** , dosya adını bir oluşturucu parametresi olarak sağlayarak bir dosyaya günlük kaydını destekler.</span><span class="sxs-lookup"><span data-stu-id="96908-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="96908-149">Bu, varsayılan olarak, uygulama her başlatıldığında günlük dosyasının üzerine yazılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="96908-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="96908-150">Bunun yerine, zaten varsa günlük dosyasına ekleyin, örneğin:</span><span class="sxs-lookup"><span data-stu-id="96908-150">To instead append to the log file if it already exists use something like:</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="96908-151">**Databasegünlükçü** ve kayıt yaptırıcılar hakkında daha fazla bilgi için, bkz. Web [günlüğü gönderi aşv 6,1: Yeniden derleme](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/)olmadan günlüğe kaydetme açılıyor.</span><span class="sxs-lookup"><span data-stu-id="96908-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="96908-152">Code First varsayılan bağlantı fabrikası</span><span class="sxs-lookup"><span data-stu-id="96908-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="96908-153">Yapılandırma bölümü, Code First bir bağlam için kullanılacak veritabanını bulmak için kullanması gereken varsayılan bir bağlantı fabrikası belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="96908-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="96908-154">Varsayılan bağlantı fabrikası yalnızca bir bağlam için yapılandırma dosyasına bir bağlantı dizesi eklendiğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="96908-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="96908-155">EF NuGet paketini yüklediğinizde, hangisinin yüklü olduğuna bağlı olarak SQL Express veya LocalDB 'ye işaret eden bir varsayılan bağlantı fabrikası kayıtlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="96908-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="96908-156">Bir bağlantı fabrikası ayarlamak için, **Defaultconnectionfactory** öğesinde derleme nitelikli tür adını belirtirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96908-156">To set a connection factory, you specify the assembly qualified type name in the **defaultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="96908-157">Bütünleştirilmiş kod nitelikli adı ad alanı nitelenmiş addır, ardından virgül ve türün bulunduğu derleme.</span><span class="sxs-lookup"><span data-stu-id="96908-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="96908-158">İsteğe bağlı olarak, derleme sürümünü, kültürü ve ortak anahtar belirtecini de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96908-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="96908-159">Kendi varsayılan bağlantı fabrikanızı ayarlamaya bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="96908-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="96908-160">Yukarıdaki örnek, özel fabrikasının parametresiz bir oluşturucuya sahip olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="96908-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="96908-161">Gerekirse, **Parameters** öğesini kullanarak Oluşturucu parametreleri belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96908-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="96908-162">Örneğin, Entity Framework dahil olan SqlCeConnectionFactory, oluşturucuya bir sağlayıcı sabit adı sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="96908-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="96908-163">Sağlayıcı sabit adı, kullanmak istediğiniz SQL Compact sürümünü tanımlar.</span><span class="sxs-lookup"><span data-stu-id="96908-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="96908-164">Aşağıdaki yapılandırma, bağlamların SQL Compact sürüm 4,0 ' i varsayılan olarak kullanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="96908-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="96908-165">Varsayılan bir bağlantı fabrikası ayarlamazsanız Code First, ' nin üzerine gelip `.\SQLEXPRESS`SqlConnectionFactory 'yi kullanır.</span><span class="sxs-lookup"><span data-stu-id="96908-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="96908-166">SqlConnectionFactory ayrıca bağlantı dizesinin parçalarını geçersiz kılmanızı sağlayan bir oluşturucuya sahiptir.</span><span class="sxs-lookup"><span data-stu-id="96908-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="96908-167">Dışında SQL Server bir örnek `.\SQLEXPRESS` kullanmak istiyorsanız, bu oluşturucuyu sunucuyu ayarlamak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96908-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="96908-168">Aşağıdaki yapılandırma, Code First bir açık bağlantı dizesi kümesi olmayan bağlamlarda **MyDatabaseServer** 'ı kullanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="96908-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="96908-169">Varsayılan olarak, Oluşturucu bağımsız değişkenlerinin dize türünde olduğu varsayılır.</span><span class="sxs-lookup"><span data-stu-id="96908-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="96908-170">Bunu değiştirmek için Type özniteliğini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96908-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="96908-171">Veritabanı başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="96908-171">Database Initializers</span></span>  

<span data-ttu-id="96908-172">Veritabanı başlatıcıları bağlam temelinde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="96908-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="96908-173">**Bağlam** öğesi kullanılarak yapılandırma dosyasında ayarlanamazlar.</span><span class="sxs-lookup"><span data-stu-id="96908-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="96908-174">Bu öğe, yapılandırılmakta olan bağlamı tanımlamak için derleme nitelikli adını kullanır.</span><span class="sxs-lookup"><span data-stu-id="96908-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="96908-175">Varsayılan olarak, Code First bağlamları CreateDatabaseIfNotExists başlatıcısı 'nı kullanacak şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="96908-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="96908-176">**Bağlam** öğesinde, veritabanı başlatmasını devre dışı bırakmak için kullanılabilecek bir **disabledatabaseınitialization** özniteliği vardır.</span><span class="sxs-lookup"><span data-stu-id="96908-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="96908-177">Örneğin, aşağıdaki yapılandırma MyAssembly. dll içinde tanımlanan blog. BlogContext bağlamı için veritabanını başlatmayı devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="96908-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="96908-178">Özel bir başlatıcı ayarlamak için **Databaseınitializer** öğesini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96908-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="96908-179">Oluşturucu parametreleri varsayılan bağlantı fabrikaları ile aynı sözdizimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="96908-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

<span data-ttu-id="96908-180">Entity Framework bulunan genel veritabanı başlatıcılarının birini yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96908-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="96908-181">**Tür** özniteliği genel türler için .NET Framework biçimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="96908-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="96908-182">Örneğin, Code First Migrations kullanıyorsanız, veritabanını `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` Başlatıcı kullanılarak otomatik olarak geçirilecek şekilde yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96908-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
