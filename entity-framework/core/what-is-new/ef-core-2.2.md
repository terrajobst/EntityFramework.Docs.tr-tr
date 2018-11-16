---
title: EF Core 2.2 - EF Core yenilikleri
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: 79b4efc3aee23e19a9ea1deb6373b9984b77f886
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688807"
---
# <a name="new-features-in-ef-core-22"></a>EF Core 2.2 yeni özellikler

## <a name="spatial-data-support"></a>Uzamsal veri desteği

Uzamsal veriler fiziksel konuma ve nesnelerin şeklini temsil etmek için kullanılabilir.
Yerel olarak çok sayıda veritabanında depolamak, dizin ve uzamsal verileri sorgulama. Yaygın senaryolar şunlardır: belirli bir uzaklık içindeki nesneler için sorgulama ve belirli bir konuma bir Çokgen içeriyorsa, test etme.
EF Core 2.2 destekler türlerinden kullanarak çeşitli veritabanları uzamsal verilerle çalışma [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) kitaplığı.

Uzamsal veri desteği, sağlayıcıya özgü uzantı paketleri bir dizi olarak uygulanır.
Bu paketlerin her NTS türleri ve yöntemleri ve karşılık gelen uzamsal türleri ve işlevleri veritabanında eşlemelerini katkıda bulunur.
Bu tür sağlayıcısı uzantılar için kullanıma sunulmuştur [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), ve [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (gelen [Npgsql proje](http://www.npgsql.org/)).
Uzamsal türleri, doğrudan kullanılabilir [EF Core bellek içi sağlayıcısı](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) ek uzantıları olmadan.

Sağlayıcı uzantısı yüklendikten sonra desteklenen tür özellikleri için varlıklarınızı ekleyebilirsiniz. Örneğin:

``` csharp
using NetTopologySuite.Geometries;

namespace MyApp
{
  public class Friend
  {
    [Key]
    public string Name { get; set; }
  
    [Required]
    public Point Location { get; set; }
  }
}
``` 

Ardından, uzamsal verileri içeren varlıklar devam edebilir:

``` csharp
using (var context = new MyDbContext())
{
    context.Add(
        new Friend
        {
            Name = "Bill",
            Location = new Point(-122.34877, 47.6233355) {SRID = 4326 }
        });
    context.SaveChanges();
}
```
Ve uzamsal verileri ve işlemleri temel alan veritabanı sorgularını yürütebilirsiniz:

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Bu özellik hakkında daha fazla bilgi için bkz. [uzamsal türler belgeleri](xref:core/modeling/spatial). 

## <a name="collections-of-owned-entities"></a>Sahip olunan varlık koleksiyonları

EF Core 2.0 için bire bir ilişkileri modeli sahip olma özelliği eklendi.
EF Core 2.2-çok ilişkileri sahipliği express olanağı genişletir.
Sahipliği varlıkları nasıl kullanıldığını sınırlamak yardımcı olur.

Örneğin, sahip olunan varlık:
- Gezinti özellikleri diğer varlık türleri yalnızca görünür. 
- Otomatik olarak yüklenir ve yalnızca bir DbContext birlikte bunların sahibi tarafından izlenebilir.

İlişkisel veritabanları, sahibi, normal bir-çok ilişkileri gibi tablolar ayırmak için sahip olduğu koleksiyonları eşlenir.
Ancak, belge yönelimli veritabanlarında, sahip olunan varlıklar (sahibi koleksiyonları veya başvuruları) aynı belge sahibi olarak içinde iç içe planlıyoruz.

Yeni OwnsMany() API'sini çağırarak özelliğini kullanabilirsiniz:

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

Daha fazla bilgi için [sahip olunan varlık belgeleri güncelleştirildi](xref:core/modeling/owned-entities#collections-of-owned-types).

## <a name="query-tags"></a>Sorgu etiketleri

Bu özellik, kodu, LINQ sorgularında günlüklerde yakalanan oluşturulan SQL sorgularının bağıntısını basitleştirir.

Sorgu etiketleri yararlanmak için yeni TagWith() yöntemini kullanarak bir LINQ Sorgu açıklama ekleyin.
Önceki örnekte uzamsal sorgu kullanarak:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Bu LINQ sorgusu SQL aşağıdaki çıktıyı üretir:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Daha fazla bilgi için [sorgu etiketleri belgeleri](xref:core/querying/tags). 