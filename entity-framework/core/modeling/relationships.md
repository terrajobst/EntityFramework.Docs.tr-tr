---
title: İlişkiler-EF Core
description: Entity Framework Core kullanırken varlık türleri arasında ilişkiler yapılandırma
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relationships
ms.openlocfilehash: 6d68e813cec6c989e8e4cb848f8740489645c65c
ms.sourcegitcommit: 89567d08c9d8bf9c33bb55a62f17067094a4065a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77051413"
---
# <a name="relationships"></a><span data-ttu-id="5fd64-103">İlişkiler</span><span class="sxs-lookup"><span data-stu-id="5fd64-103">Relationships</span></span>

<span data-ttu-id="5fd64-104">Bir ilişki, iki varlığın birbirleriyle ilişkisini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="5fd64-104">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="5fd64-105">İlişkisel bir veritabanında bu, yabancı anahtar kısıtlaması tarafından temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-105">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="5fd64-106">Bu makaledeki örneklerin çoğu, kavramları göstermek için bire çok ilişki kullanır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-106">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="5fd64-107">Bire bir ve çoktan çoğa ilişkilerin örnekleri için makalenin sonundaki [diğer Ilişki desenleri](#other-relationship-patterns) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="5fd64-107">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="5fd64-108">Koşulların tanımı</span><span class="sxs-lookup"><span data-stu-id="5fd64-108">Definition of terms</span></span>

<span data-ttu-id="5fd64-109">İlişkileri anlatmak için kullanılan bazı terimler vardır</span><span class="sxs-lookup"><span data-stu-id="5fd64-109">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="5fd64-110">**Bağımlı varlık:** Bu, yabancı anahtar özelliklerini içeren varlıktır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-110">**Dependent entity:** This is the entity that contains the foreign key properties.</span></span> <span data-ttu-id="5fd64-111">Bazen ilişkinin ' Child ' olarak da adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-111">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="5fd64-112">**Asıl varlık:** Bu, birincil/alternatif anahtar özelliklerini içeren varlıktır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-112">**Principal entity:** This is the entity that contains the primary/alternate key properties.</span></span> <span data-ttu-id="5fd64-113">Bazen ilişkinin ' Parent ' olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-113">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="5fd64-114">**Asıl anahtar:** Asıl varlığı benzersiz bir şekilde tanımlayan Özellikler.</span><span class="sxs-lookup"><span data-stu-id="5fd64-114">**Principal key:** The properties that uniquely identify the principal entity.</span></span> <span data-ttu-id="5fd64-115">Bu, birincil anahtar veya alternatif bir anahtar olabilir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="5fd64-116">**Yabancı anahtar:** İlgili varlık için asıl anahtar değerlerini depolamak için kullanılan bağımlı varlıktaki Özellikler.</span><span class="sxs-lookup"><span data-stu-id="5fd64-116">**Foreign key:** The properties in the dependent entity that are used to store the principal key values for the related entity.</span></span>

* <span data-ttu-id="5fd64-117">**Gezinti özelliği:** Sorumlu ve/veya bağımlı varlık üzerinde tanımlanan, ilgili varlığa başvuran bir özellik.</span><span class="sxs-lookup"><span data-stu-id="5fd64-117">**Navigation property:** A property defined on the principal and/or dependent entity that references the related entity.</span></span>

  * <span data-ttu-id="5fd64-118">**Koleksiyon gezinti özelliği:** Birçok ilgili varlığa başvurular içeren bir gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="5fd64-118">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="5fd64-119">**Başvuru gezinti özelliği:** Tek bir ilgili varlığa başvuruyu tutan bir gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="5fd64-119">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="5fd64-120">**Ters gezinme özelliği:** Belirli bir gezinti özelliği tartışırken, bu terim ilişkinin diğer ucundaki gezinti özelliğine başvurur.</span><span class="sxs-lookup"><span data-stu-id="5fd64-120">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>
  
* <span data-ttu-id="5fd64-121">**Kendine başvuran ilişki:** Bağımlı ve asıl varlık türlerinin aynı olduğu bir ilişki.</span><span class="sxs-lookup"><span data-stu-id="5fd64-121">**Self-referencing relationship:** A relationship in which the dependent and the principal entity types are the same.</span></span>

<span data-ttu-id="5fd64-122">Aşağıdaki kod, `Blog` ve `Post` arasında bir-çok ilişkisi gösterir</span><span class="sxs-lookup"><span data-stu-id="5fd64-122">The following code shows a one-to-many relationship between `Blog` and `Post`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Full)]

