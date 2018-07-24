---
title: Varlık türleri - EF Core ait
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: 3eb7480625db4ebc3ce0b7a18d042139f888dab8
ms.sourcegitcommit: 0935ff275ae739243297f5b97eb21414398125c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39201899"
---
# <a name="owned-entity-types"></a>Sahip olunan varlık türleri

>[!NOTE]
> Bu özellik, EF Core 2.0 sürümünde yenidir.

EF Core, diğer varlık türleri Gezinti özellikleri yalnızca görüntülenebilir modeli varlık türleri sağlar. Bunlar adlandırılır _varlık türlerine ait_. Sahip olunan varlık türünü içeren bir varlık, _sahibi_.

## <a name="explicit-configuration"></a>Açık yapılandırma

Sahip olunan varlık türleri hiçbir zaman EF Core tarafından modelde kuralı olarak eklenir. Kullanabileceğiniz `OwnsOne` yöntemi `OnModelCreating` veya açıklama türüyle `OwnedAttribute` (EF Core 2.1 yeni) sahip olunan bir türü türünü yapılandırmak için.

Bu örnekte, StreetAddress hiçbir kimlik özelliği türüdür. Sipariş türünde bir özellik, belirli bir siparişi teslimat adresini belirtmek için kullanılır. İçinde `OnModelCreating`, kullandığımız `OwnsOne` ShippingAddress özelliği siparişini ait bir varlığın olduğunu belirtmek için yöntemi.

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

ShippingAddress özellik siparişini özel ise, dize sürümü kullanabileceğiniz `OwnsOne` yöntemi:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

Bu örnekte, kullandığımız `OwnedAttribute` aynı hedefe ulaşmak için:

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

## <a name="implicit-keys"></a>Örtük anahtarları

EF Core 2.0 ve 2.1, yalnızca başvuru Gezinti özellikleri ait türlerine işaret edebilir. Sahip olunan türler desteklenmez. Bu başvuru ait türleri, her zaman bire bir ilişki sahip olması, bu nedenle anahtar değerlerini gerekmez. Önceki örnekte StreetAddress türü tanımlayan bir anahtar özellik gerekmez.  

EF Core bu nesnelerin nasıl izlediği anlamak için birincil anahtar olarak oluşturduğunuz düşünün yararlıdır bir [gölge özelliği](xref:core/modeling/shadow-properties) ait türü. Sahip olunan türünün bir örneği anahtarın değerini sahibi örneğinin anahtarı değeri ile aynı olacaktır.      

## <a name="mapping-owned-types-with-table-splitting"></a>Tablo bölme ile türleri ait eşleme

İlişkisel veritabanları kullanırken türlerine sahip kurala göre aynı tablonun sahibi olarak eşlenir. Bu iki tablodaki bölünmesini gerektirir: bazı sütunları sahibinin verileri depolamak için kullanılan ve bazı sütunları, sahip olunan varlık verilerini depolamak için kullanılacak. Bu tablo bölme olarak bilinen genel bir özelliğidir.

> [!TIP]
> Tablo bölme depolanan türleri olabilir sahip olduğu için nasıl karmaşık türler içinde EF6 kullanılan çok benzer şekilde kullanılır.

Kural gereği, EF Core düzenini izleyerek sahip olunan varlık türünün özelliklerini veritabanı sütunlarını adlandıracaktır _EntityProperty_OwnedEntityProperty_. Bu nedenle StreetAddress özellikleri, Northwind'deki Siparişler tablosunda ShippingAddress_Street ve ShippingAddress_City adlarla görünür.

Ekleyebilirsiniz `HasColumnName` bu sütunları yeniden adlandırma için yöntemi. Eşlemeleri StreetAddress ortak özelliği olduğu durumda olacaktır

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Aynı .NET türü birden çok ait türleri arasında paylaşma

Sahip olunan varlık türü başka bir sahip olunan varlık türü olarak aynı .NET türü, bu nedenle .NET türü sahip olunan bir tür tanımlamak için yeterli olmayabilir olabilir.

