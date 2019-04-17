---
title: Kod tabanlı yapılandırma - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 079a4ab30af74eac8b1f51ece5801ff40a867a29
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59619291"
---
# <a name="code-based-configuration"></a>Kod tabanlı yapılandırma
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.  

Entity Framework uygulama yapılandırması, bir yapılandırma dosyasında (app.config/web.config) veya kod aracılığıyla belirtilebilir. İkinci kod tabanlı yapılandırma bilinir.  

Yapılandırma yapılandırma dosyasında açıklanan bir [ayrı makale](config-file.md). Yapılandırma dosyası, kod tabanlı yapılandırma üzerinde önceliklidir. Bir yapılandırma seçeneği hem kodda ve yapılandırma dosyasında ayarlanırsa, diğer bir deyişle, ardından yapılandırma dosyasında ayarı kullanılır.  

## <a name="using-dbconfiguration"></a>DbConfiguration kullanma  

Kod tabanlı yapılandırma EF6 ve yukarıdaki System.Data.Entity.Config.DbConfiguration öğesinin oluşturarak elde edilir. DbConfiguration sınıflara olduğunda aşağıdaki yönergeleri izlenmesi gerekir:  

- Uygulamanız için yalnızca bir DbConfiguration sınıf oluşturun. Bu sınıf, uygulama etki alanı genelinde ayarları belirtir.  
- Aynı bütünleştirilmiş kodun DbContext sınıfınıza olarak DbConfiguration sınıfınıza yerleştirin. (Bkz *taşıma DbConfiguration* bunu değiştirmek istiyorsanız bölümünde.)  
- Genel parametresiz oluşturucusu DbConfiguration sınıfınıza verin.  
- Bu oluşturucu korumalı DbConfiguration yöntemleri çağırarak yapılandırma seçeneklerini ayarlayın.  

Bu yönergeler, bulmak ve yapılandırmanızı, modelinizi erişmesi ve uygulamanız çalıştırıldığında her iki araçları tarafından otomatik olarak kullanmak EF sağlar.  

## <a name="example"></a>Örnek  

DbConfiguration türetilmiş bir sınıf şuna benzeyebilir:  

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

Bu sınıf EF başarısız olan veritabanı işlemleri - otomatik olarak yeniden ve kuralı Code First tarafından oluşturulan veritabanları için yerel DB kullanmak için SQL Azure yürütme stratejisi - kullanılacak ayarlar.  

## <a name="moving-dbconfiguration"></a>DbConfiguration taşıma  

Burada aynı bütünleştirilmiş kodun DbContext sınıfınıza olarak DbConfiguration sınıfınıza yerleştirmek mümkün olmadığı durumlarda vardır. Örneğin, iki DbContext sınıfı her farklı derlemelerde farklı olabilir. Bu işleme için iki seçenek vardır.  

İlk seçenek, kullanılacak DbConfiguration örneğini belirtmek için yapılandırma dosyası kullanmaktır. Bunu yapmak için entityFramework bölümün codeConfigurationType özniteliği ayarlayın. Örneğin:  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

CodeConfigurationType değerini derlemeyi ve ad alanı DbConfiguration sınıfının tam adı olması gerekir.  

İkinci seçenek, bağlam sınıfınıza DbConfigurationTypeAttribute yerleştirmektir. Örneğin:  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

Geçirilen değer özniteliğine DbConfiguration türünüz - - yukarıda gösterildiği olabilir veya ad alanı ve derleme türü adı dizesi tam ad. Örneğin:  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>DbConfiguration açık olarak ayarlama  

Herhangi bir DbContext tür kullanılan önce burada yapılandırma gerekli olabilecek bazı durumlar vardır. Bu örnekleri:  

- Bir bağlam olmadan bir model oluşturmak üzere DbModelBuilder kullanma  
- Bu bağlamı uygulama Bağlamınızı kullanılmadan önce kullanıldığı bir DbContext kullanan diğer framework/yardımcı kod kullanma  

Bu gibi durumlarda EF yapılandırmasını otomatik olarak bulmak üzere belirleyemez ve bunun yerine aşağıdakilerden birini yapmalısınız:  

- DbConfiguration türü yapılandırma dosyasında açıklanan şekilde ayarlamak *taşıma DbConfiguration* yukarıdaki bölümde
- Uygulama başlatma sırasında statik DbConfiguration.SetConfiguration metodunu çağırın  

## <a name="overriding-dbconfiguration"></a>DbConfiguration geçersiz kılma  

Yapılandırması içinde DbConfiguration ayarlanmış geçersiz kılmak için gerek duyduğunuz bazı durumlar vardır. Bu genellikle uygulama geliştiricilerinin ancak bunun yerine üçüncü taraf sağlayıcılar ve DbConfiguration türetilen kullanamazsınız eklentileri tarafından belirtilmez.  

Bunun için aşağı kilitlenmeden önce geçmesi gereken, mevcut yapılandırmayı değiştirebilirsiniz kaydedilmesi için bir olay işleyicisi EntityFramework sağlar.  Ayrıca, özellikle EF hizmet bulucudaki tarafından döndürülen herhangi bir hizmeti değiştirerek sugar yöntemi sağlar. Bu nasıl kullanılmak üzere tasarlanmıştır.  

- Uygulama başlangıcında (EF kullanılmadan önce) eklentisi veya bu olayı için olay işleyicisi yöntemi sağlayıcısını kaydetmeniz. (Uygulamanın EF kullanan önce Bunun yapılması gerektiğini unutmayın.)  
- Olay işleyicisi ReplaceService değiştirilmesi gereken her hizmet için bir çağrı yapar.  

Örneğin, IDbConnectionFactory ve DbProviderService değiştirmek şuna benzeyen bir işleyici kaydedin:  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

MyProviderServices ve MyConnectionFactory üzerindeki kodda, hizmet uygulamaları temsil eder.  

Ayrıca, aynı etkiyi görmek için ek bağımlılık işleyicileri ekleyebilirsiniz.  

DbProviderFactory bu şekilde sarabilirsiniz, ancak bunu yaparsanız bu nedenle yalnızca EF ve DbProviderFactory EF dışında kullanımları etkiler unutmayın. Bu nedenle büyük olasılıkla önce sahip olduğunuz DbProviderFactory sarmalamak devam etmek istersiniz.  

Geçişler Paket Yöneticisi konsolundan çalıştırırken harici olarak uygulamanıza - Örneğin, çalışan hizmetleri de aklınızda tutmanız gerekir. Programını çalıştırdığınızda, DbConfiguration bulmayı dener konsoldan geçirin. Ancak alınıp alınmayacağını Sarmalanan hizmet alırsınız nerede bağlıdır kayıtlı olay işleyicisi. DbConfiguration, oluşumunu bir parçası olarak kayıtlı değilse kod yürütülmesi gerektiğini ve hizmet sarmalanmış. Genellikle bu durum olmaz ve bu araçları Sarmalanan hizmet vermeyeceğiz anlamına gelir.  
