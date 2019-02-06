---
title: Ham SQL sorguları - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 343162596780e6146b57f73a38221701009cd855
ms.sourcegitcommit: 85d17524d8e022f933cde7fc848313f57dfd3eb8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55760515"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="2c2dc-102">Ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="2c2dc-102">Raw SQL Queries</span></span>

<span data-ttu-id="2c2dc-103">Entity Framework Core, ilişkisel bir veritabanı ile çalışırken, ham SQL sorguları açılan olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="2c2dc-104">Gerçekleştirmek istediğiniz sorguyu LINQ kullanılarak ifade edilemediğinde ya da verimsiz SQL sorgularında LINQ sorgusu kullanarak kaynaklanan bu yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL queries.</span></span> <span data-ttu-id="2c2dc-105">Ham SQL sorguları varlık türleri veya EF Core 2.1 ile başlayan döndürebilir [sorgu türü](xref:core/modeling/query-types) modelinizin bir parçası.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="2c2dc-106">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="2c2dc-107">Temel ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="2c2dc-107">Basic raw SQL queries</span></span>

<span data-ttu-id="2c2dc-108">Kullanabileceğiniz *SQL* ham SQL sorgu temelli bir LINQ Sorgu başlamak için genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-108">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="2c2dc-109">Ham SQL sorguları bir saklı yordamı yürütmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="2c2dc-110">Parametreleri geçirme</span><span class="sxs-lookup"><span data-stu-id="2c2dc-110">Passing parameters</span></span>

