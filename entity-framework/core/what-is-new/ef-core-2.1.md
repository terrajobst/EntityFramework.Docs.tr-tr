---
title: "EF çekirdek 2.1 - EF çekirdek yenilikler nelerdir?"
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: 1e5e9839bae1e5da4082d90c02d098bb3b2b43bd
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="new-features-in-ef-core-21"></a>EF çekirdek 2.1 yeni özellikler
> [!NOTE]  
> Bu sürüm hala önizlemede değil.

Çok sayıda küçük geliştirmeleri ve birden fazla yüzlerce ürün hata düzeltmeleri yanı sıra, EF çekirdek 2.1 çeşitli yeni özellikler içerir:

## <a name="lazy-loading"></a>yavaş yükleniyor
EF çekirdek şimdi herkes, gezinti özellikleri isteğe bağlı olarak yükleyebilir varlık sınıfları yazmak gerekli yapı taşlarını içerir. Bu yapı taşları yararlanır Microsoft.EntityFrameworkCore.Proxies, yeni bir paket de oluşturduk yavaş yükleniyor proxy üretmek için en düşük düzeyde temel sınıfları sınıflar (örneğin sanal Gezinti özellikleri sınıflarıyla) değiştirdi.

Okuma [geç yükleme bölümünde](xref:core/querying/related-data#lazy-loading) Bu konu hakkında daha fazla bilgi için.

## <a name="parameters-in-entity-constructors"></a>Varlık oluşturucuları parametrelerinde
Geç yükleme için gerekli yapı taşları biri olarak size, kendi kurucularda parametre almaz varlıkları oluşturma etkin. Özellik değerleri, yavaş yükleniyor Temsilciler ve Hizmetleri eklemesine parametrelerini kullanabilirsiniz.

Okuma [varlık Oluşturucusu parametrelerle bölüm](xref:core/modeling/constructors) Bu konu hakkında daha fazla bilgi için.

## <a name="value-conversions"></a>Değer dönüşümler
Şimdiye kadar EF çekirdek yalnızca yerel olarak temel alınan veritabanı sağlayıcısı tarafından desteklenen türlerin özellikleri eşleyin. Değerleri, sütunları ve herhangi bir dönüştürme olmadan özellikleri arasında ileri ve geri kopyalandı. EF çekirdek 2.1 ile başlayarak, değer dönüşümler özelliklerine ve tersi yönde uygulanmadan önce sütunlarından elde edilen değerleri dönüştürmek için uygulanabilir. Sütunları ve özellikleri arasında özel dönüştürmeler kaydetme imkan tanıyan bir açık yapılandırma API'si yanı sıra, gerekli olarak kural tarafından uygulanan dönüşümleri sayısı sunuyoruz. Bu özellik uygulama bazıları şunlardır:

- Numaralandırmalar dizeleri depolama
- Tamsayıları SQL Server ile eşleme imzalanmamış
- Otomatik şifreleme ve şifre çözme özellik değeri

Okuma [değer dönüşümler bölüm](xref:core/modeling/value-conversions) Bu konu hakkında daha fazla bilgi için.  

## <a name="linq-groupby-translation"></a>LINQ GroupBy çevirisi
GroupBy LINQ işleci her zaman olduğu EF çekirdek 2.1 sürümünde bellekte değerlendirilmesi önce. GROUP BY yan tümcesine en yaygın durumlarda çevirme artık destekler.

Bu örnek bir sorgu ile çeşitli toplama işlevleri hesaplamak için kullanılan GroupBy gösterir:

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => Amount)
        });
