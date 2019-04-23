---
title: İlişkiler - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 9ef1a9269fc99f5b27a81c11a161ed5f9d74180d
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929943"
---
# <a name="relationships"></a><span data-ttu-id="77e32-102">İlişkiler</span><span class="sxs-lookup"><span data-stu-id="77e32-102">Relationships</span></span>

<span data-ttu-id="77e32-103">Bir ilişki nasıl iki varlıklar tanımlayan birbirleriyle.</span><span class="sxs-lookup"><span data-stu-id="77e32-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="77e32-104">İlişkisel bir veritabanında bu bir yabancı anahtar kısıtlaması tarafından temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="77e32-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="77e32-105">Bu makaledeki örnekleri çoğu bire çok ilişkisi kavramları göstermek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="77e32-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="77e32-106">Bire bir veya çoktan çoğa ilişkilerini örneklerini görmek için [diğer ilişki desenleri](#other-relationship-patterns) makalenin sonunda bölüm.</span><span class="sxs-lookup"><span data-stu-id="77e32-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="77e32-107">Koşulları tanımı</span><span class="sxs-lookup"><span data-stu-id="77e32-107">Definition of Terms</span></span>

<span data-ttu-id="77e32-108">Bir dizi ilişkileri tanımlamak için kullanılan terimler vardır.</span><span class="sxs-lookup"><span data-stu-id="77e32-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="77e32-109">**Bağımlı varlık:** Yabancı anahtar özellik içeren varlık budur.</span><span class="sxs-lookup"><span data-stu-id="77e32-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="77e32-110">Bazen 'alt' ilişkisinin olarak da adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="77e32-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="77e32-111">**Asıl varlık:** Birincil/diğer temel özelliklerini içeren varlık budur.</span><span class="sxs-lookup"><span data-stu-id="77e32-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="77e32-112">Bazen 'parent' ilişkisinin da adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="77e32-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="77e32-113">**Yabancı anahtar:** Bağımlı varlığındaki varlık ilgili asıl anahtar özellik değerlerini depolamak için kullanılan özellik.</span><span class="sxs-lookup"><span data-stu-id="77e32-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="77e32-114">**Birincil anahtarı:** Asıl varlığı benzersiz olarak tanımlayan özellik.</span><span class="sxs-lookup"><span data-stu-id="77e32-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="77e32-115">Bu, birincil anahtar veya alternatif anahtar olabilir.</span><span class="sxs-lookup"><span data-stu-id="77e32-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="77e32-116">**Gezinti özelliği:** Bir ilgili entity(s) başvuruları içeren asıl ve/veya bağımlı varlık üzerinde bir özelliği tanımlı.</span><span class="sxs-lookup"><span data-stu-id="77e32-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="77e32-117">**Koleksiyon gezinme özelliği:** Birçok ilgili varlıklara başvurular içeren bir gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="77e32-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="77e32-118">**Başvuru gezinti özelliği:** Tek bir ilgili varlığa bir başvuru tutan bir gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="77e32-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="77e32-119">**Ters gezinti özelliği:** Belirli bir gezinti özelliği ele alırken, bu terim diğer ucundaki ilişkinin gezinme özelliğini ifade eder.</span><span class="sxs-lookup"><span data-stu-id="77e32-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="77e32-120">Aşağıdaki kod listesi arasında bir-çok ilişkisi gösterilir `Blog` ve `Post`</span><span class="sxs-lookup"><span data-stu-id="77e32-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="77e32-121">`Post` bağımlı bir varlık</span><span class="sxs-lookup"><span data-stu-id="77e32-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="77e32-122">`Blog` Asıl varlığı</span><span class="sxs-lookup"><span data-stu-id="77e32-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="77e32-123">`Post.BlogId` yabancı anahtar</span><span class="sxs-lookup"><span data-stu-id="77e32-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="77e32-124">`Blog.BlogId` (Bu durumda alternatif anahtar yerine birincil anahtar olduğu) birincil anahtar</span><span class="sxs-lookup"><span data-stu-id="77e32-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="77e32-125">`Post.Blog` bir başvuru gezinti özelliği</span><span class="sxs-lookup"><span data-stu-id="77e32-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="77e32-126">`Blog.Posts` bir koleksiyon gezinme özelliği</span><span class="sxs-lookup"><span data-stu-id="77e32-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="77e32-127">`Post.Blog` Ters gezinti özelliği `Blog.Posts` (ve tersi)</span><span class="sxs-lookup"><span data-stu-id="77e32-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="77e32-128">Kurallar</span><span class="sxs-lookup"><span data-stu-id="77e32-128">Conventions</span></span>

