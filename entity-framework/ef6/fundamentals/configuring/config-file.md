---
title: Yapılandırma dosyası ayarlarının - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 949ad4f030205753c5fbf9b95f4d183d8c0d1fe7
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490882"
---
# <a name="configuration-file-settings"></a><span data-ttu-id="fe0f4-102">Yapılandırma dosyası ayarları</span><span class="sxs-lookup"><span data-stu-id="fe0f4-102">Configuration File Settings</span></span>
<span data-ttu-id="fe0f4-103">Entity Framework çeşitli ayarlar yapılandırma dosyasından belirtilmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="fe0f4-104">Genel 'kuralı yapılandırmanız üzerinde' İlkesi EF izler: Bu yayında tartışılan tüm ayarları varsayılan bir davranışa sahip, yalnızca varsayılan artık gereksinimlerinizi sağladığında ayarını değiştirme hakkında endişe etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="fe0f4-105">Bir kod tabanlı alternatif</span><span class="sxs-lookup"><span data-stu-id="fe0f4-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="fe0f4-106">Tüm bu ayarlar, kod kullanarak da uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="fe0f4-107">İçinde kullanıma sunduk EF6 başlangıç [kod tabanlı yapılandırma](code-based.md), uygulama kodu yapılandırma merkezi bir yolu sağlar.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="fe0f4-108">EF6 önce yapılandırma hala koddan uygulanabilir ancak farklı alanları yapılandırmak için çeşitli API'leri kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="fe0f4-109">Dağıtım sırasında kodu güncelleştirmeden kolayca değiştirilmesi için bu ayarları yapılandırma dosyası seçeneği sağlar.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="fe0f4-110">Entity Framework yapılandırma bölümü</span><span class="sxs-lookup"><span data-stu-id="fe0f4-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="fe0f4-111">EF4.1 ile başlangıç bağlamı kullanarak veritabanı Başlatıcı ayarlayabilirsiniz **appSettings** yapılandırma dosyasının.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="fe0f4-112">EF 4.3 içinde özel sunduk **entityFramework** yeni ayarları işlemek için bölüm.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="fe0f4-113">Entity Framework hala eski biçimi kullanılarak ayarlanan veritabanı başlatıcılar algılar, ancak mümkün olduğunca yeni biçime geçmeniz önerilir.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="fe0f4-114">**EntityFramework** EntityFramework NuGet paketi yüklendiğinde bölümünde projenizin yapılandırma dosyasına otomatik olarak eklendiğinden.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

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

## <a name="connection-strings"></a><span data-ttu-id="fe0f4-115">Bağlantı dizeleri</span><span class="sxs-lookup"><span data-stu-id="fe0f4-115">Connection Strings</span></span>  

