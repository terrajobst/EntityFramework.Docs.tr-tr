---
title: Ham SQL Sorguları - EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417672"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="dfcc1-102">Ham SQL Sorguları</span><span class="sxs-lookup"><span data-stu-id="dfcc1-102">Raw SQL Queries</span></span>

<span data-ttu-id="dfcc1-103">Entity Framework Core, ilişkisel bir veritabanıyla çalışırken ham SQL sorgularına düşmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="dfcc1-104">İstediğiniz sorgu LINQ kullanılarak ifade edilenemiyorsa Ham SQL sorguları yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-104">Raw SQL queries are useful if the query you want can't be expressed using LINQ.</span></span> <span data-ttu-id="dfcc1-105">Bir LINQ sorgusu kullanarak verimsiz bir SQL sorgusu neden olduğunda Ham SQL sorguları da kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-105">Raw SQL queries are also used if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="dfcc1-106">Raw SQL sorguları, normal varlık türlerini veya modelinizin bir parçası olan [anahtarsız varlık türlerini](xref:core/modeling/keyless-entity-types) döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-106">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="dfcc1-107">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) GitHub'da görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-107">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="dfcc1-108">Temel ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="dfcc1-108">Basic raw SQL queries</span></span>

<span data-ttu-id="dfcc1-109">Ham bir `FromSqlRaw` SQL sorgusunu temel alan bir LINQ sorgusu başlatmak için uzantı yöntemini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-109">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span> <span data-ttu-id="dfcc1-110">`FromSqlRaw`yalnızca doğrudan sorgu köklerinde `DbSet<>`kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-110">`FromSqlRaw` can only be used on query roots, that is directly on the `DbSet<>`.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

<span data-ttu-id="dfcc1-111">Raw SQL sorguları depolanan yordamı yürütmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-111">Raw SQL queries can be used to execute a stored procedure.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a><span data-ttu-id="dfcc1-112">Parametreleri geçirme</span><span class="sxs-lookup"><span data-stu-id="dfcc1-112">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="dfcc1-113">**Her zaman ham SQL sorguları için parametreleştirme kullanın**</span><span class="sxs-lookup"><span data-stu-id="dfcc1-113">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="dfcc1-114">Kullanıcı tarafından sağlanan değerleri ham bir SQL sorgusuna sokarken, SQL enjeksiyon saldırılarından kaçınmak için dikkatli olunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-114">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="dfcc1-115">Bu tür değerlerin geçersiz karakterler içermediğini doğrulamanın yanı sıra, her zaman SQL metninden ayrı değerleri gönderen parametrelendirmekullanın.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-115">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="dfcc1-116">Özellikle, kullanıcı tarafından sağlanan doğrulanmış olmayan değerlere`$""`sahip, hiçbir zaman bir sıkıştırılmış `FromSqlRaw` `ExecuteSqlRaw`veya enterpolasyonlu dizeyi () hiçbir zaman geçemez.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-116">In particular, never pass a concatenated or interpolated string (`$""`) with non-validated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="dfcc1-117">Ve `FromSqlInterpolated` `ExecuteSqlInterpolated` yöntemler, string enterpolasyon sözdizimini SQL enjeksiyon saldırılarına karşı koruyacak şekilde kullanmaolanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-117">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="dfcc1-118">Aşağıdaki örnek, SQL sorgu dizesi bir parametre yer tutucusu ekleyerek ve ek bir bağımsız değişken sağlayarak depolanan yordamı tek bir parametre geçer.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-118">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="dfcc1-119">Bu sözdizimi sözdizimi gibi `String.Format` görünse de, sağlanan `DbParameter` değer a'ya sarılır ve `{0}` yer tutucunun belirtildiği yere oluşturulan parametre adı eklenir.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-119">While this syntax may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

<span data-ttu-id="dfcc1-120">`FromSqlInterpolated`benzer `FromSqlRaw` dir, ancak dize enterpolasyon sözdizimini kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-120">`FromSqlInterpolated` is similar to `FromSqlRaw` but allows you to use string interpolation syntax.</span></span> <span data-ttu-id="dfcc1-121">Gibi `FromSqlRaw`, `FromSqlInterpolated` sadece sorgu kökleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-121">Just like `FromSqlRaw`, `FromSqlInterpolated` can only be used on query roots.</span></span> <span data-ttu-id="dfcc1-122">Önceki örnekte olduğu gibi, değer a `DbParameter` dönüştürülür ve SQL enjeksiyonuna karşı savunmasız değildir.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-122">As with the previous example, the value is converted to a `DbParameter` and isn't vulnerable to SQL injection.</span></span>

