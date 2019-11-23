---
title: Karmaşık sorgu Işleçleri-EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 350a7fa6a3ee1de16bad4b63e10842f9356a1b60
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72186262"
---
# <a name="complex-query-operators"></a><span data-ttu-id="1a1bb-102">Karmaşık Sorgu İşleçleri</span><span class="sxs-lookup"><span data-stu-id="1a1bb-102">Complex Query Operators</span></span>

<span data-ttu-id="1a1bb-103">Dil ile tümleşik sorgu (LINQ), birden çok veri kaynağını birleştiren veya karmaşık işleme olan birçok karmaşık işleç içerir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-103">Language Integrated Query (LINQ) contains many complex operators, which combine multiple data sources or does complex processing.</span></span> <span data-ttu-id="1a1bb-104">Tüm LINQ işleçleri sunucu tarafında uygun Çeviriler içermez.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-104">Not all LINQ operators have suitable translations on the server side.</span></span> <span data-ttu-id="1a1bb-105">Bazen, tek bir formdaki bir sorgu sunucuya çevrilir ancak farklı bir biçimde yazılmışsa, sonuç aynı olsa bile bu şekilde çevrilmez.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-105">Sometimes, a query in one form translates to the server but if written in a different form doesn't translate even if the result is the same.</span></span> <span data-ttu-id="1a1bb-106">Bu sayfada bazı karmaşık işleçler ve bunların desteklenen çeşitlemeleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-106">This page describes some of the complex operators and their supported variations.</span></span> <span data-ttu-id="1a1bb-107">Gelecek sürümlerde, daha fazla desen tanıyabilir ve bunlara karşılık gelen çevirileri ekleyebiliriz.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-107">In future releases, we may recognize more patterns and add their corresponding translations.</span></span> <span data-ttu-id="1a1bb-108">Ayrıca, çeviri desteğinin sağlayıcılar arasında değiştiğini aklınızda bulundurmanız önemlidir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-108">It's also important to keep in mind that translation support varies between providers.</span></span> <span data-ttu-id="1a1bb-109">SqlServer içinde çevrilen belirli bir sorgu, SQLite veritabanları için çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-109">A particular query, which is translated in SqlServer, may not work for SQLite databases.</span></span>

> [!TIP]
> <span data-ttu-id="1a1bb-110">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-110">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="join"></a><span data-ttu-id="1a1bb-111">Birleştirme</span><span class="sxs-lookup"><span data-stu-id="1a1bb-111">Join</span></span>

<span data-ttu-id="1a1bb-112">LINQ JOIN işleci, her bir kaynak için anahtar seçicisine göre iki veri kaynağını bağlamanıza olanak tanır ve anahtar eşleştiğinde bir değer grubu oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-112">The LINQ Join operator allows you to connect two data sources based on the key selector for each source, generating a tuple of values when the key matches.</span></span> <span data-ttu-id="1a1bb-113">İlişkisel veritabanlarında `INNER JOIN` doğal olarak çevirir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-113">It naturally translates to `INNER JOIN` on relational databases.</span></span> <span data-ttu-id="1a1bb-114">LINQ birleşimi dış ve iç anahtar seçicileri içerdiğinden, veritabanı tek bir JOIN koşulu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-114">While the LINQ Join has outer and inner key selectors, the database requires a single join condition.</span></span> <span data-ttu-id="1a1bb-115">EF Core, dış anahtar seçiciyi eşitlik için iç anahtar seçiciyle karşılaştırarak bir birleşim koşulu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-115">So EF Core generates a join condition by comparing the outer key selector to the inner key selector for equality.</span></span> <span data-ttu-id="1a1bb-116">Ayrıca, anahtar seçicileri anonim türse EF Core eşitlik bileşeni yönlerini karşılaştırmak için bir JOIN koşulu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-116">Further, if the key selectors are anonymous types, EF Core generates a join condition to compare equality component wise.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a><span data-ttu-id="1a1bb-117">GroupJoin</span><span class="sxs-lookup"><span data-stu-id="1a1bb-117">GroupJoin</span></span>

