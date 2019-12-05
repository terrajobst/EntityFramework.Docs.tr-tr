---
title: İlişkiler-EF Core
description: Entity Framework Core kullanırken varlık türleri arasında ilişkiler yapılandırma
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relationships
ms.openlocfilehash: 452169c902d56fda0a65a5c2846a9b42c80640fd
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824767"
---
# <a name="relationships"></a><span data-ttu-id="a8e8a-103">İlişkiler</span><span class="sxs-lookup"><span data-stu-id="a8e8a-103">Relationships</span></span>

<span data-ttu-id="a8e8a-104">Bir ilişki, iki varlığın birbirleriyle ilişkisini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-104">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="a8e8a-105">İlişkisel bir veritabanında bu, yabancı anahtar kısıtlaması tarafından temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-105">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="a8e8a-106">Bu makaledeki örneklerin çoğu, kavramları göstermek için bire çok ilişki kullanır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-106">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="a8e8a-107">Bire bir ve çoktan çoğa ilişkilerin örnekleri için makalenin sonundaki [diğer Ilişki desenleri](#other-relationship-patterns) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-107">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="a8e8a-108">Koşulların tanımı</span><span class="sxs-lookup"><span data-stu-id="a8e8a-108">Definition of terms</span></span>

<span data-ttu-id="a8e8a-109">İlişkileri anlatmak için kullanılan bazı terimler vardır</span><span class="sxs-lookup"><span data-stu-id="a8e8a-109">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="a8e8a-110">**Bağımlı varlık:** Bu, yabancı anahtar özelliklerini içeren varlıktır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-110">**Dependent entity:** This is the entity that contains the foreign key properties.</span></span> <span data-ttu-id="a8e8a-111">Bazen ilişkinin ' Child ' olarak da adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-111">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="a8e8a-112">**Asıl varlık:** Bu, birincil/alternatif anahtar özelliklerini içeren varlıktır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-112">**Principal entity:** This is the entity that contains the primary/alternate key properties.</span></span> <span data-ttu-id="a8e8a-113">Bazen ilişkinin ' Parent ' olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-113">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="a8e8a-114">**Yabancı anahtar:** İlgili varlık için asıl anahtar değerlerini depolamak için kullanılan bağımlı varlıktaki Özellikler.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-114">**Foreign key:** The properties in the dependent entity that are used to store the principal key values for the related entity.</span></span>

* <span data-ttu-id="a8e8a-115">**Asıl anahtar:** Asıl varlığı benzersiz bir şekilde tanımlayan Özellikler.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-115">**Principal key:** The properties that uniquely identify the principal entity.</span></span> <span data-ttu-id="a8e8a-116">Bu, birincil anahtar veya alternatif bir anahtar olabilir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-116">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="a8e8a-117">**Gezinti özelliği:** Sorumlu ve/veya bağımlı varlık üzerinde tanımlanan, ilgili varlığa başvuran bir özellik.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-117">**Navigation property:** A property defined on the principal and/or dependent entity that references the related entity.</span></span>

  * <span data-ttu-id="a8e8a-118">**Koleksiyon gezinti özelliği:** Birçok ilgili varlığa başvurular içeren bir gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-118">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="a8e8a-119">**Başvuru gezinti özelliği:** Tek bir ilgili varlığa başvuruyu tutan bir gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-119">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="a8e8a-120">**Ters gezinme özelliği:** Belirli bir gezinti özelliği tartışırken, bu terim ilişkinin diğer ucundaki gezinti özelliğine başvurur.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-120">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>
  
* <span data-ttu-id="a8e8a-121">**Kendine başvuran ilişki:** Bağımlı ve asıl varlık türlerinin aynı olduğu bir ilişki.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-121">**Self-referencing relationship:** A relationship in which the dependent and the principal entity types are the same.</span></span>

