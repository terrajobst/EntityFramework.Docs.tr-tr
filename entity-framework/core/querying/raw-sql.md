---
title: Ham SQL sorguları-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 7a0df6fb656be58103971f45b9e12e9f1383311f
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921713"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="48e75-102">Ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="48e75-102">Raw SQL Queries</span></span>

<span data-ttu-id="48e75-103">Entity Framework Core, ilişkisel bir veritabanıyla çalışırken ham SQL sorgularına karşı aşağı açılan bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="48e75-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="48e75-104">Bu, gerçekleştirmek istediğiniz sorgu LINQ kullanılarak ifade edilenemediğini veya bir LINQ sorgusunun kullanılması verimsiz SQL sorgularının oluşmasına neden olduğunda yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="48e75-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL queries.</span></span> <span data-ttu-id="48e75-105">Ham SQL sorguları, EF Core 2,1 ' den başlayarak, modelinizin bir parçası olan [sorgu türleri](xref:core/modeling/query-types) döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="48e75-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="48e75-106">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="48e75-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="48e75-107">Temel ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="48e75-107">Basic raw SQL queries</span></span>

<span data-ttu-id="48e75-108">Bir ham SQL sorgusuna dayalı bir LINQ sorgusuna başlamak için *fromsql* Extension yöntemini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="48e75-108">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="48e75-109">Ham SQL sorguları, saklı bir yordamı yürütmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="48e75-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="48e75-110">Parametreleri geçirme</span><span class="sxs-lookup"><span data-stu-id="48e75-110">Passing parameters</span></span>

<span data-ttu-id="48e75-111">SQL 'i kabul eden herhangi bir API 'de olduğu gibi, bir SQL ekleme saldırısına karşı korunmak için herhangi bir kullanıcı girişini parametreleştirmek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="48e75-111">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="48e75-112">SQL sorgu dizesinde parametre yer tutucuları dahil edebilir ve ardından parametre değerlerini ek bağımsız değişkenler olarak sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="48e75-112">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="48e75-113">Sağladığınız herhangi bir parametre değeri otomatik olarak bir `DbParameter`olarak dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="48e75-113">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="48e75-114">Aşağıdaki örnek, saklı yordama tek bir parametre geçirir.</span><span class="sxs-lookup"><span data-stu-id="48e75-114">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="48e75-115">Bu, söz dizimi gibi `String.Format` görünebilir, ancak sağlanan değer bir parametreye sarmalanır ve `{0}` yer tutucunun belirtildiği yerde oluşturulan parametre adı eklenir.</span><span class="sxs-lookup"><span data-stu-id="48e75-115">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="48e75-116">Bu sorgu, ancak EF Core 2,0 ve üzeri sürümlerde desteklenen dize ilişkilendirme sözdizimi kullanılarak aynıdır:</span><span class="sxs-lookup"><span data-stu-id="48e75-116">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="48e75-117">Ayrıca bir DbParameter oluşturup bunu bir parametre değeri olarak sağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="48e75-117">You can also construct a DbParameter and supply it as a parameter value:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="48e75-118">Bu, bir saklı yordam isteğe bağlı parametrelere sahip olduğunda yararlı olan SQL sorgu dizesinde adlandırılmış parametreleri kullanmanıza olanak sağlar:</span><span class="sxs-lookup"><span data-stu-id="48e75-118">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="48e75-119">LINQ ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="48e75-119">Composing with LINQ</span></span>

<span data-ttu-id="48e75-120">SQL sorgusu veritabanında yer alıyorsa, LINQ işleçlerini kullanarak ilk ham SQL sorgusunun üzerine oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="48e75-120">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="48e75-121">Üzerinde birleştirilebilen SQL sorguları `SELECT` anahtar sözcüğüyle başlar.</span><span class="sxs-lookup"><span data-stu-id="48e75-121">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="48e75-122">Aşağıdaki örnek, tablo değerli bir Işlevden (TVF) seçen ham bir SQL sorgusu kullanır ve ardından filtreleme ve sıralamayı gerçekleştirmek için LINQ kullanarak buna dayanır.</span><span class="sxs-lookup"><span data-stu-id="48e75-122">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a><span data-ttu-id="48e75-123">Değişiklik İzleme</span><span class="sxs-lookup"><span data-stu-id="48e75-123">Change Tracking</span></span>

<span data-ttu-id="48e75-124">Kullanan sorgular, `FromSql()` EF Core ' deki diğer LINQ sorgularıyla tam olarak aynı değişiklik izleme kurallarını izler.</span><span class="sxs-lookup"><span data-stu-id="48e75-124">Queries that use the `FromSql()` follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="48e75-125">Örneğin, sorgu projeleri varlık türlerdir, sonuçlar varsayılan olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="48e75-125">For example, if the query projects entity types, the results will be tracked by default.</span></span>  

<span data-ttu-id="48e75-126">Aşağıdaki örnek, bir tablo değerli Işlevden (TVF) seçim yapan ham bir SQL sorgusu kullanır ve sonra yapılan çağrısıyla değişiklik izlemeyi devre dışı bırakır. AsNoTracking ():</span><span class="sxs-lookup"><span data-stu-id="48e75-126">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to .AsNoTracking():</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="48e75-127">İlgili verileri dahil etme</span><span class="sxs-lookup"><span data-stu-id="48e75-127">Including related data</span></span>