<span data-ttu-id="1a1bb-118">LINQ Groupjoın işleci, birleşime benzer iki veri kaynağına bağlanmanızı sağlar, ancak eşleşen dış öğeler için bir iç değer grubu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-118">The LINQ GroupJoin operator allows you to connect two data sources similar to Join, but it creates a group of inner values for matching outer elements.</span></span> <span data-ttu-id="1a1bb-119">Aşağıdaki örnek gibi bir sorgu yürütmek `IEnumerable<Post>``Blog` & sonucunu üretir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-119">Executing a query like the following example generates a result of `Blog` & `IEnumerable<Post>`.</span></span> <span data-ttu-id="1a1bb-120">Veritabanları (özellikle de ilişkisel veritabanları), istemci tarafı nesneler koleksiyonunu temsil etmek için bir yol içermediğinden, Groupjoın birçok durumda sunucuya çevrilmez.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-120">Since databases (especially relational databases) don't have a way to represent a collection of client-side objects, GroupJoin doesn't translate to the server in many cases.</span></span> <span data-ttu-id="1a1bb-121">Özel bir seçici olmadan (aşağıdaki ilk sorgu), sunucudan tüm verileri Groupjoın yapmak üzere almanızı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-121">It requires you to get all of the data from the server to do GroupJoin without a special selector (first query below).</span></span> <span data-ttu-id="1a1bb-122">Ancak seçici verilerin seçili olduğunu sınırlandırırken, sunucudaki tüm verilerin getirilmesi performans sorunlarına neden olabilir (aşağıdaki ikinci sorgu aşağıda verilmiştir).</span><span class="sxs-lookup"><span data-stu-id="1a1bb-122">But if the selector is limiting data being selected then fetching all of the data from the server may cause performance issues (second query below).</span></span> <span data-ttu-id="1a1bb-123">EF Core Groupjoın 'i çevirmez.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-123">That's why EF Core doesn't translate GroupJoin.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a><span data-ttu-id="1a1bb-124">SelectMany</span><span class="sxs-lookup"><span data-stu-id="1a1bb-124">SelectMany</span></span>

<span data-ttu-id="1a1bb-125">LINQ SelectMany işleci her bir dış öğe için bir koleksiyon seçicisi üzerinde listeleme ve her bir veri kaynağından değer başlıkları oluşturma olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-125">The LINQ SelectMany operator allows you to enumerate over a collection selector for each outer element and generate tuples of values from each data source.</span></span> <span data-ttu-id="1a1bb-126">Bir şekilde, her dış öğenin koleksiyon kaynağından bir öğe ile bağlanması için herhangi bir koşul olmadan bir birleşimdir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-126">In a way, it's a join but without any condition so every outer element is connected with an element from the collection source.</span></span> <span data-ttu-id="1a1bb-127">Koleksiyon seçicinin dış veri kaynağıyla nasıl ilişkili olduğuna bağlı olarak, SelectMany sunucu tarafında çeşitli farklı sorgulara çevrilebilir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-127">Depending on how the collection selector is related to the outer data source, SelectMany can translate into various different queries on the server side.</span></span>

### <a name="collection-selector-doesnt-reference-outer"></a><span data-ttu-id="1a1bb-128">Koleksiyon Seçicisi dış başvuruya başvurmuyor</span><span class="sxs-lookup"><span data-stu-id="1a1bb-128">Collection selector doesn't reference outer</span></span>

<span data-ttu-id="1a1bb-129">Koleksiyon seçici dış kaynaktan herhangi bir şeye başvurmazsa, sonuç her iki veri kaynağına ait bir Kartezyen ürünüdür.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-129">When the collection selector isn't referencing anything from the outer source, the result is a cartesian product of both data sources.</span></span> <span data-ttu-id="1a1bb-130">İlişkisel veritabanlarında `CROSS JOIN` çevirir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-130">It translates to `CROSS JOIN` in relational databases.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a><span data-ttu-id="1a1bb-131">Koleksiyon Seçicisi bir where yan tümcesinde Outer öğesine başvuruyor</span><span class="sxs-lookup"><span data-stu-id="1a1bb-131">Collection selector references outer in a where clause</span></span>

<span data-ttu-id="1a1bb-132">Koleksiyon seçicisinin dış öğeye başvuran bir where yan tümcesi olduğunda EF Core, bunu bir veritabanı JOIN 'e çevirir ve koşul birleşim koşulu olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-132">When the collection selector has a where clause, which references the outer element, then EF Core translates it to a database join and uses the predicate as the join condition.</span></span> <span data-ttu-id="1a1bb-133">Normalde bu durum, koleksiyon seçici olarak dış öğe üzerinde koleksiyon gezintisi kullanılırken ortaya çıkar.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-133">Normally this case arises when using collection navigation on the outer element as the collection selector.</span></span> <span data-ttu-id="1a1bb-134">Koleksiyon bir dış öğe için boşsa, bu dış öğe için hiçbir sonuç oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-134">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="1a1bb-135">Ancak koleksiyon seçicisine `DefaultIfEmpty` uygulanmışsa dıştaki öğe, iç öğenin varsayılan bir değeriyle bağlanır.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-135">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="1a1bb-136">Bu ayrım nedeniyle, bu tür sorgular `DefaultIfEmpty` ve `DefaultIfEmpty` uygulandığında `LEFT JOIN` `INNER JOIN` dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-136">Because of this distinction, this kind of queries translates to `INNER JOIN` in the absence of `DefaultIfEmpty` and `LEFT JOIN` when `DefaultIfEmpty` is applied.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a><span data-ttu-id="1a1bb-137">Koleksiyon seçici, bir yerde olmayan bir dış durum başvurusu</span><span class="sxs-lookup"><span data-stu-id="1a1bb-137">Collection selector references outer in a non-where case</span></span>

