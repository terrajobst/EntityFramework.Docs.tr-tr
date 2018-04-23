---
title: EF çekirdek varlık türleri - ait
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: f2f05499a3e3494f420d916df2db19667a6f1e29
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="owned-entity-types"></a><span data-ttu-id="261f9-102">Ait varlık türleri</span><span class="sxs-lookup"><span data-stu-id="261f9-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="261f9-103">Bu özelliği EF çekirdek 2. 0'yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="261f9-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="261f9-104">EF çekirdek her zaman sadece diğer varlık türleri Gezinti özelliklerini görünebilir modeli varlık türlerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="261f9-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="261f9-105">Bunlar adlandırılır _varlık türlerine ait_.</span><span class="sxs-lookup"><span data-stu-id="261f9-105">These are called _owned entity types_.</span></span> <span data-ttu-id="261f9-106">Bir ait varlık türünü içeren varlık kendi _sahibi_.</span><span class="sxs-lookup"><span data-stu-id="261f9-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="261f9-107">Açık yapılandırma</span><span class="sxs-lookup"><span data-stu-id="261f9-107">Explicit configuration</span></span>

<span data-ttu-id="261f9-108">Varlık türleri hiçbir zaman EF çekirdek tarafından modelde kurala göre dahil edilen ait.</span><span class="sxs-lookup"><span data-stu-id="261f9-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="261f9-109">Kullanabileceğiniz `OwnsOne` yönteminde `OnModelCreating` veya türüyle açıklama `OwnedAttribute` (EF çekirdek 2.1 yeni) ait bir tür olarak türünü yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="261f9-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="261f9-110">Bu örnekte, StreetAddress hiçbir kimlik özelliği türüdür.</span><span class="sxs-lookup"><span data-stu-id="261f9-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="261f9-111">Sipariş türünde bir özellik, belirli bir sıraya sevkiyat adresini belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="261f9-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="261f9-112">İçinde `OnModelCreating`, kullandığımız `OwnsOne` ShippingAddress özelliği sipariş türünde bir ait varlık olduğunu belirtmek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="261f9-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

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

<span data-ttu-id="261f9-113">ShippingAddress özelliği sipariş türünde özel ise, dize sürümü kullanabilirsiniz `OwnsOne` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="261f9-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="261f9-114">Bu örnekte, kullandığımız `OwnedAttribute` aynı hedefe ulaşmak için:</span><span class="sxs-lookup"><span data-stu-id="261f9-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

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

## <a name="implicit-keys"></a><span data-ttu-id="261f9-115">Örtük anahtarları</span><span class="sxs-lookup"><span data-stu-id="261f9-115">Implicit keys</span></span>

