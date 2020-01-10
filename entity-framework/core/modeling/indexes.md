---
title: Dizinler-EF Core
author: roji
ms.date: 12/16/2019
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 810fccc0c6b035f515107601b245811f7b4118a6
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502142"
---
# <a name="indexes"></a><span data-ttu-id="0fd60-102">Dizinler</span><span class="sxs-lookup"><span data-stu-id="0fd60-102">Indexes</span></span>

<span data-ttu-id="0fd60-103">Dizinler birçok veri deposu genelinde ortak bir kavramdır.</span><span class="sxs-lookup"><span data-stu-id="0fd60-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="0fd60-104">Veri deposundaki uygulamaları farklılık gösterebilir, ancak bir sütuna (veya sütun kümesine) göre aramalar yapmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0fd60-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

<span data-ttu-id="0fd60-105">Dizinler, veri açıklamaları kullanılarak oluşturulamaz.</span><span class="sxs-lookup"><span data-stu-id="0fd60-105">Indexes cannot be created using data annotations.</span></span> <span data-ttu-id="0fd60-106">Tek bir sütunda dizin belirtmek için akıcı API 'yi aşağıdaki şekilde kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0fd60-106">You can use the Fluent API to specify an index on a single column as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=4)]

<span data-ttu-id="0fd60-107">Ayrıca, birden fazla sütundan bir dizin belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0fd60-107">You can also specify an index over more than one column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=4)]

> [!NOTE]
> <span data-ttu-id="0fd60-108">Kurala göre, yabancı anahtar olarak kullanılan her bir özellikte (veya özellik kümesinde) bir dizin oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0fd60-108">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>
>
> <span data-ttu-id="0fd60-109">EF Core, farklı özellik kümesi başına yalnızca bir dizini destekler.</span><span class="sxs-lookup"><span data-stu-id="0fd60-109">EF Core only supports one index per distinct set of properties.</span></span> <span data-ttu-id="0fd60-110">Zaten bir dizini tanımlanmış, kural veya önceki yapılandırma ile tanımlanmış bir dizi özellik kümesi üzerinde bir dizin yapılandırmak için akıcı API kullanıyorsanız, bu dizinin tanımını değiştirmiş olursunuz.</span><span class="sxs-lookup"><span data-stu-id="0fd60-110">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="0fd60-111">Kural tarafından oluşturulan bir dizini daha fazla yapılandırmak istiyorsanız bu yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="0fd60-111">This is useful if you want to further configure an index that was created by convention.</span></span>

## <a name="index-uniqueness"></a><span data-ttu-id="0fd60-112">Dizin benzersizliği</span><span class="sxs-lookup"><span data-stu-id="0fd60-112">Index uniqueness</span></span>

<span data-ttu-id="0fd60-113">Varsayılan olarak, dizinler benzersiz değildir: birden çok satırın, dizinin sütun kümesi için aynı değere sahip olmasına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="0fd60-113">By default, indexes aren't unique: multiple rows are allowed to have the same value(s) for the index's column set.</span></span> <span data-ttu-id="0fd60-114">Dizini aşağıdaki gibi benzersiz hale getirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0fd60-114">You can make an index unique as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=IndexUnique&highlight=5)]

<span data-ttu-id="0fd60-115">Dizinin sütun kümesi için aynı değerlere sahip birden fazla varlık eklenmeye çalışıldığında bir özel durum oluşturulmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="0fd60-115">Attempting to insert more than one entity with the same values for the index's column set will cause an exception to be thrown.</span></span>

## <a name="index-name"></a><span data-ttu-id="0fd60-116">Dizin adı</span><span class="sxs-lookup"><span data-stu-id="0fd60-116">Index name</span></span>

<span data-ttu-id="0fd60-117">Kurala göre, ilişkisel bir veritabanında oluşturulan dizinlerin `IX_<type name>_<property name>`adı verilir.</span><span class="sxs-lookup"><span data-stu-id="0fd60-117">By convention, indexes created in a relational database are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="0fd60-118">Bileşik dizinler için `<property name>`, özellik adlarının alt çizgiyle ayrılmış bir listesi haline gelir.</span><span class="sxs-lookup"><span data-stu-id="0fd60-118">For composite indexes, `<property name>` becomes an underscore separated list of property names.</span></span>

