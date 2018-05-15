---
title: Ham SQL sorguları - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: 29b7e20e875bf791a88a92636c1df4bc4e31656b
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="raw-sql-queries"></a><span data-ttu-id="b82f1-102">Ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="b82f1-102">Raw SQL Queries</span></span>

<span data-ttu-id="b82f1-103">Entity Framework Çekirdek, ilişkisel veritabanı ile çalışırken, ham SQL sorguları açılan olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="b82f1-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="b82f1-104">Gerçekleştirmek istediğiniz sorgu LINQ kullanılarak ifade edilemeyen veya bir LINQ Sorgu kullanarak verimsiz SQL veritabanına gönderilen kaynaklanan bu yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="b82f1-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="b82f1-105">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) github'da.</span><span class="sxs-lookup"><span data-stu-id="b82f1-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="b82f1-106">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="b82f1-106">Limitations</span></span>

<span data-ttu-id="b82f1-107">Ham SQL sorguları kullanırken dikkat edilmesi gereken bazı sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="b82f1-107">There are a few limitations to be aware of when using raw SQL queries:</span></span>
* <span data-ttu-id="b82f1-108">SQL sorguları yalnızca dönüş modelinizi parçası olan varlık türleri için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b82f1-108">SQL queries can only be used to return entity types that are part of your model.</span></span> <span data-ttu-id="b82f1-109">Bizim biriktirme listesi üzerinde bir geliştirme yoktur [ham SQL sorgularından geçici türleri döndüren etkinleştir](https://github.com/aspnet/EntityFramework/issues/1862).</span><span class="sxs-lookup"><span data-stu-id="b82f1-109">There is an enhancement on our backlog to [enable returning ad-hoc types from raw SQL queries](https://github.com/aspnet/EntityFramework/issues/1862).</span></span>

