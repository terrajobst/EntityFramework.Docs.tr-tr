---
title: Sahip olan varlık türleri-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: f69bae2de28156876e0aa57376b5dfac053adb9c
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149134"
---
# <a name="owned-entity-types"></a>Sahip olan varlık türleri

>[!NOTE]
> Bu özellik EF Core 2,0 ' de yenidir.

EF Core, yalnızca diğer varlık türlerinin gezinti özelliklerinde görünebilen varlık türlerini modeletmenize olanak tanır. Bunlara sahip olan _varlık türleri_denir. Sahip olduğu varlık türü içeren varlık _sahibi_.

Sahibi olan varlıklar aslında sahibin bir parçasıdır ve bu olmadan mevcut olamaz, bu değerler [toplamalara](https://martinfowler.com/bliki/DDD_Aggregate.html)kavramsal olarak benzerdir.

## <a name="explicit-configuration"></a>Açık yapılandırma

Sahip olan varlık türleri hiçbir şekilde model by kuralına EF Core hiçbir şekilde dahil değildir. Türünü sahip bir tür `OwnsOne` olarak yapılandırmak `OnModelCreating` için içinde yöntemini kullanabilirsiniz `OwnedAttribute` veya (EF Core 2,1 ' de yenidir) yazın.

Bu örnekte, `StreetAddress` Identity özelliği olmayan bir türdür. Bu, belirli bir siparişin sevkiyat adresini belirtmek için sipariş türünün bir özelliği olarak kullanılır.

Başka bir varlık türünden `OwnedAttribute` başvuruluyorsa onu sahip olan bir varlık olarak değerlendirmek için ' i kullanabiliriz:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

Ayrıca, `OwnsOne` `ShippingAddress` özelliğinin `Order` varlık türünün sahip olduğu bir varlık `OnModelCreating` olduğunu belirtmek ve gerekirse ek modeller yapılandırmak için ' de metodunu kullanmak mümkündür.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Özelliği tür içinde Private ise, `OwnsOne` yönteminin dize sürümünü kullanabilirsiniz: `Order` `ShippingAddress`

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Daha fazla bağlam için bkz. [tam örnek proje](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) . 

## <a name="implicit-keys"></a>Örtük anahtarlar

Bir başvuru gezintisi aracılığıyla `OwnsOne` yapılandırılan veya keşfedilen sahipli türler her zaman sahibiyle bire bir ilişkiye sahiptir, bu nedenle yabancı anahtar değerleri benzersiz olduğundan kendi anahtar değerlerine gerek kalmaz. Önceki örnekte, `StreetAddress` türün bir anahtar özelliği tanımlamasına gerek yoktur.  

EF Core bu nesneleri nasıl izlediğini anlamak için, bir birincil anahtarın, sahip olduğu tür için bir [Gölge Özellik](xref:core/modeling/shadow-properties) olarak oluşturulduğunu bilmeniz yararlı olur. Sahip türünün bir örneğinin anahtarı değeri, sahip örneği anahtarının değeri ile aynı olacaktır.

## <a name="collections-of-owned-types"></a>Sahip olunan türlerin koleksiyonları

> [!NOTE]
> Bu özellik EF Core 2,2 ' de yenidir.

`OwnsMany` İçinde`OnModelCreating`kullanım türlerinin bir koleksiyonunu yapılandırmak için.

Sahip olunan türlerin bir birincil anahtar olması gerekir. .NET türünde iyi aday özellikleri yoksa EF Core bir tane oluşturmayı deneyebilir. Bununla birlikte, sahip olunan türler bir koleksiyon aracılığıyla tanımlandığında, bizim için `OwnsOne`yaptığımız gibi, hem yabancı anahtar hem de sahibi olan Örneğin birincil anahtarı olarak davranacak bir gölge özellik oluşturmak için yeterli değildir: her sahip ve bu nedenle sahip anahtarı, sahip olunan her örnek için benzersiz bir kimlik sağlamak için yeterli değildir.

Bunun en kolay iki çözümü şunlardır:
- Yeni bir özellikte, sahibine işaret eden yabancı anahtardan bağımsız olarak bir vekil birincil anahtar tanımlama. İçerilen değerlerin tüm sahiplerin benzersiz olması gerekir ( {1} Örneğin, üst öğe alt {1}öğesi varsa üst {2} öğesi alt {1}öğesi olamaz), bu nedenle değerin hiç bir anlamı yoktur. Yabancı anahtar birincil anahtarın parçası olmadığından, değerleri değiştirilebilir, bu nedenle alt öğeyi bir üst öğeden diğerine taşıyabilirsiniz, ancak bu genellikle toplam semantiğe karşı gider.
- Yabancı anahtar ve ek bir özelliği bileşik anahtar olarak kullanma. Ek özellik değerinin artık yalnızca belirli bir üst öğe için benzersiz olması gerekir (Bu nedenle, üst {1} öğe alt {1,1} öğesi varsa {2} üst öğesi hala alt {2,1}öğeye sahip olabilir). Birincil anahtarın yabancı anahtar bölümünü, sahibi ve sahibi olan varlık arasındaki ilişki sabit hale gelir ve toplam semantiğini daha iyi yansıtır. Varsayılan olarak EF Core budur.

Bu örnekte, `Distributor` sınıfını kullanacağız:

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

Varsayılan olarak `ShippingCenters` , gezinti özelliği `"DistributorId"` `("DistributorId", "Id")` aracılığıyla başvurulan sahip türü için kullanılan birincil anahtar, FK olduğu ve `"Id"` benzersiz `int` bir değerdir.

Farklı bir PK çağrısını `HasKey`yapılandırmak için:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> EF Core 3,0 `WithOwner()` yöntemi olmadığından, bu çağrının kaldırılması gerekir.

## <a name="mapping-owned-types-with-table-splitting"></a>Sahip olunan türleri tablo bölme ile eşleme

İlişkisel veritabanları kullanılırken, varsayılan başvuruya ait türler, sahibiyle aynı tabloyla eşleştirilir. Bu, tablonun iki içinde bölünmesini gerektirir: bazı sütunlar sahibin verilerini depolamak için kullanılacaktır ve bu varlığın verilerini depolamak için bazı sütunlar kullanılacaktır. Bu, [tablo bölme](table-splitting.md)olarak bilinen yaygın bir özelliktir.

Varsayılan olarak EF Core, _Navigation_OwnedEntityProperty_örüntüsünün ardından ait olan varlık türünün özelliklerinin veritabanı sütunlarını adlandırın. Bu nedenle `StreetAddress` , Özellikler ' ShippingAddress_Street ' ve ' ShippingAddress_City ' adlarıyla ' Orders ' tablosunda görünür.

Bu sütunları yeniden adlandırmak `HasColumnName` için yöntemini kullanabilirsiniz:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Aynı .NET türünü sahip olunan birden çok tür arasında paylaşma

Sahip olunan bir varlık türü, sahip olduğu başka bir varlık türüyle aynı .NET türünde olabilir, bu nedenle .NET türü, sahip olunan bir türü belirlemek için yeterli olmayabilir.

Bu gibi durumlarda, sahibi olan varlığa işaret eden özellik sahip olan varlık türü için _gezinme_ haline gelir. EF Core perspektifinden, tanımlama gezintisi .NET türüyle birlikte türün kimliğinin bir parçasıdır.   

Örneğin, aşağıdaki sınıfta `ShippingAddress` ve `BillingAddress` her ikisi de aynı .net türüdür `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

EF Core, bu nesnelerin izlenen örneklerini nasıl ayıracağınızı anlamak için, tanımlama gezintisinin, sahip olduğu türün değeri ve sahip olduğu .NET türü ile birlikte örnek anahtarının bir parçası haline geldiğini düşünmek yararlı olabilir.

## <a name="nested-owned-types"></a>İç içe sahip türler

`OrderDetails` Bu örnekte `BillingAddress` , ve `ShippingAddress`her iki`StreetAddress` türü de vardır. Daha `OrderDetails` sonra `DetailedOrder` türüne aittir.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Sahip olunan iç içe geçmiş türlere ek olarak, sahip olunan bir tür düzenli bir varlığa başvurabilir, sahip olan varlık bağımlı tarafta olduğu sürece sahip veya farklı bir varlık olabilir. Bu yetenek, sahip olduğu varlık türlerini EF6 içindeki karmaşık türlerden ayrı olarak ayarlar.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Bu modeli yapılandırmak için bu `OwnsOne` yöntemi akıcı bir çağrıda zincirlemek mümkündür:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Sahibe doğru işaret eden gezinti özelliğini yapılandırmak için kullanılan çağrıyadikkatedin.`WithOwner`

Hem hem de `OwnedAttribute` `OrderDetails` üzerinde kullanarak sonuca ulaşmak mümkündür. `StreetAdress`

## <a name="storing-owned-types-in-separate-tables"></a>Sahip olunan türleri ayrı tablolarda depolama

Ayrıca, EF6 karmaşık türlerin aksine, sahip olunan türler sahibinden ayrı bir tabloda depolanabilir. Sahip olan bir türü sahip ile aynı tabloya eşleyen kuralı geçersiz kılmak için, yalnızca farklı bir tablo adı çağırıp `ToTable` bu adı sağlayabilirsiniz. Aşağıdaki örnek, ve iki `OrderDetails` adresini öğesinden `DetailedOrder`ayrı bir tabloyla eşleşmeyecektir:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a>Sahip olunan türler sorgulanıyor

Sahibi sorgulanırken sahip olan türler varsayılan olarak dahil edilir. Sahip olan türler ayrı bir tabloda depolansa bile `Include` yöntemini kullanmak gerekli değildir. Daha önce açıklanan modele bağlı olarak aşağıdaki sorgu alınır `Order` `OrderDetails` ve veritabanına ait olan iki sahip `StreetAddresses` olur:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Sınırlamalar

Bu sınırlamaların bazıları, sahip olduğu varlık türlerinin çalışması için temeldir, ancak bazı diğerleri sonraki sürümlerde kaldırabilecekler konusunda daha fazla kısıtlamayla karşılaşabilirsiniz:

### <a name="by-design-restrictions"></a>Tasarıma göre kısıtlamalar
- Sahip olunan bir tür `DbSet<T>` için bir oluşturamazsınız
- Üzerinde sahip olunan `Entity<T>()` bir tür ile çağrılamaz`ModelBuilder`

### <a name="current-shortcomings"></a>Geçerli eksikler
- Sahibi olan varlık türlerini içeren devralma hiyerarşileri desteklenmez
- Sahip olunan varlık türlerine yönelik başvuru gezginleri, sahip tarafından ayrı bir tabloya açık olarak eşlenmediği müddetçe null olamaz
- Sahip varlık türlerinin örnekleri birden çok sahip tarafından paylaşılamaz (Bu, sahip olduğu varlık türleri kullanılarak uygulanamaz değer nesneleri için iyi bilinen bir senaryodur)

### <a name="shortcomings-in-previous-versions"></a>Önceki sürümlerde shortcomler
- EF Core 2,0 ' de, sahip olunan varlıklar sahip hiyerarşisinden ayrı bir tabloya açık bir şekilde eşlenmediği müddetçe, sahip olunan varlık türlerine ait gezintiler türetilmiş varlık türlerinde bildirilemez. EF Core 2,1 ' de bu sınırlama kaldırılmıştır
- EF Core 2,0 ve 2,1 ' de yalnızca sahip olunan türlerin başvuru gezginlerini destekliyordu. EF Core 2,2 ' de bu sınırlama kaldırılmıştır
