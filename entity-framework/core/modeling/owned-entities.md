---
title: Varlık türleri - EF Core ait
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: 58da3b6b951b3fa4aa04ec75f5759555c1f0cde5
ms.sourcegitcommit: 39080d38e1adea90db741257e60dc0e7ed08aa82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980034"
---
# <a name="owned-entity-types"></a><span data-ttu-id="55458-102">Sahip olunan varlık türleri</span><span class="sxs-lookup"><span data-stu-id="55458-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="55458-103">Bu özellik, EF Core 2.0 sürümünde yenidir.</span><span class="sxs-lookup"><span data-stu-id="55458-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="55458-104">EF Core, diğer varlık türleri Gezinti özellikleri yalnızca görüntülenebilir modeli varlık türleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="55458-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="55458-105">Bunlar adlandırılır _varlık türlerine ait_.</span><span class="sxs-lookup"><span data-stu-id="55458-105">These are called _owned entity types_.</span></span> <span data-ttu-id="55458-106">Sahip olunan varlık türünü içeren bir varlık, _sahibi_.</span><span class="sxs-lookup"><span data-stu-id="55458-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="55458-107">Açık yapılandırma</span><span class="sxs-lookup"><span data-stu-id="55458-107">Explicit configuration</span></span>

<span data-ttu-id="55458-108">Sahip olunan varlık türleri hiçbir zaman EF Core tarafından modelde kuralı olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="55458-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="55458-109">Kullanabileceğiniz `OwnsOne` yöntemi `OnModelCreating` veya açıklama türüyle `OwnedAttribute` (EF Core 2.1 yeni) sahip olunan bir türü türünü yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="55458-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="55458-110">Bu örnekte, `StreetAddress` hiçbir kimlik özelliğine sahip bir tür.</span><span class="sxs-lookup"><span data-stu-id="55458-110">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="55458-111">Sipariş türünde bir özellik, belirli bir siparişi teslimat adresini belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="55458-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="55458-112">Kullanabiliriz `OwnedAttribute` başka bir varlık türü, başvurulduğunda sahip olunan bir varlık olarak değerlendirmek için:</span><span class="sxs-lookup"><span data-stu-id="55458-112">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="55458-113">Kullanmak da mümkündür `OwnsOne` yönteminde `OnModelCreating` belirtmek için `ShippingAddress` özelliktir ait bir varlığın `Order` varlık türü ve gerekirse ek özellikleri yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="55458-113">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="55458-114">Varsa `ShippingAddress` özelliği, özel `Order` türü, dize sürümü kullanabilirsiniz `OwnsOne` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="55458-114">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="55458-115">Bkz: [tam örnek proje](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) daha fazla bağlam için.</span><span class="sxs-lookup"><span data-stu-id="55458-115">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span> 

## <a name="implicit-keys"></a><span data-ttu-id="55458-116">Örtük anahtarları</span><span class="sxs-lookup"><span data-stu-id="55458-116">Implicit keys</span></span>

<span data-ttu-id="55458-117">Sahip yapılandırılmış türleri `OwnsOne` veya bir başvuru gezinmeyi bulunan her zaman sahibiyle bire bir ilişki varsa, bu nedenle yabancı anahtar değerlerinin benzersiz olduğu gibi anahtar değerlerini gerekmez.</span><span class="sxs-lookup"><span data-stu-id="55458-117">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="55458-118">Önceki örnekte, `StreetAddress` türü tanımlayan bir anahtar özellik gerekmez.</span><span class="sxs-lookup"><span data-stu-id="55458-118">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="55458-119">EF Core bu nesnelerin nasıl izlediği anlamak için birincil anahtar olarak oluşturduğunuz düşünün yararlıdır bir [gölge özelliği](xref:core/modeling/shadow-properties) ait türü.</span><span class="sxs-lookup"><span data-stu-id="55458-119">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="55458-120">Sahip olunan türünün bir örneği anahtarın değerini sahibi örneğinin anahtarı değeri ile aynı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="55458-120">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="55458-121">Sahip olunan türler</span><span class="sxs-lookup"><span data-stu-id="55458-121">Collections of owned types</span></span>

