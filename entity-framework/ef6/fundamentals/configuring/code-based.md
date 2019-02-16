---
title: Kod tabanlı yapılandırma - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: c317f112f713612f7b9aef3764a0bd004fef5424
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325359"
---
# <a name="code-based-configuration"></a><span data-ttu-id="8d607-102">Kod tabanlı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8d607-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="8d607-103">**EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="8d607-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="8d607-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="8d607-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="8d607-105">Entity Framework uygulama yapılandırması, bir yapılandırma dosyasında (app.config/web.config) veya kod aracılığıyla belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="8d607-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="8d607-106">İkinci kod tabanlı yapılandırma bilinir.</span><span class="sxs-lookup"><span data-stu-id="8d607-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="8d607-107">Yapılandırma yapılandırma dosyasında açıklanan bir [ayrı makale](config-file.md).</span><span class="sxs-lookup"><span data-stu-id="8d607-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="8d607-108">Yapılandırma dosyası, kod tabanlı yapılandırma üzerinde önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="8d607-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="8d607-109">Bir yapılandırma seçeneği hem kodda ve yapılandırma dosyasında ayarlanırsa, diğer bir deyişle, ardından yapılandırma dosyasında ayarı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8d607-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="8d607-110">DbConfiguration kullanma</span><span class="sxs-lookup"><span data-stu-id="8d607-110">Using DbConfiguration</span></span>  

<span data-ttu-id="8d607-111">Kod tabanlı yapılandırma EF6 ve yukarıdaki System.Data.Entity.Config.DbConfiguration öğesinin oluşturarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="8d607-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="8d607-112">DbConfiguration sınıflara olduğunda aşağıdaki yönergeleri izlenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="8d607-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="8d607-113">Uygulamanız için yalnızca bir DbConfiguration sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8d607-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="8d607-114">Bu sınıf, uygulama etki alanı genelinde ayarları belirtir.</span><span class="sxs-lookup"><span data-stu-id="8d607-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="8d607-115">Aynı bütünleştirilmiş kodun DbContext sınıfınıza olarak DbConfiguration sınıfınıza yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="8d607-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="8d607-116">(Bkz *taşıma DbConfiguration* bunu değiştirmek istiyorsanız bölümünde.)</span><span class="sxs-lookup"><span data-stu-id="8d607-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="8d607-117">Genel parametresiz oluşturucusu DbConfiguration sınıfınıza verin.</span><span class="sxs-lookup"><span data-stu-id="8d607-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="8d607-118">Bu oluşturucu korumalı DbConfiguration yöntemleri çağırarak yapılandırma seçeneklerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8d607-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="8d607-119">Bu yönergeler, bulmak ve yapılandırmanızı, modelinizi erişmesi ve uygulamanız çalıştırıldığında her iki araçları tarafından otomatik olarak kullanmak EF sağlar.</span><span class="sxs-lookup"><span data-stu-id="8d607-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="8d607-120">Örnek</span><span class="sxs-lookup"><span data-stu-id="8d607-120">Example</span></span>  

<span data-ttu-id="8d607-121">DbConfiguration türetilmiş bir sınıf şuna benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="8d607-121">A class derived from DbConfiguration might look like this:</span></span>  

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

<span data-ttu-id="8d607-122">Bu sınıf EF başarısız olan veritabanı işlemleri - otomatik olarak yeniden ve kuralı Code First tarafından oluşturulan veritabanları için yerel DB kullanmak için SQL Azure yürütme stratejisi - kullanılacak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8d607-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="8d607-123">DbConfiguration taşıma</span><span class="sxs-lookup"><span data-stu-id="8d607-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="8d607-124">Burada aynı bütünleştirilmiş kodun DbContext sınıfınıza olarak DbConfiguration sınıfınıza yerleştirmek mümkün olmadığı durumlarda vardır.</span><span class="sxs-lookup"><span data-stu-id="8d607-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="8d607-125">Örneğin, iki DbContext sınıfı her farklı derlemelerde farklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="8d607-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="8d607-126">Bu işleme için iki seçenek vardır.</span><span class="sxs-lookup"><span data-stu-id="8d607-126">There are two options for handling this.</span></span>  

<span data-ttu-id="8d607-127">İlk seçenek, kullanılacak DbConfiguration örneğini belirtmek için yapılandırma dosyası kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="8d607-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="8d607-128">Bunu yapmak için entityFramework bölümün codeConfigurationType özniteliği ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8d607-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="8d607-129">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8d607-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="8d607-130">CodeConfigurationType değerini derlemeyi ve ad alanı DbConfiguration sınıfının tam adı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8d607-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="8d607-131">İkinci seçenek, bağlam sınıfınıza DbConfigurationTypeAttribute yerleştirmektir.</span><span class="sxs-lookup"><span data-stu-id="8d607-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="8d607-132">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8d607-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="8d607-133">Geçirilen değer özniteliğine DbConfiguration türünüz - - yukarıda gösterildiği olabilir veya ad alanı ve derleme türü adı dizesi tam ad.</span><span class="sxs-lookup"><span data-stu-id="8d607-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="8d607-134">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8d607-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="8d607-135">DbConfiguration açık olarak ayarlama</span><span class="sxs-lookup"><span data-stu-id="8d607-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="8d607-136">Herhangi bir DbContext tür kullanılan önce burada yapılandırma gerekli olabilecek bazı durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="8d607-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="8d607-137">Bu örnekleri:</span><span class="sxs-lookup"><span data-stu-id="8d607-137">Examples of this include:</span></span>  

