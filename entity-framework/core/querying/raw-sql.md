---
title: Ham SQL sorguları-EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417672"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="edccf-102">Ham SQL Sorguları</span><span class="sxs-lookup"><span data-stu-id="edccf-102">Raw SQL Queries</span></span>

<span data-ttu-id="edccf-103">Entity Framework Core, ilişkisel bir veritabanıyla çalışırken ham SQL sorgularına karşı aşağı açılan bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="edccf-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="edccf-104">Ham SQL sorguları, istediğiniz sorgu LINQ kullanılarak ifade edilebilmesi yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="edccf-104">Raw SQL queries are useful if the query you want can't be expressed using LINQ.</span></span> <span data-ttu-id="edccf-105">Ham SQL sorguları, bir LINQ sorgusunun kullanılması verimsiz bir SQL sorgusuna neden olursa da kullanılır.</span><span class="sxs-lookup"><span data-stu-id="edccf-105">Raw SQL queries are also used if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="edccf-106">Ham SQL sorguları, modelinizin bir parçası olan normal varlık türlerini veya [anahtarsız varlık türlerini](xref:core/modeling/keyless-entity-types) döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="edccf-106">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="edccf-107">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) GitHub ' da görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="edccf-107">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="edccf-108">Temel ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="edccf-108">Basic raw SQL queries</span></span>

<span data-ttu-id="edccf-109">Ham SQL sorgusuna dayalı bir LINQ sorgusuna başlamak için `FromSqlRaw` uzantısı yöntemini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="edccf-109">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span> <span data-ttu-id="edccf-110">`FromSqlRaw` yalnızca doğrudan `DbSet<>`olan sorgu köklerine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="edccf-110">`FromSqlRaw` can only be used on query roots, that is directly on the `DbSet<>`.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

<span data-ttu-id="edccf-111">Ham SQL sorguları, saklı bir yordamı yürütmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="edccf-111">Raw SQL queries can be used to execute a stored procedure.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a><span data-ttu-id="edccf-112">Parametreleri geçirme</span><span class="sxs-lookup"><span data-stu-id="edccf-112">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="edccf-113">**Ham SQL sorguları için her zaman Parametreleştirme kullanın**</span><span class="sxs-lookup"><span data-stu-id="edccf-113">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="edccf-114">Bir ham SQL sorgusuna Kullanıcı tarafından sağlanmış herhangi bir değer alındığınızda, SQL ekleme saldırılarından kaçınmak için dikkatli olunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="edccf-114">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="edccf-115">Bu tür değerlerin geçersiz karakterler içermediğini doğrulamaya ek olarak, her zaman değerleri SQL metinden ayrı gönderen Parametreleştirme kullanın.</span><span class="sxs-lookup"><span data-stu-id="edccf-115">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="edccf-116">Özellikle, hiç bir birleştirilmiş veya enterpolasyonsuz dize (`$""`) `FromSqlRaw` veya `ExecuteSqlRaw`olarak doğrulanmamış kullanıcı tarafından sağlanmış değerlerle geçirmez.</span><span class="sxs-lookup"><span data-stu-id="edccf-116">In particular, never pass a concatenated or interpolated string (`$""`) with non-validated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="edccf-117">`FromSqlInterpolated` ve `ExecuteSqlInterpolated` yöntemleri, SQL ekleme saldırılarına karşı koruma sağlayacak şekilde dize ilişkilendirme sözdiziminin kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="edccf-117">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="edccf-118">Aşağıdaki örnek, SQL sorgu dizesinde bir parametre yer tutucusu ekleyerek ve ek bir bağımsız değişken sağlayarak, saklı yordama tek bir parametre geçirir.</span><span class="sxs-lookup"><span data-stu-id="edccf-118">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="edccf-119">Bu söz dizimi `String.Format` sözdizimine benzeyebilir, ancak verilen değer bir `DbParameter` sarmalanır ve `{0}` yer tutucusunun belirtilmiş olduğu oluşturulan parametre adı eklenir.</span><span class="sxs-lookup"><span data-stu-id="edccf-119">While this syntax may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

<span data-ttu-id="edccf-120">`FromSqlInterpolated` `FromSqlRaw` benzerdir ancak dize ilişkilendirme söz dizimini kullanmanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="edccf-120">`FromSqlInterpolated` is similar to `FromSqlRaw` but allows you to use string interpolation syntax.</span></span> <span data-ttu-id="edccf-121">`FromSqlRaw`tıpkı tıpkı yalnızca sorgu köklerinin üzerinde `FromSqlInterpolated` kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="edccf-121">Just like `FromSqlRaw`, `FromSqlInterpolated` can only be used on query roots.</span></span> <span data-ttu-id="edccf-122">Önceki örnekte olduğu gibi, değer bir `DbParameter` dönüştürülür ve SQL ekleme ile savunmasız değildir.</span><span class="sxs-lookup"><span data-stu-id="edccf-122">As with the previous example, the value is converted to a `DbParameter` and isn't vulnerable to SQL injection.</span></span>

