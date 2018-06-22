---
title: İlişkileri - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054327"
---
# <a name="relationships"></a><span data-ttu-id="55ba7-102">İlişkileri</span><span class="sxs-lookup"><span data-stu-id="55ba7-102">Relationships</span></span>

<span data-ttu-id="55ba7-103">Bir ilişki nasıl iki varlık tanımlar birbirleri ile.</span><span class="sxs-lookup"><span data-stu-id="55ba7-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="55ba7-104">İlişkisel bir veritabanında bu bir yabancı anahtar kısıtlaması ile temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="55ba7-105">Bu makaledeki örnekler çoğunu bir-çok ilişkisi kavramları göstermek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="55ba7-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="55ba7-106">Bire bir ve çok-çok ilişkileri örnekleri görmek için [diğer ilişki desenleri](#other-relationship-patterns) makalenin sonunda bölüm.</span><span class="sxs-lookup"><span data-stu-id="55ba7-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="55ba7-107">Terimlerin tanımı</span><span class="sxs-lookup"><span data-stu-id="55ba7-107">Definition of Terms</span></span>

<span data-ttu-id="55ba7-108">Birkaç ilişkileri tanımlamak için kullanılan terimler vardır</span><span class="sxs-lookup"><span data-stu-id="55ba7-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="55ba7-109">**Bağımlı varlık:** bu yabancı anahtar özellik içeren bir varlıktır.</span><span class="sxs-lookup"><span data-stu-id="55ba7-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="55ba7-110">Bazen 'alt' ilişkisinin olarak da adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="55ba7-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="55ba7-111">**Asıl varlık:** bu birincil/alternatif temel özelliklerini içeren varlıktır.</span><span class="sxs-lookup"><span data-stu-id="55ba7-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="55ba7-112">Bazen 'parent' ilişkisinin da adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="55ba7-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="55ba7-113">**Yabancı anahtar:** bağımlı varlığındaki için ilgili varlık asıl anahtar özellik değerlerini depolamak için kullanılan özellik.</span><span class="sxs-lookup"><span data-stu-id="55ba7-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="55ba7-114">**Asıl Anahtar:** asıl varlık benzersiz olarak tanımlayan özellik.</span><span class="sxs-lookup"><span data-stu-id="55ba7-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="55ba7-115">Bu, birincil anahtar veya alternatif bir anahtarı olabilir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="55ba7-116">**Gezinti özelliği:** ilgili entity(s) alanlara içeren asıl ve/veya bağımlı varlık üzerinde tanımlanmış bir özellik.</span><span class="sxs-lookup"><span data-stu-id="55ba7-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="55ba7-117">**Koleksiyon gezinti özelliği:** birçok ilgili varlık başvuruları içeren bir gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="55ba7-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="55ba7-118">**Başvuru gezinti özelliği:** ilgili tek bir varlığa bir başvuru içeren bir gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="55ba7-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="55ba7-119">**Ters gezinti özelliği:** belirli gezinti özelliği açıklanırken, bu terimi ilişkinin diğer ucundaki gezinti özelliği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="55ba7-120">Aşağıdaki kod listesi arasında bir-çok ilişkisi gösterir `Blog` ve`Post`</span><span class="sxs-lookup"><span data-stu-id="55ba7-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="55ba7-121">`Post`bağımlı varlık</span><span class="sxs-lookup"><span data-stu-id="55ba7-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="55ba7-122">`Blog`Asıl varlığı</span><span class="sxs-lookup"><span data-stu-id="55ba7-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="55ba7-123">`Post.BlogId`yabancı anahtar</span><span class="sxs-lookup"><span data-stu-id="55ba7-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="55ba7-124">`Blog.BlogId`(Bu durumda, alternatif bir anahtarı yerine bir birincil anahtar olan) asıl anahtarı</span><span class="sxs-lookup"><span data-stu-id="55ba7-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="55ba7-125">`Post.Blog`bir başvuru Gezinti özelliğidir</span><span class="sxs-lookup"><span data-stu-id="55ba7-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="55ba7-126">`Blog.Posts`bir koleksiyon gezinti özelliği değil</span><span class="sxs-lookup"><span data-stu-id="55ba7-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="55ba7-127">`Post.Blog`Ters gezinti özelliği `Blog.Posts` (ve tersi)</span><span class="sxs-lookup"><span data-stu-id="55ba7-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="55ba7-128">Kurallar</span><span class="sxs-lookup"><span data-stu-id="55ba7-128">Conventions</span></span>

<span data-ttu-id="55ba7-129">Bir tür üzerinde bulunan bir gezinti özelliği olduğunda kurala göre bir ilişki oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="55ba7-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="55ba7-130">İşaret türü skaler bir tür geçerli veritabanı sağlayıcısı tarafından eşleştirilemez varsa bir özelliği bir gezinti özelliği olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="55ba7-131">Kural tarafından bulunan ilişkileri her zaman asıl varlığın birincil anahtarı hedefleyecektir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="55ba7-132">Alternatif bir anahtarı hedeflemek için ek yapılandırma Fluent API kullanılarak gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="55ba7-133">Tam olarak tanımlanmış ilişkileri</span><span class="sxs-lookup"><span data-stu-id="55ba7-133">Fully Defined Relationships</span></span>

<span data-ttu-id="55ba7-134">En yaygın düzeni ilişkiler için Gezinti özellikleri ilişkinin ve yabancı anahtar özelliği bağımlı varlık sınıfında tanımlanmış iki ucunda tanımlanmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="55ba7-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="55ba7-135">Sonra Gezinti özellikleri çifti arasında iki tür bulunursa, aynı ilişki ters Gezinti özellikleri olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="55ba7-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="55ba7-136">Bağımlı varlık adında bir özellik varsa `<primary key property name>`, `<navigation property name><primary key property name>`, veya `<principal entity name><primary key property name>` yabancı anahtar olarak yapılandırılacak sonra.</span><span class="sxs-lookup"><span data-stu-id="55ba7-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="55ba7-137">Birden çok gezinti özellikleri arasında iki tür tanımlı olup olmadığını (yani birbirlerine noktası gezintilerini birden fazla ayrı çiftinin), ardından hiçbir ilişki kurala göre oluşturulur ve bunları tanımlamak için el ile yapılandırmanız gerekecektir nasıl Gezinti özellikleri çifti ayarlama.</span><span class="sxs-lookup"><span data-stu-id="55ba7-137">If there are multiple navigation properties defined between two types (i.e. more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="55ba7-138">Yabancı anahtar özellik</span><span class="sxs-lookup"><span data-stu-id="55ba7-138">No Foreign Key Property</span></span>

<span data-ttu-id="55ba7-139">Yabancı anahtar özelliği bağımlı varlık sınıfında tanımlanmış önerilir, ancak gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="55ba7-140">Yabancı anahtar özellik bulunursa, bir gölge yabancı anahtar özellik adı ile görülecektir `<navigation property name><principal key property name>` (bkz [gölge özellikleri](shadow-properties.md) daha fazla bilgi için).</span><span class="sxs-lookup"><span data-stu-id="55ba7-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="55ba7-141">Tek gezinme özelliği</span><span class="sxs-lookup"><span data-stu-id="55ba7-141">Single Navigation Property</span></span>

<span data-ttu-id="55ba7-142">Tek bir gezinti özelliği (hiçbir ters gezinti ve yabancı anahtar özellik) dahil olmak üzere kurala göre tanımlanan bir ilişki için yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="55ba7-143">Tek bir gezinme özelliği ve yabancı anahtar özelliğine de sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="55ba7-144">Art arda silme</span><span class="sxs-lookup"><span data-stu-id="55ba7-144">Cascade Delete</span></span>

<span data-ttu-id="55ba7-145">Kurala göre art arda silme ayarlanacak *Cascade* gerekli ilişkiler ve *ClientSetNull* isteğe bağlı ilişkiler.</span><span class="sxs-lookup"><span data-stu-id="55ba7-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="55ba7-146">*CASCADE* bağımlı varlıkları da silinir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="55ba7-147">*ClientSetNull* belleğe yüklenmez bağımlı varlıkları kalacak anlamına gelir değişmeden ve el ile silinmiş veya gerekir geçerli bir asıl varlık işaret edecek şekilde güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="55ba7-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="55ba7-148">Belleğe yüklenen varlıklar için EF çekirdek yabancı anahtar özellikleri null olarak ayarlamanız dener.</span><span class="sxs-lookup"><span data-stu-id="55ba7-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="55ba7-149">Bkz: [gerekli ve isteğe bağlı ilişkileri](#required-and-optional-relationships) bölümü için gerekli ve isteğe bağlı ilişkileri arasındaki fark.</span><span class="sxs-lookup"><span data-stu-id="55ba7-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="55ba7-150">Bkz: [art arda silme](../saving/cascade-delete.md) farklı hakkında daha fazla ayrıntı davranışları ve kural tarafından kullanılan varsayılan silmek için.</span><span class="sxs-lookup"><span data-stu-id="55ba7-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="55ba7-151">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="55ba7-151">Data Annotations</span></span>

<span data-ttu-id="55ba7-152">İlişkiler yapılandırmak için kullanılan iki veri ek açıklamaları vardır `[ForeignKey]` ve `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="55ba7-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="55ba7-153">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="55ba7-153">[ForeignKey]</span></span>

<span data-ttu-id="55ba7-154">Hangi özelliği için belirtilen bir ilişki yabancı anahtar özelliği olarak kullanılmalıdır yapılandırmak için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55ba7-154">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="55ba7-155">Yabancı anahtar özelliği kurala göre bulunmayan olduğunda bu genellikle gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-155">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> <span data-ttu-id="55ba7-156">`[ForeignKey]` Ek açıklama ya da gezinti özelliği ilişkideki yerleştirilebilen.</span><span class="sxs-lookup"><span data-stu-id="55ba7-156">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="55ba7-157">Bağımlı varlık sınıfı Gezinti özelliğinde gitmek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="55ba7-157">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="55ba7-158">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="55ba7-158">[InverseProperty]</span></span>

<span data-ttu-id="55ba7-159">Veri ek açıklamaları nasıl bağımlı ve asıl varlık Gezinti özellikleri eşleştirin yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55ba7-159">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="55ba7-160">İki varlık türleri arasında gezinti özellikleri birden fazla Çifti olduğunda bu genellikle gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-160">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a><span data-ttu-id="55ba7-161">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="55ba7-161">Fluent API</span></span>

<span data-ttu-id="55ba7-162">Bir ilişki Fluent API'si yapılandırmak için ilişkisi olun Gezinti özellikleri belirleyerek başlatın.</span><span class="sxs-lookup"><span data-stu-id="55ba7-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="55ba7-163">`HasOne`veya `HasMany` , başlangıç yapılandırması üzerinde varlık türünün gezinti özelliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="55ba7-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="55ba7-164">Ardından bir çağrı zincir `WithOne` veya `WithMany` ters gezinti tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="55ba7-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="55ba7-165">`HasOne`/`WithOne`başvuru Gezinti özellikleri için kullanılır ve `HasMany` / `WithMany` koleksiyon Gezinti özellikleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="55ba7-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a><span data-ttu-id="55ba7-166">Tek gezinme özelliği</span><span class="sxs-lookup"><span data-stu-id="55ba7-166">Single Navigation Property</span></span>

<span data-ttu-id="55ba7-167">Yalnızca bir gezinti özelliğine sahip sonra parametresiz aşırı `WithOne` ve `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="55ba7-167">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="55ba7-168">Bu kavramsal olarak bir başvuru veya koleksiyon ilişkinin diğer ucundaki olmakla birlikte, varlık sınıfında dahil gezinti özelliği yok olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-168">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a><span data-ttu-id="55ba7-169">Yabancı anahtar</span><span class="sxs-lookup"><span data-stu-id="55ba7-169">Foreign Key</span></span>

<span data-ttu-id="55ba7-170">Hangi özelliği için belirtilen bir ilişki yabancı anahtar özelliği olarak kullanılmalıdır yapılandırmak için Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55ba7-170">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

<span data-ttu-id="55ba7-171">Aşağıdaki kod listesi nasıl birleşik yabancı anahtar yapılandırılacağını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-171">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

<span data-ttu-id="55ba7-172">Dize aşırı yüklemesini kullanabilirsiniz `HasForeignKey(...)` gölge özelliğini yabancı anahtar olarak yapılandırmak için (bkz [gölge özellikleri](shadow-properties.md) daha fazla bilgi için).</span><span class="sxs-lookup"><span data-stu-id="55ba7-172">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="55ba7-173">Yabancı anahtar olarak (aşağıda gösterildiği gibi) kullanmadan önce gölge özelliği modeline açıkça eklenmesi öneririz.</span><span class="sxs-lookup"><span data-stu-id="55ba7-173">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a><span data-ttu-id="55ba7-174">Asıl anahtar</span><span class="sxs-lookup"><span data-stu-id="55ba7-174">Principal Key</span></span>

<span data-ttu-id="55ba7-175">Birincil anahtar dışındaki bir özellik başvurmak için yabancı anahtarı istiyorsanız, asıl anahtar özelliği ilişki için yapılandırmak için Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55ba7-175">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="55ba7-176">Asıl Anahtar otomatik olarak yapılandırma özelliği tarafından Kurulum alternatif bir anahtar olarak (bkz [alternatif anahtarları](alternate-keys.md) daha fazla bilgi için).</span><span class="sxs-lookup"><span data-stu-id="55ba7-176">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

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

<span data-ttu-id="55ba7-177">Aşağıdaki kod listesi, bileşik bir asıl anahtar yapılandırma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-177">The following code listing shows how to configure a composite principal key.</span></span>

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
> <span data-ttu-id="55ba7-178">Asıl anahtar özelliklerini belirtmek için yabancı anahtar belirtilen sırada eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-178">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="55ba7-179">Gerekli ve isteğe bağlı ilişkileri</span><span class="sxs-lookup"><span data-stu-id="55ba7-179">Required and Optional Relationships</span></span>

<span data-ttu-id="55ba7-180">Fluent API ilişki gerekli veya isteğe bağlı olup olmadığını yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55ba7-180">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="55ba7-181">Sonuç olarak bu yabancı anahtar özellik gerekli veya isteğe bağlı olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="55ba7-181">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="55ba7-182">Gölge durumu yabancı anahtar kullanırken bu kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="55ba7-182">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="55ba7-183">Varlık sınıfınızda bir yabancı anahtar özelliğine sahip sonra requiredness ilişkinin yabancı anahtar özellik gerekli veya isteğe bağlı olarak belirlenir (bkz [gerekli ve isteğe bağlı özellikler](required-optional.md) daha fazla bilgi için bilgileri).</span><span class="sxs-lookup"><span data-stu-id="55ba7-183">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

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

### <a name="cascade-delete"></a><span data-ttu-id="55ba7-184">Art arda silme</span><span class="sxs-lookup"><span data-stu-id="55ba7-184">Cascade Delete</span></span>

<span data-ttu-id="55ba7-185">Fluent API açıkça belirtilen bir ilişki için cascade delete davranışı yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55ba7-185">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="55ba7-186">Bkz: [art arda silme](../saving/cascade-delete.md) her seçenek hakkında ayrıntılı bilgi için verileri kaydetme bölümünde.</span><span class="sxs-lookup"><span data-stu-id="55ba7-186">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

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

## <a name="other-relationship-patterns"></a><span data-ttu-id="55ba7-187">Diğer ilişki desenleri</span><span class="sxs-lookup"><span data-stu-id="55ba7-187">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="55ba7-188">Bire bir</span><span class="sxs-lookup"><span data-stu-id="55ba7-188">One-to-one</span></span>

<span data-ttu-id="55ba7-189">Bire bir ilişkiler iki tarafta bir başvuru gezinti özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="55ba7-189">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="55ba7-190">Bir-çok ilişkileri aynı kuralları izleyin, ancak benzersiz bir dizin, yalnızca bir bağımlı her asıl ilgili emin olmak için yabancı anahtar özellik sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="55ba7-190">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

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
> <span data-ttu-id="55ba7-191">EF yabancı anahtar özelliği algılamak için kendi yeteneği dayalı bağımlı olması için varlıkları birini seçeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="55ba7-191">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="55ba7-192">Yanlış varlık bağımlı seçilirse, bu sorunu gidermek için Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55ba7-192">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="55ba7-193">İlişki Fluent API'si ile yapılandırırken kullandığınız `HasOne` ve `WithOne` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="55ba7-193">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="55ba7-194">Bağımlı varlık türü - belirtmeniz gerekir yabancı anahtar yapılandırırken genel parametresi için sağlanan fark `HasForeignKey` listesinde.</span><span class="sxs-lookup"><span data-stu-id="55ba7-194">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="55ba7-195">Bir-çok ilişkisi içinde varlıkla başvuru Gezinti bağımlı ve koleksiyon ile bir asıl olduğunu işaretlenmemiştir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-195">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="55ba7-196">Ancak bu bire bir ilişkide - bu nedenle bunu açıkça tanımlamak için gereken.</span><span class="sxs-lookup"><span data-stu-id="55ba7-196">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

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

### <a name="many-to-many"></a><span data-ttu-id="55ba7-197">Çok-</span><span class="sxs-lookup"><span data-stu-id="55ba7-197">Many-to-many</span></span>

<span data-ttu-id="55ba7-198">Çok-çok ilişkileri birleşim tablosundan temsil etmek için bir varlık sınıfı olmadan henüz desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="55ba7-198">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="55ba7-199">Ancak, bir varlık sınıfı birleşim tablosundan ve iki ayrı bir-çok ilişkileri eşleme için ekleyerek bir çok-çok ilişkisi temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="55ba7-199">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

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