<span data-ttu-id="48e75-128">`Include()` Yöntemi, diğer herhangi bir LINQ sorgusuyla olduğu gibi ilgili verileri dahil etmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="48e75-128">The `Include()` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a><span data-ttu-id="48e75-129">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="48e75-129">Limitations</span></span>

<span data-ttu-id="48e75-130">Ham SQL sorguları kullanırken dikkat etmeniz için bazı sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="48e75-130">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="48e75-131">SQL sorgusu, varlığın veya sorgu türünün tüm özellikleri için veri döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="48e75-131">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="48e75-132">Sonuç kümesindeki sütun adları, özelliklerin eşlendiği sütun adlarıyla eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="48e75-132">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="48e75-133">Bu, ham SQL sorguları için özellik/sütun eşlemesinin yoksayıldığı ve sonuç kümesi sütun adlarının Özellik adlarıyla eşleşmesi gerekiyordu EF6 öğesinden farklıdır.</span><span class="sxs-lookup"><span data-stu-id="48e75-133">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="48e75-134">SQL sorgusu ilgili verileri içeremez.</span><span class="sxs-lookup"><span data-stu-id="48e75-134">The SQL query cannot contain related data.</span></span> <span data-ttu-id="48e75-135">Bununla birlikte, çoğu durumda ilgili verileri ( [ilgili verileri dahil olmak](#including-related-data)üzere) döndürmek `Include` için işlecini kullanarak sorgunun üzerine oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="48e75-135">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="48e75-136">`SELECT`Bu yönteme geçirilen deyimler genellikle birleştirilebilir olmalıdır: EF Core sunucuda ek sorgu işleçlerini değerlendirmesi gerekiyorsa (örneğin, daha sonra `FromSql`uygulanan LINQ işleçlerini çevirmek için), sağlanan SQL bir alt sorgu olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="48e75-136">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="48e75-137">Bu, geçirilen SQL 'in bir alt sorgu üzerinde geçerli olmayan herhangi bir karakter veya seçenek içermemesi gerektiği anlamına gelir; örneğin:</span><span class="sxs-lookup"><span data-stu-id="48e75-137">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="48e75-138">sondaki noktalı virgül</span><span class="sxs-lookup"><span data-stu-id="48e75-138">a trailing semicolon</span></span>
  * <span data-ttu-id="48e75-139">SQL Server, sondaki sorgu düzeyi İpucu (örneğin, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="48e75-139">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="48e75-140">SQL Server, `ORDER BY` `SELECT` yan tümcelerinde `OFFSET 0` or `TOP 100 PERCENT` içinde olmayan bir yan tümce</span><span class="sxs-lookup"><span data-stu-id="48e75-140">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="48e75-141">Dışındaki `SELECT` SQL deyimleri otomatik olarak birleştirilemeyen olarak tanınır.</span><span class="sxs-lookup"><span data-stu-id="48e75-141">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="48e75-142">Sonuç olarak, saklı yordamların tam sonuçları istemciye her zaman döndürülür ve tüm LINQ işleçleri bellek içinde değerlendirildikten sonra `FromSql` uygulanır.</span><span class="sxs-lookup"><span data-stu-id="48e75-142">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

> [!WARNING]  
> <span data-ttu-id="48e75-143">**Ham SQL sorguları için her zaman Parametreleştirme kullanın:** Kullanıcı girişini doğrulamaya ek olarak, ham bir SQL sorgusunda/komutunda kullanılan değerler için her zaman Parametreleştirme kullanın.</span><span class="sxs-lookup"><span data-stu-id="48e75-143">**Always use parameterization for raw SQL queries:** In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="48e75-144">`FromSql` Ve gibi ham bir SQL dizesini kabul eden API 'ler `ExecuteSqlCommand` ve değerlerin kolayca parametre olarak geçirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="48e75-144">APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="48e75-145">FormattableString `ExecuteSqlCommand` kabul eden ve ' nin `FromSql` aşırı yüklemeleri, SQL ekleme saldırılarına karşı korumaya yardımcı olacak şekilde dize ilişkilendirme sözdiziminin kullanılmasına da imkan tanır.</span><span class="sxs-lookup"><span data-stu-id="48e75-145">Overloads of `FromSql` and `ExecuteSqlCommand` that accept FormattableString also allow using string interpolation syntax in a way that helps protect against SQL injection attacks.</span></span> 
> 
> <span data-ttu-id="48e75-146">Sorgu dizesinin herhangi bir bölümünü dinamik olarak derlemek için dize birleştirme veya ilişkilendirme kullanıyorsanız veya bu girişleri dinamik SQL olarak yürütemeyen deyimlere veya saklı yordamlara geçirmeniz durumunda, herhangi bir girişin doğrulanması sizin sorumluluğunuzdadır SQL ekleme saldırılarına karşı koruma.</span><span class="sxs-lookup"><span data-stu-id="48e75-146">If you are using string concatenation or interpolation to dynamically build any part of the query string, or passing user input to statements or stored procedures that can execute those inputs as dynamic SQL, then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