> [!NOTE]
> <span data-ttu-id="edccf-123">Sürüm 3,0 ' den önce, `FromSqlRaw` ve `FromSqlInterpolated` `FromSql`adlı iki aşırı yüklemedir.</span><span class="sxs-lookup"><span data-stu-id="edccf-123">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="edccf-124">Daha fazla bilgi için [önceki sürümler bölümüne](#previous-versions)bakın.</span><span class="sxs-lookup"><span data-stu-id="edccf-124">For more information, see the [previous versions section](#previous-versions).</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

<span data-ttu-id="edccf-125">Ayrıca bir DbParameter oluşturup bunu bir parametre değeri olarak sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="edccf-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="edccf-126">Bir dize yer tutucusu yerine düzenli bir SQL parametre yer tutucusu kullanıldığından `FromSqlRaw` güvenle kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="edccf-126">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

<span data-ttu-id="edccf-127">`FromSqlRaw`, SQL sorgu dizesinde adlandırılmış parametreleri kullanmanıza olanak sağlar. Bu, saklı yordam isteğe bağlı parametrelere sahip olduğunda yararlı olur:</span><span class="sxs-lookup"><span data-stu-id="edccf-127">`FromSqlRaw` allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a><span data-ttu-id="edccf-128">LINQ ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="edccf-128">Composing with LINQ</span></span>

<span data-ttu-id="edccf-129">LINQ işleçleri kullanarak ilk ham SQL sorgusunun üzerine oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="edccf-129">You can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="edccf-130">EF Core, bunu alt sorgu olarak değerlendirir ve veritabanında oluşturun.</span><span class="sxs-lookup"><span data-stu-id="edccf-130">EF Core will treat it as subquery and compose over it in the database.</span></span> <span data-ttu-id="edccf-131">Aşağıdaki örnek, bir tablo değerli Işlevden (TVF) seçim yapan ham bir SQL sorgusu kullanır.</span><span class="sxs-lookup"><span data-stu-id="edccf-131">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF).</span></span> <span data-ttu-id="edccf-132">Daha sonra, filtre uygulamak ve sıralamak için LINQ kullanarak buna bir bileşim yapın.</span><span class="sxs-lookup"><span data-stu-id="edccf-132">And then composes on it using LINQ to do filtering and sorting.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

<span data-ttu-id="edccf-133">Yukarıdaki sorgu aşağıdaki SQL 'i oluşturur:</span><span class="sxs-lookup"><span data-stu-id="edccf-133">Above query generates following SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a><span data-ttu-id="edccf-134">İlgili verileri dahil etme</span><span class="sxs-lookup"><span data-stu-id="edccf-134">Including related data</span></span>

<span data-ttu-id="edccf-135">`Include` yöntemi, diğer herhangi bir LINQ sorgusuyla olduğu gibi ilgili verileri dahil etmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="edccf-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

<span data-ttu-id="edccf-136">EF Core, sağlanan SQL 'in bir alt sorgu olarak davranmasından bu yana, LINQ ile oluşturmak için ham SQL sorgusunun birleştirilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="edccf-136">Composing with LINQ requires your raw SQL query to be composable since EF Core will treat the supplied SQL as a subquery.</span></span> <span data-ttu-id="edccf-137">Üzerinde birleştirilebilen SQL sorguları `SELECT` anahtar sözcüğüyle başlar.</span><span class="sxs-lookup"><span data-stu-id="edccf-137">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span> <span data-ttu-id="edccf-138">Ayrıca, geçirilen SQL, bir alt sorgu üzerinde geçerli olmayan herhangi bir karakter veya seçenek içermemelidir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="edccf-138">Further, SQL passed shouldn't contain any characters or options that aren't valid on a subquery, such as:</span></span>

