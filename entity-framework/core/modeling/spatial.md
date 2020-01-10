---
title: Uzamsal veriler-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 8dae1ab949c77ffa08904b12a5716b729e6913a1
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502246"
---
# <a name="spatial-data"></a><span data-ttu-id="823e0-102">Uzamsal veriler</span><span class="sxs-lookup"><span data-stu-id="823e0-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="823e0-103">Bu özellik EF Core 2,2 ' ye eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="823e0-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="823e0-104">Uzamsal veriler, fiziksel konumu ve nesnelerin şeklini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="823e0-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="823e0-105">Birçok veritabanı, bu tür veriler için destek sağlar, böylece diğer verilerle birlikte dizinlenebilir ve sorgulanabilir.</span><span class="sxs-lookup"><span data-stu-id="823e0-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="823e0-106">Yaygın senaryolar, bir konumdan belirli bir uzaklıkta bulunan nesnelerin sorgulanmasını veya kenarlığını belirli bir konumu içeren nesneyi seçmeyi içerir.</span><span class="sxs-lookup"><span data-stu-id="823e0-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="823e0-107">EF Core, [Nettopologyısuite](https://github.com/NetTopologySuite/NetTopologySuite) uzamsal kitaplığı kullanılarak uzamsal veri türlerine eşlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="823e0-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="823e0-108">Yükleme</span><span class="sxs-lookup"><span data-stu-id="823e0-108">Installing</span></span>

<span data-ttu-id="823e0-109">Uzamsal verileri EF Core kullanmak için, uygun destekleyici NuGet paketini yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="823e0-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="823e0-110">Yüklemeniz gereken paket, kullanmakta olduğunuz sağlayıcıya bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="823e0-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="823e0-111">EF Core Sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="823e0-111">EF Core Provider</span></span>                        | <span data-ttu-id="823e0-112">Uzamsal NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="823e0-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="823e0-113">Microsoft. EntityFrameworkCore. SqlServer</span><span class="sxs-lookup"><span data-stu-id="823e0-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="823e0-114">Microsoft. EntityFrameworkCore. SqlServer. Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="823e0-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="823e0-115">Microsoft. EntityFrameworkCore. SQLite</span><span class="sxs-lookup"><span data-stu-id="823e0-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="823e0-116">Microsoft. EntityFrameworkCore. SQLite. Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="823e0-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="823e0-117">Microsoft. EntityFrameworkCore. InMemory</span><span class="sxs-lookup"><span data-stu-id="823e0-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="823e0-118">Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="823e0-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="823e0-119">Npgsql. EntityFrameworkCore. PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="823e0-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="823e0-120">Npgsql. EntityFrameworkCore. PostgreSQL. Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="823e0-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="823e0-121">Tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="823e0-121">Reverse engineering</span></span>

