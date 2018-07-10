---
title: Kod tabanlı yapılandırma - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
caps.latest.revision: 3
ms.openlocfilehash: da63d36e76b9658a17557707076073be4c1cd95e
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914235"
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
            SetDefaultConnectionFactory(new LocalDBConnectionFactory("mssqllocaldb"));
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

Örneğin, repalce IDbConnectionFactory ve DbProviderService şuna benzeyen bir işleyici kaydetmek:  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

MyProviderServices ve MyConnectionFactory üzerindeki kodda, hizmet uygulamaları temsil eder.  

Ayrıca, aynı etkiyi görmek için ek bağımlılık işleyicileri ekleyebilirsiniz.  

DbProviderFactory bu şekilde sarabilirsiniz, ancak bunun yapılması yalnızca EF etkili olur ve DbProviderFactory EF dışında kullanımları değil unutmayın. Bu nedenle büyük olasılıkla önce sahip olduğunuz DbProviderFactory sarmalamak devam etmek istersiniz.  

Ayrıca, uygulamanıza - örneğin Paket Yöneticisi konsolundan geçişin çalıştırılması harici olarak çalışan hizmetlerin aklınızda tutmanız gerekir. Programını çalıştırdığınızda, DbConfiguration bulmayı dener konsoldan geçirin. Ancak alınıp alınmayacağını Sarmalanan hizmet alırsınız nerede bağlıdır kayıtlı olay işleyicisi. DbConfiguration, oluşumunu bir parçası olarak kayıtlı değilse kod yürütülmesi gerektiğini ve hizmet sarmalanmış. Genellikle bu durum olmaz ve bu araçları Sarmalanan hizmet vermeyeceğiz anlamına gelir.  
