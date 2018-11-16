---
title: Uzamsal veriler - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 49c18758af2f2383ea082ead2f6df4c022152b36
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688814"
---
# <a name="spatial-data"></a><span data-ttu-id="07f6c-102">Uzamsal veriler</span><span class="sxs-lookup"><span data-stu-id="07f6c-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="07f6c-103">Bu özellik, EF Core 2.2 içinde yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="07f6c-104">Uzamsal veriler fiziksel konuma ve nesnelerin şeklini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="07f6c-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="07f6c-105">Çok sayıda veritabanı bu tür veriler için destek sağlar, böylece dizine ve yanı sıra diğer veriler sorgulandı.</span><span class="sxs-lookup"><span data-stu-id="07f6c-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="07f6c-106">Bir konumdan belirli bir uzaklık içindeki nesneler için sorgulama ve belirli bir konuma kenarlığını içeren nesneyi yaygın senaryolar şunlardır.</span><span class="sxs-lookup"><span data-stu-id="07f6c-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="07f6c-107">EF Core destekler kullanarak uzamsal veri türleri için eşleme [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) uzamsal kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="07f6c-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="07f6c-108">Yükleme</span><span class="sxs-lookup"><span data-stu-id="07f6c-108">Installing</span></span>

<span data-ttu-id="07f6c-109">Uzamsal veriler EF Core ile kullanmak için uygun destek NuGet paketini yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="07f6c-110">Hangi paketini yüklemeniz gerekir, kullanmakta olduğunuz sağlayıcısına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="07f6c-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="07f6c-111">EF Core sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="07f6c-111">EF Core Provider</span></span>                        | <span data-ttu-id="07f6c-112">Uzamsal NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="07f6c-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="07f6c-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="07f6c-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="07f6c-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="07f6c-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="07f6c-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="07f6c-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="07f6c-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="07f6c-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="07f6c-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="07f6c-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="07f6c-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="07f6c-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="07f6c-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="07f6c-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="07f6c-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="07f6c-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="07f6c-121">Tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="07f6c-121">Reverse engineering</span></span>