* <span data-ttu-id="b82f1-110">SQL sorgusu, varlık veya sorgu türü tüm özelliklerde için veri döndürmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="b82f1-110">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="b82f1-111">Sonuç kümesi içindeki sütun adlarının özellikleri eşlendiği sütun adlarının eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="b82f1-111">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="b82f1-112">Bu özelliği/sütun eşlemesi için ham SQL sorguları yeri yoksayıldı EF6 farklı olduğuna dikkat edin ve sonuç kümesi sütunu adları özellik adlarının eşleşmesi gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="b82f1-112">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="b82f1-113">SQL sorgusu, ilgili veri içeremez.</span><span class="sxs-lookup"><span data-stu-id="b82f1-113">The SQL query cannot contain related data.</span></span> <span data-ttu-id="b82f1-114">Bununla birlikte, çoğu durumda, sorgu kullanarak üstünde oluşturabilirsiniz `Include` ilgili verileri döndürmek için işleci (bkz [ilgili verileri de dahil olmak üzere](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="b82f1-114">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="b82f1-115">`SELECT` Bu yönteme geçirilen deyimleri genellikle birleştirilebilir olmalıdır: varsa EF çekirdek gereken sunucu üzerindeki Ek sorgu işleçleri değerlendirmek (örneğin sonra uygulanan LINQ işleçleri çevirmek için `FromSql`), sağlanan SQL alt sorgu kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="b82f1-115">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (e.g. to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="b82f1-116">Başka bir deyişle, geçirilen SQL herhangi bir karakter veya gibi bir alt sorgu geçerli olmayan seçenekler içermemelidir:</span><span class="sxs-lookup"><span data-stu-id="b82f1-116">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="b82f1-117">sondaki noktalı virgül</span><span class="sxs-lookup"><span data-stu-id="b82f1-117">a trailing semicolon</span></span>
  * <span data-ttu-id="b82f1-118">SQL Server'da sorgu düzeyi sondaki ipucu, örn. `OPTION (HASH JOIN)`</span><span class="sxs-lookup"><span data-stu-id="b82f1-118">On SQL Server, a trailing query-level hint, e.g. `OPTION (HASH JOIN)`</span></span>
  * <span data-ttu-id="b82f1-119">SQL Server'da bir `ORDER BY` , eşlik yan tümcesi `TOP 100 PERCENT` içinde `SELECT` yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="b82f1-119">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="b82f1-120">SQL deyimleri dışında `SELECT` otomatik birleştirilebilir olmayan tanınır.</span><span class="sxs-lookup"><span data-stu-id="b82f1-120">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="b82f1-121">Sonuç olarak, tam sonuçları saklı yordamlar, her zaman istemciye döndürülen ve sonra LINQ işleçleri uygulanan `FromSql` değerlendirilen bellek içi şunlardır.</span><span class="sxs-lookup"><span data-stu-id="b82f1-121">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span> 

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="b82f1-122">Temel ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="b82f1-122">Basic raw SQL queries</span></span>

<span data-ttu-id="b82f1-123">Kullanabileceğiniz *FromSql* ham SQL sorgu temelli bir LINQ Sorgu başlamak için genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b82f1-123">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="b82f1-124">Ham SQL sorguları bir saklı yordamı yürütmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b82f1-124">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="b82f1-125">Parametreleri geçirme</span><span class="sxs-lookup"><span data-stu-id="b82f1-125">Passing parameters</span></span>

<span data-ttu-id="b82f1-126">SQL kabul eden herhangi bir API'yi gibi ile giriş SQL ekleme saldırısına karşı korumak için herhangi bir kullanıcı Parametreleştirme önemlidir.</span><span class="sxs-lookup"><span data-stu-id="b82f1-126">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="b82f1-127">SQL sorgu dizesinde parametre yer tutucuları içerir ve ek bağımsız değişkenler olarak parametre değerlerini sağlayın.</span><span class="sxs-lookup"><span data-stu-id="b82f1-127">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="b82f1-128">Sağladığınız herhangi bir parametre değeri için otomatik olarak dönüştürülecek bir `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="b82f1-128">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="b82f1-129">Aşağıdaki örnek, tek bir parametre saklı yordama geçirir.</span><span class="sxs-lookup"><span data-stu-id="b82f1-129">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="b82f1-130">Bu görünebilir ancak ister `String.Format` sözdizimi, sağlanan değer kaydırılır içindeki bir parametre ve oluşturulan parametre adı eklenen nereye `{0}` yer tutucu belirtildi.</span><span class="sxs-lookup"><span data-stu-id="b82f1-130">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="b82f1-131">Bu aynı olduğundan sorgu ancak EF çekirdek 2.0 ve üzeri desteklenir dize ilişkilendirme sözdizimini kullanarak:</span><span class="sxs-lookup"><span data-stu-id="b82f1-131">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="b82f1-132">Ayrıca, bir DbParameter oluşturmak ve parametre değeri olarak sağlayın.</span><span class="sxs-lookup"><span data-stu-id="b82f1-132">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="b82f1-133">Bu adlandırılmış parametreleri SQL sorgu dizesinde kullanmanıza olanak sağlar</span><span class="sxs-lookup"><span data-stu-id="b82f1-133">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="b82f1-134">LINQ ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="b82f1-134">Composing with LINQ</span></span>

<span data-ttu-id="b82f1-135">Ardından SQL sorgusuna üzerinde veritabanında birleştirilebilen, LINQ işleçleri kullanarak ilk ham SQL sorgusu üstünde oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b82f1-135">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="b82f1-136">İle olan birleştirilebilen SQL sorguları `SELECT` anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="b82f1-136">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="b82f1-137">Aşağıdaki örnek, bir tablo değerli fonksiyon (TVF) gelen seçer ve ardından oluşturur ham bir SQL sorgusu filtreleme ve sıralama gerçekleştirmek için LINQ kullanarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="b82f1-137">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="b82f1-138">İlgili verileri de dahil olmak üzere</span><span class="sxs-lookup"><span data-stu-id="b82f1-138">Including related data</span></span>

<span data-ttu-id="b82f1-139">LINQ işleçlerle oluşturma sorguda ilgili verileri dahil etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b82f1-139">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="b82f1-140">**Her zaman parametrelemeyi ham SQL sorgular için kullanın:** ham SQL kabul API'leri dize gibi `FromSql` ve `ExecuteSqlCommand` parametre olarak kolayca geçirilecek değerlere izin.</span><span class="sxs-lookup"><span data-stu-id="b82f1-140">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="b82f1-141">Kullanıcı girişini doğrulama yanı sıra, her zaman parametrelemeyi ham bir SQL sorgusu/komutta kullanılan herhangi bir değeri için kullanın.</span><span class="sxs-lookup"><span data-stu-id="b82f1-141">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="b82f1-142">Dize birleştirme SQL yerleştirme saldırılarına karşı korumak için herhangi bir giriş doğrulamak için sorumlu sonra herhangi bir kısmını sorgu dizesini dinamik olarak oluşturmak için kullanıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="b82f1-142">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