* <span data-ttu-id="5fd64-123">bağımlı varlık `Post`</span><span class="sxs-lookup"><span data-stu-id="5fd64-123">`Post` is the dependent entity</span></span>

* <span data-ttu-id="5fd64-124">Asıl varlık `Blog`</span><span class="sxs-lookup"><span data-stu-id="5fd64-124">`Blog` is the principal entity</span></span>

* <span data-ttu-id="5fd64-125">`Blog.BlogId` asıl anahtardır (Bu durumda, alternatif bir anahtar yerine birincil anahtardır)</span><span class="sxs-lookup"><span data-stu-id="5fd64-125">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="5fd64-126">yabancı anahtar `Post.BlogId`</span><span class="sxs-lookup"><span data-stu-id="5fd64-126">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="5fd64-127">`Post.Blog` bir başvuru gezinti özelliğidir</span><span class="sxs-lookup"><span data-stu-id="5fd64-127">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="5fd64-128">bir koleksiyon gezinti özelliği `Blog.Posts`</span><span class="sxs-lookup"><span data-stu-id="5fd64-128">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="5fd64-129">`Post.Blog`, `Blog.Posts` ters gezinti özelliğidir (ve tam tersi)</span><span class="sxs-lookup"><span data-stu-id="5fd64-129">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

## <a name="conventions"></a><span data-ttu-id="5fd64-130">Kurallar</span><span class="sxs-lookup"><span data-stu-id="5fd64-130">Conventions</span></span>

<span data-ttu-id="5fd64-131">Varsayılan olarak, bir tür üzerinde bulunan bir gezinti özelliği olduğunda bir ilişki oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5fd64-131">By default, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="5fd64-132">Bir özellik, işaret ettiği türün geçerli veritabanı sağlayıcısı tarafından skalar bir tür olarak eşleştirilemez olması halinde bir gezinti özelliği olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-132">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="5fd64-133">Kural tarafından keşfedilen ilişkiler her zaman asıl varlığın birincil anahtarını hedefler.</span><span class="sxs-lookup"><span data-stu-id="5fd64-133">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="5fd64-134">Alternatif bir anahtarı hedeflemek için, akıcı API kullanılarak ek yapılandırma gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-134">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="5fd64-135">Tam olarak tanımlanmış ilişkiler</span><span class="sxs-lookup"><span data-stu-id="5fd64-135">Fully defined relationships</span></span>

<span data-ttu-id="5fd64-136">İlişkiler için en yaygın model, ilişkinin her iki ucunda ve bağımlı varlık sınıfında tanımlanmış yabancı anahtar özelliği için tanımlanmış gezinti özelliklerine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-136">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="5fd64-137">İki tür arasında bir dizi gezinti özelliği bulunursa, bu, aynı ilişkinin ters gezinme özellikleri olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-137">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="5fd64-138">Bağımlı varlık, bu desenlerden biriyle eşleşen bir ada sahip bir özellik içeriyorsa, yabancı anahtar olarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="5fd64-138">If the dependent entity contains a property with a name matching one of these patterns then it will be configured as the foreign key:</span></span>
  * `<navigation property name><principal key property name>`
  * `<navigation property name>Id`
  * `<principal entity name><principal key property name>`
  * `<principal entity name>Id`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Full&highlight=6,15-16)]

<span data-ttu-id="5fd64-139">Bu örnekte, ilişkiyi yapılandırmak için vurgulanan Özellikler kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-139">In this example the highlighted properties will be used to configure the relationship.</span></span>