<span data-ttu-id="0fd60-119">Veritabanında oluşturulan dizinin adını ayarlamak için Floent API 'sini kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0fd60-119">You can use the Fluent API to set the name of the index created in the database:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexName.cs?name=IndexName&highlight=5)]

## <a name="index-filter"></a><span data-ttu-id="0fd60-120">Dizin filtresi</span><span class="sxs-lookup"><span data-stu-id="0fd60-120">Index filter</span></span>

<span data-ttu-id="0fd60-121">Bazı ilişkisel veritabanları filtrelenmiş veya kısmi dizin belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="0fd60-121">Some relational databases allow you to specify a filtered or partial index.</span></span> <span data-ttu-id="0fd60-122">Bu, bir sütunun değerlerinin yalnızca bir alt kümesini dizinetmenize, dizin boyutunu azaltmanızı ve hem performans hem de disk alanı kullanımını iyileştirmeye olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="0fd60-122">This allows you to index only a subset of a column's values, reducing the index's size and improving both performance and disk space usage.</span></span> <span data-ttu-id="0fd60-123">Filtrelenmiş dizinler SQL Server hakkında daha fazla bilgi için [belgelerine bakın](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes).</span><span class="sxs-lookup"><span data-stu-id="0fd60-123">For more information on SQL Server filtered indexes, [see the documentation](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes).</span></span>

<span data-ttu-id="0fd60-124">Bir dizin üzerinde SQL ifadesi olarak sağlanmış bir filtre belirtmek için Floent API 'sini kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0fd60-124">You can use the Fluent API to specify a filter on an index, provided as a SQL expression:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexFilter.cs?name=IndexFilter&highlight=5)]

<span data-ttu-id="0fd60-125">SQL Server sağlayıcısı EF kullanıldığında, benzersiz bir dizinin parçası olan tüm null yapılabilen sütunlar için bir `'IS NOT NULL'` filtresi ekler.</span><span class="sxs-lookup"><span data-stu-id="0fd60-125">When using the SQL Server provider EF adds an `'IS NOT NULL'` filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="0fd60-126">Bu kuralı geçersiz kılmak için `null` bir değer sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0fd60-126">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexNoFilter.cs?name=IndexNoFilter&highlight=6)]

## <a name="included-columns"></a><span data-ttu-id="0fd60-127">Dahil edilen sütunlar</span><span class="sxs-lookup"><span data-stu-id="0fd60-127">Included columns</span></span>

<span data-ttu-id="0fd60-128">Bazı ilişkisel veritabanları, dizine dahil edilen ancak "Key" ın bir parçası olmayan bir sütun kümesini yapılandırmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="0fd60-128">Some relational databases allow you to configure a set of columns which get included in the index, but aren't part of its "key".</span></span> <span data-ttu-id="0fd60-129">Bu, tabloya erişilmesi gerekmiyorsa sorgudaki tüm sütunlar anahtar veya anahtar olmayan sütunlar olarak dizine eklendiğinde sorgu performansını önemli ölçüde iyileştirebilir.</span><span class="sxs-lookup"><span data-stu-id="0fd60-129">This can significantly improve query performance when all columns in the query are included in the index either as key or nonkey columns, as the table itself doesn't need to be accessed.</span></span> <span data-ttu-id="0fd60-130">İçerilen SQL Server sütunları hakkında daha fazla bilgi için [belgelerine bakın](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns).</span><span class="sxs-lookup"><span data-stu-id="0fd60-130">For more information on SQL Server included columns, [see the documentation](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns).</span></span>

<span data-ttu-id="0fd60-131">Aşağıdaki örnekte, `Url` sütunu dizin anahtarının bir parçası olduğundan, bu sütunda herhangi bir sorgu filtrelemesi dizini kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="0fd60-131">In the following example, the `Url` column is part of the index key, so any query filtering on that column can use the index.</span></span> <span data-ttu-id="0fd60-132">Ancak buna ek olarak, yalnızca `Title` ve `PublishedOn` sütunlarına erişen sorguların tabloya erişmesi ve daha verimli bir şekilde çalışması gerekecektir:</span><span class="sxs-lookup"><span data-stu-id="0fd60-132">But in addition, queries accessing only the `Title` and `PublishedOn` columns will not need to access the table and will run more efficiently:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexInclude.cs?name=IndexInclude&highlight=5-9)]
