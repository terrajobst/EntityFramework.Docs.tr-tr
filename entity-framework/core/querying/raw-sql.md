---
title: Ham SQL sorguları-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: d8f52edfdf4bd7776ab8d81185c867cbfd7bcf44
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813592"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="d0ada-102">Ham SQL Sorguları</span><span class="sxs-lookup"><span data-stu-id="d0ada-102">Raw SQL Queries</span></span>

<span data-ttu-id="d0ada-103">Entity Framework Core, ilişkisel bir veritabanıyla çalışırken ham SQL sorgularına karşı aşağı açılan bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="d0ada-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="d0ada-104">Bu, gerçekleştirmek istediğiniz sorgu LINQ kullanılarak ifade edilenemediğini veya bir LINQ sorgusunun kullanılması verimsiz bir SQL sorgusuyla sonuçlıysa yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="d0ada-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="d0ada-105">Ham SQL sorguları, modelinizin bir parçası olan normal varlık türlerini veya [anahtarsız varlık türlerini](xref:core/modeling/keyless-entity-types) döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="d0ada-105">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="d0ada-106">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="d0ada-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="d0ada-107">Temel ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="d0ada-107">Basic raw SQL queries</span></span>

<span data-ttu-id="d0ada-108">Ham SQL sorgusuna dayalı `FromSqlRaw` bir LINQ sorgusuna başlamak için genişletme yöntemini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0ada-108">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="d0ada-109">Ham SQL sorguları, saklı bir yordamı yürütmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d0ada-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="d0ada-110">Parametreleri geçirme</span><span class="sxs-lookup"><span data-stu-id="d0ada-110">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="d0ada-111">**Ham SQL sorguları için her zaman Parametreleştirme kullanın**</span><span class="sxs-lookup"><span data-stu-id="d0ada-111">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="d0ada-112">Bir ham SQL sorgusuna Kullanıcı tarafından sağlanmış herhangi bir değer alındığınızda, SQL ekleme saldırılarından kaçınmak için dikkatli olunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d0ada-112">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="d0ada-113">Bu tür değerlerin geçersiz karakterler içermediğini doğrulamaya ek olarak, her zaman değerleri SQL metinden ayrı gönderen Parametreleştirme kullanın.</span><span class="sxs-lookup"><span data-stu-id="d0ada-113">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="d0ada-114">Özellikle, hiç bir birleştirilmiş veya enterpolasyonlu dize`$""`() ' i doğrulanmamış kullanıcı tarafından sağlanmış değerlerle `FromSqlRaw` veya `ExecuteSqlRaw`içine geçirmeyin.</span><span class="sxs-lookup"><span data-stu-id="d0ada-114">In particular, never pass a concatenated or interpolated string (`$""`) with unvalidated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="d0ada-115">`FromSqlInterpolated` Ve`ExecuteSqlInterpolated` yöntemleri, dize ilişkilendirme sözdiziminin SQL ekleme saldırılarına karşı korunmasını sağlayan bir biçimde kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="d0ada-115">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="d0ada-116">Aşağıdaki örnek, SQL sorgu dizesinde bir parametre yer tutucusu ekleyerek ve ek bir bağımsız değişken sağlayarak, saklı yordama tek bir parametre geçirir.</span><span class="sxs-lookup"><span data-stu-id="d0ada-116">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="d0ada-117">Bu, söz dizimi gibi `String.Format` görünebilir, ancak sağlanan değer bir `DbParameter` ve `{0}` yer tutucunun belirtildiği yerde oluşturulmuş parametre adına sarmalanır.</span><span class="sxs-lookup"><span data-stu-id="d0ada-117">While this may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="d0ada-118">' A alternatif olarak `FromSqlRaw`, dize ilişkilendirmeden `FromSqlInterpolated` güvenli bir şekilde kullanılmasına izin veren ' yi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0ada-118">As an alternative to `FromSqlRaw`, you can use `FromSqlInterpolated` which allows the safe use of string interpolation.</span></span> <span data-ttu-id="d0ada-119">Önceki örnekte olduğu gibi, değer bir `DbParameter` öğesine dönüştürülür ve bu nedenle SQL ekleme ile ilgili değildir:</span><span class="sxs-lookup"><span data-stu-id="d0ada-119">As with the previous example, the value is converted to a `DbParameter` and is therefore not vulnerable to SQL injection:</span></span>

