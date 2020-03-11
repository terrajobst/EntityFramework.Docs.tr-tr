---
title: Uzamsal veriler-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 5b45f83ca7f02665f52ccfe16b5af506a6046a62
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417407"
---
# <a name="spatial-data"></a><span data-ttu-id="4fff6-102">Uzamsal Veriler</span><span class="sxs-lookup"><span data-stu-id="4fff6-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="4fff6-103">Bu özellik EF Core 2,2 ' ye eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="4fff6-104">Uzamsal veriler, fiziksel konumu ve nesnelerin şeklini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4fff6-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="4fff6-105">Birçok veritabanı, bu tür veriler için destek sağlar, böylece diğer verilerle birlikte dizinlenebilir ve sorgulanabilir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="4fff6-106">Yaygın senaryolar, bir konumdan belirli bir uzaklıkta bulunan nesnelerin sorgulanmasını veya kenarlığını belirli bir konumu içeren nesneyi seçmeyi içerir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="4fff6-107">EF Core, [Nettopologyısuite](https://github.com/NetTopologySuite/NetTopologySuite) uzamsal kitaplığı kullanılarak uzamsal veri türlerine eşlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="4fff6-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="4fff6-108">Yükleniyor</span><span class="sxs-lookup"><span data-stu-id="4fff6-108">Installing</span></span>

<span data-ttu-id="4fff6-109">Uzamsal verileri EF Core kullanmak için, uygun destekleyici NuGet paketini yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="4fff6-110">Yüklemeniz gereken paket, kullanmakta olduğunuz sağlayıcıya bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="4fff6-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="4fff6-111">EF Core sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="4fff6-111">EF Core Provider</span></span>                        | <span data-ttu-id="4fff6-112">Uzamsal NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="4fff6-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="4fff6-113">Microsoft. EntityFrameworkCore. SqlServer</span><span class="sxs-lookup"><span data-stu-id="4fff6-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="4fff6-114">Microsoft. EntityFrameworkCore. SqlServer. Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="4fff6-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="4fff6-115">Microsoft. EntityFrameworkCore. SQLite</span><span class="sxs-lookup"><span data-stu-id="4fff6-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="4fff6-116">Microsoft. EntityFrameworkCore. SQLite. Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="4fff6-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="4fff6-117">Microsoft. EntityFrameworkCore. InMemory</span><span class="sxs-lookup"><span data-stu-id="4fff6-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="4fff6-118">Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="4fff6-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="4fff6-119">Npgsql. EntityFrameworkCore. PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="4fff6-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="4fff6-120">Npgsql. EntityFrameworkCore. PostgreSQL. Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="4fff6-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="4fff6-121">Tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="4fff6-121">Reverse engineering</span></span>