<span data-ttu-id="261f9-116">EF çekirdek 2.0 ve 2.1, yalnızca başvuru Gezinti özellikleri ait türlerine işaret edebilir.</span><span class="sxs-lookup"><span data-stu-id="261f9-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="261f9-117">Ait türler desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="261f9-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="261f9-118">Bu başvuru türleri her zaman bire bir ilişki ile sahip, bu nedenle anahtar değerlerine gerekmeyen ait.</span><span class="sxs-lookup"><span data-stu-id="261f9-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="261f9-119">Önceki örnekte StreetAddress türü bir anahtar özellik tanımlamak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="261f9-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="261f9-120">EF çekirdek bu nesnelerin nasıl izler anlamak için sırayla bir birincil anahtar olarak oluşturduğunuz düşünmek yararlı olur bir [gölge özelliği](xref:core/modeling/shadow-properties) ait türü.</span><span class="sxs-lookup"><span data-stu-id="261f9-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="261f9-121">Ait türünün bir örneği anahtarının değerini sahibi örneğinin anahtarının değeri ile aynı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="261f9-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="261f9-122">Tablo bölme ile türlerine ait eşleme</span><span class="sxs-lookup"><span data-stu-id="261f9-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="261f9-123">İlişkisel veritabanları kullanırken türlerine ait kurala göre aynı tablonun sahibi olarak eşlenir.</span><span class="sxs-lookup"><span data-stu-id="261f9-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="261f9-124">Bu iki tabloda bölme gerektirir: bazı sütunları sahibinin verileri depolamak için kullanılan ve bazı sütunları ait varlık verilerini depolamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="261f9-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="261f9-125">Bu tablo bölme olarak bilinen ortak bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="261f9-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="261f9-126">Tablo bölme depolanan türleri olabilir ait çok benzer bir şekilde nasıl karmaşık türler EF6 kullanılan kullanılır.</span><span class="sxs-lookup"><span data-stu-id="261f9-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="261f9-127">Kurala göre EF çekirdek desen aşağıdaki ait varlık türü özellikler için veritabanı sütunlarını adlandıracaktır _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="261f9-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="261f9-128">Bu nedenle StreetAddress özellikler ShippingAddress_Street ve ShippingAddress_City adlarıyla Siparişler tablosundaki görünür.</span><span class="sxs-lookup"><span data-stu-id="261f9-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="261f9-129">Ekleyebilirsiniz `HasColumnName` bu sütunları yeniden adlandırmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="261f9-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="261f9-130">Eşlemeleri StreetAddress genel özelliği olduğu durumda olacaktır</span><span class="sxs-lookup"><span data-stu-id="261f9-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="261f9-131">Aynı .NET türü birden çok ait türleri arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="261f9-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="261f9-132">Bir ait varlık türü aynı .NET türde başka bir ait varlık türü, bu nedenle .NET türü ait türünü tanımlamak için yeterli olmayabilir olabilir.</span><span class="sxs-lookup"><span data-stu-id="261f9-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="261f9-133">Bu gibi durumlarda sahibinden ait varlığa işaret eden özellik olur _Gezinti tanımlama_ ait varlık türü.</span><span class="sxs-lookup"><span data-stu-id="261f9-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="261f9-134">EF çekirdek perspektifinden tanımlayan gezinmeyi tür kimliği .NET türü yanında bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="261f9-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="261f9-135">Örneğin, aşağıdaki sınıfında ShippingAddress ve BillingAddress aynı .NET türü StreetAddress her ikisi de şunlardır:</span><span class="sxs-lookup"><span data-stu-id="261f9-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="261f9-136">EF çekirdek izlenen nesnelerin örneklerini nasıl ayıracaktır olduğunu anlamak için tanımlayan gezinmeyi sahibinin anahtarının değeri yanında örneğinin anahtarının bir parçası ve ait türün .NET türü haline gelmiştir düşünmek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="261f9-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="261f9-137">İç içe geçmiş ait türler</span><span class="sxs-lookup"><span data-stu-id="261f9-137">Nested owned types</span></span>

<span data-ttu-id="261f9-138">Bu örnekte, Sipariş Ayrıntıları BillingAddress ve her iki StreetAddress türleri ShippingAddress sahip olur.</span><span class="sxs-lookup"><span data-stu-id="261f9-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="261f9-139">Ardından Sipariş Ayrıntıları sipariş türüne göre aittir.</span><span class="sxs-lookup"><span data-stu-id="261f9-139">Then OrderDetails is owned by the Order type.</span></span>

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

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="261f9-140">Zinciri mümkündür `OwnsOne` yöntemi fluent eşlemesinde bu modeli yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="261f9-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="261f9-141">Kullanarak aynı şey elde mümkündür `OwnedAttribute` sipariş ayrıntıları ve StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="261f9-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="261f9-142">İç içe geçmiş ait türlerine ek olarak ait türü normal bir varlığa başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="261f9-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="261f9-143">Aşağıdaki örnekte, ülke bir normal (yani sahibi olmayan) varlıktır:</span><span class="sxs-lookup"><span data-stu-id="261f9-143">In the following example, Country is a regular (i.e. non-owned) entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="261f9-144">Bu özellik ait varlık türleri karmaşık türler dışında EF6 ayarlar.</span><span class="sxs-lookup"><span data-stu-id="261f9-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="261f9-145">Depolama türleri ayrı tablolarda ait</span><span class="sxs-lookup"><span data-stu-id="261f9-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="261f9-146">Ayrıca EF6 karmaşık türlerinin aksine ait türleri sahibinden ayrı bir tablo depolanabilir.</span><span class="sxs-lookup"><span data-stu-id="261f9-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="261f9-147">Sahibi olarak aynı tabloya ait türü eşlemeleri kuralını geçersiz kılmak için yalnızca çağırabilirsiniz `ToTable` ve farklı bir tablo adı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="261f9-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="261f9-148">Aşağıdaki örnek sipariş ayrıntıları ve iki adreslere ayrı bir tabloya siparişi eşler:</span><span class="sxs-lookup"><span data-stu-id="261f9-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="261f9-149">Ait türleri sorgulama</span><span class="sxs-lookup"><span data-stu-id="261f9-149">Querying owned types</span></span>

