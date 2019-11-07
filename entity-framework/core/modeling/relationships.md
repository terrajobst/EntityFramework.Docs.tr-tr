---
title: İlişkiler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e59ce9e19c12aa5564bc8467dcfcb3be8ee8996
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655670"
---
# <a name="relationships"></a><span data-ttu-id="02185-102">İlişkiler</span><span class="sxs-lookup"><span data-stu-id="02185-102">Relationships</span></span>

<span data-ttu-id="02185-103">Bir ilişki, iki varlığın birbirleriyle ilişkisini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="02185-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="02185-104">İlişkisel bir veritabanında bu, yabancı anahtar kısıtlaması tarafından temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="02185-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="02185-105">Bu makaledeki örneklerin çoğu, kavramları göstermek için bire çok ilişki kullanır.</span><span class="sxs-lookup"><span data-stu-id="02185-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="02185-106">Bire bir ve çoktan çoğa ilişkilerin örnekleri için makalenin sonundaki [diğer Ilişki desenleri](#other-relationship-patterns) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="02185-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="02185-107">Koşulların tanımı</span><span class="sxs-lookup"><span data-stu-id="02185-107">Definition of Terms</span></span>

<span data-ttu-id="02185-108">İlişkileri anlatmak için kullanılan bazı terimler vardır</span><span class="sxs-lookup"><span data-stu-id="02185-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="02185-109">**Bağımlı varlık:** Bu, yabancı anahtar özelliğini içeren varlıktır.</span><span class="sxs-lookup"><span data-stu-id="02185-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="02185-110">Bazen ilişkinin ' Child ' olarak da adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="02185-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="02185-111">**Asıl varlık:** Bu, birincil/alternatif anahtar Özellik (ler) i içeren varlıktır.</span><span class="sxs-lookup"><span data-stu-id="02185-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="02185-112">Bazen ilişkinin ' Parent ' olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="02185-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="02185-113">**Yabancı anahtar:** Bağımlı varlıktaki, varlığın ilişkili olduğu asıl anahtar özelliğinin değerlerini depolamak için kullanılan Özellik (ler).</span><span class="sxs-lookup"><span data-stu-id="02185-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="02185-114">**Asıl anahtar:** Asıl varlığı benzersiz bir şekilde tanımlayan Özellik (ler).</span><span class="sxs-lookup"><span data-stu-id="02185-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="02185-115">Bu, birincil anahtar veya alternatif bir anahtar olabilir.</span><span class="sxs-lookup"><span data-stu-id="02185-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="02185-116">**Gezinti özelliği:** Sorumlu ve/veya bağımlı varlık üzerinde tanımlanan, ilişkili varlıklara başvuru içeren bir özellik.</span><span class="sxs-lookup"><span data-stu-id="02185-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="02185-117">**Koleksiyon gezinti özelliği:** Birçok ilgili varlığa başvurular içeren bir gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="02185-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="02185-118">**Başvuru gezinti özelliği:** Tek bir ilgili varlığa başvuruyu tutan bir gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="02185-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="02185-119">**Ters gezinme özelliği:** Belirli bir gezinti özelliği tartışırken, bu terim ilişkinin diğer ucundaki gezinti özelliğine başvurur.</span><span class="sxs-lookup"><span data-stu-id="02185-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="02185-120">Aşağıdaki kod listesi `Blog` ve `Post` arasında bir-çok ilişkisi gösterir</span><span class="sxs-lookup"><span data-stu-id="02185-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="02185-121">bağımlı varlık `Post`</span><span class="sxs-lookup"><span data-stu-id="02185-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="02185-122">Asıl varlık `Blog`</span><span class="sxs-lookup"><span data-stu-id="02185-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="02185-123">yabancı anahtar `Post.BlogId`</span><span class="sxs-lookup"><span data-stu-id="02185-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="02185-124">`Blog.BlogId` asıl anahtardır (Bu durumda, alternatif bir anahtar yerine birincil anahtardır)</span><span class="sxs-lookup"><span data-stu-id="02185-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="02185-125">`Post.Blog` bir başvuru gezinti özelliğidir</span><span class="sxs-lookup"><span data-stu-id="02185-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="02185-126">bir koleksiyon gezinti özelliği `Blog.Posts`</span><span class="sxs-lookup"><span data-stu-id="02185-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="02185-127">`Post.Blog`, `Blog.Posts` ters gezinti özelliğidir (ve tam tersi)</span><span class="sxs-lookup"><span data-stu-id="02185-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="02185-128">Kurallar</span><span class="sxs-lookup"><span data-stu-id="02185-128">Conventions</span></span>

