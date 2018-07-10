---
title: Varlık türleri - EF Core ait
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: 768429b857b09c1974f4ade31b5bbb6b1c7e15c3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912693"
---
# <a name="owned-entity-types"></a><span data-ttu-id="2d385-102">Sahip olunan varlık türleri</span><span class="sxs-lookup"><span data-stu-id="2d385-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="2d385-103">Bu özellik, EF Core 2.0 sürümünde yenidir.</span><span class="sxs-lookup"><span data-stu-id="2d385-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="2d385-104">EF Core, diğer varlık türleri Gezinti özellikleri yalnızca görüntülenebilir modeli varlık türleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="2d385-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="2d385-105">Bunlar adlandırılır _varlık türlerine ait_.</span><span class="sxs-lookup"><span data-stu-id="2d385-105">These are called _owned entity types_.</span></span> <span data-ttu-id="2d385-106">Sahip olunan varlık türünü içeren bir varlık, _sahibi_.</span><span class="sxs-lookup"><span data-stu-id="2d385-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="2d385-107">Açık yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2d385-107">Explicit configuration</span></span>

<span data-ttu-id="2d385-108">Sahip olunan varlık türleri hiçbir zaman EF Core tarafından modelde kuralı olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="2d385-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="2d385-109">Kullanabileceğiniz `OwnsOne` yöntemi `OnModelCreating` veya açıklama türüyle `OwnedAttribute` (EF Core 2.1 yeni) sahip olunan bir türü türünü yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="2d385-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="2d385-110">Bu örnekte, StreetAddress hiçbir kimlik özelliği türüdür.</span><span class="sxs-lookup"><span data-stu-id="2d385-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="2d385-111">Sipariş türünde bir özellik, belirli bir siparişi teslimat adresini belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2d385-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="2d385-112">İçinde `OnModelCreating`, kullandığımız `OwnsOne` ShippingAddress özelliği siparişini ait bir varlığın olduğunu belirtmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2d385-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