<span data-ttu-id="1a1bb-138">Koleksiyon Seçicisi bir where yan tümcesinde olmayan dış öğeye başvurduğunda (Yukarıdaki durum gibi), bir veritabanı katılımı çevirmez.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-138">When the collection selector references the outer element, which isn't in a where clause (as the case above), it doesn't translate to a database join.</span></span> <span data-ttu-id="1a1bb-139">Bu nedenle her bir dış öğe için koleksiyon seçicisini değerlendirmemiz gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-139">That's why we need to evaluate the collection selector for each outer element.</span></span> <span data-ttu-id="1a1bb-140">Birçok ilişkisel veritabanında `APPLY` işlemlerine çevirir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-140">It translates to `APPLY` operations in many relational databases.</span></span> <span data-ttu-id="1a1bb-141">Koleksiyon bir dış öğe için boşsa, bu dış öğe için hiçbir sonuç oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-141">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="1a1bb-142">Ancak koleksiyon seçicisine `DefaultIfEmpty` uygulanmışsa dıştaki öğe, iç öğenin varsayılan bir değeriyle bağlanır.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-142">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="1a1bb-143">Bu ayrım nedeniyle, bu tür sorgular `DefaultIfEmpty` ve `DefaultIfEmpty` uygulandığında `OUTER APPLY` `CROSS APPLY` dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-143">Because of this distinction, this kind of queries translates to `CROSS APPLY` in the absence of `DefaultIfEmpty` and `OUTER APPLY` when `DefaultIfEmpty` is applied.</span></span> <span data-ttu-id="1a1bb-144">SQLite gibi bazı veritabanları `APPLY` işleçlerini desteklemediğinden, bu tür bir sorgu çevrilemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-144">Certain databases like SQLite don't support `APPLY` operators so this kind of query may not be translated.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a><span data-ttu-id="1a1bb-145">Ölçütü</span><span class="sxs-lookup"><span data-stu-id="1a1bb-145">GroupBy</span></span>

<span data-ttu-id="1a1bb-146">LINQ GroupBy işleçleri, `TKey` ve `TElement` herhangi bir rastgele tür olabilecek `IGrouping<TKey, TElement>` türünde bir sonuç oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-146">LINQ GroupBy operators create a result of type `IGrouping<TKey, TElement>` where `TKey` and `TElement` could be any arbitrary type.</span></span> <span data-ttu-id="1a1bb-147">Ayrıca, `IGrouping` `IEnumerable<TElement>`uygular, bu da Gruplamadan sonra herhangi bir LINQ işlecini kullanarak oluşturabileceğiniz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-147">Furthermore, `IGrouping` implements `IEnumerable<TElement>`, which means you can compose over it using any LINQ operator after the grouping.</span></span> <span data-ttu-id="1a1bb-148">Hiçbir veritabanı yapısı bir `IGrouping`temsil etmediği için, GroupBy işleçleri çoğu durumda çeviri içermez.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-148">Since no database structure can represent an `IGrouping`, GroupBy operators have no translation in most cases.</span></span> <span data-ttu-id="1a1bb-149">Bir skaler döndüren her gruba bir toplama işleci uygulandığında, ilişkisel veritabanlarında SQL `GROUP BY` çevrilebilir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-149">When an aggregate operator is applied to each group, which returns a scalar, it can be translated to SQL `GROUP BY` in relational databases.</span></span> <span data-ttu-id="1a1bb-150">SQL `GROUP BY` de kısıtlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-150">The SQL `GROUP BY` is restrictive too.</span></span> <span data-ttu-id="1a1bb-151">Yalnızca skaler değerlere göre gruplandırma yapmanızı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-151">It requires you to group only by scalar values.</span></span> <span data-ttu-id="1a1bb-152">Projeksiyon yalnızca gruplandırma anahtarı sütunlarını veya bir sütun üzerinde uygulanan herhangi bir toplamı içerebilir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-152">The projection can only contain grouping key columns or any aggregate applied over a column.</span></span> <span data-ttu-id="1a1bb-153">EF Core, bu kalıbı tanımlar ve aşağıdaki örnekte olduğu gibi sunucuya çevirir:</span><span class="sxs-lookup"><span data-stu-id="1a1bb-153">EF Core identifies this pattern and translates it to the server, as in the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