<span data-ttu-id="2c2dc-111">SQL kabul eden bir API ile gibi SQL ekleme saldırısına karşı korumak için tüm kullanıcı parametre haline getirmek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-111">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="2c2dc-112">SQL sorgu dizesi parametresi yer tutucular içerir ve sonra ek bağımsız değişkenler olarak parametre değerlerini sağlayın.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-112">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="2c2dc-113">Sağladığınız parametre değerlerini otomatik olarak dönüştürülür bir `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-113">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="2c2dc-114">Aşağıdaki örnek, bir saklı yordam için tek bir parametre geçirir.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-114">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="2c2dc-115">Bu görünebilir ancak ister `String.Format` söz dizimi, sağlanan değer kaydırılır bir parametre ve oluşturulan parametre adı eklenen nerede `{0}` yer tutucu belirtildi.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-115">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="2c2dc-116">Aynıdır, ancak EF Core 2.0 ve üzeri sürümlerde desteklenir. dize ilişkilendirme sözdizimini kullanarak sorgu:</span><span class="sxs-lookup"><span data-stu-id="2c2dc-116">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="2c2dc-117">Ayrıca, bir DbParameter oluşturun ve parametre değeri olarak sağlayın.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-117">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="2c2dc-118">Bu, SQL sorgu dizesinde adlandırılmış parametreler kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-118">This allows you to use named parameters in the SQL query string.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="2c2dc-119">LINQ ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="2c2dc-119">Composing with LINQ</span></span>

<span data-ttu-id="2c2dc-120">Ardından SQL sorgusuna üzerinde veritabanında kullanılıp kullanılamayacağı, LINQ işleçleri kullanarak ilk ham SQL sorgusunun üstüne oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-120">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="2c2dc-121">Üzerinde oluşan SQL sorguları ile başlayan `SELECT` anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-121">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="2c2dc-122">Aşağıdaki örnek filtreleme ve sıralama gerçekleştirmek için LINQ kullanarak bir Table-Valued işlev (TVF) öğesinden seçer ve ardından ölçeklemesini ham bir SQL sorgusu kullanır.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-122">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a><span data-ttu-id="2c2dc-123">Değişiklik İzleme</span><span class="sxs-lookup"><span data-stu-id="2c2dc-123">Change Tracking</span></span>

<span data-ttu-id="2c2dc-124">Sorgular kullanan `FromSql()` EF Core, diğer bir LINQ sorgusu olarak kuralları izleme tam aynı değişikliği izleyin.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-124">Queries that use the `FromSql()` follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="2c2dc-125">Örneğin, sorgu varlık türleri projeleri sonuçları varsayılan olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-125">For example, if the query projects entity types, the results will be tracked by default.</span></span>  

<span data-ttu-id="2c2dc-126">Aşağıdaki örnek, bir Table-Valued işlev (TVF) öğesinden seçer ham bir SQL sorgusu kullanır ve ardından değişiklik izleme çağrısıyla devre dışı bırakır. AsNoTracking():</span><span class="sxs-lookup"><span data-stu-id="2c2dc-126">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with teh call to .AsNoTracking():</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="2c2dc-127">İlgili verileri de dahil olmak üzere</span><span class="sxs-lookup"><span data-stu-id="2c2dc-127">Including related data</span></span>

<span data-ttu-id="2c2dc-128">`Include()` Yöntemi, herhangi bir LINQ Sorgu ile olduğu gibi ilgili verileri dahil etmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2c2dc-128">The `Include()` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a><span data-ttu-id="2c2dc-129">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="2c2dc-129">Limitations</span></span>

<span data-ttu-id="2c2dc-130">Ham SQL sorguları kullanırken dikkat edilmesi gereken bazı sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="2c2dc-130">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="2c2dc-131">SQL sorgusu, veri varlığı veya sorguyu türü tüm özelliklerde için döndürmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-131">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="2c2dc-132">Sonuç kümesi sütun adları, özellikler için eşlenen sütun adları eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-132">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="2c2dc-133">Bu özellik/sütun eşlemesi için ham SQL sorguları burada yoksayıldı EF6 farklı olduğuna dikkat edin ve sonuç kümesi sütun adları, özellik adlarının eşleşmesi gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-133">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="2c2dc-134">SQL sorgusu, ilgili verileri içeremez.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-134">The SQL query cannot contain related data.</span></span> <span data-ttu-id="2c2dc-135">Ancak, çoğu durumda, üzerinde sorgu kullanarak oluşturabileceğiniz `Include` ilgili verileri döndürmek için işleci (bkz [ilgili veriler dahil olmak üzere](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="2c2dc-135">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="2c2dc-136">`SELECT` Bu yönteme geçirilen deyimler genellikle birleştirilebilir olması gerekir: EF Core ek sorgu işleçleri sunucusunda değerlendirilecek gerekip gerekmediğini (örneğin, LINQ işleçleri çevirmek için uygulanan sonra `FromSql`), sağlanan SQL alt sorgu kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-136">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="2c2dc-137">Bu, geçirilen SQL herhangi bir karakter veya gibi geçerli bir alt sorgu olmayan seçenekleri içermemelidir anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="2c2dc-137">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="2c2dc-138">sondaki noktalı virgül</span><span class="sxs-lookup"><span data-stu-id="2c2dc-138">a trailing semicolon</span></span>
  * <span data-ttu-id="2c2dc-139">SQL Server'da izleyen bir sorgu düzeyi İpucu (örneğin, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="2c2dc-139">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="2c2dc-140">SQL Server'da bir `ORDER BY` , eşlik yan tümcesi `TOP 100 PERCENT` içinde `SELECT` yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="2c2dc-140">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="2c2dc-141">SQL deyimleri dışında `SELECT` otomatik birleştirilebilir olmayan tanınır.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-141">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="2c2dc-142">Sonuç olarak, saklı yordamları tam sonuçları her zaman istemciye döndürülen ve sonra herhangi bir LINQ işlecini uygulandı `FromSql` değerlendirilen bellek içi olduğu.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-142">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

> [!WARNING]  
> <span data-ttu-id="2c2dc-143">**Her zaman için ham SQL sorguları Parametreleştirme kullanın:** Ham SQL kabul API'leri gibi dize `FromSql` ve `ExecuteSqlCommand` değerleri kolayca parametre olarak geçirilmesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-143">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="2c2dc-144">Kullanıcı girişini doğrulama ek olarak, her zaman Parametreleştirme ham bir SQL sorgu/komutta kullanılan herhangi bir değeri için kullanın.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-144">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="2c2dc-145">Dize birleştirme SQL ekleme saldırılarına karşı korumak için herhangi bir giriş doğrulamak için sorumlu olursunuz herhangi bir bölümünü sorgu dizesini dinamik olarak oluşturmak için kullanıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-145">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
