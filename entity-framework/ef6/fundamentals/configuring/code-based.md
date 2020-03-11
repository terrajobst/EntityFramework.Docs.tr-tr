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
# <a name="code-based-configuration"></a>Kod tabanlı yapılandırma
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.  

Bir Entity Framework uygulama yapılandırması, bir yapılandırma dosyasında (App. config/Web. config) veya kod aracılığıyla belirtilebilir. İkincisi, kod tabanlı yapılandırma olarak bilinir.  

Bir yapılandırma dosyasındaki yapılandırma [ayrı bir makalede](config-file.md)açıklanmıştır. Yapılandırma dosyası kod tabanlı yapılandırmayı temel alır. Diğer bir deyişle, bir yapılandırma seçeneği hem kodda hem de yapılandırma dosyasında ayarlandıysa, yapılandırma dosyasındaki ayar kullanılır.  

## <a name="using-dbconfiguration"></a>DbConfiguration kullanma  

EF6 ve üzeri sürümlerde kod tabanlı yapılandırma System. Data. Entity. config. DbConfiguration sınıfının bir alt sınıfı oluşturularak elde edilir. Altsınıflama DbConfiguration olduğunda aşağıdaki yönergelerin izlenmesi gerekir:  

- Uygulamanız için yalnızca bir DbConfiguration sınıfı oluşturun. Bu sınıf, uygulama etki alanı genelinde ayarları belirler.  
- DbConfiguration sınıfınızı DbContext sınıfınız ile aynı derlemeye yerleştirin. (Bunu değiştirmek istiyorsanız, bkz. *DbConfiguration bölümünü taşıma* .)  
- DbConfiguration sınıfınıza Ortak parametresiz Oluşturucu verin.  
- Bu oluşturucunun içinden korumalı DbConfiguration yöntemlerini çağırarak yapılandırma seçeneklerini ayarlayın.  

Bu kurallara uyulması, EF 'in, modelinize erişmesi gereken her iki araç tarafından otomatik olarak yapılandırmanızı bulmasını ve kullanmasını, uygulamanızın ne zaman çalıştırılacağını de sağlar.  

## <a name="example"></a>Örnek  

DbConfiguration 'dan türetilen bir sınıf şöyle görünebilir:  

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

Bu sınıf, başarısız veritabanı işlemlerini otomatik olarak yeniden denemek ve Code First kural tarafından oluşturulan veritabanları için yerel veritabanını kullanmak üzere SQL Azure yürütme stratejisini kullanmak için EF 'i ayarlar.  

## <a name="moving-dbconfiguration"></a>DbConfiguration taşınıyor  

DbConfiguration sınıfınızın DbContext sınıfınız ile aynı derlemeye yerleştirmesinin mümkün olmadığı durumlar vardır. Örneğin, her biri farklı derlemede iki DbContext sınıfınız olabilir. Bunu işlemek için iki seçenek vardır.  

İlk seçenek, kullanılacak DbConfiguration örneğini belirtmek için yapılandırma dosyasını kullanmaktır. Bunu yapmak için entityFramework bölümünün codeConfigurationType özniteliğini ayarlayın. Örnek:  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

CodeConfigurationType değeri, DbConfiguration sınıfınızın derleme ve ad alanı nitelikli adı olmalıdır.  

İkinci seçenek, DbConfigurationTypeAttribute öğesini bağlam sınıfınıza yerleştirkdır. Örnek:  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

Özniteliğe geçirilen değer, yukarıda gösterilen DbConfiguration türünde olabilir-ya da derleme ve ad alanı nitelenmiş tür adı dizesi. Örnek:  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>DbConfiguration 'ı açıkça ayarlama  

Herhangi bir DbContext türü kullanılmadan önce yapılandırmanın gerekli olabileceği bazı durumlar vardır. Buna örnek olarak şunlar verilebilir:  

- Bağlam olmadan model oluşturmak için DbModelBuilder kullanma  
- Uygulama içeriğiniz kullanılmadan önce bu bağlamın kullanıldığı bir DbContext kullanan başka bir Framework/Utility kodu kullanma  

Bu tür durumlarda, EF yapılandırmayı otomatik olarak bulamaz ve bunun yerine aşağıdakilerden birini yapmanız gerekir:  

- Yapılandırma dosyasında, yukarıdaki *DBConfiguration* bölümünde açıklandığı gibi DBConfiguration türünü ayarlayın
- Uygulamanın başlatılması sırasında statik DbConfiguration. SetConfiguration metodunu çağırma  

## <a name="overriding-dbconfiguration"></a>DbConfiguration geçersiz kılma  

DbConfiguration içindeki yapılandırma kümesini geçersiz kılmanız gereken bazı durumlar vardır. Bu genellikle uygulama geliştiricileri tarafından değil, türetilmiş bir DbConfiguration sınıfı kullanmayan üçüncü taraf sağlayıcılar ve eklentiler tarafından değil.  

Bu, EntityFramework, bir olay işleyicisinin, var olan yapılandırmayı kilitlemeli bir şekilde değiştirebilecek şekilde kaydedilmesini sağlar.  Ayrıca, özel olarak EF Service Locator tarafından döndürülen herhangi bir hizmeti değiştirmeye yönelik bir cukr yöntemi sağlar. Bu, kullanılması amaçlanan bir bu şekilde belirlenir:  

- Uygulamanın başlangıcında (EF kullanılmadan önce), eklenti veya sağlayıcı bu olay için olay işleyicisi yöntemini kaydetmelidir. (Uygulamanın EF kullanılmadan önce bunun gerçekleşmesi gerektiğini unutmayın.)  
- Olay işleyicisi, değiştirilmesini gerektiren her hizmet için ReplaceService 'e bir çağrı yapar.  

Örneğin, ıdbconnectionfactory ve DbProviderService 'i değiştirmek için bir işleyiciyi şuna benzer bir şekilde kaydedebilirsiniz:  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

Bu kodda, MyProviderServices ve MyConnectionFactory, hizmet uygulamalarınızı temsil eder.  

Aynı etkiyi sağlamak için ek bağımlılık işleyicileri de ekleyebilirsiniz.  

Aynı zamanda DbProviderFactory öğesini bu şekilde kaydırabileceğinizi unutmayın, ancak bunu yapmak EF 'in dışında yalnızca EF 'i ve DbProviderFactory kullanımlarını etkilemez. Bu nedenle, büyük olasılıkla daha önce yaptığınız gibi DbProviderFactory öğesini kaydırmaya devam etmek isteyeceksiniz.  

Ayrıca, paket yöneticisi konsolundan geçişleri çalıştırırken uygulamanızda dışarıdan çalıştırdığınız hizmetleri de göz önünde bulundurmanız gerekir. Konsolundan geçişi çalıştırdığınızda, DbConfiguration 'nizi bulmayı dener. Bununla birlikte, Sarmalanan hizmetin, bu hizmet tarafından kaydedildiği yere bağlı olup olmadığı. DbConfiguration 'ın yapımını bir parçası olarak kaydedilmişse kodun yürütülmesi ve hizmetin sarmalanması gerekir. Genellikle bu durum böyle olmayacaktır ve bu, araç, Sarmalanan hizmetin sarmalanmayacağı anlamına gelir.  