<span data-ttu-id="02185-129">Kurala göre, bir tür üzerinde bulunan bir gezinti özelliği olduğunda bir ilişki oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="02185-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="02185-130">Bir özellik, işaret ettiği türün geçerli veritabanı sağlayıcısı tarafından skalar bir tür olarak eşleştirilemez olması halinde bir gezinti özelliği olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="02185-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="02185-131">Kural tarafından keşfedilen ilişkiler her zaman asıl varlığın birincil anahtarını hedefler.</span><span class="sxs-lookup"><span data-stu-id="02185-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="02185-132">Alternatif bir anahtarı hedeflemek için, akıcı API kullanılarak ek yapılandırma gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="02185-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="02185-133">Tam olarak tanımlanmış Ilişkiler</span><span class="sxs-lookup"><span data-stu-id="02185-133">Fully Defined Relationships</span></span>

<span data-ttu-id="02185-134">İlişkiler için en yaygın model, ilişkinin her iki ucunda ve bağımlı varlık sınıfında tanımlanmış yabancı anahtar özelliği için tanımlanmış gezinti özelliklerine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="02185-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="02185-135">İki tür arasında bir dizi gezinti özelliği bulunursa, bu, aynı ilişkinin ters gezinme özellikleri olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="02185-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="02185-136">Bağımlı varlık `<primary key property name>`, `<navigation property name><primary key property name>`veya `<principal entity name><primary key property name>` adlı bir özellik içeriyorsa, yabancı anahtar olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="02185-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="02185-137">İki tür arasında tanımlanmış birden çok gezinti özelliği varsa (diğer bir deyişle, birbirine işaret eden birden çok farklı gezginler çifti), kurala göre hiçbir ilişki oluşturulmaz ve Gezinti özellikleri eşleştirin.</span><span class="sxs-lookup"><span data-stu-id="02185-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="02185-138">Yabancı anahtar özelliği yok</span><span class="sxs-lookup"><span data-stu-id="02185-138">No Foreign Key Property</span></span>

<span data-ttu-id="02185-139">Bağımlı varlık sınıfında tanımlanmış yabancı anahtar özelliği olması önerilse de, bu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="02185-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="02185-140">Yabancı anahtar özelliği bulunmazsa, `<navigation property name><principal key property name>` adıyla bir gölge yabancı anahtar özelliği tanıtılacaktır (daha fazla bilgi için bkz. [Gölge özelliklerine](shadow-properties.md) bakın).</span><span class="sxs-lookup"><span data-stu-id="02185-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="02185-141">Tek gezinti özelliği</span><span class="sxs-lookup"><span data-stu-id="02185-141">Single Navigation Property</span></span>

<span data-ttu-id="02185-142">Tek bir gezinti özelliği (ters gezinme olmadan ve yabancı anahtar özelliği olmadan) dahil olmak üzere, kural tarafından tanımlanan bir ilişkiye sahip olmak için yeterli değildir.</span><span class="sxs-lookup"><span data-stu-id="02185-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="02185-143">Tek bir gezinti özelliğine ve yabancı anahtar özelliğine de sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="02185-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="02185-144">Basamaklı Silme</span><span class="sxs-lookup"><span data-stu-id="02185-144">Cascade Delete</span></span>

