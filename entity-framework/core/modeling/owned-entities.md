---
title: Sahip olan varlık türleri-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: a0665bfa27134b8dc3eba854ff3f7b1af4b69217
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655929"
---
# <a name="owned-entity-types"></a><span data-ttu-id="dea18-102">Sahip Olunan Varlık Türleri</span><span class="sxs-lookup"><span data-stu-id="dea18-102">Owned Entity Types</span></span>

> [!NOTE]
> <span data-ttu-id="dea18-103">Bu özellik EF Core 2,0 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="dea18-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="dea18-104">EF Core, yalnızca diğer varlık türlerinin gezinti özelliklerinde görünebilen varlık türlerini modeletmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="dea18-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="dea18-105">Bunlara sahip olan _varlık türleri_denir.</span><span class="sxs-lookup"><span data-stu-id="dea18-105">These are called _owned entity types_.</span></span> <span data-ttu-id="dea18-106">Sahip olduğu varlık türü içeren varlık _sahibi_.</span><span class="sxs-lookup"><span data-stu-id="dea18-106">The entity containing an owned entity type is its _owner_.</span></span>

<span data-ttu-id="dea18-107">Sahibi olan varlıklar aslında sahibin bir parçasıdır ve bu olmadan mevcut olamaz, bu değerler [toplamalara](https://martinfowler.com/bliki/DDD_Aggregate.html)kavramsal olarak benzerdir.</span><span class="sxs-lookup"><span data-stu-id="dea18-107">Owned entities are essentially a part of the owner and cannot exist without it, they are conceptually similar to [aggregates](https://martinfowler.com/bliki/DDD_Aggregate.html).</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="dea18-108">Açık yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dea18-108">Explicit configuration</span></span>

<span data-ttu-id="dea18-109">Sahip olan varlık türleri hiçbir şekilde model by kuralına EF Core hiçbir şekilde dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="dea18-109">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="dea18-110">Türü sahip bir tür olarak yapılandırmak için `OnModelCreating` `OwnsOne` yöntemi veya `OwnedAttribute` (EF Core 2,1 ' de yeni) ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dea18-110">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="dea18-111">Bu örnekte, `StreetAddress` Identity özelliği olmayan bir türdür.</span><span class="sxs-lookup"><span data-stu-id="dea18-111">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="dea18-112">Bu, belirli bir siparişin sevkiyat adresini belirtmek için sipariş türünün bir özelliği olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dea18-112">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="dea18-113">`OwnedAttribute` başka bir varlık türünden başvuruluyorsa sahip olan bir varlık olarak değerlendirmek için kullanabiliriz:</span><span class="sxs-lookup"><span data-stu-id="dea18-113">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="dea18-114">Ayrıca, `ShippingAddress` özelliğinin `Order` varlık türüne ait bir varlık olduğunu belirtmek ve gerekirse ek modelleri yapılandırmak için `OnModelCreating` `OwnsOne` yöntemini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dea18-114">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="dea18-115">`ShippingAddress` özelliği `Order` türünde özel ise, `OwnsOne` yönteminin dize sürümünü kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="dea18-115">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="dea18-116">Daha fazla bağlam için bkz. [tam örnek proje](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) .</span><span class="sxs-lookup"><span data-stu-id="dea18-116">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span>

## <a name="implicit-keys"></a><span data-ttu-id="dea18-117">Örtük anahtarlar</span><span class="sxs-lookup"><span data-stu-id="dea18-117">Implicit keys</span></span>

<span data-ttu-id="dea18-118">`OwnsOne` ile yapılandırılmış veya başvuru gezintisi aracılığıyla keşfedilen sahip olan türler her zaman sahibiyle bire bir ilişkiye sahiptir, bu nedenle yabancı anahtar değerleri benzersiz olduğundan kendi anahtar değerlerine gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="dea18-118">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="dea18-119">Önceki örnekte, `StreetAddress` türünün bir anahtar özelliği tanımlamasına gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="dea18-119">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="dea18-120">EF Core bu nesneleri nasıl izlediğini anlamak için, bir birincil anahtarın, sahip olduğu tür için bir [Gölge Özellik](xref:core/modeling/shadow-properties) olarak oluşturulduğunu bilmeniz yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="dea18-120">In order to understand how EF Core tracks these objects, it is useful to know that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="dea18-121">Sahip türünün bir örneğinin anahtarı değeri, sahip örneği anahtarının değeri ile aynı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="dea18-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="dea18-122">Sahip olunan türlerin koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="dea18-122">Collections of owned types</span></span>

> [!NOTE]
> <span data-ttu-id="dea18-123">Bu özellik EF Core 2,2 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="dea18-123">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="dea18-124">Sahip olunan türlerin bir koleksiyonunu yapılandırmak için `OnModelCreating``OwnsMany` kullanın.</span><span class="sxs-lookup"><span data-stu-id="dea18-124">To configure a collection of owned types use `OwnsMany` in `OnModelCreating`.</span></span>

<span data-ttu-id="dea18-125">Sahip olunan türlerin bir birincil anahtar olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="dea18-125">Owned types need a primary key.</span></span> <span data-ttu-id="dea18-126">.NET türünde iyi aday özellikleri yoksa EF Core bir tane oluşturmayı deneyebilir.</span><span class="sxs-lookup"><span data-stu-id="dea18-126">If there are no good candidates properties on the .NET type, EF Core can try to create one.</span></span> <span data-ttu-id="dea18-127">Ancak, sahip olunan türler bir koleksiyon aracılığıyla tanımlandığında, `OwnsOne`için yaptığımız gibi, hem yabancı anahtar hem de sahip olan Örneğin birincil anahtarı olarak görev yapacak bir gölge özellik oluşturmak için yeterli değildir: her biri için birden çok sahip tür örneği olabilir sahip ve bu nedenle sahip anahtarı, sahip olunan her örnek için benzersiz bir kimlik sağlamak için yeterli değildir.</span><span class="sxs-lookup"><span data-stu-id="dea18-127">However, when owned types are defined through a collection, it isn't enough to just create a shadow property to act as both the foreign key into the owner and the primary key of the owned instance, as we do for `OwnsOne`: there can be multiple owned type instances for each owner, and hence the key of the owner isn't enough to provide a unique identity for each owned instance.</span></span>

<span data-ttu-id="dea18-128">Bunun en kolay iki çözümü şunlardır:</span><span class="sxs-lookup"><span data-stu-id="dea18-128">The two most straightforward solutions to this are:</span></span>

- <span data-ttu-id="dea18-129">Yeni bir özellikte, sahibine işaret eden yabancı anahtardan bağımsız olarak bir vekil birincil anahtar tanımlama.</span><span class="sxs-lookup"><span data-stu-id="dea18-129">Defining a surrogate primary key on a new property independent of the foreign key that points to the owner.</span></span> <span data-ttu-id="dea18-130">İçerilen değerlerin tüm sahiplerin benzersiz olması gerekir (örneğin, üst {1} alt {1}varsa, üst {2} alt {1}olamaz), bu nedenle değerin hiç bir anlamı yoktur.</span><span class="sxs-lookup"><span data-stu-id="dea18-130">The contained values would need to be unique across all owners (e.g. if Parent {1} has Child {1}, then Parent {2} cannot have Child {1}), so the value doesn't have any inherent meaning.</span></span> <span data-ttu-id="dea18-131">Yabancı anahtar birincil anahtarın parçası olmadığından, değerleri değiştirilebilir, bu nedenle alt öğeyi bir üst öğeden diğerine taşıyabilirsiniz, ancak bu genellikle toplam semantiğe karşı gider.</span><span class="sxs-lookup"><span data-stu-id="dea18-131">Since the foreign key is not part of the primary key its values can be changed, so you could move a child from one parent to another one, however this usually goes against aggregate semantics.</span></span>
- <span data-ttu-id="dea18-132">Yabancı anahtar ve ek bir özelliği bileşik anahtar olarak kullanma.</span><span class="sxs-lookup"><span data-stu-id="dea18-132">Using the foreign key and an additional property as a composite key.</span></span> <span data-ttu-id="dea18-133">Ek özellik değerinin artık yalnızca belirli bir üst öğe için benzersiz olması gerekir (Bu nedenle, üst {1} alt {1,1} varsa, üst {2} hala alt {2,1}olabilir).</span><span class="sxs-lookup"><span data-stu-id="dea18-133">The additional property value now only needs to be unique for a given parent (so if Parent {1} has Child {1,1} then Parent {2} can still have Child {2,1}).</span></span> <span data-ttu-id="dea18-134">Birincil anahtarın yabancı anahtar bölümünü, sahibi ve sahibi olan varlık arasındaki ilişki sabit hale gelir ve toplam semantiğini daha iyi yansıtır.</span><span class="sxs-lookup"><span data-stu-id="dea18-134">By making the foreign key part of the primary key the relationship between the owner and the owned entity becomes immutable and reflects aggregate semantics better.</span></span> <span data-ttu-id="dea18-135">Varsayılan olarak EF Core budur.</span><span class="sxs-lookup"><span data-stu-id="dea18-135">This is what EF Core does by default.</span></span>

<span data-ttu-id="dea18-136">Bu örnekte `Distributor` sınıfını kullanacağız:</span><span class="sxs-lookup"><span data-stu-id="dea18-136">In this example we'll use the `Distributor` class:</span></span>

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

<span data-ttu-id="dea18-137">Varsayılan olarak, `ShippingCenters` gezinti özelliği aracılığıyla başvurulan sahip türü için kullanılan birincil anahtar, `"DistributorId"` FK olduğu ve `"Id"` benzersiz bir `int` değeri olduğu `("DistributorId", "Id")` olacaktır.</span><span class="sxs-lookup"><span data-stu-id="dea18-137">By default the primary key used for the owned type referenced through the `ShippingCenters` navigation property will be `("DistributorId", "Id")` where `"DistributorId"` is the FK and `"Id"` is a unique `int` value.</span></span>

<span data-ttu-id="dea18-138">Farklı bir PK çağrısı `HasKey`yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="dea18-138">To configure a different PK call `HasKey`:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> <span data-ttu-id="dea18-139">EF Core 3,0 `WithOwner()` yöntemi mevcut olmadığı için bu çağrının kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="dea18-139">Before EF Core 3.0 `WithOwner()` method didn't exist so this call should be removed.</span></span>

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="dea18-140">Sahip olunan türleri tablo bölme ile eşleme</span><span class="sxs-lookup"><span data-stu-id="dea18-140">Mapping owned types with table splitting</span></span>

<span data-ttu-id="dea18-141">İlişkisel veritabanları kullanılırken, varsayılan başvuruya ait türler, sahibiyle aynı tabloyla eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="dea18-141">When using relational databases, by default reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="dea18-142">Bu, tablonun iki içinde bölünmesini gerektirir: bazı sütunlar sahibin verilerini depolamak için kullanılacaktır ve bu varlığın verilerini depolamak için bazı sütunlar kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="dea18-142">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="dea18-143">Bu, [tablo bölme](table-splitting.md)olarak bilinen yaygın bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="dea18-143">This is a common feature known as [table splitting](table-splitting.md).</span></span>

<span data-ttu-id="dea18-144">Varsayılan olarak EF Core, _Navigation_OwnedEntityProperty_örüntüsünün ardından ait olan varlık türünün özelliklerinin veritabanı sütunlarını adlandırın.</span><span class="sxs-lookup"><span data-stu-id="dea18-144">By default, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="dea18-145">Bu nedenle `StreetAddress` özellikleri ' ShippingAddress_Street ' ve ' ShippingAddress_City ' adlarıyla ' Orders ' tablosunda görünür.</span><span class="sxs-lookup"><span data-stu-id="dea18-145">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="dea18-146">Bu sütunları yeniden adlandırmak için `HasColumnName` yöntemini kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="dea18-146">You can use the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="dea18-147">Aynı .NET türünü sahip olunan birden çok tür arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="dea18-147">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="dea18-148">Sahip olunan bir varlık türü, sahip olduğu başka bir varlık türüyle aynı .NET türünde olabilir, bu nedenle .NET türü, sahip olunan bir türü belirlemek için yeterli olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="dea18-148">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="dea18-149">Bu gibi durumlarda, sahibi olan varlığa işaret eden özellik sahip olan varlık türü için _gezinme_ haline gelir.</span><span class="sxs-lookup"><span data-stu-id="dea18-149">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="dea18-150">EF Core perspektifinden, tanımlama gezintisi .NET türüyle birlikte türün kimliğinin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="dea18-150">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>

<span data-ttu-id="dea18-151">Örneğin, aşağıdaki sınıfta `ShippingAddress` ve `BillingAddress` aynı .NET türüdür `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="dea18-151">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="dea18-152">EF Core, bu nesnelerin izlenen örneklerini nasıl ayıracağınızı anlamak için, tanımlama gezintisinin, sahip olduğu türün değeri ve sahip olduğu .NET türü ile birlikte örnek anahtarının bir parçası haline geldiğini düşünmek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="dea18-152">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="dea18-153">İç içe sahip türler</span><span class="sxs-lookup"><span data-stu-id="dea18-153">Nested owned types</span></span>

<span data-ttu-id="dea18-154">Bu örnekte, hem `StreetAddress` türü olan `BillingAddress` ve `ShippingAddress`sahip `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="dea18-154">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="dea18-155">`OrderDetails`, `DetailedOrder` türüne aittir.</span><span class="sxs-lookup"><span data-stu-id="dea18-155">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="dea18-156">Sahip olunan iç içe geçmiş türlere ek olarak, sahip olunan bir tür düzenli bir varlığa başvurabilir, sahip olan varlık bağımlı tarafta olduğu sürece sahip veya farklı bir varlık olabilir.</span><span class="sxs-lookup"><span data-stu-id="dea18-156">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="dea18-157">Bu yetenek, sahip olduğu varlık türlerini EF6 içindeki karmaşık türlerden ayrı olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="dea18-157">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="dea18-158">Bu modeli yapılandırmak için `OwnsOne` yönteminin akıcı bir çağrıda zinciri oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="dea18-158">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="dea18-159">Sahibe doğru işaret eden gezinti özelliğini yapılandırmak için kullanılan `WithOwner` çağrısına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="dea18-159">Notice the `WithOwner` call used to configure the navigation property pointing back at the owner.</span></span>

<span data-ttu-id="dea18-160">`OrderDetails` ve `StreetAdress``OwnedAttribute` kullanarak sonuca ulaşmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="dea18-160">It is possible to achieve the result using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="dea18-161">Sahip olunan türleri ayrı tablolarda depolama</span><span class="sxs-lookup"><span data-stu-id="dea18-161">Storing owned types in separate tables</span></span>

<span data-ttu-id="dea18-162">Ayrıca, EF6 karmaşık türlerin aksine, sahip olunan türler sahibinden ayrı bir tabloda depolanabilir.</span><span class="sxs-lookup"><span data-stu-id="dea18-162">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="dea18-163">Sahip olan bir türü sahip ile aynı tabloya eşleyen kuralı geçersiz kılmak için, yalnızca `ToTable` çağırıp farklı bir tablo adı sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dea18-163">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="dea18-164">Aşağıdaki örnek `OrderDetails` ve iki adresini `DetailedOrder`olan ayrı bir tabloyla eşleyebilir:</span><span class="sxs-lookup"><span data-stu-id="dea18-164">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="dea18-165">Sahip olunan türler sorgulanıyor</span><span class="sxs-lookup"><span data-stu-id="dea18-165">Querying owned types</span></span>

<span data-ttu-id="dea18-166">Sahibi sorgulanırken sahip olan türler varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="dea18-166">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="dea18-167">Sahip olan türler ayrı bir tabloda depolansa bile `Include` yönteminin kullanılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="dea18-167">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="dea18-168">Daha önce açıklanan modele bağlı olarak, aşağıdaki sorgu `Order`, `OrderDetails` ve veritabanından gelen `StreetAddresses` sahip olur:</span><span class="sxs-lookup"><span data-stu-id="dea18-168">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="dea18-169">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="dea18-169">Limitations</span></span>

<span data-ttu-id="dea18-170">Bu sınırlamaların bazıları, sahip olduğu varlık türlerinin çalışması için temeldir, ancak bazı diğerleri sonraki sürümlerde kaldırabilecekler konusunda daha fazla kısıtlamayla karşılaşabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="dea18-170">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="dea18-171">Tasarıma göre kısıtlamalar</span><span class="sxs-lookup"><span data-stu-id="dea18-171">By-design restrictions</span></span>

- <span data-ttu-id="dea18-172">Sahip olunan bir tür için `DbSet<T>` oluşturamazsınız</span><span class="sxs-lookup"><span data-stu-id="dea18-172">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="dea18-173">`ModelBuilder` sahip bir tür ile `Entity<T>()` çağıramaz</span><span class="sxs-lookup"><span data-stu-id="dea18-173">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="dea18-174">Geçerli eksikler</span><span class="sxs-lookup"><span data-stu-id="dea18-174">Current shortcomings</span></span>

- <span data-ttu-id="dea18-175">Sahibi olan varlık türlerini içeren devralma hiyerarşileri desteklenmez</span><span class="sxs-lookup"><span data-stu-id="dea18-175">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="dea18-176">Sahip olunan varlık türlerine yönelik başvuru gezginleri, sahip tarafından ayrı bir tabloya açık olarak eşlenmediği müddetçe null olamaz</span><span class="sxs-lookup"><span data-stu-id="dea18-176">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="dea18-177">Sahip varlık türlerinin örnekleri birden çok sahip tarafından paylaşılamaz (Bu, sahip olduğu varlık türleri kullanılarak uygulanamaz değer nesneleri için iyi bilinen bir senaryodur)</span><span class="sxs-lookup"><span data-stu-id="dea18-177">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="dea18-178">Önceki sürümlerde shortcomler</span><span class="sxs-lookup"><span data-stu-id="dea18-178">Shortcomings in previous versions</span></span>

- <span data-ttu-id="dea18-179">EF Core 2,0 ' de, sahip olunan varlıklar sahip hiyerarşisinden ayrı bir tabloya açık bir şekilde eşlenmediği müddetçe, sahip olunan varlık türlerine ait gezintiler türetilmiş varlık türlerinde bildirilemez.</span><span class="sxs-lookup"><span data-stu-id="dea18-179">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="dea18-180">EF Core 2,1 ' de bu sınırlama kaldırılmıştır</span><span class="sxs-lookup"><span data-stu-id="dea18-180">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="dea18-181">EF Core 2,0 ve 2,1 ' de yalnızca sahip olunan türlerin başvuru gezginlerini destekliyordu.</span><span class="sxs-lookup"><span data-stu-id="dea18-181">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="dea18-182">EF Core 2,2 ' de bu sınırlama kaldırılmıştır</span><span class="sxs-lookup"><span data-stu-id="dea18-182">This limitation has been removed in EF Core 2.2</span></span>
