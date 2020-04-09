---
title: EF Core 3.0'daki son dakika değişiklikleri - EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 6e0c17a22b56b206f18e47f678e3e237d5c42375
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417463"
---
# <a name="breaking-changes-included-in-ef-core-30"></a>EF Core 3.0'da yer alan son kesme değişiklikleri

Aşağıdaki API ve davranış değişiklikleri, varolan uygulamaları 3.0.0'a yükselterken kırma potansiyeline sahiptir.
Yalnızca veritabanı sağlayıcılarını etkilemesini beklediğimiz [değişiklikler sağlayıcı değişiklikleri](xref:core/providers/provider-log)altında belgelenir.

## <a name="summary"></a>Özet

| **Son dakika değişikliği**                                                                                               | **Etki** |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [LINQ sorguları artık istemciüzerinde değerlendirilemez](#linq-queries-are-no-longer-evaluated-on-the-client)         | Yüksek       |
| [EF Core 3.0 hedefleri .NET Standart 2.1 yerine .NET Standart 2.0](#netstandard21) | Yüksek      |
| [EF Core komut satırı aracı, dotnet ef, artık .NET Core SDK'nın bir parçası değil](#dotnet-ef) | Yüksek      |
| [DetectChanges, mağaza tarafından oluşturulan anahtar değerleri onurlandırıyor](#dc) | Yüksek      |
| [FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı](#fromsql) | Yüksek      |
| [Sorgu türleri varlık türleri ile birleştirilir](#qt) | Yüksek      |
| [Entity Framework Core artık ASP.NET Core paylaşılan çerçevesinin bir parçası değildir](#no-longer) | Orta      |
| [Basamaklı silme ler artık varsayılan olarak hemen gerçekleşir](#cascade) | Orta      |
| [İlgili varlıkların hevesle yüklenmesi artık tek bir sorguda gerçekleşir](#eager-loading-single-query) | Orta      |
| [DeleteBehavior.Restrict temiz semantik var](#deletebehavior) | Orta      |
| [Sahip olunan tür ilişkileri için yapılandırma API'sı değişti](#config) | Orta      |
| [Her özellik, bağımsız bellek tümseci anahtar oluşturma](#each) | Orta      |
| [Sorguları izlememe artık kimlik çözümlemesi gerçekleştir](#notrackingresolution) | Orta      |
| [Meta veri API değişiklikleri](#metadata-api-changes) | Orta      |
| [Sağlayıcıya özel Meta veri API değişiklikleri](#provider) | Orta      |
| [UseRowNumberForPaging kaldırıldı](#urn) | Orta      |
| [FromSql yöntemi depolanan yordam ile kullanıldığında oluşturulamıyor](#fromsqlsproc) | Orta      |
| [FromSql yöntemleri yalnızca sorgu köklerinde belirtilebilir](#fromsql) | Düşük      |
| [~~Sorgu yürütme Hata Ayıklama düzeyinde günlüğe kaydedilir~~ Döndürülür](#qe) | Düşük      |
| [Geçici anahtar değerleri artık varlık örneklerine ayarlı değil](#tkv) | Düşük      |
| [Tabloyu anaparayla paylaşan bağımlı varlıklar artık isteğe bağlıdır](#de) | Düşük      |
| [Eşzamanlılık belirteç sütunu olan bir tabloyu paylaşan tüm varlıklar tabloyu bir özellik ile eşlene](#aes) | Düşük      |
| [Sahibi izleme sorgusu kullanmadan sahip olunan varlıklar sorgulanamaz](#owned-query) | Düşük      |
| [Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütuna eşlenir](#ip) | Düşük      |
| [Yabancı anahtar mülkiyet sözleşmesi artık ana özellik ile aynı adla eşleşmiş](#fkp) | Düşük      |
| [İşlemKapsamı tamamlanmadan önce artık kullanılmazsa veritabanı bağlantısı artık kapatılır](#dbc) | Düşük      |
| [Destek alanları varsayılan olarak kullanılır](#backing-fields-are-used-by-default) | Düşük      |
| [Birden çok uyumlu destek alanı bulunursa at](#throw-if-multiple-compatible-backing-fields-are-found) | Düşük      |
| [Yalnızca alan özelliği adları alan adı ile eşleşmelidir](#field-only-property-names-should-match-the-field-name) | Düşük      |
| [AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache arama](#adddbc) | Düşük      |
| [AddEntityFramework* boyut sınırıyla IMemoryCache ekler](#addentityframework-adds-imemorycache-with-a-size-limit) | Düşük      |
| [DbContext.Entry şimdi yerel bir DetectChanges gerçekleştirir](#dbe) | Düşük      |
| [String ve bayt dizi anahtarları varsayılan olarak istemci tarafından oluşturulmaz](#string-and-byte-array-keys-are-not-client-generated-by-default) | Düşük      |
| [ILoggerFactory artık kapsamlı bir hizmettir](#ilf) | Düşük      |
| [Tembel yükleme lisi artık navigasyon özelliklerinin tamamen yüklendiğini varsaymaz](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | Düşük      |
| [Dahili hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | Düşük      |
| [HasOne/HasMany için yeni davranış tek bir dize ile çağrıldı](#nbh) | Düşük      |
| [Birkaç async yönteminin dönüş türü Görevden ValueTask'a değiştirildi](#rtnt) | Düşük      |
| [İlişkisel: TypeMapping ek açıklama şimdi sadece TypeMapping olduğunu](#rtt) | Düşük      |
| [Türetilmiş bir türde ToTable bir özel durum atar](#totable-on-a-derived-type-throws-an-exception) | Düşük      |
| [EF Core artık SQLite FK uygulama için pragma gönderir](#pragma) | Düşük      |
| [Microsoft.EntityFrameworkCore.Sqlite şimdi SQLitePCLRaw.bundle_e_sqlite3 bağlıdır](#sqlite3) | Düşük      |
| [Kılavuz değerleri artık SQLite'da METİn olarak depolanır](#guid) | Düşük      |
| [Char değerleri artık SQLite'da TEXT olarak depolanır](#char) | Düşük      |
| [Geçiş disleri artık değişmez kültürün takvimi kullanılarak oluşturulur](#migid) | Düşük      |
| [Uzantı bilgileri/meta veriler IDbContextOptionsExtension kaldırıldı](#xinfo) | Düşük      |
| [LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı](#lqpe) | Düşük      |
| [Yabancı anahtar kısıtlama adları için API'yi netleştir](#clarify) | Düşük      |
| [iRelationalDatabaseCreator.HasTables/HasTablesAsync genel kullanıma açıklanmıştır](#irdc2) | Düşük      |
| [Microsoft.EntityFrameworkCore.Design artık bir DevelopmentDependency paketidir](#dip) | Düşük      |
| [SQLitePCL.raw sürüm 2.0.0 güncellendi](#SQLitePCL) | Düşük      |
| [NetTopologySuite sürüm 2.0.0 güncellendi](#NetTopologySuite) | Düşük      |
| [System.Data.SqlClient yerine Microsoft.Data.SqlClient kullanılır](#SqlClient) | Düşük      |
| [Birden çok belirsiz kendi kendine başvuran ilişkiler yapılandırılmalıdır](#mersa) | Düşük      |
| [DbFunction.Schema null veya boş dize olmak modelin varsayılan şema olarak yapılandırır](#udf-empty-string) | Düşük      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>LINQ sorguları artık istemciüzerinde değerlendirilemez

[İzleme Sorunu #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Ayrıca #12795 sorunu da görme](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

**Eski davranış**

3.0'dan önce, EF Core sorgunun parçası olan bir ifadeyi SQL veya parametreye dönüştüremediğinde, istemcideki ifadeyi otomatik olarak değerlendirir.
Varsayılan olarak, olası pahalı ifadelerin istemci değerlendirmesi yalnızca bir uyarı tetikledi.

**Yeni davranış**

3.0 ile başlayan EF Core, yalnızca üst düzey projeksiyondaki `Select()` (sorgudaki son çağrı) ifadelerin istemci üzerinde değerlendirilmesine izin verir.
Sorgunun başka bir bölümündeki ifadeler SQL veya parametreye dönüştürülemediğinde bir özel durum atılır.

**Neden**

Sorguların otomatik istemci değerlendirmesi, önemli bölümleri çevrilemese bile birçok sorgunun yürütülmesini sağlar.
Bu davranış, beklenmeyen ve zarar verici davranışlara neden olabilir ve bu davranış yalnızca üretimde belirginleşebilir.
Örneğin, çevrilmeyen `Where()` bir aramadaki bir koşul, tablodaki tüm satırların veritabanı sunucusundan aktarılmasına ve filtrenin istemciye uygulanmasına neden olabilir.
Tablo geliştirme aşamasında yalnızca birkaç satır içeriyorsa, ancak uygulama üretime geçtiğinde tablo milyonlarca satır içerebileceği bu durum kolayca algılanmayabilir.
İstemci değerlendirme uyarıları da geliştirme sırasında göz ardı etmek çok kolay olduğunu kanıtladı.

Bunun yanı sıra, otomatik istemci değerlendirmesi, belirli ifadeler için sorgu çevirisinin iyileştirilmesinin sürümler arasında istenmeyen kırılma değişikliklerine neden olduğu sorunlara yol açabilir.

**Risk Azaltıcı Etkenler**

Bir sorgu tam olarak çevrilemezse, sorguyu çevrilebilecek bir biçimde yeniden yazın `AsEnumerable()`veya `ToList()`linq-to-Objects kullanılarak işlenebilir şekilde istemciye açıkça veri getirmek için benzer bir şekilde kullanın.

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a>EF Core 3.0 hedefleri .NET Standart 2.1 yerine .NET Standart 2.0

[İzleme Sorunu #15498](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

> [!IMPORTANT] 
> EF Core 3.1 hedefleri .NET Standart 2.0 tekrar. Bu da .NET Framework için desteği geri getirir.

**Eski davranış**

3.0'dan önce EF Core .NET Standard 2.0'ı hedefledi ve .NET Framework de dahil olmak üzere bu standardı destekleyen tüm platformlarda çalışacaktı.

**Yeni davranış**

3.0 ile başlayan EF Core, .NET Standard 2.1'i hedefler ve bu standardı destekleyen tüm platformlarda çalışır. Bu ,NET Framework içermez.

**Neden**

Bu, .NET teknolojileri arasında enerjiyi .NET Core ve Xamarin gibi diğer modern .NET platformlarına odaklama kararının bir parçasıdır.

**Risk Azaltıcı Etkenler**

EF Core 3.1 kullanın.

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>Entity Framework Core artık ASP.NET Core paylaşılan çerçevesinin bir parçası değildir

[Takip Sorunu Duyurular#325](https://github.com/aspnet/Announcements/issues/325)

**Eski davranış**

Core 3.0ASP.NET önce, bir paket `Microsoft.AspNetCore.App` başvurusu `Microsoft.AspNetCore.All`eklediğinizde veya , EF Core ve SQL Server sağlayıcısı gibi EF Core veri sağlayıcılarının bazılarını içerir.

**Yeni davranış**

3.0'dan itibaren, ASP.NET Core paylaşılan çerçevesi EF Core veya herhangi bir EF Core veri sağlayıcısını içermez.

**Neden**

Bu değişiklikten önce, EF Core almak, uygulamanın Core ve SQL Server ASP.NET hedeflenip hedeflemediğine bağlı olarak farklı adımlar gerektir. Ayrıca, ASP.NET Core yükseltme her zaman arzu değildir EF Core ve SQL Server sağlayıcısı, yükseltme zorladı.

Bu değişiklikle, EF Core alma deneyimi tüm sağlayıcılar, desteklenen .NET uygulamaları ve uygulama türleri arasında aynıdır.
Geliştiriciler artık EF Core ve EF Core veri sağlayıcılarının ne zaman yükseltilmelerini de tam olarak denetleyebilir.

**Risk Azaltıcı Etkenler**

EF Core'u ASP.NET Core 3.0 uygulamasında veya desteklenen başka bir uygulamada kullanmak için, uygulamanızın kullanacağı EF Core veritabanı sağlayıcısına açıkça bir paket başvurusu ekleyin.

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a>EF Core komut satırı aracı, dotnet ef, artık .NET Core SDK'nın bir parçası değil

[İzleme Sorunu #14016](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

**Eski davranış**

3.0'dan `dotnet ef` önce, araç .NET Core SDK'ya dahil edildi ve ek adımlar gerektirmeden herhangi bir projeden komut satırından kullanıma hazır dı. 

**Yeni davranış**

3.0'dan başlayarak ,NET SDK `dotnet ef` aracı içermez, bu nedenle kullanmadan önce aracı açıkça yerel veya genel bir araç olarak yüklemeniz gerekir. 

**Neden**

Bu değişiklik, EF Core `dotnet ef` 3.0'ın her zaman Bir NuGet paketi olarak dağıtılması yla tutarlı olarak NuGet'de düzenli bir .NET CLI aracı olarak dağıtmamıza ve güncellememize olanak tanır.

**Risk Azaltıcı Etkenler**

Geçişleri veya iskeleyi `DbContext`yönetebilmek için `dotnet-ef` genel bir araç olarak yükleyin:

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

Ayrıca, bir [araç bildirimi dosyası](https://github.com/dotnet/cli/issues/10288)kullanarak bir araç bağımlılığı olarak bildiren bir projenin bağımlılıklarını geri yüklediğinizde, bunu yerel bir araç da elde edebilirsiniz.

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a>FromSql, ExecuteSql ve ExecuteSqlAsync yeniden adlandırıldı

[İzleme Sorunu #10996](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

**Eski davranış**

EF Core 3.0'dan önce, bu yöntem adları normal bir dize yle veya SQL ve parametrelere enterpolasyon yapılması gereken bir dizeyle çalışmak üzere aşırı yüklendi.

**Yeni davranış**

EF Core 3.0 ile `FromSqlRaw` `ExecuteSqlRaw`başlayarak, parametrelerin sorgu dizesinden ayrı olarak geçirildiği parametreli bir sorgu kullanın. `ExecuteSqlRawAsync`
Örneğin:

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

Ve `FromSqlInterpolated` `ExecuteSqlInterpolated`parametrelerin `ExecuteSqlInterpolatedAsync` enterpolasyonlu sorgu dizesinin bir parçası olarak geçtiği parametreli bir sorgu oluşturmak için kullanın.
Örneğin:

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

Yukarıdaki sorguların her ikisinin de aynı SQL parametreleri ile aynı parametreli SQL üreteceğini unutmayın.

**Neden**

Bu gibi yöntem aşırı yüklemeleri, amaç enterpolasyonlu dize yöntemini aramak ken, ham dize yöntemini yanlışlıkla aramayı çok kolaylaştırır.
Bu, sorguların olması gerekirken parametrelendirilemelerine neden olabilir.

**Risk Azaltıcı Etkenler**

Yeni yöntem adlarını kullanmak için geçin.

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a>FromSql yöntemi depolanan yordam ile kullanıldığında oluşturulamıyor

[İzleme Sorunu #15392](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

**Eski davranış**

EF Core 3.0'dan önce FromSql yöntemi, geçirilen SQL'in üzerine besteleilip oluşturulamaya çalışılsın. SQL depolanan bir yordam gibi tek birleştirilebilir olmadığında istemci değerlendirmesi yaptı. Aşağıdaki sorgu, depolanan yordamı sunucuda çalıştırarak ve istemci tarafında FirstOrDefault yaparak çalıştı.

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

**Yeni davranış**

EF Core 3.0 ile başlayarak, EF Core SQL ayrıştırmaya çalışmaz. Yani FromSqlRaw/FromSqlInterpolated'den sonra beste yapacaksanız, EF Core alt sorguya neden olarak SQL'i oluşturur. Bu nedenle, kompozisyonlu depolanmış bir yordam kullanıyorsanız, geçersiz SQL sözdizimi için bir özel durum alırsınız.

**Neden**

EF Core 3.0 otomatik istemci değerlendirmesini desteklemez, çünkü [burada](#linq-queries-are-no-longer-evaluated-on-the-client)açıklandığı gibi hataya yatkındır.

**Risk azaltma**

FromSqlRaw/FromSqlInterpolated'de depolanmış bir yordam kullanıyorsanız, bunun üzerine oluşturulamayacağını biliyorsunuz, bu nedenle sunucu tarafında herhangi bir kompozisyonu önlemek için FromSql yöntemi çağrısından hemen sonra __AsEnumerable/AsAsyncEnumerable'ı__ ekleyebilirsiniz.

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a>FromSql yöntemleri yalnızca sorgu köklerinde belirtilebilir

[İzleme Sorunu #15704](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

**Eski davranış**

EF Core 3.0'dan `FromSql` önce, yöntem sorgunun herhangi bir yerinde belirtilebilir.

**Yeni davranış**

EF Core 3.0 ile `FromSqlRaw` başlayarak, yeni ve `FromSqlInterpolated` yöntemler (yerine) `FromSql`sadece sorgu kökleri, `DbSet<>`yani doğrudan belirtilebilir . Bunları başka bir yerde belirtmeye çalışmak bir derleme hatasına neden olur.

**Neden**

Başka `FromSql` bir `DbSet` yerde belirtmenin hiçbir anlamı veya katma değeri yoktur ve belirli senaryolarda belirsizliğe neden olabilir.

**Risk Azaltıcı Etkenler**

`FromSql`çağrıları doğrudan başvurdukları yer `DbSet` olacak şekilde taşınmalıdır.

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a>Sorguları izlememe artık kimlik çözümlemesi gerçekleştir

[İzleme Sorunu #13518](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

**Eski davranış**

EF Core 3.0'dan önce, belirli bir tür ve kimlikle bir varlığın her oluşumu için aynı varlık örneği kullanılır. Bu, sorguları izleme davranışıyla eşleşir. Örneğin, bu sorgu:

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
verilen kategoriyle `Category` ilişkili `Product` her biri için aynı örneği döndürecek.

**Yeni davranış**

EF Core 3.0 ile başlayarak, belirli bir türe ve kimliğine sahip bir varlık döndürülen grafikte farklı yerlerde karşılaşıldığında farklı varlık örnekleri oluşturulur. Örneğin, iki ürün aynı kategoriyle `Category` ilişkilendirilse bile, yukarıdaki sorgu artık her biri `Product` için yeni bir örnek döndürecektir.

**Neden**

Kimlik çözümlemesi (diğer bir şekilde, bir varlığın daha önce karşılaşılan bir varlıkla aynı tür ve kimliğe sahip olduğunu belirleme) ek performans ve bellek ek yükü ekler. Bu genellikle ilk etapta izleme sorgularının neden kullanıldığına ters çalışır. Ayrıca, kimlik çözümlemesi bazen yararlı olabilir, ancak varlıklar seri hale getirilecek ve sorguları izlememek için yaygın olan bir istemciye gönderilecekse gerekli değildir.

**Risk Azaltıcı Etkenler**

Kimlik çözümlemesi gerekiyorsa izleme sorgusu kullanın.

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a>~~Sorgu yürütme Hata Ayıklama düzeyinde günlüğe kaydedilir~~ Döndürülür

[İzleme Sorunu #14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

EF Core 3.0'daki yeni yapılandırma, uygulama tarafından belirtilecek herhangi bir olayın günlük düzeyine izin verdiği için bu değişikliği geri aldık. Örneğin, SQL'in günlüğe `Debug`kaydetmesini , açıkça `OnConfiguring` düzeyveya şu şekilde yapılandıracak şekilde değiştirmek için: `AddDbContext`
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>Geçici anahtar değerleri artık varlık örneklerine ayarlı değil

[İzleme Sorunu #12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

**Eski davranış**

EF Core 3.0'dan önce, daha sonra veritabanı tarafından oluşturulan gerçek bir değere sahip olacak tüm önemli özelliklere geçici değerler atanmıştır.
Genellikle bu geçici değerler büyük negatif sayılardı.

**Yeni davranış**

3.0 ile başlayan EF Core, varlığın izleme bilgilerinin bir parçası olarak geçici anahtar değerini depolar ve anahtar özelliğin kendisini değiştirmeden bırakır.

**Neden**

Bu değişiklik, daha önce bir `DbContext` örnek tarafından izlenen bir varlık farklı `DbContext` bir örneğe taşındığında geçici anahtar değerlerin hatalı bir şekilde kalıcı olmasını önlemek için yapılmıştır. 

**Risk Azaltıcı Etkenler**

Birincil anahtarlar varlıklar arasında ilişki kurmak için yabancı anahtarlara birincil anahtar değerleri atayabilen uygulamalar, birincil anahtarlar `Added` depoda oluşturulursa ve devletteki varlıklara aitse eski davranışa bağlı olabilir.
Bu, şu lar tarafından önlenebilir:
* Mağaza tarafından oluşturulan anahtarları kullanmamak.
* Gezinti özelliklerini yabancı anahtar değerlerini ayarlamak yerine ilişkiler oluşturacak şekilde ayarlama.
* Varlığın izleme bilgilerinden gerçek geçici anahtar değerlerini edinin.
Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue` kendisi ayarlanmış olsa `blog.Id` bile geçici değeri döndürür.

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a>DetectChanges, mağaza tarafından oluşturulan anahtar değerleri onurlandırıyor

[İzleme Sorunu #14616](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

**Eski davranış**

EF Core 3.0'dan önce, izlenmemiş bir varlık `DetectChanges` `Added` tarafından bulunan bir varlık durumda `SaveChanges` izlenir ve çağrıldığında yeni bir satır olarak eklenir.

**Yeni davranış**

EF Core 3.0 ile başlayarak, bir varlık oluşturulan anahtar değerleri kullanıyorsa ve bazı `Modified` anahtar değeri ayarlanırsa, varlık durumda izlenir.
Bu, varlık için bir satırın var olduğu varsayılır `SaveChanges` ve çağrıldığında güncelleştirilecektir.
Anahtar değeri ayarlanmıyorsa veya varlık türü oluşturulan anahtarları kullanmıyorsa, yeni varlık önceki sürümlerde `Added` olduğu gibi izlenir.

**Neden**

Bu değişiklik, mağaza tarafından oluşturulan anahtarları kullanırken bağlantısı kesilen varlık grafikleri ile çalışmayı daha kolay ve tutarlı hale getirmek için yapıldı.

**Risk Azaltıcı Etkenler**

Bir varlık türü oluşturulan anahtarları kullanmak üzere yapılandırılır, ancak anahtar değerleri açıkça yeni örnekler için ayarlanırsa, bu değişiklik bir uygulamayı bozabilir.
Düzeltme, anahtar özelliklerini oluşturulan değerleri kullanmamak için açıkça yapılandırmaktır.
Örneğin, akıcı API ile:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

Veya veri ek açıklamaları ile:

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a>Basamaklı silme ler artık varsayılan olarak hemen gerçekleşir

[İzleme Sorunu #10114](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

**Eski davranış**

3.0'dan önce, EF Core basamaklı eylemler uyguladı (gerekli bir asıl silindiğinde veya gerekli bir anaparayla ilişki kesildiğinde bağımlı varlıkları silme) SaveChanges çağrılana kadar gerçekleşmedi.

**Yeni davranış**

3.0 ile başlayarak, EF Core tetikleme durumu algılanır algılanır algılanmadı basamaklı eylemler uygular.
Örneğin, asıl `context.Remove()` varlığı silmek için arama yapmak, izlenen ilgili tüm `Deleted` bağımlıların da hemen ayarlanmasına neden olur.

**Neden**

Bu değişiklik, hangi varlıkların çağrılmadan _önce_ `SaveChanges` silineceğini anlamanın önemli olduğu veri bağlama ve denetleme senaryoları için deneyimi geliştirmek için yapılmıştır.

**Risk Azaltıcı Etkenler**

Önceki davranış, ''deki `context.ChangeTracker`ayarlar aracılığıyla geri yüklenebilir.
Örneğin:

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a>İlgili varlıkların hevesle yüklenmesi artık tek bir sorguda gerçekleşir

[İzleme sorunu #18022](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

**Eski davranış**

3.0'dan önce, operatörler `Include` aracılığıyla hevesle yüklenen koleksiyon gezintileri, ilişkili her varlık türü için bir tane olmak üzere ilişkisel veritabanında birden çok sorgu oluşturulmasına neden oldu.

**Yeni davranış**

3.0 ile başlayarak, EF Core ilişkisel veritabanlarında JO'larla tek bir sorgu oluşturur.

**Neden**

Tek bir LINQ sorgusuuygulamak için birden çok sorgu verilmesi, birden çok veritabanı roundtrips gerekli olduğu gibi negatif performans da dahil olmak üzere çok sayıda soruna neden oldu ve her sorgu veritabanının farklı bir durumu gözlemlemek gibi veri tutarlılık sorunları.

**Risk Azaltıcı Etkenler**

Teknik olarak bu bir kırılma değişikliği olmasa da, tek bir sorgu koleksiyon gezintilerinde çok `Include` sayıda işleç içerdiğinde uygulama performansı üzerinde önemli bir etkisi olabilir. Daha fazla bilgi ve sorguları daha verimli bir şekilde yeniden yazmak için [bu yoruma bakın.](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085)

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a>DeleteBehavior.Restrict temiz semantik var

[İzleme Sorunu #12661](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

**Eski davranış**

3.0'dan `DeleteBehavior.Restrict` önce, veritabanında `Restrict` anlamsal olarak yabancı anahtarlar oluşturuldu, ancak aynı zamanda iç düzeltmeyi de bariz olmayan bir şekilde değiştirdi.

**Yeni davranış**

3.0 ile `DeleteBehavior.Restrict` başlayarak, yabancı anahtarların `Restrict` semantikle oluşturulmasını sağlar, yani basamaklar olmadan; ayrıca EF dahili fiksatlama etkilemeden kısıtlama ihlali atmak.

**Neden**

Bu değişiklik beklenmedik yan etkileri `DeleteBehavior` olmadan, sezgisel bir şekilde kullanma deneyimini geliştirmek için yapıldı.

**Risk Azaltıcı Etkenler**

Önceki davranış kullanılarak `DeleteBehavior.ClientNoAction`geri yüklenebilir.

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a>Sorgu türleri varlık türleri ile birleştirilir

[İzleme Sorunu #14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

**Eski davranış**

EF Core 3.0'dan önce, [sorgu türleri](xref:core/modeling/keyless-entity-types) birincil anahtarı yapılandırılmış bir şekilde tanımlamayan verileri sorgulamak için bir araçtı.
Diğer bir zamanda, anahtarolmadan varlık türlerini eşleme için bir sorgu türü kullanılırken (büyük olasılıkla bir görünümden, ancak büyük olasılıkla bir tablodan) normal bir varlık türü kullanılırken (büyük olasılıkla bir tablodan, ancak büyük olasılıkla bir görünümden).

**Yeni davranış**

Sorgu türü artık birincil anahtarı olmayan bir varlık türü haline gelir.
Anahtarsız varlık türleri önceki sürümlerde sorgu türleri ile aynı işlevsellik vardır.

**Neden**

Bu değişiklik, sorgu türlerinin amacı etrafındaki karışıklığı azaltmak için yapılmıştır.
Özellikle, bunlar anahtarsız varlık türleridir ve bu nedenle doğal olarak salt okunurlar, ancak yalnızca bir varlık türünün salt okunması gerektiğinden kullanılmamalıdır.
Aynı şekilde, genellikle görünümlere eşlenirler, ancak bunun tek nedeni görünümlerin genellikle anahtarları tanımlamamasıdır.

**Risk Azaltıcı Etkenler**

API'nin aşağıdaki bölümleri artık geçersizdir:
* **`ModelBuilder.Query<>()`**- `ModelBuilder.Entity<>().HasNoKey()` Bunun yerine hiçbir anahtarları olan bir varlık türü işaretlemek için çağrılması gerekir.
Bu, birincil anahtar beklendiğinde yanlış yapılandırmayı önlemek için yine de kuralla yapılandırılamaz, ancak kuralı eşleştirmez.
* **`DbQuery<>`**- `DbSet<>` Bunun yerine kullanılmalıdır.
* **`DbContext.Query<>()`**- `DbContext.Set<>()` Bunun yerine kullanılmalıdır.

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a>Sahip olunan tür ilişkileri için yapılandırma API'sı değişti

[İzleme Sorunu #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[İzleme Sorunu #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[İzleme Sorunu #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

**Eski davranış**

EF Core 3.0'dan önce, sahip olunan `OwnsOne` `OwnsMany` ilişkinin yapılandırması doğrudan çağrıdan sonra gerçekleştirilmiştir. 

**Yeni davranış**

EF Core 3.0 ile başlayarak, artık kullanarak sahibine bir navigasyon özelliği `WithOwner()`yapılandırmak için akıcı API vardır.
Örneğin:

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

Sahibi ve sahibi arasındaki ilişki ile ilgili yapılandırma `WithOwner()` şimdi diğer ilişkilerin nasıl yapılandırıldığına benzer şekilde zincirlenmelidir.
Sahip olunan tür kendisi için yapılandırma hala `OwnsOne()/OwnsMany()`sonra zincirlenmiş olsa da.
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

Ayrıca, `Entity()`'veya `HasOne()` `Set()` sahip olunan bir tür hedef ile arama şimdi bir özel durum atacaktır.

**Neden**

Bu değişiklik, sahip olunan tür kendisi ve sahip olunan tür _ile ilişki_ arasında daha temiz bir ayrım oluşturmak için yapıldı.
Bu da gibi `HasForeignKey`yöntemler etrafında belirsizlik ve karışıklık kaldırır.

**Risk Azaltıcı Etkenler**

Yukarıdaki örnekte gösterildiği gibi yeni API yüzeyini kullanmak için sahip olunan tür ilişkilerinin yapılandırmasını değiştirin.

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Tabloyu anaparayla paylaşan bağımlı varlıklar artık isteğe bağlıdır

[İzleme Sorunu #9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

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
EF Core 3.0'dan önce, aynı tabloya ait yse `OrderDetails` `OrderDetails` `Order` `Order` veya açıkça aynı tabloya eşlenmişse, yeni bir tablo eklerken her zaman bir örnek gereklidir.


**Yeni davranış**

3.0 ile başlayarak, EF `Order` Core `OrderDetails` bir olmadan eklemeye ve nullable sütunlar için birincil anahtar dışında tüm `OrderDetails` özellikleri eşler eklemenize olanak sağlar.
EF Core `OrderDetails` sorgularken, `null` gerekli özelliklerinden herhangi birinin bir değeri yoksa veya birincil anahtar dışında gerekli özellikleri `null`yoksa ve tüm özellikler .

**Risk Azaltıcı Etkenler**

Modelinizin tüm isteğe bağlı sütunlara bağlı bir tablo paylaşımı varsa, `null` ancak buna işaret eden gezintinin olması `null`beklenmiyorsa, uygulama nın gezinti olduğunda servis taleplerini işlemek için değiştirilmesi gerekir. Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmelidir veya en`null` az bir özelliğin kendisine atanmış olmayan bir değeri olmalıdır.

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a>Eşzamanlılık belirteç sütunu olan bir tabloyu paylaşan tüm varlıklar tabloyu bir özellik ile eşlene

[İzleme Sorunu #14154](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

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
EF Core 3.0'dan önce, aynı tabloya `OrderDetails` ait `Order` veya açıkça `OrderDetails` eşlenmişse, güncelleştirme istemcideki değeri güncelleştirmez `Version` ve bir sonraki güncelleştirme başarısız olur.


**Yeni davranış**

3.0 ile başlayarak, EF Core `Version` yeni `Order` değeri `OrderDetails`sahipse yayır. Aksi takdirde model doğrulama sırasında bir özel durum atılır.

**Neden**

Bu değişiklik, aynı tabloya eşlenen varlıklardan yalnızca biri güncelleştirildiğinde, eski bir eşzamanlılık belirteci değerini önlemek için yapılmıştır.

**Risk Azaltıcı Etkenler**

Tabloyu paylaşan tüm varlıklar, eşzamanlılık belirteç sütununa eşlenen bir özellik içermelidir. Gölge durumunda bir tane oluşturmak mümkündür:
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a>Sahibi izleme sorgusu kullanmadan sahip olunan varlıklar sorgulanamaz

[İzleme Sorunu #18876](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

**Eski davranış**

EF Core 3.0'dan önce, sahip olunan varlıklar başka bir gezinme olarak sorgulanabilir.

```csharp
context.People.Select(p => p.Address);
```

**Yeni davranış**

3.0 ile başlayarak, bir izleme sorgusu sahibi olmadan sahip olunan bir varlık projeleri eğer EF Core atar.

**Neden**

Sahip olunan varlıklar sahibi olmadan manipüle edilemez, bu nedenle vakaların büyük çoğunluğunda bu şekilde sorgulayan bir hatadır.

**Risk Azaltıcı Etkenler**

Sahip olunan varlığın daha sonra herhangi bir şekilde değiştirilecek şekilde izlenmesi gerekiyorsa, sahibi nin sorguya dahil edilmesi gerekir.

Aksi takdirde `AsNoTracking()` bir çağrı ekleyin:

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a>Eşlenmemiş türlerden devralınan özellikler artık tüm türetilmiş türler için tek bir sütuna eşlenir

[İzleme Sorunu #13998](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

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

EF Core 3.0'dan `ShippingAddress` önce, özellik sütunları `BulkOrder` varsayılan `Order` olarak ayırmak üzere eşlenir.

**Yeni davranış**

3.0 ile başlayarak, EF Core `ShippingAddress`için yalnızca bir sütun oluşturur.

**Neden**

Eski behavoir beklenmedikti.

**Risk Azaltıcı Etkenler**

Özellik, türemiş türler de ayrı sütuniçin açıkça eşlenebilir:

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>Yabancı anahtar mülkiyet sözleşmesi artık ana özellik ile aynı adla eşleşmiş

[İzleme Sorunu #13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

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
EF Core 3.0'dan önce, `CustomerId` özellik sözleşmeye göre yabancı anahtar için kullanılacaktı.
Ancak, `Order` sahip olunan bir tür ise, o zaman bu da birincil anahtar yapmak `CustomerId` ve bu genellikle beklenti değildir.

**Yeni davranış**

3.0 ile başlayarak, EF Core ana özellik ile aynı ada sahipse, yabancı anahtarlar için özellikleri sözleşmeye göre kullanmaya çalışmaz.
Ana özellik adı ile kaplanmış ana tür adı ve ana özellik adı desenleri ile kaplanmış gezinti adı hala eşleştirilir.
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

**Neden**

Bu değişiklik, sahip olunan türde birincil anahtar özelliğihatalı bir şekilde tanımlamamak için yapılmıştır.

**Risk Azaltıcı Etkenler**

Özellik yabancı anahtar ve dolayısıyla birincil anahtarın bir parçası olması amaçlandıysa, o zaman açıkça bu şekilde yapılandırın.

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a>İşlemKapsamı tamamlanmadan önce artık kullanılmazsa veritabanı bağlantısı artık kapatılır

[İzleme Sorunu #14218](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

**Eski davranış**

EF Core 3.0'dan önce, bağlam `TransactionScope`içindeki bağlantıyı açarsa , `TransactionScope` akım etkinken bağlantı açık kalır.

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

3.0 ile başlayan EF Core, bağlantıyı kullanır kullanmaz kapatır.

**Neden**

Bu değişiklik aynı `TransactionScope`birden çok bağlamı kullanmanıza olanak sağlar. Yeni davranış da EF6 eşleşir.

**Risk Azaltıcı Etkenler**

Bağlantının açık kalması gerekiyorsa, `OpenConnection()` EF Core'un bu çağrıyı zamanından önce kapatmamasını sağlayacaktır:

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Her özellik, bağımsız bellek tümseci anahtar oluşturma

[İzleme Sorunu #6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

**Eski davranış**

EF Core 3.0'dan önce, tüm bellek tüm inseda anahtar özellikleri için paylaşılan bir değer üreteci kullanılmıştır.

**Yeni davranış**

EF Core 3.0 ile başlayarak, her bir tümsek anahtar özelliği bellek veritabanını kullanırken kendi değer üreteci alır.
Ayrıca, veritabanı silinirse, anahtar oluşturma tüm tablolar için sıfırlanır.

**Neden**

Bu değişiklik, bellek içi anahtar oluşturmayı gerçek veritabanı anahtar oluşturmayla daha yakından hizalamak ve bellek veritabanını kullanırken testleri birbirinden ayırabilme yeteneğini geliştirmek için yapılmıştır.

**Risk Azaltıcı Etkenler**

Bu, ayarlanacak belirli bellek içi anahtar değerlerine dayanan bir uygulamayı bozabilir.
Bunun yerine belirli anahtar değerlere güvenmemeyi veya yeni davranışla eşleşecek şekilde güncelleştirmeyi düşünün.

### <a name="backing-fields-are-used-by-default"></a>Destek alanları varsayılan olarak kullanılır

[İzleme Sorunu #12430](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

**Eski davranış**

3.0'dan önce, bir özelliğin destek alanı bilinse bile, EF Core yine de varsayılan olarak özellik getter ve ayarlayıcı yöntemlerini kullanarak özellik değerini okur ve yazar.
Bunun istisnası, destek alanının biliniyorsa doğrudan ayarlanacağı sorgu yürütmesiydi.

**Yeni davranış**

EF Core 3.0 ile başlayarak, bir özelliğin destek alanı biliniyorsa, EF Core bu özelliği her zaman destek alanını kullanarak okur ve yazar.
Uygulama, getter veya ayarlayıcı yöntemlerine kodlanmış ek davranışa dayanıyorsa, bu uygulama nın kırılmasına neden olabilir.

**Neden**

Bu değişiklik, EF Core'un varlıkları içeren veritabanı işlemlerini gerçekleştirirken varsayılan olarak iş mantığını hatalı bir şekilde tetiklemesini önlemek için yapılmıştır.

**Risk Azaltıcı Etkenler**

Pre-3.0 davranışı özelliği erişim modu yapılandırması ile `ModelBuilder`geri yüklenebilir.
Örneğin:

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Birden çok uyumlu destek alanı bulunursa at

[İzleme Sorunu #12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

**Eski davranış**

EF Core 3.0'dan önce, birden çok alan bir özelliğin destek alanını bulma kurallarıyla eşleştiyse, bazı öncelik sırasına göre bir alan seçilir.
Bu, belirsiz durumlarda yanlış alanın kullanılmasına neden olabilir.

**Yeni davranış**

EF Core 3.0 ile başlayarak, birden çok alan aynı özellik ile eşleşirse, bir özel durum atılır.

**Neden**

Bu değişiklik, yalnızca bir doğru olabilir başka bir alan üzerinde sessizce kullanmaktan kaçınmak için yapıldı.

**Risk Azaltıcı Etkenler**

Belirsiz destek alanlarına sahip özellikler, açıkça kullanılacak alana sahip olmalıdır.
Örneğin, akıcı API kullanarak:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a>Yalnızca alan özelliği adları alan adı ile eşleşmelidir

**Eski davranış**

EF Core 3.0'dan önce, bir özellik bir dize değeriyle belirtilebilir ve .NET türünde bu ada sahip bir özellik bulunamazsa, EF Core bu özelliği kural kurallarını kullanarak bir alanla eşleştirmeye çalışır.

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

EF Core 3.0 ile başlayarak, yalnızca alan özelliği alan adı ile tam olarak eşleşmelidir.

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

**Neden**

Bu değişiklik, benzer adlı iki özellik için aynı alanı kullanmaktan kaçınmak için yapıldı, aynı zamanda clr özellikleri eşlenen özellikleri için aynı alan yalnızca özellikleri için eşleşen kurallar yapar.

**Risk Azaltıcı Etkenler**

Yalnızca alan özellikleri, eşlendikleri alanla aynı adlandırılmalıdır.
3.0'dan sonra EF Core'un gelecekteki sürümünde, özellik adından farklı bir alan adını açıkça yapılandırmayı yeniden etkinleştirmeyi planlıyoruz (bkz. sorun [#15307):](https://github.com/aspnet/EntityFrameworkCore/issues/15307)

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache arama

[İzleme Sorunu #14756](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

**Eski davranış**

EF Core 3.0 `AddDbContext` önce, arama veya `AddDbContextPool` aynı zamanda AddLogging ve [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)aramaları aracılığıyla DI ile günlük ve bellek önbelleğe alma hizmetleri kaydeder. [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging)

**Yeni davranış**

EF Core 3.0 `AddDbContext` ile `AddDbContextPool` başlayarak ve artık Bağımlılık Enjeksiyon (DI) ile bu hizmetleri kayıt olacaktır.

**Neden**

EF Core 3.0, bu hizmetlerin uygulamanın DI kapsayıcısında olduğunu gerektirmez. Ancak, `ILoggerFactory` uygulamanın DI kapsayıcısında kayıtlıysa, yine de EF Core tarafından kullanılır.

**Risk Azaltıcı Etkenler**

Uygulamanızın bu hizmetlere ihtiyacı varsa, [bunları AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [AddMemoryCache'yi](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)kullanarak DI kapsayıcısı ile açıkça kaydedin.

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a>AddEntityFramework* boyut sınırıyla IMemoryCache ekler

[İzleme Sorunu #12905](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

**Eski davranış**

EF Core 3.0'dan önce, arama `AddEntityFramework*` yöntemleri de boyut sınırı olmadan DI bellek önbelleğe alma hizmetlerini kaydeder.

**Yeni davranış**

EF Core 3.0 `AddEntityFramework*` ile başlayarak, bir boyut sınırı ile bir IMemoryCache hizmeti kaydeder. Daha sonra eklenen diğer hizmetler IMemoryCache'ye bağlıysa, özel durumlara veya düşük performansa neden olan varsayılan sınıra hızla ulaşabilirler.

**Neden**

Sorgu önbelleğe alma mantığında bir hata varsa veya sorgular dinamik olarak oluşturulursa, iMemoryCache'nin sınır olmadan kullanılması denetimsiz bellek kullanımına neden olabilir. Varsayılan sınıra sahip olmak olası bir DoS saldırısını azaltır.

**Risk Azaltıcı Etkenler**

Çoğu durumda `AddEntityFramework*` arama gerekli `AddDbContext` değildir `AddDbContextPool` veya de denir. Bu nedenle, en iyi azaltma `AddEntityFramework*` aramayı kaldırmaktır.

Uygulamanızın bu hizmetlere ihtiyacı varsa, [AddMemoryCache'yi](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)kullanarak önceden bir IMemoryCache uygulamasını açıkça DI kapsayıcısına kaydettirin.

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext.Entry şimdi yerel bir DetectChanges gerçekleştirir

[İzleme Sorunu #13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

**Eski davranış**

EF Core 3.0'dan önce, arama `DbContext.Entry` tüm izlenen varlıklar için değişikliklerin algılanabına neden olur.
Bu da ortaya çıkan `EntityEntry` devletin güncel olmasını sağladı.

**Yeni davranış**

EF Core 3.0 ile `DbContext.Entry` başlayarak, arama artık yalnızca verilen varlıktaki değişiklikleri ve bununla ilgili izlenen ana varlıkları algılamaya çalışır.
Bu, uygulama durumu üzerinde etkileri olabilir bu yöntem çağırarak başka bir değişiklik algılanmış olmayabilir anlamına gelir.

`ChangeTracker.AutoDetectChangesEnabled` Daha `false` sonra bu yerel değişiklik algılama devre dışı bırakılacak şekilde ayarlanmışsa unutmayın.

Örneğin `ChangeTracker.Entries` ve `SaveChanges`yine de izlenen tüm varlıklarla `DetectChanges` dolu bir şekilde değişiklik algılamaya neden olan diğer yöntemler.

**Neden**

Bu değişiklik kullanarak `context.Entry`varsayılan performansını artırmak için yapıldı.

**Risk Azaltıcı Etkenler**

Ön-3.0 `Entry` davranışını sağlamak için aramadan önce açıkça arayın. `ChangeTracker.DetectChanges()`

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>String ve bayt dizi anahtarları varsayılan olarak istemci tarafından oluşturulmaz

[İzleme Sorunu #14617](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

**Eski davranış**

EF Core 3.0'dan önce `string` ve `byte[]` anahtar özellikleri açıkça null olmayan bir değer ayarlamadan kullanılabilir.
Böyle bir durumda, anahtar değeri bir GUID olarak istemci üzerinde oluşturulur, `byte[]`için bayt serileştirilmiş.

**Yeni davranış**

EF Core 3.0 ile başlayarak anahtar değerinin ayarlandığını belirten bir özel durum atılır.

**Neden**

İstemci tarafından oluşturulan `string` / `byte[]` değerler genellikle yararlı olmadığından ve varsayılan davranış, oluşturulan anahtar değerleri ortak bir şekilde ikna etmeyi zorlaştırdığından, bu değişiklik yapıldı.

**Risk Azaltıcı Etkenler**

3.0 öncesi davranış, başka null olmayan değer ayarlanmazsa, anahtar özelliklerinin oluşturulan değerleri kullanması gerektiğini açıkça belirterek elde edilebilir.
Örneğin, akıcı API ile:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

Veya veri ek açıklamaları ile:

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a>ILoggerFactory artık kapsamlı bir hizmettir

[İzleme Sorunu #14698](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

**Eski davranış**

ÖNCE EF Core 3.0, `ILoggerFactory` singleton hizmet olarak kaydedildi.

**Yeni davranış**

EF Core 3.0 `ILoggerFactory` ile başlayarak, şimdi kapsamlı olarak kaydedilir.

**Neden**

Bu değişiklik, bir logger'ın diğer `DbContext` işlevleri etkinleştiren ve dahili servis sağlayıcılarının patlaması gibi bazı patolojik davranış durumlarını ortadan kaldıran bir örnekle ilişkilendirmesine izin vermek için yapılmıştır.

**Risk Azaltıcı Etkenler**

Bu değişiklik, EF Core dahili hizmet sağlayıcısında özel hizmetler kaydedilmedikçe ve kullanmadığı sürece uygulama kodunu etkilememelidir.
Bu yaygın bir şey değil.
Bu gibi durumlarda, çoğu şey hala çalışacaktır, ancak bağlı `ILoggerFactory` olduğu herhangi bir singleton hizmet farklı bir şekilde elde `ILoggerFactory` etmek için değiştirilmesi gerekir.

Bu gibi durumlarla karşı karşıya ysanız, lütfen [EF Core GitHub sorun izleyicisine](https://github.com/aspnet/EntityFrameworkCore/issues) bir `ILoggerFactory` sorun dosyalayın ve gelecekte bunu nasıl tekrar kıramayacağımızı daha iyi anlayabileceğimizi bize bildirin.

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>Tembel yükleme lisi artık navigasyon özelliklerinin tamamen yüklendiğini varsaymaz

[İzleme Sorunu #12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

**Eski davranış**

EF Core 3.0'dan `DbContext` önce, a bir kez elden çıkarılmışsa, bu bağlamdan elde edilen bir varlık üzerindeki belirli bir navigasyon özelliğinin tam olarak yüklenip yüklenmediğini bilmenin bir yolu yoktu.
Ekseksiler bunun yerine, null olmayan bir değeri varsa bir başvuru gezintisi yüklendiğini ve boş değilse koleksiyon gezintisi yüklendiğini varsayar.
Bu gibi durumlarda, tembel yükleme ye çalışmak bir no-op olacaktır.

**Yeni davranış**

EF Core 3.0 ile başlayarak, yakınlık lar bir gezinti özelliğinin yüklenip yüklenmediğini izler.
Bu, bağlam elden çıkarıldıktan sonra yüklenen bir gezinti özelliğine erişmeye çalışmak, yüklenen gezinti boş veya boş olsa bile her zaman bir no-op olacağı anlamına gelir.
Tersine, yüklenmeyen bir gezinti özelliğine erişmeye çalışmak, gezinti özelliği boş olmayan bir koleksiyon olsa bile bağlam atılırsa bir özel durum oluşturur.
Bu durum ortaya çıkarsa, uygulama kodunun geçersiz bir zamanda tembel yükleme kullanmaya çalıştığı ve uygulamanın bunu yapmamak için değiştirilmesi gerektiği anlamına gelir.

**Neden**

Bu değişiklik, elden çıkarılan `DbContext` bir örneğin üzerine tembelce yükleme yapmaya çalışırken davranışı tutarlı ve doğru hale getirmek için yapılmıştır.

**Risk Azaltıcı Etkenler**

Uygulama kodunu, elden çıkarılmış bir bağlamla tembel yüklemegirişiminde bulunmamak için güncelleştirin veya bunu özel durum iletisinde açıklandığı gibi bir no-op olarak yapılandırın.

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>Dahili hizmet sağlayıcılarının aşırı oluşturulması artık varsayılan olarak bir hatadır

[İzleme Sorunu #10236](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

**Eski davranış**

EF Core 3.0'dan önce, patolojik sayıda dahili hizmet sağlayıcısı oluşturan bir uygulama için bir uyarı günlüğe kaydedilir.

**Yeni davranış**

EF Core 3.0 ile başlayarak, bu uyarı artık kabul edilir ve hata ve bir özel durum atılır. 

**Neden**

Bu değişiklik, bu patolojik durumu daha açık bir şekilde ortaya çıkararak daha iyi uygulama kodu kullanmak için yapılmıştır.

**Risk Azaltıcı Etkenler**

Bu hatayla karşılaşmanın en uygun eylem nedeni, temel nedeni anlamak ve bu kadar çok dahili hizmet sağlayıcısı oluşturmayı durdurmaktır.
Ancak, hata üzerinde yapılandırma yoluyla bir uyarı (veya göz `DbContextOptionsBuilder`ardı) geri dönüştürülebilir.
Örneğin:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>HasOne/HasMany için yeni davranış tek bir dize ile çağrıldı

[İzleme Sorunu #9171](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

**Eski davranış**

EF Core 3.0'dan `HasOne` `HasMany` önce, kod arama veya tek bir dize ile kafa karıştırıcı bir şekilde yorumlandı.
Örneğin:
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

Kod, özel olabilecek `Samurai` gezinti özelliğini `Entrance` kullanarak başka bir varlık türüyle ilgili gibi görünüyor.

Gerçekte, bu kod hiçbir gezinti özelliği ile `Entrance` adlandırılan bazı varlık türü için bir ilişki oluşturmak için çalışır.

**Yeni davranış**

EF Core 3.0 ile başlayarak, yukarıdaki kod şimdi daha önce yapıyor olması gerektiği gibi görünüyordu ne yapar.

**Neden**

Eski davranış, özellikle yapılandırma kodu okuma ve hataları ararken, çok kafa karıştırıcı oldu.

**Risk Azaltıcı Etkenler**

Bu, yalnızca tür adları için dizeleri kullanarak ve gezinti özelliğiaçıkça belirtmeden ilişkileri açıkça yapılandıran uygulamaları kırar.
Bu yaygın değildir.
Önceki davranış, gezinti özelliği adı `null` için açıkça geçerek elde edilebilir.
Örneğin:

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a>Birkaç async yönteminin dönüş türü Görevden ValueTask'a değiştirildi

[İzleme Sorunu #15184](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

**Eski davranış**

Aşağıdaki async yöntemleri daha `Task<T>`önce döndürülen a:

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()`(ve türeyen sınıflar)

**Yeni davranış**

Söz konusu yöntemler şimdi daha `ValueTask<T>` önce olduğu `T` gibi bir üzerinde döndürün.

**Neden**

Bu değişiklik, genel performansı artırarak, bu yöntemleri çağırarak oluşan yığın ayırmalarının sayısını azaltır.

**Risk Azaltıcı Etkenler**

Yukarıdaki API'leri bekleyen uygulamaların yalnızca yeniden derlenmeleri gerekir - kaynak değişiklikleri gerekmez.
Daha karmaşık bir kullanım (örn. `Task` döndürülen `Task.WhenAny()`ekibe geçmek) genellikle döndürülen `ValueTask<T>` lerin onu çağırarak `Task<T>` `AsTask()` a'ya dönüştürülmesini gerektirir.
Bu değişikliğin getirdiği ayırma azaltma inkâr ları unutmayın.

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>İlişkisel: TypeMapping ek açıklama şimdi sadece TypeMapping olduğunu

[İzleme Sorunu #9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

**Eski davranış**

Tür eşleme ek açıklamaları için ek açıklama adı "İlişkisel:TypeMapping" idi.

**Yeni davranış**

Tür eşleme ek açıklamaları için ek açıklama adı artık "TypeMapping"dir.

**Neden**

Tür eşlemeleri artık ilişkisel veritabanı sağlayıcılarından daha fazlası için kullanılır.

**Risk Azaltıcı Etkenler**

Bu, yalnızca tür eşlemesine doğrudan ek açıklama olarak erişen ve yaygın olmayan uygulamaları bozar.
Düzeltmek için en uygun eylem, doğrudan ek açıklama kullanmak yerine tür eşlemeleri erişmek için API yüzeyi kullanmaktır.

### <a name="totable-on-a-derived-type-throws-an-exception"></a>Türetilmiş bir türde ToTable bir özel durum atar 

[İzleme Sorunu #11811](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

**Eski davranış**

EF Core 3.0'dan önce, `ToTable()` türetilmiş bir tür elendi, çünkü bu geçerli olmayan yalnızca devralma eşleme stratejisi TPH olduğundan, bu türde çağrı yapılmaz. 

**Yeni davranış**

EF Core 3.0 ile başlayan ve daha sonraki bir sürümde `ToTable()` TPT ve TPC desteği eklemeye hazırlık olarak, türetilmiş bir tür şimdi gelecekte beklenmedik bir harita değişikliği önlemek için bir özel durum atacaktır çağırdı.

**Neden**

Şu anda türetilen bir türü farklı bir tabloyla eşlemek geçerli değildir.
Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda kırılmayı önler.

**Risk Azaltıcı Etkenler**

Türetilen türleri diğer tablolarla eşlenemeye yönelik tüm girişimleri kaldırın.

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a>ForSqlServerHasIndex HasIndex ile değiştirildi 

[İzleme Sorunu #12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

**Eski davranış**

EF Core 3.0'dan önce, `ForSqlServerHasIndex().ForSqlServerInclude()` 'ile `INCLUDE`kullanılan sütunları yapılandırmak için bir yol sağlanmıştır.

**Yeni davranış**

EF Core 3.0 ile `Include` başlayarak, bir dizin üzerinde kullanmak artık ilişkisel düzeyde desteklenir.
`HasIndex().ForSqlServerInclude()` adresini kullanın.

**Neden**

Bu değişiklik, dizinler `Include` için API'yi tüm veritabanı sağlayıcıları için tek bir yerde birleştirmek için yapılmıştır.

**Risk Azaltıcı Etkenler**

Yukarıda gösterildiği gibi yeni API'yi kullanın.

### <a name="metadata-api-changes"></a>Meta veri API değişiklikleri

[İzleme Sorunu #214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

**Yeni davranış**

Aşağıdaki özellikler uzantı yöntemlerine dönüştürüldü:

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

**Neden**

Bu değişiklik, yukarıda belirtilen arabirimlerin uygulanmasını kolaylaştırır.

**Risk Azaltıcı Etkenler**

Yeni uzantı yöntemlerini kullanın.

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a>Sağlayıcıya özel Meta veri API değişiklikleri

[İzleme Sorunu #214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

**Yeni davranış**

Sağlayıcıya özel uzatma yöntemleri düzleştirilmiş olacaktır:

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

**Neden**

Bu değişiklik, yukarıda belirtilen uzantı yöntemlerinin uygulanmasını kolaylaştırır.

**Risk Azaltıcı Etkenler**

Yeni uzantı yöntemlerini kullanın.

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>EF Core artık SQLite FK uygulama için pragma gönderir

[İzleme Sorunu #12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

**Eski davranış**

EF Core 3.0'dan önce, SQLite bağlantısı açıldığında EF Core gönderir. `PRAGMA foreign_keys = 1`

**Yeni davranış**

EF Core 3.0 ile başlayarak, `PRAGMA foreign_keys = 1` SQLite bağlantısı açıldığında EF Core artık göndermez.

**Neden**

Bu değişiklik, EF Core `SQLitePCLRaw.bundle_e_sqlite3` varsayılan olarak kullandığı için yapıldı, bu da FK zorlamanın varsayılan olarak açık olduğu ve her bağlantı açıldığında açıkça etkinleştirilmesi gerekmediği anlamına geliyor.

**Risk Azaltıcı Etkenler**

Yabancı anahtarlar, varsayılan olarak EF Core için kullanılan SQLitePCLRaw.bundle_e_sqlite3'da varsayılan olarak etkinleştirilir.
Diğer durumlarda, bağlantı dizenizde belirtilerek `Foreign Keys=True` yabancı anahtarlar etkinleştirilebilir.

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a>Microsoft.EntityFrameworkCore.Sqlite şimdi SQLitePCLRaw.bundle_e_sqlite3 bağlıdır

**Eski davranış**

EF Core 3.0'dan `SQLitePCLRaw.bundle_green`önce EF Core kullanılır.

**Yeni davranış**

EF Core 3.0 ile başlayarak, EF Core kullanır. `SQLitePCLRaw.bundle_e_sqlite3`

**Neden**

Bu değişiklik, sqlite sürümü diğer platformlar ile tutarlı iOS kullanılan böylece yapıldı.

**Risk Azaltıcı Etkenler**

iOS'ta yerel SQLite sürümünü kullanmak `Microsoft.Data.Sqlite` için farklı `SQLitePCLRaw` bir paket kullanacak şekilde yapılandırın.

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a>Kılavuz değerleri artık SQLite'da METİn olarak depolanır

[İzleme Sorunu #15078](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

**Eski davranış**

Guid değerleri daha önce SQLite'da BLOB değerleri olarak depolanmış.

**Yeni davranış**

Kılavuz değerleri artık TEXT olarak depolanır.

**Neden**

Guids'in ikili biçimi standartlaştırılamaz. Değerlerin TEXT olarak depolanması, veritabanını diğer teknolojilerle daha uyumlu hale getirir.

**Risk Azaltıcı Etkenler**

Aşağıdaki gibi SQL çalıştırarak varolan veritabanlarını yeni biçime geçirebilirsiniz.

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

EF Core'da, bu özellikler üzerinde bir değer dönüştürücüsi yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

Microsoft.Data.Sqlite, hem BLOB hem de TEXT sütunlarından Guid değerlerini okuma yeteneğine sahiptir; ancak, parametreler ve sabitler için varsayılan biçim değiştiğinden, Guids içeren çoğu senaryo için büyük olasılıkla işlem yapmanız gerekir.

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Char değerleri artık SQLite'da TEXT olarak depolanır

[İzleme Sorunu #15020](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

**Eski davranış**

Char değerleri daha önce SQLite'da INTEGER değerleri olarak ayrıştırıldı. Örneğin, *A'nın* bir char değeri tamsayı değeri 65 olarak depolandı.

**Yeni davranış**

Char değerleri artık TEXT olarak depolanır.

**Neden**

Değerleri TEXT olarak depolamak daha doğaldır ve veritabanını diğer teknolojilerle daha uyumlu hale getirir.

**Risk Azaltıcı Etkenler**

Aşağıdaki gibi SQL çalıştırarak varolan veritabanlarını yeni biçime geçirebilirsiniz.

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

EF Core'da, bu özellikler üzerinde bir değer dönüştürücüsi yapılandırarak önceki davranışı kullanmaya devam edebilirsiniz.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

Microsoft.Data.Sqlite ayrıca hem INTEGER hem de TEXT sütunlarından karakter değerlerini okuma yeteneğine sahiptir, bu nedenle bazı senaryolar herhangi bir eylem gerektirmeyebilir.

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a>Geçiş disleri artık değişmez kültürün takvimi kullanılarak oluşturulur

[İzleme Sorunu #12978](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

**Eski davranış**

Geçiş disleri, geçerli kültürün takvimi kullanılarak yanlışlıkla oluşturuldu.

**Yeni davranış**

Geçiş işlleri şimdi her zaman değişmez kültürün takvimi (Gregoryen) kullanılardı.

**Neden**

Geçişsırası, veritabanını güncelleştirirken veya birleştirme çakışmalarını çözerken önemlidir. Değişmez takvimin kullanılması, takım üyelerinin farklı sistem takvimlerine sahip olmasından kaynaklanabilir sipariş sorunlarını önler.

**Risk Azaltıcı Etkenler**

Bu değişiklik, yılın Gregoryen takviminden (Tay Budist takvimi gibi) daha büyük olduğu Gregoryen olmayan bir takvimi kullanan herkesi etkiler. Varolan geçişlerden sonra yeni geçişlerin sıralanabilmesi için varolan geçiş lerin güncellenmesi gerekir.

Geçiş kimliği, geçişlerin tasarımcı dosyalarında Geçiş özniteliğinde bulunabilir.

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

Geçişler geçmişi tablosunun da güncellenmesi gerekir.

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a>UseRowNumberForPaging kaldırıldı

[İzleme Sorunu #16400](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

**Eski davranış**

EF Core 3.0'dan önce, `UseRowNumberForPaging` SQL Server 2008 ile uyumlu sayfalama için SQL oluşturmak için kullanılabilir.

**Yeni davranış**

EF Core 3.0 ile başlayarak, EF yalnızca sonraki SQL Server sürümleriyle uyumlu olan sayfalama için SQL oluşturur. 

**Neden**

[SQL Server 2008 artık desteklenen bir ürün olmadığı](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) için bu değişikliği yapıyoruz ve bu özelliği EF Core 3.0'da yapılan sorgu değişiklikleriyle çalışacak şekilde güncelliyoruz önemli bir iş.

**Risk Azaltıcı Etkenler**

Oluşturulan SQL'in desteklenmesi için SQL Server'ın daha yeni bir sürümüne güncelleştirmenizi veya daha yüksek bir uyumluluk düzeyi kullanmanızı öneririz. Bu söyleniyor, bunu yapamıyorsanız, o zaman ayrıntıları ile [izleme sorunu hakkında yorum](https://github.com/aspnet/EntityFrameworkCore/issues/16400) lütfen. Bu kararı geri bildirimlere dayanarak tekrar gözden geçirebiliriz.

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a>Uzantı bilgileri/meta veriler IDbContextOptionsExtension kaldırıldı

[İzleme Sorunu #16119](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

**Eski davranış**

`IDbContextOptionsExtension`uzantısı hakkında meta veri sağlamak için yöntemler içeriyordu.

**Yeni davranış**

Bu yöntemler, yeni `DbContextOptionsExtensionInfo` `IDbContextOptionsExtension.Info` bir özellikten döndürülen yeni bir soyut taban sınıfına taşınmıştır.

**Neden**

2.0'dan 3.0'a kadar olan sürümler üzerinde bu yöntemleri birkaç kez eklememiz veya değiştirmemiz gerekiyordu.
Bunları yeni bir soyut taban sınıfına dönüştürmek, varolan uzantıları bozmadan bu tür değişiklikleri yapmayı kolaylaştırır.

**Risk Azaltıcı Etkenler**

Yeni deseni izlemek için uzantıları güncelleştirin.
ÖRNEKLER, EF Core kaynak `IDbContextOptionsExtension` kodundaki farklı türde uzantılar için birçok uygulamada bulunur.

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a>LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı

[İzleme Sorunu #10985](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

**Değiştir**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`olarak `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`değiştirilmiştir.

**Neden**

Bu uyarı olayının adlandırmasını diğer tüm uyarı olaylarıyla hizalar.

**Risk Azaltıcı Etkenler**

Yeni adı kullan. (Olay kimliği numarasının değişmediğini unutmayın.)

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a>Yabancı anahtar kısıtlama adları için API'yi netleştir

[İzleme Sorunu #10730](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

**Eski davranış**

EF Core 3.0'dan önce, yabancı anahtar kısıtlama adları basitçe "ad" olarak adlandırılıyordu. Örneğin:

```csharp
var constraintName = myForeignKey.Name;
```

**Yeni davranış**

EF Core 3.0 ile başlayarak, yabancı anahtar kısıtlama adları artık "kısıtlama adı" olarak adlandırılır. Örneğin:

```csharp
var constraintName = myForeignKey.ConstraintName;
```

**Neden**

Bu değişiklik, bu alanda adlandırma tutarlılık getiriyor ve aynı zamanda bu yabancı anahtar kısıtlama adı değil, yabancı anahtar tanımlanan sütun veya özellik adı olduğunu açıklar.

**Risk Azaltıcı Etkenler**

Yeni adı kullan.

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a>iRelationalDatabaseCreator.HasTables/HasTablesAsync genel kullanıma açıklanmıştır

[İzleme Sorunu #15997](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

**Eski davranış**

EF Core 3.0'dan önce bu yöntemler korunmuştur.

**Yeni davranış**

EF Core 3.0 ile başlayarak, bu yöntemler herkese açıktır.

**Neden**

Bu yöntemler, bir veritabanı nın oluşturulabilir ancak boş olup olmadığını belirlemek için EF tarafından kullanılır. Bu, geçişlerin uygulanıp uygulanmayacağını belirlerken EF dışından da yararlı olabilir.

**Risk Azaltıcı Etkenler**

Geçersiz kılmaların erişilebilirliğini değiştirin.

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a>Microsoft.EntityFrameworkCore.Design artık bir DevelopmentDependency paketidir

[İzleme Sorunu #11506](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

**Eski davranış**

EF Core 3.0'dan önce, Microsoft.EntityFrameworkCore.Design, derlemesi ona bağlı projeler tarafından başvurulan normal bir NuGet paketiydi.

**Yeni davranış**

EF Core 3.0 ile başlayarak, bir DevelopmentDependency paketidir. Bu, bağımlılığın diğer projelere geçişli olarak akamayacağı ve varsayılan olarak artık derlemesine başvuruda bulunamayacağınız anlamına gelir.

**Neden**

Bu paket yalnızca tasarım zamanında kullanılmak üzere tasarlanmıştır. Dağıtılan uygulamalar başvuru olmamalıdır. Paketi Geliştirme Bağımlılığı yapmak bu öneriyi pekiştirir.

**Risk Azaltıcı Etkenler**

EF Core'un tasarım zamanı davranışını geçersiz kılmak için bu pakete başvurmanız gerekiyorsa, projenizdeki PackageReference madde meta verilerini güncelleştirebilirsiniz.

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

Paket Microsoft.EntityFrameworkCore.Tools üzerinden geçişli olarak başvuruluyorsa, meta verilerini değiştirmek için pakete açık bir PackageReference eklemeniz gerekir. Paketteki türlerin gerekli olduğu herhangi bir projeye bu tür açık bir başvuru eklenmelidir.

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a>SQLitePCL.raw sürüm 2.0.0 güncellendi

[İzleme Sorunu #14824](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

**Eski davranış**

Microsoft.EntityFrameworkCore.Sqlite daha önce SQLitePCL.raw sürümü 1.1.12 bağlı.

**Yeni davranış**

Paketimizi sürüm 2.0.0'a bağlı olarak güncelledik.

**Neden**

Sürüm 2.0.0 SQLitePCL.raw hedefleri .NET Standart 2.0. Daha önce .NET Standart 1.1'i hedeflenene de geçişli paketlerin çalışması için büyük bir kapatma gerekiyordu.

**Risk Azaltıcı Etkenler**

SQLitePCL.raw sürüm 2.0.0 bazı kırılma değişiklikleri içerir. Ayrıntılar için [sürüm notlarına](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) bakın.

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a>NetTopologySuite sürüm 2.0.0 güncellendi

[İzleme Sorunu #14825](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

**Eski davranış**

Uzamsal paketler daha önce NetTopologySuite'in 1.15.1 sürümüne bağlıydı.

**Yeni davranış**

Paketimizi sürüm 2.0.0'a bağlı olarak güncelledik.

**Neden**

NetTopologySuite Sürüm 2.0.0 EF Core kullanıcıları tarafından karşılaşılan çeşitli kullanılabilirlik sorunları ele amaçlamaktadır.

**Risk Azaltıcı Etkenler**

NetTopologySuite sürüm 2.0.0 bazı kırılma değişiklikleri içerir. Ayrıntılar için [sürüm notlarına](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) bakın.

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a>System.Data.SqlClient yerine Microsoft.Data.SqlClient kullanılır

[İzleme Sorunu #15636](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

**Eski davranış**

Microsoft.EntityFrameworkCore.SqlServer daha önce System.Data.SqlClient'a bağlı.

**Yeni davranış**

Paketimizi Microsoft.Data.SqlClient'a bağlı olacak şekilde güncelledik.

**Neden**

Microsoft.Data.SqlClient, SQL Server'ın en önemli veri erişim sürücüsüdür ve System.Data.SqlClient artık geliştirmenin odak noktası değildir.
Her Zaman Şifrelenmiş gibi bazı önemli özellikler yalnızca Microsoft.Data.SqlClient'da kullanılabilir.

**Risk Azaltıcı Etkenler**

Kodunuz System.Data.SqlClient'a doğrudan bağımlılık gerekiyorsa, bunun yerine Microsoft.Data.SqlClient'a başvurmak üzere değiştirmeniz gerekir; iki paket API uyumluluğu çok yüksek derecede korumak gibi, bu sadece basit bir paket ve ad alanı değişikliği olmalıdır.

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a>Birden çok belirsiz kendi kendine başvuran ilişkiler yapılandırılmalıdır 

[İzleme Sorunu #13573](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

**Eski davranış**

Birden çok kendi kendine başvuran tek yönlü gezinme özellikleri ve eşleşen FK'lar ile bir varlık türü yanlış tek bir ilişki olarak yapılandırıldı. Örneğin:

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

Bu senaryo artık model binasında algılanır ve modelin belirsiz olduğunu belirten bir özel durum atılır.

**Neden**

Ortaya çıkan model belirsiz ve büyük olasılıkla genellikle bu durum için yanlış olacaktır.

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a>DbFunction.Schema null veya boş dize olmak modelin varsayılan şema olarak yapılandırır

[İzleme Sorunu #12757](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

**Eski davranış**

Boş bir dize olarak şema ile yapılandırılan bir DbFunction, şema olmadan yerleşik işlev olarak kabul edildi. Örneğin aşağıdaki kod `DatePart` CLR işlevini `DATEPART` SqlServer'daki yerleşik işleve eşler.

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

**Yeni davranış**

Tüm DbFunction eşlemeleri kullanıcı tanımlı işlevlere eşlenmiş olarak kabul edilir. Bu nedenle boş dize değeri modeli için varsayılan şema içinde işlev koyacağız. Hangi şema açıkça akıcı API `modelBuilder.HasDefaultSchema()` veya `dbo` başka bir şekilde yapılandırılan olabilir.

**Neden**

Daha önce şema boş olması bu işlevi işlemek için bir yol yerleşik ama bu mantık sadece sqlserver için geçerli olan yerleşik işlevleri herhangi bir şema ait değildir.

**Risk Azaltıcı Etkenler**

DbFunction'in çevirisini yerleşik bir işleve eşlemek için el ile yapılandırın.

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
