---
title: Varlık türleri - EF Core ait
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: b2d72b08de79939904bf4e726c695440c906a8aa
ms.sourcegitcommit: 06073f8efde97dd5f540dbfb69f574d8380566fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67333790"
---
# <a name="owned-entity-types"></a>Sahip olunan varlık türleri

>[!NOTE]
> Bu özellik, EF Core 2.0 sürümünde yenidir.

EF Core, diğer varlık türleri Gezinti özellikleri yalnızca görüntülenebilir modeli varlık türleri sağlar. Bunlar adlandırılır _varlık türlerine ait_. Sahip olunan varlık türünü içeren bir varlık, _sahibi_.

## <a name="explicit-configuration"></a>Açık yapılandırma

Sahip olunan varlık türleri hiçbir zaman EF Core tarafından modelde kuralı olarak eklenir. Kullanabileceğiniz `OwnsOne` yöntemi `OnModelCreating` veya açıklama türüyle `OwnedAttribute` (EF Core 2.1 yeni) sahip olunan bir türü türünü yapılandırmak için.

Bu örnekte, `StreetAddress` hiçbir kimlik özelliğine sahip bir tür. Sipariş türünde bir özellik, belirli bir siparişi teslimat adresini belirtmek için kullanılır.

Kullanabiliriz `OwnedAttribute` başka bir varlık türü, başvurulduğunda sahip olunan bir varlık olarak değerlendirmek için:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

Kullanmak da mümkündür `OwnsOne` yönteminde `OnModelCreating` belirtmek için `ShippingAddress` özelliktir ait bir varlığın `Order` varlık türü ve gerekirse ek özellikleri yapılandırmak için.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Varsa `ShippingAddress` özelliği, özel `Order` türü, dize sürümü kullanabilirsiniz `OwnsOne` yöntemi:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Bkz: [tam örnek proje](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) daha fazla bağlam için. 

## <a name="implicit-keys"></a>Örtük anahtarları

Sahip yapılandırılmış türleri `OwnsOne` veya bir başvuru gezinmeyi bulunan her zaman sahibiyle bire bir ilişki varsa, bu nedenle yabancı anahtar değerlerinin benzersiz olduğu gibi anahtar değerlerini gerekmez. Önceki örnekte, `StreetAddress` türü tanımlayan bir anahtar özellik gerekmez.  

EF Core bu nesnelerin nasıl izlediği anlamak için birincil anahtar olarak oluşturduğunuz düşünün yararlıdır bir [gölge özelliği](xref:core/modeling/shadow-properties) ait türü. Sahip olunan türünün bir örneği anahtarın değerini sahibi örneğinin anahtarı değeri ile aynı olacaktır.

## <a name="collections-of-owned-types"></a>Sahip olunan türler

>[!NOTE]
> Bu özellik, EF Core 2.2 içinde yeni bir özelliktir.

Sahip olunan türleri koleksiyonu yapılandırmak için `OwnsMany` kullanılmalıdır `OnModelCreating`. Ancak bunu açıkça belirtilmesi gerekir, böylece birincil anahtarı Kural gereği, yapılandırılmaz. Bu tür bir sahibi ve de gölge durumda olabilecek ek benzersiz bir özellik için yabancı anahtarı bir araya getiren varlıklar karmaşık bir anahtar kullanmak yaygın bir uygulamadır:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a>Tablo bölme ile türleri ait eşleme

İlişkisel kullanırken veritabanları aynı tablonun sahibi olarak türlerin eşlendiği kuralı başvuruya göre ait. Bu iki tablodaki bölünmesini gerektirir: bazı sütunları sahibinin verileri depolamak için kullanılan ve bazı sütunları, sahip olunan varlık verilerini depolamak için kullanılacak. Bu tablo bölme olarak bilinen genel bir özelliğidir.

> [!TIP]
> Tablo bölme depolanan türleri olabilir ait EF6 içinde nasıl karmaşık türler için kullanılan benzer şekilde kullanılır.

Kural gereği, EF Core düzenini izleyerek sahip olunan varlık türünün özelliklerini veritabanı sütunlarını adlandıracaktır _Navigation_OwnedEntityProperty_. Bu nedenle `StreetAddress` özellikleri adları 'ShippingAddress_Street' ve 'ShippingAddress_City' ile 'Siparişler' tablosunda görünür.

Ekleyebilirsiniz `HasColumnName` yönteminin bu sütunları yeniden adlandırın:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Aynı .NET türü birden çok ait türleri arasında paylaşma