- <span data-ttu-id="edccf-139">Sondaki noktalı virgül</span><span class="sxs-lookup"><span data-stu-id="edccf-139">A trailing semicolon</span></span>
- <span data-ttu-id="edccf-140">SQL Server, sondaki sorgu düzeyi İpucu (örneğin, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="edccf-140">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
- <span data-ttu-id="edccf-141">SQL Server, `SELECT` yan tümcesinde `OFFSET 0` veya `TOP 100 PERCENT` ile kullanılmayan bir `ORDER BY` yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="edccf-141">On SQL Server, an `ORDER BY` clause that isn't used with `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

<span data-ttu-id="edccf-142">SQL Server, saklı yordam çağrılarının üzerinde oluşturmaya izin vermez, bu nedenle, bu tür bir çağrıya ek sorgu işleçleri uygulama girişimleri geçersiz SQL 'e neden olur.</span><span class="sxs-lookup"><span data-stu-id="edccf-142">SQL Server doesn't allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="edccf-143">EF Core bir saklı yordam üzerinden oluşturmaya çalıştığından emin olmak için `FromSqlRaw` veya `FromSqlInterpolated` yöntemlerinden hemen sonra `AsEnumerable` veya `AsAsyncEnumerable` yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="edccf-143">Use `AsEnumerable` or `AsAsyncEnumerable` method right after `FromSqlRaw` or `FromSqlInterpolated` methods to make sure that EF Core doesn't try to compose over a stored procedure.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="edccf-144">Değişiklik İzleme</span><span class="sxs-lookup"><span data-stu-id="edccf-144">Change Tracking</span></span>

<span data-ttu-id="edccf-145">`FromSqlRaw` veya `FromSqlInterpolated` yöntemlerini kullanan sorgular, EF Core ' deki diğer LINQ sorgularıyla tam olarak aynı değişiklik izleme kurallarını izler.</span><span class="sxs-lookup"><span data-stu-id="edccf-145">Queries that use the `FromSqlRaw` or `FromSqlInterpolated` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="edccf-146">Örneğin, sorgu projeleri varlık türlerdir, sonuçlar varsayılan olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="edccf-146">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="edccf-147">Aşağıdaki örnek, bir tablo değerli Işlevden (TVF) seçim yapan ham bir SQL sorgusu kullanır ve `AsNoTracking`çağrısıyla değişiklik izlemeyi devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="edccf-147">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a><span data-ttu-id="edccf-148">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="edccf-148">Limitations</span></span>

<span data-ttu-id="edccf-149">Ham SQL sorguları kullanırken dikkat etmeniz için bazı sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="edccf-149">There are a few limitations to be aware of when using raw SQL queries:</span></span>

- <span data-ttu-id="edccf-150">SQL sorgusu, varlık türünün tüm özellikleri için veri döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="edccf-150">The SQL query must return data for all properties of the entity type.</span></span>
- <span data-ttu-id="edccf-151">Sonuç kümesindeki sütun adları, özelliklerin eşlendiği sütun adlarıyla eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="edccf-151">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="edccf-152">Bu davranışın EF6 öğesinden farklı olduğunu aklınızda edin.</span><span class="sxs-lookup"><span data-stu-id="edccf-152">Note this behavior is different from EF6.</span></span> <span data-ttu-id="edccf-153">EF6, ham SQL sorguları için sütun eşlemesine yoksayılan özelliği ve sonuç kümesi sütun adlarının Özellik adlarıyla eşleşmesi gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="edccf-153">EF6 ignored property to column mapping for raw SQL queries and result set column names had to match the property names.</span></span>
- <span data-ttu-id="edccf-154">SQL sorgusu ilgili verileri içeremez.</span><span class="sxs-lookup"><span data-stu-id="edccf-154">The SQL query can't contain related data.</span></span> <span data-ttu-id="edccf-155">Ancak çoğu durumda, ilgili verileri ( [ilgili verileri dahil olmak](#including-related-data)üzere) döndürmek için `Include` işlecini kullanarak sorgunun üzerine oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="edccf-155">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="previous-versions"></a><span data-ttu-id="edccf-156">Önceki sürümler</span><span class="sxs-lookup"><span data-stu-id="edccf-156">Previous versions</span></span>

<span data-ttu-id="edccf-157">EF Core sürüm 2,2 ve önceki sürümleri, daha yeni `FromSqlRaw` ve `FromSqlInterpolated`aynı şekilde davranmış `FromSql`adlı yöntemin iki aşırı yüküne sahipti.</span><span class="sxs-lookup"><span data-stu-id="edccf-157">EF Core version 2.2 and earlier had two overloads of method named `FromSql`, which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="edccf-158">Amaç, enterpolasyonlu dize yöntemini çağırdığınızda ve diğer bir şekilde, ham dize metodunu yanlışlıkla çağırmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="edccf-158">It was easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="edccf-159">Yanlışlıkla yanlış aşırı yükleme çağırmak, sorguların olması gerektiği zaman parametreli hale getirmemelidir.</span><span class="sxs-lookup"><span data-stu-id="edccf-159">Calling wrong overload accidentally could result in queries not being parameterized when they should have been.</span></span>
