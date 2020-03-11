---
title: Kod tabanlı yapılandırma-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 079a4ab30af74eac8b1f51ece5801ff40a867a29
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417871"
---
# <a name="code-based-configuration"></a><span data-ttu-id="c25f9-102">Kod tabanlı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c25f9-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="c25f9-103">**Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="c25f9-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="c25f9-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="c25f9-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="c25f9-105">Bir Entity Framework uygulama yapılandırması, bir yapılandırma dosyasında (App. config/Web. config) veya kod aracılığıyla belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="c25f9-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="c25f9-106">İkincisi, kod tabanlı yapılandırma olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="c25f9-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="c25f9-107">Bir yapılandırma dosyasındaki yapılandırma [ayrı bir makalede](config-file.md)açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c25f9-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="c25f9-108">Yapılandırma dosyası kod tabanlı yapılandırmayı temel alır.</span><span class="sxs-lookup"><span data-stu-id="c25f9-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="c25f9-109">Diğer bir deyişle, bir yapılandırma seçeneği hem kodda hem de yapılandırma dosyasında ayarlandıysa, yapılandırma dosyasındaki ayar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c25f9-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="c25f9-110">DbConfiguration kullanma</span><span class="sxs-lookup"><span data-stu-id="c25f9-110">Using DbConfiguration</span></span>  

<span data-ttu-id="c25f9-111">EF6 ve üzeri sürümlerde kod tabanlı yapılandırma System. Data. Entity. config. DbConfiguration sınıfının bir alt sınıfı oluşturularak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="c25f9-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="c25f9-112">Altsınıflama DbConfiguration olduğunda aşağıdaki yönergelerin izlenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="c25f9-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="c25f9-113">Uygulamanız için yalnızca bir DbConfiguration sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c25f9-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="c25f9-114">Bu sınıf, uygulama etki alanı genelinde ayarları belirler.</span><span class="sxs-lookup"><span data-stu-id="c25f9-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="c25f9-115">DbConfiguration sınıfınızı DbContext sınıfınız ile aynı derlemeye yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="c25f9-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="c25f9-116">(Bunu değiştirmek istiyorsanız, bkz. *DbConfiguration bölümünü taşıma* .)</span><span class="sxs-lookup"><span data-stu-id="c25f9-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="c25f9-117">DbConfiguration sınıfınıza Ortak parametresiz Oluşturucu verin.</span><span class="sxs-lookup"><span data-stu-id="c25f9-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="c25f9-118">Bu oluşturucunun içinden korumalı DbConfiguration yöntemlerini çağırarak yapılandırma seçeneklerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="c25f9-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="c25f9-119">Bu kurallara uyulması, EF 'in, modelinize erişmesi gereken her iki araç tarafından otomatik olarak yapılandırmanızı bulmasını ve kullanmasını, uygulamanızın ne zaman çalıştırılacağını de sağlar.</span><span class="sxs-lookup"><span data-stu-id="c25f9-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="c25f9-120">Örnek</span><span class="sxs-lookup"><span data-stu-id="c25f9-120">Example</span></span>  

<span data-ttu-id="c25f9-121">DbConfiguration 'dan türetilen bir sınıf şöyle görünebilir:</span><span class="sxs-lookup"><span data-stu-id="c25f9-121">A class derived from DbConfiguration might look like this:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDbConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

<span data-ttu-id="c25f9-122">Bu sınıf, başarısız veritabanı işlemlerini otomatik olarak yeniden denemek ve Code First kural tarafından oluşturulan veritabanları için yerel veritabanını kullanmak üzere SQL Azure yürütme stratejisini kullanmak için EF 'i ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c25f9-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="c25f9-123">DbConfiguration taşınıyor</span><span class="sxs-lookup"><span data-stu-id="c25f9-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="c25f9-124">DbConfiguration sınıfınızın DbContext sınıfınız ile aynı derlemeye yerleştirmesinin mümkün olmadığı durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="c25f9-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="c25f9-125">Örneğin, her biri farklı derlemede iki DbContext sınıfınız olabilir.</span><span class="sxs-lookup"><span data-stu-id="c25f9-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="c25f9-126">Bunu işlemek için iki seçenek vardır.</span><span class="sxs-lookup"><span data-stu-id="c25f9-126">There are two options for handling this.</span></span>  

<span data-ttu-id="c25f9-127">İlk seçenek, kullanılacak DbConfiguration örneğini belirtmek için yapılandırma dosyasını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="c25f9-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="c25f9-128">Bunu yapmak için entityFramework bölümünün codeConfigurationType özniteliğini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="c25f9-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="c25f9-129">Örnek:</span><span class="sxs-lookup"><span data-stu-id="c25f9-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="c25f9-130">CodeConfigurationType değeri, DbConfiguration sınıfınızın derleme ve ad alanı nitelikli adı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c25f9-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="c25f9-131">İkinci seçenek, DbConfigurationTypeAttribute öğesini bağlam sınıfınıza yerleştirkdır.</span><span class="sxs-lookup"><span data-stu-id="c25f9-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="c25f9-132">Örnek:</span><span class="sxs-lookup"><span data-stu-id="c25f9-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="c25f9-133">Özniteliğe geçirilen değer, yukarıda gösterilen DbConfiguration türünde olabilir-ya da derleme ve ad alanı nitelenmiş tür adı dizesi.</span><span class="sxs-lookup"><span data-stu-id="c25f9-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="c25f9-134">Örnek:</span><span class="sxs-lookup"><span data-stu-id="c25f9-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="c25f9-135">DbConfiguration 'ı açıkça ayarlama</span><span class="sxs-lookup"><span data-stu-id="c25f9-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="c25f9-136">Herhangi bir DbContext türü kullanılmadan önce yapılandırmanın gerekli olabileceği bazı durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="c25f9-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="c25f9-137">Buna örnek olarak şunlar verilebilir:</span><span class="sxs-lookup"><span data-stu-id="c25f9-137">Examples of this include:</span></span>  