<span data-ttu-id="07f6c-122">Uzamsal NuGet ayrıca etkinleştir paketleri [tersine mühendislik](../managing-schemas/scaffolding.md) uzamsal özellikler, ancak modelleriyle gereksinim paketi yüklemek ***önce*** çalıştıran `Scaffold-DbContext` veya `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="07f6c-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="07f6c-123">Aksi takdirde, türü eşlemeleri sütunlarını paylaşımın hakkında uyarılar alırsınız ve sütunları atlanacak.</span><span class="sxs-lookup"><span data-stu-id="07f6c-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="07f6c-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="07f6c-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="07f6c-125">NetTopologySuite, .NET için uzamsal bir kitaplıktır.</span><span class="sxs-lookup"><span data-stu-id="07f6c-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="07f6c-126">EF Core modelinizde NTS türlerini kullanarak uzamsal veri türleri veritabanındaki eşleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="07f6c-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="07f6c-127">Kesme noktalarını aracılığıyla uzamsal türler için eşleme etkinleştirmek için sağlayıcının DbContext seçenekleri Oluşturucusu'UseNetTopologySuite yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="07f6c-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="07f6c-128">Örneğin, SQL Server ile Bunu şöyle çağırmanız.</span><span class="sxs-lookup"><span data-stu-id="07f6c-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="07f6c-129">Birkaç uzamsal veri türleri vardır.</span><span class="sxs-lookup"><span data-stu-id="07f6c-129">There are several spatial data types.</span></span> <span data-ttu-id="07f6c-130">Hangi türü kullandığınız izin vermek istediğiniz şekillerinin türlerine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="07f6c-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="07f6c-131">Modelinizde özellikleri için kullanabileceğiniz NTS türlerin hiyerarşisi aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="07f6c-132">Bunlar içinde bulunduğu `NetTopologySuite.Geometries` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="07f6c-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span> <span data-ttu-id="07f6c-133">İlgili arabirimlere GeoAPI paketindeki (`GeoAPI.Geometries` ad alanı) de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-133">Corresponding interfaces in the GeoAPI package (`GeoAPI.Geometries` namespace) can also be used.</span></span>

* <span data-ttu-id="07f6c-134">Geometri</span><span class="sxs-lookup"><span data-stu-id="07f6c-134">Geometry</span></span>
  * <span data-ttu-id="07f6c-135">Noktası</span><span class="sxs-lookup"><span data-stu-id="07f6c-135">Point</span></span>
  * <span data-ttu-id="07f6c-136">LineString</span><span class="sxs-lookup"><span data-stu-id="07f6c-136">LineString</span></span>
  * <span data-ttu-id="07f6c-137">Çokgen</span><span class="sxs-lookup"><span data-stu-id="07f6c-137">Polygon</span></span>
  * <span data-ttu-id="07f6c-138">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="07f6c-138">GeometryCollection</span></span>
    * <span data-ttu-id="07f6c-139">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="07f6c-139">MultiPoint</span></span>
    * <span data-ttu-id="07f6c-140">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="07f6c-140">MultiLineString</span></span>
    * <span data-ttu-id="07f6c-141">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="07f6c-141">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="07f6c-142">CircularString, CompoundCurve ve CurePolygon NTS tarafından desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="07f6c-142">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="07f6c-143">Temel geometrik türünü kullanarak her türlü özelliği tarafından belirtilen şeklin sağlar.</span><span class="sxs-lookup"><span data-stu-id="07f6c-143">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="07f6c-144">Aşağıdaki varlık sınıflarının tabloları eşleştirmek için kullanılabilecek [Wide World Importers örnek veritabanını](http://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="07f6c-144">The following entity classes could be used to map to tables in the [Wide World Importers sample database](http://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="07f6c-145">Değerleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="07f6c-145">Creating values</span></span>

<span data-ttu-id="07f6c-146">Oluşturucular, geometri nesneleri oluşturmak için kullanabilirsiniz; Ancak, geometri Fabrika kullanmayı NTS önerir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-146">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="07f6c-147">Bu varsayılan SRID (koordinatları tarafından kullanılan uzamsal başvuru sistemi) belirtmenize olanak tanır ve size (hesaplama sırasında kullanılır) duyarlılığı modeli ve (hangi ordinates--boyutları belirler koordinat dizisi gibi daha gelişmiş işlemler üzerinde denetim ve ölçüler--kullanılabilir).</span><span class="sxs-lookup"><span data-stu-id="07f6c-147">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="07f6c-148">4326 WGS 84, GPS ve diğer coğrafi sistemlerinde kullanılan standart bir ifade eder.</span><span class="sxs-lookup"><span data-stu-id="07f6c-148">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="07f6c-149">Boylam ve enlem</span><span class="sxs-lookup"><span data-stu-id="07f6c-149">Longitude and Latitude</span></span>

<span data-ttu-id="07f6c-150">Kesme noktalarını koordinatlarında olan açısından X ve Y değerleri.</span><span class="sxs-lookup"><span data-stu-id="07f6c-150">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="07f6c-151">Enlemini ve boylamını temsil etmek için X boylam ve Y enlem için kullanın.</span><span class="sxs-lookup"><span data-stu-id="07f6c-151">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="07f6c-152">Bu Not **geriye doğru** gelen `latitude, longitude` bu değerleri genellikle görebileceğiniz biçimi.</span><span class="sxs-lookup"><span data-stu-id="07f6c-152">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="07f6c-153">İstemci işlemleri sırasında SRID yoksayıldı</span><span class="sxs-lookup"><span data-stu-id="07f6c-153">SRID Ignored during client operations</span></span>

<span data-ttu-id="07f6c-154">Kesme noktalarını işlemleri sırasında SRID değerleri yok sayar.</span><span class="sxs-lookup"><span data-stu-id="07f6c-154">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="07f6c-155">Bu, bir düzlem koordinat sistemi varsayar.</span><span class="sxs-lookup"><span data-stu-id="07f6c-155">It assumes a planar coordinate system.</span></span> <span data-ttu-id="07f6c-156">Boylam ve enlem, olmayan ölçümleri derece cinsinden uzaklık ve uzunluk alanı gibi bazı istemci hesaplanan değerler açısından koordinatları belirtirseniz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-156">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="07f6c-157">Daha anlamlı değerleri için önce kullanarak bir kitaplığı gibi başka bir koordinat sistemini koordinatları proje gerekir [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) önce bu değerleri hesaplama.</span><span class="sxs-lookup"><span data-stu-id="07f6c-157">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="07f6c-158">Bir işlem tarafından EF Core ile SQL server olarak değerlendirilen olursa sonucunun birim veritabanı tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-158">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="07f6c-159">İki şehirler arasındaki uzaklığı hesaplamak için ProjNet4GeoAPI kullanmaya bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-159">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="07f6c-160">Veri Sorgulama</span><span class="sxs-lookup"><span data-stu-id="07f6c-160">Querying Data</span></span>

<span data-ttu-id="07f6c-161">LINQ to SQL NTS yöntemleri ve özellikleri veritabanı işlevleri kullanılabilir çevrilir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-161">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="07f6c-162">Örneğin, içerir ve uzaklık yöntemleri aşağıdaki sorgularda çevrilir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-162">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="07f6c-163">Bu makalenin sonundaki tabloda, çeşitli EF Core sağlayıcıları tarafından desteklenen üyeleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-163">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="07f6c-164">SQL Server</span><span class="sxs-lookup"><span data-stu-id="07f6c-164">SQL Server</span></span>

<span data-ttu-id="07f6c-165">SQL Server kullanıyorsanız, bilmeniz gereken bazı ek işlemler vardır.</span><span class="sxs-lookup"><span data-stu-id="07f6c-165">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="07f6c-166">Coğrafya veya geometri</span><span class="sxs-lookup"><span data-stu-id="07f6c-166">Geography or geometry</span></span>

<span data-ttu-id="07f6c-167">Varsayılan olarak, uzamsal özellikler için eşlenen `geography` SQL Server'daki sütun.</span><span class="sxs-lookup"><span data-stu-id="07f6c-167">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="07f6c-168">Kullanılacak `geometry`, [sütun türü yapılandırma](xref:core/modeling/relational/data-types) modelinizdeki.</span><span class="sxs-lookup"><span data-stu-id="07f6c-168">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="07f6c-169">Coğrafya Çokgen halkaları</span><span class="sxs-lookup"><span data-stu-id="07f6c-169">Geography polygon rings</span></span>

<span data-ttu-id="07f6c-170">Kullanırken `geography` sütun türü, SQL Server, dış halkası (veya kabuk) ek gereksinimler uygular ve iç halkaları (veya açıkları).</span><span class="sxs-lookup"><span data-stu-id="07f6c-170">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="07f6c-171">Halkasının saat yönünün tersine yönelimli olması gerekir ve iç saat yönünde halkası.</span><span class="sxs-lookup"><span data-stu-id="07f6c-171">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="07f6c-172">Kesme noktalarını bu değerleri veritabanına göndermeden önce doğrular.</span><span class="sxs-lookup"><span data-stu-id="07f6c-172">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="07f6c-173">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="07f6c-173">FullGlobe</span></span>

<span data-ttu-id="07f6c-174">SQL Server kullanırken tam dünya temsil etmek için bir standart geometri türü var. `geography` sütun türü.</span><span class="sxs-lookup"><span data-stu-id="07f6c-174">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="07f6c-175">Ayrıca, tam dünya (olmadan bir halkasının) göre çokgenleri temsil etmek için bir yol vardır.</span><span class="sxs-lookup"><span data-stu-id="07f6c-175">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="07f6c-176">Hiçbirini NTS tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-176">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="07f6c-177">FullGlobe ve temel alan çokgenler NTS tarafından desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="07f6c-177">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="07f6c-178">SQLite</span><span class="sxs-lookup"><span data-stu-id="07f6c-178">SQLite</span></span>

<span data-ttu-id="07f6c-179">SQLite kullananlar için bazı ek bilgiler aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-179">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="07f6c-180">SpatiaLite yükleme</span><span class="sxs-lookup"><span data-stu-id="07f6c-180">Installing SpatiaLite</span></span>

<span data-ttu-id="07f6c-181">Windows üzerinde yerel mod_spatialite kitaplığı NuGet paket bağımlılık olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="07f6c-181">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="07f6c-182">Diğer platformlar ayrı olarak yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-182">Other platforms need to install it separately.</span></span> <span data-ttu-id="07f6c-183">Bu genellikle yapılır bir yazılım paketi Yöneticisi'ni kullanarak.</span><span class="sxs-lookup"><span data-stu-id="07f6c-183">This is typically done using a software package manager.</span></span> <span data-ttu-id="07f6c-184">Örneğin, Ubuntu ve MacOS üzerinde Homebrew APT kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="07f6c-184">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="07f6c-185">SRID yapılandırma</span><span class="sxs-lookup"><span data-stu-id="07f6c-185">Configuring SRID</span></span>

<span data-ttu-id="07f6c-186">SpatiaLite içinde sütunlar sütun başına bir SRID belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-186">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="07f6c-187">SRID olan varsayılan `0`.</span><span class="sxs-lookup"><span data-stu-id="07f6c-187">The default SRID is `0`.</span></span> <span data-ttu-id="07f6c-188">ForSqliteHasSrid yöntemi kullanarak farklı bir SRID belirtin.</span><span class="sxs-lookup"><span data-stu-id="07f6c-188">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="07f6c-189">Boyut</span><span class="sxs-lookup"><span data-stu-id="07f6c-189">Dimension</span></span>

<span data-ttu-id="07f6c-190">Benzer şekilde SRID, bir sütunun boyut (veya ordinates) da sütunu bir parçası olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-190">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="07f6c-191">Varsayılan ordinates olduğu X ve y etkinleştir ek ordinates (Z ve M) kullanarak ForSqliteHasDimension yöntemi.</span><span class="sxs-lookup"><span data-stu-id="07f6c-191">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="07f6c-192">Çevrilmiş işlemleri</span><span class="sxs-lookup"><span data-stu-id="07f6c-192">Translated Operations</span></span>

<span data-ttu-id="07f6c-193">Bu tablo, hangi NTS üyeleri SQL'e her EF Core sağlayıcısı tarafından çevrilir gösterir.</span><span class="sxs-lookup"><span data-stu-id="07f6c-193">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="07f6c-194">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="07f6c-194">NetTopologySuite</span></span> | <span data-ttu-id="07f6c-195">SQL Server (geometri)</span><span class="sxs-lookup"><span data-stu-id="07f6c-195">SQL Server (geometry)</span></span> | <span data-ttu-id="07f6c-196">SQL Server (regioncountryname)</span><span class="sxs-lookup"><span data-stu-id="07f6c-196">SQL Server (geography)</span></span> | <span data-ttu-id="07f6c-197">SQLite</span><span class="sxs-lookup"><span data-stu-id="07f6c-197">SQLite</span></span> | <span data-ttu-id="07f6c-198">Npgsql</span><span class="sxs-lookup"><span data-stu-id="07f6c-198">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="07f6c-199">Geometry.Area</span><span class="sxs-lookup"><span data-stu-id="07f6c-199">Geometry.Area</span></span> | <span data-ttu-id="07f6c-200">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-200">✔</span></span> | <span data-ttu-id="07f6c-201">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-201">✔</span></span> | <span data-ttu-id="07f6c-202">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-202">✔</span></span> | <span data-ttu-id="07f6c-203">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-203">✔</span></span>
<span data-ttu-id="07f6c-204">Geometry.AsBinary()</span><span class="sxs-lookup"><span data-stu-id="07f6c-204">Geometry.AsBinary()</span></span> | <span data-ttu-id="07f6c-205">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-205">✔</span></span> | <span data-ttu-id="07f6c-206">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-206">✔</span></span> | <span data-ttu-id="07f6c-207">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-207">✔</span></span> | <span data-ttu-id="07f6c-208">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-208">✔</span></span>
<span data-ttu-id="07f6c-209">Geometry.AsText()</span><span class="sxs-lookup"><span data-stu-id="07f6c-209">Geometry.AsText()</span></span> | <span data-ttu-id="07f6c-210">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-210">✔</span></span> | <span data-ttu-id="07f6c-211">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-211">✔</span></span> | <span data-ttu-id="07f6c-212">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-212">✔</span></span> | <span data-ttu-id="07f6c-213">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-213">✔</span></span>
<span data-ttu-id="07f6c-214">Geometry.Boundary</span><span class="sxs-lookup"><span data-stu-id="07f6c-214">Geometry.Boundary</span></span> | <span data-ttu-id="07f6c-215">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-215">✔</span></span> | | <span data-ttu-id="07f6c-216">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-216">✔</span></span> | <span data-ttu-id="07f6c-217">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-217">✔</span></span>
<span data-ttu-id="07f6c-218">Geometry.Buffer(double)</span><span class="sxs-lookup"><span data-stu-id="07f6c-218">Geometry.Buffer(double)</span></span> | <span data-ttu-id="07f6c-219">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-219">✔</span></span> | <span data-ttu-id="07f6c-220">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-220">✔</span></span> | <span data-ttu-id="07f6c-221">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-221">✔</span></span> | <span data-ttu-id="07f6c-222">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-222">✔</span></span>
<span data-ttu-id="07f6c-223">Geometry.Buffer (double, int)</span><span class="sxs-lookup"><span data-stu-id="07f6c-223">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="07f6c-224">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-224">✔</span></span>
<span data-ttu-id="07f6c-225">Geometry.Centroid</span><span class="sxs-lookup"><span data-stu-id="07f6c-225">Geometry.Centroid</span></span> | <span data-ttu-id="07f6c-226">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-226">✔</span></span> | | <span data-ttu-id="07f6c-227">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-227">✔</span></span> | <span data-ttu-id="07f6c-228">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-228">✔</span></span>
<span data-ttu-id="07f6c-229">Geometry.Contains(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="07f6c-230">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-230">✔</span></span> | <span data-ttu-id="07f6c-231">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-231">✔</span></span> | <span data-ttu-id="07f6c-232">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-232">✔</span></span> | <span data-ttu-id="07f6c-233">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-233">✔</span></span>
<span data-ttu-id="07f6c-234">Geometry.ConvexHull()</span><span class="sxs-lookup"><span data-stu-id="07f6c-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="07f6c-235">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-235">✔</span></span> | <span data-ttu-id="07f6c-236">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-236">✔</span></span> | <span data-ttu-id="07f6c-237">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-237">✔</span></span> | <span data-ttu-id="07f6c-238">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-238">✔</span></span>
<span data-ttu-id="07f6c-239">Geometry.CoveredBy(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="07f6c-240">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-240">✔</span></span> | <span data-ttu-id="07f6c-241">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-241">✔</span></span>
<span data-ttu-id="07f6c-242">Geometry.Covers(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="07f6c-243">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-243">✔</span></span> | <span data-ttu-id="07f6c-244">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-244">✔</span></span>
<span data-ttu-id="07f6c-245">Geometry.Crosses(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="07f6c-246">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-246">✔</span></span> | | <span data-ttu-id="07f6c-247">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-247">✔</span></span> | <span data-ttu-id="07f6c-248">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-248">✔</span></span>
<span data-ttu-id="07f6c-249">Geometry.Difference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="07f6c-250">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-250">✔</span></span> | <span data-ttu-id="07f6c-251">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-251">✔</span></span> | <span data-ttu-id="07f6c-252">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-252">✔</span></span> | <span data-ttu-id="07f6c-253">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-253">✔</span></span>
<span data-ttu-id="07f6c-254">Geometry.Dimension</span><span class="sxs-lookup"><span data-stu-id="07f6c-254">Geometry.Dimension</span></span> | <span data-ttu-id="07f6c-255">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-255">✔</span></span> | <span data-ttu-id="07f6c-256">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-256">✔</span></span> | <span data-ttu-id="07f6c-257">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-257">✔</span></span> | <span data-ttu-id="07f6c-258">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-258">✔</span></span>
<span data-ttu-id="07f6c-259">Geometry.Disjoint(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="07f6c-260">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-260">✔</span></span> | <span data-ttu-id="07f6c-261">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-261">✔</span></span> | <span data-ttu-id="07f6c-262">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-262">✔</span></span> | <span data-ttu-id="07f6c-263">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-263">✔</span></span>
<span data-ttu-id="07f6c-264">Geometry.Distance(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="07f6c-265">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-265">✔</span></span> | <span data-ttu-id="07f6c-266">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-266">✔</span></span> | <span data-ttu-id="07f6c-267">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-267">✔</span></span> | <span data-ttu-id="07f6c-268">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-268">✔</span></span>
<span data-ttu-id="07f6c-269">Geometry.Envelope</span><span class="sxs-lookup"><span data-stu-id="07f6c-269">Geometry.Envelope</span></span> | <span data-ttu-id="07f6c-270">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-270">✔</span></span> | | <span data-ttu-id="07f6c-271">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-271">✔</span></span> | <span data-ttu-id="07f6c-272">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-272">✔</span></span>
<span data-ttu-id="07f6c-273">Geometry.EqualsExact(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="07f6c-274">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-274">✔</span></span>
<span data-ttu-id="07f6c-275">Geometry.EqualsTopologically(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="07f6c-276">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-276">✔</span></span> | <span data-ttu-id="07f6c-277">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-277">✔</span></span> | <span data-ttu-id="07f6c-278">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-278">✔</span></span> | <span data-ttu-id="07f6c-279">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-279">✔</span></span>
<span data-ttu-id="07f6c-280">Geometry.GeometryType</span><span class="sxs-lookup"><span data-stu-id="07f6c-280">Geometry.GeometryType</span></span> | <span data-ttu-id="07f6c-281">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-281">✔</span></span> | <span data-ttu-id="07f6c-282">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-282">✔</span></span> | <span data-ttu-id="07f6c-283">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-283">✔</span></span> | <span data-ttu-id="07f6c-284">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-284">✔</span></span>
<span data-ttu-id="07f6c-285">Geometry.GetGeometryN(int)</span><span class="sxs-lookup"><span data-stu-id="07f6c-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="07f6c-286">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-286">✔</span></span> | | <span data-ttu-id="07f6c-287">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-287">✔</span></span> | <span data-ttu-id="07f6c-288">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-288">✔</span></span>
<span data-ttu-id="07f6c-289">Geometry.InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="07f6c-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="07f6c-290">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-290">✔</span></span> | | <span data-ttu-id="07f6c-291">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-291">✔</span></span>
<span data-ttu-id="07f6c-292">Geometry.Intersection(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-292">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="07f6c-293">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-293">✔</span></span> | <span data-ttu-id="07f6c-294">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-294">✔</span></span> | <span data-ttu-id="07f6c-295">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-295">✔</span></span> | <span data-ttu-id="07f6c-296">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-296">✔</span></span>
<span data-ttu-id="07f6c-297">Geometry.Intersects(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-297">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="07f6c-298">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-298">✔</span></span> | <span data-ttu-id="07f6c-299">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-299">✔</span></span> | <span data-ttu-id="07f6c-300">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-300">✔</span></span> | <span data-ttu-id="07f6c-301">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-301">✔</span></span>
<span data-ttu-id="07f6c-302">Geometry.IsEmpty</span><span class="sxs-lookup"><span data-stu-id="07f6c-302">Geometry.IsEmpty</span></span> | <span data-ttu-id="07f6c-303">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-303">✔</span></span> | <span data-ttu-id="07f6c-304">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-304">✔</span></span> | <span data-ttu-id="07f6c-305">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-305">✔</span></span> | <span data-ttu-id="07f6c-306">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-306">✔</span></span>
<span data-ttu-id="07f6c-307">Geometry.IsSimple</span><span class="sxs-lookup"><span data-stu-id="07f6c-307">Geometry.IsSimple</span></span> | <span data-ttu-id="07f6c-308">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-308">✔</span></span> | | <span data-ttu-id="07f6c-309">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-309">✔</span></span> | <span data-ttu-id="07f6c-310">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-310">✔</span></span>
<span data-ttu-id="07f6c-311">Geometry.IsValid</span><span class="sxs-lookup"><span data-stu-id="07f6c-311">Geometry.IsValid</span></span> | <span data-ttu-id="07f6c-312">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-312">✔</span></span> | <span data-ttu-id="07f6c-313">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-313">✔</span></span> | <span data-ttu-id="07f6c-314">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-314">✔</span></span> | <span data-ttu-id="07f6c-315">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-315">✔</span></span>
<span data-ttu-id="07f6c-316">Geometry.IsWithinDistance (geometri, çift)</span><span class="sxs-lookup"><span data-stu-id="07f6c-316">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="07f6c-317">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-317">✔</span></span> | | <span data-ttu-id="07f6c-318">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-318">✔</span></span>
<span data-ttu-id="07f6c-319">Geometry.Length</span><span class="sxs-lookup"><span data-stu-id="07f6c-319">Geometry.Length</span></span> | <span data-ttu-id="07f6c-320">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-320">✔</span></span> | <span data-ttu-id="07f6c-321">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-321">✔</span></span> | <span data-ttu-id="07f6c-322">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-322">✔</span></span> | <span data-ttu-id="07f6c-323">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-323">✔</span></span>
<span data-ttu-id="07f6c-324">Geometry.NumGeometries</span><span class="sxs-lookup"><span data-stu-id="07f6c-324">Geometry.NumGeometries</span></span> | <span data-ttu-id="07f6c-325">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-325">✔</span></span> | <span data-ttu-id="07f6c-326">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-326">✔</span></span> | <span data-ttu-id="07f6c-327">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-327">✔</span></span> | <span data-ttu-id="07f6c-328">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-328">✔</span></span>
<span data-ttu-id="07f6c-329">Geometry.NumPoints</span><span class="sxs-lookup"><span data-stu-id="07f6c-329">Geometry.NumPoints</span></span> | <span data-ttu-id="07f6c-330">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-330">✔</span></span> | <span data-ttu-id="07f6c-331">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-331">✔</span></span> | <span data-ttu-id="07f6c-332">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-332">✔</span></span> | <span data-ttu-id="07f6c-333">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-333">✔</span></span>
<span data-ttu-id="07f6c-334">Geometry.OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="07f6c-334">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="07f6c-335">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-335">✔</span></span> | <span data-ttu-id="07f6c-336">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-336">✔</span></span> | <span data-ttu-id="07f6c-337">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-337">✔</span></span>
<span data-ttu-id="07f6c-338">Geometry.Overlaps(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-338">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="07f6c-339">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-339">✔</span></span> | <span data-ttu-id="07f6c-340">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-340">✔</span></span> | <span data-ttu-id="07f6c-341">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-341">✔</span></span> | <span data-ttu-id="07f6c-342">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-342">✔</span></span>
<span data-ttu-id="07f6c-343">Geometry.PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="07f6c-343">Geometry.PointOnSurface</span></span> | <span data-ttu-id="07f6c-344">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-344">✔</span></span> | | <span data-ttu-id="07f6c-345">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-345">✔</span></span> | <span data-ttu-id="07f6c-346">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-346">✔</span></span>
<span data-ttu-id="07f6c-347">Geometry.Relate (geometri, string)</span><span class="sxs-lookup"><span data-stu-id="07f6c-347">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="07f6c-348">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-348">✔</span></span> | | <span data-ttu-id="07f6c-349">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-349">✔</span></span> | <span data-ttu-id="07f6c-350">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-350">✔</span></span>
<span data-ttu-id="07f6c-351">Geometry.Reverse()</span><span class="sxs-lookup"><span data-stu-id="07f6c-351">Geometry.Reverse()</span></span> | | | <span data-ttu-id="07f6c-352">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-352">✔</span></span> | <span data-ttu-id="07f6c-353">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-353">✔</span></span>
<span data-ttu-id="07f6c-354">Geometry.SRID</span><span class="sxs-lookup"><span data-stu-id="07f6c-354">Geometry.SRID</span></span> | <span data-ttu-id="07f6c-355">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-355">✔</span></span> | <span data-ttu-id="07f6c-356">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-356">✔</span></span> | <span data-ttu-id="07f6c-357">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-357">✔</span></span> | <span data-ttu-id="07f6c-358">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-358">✔</span></span>
<span data-ttu-id="07f6c-359">Geometry.SymmetricDifference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-359">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="07f6c-360">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-360">✔</span></span> | <span data-ttu-id="07f6c-361">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-361">✔</span></span> | <span data-ttu-id="07f6c-362">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-362">✔</span></span> | <span data-ttu-id="07f6c-363">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-363">✔</span></span>
<span data-ttu-id="07f6c-364">Geometry.ToBinary()</span><span class="sxs-lookup"><span data-stu-id="07f6c-364">Geometry.ToBinary()</span></span> | <span data-ttu-id="07f6c-365">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-365">✔</span></span> | <span data-ttu-id="07f6c-366">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-366">✔</span></span> | <span data-ttu-id="07f6c-367">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-367">✔</span></span> | <span data-ttu-id="07f6c-368">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-368">✔</span></span>
<span data-ttu-id="07f6c-369">Geometry.ToText()</span><span class="sxs-lookup"><span data-stu-id="07f6c-369">Geometry.ToText()</span></span> | <span data-ttu-id="07f6c-370">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-370">✔</span></span> | <span data-ttu-id="07f6c-371">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-371">✔</span></span> | <span data-ttu-id="07f6c-372">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-372">✔</span></span> | <span data-ttu-id="07f6c-373">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-373">✔</span></span>
<span data-ttu-id="07f6c-374">Geometry.Touches(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-374">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="07f6c-375">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-375">✔</span></span> | | <span data-ttu-id="07f6c-376">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-376">✔</span></span> | <span data-ttu-id="07f6c-377">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-377">✔</span></span>
<span data-ttu-id="07f6c-378">Geometry.Union()</span><span class="sxs-lookup"><span data-stu-id="07f6c-378">Geometry.Union()</span></span> | | | <span data-ttu-id="07f6c-379">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-379">✔</span></span>
<span data-ttu-id="07f6c-380">Geometry.Union(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-380">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="07f6c-381">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-381">✔</span></span> | <span data-ttu-id="07f6c-382">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-382">✔</span></span> | <span data-ttu-id="07f6c-383">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-383">✔</span></span> | <span data-ttu-id="07f6c-384">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-384">✔</span></span>
<span data-ttu-id="07f6c-385">Geometry.Within(Geometry)</span><span class="sxs-lookup"><span data-stu-id="07f6c-385">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="07f6c-386">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-386">✔</span></span> | <span data-ttu-id="07f6c-387">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-387">✔</span></span> | <span data-ttu-id="07f6c-388">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-388">✔</span></span> | <span data-ttu-id="07f6c-389">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-389">✔</span></span>
<span data-ttu-id="07f6c-390">GeometryCollection.Count</span><span class="sxs-lookup"><span data-stu-id="07f6c-390">GeometryCollection.Count</span></span> | <span data-ttu-id="07f6c-391">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-391">✔</span></span> | <span data-ttu-id="07f6c-392">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-392">✔</span></span> | <span data-ttu-id="07f6c-393">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-393">✔</span></span> | <span data-ttu-id="07f6c-394">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-394">✔</span></span>
<span data-ttu-id="07f6c-395">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="07f6c-395">GeometryCollection[int]</span></span> | <span data-ttu-id="07f6c-396">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-396">✔</span></span> | <span data-ttu-id="07f6c-397">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-397">✔</span></span> | <span data-ttu-id="07f6c-398">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-398">✔</span></span> | <span data-ttu-id="07f6c-399">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-399">✔</span></span>
<span data-ttu-id="07f6c-400">LineString.Count</span><span class="sxs-lookup"><span data-stu-id="07f6c-400">LineString.Count</span></span> | <span data-ttu-id="07f6c-401">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-401">✔</span></span> | <span data-ttu-id="07f6c-402">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-402">✔</span></span> | <span data-ttu-id="07f6c-403">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-403">✔</span></span> | <span data-ttu-id="07f6c-404">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-404">✔</span></span>
<span data-ttu-id="07f6c-405">LineString.EndPoint</span><span class="sxs-lookup"><span data-stu-id="07f6c-405">LineString.EndPoint</span></span> | <span data-ttu-id="07f6c-406">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-406">✔</span></span> | <span data-ttu-id="07f6c-407">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-407">✔</span></span> | <span data-ttu-id="07f6c-408">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-408">✔</span></span> | <span data-ttu-id="07f6c-409">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-409">✔</span></span>
<span data-ttu-id="07f6c-410">LineString.GetPointN(int)</span><span class="sxs-lookup"><span data-stu-id="07f6c-410">LineString.GetPointN(int)</span></span> | <span data-ttu-id="07f6c-411">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-411">✔</span></span> | <span data-ttu-id="07f6c-412">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-412">✔</span></span> | <span data-ttu-id="07f6c-413">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-413">✔</span></span> | <span data-ttu-id="07f6c-414">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-414">✔</span></span>
<span data-ttu-id="07f6c-415">LineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="07f6c-415">LineString.IsClosed</span></span> | <span data-ttu-id="07f6c-416">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-416">✔</span></span> | <span data-ttu-id="07f6c-417">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-417">✔</span></span> | <span data-ttu-id="07f6c-418">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-418">✔</span></span> | <span data-ttu-id="07f6c-419">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-419">✔</span></span>
<span data-ttu-id="07f6c-420">LineString.IsRing</span><span class="sxs-lookup"><span data-stu-id="07f6c-420">LineString.IsRing</span></span> | <span data-ttu-id="07f6c-421">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-421">✔</span></span> | | <span data-ttu-id="07f6c-422">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-422">✔</span></span> | <span data-ttu-id="07f6c-423">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-423">✔</span></span>
<span data-ttu-id="07f6c-424">LineString.StartPoint</span><span class="sxs-lookup"><span data-stu-id="07f6c-424">LineString.StartPoint</span></span> | <span data-ttu-id="07f6c-425">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-425">✔</span></span> | <span data-ttu-id="07f6c-426">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-426">✔</span></span> | <span data-ttu-id="07f6c-427">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-427">✔</span></span> | <span data-ttu-id="07f6c-428">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-428">✔</span></span>
<span data-ttu-id="07f6c-429">MultiLineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="07f6c-429">MultiLineString.IsClosed</span></span> | <span data-ttu-id="07f6c-430">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-430">✔</span></span> | <span data-ttu-id="07f6c-431">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-431">✔</span></span> | <span data-ttu-id="07f6c-432">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-432">✔</span></span> | <span data-ttu-id="07f6c-433">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-433">✔</span></span>
<span data-ttu-id="07f6c-434">Point.M</span><span class="sxs-lookup"><span data-stu-id="07f6c-434">Point.M</span></span> | <span data-ttu-id="07f6c-435">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-435">✔</span></span> | <span data-ttu-id="07f6c-436">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-436">✔</span></span> | <span data-ttu-id="07f6c-437">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-437">✔</span></span> | <span data-ttu-id="07f6c-438">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-438">✔</span></span>
<span data-ttu-id="07f6c-439">Point.X</span><span class="sxs-lookup"><span data-stu-id="07f6c-439">Point.X</span></span> | <span data-ttu-id="07f6c-440">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-440">✔</span></span> | <span data-ttu-id="07f6c-441">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-441">✔</span></span> | <span data-ttu-id="07f6c-442">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-442">✔</span></span> | <span data-ttu-id="07f6c-443">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-443">✔</span></span>
<span data-ttu-id="07f6c-444">Point.Y</span><span class="sxs-lookup"><span data-stu-id="07f6c-444">Point.Y</span></span> | <span data-ttu-id="07f6c-445">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-445">✔</span></span> | <span data-ttu-id="07f6c-446">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-446">✔</span></span> | <span data-ttu-id="07f6c-447">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-447">✔</span></span> | <span data-ttu-id="07f6c-448">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-448">✔</span></span>
<span data-ttu-id="07f6c-449">Point.Z</span><span class="sxs-lookup"><span data-stu-id="07f6c-449">Point.Z</span></span> | <span data-ttu-id="07f6c-450">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-450">✔</span></span> | <span data-ttu-id="07f6c-451">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-451">✔</span></span> | <span data-ttu-id="07f6c-452">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-452">✔</span></span> | <span data-ttu-id="07f6c-453">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-453">✔</span></span>
<span data-ttu-id="07f6c-454">Polygon.ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="07f6c-454">Polygon.ExteriorRing</span></span> | <span data-ttu-id="07f6c-455">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-455">✔</span></span> | <span data-ttu-id="07f6c-456">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-456">✔</span></span> | <span data-ttu-id="07f6c-457">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-457">✔</span></span> | <span data-ttu-id="07f6c-458">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-458">✔</span></span>
<span data-ttu-id="07f6c-459">Polygon.GetInteriorRingN(int)</span><span class="sxs-lookup"><span data-stu-id="07f6c-459">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="07f6c-460">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-460">✔</span></span> | <span data-ttu-id="07f6c-461">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-461">✔</span></span> | <span data-ttu-id="07f6c-462">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-462">✔</span></span> | <span data-ttu-id="07f6c-463">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-463">✔</span></span>
<span data-ttu-id="07f6c-464">Polygon.NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="07f6c-464">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="07f6c-465">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-465">✔</span></span> | <span data-ttu-id="07f6c-466">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-466">✔</span></span> | <span data-ttu-id="07f6c-467">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-467">✔</span></span> | <span data-ttu-id="07f6c-468">✔</span><span class="sxs-lookup"><span data-stu-id="07f6c-468">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="07f6c-469">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="07f6c-469">Additional resources</span></span>

* [<span data-ttu-id="07f6c-470">SQL Server uzamsal verileri</span><span class="sxs-lookup"><span data-stu-id="07f6c-470">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="07f6c-471">SpatiaLite giriş sayfası</span><span class="sxs-lookup"><span data-stu-id="07f6c-471">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="07f6c-472">PostGIS belgeleri</span><span class="sxs-lookup"><span data-stu-id="07f6c-472">PostGIS Documentation</span></span>](http://postgis.net/documentation/)