> [!NOTE]
> <span data-ttu-id="5fd64-140">Özellik birincil anahtar ise veya asıl anahtarla uyumlu olmayan bir türde ise, yabancı anahtar olarak yapılandırılmaz.</span><span class="sxs-lookup"><span data-stu-id="5fd64-140">If the property is the primary key or is of a type not compatible with the principal key then it won't be configured as the foreign key.</span></span>

> [!NOTE]
> <span data-ttu-id="5fd64-141">EF Core 3,0 önce, asıl anahtar özelliği ile tam olarak aynı adlı özellik [yabancı anahtar olarak da eşleştirildiği](https://github.com/aspnet/EntityFrameworkCore/issues/13274) için</span><span class="sxs-lookup"><span data-stu-id="5fd64-141">Before EF Core 3.0 the property named exactly the same as the principal key property [was also matched as the foreign key](https://github.com/aspnet/EntityFrameworkCore/issues/13274)</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="5fd64-142">Yabancı anahtar özelliği yok</span><span class="sxs-lookup"><span data-stu-id="5fd64-142">No foreign key property</span></span>

<span data-ttu-id="5fd64-143">Bağımlı varlık sınıfında tanımlanmış yabancı anahtar özelliği olması önerilse de, bu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-143">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="5fd64-144">Yabancı anahtar özelliği bulunmazsa, bağımlı tür üzerinde hiçbir gezinti yoksa, `<navigation property name><principal key property name>` veya `<principal entity name><principal key property name>` bir [gölge yabancı anahtar özelliği](shadow-properties.md) tanıtılacaktır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-144">If no foreign key property is found, a [shadow foreign key property](shadow-properties.md) will be introduced with the name `<navigation property name><principal key property name>` or `<principal entity name><principal key property name>` if no navigation is present on the dependent type.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=6,15)]

<span data-ttu-id="5fd64-145">Bu örnekte, ön bekleyen gezinti adının gereksiz olması nedeniyle gölge yabancı anahtarı `BlogId`.</span><span class="sxs-lookup"><span data-stu-id="5fd64-145">In this example the shadow foreign key is `BlogId` because prepending the navigation name would be redundant.</span></span>

> [!NOTE]
> <span data-ttu-id="5fd64-146">Aynı ada sahip bir özellik zaten varsa, gölge Özellik adı bir sayıyla sonlenir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-146">If a property with the same name already exists then the shadow property name will be suffixed with a number.</span></span>

### <a name="single-navigation-property"></a><span data-ttu-id="5fd64-147">Tek gezinti özelliği</span><span class="sxs-lookup"><span data-stu-id="5fd64-147">Single navigation property</span></span>

<span data-ttu-id="5fd64-148">Tek bir gezinti özelliği (ters gezinme olmadan ve yabancı anahtar özelliği olmadan) dahil olmak üzere, kural tarafından tanımlanan bir ilişkiye sahip olmak için yeterli değildir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-148">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="5fd64-149">Tek bir gezinti özelliğine ve yabancı anahtar özelliğine de sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fd64-149">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=OneNavigation&highlight=6)]

### <a name="limitations"></a><span data-ttu-id="5fd64-150">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="5fd64-150">Limitations</span></span>

<span data-ttu-id="5fd64-151">İki tür arasında tanımlı birden çok gezinti özelliği olduğunda (diğer bir deyişle, birbirine işaret eden tek bir bir çift gezintiden daha fazla), gezinti özellikleriyle temsil edilen ilişkiler belirsizdir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-151">When there are multiple navigation properties defined between two types (that is, more than just one pair of navigations that point to each other) the relationships represented by the navigation properties are ambiguous.</span></span> <span data-ttu-id="5fd64-152">Belirsizliği çözümlemek için bunları el ile yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-152">You will need to manually configure them to resolve the ambiguity.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="5fd64-153">Basamaklı silme</span><span class="sxs-lookup"><span data-stu-id="5fd64-153">Cascade delete</span></span>