> [!NOTE]
> <span data-ttu-id="dfcc1-123">Önce sürüm 3.0 `FromSqlRaw` `FromSqlInterpolated` ve iki overloads adlı `FromSql`edildi .</span><span class="sxs-lookup"><span data-stu-id="dfcc1-123">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="dfcc1-124">Daha fazla bilgi için [önceki sürümler bölümüne](#previous-versions)bakın.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-124">For more information, see the [previous versions section](#previous-versions).</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

<span data-ttu-id="dfcc1-125">Ayrıca bir DbParameter oluşturup parametre değeri olarak da sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="dfcc1-126">Dize yer tutucusu yerine normal bir SQL parametre `FromSqlRaw` yer tutucukullanıldığından, güvenli bir şekilde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="dfcc1-126">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

<span data-ttu-id="dfcc1-127">`FromSqlRaw`SQL sorgu dizesinde adlandırılmış parametreleri kullanmanıza olanak tanır, bu da depolanan bir yordamın isteğe bağlı parametreleri olduğunda yararlıdır:</span><span class="sxs-lookup"><span data-stu-id="dfcc1-127">`FromSqlRaw` allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a><span data-ttu-id="dfcc1-128">LINQ ile beste</span><span class="sxs-lookup"><span data-stu-id="dfcc1-128">Composing with LINQ</span></span>

<span data-ttu-id="dfcc1-129">LINQ işleçlerini kullanarak ilk ham SQL sorgusunun üstüne oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-129">You can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="dfcc1-130">EF Core bunu alt sorgu olarak ele alacak ve veritabanında üzerinde oluşturacaktır.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-130">EF Core will treat it as subquery and compose over it in the database.</span></span> <span data-ttu-id="dfcc1-131">Aşağıdaki örnekte, Tablo Değeri Olan İşlev'den (TVF) seçim yapan ham bir SQL sorgusu kullanır.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-131">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF).</span></span> <span data-ttu-id="dfcc1-132">Ve sonra filtreleme ve sıralama yapmak için LINQ kullanarak üzerine oluşturur.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-132">And then composes on it using LINQ to do filtering and sorting.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

<span data-ttu-id="dfcc1-133">Yukarıdaki sorgu aşağıdaki SQL oluşturur:</span><span class="sxs-lookup"><span data-stu-id="dfcc1-133">Above query generates following SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a><span data-ttu-id="dfcc1-134">İlgili verileri dahil etme</span><span class="sxs-lookup"><span data-stu-id="dfcc1-134">Including related data</span></span>

<span data-ttu-id="dfcc1-135">Yöntem, `Include` diğer LINQ sorgusunda olduğu gibi ilgili verileri de eklemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="dfcc1-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

<span data-ttu-id="dfcc1-136">EF Core, sağlanan SQL'i bir alt sorgu olarak ele alacağı için, LINQ ile beste yapmak için ham SQL sorgunuzun birleştirilebilir olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-136">Composing with LINQ requires your raw SQL query to be composable since EF Core will treat the supplied SQL as a subquery.</span></span> <span data-ttu-id="dfcc1-137">Üzerinde oluşturulabilen SQL sorguları `SELECT` anahtar sözcükile başlar.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-137">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span> <span data-ttu-id="dfcc1-138">Ayrıca, SQL geçirilen gibi bir alt sorgu üzerinde geçerli olmayan herhangi bir karakter veya seçenek içermemelidir:</span><span class="sxs-lookup"><span data-stu-id="dfcc1-138">Further, SQL passed shouldn't contain any characters or options that aren't valid on a subquery, such as:</span></span>

- <span data-ttu-id="dfcc1-139">İzleyen bir yarı kolon</span><span class="sxs-lookup"><span data-stu-id="dfcc1-139">A trailing semicolon</span></span>
- <span data-ttu-id="dfcc1-140">SQL Server'da, sorgu düzeyi ipucu (örneğin, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="dfcc1-140">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
- <span data-ttu-id="dfcc1-141">`ORDER BY` SQL Server'da, `OFFSET 0` `TOP 100 PERCENT` `SELECT` yan tümcede OR ile kullanılmayan bir yan tümce</span><span class="sxs-lookup"><span data-stu-id="dfcc1-141">On SQL Server, an `ORDER BY` clause that isn't used with `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

<span data-ttu-id="dfcc1-142">SQL Server, depolanan yordam çağrıları üzerinde bir araya gelmekiçin izin vermez, bu nedenle böyle bir çağrıya ek sorgu işleçleri uygulama girişimi geçersiz SQL'e neden olur.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-142">SQL Server doesn't allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="dfcc1-143">EF `AsEnumerable` `AsAsyncEnumerable` Core'un `FromSqlRaw` `FromSqlInterpolated` depolanan bir yordam üzerinde oluşturmaya çalışmadığından emin olmak için hemen sonra veya yöntem kullanın.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-143">Use `AsEnumerable` or `AsAsyncEnumerable` method right after `FromSqlRaw` or `FromSqlInterpolated` methods to make sure that EF Core doesn't try to compose over a stored procedure.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="dfcc1-144">Değişiklik İzleme</span><span class="sxs-lookup"><span data-stu-id="dfcc1-144">Change Tracking</span></span>

<span data-ttu-id="dfcc1-145">`FromSqlRaw` Veya `FromSqlInterpolated` yöntemleri kullanan sorgular, EF Core'daki diğer LINQ sorguları yla aynı değişiklik izleme kurallarını izler.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-145">Queries that use the `FromSqlRaw` or `FromSqlInterpolated` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="dfcc1-146">Örneğin, sorgu varlık türlerini projeleri ise, sonuçlar varsayılan olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-146">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="dfcc1-147">Aşağıdaki örnek, Tablo Değeri Olan İşlev'den (TVF) seçim yapan ham bir SQL sorgusu `AsNoTracking`kullanır, ardından aşağıdaki çağrıyla izlemeyi değiştirir:</span><span class="sxs-lookup"><span data-stu-id="dfcc1-147">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a><span data-ttu-id="dfcc1-148">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="dfcc1-148">Limitations</span></span>

<span data-ttu-id="dfcc1-149">Ham SQL sorgularını kullanırken dikkat edilmesi gereken birkaç sınırlama vardır:</span><span class="sxs-lookup"><span data-stu-id="dfcc1-149">There are a few limitations to be aware of when using raw SQL queries:</span></span>

- <span data-ttu-id="dfcc1-150">SQL sorgusu, varlık türünün tüm özellikleri için veri döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-150">The SQL query must return data for all properties of the entity type.</span></span>
- <span data-ttu-id="dfcc1-151">Sonuç kümesindeki sütun adları, özelliklerin eşlenen sütun adlarıyla eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-151">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="dfcc1-152">Bu davranışın EF6'dan farklı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-152">Note this behavior is different from EF6.</span></span> <span data-ttu-id="dfcc1-153">EF6, ham SQL sorguları için sütun eşleme özelliğini göz ardı etti ve sonuç kümesi sütun adları özellik adlarıyla eşleşmek zorunda kaldı.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-153">EF6 ignored property to column mapping for raw SQL queries and result set column names had to match the property names.</span></span>
- <span data-ttu-id="dfcc1-154">SQL sorgusu ilgili verileri içeremez.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-154">The SQL query can't contain related data.</span></span> <span data-ttu-id="dfcc1-155">Ancak, çoğu durumda ilgili verileri döndürmek için `Include` işleci kullanarak sorgunun üstüne oluşturabilirsiniz (bkz. ilgili verileri dahil [etme).](#including-related-data)</span><span class="sxs-lookup"><span data-stu-id="dfcc1-155">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="previous-versions"></a><span data-ttu-id="dfcc1-156">Önceki sürümler</span><span class="sxs-lookup"><span data-stu-id="dfcc1-156">Previous versions</span></span>

<span data-ttu-id="dfcc1-157">EF Core version 2.2 and earlier had two overloads of method named `FromSql`, which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-157">EF Core version 2.2 and earlier had two overloads of method named `FromSql`, which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="dfcc1-158">Amaç enterpolasyonlu dize yöntemini aramakken yanlışlıkla ham dize yöntemini aramak kolaydı.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-158">It was easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="dfcc1-159">Yanlışlıkla yanlış aşırı yükleme çağrısı, sorguların olması gerekirken parametrelendirilememelerine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="dfcc1-159">Calling wrong overload accidentally could result in queries not being parameterized when they should have been.</span></span>