<span data-ttu-id="a8e8a-122">Aşağıdaki kod, `Blog` ve `Post` arasında bir-çok ilişkisi gösterir</span><span class="sxs-lookup"><span data-stu-id="a8e8a-122">The following code shows a one-to-many relationship between `Blog` and `Post`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

* <span data-ttu-id="a8e8a-123">bağımlı varlık `Post`</span><span class="sxs-lookup"><span data-stu-id="a8e8a-123">`Post` is the dependent entity</span></span>

* <span data-ttu-id="a8e8a-124">Asıl varlık `Blog`</span><span class="sxs-lookup"><span data-stu-id="a8e8a-124">`Blog` is the principal entity</span></span>

* <span data-ttu-id="a8e8a-125">yabancı anahtar `Post.BlogId`</span><span class="sxs-lookup"><span data-stu-id="a8e8a-125">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="a8e8a-126">`Blog.BlogId` asıl anahtardır (Bu durumda, alternatif bir anahtar yerine birincil anahtardır)</span><span class="sxs-lookup"><span data-stu-id="a8e8a-126">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="a8e8a-127">`Post.Blog` bir başvuru gezinti özelliğidir</span><span class="sxs-lookup"><span data-stu-id="a8e8a-127">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="a8e8a-128">bir koleksiyon gezinti özelliği `Blog.Posts`</span><span class="sxs-lookup"><span data-stu-id="a8e8a-128">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="a8e8a-129">`Post.Blog`, `Blog.Posts` ters gezinti özelliğidir (ve tam tersi)</span><span class="sxs-lookup"><span data-stu-id="a8e8a-129">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

## <a name="conventions"></a><span data-ttu-id="a8e8a-130">Kurallar</span><span class="sxs-lookup"><span data-stu-id="a8e8a-130">Conventions</span></span>

<span data-ttu-id="a8e8a-131">Varsayılan olarak, bir tür üzerinde bulunan bir gezinti özelliği olduğunda bir ilişki oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-131">By default, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="a8e8a-132">Bir özellik, işaret ettiği türün geçerli veritabanı sağlayıcısı tarafından skalar bir tür olarak eşleştirilemez olması halinde bir gezinti özelliği olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-132">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="a8e8a-133">Kural tarafından keşfedilen ilişkiler her zaman asıl varlığın birincil anahtarını hedefler.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-133">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="a8e8a-134">Alternatif bir anahtarı hedeflemek için, akıcı API kullanılarak ek yapılandırma gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-134">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="a8e8a-135">Tam olarak tanımlanmış ilişkiler</span><span class="sxs-lookup"><span data-stu-id="a8e8a-135">Fully defined relationships</span></span>

<span data-ttu-id="a8e8a-136">İlişkiler için en yaygın model, ilişkinin her iki ucunda ve bağımlı varlık sınıfında tanımlanmış yabancı anahtar özelliği için tanımlanmış gezinti özelliklerine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-136">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="a8e8a-137">İki tür arasında bir dizi gezinti özelliği bulunursa, bu, aynı ilişkinin ters gezinme özellikleri olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-137">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="a8e8a-138">Bağımlı varlık, bu modellerden birine sahip bir adı olan bir özellik içeriyorsa, yabancı anahtar olarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="a8e8a-138">If the dependent entity contains a property with a name mathing one of these patterns then it will be configured as the foreign key:</span></span>
  * `<navigation property name><principal key property name>`
  * `<navigation property name>Id`
  * `<principal entity name><principal key property name>`
  * `<principal entity name>Id`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

<span data-ttu-id="a8e8a-139">Bu örnekte, ilişkiyi yapılandırmak için vurgulanan Özellikler kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-139">In this example the highlighted properties will be used to configure the relationship.</span></span>

> [!NOTE]
> <span data-ttu-id="a8e8a-140">Özellik birincil anahtar ise veya asıl anahtarla uyumlu olmayan bir türde ise, yabancı anahtar olarak yapılandırılmaz.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-140">If the property is the primary key or is of a type not compatible with the principal key then it won't be configured as the foreign key.</span></span>

