---
title: Ham SQL sorguları - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 0ad9731840c5f72064f2f66932b9867a0144f437
ms.sourcegitcommit: 2da6f9b05e1ce3a46491e5cc68f17758bdeb6b02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "53006875"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="18a26-102">Ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="18a26-102">Raw SQL Queries</span></span>

<span data-ttu-id="18a26-103">Entity Framework Core, ilişkisel bir veritabanı ile çalışırken, ham SQL sorguları açılan olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="18a26-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="18a26-104">Gerçekleştirmek istediğiniz sorguyu LINQ kullanılarak ifade edilemediğinde ya da verimsiz SQL sorgularında LINQ sorgusu kullanarak kaynaklanan bu yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="18a26-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL queries.</span></span> <span data-ttu-id="18a26-105">Ham SQL sorguları varlık türleri veya EF Core 2.1 ile başlayan döndürebilir [sorgu türü](xref:core/modeling/query-types) modelinizin bir parçası.</span><span class="sxs-lookup"><span data-stu-id="18a26-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="18a26-106">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="18a26-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="18a26-107">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="18a26-107">Limitations</span></span>

<span data-ttu-id="18a26-108">Ham SQL sorguları kullanırken dikkat edilmesi gereken bazı sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="18a26-108">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="18a26-109">SQL sorgusu, veri varlığı veya sorguyu türü tüm özelliklerde için döndürmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="18a26-109">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="18a26-110">Sonuç kümesi sütun adları, özellikler için eşlenen sütun adları eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="18a26-110">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="18a26-111">Bu özellik/sütun eşlemesi için ham SQL sorguları burada yoksayıldı EF6 farklı olduğuna dikkat edin ve sonuç kümesi sütun adları, özellik adlarının eşleşmesi gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="18a26-111">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="18a26-112">SQL sorgusu, ilgili verileri içeremez.</span><span class="sxs-lookup"><span data-stu-id="18a26-112">The SQL query cannot contain related data.</span></span> <span data-ttu-id="18a26-113">Ancak, çoğu durumda, üzerinde sorgu kullanarak oluşturabileceğiniz `Include` ilgili verileri döndürmek için işleci (bkz [ilgili veriler dahil olmak üzere](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="18a26-113">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="18a26-114">`SELECT` Bu yönteme geçirilen deyimler genellikle birleştirilebilir olmalıdır: varsa EF Core gereken ek sorgu işleçleri sunucusunda değerlendirilecek (örneğin, LINQ işleçleri çevirmek için uygulanan sonra `FromSql`), sağlanan SQL alt sorgu kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="18a26-114">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="18a26-115">Bu, geçirilen SQL herhangi bir karakter veya gibi geçerli bir alt sorgu olmayan seçenekleri içermemelidir anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="18a26-115">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="18a26-116">sondaki noktalı virgül</span><span class="sxs-lookup"><span data-stu-id="18a26-116">a trailing semicolon</span></span>
  * <span data-ttu-id="18a26-117">SQL Server'da izleyen bir sorgu düzeyi İpucu (örneğin, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="18a26-117">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="18a26-118">SQL Server'da bir `ORDER BY` , eşlik yan tümcesi `TOP 100 PERCENT` içinde `SELECT` yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="18a26-118">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="18a26-119">SQL deyimleri dışında `SELECT` otomatik birleştirilebilir olmayan tanınır.</span><span class="sxs-lookup"><span data-stu-id="18a26-119">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="18a26-120">Sonuç olarak, saklı yordamları tam sonuçları her zaman istemciye döndürülen ve sonra herhangi bir LINQ işlecini uygulandı `FromSql` değerlendirilen bellek içi olduğu.</span><span class="sxs-lookup"><span data-stu-id="18a26-120">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="18a26-121">Temel ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="18a26-121">Basic raw SQL queries</span></span>

<span data-ttu-id="18a26-122">Kullanabileceğiniz *SQL* ham SQL sorgu temelli bir LINQ Sorgu başlamak için genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="18a26-122">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="18a26-123">Ham SQL sorguları bir saklı yordamı yürütmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="18a26-123">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="18a26-124">Parametreleri geçirme</span><span class="sxs-lookup"><span data-stu-id="18a26-124">Passing parameters</span></span>

<span data-ttu-id="18a26-125">SQL kabul eden bir API ile gibi SQL ekleme saldırısına karşı korumak için tüm kullanıcı parametre haline getirmek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="18a26-125">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="18a26-126">SQL sorgu dizesi parametresi yer tutucular içerir ve sonra ek bağımsız değişkenler olarak parametre değerlerini sağlayın.</span><span class="sxs-lookup"><span data-stu-id="18a26-126">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="18a26-127">Sağladığınız parametre değerlerini otomatik olarak dönüştürülür bir `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="18a26-127">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="18a26-128">Aşağıdaki örnek, bir saklı yordam için tek bir parametre geçirir.</span><span class="sxs-lookup"><span data-stu-id="18a26-128">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="18a26-129">Bu görünebilir ancak ister `String.Format` söz dizimi, sağlanan değer kaydırılır bir parametre ve oluşturulan parametre adı eklenen nerede `{0}` yer tutucu belirtildi.</span><span class="sxs-lookup"><span data-stu-id="18a26-129">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="18a26-130">Aynıdır, ancak EF Core 2.0 ve üzeri sürümlerde desteklenir. dize ilişkilendirme sözdizimini kullanarak sorgu:</span><span class="sxs-lookup"><span data-stu-id="18a26-130">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="18a26-131">Ayrıca, bir DbParameter oluşturun ve parametre değeri olarak sağlayın.</span><span class="sxs-lookup"><span data-stu-id="18a26-131">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="18a26-132">Bu sayede SQL sorgu dizesinde adlandırılmış parametreler kullanılacak</span><span class="sxs-lookup"><span data-stu-id="18a26-132">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="18a26-133">LINQ ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="18a26-133">Composing with LINQ</span></span>

<span data-ttu-id="18a26-134">Ardından SQL sorgusuna üzerinde veritabanında kullanılıp kullanılamayacağı, LINQ işleçleri kullanarak ilk ham SQL sorgusunun üstüne oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="18a26-134">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="18a26-135">Üzerinde oluşan SQL sorguları ile başlayan `SELECT` anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="18a26-135">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="18a26-136">Aşağıdaki örnek filtreleme ve sıralama gerçekleştirmek için LINQ kullanarak bir Table-Valued işlev (TVF) öğesinden seçer ve ardından ölçeklemesini ham bir SQL sorgusu kullanır.</span><span class="sxs-lookup"><span data-stu-id="18a26-136">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="18a26-137">İlgili verileri de dahil olmak üzere</span><span class="sxs-lookup"><span data-stu-id="18a26-137">Including related data</span></span>

<span data-ttu-id="18a26-138">LINQ işleçleri ile oluşturma sorguda ilgili verileri dahil etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="18a26-138">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="18a26-139">**Her zaman için ham SQL sorguları Parametreleştirme kullanın:** ham SQL kabul API'leri gibi dize `FromSql` ve `ExecuteSqlCommand` değerleri kolayca parametre olarak geçirilmesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="18a26-139">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="18a26-140">Kullanıcı girişini doğrulama ek olarak, her zaman Parametreleştirme ham bir SQL sorgu/komutta kullanılan herhangi bir değeri için kullanın.</span><span class="sxs-lookup"><span data-stu-id="18a26-140">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="18a26-141">Dize birleştirme SQL ekleme saldırılarına karşı korumak için herhangi bir giriş doğrulamak için sorumlu olursunuz herhangi bir bölümünü sorgu dizesini dinamik olarak oluşturmak için kullanıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="18a26-141">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