Bu gibi durumlarda, sahip olunan varlık sahibinden işaret eden özelliği haline gelir _Gezinti tanımlama_ sahip olunan varlık türü. EF Core perspektifinden tanımlama Gezinti tipin kimliği .NET türü yanı sıra bir parçasıdır.   

Örneğin, aşağıdaki sınıfında ShippingAddress BillingAddress hem de aynı .NET türü StreetAddress olması:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

EF Core izlenen nesnelerin örneklerini nasıl ayıracaktır anlamak için tanımlama Gezinti değerin yanı sıra sahibinin anahtar örneğinin anahtarının bir parçası ve şirkete ait türün .NET türü hale geldiğini düşünmek yararlı olabilir.

## <a name="nested-owned-types"></a>Sahip olunan iç içe geçmiş türler

Bu örnekte, BillingAddress ve her iki StreetAddress türleri olan ShippingAddress OrderDetails aittir. Ardından OrderDetails sipariş türüne göre aittir.

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

Zincire mümkündür `OwnsOne` yöntemi fluent eşlemesindeki bu modeli yapılandırmak için:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

Kullanarak aynı şeyi elde etmek mümkündür `OwnedAttribute` OrderDetails hem StreetAdress.

Ek olarak sahip olunan iç içe geçmiş türler, sahip olunan bir türü normal bir varlık başvurabilirsiniz. Aşağıdaki örnekte, bir normal ait olmayan varlık ülke verilmiştir:

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Bu özellik, sahip olunan varlık türleri karmaşık türler dışında EF6 ' ayarlar.

## <a name="storing-owned-types-in-separate-tables"></a>Depolama türleri ayrı tablolarda ait

Ayrıca EF6 karmaşık türlerinin aksine, sahip olunan türleri sahibinden ayrı bir tabloda depolanabilir. Aynı tablonun sahibi olarak sahip olunan bir türü eşlendiği kuralını geçersiz kılmak için basitçe çağırabilirsiniz `ToTable` ve farklı bir tablo adı sağlayın. Aşağıdaki örnek OrderDetails ve iki adreslere ayrı bir tabloya siparişi eşler:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Sahip olunan türler sorgulanıyor

Sahibi sorgulanırken ait türleri varsayılan olarak dahil edilir. Kullanmak için gerekli değil `Include` yöntemi olsa da sahibi türleri, ayrı bir tabloda depolanır. Önce belirtilen modeline bağlı olarak, aşağıdaki sorguyu sırası, OrderDetails ve veritabanından tüm bekleyen siparişleri için iki ait StreetAddresses çeker:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Sınırlamalar

Bu sınırlamaların bazıları nasıl sahip olunan varlık türleri iş temel ancak bazıları biz gelecek sürümlerde kaldırmak mümkün olabileceğini kısıtlamaları şunlardır:

### <a name="shortcomings-in-previous-versions"></a>Önceki sürümlerde eksiklikleri
- EF Core 2.0 sürümünde, sahip olunan varlık sahibi hiyerarşiden açıkça ayrı bir tabloya eşlenmiş sürece, türetilen varlık türleri varlık türleri bildirilemez gezintiler için ait. Bu sınırlama EF Core 2.1 kaldırıldı

### <a name="current-shortcomings"></a>Geçerli eksiklikleri
- İçeren devralma hiyerarşilerini sahip olunan varlık türleri desteklenmez
- Sahip olunan varlık türleri, bir koleksiyon gezinme özelliği (yalnızca başvuru gezintiler şu anda desteklenen) tarafından yönlendirilmesi olamaz
- Gezintiler sahip olduğu için açıkça ayrı bir tabloya sahibinden eşleştirildikleri sürece varlık türleri null olamaz
- (Bu, sahip olunan varlık türleri kullanılarak uygulanamaz değer nesneleri için iyi bilinen bir senaryo) birden çok sahipleri tarafından sahip olunan varlık türleri örneklerini paylaşılamaz.

### <a name="by-design-restrictions"></a>Tasarım kısıtlamaları
- Oluşturamazsınız bir `DbSet<T>`
- Çağıramazsınız `Entity<T>()` üzerinde sahip olunan bir türü `ModelBuilder`