<span data-ttu-id="823e0-122">Uzamsal NuGet paketleri de uzamsal özelliklerle [ters mühendislik](../managing-schemas/scaffolding.md) modellerini etkinleştirir, ancak `Scaffold-DbContext` veya `dotnet ef dbcontext scaffold`çalıştırmadan ***önce*** paketi yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="823e0-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="823e0-123">Bunu yapmazsanız, sütunlar için tür eşlemelerini bulmayın hakkında uyarılar alırsınız ve sütunlar atlanır.</span><span class="sxs-lookup"><span data-stu-id="823e0-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="823e0-124">Nettopologyısuite (bir)</span><span class="sxs-lookup"><span data-stu-id="823e0-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="823e0-125">Nettopologyısuite, .NET için uzamsal bir kitaplıktır.</span><span class="sxs-lookup"><span data-stu-id="823e0-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="823e0-126">EF Core, modelinizdeki türler türlerini kullanarak veritabanındaki uzamsal veri türlerine eşlemeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="823e0-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="823e0-127">Uzamsal türlere, HI aracılığıyla eşlemeyi etkinleştirmek için, sağlayıcının DbContext seçenekleri oluşturucusunda Usenettopologyısuite yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="823e0-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="823e0-128">Örneğin, SQL Server ile bu şekilde çağrmış olursunuz.</span><span class="sxs-lookup"><span data-stu-id="823e0-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="823e0-129">Birçok uzamsal veri türü vardır.</span><span class="sxs-lookup"><span data-stu-id="823e0-129">There are several spatial data types.</span></span> <span data-ttu-id="823e0-130">Kullandığınız tür, izin vermek istediğiniz şekillerin türüne bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="823e0-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="823e0-131">Modelinizdeki özellikler için kullanabileceğiniz, bu türlerin hiyerarşisi aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="823e0-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="823e0-132">`NetTopologySuite.Geometries` ad alanı içinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="823e0-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="823e0-133">Geometrisi</span><span class="sxs-lookup"><span data-stu-id="823e0-133">Geometry</span></span>
  * <span data-ttu-id="823e0-134">Seçeneğinin</span><span class="sxs-lookup"><span data-stu-id="823e0-134">Point</span></span>
  * <span data-ttu-id="823e0-135">LineString</span><span class="sxs-lookup"><span data-stu-id="823e0-135">LineString</span></span>
  * <span data-ttu-id="823e0-136">Gen</span><span class="sxs-lookup"><span data-stu-id="823e0-136">Polygon</span></span>
  * <span data-ttu-id="823e0-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="823e0-137">GeometryCollection</span></span>
    * <span data-ttu-id="823e0-138">Çok nokta</span><span class="sxs-lookup"><span data-stu-id="823e0-138">MultiPoint</span></span>
    * <span data-ttu-id="823e0-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="823e0-139">MultiLineString</span></span>
    * <span data-ttu-id="823e0-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="823e0-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="823e0-141">CurePolygon,, CompoundCurve ve tarafından desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="823e0-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="823e0-142">Temel geometri türünü kullanmak, özelliği tarafından herhangi bir tür şeklin belirtilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="823e0-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="823e0-143">Aşağıdaki varlık sınıfları, [geniş dünya içe örnek veritabanındaki](https://go.microsoft.com/fwlink/?LinkID=800630)tablolarla eşlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="823e0-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="823e0-144">Değer oluşturma</span><span class="sxs-lookup"><span data-stu-id="823e0-144">Creating values</span></span>

<span data-ttu-id="823e0-145">Geometri nesneleri oluşturmak için oluşturucuları kullanabilirsiniz; Ancak, bunun yerine bir geometri fabrikası kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="823e0-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="823e0-146">Bu, varsayılan bir SRID (Koordinatlar tarafından kullanılan uzamsal başvuru sistemi) belirtmenizi sağlar ve duyarlık modeli (hesaplamalar sırasında kullanılır) ve koordinat sırası gibi daha gelişmiş şeyler üzerinde denetim sağlar (bu boyutları belirler ve ölçüler--kullanılabilir).</span><span class="sxs-lookup"><span data-stu-id="823e0-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="823e0-147">4326, GPS ve diğer coğrafi sistemlerde kullanılan standart olan WGS 84 ' e başvurur.</span><span class="sxs-lookup"><span data-stu-id="823e0-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="823e0-148">Boylam ve Enlem</span><span class="sxs-lookup"><span data-stu-id="823e0-148">Longitude and Latitude</span></span>

<span data-ttu-id="823e0-149">Ormallardaki koordinatlar X ve Y değerleri bakımından yapılır.</span><span class="sxs-lookup"><span data-stu-id="823e0-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="823e0-150">Boylam ve enlem 'yi göstermek için boylam için X kullanın ve enlem için Y kullanın.</span><span class="sxs-lookup"><span data-stu-id="823e0-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="823e0-151">Bunun, genellikle bu değerleri görebileceğiniz `latitude, longitude` biçiminden **geriye doğru** olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="823e0-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="823e0-152">İstemci işlemleri sırasında SRID yoksayıldı</span><span class="sxs-lookup"><span data-stu-id="823e0-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="823e0-153">, İşlemler sırasında SRID değerlerini yoksayar.</span><span class="sxs-lookup"><span data-stu-id="823e0-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="823e0-154">Bir düzlem koordinat sistemi olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="823e0-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="823e0-155">Diğer bir deyişle, boylam ve enlem açısından koordinatları belirtirseniz, uzaklık, uzunluk ve alan gibi istemci tarafından değerlendirilen bazı değerler, ölçü cinsinden değil, derece cinsinden olacaktır.</span><span class="sxs-lookup"><span data-stu-id="823e0-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="823e0-156">Daha anlamlı değerler için, önce bu değerleri hesaplamadan önce [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) gibi bir kitaplık kullanarak koordinatları başka bir koordinat sistemine proje yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="823e0-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="823e0-157">Bir işlem SQL üzerinden EF Core tarafından değerlendiriliyorsa, sonucun birimi veritabanı tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="823e0-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="823e0-158">İki şehir arasındaki mesafeyi hesaplamak için ProjNet4GeoAPI kullanılmasına bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="823e0-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="823e0-159">Veri Sorgulama</span><span class="sxs-lookup"><span data-stu-id="823e0-159">Querying Data</span></span>

<span data-ttu-id="823e0-160">LINQ 'ta, veritabanı işlevleri olarak kullanılabilen tüm yöntemler ve özellikler SQL 'e çevrilir.</span><span class="sxs-lookup"><span data-stu-id="823e0-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="823e0-161">Örneğin, uzaklık ve Contains yöntemleri aşağıdaki sorgularda çevrilir.</span><span class="sxs-lookup"><span data-stu-id="823e0-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="823e0-162">Bu makalenin sonundaki tabloda, çeşitli EF Core sağlayıcıları tarafından hangi üyelerin desteklendiği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="823e0-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="823e0-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="823e0-163">SQL Server</span></span>

<span data-ttu-id="823e0-164">SQL Server kullanıyorsanız, bilmeniz gereken bazı ek şeyler vardır.</span><span class="sxs-lookup"><span data-stu-id="823e0-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="823e0-165">Coğrafya veya geometri</span><span class="sxs-lookup"><span data-stu-id="823e0-165">Geography or geometry</span></span>

<span data-ttu-id="823e0-166">Varsayılan olarak, uzamsal özellikler SQL Server `geography` sütunlara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="823e0-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="823e0-167">`geometry`kullanmak için, modelinizde [sütun türünü yapılandırın](xref:core/modeling/entity-properties#column-data-types) .</span><span class="sxs-lookup"><span data-stu-id="823e0-167">To use `geometry`, [configure the column type](xref:core/modeling/entity-properties#column-data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="823e0-168">Coğrafi Çokgen halkaları</span><span class="sxs-lookup"><span data-stu-id="823e0-168">Geography polygon rings</span></span>

<span data-ttu-id="823e0-169">`geography` sütun türünü kullanırken, SQL Server dış halkada (veya kabukta) ve iç halkalarda (veya delikleri) ek gereksinimler uygular.</span><span class="sxs-lookup"><span data-stu-id="823e0-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="823e0-170">Dış halkasının saatin tersi yönde ve iç halkalar saat yönünde yönlendirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="823e0-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="823e0-171">Bu, verileri veritabanına göndermeden önce bunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="823e0-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="823e0-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="823e0-172">FullGlobe</span></span>

<span data-ttu-id="823e0-173">SQL Server, `geography` sütun türü kullanılırken tam dünyayı temsil eden standart olmayan bir geometri türüne sahiptir.</span><span class="sxs-lookup"><span data-stu-id="823e0-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="823e0-174">Ayrıca, tam dünyaya göre (dış halka olmadan) çokgenler temsil etmenin bir yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="823e0-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="823e0-175">Bunlardan hiçbiri bu şekilde desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="823e0-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="823e0-176">Bu temel alan Fulldünya ve çokgenler, bu şekilde desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="823e0-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="823e0-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="823e0-177">SQLite</span></span>

<span data-ttu-id="823e0-178">SQLite kullanarak bunlar için bazı ek bilgiler aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="823e0-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="823e0-179">Gereksiz Ignite yükleniyor</span><span class="sxs-lookup"><span data-stu-id="823e0-179">Installing SpatiaLite</span></span>

<span data-ttu-id="823e0-180">Windows 'da yerel mod_spatialite kitaplığı, bir NuGet paketi bağımlılığı olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="823e0-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="823e0-181">Diğer platformların da ayrı olarak yüklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="823e0-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="823e0-182">Bu, genellikle bir yazılım paketi Yöneticisi kullanılarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="823e0-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="823e0-183">Örneğin, MacOS 'ta Ubuntu ve Homebrew üzerinde APT ' i kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="823e0-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="823e0-184">SRID yapılandırma</span><span class="sxs-lookup"><span data-stu-id="823e0-184">Configuring SRID</span></span>

<span data-ttu-id="823e0-185">Gereksiz bir şekilde sütun başına bir SRID belirtmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="823e0-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="823e0-186">Varsayılan SRID `0`.</span><span class="sxs-lookup"><span data-stu-id="823e0-186">The default SRID is `0`.</span></span> <span data-ttu-id="823e0-187">ForSqliteHasSrid yöntemini kullanarak farklı bir SRID belirtin.</span><span class="sxs-lookup"><span data-stu-id="823e0-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="823e0-188">Boyut</span><span class="sxs-lookup"><span data-stu-id="823e0-188">Dimension</span></span>

<span data-ttu-id="823e0-189">SRID 'e benzer şekilde sütunun boyutu (veya ordinkılanan) de sütunun bir parçası olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="823e0-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="823e0-190">Varsayılan ordintlar X ve Y 'tir. ForSqliteHasDimension metodunu kullanarak ek ordinları (Z ve d) etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="823e0-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="823e0-191">Çevrilmiş Işlemler</span><span class="sxs-lookup"><span data-stu-id="823e0-191">Translated Operations</span></span>

<span data-ttu-id="823e0-192">Bu tabloda, her bir EF Core sağlayıcısı tarafından SQL 'e çevrilen her bir bu üye gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="823e0-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="823e0-193">Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="823e0-193">NetTopologySuite</span></span> | <span data-ttu-id="823e0-194">SQL Server (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-194">SQL Server (geometry)</span></span> | <span data-ttu-id="823e0-195">SQL Server (coğrafya)</span><span class="sxs-lookup"><span data-stu-id="823e0-195">SQL Server (geography)</span></span> | <span data-ttu-id="823e0-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="823e0-196">SQLite</span></span> | <span data-ttu-id="823e0-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="823e0-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="823e0-198">Geometry. alan</span><span class="sxs-lookup"><span data-stu-id="823e0-198">Geometry.Area</span></span> | <span data-ttu-id="823e0-199">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-199">✔</span></span> | <span data-ttu-id="823e0-200">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-200">✔</span></span> | <span data-ttu-id="823e0-201">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-201">✔</span></span> | <span data-ttu-id="823e0-202">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-202">✔</span></span>
<span data-ttu-id="823e0-203">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="823e0-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="823e0-204">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-204">✔</span></span> | <span data-ttu-id="823e0-205">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-205">✔</span></span> | <span data-ttu-id="823e0-206">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-206">✔</span></span> | <span data-ttu-id="823e0-207">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-207">✔</span></span>
<span data-ttu-id="823e0-208">Geometry. AsText ()</span><span class="sxs-lookup"><span data-stu-id="823e0-208">Geometry.AsText()</span></span> | <span data-ttu-id="823e0-209">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-209">✔</span></span> | <span data-ttu-id="823e0-210">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-210">✔</span></span> | <span data-ttu-id="823e0-211">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-211">✔</span></span> | <span data-ttu-id="823e0-212">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-212">✔</span></span>
<span data-ttu-id="823e0-213">Geometry. sınır</span><span class="sxs-lookup"><span data-stu-id="823e0-213">Geometry.Boundary</span></span> | <span data-ttu-id="823e0-214">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-214">✔</span></span> | | <span data-ttu-id="823e0-215">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-215">✔</span></span> | <span data-ttu-id="823e0-216">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-216">✔</span></span>
<span data-ttu-id="823e0-217">Geometry. Buffer (Double)</span><span class="sxs-lookup"><span data-stu-id="823e0-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="823e0-218">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-218">✔</span></span> | <span data-ttu-id="823e0-219">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-219">✔</span></span> | <span data-ttu-id="823e0-220">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-220">✔</span></span> | <span data-ttu-id="823e0-221">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-221">✔</span></span>
<span data-ttu-id="823e0-222">Geometry. Buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="823e0-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="823e0-223">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-223">✔</span></span> | <span data-ttu-id="823e0-224">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-224">✔</span></span>
<span data-ttu-id="823e0-225">Geometry. Centroıd</span><span class="sxs-lookup"><span data-stu-id="823e0-225">Geometry.Centroid</span></span> | <span data-ttu-id="823e0-226">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-226">✔</span></span> | | <span data-ttu-id="823e0-227">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-227">✔</span></span> | <span data-ttu-id="823e0-228">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-228">✔</span></span>
<span data-ttu-id="823e0-229">Geometry. Contains (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="823e0-230">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-230">✔</span></span> | <span data-ttu-id="823e0-231">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-231">✔</span></span> | <span data-ttu-id="823e0-232">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-232">✔</span></span> | <span data-ttu-id="823e0-233">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-233">✔</span></span>
<span data-ttu-id="823e0-234">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="823e0-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="823e0-235">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-235">✔</span></span> | <span data-ttu-id="823e0-236">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-236">✔</span></span> | <span data-ttu-id="823e0-237">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-237">✔</span></span> | <span data-ttu-id="823e0-238">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-238">✔</span></span>
<span data-ttu-id="823e0-239">Geometry. kapak Edby (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="823e0-240">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-240">✔</span></span> | <span data-ttu-id="823e0-241">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-241">✔</span></span>
<span data-ttu-id="823e0-242">Geometry. kapsıyorsa (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="823e0-243">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-243">✔</span></span> | <span data-ttu-id="823e0-244">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-244">✔</span></span>
<span data-ttu-id="823e0-245">Geometry. Kesişmeleri (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="823e0-246">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-246">✔</span></span> | | <span data-ttu-id="823e0-247">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-247">✔</span></span> | <span data-ttu-id="823e0-248">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-248">✔</span></span>
<span data-ttu-id="823e0-249">Geometry. fark (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="823e0-250">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-250">✔</span></span> | <span data-ttu-id="823e0-251">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-251">✔</span></span> | <span data-ttu-id="823e0-252">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-252">✔</span></span> | <span data-ttu-id="823e0-253">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-253">✔</span></span>
<span data-ttu-id="823e0-254">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="823e0-254">Geometry.Dimension</span></span> | <span data-ttu-id="823e0-255">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-255">✔</span></span> | <span data-ttu-id="823e0-256">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-256">✔</span></span> | <span data-ttu-id="823e0-257">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-257">✔</span></span> | <span data-ttu-id="823e0-258">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-258">✔</span></span>
<span data-ttu-id="823e0-259">Geometry. ayrık (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="823e0-260">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-260">✔</span></span> | <span data-ttu-id="823e0-261">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-261">✔</span></span> | <span data-ttu-id="823e0-262">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-262">✔</span></span> | <span data-ttu-id="823e0-263">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-263">✔</span></span>
<span data-ttu-id="823e0-264">Geometry. mesafe (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="823e0-265">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-265">✔</span></span> | <span data-ttu-id="823e0-266">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-266">✔</span></span> | <span data-ttu-id="823e0-267">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-267">✔</span></span> | <span data-ttu-id="823e0-268">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-268">✔</span></span>
<span data-ttu-id="823e0-269">Geometry. Envelope</span><span class="sxs-lookup"><span data-stu-id="823e0-269">Geometry.Envelope</span></span> | <span data-ttu-id="823e0-270">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-270">✔</span></span> | | <span data-ttu-id="823e0-271">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-271">✔</span></span> | <span data-ttu-id="823e0-272">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-272">✔</span></span>
<span data-ttu-id="823e0-273">Geometry. Equalsexyasası (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="823e0-274">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-274">✔</span></span>
<span data-ttu-id="823e0-275">Geometry. EqualsTopologically (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="823e0-276">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-276">✔</span></span> | <span data-ttu-id="823e0-277">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-277">✔</span></span> | <span data-ttu-id="823e0-278">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-278">✔</span></span> | <span data-ttu-id="823e0-279">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-279">✔</span></span>
<span data-ttu-id="823e0-280">Geometry. GeometryType</span><span class="sxs-lookup"><span data-stu-id="823e0-280">Geometry.GeometryType</span></span> | <span data-ttu-id="823e0-281">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-281">✔</span></span> | <span data-ttu-id="823e0-282">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-282">✔</span></span> | <span data-ttu-id="823e0-283">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-283">✔</span></span> | <span data-ttu-id="823e0-284">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-284">✔</span></span>
<span data-ttu-id="823e0-285">Geometry. Getgeometrik YN (int)</span><span class="sxs-lookup"><span data-stu-id="823e0-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="823e0-286">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-286">✔</span></span> | | <span data-ttu-id="823e0-287">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-287">✔</span></span> | <span data-ttu-id="823e0-288">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-288">✔</span></span>
<span data-ttu-id="823e0-289">Geometri. ınteriorpoint</span><span class="sxs-lookup"><span data-stu-id="823e0-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="823e0-290">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-290">✔</span></span> | | <span data-ttu-id="823e0-291">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-291">✔</span></span> | <span data-ttu-id="823e0-292">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-292">✔</span></span>
<span data-ttu-id="823e0-293">Geometry. kesişmesi (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-293">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="823e0-294">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-294">✔</span></span> | <span data-ttu-id="823e0-295">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-295">✔</span></span> | <span data-ttu-id="823e0-296">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-296">✔</span></span> | <span data-ttu-id="823e0-297">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-297">✔</span></span>
<span data-ttu-id="823e0-298">Geometry. kesişme (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-298">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="823e0-299">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-299">✔</span></span> | <span data-ttu-id="823e0-300">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-300">✔</span></span> | <span data-ttu-id="823e0-301">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-301">✔</span></span> | <span data-ttu-id="823e0-302">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-302">✔</span></span>
<span data-ttu-id="823e0-303">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="823e0-303">Geometry.IsEmpty</span></span> | <span data-ttu-id="823e0-304">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-304">✔</span></span> | <span data-ttu-id="823e0-305">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-305">✔</span></span> | <span data-ttu-id="823e0-306">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-306">✔</span></span> | <span data-ttu-id="823e0-307">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-307">✔</span></span>
<span data-ttu-id="823e0-308">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="823e0-308">Geometry.IsSimple</span></span> | <span data-ttu-id="823e0-309">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-309">✔</span></span> | | <span data-ttu-id="823e0-310">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-310">✔</span></span> | <span data-ttu-id="823e0-311">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-311">✔</span></span>
<span data-ttu-id="823e0-312">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="823e0-312">Geometry.IsValid</span></span> | <span data-ttu-id="823e0-313">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-313">✔</span></span> | <span data-ttu-id="823e0-314">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-314">✔</span></span> | <span data-ttu-id="823e0-315">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-315">✔</span></span> | <span data-ttu-id="823e0-316">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-316">✔</span></span>
<span data-ttu-id="823e0-317">Geometry. ıswithındistance (geometri, Double)</span><span class="sxs-lookup"><span data-stu-id="823e0-317">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="823e0-318">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-318">✔</span></span> | | <span data-ttu-id="823e0-319">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-319">✔</span></span> | <span data-ttu-id="823e0-320">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-320">✔</span></span>
<span data-ttu-id="823e0-321">Geometry. length</span><span class="sxs-lookup"><span data-stu-id="823e0-321">Geometry.Length</span></span> | <span data-ttu-id="823e0-322">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-322">✔</span></span> | <span data-ttu-id="823e0-323">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-323">✔</span></span> | <span data-ttu-id="823e0-324">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-324">✔</span></span> | <span data-ttu-id="823e0-325">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-325">✔</span></span>
<span data-ttu-id="823e0-326">Geometry. Numgeometrileri</span><span class="sxs-lookup"><span data-stu-id="823e0-326">Geometry.NumGeometries</span></span> | <span data-ttu-id="823e0-327">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-327">✔</span></span> | <span data-ttu-id="823e0-328">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-328">✔</span></span> | <span data-ttu-id="823e0-329">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-329">✔</span></span> | <span data-ttu-id="823e0-330">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-330">✔</span></span>
<span data-ttu-id="823e0-331">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="823e0-331">Geometry.NumPoints</span></span> | <span data-ttu-id="823e0-332">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-332">✔</span></span> | <span data-ttu-id="823e0-333">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-333">✔</span></span> | <span data-ttu-id="823e0-334">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-334">✔</span></span> | <span data-ttu-id="823e0-335">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-335">✔</span></span>
<span data-ttu-id="823e0-336">Geometry. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="823e0-336">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="823e0-337">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-337">✔</span></span> | <span data-ttu-id="823e0-338">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-338">✔</span></span> | <span data-ttu-id="823e0-339">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-339">✔</span></span> | <span data-ttu-id="823e0-340">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-340">✔</span></span>
<span data-ttu-id="823e0-341">Geometry. örtüşüyor (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-341">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="823e0-342">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-342">✔</span></span> | <span data-ttu-id="823e0-343">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-343">✔</span></span> | <span data-ttu-id="823e0-344">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-344">✔</span></span> | <span data-ttu-id="823e0-345">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-345">✔</span></span>
<span data-ttu-id="823e0-346">Geometry. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="823e0-346">Geometry.PointOnSurface</span></span> | <span data-ttu-id="823e0-347">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-347">✔</span></span> | | <span data-ttu-id="823e0-348">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-348">✔</span></span> | <span data-ttu-id="823e0-349">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-349">✔</span></span>
<span data-ttu-id="823e0-350">Geometry. Ilişkilendir (geometri, dize)</span><span class="sxs-lookup"><span data-stu-id="823e0-350">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="823e0-351">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-351">✔</span></span> | | <span data-ttu-id="823e0-352">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-352">✔</span></span> | <span data-ttu-id="823e0-353">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-353">✔</span></span>
<span data-ttu-id="823e0-354">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="823e0-354">Geometry.Reverse()</span></span> | | | <span data-ttu-id="823e0-355">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-355">✔</span></span> | <span data-ttu-id="823e0-356">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-356">✔</span></span>
<span data-ttu-id="823e0-357">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="823e0-357">Geometry.SRID</span></span> | <span data-ttu-id="823e0-358">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-358">✔</span></span> | <span data-ttu-id="823e0-359">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-359">✔</span></span> | <span data-ttu-id="823e0-360">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-360">✔</span></span> | <span data-ttu-id="823e0-361">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-361">✔</span></span>
<span data-ttu-id="823e0-362">Geometry. SymmetricDifference (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-362">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="823e0-363">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-363">✔</span></span> | <span data-ttu-id="823e0-364">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-364">✔</span></span> | <span data-ttu-id="823e0-365">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-365">✔</span></span> | <span data-ttu-id="823e0-366">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-366">✔</span></span>
<span data-ttu-id="823e0-367">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="823e0-367">Geometry.ToBinary()</span></span> | <span data-ttu-id="823e0-368">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-368">✔</span></span> | <span data-ttu-id="823e0-369">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-369">✔</span></span> | <span data-ttu-id="823e0-370">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-370">✔</span></span> | <span data-ttu-id="823e0-371">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-371">✔</span></span>
<span data-ttu-id="823e0-372">Geometry. ToText ()</span><span class="sxs-lookup"><span data-stu-id="823e0-372">Geometry.ToText()</span></span> | <span data-ttu-id="823e0-373">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-373">✔</span></span> | <span data-ttu-id="823e0-374">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-374">✔</span></span> | <span data-ttu-id="823e0-375">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-375">✔</span></span> | <span data-ttu-id="823e0-376">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-376">✔</span></span>
<span data-ttu-id="823e0-377">Geometry. dokunuşları (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-377">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="823e0-378">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-378">✔</span></span> | | <span data-ttu-id="823e0-379">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-379">✔</span></span> | <span data-ttu-id="823e0-380">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-380">✔</span></span>
<span data-ttu-id="823e0-381">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="823e0-381">Geometry.Union()</span></span> | | | <span data-ttu-id="823e0-382">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-382">✔</span></span> | <span data-ttu-id="823e0-383">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-383">✔</span></span>
<span data-ttu-id="823e0-384">Geometry. Union (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-384">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="823e0-385">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-385">✔</span></span> | <span data-ttu-id="823e0-386">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-386">✔</span></span> | <span data-ttu-id="823e0-387">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-387">✔</span></span> | <span data-ttu-id="823e0-388">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-388">✔</span></span>
<span data-ttu-id="823e0-389">Geometri. Içinde (geometri)</span><span class="sxs-lookup"><span data-stu-id="823e0-389">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="823e0-390">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-390">✔</span></span> | <span data-ttu-id="823e0-391">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-391">✔</span></span> | <span data-ttu-id="823e0-392">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-392">✔</span></span> | <span data-ttu-id="823e0-393">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-393">✔</span></span>
<span data-ttu-id="823e0-394">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="823e0-394">GeometryCollection.Count</span></span> | <span data-ttu-id="823e0-395">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-395">✔</span></span> | <span data-ttu-id="823e0-396">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-396">✔</span></span> | <span data-ttu-id="823e0-397">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-397">✔</span></span> | <span data-ttu-id="823e0-398">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-398">✔</span></span>
<span data-ttu-id="823e0-399">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="823e0-399">GeometryCollection[int]</span></span> | <span data-ttu-id="823e0-400">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-400">✔</span></span> | <span data-ttu-id="823e0-401">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-401">✔</span></span> | <span data-ttu-id="823e0-402">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-402">✔</span></span> | <span data-ttu-id="823e0-403">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-403">✔</span></span>
<span data-ttu-id="823e0-404">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="823e0-404">LineString.Count</span></span> | <span data-ttu-id="823e0-405">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-405">✔</span></span> | <span data-ttu-id="823e0-406">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-406">✔</span></span> | <span data-ttu-id="823e0-407">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-407">✔</span></span> | <span data-ttu-id="823e0-408">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-408">✔</span></span>
<span data-ttu-id="823e0-409">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="823e0-409">LineString.EndPoint</span></span> | <span data-ttu-id="823e0-410">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-410">✔</span></span> | <span data-ttu-id="823e0-411">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-411">✔</span></span> | <span data-ttu-id="823e0-412">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-412">✔</span></span> | <span data-ttu-id="823e0-413">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-413">✔</span></span>
<span data-ttu-id="823e0-414">LineString. GetPointN (int)</span><span class="sxs-lookup"><span data-stu-id="823e0-414">LineString.GetPointN(int)</span></span> | <span data-ttu-id="823e0-415">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-415">✔</span></span> | <span data-ttu-id="823e0-416">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-416">✔</span></span> | <span data-ttu-id="823e0-417">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-417">✔</span></span> | <span data-ttu-id="823e0-418">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-418">✔</span></span>
<span data-ttu-id="823e0-419">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="823e0-419">LineString.IsClosed</span></span> | <span data-ttu-id="823e0-420">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-420">✔</span></span> | <span data-ttu-id="823e0-421">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-421">✔</span></span> | <span data-ttu-id="823e0-422">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-422">✔</span></span> | <span data-ttu-id="823e0-423">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-423">✔</span></span>
<span data-ttu-id="823e0-424">LineString. IsRing</span><span class="sxs-lookup"><span data-stu-id="823e0-424">LineString.IsRing</span></span> | <span data-ttu-id="823e0-425">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-425">✔</span></span> | | <span data-ttu-id="823e0-426">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-426">✔</span></span> | <span data-ttu-id="823e0-427">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-427">✔</span></span>
<span data-ttu-id="823e0-428">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="823e0-428">LineString.StartPoint</span></span> | <span data-ttu-id="823e0-429">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-429">✔</span></span> | <span data-ttu-id="823e0-430">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-430">✔</span></span> | <span data-ttu-id="823e0-431">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-431">✔</span></span> | <span data-ttu-id="823e0-432">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-432">✔</span></span>
<span data-ttu-id="823e0-433">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="823e0-433">MultiLineString.IsClosed</span></span> | <span data-ttu-id="823e0-434">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-434">✔</span></span> | <span data-ttu-id="823e0-435">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-435">✔</span></span> | <span data-ttu-id="823e0-436">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-436">✔</span></span> | <span data-ttu-id="823e0-437">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-437">✔</span></span>
<span data-ttu-id="823e0-438">Nokta. d</span><span class="sxs-lookup"><span data-stu-id="823e0-438">Point.M</span></span> | <span data-ttu-id="823e0-439">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-439">✔</span></span> | <span data-ttu-id="823e0-440">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-440">✔</span></span> | <span data-ttu-id="823e0-441">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-441">✔</span></span> | <span data-ttu-id="823e0-442">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-442">✔</span></span>
<span data-ttu-id="823e0-443">Point. X</span><span class="sxs-lookup"><span data-stu-id="823e0-443">Point.X</span></span> | <span data-ttu-id="823e0-444">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-444">✔</span></span> | <span data-ttu-id="823e0-445">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-445">✔</span></span> | <span data-ttu-id="823e0-446">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-446">✔</span></span> | <span data-ttu-id="823e0-447">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-447">✔</span></span>
<span data-ttu-id="823e0-448">Nokta. Y</span><span class="sxs-lookup"><span data-stu-id="823e0-448">Point.Y</span></span> | <span data-ttu-id="823e0-449">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-449">✔</span></span> | <span data-ttu-id="823e0-450">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-450">✔</span></span> | <span data-ttu-id="823e0-451">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-451">✔</span></span> | <span data-ttu-id="823e0-452">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-452">✔</span></span>
<span data-ttu-id="823e0-453">Point. Z</span><span class="sxs-lookup"><span data-stu-id="823e0-453">Point.Z</span></span> | <span data-ttu-id="823e0-454">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-454">✔</span></span> | <span data-ttu-id="823e0-455">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-455">✔</span></span> | <span data-ttu-id="823e0-456">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-456">✔</span></span> | <span data-ttu-id="823e0-457">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-457">✔</span></span>
<span data-ttu-id="823e0-458">Çokgen. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="823e0-458">Polygon.ExteriorRing</span></span> | <span data-ttu-id="823e0-459">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-459">✔</span></span> | <span data-ttu-id="823e0-460">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-460">✔</span></span> | <span data-ttu-id="823e0-461">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-461">✔</span></span> | <span data-ttu-id="823e0-462">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-462">✔</span></span>
<span data-ttu-id="823e0-463">Çokgen. Getınteriorringn (int)</span><span class="sxs-lookup"><span data-stu-id="823e0-463">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="823e0-464">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-464">✔</span></span> | <span data-ttu-id="823e0-465">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-465">✔</span></span> | <span data-ttu-id="823e0-466">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-466">✔</span></span> | <span data-ttu-id="823e0-467">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-467">✔</span></span>
<span data-ttu-id="823e0-468">Çokgen. Numinteriorhalkalar</span><span class="sxs-lookup"><span data-stu-id="823e0-468">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="823e0-469">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-469">✔</span></span> | <span data-ttu-id="823e0-470">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-470">✔</span></span> | <span data-ttu-id="823e0-471">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-471">✔</span></span> | <span data-ttu-id="823e0-472">✔</span><span class="sxs-lookup"><span data-stu-id="823e0-472">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="823e0-473">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="823e0-473">Additional resources</span></span>

* [<span data-ttu-id="823e0-474">SQL Server uzamsal veriler</span><span class="sxs-lookup"><span data-stu-id="823e0-474">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="823e0-475">Spaıalite ana sayfası</span><span class="sxs-lookup"><span data-stu-id="823e0-475">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="823e0-476">Npgsql uzamsal belgeleri</span><span class="sxs-lookup"><span data-stu-id="823e0-476">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="823e0-477">PostGIS belgeleri</span><span class="sxs-lookup"><span data-stu-id="823e0-477">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
