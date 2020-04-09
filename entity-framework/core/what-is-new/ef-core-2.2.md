---
title: EF Core 2.2'deki yenilikler - EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: fb9de799753bebd7b4092cd8f4af74703dee3e45
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417449"
---
# <a name="new-features-in-ef-core-22"></a>EF Core 2.2'deki yeni özellikler

## <a name="spatial-data-support"></a>Uzamsal veri desteği

Uzamsal veriler nesnelerin fiziksel konumunu ve şeklini temsil etmek için kullanılabilir.
Birçok veritabanları yerel olarak depolayabilir, dizine ekleyebilir ve uzamsal verileri sorgulayabilir.
Yaygın senaryolar, belirli bir mesafedeki nesneler için sorgulama ve çokgenin belirli bir konum içeriyorsa test edilmesidir.
EF Core 2.2 artık [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) kitaplığından türleri kullanarak çeşitli veritabanlarından gelen uzamsal verilerle çalışmayı desteklemektedir.

Uzamsal veri desteği sağlayıcıya özgü bir dizi uzantı paketi olarak uygulanır.
Bu paketlerin her biri NTS türleri ve yöntemleri ve veritabanındaki ilgili uzamsal türleri ve işlevleri için eşlemeler katkıda bulunur.
Bu tür sağlayıcı uzantıları artık [SQL Server,](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/) [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/)ve [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) [(Npgsql projesinden)](https://www.npgsql.org/)için kullanılabilir.
Uzamsal türler ek uzantılar olmadan doğrudan [EF Core bellek sağlayıcı](xref:core/providers/in-memory/index) ile kullanılabilir.

Sağlayıcı uzantısı yüklendikten sonra, kuruluşlarınıza desteklenen türlerin özelliklerini ekleyebilirsiniz. Örneğin:

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

Daha sonra uzamsal verilerle varlıkları devam ettir:

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

Ayrıca, uzamsal verilere ve işlemlere dayalı veritabanı sorgularını yürütebilirsiniz:

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Bu özellik hakkında daha fazla bilgi [için, uzamsal türleri belgelerine](xref:core/modeling/spatial)bakın.

## <a name="collections-of-owned-entities"></a>Sahip olunan varlıkların koleksiyonları

EF Core 2.0, bire bir ilişkilendirmelerde sahipliği modelleme olanağını ekledi.
EF Core 2.2, mülkiyeti bir-çok ilişkilendirmeye ifade etme yeteneğini genişletir.
Sahiplik, varlıkların nasıl kullanıldığını kısıtlamaya yardımcı olur.

Örneğin, sahip olunan varlıklar:

- Yalnızca diğer varlık türlerinin gezinti özelliklerinde görüntülenebilir.
- Otomatik olarak yüklenir ve yalnızca sahibinin yanında bir DbContext tarafından izlenebilir.

İlişkisel veritabanlarında, sahip olunan koleksiyonlar, normal bire bir ilişki ilişkisi gibi tablolar sahibinden ayırmak üzere eşlenir.
Ancak belge yönelimli veritabanlarında, sahip olunan varlıkları (sahip olunan koleksiyonlarda veya başvurularda) sahibiyle aynı belge içinde yerleştirmeyi planlıyoruz.

Yeni OwnsMany() API'sini arayarak özelliği kullanabilirsiniz:

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

Daha fazla bilgi [için, güncelleştirilmiş sahip olunan varlıklar belgelerine](xref:core/modeling/owned-entities#collections-of-owned-types)bakın.

## <a name="query-tags"></a>Sorgu etiketleri

Bu özellik, linq sorgularının koddaki ilişkisini günlüklerde yakalanan oluşturulan SQL sorgularıyla basitleştirir.

Sorgu etiketlerinden yararlanmak için, yeni TagWith() yöntemini kullanarak bir LINQ sorgusuna açıklama eklersiniz.
Önceki örnekteki uzamsal sorguyu kullanma:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Bu LINQ sorgusu aşağıdaki SQL çıktısını üretecektir:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Daha fazla bilgi için [sorgu etiketleri belgelerine](xref:core/querying/tags)bakın.