<span data-ttu-id="4fff6-122">Uzamsal NuGet paketleri de uzamsal özelliklerle [ters mühendislik](../managing-schemas/scaffolding.md) modellerini etkinleştirir, ancak `Scaffold-DbContext` veya `dotnet ef dbcontext scaffold`çalıştırmadan ***önce*** paketi yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="4fff6-123">Bunu yapmazsanız, sütunlar için tür eşlemelerini bulmayın hakkında uyarılar alırsınız ve sütunlar atlanır.</span><span class="sxs-lookup"><span data-stu-id="4fff6-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="4fff6-124">Nettopologyısuite (bir)</span><span class="sxs-lookup"><span data-stu-id="4fff6-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="4fff6-125">Nettopologyısuite, .NET için uzamsal bir kitaplıktır.</span><span class="sxs-lookup"><span data-stu-id="4fff6-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="4fff6-126">EF Core, modelinizdeki türler türlerini kullanarak veritabanındaki uzamsal veri türlerine eşlemeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="4fff6-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="4fff6-127">Uzamsal türlere, HI aracılığıyla eşlemeyi etkinleştirmek için, sağlayıcının DbContext seçenekleri oluşturucusunda Usenettopologyısuite yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="4fff6-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="4fff6-128">Örneğin, SQL Server ile bu şekilde çağrmış olursunuz.</span><span class="sxs-lookup"><span data-stu-id="4fff6-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="4fff6-129">Birçok uzamsal veri türü vardır.</span><span class="sxs-lookup"><span data-stu-id="4fff6-129">There are several spatial data types.</span></span> <span data-ttu-id="4fff6-130">Kullandığınız tür, izin vermek istediğiniz şekillerin türüne bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="4fff6-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="4fff6-131">Modelinizdeki özellikler için kullanabileceğiniz, bu türlerin hiyerarşisi aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="4fff6-132">`NetTopologySuite.Geometries` ad alanı içinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="4fff6-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="4fff6-133">Geometrisi</span><span class="sxs-lookup"><span data-stu-id="4fff6-133">Geometry</span></span>
  * <span data-ttu-id="4fff6-134">Seçeneğinin</span><span class="sxs-lookup"><span data-stu-id="4fff6-134">Point</span></span>
  * <span data-ttu-id="4fff6-135">LineString</span><span class="sxs-lookup"><span data-stu-id="4fff6-135">LineString</span></span>
  * <span data-ttu-id="4fff6-136">Gen</span><span class="sxs-lookup"><span data-stu-id="4fff6-136">Polygon</span></span>
  * <span data-ttu-id="4fff6-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="4fff6-137">GeometryCollection</span></span>
    * <span data-ttu-id="4fff6-138">Çok nokta</span><span class="sxs-lookup"><span data-stu-id="4fff6-138">MultiPoint</span></span>
    * <span data-ttu-id="4fff6-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="4fff6-139">MultiLineString</span></span>
    * <span data-ttu-id="4fff6-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="4fff6-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="4fff6-141">CurePolygon,, CompoundCurve ve tarafından desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="4fff6-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="4fff6-142">Temel geometri türünü kullanmak, özelliği tarafından herhangi bir tür şeklin belirtilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="4fff6-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="4fff6-143">Aşağıdaki varlık sınıfları, [geniş dünya içe örnek veritabanındaki](https://go.microsoft.com/fwlink/?LinkID=800630)tablolarla eşlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="4fff6-144">Değer oluşturma</span><span class="sxs-lookup"><span data-stu-id="4fff6-144">Creating values</span></span>

<span data-ttu-id="4fff6-145">Geometri nesneleri oluşturmak için oluşturucuları kullanabilirsiniz; Ancak, bunun yerine bir geometri fabrikası kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="4fff6-146">Bu, varsayılan bir SRID (Koordinatlar tarafından kullanılan uzamsal başvuru sistemi) belirtmenizi sağlar ve duyarlık modeli (hesaplamalar sırasında kullanılır) ve koordinat sırası gibi daha gelişmiş şeyler üzerinde denetim sağlar (bu boyutları belirler ve ölçüler--kullanılabilir).</span><span class="sxs-lookup"><span data-stu-id="4fff6-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="4fff6-147">4326, GPS ve diğer coğrafi sistemlerde kullanılan standart olan WGS 84 ' e başvurur.</span><span class="sxs-lookup"><span data-stu-id="4fff6-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="4fff6-148">Boylam ve Enlem</span><span class="sxs-lookup"><span data-stu-id="4fff6-148">Longitude and Latitude</span></span>

<span data-ttu-id="4fff6-149">Ormallardaki koordinatlar X ve Y değerleri bakımından yapılır.</span><span class="sxs-lookup"><span data-stu-id="4fff6-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="4fff6-150">Boylam ve enlem 'yi göstermek için boylam için X kullanın ve enlem için Y kullanın.</span><span class="sxs-lookup"><span data-stu-id="4fff6-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="4fff6-151">Bunun, genellikle bu değerleri görebileceğiniz `latitude, longitude` biçiminden **geriye doğru** olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4fff6-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="4fff6-152">İstemci işlemleri sırasında SRID yoksayıldı</span><span class="sxs-lookup"><span data-stu-id="4fff6-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="4fff6-153">, İşlemler sırasında SRID değerlerini yoksayar.</span><span class="sxs-lookup"><span data-stu-id="4fff6-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="4fff6-154">Bir düzlem koordinat sistemi olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="4fff6-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="4fff6-155">Diğer bir deyişle, boylam ve enlem açısından koordinatları belirtirseniz, uzaklık, uzunluk ve alan gibi istemci tarafından değerlendirilen bazı değerler, ölçü cinsinden değil, derece cinsinden olacaktır.</span><span class="sxs-lookup"><span data-stu-id="4fff6-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="4fff6-156">Daha anlamlı değerler için, önce bu değerleri hesaplamadan önce [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) gibi bir kitaplık kullanarak koordinatları başka bir koordinat sistemine proje yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="4fff6-157">Bir işlem SQL üzerinden EF Core tarafından değerlendiriliyorsa, sonucun birimi veritabanı tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="4fff6-158">İki şehir arasındaki mesafeyi hesaplamak için ProjNet4GeoAPI kullanılmasına bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="4fff6-159">Veri Sorgulama</span><span class="sxs-lookup"><span data-stu-id="4fff6-159">Querying Data</span></span>

<span data-ttu-id="4fff6-160">LINQ 'ta, veritabanı işlevleri olarak kullanılabilen tüm yöntemler ve özellikler SQL 'e çevrilir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="4fff6-161">Örneğin, uzaklık ve Contains yöntemleri aşağıdaki sorgularda çevrilir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="4fff6-162">Bu makalenin sonundaki tabloda, çeşitli EF Core sağlayıcıları tarafından hangi üyelerin desteklendiği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="4fff6-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="4fff6-163">SQL Server</span></span>

<span data-ttu-id="4fff6-164">SQL Server kullanıyorsanız, bilmeniz gereken bazı ek şeyler vardır.</span><span class="sxs-lookup"><span data-stu-id="4fff6-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="4fff6-165">Coğrafya veya geometri</span><span class="sxs-lookup"><span data-stu-id="4fff6-165">Geography or geometry</span></span>

<span data-ttu-id="4fff6-166">Varsayılan olarak, uzamsal özellikler SQL Server `geography` sütunlara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="4fff6-167">`geometry`kullanmak için, modelinizde [sütun türünü yapılandırın](xref:core/modeling/entity-properties#column-data-types) .</span><span class="sxs-lookup"><span data-stu-id="4fff6-167">To use `geometry`, [configure the column type](xref:core/modeling/entity-properties#column-data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="4fff6-168">Coğrafi Çokgen halkaları</span><span class="sxs-lookup"><span data-stu-id="4fff6-168">Geography polygon rings</span></span>

<span data-ttu-id="4fff6-169">`geography` sütun türünü kullanırken, SQL Server dış halkada (veya kabukta) ve iç halkalarda (veya delikleri) ek gereksinimler uygular.</span><span class="sxs-lookup"><span data-stu-id="4fff6-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="4fff6-170">Dış halkasının saatin tersi yönde ve iç halkalar saat yönünde yönlendirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="4fff6-171">Bu, verileri veritabanına göndermeden önce bunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="4fff6-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="4fff6-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="4fff6-172">FullGlobe</span></span>

<span data-ttu-id="4fff6-173">SQL Server, `geography` sütun türü kullanılırken tam dünyayı temsil eden standart olmayan bir geometri türüne sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="4fff6-174">Ayrıca, tam dünyaya göre (dış halka olmadan) çokgenler temsil etmenin bir yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="4fff6-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="4fff6-175">Bunlardan hiçbiri bu şekilde desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="4fff6-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="4fff6-176">Bu temel alan Fulldünya ve çokgenler, bu şekilde desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="4fff6-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="4fff6-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="4fff6-177">SQLite</span></span>

<span data-ttu-id="4fff6-178">SQLite kullanarak bunlar için bazı ek bilgiler aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="4fff6-179">Gereksiz Ignite yükleniyor</span><span class="sxs-lookup"><span data-stu-id="4fff6-179">Installing SpatiaLite</span></span>

<span data-ttu-id="4fff6-180">Windows 'da yerel mod_spatialite kitaplığı, bir NuGet paketi bağımlılığı olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="4fff6-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="4fff6-181">Diğer platformların da ayrı olarak yüklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="4fff6-182">Bu, genellikle bir yazılım paketi Yöneticisi kullanılarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="4fff6-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="4fff6-183">Örneğin, MacOS 'ta Ubuntu ve Homebrew üzerinde APT ' i kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4fff6-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

<span data-ttu-id="4fff6-184">Ne yazık ki, PROJ 'in daha yeni sürümleri (SpatiaLite bağımlılığı) EF 'in varsayılan [SQLitePCLRaw paketiyle](/dotnet/standard/data/sqlite/custom-versions#bundles)uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="4fff6-184">Unfortunately, newer versions of PROJ (a dependency of SpatiaLite) are incompatible with EF's default [SQLitePCLRaw bundle](/dotnet/standard/data/sqlite/custom-versions#bundles).</span></span> <span data-ttu-id="4fff6-185">Bu sorunu çözmek için sistem SQLite kitaplığı kullanan özel bir [SQLitePCLRaw sağlayıcısı](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) oluşturabilir ya da proj desteğini devre dışı bırakan özel bir SpatiaLite derlemesi yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4fff6-185">You can work around this by either creating a custom [SQLitePCLRaw provider](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) that uses the system SQLite library, or you can install a custom build of SpatiaLite disabling PROJ support.</span></span>

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

### <a name="configuring-srid"></a><span data-ttu-id="4fff6-186">SRID yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4fff6-186">Configuring SRID</span></span>

<span data-ttu-id="4fff6-187">Gereksiz bir şekilde sütun başına bir SRID belirtmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-187">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="4fff6-188">Varsayılan SRID `0`.</span><span class="sxs-lookup"><span data-stu-id="4fff6-188">The default SRID is `0`.</span></span> <span data-ttu-id="4fff6-189">ForSqliteHasSrid yöntemini kullanarak farklı bir SRID belirtin.</span><span class="sxs-lookup"><span data-stu-id="4fff6-189">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="4fff6-190">Boyut</span><span class="sxs-lookup"><span data-stu-id="4fff6-190">Dimension</span></span>

<span data-ttu-id="4fff6-191">SRID 'e benzer şekilde sütunun boyutu (veya ordinkılanan) de sütunun bir parçası olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-191">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="4fff6-192">Varsayılan ordintlar X ve Y 'tir. ForSqliteHasDimension metodunu kullanarak ek ordinları (Z ve d) etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="4fff6-192">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="4fff6-193">Çevrilmiş Işlemler</span><span class="sxs-lookup"><span data-stu-id="4fff6-193">Translated Operations</span></span>

<span data-ttu-id="4fff6-194">Bu tabloda, her bir EF Core sağlayıcısı tarafından SQL 'e çevrilen her bir bu üye gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="4fff6-194">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="4fff6-195">Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="4fff6-195">NetTopologySuite</span></span> | <span data-ttu-id="4fff6-196">SQL Server (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-196">SQL Server (geometry)</span></span> | <span data-ttu-id="4fff6-197">SQL Server (coğrafya)</span><span class="sxs-lookup"><span data-stu-id="4fff6-197">SQL Server (geography)</span></span> | <span data-ttu-id="4fff6-198">SQLite</span><span class="sxs-lookup"><span data-stu-id="4fff6-198">SQLite</span></span> | <span data-ttu-id="4fff6-199">Npgsql</span><span class="sxs-lookup"><span data-stu-id="4fff6-199">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="4fff6-200">Geometry. alan</span><span class="sxs-lookup"><span data-stu-id="4fff6-200">Geometry.Area</span></span> | <span data-ttu-id="4fff6-201">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-201">✔</span></span> | <span data-ttu-id="4fff6-202">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-202">✔</span></span> | <span data-ttu-id="4fff6-203">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-203">✔</span></span> | <span data-ttu-id="4fff6-204">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-204">✔</span></span>
<span data-ttu-id="4fff6-205">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="4fff6-205">Geometry.AsBinary()</span></span> | <span data-ttu-id="4fff6-206">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-206">✔</span></span> | <span data-ttu-id="4fff6-207">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-207">✔</span></span> | <span data-ttu-id="4fff6-208">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-208">✔</span></span> | <span data-ttu-id="4fff6-209">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-209">✔</span></span>
<span data-ttu-id="4fff6-210">Geometry. AsText ()</span><span class="sxs-lookup"><span data-stu-id="4fff6-210">Geometry.AsText()</span></span> | <span data-ttu-id="4fff6-211">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-211">✔</span></span> | <span data-ttu-id="4fff6-212">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-212">✔</span></span> | <span data-ttu-id="4fff6-213">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-213">✔</span></span> | <span data-ttu-id="4fff6-214">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-214">✔</span></span>
<span data-ttu-id="4fff6-215">Geometry. sınır</span><span class="sxs-lookup"><span data-stu-id="4fff6-215">Geometry.Boundary</span></span> | <span data-ttu-id="4fff6-216">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-216">✔</span></span> | | <span data-ttu-id="4fff6-217">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-217">✔</span></span> | <span data-ttu-id="4fff6-218">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-218">✔</span></span>
<span data-ttu-id="4fff6-219">Geometry. Buffer (Double)</span><span class="sxs-lookup"><span data-stu-id="4fff6-219">Geometry.Buffer(double)</span></span> | <span data-ttu-id="4fff6-220">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-220">✔</span></span> | <span data-ttu-id="4fff6-221">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-221">✔</span></span> | <span data-ttu-id="4fff6-222">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-222">✔</span></span> | <span data-ttu-id="4fff6-223">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-223">✔</span></span>
<span data-ttu-id="4fff6-224">Geometry. Buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="4fff6-224">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="4fff6-225">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-225">✔</span></span> | <span data-ttu-id="4fff6-226">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-226">✔</span></span>
<span data-ttu-id="4fff6-227">Geometry. Centroıd</span><span class="sxs-lookup"><span data-stu-id="4fff6-227">Geometry.Centroid</span></span> | <span data-ttu-id="4fff6-228">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-228">✔</span></span> | | <span data-ttu-id="4fff6-229">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-229">✔</span></span> | <span data-ttu-id="4fff6-230">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-230">✔</span></span>
<span data-ttu-id="4fff6-231">Geometry. Contains (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-231">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="4fff6-232">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-232">✔</span></span> | <span data-ttu-id="4fff6-233">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-233">✔</span></span> | <span data-ttu-id="4fff6-234">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-234">✔</span></span> | <span data-ttu-id="4fff6-235">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-235">✔</span></span>
<span data-ttu-id="4fff6-236">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="4fff6-236">Geometry.ConvexHull()</span></span> | <span data-ttu-id="4fff6-237">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-237">✔</span></span> | <span data-ttu-id="4fff6-238">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-238">✔</span></span> | <span data-ttu-id="4fff6-239">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-239">✔</span></span> | <span data-ttu-id="4fff6-240">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-240">✔</span></span>
<span data-ttu-id="4fff6-241">Geometry. kapak Edby (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-241">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="4fff6-242">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-242">✔</span></span> | <span data-ttu-id="4fff6-243">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-243">✔</span></span>
<span data-ttu-id="4fff6-244">Geometry. kapsıyorsa (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-244">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="4fff6-245">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-245">✔</span></span> | <span data-ttu-id="4fff6-246">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-246">✔</span></span>
<span data-ttu-id="4fff6-247">Geometry. Kesişmeleri (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-247">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="4fff6-248">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-248">✔</span></span> | | <span data-ttu-id="4fff6-249">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-249">✔</span></span> | <span data-ttu-id="4fff6-250">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-250">✔</span></span>
<span data-ttu-id="4fff6-251">Geometry. fark (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-251">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="4fff6-252">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-252">✔</span></span> | <span data-ttu-id="4fff6-253">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-253">✔</span></span> | <span data-ttu-id="4fff6-254">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-254">✔</span></span> | <span data-ttu-id="4fff6-255">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-255">✔</span></span>
<span data-ttu-id="4fff6-256">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="4fff6-256">Geometry.Dimension</span></span> | <span data-ttu-id="4fff6-257">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-257">✔</span></span> | <span data-ttu-id="4fff6-258">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-258">✔</span></span> | <span data-ttu-id="4fff6-259">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-259">✔</span></span> | <span data-ttu-id="4fff6-260">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-260">✔</span></span>
<span data-ttu-id="4fff6-261">Geometry. ayrık (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-261">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="4fff6-262">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-262">✔</span></span> | <span data-ttu-id="4fff6-263">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-263">✔</span></span> | <span data-ttu-id="4fff6-264">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-264">✔</span></span> | <span data-ttu-id="4fff6-265">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-265">✔</span></span>
<span data-ttu-id="4fff6-266">Geometry. mesafe (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-266">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="4fff6-267">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-267">✔</span></span> | <span data-ttu-id="4fff6-268">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-268">✔</span></span> | <span data-ttu-id="4fff6-269">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-269">✔</span></span> | <span data-ttu-id="4fff6-270">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-270">✔</span></span>
<span data-ttu-id="4fff6-271">Geometry. Envelope</span><span class="sxs-lookup"><span data-stu-id="4fff6-271">Geometry.Envelope</span></span> | <span data-ttu-id="4fff6-272">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-272">✔</span></span> | | <span data-ttu-id="4fff6-273">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-273">✔</span></span> | <span data-ttu-id="4fff6-274">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-274">✔</span></span>
<span data-ttu-id="4fff6-275">Geometry. Equalsexyasası (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-275">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="4fff6-276">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-276">✔</span></span>
<span data-ttu-id="4fff6-277">Geometry. EqualsTopologically (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-277">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="4fff6-278">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-278">✔</span></span> | <span data-ttu-id="4fff6-279">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-279">✔</span></span> | <span data-ttu-id="4fff6-280">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-280">✔</span></span> | <span data-ttu-id="4fff6-281">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-281">✔</span></span>
<span data-ttu-id="4fff6-282">Geometry. GeometryType</span><span class="sxs-lookup"><span data-stu-id="4fff6-282">Geometry.GeometryType</span></span> | <span data-ttu-id="4fff6-283">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-283">✔</span></span> | <span data-ttu-id="4fff6-284">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-284">✔</span></span> | <span data-ttu-id="4fff6-285">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-285">✔</span></span> | <span data-ttu-id="4fff6-286">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-286">✔</span></span>
<span data-ttu-id="4fff6-287">Geometry. Getgeometrik YN (int)</span><span class="sxs-lookup"><span data-stu-id="4fff6-287">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="4fff6-288">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-288">✔</span></span> | | <span data-ttu-id="4fff6-289">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-289">✔</span></span> | <span data-ttu-id="4fff6-290">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-290">✔</span></span>
<span data-ttu-id="4fff6-291">Geometri. ınteriorpoint</span><span class="sxs-lookup"><span data-stu-id="4fff6-291">Geometry.InteriorPoint</span></span> | <span data-ttu-id="4fff6-292">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-292">✔</span></span> | | <span data-ttu-id="4fff6-293">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-293">✔</span></span> | <span data-ttu-id="4fff6-294">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-294">✔</span></span>
<span data-ttu-id="4fff6-295">Geometry. kesişmesi (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-295">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="4fff6-296">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-296">✔</span></span> | <span data-ttu-id="4fff6-297">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-297">✔</span></span> | <span data-ttu-id="4fff6-298">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-298">✔</span></span> | <span data-ttu-id="4fff6-299">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-299">✔</span></span>
<span data-ttu-id="4fff6-300">Geometry. kesişme (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-300">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="4fff6-301">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-301">✔</span></span> | <span data-ttu-id="4fff6-302">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-302">✔</span></span> | <span data-ttu-id="4fff6-303">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-303">✔</span></span> | <span data-ttu-id="4fff6-304">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-304">✔</span></span>
<span data-ttu-id="4fff6-305">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="4fff6-305">Geometry.IsEmpty</span></span> | <span data-ttu-id="4fff6-306">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-306">✔</span></span> | <span data-ttu-id="4fff6-307">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-307">✔</span></span> | <span data-ttu-id="4fff6-308">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-308">✔</span></span> | <span data-ttu-id="4fff6-309">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-309">✔</span></span>
<span data-ttu-id="4fff6-310">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="4fff6-310">Geometry.IsSimple</span></span> | <span data-ttu-id="4fff6-311">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-311">✔</span></span> | | <span data-ttu-id="4fff6-312">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-312">✔</span></span> | <span data-ttu-id="4fff6-313">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-313">✔</span></span>
<span data-ttu-id="4fff6-314">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="4fff6-314">Geometry.IsValid</span></span> | <span data-ttu-id="4fff6-315">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-315">✔</span></span> | <span data-ttu-id="4fff6-316">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-316">✔</span></span> | <span data-ttu-id="4fff6-317">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-317">✔</span></span> | <span data-ttu-id="4fff6-318">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-318">✔</span></span>
<span data-ttu-id="4fff6-319">Geometry. ıswithındistance (geometri, Double)</span><span class="sxs-lookup"><span data-stu-id="4fff6-319">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="4fff6-320">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-320">✔</span></span> | | <span data-ttu-id="4fff6-321">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-321">✔</span></span> | <span data-ttu-id="4fff6-322">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-322">✔</span></span>
<span data-ttu-id="4fff6-323">Geometry. length</span><span class="sxs-lookup"><span data-stu-id="4fff6-323">Geometry.Length</span></span> | <span data-ttu-id="4fff6-324">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-324">✔</span></span> | <span data-ttu-id="4fff6-325">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-325">✔</span></span> | <span data-ttu-id="4fff6-326">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-326">✔</span></span> | <span data-ttu-id="4fff6-327">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-327">✔</span></span>
<span data-ttu-id="4fff6-328">Geometry. Numgeometrileri</span><span class="sxs-lookup"><span data-stu-id="4fff6-328">Geometry.NumGeometries</span></span> | <span data-ttu-id="4fff6-329">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-329">✔</span></span> | <span data-ttu-id="4fff6-330">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-330">✔</span></span> | <span data-ttu-id="4fff6-331">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-331">✔</span></span> | <span data-ttu-id="4fff6-332">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-332">✔</span></span>
<span data-ttu-id="4fff6-333">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="4fff6-333">Geometry.NumPoints</span></span> | <span data-ttu-id="4fff6-334">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-334">✔</span></span> | <span data-ttu-id="4fff6-335">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-335">✔</span></span> | <span data-ttu-id="4fff6-336">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-336">✔</span></span> | <span data-ttu-id="4fff6-337">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-337">✔</span></span>
<span data-ttu-id="4fff6-338">Geometry. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="4fff6-338">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="4fff6-339">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-339">✔</span></span> | <span data-ttu-id="4fff6-340">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-340">✔</span></span> | <span data-ttu-id="4fff6-341">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-341">✔</span></span> | <span data-ttu-id="4fff6-342">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-342">✔</span></span>
<span data-ttu-id="4fff6-343">Geometry. örtüşüyor (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-343">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="4fff6-344">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-344">✔</span></span> | <span data-ttu-id="4fff6-345">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-345">✔</span></span> | <span data-ttu-id="4fff6-346">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-346">✔</span></span> | <span data-ttu-id="4fff6-347">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-347">✔</span></span>
<span data-ttu-id="4fff6-348">Geometry. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="4fff6-348">Geometry.PointOnSurface</span></span> | <span data-ttu-id="4fff6-349">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-349">✔</span></span> | | <span data-ttu-id="4fff6-350">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-350">✔</span></span> | <span data-ttu-id="4fff6-351">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-351">✔</span></span>
<span data-ttu-id="4fff6-352">Geometry. Ilişkilendir (geometri, dize)</span><span class="sxs-lookup"><span data-stu-id="4fff6-352">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="4fff6-353">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-353">✔</span></span> | | <span data-ttu-id="4fff6-354">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-354">✔</span></span> | <span data-ttu-id="4fff6-355">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-355">✔</span></span>
<span data-ttu-id="4fff6-356">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="4fff6-356">Geometry.Reverse()</span></span> | | | <span data-ttu-id="4fff6-357">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-357">✔</span></span> | <span data-ttu-id="4fff6-358">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-358">✔</span></span>
<span data-ttu-id="4fff6-359">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="4fff6-359">Geometry.SRID</span></span> | <span data-ttu-id="4fff6-360">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-360">✔</span></span> | <span data-ttu-id="4fff6-361">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-361">✔</span></span> | <span data-ttu-id="4fff6-362">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-362">✔</span></span> | <span data-ttu-id="4fff6-363">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-363">✔</span></span>
<span data-ttu-id="4fff6-364">Geometry. SymmetricDifference (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-364">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="4fff6-365">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-365">✔</span></span> | <span data-ttu-id="4fff6-366">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-366">✔</span></span> | <span data-ttu-id="4fff6-367">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-367">✔</span></span> | <span data-ttu-id="4fff6-368">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-368">✔</span></span>
<span data-ttu-id="4fff6-369">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="4fff6-369">Geometry.ToBinary()</span></span> | <span data-ttu-id="4fff6-370">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-370">✔</span></span> | <span data-ttu-id="4fff6-371">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-371">✔</span></span> | <span data-ttu-id="4fff6-372">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-372">✔</span></span> | <span data-ttu-id="4fff6-373">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-373">✔</span></span>
<span data-ttu-id="4fff6-374">Geometry. ToText ()</span><span class="sxs-lookup"><span data-stu-id="4fff6-374">Geometry.ToText()</span></span> | <span data-ttu-id="4fff6-375">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-375">✔</span></span> | <span data-ttu-id="4fff6-376">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-376">✔</span></span> | <span data-ttu-id="4fff6-377">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-377">✔</span></span> | <span data-ttu-id="4fff6-378">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-378">✔</span></span>
<span data-ttu-id="4fff6-379">Geometry. dokunuşları (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-379">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="4fff6-380">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-380">✔</span></span> | | <span data-ttu-id="4fff6-381">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-381">✔</span></span> | <span data-ttu-id="4fff6-382">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-382">✔</span></span>
<span data-ttu-id="4fff6-383">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="4fff6-383">Geometry.Union()</span></span> | | | <span data-ttu-id="4fff6-384">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-384">✔</span></span> | <span data-ttu-id="4fff6-385">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-385">✔</span></span>
<span data-ttu-id="4fff6-386">Geometry. Union (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-386">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="4fff6-387">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-387">✔</span></span> | <span data-ttu-id="4fff6-388">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-388">✔</span></span> | <span data-ttu-id="4fff6-389">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-389">✔</span></span> | <span data-ttu-id="4fff6-390">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-390">✔</span></span>
<span data-ttu-id="4fff6-391">Geometri. Içinde (geometri)</span><span class="sxs-lookup"><span data-stu-id="4fff6-391">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="4fff6-392">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-392">✔</span></span> | <span data-ttu-id="4fff6-393">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-393">✔</span></span> | <span data-ttu-id="4fff6-394">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-394">✔</span></span> | <span data-ttu-id="4fff6-395">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-395">✔</span></span>
<span data-ttu-id="4fff6-396">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="4fff6-396">GeometryCollection.Count</span></span> | <span data-ttu-id="4fff6-397">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-397">✔</span></span> | <span data-ttu-id="4fff6-398">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-398">✔</span></span> | <span data-ttu-id="4fff6-399">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-399">✔</span></span> | <span data-ttu-id="4fff6-400">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-400">✔</span></span>
<span data-ttu-id="4fff6-401">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="4fff6-401">GeometryCollection[int]</span></span> | <span data-ttu-id="4fff6-402">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-402">✔</span></span> | <span data-ttu-id="4fff6-403">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-403">✔</span></span> | <span data-ttu-id="4fff6-404">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-404">✔</span></span> | <span data-ttu-id="4fff6-405">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-405">✔</span></span>
<span data-ttu-id="4fff6-406">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="4fff6-406">LineString.Count</span></span> | <span data-ttu-id="4fff6-407">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-407">✔</span></span> | <span data-ttu-id="4fff6-408">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-408">✔</span></span> | <span data-ttu-id="4fff6-409">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-409">✔</span></span> | <span data-ttu-id="4fff6-410">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-410">✔</span></span>
<span data-ttu-id="4fff6-411">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="4fff6-411">LineString.EndPoint</span></span> | <span data-ttu-id="4fff6-412">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-412">✔</span></span> | <span data-ttu-id="4fff6-413">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-413">✔</span></span> | <span data-ttu-id="4fff6-414">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-414">✔</span></span> | <span data-ttu-id="4fff6-415">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-415">✔</span></span>
<span data-ttu-id="4fff6-416">LineString. GetPointN (int)</span><span class="sxs-lookup"><span data-stu-id="4fff6-416">LineString.GetPointN(int)</span></span> | <span data-ttu-id="4fff6-417">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-417">✔</span></span> | <span data-ttu-id="4fff6-418">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-418">✔</span></span> | <span data-ttu-id="4fff6-419">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-419">✔</span></span> | <span data-ttu-id="4fff6-420">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-420">✔</span></span>
<span data-ttu-id="4fff6-421">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="4fff6-421">LineString.IsClosed</span></span> | <span data-ttu-id="4fff6-422">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-422">✔</span></span> | <span data-ttu-id="4fff6-423">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-423">✔</span></span> | <span data-ttu-id="4fff6-424">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-424">✔</span></span> | <span data-ttu-id="4fff6-425">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-425">✔</span></span>
<span data-ttu-id="4fff6-426">LineString. IsRing</span><span class="sxs-lookup"><span data-stu-id="4fff6-426">LineString.IsRing</span></span> | <span data-ttu-id="4fff6-427">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-427">✔</span></span> | | <span data-ttu-id="4fff6-428">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-428">✔</span></span> | <span data-ttu-id="4fff6-429">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-429">✔</span></span>
<span data-ttu-id="4fff6-430">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="4fff6-430">LineString.StartPoint</span></span> | <span data-ttu-id="4fff6-431">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-431">✔</span></span> | <span data-ttu-id="4fff6-432">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-432">✔</span></span> | <span data-ttu-id="4fff6-433">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-433">✔</span></span> | <span data-ttu-id="4fff6-434">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-434">✔</span></span>
<span data-ttu-id="4fff6-435">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="4fff6-435">MultiLineString.IsClosed</span></span> | <span data-ttu-id="4fff6-436">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-436">✔</span></span> | <span data-ttu-id="4fff6-437">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-437">✔</span></span> | <span data-ttu-id="4fff6-438">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-438">✔</span></span> | <span data-ttu-id="4fff6-439">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-439">✔</span></span>
<span data-ttu-id="4fff6-440">Nokta. d</span><span class="sxs-lookup"><span data-stu-id="4fff6-440">Point.M</span></span> | <span data-ttu-id="4fff6-441">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-441">✔</span></span> | <span data-ttu-id="4fff6-442">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-442">✔</span></span> | <span data-ttu-id="4fff6-443">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-443">✔</span></span> | <span data-ttu-id="4fff6-444">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-444">✔</span></span>
<span data-ttu-id="4fff6-445">Point. X</span><span class="sxs-lookup"><span data-stu-id="4fff6-445">Point.X</span></span> | <span data-ttu-id="4fff6-446">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-446">✔</span></span> | <span data-ttu-id="4fff6-447">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-447">✔</span></span> | <span data-ttu-id="4fff6-448">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-448">✔</span></span> | <span data-ttu-id="4fff6-449">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-449">✔</span></span>
<span data-ttu-id="4fff6-450">Nokta. Y</span><span class="sxs-lookup"><span data-stu-id="4fff6-450">Point.Y</span></span> | <span data-ttu-id="4fff6-451">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-451">✔</span></span> | <span data-ttu-id="4fff6-452">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-452">✔</span></span> | <span data-ttu-id="4fff6-453">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-453">✔</span></span> | <span data-ttu-id="4fff6-454">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-454">✔</span></span>
<span data-ttu-id="4fff6-455">Point. Z</span><span class="sxs-lookup"><span data-stu-id="4fff6-455">Point.Z</span></span> | <span data-ttu-id="4fff6-456">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-456">✔</span></span> | <span data-ttu-id="4fff6-457">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-457">✔</span></span> | <span data-ttu-id="4fff6-458">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-458">✔</span></span> | <span data-ttu-id="4fff6-459">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-459">✔</span></span>
<span data-ttu-id="4fff6-460">Çokgen. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="4fff6-460">Polygon.ExteriorRing</span></span> | <span data-ttu-id="4fff6-461">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-461">✔</span></span> | <span data-ttu-id="4fff6-462">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-462">✔</span></span> | <span data-ttu-id="4fff6-463">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-463">✔</span></span> | <span data-ttu-id="4fff6-464">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-464">✔</span></span>
<span data-ttu-id="4fff6-465">Çokgen. Getınteriorringn (int)</span><span class="sxs-lookup"><span data-stu-id="4fff6-465">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="4fff6-466">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-466">✔</span></span> | <span data-ttu-id="4fff6-467">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-467">✔</span></span> | <span data-ttu-id="4fff6-468">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-468">✔</span></span> | <span data-ttu-id="4fff6-469">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-469">✔</span></span>
<span data-ttu-id="4fff6-470">Çokgen. Numinteriorhalkalar</span><span class="sxs-lookup"><span data-stu-id="4fff6-470">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="4fff6-471">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-471">✔</span></span> | <span data-ttu-id="4fff6-472">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-472">✔</span></span> | <span data-ttu-id="4fff6-473">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-473">✔</span></span> | <span data-ttu-id="4fff6-474">✔</span><span class="sxs-lookup"><span data-stu-id="4fff6-474">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4fff6-475">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4fff6-475">Additional resources</span></span>

* [<span data-ttu-id="4fff6-476">SQL Server uzamsal veriler</span><span class="sxs-lookup"><span data-stu-id="4fff6-476">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="4fff6-477">Spaıalite ana sayfası</span><span class="sxs-lookup"><span data-stu-id="4fff6-477">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="4fff6-478">Npgsql uzamsal belgeleri</span><span class="sxs-lookup"><span data-stu-id="4fff6-478">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="4fff6-479">PostGIS belgeleri</span><span class="sxs-lookup"><span data-stu-id="4fff6-479">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