<span data-ttu-id="261f9-150">Sahibi sorgulanırken ait türleri varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="261f9-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="261f9-151">Kullanmak gerekli değildir `Include` yöntemi, olsa da ait türü ayrı bir tabloda depolanır.</span><span class="sxs-lookup"><span data-stu-id="261f9-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="261f9-152">Önce açıklanan modeline bağlı olarak, aşağıdaki sorguyu sırası, Sipariş Ayrıntıları ve veritabanından tüm bekleyen siparişler için iki ait StreeAddresses çeker:</span><span class="sxs-lookup"><span data-stu-id="261f9-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreeAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="261f9-153">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="261f9-153">Limitations</span></span>

<span data-ttu-id="261f9-154">Burada, bazı sınırlamalar ait varlık türlerinin bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="261f9-154">Here are some limitations of owned entity types.</span></span> <span data-ttu-id="261f9-155">Bu sınırlamalara nasıl ait türleri çalışmaya temel bazıları, ancak bazı diğer gelecek sürümlerde kaldırmak için isteriz zaman içinde nokta kısıtlamaları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="261f9-155">Some of these limitations are fundamental to how owned types work, but some others are point-in-time restrictions that we would like to remove in future releases:</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="261f9-156">Geçerli eksik</span><span class="sxs-lookup"><span data-stu-id="261f9-156">Current shortcomings</span></span>
- <span data-ttu-id="261f9-157">Devralma ait türleri desteklenmiyor</span><span class="sxs-lookup"><span data-stu-id="261f9-157">Inheritance of owned types is not supported</span></span>
- <span data-ttu-id="261f9-158">Ait türleri konumundaki bir koleksiyon gezinti özelliği tarafından yönlendirilmesi olamaz</span><span class="sxs-lookup"><span data-stu-id="261f9-158">Owned types cannot be pointed at by a collection navigation property</span></span>
- <span data-ttu-id="261f9-159">Tablo tarafından bölme kullandığından varsayılan türleri aşağıdaki kısıtlamalar açıkça farklı bir tabloya eşlenen sürece de ait:</span><span class="sxs-lookup"><span data-stu-id="261f9-159">Since they use table splitting by default owned types also have the following restrictions unless explicitly mapped to a different table:</span></span>
   - <span data-ttu-id="261f9-160">Türetilmiş bir tür tarafından sahibi olamaz</span><span class="sxs-lookup"><span data-stu-id="261f9-160">They cannot be owned by a derived type</span></span>
   - <span data-ttu-id="261f9-161">Tanımlayıcı gezinti özelliği ayarlanamıyor null (örneğin aynı tablo türlerinde gerekli her zaman ait)</span><span class="sxs-lookup"><span data-stu-id="261f9-161">The defining navigation property cannot be set to null (i.e. owned types on the same table are always required)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="261f9-162">Tasarım kaynaklı kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="261f9-162">By-design restrictions</span></span>
- <span data-ttu-id="261f9-163">Oluşturamazsınız bir `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="261f9-163">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="261f9-164">Çağıramazsınız `Entity<T>()` ait türüne sahip `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="261f9-164">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