<span data-ttu-id="02185-145">Kurala göre, Cascade DELETE, isteğe bağlı ilişkiler için gerekli ilişkiler ve *Clientsetnull* için *Cascade* olarak ayarlanacak.</span><span class="sxs-lookup"><span data-stu-id="02185-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="02185-146">*Cascade* , bağımlı varlıkların de silindiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="02185-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="02185-147">*Clientsetnull* , belleğe yüklenmeyen bağımlı varlıkların değişmeden kalacağından, el ile silinmesi ya da geçerli bir sorumlu varlığına işaret edecek şekilde güncelleştirilmeleri anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="02185-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="02185-148">Belleğe yüklenen varlıklar için EF Core yabancı anahtar özelliklerini null olarak ayarlamaya çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="02185-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="02185-149">Gerekli ve isteğe bağlı ilişkiler arasındaki fark için [gerekli ve Isteğe bağlı ilişkiler](#required-and-optional-relationships) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="02185-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="02185-150">Farklı silme davranışları ve kural tarafından kullanılan varsayılanlar hakkında daha fazla ayrıntı için bkz. [Cascade Delete](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="02185-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="02185-151">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="02185-151">Data Annotations</span></span>

<span data-ttu-id="02185-152">`[ForeignKey]` ve `[InverseProperty]`ilişkilerini yapılandırmak için kullanılabilecek iki veri ek açıklaması vardır.</span><span class="sxs-lookup"><span data-stu-id="02185-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="02185-153">Bunlar `System.ComponentModel.DataAnnotations.Schema` ad alanında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="02185-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="02185-154">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="02185-154">[ForeignKey]</span></span>

<span data-ttu-id="02185-155">Belirli bir ilişki için yabancı anahtar özelliği olarak kullanılması gereken özelliği yapılandırmak için veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="02185-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="02185-156">Bu, genellikle yabancı anahtar özelliği kural tarafından keşfedildiğinde yapılır.</span><span class="sxs-lookup"><span data-stu-id="02185-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="02185-157">`[ForeignKey]` ek açıklaması, ilişkide herhangi bir gezinti özelliğine eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="02185-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="02185-158">Bağımlı varlık sınıfında gezinti özelliğine gitmesinin gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="02185-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="02185-159">[Evirseproperty]</span><span class="sxs-lookup"><span data-stu-id="02185-159">[InverseProperty]</span></span>

<span data-ttu-id="02185-160">Bağımlı ve birincil varlıklarda gezinti özelliklerinin nasıl kullandığını yapılandırmak için veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="02185-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="02185-161">Bu, genellikle iki varlık türü arasında birden fazla gezinti özelliği olduğunda yapılır.</span><span class="sxs-lookup"><span data-stu-id="02185-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="02185-162">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="02185-162">Fluent API</span></span>

<span data-ttu-id="02185-163">Akıcı API 'de bir ilişki yapılandırmak için, ilişkiyi oluşturan gezinti özelliklerini tanımlayarak başlacaksınız.</span><span class="sxs-lookup"><span data-stu-id="02185-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="02185-164">`HasOne` veya `HasMany`, üzerinde yapılandırmaya başmakta olduğunuz varlık türünde gezinti özelliğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="02185-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="02185-165">Daha sonra ters gezintiyi belirlemek için `WithOne` veya `WithMany` bir çağrı zincirleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="02185-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="02185-166">`HasOne`/`WithOne` başvuru gezinti özellikleri için kullanılır ve koleksiyon gezinti özellikleri için `HasMany`/`WithMany` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="02185-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="02185-167">Tek gezinti özelliği</span><span class="sxs-lookup"><span data-stu-id="02185-167">Single Navigation Property</span></span>

<span data-ttu-id="02185-168">Yalnızca bir gezinti özelliğine sahipseniz, `WithOne` ve `WithMany`Parametresiz aşırı yüklemeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="02185-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="02185-169">Bu, ilişkinin diğer ucunda kavramsal olarak bir başvuru veya koleksiyon olduğunu gösterir, ancak varlık sınıfında hiçbir gezinti özelliği dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="02185-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="02185-170">Yabancı anahtar</span><span class="sxs-lookup"><span data-stu-id="02185-170">Foreign Key</span></span>

<span data-ttu-id="02185-171">Belirli bir ilişki için yabancı anahtar özelliği olarak kullanılması gereken özelliği yapılandırmak için Floent API 'yi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="02185-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="02185-172">Aşağıdaki kod listesi, bileşik yabancı anahtarın nasıl yapılandırılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="02185-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="02185-173">Bir gölge özelliği yabancı anahtar olarak yapılandırmak için `HasForeignKey(...)` dize aşırı yüklemesini kullanabilirsiniz (daha fazla bilgi için bkz. [gölge özellikleri](shadow-properties.md) ).</span><span class="sxs-lookup"><span data-stu-id="02185-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="02185-174">Gölge özelliğini, yabancı anahtar olarak kullanmadan önce modele açıkça eklememiz önerilir (aşağıda gösterildiği gibi).</span><span class="sxs-lookup"><span data-stu-id="02185-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="02185-175">Gezinti özelliği olmadan</span><span class="sxs-lookup"><span data-stu-id="02185-175">Without Navigation Property</span></span>

<span data-ttu-id="02185-176">Bir gezinti özelliği sağlamanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="02185-176">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="02185-177">İlişkinin yalnızca bir tarafında yabancı anahtar sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="02185-177">You can simply provide a Foreign Key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="02185-178">Asıl anahtar</span><span class="sxs-lookup"><span data-stu-id="02185-178">Principal Key</span></span>

<span data-ttu-id="02185-179">Yabancı anahtarın birincil anahtar dışında bir özelliğe başvurmasına isterseniz, ilişkinin asıl anahtar özelliğini yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="02185-179">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="02185-180">Asıl anahtar olarak yapılandırdığınız özelliği otomatik olarak alternatif bir anahtar olarak ayarlar (daha fazla bilgi için bkz. [Alternatif anahtarlar](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="02185-180">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

<span data-ttu-id="02185-181">Aşağıdaki kod listesinde bir bileşik asıl anahtarın nasıl yapılandırılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="02185-181">The following code listing shows how to configure a composite principal key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> <span data-ttu-id="02185-182">Asıl anahtar özelliklerini belirttiğiniz sıra, yabancı anahtar için belirtildikleri sırayla eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="02185-182">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="02185-183">Gerekli ve Isteğe bağlı Ilişkiler</span><span class="sxs-lookup"><span data-stu-id="02185-183">Required and Optional Relationships</span></span>

<span data-ttu-id="02185-184">İlişkinin gerekli veya isteğe bağlı olup olmadığını yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="02185-184">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="02185-185">Sonuç olarak bu, yabancı anahtar özelliğinin gerekli veya isteğe bağlı olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="02185-185">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="02185-186">Bu, büyük bir gölge durumu yabancı anahtar kullanırken faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="02185-186">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="02185-187">Varlık sınıfınızda yabancı anahtar özelliği varsa, ilişkinin gereklik durumu yabancı anahtar özelliğinin gerekli veya isteğe bağlı olup olmadığına göre belirlenir (daha fazla bilgi için bkz. [gerekli ve Isteğe bağlı özellikler](required-optional.md) ).</span><span class="sxs-lookup"><span data-stu-id="02185-187">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

### <a name="cascade-delete"></a><span data-ttu-id="02185-188">Basamaklı Silme</span><span class="sxs-lookup"><span data-stu-id="02185-188">Cascade Delete</span></span>

<span data-ttu-id="02185-189">Belirli bir ilişki için basamaklı silme davranışını açıkça yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="02185-189">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="02185-190">Her seçeneğe ilişkin ayrıntılı bir tartışma için verileri kaydetme bölümündeki [basamaklı silme](../saving/cascade-delete.md) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="02185-190">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a><span data-ttu-id="02185-191">Diğer Ilişki desenleri</span><span class="sxs-lookup"><span data-stu-id="02185-191">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="02185-192">Bire bir</span><span class="sxs-lookup"><span data-stu-id="02185-192">One-to-one</span></span>

<span data-ttu-id="02185-193">Bir ilişkinin her iki tarafında da başvuru gezintisi özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="02185-193">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="02185-194">Tek-çok ilişkilerle aynı kurallara uyar, ancak her sorumluyla yalnızca bir bağımlı ilişki olduğundan emin olmak için yabancı anahtar özelliğinde benzersiz bir dizin sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="02185-194">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> <span data-ttu-id="02185-195">EF, bir yabancı anahtar özelliği algılama özelliğine göre bağımlı olacak varlıklardan birini seçer.</span><span class="sxs-lookup"><span data-stu-id="02185-195">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="02185-196">Bağımlı olarak yanlış varlık seçilirse, bunu düzeltmek için akıcı API 'yi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="02185-196">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="02185-197">Akıcı API ile ilişkiyi yapılandırırken `HasOne` ve `WithOne` yöntemlerini kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="02185-197">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="02185-198">Yabancı anahtarı yapılandırırken, bağımlı varlık türünü belirtmeniz gerekir-aşağıdaki listede `HasForeignKey` için belirtilen genel parametreyi görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="02185-198">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="02185-199">Bire çok ilişkisinde, başvuru gezinmesi olan varlığın bağımlı olduğunu ve koleksiyonun asıl öğe olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="02185-199">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="02185-200">Ancak bu, bire bir ilişkide değildir, bu nedenle açıkça tanımlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="02185-200">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a><span data-ttu-id="02185-201">Çoktan çoğa</span><span class="sxs-lookup"><span data-stu-id="02185-201">Many-to-many</span></span>

<span data-ttu-id="02185-202">JOIN tablosunu temsil eden bir varlık sınıfı olmayan çoktan çoğa ilişkiler henüz desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="02185-202">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="02185-203">Bununla birlikte, JOIN tablosuna bir varlık sınıfı ekleyerek ve iki ayrı bire çok ilişkiyi eşleyerek çoktan çoğa ilişkiyi temsil edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="02185-203">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