> [!NOTE]
> <span data-ttu-id="d0ada-120">Sürüm 3,0 ' `FromSqlRaw` den önce ve `FromSqlInterpolated` adında `FromSql`iki aşırı yükleme yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="d0ada-120">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="d0ada-121">Daha ayrıntılı bilgi için [önceki sürümler bölümüne](#previous-versions) bakın.</span><span class="sxs-lookup"><span data-stu-id="d0ada-121">See the [previous versions section](#previous-versions) for more details.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="d0ada-122">Ayrıca bir DbParameter oluşturup bunu bir parametre değeri olarak sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0ada-122">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="d0ada-123">Bir dize yer tutucusu `FromSqlRaw` yerine normal bir SQL parametre yer tutucusu kullanıldığından, güvenli bir şekilde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d0ada-123">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="d0ada-124">Bu, bir saklı yordam isteğe bağlı parametrelere sahip olduğunda yararlı olan SQL sorgu dizesinde adlandırılmış parametreleri kullanmanıza olanak sağlar:</span><span class="sxs-lookup"><span data-stu-id="d0ada-124">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="d0ada-125">LINQ ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="d0ada-125">Composing with LINQ</span></span>

<span data-ttu-id="d0ada-126">SQL sorgusu veritabanında yer alıyorsa, LINQ işleçlerini kullanarak ilk ham SQL sorgusunun üzerine oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0ada-126">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="d0ada-127">Üzerinde birleştirilebilen SQL sorguları `SELECT` anahtar sözcüğüyle başlar.</span><span class="sxs-lookup"><span data-stu-id="d0ada-127">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="d0ada-128">Aşağıdaki örnek, tablo değerli bir Işlevden (TVF) seçen ham bir SQL sorgusu kullanır ve ardından filtreleme ve sıralamayı gerçekleştirmek için LINQ kullanarak buna dayanır.</span><span class="sxs-lookup"><span data-stu-id="d0ada-128">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

<span data-ttu-id="d0ada-129">Bu, aşağıdaki SQL sorgusunu üretir:</span><span class="sxs-lookup"><span data-stu-id="d0ada-129">This will produce the following SQL query:</span></span>

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a><span data-ttu-id="d0ada-130">Değişiklik İzleme</span><span class="sxs-lookup"><span data-stu-id="d0ada-130">Change Tracking</span></span>

<span data-ttu-id="d0ada-131">`FromSql` Yöntemleri kullanan sorgular, EF Core ' deki diğer herhangi bir LINQ sorgusuyla tam olarak aynı değişiklik izleme kurallarını izler.</span><span class="sxs-lookup"><span data-stu-id="d0ada-131">Queries that use the `FromSql` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="d0ada-132">Örneğin, sorgu projeleri varlık türlerdir, sonuçlar varsayılan olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="d0ada-132">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="d0ada-133">Aşağıdaki örnek, bir tablo değerli Işlevden (TVF) seçen ham bir SQL sorgusu kullanır ve sonra yapılan `AsNoTracking`çağrısıyla değişiklik izlemeyi devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="d0ada-133">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="d0ada-134">İlgili verileri dahil etme</span><span class="sxs-lookup"><span data-stu-id="d0ada-134">Including related data</span></span>

<span data-ttu-id="d0ada-135">`Include` Yöntemi, diğer herhangi bir LINQ sorgusuyla olduğu gibi ilgili verileri dahil etmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d0ada-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