Sahip olunan varlık türü başka bir sahip olunan varlık türü olarak aynı .NET türü, bu nedenle .NET türü sahip olunan bir tür tanımlamak için yeterli olmayabilir olabilir.

Bu gibi durumlarda, sahip olunan varlık sahibinden işaret eden özelliği haline gelir _Gezinti tanımlama_ sahip olunan varlık türü. EF Core perspektifinden tanımlama Gezinti tipin kimliği .NET türü yanı sıra bir parçasıdır.   

Örneğin, aşağıdaki sınıfında `ShippingAddress` ve `BillingAddress` aynı .NET türü, her ikisi de `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

EF Core izlenen nesnelerin örneklerini nasıl ayıracaktır anlamak için tanımlama Gezinti değerin yanı sıra sahibinin anahtar örneğinin anahtarının bir parçası ve şirkete ait türün .NET türü hale geldiğini düşünmek yararlı olabilir.

## <a name="nested-owned-types"></a>Sahip olunan iç içe geçmiş türler

Bu örnekte `OrderDetails` sahibi `BillingAddress` ve `ShippingAddress`, her ikisi de olduğu `StreetAddress` türleri. Ardından `OrderDetails` tarafından sahip olunan `DetailedOrder` türü.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Sahip olunan iç içe geçmiş türler ek olarak sahip olunan bir türü normal bir varlığa başvurabilir, sahip olunan varlık bağımlı tarafında olduğu sürece sahibi ya da farklı bir varlığın olabilir. Bu özellik, sahip olunan varlık türleri karmaşık türler dışında EF6 ' ayarlar.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Zincire mümkündür `OwnsOne` yöntemi fluent çağrıda bu modeli yapılandırmak için:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Kullanarak aynı şeyi elde etmek mümkündür `OwnedAttribute` hem de `OrderDetails` ve `StreetAdress`.

## <a name="storing-owned-types-in-separate-tables"></a>Depolama türleri ayrı tablolarda ait

Ayrıca EF6 karmaşık türlerinin aksine, sahip olunan türleri sahibinden ayrı bir tabloda depolanabilir. Aynı tablonun sahibi olarak sahip olunan bir türü eşlendiği kuralını geçersiz kılmak için basitçe çağırabilirsiniz `ToTable` ve farklı bir tablo adı sağlayın. Aşağıdaki örnek eşler `OrderDetails` ve bunun iki ayrı tablodan adreslere `DetailedOrder`:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a>Sahip olunan türler sorgulanıyor

Sahibi sorgulanırken ait türleri varsayılan olarak dahil edilir. Kullanmak için gerekli değil `Include` yöntemi olsa da sahibi türleri, ayrı bir tabloda depolanır. Önce belirtilen modeline bağlı olarak, aşağıdaki sorguyu elde edersiniz `Order`, `OrderDetails` ait ve iki `StreetAddresses` veritabanından:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Sınırlamalar

Bu sınırlamaların bazıları nasıl sahip olunan varlık türleri iş temel ancak bazıları biz gelecek sürümlerde kaldırmak mümkün olabileceğini kısıtlamaları şunlardır:

### <a name="by-design-restrictions"></a>Tasarım kısıtlamaları
- Oluşturamazsınız bir `DbSet<T>` sahip olunan bir türü için
- Çağıramazsınız `Entity<T>()` üzerinde sahip olunan bir türü `ModelBuilder`

### <a name="current-shortcomings"></a>Geçerli eksiklikleri
- İçeren devralma hiyerarşilerini sahip olunan varlık türleri desteklenmez
- Başvuru gezintiler için sahip olduğu açıkça ayrı bir tabloya sahibinden eşleştirildikleri sürece varlık türleri null olamaz
- (Bu, sahip olunan varlık türleri kullanılarak uygulanamaz değer nesneleri için iyi bilinen bir senaryo) birden çok sahipleri tarafından sahip olunan varlık türleri örneklerini paylaşılamaz.

### <a name="shortcomings-in-previous-versions"></a>Önceki sürümlerde eksiklikleri
- EF Core 2.0 sürümünde, sahip olunan varlık sahibi hiyerarşiden açıkça ayrı bir tabloya eşlenmiş sürece, türetilen varlık türleri varlık türleri bildirilemez gezintiler için ait. Bu sınırlama EF Core 2.1 kaldırıldı
- EF Core 2.0 ve 2.1 yalnızca başvuru türlerine ait gezintiler desteklendi. Bu sınırlama EF Core 2.2 kaldırıldı
