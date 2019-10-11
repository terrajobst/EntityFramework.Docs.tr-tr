---
title: EF Core 2,2 ' deki yenilikler-EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: 5fcf7c6dfb4d8cb7928ef974af6deb52df7c63eb
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181370"
---
# <a name="new-features-in-ef-core-22"></a>EF Core 2,2 ' deki yeni özellikler

## <a name="spatial-data-support"></a>Uzamsal veri desteği

Uzamsal veriler, nesnelerin fiziksel konumunu ve şeklini temsil etmek için kullanılabilir.
Birçok veritabanı, uzamsal verileri yerel olarak saklayabilir, dizine alabilir ve sorgulayabilir. Yaygın senaryolar belirli bir mesafe içindeki nesnelerin sorgulanmasını ve bir çokgenin belirli bir konumu içerip içermediği test edilmesini içerir.
EF Core 2,2 artık [Nettopologyısuite](https://github.com/NetTopologySuite/NetTopologySuite) (,) kitaplığındaki türler kullanılarak çeşitli veritabanlarındaki uzamsal verilerle çalışmayı desteklemektedir.

Uzamsal veri desteği, bir dizi sağlayıcıya özgü uzantı paketleri olarak uygulanır.
Bu paketlerin her biri, türler ve yöntemler için eşlemeler ve veritabanındaki karşılık gelen uzamsal türleri ve işlevleri katkıda bulunur.
Bu tür sağlayıcı uzantıları artık [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/)ve [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) ( [npgsql projesinden](https://www.npgsql.org/)) için kullanılabilir.
Uzamsal türler, ek Uzantılar olmadan doğrudan [EF Core bellek içi sağlayıcıyla](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) birlikte kullanılabilir.

Sağlayıcı uzantısı yüklendikten sonra, varlıklarınızda desteklenen türlerin özelliklerini ekleyebilirsiniz. Örneğin:

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

Daha sonra varlıkları uzamsal verilerle kalıcı hale getirebilirsiniz:

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
Ve uzamsal verilere ve işlemlere göre veritabanı sorguları yürütebilirsiniz:

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Bu özellik hakkında daha fazla bilgi için bkz. [uzamsal türler belgeleri](xref:core/modeling/spatial). 

## <a name="collections-of-owned-entities"></a>Sahip olunan varlıkların koleksiyonları

EF Core 2,0, bire bir İlişkilendirmelerde sahiplik modelleyebilme özelliği ekledi.
EF Core 2,2, sahipliğin bire çok ilişkilerine kapsamını ifade etme yeteneğini uzatır.
Sahiplik, varlıkların nasıl kullanıldığını sınırlandırmanıza yardımcı olur.

Örneğin, sahip olunan varlıklar:
- Yalnızca diğer varlık türlerinin gezinti özelliklerinde görünebilirler. 
- Otomatik olarak yüklenir ve yalnızca sahibinden sonra bir DbContext ile izlenebilir.

İlişkisel veritabanlarında, sahip olunan koleksiyonlar, benzer bir şekilde, normal bire çok İlişkilendirmelerde olduğu gibi, sahip tarafından ayrı tablolara eşlenir.
Ancak, belge odaklı veritabanlarında, sahip olduğu aynı belge içinde sahip olunan varlıkları (sahip olan koleksiyonlar veya başvurular) iç içe bir şekilde planlıyoruz.

Yeni OwnsMany () API 'sini çağırarak özelliği kullanabilirsiniz:

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

Daha fazla bilgi için bkz. [sahip olunan varlıklar belgeleri](xref:core/modeling/owned-entities#collections-of-owned-types).

## <a name="query-tags"></a>Sorgu etiketleri

Bu özellik, günlüklerde yakalanan oluşturulmuş SQL sorguları ile koddaki LINQ sorgularının bağıntısını basitleştirir.

Sorgu etiketlerinin avantajlarından yararlanmak için New TagWith () yöntemini kullanarak bir LINQ sorgusuna açıklama ekleyebilirsiniz.
Önceki bir örnekteki uzamsal sorguyu kullanma:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Bu LINQ sorgusu aşağıdaki SQL çıktısını oluşturacak:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Daha fazla bilgi için bkz. [sorgu etiketleri belgeleri](xref:core/querying/tags). 