---
title: EF Core 2.1 - EF Core yenilikleri
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: 44cbbc965755a694772dc4336ca2c1efc51fd6cd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949237"
---
# <a name="new-features-in-ef-core-21"></a>EF Core 2.1 yeni özellikler

EF Core 2.1, çeşitli hata düzeltmeleri ve küçük işlevsel ve performans iyileştirmeleri yanı sıra, bazı ilgi çekici yeni özellikler içerir:

## <a name="lazy-loading"></a>Yavaş yükleniyor
EF Core artık herkes Gezinti özelliklerini isteğe bağlı olarak yükleyebilir, varlık sınıfları yazmak gerekli yapı taşlarını içerir. Ayrıca yeni bir paket, bu yapı taşlarını yararlanan Microsoft.EntityFrameworkCore.Proxies oluşturduk yavaş yükleniyor proxy oluşturmak için en düşük düzeyde temel sınıfları varlık sınıflarının (örneğin, sanal Gezinti özellikleri ile sınıflar) değiştirdi.

Okuma [yavaş yükleme bölümünde](xref:core/querying/related-data#lazy-loading) Bu konu hakkında daha fazla bilgi için.

## <a name="parameters-in-entity-constructors"></a>Varlık Yapıcılardaki parametreleri
Gecikmeli yükleme için gerekli yapı taşlarını biri olarak kendi oluşturucularda parametre alan bir varlık oluşturma etkinleştirdik. Özellik değerleri, yavaş yükleniyor Temsilciler ve Hizmetleri ekleme için parametreleri kullanabilirsiniz.

Okuma [varlık Oluşturucu parametrelere sahip bölüm](xref:core/modeling/constructors) Bu konu hakkında daha fazla bilgi için.

## <a name="value-conversions"></a>Değer dönüştürmeleri
Şimdiye kadar EF Core, yalnızca yerel olarak temel alınan veritabanı sağlayıcısı tarafından desteklenen tür özellikleri eşleyebilirsiniz. Değerleri sütunlar ve herhangi bir dönüştürme olmadan özellikler arasında ileri ve geri kopyalandı. EF Core 2.1 ile başlayarak, değer dönüştürmeleri özelliklerine ve tam tersi uygulanmadan önce sütunlarından elde edilen değerleri dönüştürmek için uygulanabilir. Sütunları ve özellikleri arasında özel dönüştürmeler kaydetme izin veren açık yapılandırma API yanı sıra, gerektiğinde kural tarafından uygulanan dönüştürmeler bir dizi sahibiz. Bu özellik uygulamanın bazıları şunlardır:

- Numaralandırmalar dize olarak depolama
- Eşleme işaretsiz tamsayılar SQL Server ile
- Otomatik şifreleme ve şifre çözme özellik değerleri

Okuma [değer dönüştürmeleri bölüm](xref:core/modeling/value-conversions) Bu konu hakkında daha fazla bilgi için.  

## <a name="linq-groupby-translation"></a>LINQ GroupBy çeviri
Sürüm 2.1 önce EF Core GroupBy LINQ işlecini her zaman bellekte değerlendirilmesi. GROUP BY yan tümcesine en yaygın durumlarda çevirme artık desteklenmektedir.

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

## <a name="data-seeding"></a>Veri çekirdeği oluşturma
Yeni sürümle birlikte bir veritabanını doldurmak için ilk veri sağlamak mümkün olacaktır. Farklı EF6 içinde veri dengeli dağıtım modeli yapılandırmasının bir parçası olarak bir varlık türü için ilişkilidir. EF Core geçişleri sonra otomatik olarak ne ekleme, güncelleştirme veya silme işlemleri veritabanı modelin yeni bir sürüme yükseltirken uygulanmasına gerek hesaplayabilirsiniz.

Örnek olarak, bu veri kaynağı bir Post yapılandırmak için kullanabileceğiniz `OnModelCreating`:

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

Okuma [veri üretme bölümü](xref:core/modeling/data-seeding) Bu konu hakkında daha fazla bilgi için.  

## <a name="query-types"></a>Sorgu türleri
EF Core model artık sorgu türleri içerebilir. Varlık türlerinden farklı olarak, sorgu türleri değil üzerinde tanımlanmış tuşu ve eklenmiş, silinmiş veya güncelleştirilmiş olamaz (diğer bir deyişle, bunlar salt okunur), ancak doğrudan sorgular tarafından döndürülebilir. Sorgu türleri için kullanım senaryoları bazıları şunlardır:

- Birincil anahtarları olmadan görünümlerine eşleme
- Tabloların birincil anahtarları olmadan eşleme
- Modelde tanımlanan sorguları eşleme
- İçin dönüş türü olarak hizmet veren `FromSql()` sorguları

Okuma [sorgu türleri bölümüne](xref:core/modeling/query-types) Bu konu hakkında daha fazla bilgi için.

## <a name="include-for-derived-types"></a>Türetilmiş türler
Türetilmiş türler için ifadeleri yazarken yalnızca tanımlanan Gezinti özelliklerini tanıyabilir artık olacaktır `Include` yöntemi. Kesin türü belirtilmiş sürümünü için `Include`, açık bir yayın kullanarak destekliyoruz veya `as` işleci. Ayrıca artık türetilmiş türler dize sürümünde tanımlanan gezinti özelliği adlarını başvuran destekliyoruz `Include`:

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Okuma [INCLUDE bölüm türetilen türlerle](xref:core/querying/related-data#include-on-derived-types) Bu konu hakkında daha fazla bilgi için.

## <a name="systemtransactions-support"></a>System.Transactions desteği
TransactionScope gibi System.Transactions özelliklerle çalışma olanağı ekledik. Bu hem .NET Framework hem de .NET Core destekleyen veritabanı sağlayıcıları kullanırken çalışır.

Okuma [System.Transactions bölüm](xref:core/saving/transactions#using-systemtransactions) Bu konu hakkında daha fazla bilgi için.

## <a name="better-column-ordering-in-initial-migration"></a>İlk geçişinde sıralama daha iyi sütun
Müşteri geri bildirimi doğrultusunda, özellikleri, sınıflarda bildirilen başlangıçta tablolar için aynı sırada vygenerovat sloupce geçişlerini güncelleştirdik. EF Core ilk tablo oluşturulduktan sonra yeni üye eklendiğinde sipariş değiştiremeyeceğinizi unutmayın.

## <a name="optimization-of-correlated-subqueries"></a>Bağıntılı alt sorgularda en iyi duruma getirilmesi
"N + 1" yürütme önlemek için sunduğumuz sorgu çevirisi geliştirildi SQL sorgularında projeksiyon Gezinti özelliğinde kullanımını müşteri adayları verilerle ilişkili alt sorgu kök sorgudan verileri birleştirme için birçok yaygın senaryo. En iyi duruma getirme, form alt sorgu sonuçları arabelleğe alma ve yeni davranış katılımı için sorguyu değiştirin gerektirir gerektirir.

Örnek olarak, aşağıdaki sorguyu normalde bir sorguya müşterileri, ayrıca (burada döndürülen müşteri sayısı, "N") N çevrilir sorguları siparişlerini ayırın:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

Ekleyerek `ToList()` doğru yerde arabelleğe alma iyileştirmeyi etkinleştirme siparişleri için uygun olduğunu gösterir:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Bu sorgu için yalnızca iki SQL sorguları çevrilir unutmayın: biri müşteriler ve siparişler için sonraki komutu.

## <a name="owned-attribute"></a>[Ait] özniteliği

Şimdi yapılandırmak mümkün [varlık türlerine ait](xref:core/modeling/owned-entities) yalnızca türüyle yorumlama tarafından `[Owned]` ve sahibi varlık emin olduktan sonra modele eklenir:

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

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a>Komut satırı aracı dotnet-ef .NET Core SDK'sına dahil

_Dotnet-ef_ komutları artık .NET Core SDK'sının parçası, bu nedenle, artık DotNetCliToolReference geçişler kullanın veya var olan bir veritabanından bir DbContext iskelesini kullanabilmek için projede kullanmak için gerekli olacaktır.

Bölümüne [araçlarını yükleme](xref:core/miscellaneous/cli/dotnet#installing-the-tools) EF Core ve .NET Core SDK'ün farklı sürümleri için komut satırı araçları'nı etkinleştirme hakkında daha fazla ayrıntı için.

## <a name="microsoftentityframeworkcoreabstractions-package"></a>Microsoft.EntityFrameworkCore.Abstractions paket
Yeni paket öznitelikleri ve projelerinizde kullanan bir bütün olarak EF Core üzerinde bir bağımlılık almadan EF Core özellikleri açık arabirimler içerir. Örneğin, [ait] özniteliği ve ILazyLoader arabirimi burada yer alır.

## <a name="state-change-events"></a>Durum değişikliği olayları

Yeni `Tracked` ve `StateChanged` olaylarına `ChangeTracker` DbContext girme ve durumlarını değiştirerek varlıklara tepki verdiğini mantığı yazmak için kullanılır.

## <a name="raw-sql-parameter-analyzer"></a>Ham SQL parametresi Çözümleyicisi

Yeni bir kod Çözümleyicisi EF Core ile SQL ham apı'lerimizi güvensiz kullanımı gibi algılar dahil edilen `FromSql` veya `ExecuteSqlCommand`. Örneğin, aşağıdaki sorgu için bir uyarı çünkü göreceğiniz _minAge_ değil parametreli:

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a>Veritabanı sağlayıcısı uyumluluğu

Güncelleştirilen veya EF Core 2.1 ile çalışmak üzere en az test sağlayıcıları ile EF Core 2.1 kullanmanız önerilir.

> [!TIP]
> Herhangi bir beklenmeyen bulursanız uyumsuzluk herhangi sorun yeni özellikler veya bunlar üzerinde geri bildirimde bulunmak istiyorsanız lütfen kullanarak rapor [müşterilerimize sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues/new).
