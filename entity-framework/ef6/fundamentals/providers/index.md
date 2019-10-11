---
title: Entity Framework sağlayıcıları-EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
ms.openlocfilehash: bf07296503e4bb5d1e13f5f6f29e7118cbbde61d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181684"
---
# <a name="entity-framework-6-providers"></a>Entity Framework 6 sağlayıcı
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.

Entity Framework artık açık kaynaklı bir lisans altında geliştirilmektedir ve EF6 bir .NET Framework parçası olarak, yukarıdaki ve üzeri olarak gönderilmeyecektir. Bunun birçok avantajı vardır, ancak EF6 derlemelerinde EF sağlayıcılarının yeniden oluşturulmasını gerektirir. Bu, EF5 ve aşağıdaki için EF sağlayıcılarının yeniden oluşturulana kadar EF6 ile çalışmadıkları anlamına gelir.

## <a name="which-providers-are-available-for-ef6"></a>EF6 için hangi sağlayıcılar kullanılabilir?

EF6 şunlar için yeniden derlendiğimiz sağlayıcılar:

*   **Microsoft SQL Server sağlayıcı**
    *   [Entity Framework açık kaynak kodu tabanı](https://github.com/aspnet/EntityFramework6) tarafından oluşturuldu
    *   [EntityFramework NuGet paketinin](https://nuget.org/packages/EntityFramework) parçası olarak sevk edildi
*   **Microsoft SQL Server Compact sürümü sağlayıcısı**
    *   [Entity Framework açık kaynak kodu tabanı](https://github.com/aspnet/EntityFramework6) tarafından oluşturuldu
    *   [EntityFramework. SqlServerCompact NuGet paketinde](https://nuget.org/packages/EntityFramework.SqlServerCompact) sevk edildi
*   [**Devart dotConnect veri sağlayıcıları**](https://www.devart.com/dotconnect/)
    *   Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 ve SQL Server gibi çeşitli veritabanları için [Devart](https://www.devart.com/) 'ten üçüncü taraf sağlayıcılar vardır
*   [**CData yazılım sağlayıcıları**](https://www.cdata.com/ado/)
    *   Salesforce, Azure Table Storage, MySql ve çok daha fazlasını içeren çeşitli veri depoları için [CDATA yazılımından](https://www.cdata.com/ado/) üçüncü taraf sağlayıcılar vardır
*   **Firebird sağlayıcısı**
    *   [NuGet paketi](https://www.nuget.org/packages/EntityFramework.Firebird/) olarak kullanılabilir
*   **Visual Fox Pro sağlayıcısı**
    *   [NuGet paketi](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/) olarak kullanılabilir
*   **MySQL**
    *   [Entity Framework için MySQL Bağlayıcısı/ağı](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   **PostgreSQL**
    *   Npgsql bir [NuGet paketi](https://www.nuget.org/packages/EntityFramework6.Npgsql/) olarak kullanılabilir
*   **Oracle**
    *   ODP.NET, bir [NuGet paketi](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/) olarak kullanılabilir

Bu listede yer alan, belirli bir sağlayıcı için işlev düzeyini veya desteğin olduğunu göstermez, yalnızca bir EF6 derlemesi kullanılabilir hale getirilir.

## <a name="registering-ef-providers"></a>EF sağlayıcılarını kaydetme

Entity Framework 6 EF sağlayıcılarından itibaren, kod tabanlı yapılandırma veya uygulamanın yapılandırma dosyası kullanılarak kayıt yapılabilir.

### <a name="config-file-registration"></a>Yapılandırma dosyası kaydı

App. config veya Web. config dosyasındaki EF sağlayıcısı 'nın kaydı şu biçimdedir:


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

Bu durum genellikle, NuGet 'den EF sağlayıcısı yüklüyse, NuGet paketinin bu kaydı yapılandırma dosyasına otomatik olarak eklemesini unutmayın. NuGet paketini uygulamanız için başlangıç projesi olmayan bir projeye yüklerseniz, kaydı başlangıç projeniz için yapılandırma dosyasına kopyalamanız gerekebilir.

Bu kayıtta "invariantName", bir ADO.NET sağlayıcısı tanımlamak için kullanılan sabit addır. Bu, bir DbProviderFactory kaydında "sabit" özniteliği olarak ve bir bağlantı dizesi kaydında "providerName" özniteliği olarak bulunabilir. Kullanılacak sabit ad, sağlayıcıya yönelik belgelere de eklenmelidir. Sabit adlara örnek olarak, SQL Server için "System. Data. SqlClient" ve SQL Server Compact için "System. Data. SqlServerCe. 4.0" verilebilir.

Bu kayıtta "tür", "System. Data. Entity. Core. Common. DbProviderServices" öğesinden türetilen sağlayıcı türünün derleme nitelikli adıdır. Örneğin, SQL Compact için kullanılacak dize "System. Data. Entity. SqlServerCompact. SqlCeProviderServices, EntityFramework. SqlServerCompact" dir. Burada kullanılacak tür, sağlayıcının belgelerine eklenmelidir.

### <a name="code-based-registration"></a>Kod tabanlı kayıt

Entity Framework 6 ' dan itibaren EF için uygulama genelinde yapılandırma, kodda belirtilebilir. Tam Ayrıntılar için bkz. _[kod tabanlı yapılandırma Entity Framework](https://msdn.microsoft.com/data/jj680699)_ . Kod tabanlı yapılandırma kullanarak EF sağlayıcısını kaydetmek için normal yol, System. Data. Entity. DbConfiguration ' tan türetilen yeni bir sınıf oluşturmak ve bunu DbContext sınıfınız ile aynı derlemeye yerleştirkullanmaktır. DbConfiguration sınıfınız daha sonra sağlayıcıyı yapıcısına kaydetmelidir. Örneğin, SQL Compact sağlayıcısını kaydetmek için DbConfiguration sınıfı şuna benzer:

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

Bu kodda, "SqlCeProviderServices. ProviderInvariantName", SQL Server Compact sağlayıcı sabit adı dizesi ("System. Data. SqlServerCe. 4.0") ve SqlCeProviderServices. Instance için SQL Compact 'in Singleton örneğini döndürür EF sağlayıcısı.

## <a name="what-if-the-provider-i-need-isnt-available"></a>Yapmam gereken sağlayıcı yoksa ne olacak?

Sağlayıcı, EF 'in önceki sürümleri için kullanılabilirse, sağlayıcının sahibine başvurmanız ve bir EF6 sürümü oluşturmasını isteyeceğiz. [EF6 sağlayıcı modelinin belgelerine](~/ef6/fundamentals/providers/provider-model.md)bir başvuru eklemeniz gerekir.

## <a name="can-i-write-a-provider-myself"></a>Bir sağlayıcıyı kendim yazabilir miyim?

Tamamen bir EF sağlayıcısı oluşturmak, ancak önemsiz bir şekilde düşünülmemelidir. EF6 sağlayıcı modeliyle ilgili yukarıdaki bağlantının başlaması iyi bir yerdir. Ayrıca, bir başlangıç noktası veya başvuru olarak [EF açık kaynak kod](https://github.com/aspnet/EntityFramework6) tabanına dahil SQL Server ve SQL CE sağlayıcısı için kodu kullanmayı yararlı bulabilirsiniz.

EF6 ile başlayan EF sağlayıcısı, temel alınan ADO.NET sağlayıcısına daha sıkı bir şekilde bağlanmış olduğunu unutmayın. Bu, ADO.NET sınıfları yazmaya veya kaydırmaya gerek kalmadan bir EF sağlayıcısı yazmayı kolaylaştırır.