>[!NOTE]
> <span data-ttu-id="55458-122">Bu özellik, EF Core 2.2 içinde yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="55458-122">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="55458-123">Sahip olunan türleri koleksiyonu yapılandırmak için `OwnsMany` kullanılmalıdır `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="55458-123">To configure a collection of owned types `OwnsMany` should be used in `OnModelCreating`.</span></span> <span data-ttu-id="55458-124">Ancak açıkça belirtilmesi ihtiyaç duyduğunuz şekilde birincil anahtarı kuralı tarafından yapılandırılmaz.</span><span class="sxs-lookup"><span data-stu-id="55458-124">However the primary key will not be configured by convention, so it need to be specified explicitly.</span></span> <span data-ttu-id="55458-125">Bu tür bir sahibi ve de gölge durumda olabilecek ek benzersiz bir özellik için yabancı anahtarı bir araya getiren varlıklar karmaşık bir anahtar kullanmak yaygın bir uygulamadır:</span><span class="sxs-lookup"><span data-stu-id="55458-125">It is common to use a complex key for these type of entities incorporating the foreign key to the owner and an additional unique property that can also be in shadow state:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="55458-126">Tablo bölme ile türleri ait eşleme</span><span class="sxs-lookup"><span data-stu-id="55458-126">Mapping owned types with table splitting</span></span>

<span data-ttu-id="55458-127">İlişkisel kullanırken veritabanları aynı tablonun sahibi olarak türlerin eşlendiği kuralı başvuruya göre ait.</span><span class="sxs-lookup"><span data-stu-id="55458-127">When using relational databases, by convention reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="55458-128">Bu iki tablodaki bölünmesini gerektirir: bazı sütunları sahibinin verileri depolamak için kullanılan ve bazı sütunları, sahip olunan varlık verilerini depolamak için kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="55458-128">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="55458-129">Bu tablo bölme olarak bilinen genel bir özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="55458-129">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="55458-130">Tablo bölme depolanan türleri olabilir ait EF6 içinde nasıl karmaşık türler için kullanılan benzer şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="55458-130">Owned types stored with table splitting can be used similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="55458-131">Kural gereği, EF Core düzenini izleyerek sahip olunan varlık türünün özelliklerini veritabanı sütunlarını adlandıracaktır _Navigation_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="55458-131">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="55458-132">Bu nedenle `StreetAddress` özellikleri adları 'ShippingAddress_Street' ve 'ShippingAddress_City' ile 'Siparişler' tablosunda görünür.</span><span class="sxs-lookup"><span data-stu-id="55458-132">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="55458-133">Ekleyebilirsiniz `HasColumnName` yönteminin bu sütunları yeniden adlandırın:</span><span class="sxs-lookup"><span data-stu-id="55458-133">You can append the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="55458-134">Aynı .NET türü birden çok ait türleri arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="55458-134">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="55458-135">Sahip olunan varlık türü başka bir sahip olunan varlık türü olarak aynı .NET türü, bu nedenle .NET türü sahip olunan bir tür tanımlamak için yeterli olmayabilir olabilir.</span><span class="sxs-lookup"><span data-stu-id="55458-135">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="55458-136">Bu gibi durumlarda, sahip olunan varlık sahibinden işaret eden özelliği haline gelir _Gezinti tanımlama_ sahip olunan varlık türü.</span><span class="sxs-lookup"><span data-stu-id="55458-136">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="55458-137">EF Core perspektifinden tanımlama Gezinti tipin kimliği .NET türü yanı sıra bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="55458-137">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="55458-138">Örneğin, aşağıdaki sınıfında `ShippingAddress` ve `BillingAddress` aynı .NET türü, her ikisi de `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="55458-138">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="55458-139">EF Core izlenen nesnelerin örneklerini nasıl ayıracaktır anlamak için tanımlama Gezinti değerin yanı sıra sahibinin anahtar örneğinin anahtarının bir parçası ve şirkete ait türün .NET türü hale geldiğini düşünmek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="55458-139">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="55458-140">Sahip olunan iç içe geçmiş türler</span><span class="sxs-lookup"><span data-stu-id="55458-140">Nested owned types</span></span>

<span data-ttu-id="55458-141">Bu örnekte `OrderDetails` sahibi `BillingAddress` ve `ShippingAddress`, her ikisi de olduğu `StreetAddress` türleri.</span><span class="sxs-lookup"><span data-stu-id="55458-141">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="55458-142">Ardından `OrderDetails` tarafından sahip olunan `DetailedOrder` türü.</span><span class="sxs-lookup"><span data-stu-id="55458-142">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="55458-143">Sahip olunan iç içe geçmiş türler ek olarak sahip olunan bir türü normal bir varlığa başvurabilir, sahip olunan varlık bağımlı tarafında olduğu sürece sahibi ya da farklı bir varlığın olabilir.</span><span class="sxs-lookup"><span data-stu-id="55458-143">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="55458-144">Bu özellik, sahip olunan varlık türleri karmaşık türler dışında EF6 ' ayarlar.</span><span class="sxs-lookup"><span data-stu-id="55458-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="55458-145">Zincire mümkündür `OwnsOne` yöntemi fluent çağrıda bu modeli yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="55458-145">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="55458-146">Kullanarak aynı şeyi elde etmek mümkündür `OwnedAttribute` hem de `OrderDetails` ve `StreetAdress`.</span><span class="sxs-lookup"><span data-stu-id="55458-146">It is also possible to achieve the same thing using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="55458-147">Depolama türleri ayrı tablolarda ait</span><span class="sxs-lookup"><span data-stu-id="55458-147">Storing owned types in separate tables</span></span>