<span data-ttu-id="5fd64-154">Kurala göre, Cascade DELETE, isteğe bağlı ilişkiler için gerekli ilişkiler ve *Clientsetnull* için *Cascade* olarak ayarlanacak.</span><span class="sxs-lookup"><span data-stu-id="5fd64-154">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="5fd64-155">*Cascade* , bağımlı varlıkların de silindiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-155">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="5fd64-156">*Clientsetnull* , belleğe yüklenmeyen bağımlı varlıkların değişmeden kalacağından, el ile silinmesi ya da geçerli bir sorumlu varlığına işaret edecek şekilde güncelleştirilmeleri anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-156">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="5fd64-157">Belleğe yüklenen varlıklar için EF Core yabancı anahtar özelliklerini null olarak ayarlamaya çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-157">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="5fd64-158">Gerekli ve isteğe bağlı ilişkiler arasındaki fark için [gerekli ve Isteğe bağlı ilişkiler](#required-and-optional-relationships) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="5fd64-158">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="5fd64-159">Farklı silme davranışları ve kural tarafından kullanılan varsayılanlar hakkında daha fazla ayrıntı için bkz. [Cascade Delete](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="5fd64-159">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="manual-configuration"></a><span data-ttu-id="5fd64-160">El ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="5fd64-160">Manual configuration</span></span>

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="5fd64-161">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="5fd64-161">Fluent API</span></span>](#tab/fluent-api)

<span data-ttu-id="5fd64-162">Akıcı API 'de bir ilişki yapılandırmak için, ilişkiyi oluşturan gezinti özelliklerini tanımlayarak başlacaksınız.</span><span class="sxs-lookup"><span data-stu-id="5fd64-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="5fd64-163">`HasOne` veya `HasMany`, üzerinde yapılandırmaya başmakta olduğunuz varlık türünde gezinti özelliğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="5fd64-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="5fd64-164">Daha sonra ters gezintiyi belirlemek için `WithOne` veya `WithMany` bir çağrı zincirleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fd64-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="5fd64-165">`HasOne`/`WithOne` başvuru gezinti özellikleri için kullanılır ve koleksiyon gezinti özellikleri için `HasMany`/`WithMany` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=8-10)]

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="5fd64-166">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="5fd64-166">Data annotations</span></span>](#tab/data-annotations)

<span data-ttu-id="5fd64-167">Bağımlı ve birincil varlıklarda gezinti özelliklerinin nasıl kullandığını yapılandırmak için veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fd64-167">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="5fd64-168">Bu, genellikle iki varlık türü arasında birden fazla gezinti özelliği olduğunda yapılır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-168">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?name=InverseProperty&highlight=20,23)]

> [!NOTE]
> <span data-ttu-id="5fd64-169">İlişkinin gerekilliğini etkilemek için yalnızca bağımlı varlıktaki özelliklerde [Required] kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fd64-169">You can only use [Required] on properties on the dependent entity to impact the requiredness of the relationship.</span></span> <span data-ttu-id="5fd64-170">[Gerekli] asıl varlıktaki gezinmede genellikle yok sayılır, ancak varlığın bağımlı bir duruma gelmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-170">[Required] on the navigation from the principal entity is usually ignored, but it may cause the entity to become the dependent one.</span></span>

> [!NOTE]
> <span data-ttu-id="5fd64-171">`[ForeignKey]` ve `[InverseProperty]` veri ek açıklamaları `System.ComponentModel.DataAnnotations.Schema` ad alanında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-171">The data annotations `[ForeignKey]` and `[InverseProperty]` are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span> <span data-ttu-id="5fd64-172">`[Required]`, `System.ComponentModel.DataAnnotations` ad alanında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-172">`[Required]` is available in the `System.ComponentModel.DataAnnotations` namespace.</span></span>

---

### <a name="single-navigation-property"></a><span data-ttu-id="5fd64-173">Tek gezinti özelliği</span><span class="sxs-lookup"><span data-stu-id="5fd64-173">Single navigation property</span></span>

