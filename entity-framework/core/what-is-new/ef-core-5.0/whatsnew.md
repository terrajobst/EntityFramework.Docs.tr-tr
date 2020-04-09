---
title: EF Core 5.0'da Yenilikler
description: EF Core 5.0'daki yeni özelliklere genel bakış
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c047a308cadf44eea577dcab29b68b36942a50df
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634274"
---
# <a name="whats-new-in-ef-core-50"></a>EF Core 5.0'da Yenilikler

EF Core 5.0 şu anda geliştirilmektedir.
Bu sayfa, her önizlemede tanıtılan ilginç değişikliklerin genel bir özetini içerir.

Bu [sayfa, EF Core 5.0 için planı](plan.md)çoğaltmaz.
Planda, son sürümü göndermeden önce eklemeyi planladığımız her şey de dahil olmak üzere EF Core 5.0'ın genel temaları açıklanmaktadır.

Yayınlandığı resmi belgelere buradan linkler ekleyeceğiz.

## <a name="preview-2"></a>Önizleme 2

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a>Özellik destek alanı belirtmek için C# özniteliği kullanma

C# özniteliği artık bir özelliğin destek alanını belirtmek için kullanılabilir.
Bu öznitelik, EF Core'un destek alanı otomatik olarak bulunamasa bile normalde olduğu gibi destek alanına yazmaya ve okumasına olanak tanır.
Örneğin:

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

Dokümantasyon, sorun [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230)tarafından izlenir.

### <a name="complete-discriminator-mapping"></a>Tam ayırıcı eşleme

EF Core, [bir kalıtım hiyerarşisinin TPH eşlemi](/ef/core/modeling/inheritance)için bir ayırıcı sütun kullanır.
EF Core ayırıcı için tüm olası değerleri bildiği sürece bazı performans geliştirmeleri mümkündür.
EF Core 5.0 şimdi bu geliştirmeleri uygular.

Örneğin, EF Core'un önceki sürümleri, hiyerarşideki tüm türleri döndüren bir sorgu için her zaman bu SQL'i oluşturur:

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

EF Core 5.0 şimdi tam bir ayrımcı eşleme yapılandırıldığınızda aşağıdakileri oluşturacaktır:

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

Önizleme 3 ile başlayan varsayılan davranış olacaktır.

### <a name="performance-improvements-in-microsoftdatasqlite"></a>Microsoft.Data.Sqlite'de performans iyileştirmeleri

SQLIte için iki performans iyileştirmesi yaptık:

* GetBytes, GetChars ve GetTextReader ile ikili ve dize verilerini alma artık SqliteBlob ve akışlardan yararlanarak daha verimli hale geldi.
* SqliteConnection'ın başlatılması artık tembel.

Bu geliştirmeler Microsoft.Data.Sqlite sağlayıcısı ADO.NET ve dolayısıyla DA EF Core dışında performansı artırmak bulunmaktadır.

## <a name="preview-1"></a>Önizleme 1

### <a name="simple-logging"></a>Basit günlüğe kaydetme

Bu özellik, EF6'dakine `Database.Log` benzer işlevsellik ekler.
Diğer bir zamanda, her türlü dış günlük çerçevesini yapılandırmaya gerek kalmadan EF Core'dan günlük almanın basit bir yolunu sağlar.

Ön belgeler [5 Aralık 2019 için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)dahildir.

Ek belgeler [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)soruna göre izlenir.

### <a name="simple-way-to-get-generated-sql"></a>Oluşturulan SQL almak için basit bir yol

EF Core 5.0, `ToQueryString` BIR LINQ sorgusu yürükarırken EF Core'un oluşturacağı SQL'i döndürecek uzantı yöntemini sunar.

Ön belgeler [9 Ocak 2020 için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)dahildir.

Ek belgeler [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)soruna göre izlenir.

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a>Bir varlığın anahtarı olmadığını belirtmek için C# özniteliği kullanın

Bir varlık türü artık yeni `KeylessAttribute`yi kullanarak anahtara sahip olmayacak şekilde yapılandırılabilir.
Örneğin:

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

Dokümantasyon, sorun [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186)tarafından izlenir.

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>Başlatmalı DbContext'da bağlantı veya bağlantı dizesi değiştirilebilir

Herhangi bir bağlantı veya bağlantı dizesi olmadan bir DbContext örneği oluşturmak artık daha kolaydır.
Ayrıca, bağlantı veya bağlantı dizesi artık bağlam örneğinde mutasyona uğrayabilir.
Bu özellik, aynı bağlam örneğinin farklı veritabanlarına dinamik olarak bağlanmasını sağlar.

Dokümantasyon, sorun [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)tarafından izlenir.

### <a name="change-tracking-proxies"></a>Değiştirme izleme vekilleri

EF Core artık otomatik olarak [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) ve [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1)uygulayan çalışma zamanı vekilleri oluşturabilir.
Bunlar daha sonra varlık özelliklerindeki değer değişikliklerini doğrudan EF Core'a raporlayarak değişiklikleri taramaya ihtiyaç duymayı önler.
Ancak, vekiller sınırlamalar kendi kümesi ile gelir, bu yüzden herkes için değildir.