<span data-ttu-id="55458-148">Ayrıca EF6 karmaşık türlerinin aksine, sahip olunan türleri sahibinden ayrı bir tabloda depolanabilir.</span><span class="sxs-lookup"><span data-stu-id="55458-148">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="55458-149">Aynı tablonun sahibi olarak sahip olunan bir türü eşlendiği kuralını geçersiz kılmak için basitçe çağırabilirsiniz `ToTable` ve farklı bir tablo adı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="55458-149">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="55458-150">Aşağıdaki örnek eşler `OrderDetails` ve bunun iki ayrı tablodan adreslere `DetailedOrder`:</span><span class="sxs-lookup"><span data-stu-id="55458-150">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="55458-151">Sahip olunan türler sorgulanıyor</span><span class="sxs-lookup"><span data-stu-id="55458-151">Querying owned types</span></span>

<span data-ttu-id="55458-152">Sahibi sorgulanırken ait türleri varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="55458-152">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="55458-153">Kullanmak için gerekli değil `Include` yöntemi olsa da sahibi türleri, ayrı bir tabloda depolanır.</span><span class="sxs-lookup"><span data-stu-id="55458-153">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="55458-154">Önce belirtilen modeline bağlı olarak, aşağıdaki sorguyu elde edersiniz `Order`, `OrderDetails` ait ve iki `StreetAddresses` veritabanından:</span><span class="sxs-lookup"><span data-stu-id="55458-154">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="55458-155">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="55458-155">Limitations</span></span>

<span data-ttu-id="55458-156">Bu sınırlamaların bazıları nasıl sahip olunan varlık türleri iş temel ancak bazıları biz gelecek sürümlerde kaldırmak mümkün olabileceğini kısıtlamaları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="55458-156">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="55458-157">Tasarım kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="55458-157">By-design restrictions</span></span>
- <span data-ttu-id="55458-158">Oluşturamazsınız bir `DbSet<T>` sahip olunan bir türü için</span><span class="sxs-lookup"><span data-stu-id="55458-158">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="55458-159">Çağıramazsınız `Entity<T>()` üzerinde sahip olunan bir türü `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="55458-159">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="55458-160">Geçerli eksiklikleri</span><span class="sxs-lookup"><span data-stu-id="55458-160">Current shortcomings</span></span>
- <span data-ttu-id="55458-161">İçeren devralma hiyerarşilerini sahip olunan varlık türleri desteklenmez</span><span class="sxs-lookup"><span data-stu-id="55458-161">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="55458-162">Başvuru gezintiler için sahip olduğu açıkça ayrı bir tabloya sahibinden eşleştirildikleri sürece varlık türleri null olamaz</span><span class="sxs-lookup"><span data-stu-id="55458-162">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="55458-163">(Bu, sahip olunan varlık türleri kullanılarak uygulanamaz değer nesneleri için iyi bilinen bir senaryo) birden çok sahipleri tarafından sahip olunan varlık türleri örneklerini paylaşılamaz.</span><span class="sxs-lookup"><span data-stu-id="55458-163">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="55458-164">Önceki sürümlerde eksiklikleri</span><span class="sxs-lookup"><span data-stu-id="55458-164">Shortcomings in previous versions</span></span>
- <span data-ttu-id="55458-165">EF Core 2.0 sürümünde, sahip olunan varlık sahibi hiyerarşiden açıkça ayrı bir tabloya eşlenmiş sürece, türetilen varlık türleri varlık türleri bildirilemez gezintiler için ait.</span><span class="sxs-lookup"><span data-stu-id="55458-165">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="55458-166">Bu sınırlama EF Core 2.1 kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="55458-166">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="55458-167">Tr EF Core 2.0 ve 2.1 yalnızca başvuru gezintiler ait türlerine desteklendi.</span><span class="sxs-lookup"><span data-stu-id="55458-167">En EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="55458-168">Bu sınırlama EF Core 2.2 kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="55458-168">This limitation has been removed in EF Core 2.2</span></span>