> [!NOTE]
> <span data-ttu-id="a8e8a-141">EF Core 3,0 önce, asıl anahtar özelliği ile tam olarak aynı adlı özellik [yabancı anahtar olarak da eşleştirildiği](https://github.com/aspnet/EntityFrameworkCore/issues/13274) için</span><span class="sxs-lookup"><span data-stu-id="a8e8a-141">Before EF Core 3.0 the property named exactly the same as the principal key property [was also matched as the foreign key](https://github.com/aspnet/EntityFrameworkCore/issues/13274)</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="a8e8a-142">Yabancı anahtar özelliği yok</span><span class="sxs-lookup"><span data-stu-id="a8e8a-142">No foreign key property</span></span>

<span data-ttu-id="a8e8a-143">Bağımlı varlık sınıfında tanımlanmış yabancı anahtar özelliği olması önerilse de, bu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-143">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="a8e8a-144">Yabancı anahtar özelliği bulunmazsa, bağımlı tür üzerinde hiçbir gezinti yoksa, `<navigation property name><principal key property name>` veya `<principal entity name><principal key property name>` bir [gölge yabancı anahtar özelliği](shadow-properties.md) tanıtılacaktır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-144">If no foreign key property is found, a [shadow foreign key property](shadow-properties.md) will be introduced with the name `<navigation property name><principal key property name>` or `<principal entity name><principal key property name>` if no navigation is present on the dependent type.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

<span data-ttu-id="a8e8a-145">Bu örnekte, ön bekleyen gezinti adının gereksiz olması nedeniyle gölge yabancı anahtarı `BlogId`.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-145">In this example the shadow foreign key is `BlogId` because prepending the navigation name would be redundant.</span></span>

> [!NOTE]
> <span data-ttu-id="a8e8a-146">Aynı ada sahip bir özellik zaten varsa, gölge Özellik adı bir sayıyla sonlenir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-146">If a property with the same name already exists then the shadow property name will be suffixed with a number.</span></span>

### <a name="single-navigation-property"></a><span data-ttu-id="a8e8a-147">Tek gezinti özelliği</span><span class="sxs-lookup"><span data-stu-id="a8e8a-147">Single navigation property</span></span>

<span data-ttu-id="a8e8a-148">Tek bir gezinti özelliği (ters gezinme olmadan ve yabancı anahtar özelliği olmadan) dahil olmak üzere, kural tarafından tanımlanan bir ilişkiye sahip olmak için yeterli değildir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-148">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="a8e8a-149">Tek bir gezinti özelliğine ve yabancı anahtar özelliğine de sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-149">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="limitations"></a><span data-ttu-id="a8e8a-150">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="a8e8a-150">Limitations</span></span>

<span data-ttu-id="a8e8a-151">İki tür arasında tanımlı birden çok gezinti özelliği olduğunda (diğer bir deyişle, birbirine işaret eden tek bir bir çift gezintiden daha fazla), gezinti özellikleriyle temsil edilen ilişkiler belirsizdir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-151">When there are multiple navigation properties defined between two types (that is, more than just one pair of navigations that point to each other) the relationships represented by the navigation properties are ambiguous.</span></span> <span data-ttu-id="a8e8a-152">Belirsizliği çözümlemek için bunları el ile yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-152">You will need to manually configure them to resolve the ambiguity.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="a8e8a-153">Art arda silme</span><span class="sxs-lookup"><span data-stu-id="a8e8a-153">Cascade delete</span></span>

<span data-ttu-id="a8e8a-154">Kurala göre, Cascade DELETE, isteğe bağlı ilişkiler için gerekli ilişkiler ve *Clientsetnull* için *Cascade* olarak ayarlanacak.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-154">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="a8e8a-155">*Cascade* , bağımlı varlıkların de silindiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-155">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="a8e8a-156">*Clientsetnull* , belleğe yüklenmeyen bağımlı varlıkların değişmeden kalacağından, el ile silinmesi ya da geçerli bir sorumlu varlığına işaret edecek şekilde güncelleştirilmeleri anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-156">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="a8e8a-157">Belleğe yüklenen varlıklar için EF Core yabancı anahtar özelliklerini null olarak ayarlamaya çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-157">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="a8e8a-158">Gerekli ve isteğe bağlı ilişkiler arasındaki fark için [gerekli ve Isteğe bağlı ilişkiler](#required-and-optional-relationships) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-158">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="a8e8a-159">Farklı silme davranışları ve kural tarafından kullanılan varsayılanlar hakkında daha fazla ayrıntı için bkz. [Cascade Delete](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="a8e8a-159">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="manual-configuration"></a><span data-ttu-id="a8e8a-160">Elle yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a8e8a-160">Manual configuration</span></span>

#### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="a8e8a-161">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="a8e8a-161">Fluent API</span></span>](#tab/fluent-api)

<span data-ttu-id="a8e8a-162">Akıcı API 'de bir ilişki yapılandırmak için, ilişkiyi oluşturan gezinti özelliklerini tanımlayarak başlacaksınız.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="a8e8a-163">`HasOne` veya `HasMany`, üzerinde yapılandırmaya başmakta olduğunuz varlık türünde gezinti özelliğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="a8e8a-164">Daha sonra ters gezintiyi belirlemek için `WithOne` veya `WithMany` bir çağrı zincirleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="a8e8a-165">`HasOne`/`WithOne` başvuru gezinti özellikleri için kullanılır ve koleksiyon gezinti özellikleri için `HasMany`/`WithMany` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

#### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="a8e8a-166">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="a8e8a-166">Data annotations</span></span>](#tab/data-annotations)

<span data-ttu-id="a8e8a-167">Bağımlı ve birincil varlıklarda gezinti özelliklerinin nasıl kullandığını yapılandırmak için veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-167">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="a8e8a-168">Bu, genellikle iki varlık türü arasında birden fazla gezinti özelliği olduğunda yapılır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-168">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

> [!NOTE]
> <span data-ttu-id="a8e8a-169">İlişkinin gerekilliğini etkilemek için yalnızca bağımlı varlıktaki özelliklerde [Required] kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-169">You can only use [Required] on properties on the dependent entity to impact the requiredness of the relationship.</span></span> <span data-ttu-id="a8e8a-170">[Gerekli] asıl varlıktaki gezinmede genellikle yok sayılır, ancak varlığın bağımlı bir duruma gelmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-170">[Required] on the navigation from the principal entity is usually ignored, but it may cause the entity to become the dependent one.</span></span>

> [!NOTE]
> <span data-ttu-id="a8e8a-171">`[ForeignKey]` ve `[InverseProperty]` veri ek açıklamaları `System.ComponentModel.DataAnnotations.Schema` ad alanında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-171">The data annotations `[ForeignKey]` and `[InverseProperty]` are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span> <span data-ttu-id="a8e8a-172">`[Required]`, `System.ComponentModel.DataAnnotations` ad alanında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-172">`[Required]` is available in the `System.ComponentModel.DataAnnotations` namespace.</span></span>

---

### <a name="single-navigation-property"></a><span data-ttu-id="a8e8a-173">Tek gezinti özelliği</span><span class="sxs-lookup"><span data-stu-id="a8e8a-173">Single navigation property</span></span>

<span data-ttu-id="a8e8a-174">Yalnızca bir gezinti özelliğine sahipseniz, `WithOne` ve `WithMany`Parametresiz aşırı yüklemeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-174">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="a8e8a-175">Bu, ilişkinin diğer ucunda kavramsal olarak bir başvuru veya koleksiyon olduğunu gösterir, ancak varlık sınıfında hiçbir gezinti özelliği dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-175">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="a8e8a-176">Yabancı anahtar</span><span class="sxs-lookup"><span data-stu-id="a8e8a-176">Foreign key</span></span>

#### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="a8e8a-177">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="a8e8a-177">Fluent API</span></span>](#tab/fluent-api)

<span data-ttu-id="a8e8a-178">Belirli bir ilişki için yabancı anahtar özelliği olarak kullanılması gereken özelliği yapılandırmak için Floent API 'yi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-178">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

#### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="a8e8a-179">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="a8e8a-179">Data annotations</span></span>](#tab/data-annotations)

<span data-ttu-id="a8e8a-180">Belirli bir ilişki için yabancı anahtar özelliği olarak kullanılması gereken özelliği yapılandırmak için veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-180">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="a8e8a-181">Bu, genellikle yabancı anahtar özelliği kural tarafından keşfedildiğinde yapılır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-181">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="a8e8a-182">`[ForeignKey]` ek açıklaması, ilişkide herhangi bir gezinti özelliğine eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-182">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="a8e8a-183">Bağımlı varlık sınıfında gezinti özelliğine gitmesinin gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-183">It does not need to go on the navigation property in the dependent entity class.</span></span>

> [!NOTE]
> <span data-ttu-id="a8e8a-184">Gezinti özelliğinde `[ForeignKey]` kullanılarak belirtilen özelliğin bağımlı tür olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-184">The property specified using `[ForeignKey]` on a navigation property doesn't need to exist the the dependent type.</span></span> <span data-ttu-id="a8e8a-185">Bu durumda, belirtilen ad bir gölge yabancı anahtar oluşturmak için kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-185">In this case the specified name will be used to create a shadow foreign key.</span></span>

---

<span data-ttu-id="a8e8a-186">Aşağıdaki kod, bileşik yabancı anahtarın nasıl yapılandırılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-186">The following code shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="a8e8a-187">Bir gölge özelliği yabancı anahtar olarak yapılandırmak için `HasForeignKey(...)` dize aşırı yüklemesini kullanabilirsiniz (daha fazla bilgi için bkz. [gölge özellikleri](shadow-properties.md) ).</span><span class="sxs-lookup"><span data-stu-id="a8e8a-187">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="a8e8a-188">Gölge özelliğini, yabancı anahtar olarak kullanmadan önce modele açıkça eklememiz önerilir (aşağıda gösterildiği gibi).</span><span class="sxs-lookup"><span data-stu-id="a8e8a-188">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="a8e8a-189">Gezinti özelliği olmadan</span><span class="sxs-lookup"><span data-stu-id="a8e8a-189">Without navigation property</span></span>

<span data-ttu-id="a8e8a-190">Bir gezinti özelliği sağlamanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-190">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="a8e8a-191">İlişkinin yalnızca bir tarafında yabancı anahtar sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-191">You can simply provide a foreign key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="a8e8a-192">Asıl anahtar</span><span class="sxs-lookup"><span data-stu-id="a8e8a-192">Principal key</span></span>

<span data-ttu-id="a8e8a-193">Yabancı anahtarın birincil anahtar dışında bir özelliğe başvurmasına isterseniz, ilişkinin asıl anahtar özelliğini yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-193">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="a8e8a-194">Asıl anahtar olarak yapılandırdığınız özellik otomatik olarak [alternatif bir anahtar](alternate-keys.md)olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-194">The property that you configure as the principal key will automatically be setup as an [alternate key](alternate-keys.md).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

<span data-ttu-id="a8e8a-195">Aşağıdaki kod, bileşik bir asıl anahtarın nasıl yapılandırılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-195">The following code shows how to configure a composite principal key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> <span data-ttu-id="a8e8a-196">Asıl anahtar özelliklerini belirttiğiniz sıra, yabancı anahtar için belirtildikleri sırayla eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-196">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="a8e8a-197">Gerekli ve isteğe bağlı ilişkiler</span><span class="sxs-lookup"><span data-stu-id="a8e8a-197">Required and optional relationships</span></span>

<span data-ttu-id="a8e8a-198">İlişkinin gerekli veya isteğe bağlı olup olmadığını yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-198">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="a8e8a-199">Sonuç olarak bu, yabancı anahtar özelliğinin gerekli veya isteğe bağlı olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-199">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="a8e8a-200">Bu, büyük bir gölge durumu yabancı anahtar kullanırken faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-200">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="a8e8a-201">Varlık sınıfınızda yabancı anahtar özelliği varsa, ilişkinin gereklik durumu yabancı anahtar özelliğinin gerekli veya isteğe bağlı olup olmadığına göre belirlenir (daha fazla bilgi için bkz. [gerekli ve Isteğe bağlı özellikler](required-optional.md) ).</span><span class="sxs-lookup"><span data-stu-id="a8e8a-201">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

> [!NOTE]
> <span data-ttu-id="a8e8a-202">`IsRequired(false)` çağrısı, aksi belirtilmedikçe yabancı anahtar özelliğini de isteğe bağlı hale getirir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-202">Calling `IsRequired(false)` also makes the foreign key property optional unless it's configured otherwise.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="a8e8a-203">Art arda silme</span><span class="sxs-lookup"><span data-stu-id="a8e8a-203">Cascade delete</span></span>

<span data-ttu-id="a8e8a-204">Belirli bir ilişki için basamaklı silme davranışını açıkça yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-204">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="a8e8a-205">Her seçeneğe ilişkin ayrıntılı bir tartışma için bkz. [Cascade Delete](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="a8e8a-205">See [Cascade Delete](../saving/cascade-delete.md) for a detailed discussion of each option.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a><span data-ttu-id="a8e8a-206">Diğer ilişki desenleri</span><span class="sxs-lookup"><span data-stu-id="a8e8a-206">Other relationship patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="a8e8a-207">Bir-bir</span><span class="sxs-lookup"><span data-stu-id="a8e8a-207">One-to-one</span></span>

<span data-ttu-id="a8e8a-208">Bir ilişkinin her iki tarafında da başvuru gezintisi özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-208">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="a8e8a-209">Tek-çok ilişkilerle aynı kurallara uyar, ancak her sorumluyla yalnızca bir bağımlı ilişki olduğundan emin olmak için yabancı anahtar özelliğinde benzersiz bir dizin sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-209">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> <span data-ttu-id="a8e8a-210">EF, bir yabancı anahtar özelliği algılama özelliğine göre bağımlı olacak varlıklardan birini seçer.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-210">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="a8e8a-211">Bağımlı olarak yanlış varlık seçilirse, bunu düzeltmek için akıcı API 'yi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-211">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="a8e8a-212">Akıcı API ile ilişkiyi yapılandırırken `HasOne` ve `WithOne` yöntemlerini kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-212">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="a8e8a-213">Yabancı anahtarı yapılandırırken, bağımlı varlık türünü belirtmeniz gerekir-aşağıdaki listede `HasForeignKey` için belirtilen genel parametreyi görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-213">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="a8e8a-214">Bire çok ilişkisinde, başvuru gezinmesi olan varlığın bağımlı olduğunu ve koleksiyonun asıl öğe olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-214">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="a8e8a-215">Ancak bu, bire bir ilişkide değildir, bu nedenle açıkça tanımlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-215">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a><span data-ttu-id="a8e8a-216">Çok-çok</span><span class="sxs-lookup"><span data-stu-id="a8e8a-216">Many-to-many</span></span>

<span data-ttu-id="a8e8a-217">JOIN tablosunu temsil eden bir varlık sınıfı olmayan çoktan çoğa ilişkiler henüz desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-217">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="a8e8a-218">Bununla birlikte, JOIN tablosuna bir varlık sınıfı ekleyerek ve iki ayrı bire çok ilişkiyi eşleyerek çoktan çoğa ilişkiyi temsil edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8e8a-218">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
