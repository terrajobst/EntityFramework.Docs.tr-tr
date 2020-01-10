---
title: EF Core 3,0 ' deki Son değişiklikler EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: cac166e9e194e512de7d730d27c061e6deaf5191
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502233"
---
# <a name="breaking-changes-included-in-ef-core-30"></a>EF Core 3,0 ' de yer alan son değişiklikler

Aşağıdaki API ve davranış değişiklikleri, 3.0.0 sürümüne yükseltirken mevcut uygulamaları bozmak için olası bir davranıştır.
Veritabanı sağlayıcılarını yalnızca etkilemek için beklediğimiz değişiklikler, [sağlayıcı değişiklikleri](xref:core/providers/provider-log)altında belgelenmiştir.

## <a name="summary"></a>Özet

| **Yeni değişiklik**                                                                                               | **Etkisi** |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [LINQ sorguları artık istemcide değerlendirilmedi](#linq-queries-are-no-longer-evaluated-on-the-client)         | Yüksek       |
| [EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1](#netstandard21) | Yüksek      |
| [EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil](#dotnet-ef) | Yüksek      |
| [DetectChanges, Store tarafından oluşturulan anahtar değerlerini](#dc) | Yüksek      |
| [FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı](#fromsql) | Yüksek      |
| [Sorgu türleri varlık türleriyle birleştirilir](#qt) | Yüksek      |
| [Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil](#no-longer) | Orta      |
| [Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir](#cascade) | Orta      |
| [İlgili varlıkların yüklenmesi artık tek bir sorguda gerçekleşir](#eager-loading-single-query) | Orta      |
| [DeleteBehavior. restrict Temizleme semantiğine sahip](#deletebehavior) | Orta      |
| [Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti](#config) | Orta      |
| [Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır](#each) | Orta      |
| [Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz](#notrackingresolution) | Orta      |
| [Meta veri API 'SI değişiklikleri](#metadata-api-changes) | Orta      |
| [Sağlayıcıya özel meta veri API 'SI değişiklikleri](#provider) | Orta      |
| [UseRowNumberForPaging kaldırıldı](#urn) | Orta      |
| [Saklı yordamla birlikte kullanıldığında FromSql metodu birleştirilemez](#fromsqlsproc) | Orta      |
| [FromSql metotları yalnızca sorgu köklerine göre belirtilebilir](#fromsql) | Düşük      |
| [~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi](#qe) | Düşük      |
| [Geçici anahtar değerleri artık varlık örneklerine ayarlı değil](#tkv) | Düşük      |
| [Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır](#de) | Düşük      |
| [Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir](#aes) | Düşük      |
| [Sahip olunan varlıklar, izleme sorgusu kullanılarak sahip olmadan sorgulanamıyor](#owned-query) | Düşük      |
| [Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir](#ip) | Düşük      |
| [Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor](#fkp) | Düşük      |
| [TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı](#dbc) | Düşük      |
| [Yedekleme alanları varsayılan olarak kullanılır](#backing-fields-are-used-by-default) | Düşük      |
| [Birden çok uyumlu yedekleme alanı bulunursa throw](#throw-if-multiple-compatible-backing-fields-are-found) | Düşük      |
| [Yalnızca alan özellik adları alan adıyla eşleşmelidir](#field-only-property-names-should-match-the-field-name) | Düşük      |
| [AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor](#adddbc) | Düşük      |
| [AddEntityFramework * bir boyut sınırı ile ımemorycache ekler](#addentityframework-adds-imemorycache-with-a-size-limit) | Düşük      |
| [DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor](#dbe) | Düşük      |
| [Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur](#string-and-byte-array-keys-are-not-client-generated-by-default) | Düşük      |
| [Iloggerfactory artık kapsamlı bir hizmettir](#ilf) | Düşük      |
| [Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | Düşük      |
| [İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | Düşük      |
| [HasOne/HasMany için tek bir dize ile çağrılan yeni davranış](#nbh) | Düşük      |
| [Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi](#rtnt) | Düşük      |
| [Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping](#rtt) | Düşük      |
| [Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur](#totable-on-a-derived-type-throws-an-exception) | Düşük      |
| [EF Core, SQLite FK zorlaması için artık pragma göndermez](#pragma) | Düşük      |
| [Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 bağımlıdır](#sqlite3) | Düşük      |
| [GUID değerleri artık SQLite üzerinde metın olarak depolanır](#guid) | Düşük      |
| [Char değerleri artık SQLite üzerinde metın olarak depolanır](#char) | Düşük      |
| [Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur](#migid) | Düşük      |
| [Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı](#xinfo) | Düşük      |
| [LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı](#lqpe) | Düşük      |
| [Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme](#clarify) | Düşük      |
| [Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı](#irdc2) | Düşük      |
| [Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi](#dip) | Düşük      |
| [SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi](#SQLitePCL) | Düşük      |
| [Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi](#NetTopologySuite) | Düşük      |
| [System. Data. SqlClient yerine Microsoft. Data. SqlClient kullanılır](#SqlClient) | Düşük      |
| [Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor](#mersa) | Düşük      |
| [DbFunction. Schema null ya da boş dize, modeli varsayılan şemasında olacak şekilde yapılandırır](#udf-empty-string) | Düşük      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>LINQ sorguları artık istemcide değerlendirilmedi

[Sorun izleme #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[da bakın #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

**Eski davranış**

3,0 öncesinde, EF Core bir sorgunun parçası olan bir ifadeyi SQL 'e veya bir parametreye dönüştüremediğinden, istemci üzerindeki ifadeyi otomatik olarak değerlendirdi.
Varsayılan olarak, pahalı olabilecek ifadelerin istemci değerlendirmesi yalnızca bir uyarı tetikledi.

**Yeni davranış**

3,0 ' den başlayarak EF Core, yalnızca üst düzey projeksiyonun (sorgudaki son `Select()` çağrı) istemci üzerinde değerlendirilmek üzere ifade etmesine izin verir.
Sorgunun başka bir bölümündeki deyimler SQL 'e veya parametreye dönüştürülemiyorsa, bir özel durum oluşturulur.

**Kaydol**

Sorguların otomatik istemci değerlendirmesi, önemli kısımları çevrilemeyecek halde birçok sorgunun yürütülmesini sağlar.
Bu davranış, yalnızca üretimde öngelebilecek beklenmedik ve zararlı olabilecek davranışlara neden olabilir.
Örneğin, çevrilemeyen bir `Where()` çağrısındaki bir koşul, tablodaki tüm satırların veritabanı sunucusundan aktarılmasına ve bu filtrenin istemciye uygulanmasına neden olabilir.
Bu durum, tablo yalnızca geliştirme sırasında yalnızca birkaç satır içeriyorsa, ancak tablo milyonlarca satır içerebilen, uygulamanın üretime taşınması halinde, kolayca algılanabilmelidir.
Geliştirme sırasında de istemci değerlendirmesi uyarıları yok sayılacak çok kolay.

Bunun yanı sıra, otomatik istemci değerlendirmesi, sürümler arasında istenmeyen değişikliklere neden olan belirli ifadeler için sorgu çevirisini geliştirme ile ilgili sorunlara yol açabilir.

**Risk Azaltıcı Etkenler**

Bir sorgu tam olarak çevrilemeyecek şekilde, sorguyu çevrilebilen bir biçimde yeniden yazın veya `AsEnumerable()`, `ToList()`ya da benzer bir şekilde, daha sonra LINQ-Objects kullanılarak daha sonra işlenebileceği istemciye açık bir şekilde veri getirmek için benzer.

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a>EF Core 3,0 hedefi .NET Standard 2,0 değil .NET Standard 2,1

[Sorun izleniyor #15498](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

**Eski davranış**

3,0 öncesinde, EF Core .NET Standard 2,0 ' i hedefledi ve .NET Framework dahil olmak üzere bu standardı destekleyen tüm platformlarda çalışacaktır.

**Yeni davranış**

3,0 ' den başlayarak, .NET Standard 2,1 ' EF Core ve bu standardı destekleyen tüm platformlarda çalışır. Bu, .NET Framework içermez.

**Kaydol**

Bu, .NET Core ve Xamarin gibi diğer modern .NET platformlarına enerji tasarrufu sağlamak için .NET teknolojilerinde stratejik bir kararın bir parçasıdır.

**Risk Azaltıcı Etkenler**

Modern bir .NET platformuna geçmeyi düşünün. Bu mümkün değilse, her ikisi de .NET Framework EF Core 2,1 veya EF Core 2,2 ' i kullanmaya devam edin.

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>Entity Framework Core artık ASP.NET Core paylaşılan Framework 'ün bir parçası değil

[Sorun bildirimleri izleniyor # 325](https://github.com/aspnet/Announcements/issues/325)

**Eski davranış**

ASP.NET Core 3,0 öncesinde, `Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All`bir paket başvurusu eklediğinizde, EF Core ve EF Core sağlayıcı gibi SQL Server veri sağlayıcılarından bazılarını dahil eder.

**Yeni davranış**

3,0 ' den başlayarak ASP.NET Core paylaşılan çerçeve EF Core veya EF Core veri sağlayıcıları içermez.

**Kaydol**

Bu değişiklikten önce, uygulamanın ASP.NET Core ve SQL Server hedeflendiğine bağlı olarak EF Core farklı adımlar gerekir. Ayrıca, yükseltme ASP.NET Core EF Core ve SQL Server sağlayıcının yükseltmesini zorlarak her zaman istenmez.

Bu değişiklik ile, EF Core alma deneyimi tüm sağlayıcılar, desteklenen .NET uygulamaları ve uygulama türleri arasında aynıdır.
Geliştiriciler artık EF Core ve EF Core veri sağlayıcılarının yükseltilme sırasında tam olarak denetim de alabilir.

**Risk Azaltıcı Etkenler**

ASP.NET Core 3,0 uygulamasında veya desteklenen başka bir uygulamada EF Core kullanmak için, uygulamanızın kullanacağı EF Core veritabanı sağlayıcısına açıkça bir paket başvurusu ekleyin.

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a>EF Core komut satırı aracı, DotNet EF, artık .NET Core SDK bir parçası değil

[Sorun izleniyor #14016](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

**Eski davranış**

3,0 ' dan önce, `dotnet ef` aracı .NET Core SDK eklenmiştir ve ek adımlara gerek duymadan herhangi bir projeden komut satırından kullanılmak üzere hazırdır. 

**Yeni davranış**

3,0 ' den başlayarak .NET SDK `dotnet ef` aracını içermez, bu nedenle kullanabilmeniz için önce yerel veya küresel bir araç olarak açıkça kurmanız gerekir. 

**Kaydol**

Bu değişiklik, NuGet üzerinde düzenli bir .NET CLı aracı olarak `dotnet ef` dağıtmamızı ve güncelleştirmenizi sağlar ve bu da EF Core 3,0 her zaman bir NuGet paketi olarak dağıtılır.

**Risk Azaltıcı Etkenler**

`DbContext`geçişleri veya yapı iskelesi yönetmek için `dotnet-ef` genel bir araç olarak yükler:

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

Ayrıca, bir [araç bildirim dosyası](https://github.com/dotnet/cli/issues/10288)kullanarak araç bağımlılığı olarak bildiren bir projenin bağımlılıklarını geri yüklediğinizde de bunu yerel bir araç edinebilirsiniz.

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a>FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı

[Sorun izleniyor #10996](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

**Eski davranış**

3,0 EF Core önce, bu yöntem adları, bir normal dize veya SQL ve parametrelere yönelik bir dizeyle çalışmak üzere aşırı yüklendi.

**Yeni davranış**

EF Core 3,0 ' den başlayarak, parametrelerin sorgu dizesinden ayrı olarak geçirildiği parametreli bir sorgu oluşturmak için `FromSqlRaw`, `ExecuteSqlRaw`ve `ExecuteSqlRawAsync` kullanın.
Örneğin:

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

Parametrelerin, enterpolasyonlu bir sorgu dizesinin parçası olarak geçirildiği parametreli bir sorgu oluşturmak için `FromSqlInterpolated`, `ExecuteSqlInterpolated`ve `ExecuteSqlInterpolatedAsync` kullanın.
Örneğin:

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

Yukarıdaki sorguların her ikisinin de aynı SQL parametreleriyle aynı parametreli SQL 'i ürettiğine unutmayın.

**Kaydol**

Bu gibi yöntem aşırı yüklemeleri, amaç, enterpolasyonlu dize yöntemini çağırmak ve diğer şekilde, ham dize metodunu yanlışlıkla çağırmak çok kolay hale getirir.
Bu, sorguların olması gerektiğinde parametreli hale getirmemelidir.

**Risk Azaltıcı Etkenler**

Yeni yöntem adlarını kullanmak için geçiş yapın.

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a>Saklı yordamla birlikte kullanıldığında FromSql metodu birleştirilemez

[Sorun izleniyor #15392](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

**Eski davranış**

3,0 EF Core önce, FromSql yöntemi, geçirilen SQL 'in üzerine oluşmayabilir. SQL, saklı yordam gibi birleştirilmemiş bir işlem olduğu zaman istemci değerlendirmesi gerçekleştirdi. Aşağıdaki sorgu, sunucuda saklı yordam çalıştırılarak ve istemci tarafında FirstOrDefault ' i gerçekleştirerek çalıştı.

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

**Yeni davranış**

EF Core 3,0 ' den başlayarak, EF Core SQL ayrıştırmaya çalışmaz. Bu nedenle, FromSqlRaw/Fromsqlenterpolasyonından sonra oluşturuyorsanız, EF Core alt sorguya neden olarak SQL 'i oluşturacak. Bu nedenle, bileşim ile saklı yordam kullanıyorsanız, geçersiz SQL söz dizimi için bir özel durum alırsınız.

**Kaydol**

EF Core 3,0, [burada](#linq-queries-are-no-longer-evaluated-on-the-client)açıklandığı gibi bir hata olduğundan, otomatik istemci değerlendirmesini desteklemez.

**Mayı**

FromSqlRaw/Fromsqlenterpolasyonnda saklı bir yordam kullanıyorsanız, bunun üzerine oluşmadığını bilirsiniz; böylece sunucu tarafında herhangi bir kompozisyonu önlemek için FromSql yöntem çağrısından sonra __asslanabilir/AsAsyncEnumerable__ ekleyebilirsiniz.

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a>FromSql metotları yalnızca sorgu köklerine göre belirtilebilir

[Sorun izleniyor #15704](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

**Eski davranış**

3,0 EF Core önce, `FromSql` yöntemi sorgunun herhangi bir yerinden belirtilebilir.

**Yeni davranış**

EF Core 3,0 ' den başlayarak, yeni `FromSqlRaw` ve `FromSqlInterpolated` Yöntemleri (`FromSql`değiştirme) yalnızca sorgu köklerinin üzerinde belirtilebilir, yani doğrudan `DbSet<>`. Bunları başka her yerde belirtmeye çalışmak, derleme hatasına neden olur.

**Kaydol**

`DbSet` üzerinde `FromSql` herhangi bir yerde, hiçbir anlamı veya eklenen değeri belirtmediyse, bazı senaryolarda belirsizliğe neden olabilir.

**Risk Azaltıcı Etkenler**

`FromSql` etkinleştirmeleri, uygulandıkları `DbSet` doğrudan taşınmalıdır.

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a>Hiçbir izleme sorgusu artık kimlik çözümlemesi gerçekleştirmesiz

[Sorun izleniyor #13518](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

**Eski davranış**

EF Core 3,0 ' dan önce, belirli bir tür ve KIMLIĞE sahip bir varlığın her oluşumu için aynı varlık örneği kullanılacaktır. Bu, sorguları izleme davranışıyla eşleşir. Örneğin, bu sorgu:

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
, verilen kategoriyle ilişkilendirilen her bir `Product` için aynı `Category` örneğini döndürür.

**Yeni davranış**

EF Core 3,0 ' den başlayarak, döndürülen grafikte farklı yerlerde verilen tür ve KIMLIĞE sahip bir varlık ile karşılaşıldığında farklı varlık örnekleri oluşturulacaktır. Örneğin, yukarıdaki sorgu artık aynı kategori ile iki ürün ilişkilendirildiğinde bile her bir `Product` için yeni bir `Category` örneği döndürecek.

**Kaydol**

Kimlik çözümlemesi (diğer bir deyişle, bir varlığın daha önce karşılaşılan varlıkla aynı türe ve KIMLIĞE sahip olduğunu belirlemek) ek performans ve bellek yükü ekler. Bu genellikle, ilk yerde hiçbir izleme sorgusunun neden kullanıldığına ilişkin sayacı çalıştırır. Ayrıca, kimlik çözümlemesi bazen faydalı olabilirken, varlıkların serileştirilmek ve bir istemciye gönderilmesi, hiçbir izleme sorgusu için ortak olan bir istemciye gönderiliyorsa, bu gerekli değildir.

**Risk Azaltıcı Etkenler**

Kimlik çözümlemesi gerekiyorsa izleme sorgusu kullanın.

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a>~~Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir~~ Çevrildi

[Sorun izleniyor #14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

EF Core 3,0 ' deki yeni yapılandırma, uygulama tarafından herhangi bir olayın günlük düzeyinin belirtilmesini sağladığından bu değişikliği geri çevirdik. Örneğin, SQL 'in günlüğe kaydedilmesini `Debug`değiştirmek için `OnConfiguring` veya `AddDbContext`düzeyini açıkça yapılandırın:
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>Geçici anahtar değerleri artık varlık örneklerine ayarlı değil

[Sorun izleniyor #12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

**Eski davranış**

3,0 EF Core önce, geçici değerler daha sonra veritabanı tarafından oluşturulan gerçek bir değere sahip olan tüm anahtar özelliklerine atandı.
Genellikle bu geçici değerler büyük negatif sayılardır.

**Yeni davranış**

3,0 ile başlayarak, EF Core, geçici anahtar değerini varlığın izleme bilgilerinin bir parçası olarak depolar ve anahtar özelliğinin kendisini değiştirmeden bırakır.

**Kaydol**

Daha önce bazı `DbContext` örnekleri tarafından izlenen bir varlık farklı bir `DbContext` örneğine taşındığında, geçici anahtar değerlerinin yanlışlıkla kalıcı hale gelmesini engellemek için bu değişiklik yapılmıştır. 

**Risk Azaltıcı Etkenler**

Varlıklar arasındaki ilişkilendirmeleri oluşturmak için yabancı anahtarlara birincil anahtar değerleri atayan uygulamalar, birincil anahtarların mağaza oluşturulup `Added` durumundaki varlıklara ait olması durumunda eski davranışa bağlı olabilir.
Bu, şunları önlenebilir:
* Mağaza tarafından oluşturulan anahtarlar kullanmıyor.
* Yabancı anahtar değerlerini ayarlamak yerine gezinti özelliklerini, ilişkileri oluşturmak için ayarlama.
* Varlığın izleme bilgileriyle gerçek geçici anahtar değerlerini elde edin.
Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue`, `blog.Id` kendisi ayarlanmamışsa bile geçici değeri döndürür.

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a>DetectChanges, Store tarafından oluşturulan anahtar değerlerini

[Sorun izleniyor #14616](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

**Eski davranış**

EF Core 3,0 ' dan önce, `DetectChanges` tarafından bulunan izlenmeyen bir varlık `Added` durumunda izlenir ve `SaveChanges` çağrıldığında yeni bir satır olarak eklenir.

**Yeni davranış**

EF Core 3,0 ' den başlayarak, bir varlık üretilen anahtar değerlerini kullanıyorsa ve bazı anahtar değer ayarlandıysa, varlık `Modified` durumunda izlenir.
Bu, varlık için bir satırın var olduğu varsayılır ve `SaveChanges` çağrıldığında güncelleştirilir.
Anahtar değeri ayarlanmamışsa veya varlık türü oluşturulan anahtarları kullanmazsa, yeni varlık önceki sürümlerde olduğu gibi `Added` olarak izlenir.

**Kaydol**

Bu değişiklik, Store tarafından oluşturulan anahtarlar kullanılırken bağlantısı kesilen varlık grafikleriyle daha kolay ve daha tutarlı hale getirilmek üzere yapılmıştır.

**Risk Azaltıcı Etkenler**

Bu değişiklik, bir varlık türü oluşturulan anahtarları kullanacak şekilde yapılandırıldıysa ancak anahtar değerleri açıkça yeni örnekler için ayarlandıysa, bir uygulamayı bozabilir.
Bu çözüm, anahtar özelliklerinin oluşturulan değerleri kullanmamasına açık bir şekilde yapılandırmaktır.
Örneğin, Fluent API:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

Ya da veri ek açıklamalarıyla:

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a>Art arda silme işlemleri artık varsayılan olarak hemen gerçekleşir

[Sorun izleniyor #10114](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

**Eski davranış**

3,0 öncesinde, EF Core (gerekli bir asıl öğe silindiğinde veya gerekli bir sorumluya ilişki olmadığında bağımlı varlıkları silme), SaveChanges çağrılana kadar gerçekleşmediği için.

**Yeni davranış**

3,0 ile başlayarak, Tetikleme koşulu algılandığında EF Core geçişli eylemleri uygular.
Örneğin, bir sorumlu varlığı silmek için `context.Remove()` çağırmak, izlenen ilgili tüm bağımlı bağımlıların de hemen `Deleted` olarak ayarlanabilmesini sağlar.

**Kaydol**

Bu değişiklik, `SaveChanges` çağrılmadan _önce_ hangi varlıkların silineceğini anlamak için önemli olan veri bağlama ve denetim senaryolarına yönelik deneyimi geliştirmek üzere yapılmıştır.

**Risk Azaltıcı Etkenler**

Önceki davranış `context.ChangeTracker`ayarları aracılığıyla geri yüklenebilir.
Örneğin:

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a>İlgili varlıkların yüklenmesi artık tek bir sorguda gerçekleşir

[Sorun izleniyor #18022](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

**Eski davranış**

3,0 öncesinde, `Include` işleçleri aracılığıyla koleksiyon gezginlerini yüklemek, her ilgili varlık türü için bir tane olmak üzere ilişkisel veritabanında birden çok sorgunun oluşturulmasına neden oldu.

**Yeni davranış**

3,0 ile başlayarak, EF Core ilişkisel veritabanlarında birleştirmelere sahip tek bir sorgu oluşturur.

**Kaydol**

Tek bir LINQ sorgusu uygulamak için birden çok sorgu verilmesi, birden çok veritabanı yuvarlaklığına gerek olduğu için negatif performans dahil olmak üzere çok sayıda soruna neden olmuş ve her sorgu veritabanının farklı bir durumunu gözlemleyebilmelidir.

**Risk Azaltıcı Etkenler**

Teknik olarak bu bir değişiklik olmadığı sürece, tek bir sorgu koleksiyon gezginlerinin çok sayıda `Include` işleci içerdiğinde uygulama performansının önemli bir etkisi olabilir. Daha fazla bilgi edinmek ve sorguları daha verimli bir şekilde yeniden yazmak için [Bu açıklamaya bakın](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) .

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a>DeleteBehavior. restrict Temizleme semantiğine sahip

[Sorun izleniyor #12661](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

**Eski davranış**

3,0 ' den önce, `Restrict` anlamlarında `DeleteBehavior.Restrict` yabancı anahtarlar oluşturdunuz, ancak aynı zamanda iç düzeltmeyi belirgin olmayan bir şekilde değiştirdi.

**Yeni davranış**

3,0 ile başlayarak, `DeleteBehavior.Restrict` yabancı anahtarların `Restrict` semantiğinin oluşturulmasını sağlar; Yani bu, basamaksız; sınır iç düzeltmesini etkilemeden kısıtlama ihlalinde throw.

**Kaydol**

Bu değişiklik, beklenmeyen yan etkileri olmadan `DeleteBehavior` kullanımı kolay bir şekilde geliştirmek için yapılmıştır.

**Risk Azaltıcı Etkenler**

Önceki davranış `DeleteBehavior.ClientNoAction`kullanılarak geri yüklenebilir.

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a>Sorgu türleri varlık türleriyle birleştirilir

[Sorun izleniyor #14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

**Eski davranış**

EF Core 3,0 öncesinde, [sorgu türleri](xref:core/modeling/keyless-entity-types) bir birincil anahtarı yapılandırılmış bir şekilde tanımlamayan verileri sorgulamak için bir araçtır.
Diğer bir deyişle, anahtar olmadan varlık türlerini (bir görünümden daha büyük olasılıkla, belki de bir tablodan daha büyük bir şekilde) eşlemek için bir sorgu türü kullanılmıştır (bir tablodan daha büyük olabilir, ancak muhtemelen bir görünümden).

**Yeni davranış**

Bir sorgu türü artık birincil anahtar olmadan yalnızca bir varlık türü haline gelir.
Keyless varlık türleri, önceki sürümlerdeki sorgu türleriyle aynı işlevselliğe sahiptir.

**Kaydol**

Bu değişiklik, sorgu türlerinin amacını aşmak için yapılmıştır.
Özellikle, bunlar, anahtarsız varlık türlerdir ve bu, doğal olarak salt okunurdur, ancak yalnızca bir varlık türünün Salt okunabilir olması gerektiğinden kullanılmamalıdır.
Benzer şekilde, genellikle görünümlere eşlenir, ancak bu yalnızca görünümler genellikle anahtar tanımlamaz.

**Risk Azaltıcı Etkenler**

API 'nin aşağıdaki bölümleri artık kullanılmıyor:
* **`ModelBuilder.Query<>()`** -bir varlık türünü anahtar olmadan işaretlemek için `ModelBuilder.Entity<>().HasNoKey()` çağrılması gerekir.
Birincil anahtar beklendiğinde ancak kuralıyla eşleşmediği zaman yanlış yapılandırılmaması için bu kural tarafından yapılandırılmamalıdır.
* **`DbQuery<>`** bunun yerine `DbSet<>` kullanılmalıdır.
* **`DbContext.Query<>()`** bunun yerine `DbContext.Set<>()` kullanılmalıdır.

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a>Sahip olunan tür ilişkilerinin Yapılandırma API 'SI değişti

Sorun izleme [#12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444) sorunu
izleme sorunu [#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[izleme sorunu #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

**Eski davranış**

3,0 EF Core önce, sahip olan ilişkinin yapılandırması `OwnsOne` veya `OwnsMany` çağrısından sonra doğrudan gerçekleştirildi. 

**Yeni davranış**

EF Core 3,0 ' den başlayarak, artık `WithOwner()`kullanarak sahip için bir gezinti özelliği yapılandırmak Fluent API.
Örneğin:

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

Sahip ve sahibi arasındaki ilişkiyle ilgili yapılandırma artık diğer ilişkilerin yapılandırılmasına benzer şekilde `WithOwner()` sonra zincirde olmalıdır.
Sahip olduğu için yapılandırma, `OwnsOne()/OwnsMany()`sonra hala zincirleme olmaya devam edecektir.
Örneğin:

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

Ayrıca, sahip bir tür hedefi ile `Entity()`, `HasOne()`veya `Set()` çağırmak artık bir özel durum oluşturacak.

**Kaydol**

Bu değişiklik, sahip olunan türün kendisini ve sahip olduğu _ilişkiyi_ yapılandırma arasında bir temizleyici ayrım oluşturmak için yapılmıştır.
Bu, sırasıyla `HasForeignKey`gibi yöntemlere ve karışıklığı ortadan kaldırır.

**Risk Azaltıcı Etkenler**

Yukarıdaki örnekte gösterildiği gibi, sahip olunan tür ilişkilerinin yapılandırmasını yeni API yüzeyini kullanacak şekilde değiştirin.

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır

[Sorun izleniyor #9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

**Eski davranış**

Aşağıdaki modeli göz önünde bulundurun:
```csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
EF Core 3,0 ' dan önce, `OrderDetails` aynı tabloyla `Order` veya açıkça eşlenmişse, yeni bir `Order`eklenirken `OrderDetails` örneği her zaman gereklidir.


**Yeni davranış**

3,0 ile başlayarak EF Core, `OrderDetails` olmayan bir `Order` eklemesine ve birincil anahtar null yapılabilir sütunlara hariç tüm `OrderDetails` özelliklerini eşleştirme olanağı sağlar.
EF Core kümelerini sorgularken, gerekli özelliklerinden herhangi birinin bir değeri yoksa veya birincil anahtar ve tüm `null`Özellikler ' in yanı sıra gerekli özellikleri yoksa, `null` `OrderDetails`.

**Risk Azaltıcı Etkenler**

Modelinizin tüm isteğe bağlı sütunlarla ilişkili bir tablo paylaşımına sahipse, ancak buna işaret eden gezinmenin `null` olması beklenmiyorsa, gezinti `null`olduğunda, uygulamanın servis taleplerini işleyecek şekilde değiştirilmesi gerekir. Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmelidir ya da en az bir özellik kendisine atanmış`null` bir değere sahip olmalıdır.

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a>Bir eşzamanlılık belirteci sütunuyla bir tabloyu paylaşan tüm varlıkların onu bir özellik ile eşlemesi gerekir

[Sorun izleniyor #14154](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

**Eski davranış**

Aşağıdaki modeli göz önünde bulundurun:
```csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
EF Core 3,0 ' dan önce, `OrderDetails` aynı tabloyla `Order` ya da açıkça eşlenmişse, yalnızca `OrderDetails` güncelleştirilmesi istemci üzerindeki `Version` değerini güncelleştirmez ve sonraki güncelleştirme başarısız olur.


**Yeni davranış**

3,0 ' den itibaren, yeni `Version` değerini `OrderDetails`sahip `Order` EF Core yayar. Aksi takdirde model doğrulaması sırasında bir özel durum oluşturulur.

**Kaydol**

Aynı tabloyla eşlenmiş varlıkların yalnızca biri güncelleştirildiği zaman eski bir eşzamanlılık belirteci değerinden kaçınmak için bu değişiklik yapılmıştır.

**Risk Azaltıcı Etkenler**

Tabloyu paylaşan tüm varlıkların eşzamanlılık belirteci sütunuyla eşlenen bir özelliği içermesi gerekir. Gölge durumundaki bir oluşturma olasılığı vardır:
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a>Sahip olunan varlıklar, izleme sorgusu kullanılarak sahip olmadan sorgulanamıyor

[Sorun izleniyor #18876](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

**Eski davranış**

EF Core 3,0 ' dan önce, sahip olan varlıklar başka bir gezinti olarak sorgulanabilir.

```csharp
context.People.Select(p => p.Address);
```

**Yeni davranış**

3,0 ile başlayarak, bir izleme sorgusu sahip olmayan bir varlık projesinde proje EF Core oluşturulur.

**Kaydol**

Sahip olunan varlıklar sahip olmadan değiştirilemez, bu nedenle bu şekilde sorgulama durumlarının büyük çoğunluğunda bir hata olur.

**Risk Azaltıcı Etkenler**

Sahip olduğu varlık daha sonra değiştirilecek şekilde izleniyorsa, sahibin sorguya eklenmesi gerekir.

Aksi takdirde `AsNoTracking()` çağrısı ekleyin:

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a>Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütunla eşleştirilir

[Sorun izleniyor #13998](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

**Eski davranış**

Aşağıdaki modeli göz önünde bulundurun:
```csharp
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

EF Core 3,0 ' dan önce, `ShippingAddress` özelliği varsayılan olarak `BulkOrder` ve `Order` ayrı sütunlara eşlenir.

**Yeni davranış**

3,0 ile başlayarak, EF Core `ShippingAddress`için yalnızca bir sütun oluşturur.

**Kaydol**

Eski davranınır beklenmiyordu.

**Risk Azaltıcı Etkenler**

Özelliği yine de türetilmiş türlerde ayrı sütunlara açıkça eşlenmiş olabilir:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>Yabancı anahtar özellik kuralı artık Principal özelliği ile aynı ad ile eşleşmiyor

[Sorun izleniyor #13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

**Eski davranış**

Aşağıdaki modeli göz önünde bulundurun:
```csharp
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
EF Core 3,0 ' dan önce, `CustomerId` özelliği kural tarafından yabancı anahtar için kullanılır.
Ancak, `Order` sahipli bir tür ise, bu da birincil anahtar `CustomerId` olur ve bu genellikle beklenmez.

**Yeni davranış**

3,0 ile başlayarak, EF Core, Principal özelliğiyle aynı ada sahip olmaları durumunda yabancı anahtarlar için özellikleri kullanmayı denemez.
Asıl Özellik adı ile birleştirilmiş asıl tür adı ve asıl özellik adı desenleriyle birleştirilmiş gezinti adı hala eşleştirilir.
Örneğin:

```csharp
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```csharp
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**Kaydol**

Bu değişiklik, sahip olan türde birincil anahtar özelliğini yanlışlıkla tanımlamayı önlemek için yapılmıştır.

**Risk Azaltıcı Etkenler**

Özelliğin yabancı anahtar ve bu nedenle birincil anahtarın bir parçası olması amaçlandıysa, bu şekilde açıkça yapılandırın.

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a>TransactionScope tamamlanmadan önce artık kullanılmıyorsa, veritabanı bağlantısı artık kapalı

[Sorun izleniyor #14218](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

**Eski davranış**

EF Core 3,0 ' dan önce, bağlam bağlantıyı bir `TransactionScope`içinde açarsa, geçerli `TransactionScope` etkinken bağlantı açık kalır.

```csharp
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point

        var categories = context.ProductCategories().ToList();
    }
}
```

**Yeni davranış**

3,0 ' den itibaren EF Core, bağlantıyı kullanarak işlemi tamamladıktan hemen sonra kapatır.

**Kaydol**

Bu değişiklik aynı `TransactionScope`birden çok bağlam kullanılmasına izin verir. Yeni davranış ayrıca EF6 ile eşleşir.

**Risk Azaltıcı Etkenler**

Bağlantının açık açık olarak kalması gerekiyorsa `OpenConnection()`, EF Core zamanından önce kapanmasını sağlar:

```csharp
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Her özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır

[Sorun izleniyor #6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

**Eski davranış**

3,0 EF Core önce, tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanılmıştır.

**Yeni davranış**

EF Core 3,0 ' den başlayarak, bellek içi veritabanı kullanılırken her tamsayı anahtar özelliği kendi değer oluşturucuyu alır.
Ayrıca, veritabanı silinirse, tüm tablolar için anahtar oluşturma sıfırlanır.

**Kaydol**

Bu değişiklik, bellek içi anahtar oluşturmayı gerçek veritabanı anahtarı oluşturmaya daha yakından hizalamaya ve bellek içi veritabanını kullanırken testlerin birbirinden yalıtılmasına olanak sağlamak için yapılmıştır.

**Risk Azaltıcı Etkenler**

Bu, belirli bellek içi anahtar değerlerine bağlı olan bir uygulamayı bölebilir.
Bunun yerine, belirli anahtar değerlerine bağlı değil veya yeni davranışla eşleşecek şekilde güncellemeden düşünün.

### <a name="backing-fields-are-used-by-default"></a>Yedekleme alanları varsayılan olarak kullanılır

[Sorun izleniyor #12430](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

**Eski davranış**

3,0 ' den önce, bir özellik için yedekleme alanı bilinse bile EF Core, özellik alıcı ve ayarlayıcı yöntemlerini kullanarak özellik değerini yine de okur ve yazar.
Bunun özel durumu sorgu yürütmeyle, burada yedekleme alanının biliniyorsa doğrudan ayarlandığı durumdur.

**Yeni davranış**

EF Core 3,0 ' den başlayarak, bir özellik için yedekleme alanı biliniyorsa, EF Core, bu özelliği her zaman, bu özelliği yedekleme alanını kullanarak okur ve yazar.
Bu, uygulama, alıcı veya ayarlayıcı yöntemleriyle kodlanmış ek davranışa bağlı olduğunda uygulama kesintiye neden olabilir.

**Kaydol**

Bu değişiklik, varlıklarla ilgili veritabanı işlemlerini gerçekleştirirken, EF Core yanlışlıkla iş mantığını tetiklemesini engellemek üzere yapılmıştır.

**Risk Azaltıcı Etkenler**

3,0 öncesi davranış `ModelBuilder`özellik erişim modunun yapılandırması aracılığıyla geri yüklenebilir.
Örneğin:

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Birden çok uyumlu yedekleme alanı bulunursa throw

[Sorun izleniyor #12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

**Eski davranış**

EF Core 3,0 öncesinde, birden çok alan bir özelliğin yedekleme alanını bulmaya yönelik kurallarla eşleşirse, bir alan belirli bir öncelik sırasına göre seçilir.
Bu, belirsiz durumlarda yanlış alanın kullanılmasına neden olabilir.

**Yeni davranış**

EF Core 3,0 ' den başlayarak, birden çok alan aynı özellik ile eşleşirse, bir özel durum oluşturulur.

**Kaydol**

Bu değişiklik, yalnızca bir tane doğru olduğunda bir alanı başka bir alan ile sessizce kullanmaktan kaçınmak için yapılmıştır.

**Risk Azaltıcı Etkenler**

Belirsiz yedekleme alanları olan özelliklerin açık olarak kullanılması için alanı olmalıdır.
Örneğin, Fluent API kullanımı:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a>Yalnızca alan özellik adları alan adıyla eşleşmelidir

**Eski davranış**

EF Core 3,0 ' dan önce bir özellik bir dize değeri ile belirtilebilir ve .NET türünde bu ada sahip bir özellik bulunmazsa EF Core, kural kurallarını kullanarak bir alanla eşleştirmeye çalışır.

```csharp
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

**Yeni davranış**

EF Core 3,0 ' den başlayarak, yalnızca alan özelliği alan adı ile aynı olmalıdır.

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

**Kaydol**

Benzer şekilde adlandırılan iki özellik için aynı alanı kullanmaktan kaçınmak için bu değişiklik yapılmıştır. aynı zamanda yalnızca alan özellikleri için eşleşen kuralların CLR özellikleriyle eşlenen özelliklerle aynı olmasını sağlar.

**Risk Azaltıcı Etkenler**

Yalnızca alan özellikleri, eşlendiği alanla aynı olarak adlandırılmalıdır.
3,0 sonrasında EF Core gelecek bir sürümünde, özellik adından farklı olan bir alan adını açıkça yapılandırmayı yeniden etkinleştirmeyi planlıyoruz (bkz. sorun [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağrısını içermiyor

[Sorun izleniyor #14756](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

**Eski davranış**

EF Core 3,0 ' dan önce, `AddDbContext` veya `AddDbContextPool` çağırma işlemi, [Addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [addmemorycache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)çağrılarına çağrı ile günlüğe kaydetme ve bellek önbelleğe alma hizmetlerini de kaydeder.

**Yeni davranış**

EF Core 3,0 ' den başlayarak `AddDbContext` ve `AddDbContextPool` artık bu hizmetleri bağımlılık ekleme (dı) ile kaydetmez.

**Kaydol**

EF Core 3,0, bu hizmetlerin uygulamanın DI kapsayıcısında olmasını gerektirmez. Ancak, `ILoggerFactory` uygulamanın dı kapsayıcısına kayıtlıysa, EF Core tarafından hala kullanılacaktır.

**Risk Azaltıcı Etkenler**

Uygulamanız bu hizmetlere ihtiyaç duyuyorsa, bunları [Addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [ADDMEMORYCACHE](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)kullanarak dı kapsayıcısı ile açık olarak kaydedin.

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a>AddEntityFramework * bir boyut sınırı ile ımemorycache ekler

[Sorun izleniyor #12905](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

**Eski davranış**

EF Core 3,0 ' dan önce, `AddEntityFramework*` yöntemlerin çağrılması, bellek önbelleğe alma hizmetlerini bir boyut sınırı olmadan DI ile de kaydeder.

**Yeni davranış**

EF Core 3,0 ' den başlayarak `AddEntityFramework*` bir ımemorycache hizmetini boyut sınırı ile kaydeder. Daha sonra eklenen diğer hizmetler ımemorycache 'e bağımlıysa, özel durumlara veya performans düşüklüğe neden olan varsayılan sınıra hızlıca ulaşabilirler.

**Kaydol**

Sorgu önbelleğe alma mantığındaki bir hata varsa veya sorgular dinamik olarak oluşturulduysa, ımemorycache 'i sınır olmadan kullanmak denetlenmeyen bellek kullanımına neden olabilir. Varsayılan sınırın olması olası bir DoS saldırısının etkisini azaltır.

**Risk Azaltıcı Etkenler**

Çoğu durumda `AddEntityFramework*` çağrısı `AddDbContext` veya `AddDbContextPool` da çağrılırsa gerekli değildir. Bu nedenle, en iyi risk azaltma `AddEntityFramework*` çağrısını kaldırmayacak.

Uygulamanız bu hizmetlere ihtiyaç duyuyorsa, daha önce [addmemorycache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)' i kullanarak bir ıMEMORYCACHE uygulamasını dı kapsayıcısı ile açık olarak kaydedin.

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext. Entry artık yerel bir DetectChanges gerçekleştiriyor

[Sorun izleniyor #13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

**Eski davranış**

EF Core 3,0 önce, `DbContext.Entry` çağırma, izlenen tüm varlıklar için değişikliklerin algılanmasına neden olur.
Bu, `EntityEntry` açığa çıkarılan durumun güncel olduğunu ortaya çıkardık.

**Yeni davranış**

EF Core 3,0 ' den başlayarak, çağırma `DbContext.Entry` artık yalnızca verilen varlıktaki değişiklikleri ve bununla ilgili tüm izlenen ana varlıkları algılamaya çalışır.
Bu, uygulama durumunda etkileri olabilecek, bu yöntemi çağırarak başka bir yerde değişiklik algılanmamış olabileceği anlamına gelir.

`ChangeTracker.AutoDetectChangesEnabled` `false` olarak ayarlanırsa, bu yerel değişikliğin algılanması de devre dışı bırakılır.

Değişiklik algılamasına neden olan diğer yöntemler--örneğin `ChangeTracker.Entries` ve `SaveChanges`--yine de izlenen tüm varlıkların tam `DetectChanges` neden olur.

**Kaydol**

Bu değişiklik, `context.Entry`kullanmanın varsayılan performansını geliştirmek için yapılmıştır.

**Risk Azaltıcı Etkenler**

3,0 öncesi davranışı sağlamak için `Entry` çağrılmadan önce `ChgangeTracker.DetectChanges()` çağırın.

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>Dize ve bayt dizisi anahtarları, varsayılan olarak istemci tarafından oluşturulur

[Sorun izleniyor #14617](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

**Eski davranış**

EF Core 3,0 önce `string` ve `byte[]` anahtar özellikleri açık bir şekilde null olmayan bir değer ayarlamadan kullanılabilir.
Böyle bir durumda, anahtar değeri istemci üzerinde `byte[]`için bayt olarak serileştirilmiş bir GUID olarak oluşturulur.

**Yeni davranış**

EF Core 3,0 ' den itibaren, hiçbir anahtar değer ayarlanmadığını belirten bir özel durum atılır.

**Kaydol**

Bu değişiklik, istemci tarafından oluşturulan `string`/`byte[]` değerleri genellikle yararlı olmadığından ve varsayılan davranış ortak bir şekilde oluşturulan anahtar değerleri hakkında bir nedene kadar zor hale getirildiğinden yapılmıştır.

**Risk Azaltıcı Etkenler**

Önceden 3,0 davranışı, hiçbir null olmayan değer ayarlanmamışsa, anahtar özelliklerinin oluşturulan değerleri kullanması gerektiğini açıkça belirterek elde edilebilir.
Örneğin, Fluent API:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

Ya da veri ek açıklamalarıyla:

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a>Iloggerfactory artık kapsamlı bir hizmettir

[Sorun izleniyor #14698](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

**Eski davranış**

EF Core 3,0 tarihinden önce `ILoggerFactory` bir tek hizmet olarak kaydedildi.

**Yeni davranış**

EF Core 3,0 ' den başlayarak `ILoggerFactory` artık kapsamlı olarak kaydedilir.

**Kaydol**

Bu değişiklik, diğer işlevleri sağlayan ve iç hizmet sağlayıcılarının açılımı gibi bazı bazı durumları kaldıran `DbContext` örneğiyle bir günlükçü ilişkilendirmesine izin vermek üzere yapılmıştır.

**Risk Azaltıcı Etkenler**

Bu değişiklik, EF Core iç hizmet sağlayıcısı 'nda özel hizmetleri kaydetmediğiniz ve kullanmadıkça uygulama kodunu etkilememelidir.
Bu, yaygın değildir.
Bu durumlarda, çoğu şey çalışmaya devam edecektir, ancak `ILoggerFactory` bağlı olan herhangi bir singleton hizmeti, `ILoggerFactory` farklı bir şekilde almak için değiştirilmelidir.

Bu gibi durumlarda çalıştırırsanız, daha sonra bunu nasıl yeniden keseceğimizi daha iyi anlayabilmemiz için lütfen [EF Core GitHub sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues) ' nde bir sorun `ILoggerFactory` bildirin.

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>Yavaş yükleme proxy 'leri artık gezinti özelliklerinin tam olarak yüklenmediğini varsaymaz

[Sorun izleniyor #12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

**Eski davranış**

EF Core 3,0 ' dan önce, bir `DbContext` atıldıktan sonra, söz konusu bağlamdan alınan bir varlık üzerinde verilen bir gezinti özelliğinin tam olarak yüklenip yüklenmediğini bilmenin bir yolu yoktur.
Proxy 'ler bunun yerine, null olmayan bir değer varsa ve bir koleksiyon gezintisi boş değilse yüklenmiş bir başvuru gezintisi olduğunu varsayacaktır.
Bu durumlarda, yavaş yüklemeye çalışılması, bir op değildir.

**Yeni davranış**

EF Core 3,0 ' den başlayarak, proxy 'ler, Gezinti özelliğinin yüklenip yüklenmediğini izler.
Bu, bağlam atıldıktan sonra yüklenen bir gezinti özelliğine erişmeye çalışan, yüklü gezinti boş veya null olduğunda bile her zaman bir op olmayacaktır.
Buna karşılık, yüklü olmayan bir gezinti özelliğine erişme girişimi, gezinti özelliği boş olmayan bir koleksiyon olsa bile, bağlam atıldıysa bir özel durum oluşturur.
Bu durum ortaya çıkarsa, uygulama kodunun geç yüklemeyi geçersiz bir zamanda kullanmaya çalıştığı ve uygulamanın bunu yapamayacağı şekilde değiştirilmesi gerektiği anlamına gelir.

**Kaydol**

Bu değişiklik, atılmış bir `DbContext` örneğine geç yüklemeye çalışırken davranışı tutarlı ve doğru hale getirmek için yapılmıştır.

**Risk Azaltıcı Etkenler**

Uygulama kodunu, atılmış bağlamla geç yüklemeye kalkışacak şekilde güncelleştirin veya bunu özel durum iletisinde açıklandığı şekilde bir op olarak yapılandırın.

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>İç hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır

[Sorun izleniyor #10236](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

**Eski davranış**

3,0 ' dan EF Core önce, bir yol için iç hizmet sağlayıcılarının bulunduğu bir uygulama için bir uyarı günlüğe kaydedilir.

**Yeni davranış**

EF Core 3,0 ' den başlayarak bu uyarı artık dikkate alınır ve hata oluşur ve bir özel durum oluşturulur. 

**Kaydol**

Bu değişiklik, bu Pathik büyük/küçük harf daha açık bir şekilde kullanıma sunularak daha iyi uygulama kodu daha

**Risk Azaltıcı Etkenler**

Bu hatayla karşılaşıldığında eylemin en uygun nedeni, kök nedenin anlaşılması ve çok sayıda iç hizmet sağlayıcısının oluşturulmasını durdurmaktır.
Ancak, hata `DbContextOptionsBuilder`yapılandırması aracılığıyla bir uyarıya (veya yoksayıldı) geri dönüştürülebilir.
Örneğin:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>HasOne/HasMany için tek bir dize ile çağrılan yeni davranış

[Sorun izleniyor #9171](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

**Eski davranış**

EF Core 3,0 öncesinde, tek bir dize ile `HasOne` veya `HasMany` kodu, kafa karıştırıcı bir şekilde yorumlandı.
Örneğin:
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

Kod, özel olabilecek `Entrance` gezinti özelliğini kullanarak `Samurai` başka bir varlık türüyle ilişkili olduğu gibi görünür.

Gerçekte, bu kod, gezinti özelliği olmayan `Entrance` adlı bazı varlık türlerine bir ilişki oluşturmaya çalışır.

**Yeni davranış**

EF Core 3,0 ' den itibaren, yukarıdaki kod artık daha önce yaptığımız gibi

**Kaydol**

Özellikle yapılandırma kodunu okurken ve hata ararken eski davranış çok karmaşıktır.

**Risk Azaltıcı Etkenler**

Bu, yalnızca tür adları için dizeler kullanarak ve gezinti özelliğini açıkça belirtmeden ilişkileri açıkça yapılandıran uygulamaları keser.
Bu, yaygın değildir.
Önceki davranış, gezinti özelliği adı için `null` açıkça geçirerek elde edilebilir.
Örneğin:

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a>Birkaç zaman uyumsuz yöntemin dönüş türü görevden ValueTask olarak değiştirildi

[Sorun izleniyor #15184](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

**Eski davranış**

Aşağıdaki zaman uyumsuz yöntemler daha önce bir `Task<T>`döndürdü:

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()` (ve türetme sınıfları)

**Yeni davranış**

Yukarıda bahsedilen yöntemler bundan sonra aynı `T` daha `ValueTask<T>` döndürür.

**Kaydol**

Bu değişiklik, bu yöntemler çağrılırken oluşan yığın ayırma sayısını azaltarak genel performansı geliştirir.

**Risk Azaltıcı Etkenler**

Yalnızca yukarıdaki API 'Leri bekleyen uygulamaların yeniden derlenmesi gerekiyor-kaynak değişikliği gerekli değildir.
Daha karmaşık bir kullanım (örn. döndürülen `Task` `Task.WhenAny()`geçirilmesi), genellikle döndürülen `ValueTask<T>` `AsTask()` çağırarak `Task<T>` dönüştürmesini gerektirir.
Bunun, bu değişikliğin getirdiği ayırma azaltmasını geçersiz hale getirdiğine unutmayın.

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>Ilişkisel: TypeMapping ek açıklaması artık yalnızca TypeMapping

[Sorun izleniyor #9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

**Eski davranış**

Tür eşleme ek açıklaması için ek açıklama adı "Ilişkisel: TypeMapping" idi.

**Yeni davranış**

Tür eşleme ek açıklaması için ek açıklama adı artık "TypeMapping" olur.

**Kaydol**

Tür eşlemeleri artık yalnızca ilişkisel veritabanı sağlayıcılarının daha fazlası için kullanılır.

**Risk Azaltıcı Etkenler**

Bu, yalnızca tür eşlemesine doğrudan bir ek açıklama olarak erişen uygulamaları keser. Bu, yaygın olmayan bir şekilde yapılır.
Düzeltilmesi gereken en uygun eylem, ek açıklamayı doğrudan kullanmak yerine tür eşlemelere erişmek için API yüzeyini kullanmaktır.

### <a name="totable-on-a-derived-type-throws-an-exception"></a>Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur 

[Sorun izleniyor #11811](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

**Eski davranış**

EF Core 3,0 ' dan önce, yalnızca devralma eşleme stratejisi bu geçerli olmayan bir TPH olduğundan, türetilmiş bir tür üzerinde çağrılan `ToTable()` yok sayılır. 

**Yeni davranış**

EF Core 3,0 ' den başlayarak ve daha sonraki bir sürümde TPT ve TPC desteği ekleme hazırlığı sırasında, bir türetilmiş tür üzerinde çağrılan `ToTable()` artık gelecekte beklenmedik bir eşleme değişikliğini önlemek için bir özel durum oluşturacak.

**Kaydol**

Şu anda türetilmiş bir türü farklı bir tabloya eşlemek için geçerli değildir.
Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda daha sonra bozmadan kaçınır.

**Risk Azaltıcı Etkenler**

Türetilmiş türleri diğer tablolarla eşleme girişimlerini kaldırın.

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a>Forsqlserverhasındex, HasIndex ile değiştirilmiştir 

[Sorun izleniyor #12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

**Eski davranış**

EF Core 3,0 ' dan önce `ForSqlServerHasIndex().ForSqlServerInclude()` `INCLUDE`ile kullanılan sütunları yapılandırmak için bir yol sağladı.

**Yeni davranış**

EF Core 3,0 ' den itibaren, bir dizinde `Include` kullanmak artık ilişkisel düzeyde destekleniyor.
`HasIndex().ForSqlServerInclude()`kullanın.

**Kaydol**

Bu değişiklik, tüm veritabanı sağlayıcıları için `Include` olan dizinler için API 'yi tek bir yerde birleştirmek üzere yapılmıştır.

**Risk Azaltıcı Etkenler**

Yukarıda gösterildiği gibi yeni API 'yi kullanın.

### <a name="metadata-api-changes"></a>Meta veri API 'SI değişiklikleri

[Sorun izleniyor #214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

**Yeni davranış**

Aşağıdaki özellikler genişletme yöntemlerine dönüştürüldü:

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

**Kaydol**

Bu değişiklik, belirtilen arabirimlerin uygulanmasını basitleştirir.

**Risk Azaltıcı Etkenler**

Yeni uzantı yöntemlerini kullanın.

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a>Sağlayıcıya özel meta veri API 'SI değişiklikleri

[Sorun izleniyor #214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

**Yeni davranış**

Sağlayıcıya özgü uzantı yöntemleri düzleştirilecektir:

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

**Kaydol**

Bu değişiklik, belirtilen genişletme yöntemlerinin uygulanmasını basitleştirir.

**Risk Azaltıcı Etkenler**

Yeni uzantı yöntemlerini kullanın.

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>EF Core, SQLite FK zorlaması için artık pragma göndermez

[Sorun izleniyor #12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

**Eski davranış**

3,0 EF Core önce, bir SQLite bağlantısı açıldığında EF Core `PRAGMA foreign_keys = 1` gönderebilirim.

**Yeni davranış**

EF Core 3,0 ' den başlayarak, bir SQLite bağlantısı açıldığında EF Core artık `PRAGMA foreign_keys = 1` göndermez.

**Kaydol**

Bu değişiklik, EF Core varsayılan olarak `SQLitePCLRaw.bundle_e_sqlite3` kullandığından, bu, sırasıyla FK zorlamasının varsayılan olarak açık olduğu ve bağlantı her açıldığında açık bir şekilde etkinleştirilmesi gerekmediği anlamına gelir.

**Risk Azaltıcı Etkenler**

EF Core için varsayılan olarak kullanılan SQLitePCLRaw. bundle_e_sqlite3 öğesinde yabancı anahtarlar varsayılan olarak etkinleştirilir.
Diğer durumlarda, yabancı anahtarlar bağlantı dizeniz `Foreign Keys=True` belirtilerek etkinleştirilebilir.

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a>Microsoft. EntityFrameworkCore. SQLite artık SQLitePCLRaw. bundle_e_sqlite3 bağımlıdır

**Eski davranış**

3,0 EF Core önce EF Core `SQLitePCLRaw.bundle_green`kullanılır.

**Yeni davranış**

EF Core 3,0 ' den başlayarak EF Core `SQLitePCLRaw.bundle_e_sqlite3`kullanır.

**Kaydol**

Bu değişiklik, iOS üzerinde kullanılan SQLite sürümünün diğer platformlarla tutarlı olması için yapılmıştır.

**Risk Azaltıcı Etkenler**

İOS 'ta yerel SQLite sürümünü kullanmak için `Microsoft.Data.Sqlite` farklı bir `SQLitePCLRaw` paketi kullanacak şekilde yapılandırın.

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a>GUID değerleri artık SQLite üzerinde metın olarak depolanır

[Sorun izleniyor #15078](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

**Eski davranış**

GUID değerleri daha önce SQLite üzerinde BLOB değerleri olarak depolandı.

**Yeni davranış**

GUID değerleri artık metın olarak depolanır.

**Kaydol**

GUID 'lerin ikili biçimi standartlaştırılmış değildir. Değerlerin metın olarak depolanması, veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.

**Risk Azaltıcı Etkenler**

Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

Microsoft. Data. SQLite, hem BLOB hem de metın sütunlarından GUID değerlerini okuyabilme yeteneğine sahiptir; Ancak, parametrelerin ve sabitlerin varsayılan biçimi değiştiğinden, büyük olasılıkla GUID 'Leri içeren çoğu senaryo için işlem yapmanız gerekir.

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Char değerleri artık SQLite üzerinde metın olarak depolanır

[Sorun izleniyor #15020](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

**Eski davranış**

Char değerleri daha önce SQLite üzerinde tamsayı değerleri olarak sokmıştı. Örneğin, *a* 'nın char değeri 65 tamsayı değeri olarak depolandı.

**Yeni davranış**

Char değerleri artık metın olarak depolanır.

**Kaydol**

Değerlerin metın olarak depolanması daha doğal hale gelir ve veritabanının diğer teknolojilerle daha uyumlu olmasını sağlar.

**Risk Azaltıcı Etkenler**

Aşağıdaki gibi SQL 'i yürüterek mevcut veritabanlarını yeni biçime geçirebilirsiniz.

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

EF Core ' de, bu özelliklerde bir değer Dönüştürücüsü yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

Microsoft. Data. SQLite Ayrıca tamsayı ve metın sütunlarından karakter değerlerini okuyabilme yeteneğine sahiptir, bu nedenle bazı senaryolar herhangi bir işlem gerektirmeyebilir.

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a>Geçiş kimlikleri artık sabit kültürün takvimi kullanılarak oluşturulmuştur

[Sorun izleniyor #12978](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

**Eski davranış**

Geçiş kimlikleri, geçerli kültürün takvimi kullanılarak yanlışlıkla oluşturulmuştur.

**Yeni davranış**

Geçiş kimlikleri artık her zaman sabit kültürün takvimi (Gregoryen) kullanılarak oluşturulmuştur.

**Kaydol**

Veritabanının güncelleştirilmesi veya birleştirme çakışmalarını çözmek için geçişlerin sırası önemlidir. Sabit takvimin kullanılması, takım üyelerinden farklı sistem takvimlerine neden olan sorunları sıralamayı önler.

**Risk Azaltıcı Etkenler**

Bu değişiklik, yılın Gregoryen takvimden büyük olduğu Gregoryen olmayan bir takvim kullanan herkesi etkiler (Tay dili Budist takvimi gibi). Yeni geçişlerin mevcut geçişlerden sonra sıralanabilmesi için mevcut geçiş kimliklerinin güncellenmesi gerekir.

Geçiş KIMLIĞI, geçişler ' tasarımcı dosyalarındaki geçiş özniteliğinde bulunabilir.

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

Geçişler geçmiş tablosunun da güncelleştirilmesi gerekir.

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a>UseRowNumberForPaging kaldırıldı

[Sorun izleniyor #16400](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

**Eski davranış**

EF Core 3,0 ' dan önce `UseRowNumberForPaging`, SQL Server 2008 ile uyumlu sayfalama için SQL oluşturmak üzere kullanılabilir.

**Yeni davranış**

EF Core 3,0 ' den başlayarak, EF yalnızca daha sonraki SQL Server sürümlerle uyumlu olan sayfalama için SQL oluşturur. 

**Kaydol**

[SQL Server 2008 artık desteklenen bir ürün](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) olmadığından ve bu özelliğin EF Core 3,0 ' de yapılan sorgu değişiklikleriyle çalışacak şekilde güncelleştirilmesi önemli bir çalışmadır çünkü bu değişikliği yapıyoruz.

**Risk Azaltıcı Etkenler**

Oluşturulan SQL 'in desteklenmesi için SQL Server daha yeni bir sürüme veya daha yüksek bir uyumluluk düzeyi kullanarak güncelleştirmenizi öneririz. Bu, bunu yapamamanızın ardından, Ayrıntılar için lütfen [izleme sorunu hakkında yorum](https://github.com/aspnet/EntityFrameworkCore/issues/16400) yapın. Bu kararı geri bildirime göre geri ziyaret edebilirsiniz.

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a>Uzantı bilgisi/meta veriler ıdbcontextoptionsextenı' den kaldırıldı

[Sorun izleniyor #16119](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

**Eski davranış**

`IDbContextOptionsExtension`, uzantı hakkında meta veriler sağlamak için yöntemler içeriyordu.

**Yeni davranış**

Bu yöntemler yeni bir `IDbContextOptionsExtension.Info` özelliğinden döndürülen yeni bir `DbContextOptionsExtensionInfo` soyut taban sınıfına taşınmıştır.

**Kaydol**

2,0 ' den 3,0 ' e kadar olan yayınlar için bu yöntemlere birkaç kez ekleme veya bu yöntemleri değiştirme gerekiyordu.
Bunları yeni bir soyut taban sınıfına bölmek, var olan uzantıları bozmadan bu tür değişiklikleri daha kolay hale getirir.

**Risk Azaltıcı Etkenler**

Yeni kalıbı izlemek için uzantıları güncelleştirin.
EF Core kaynak kodundaki farklı tür uzantılara yönelik `IDbContextOptionsExtension` birçok uygulamada örnek bulunur.

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a>LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı

[Sorun izleniyor #10985](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

**Değişebilir**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`olarak yeniden adlandırıldı.

**Kaydol**

Bu uyarı olayının adlandırmasını diğer tüm uyarı olaylarıyla hizalar.

**Risk Azaltıcı Etkenler**

Yeni adı kullanın. (Olay KIMLIĞI numarasının değiştirilmediğini unutmayın.)

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a>Yabancı anahtar kısıtlama adları için API 'YI belirginleştirme

[Sorun izleniyor #10730](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

**Eski davranış**

3,0 EF Core önce, yabancı anahtar kısıtlama adlarına yalnızca "ad" adı verilir. Örneğin:

```csharp
var constraintName = myForeignKey.Name;
```

**Yeni davranış**

EF Core 3,0 ' den başlayarak, yabancı anahtar kısıtlama adları artık "kısıtlama adı" olarak anılacaktır. Örneğin:

```csharp
var constraintName = myForeignKey.ConstraintName;
```

**Kaydol**

Bu değişiklik, bu alandaki adlandırma tutarlılığını sağlar ve ayrıca bu, yabancı anahtar kısıtlamasının adı ve yabancı anahtarın tanımlandığı sütun veya özellik adı değil, yabancı anahtar kısıtlaması olduğunu da açıklar.

**Risk Azaltıcı Etkenler**

Yeni adı kullanın.

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a>Irelationaldatabasecreator. HasTables/HasTablesAsync genel kullanıma açıldı

[Sorun izleniyor #15997](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

**Eski davranış**

3,0 EF Core önce bu yöntemler korundu.

**Yeni davranış**

EF Core 3,0 ' den itibaren bu yöntemler geneldir.

**Kaydol**

Bu yöntemler, bir veritabanının oluşturulup oluşturulmadığını ve boş olduğunu anlamak için EF tarafından kullanılır. Bu, geçiş uygulanıp uygulanmadığını belirlemek için EF dışından da yararlı olabilir.

**Risk Azaltıcı Etkenler**

Herhangi bir geçersiz kılmanın erişilebilirliğini değiştirin.

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a>Microsoft. EntityFrameworkCore. Design artık bir DevelopmentDependency paketi

[Sorun izleniyor #11506](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

**Eski davranış**

EF Core 3,0 tarihinden önce, Microsoft. EntityFrameworkCore. Design, derlemeye bağımlı olan projeler tarafından başvurulabilen düzenli bir NuGet paketidir.

**Yeni davranış**

EF Core 3,0 ' den başlayarak, bu bir DevelopmentDependency paketidir. Bu, bağımlılığın diğer projelere geçişli olarak akamayacağı ve artık varsayılan olarak kendi derlemesine başvurmayabileceği anlamına gelir.

**Kaydol**

Bu paket yalnızca tasarım zamanında kullanılmak üzere tasarlanmıştır. Dağıtılan uygulamalar buna başvurmamalıdır. Paketi bir DevelopmentDependency hale getirmek, bu öneriyi yeniden zorlar.

**Risk Azaltıcı Etkenler**

EF Core tasarım zamanı davranışını geçersiz kılmak için bu pakete başvurmanız gerekiyorsa, projenizdeki PackageReference öğe meta verilerini güncelleştirebilirsiniz.

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

Pakete Microsoft. EntityFrameworkCore. Tools aracılığıyla doğrudan başvuruluyorsa, meta verilerini değiştirmek için pakete açık bir PackageReference eklemeniz gerekir. Bu tür bir açık başvuru, paketteki türlerin gerekli olduğu herhangi bir projeye eklenmelidir.

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a>SQLitePCL. RAW, 2.0.0 sürümüne güncelleştirildi

[Sorun izleniyor #14824](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

**Eski davranış**

Microsoft. EntityFrameworkCore. SQLite daha önce SQLitePCL. RAW sürümüne bağımlı.

**Yeni davranış**

Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.

**Kaydol**

SQLitePCL. RAW hedeflerinin sürümü 2.0.0 .NET Standard 2,0. Daha önce .NET Standard, geçişli paketlerin büyük bir kapanışının çalışmasını gerektiren 1,1 ' i hedefledi.

**Risk Azaltıcı Etkenler**

SQLitePCL. Raw sürüm 2.0.0 bazı önemli değişiklikler içerir. Ayrıntılar için [sürüm notlarına](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) bakın.

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a>Nettopologyısuite, 2.0.0 sürümüne güncelleştirildi

[Sorun izleniyor #14825](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

**Eski davranış**

Uzamsal paketler daha önce Nettopologyısuite 1.15.1 sürümüne bağımlı.

**Yeni davranış**

Paketinizin 2.0.0 sürümüne bağlı olarak paketimizi güncelleştirdik.

**Kaydol**

EF Core kullanıcıların karşılaştığı çeşitli kullanılabilirlik sorunlarını gidermek için nettopologyısuite amaçlar 'nin sürüm 2.0.0.

**Risk Azaltıcı Etkenler**

Nettopologyısuite sürüm 2.0.0 bazı önemli değişiklikler içerir. Ayrıntılar için [sürüm notlarına](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) bakın.

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a>System. Data. SqlClient yerine Microsoft. Data. SqlClient kullanılır

[Sorun izleniyor #15636](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

**Eski davranış**

Microsoft. EntityFrameworkCore. SqlServer daha önce System. Data. SqlClient 'a bağımlı.

**Yeni davranış**

Paketimizi Microsoft. Data. SqlClient 'e göre güncelleştirdik.

**Kaydol**

Microsoft. Data. SqlClient, SQL Server ileriye dönük tanıtım verileri erişim sürücüsüdür ve System. Data. SqlClient artık geliştirme odağı değildir.
Always Encrypted gibi bazı önemli özellikler yalnızca Microsoft. Data. SqlClient ile kullanılabilir.

**Risk Azaltıcı Etkenler**

Kodunuz System. Data. SqlClient üzerinde doğrudan bir bağımlılık alırsa bunun yerine Microsoft. Data. SqlClient öğesine başvuracak şekilde değiştirmeniz gerekir; iki paket çok yüksek düzeyde API uyumluluğu korudıkça, bu yalnızca basit bir paket ve ad alanı değişikliği olmalıdır.

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a>Birden çok belirsiz kendine başvuran ilişki yapılandırılması gerekiyor 

[Sorun izleniyor #13573](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

**Eski davranış**

Birden çok kendine başvuran tek yönlü gezinti özelliklerine ve eşleşen FKs 'e sahip bir varlık türü yanlış bir ilişki olarak yapılandırılmış. Örneğin:

```csharp
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

**Yeni davranış**

Bu senaryo artık model oluşturma bölümünde algılanır ve modelin belirsiz olduğunu belirten bir özel durum atılır.

**Kaydol**

Sonuç modeli belirsizdir ve genellikle bu durum için yanlış olur.

**Risk Azaltıcı Etkenler**

İlişkinin tam yapılandırmasını kullanın. Örneğin:

```csharp
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a>DbFunction. Schema null ya da boş dize, modeli varsayılan şemasında olacak şekilde yapılandırır

[Sorun izleniyor #12757](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

**Eski davranış**

Şema ile boş bir dize olarak yapılandırılmış bir DbFunction, şema olmadan yerleşik işlev olarak değerlendirildi. Örneğin, aşağıdaki kod `DatePart` CLR işlevini SqlServer üzerinde `DATEPART` yerleşik işlevine eşleyecek.

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

**Yeni davranış**

Tüm DbFunction eşlemeleri Kullanıcı tanımlı işlevlere eşlenildiği kabul edilir. Bu nedenle boş dize değeri, işlevi model için varsayılan şemanın içine yerleştirir. Şema, Fluent API `modelBuilder.HasDefaultSchema()` aracılığıyla açıkça yapılandırılabilir veya `dbo` Aksi takdirde.

**Kaydol**

Daha önceden şemanın boş olması, işlevin yerleşik olduğunu değerlendirmek için bir yoldur, ancak bu mantık yalnızca yerleşik işlevlerin herhangi bir şemaya ait olmadığı SqlServer için geçerlidir.

**Risk Azaltıcı Etkenler**

DbFunction 'ın çevirisini yerleşik bir işlevle eşlemek için el ile yapılandırın.

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