- <span data-ttu-id="c25f9-138">Bağlam olmadan model oluşturmak için DbModelBuilder kullanma</span><span class="sxs-lookup"><span data-stu-id="c25f9-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="c25f9-139">Uygulama içeriğiniz kullanılmadan önce bu bağlamın kullanıldığı bir DbContext kullanan başka bir Framework/Utility kodu kullanma</span><span class="sxs-lookup"><span data-stu-id="c25f9-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="c25f9-140">Bu tür durumlarda, EF yapılandırmayı otomatik olarak bulamaz ve bunun yerine aşağıdakilerden birini yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="c25f9-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="c25f9-141">Yapılandırma dosyasında, yukarıdaki *DBConfiguration* bölümünde açıklandığı gibi DBConfiguration türünü ayarlayın</span><span class="sxs-lookup"><span data-stu-id="c25f9-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="c25f9-142">Uygulamanın başlatılması sırasında statik DbConfiguration. SetConfiguration metodunu çağırma</span><span class="sxs-lookup"><span data-stu-id="c25f9-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="c25f9-143">DbConfiguration geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="c25f9-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="c25f9-144">DbConfiguration içindeki yapılandırma kümesini geçersiz kılmanız gereken bazı durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="c25f9-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="c25f9-145">Bu genellikle uygulama geliştiricileri tarafından değil, türetilmiş bir DbConfiguration sınıfı kullanmayan üçüncü taraf sağlayıcılar ve eklentiler tarafından değil.</span><span class="sxs-lookup"><span data-stu-id="c25f9-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="c25f9-146">Bu, EntityFramework, bir olay işleyicisinin, var olan yapılandırmayı kilitlemeli bir şekilde değiştirebilecek şekilde kaydedilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="c25f9-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="c25f9-147">Ayrıca, özel olarak EF Service Locator tarafından döndürülen herhangi bir hizmeti değiştirmeye yönelik bir cukr yöntemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="c25f9-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="c25f9-148">Bu, kullanılması amaçlanan bir bu şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="c25f9-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="c25f9-149">Uygulamanın başlangıcında (EF kullanılmadan önce), eklenti veya sağlayıcı bu olay için olay işleyicisi yöntemini kaydetmelidir.</span><span class="sxs-lookup"><span data-stu-id="c25f9-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="c25f9-150">(Uygulamanın EF kullanılmadan önce bunun gerçekleşmesi gerektiğini unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="c25f9-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="c25f9-151">Olay işleyicisi, değiştirilmesini gerektiren her hizmet için ReplaceService 'e bir çağrı yapar.</span><span class="sxs-lookup"><span data-stu-id="c25f9-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="c25f9-152">Örneğin, ıdbconnectionfactory ve DbProviderService 'i değiştirmek için bir işleyiciyi şuna benzer bir şekilde kaydedebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c25f9-152">For example, to replace IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="c25f9-153">Bu kodda, MyProviderServices ve MyConnectionFactory, hizmet uygulamalarınızı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c25f9-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="c25f9-154">Aynı etkiyi sağlamak için ek bağımlılık işleyicileri de ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c25f9-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="c25f9-155">Aynı zamanda DbProviderFactory öğesini bu şekilde kaydırabileceğinizi unutmayın, ancak bunu yapmak EF 'in dışında yalnızca EF 'i ve DbProviderFactory kullanımlarını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="c25f9-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="c25f9-156">Bu nedenle, büyük olasılıkla daha önce yaptığınız gibi DbProviderFactory öğesini kaydırmaya devam etmek isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="c25f9-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="c25f9-157">Ayrıca, paket yöneticisi konsolundan geçişleri çalıştırırken uygulamanızda dışarıdan çalıştırdığınız hizmetleri de göz önünde bulundurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c25f9-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="c25f9-158">Konsolundan geçişi çalıştırdığınızda, DbConfiguration 'nizi bulmayı dener.</span><span class="sxs-lookup"><span data-stu-id="c25f9-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="c25f9-159">Bununla birlikte, Sarmalanan hizmetin, bu hizmet tarafından kaydedildiği yere bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="c25f9-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="c25f9-160">DbConfiguration 'ın yapımını bir parçası olarak kaydedilmişse kodun yürütülmesi ve hizmetin sarmalanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c25f9-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="c25f9-161">Genellikle bu durum böyle olmayacaktır ve bu, araç, Sarmalanan hizmetin sarmalanmayacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="c25f9-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
