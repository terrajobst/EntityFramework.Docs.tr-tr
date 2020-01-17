---
title: Uzamsal veriler-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 5b45f83ca7f02665f52ccfe16b5af506a6046a62
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124437"
---
# <a name="spatial-data"></a>Uzamsal Veriler

> [!NOTE]
> Bu özellik EF Core 2,2 ' ye eklenmiştir.

Uzamsal veriler, fiziksel konumu ve nesnelerin şeklini temsil eder. Birçok veritabanı, bu tür veriler için destek sağlar, böylece diğer verilerle birlikte dizinlenebilir ve sorgulanabilir. Yaygın senaryolar, bir konumdan belirli bir uzaklıkta bulunan nesnelerin sorgulanmasını veya kenarlığını belirli bir konumu içeren nesneyi seçmeyi içerir. EF Core, [Nettopologyısuite](https://github.com/NetTopologySuite/NetTopologySuite) uzamsal kitaplığı kullanılarak uzamsal veri türlerine eşlemeyi destekler.

## <a name="installing"></a>Yükleme

Uzamsal verileri EF Core kullanmak için, uygun destekleyici NuGet paketini yüklemeniz gerekir. Yüklemeniz gereken paket, kullanmakta olduğunuz sağlayıcıya bağlıdır.

EF Core Sağlayıcısı                        | Uzamsal NuGet paketi
--------------------------------------- | ---------------------
Microsoft. EntityFrameworkCore. SqlServer | [Microsoft. EntityFrameworkCore. SqlServer. Nettopologyısuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft. EntityFrameworkCore. SQLite    | [Microsoft. EntityFrameworkCore. SQLite. Nettopologyısuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft. EntityFrameworkCore. InMemory  | [Nettopologyısuite](https://www.nuget.org/packages/NetTopologySuite)
Npgsql. EntityFrameworkCore. PostgreSQL   | [Npgsql. EntityFrameworkCore. PostgreSQL. Nettopologyısuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>Tersine mühendislik

Uzamsal NuGet paketleri de uzamsal özelliklerle [ters mühendislik](../managing-schemas/scaffolding.md) modellerini etkinleştirir, ancak `Scaffold-DbContext` veya `dotnet ef dbcontext scaffold`çalıştırmadan ***önce*** paketi yüklemeniz gerekir. Bunu yapmazsanız, sütunlar için tür eşlemelerini bulmayın hakkında uyarılar alırsınız ve sütunlar atlanır.

## <a name="nettopologysuite-nts"></a>Nettopologyısuite (bir)

Nettopologyısuite, .NET için uzamsal bir kitaplıktır. EF Core, modelinizdeki türler türlerini kullanarak veritabanındaki uzamsal veri türlerine eşlemeyi sağlar.

Uzamsal türlere, HI aracılığıyla eşlemeyi etkinleştirmek için, sağlayıcının DbContext seçenekleri oluşturucusunda Usenettopologyısuite yöntemini çağırın. Örneğin, SQL Server ile bu şekilde çağrmış olursunuz.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

Birçok uzamsal veri türü vardır. Kullandığınız tür, izin vermek istediğiniz şekillerin türüne bağlıdır. Modelinizdeki özellikler için kullanabileceğiniz, bu türlerin hiyerarşisi aşağıda verilmiştir. `NetTopologySuite.Geometries` ad alanı içinde yer alır.

* Geometrisi
  * Seçeneğinin
  * LineString
  * Gen
  * GeometryCollection
    * Çok nokta
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CurePolygon,, CompoundCurve ve tarafından desteklenmez.

Temel geometri türünü kullanmak, özelliği tarafından herhangi bir tür şeklin belirtilmesini sağlar.

Aşağıdaki varlık sınıfları, [geniş dünya içe örnek veritabanındaki](https://go.microsoft.com/fwlink/?LinkID=800630)tablolarla eşlemek için kullanılabilir.

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public Point Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public Geometry Border { get; set; }
}
```

### <a name="creating-values"></a>Değer oluşturma

Geometri nesneleri oluşturmak için oluşturucuları kullanabilirsiniz; Ancak, bunun yerine bir geometri fabrikası kullanılması önerilir. Bu, varsayılan bir SRID (Koordinatlar tarafından kullanılan uzamsal başvuru sistemi) belirtmenizi sağlar ve duyarlık modeli (hesaplamalar sırasında kullanılır) ve koordinat sırası gibi daha gelişmiş şeyler üzerinde denetim sağlar (bu boyutları belirler ve ölçüler--kullanılabilir).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326, GPS ve diğer coğrafi sistemlerde kullanılan standart olan WGS 84 ' e başvurur.

### <a name="longitude-and-latitude"></a>Boylam ve Enlem

Ormallardaki koordinatlar X ve Y değerleri bakımından yapılır. Boylam ve enlem 'yi göstermek için boylam için X kullanın ve enlem için Y kullanın. Bunun, genellikle bu değerleri görebileceğiniz `latitude, longitude` biçiminden **geriye doğru** olduğunu unutmayın.

### <a name="srid-ignored-during-client-operations"></a>İstemci işlemleri sırasında SRID yoksayıldı

, İşlemler sırasında SRID değerlerini yoksayar. Bir düzlem koordinat sistemi olduğunu varsayar. Diğer bir deyişle, boylam ve enlem açısından koordinatları belirtirseniz, uzaklık, uzunluk ve alan gibi istemci tarafından değerlendirilen bazı değerler, ölçü cinsinden değil, derece cinsinden olacaktır. Daha anlamlı değerler için, önce bu değerleri hesaplamadan önce [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) gibi bir kitaplık kullanarak koordinatları başka bir koordinat sistemine proje yapmanız gerekir.

Bir işlem SQL üzerinden EF Core tarafından değerlendiriliyorsa, sonucun birimi veritabanı tarafından belirlenir.

İki şehir arasındaki mesafeyi hesaplamak için ProjNet4GeoAPI kullanılmasına bir örnek aşağıda verilmiştir.

``` csharp
static class GeometryExtensions
{
    static readonly CoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                [4326] = GeographicCoordinateSystem.WGS84.WKT,

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

    public static Geometry ProjectTo(this Geometry geometry, int srid)
    {
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        var result = geometry.Copy();
        result.Apply(new MathTransformFilter(transformation.MathTransform));

        return result;
    }

    class MathTransformFilter : ICoordinateSequenceFilter
    {
        readonly MathTransform _transform;

        public MathTransformFilter(MathTransform transform)
            => _transform = transform;

        public bool Done => false;
        public bool GeometryChanged => true;

        public void Filter(CoordinateSequence seq, int i)
        {
            var result = _transform.Transform(
                new[]
                {
                    seq.GetOrdinate(i, Ordinate.X),
                    seq.GetOrdinate(i, Ordinate.Y)
                });
            seq.SetOrdinate(i, Ordinate.X, result[0]);
            seq.SetOrdinate(i, Ordinate.Y, result[1]);
        }
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a>Veri Sorgulama

LINQ 'ta, veritabanı işlevleri olarak kullanılabilen tüm yöntemler ve özellikler SQL 'e çevrilir. Örneğin, uzaklık ve Contains yöntemleri aşağıdaki sorgularda çevrilir. Bu makalenin sonundaki tabloda, çeşitli EF Core sağlayıcıları tarafından hangi üyelerin desteklendiği gösterilmektedir.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

SQL Server kullanıyorsanız, bilmeniz gereken bazı ek şeyler vardır.

### <a name="geography-or-geometry"></a>Coğrafya veya geometri

Varsayılan olarak, uzamsal özellikler SQL Server `geography` sütunlara eşlenir. `geometry`kullanmak için, modelinizde [sütun türünü yapılandırın](xref:core/modeling/entity-properties#column-data-types) .

### <a name="geography-polygon-rings"></a>Coğrafi Çokgen halkaları

`geography` sütun türünü kullanırken, SQL Server dış halkada (veya kabukta) ve iç halkalarda (veya delikleri) ek gereksinimler uygular. Dış halkasının saatin tersi yönde ve iç halkalar saat yönünde yönlendirilmelidir. Bu, verileri veritabanına göndermeden önce bunu doğrular.

### <a name="fullglobe"></a>FullGlobe

SQL Server, `geography` sütun türü kullanılırken tam dünyayı temsil eden standart olmayan bir geometri türüne sahiptir. Ayrıca, tam dünyaya göre (dış halka olmadan) çokgenler temsil etmenin bir yolu vardır. Bunlardan hiçbiri bu şekilde desteklenmez.

> [!WARNING]
> Bu temel alan Fulldünya ve çokgenler, bu şekilde desteklenmez.

## <a name="sqlite"></a>SQLite

SQLite kullanarak bunlar için bazı ek bilgiler aşağıda verilmiştir.

### <a name="installing-spatialite"></a>Gereksiz Ignite yükleniyor

Windows 'da yerel mod_spatialite kitaplığı, bir NuGet paketi bağımlılığı olarak dağıtılır. Diğer platformların da ayrı olarak yüklenmesi gerekir. Bu, genellikle bir yazılım paketi Yöneticisi kullanılarak yapılır. Örneğin, MacOS 'ta Ubuntu ve Homebrew üzerinde APT ' i kullanabilirsiniz.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

Ne yazık ki, PROJ 'in daha yeni sürümleri (SpatiaLite bağımlılığı) EF 'in varsayılan [SQLitePCLRaw paketiyle](/dotnet/standard/data/sqlite/custom-versions#bundles)uyumsuzdur. Bu sorunu çözmek için sistem SQLite kitaplığı kullanan özel bir [SQLitePCLRaw sağlayıcısı](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) oluşturabilir ya da proj desteğini devre dışı bırakan özel bir SpatiaLite derlemesi yükleyebilirsiniz.

``` sh
curl https://www.gaia-gis.it/gaia-sins/libspatialite-4.3.0a.tar.gz | tar -xz
cd libspatialite-4.3.0a

if [[ `uname -s` == Darwin* ]]; then
    # Mac OS requires some minor patching
    sed -i "" "s/shrext_cmds='\`test \\.\$module = .yes && echo .so \\|\\| echo \\.dylib\`'/shrext_cmds='.dylib'/g" configure
fi

./configure --disable-proj
make
make install
```

### <a name="configuring-srid"></a>SRID yapılandırma

Gereksiz bir şekilde sütun başına bir SRID belirtmesi gerekir. Varsayılan SRID `0`. ForSqliteHasSrid yöntemini kullanarak farklı bir SRID belirtin.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Boyut

SRID 'e benzer şekilde sütunun boyutu (veya ordinkılanan) de sütunun bir parçası olarak belirtilir. Varsayılan ordintlar X ve Y 'tir. ForSqliteHasDimension metodunu kullanarak ek ordinları (Z ve d) etkinleştirin.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>Çevrilmiş Işlemler

Bu tabloda, her bir EF Core sağlayıcısı tarafından SQL 'e çevrilen her bir bu üye gösterilmektedir.

Nettopologyısuite | SQL Server (geometri) | SQL Server (coğrafya) | SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
Geometry. alan | ✔ | ✔ | ✔ | ✔
Geometry. AsBinary () | ✔ | ✔ | ✔ | ✔
Geometry. AsText () | ✔ | ✔ | ✔ | ✔
Geometry. sınır | ✔ | | ✔ | ✔
Geometry. Buffer (Double) | ✔ | ✔ | ✔ | ✔
Geometry. Buffer (Double, int) | | | ✔ | ✔
Geometry. Centroıd | ✔ | | ✔ | ✔
Geometry. Contains (geometri) | ✔ | ✔ | ✔ | ✔
Geometry. ConvexHull () | ✔ | ✔ | ✔ | ✔
Geometry. kapak Edby (geometri) | | | ✔ | ✔
Geometry. kapsıyorsa (geometri) | | | ✔ | ✔
Geometry. Kesişmeleri (geometri) | ✔ | | ✔ | ✔
Geometry. fark (geometri) | ✔ | ✔ | ✔ | ✔
Geometry. Dimension | ✔ | ✔ | ✔ | ✔
Geometry. ayrık (geometri) | ✔ | ✔ | ✔ | ✔
Geometry. mesafe (geometri) | ✔ | ✔ | ✔ | ✔
Geometry. Envelope | ✔ | | ✔ | ✔
Geometry. Equalsexyasası (geometri) | | | | ✔
Geometry. EqualsTopologically (geometri) | ✔ | ✔ | ✔ | ✔
Geometry. GeometryType | ✔ | ✔ | ✔ | ✔
Geometry. Getgeometrik YN (int) | ✔ | | ✔ | ✔
Geometri. ınteriorpoint | ✔ | | ✔ | ✔
Geometry. kesişmesi (geometri) | ✔ | ✔ | ✔ | ✔
Geometry. kesişme (geometri) | ✔ | ✔ | ✔ | ✔
Geometry. IsEmpty | ✔ | ✔ | ✔ | ✔
Geometry. IsSimple | ✔ | | ✔ | ✔
Geometry. IsValid | ✔ | ✔ | ✔ | ✔
Geometry. ıswithındistance (geometri, Double) | ✔ | | ✔ | ✔
Geometry. length | ✔ | ✔ | ✔ | ✔
Geometry. Numgeometrileri | ✔ | ✔ | ✔ | ✔
Geometry. NumPoints | ✔ | ✔ | ✔ | ✔
Geometry. OgcGeometryType | ✔ | ✔ | ✔ | ✔
Geometry. örtüşüyor (geometri) | ✔ | ✔ | ✔ | ✔
Geometry. PointOnSurface | ✔ | | ✔ | ✔
Geometry. Ilişkilendir (geometri, dize) | ✔ | | ✔ | ✔
Geometry. Reverse () | | | ✔ | ✔
Geometry. SRID | ✔ | ✔ | ✔ | ✔
Geometry. SymmetricDifference (geometri) | ✔ | ✔ | ✔ | ✔
Geometry. ToBinary () | ✔ | ✔ | ✔ | ✔
Geometry. ToText () | ✔ | ✔ | ✔ | ✔
Geometry. dokunuşları (geometri) | ✔ | | ✔ | ✔
Geometry. Union () | | | ✔ | ✔
Geometry. Union (geometri) | ✔ | ✔ | ✔ | ✔
Geometri. Içinde (geometri) | ✔ | ✔ | ✔ | ✔
GeometryCollection. Count | ✔ | ✔ | ✔ | ✔
GeometryCollection [int] | ✔ | ✔ | ✔ | ✔
LineString. Count | ✔ | ✔ | ✔ | ✔
LineString. EndPoint | ✔ | ✔ | ✔ | ✔
LineString. GetPointN (int) | ✔ | ✔ | ✔ | ✔
LineString. IsClosed | ✔ | ✔ | ✔ | ✔
LineString. IsRing | ✔ | | ✔ | ✔
LineString. StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString. IsClosed | ✔ | ✔ | ✔ | ✔
Nokta. d | ✔ | ✔ | ✔ | ✔
Point. X | ✔ | ✔ | ✔ | ✔
Nokta. Y | ✔ | ✔ | ✔ | ✔
Point. Z | ✔ | ✔ | ✔ | ✔
Çokgen. ExteriorRing | ✔ | ✔ | ✔ | ✔
Çokgen. Getınteriorringn (int) | ✔ | ✔ | ✔ | ✔
Çokgen. Numinteriorhalkalar | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>Ek kaynaklar

* [SQL Server uzamsal veriler](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [Spaıalite ana sayfası](https://www.gaia-gis.it/fossil/libspatialite)
* [Npgsql uzamsal belgeleri](https://www.npgsql.org/efcore/mapping/nts.html)
* [PostGIS belgeleri](https://postgis.net/documentation/)