<span data-ttu-id="1a1bb-154">EF Core Ayrıca, gruplandırmadaki bir toplama işlecinin WHERE veya OrderBy (ya da başka bir sıralama) LINQ işlecinde göründüğü sorguları çevirir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-154">EF Core also translates queries where an aggregate operator on the grouping appears in a Where or OrderBy (or other ordering) LINQ operator.</span></span> <span data-ttu-id="1a1bb-155">WHERE yan tümcesi için SQL 'de `HAVING` yan tümcesini kullanır.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-155">It uses `HAVING` clause in SQL for the where clause.</span></span> <span data-ttu-id="1a1bb-156">Bir sorgu, GroupBy işlecini uygulamadan önce, sunucuya çevrilebilmesi gerektiği sürece herhangi bir karmaşık sorgu olabilir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-156">The part of the query before applying the GroupBy operator can be any complex query as long as it can be translated to server.</span></span> <span data-ttu-id="1a1bb-157">Ayrıca, sonuçta elde edilen kaynaktaki gruplandırmaları kaldırmak için bir gruplandırma sorgusuna toplama işleçleri uyguladıktan sonra, bunun üzerine diğer sorgular gibi oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-157">Furthermore, once you apply aggregate operators on a grouping query to remove groupings from the resulting source, you can compose on top of it like any other query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

<span data-ttu-id="1a1bb-158">EF Core tarafından desteklenen toplama işleçleri şunlardır</span><span class="sxs-lookup"><span data-stu-id="1a1bb-158">The aggregate operators EF Core supports are as follows</span></span>

- <span data-ttu-id="1a1bb-159">Ortalama</span><span class="sxs-lookup"><span data-stu-id="1a1bb-159">Average</span></span>
- <span data-ttu-id="1a1bb-160">Sayısı</span><span class="sxs-lookup"><span data-stu-id="1a1bb-160">Count</span></span>
- <span data-ttu-id="1a1bb-161">LongCount</span><span class="sxs-lookup"><span data-stu-id="1a1bb-161">LongCount</span></span>
- <span data-ttu-id="1a1bb-162">Maks</span><span class="sxs-lookup"><span data-stu-id="1a1bb-162">Max</span></span>
- <span data-ttu-id="1a1bb-163">Min</span><span class="sxs-lookup"><span data-stu-id="1a1bb-163">Min</span></span>
- <span data-ttu-id="1a1bb-164">TOPLA</span><span class="sxs-lookup"><span data-stu-id="1a1bb-164">Sum</span></span>

## <a name="left-join"></a><span data-ttu-id="1a1bb-165">Sola ekleme</span><span class="sxs-lookup"><span data-stu-id="1a1bb-165">Left Join</span></span>

<span data-ttu-id="1a1bb-166">LEFT JOIN bir LINQ işleci olmadığından, ilişkisel veritabanları sorgularda sık kullanılan bir LEFT JOIN kavramıdır.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-166">While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries.</span></span> <span data-ttu-id="1a1bb-167">LINQ sorgularında belirli bir model, sunucudaki `LEFT JOIN` aynı sonucu verir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-167">A particular pattern in LINQ queries gives the same result as a `LEFT JOIN` on the server.</span></span> <span data-ttu-id="1a1bb-168">EF Core bu desenleri tanımlar ve sunucu tarafında eşdeğer `LEFT JOIN` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-168">EF Core identifies such patterns and generates the equivalent `LEFT JOIN` on the server side.</span></span> <span data-ttu-id="1a1bb-169">Bu model, her iki veri kaynağı arasında bir Groupjoın oluşturmayı ve sonra, iç ilgili bir öğe olmadığında null ile eşleştirmek için gruplandırma kaynağında Defaultıempty ile Selectınempty olan selectıempty işlecini kullanarak gruplamayı düzleştirmeyi içerir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-169">The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element.</span></span> <span data-ttu-id="1a1bb-170">Aşağıdaki örnek, bu düzenin neye benzemediğini ve ne yaptığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-170">The following example shows what that pattern looks like and what it generates.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

<span data-ttu-id="1a1bb-171">Yukarıdaki model ifade ağacında karmaşık bir yapı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-171">The above pattern creates a complex structure in the expression tree.</span></span> <span data-ttu-id="1a1bb-172">Bu nedenle, EF Core, bir adım içindeki Groupjoın işlecinin gruplandırma sonuçlarını, işlecin hemen sonrasında bir adımla düzleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-172">Because of that, EF Core requires you to flatten out the grouping results of the GroupJoin operator in a step immediately following the operator.</span></span> <span data-ttu-id="1a1bb-173">Groupjoın-Defaultıempty-SelectMany çok fazla kullanılıyor olsa da, farklı bir düzende bunu bir LEFT JOIN olarak belirleyemeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1a1bb-173">Even if the GroupJoin-DefaultIfEmpty-SelectMany is used but in a different pattern, we may not identify it as a Left Join.</span></span>