// OnModelCreating
modelBuilder.Entity<Order>().OwnsOne(p => p.ShippingAddress);
```

<span data-ttu-id="2d385-113">ShippingAddress özellik siparişini özel ise, dize sürümü kullanabileceğiniz `OwnsOne` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="2d385-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="2d385-114">Bu örnekte, kullandığımız `OwnedAttribute` aynı hedefe ulaşmak için:</span><span class="sxs-lookup"><span data-stu-id="2d385-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="implicit-keys"></a><span data-ttu-id="2d385-115">Örtük anahtarları</span><span class="sxs-lookup"><span data-stu-id="2d385-115">Implicit keys</span></span>

<span data-ttu-id="2d385-116">EF Core 2.0 ve 2.1, yalnızca başvuru Gezinti özellikleri ait türlerine işaret edebilir.</span><span class="sxs-lookup"><span data-stu-id="2d385-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="2d385-117">Sahip olunan türler desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="2d385-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="2d385-118">Bu başvuru ait türleri, her zaman bire bir ilişki sahip olması, bu nedenle anahtar değerlerini gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2d385-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="2d385-119">Önceki örnekte StreetAddress türü tanımlayan bir anahtar özellik gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2d385-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="2d385-120">EF Core bu nesnelerin nasıl izlediği anlamanın sırada bir birincil anahtar olarak oluşturduğunuz düşünün yararlı bir [gölge özelliği](xref:core/modeling/shadow-properties) ait türü.</span><span class="sxs-lookup"><span data-stu-id="2d385-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="2d385-121">Sahip olunan türünün bir örneği anahtarın değerini sahibi örneğinin anahtarı değeri ile aynı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2d385-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="2d385-122">Tablo bölme ile türleri ait eşleme</span><span class="sxs-lookup"><span data-stu-id="2d385-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="2d385-123">İlişkisel veritabanları kullanırken türlerine sahip kurala göre aynı tablonun sahibi olarak eşlenir.</span><span class="sxs-lookup"><span data-stu-id="2d385-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="2d385-124">Bu iki tablodaki bölünmesini gerektirir: bazı sütunları sahibinin verileri depolamak için kullanılan ve bazı sütunları, sahip olunan varlık verilerini depolamak için kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="2d385-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="2d385-125">Bu tablo bölme olarak bilinen genel bir özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="2d385-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="2d385-126">Tablo bölme depolanan türleri olabilir sahip olduğu için nasıl karmaşık türler içinde EF6 kullanılan çok benzer şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2d385-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="2d385-127">Kural gereği, EF Core düzenini izleyerek sahip olunan varlık türünün özelliklerini veritabanı sütunlarını adlandıracaktır _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="2d385-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="2d385-128">Bu nedenle StreetAddress özellikleri, Northwind'deki Siparişler tablosunda ShippingAddress_Street ve ShippingAddress_City adlarla görünür.</span><span class="sxs-lookup"><span data-stu-id="2d385-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="2d385-129">Ekleyebilirsiniz `HasColumnName` bu sütunları yeniden adlandırma için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2d385-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="2d385-130">Eşlemeleri StreetAddress ortak özelliği olduğu durumda olacaktır</span><span class="sxs-lookup"><span data-stu-id="2d385-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="2d385-131">Aynı .NET türü birden çok ait türleri arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="2d385-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="2d385-132">Sahip olunan varlık türü başka bir sahip olunan varlık türü olarak aynı .NET türü, bu nedenle .NET türü sahip olunan bir tür tanımlamak için yeterli olmayabilir olabilir.</span><span class="sxs-lookup"><span data-stu-id="2d385-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="2d385-133">Bu gibi durumlarda, sahip olunan varlık sahibinden işaret eden özelliği haline gelir _Gezinti tanımlama_ sahip olunan varlık türü.</span><span class="sxs-lookup"><span data-stu-id="2d385-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="2d385-134">EF Core perspektifinden tanımlama Gezinti tipin kimliği .NET türü yanı sıra bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="2d385-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="2d385-135">Örneğin, aşağıdaki sınıfında ShippingAddress BillingAddress hem de aynı .NET türü StreetAddress olması:</span><span class="sxs-lookup"><span data-stu-id="2d385-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="2d385-136">EF Core izlenen nesnelerin örneklerini nasıl ayıracaktır anlamak için tanımlama Gezinti değerin yanı sıra sahibinin anahtar örneğinin anahtarının bir parçası ve şirkete ait türün .NET türü hale geldiğini düşünmek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="2d385-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="2d385-137">Sahip olunan iç içe geçmiş türler</span><span class="sxs-lookup"><span data-stu-id="2d385-137">Nested owned types</span></span>

<span data-ttu-id="2d385-138">Bu örnekte, BillingAddress ve her iki StreetAddress türleri olan ShippingAddress OrderDetails aittir.</span><span class="sxs-lookup"><span data-stu-id="2d385-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="2d385-139">Ardından OrderDetails sipariş türüne göre aittir.</span><span class="sxs-lookup"><span data-stu-id="2d385-139">Then OrderDetails is owned by the Order type.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
    public OrderStatus Status { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public enum OrderStatus
{
    Pending,
    Shipped
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="2d385-140">Zincire mümkündür `OwnsOne` yöntemi fluent eşlemesindeki bu modeli yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="2d385-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="2d385-141">Kullanarak aynı şeyi elde etmek mümkündür `OwnedAttribute` OrderDetails hem StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="2d385-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="2d385-142">Ek olarak sahip olunan iç içe geçmiş türler, sahip olunan bir türü normal bir varlık başvurabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2d385-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="2d385-143">Aşağıdaki örnekte, bir normal (yani sahibi olmayan) varlık ülke verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="2d385-143">In the following example, Country is a regular (i.e. non-owned) entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="2d385-144">Bu özellik, sahip olunan varlık türleri karmaşık türler dışında EF6 ' ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2d385-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="2d385-145">Depolama türleri ayrı tablolarda ait</span><span class="sxs-lookup"><span data-stu-id="2d385-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="2d385-146">Ayrıca EF6 karmaşık türlerinin aksine, sahip olunan türleri sahibinden ayrı bir tabloda depolanabilir.</span><span class="sxs-lookup"><span data-stu-id="2d385-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="2d385-147">Aynı tablonun sahibi olarak sahip olunan bir türü eşlendiği kuralını geçersiz kılmak için basitçe çağırabilirsiniz `ToTable` ve farklı bir tablo adı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="2d385-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="2d385-148">Aşağıdaki örnek OrderDetails ve iki adreslere ayrı bir tabloya siparişi eşler:</span><span class="sxs-lookup"><span data-stu-id="2d385-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="2d385-149">Sahip olunan türler sorgulanıyor</span><span class="sxs-lookup"><span data-stu-id="2d385-149">Querying owned types</span></span>

<span data-ttu-id="2d385-150">Sahibi sorgulanırken ait türleri varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="2d385-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="2d385-151">Kullanmak için gerekli değil `Include` yöntemi olsa da sahibi türleri, ayrı bir tabloda depolanır.</span><span class="sxs-lookup"><span data-stu-id="2d385-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="2d385-152">Önce belirtilen modeline bağlı olarak, aşağıdaki sorguyu sırası, OrderDetails ve veritabanından tüm bekleyen siparişleri için iki ait StreeAddresses çeker:</span><span class="sxs-lookup"><span data-stu-id="2d385-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreeAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="2d385-153">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="2d385-153">Limitations</span></span>

<span data-ttu-id="2d385-154">Bu sınırlamaların bazıları nasıl sahip olunan varlık türleri iş temel ancak bazıları biz gelecek sürümlerde kaldırmak mümkün olabileceğini kısıtlamaları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2d385-154">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="2d385-155">Önceki sürümlerde eksiklikleri</span><span class="sxs-lookup"><span data-stu-id="2d385-155">Shortcomings in previous versions</span></span>
- <span data-ttu-id="2d385-156">EF Core 2.0 sürümünde, sahip olunan varlık sahibi hiyerarşiden açıkça ayrı bir tabloya eşlenmiş sürece, türetilen varlık türleri varlık türleri bildirilemez gezintiler için ait.</span><span class="sxs-lookup"><span data-stu-id="2d385-156">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="2d385-157">Bu sınırlama EF Core 2.1 kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="2d385-157">This limitation has been removed in EF Core 2.1</span></span>
 
### <a name="current-shortcomings"></a><span data-ttu-id="2d385-158">Geçerli eksiklikleri</span><span class="sxs-lookup"><span data-stu-id="2d385-158">Current shortcomings</span></span>
- <span data-ttu-id="2d385-159">İçeren devralma hiyerarşilerini sahip olunan varlık türleri desteklenmez</span><span class="sxs-lookup"><span data-stu-id="2d385-159">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="2d385-160">Sahip olunan varlık türleri, bir koleksiyon gezinme özelliği (yalnızca başvuru gezintiler şu anda desteklenen) tarafından yönlendirilmesi olamaz</span><span class="sxs-lookup"><span data-stu-id="2d385-160">Owned entity types cannot be pointed at by a collection navigation property (only reference navigations are currently supported)</span></span>
- <span data-ttu-id="2d385-161">Gezintiler sahip olduğu için açıkça ayrı bir tabloya sahibinden eşleştirildikleri sürece varlık türleri null olamaz</span><span class="sxs-lookup"><span data-stu-id="2d385-161">Navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span> 
- <span data-ttu-id="2d385-162">(Bu, sahip olunan varlık türleri kullanılarak uygulanamaz değer nesneleri için iyi bilinen bir senaryo) birden çok sahipleri tarafından sahip olunan varlık türleri örneklerini paylaşılamaz.</span><span class="sxs-lookup"><span data-stu-id="2d385-162">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="2d385-163">Tasarım kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="2d385-163">By-design restrictions</span></span>
- <span data-ttu-id="2d385-164">Oluşturamazsınız bir `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="2d385-164">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="2d385-165">Çağıramazsınız `Entity<T>()` üzerinde sahip olunan bir türü `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="2d385-165">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
