---
title: Uzamsal veriler - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: cf488c6b7d94ca19018efe1c23ff410fe7eb594b
ms.sourcegitcommit: 81c53ac43d8f15b900f117294ec71dc49fe028fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51817916"
---
# <a name="spatial-data"></a>Uzamsal veriler

> [!NOTE]
> Bu özellik, EF Core 2.2 içinde yeni bir özelliktir.

Uzamsal veriler fiziksel konuma ve nesnelerin şeklini temsil eder. Çok sayıda veritabanı bu tür veriler için destek sağlar, böylece dizine ve yanı sıra diğer veriler sorgulandı. Bir konumdan belirli bir uzaklık içindeki nesneler için sorgulama ve belirli bir konuma kenarlığını içeren nesneyi yaygın senaryolar şunlardır. EF Core destekler kullanarak uzamsal veri türleri için eşleme [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) uzamsal kitaplığı.

## <a name="installing"></a>Yükleme

Uzamsal veriler EF Core ile kullanmak için uygun destek NuGet paketini yüklemeniz gerekir. Hangi paketini yüklemeniz gerekir, kullanmakta olduğunuz sağlayıcısına bağlıdır.

EF Core sağlayıcısı                        | Uzamsal NuGet paketi
--------------------------------------- | ---------------------
Microsoft.EntityFrameworkCore.SqlServer | [Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft.EntityFrameworkCore.Sqlite    | [Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft.EntityFrameworkCore.InMemory  | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite)
Npgsql.EntityFrameworkCore.PostgreSQL   | [Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>Tersine mühendislik

Uzamsal NuGet ayrıca etkinleştir paketleri [tersine mühendislik](../managing-schemas/scaffolding.md) uzamsal özellikler, ancak modelleriyle gereksinim paketi yüklemek ***önce*** çalıştıran `Scaffold-DbContext` veya `dotnet ef dbcontext scaffold`. Aksi takdirde, türü eşlemeleri sütunlarını paylaşımın hakkında uyarılar alırsınız ve sütunları atlanacak.

## <a name="nettopologysuite-nts"></a>NetTopologySuite (NTS)

NetTopologySuite, .NET için uzamsal bir kitaplıktır. EF Core modelinizde NTS türlerini kullanarak uzamsal veri türleri veritabanındaki eşleme sağlar.

Kesme noktalarını aracılığıyla uzamsal türler için eşleme etkinleştirmek için sağlayıcının DbContext seçenekleri Oluşturucusu'UseNetTopologySuite yöntemi çağırın. Örneğin, SQL Server ile Bunu şöyle çağırmanız.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

Birkaç uzamsal veri türleri vardır. Hangi türü kullandığınız izin vermek istediğiniz şekillerinin türlerine bağlıdır. Modelinizde özellikleri için kullanabileceğiniz NTS türlerin hiyerarşisi aşağıda verilmiştir. Bunlar içinde bulunduğu `NetTopologySuite.Geometries` ad alanı. İlgili arabirimlere GeoAPI paketindeki (`GeoAPI.Geometries` ad alanı) de kullanılabilir.

* Geometri
  * Noktası
  * LineString
  * Çokgen
  * GeometryCollection
    * MultiPoint
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString, CompoundCurve ve CurePolygon NTS tarafından desteklenmez.

Temel geometrik türünü kullanarak her türlü özelliği tarafından belirtilen şeklin sağlar.

Aşağıdaki varlık sınıflarının tabloları eşleştirmek için kullanılabilecek [Wide World Importers örnek veritabanını](http://go.microsoft.com/fwlink/?LinkID=800630).

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public IPoint Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public IGeometry Border { get; set; }
}
```

### <a name="creating-values"></a>Değerleri oluşturma

Oluşturucular, geometri nesneleri oluşturmak için kullanabilirsiniz; Ancak, geometri Fabrika kullanmayı NTS önerir. Bu varsayılan SRID (koordinatları tarafından kullanılan uzamsal başvuru sistemi) belirtmenize olanak tanır ve size (hesaplama sırasında kullanılır) duyarlılığı modeli ve (hangi ordinates--boyutları belirler koordinat dizisi gibi daha gelişmiş işlemler üzerinde denetim ve ölçüler--kullanılabilir).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326 WGS 84, GPS ve diğer coğrafi sistemlerinde kullanılan standart bir ifade eder.

### <a name="longitude-and-latitude"></a>Boylam ve enlem

Kesme noktalarını koordinatlarında olan açısından X ve Y değerleri. Enlemini ve boylamını temsil etmek için X boylam ve Y enlem için kullanın. Bu Not **geriye doğru** gelen `latitude, longitude` bu değerleri genellikle görebileceğiniz biçimi.

### <a name="srid-ignored-during-client-operations"></a>İstemci işlemleri sırasında SRID yoksayıldı

Kesme noktalarını işlemleri sırasında SRID değerleri yok sayar. Bu, bir düzlem koordinat sistemi varsayar. Boylam ve enlem, olmayan ölçümleri derece cinsinden uzaklık ve uzunluk alanı gibi bazı istemci hesaplanan değerler açısından koordinatları belirtirseniz anlamına gelir. Daha anlamlı değerleri için önce kullanarak bir kitaplığı gibi başka bir koordinat sistemini koordinatları proje gerekir [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) önce bu değerleri hesaplama.

Bir işlem tarafından EF Core ile SQL server olarak değerlendirilen olursa sonucunun birim veritabanı tarafından belirlenir.

İki şehirler arasındaki uzaklığı hesaplamak için ProjNet4GeoAPI kullanmaya bir örnek aşağıda verilmiştir.

``` csharp
static class GeometryExtensions
{
    static readonly IGeometryServices _geometryServices = NtsGeometryServices.Instance;
    static readonly ICoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                // (3857 and 4326 included automatically)

                // This coordinate system covers the area of our data.
                // Different data requires a different coordinate system.
                [2855] =
                @"
                    PROJCS[""NAD83(HARN) / Washington North"",
                        GEOGCS[""NAD83(HARN)"",
                            DATUM[""NAD83_High_Accuracy_Regional_Network"",
                                SPHEROID[""GRS 1980"",6378137,298.257222101,
                                    AUTHORITY[""EPSG"",""7019""]],
                                AUTHORITY[""EPSG"",""6152""]],
                            PRIMEM[""Greenwich"",0,
                                AUTHORITY[""EPSG"",""8901""]],
                            UNIT[""degree"",0.01745329251994328,
                                AUTHORITY[""EPSG"",""9122""]],
                            AUTHORITY[""EPSG"",""4152""]],
                        PROJECTION[""Lambert_Conformal_Conic_2SP""],
                        PARAMETER[""standard_parallel_1"",48.73333333333333],
                        PARAMETER[""standard_parallel_2"",47.5],
                        PARAMETER[""latitude_of_origin"",47],
                        PARAMETER[""central_meridian"",-120.8333333333333],
                        PARAMETER[""false_easting"",500000],
                        PARAMETER[""false_northing"",0],
                        UNIT[""metre"",1,
                            AUTHORITY[""EPSG"",""9001""]],
                        AUTHORITY[""EPSG"",""2855""]]
                "
            });

    public static IGeometry ProjectTo(this IGeometry geometry, int srid)
    {
        var geometryFactory = _geometryServices.CreateGeometryFactory(srid);
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        return GeometryTransform.TransformGeometry(
            geometryFactory,
            geometry,
            transformation.MathTransform);
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a>Veri Sorgulama

LINQ to SQL NTS yöntemleri ve özellikleri veritabanı işlevleri kullanılabilir çevrilir. Örneğin, içerir ve uzaklık yöntemleri aşağıdaki sorgularda çevrilir. Bu makalenin sonundaki tabloda, çeşitli EF Core sağlayıcıları tarafından desteklenen üyeleri gösterir.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

SQL Server kullanıyorsanız, bilmeniz gereken bazı ek işlemler vardır.

### <a name="geography-or-geometry"></a>Coğrafya veya geometri

Varsayılan olarak, uzamsal özellikler için eşlenen `geography` SQL Server'daki sütun. Kullanılacak `geometry`, [sütun türü yapılandırma](xref:core/modeling/relational/data-types) modelinizdeki.

### <a name="geography-polygon-rings"></a>Coğrafya Çokgen halkaları

Kullanırken `geography` sütun türü, SQL Server, dış halkası (veya kabuk) ek gereksinimler uygular ve iç halkaları (veya açıkları). Halkasının saat yönünün tersine yönelimli olması gerekir ve iç saat yönünde halkası. Kesme noktalarını bu değerleri veritabanına göndermeden önce doğrular.

### <a name="fullglobe"></a>FullGlobe

SQL Server kullanırken tam dünya temsil etmek için bir standart geometri türü var. `geography` sütun türü. Ayrıca, tam dünya (olmadan bir halkasının) göre çokgenleri temsil etmek için bir yol vardır. Hiçbirini NTS tarafından desteklenir.

> [!WARNING]
> FullGlobe ve temel alan çokgenler NTS tarafından desteklenmez.

## <a name="sqlite"></a>SQLite

SQLite kullananlar için bazı ek bilgiler aşağıda verilmiştir.

### <a name="installing-spatialite"></a>SpatiaLite yükleme

Windows üzerinde yerel mod_spatialite kitaplığı NuGet paket bağımlılık olarak dağıtılır. Diğer platformlar ayrı olarak yüklemeniz gerekir. Bu genellikle yapılır bir yazılım paketi Yöneticisi'ni kullanarak. Örneğin, Ubuntu ve MacOS üzerinde Homebrew APT kullanabilirsiniz.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a>SRID yapılandırma

SpatiaLite içinde sütunlar sütun başına bir SRID belirtmeniz gerekir. SRID olan varsayılan `0`. ForSqliteHasSrid yöntemi kullanarak farklı bir SRID belirtin.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Boyut

Benzer şekilde SRID, bir sütunun boyut (veya ordinates) da sütunu bir parçası olarak belirtilir. Varsayılan ordinates olduğu X ve y etkinleştir ek ordinates (Z ve M) kullanarak ForSqliteHasDimension yöntemi.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>Çevrilmiş işlemleri

Bu tablo, hangi NTS üyeleri SQL'e her EF Core sağlayıcısı tarafından çevrilir gösterir.

NetTopologySuite | SQL Server (geometri) | SQL Server (regioncountryname) | SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
Geometry.Area | ✔ | ✔ | ✔ | ✔
Geometry.AsBinary() | ✔ | ✔ | ✔ | ✔
Geometry.AsText() | ✔ | ✔ | ✔ | ✔
Geometry.Boundary | ✔ | | ✔ | ✔
Geometry.Buffer(double) | ✔ | ✔ | ✔ | ✔
Geometry.Buffer (double, int) | | | ✔
Geometry.Centroid | ✔ | | ✔ | ✔
Geometry.Contains(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ConvexHull() | ✔ | ✔ | ✔ | ✔
Geometry.CoveredBy(Geometry) | | | ✔ | ✔
Geometry.Covers(Geometry) | | | ✔ | ✔
Geometry.Crosses(Geometry) | ✔ | | ✔ | ✔
Geometry.Difference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Dimension | ✔ | ✔ | ✔ | ✔
Geometry.Disjoint(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Distance(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Envelope | ✔ | | ✔ | ✔
Geometry.EqualsExact(Geometry) | | | | ✔
Geometry.EqualsTopologically(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.GeometryType | ✔ | ✔ | ✔ | ✔
Geometry.GetGeometryN(int) | ✔ | | ✔ | ✔
Geometry.InteriorPoint | ✔ | | ✔
Geometry.Intersection(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Intersects(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.IsEmpty | ✔ | ✔ | ✔ | ✔
Geometry.IsSimple | ✔ | | ✔ | ✔
Geometry.IsValid | ✔ | ✔ | ✔ | ✔
Geometry.IsWithinDistance (geometri, çift) | ✔ | | ✔
Geometry.Length | ✔ | ✔ | ✔ | ✔
Geometry.NumGeometries | ✔ | ✔ | ✔ | ✔
Geometry.NumPoints | ✔ | ✔ | ✔ | ✔
Geometry.OgcGeometryType | ✔ | ✔ | ✔
Geometry.Overlaps(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.PointOnSurface | ✔ | | ✔ | ✔
Geometry.Relate (geometri, string) | ✔ | | ✔ | ✔
Geometry.Reverse() | | | ✔ | ✔
Geometry.SRID | ✔ | ✔ | ✔ | ✔
Geometry.SymmetricDifference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ToBinary() | ✔ | ✔ | ✔ | ✔
Geometry.ToText() | ✔ | ✔ | ✔ | ✔
Geometry.Touches(Geometry) | ✔ | | ✔ | ✔
Geometry.Union() | | | ✔
Geometry.Union(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Within(Geometry) | ✔ | ✔ | ✔ | ✔
GeometryCollection.Count | ✔ | ✔ | ✔ | ✔
GeometryCollection [int] | ✔ | ✔ | ✔ | ✔
LineString.Count | ✔ | ✔ | ✔ | ✔
LineString.EndPoint | ✔ | ✔ | ✔ | ✔
LineString.GetPointN(int) | ✔ | ✔ | ✔ | ✔
LineString.IsClosed | ✔ | ✔ | ✔ | ✔
LineString.IsRing | ✔ | | ✔ | ✔
LineString.StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString.IsClosed | ✔ | ✔ | ✔ | ✔
Point.M | ✔ | ✔ | ✔ | ✔
Point.X | ✔ | ✔ | ✔ | ✔
Point.Y | ✔ | ✔ | ✔ | ✔
Point.Z | ✔ | ✔ | ✔ | ✔
Polygon.ExteriorRing | ✔ | ✔ | ✔ | ✔
Polygon.GetInteriorRingN(int) | ✔ | ✔ | ✔ | ✔
Polygon.NumInteriorRings | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>Ek kaynaklar

* [SQL Server uzamsal verileri](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [SpatiaLite giriş sayfası](https://www.gaia-gis.it/fossil/libspatialite)
* [Npgsql uzamsal belgeleri](http://www.npgsql.org/efcore/mapping/nts.html)
* [PostGIS belgeleri](http://postgis.net/documentation/)