<span data-ttu-id="fe0f4-116">[Bu sayfa](~/ef6/fundamentals/configuring/connection-strings.md) yapılandırma dosyasında bağlantı dizeleri dahil olmak üzere daha fazla ayrıntı Entity Framework veritabanı kullanılmak üzere nasıl belirlediğini sağlar.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="fe0f4-117">Bağlantı dizeleri, standart go **connectionStrings** öğesi ve gerekli olmayan **entityFramework** bölümü.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="fe0f4-118">İlk göre kod modelleri normal ADO.NET bağlantı dizesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="fe0f4-119">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fe0f4-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="fe0f4-120">EF Designer modelleri kullanım özel EF bağlantı dizelerini temel.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="fe0f4-121">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fe0f4-121">For example:</span></span>  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="fe0f4-122">Kod tabanlı yapılandırma türü (EF6 sonrası)</span><span class="sxs-lookup"><span data-stu-id="fe0f4-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="fe0f4-123">EF6 ile başlayarak, kullanılacak EF için DbConfiguration belirtebilirsiniz [kod tabanlı yapılandırma](code-based.md) uygulamanızdaki.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="fe0f4-124">Çoğu durumda EF, DbConfiguration otomatik olarak bulmak gibi bu ayarı belirtmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="fe0f4-125">Ne zaman yapılandırma dosyanızda DbConfiguration belirtmeniz gerekebilir, Ayrıntılar için bkz. **taşıma DbConfiguration** bölümünü [kod tabanlı yapılandırma](code-based.md).</span><span class="sxs-lookup"><span data-stu-id="fe0f4-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="fe0f4-126">DbConfiguration türünü belirlemek için derleme nitelikli tür adı belirtin. **codeConfigurationType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="fe0f4-127">Bir bütünleştirilmiş kod adı türü bulunan derleme ardından virgül tarafından izlenen ad alanı tam adıdır.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="fe0f4-128">İsteğe bağlı olarak yapabilecekleriniz de derleme sürümü, kültürü ve genel anahtar belirtecini belirtin.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="fe0f4-129">EF veritabanı sağlayıcıları (EF6 sonrası)</span><span class="sxs-lookup"><span data-stu-id="fe0f4-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="fe0f4-130">EF6 önce veritabanı sağlayıcısı Entity Framework özgü bölümlerini çekirdek ADO.NET sağlayıcısı bir parçası olarak dahil edilmesi gerekti.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="fe0f4-131">EF6 ile başlayarak, EF belirli kısımlarını artık yönetilir ve ayrı olarak kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="fe0f4-132">Normalde kendiniz sağlayıcılarını kaydetme gerekmez.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="fe0f4-133">Yüklediğinizde bu genellikle sağlayıcı tarafından gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="fe0f4-134">Sağlayıcılar dahil ederek kayıtlı bir **sağlayıcısı** öğesi altında **sağlayıcıları** alt kısmında **entityFramework** bölümü.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="fe0f4-135">Sağlayıcı girişi için gerekli iki öznitelikleri vardır:</span><span class="sxs-lookup"><span data-stu-id="fe0f4-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="fe0f4-136">**Invariantname** temel ADO.NET sağlayıcısı tanımlayan bu EF sağlayıcısı hedefleri</span><span class="sxs-lookup"><span data-stu-id="fe0f4-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="fe0f4-137">**tür** EF sağlayıcı uygulaması derleme nitelikli tür adı</span><span class="sxs-lookup"><span data-stu-id="fe0f4-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="fe0f4-138">Bir bütünleştirilmiş kod adı türü bulunan derleme ardından virgül tarafından izlenen ad alanı tam adıdır.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="fe0f4-139">İsteğe bağlı olarak yapabilecekleriniz de derleme sürümü, kültürü ve genel anahtar belirtecini belirtin.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="fe0f4-140">Örneğin, Entity Framework yüklediğinizde, varsayılan SQL Server sağlayıcıyı kaydetmek için oluşturulan girişi aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="fe0f4-141">Dinleyiciler (EF6.1 ve sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="fe0f4-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="fe0f4-142">Kesiciler, EF6.1 ile başlayan yapılandırma dosyasında kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="fe0f4-143">Kesiciler, EF açılış bağlantıları, vb. veritabanı sorguları, yürütme gibi bazı işlemleri gerçekleştirirken ek mantık çalıştırmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="fe0f4-144">Kesiciler dahil ederek kayıtlı bir **dinleyiciyi** öğesi altında **dinleyicileri** alt kısmında **entityFramework** bölümü.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="fe0f4-145">Örneğin, aşağıdaki yapılandırma yerleşik kaydeder **DatabaseLogger** tüm veritabanı işlemleri konsola oturum dinleyiciyi.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="fe0f4-146">Günlük veritabanı işlemleri bir dosyaya (EF6.1 ve sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="fe0f4-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="fe0f4-147">Günlük bir sorunla ilgili hataları ayıklamak yardımcı olmak için mevcut bir uygulamaya eklemek istediğiniz yapılandırma dosyası aracılığıyla dinleyicileri kaydetme özellikle yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="fe0f4-148">**DatabaseLogger** Oluşturucu parametresi olarak dosya adı sağlayarak dosyaya günlük kaydı destekler.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="fe0f4-149">Varsayılan olarak bu günlük dosyası, uygulama her başlatıldığında yeni bir dosya ile üzerine yazılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="fe0f4-150">Bunun yerine'ına eklenecek dosya zaten varsa kullanın aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="fe0f4-150">To instead append to the log file if it already exists use something like:</span></span>  

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

<span data-ttu-id="fe0f4-151">Hakkında daha fazla bilgi için **DatabaseLogger** ve dinleyicileri kayıt, blog gönderisine bakın [EF 6.1: derlemeden günlüğünü açma](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span><span class="sxs-lookup"><span data-stu-id="fe0f4-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="fe0f4-152">Kod ilk varsayılan bağlantı üretecini</span><span class="sxs-lookup"><span data-stu-id="fe0f4-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="fe0f4-153">Yapılandırma bölümü, Code First bağlam için kullanılacak bir veritabanı bulmak için kullanacağı varsayılan bağlantı üretecini belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="fe0f4-154">Bağlantı dizesi yapılandırma dosyasının bir bağlam için eklendiğinde varsayılan bağlantı üretecini yalnızca kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="fe0f4-155">EF NuGet paketi yüklendiğinde varsayılan bağlantı üretecini SQL Express LocalDB, hangisinin bağlı olarak, yüklü olduğu için veya işaret eden kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="fe0f4-156">Bağlantı üreteci ayarlamak için derleme nitelikli tür adı belirtin. **deafultConnectionFactory** öğesi.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-156">To set a connection factory, you specify the assembly qualified type name in the **deafultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="fe0f4-157">Bir bütünleştirilmiş kod adı türü bulunan derleme ardından virgül tarafından izlenen ad alanı tam adıdır.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="fe0f4-158">İsteğe bağlı olarak yapabilecekleriniz de derleme sürümü, kültürü ve genel anahtar belirtecini belirtin.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="fe0f4-159">Kendi varsayılan bağlantı üretecini ayarlamanın bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fe0f4-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="fe0f4-160">Yukarıdaki örnekte, parametresiz bir oluşturucu özel fabrika gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="fe0f4-161">Gerekirse, oluşturucu parametresi kullanarak belirtebilirsiniz **parametreleri** öğesi.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="fe0f4-162">Örneğin, varlık Çerçevesi'nde bulunan SqlCeConnectionFactory, oluşturucusuna bir sağlayıcının değişmez adı sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="fe0f4-163">Sağlayıcının değişmez adı kullanmak istediğiniz SQL Compact sürümünü tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="fe0f4-164">Aşağıdaki yapılandırma, SQL Compact sürüm 4. 0'ın varsayılan olarak kullanılacak bağlamları neden olur.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="fe0f4-165">Varsayılan bağlantı üretecini ayarlamazsanız, Code First işaret SqlConnectionFactory kullanan `.\SQLEXPRESS`.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="fe0f4-166">SqlConnectionFactory, ayrıca bağlantı dizesi bölümleri geçersiz kılmak izin veren bir oluşturucusu vardır.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="fe0f4-167">Başka bir SQL Server örneğini kullanmak istiyorsanız `.\SQLEXPRESS` sunucusu için bu oluşturucu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="fe0f4-168">Aşağıdaki yapılandırmayı kullanmak Code First neden **MyDatabaseServer** için ayarlanmış bir açık bir bağlantı dizesi yok bağlamı.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="fe0f4-169">Varsayılan olarak, oluşturucu bağımsız değişkenleri dize türünde olduğu varsayılır.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="fe0f4-170">Bunu değiştirmek için type özniteliği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="fe0f4-171">Veritabanı başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="fe0f4-171">Database Initializers</span></span>  

<span data-ttu-id="fe0f4-172">Veritabanı başlatıcılar bağlam içi olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="fe0f4-173">Kullanılarak yapılandırma dosyasında ayarlanabilir **bağlam** öğesi.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="fe0f4-174">Bu öğesi derleme nitelenmiş adı yapılandırılmakta içeriği tanımlamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="fe0f4-175">Varsayılan olarak, Code First bağlamları Createdatabaseıfnotexists Başlatıcı kullanmak üzere yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="fe0f4-176">Var olan bir **disableDatabaseInitialization** özniteliği **bağlam** veritabanı başlatma devre dışı bırakmak için kullanılabilir öğe.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="fe0f4-177">Örneğin, aşağıdaki yapılandırma veritabanı başlatma da MyAssembly.dll tanımlanan Blogging.BlogContext bağlamı için devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="fe0f4-178">Kullanabileceğiniz **databaseInitializer** özel bir başlatıcı ayarlamak için öğesi.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="fe0f4-179">Oluşturucu parametresi varsayılan bağlantı fabrikaları aynı sözdizimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

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

<span data-ttu-id="fe0f4-180">Entity Framework'e dahil edilen genel veritabanı başlatıcılar birini yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="fe0f4-181">**Türü** özniteliği, genel türler için .NET Framework biçimi kullanır.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="fe0f4-182">Örneğin, Code First Migrations'ı kullanıyorsanız, veritabanı kullanılarak otomatik olarak geçirilecek yapılandırabilirsiniz `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` Başlatıcı.</span><span class="sxs-lookup"><span data-stu-id="fe0f4-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