<span data-ttu-id="5fd64-174">Yalnızca bir gezinti özelliğine sahipseniz, `WithOne` ve `WithMany`Parametresiz aşırı yüklemeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-174">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="5fd64-175">Bu, ilişkinin diğer ucunda kavramsal olarak bir başvuru veya koleksiyon olduğunu gösterir, ancak varlık sınıfında hiçbir gezinti özelliği dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-175">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?name=OneNavigation&highlight=8-10)]

### <a name="foreign-key"></a><span data-ttu-id="5fd64-176">Yabancı anahtar</span><span class="sxs-lookup"><span data-stu-id="5fd64-176">Foreign key</span></span>

#### <a name="fluent-api-simple-keytabfluent-api-simple-key"></a>[<span data-ttu-id="5fd64-177">Akıcı API (basit anahtar)</span><span class="sxs-lookup"><span data-stu-id="5fd64-177">Fluent API (simple key)</span></span>](#tab/fluent-api-simple-key)

<span data-ttu-id="5fd64-178">Belirli bir ilişki için yabancı anahtar özelliği olarak kullanılması gereken özelliği yapılandırmak için Floent API 'sini kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5fd64-178">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?name=ForeignKey&highlight=11)]

#### <a name="fluent-api-composite-keytabfluent-api-composite-key"></a>[<span data-ttu-id="5fd64-179">Akıcı API (bileşik anahtar)</span><span class="sxs-lookup"><span data-stu-id="5fd64-179">Fluent API (composite key)</span></span>](#tab/fluent-api-composite-key)

<span data-ttu-id="5fd64-180">Belirli bir ilişki için bileşik yabancı anahtar özellikleri olarak kullanılması gereken özellikleri yapılandırmak için akıcı API 'YI kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5fd64-180">You can use the Fluent API to configure which properties should be used as the composite foreign key properties for a given relationship:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?name=CompositeForeignKey&highlight=13)]

#### <a name="data-annotations-simple-keytabdata-annotations-simple-key"></a>[<span data-ttu-id="5fd64-181">Veri açıklamaları (basit anahtar)</span><span class="sxs-lookup"><span data-stu-id="5fd64-181">Data annotations (simple key)</span></span>](#tab/data-annotations-simple-key)

<span data-ttu-id="5fd64-182">Belirli bir ilişki için yabancı anahtar özelliği olarak kullanılması gereken özelliği yapılandırmak için veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fd64-182">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="5fd64-183">Bu, genellikle yabancı anahtar özelliği kural tarafından keşfedildiğinde yapılır:</span><span class="sxs-lookup"><span data-stu-id="5fd64-183">This is typically done when the foreign key property is not discovered by convention:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?name=ForeignKey&highlight=17)]

> [!TIP]  
> <span data-ttu-id="5fd64-184">`[ForeignKey]` ek açıklaması, ilişkide herhangi bir gezinti özelliğine eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-184">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="5fd64-185">Bağımlı varlık sınıfında gezinti özelliğine gitmesinin gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-185">It does not need to go on the navigation property in the dependent entity class.</span></span>

> [!NOTE]
> <span data-ttu-id="5fd64-186">Gezinti özelliğinde `[ForeignKey]` kullanılarak belirtilen özelliğin bağımlı tür olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5fd64-186">The property specified using `[ForeignKey]` on a navigation property doesn't need to exist the the dependent type.</span></span> <span data-ttu-id="5fd64-187">Bu durumda, belirtilen ad bir gölge yabancı anahtar oluşturmak için kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-187">In this case the specified name will be used to create a shadow foreign key.</span></span>

---

#### <a name="shadow-foreign-key"></a><span data-ttu-id="5fd64-188">Gölge yabancı anahtar</span><span class="sxs-lookup"><span data-stu-id="5fd64-188">Shadow foreign key</span></span>

