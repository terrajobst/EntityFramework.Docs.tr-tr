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
# <a name="entity-framework-6-providers"></a>Varlık Çerçevesi 6 Sağlayıcıları
> [!NOTE]
> **EF6 Yalnızca -** Bu sayfada tartışılan özellikler, API'ler vb. Entity Framework 6'da tanıtıldı. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü geçerli değildir.

Varlık Çerçevesi şu anda açık kaynak lisansı altında geliştirilmektedir ve EF6 ve üzeri .NET Çerçevesi'nin bir parçası olarak sevk edilmeyecektir. Bunun birçok avantajı vardır, ancak EF sağlayıcılarının EF6 montajlarına karşı yeniden inşa edilmesi de gerekir. Bu, EF5 ve altındaki EF sağlayıcılarının yeniden oluşturulana kadar EF6 ile çalışmayacağı anlamına gelir.

## <a name="which-providers-are-available-for-ef6"></a>EF6 için hangi sağlayıcılar kullanılabilir?

EF6 için yeniden inşa edildiğini bildiğimiz sağlayıcılar şunlardır:

*   **Microsoft SQL Server sağlayıcısı**
    *   [Varlık Çerçevesi açık kaynak kod tabanından oluşturulmuştur](https://github.com/aspnet/EntityFramework6)
    *   [EntityFramework NuGet paketinin](https://nuget.org/packages/EntityFramework) bir parçası olarak sevk edildi
*   **Microsoft SQL Server Compact Edition sağlayıcısı**
    *   [Varlık Çerçevesi açık kaynak kod tabanından oluşturulmuştur](https://github.com/aspnet/EntityFramework6)
    *   [EntityFramework.SqlServerCompact NuGet paketinde](https://nuget.org/packages/EntityFramework.SqlServerCompact) gönderildi
*   [**Devart dotConnect Veri Sağlayıcıları**](https://www.devart.com/dotconnect/)
    *   Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 ve SQL Server dahil olmak üzere çeşitli veritabanları için [Devart'ın](https://www.devart.com/) üçüncü taraf sağlayıcıları vardır
*   [**CData Yazılım sağlayıcıları**](https://www.cdata.com/ado/)
    *   Salesforce, Azure Table Storage, MySql ve daha birçok veri deposu için [CData Software'in](https://www.cdata.com/ado/) üçüncü taraf sağlayıcıları vardır
*   **Firebird sağlayıcısı**
    *   [NuGet Paketi](https://www.nuget.org/packages/EntityFramework.Firebird/) olarak kullanılabilir
*   **Görsel Fox Pro sağlayıcısı**
    *   [NuGet paketi](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/) olarak kullanılabilir
*   **MySQL**
    *   [Entity Framework için MySQL Bağlayıcısı/NET](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   **PostgreSQL**
    *   Npgsql [NuGet paketi](https://www.nuget.org/packages/EntityFramework6.Npgsql/) olarak kullanılabilir
*   **Oracle**
    *   ODP.NET [NuGet paketi](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/) olarak kullanılabilir

Bu listeye eklenmesinin belirli bir sağlayıcı için işlevsellik düzeyini veya destek düzeyini göstermediğini, yalnızca EF6 için bir yapının kullanıma sunulduğunu unutmayın.

## <a name="registering-ef-providers"></a>EF sağlayıcılarının kaydedilmesi

Entity Framework 6 EF sağlayıcıları ndan başlayarak kod tabanlı yapılandırma kullanılarak veya uygulamanın config dosyasına kaydedilebilir.

### <a name="config-file-registration"></a>Config dosya kaydı

EF sağlayıcısının app.config veya web.config'e kaydolması aşağıdaki biçimdedir:


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

EF sağlayıcısı NuGet'den yüklüyse, NuGet paketinin bu kaydı otomatik olarak config dosyasına ekleyeceğini unutmayın. NuGet paketini uygulamanızın başlangıç projesi olmayan bir projeye yüklerseniz, başlangıç projeniz için kaydı config dosyasına kopyalamanız gerekebilir.

Bu kayıttaki "değişmez Ad", ADO.NET sağlayıcısını tanımlamak için kullanılan değişmez adla aynıdır. Bu, Bir DbProviderFactorys kaydında "değişmez" özniteliği ve bağlantı dize kaydındaki "providerName" özniteliği olarak bulunabilir. Kullanılacak değişmez ad da sağlayıcı için belgelere eklenmelidir. Değişmez adlara örnek olarak SQL Server için "System.Data.SqlClientClient" ve SQL Server Compact için "System.Data.SqlServerCe.4.0" verilebilir.

Bu kayıttaki "tür" "System.Data.Entity.Core.Common.DbProviderServices" adresinden türeyen sağlayıcı türünün montaja uygun adıdır. Örneğin, SQL Compact için kullanılacak dize "System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact" dır. Burada kullanılacak tür sağlayıcı için belgelere eklenmelidir.

### <a name="code-based-registration"></a>Kod tabanlı kayıt

VARLıK Framework 6 ile başlayan EF için uygulama çapında yapılandırma kod olarak belirtilebilir. Tüm ayrıntılar için Entity _[Framework Code Tabanlı Yapılandırma'ya](https://msdn.microsoft.com/data/jj680699)_ bakın. Kod tabanlı yapılandırmakullanarak bir EF sağlayıcısını kaydetmenin normal yolu, System.Data.Entity.DbConfiguration'dan türetilen yeni bir sınıf oluşturmak ve onu DbContext sınıfınızla aynı derlemeye yerleştirmektir. DbConfiguration sınıfınız sağlayıcıyı oluşturucusuna kaydettirmelidir. Örneğin, SQL Compact sağlayıcısını kaydetmek için DbConfiguration sınıfı aşağıdaki gibi görünür:

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

Bu kodda "SqlCeProviderServices.ProviderInvariantName" SQL Server Compact sağlayıcı değişmez ad dizesi ("System.Data.SqlServerCe.4.0") ve SqlCeProviderServices.Instance sql Compact EF sağlayıcısının singleton örneğini döndürür.

## <a name="what-if-the-provider-i-need-isnt-available"></a>İhtiyacım olan sağlayıcı kullanılamıyorsa ne olur?

Sağlayıcı EF'nin önceki sürümleri için kullanılabilirse, sağlayıcının sahibiyle iletişime geçmenizi ve onlardan bir EF6 sürümü oluşturmalarını istemenizi öneririz. [EF6 sağlayıcı modeli](~/ef6/fundamentals/providers/provider-model.md)için belgelere bir başvuru eklemeniz gerekir.

## <a name="can-i-write-a-provider-myself"></a>Ben de bir sağlayıcı yazabilir miyim?

Önemsiz bir girişim olarak kabul edilmemelidir rağmen kesinlikle bir EF sağlayıcı kendiniz oluşturmak mümkündür. EF6 sağlayıcı modeli hakkında yukarıdaki bağlantı başlamak için iyi bir yerdir. Ayrıca, [EF açık kaynak kod tabanında](https://github.com/aspnet/EntityFramework6) yer alan SQL Server ve SQL CE sağlayıcısının kodunu başlangıç noktası olarak veya başvuru için kullanmayı da yararlı bulabilirsiniz.

EF6'dan başlayarak EF sağlayıcısının altta yatan ADO.NET sağlayıcısıyla daha az sıkı bir şekilde birleştiğinde olduğunu unutmayın. Bu, ADO.NET sınıflarını yazmaya veya sarmaya gerek kalmadan bir EF sağlayıcısı yazmayı kolaylaştırır.