- <span data-ttu-id="8d607-138">Bir bağlam olmadan bir model oluşturmak üzere DbModelBuilder kullanma</span><span class="sxs-lookup"><span data-stu-id="8d607-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="8d607-139">Bu bağlamı uygulama Bağlamınızı kullanılmadan önce kullanıldığı bir DbContext kullanan diğer framework/yardımcı kod kullanma</span><span class="sxs-lookup"><span data-stu-id="8d607-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="8d607-140">Bu gibi durumlarda EF yapılandırmasını otomatik olarak bulmak üzere belirleyemez ve bunun yerine aşağıdakilerden birini yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="8d607-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="8d607-141">DbConfiguration türü yapılandırma dosyasında açıklanan şekilde ayarlamak *taşıma DbConfiguration* yukarıdaki bölümde</span><span class="sxs-lookup"><span data-stu-id="8d607-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="8d607-142">Uygulama başlatma sırasında statik DbConfiguration.SetConfiguration metodunu çağırın</span><span class="sxs-lookup"><span data-stu-id="8d607-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="8d607-143">DbConfiguration geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="8d607-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="8d607-144">Yapılandırması içinde DbConfiguration ayarlanmış geçersiz kılmak için gerek duyduğunuz bazı durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="8d607-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="8d607-145">Bu genellikle uygulama geliştiricilerinin ancak bunun yerine üçüncü taraf sağlayıcılar ve DbConfiguration türetilen kullanamazsınız eklentileri tarafından belirtilmez.</span><span class="sxs-lookup"><span data-stu-id="8d607-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="8d607-146">Bunun için aşağı kilitlenmeden önce geçmesi gereken, mevcut yapılandırmayı değiştirebilirsiniz kaydedilmesi için bir olay işleyicisi EntityFramework sağlar.</span><span class="sxs-lookup"><span data-stu-id="8d607-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="8d607-147">Ayrıca, özellikle EF hizmet bulucudaki tarafından döndürülen herhangi bir hizmeti değiştirerek sugar yöntemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="8d607-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="8d607-148">Bu nasıl kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="8d607-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="8d607-149">Uygulama başlangıcında (EF kullanılmadan önce) eklentisi veya bu olayı için olay işleyicisi yöntemi sağlayıcısını kaydetmeniz.</span><span class="sxs-lookup"><span data-stu-id="8d607-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="8d607-150">(Uygulamanın EF kullanan önce Bunun yapılması gerektiğini unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="8d607-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="8d607-151">Olay işleyicisi ReplaceService değiştirilmesi gereken her hizmet için bir çağrı yapar.</span><span class="sxs-lookup"><span data-stu-id="8d607-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="8d607-152">Örneğin, repalce IDbConnectionFactory ve DbProviderService şuna benzeyen bir işleyici kaydetmek:</span><span class="sxs-lookup"><span data-stu-id="8d607-152">For example, to repalce IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="8d607-153">MyProviderServices ve MyConnectionFactory üzerindeki kodda, hizmet uygulamaları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="8d607-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="8d607-154">Ayrıca, aynı etkiyi görmek için ek bağımlılık işleyicileri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8d607-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="8d607-155">DbProviderFactory bu şekilde sarabilirsiniz, ancak bunu yaparsanız bu nedenle yalnızca EF ve DbProviderFactory EF dışında kullanımları etkiler unutmayın.</span><span class="sxs-lookup"><span data-stu-id="8d607-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="8d607-156">Bu nedenle büyük olasılıkla önce sahip olduğunuz DbProviderFactory sarmalamak devam etmek istersiniz.</span><span class="sxs-lookup"><span data-stu-id="8d607-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="8d607-157">Geçişler Paket Yöneticisi konsolundan çalıştırırken harici olarak uygulamanıza - Örneğin, çalışan hizmetleri de aklınızda tutmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="8d607-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="8d607-158">Programını çalıştırdığınızda, DbConfiguration bulmayı dener konsoldan geçirin.</span><span class="sxs-lookup"><span data-stu-id="8d607-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="8d607-159">Ancak alınıp alınmayacağını Sarmalanan hizmet alırsınız nerede bağlıdır kayıtlı olay işleyicisi.</span><span class="sxs-lookup"><span data-stu-id="8d607-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="8d607-160">DbConfiguration, oluşumunu bir parçası olarak kayıtlı değilse kod yürütülmesi gerektiğini ve hizmet sarmalanmış.</span><span class="sxs-lookup"><span data-stu-id="8d607-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="8d607-161">Genellikle bu durum olmaz ve bu araçları Sarmalanan hizmet vermeyeceğiz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="8d607-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
