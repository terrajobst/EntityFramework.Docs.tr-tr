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
# <a name="owned-entity-types"></a>Ait varlık türleri

>[!NOTE]
> Bu özelliği EF çekirdek 2. 0'yeni bir özelliktir.

EF çekirdek her zaman sadece diğer varlık türleri Gezinti özelliklerini görünebilir modeli varlık türlerine izin verir. Bunlar adlandırılır _varlık türlerine ait_. Bir ait varlık türünü içeren varlık kendi _sahibi_.

## <a name="explicit-configuration"></a>Açık yapılandırma

Varlık türleri hiçbir zaman EF çekirdek tarafından modelde kurala göre dahil edilen ait. Kullanabileceğiniz `OwnsOne` yönteminde `OnModelCreating` veya türüyle açıklama `OwnedAttribute` (EF çekirdek 2.1 yeni) ait bir tür olarak türünü yapılandırmak için.

Bu örnekte, StreetAddress hiçbir kimlik özelliği türüdür. Sipariş türünde bir özellik, belirli bir sıraya sevkiyat adresini belirtmek için kullanılır. İçinde `OnModelCreating`, kullandığımız `OwnsOne` ShippingAddress özelliği sipariş türünde bir ait varlık olduğunu belirtmek için yöntem.

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

ShippingAddress özelliği sipariş türünde özel ise, dize sürümü kullanabilirsiniz `OwnsOne` yöntemi:

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

EF çekirdek 2.0 ve 2.1, yalnızca başvuru Gezinti özellikleri ait türlerine işaret edebilir. Ait türler desteklenmez. Bu başvuru türleri her zaman bire bir ilişki ile sahip, bu nedenle anahtar değerlerine gerekmeyen ait. Önceki örnekte StreetAddress türü bir anahtar özellik tanımlamak gerekmez.  

EF çekirdek bu nesnelerin nasıl izler anlamak için sırayla bir birincil anahtar olarak oluşturduğunuz düşünmek yararlı olur bir [gölge özelliği](xref:core/modeling/shadow-properties) ait türü. Ait türünün bir örneği anahtarının değerini sahibi örneğinin anahtarının değeri ile aynı olacaktır.      

## <a name="mapping-owned-types-with-table-splitting"></a>Tablo bölme ile türlerine ait eşleme

İlişkisel veritabanları kullanırken türlerine ait kurala göre aynı tablonun sahibi olarak eşlenir. Bu iki tabloda bölme gerektirir: bazı sütunları sahibinin verileri depolamak için kullanılan ve bazı sütunları ait varlık verilerini depolamak için kullanılır. Bu tablo bölme olarak bilinen ortak bir özelliktir.

> [!TIP]
> Tablo bölme depolanan türleri olabilir ait çok benzer bir şekilde nasıl karmaşık türler EF6 kullanılan kullanılır.

Kurala göre EF çekirdek desen aşağıdaki ait varlık türü özellikler için veritabanı sütunlarını adlandıracaktır _EntityProperty_OwnedEntityProperty_. Bu nedenle StreetAddress özellikler ShippingAddress_Street ve ShippingAddress_City adlarıyla Siparişler tablosundaki görünür.

Ekleyebilirsiniz `HasColumnName` bu sütunları yeniden adlandırmak için yöntem. Eşlemeleri StreetAddress genel özelliği olduğu durumda olacaktır

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

Bir ait varlık türü aynı .NET türde başka bir ait varlık türü, bu nedenle .NET türü ait türünü tanımlamak için yeterli olmayabilir olabilir.

Bu gibi durumlarda sahibinden ait varlığa işaret eden özellik olur _Gezinti tanımlama_ ait varlık türü. EF çekirdek perspektifinden tanımlayan gezinmeyi tür kimliği .NET türü yanında bir parçasıdır.   

Örneğin, aşağıdaki sınıfında ShippingAddress ve BillingAddress aynı .NET türü StreetAddress her ikisi de şunlardır:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

EF çekirdek izlenen nesnelerin örneklerini nasıl ayıracaktır olduğunu anlamak için tanımlayan gezinmeyi sahibinin anahtarının değeri yanında örneğinin anahtarının bir parçası ve ait türün .NET türü haline gelmiştir düşünmek yararlı olabilir.

## <a name="nested-owned-types"></a>İç içe geçmiş ait türler

Bu örnekte, Sipariş Ayrıntıları BillingAddress ve her iki StreetAddress türleri ShippingAddress sahip olur. Ardından Sipariş Ayrıntıları sipariş türüne göre aittir.

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

Zinciri mümkündür `OwnsOne` yöntemi fluent eşlemesinde bu modeli yapılandırmak için:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

Kullanarak aynı şey elde mümkündür `OwnedAttribute` sipariş ayrıntıları ve StreetAdress.

İç içe geçmiş ait türlerine ek olarak ait türü normal bir varlığa başvurabilir. Aşağıdaki örnekte, ülke bir normal (yani sahibi olmayan) varlıktır:

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Bu özellik ait varlık türleri karmaşık türler dışında EF6 ayarlar.

## <a name="storing-owned-types-in-separate-tables"></a>Depolama türleri ayrı tablolarda ait

Ayrıca EF6 karmaşık türlerinin aksine ait türleri sahibinden ayrı bir tablo depolanabilir. Sahibi olarak aynı tabloya ait türü eşlemeleri kuralını geçersiz kılmak için yalnızca çağırabilirsiniz `ToTable` ve farklı bir tablo adı sağlayın. Aşağıdaki örnek sipariş ayrıntıları ve iki adreslere ayrı bir tabloya siparişi eşler:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Ait türleri sorgulama

Sahibi sorgulanırken ait türleri varsayılan olarak dahil edilir. Kullanmak gerekli değildir `Include` yöntemi, olsa da ait türü ayrı bir tabloda depolanır. Önce açıklanan modeline bağlı olarak, aşağıdaki sorguyu sırası, Sipariş Ayrıntıları ve veritabanından tüm bekleyen siparişler için iki ait StreeAddresses çeker:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Sınırlamalar

Burada, bazı sınırlamalar ait varlık türlerinin bulunmaktadır. Bu sınırlamalara nasıl ait türleri çalışmaya temel bazıları, ancak bazı diğer gelecek sürümlerde kaldırmak için isteriz zaman içinde nokta kısıtlamaları şunlardır:

### <a name="current-shortcomings"></a>Geçerli eksik
- Devralma ait türleri desteklenmiyor
- Ait türleri konumundaki bir koleksiyon gezinti özelliği tarafından yönlendirilmesi olamaz
- Tablo tarafından bölme kullandığından varsayılan türleri aşağıdaki kısıtlamalar açıkça farklı bir tabloya eşlenen sürece de ait:
   - Türetilmiş bir tür tarafından sahibi olamaz
   - Tanımlayıcı gezinti özelliği ayarlanamıyor null (örneğin aynı tablo türlerinde gerekli her zaman ait)

### <a name="by-design-restrictions"></a>Tasarım kaynaklı kısıtlamaları
- Oluşturamazsınız bir `DbSet<T>`
- Çağıramazsınız `Entity<T>()` ait türüne sahip `ModelBuilder`