<span data-ttu-id="77e32-129">Bir tür üzerinde bulunan bir gezinti özelliği olduğunda, kural olarak, bir ilişki oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="77e32-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="77e32-130">İşaret türü geçerli bir veritabanı sağlayıcısı tarafından skaler bir tür olarak eşleştirilemez bir özelliği bir gezinti özelliği kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="77e32-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="77e32-131">Kural gereği bulunan ilişkiler her zaman asıl varlığın birincil anahtarı hedefleyecektir.</span><span class="sxs-lookup"><span data-stu-id="77e32-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="77e32-132">Alternatif anahtar hedeflemek için ek yapılandırma Fluent API'sini kullanarak gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="77e32-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="77e32-133">Tam olarak tanımlanan ilişkileri</span><span class="sxs-lookup"><span data-stu-id="77e32-133">Fully Defined Relationships</span></span>

<span data-ttu-id="77e32-134">İlişkiler için en yaygın desen, her iki ucunda da ilişki ve bağımlı varlık sınıfı içinde tanımlanan bir yabancı anahtar özellik tanımlanan Gezinti özelliklerini sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="77e32-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="77e32-135">Ardından, gezinti özellikleri bir çift iki tür arasında bulunursa, aynı ilişki ters Gezinti özellikleri olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="77e32-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="77e32-136">Bağımlı varlık adlı bir özellik varsa `<primary key property name>`, `<navigation property name><primary key property name>`, veya `<principal entity name><primary key property name>` havuzla yabancı anahtar olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="77e32-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="77e32-137">İki tür arasında tanımlanan birden çok gezinti özellikleri olup olmadığını (diğer bir deyişle, birden fazla ayrı çiftinin birbirine noktası gezintiler), hiçbir ilişki kural tarafından oluşturulur ve bunları tanımlamak için el ile yapılandırmanız gerekecektir nasıl Gezinti özellikleri çifti ayarlama.</span><span class="sxs-lookup"><span data-stu-id="77e32-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="77e32-138">Yabancı anahtar özelliği yok</span><span class="sxs-lookup"><span data-stu-id="77e32-138">No Foreign Key Property</span></span>

<span data-ttu-id="77e32-139">Bağımlı varlık sınıfı içinde tanımlanan bir yabancı anahtarı özelliği olması önerilir, ancak gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="77e32-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="77e32-140">Yabancı anahtar özellik bulunursa, bir gölge yabancı anahtar özellik adıyla tanıtılmak `<navigation property name><principal key property name>` (bkz [gölge Özellikler](shadow-properties.md) daha fazla bilgi için).</span><span class="sxs-lookup"><span data-stu-id="77e32-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="77e32-141">Gezinme özelliği</span><span class="sxs-lookup"><span data-stu-id="77e32-141">Single Navigation Property</span></span>

<span data-ttu-id="77e32-142">Tek bir gezinti özelliği (ters hiçbir gezinti ve herhangi bir yabancı anahtar özelliği) dahil olmak üzere kural olarak tanımlanmış bir ilişkisi olması yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="77e32-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="77e32-143">Bir gezinme özelliği ve yabancı anahtar özelliğine de sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="77e32-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="77e32-144">Art arda silme</span><span class="sxs-lookup"><span data-stu-id="77e32-144">Cascade Delete</span></span>