<span data-ttu-id="5fd64-189">Bir gölge özelliği yabancı anahtar olarak yapılandırmak için `HasForeignKey(...)` dize aşırı yüklemesini kullanabilirsiniz (daha fazla bilgi için bkz. [gölge özellikleri](shadow-properties.md) ).</span><span class="sxs-lookup"><span data-stu-id="5fd64-189">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="5fd64-190">Gölge özelliğini, yabancı anahtar olarak kullanmadan önce modele açıkça eklememiz önerilir (aşağıda gösterildiği gibi).</span><span class="sxs-lookup"><span data-stu-id="5fd64-190">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs?name=ShadowForeignKey&highlight=10,16)]

#### <a name="foreign-key-constraint-name"></a><span data-ttu-id="5fd64-191">Yabancı anahtar kısıtlama adı</span><span class="sxs-lookup"><span data-stu-id="5fd64-191">Foreign key constraint name</span></span>

<span data-ttu-id="5fd64-192">Kural gereği, bir ilişkisel veritabanını hedeflerken, yabancı anahtar kısıtlamaları FK_<dependent type name> _<principal type name>_ <foreign key property name>olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-192">By convention, when targeting a relational database, foreign key constraints are named FK_<dependent type name>_<principal type name>_<foreign key property name>.</span></span> <span data-ttu-id="5fd64-193">Bileşik yabancı anahtarlar için <foreign key property name> yabancı anahtar özellik adlarının alt çizgiyle ayrılmış bir listesi haline gelir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-193">For composite foreign keys <foreign key property name> becomes an underscore separated list of foreign key property names.</span></span>

<span data-ttu-id="5fd64-194">Kısıtlama adını aşağıdaki şekilde de yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5fd64-194">You can also configure the constraint name as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ConstraintName.cs?name=ConstraintName&highlight=6-7)]

### <a name="without-navigation-property"></a><span data-ttu-id="5fd64-195">Gezinti özelliği olmadan</span><span class="sxs-lookup"><span data-stu-id="5fd64-195">Without navigation property</span></span>

<span data-ttu-id="5fd64-196">Bir gezinti özelliği sağlamanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5fd64-196">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="5fd64-197">İlişkinin yalnızca bir tarafında yabancı anahtar sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fd64-197">You can simply provide a foreign key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?name=NoNavigation&highlight=8-11)]

### <a name="principal-key"></a><span data-ttu-id="5fd64-198">Asıl anahtar</span><span class="sxs-lookup"><span data-stu-id="5fd64-198">Principal key</span></span>

<span data-ttu-id="5fd64-199">Yabancı anahtarın birincil anahtar dışında bir özelliğe başvurmasına isterseniz, ilişkinin asıl anahtar özelliğini yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fd64-199">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="5fd64-200">Asıl anahtar olarak yapılandırdığınız özellik otomatik olarak [alternatif bir anahtar](alternate-keys.md)olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="5fd64-200">The property that you configure as the principal key will automatically be setup as an [alternate key](alternate-keys.md).</span></span>

#### <a name="simple-keytabsimple-key"></a>[<span data-ttu-id="5fd64-201">Basit anahtar</span><span class="sxs-lookup"><span data-stu-id="5fd64-201">Simple key</span></span>](#tab/simple-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

#### <a name="composite-keytabcomposite-key"></a>[<span data-ttu-id="5fd64-202">Bileşik anahtar</span><span class="sxs-lookup"><span data-stu-id="5fd64-202">Composite key</span></span>](#tab/composite-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=CompositePrincipalKey&highlight=11)]

> [!WARNING]  
> <span data-ttu-id="5fd64-203">Asıl anahtar özelliklerini belirttiğiniz sıra, yabancı anahtar için belirtildikleri sırayla eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-203">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

---

### <a name="required-and-optional-relationships"></a><span data-ttu-id="5fd64-204">Gerekli ve isteğe bağlı ilişkiler</span><span class="sxs-lookup"><span data-stu-id="5fd64-204">Required and optional relationships</span></span>