```

Karşılık gelen SQL çeviri şöyle görünür:

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a>Dengeli veri
Yeni sürümde bir veritabanını doldurmak için ilk veri sağlamak mümkün olacaktır. EF6 içinde verileri dengeli modeli yapılandırmasının bir parçası olarak bir varlık türü için ilişkili benzemez. Ardından EF çekirdek geçişler otomatik olarak ne ekleme, güncelleştirme veya silme işlemleri veritabanı modeli yeni bir sürüme yükseltirken uygulanması gerek hesaplayabilirsiniz.

Örnek olarak, bu bir POST çekirdek verileri yapılandırmak için kullanabileceğiniz `OnModelCreating`:

``` csharp
modelBuilder.Entity<Post>().SeedData(new Post{ Id = 1, Text = "Hello World!" });
```

Okuma [verileri dengeli üzerinde bölüm](xref:core/modeling/data-seeding) Bu konu hakkında daha fazla bilgi için.  

## <a name="query-types"></a>Sorgu türleri
EF çekirdek modeli şimdi sorgu türleri içerebilir. Varlık türlerinin aksine, sorgu türleri değil sahip üzerlerinde tanımlanmış anahtarları ve eklenen, silinemez veya güncelleştirilmiş (yani, salt okunur), ancak doğrudan sorgular tarafından döndürdü. Sorgu türleri için kullanım senaryoları bazıları şunlardır:

- Birincil anahtarlar olmadan görünümlerine eşleme
- Birincil anahtarlar olmadan tablolara eşleme
- Model içinde tanımlanan sorgulara eşleme
- Dönüş türü olarak hizmet veren `FromSql()` sorguları

Okuma [sorgu türleri bölümüne](xref:core/modeling/query-types) Bu konu hakkında daha fazla bilgi için.

## <a name="include-for-derived-types"></a>Türetilen türler için
Yalnızca tanımlanan Gezinti özelliklerini belirtmek için olası türetilmiş tür ifadeler için yazarken artık olacaktır `Include` yöntemi. Kesin türü belirtilmiş sürümünü için `Include`, açık bir atama kullanarak destekliyoruz veya `as` işleci. Ayrıca artık adları türetilmiş türler dize sürümünde tanımlı Gezinti özelliğinin başvuru destekliyoruz `Include`:

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Okuma [INCLUDE bölüm türetilen türlerle](xref:core/querying/related-data#include-on-derived-types) Bu konu hakkında daha fazla bilgi için.

## <a name="systemtransactions-support"></a>System.Transactions desteği
System.Transactions TransactionScope gibi özellikler ile çalışma olanağını ekledik. Bu hem .NET Framework hem de .NET Core destekleyen veritabanı sağlayıcıları kullanırken çalışır.

Okuma [System.Transactions bölüm](xref:core/saving/transactions#using-systemtransactions) Bu konu hakkında daha fazla bilgi için.

## <a name="better-column-ordering-in-initial-migration"></a>Daha iyi sütun ilk geçişte sıralaması
Müşteri geri bildirimi doğrultusunda, sınıflar olarak bildirilen özelliklerde gibi tablolar için sütunları aynı sırada başlangıçta oluşturmak için geçişler güncelleştirdiniz. İlk tablo oluşturulduktan sonra yeni üye eklediğinizde EF çekirdek sırası değiştirilemiyor unutmayın.

## <a name="optimization-of-correlated-subqueries"></a>Bağıntılı alt sorgulara en iyi duruma getirme
Biz "N + 1" yürütme önlemek için bizim sorgusu çevirisi iyileştirilmiştir projeksiyon Gezinti özelliğinde kullanımını müşteri adayları verileriyle ilişkili bir alt sorgu kök sorgudan verileri birleştirme için birçok yaygın senaryolar SQL sorguları. En iyi duruma getirme form alt sorgu sonuçları arabelleğe alma ve katılımı yeni davranışı için sorguyu değiştirin gerektiren gerektirir.

Örnek olarak, aşağıdaki sorguyu normalde bir sorguyu müşteriler artı (burada "N" döndürülen müşteri sayısını alır) çevrilir sorgular için siparişleri ayırın:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

Ekleyerek `ToList()` doğru yerde arabelleğe alma iyileştirmeyi etkinleştirme siparişleri için uygun olduğunu gösteriyor:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Bu sorgu için yalnızca iki SQL sorguları çevrilir unutmayın: biri müşteriler ve siparişler bir sonraki.

### <a name="ownedattribute"></a>OwnedAttribute

Şimdi yapılandırmak olası [varlık türlerine ait](xref:core/modeling/owned-entities) yalnızca türüyle yorumlama tarafından `[Owned]` ve sahibi varlık emin olmak için model eklenir:

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="database-provider-compatibility"></a>Veritabanı sağlayıcısı uyumluluğu

EF çekirdek 2.1 EF çekirdek 2.0 için oluşturulan veritabanı sağlayıcıları ile uyumlu olacak şekilde tasarlanmıştır. (Örn. değer dönüşümler) açıklanan özelliklerden bazıları güncellenmiş bir sağlayıcı gerektirirken başkalarının (örneğin yavaş yükleniyor) mevcut sağlayıcılarıyla yanar.

> [!TIP]
> Her beklenmeyen bulursanız uyumsuzluk herhangi sorun'deki yeni özelliklerin veya bunlar üzerinde Geribildiriminiz varsa lütfen kullanarak rapor [bizim sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues/new).