<span data-ttu-id="77e32-145">Kural gereği, art arda silme ayarlanacak *Cascade* gerekli ilişkileri ve *ClientSetNull* isteğe bağlı ilişkiler için.</span><span class="sxs-lookup"><span data-stu-id="77e32-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="77e32-146">*Art arda* bağımlı varlıklar da silinir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="77e32-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="77e32-147">*ClientSetNull* belleğe yüklenmeyen bağımlı varlıkları kalacağı anlamına gelir değişmeden ve gereken el ile silinmesi veya geçerli bir asıl varlığa işaret edecek şekilde güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="77e32-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="77e32-148">Belleğe yüklenen varlıklar için yabancı anahtar özellikleri null olarak ayarlamak EF Core dener.</span><span class="sxs-lookup"><span data-stu-id="77e32-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="77e32-149">Bkz: [gerekli ve isteğe bağlı ilişkileri](#required-and-optional-relationships) gerekli ve isteğe bağlı ilişkileri arasındaki fark için bölüm.</span><span class="sxs-lookup"><span data-stu-id="77e32-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="77e32-150">Bkz: [art arda silme](../saving/cascade-delete.md) davranışları ve kuralı tarafından kullanılan varsayılan değerleri farklı hakkında daha fazla ayrıntı silin.</span><span class="sxs-lookup"><span data-stu-id="77e32-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="77e32-151">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="77e32-151">Data Annotations</span></span>

<span data-ttu-id="77e32-152">İlişkiler yapılandırmak için kullanılan iki veri ek açıklamaları vardır `[ForeignKey]` ve `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="77e32-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="77e32-153">Bunlar, kullanılabilir `System.ComponentModel.DataAnnotations.Schema` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="77e32-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="77e32-154">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="77e32-154">[ForeignKey]</span></span>

<span data-ttu-id="77e32-155">Veri ek açıklamaları belirli bir ilişki için yabancı anahtar özellik olarak hangi özelliğinin kullanılması gerektiğini yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="77e32-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="77e32-156">Kural gereği yabancı anahtar özelliği bulunmayan olduğunda bu genellikle gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="77e32-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="77e32-157">`[ForeignKey]` Ek açıklama ya da gezinti özelliğindeki ilişkisinde yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="77e32-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="77e32-158">Gezinti özelliğindeki bağımlı varlık sınıfı içinde Git gerekmez.</span><span class="sxs-lookup"><span data-stu-id="77e32-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="77e32-159">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="77e32-159">[InverseProperty]</span></span>

<span data-ttu-id="77e32-160">Veri ek açıklamaları, bağımlı ve asıl varlık Gezinti özellikleri nasıl pair yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="77e32-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="77e32-161">Bu, genellikle birden fazla Çifti iki varlık türleri arasında gezinti özellikleri olduğunda gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="77e32-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="77e32-162">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="77e32-162">Fluent API</span></span>

<span data-ttu-id="77e32-163">Bir ilişki Fluent API'si yapılandırmak için ilişkiyi gezinme özelliklerini belirleyerek başlayın.</span><span class="sxs-lookup"><span data-stu-id="77e32-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="77e32-164">`HasOne` veya `HasMany` , başlangıç yapılandırması üzerinde varlık türünün gezinme özelliğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77e32-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="77e32-165">Ardından bir çağrı zinciri `WithOne` veya `WithMany` ters gezinti tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="77e32-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="77e32-166">`HasOne`/`WithOne` başvuru Gezinti özellikleri için kullanılır ve `HasMany` / `WithMany` koleksiyon Gezinti özellikleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="77e32-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="77e32-167">Gezinme özelliği</span><span class="sxs-lookup"><span data-stu-id="77e32-167">Single Navigation Property</span></span>

<span data-ttu-id="77e32-168">Yalnızca sahip olduğunuz bir gezinti özelliği sonra parametresiz aşırı yükleme `WithOne` ve `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="77e32-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="77e32-169">Bu, kavramsal olarak bir başvuru ya da koleksiyon ilişkinin diğer ucundaki olmakla birlikte, varlık sınıfı dahil bir gezinti özelliği yok, gösterir.</span><span class="sxs-lookup"><span data-stu-id="77e32-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="77e32-170">Yabancı anahtar</span><span class="sxs-lookup"><span data-stu-id="77e32-170">Foreign Key</span></span>

<span data-ttu-id="77e32-171">Hangi özelliği için belirtilen bir ilişki yabancı anahtar özellik olarak kullanılmalıdır yapılandırmak için Fluent API'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="77e32-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="77e32-172">Aşağıdaki kod listesi, bileşik bir yabancı anahtar yapılandırma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="77e32-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="77e32-173">Dize aşırı yüklemesini kullanabilirsiniz `HasForeignKey(...)` yabancı anahtar olarak bir gölge özelliğini yapılandırmak için (bkz [gölge Özellikler](shadow-properties.md) daha fazla bilgi için).</span><span class="sxs-lookup"><span data-stu-id="77e32-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="77e32-174">Yabancı anahtar olarak (aşağıda gösterildiği gibi) kullanmadan önce gölge özellik modele açıkça eklemenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="77e32-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a><span data-ttu-id="77e32-175">Sorumlusu anahtarı</span><span class="sxs-lookup"><span data-stu-id="77e32-175">Principal Key</span></span>

<span data-ttu-id="77e32-176">Birincil anahtarı dışında bir özellik başvurmak için yabancı anahtarı istiyorsanız, birincil anahtar özelliği ilişki için yapılandırmak için Fluent API'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="77e32-176">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="77e32-177">Asıl Anahtar otomatik olarak yapılandırma özelliği alternatif anahtar olarak Kurulum olabilir (bakın [alternatif anahtarlar](alternate-keys.md) daha fazla bilgi için).</span><span class="sxs-lookup"><span data-stu-id="77e32-177">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

<span data-ttu-id="77e32-178">Aşağıdaki kod listesi, bileşik bir birincil anahtar yapılandırma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="77e32-178">The following code listing shows how to configure a composite principal key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> <span data-ttu-id="77e32-179">Birincil anahtar özellikleri belirttiğiniz sırada yabancı anahtar için belirtilen sırayla eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="77e32-179">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="77e32-180">Gerekli ve isteğe bağlı bir ilişki</span><span class="sxs-lookup"><span data-stu-id="77e32-180">Required and Optional Relationships</span></span>

<span data-ttu-id="77e32-181">İlişkiyi gerekli veya isteğe bağlı olup olmadığını yapılandırmak için Fluent API'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="77e32-181">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="77e32-182">Sonuç olarak bu yabancı anahtar özellik gerekli veya isteğe bağlı olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="77e32-182">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="77e32-183">Bu, bir gölge durum yabancı anahtarı kullanılırken en kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="77e32-183">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="77e32-184">Varlık Sınıfınız içinde bir yabancı anahtar özelliğine sahip sonra requiredness ilişkinin yabancı anahtar özellik gerekli veya isteğe bağlı olarak belirlenir (bkz [gerekli ve isteğe bağlı özellikler](required-optional.md) daha fazla bilgi için bilgiler).</span><span class="sxs-lookup"><span data-stu-id="77e32-184">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a><span data-ttu-id="77e32-185">Art arda silme</span><span class="sxs-lookup"><span data-stu-id="77e32-185">Cascade Delete</span></span>

<span data-ttu-id="77e32-186">Fluent API'si, açıkça belirtilen bir ilişki art arda silme davranışını yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="77e32-186">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="77e32-187">Bkz: [art arda silme](../saving/cascade-delete.md) verileri kaydetme bölümünde ayrıntılı bir irdelemesi ve her seçeneği.</span><span class="sxs-lookup"><span data-stu-id="77e32-187">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a><span data-ttu-id="77e32-188">Diğer ilişki desenleri</span><span class="sxs-lookup"><span data-stu-id="77e32-188">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="77e32-189">Bire bir</span><span class="sxs-lookup"><span data-stu-id="77e32-189">One-to-one</span></span>

<span data-ttu-id="77e32-190">Bire bir ilişkiler, her iki kenarı da bir başvuru Gezinti özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="77e32-190">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="77e32-191">Bire çok ilişkileri aynı kuralları takip etmeleri ancak benzersiz bir dizin üzerinde yalnızca bir bağımlı her asıl ilgili emin olmak için yabancı anahtar özellik sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="77e32-191">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> <span data-ttu-id="77e32-192">EF bir yabancı anahtar özellik algılamak için yeteneklerine bağlı bağımlı olmasını varlıkları birini seçin.</span><span class="sxs-lookup"><span data-stu-id="77e32-192">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="77e32-193">Yanlış varlık bağlı belirtildiyse, bu sorunu gidermek için Fluent API'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="77e32-193">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="77e32-194">İlişki Fluent API'si ile yapılandırırken kullandığınız `HasOne` ve `WithOne` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="77e32-194">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="77e32-195">Sağlanan genel parametre - bağımlı varlık türünü belirtmek için yabancı anahtarı yapılandırırken fark `HasForeignKey` listesinde.</span><span class="sxs-lookup"><span data-stu-id="77e32-195">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="77e32-196">Bir-çok ilişkisine başvuru Gezinti varlığa bağlıdır ve koleksiyon ile bir sorumlu olduğunu işaretlenmemiştir.</span><span class="sxs-lookup"><span data-stu-id="77e32-196">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="77e32-197">Ancak bu bire bir ilişki - bu nedenle, bu nedenle açıkça tanımlamak için gerek.</span><span class="sxs-lookup"><span data-stu-id="77e32-197">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a><span data-ttu-id="77e32-198">Çok-çok</span><span class="sxs-lookup"><span data-stu-id="77e32-198">Many-to-many</span></span>

<span data-ttu-id="77e32-199">Çoktan çoğa ilişkiler birleştirme tablosunu temsil edecek bir varlık sınıfı olmadan henüz desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="77e32-199">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="77e32-200">Ancak, birleşim tablosu ve iki ayrı bir-çok ilişkileri eşleme için bir varlık sınıfı dahil ederek bir çoktan çoğa ilişki temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="77e32-200">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
