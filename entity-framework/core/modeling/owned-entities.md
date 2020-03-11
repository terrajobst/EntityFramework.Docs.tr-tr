---
title: Sahip olan varlık türleri-EF Core
description: Entity Framework Core kullanırken sahip olan varlık türlerini veya toplamları yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/owned-entities
ms.openlocfilehash: da4a459fbc40010fc14190204c8ed66fe0495b84
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416462"
---
# <a name="owned-entity-types"></a>Sahip Olunan Varlık Türleri

EF Core, yalnızca diğer varlık türlerinin gezinti özelliklerinde görünebilen varlık türlerini modeletmenize olanak tanır. Bunlara sahip olan _varlık türleri_denir. Sahip olduğu varlık türü içeren varlık _sahibi_.

Sahibi olan varlıklar aslında sahibin bir parçasıdır ve bu olmadan mevcut olamaz, bu değerler [toplamalara](https://martinfowler.com/bliki/DDD_Aggregate.html)kavramsal olarak benzerdir. Bu, sahip olduğu varlığın, sahip olduğu ilişkinin bağımlı tarafında tanımına göre olduğu anlamına gelir.

## <a name="explicit-configuration"></a>Açık yapılandırma

Sahip olan varlık türleri hiçbir şekilde model by kuralına EF Core hiçbir şekilde dahil değildir. Türü sahip bir tür olarak yapılandırmak için `OnModelCreating` `OwnsOne` yöntemi veya `OwnedAttribute` (EF Core 2,1 ' de yeni) ekleyebilirsiniz.

Bu örnekte, `StreetAddress` Identity özelliği olmayan bir türdür. Bu, belirli bir siparişin sevkiyat adresini belirtmek için sipariş türünün bir özelliği olarak kullanılır.

`OwnedAttribute` başka bir varlık türünden başvuruluyorsa sahip olan bir varlık olarak değerlendirmek için kullanabiliriz:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

Ayrıca, `ShippingAddress` özelliğinin `Order` varlık türüne ait bir varlık olduğunu belirtmek ve gerekirse ek modelleri yapılandırmak için `OnModelCreating` `OwnsOne` yöntemini kullanabilirsiniz.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

`ShippingAddress` özelliği `Order` türünde özel ise, `OwnsOne` yönteminin dize sürümünü kullanabilirsiniz:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Daha fazla bağlam için bkz. [tam örnek proje](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) .

## <a name="implicit-keys"></a>Örtük anahtarlar

`OwnsOne` ile yapılandırılmış veya başvuru gezintisi aracılığıyla keşfedilen sahip olan türler her zaman sahibiyle bire bir ilişkiye sahiptir, bu nedenle yabancı anahtar değerleri benzersiz olduğundan kendi anahtar değerlerine gerek kalmaz. Önceki örnekte, `StreetAddress` türünün bir anahtar özelliği tanımlamasına gerek yoktur.  

EF Core bu nesneleri nasıl izlediğini anlamak için, bir birincil anahtarın, sahip olduğu tür için bir [Gölge Özellik](xref:core/modeling/shadow-properties) olarak oluşturulduğunu bilmeniz yararlı olur. Sahip türünün bir örneğinin anahtarı değeri, sahip örneği anahtarının değeri ile aynı olacaktır.

## <a name="collections-of-owned-types"></a>Sahip olunan türlerin koleksiyonları

> [!NOTE]
> Bu özellik EF Core 2,2 ' de yenidir.

Sahip olunan türlerin bir koleksiyonunu yapılandırmak için `OnModelCreating``OwnsMany` kullanın.

Sahip olunan türlerin bir birincil anahtar olması gerekir. .NET türünde iyi aday özellikleri yoksa EF Core bir tane oluşturmayı deneyebilir. Bununla birlikte, sahip olunan türler bir koleksiyon aracılığıyla tanımlandığında, `OwnsOne`için yaptığımız gibi, hem yabancı anahtar hem de sahip olunan birincil anahtar olarak davranmak için bir gölge özellik oluşturmak yeterli değildir: her bir sahip için birden çok sahip tür örneği olabilir ve bu nedenle sahip anahtarı, sahip olduğu her bir örnek için benzersiz bir kimlik sağlamak için yeterli değildir.

Bunun en kolay iki çözümü şunlardır:

- Yeni bir özellikte, sahibine işaret eden yabancı anahtardan bağımsız olarak bir vekil birincil anahtar tanımlama. İçerilen değerlerin tüm sahiplerin benzersiz olması gerekir (örneğin, üst {1} alt {1}varsa, üst {2} alt {1}olamaz), bu nedenle değerin hiç bir anlamı yoktur. Yabancı anahtar birincil anahtarın parçası olmadığından, değerleri değiştirilebilir, bu nedenle alt öğeyi bir üst öğeden diğerine taşıyabilirsiniz, ancak bu genellikle toplam semantiğe karşı gider.
- Yabancı anahtar ve ek bir özelliği bileşik anahtar olarak kullanma. Ek özellik değerinin artık yalnızca belirli bir üst öğe için benzersiz olması gerekir (Bu nedenle, üst {1} alt {1,1} varsa, üst {2} hala alt {2,1}olabilir). Birincil anahtarın yabancı anahtar bölümünü, sahibi ve sahibi olan varlık arasındaki ilişki sabit hale gelir ve toplam semantiğini daha iyi yansıtır. Varsayılan olarak EF Core budur.

Bu örnekte `Distributor` sınıfını kullanacağız:

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

Varsayılan olarak, `ShippingCenters` gezinti özelliği aracılığıyla başvurulan sahip türü için kullanılan birincil anahtar, `"DistributorId"` FK olduğu ve `"Id"` benzersiz bir `int` değeri olduğu `("DistributorId", "Id")` olacaktır.

Farklı bir PK çağrısı `HasKey`yapılandırmak için:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> EF Core 3,0 `WithOwner()` yöntemi mevcut olmadığı için bu çağrının kaldırılması gerekir. Ayrıca birincil anahtar otomatik olarak keşfedilmemiş, her zaman belirtilmelidir.

## <a name="mapping-owned-types-with-table-splitting"></a>Sahip olunan türleri tablo bölme ile eşleme

İlişkisel veritabanları kullanılırken, varsayılan başvuruya ait türler, sahibiyle aynı tabloyla eşleştirilir. Bu, tablonun iki içinde bölünmesini gerektirir: bazı sütunlar sahibin verilerini depolamak için kullanılacaktır ve bu varlığın verilerini depolamak için bazı sütunlar kullanılacaktır. Bu, [tablo bölme](table-splitting.md)olarak bilinen yaygın bir özelliktir.

Varsayılan olarak, EF Core, model _Navigation_OwnedEntityProperty_takip eden varlık türünün özelliklerine ait veritabanı sütunlarını adı verecek. Bu nedenle `StreetAddress` özellikleri ' Orders ' tablosunda ' ShippingAddress_Street ' ve ' ShippingAddress_City ' adlarına sahip olacak şekilde görünür.

Bu sütunları yeniden adlandırmak için `HasColumnName` yöntemini kullanabilirsiniz:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

> [!NOTE]
> [Yok sayma](/dotnet/api/microsoft.entityframeworkcore.metadata.builders.ownednavigationbuilder.ignore) gibi normal varlık türü yapılandırma yöntemlerinin çoğu aynı şekilde çağrılabilir.

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Aynı .NET türünü sahip olunan birden çok tür arasında paylaşma

Sahip olunan bir varlık türü, sahip olduğu başka bir varlık türüyle aynı .NET türünde olabilir, bu nedenle .NET türü, sahip olunan bir türü belirlemek için yeterli olmayabilir.

Bu gibi durumlarda, sahibi olan varlığa işaret eden özellik sahip olan varlık türü için _gezinme_ haline gelir. EF Core perspektifinden, tanımlama gezintisi .NET türüyle birlikte türün kimliğinin bir parçasıdır.

Örneğin, aşağıdaki sınıfta `ShippingAddress` ve `BillingAddress` aynı .NET türüdür `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

EF Core, bu nesnelerin izlenen örneklerini nasıl ayıracağınızı anlamak için, tanımlama gezintisinin, sahip olduğu türün değeri ve sahip olduğu .NET türü ile birlikte örnek anahtarının bir parçası haline geldiğini düşünmek yararlı olabilir.

## <a name="nested-owned-types"></a>İç içe sahip türler

Bu örnekte, hem `StreetAddress` türü olan `BillingAddress` ve `ShippingAddress`sahip `OrderDetails`. `OrderDetails`, `DetailedOrder` türüne aittir.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Sahip olan bir türe her gezinti, tamamen bağımsız yapılandırmayla ayrı bir varlık türü tanımlar.

Sahip olunan iç içe geçmiş türlere ek olarak, sahip olunan bir tür düzenli bir varlığa başvurabilir, sahip olan varlık bağımlı tarafta olduğu sürece sahip veya farklı bir varlık olabilir. Bu yetenek, sahip olduğu varlık türlerini EF6 içindeki karmaşık türlerden ayrı olarak ayarlar.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Bu modeli yapılandırmak için `OwnsOne` yönteminin akıcı bir çağrıda zinciri oluşturulabilir:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Sahibe doğru işaret eden gezinti özelliğini yapılandırmak için kullanılan `WithOwner` çağrısına dikkat edin. Sahiplik ilişkisinin parçası olmayan sahip varlık türü için bir gezinti yapılandırmak üzere `WithOwner()` herhangi bir bağımsız değişken olmadan çağrılmalıdır.

`OrderDetails` ve `StreetAddress``OwnedAttribute` kullanarak sonuca ulaşmak mümkündür.

## <a name="storing-owned-types-in-separate-tables"></a>Sahip olunan türleri ayrı tablolarda depolama

Ayrıca, EF6 karmaşık türlerin aksine, sahip olunan türler sahibinden ayrı bir tabloda depolanabilir. Sahip olan bir türü sahip ile aynı tabloya eşleyen kuralı geçersiz kılmak için, yalnızca `ToTable` çağırıp farklı bir tablo adı sağlayabilirsiniz. Aşağıdaki örnek `OrderDetails` ve iki adresini `DetailedOrder`olan ayrı bir tabloyla eşleyebilir:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

Bunu gerçekleştirmek için `TableAttribute` kullanmak da mümkündür, ancak bu, sahip olan türde birden çok gezinmeler varsa, bu durumda birden çok varlık türünün aynı tabloyla eşlenmesinin ardından başarısız olacağını unutmayın.

## <a name="querying-owned-types"></a>Sahip olunan türler sorgulanıyor

Sahibi sorgulanırken sahip olan türler varsayılan olarak dahil edilir. Sahip olan türler ayrı bir tabloda depolansa bile `Include` yönteminin kullanılması gerekli değildir. Daha önce açıklanan modele bağlı olarak, aşağıdaki sorgu `Order`, `OrderDetails` ve veritabanından gelen `StreetAddresses` sahip olur:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Sınırlamalar

Bu sınırlamaların bazıları, sahip olduğu varlık türlerinin çalışması için temeldir, ancak bazı diğerleri sonraki sürümlerde kaldırabilecekler konusunda daha fazla kısıtlamayla karşılaşabilirsiniz:

### <a name="by-design-restrictions"></a>Tasarıma göre kısıtlamalar

- Sahip olunan bir tür için `DbSet<T>` oluşturamazsınız
- `ModelBuilder` sahip bir tür ile `Entity<T>()` çağıramaz

### <a name="current-shortcomings"></a>Geçerli eksikler

- Sahip olan varlık türlerinde devralma hiyerarşileri olamaz
- Sahip olunan varlık türlerine yönelik başvuru gezginleri, sahip tarafından ayrı bir tabloya açık olarak eşlenmediği müddetçe null olamaz
- Sahip varlık türlerinin örnekleri birden çok sahip tarafından paylaşılamaz (Bu, sahip olduğu varlık türleri kullanılarak uygulanamaz değer nesneleri için iyi bilinen bir senaryodur)

### <a name="shortcomings-in-previous-versions"></a>Önceki sürümlerde shortcomler

- EF Core 2,0 ' de, sahip olunan varlıklar sahip hiyerarşisinden ayrı bir tabloya açık bir şekilde eşlenmediği müddetçe, sahip olunan varlık türlerine ait gezintiler türetilmiş varlık türlerinde bildirilemez. EF Core 2,1 ' de bu sınırlama kaldırılmıştır
- EF Core 2,0 ve 2,1 ' de yalnızca sahip olunan türlerin başvuru gezginlerini destekliyordu. EF Core 2,2 ' de bu sınırlama kaldırılmıştır
