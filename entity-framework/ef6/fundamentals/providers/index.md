---
title: Entity Framework sağlayıcıları - EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
ms.openlocfilehash: f6e34d1273bd1004ce9d1610ce3613068088eb5e
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668745"
---
# <a name="entity-framework-6-providers"></a>Entity Framework 6 sağlayıcıları
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.

Entity Framework, açık kaynaklı lisans ve EF6 altında artık geliştirilmektedir ve yukarıda .NET Framework'ün bir parçası olarak gönderilmeyecek. Bu, birçok avantaj sunar ancak ayrıca EF sağlayıcıları karşı EF6 derlemelerin yeniden oluşturulması gerekir. Başka bir deyişle, bunlar yeniden kadar EF sağlayıcıları EF5 ve aşağıda EF6 ile çalışmaz.

## <a name="which-providers-are-available-for-ef6"></a>Hangi sağlayıcıları EF6 için kullanılabilir mi?

Farkında duyuyoruz sağlayıcıları, yeniden EF6 içerir:

*   **Microsoft SQL Server sağlayıcısı**
    *   Oluşturulmuş [Entity Framework açık kaynak kod tabanı](http://github.com/aspnet/EntityFramework6)
    *   Parçası olarak gönderildiğini [EntityFramework NuGet paketi](http://nuget.org/packages/EntityFramework)
*   **Microsoft SQL Server Compact Edition sağlayıcısı**
    *   Oluşturulmuş [Entity Framework açık kaynak kod tabanı](http://github.com/aspnet/EntityFramework6)
    *   İçinde sevk [EntityFramework.SqlServerCompact NuGet paketi](http://nuget.org/packages/EntityFramework.SqlServerCompact)
*   [**Devart dotConnect veri sağlayıcıları**](http://www.devart.com/dotconnect/)
    *   Üçüncü taraf sağlayıcılarından vardır [Devart](http://www.devart.com/) Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 ve SQL Server dahil olmak üzere çeşitli
*   [**CData yazılım sağlayıcıları**](http://www.cdata.com/ado/)
    *   Üçüncü taraf sağlayıcılarından vardır [CData yazılım](http://www.cdata.com/ado/) veri depoları, Salesforce, Azure Table Storage, MySql ve çok daha fazlası dahil olmak üzere çeşitli
*   **Firebird sağlayıcısı**
    *   Olarak kullanılabilir bir [NuGet paketi](https://www.nuget.org/packages/EntityFramework.Firebird/)
*   **Görsel Fox Pro sağlayıcısı**
    *   Olarak kullanılabilir bir [NuGet paketi](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)
*   **MySQL**
    *   [MySQL Connector/NET Entity Framework için](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   **PostgreSQL**
    *   Npgsql olarak kullanılabilir bir [NuGet paketi](https://www.nuget.org/packages/EntityFramework6.Npgsql/)
*   **Oracle**
    *   ODP.NET olarak kullanılabilir bir [NuGet paketi](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)

Bu listesine eklenmek üzere işlevsellik veya yalnızca, bir derleme EF6 için kullanılabilir duruma getirilen belirli bir sağlayıcı için destek düzeyini göstermez unutmayın.

## <a name="registering-ef-providers"></a>EF sağlayıcılar kaydediliyor

Entity Framework 6 EF sağlayıcılardan başlayarak kaydedilebilir ya da kod tabanlı yapılandırma kullanarak veya uygulamanın yapılandırma dosyasında.

### <a name="config-file-registration"></a>Yapılandırma dosyası kayıt

App.config veya web.config EF sağlayıcı kaydı, aşağıdaki biçime sahiptir:


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

Nuget'ten EF sağlayıcısı yüklüyse, NuGet paketi yapılandırma dosyasına otomatik olarak bu kayıt ekleyeceksiniz sonra genellikle unutmayın. Uygulamanız için başlangıç projesi olmayan bir proje içinde NuGet paketi yüklerseniz, başlangıç projeniz için yapılandırma dosyası kayıt kopyalamamız gerekebilir.

Bu kayıt "Invariantname", bir ADO.NET sağlayıcısı tanımlamak için kullanılan aynı sabit adıdır. Bu, "sabit" özniteliği DbProviderFactories kayıt ve bağlantı dizesini kayıt "providerName" özniteliği olarak bulunabilir. Değişmez adını kullanmak için Ayrıca sağlayıcının belgelerine eklenmelidir. SQL Server için "System.Data.SqlClient" ve "System.Data.SqlServerCe.4.0" SQL Server Compact için sabit adları örnekleridir.

Bu kayıt "type" bütünleştirilmiş kodla nitelenen "System.Data.Entity.Core.Common.DbProviderServices" türetilen sağlayıcı türü adıdır. Örneğin, SQL Compact için kullanılacak dize "System.Data.Entity.SqlServerCompact.SqlCeProviderServices EntityFramework.SqlServerCompact" olur. Burada kullanmak üzere türü sağlayıcının belgelerine eklenmelidir.

### <a name="code-based-registration"></a>Kod tabanlı kayıt

EF için birçok farklı uygulama yapılandırma Entity Framework 6 ile başlayarak, kod içinde belirtilebilir. Tam Ayrıntılar için bkz.  _[Entity Framework Code-Based yapılandırma](https://msdn.microsoft.com/data/jj680699)_. Kod tabanlı yapılandırma kullanarak bir EF sağlayıcısını kaydetmek için normal System.Data.Entity.DbConfiguration türeyen yeni bir sınıf oluşturun ve aynı bütünleştirilmiş kodun DbContext sınıfınıza olarak yerleştirme yoludur. DbConfiguration sınıfınıza ardından oluşturucusunda sağlayıcısını kaydetmeniz. Örneğin, SQL Compact kaydettirmek için sağlayıcı DbConfiguration sınıfı şu şekilde görünür:

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

Bu kodu içinde "SqlCeProviderServices.ProviderInvariantName" SQL Server Compact sağlayıcı değişmez adı dize ("System.Data.SqlServerCe.4.0") için bir kolaylık ve SqlCeProviderServices.Instance SQL Compact tekil örneğini döndürür. EF sağlayıcısı.

## <a name="what-if-the-provider-i-need-isnt-available"></a>İhtiyacım olursa ne sağlayıcı yapabilirim kullanılamıyor?

Sağlayıcı EF'ın önceki sürümleri için kullanılabilir haldeyse, ardından sağlayıcı sahibine başvurun ve EF6 sürüm oluşturma isteyin geçirmenizi öneririz. Bir başvuru içermelidir [EF6 sağlayıcı modeli için belgeleri](~/ef6/fundamentals/providers/provider-model.md).

## <a name="can-i-write-a-provider-myself"></a>Ben bir sağlayıcı kendim yazmak?

Bu, kesinlikle önemsiz erişilmediğinden değerlendirilmemelidir rağmen bir EF sağlayıcısı kendiniz oluşturmanız mümkündür. EF6 sağlayıcı modeli hakkında bilgi için yukarıdaki bağlantıya başlatmak için iyi bir yerdir. Ayrıca SQL Server ve SQL CE sağlayıcısı dahil kodu kullanmak faydalı [EF açık kaynak kod tabanı](https://github.com/aspnet/EntityFramework6) bir başlangıç noktası olarak veya başvuru.

EF sağlayıcısı EF6'ile başlayan daha az sıkı bir şekilde temel alınan ADO.NET sağlayıcısı için katı olarak birleştirilmiş olduğunu unutmayın. Bu, yazma ya da ADO.NET sınıfları sarmalamak gerek kalmadan bir EF sağlayıcı yazma kolaylaştırır.