<span data-ttu-id="5fd64-205">İlişkinin gerekli veya isteğe bağlı olup olmadığını yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fd64-205">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="5fd64-206">Sonuç olarak bu, yabancı anahtar özelliğinin gerekli veya isteğe bağlı olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="5fd64-206">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="5fd64-207">Bu, büyük bir gölge durumu yabancı anahtar kullanırken faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-207">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="5fd64-208">Varlık sınıfınızda yabancı anahtar özelliği varsa, ilişkinin gereklik durumu yabancı anahtar özelliğinin gerekli veya isteğe bağlı olup olmadığına göre belirlenir (daha fazla bilgi için bkz. [gerekli ve Isteğe bağlı özellikler](required-optional.md) ).</span><span class="sxs-lookup"><span data-stu-id="5fd64-208">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=6)]

> [!NOTE]
> <span data-ttu-id="5fd64-209">`IsRequired(false)` çağrısı, aksi belirtilmedikçe yabancı anahtar özelliğini de isteğe bağlı hale getirir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-209">Calling `IsRequired(false)` also makes the foreign key property optional unless it's configured otherwise.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="5fd64-210">Basamaklı silme</span><span class="sxs-lookup"><span data-stu-id="5fd64-210">Cascade delete</span></span>

<span data-ttu-id="5fd64-211">Belirli bir ilişki için basamaklı silme davranışını açıkça yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fd64-211">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="5fd64-212">Her seçeneğe ilişkin ayrıntılı bir tartışma için bkz. [Cascade Delete](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="5fd64-212">See [Cascade Delete](../saving/cascade-delete.md) for a detailed discussion of each option.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=6)]

## <a name="other-relationship-patterns"></a><span data-ttu-id="5fd64-213">Diğer ilişki desenleri</span><span class="sxs-lookup"><span data-stu-id="5fd64-213">Other relationship patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="5fd64-214">Bire bir</span><span class="sxs-lookup"><span data-stu-id="5fd64-214">One-to-one</span></span>

<span data-ttu-id="5fd64-215">Bir ilişkinin her iki tarafında da başvuru gezintisi özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="5fd64-215">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="5fd64-216">Tek-çok ilişkilerle aynı kurallara uyar, ancak her sorumluyla yalnızca bir bağımlı ilişki olduğundan emin olmak için yabancı anahtar özelliğinde benzersiz bir dizin sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="5fd64-216">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=OneToOne&highlight=6,15-16)]

> [!NOTE]  
> <span data-ttu-id="5fd64-217">EF, bir yabancı anahtar özelliği algılama özelliğine göre bağımlı olacak varlıklardan birini seçer.</span><span class="sxs-lookup"><span data-stu-id="5fd64-217">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="5fd64-218">Bağımlı olarak yanlış varlık seçilirse, bunu düzeltmek için akıcı API 'yi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fd64-218">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="5fd64-219">Akıcı API ile ilişkiyi yapılandırırken `HasOne` ve `WithOne` yöntemlerini kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="5fd64-219">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="5fd64-220">Yabancı anahtarı yapılandırırken, bağımlı varlık türünü belirtmeniz gerekir-aşağıdaki listede `HasForeignKey` için belirtilen genel parametreyi görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="5fd64-220">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="5fd64-221">Bire çok ilişkisinde, başvuru gezinmesi olan varlığın bağımlı olduğunu ve koleksiyonun asıl öğe olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="5fd64-221">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="5fd64-222">Ancak bu, bire bir ilişkide değildir, bu nedenle açıkça tanımlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-222">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a><span data-ttu-id="5fd64-223">Çok-çok</span><span class="sxs-lookup"><span data-stu-id="5fd64-223">Many-to-many</span></span>

<span data-ttu-id="5fd64-224">JOIN tablosunu temsil eden bir varlık sınıfı olmayan çoktan çoğa ilişkiler henüz desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="5fd64-224">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="5fd64-225">Bununla birlikte, JOIN tablosuna bir varlık sınıfı ekleyerek ve iki ayrı bire çok ilişkiyi eşleyerek çoktan çoğa ilişkiyi temsil edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fd64-225">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11-14,16-19,39-46)]
