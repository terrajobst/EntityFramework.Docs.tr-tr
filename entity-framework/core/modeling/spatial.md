---
title: Uzamsal veriler-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 026df735473e31f1c1463c1fbc6f46c4fd6dfd4f
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921729"
---
# <a name="spatial-data"></a><span data-ttu-id="a3e3a-102">Uzamsal veriler</span><span class="sxs-lookup"><span data-stu-id="a3e3a-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="a3e3a-103">Bu özellik EF Core 2,2 ' ye eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="a3e3a-104">Uzamsal veriler, fiziksel konumu ve nesnelerin şeklini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="a3e3a-105">Birçok veritabanı, bu tür veriler için destek sağlar, böylece diğer verilerle birlikte dizinlenebilir ve sorgulanabilir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="a3e3a-106">Yaygın senaryolar, bir konumdan belirli bir uzaklıkta bulunan nesnelerin sorgulanmasını veya kenarlığını belirli bir konumu içeren nesneyi seçmeyi içerir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="a3e3a-107">EF Core, [Nettopologyısuite](https://github.com/NetTopologySuite/NetTopologySuite) uzamsal kitaplığı kullanılarak uzamsal veri türlerine eşlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="a3e3a-108">Yüklemenin</span><span class="sxs-lookup"><span data-stu-id="a3e3a-108">Installing</span></span>

<span data-ttu-id="a3e3a-109">Uzamsal verileri EF Core kullanmak için, uygun destekleyici NuGet paketini yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="a3e3a-110">Yüklemeniz gereken paket, kullanmakta olduğunuz sağlayıcıya bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="a3e3a-111">EF Core sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="a3e3a-111">EF Core Provider</span></span>                        | <span data-ttu-id="a3e3a-112">Uzamsal NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="a3e3a-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="a3e3a-113">Microsoft. EntityFrameworkCore. SqlServer</span><span class="sxs-lookup"><span data-stu-id="a3e3a-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="a3e3a-114">Microsoft. EntityFrameworkCore. SqlServer. Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="a3e3a-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="a3e3a-115">Microsoft. EntityFrameworkCore. SQLite</span><span class="sxs-lookup"><span data-stu-id="a3e3a-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="a3e3a-116">Microsoft. EntityFrameworkCore. SQLite. Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="a3e3a-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="a3e3a-117">Microsoft. EntityFrameworkCore. InMemory</span><span class="sxs-lookup"><span data-stu-id="a3e3a-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="a3e3a-118">Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="a3e3a-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="a3e3a-119">Npgsql. EntityFrameworkCore. PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="a3e3a-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="a3e3a-120">Npgsql. EntityFrameworkCore. PostgreSQL. Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="a3e3a-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="a3e3a-121">Tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="a3e3a-121">Reverse engineering</span></span>