<span data-ttu-id="d0ada-136">Bunun için ham SQL sorgusunun birleştirilebilir olması gerektiğini unutmayın; Bu, özellikle saklı yordam çağrılarında çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="d0ada-136">Note that this requires your raw SQL query to be composable; it will notably not work with stored procedure calls.</span></span> <span data-ttu-id="d0ada-137">[Sınırlamalar](#limitations)bölümünde yer alan notlara bakın.</span><span class="sxs-lookup"><span data-stu-id="d0ada-137">See notes on composability under [Limitations](#limitations)).</span></span>

## <a name="limitations"></a><span data-ttu-id="d0ada-138">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="d0ada-138">Limitations</span></span>

<span data-ttu-id="d0ada-139">Ham SQL sorguları kullanırken dikkat etmeniz için bazı sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="d0ada-139">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="d0ada-140">SQL sorgusu, varlık türünün tüm özellikleri için veri döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="d0ada-140">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="d0ada-141">Sonuç kümesindeki sütun adları, özelliklerin eşlendiği sütun adlarıyla eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="d0ada-141">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="d0ada-142">Bu, ham SQL sorguları için özellik/sütun eşlemesinin yoksayıldığı ve sonuç kümesi sütun adlarının Özellik adlarıyla eşleşmesi gerekiyordu EF6 öğesinden farklıdır.</span><span class="sxs-lookup"><span data-stu-id="d0ada-142">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="d0ada-143">SQL sorgusu ilgili verileri içeremez.</span><span class="sxs-lookup"><span data-stu-id="d0ada-143">The SQL query cannot contain related data.</span></span> <span data-ttu-id="d0ada-144">Bununla birlikte, çoğu durumda ilgili verileri ( [ilgili verileri dahil olmak](#including-related-data)üzere) döndürmek `Include` için işlecini kullanarak sorgunun üzerine oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0ada-144">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="d0ada-145">`SELECT`Bu yönteme geçirilen deyimler genellikle birleştirilebilir olmalıdır: EF Core sunucuda ek sorgu işleçlerini değerlendirmesi gerekiyorsa (örneğin, yöntemlerden sonra `FromSql` uygulanan LINQ işleçlerini çevirmek için), sağlanan SQL bir alt sorgu olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="d0ada-145">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql` methods), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="d0ada-146">Bu, geçirilen SQL 'in bir alt sorgu üzerinde geçerli olmayan herhangi bir karakter veya seçenek içermemesi gerektiği anlamına gelir; örneğin:</span><span class="sxs-lookup"><span data-stu-id="d0ada-146">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="d0ada-147">sondaki noktalı virgül</span><span class="sxs-lookup"><span data-stu-id="d0ada-147">A trailing semicolon</span></span>
  * <span data-ttu-id="d0ada-148">SQL Server, sondaki sorgu düzeyi İpucu (örneğin, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="d0ada-148">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="d0ada-149">SQL Server, `ORDER BY` `SELECT` yan tümcelerinde `OFFSET 0` or `TOP 100 PERCENT` içinde olmayan bir yan tümce</span><span class="sxs-lookup"><span data-stu-id="d0ada-149">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="d0ada-150">SQL Server, saklı yordam çağrılarının üzerinde oluşturmaya izin vermediğini unutmayın. bu nedenle, bu tür bir çağrıya ek sorgu işleçleri uygulama girişimleri geçersiz SQL sonucu verir.</span><span class="sxs-lookup"><span data-stu-id="d0ada-150">Note that SQL Server does not allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="d0ada-151">Sorgu işleçleri, istemci değerlendirmesi sonrasında `AsEnumerable()` tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="d0ada-151">Query operators may be introduced after `AsEnumerable()` for client evaluation.</span></span>

## <a name="previous-versions"></a><span data-ttu-id="d0ada-152">Önceki sürümler</span><span class="sxs-lookup"><span data-stu-id="d0ada-152">Previous versions</span></span>

<span data-ttu-id="d0ada-153">EF Core sürüm 2,2 ve önceki sürümlerde, daha yeni `FromSql` `FromSqlRaw` ve ile `FromSqlInterpolated`aynı şekilde davranmış iki aşırı yükleme vardı.</span><span class="sxs-lookup"><span data-stu-id="d0ada-153">EF Core version 2.2 and earlier had two overloads named `FromSql` which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="d0ada-154">Bu, amaç enterpolasyonlu dize yöntemini çağırdığınızda ham dize yöntemini yanlışlıkla çağırmak çok kolay hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="d0ada-154">This made it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="d0ada-155">Bu, sorguların olması gerektiğinde parametreli hale getirmemelidir.</span><span class="sxs-lookup"><span data-stu-id="d0ada-155">This could result in queries not being parameterized when they should have been.</span></span>