Dokümantasyon, #2076 [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)soruna göre izlenir.

### <a name="enhanced-debug-views"></a>Gelişmiş hata ayıklama görünümleri

Hata ayıklama görünümleri, hata ayıklama sorunları sırasında EF Core'un iç etkilerine bakmanın kolay bir yoludur.
Model için hata ayıklama görünümü bir süre önce uygulandı.
EF Core 5.0 için, model görünümünün okunmasını kolaylaştırdık ve devlet yöneticisindeki izlenen varlıklar için yeni bir hata ayıklama görünümü ekledik.

Ön belgeler [12 Aralık 2019 için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206)dahildir.

Ek belgeler sorun [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)tarafından izlenir.

### <a name="improved-handling-of-database-null-semantics"></a>Veritabanı null semantik geliştirilmiş işleme

İlişkisel veritabanları genellikle null'u bilinmeyen bir değer olarak ele alacaktır ve bu nedenle diğer NULL'lara eşit değildir.
C# null'u diğer null'lara eşit olan tanımlı bir değer olarak değerlendirirken.
EF Core varsayılan olarak sorguları c# null semantiklerini kullanarak çevirir.
EF Core 5.0 bu çevirilerin verimliliğini büyük ölçüde artırır.

Dokümantasyon, sorun [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)tarafından izlenir.

### <a name="indexer-properties"></a>Dizinleyici özellikleri

EF Core 5.0, C# dizinleyici özelliklerinin eşlenemesini destekler.
Bu özellikler, varlıkların, sütunların çantadaki adlandırılmış özelliklere eşlendiği mülk torbası olarak hareket etmesine olanak sağlar.

Dokümantasyon, #2018 [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)tarafından izlenir.

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Enum eşlemeleri için denetim kısıtlamaları oluşturma

EF Core 5.0 Geçişler artık enum özellik eşlemeleri için ÇEK kısıtlamaları oluşturabilir.
Örneğin:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

Dokümantasyon, sorun [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)tarafından izlenir.

### <a name="isrelational"></a>İlişkisel

Yeni `IsRelational` bir yöntem varolan `IsSqlServer`ek olarak `IsSqlite`eklendi `IsInMemory`, , ve .
Bu yöntem, DbContext'ın herhangi bir ilişkisel veritabanı sağlayıcısı kullanıp kullanmadığını sınamak için kullanılabilir.
Örneğin:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

Dokümantasyon, sorun [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185)tarafından izlenir.

### <a name="cosmos-optimistic-concurrency-with-etags"></a>ETags ile Cosmos iyimser eşzamanlılık

Azure Cosmos DB veritabanı sağlayıcısı artık ETags kullanarak iyimser eşzamanlılığı destekliyor.
Bir ETag yapılandırmak için OnModelCreating modeli oluşturucu kullanın:

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

SaveChanges daha sonra `DbUpdateConcurrencyException` yeniden denemeler, vb uygulamak için [ele alınabilir](https://docs.microsoft.com/ef/core/saving/concurrency) bir eşzamanlılık çakışması, bir atar.

Dokümantasyon, #2099 [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099)soruna göre izlenir.

### <a name="query-translations-for-more-datetime-constructs"></a>Daha fazla DateTime yapıları için sorgu çevirileri

Yeni DateTime yapısı içeren sorgular artık çevrilmiştir.

Buna ek olarak, aşağıdaki SQL Server işlevleri şimdi eşlenir:

* TarihDiffWeek
* DateFromParts

Örneğin:

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

Dokümantasyon, sorun [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)tarafından izlenir.

### <a name="query-translations-for-more-byte-array-constructs"></a>Daha fazla bayt dizi yapıları için sorgu çevirileri

Bayt[] özelliklerindeki İçer, Uzunluk, DiziEşit vb. sorgular artık SQL'e çevrilmiştir.

Ön belgeler [5 Aralık 2019 için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)dahildir.

Ek belgeler [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.

### <a name="query-translation-for-reverse"></a>Ters için sorgu çevirisi

Kullanan `Reverse` sorgular artık çevrilmiştir.
Örneğin:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

Dokümantasyon, sorun [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)tarafından izlenir.

### <a name="query-translation-for-bitwise-operators"></a>Bitwise işleçleri için sorgu çevirisi

Bitwise işleçleri kullanan sorgular artık daha fazla durumda tercüme edilir Örneğin:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

Dokümantasyon, sorun [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)tarafından izlenir.

### <a name="query-translation-for-strings-on-cosmos"></a>Cosmos'taki dizeleri sorgula çeviri

İçeren, StartsWith ve EndsWith dize yöntemlerini kullanan sorgular artık Azure Cosmos DB sağlayıcısını kullanırken çevrilmiştir.

Dokümantasyon, sorun [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)tarafından izlenir.