<span data-ttu-id="a3e3a-122">Uzamsal NuGet paketleri de uzamsal özelliklerle [ters mühendislik](../managing-schemas/scaffolding.md) modellerini etkinleştirir, ancak veya `dotnet ef dbcontext scaffold`çalıştırmadan `Scaffold-DbContext` ***önce*** paketi yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="a3e3a-123">Bunu yapmazsanız, sütunlar için tür eşlemelerini bulmayın hakkında uyarılar alırsınız ve sütunlar atlanır.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="a3e3a-124">Nettopologyısuite (bir)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="a3e3a-125">Nettopologyısuite, .NET için uzamsal bir kitaplıktır.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="a3e3a-126">EF Core, modelinizdeki türler türlerini kullanarak veritabanındaki uzamsal veri türlerine eşlemeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="a3e3a-127">Uzamsal türlere, HI aracılığıyla eşlemeyi etkinleştirmek için, sağlayıcının DbContext seçenekleri oluşturucusunda Usenettopologyısuite yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="a3e3a-128">Örneğin, SQL Server ile bu şekilde çağrmış olursunuz.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="a3e3a-129">Birçok uzamsal veri türü vardır.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-129">There are several spatial data types.</span></span> <span data-ttu-id="a3e3a-130">Kullandığınız tür, izin vermek istediğiniz şekillerin türüne bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="a3e3a-131">Modelinizdeki özellikler için kullanabileceğiniz, bu türlerin hiyerarşisi aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="a3e3a-132">`NetTopologySuite.Geometries` Ad alanı içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="a3e3a-133">Geometrisi</span><span class="sxs-lookup"><span data-stu-id="a3e3a-133">Geometry</span></span>
  * <span data-ttu-id="a3e3a-134">Seçeneğinin</span><span class="sxs-lookup"><span data-stu-id="a3e3a-134">Point</span></span>
  * <span data-ttu-id="a3e3a-135">LineString</span><span class="sxs-lookup"><span data-stu-id="a3e3a-135">LineString</span></span>
  * <span data-ttu-id="a3e3a-136">Gen</span><span class="sxs-lookup"><span data-stu-id="a3e3a-136">Polygon</span></span>
  * <span data-ttu-id="a3e3a-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="a3e3a-137">GeometryCollection</span></span>
    * <span data-ttu-id="a3e3a-138">Noktalı</span><span class="sxs-lookup"><span data-stu-id="a3e3a-138">MultiPoint</span></span>
    * <span data-ttu-id="a3e3a-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="a3e3a-139">MultiLineString</span></span>
    * <span data-ttu-id="a3e3a-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="a3e3a-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="a3e3a-141">CurePolygon,, CompoundCurve ve tarafından desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="a3e3a-142">Temel geometri türünü kullanmak, özelliği tarafından herhangi bir tür şeklin belirtilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="a3e3a-143">Aşağıdaki varlık sınıfları, [geniş dünya içe örnek veritabanındaki](http://go.microsoft.com/fwlink/?LinkID=800630)tablolarla eşlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](http://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="a3e3a-144">Değer oluşturma</span><span class="sxs-lookup"><span data-stu-id="a3e3a-144">Creating values</span></span>

<span data-ttu-id="a3e3a-145">Geometri nesneleri oluşturmak için oluşturucuları kullanabilirsiniz; Ancak, bunun yerine bir geometri fabrikası kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="a3e3a-146">Bu, varsayılan bir SRID (Koordinatlar tarafından kullanılan uzamsal başvuru sistemi) belirtmenizi sağlar ve duyarlık modeli (hesaplamalar sırasında kullanılır) ve koordinat sırası gibi daha gelişmiş şeyler üzerinde denetim sağlar (bu boyutları belirler ve ölçüler--kullanılabilir).</span><span class="sxs-lookup"><span data-stu-id="a3e3a-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="a3e3a-147">4326, GPS ve diğer coğrafi sistemlerde kullanılan standart olan WGS 84 ' e başvurur.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="a3e3a-148">Boylam ve Enlem</span><span class="sxs-lookup"><span data-stu-id="a3e3a-148">Longitude and Latitude</span></span>

<span data-ttu-id="a3e3a-149">Ormallardaki koordinatlar X ve Y değerleri bakımından yapılır.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="a3e3a-150">Boylam ve enlem 'yi göstermek için boylam için X kullanın ve enlem için Y kullanın.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="a3e3a-151">Bunun, genellikle bu değerleri görebileceğiniz `latitude, longitude` biçimden **geriye doğru** olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="a3e3a-152">İstemci işlemleri sırasında SRID yoksayıldı</span><span class="sxs-lookup"><span data-stu-id="a3e3a-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="a3e3a-153">, İşlemler sırasında SRID değerlerini yoksayar.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="a3e3a-154">Bir düzlem koordinat sistemi olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="a3e3a-155">Diğer bir deyişle, boylam ve enlem açısından koordinatları belirtirseniz, uzaklık, uzunluk ve alan gibi istemci tarafından değerlendirilen bazı değerler, ölçü cinsinden değil, derece cinsinden olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="a3e3a-156">Daha anlamlı değerler için, önce bu değerleri hesaplamadan önce [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) gibi bir kitaplık kullanarak koordinatları başka bir koordinat sistemine proje yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="a3e3a-157">Bir işlem SQL üzerinden EF Core tarafından değerlendiriliyorsa, sonucun birimi veritabanı tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="a3e3a-158">İki şehir arasındaki mesafeyi hesaplamak için ProjNet4GeoAPI kullanılmasına bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="a3e3a-159">Veri Sorgulama</span><span class="sxs-lookup"><span data-stu-id="a3e3a-159">Querying Data</span></span>

<span data-ttu-id="a3e3a-160">LINQ 'ta, veritabanı işlevleri olarak kullanılabilen tüm yöntemler ve özellikler SQL 'e çevrilir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="a3e3a-161">Örneğin, uzaklık ve Contains yöntemleri aşağıdaki sorgularda çevrilir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="a3e3a-162">Bu makalenin sonundaki tabloda, çeşitli EF Core sağlayıcıları tarafından hangi üyelerin desteklendiği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="a3e3a-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a3e3a-163">SQL Server</span></span>

<span data-ttu-id="a3e3a-164">SQL Server kullanıyorsanız, bilmeniz gereken bazı ek şeyler vardır.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="a3e3a-165">Coğrafya veya geometri</span><span class="sxs-lookup"><span data-stu-id="a3e3a-165">Geography or geometry</span></span>

<span data-ttu-id="a3e3a-166">Varsayılan olarak, uzamsal özellikler SQL Server `geography` sütunlara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="a3e3a-167">Kullanmak `geometry`için, modelinizdeki [sütun türünü yapılandırın](xref:core/modeling/relational/data-types) .</span><span class="sxs-lookup"><span data-stu-id="a3e3a-167">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="a3e3a-168">Coğrafi Çokgen halkaları</span><span class="sxs-lookup"><span data-stu-id="a3e3a-168">Geography polygon rings</span></span>

<span data-ttu-id="a3e3a-169">`geography` Sütun türü kullanılırken, SQL Server dış halkada (veya kabukta) ve iç halkalarda (veya delikleri) ek gereksinimler uygular.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="a3e3a-170">Dış halkasının saatin tersi yönde ve iç halkalar saat yönünde yönlendirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="a3e3a-171">Bu, verileri veritabanına göndermeden önce bunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="a3e3a-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="a3e3a-172">FullGlobe</span></span>

<span data-ttu-id="a3e3a-173">SQL Server, `geography` sütun türü kullanılırken tam dünyayı temsil eden standart olmayan bir geometri türüne sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="a3e3a-174">Ayrıca, tam dünyaya göre (dış halka olmadan) çokgenler temsil etmenin bir yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="a3e3a-175">Bunlardan hiçbiri bu şekilde desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="a3e3a-176">Bu temel alan Fulldünya ve çokgenler, bu şekilde desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="a3e3a-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="a3e3a-177">SQLite</span></span>

<span data-ttu-id="a3e3a-178">SQLite kullanarak bunlar için bazı ek bilgiler aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="a3e3a-179">Gereksiz Ignite yükleniyor</span><span class="sxs-lookup"><span data-stu-id="a3e3a-179">Installing SpatiaLite</span></span>

<span data-ttu-id="a3e3a-180">Windows 'da yerel mod_spatialite kitaplığı, bir NuGet paketi bağımlılığı olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="a3e3a-181">Diğer platformların da ayrı olarak yüklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="a3e3a-182">Bu, genellikle bir yazılım paketi Yöneticisi kullanılarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="a3e3a-183">Örneğin, MacOS 'ta Ubuntu ve Homebrew üzerinde APT ' i kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="a3e3a-184">SRID yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a3e3a-184">Configuring SRID</span></span>

<span data-ttu-id="a3e3a-185">Gereksiz bir şekilde sütun başına bir SRID belirtmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="a3e3a-186">Varsayılan SRID `0`.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-186">The default SRID is `0`.</span></span> <span data-ttu-id="a3e3a-187">ForSqliteHasSrid yöntemini kullanarak farklı bir SRID belirtin.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="a3e3a-188">Boyut</span><span class="sxs-lookup"><span data-stu-id="a3e3a-188">Dimension</span></span>

<span data-ttu-id="a3e3a-189">SRID 'e benzer şekilde sütunun boyutu (veya ordinkılanan) de sütunun bir parçası olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="a3e3a-190">Varsayılan ordintlar X ve Y 'tir. ForSqliteHasDimension metodunu kullanarak ek ordinları (Z ve d) etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="a3e3a-191">Çevrilmiş Işlemler</span><span class="sxs-lookup"><span data-stu-id="a3e3a-191">Translated Operations</span></span>

<span data-ttu-id="a3e3a-192">Bu tabloda, her bir EF Core sağlayıcısı tarafından SQL 'e çevrilen her bir bu üye gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a3e3a-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="a3e3a-193">Nettopologyısuite</span><span class="sxs-lookup"><span data-stu-id="a3e3a-193">NetTopologySuite</span></span> | <span data-ttu-id="a3e3a-194">SQL Server (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-194">SQL Server (geometry)</span></span> | <span data-ttu-id="a3e3a-195">SQL Server (coğrafya)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-195">SQL Server (geography)</span></span> | <span data-ttu-id="a3e3a-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="a3e3a-196">SQLite</span></span> | <span data-ttu-id="a3e3a-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="a3e3a-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="a3e3a-198">Geometry. alan</span><span class="sxs-lookup"><span data-stu-id="a3e3a-198">Geometry.Area</span></span> | <span data-ttu-id="a3e3a-199">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-199">✔</span></span> | <span data-ttu-id="a3e3a-200">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-200">✔</span></span> | <span data-ttu-id="a3e3a-201">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-201">✔</span></span> | <span data-ttu-id="a3e3a-202">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-202">✔</span></span>
<span data-ttu-id="a3e3a-203">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="a3e3a-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="a3e3a-204">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-204">✔</span></span> | <span data-ttu-id="a3e3a-205">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-205">✔</span></span> | <span data-ttu-id="a3e3a-206">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-206">✔</span></span> | <span data-ttu-id="a3e3a-207">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-207">✔</span></span>
<span data-ttu-id="a3e3a-208">Geometry. AsText ()</span><span class="sxs-lookup"><span data-stu-id="a3e3a-208">Geometry.AsText()</span></span> | <span data-ttu-id="a3e3a-209">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-209">✔</span></span> | <span data-ttu-id="a3e3a-210">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-210">✔</span></span> | <span data-ttu-id="a3e3a-211">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-211">✔</span></span> | <span data-ttu-id="a3e3a-212">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-212">✔</span></span>
<span data-ttu-id="a3e3a-213">Geometry. sınır</span><span class="sxs-lookup"><span data-stu-id="a3e3a-213">Geometry.Boundary</span></span> | <span data-ttu-id="a3e3a-214">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-214">✔</span></span> | | <span data-ttu-id="a3e3a-215">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-215">✔</span></span> | <span data-ttu-id="a3e3a-216">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-216">✔</span></span>
<span data-ttu-id="a3e3a-217">Geometry. Buffer (Double)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="a3e3a-218">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-218">✔</span></span> | <span data-ttu-id="a3e3a-219">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-219">✔</span></span> | <span data-ttu-id="a3e3a-220">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-220">✔</span></span> | <span data-ttu-id="a3e3a-221">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-221">✔</span></span>
<span data-ttu-id="a3e3a-222">Geometry. Buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="a3e3a-223">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-223">✔</span></span>
<span data-ttu-id="a3e3a-224">Geometry. Centroıd</span><span class="sxs-lookup"><span data-stu-id="a3e3a-224">Geometry.Centroid</span></span> | <span data-ttu-id="a3e3a-225">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-225">✔</span></span> | | <span data-ttu-id="a3e3a-226">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-226">✔</span></span> | <span data-ttu-id="a3e3a-227">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-227">✔</span></span>
<span data-ttu-id="a3e3a-228">Geometry. Contains (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-228">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="a3e3a-229">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-229">✔</span></span> | <span data-ttu-id="a3e3a-230">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-230">✔</span></span> | <span data-ttu-id="a3e3a-231">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-231">✔</span></span> | <span data-ttu-id="a3e3a-232">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-232">✔</span></span>
<span data-ttu-id="a3e3a-233">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="a3e3a-233">Geometry.ConvexHull()</span></span> | <span data-ttu-id="a3e3a-234">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-234">✔</span></span> | <span data-ttu-id="a3e3a-235">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-235">✔</span></span> | <span data-ttu-id="a3e3a-236">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-236">✔</span></span> | <span data-ttu-id="a3e3a-237">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-237">✔</span></span>
<span data-ttu-id="a3e3a-238">Geometry. kapak Edby (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-238">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="a3e3a-239">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-239">✔</span></span> | <span data-ttu-id="a3e3a-240">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-240">✔</span></span>
<span data-ttu-id="a3e3a-241">Geometry. kapsıyorsa (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-241">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="a3e3a-242">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-242">✔</span></span> | <span data-ttu-id="a3e3a-243">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-243">✔</span></span>
<span data-ttu-id="a3e3a-244">Geometry. Kesişmeleri (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-244">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="a3e3a-245">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-245">✔</span></span> | | <span data-ttu-id="a3e3a-246">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-246">✔</span></span> | <span data-ttu-id="a3e3a-247">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-247">✔</span></span>
<span data-ttu-id="a3e3a-248">Geometry. fark (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-248">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="a3e3a-249">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-249">✔</span></span> | <span data-ttu-id="a3e3a-250">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-250">✔</span></span> | <span data-ttu-id="a3e3a-251">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-251">✔</span></span> | <span data-ttu-id="a3e3a-252">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-252">✔</span></span>
<span data-ttu-id="a3e3a-253">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="a3e3a-253">Geometry.Dimension</span></span> | <span data-ttu-id="a3e3a-254">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-254">✔</span></span> | <span data-ttu-id="a3e3a-255">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-255">✔</span></span> | <span data-ttu-id="a3e3a-256">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-256">✔</span></span> | <span data-ttu-id="a3e3a-257">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-257">✔</span></span>
<span data-ttu-id="a3e3a-258">Geometry. ayrık (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-258">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="a3e3a-259">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-259">✔</span></span> | <span data-ttu-id="a3e3a-260">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-260">✔</span></span> | <span data-ttu-id="a3e3a-261">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-261">✔</span></span> | <span data-ttu-id="a3e3a-262">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-262">✔</span></span>
<span data-ttu-id="a3e3a-263">Geometry. mesafe (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-263">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="a3e3a-264">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-264">✔</span></span> | <span data-ttu-id="a3e3a-265">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-265">✔</span></span> | <span data-ttu-id="a3e3a-266">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-266">✔</span></span> | <span data-ttu-id="a3e3a-267">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-267">✔</span></span>
<span data-ttu-id="a3e3a-268">Geometry. Envelope</span><span class="sxs-lookup"><span data-stu-id="a3e3a-268">Geometry.Envelope</span></span> | <span data-ttu-id="a3e3a-269">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-269">✔</span></span> | | <span data-ttu-id="a3e3a-270">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-270">✔</span></span> | <span data-ttu-id="a3e3a-271">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-271">✔</span></span>
<span data-ttu-id="a3e3a-272">Geometry. Equalsexyasası (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-272">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="a3e3a-273">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-273">✔</span></span>
<span data-ttu-id="a3e3a-274">Geometry. EqualsTopologically (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-274">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="a3e3a-275">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-275">✔</span></span> | <span data-ttu-id="a3e3a-276">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-276">✔</span></span> | <span data-ttu-id="a3e3a-277">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-277">✔</span></span> | <span data-ttu-id="a3e3a-278">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-278">✔</span></span>
<span data-ttu-id="a3e3a-279">Geometry. GeometryType</span><span class="sxs-lookup"><span data-stu-id="a3e3a-279">Geometry.GeometryType</span></span> | <span data-ttu-id="a3e3a-280">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-280">✔</span></span> | <span data-ttu-id="a3e3a-281">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-281">✔</span></span> | <span data-ttu-id="a3e3a-282">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-282">✔</span></span> | <span data-ttu-id="a3e3a-283">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-283">✔</span></span>
<span data-ttu-id="a3e3a-284">Geometry. Getgeometrik YN (int)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-284">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="a3e3a-285">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-285">✔</span></span> | | <span data-ttu-id="a3e3a-286">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-286">✔</span></span> | <span data-ttu-id="a3e3a-287">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-287">✔</span></span>
<span data-ttu-id="a3e3a-288">Geometri. ınteriorpoint</span><span class="sxs-lookup"><span data-stu-id="a3e3a-288">Geometry.InteriorPoint</span></span> | <span data-ttu-id="a3e3a-289">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-289">✔</span></span> | | <span data-ttu-id="a3e3a-290">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-290">✔</span></span>
<span data-ttu-id="a3e3a-291">Geometry. kesişmesi (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-291">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="a3e3a-292">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-292">✔</span></span> | <span data-ttu-id="a3e3a-293">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-293">✔</span></span> | <span data-ttu-id="a3e3a-294">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-294">✔</span></span> | <span data-ttu-id="a3e3a-295">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-295">✔</span></span>
<span data-ttu-id="a3e3a-296">Geometry. kesişme (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-296">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="a3e3a-297">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-297">✔</span></span> | <span data-ttu-id="a3e3a-298">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-298">✔</span></span> | <span data-ttu-id="a3e3a-299">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-299">✔</span></span> | <span data-ttu-id="a3e3a-300">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-300">✔</span></span>
<span data-ttu-id="a3e3a-301">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="a3e3a-301">Geometry.IsEmpty</span></span> | <span data-ttu-id="a3e3a-302">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-302">✔</span></span> | <span data-ttu-id="a3e3a-303">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-303">✔</span></span> | <span data-ttu-id="a3e3a-304">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-304">✔</span></span> | <span data-ttu-id="a3e3a-305">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-305">✔</span></span>
<span data-ttu-id="a3e3a-306">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="a3e3a-306">Geometry.IsSimple</span></span> | <span data-ttu-id="a3e3a-307">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-307">✔</span></span> | | <span data-ttu-id="a3e3a-308">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-308">✔</span></span> | <span data-ttu-id="a3e3a-309">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-309">✔</span></span>
<span data-ttu-id="a3e3a-310">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="a3e3a-310">Geometry.IsValid</span></span> | <span data-ttu-id="a3e3a-311">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-311">✔</span></span> | <span data-ttu-id="a3e3a-312">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-312">✔</span></span> | <span data-ttu-id="a3e3a-313">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-313">✔</span></span> | <span data-ttu-id="a3e3a-314">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-314">✔</span></span>
<span data-ttu-id="a3e3a-315">Geometry. ıswithındistance (geometri, Double)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-315">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="a3e3a-316">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-316">✔</span></span> | | <span data-ttu-id="a3e3a-317">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-317">✔</span></span>
<span data-ttu-id="a3e3a-318">Geometry. length</span><span class="sxs-lookup"><span data-stu-id="a3e3a-318">Geometry.Length</span></span> | <span data-ttu-id="a3e3a-319">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-319">✔</span></span> | <span data-ttu-id="a3e3a-320">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-320">✔</span></span> | <span data-ttu-id="a3e3a-321">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-321">✔</span></span> | <span data-ttu-id="a3e3a-322">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-322">✔</span></span>
<span data-ttu-id="a3e3a-323">Geometry. Numgeometrileri</span><span class="sxs-lookup"><span data-stu-id="a3e3a-323">Geometry.NumGeometries</span></span> | <span data-ttu-id="a3e3a-324">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-324">✔</span></span> | <span data-ttu-id="a3e3a-325">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-325">✔</span></span> | <span data-ttu-id="a3e3a-326">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-326">✔</span></span> | <span data-ttu-id="a3e3a-327">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-327">✔</span></span>
<span data-ttu-id="a3e3a-328">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="a3e3a-328">Geometry.NumPoints</span></span> | <span data-ttu-id="a3e3a-329">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-329">✔</span></span> | <span data-ttu-id="a3e3a-330">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-330">✔</span></span> | <span data-ttu-id="a3e3a-331">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-331">✔</span></span> | <span data-ttu-id="a3e3a-332">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-332">✔</span></span>
<span data-ttu-id="a3e3a-333">Geometry. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="a3e3a-333">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="a3e3a-334">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-334">✔</span></span> | <span data-ttu-id="a3e3a-335">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-335">✔</span></span> | <span data-ttu-id="a3e3a-336">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-336">✔</span></span>
<span data-ttu-id="a3e3a-337">Geometry. örtüşüyor (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-337">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="a3e3a-338">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-338">✔</span></span> | <span data-ttu-id="a3e3a-339">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-339">✔</span></span> | <span data-ttu-id="a3e3a-340">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-340">✔</span></span> | <span data-ttu-id="a3e3a-341">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-341">✔</span></span>
<span data-ttu-id="a3e3a-342">Geometry. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="a3e3a-342">Geometry.PointOnSurface</span></span> | <span data-ttu-id="a3e3a-343">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-343">✔</span></span> | | <span data-ttu-id="a3e3a-344">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-344">✔</span></span> | <span data-ttu-id="a3e3a-345">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-345">✔</span></span>
<span data-ttu-id="a3e3a-346">Geometry. Ilişkilendir (geometri, dize)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-346">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="a3e3a-347">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-347">✔</span></span> | | <span data-ttu-id="a3e3a-348">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-348">✔</span></span> | <span data-ttu-id="a3e3a-349">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-349">✔</span></span>
<span data-ttu-id="a3e3a-350">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="a3e3a-350">Geometry.Reverse()</span></span> | | | <span data-ttu-id="a3e3a-351">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-351">✔</span></span> | <span data-ttu-id="a3e3a-352">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-352">✔</span></span>
<span data-ttu-id="a3e3a-353">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="a3e3a-353">Geometry.SRID</span></span> | <span data-ttu-id="a3e3a-354">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-354">✔</span></span> | <span data-ttu-id="a3e3a-355">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-355">✔</span></span> | <span data-ttu-id="a3e3a-356">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-356">✔</span></span> | <span data-ttu-id="a3e3a-357">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-357">✔</span></span>
<span data-ttu-id="a3e3a-358">Geometry. SymmetricDifference (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-358">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="a3e3a-359">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-359">✔</span></span> | <span data-ttu-id="a3e3a-360">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-360">✔</span></span> | <span data-ttu-id="a3e3a-361">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-361">✔</span></span> | <span data-ttu-id="a3e3a-362">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-362">✔</span></span>
<span data-ttu-id="a3e3a-363">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="a3e3a-363">Geometry.ToBinary()</span></span> | <span data-ttu-id="a3e3a-364">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-364">✔</span></span> | <span data-ttu-id="a3e3a-365">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-365">✔</span></span> | <span data-ttu-id="a3e3a-366">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-366">✔</span></span> | <span data-ttu-id="a3e3a-367">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-367">✔</span></span>
<span data-ttu-id="a3e3a-368">Geometry. ToText ()</span><span class="sxs-lookup"><span data-stu-id="a3e3a-368">Geometry.ToText()</span></span> | <span data-ttu-id="a3e3a-369">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-369">✔</span></span> | <span data-ttu-id="a3e3a-370">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-370">✔</span></span> | <span data-ttu-id="a3e3a-371">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-371">✔</span></span> | <span data-ttu-id="a3e3a-372">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-372">✔</span></span>
<span data-ttu-id="a3e3a-373">Geometry. dokunuşları (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-373">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="a3e3a-374">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-374">✔</span></span> | | <span data-ttu-id="a3e3a-375">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-375">✔</span></span> | <span data-ttu-id="a3e3a-376">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-376">✔</span></span>
<span data-ttu-id="a3e3a-377">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="a3e3a-377">Geometry.Union()</span></span> | | | <span data-ttu-id="a3e3a-378">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-378">✔</span></span>
<span data-ttu-id="a3e3a-379">Geometry. Union (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-379">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="a3e3a-380">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-380">✔</span></span> | <span data-ttu-id="a3e3a-381">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-381">✔</span></span> | <span data-ttu-id="a3e3a-382">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-382">✔</span></span> | <span data-ttu-id="a3e3a-383">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-383">✔</span></span>
<span data-ttu-id="a3e3a-384">Geometri. Içinde (geometri)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-384">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="a3e3a-385">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-385">✔</span></span> | <span data-ttu-id="a3e3a-386">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-386">✔</span></span> | <span data-ttu-id="a3e3a-387">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-387">✔</span></span> | <span data-ttu-id="a3e3a-388">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-388">✔</span></span>
<span data-ttu-id="a3e3a-389">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="a3e3a-389">GeometryCollection.Count</span></span> | <span data-ttu-id="a3e3a-390">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-390">✔</span></span> | <span data-ttu-id="a3e3a-391">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-391">✔</span></span> | <span data-ttu-id="a3e3a-392">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-392">✔</span></span> | <span data-ttu-id="a3e3a-393">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-393">✔</span></span>
<span data-ttu-id="a3e3a-394">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="a3e3a-394">GeometryCollection[int]</span></span> | <span data-ttu-id="a3e3a-395">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-395">✔</span></span> | <span data-ttu-id="a3e3a-396">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-396">✔</span></span> | <span data-ttu-id="a3e3a-397">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-397">✔</span></span> | <span data-ttu-id="a3e3a-398">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-398">✔</span></span>
<span data-ttu-id="a3e3a-399">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="a3e3a-399">LineString.Count</span></span> | <span data-ttu-id="a3e3a-400">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-400">✔</span></span> | <span data-ttu-id="a3e3a-401">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-401">✔</span></span> | <span data-ttu-id="a3e3a-402">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-402">✔</span></span> | <span data-ttu-id="a3e3a-403">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-403">✔</span></span>
<span data-ttu-id="a3e3a-404">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="a3e3a-404">LineString.EndPoint</span></span> | <span data-ttu-id="a3e3a-405">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-405">✔</span></span> | <span data-ttu-id="a3e3a-406">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-406">✔</span></span> | <span data-ttu-id="a3e3a-407">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-407">✔</span></span> | <span data-ttu-id="a3e3a-408">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-408">✔</span></span>
<span data-ttu-id="a3e3a-409">LineString. GetPointN (int)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-409">LineString.GetPointN(int)</span></span> | <span data-ttu-id="a3e3a-410">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-410">✔</span></span> | <span data-ttu-id="a3e3a-411">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-411">✔</span></span> | <span data-ttu-id="a3e3a-412">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-412">✔</span></span> | <span data-ttu-id="a3e3a-413">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-413">✔</span></span>
<span data-ttu-id="a3e3a-414">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="a3e3a-414">LineString.IsClosed</span></span> | <span data-ttu-id="a3e3a-415">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-415">✔</span></span> | <span data-ttu-id="a3e3a-416">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-416">✔</span></span> | <span data-ttu-id="a3e3a-417">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-417">✔</span></span> | <span data-ttu-id="a3e3a-418">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-418">✔</span></span>
<span data-ttu-id="a3e3a-419">LineString. IsRing</span><span class="sxs-lookup"><span data-stu-id="a3e3a-419">LineString.IsRing</span></span> | <span data-ttu-id="a3e3a-420">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-420">✔</span></span> | | <span data-ttu-id="a3e3a-421">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-421">✔</span></span> | <span data-ttu-id="a3e3a-422">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-422">✔</span></span>
<span data-ttu-id="a3e3a-423">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="a3e3a-423">LineString.StartPoint</span></span> | <span data-ttu-id="a3e3a-424">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-424">✔</span></span> | <span data-ttu-id="a3e3a-425">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-425">✔</span></span> | <span data-ttu-id="a3e3a-426">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-426">✔</span></span> | <span data-ttu-id="a3e3a-427">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-427">✔</span></span>
<span data-ttu-id="a3e3a-428">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="a3e3a-428">MultiLineString.IsClosed</span></span> | <span data-ttu-id="a3e3a-429">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-429">✔</span></span> | <span data-ttu-id="a3e3a-430">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-430">✔</span></span> | <span data-ttu-id="a3e3a-431">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-431">✔</span></span> | <span data-ttu-id="a3e3a-432">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-432">✔</span></span>
<span data-ttu-id="a3e3a-433">Nokta. d</span><span class="sxs-lookup"><span data-stu-id="a3e3a-433">Point.M</span></span> | <span data-ttu-id="a3e3a-434">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-434">✔</span></span> | <span data-ttu-id="a3e3a-435">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-435">✔</span></span> | <span data-ttu-id="a3e3a-436">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-436">✔</span></span> | <span data-ttu-id="a3e3a-437">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-437">✔</span></span>
<span data-ttu-id="a3e3a-438">Point. X</span><span class="sxs-lookup"><span data-stu-id="a3e3a-438">Point.X</span></span> | <span data-ttu-id="a3e3a-439">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-439">✔</span></span> | <span data-ttu-id="a3e3a-440">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-440">✔</span></span> | <span data-ttu-id="a3e3a-441">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-441">✔</span></span> | <span data-ttu-id="a3e3a-442">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-442">✔</span></span>
<span data-ttu-id="a3e3a-443">Nokta. Y</span><span class="sxs-lookup"><span data-stu-id="a3e3a-443">Point.Y</span></span> | <span data-ttu-id="a3e3a-444">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-444">✔</span></span> | <span data-ttu-id="a3e3a-445">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-445">✔</span></span> | <span data-ttu-id="a3e3a-446">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-446">✔</span></span> | <span data-ttu-id="a3e3a-447">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-447">✔</span></span>
<span data-ttu-id="a3e3a-448">Point. Z</span><span class="sxs-lookup"><span data-stu-id="a3e3a-448">Point.Z</span></span> | <span data-ttu-id="a3e3a-449">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-449">✔</span></span> | <span data-ttu-id="a3e3a-450">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-450">✔</span></span> | <span data-ttu-id="a3e3a-451">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-451">✔</span></span> | <span data-ttu-id="a3e3a-452">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-452">✔</span></span>
<span data-ttu-id="a3e3a-453">Çokgen. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="a3e3a-453">Polygon.ExteriorRing</span></span> | <span data-ttu-id="a3e3a-454">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-454">✔</span></span> | <span data-ttu-id="a3e3a-455">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-455">✔</span></span> | <span data-ttu-id="a3e3a-456">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-456">✔</span></span> | <span data-ttu-id="a3e3a-457">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-457">✔</span></span>
<span data-ttu-id="a3e3a-458">Çokgen. Getınteriorringn (int)</span><span class="sxs-lookup"><span data-stu-id="a3e3a-458">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="a3e3a-459">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-459">✔</span></span> | <span data-ttu-id="a3e3a-460">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-460">✔</span></span> | <span data-ttu-id="a3e3a-461">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-461">✔</span></span> | <span data-ttu-id="a3e3a-462">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-462">✔</span></span>
<span data-ttu-id="a3e3a-463">Çokgen. Numinteriorhalkalar</span><span class="sxs-lookup"><span data-stu-id="a3e3a-463">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="a3e3a-464">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-464">✔</span></span> | <span data-ttu-id="a3e3a-465">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-465">✔</span></span> | <span data-ttu-id="a3e3a-466">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-466">✔</span></span> | <span data-ttu-id="a3e3a-467">✔</span><span class="sxs-lookup"><span data-stu-id="a3e3a-467">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a3e3a-468">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a3e3a-468">Additional resources</span></span>

* [<span data-ttu-id="a3e3a-469">SQL Server uzamsal veriler</span><span class="sxs-lookup"><span data-stu-id="a3e3a-469">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="a3e3a-470">Spaıalite ana sayfası</span><span class="sxs-lookup"><span data-stu-id="a3e3a-470">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="a3e3a-471">Npgsql uzamsal belgeleri</span><span class="sxs-lookup"><span data-stu-id="a3e3a-471">Npgsql Spatial Documentation</span></span>](http://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="a3e3a-472">PostGIS belgeleri</span><span class="sxs-lookup"><span data-stu-id="a3e3a-472">PostGIS Documentation</span></span>](http://postgis.net/documentation/)
