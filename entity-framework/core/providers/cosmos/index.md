---
title: Azure Cosmos DB Sağlayıcısı - EF Core
description: Entity Framework Core'un Azure Cosmos DB SQL API ile kullanılmasına olanak tanıyan veritabanı sağlayıcısı için belgeler
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/index
ms.openlocfilehash: 74284bf78f404e376436a1ef5d5933186c85ae49
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417321"
---
# <a name="ef-core-azure-cosmos-db-provider"></a>EF Core Azure Cosmos DB Sağlayıcısı

> [!NOTE]
> Bu sağlayıcı EF Core 3.0'da yenidir.

Bu veritabanı sağlayıcısı, Entity Framework Core'un Azure Cosmos DB ile kullanılmasına izin verir. Sağlayıcı, Varlık Çerçeve Çekirdek [Projesi'nin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak korunur.

Bu bölümü okumadan önce Azure [Cosmos DB belgelerine](/azure/cosmos-db/introduction) alışmanız önemle tavsiye edilir.

> [!NOTE]
> Bu sağlayıcı yalnızca Azure Cosmos DB'nin SQL API'si ile çalışır.

## <a name="install"></a>Yükleme

[Microsoft.EntityFrameworkCore.Cosmos NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/)yükleyin.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a>başlarken

> [!TIP]  
> Bu makalenin [örneğini GitHub'da](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Cosmos)görüntüleyebilirsiniz.

Diğer sağlayıcılar için olduğu gibi ilk adım [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos)aramaktır:

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> Uç nokta ve anahtar basitlik için burada kodlanır, ancak bir üretim uygulamasında bunlar [güvenli bir şekilde depolanmalıdır.](/aspnet/core/security/app-secrets#secret-manager)

Bu `Order` örnekte, [sahip olunan türe](../../modeling/owned-entities.md) `StreetAddress`atıfta bulunan basit bir varlık yer almaktadır.

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

Veri kaydetme ve quering normal EF deseni izler:

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> Gerekli kapsayıcıları oluşturmak ve modelde varsa tohum [verilerini](../../modeling/data-seeding.md) eklemek için [EnsureCreatedAsync'i](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) aramak gereklidir. Ancak, `EnsureCreatedAsync` performans sorunlarına neden olabileceğiiçin, normal çalışma sırasında değil, yalnızca dağıtım sırasında çağrılmalıdır.

## <a name="cosmos-specific-model-customization"></a>Cosmos'a özel model özelleştirmesi

Varsayılan olarak tüm varlık türleri, türetilmiş bağlamdan (bu`"OrderContext"` durumda) sonra adlandırılmış aynı kapsayıcıya eşlenir. Varsayılan kapsayıcı adını değiştirmek için [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer)kullanın:

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Bir varlık türünü farklı bir kapsayıcıyla eşlemek için [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer)kullanın:

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Belirli bir öğenin EF Core'u temsil ettiği varlık türünü tanımlamak için, türetilmiş varlık türü olmasa bile ayrımcı bir değer ekler. Ayırıcının adı ve değeri [değiştirilebilir.](../../modeling/inheritance.md)

Başka bir varlık türü aynı kapta depolanmayacaksa, ayırıcı [HasNoDiscriminator'u](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator)arayarak kaldırılabilir:

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a>Bölüm tuşları

Varsayılan olarak EF Core, öğeleri eklerken `"__partitionKey"` bunun için herhangi bir değer sağlamadan bölüm anahtarı ayarlanmış kapsayıcılar oluşturur. Ancak Azure Cosmos'un performans özelliklerinden tam olarak yararlanmak için özenle seçilmiş bir [bölüm anahtarı](/azure/cosmos-db/partition-data) kullanılmalıdır. [Bu HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey)arayarak yapılandırılabilir:

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
>Bölüm anahtar özelliği [dize dönüştürüldüğü](xref:core/modeling/value-conversions)sürece herhangi bir tür olabilir.

Bir kez yapılandırılan bölüm anahtar özelliği her zaman null olmayan bir değere sahip olmalıdır. Sorgu verirken, tek bölümyapmak için bir koşul eklenebilir.

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a>Gömülü varlıklar

Çünkü Cosmos'a ait varlıklar sahibiyle aynı öğeye gömülür. Bir özellik adını değiştirmek için [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty)kullanın:

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

Bu yapılandırma ile yukarıdaki örnekten gelen sipariş aşağıdaki gibi depolanır:

``` json
{
    "Id": 1,
    "PartitionKey": "1",
    "TrackingNumber": null,
    "id": "1",
    "Address": {
        "ShipsToCity": "London",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

Sahip olunan varlıkların koleksiyonları da gömülür. Bir sonraki örnek için `Distributor` bir koleksiyon ile `StreetAddress`sınıf kullanacağız:

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

Sahip olunan varlıkların depolanacak açık anahtar değerleri sağlaması gerekmez:

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

Bu şekilde devam edeceklerdir:

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
            "Street": "5650 Dolly Ave"
        }
    ],
    "_rid": "6QEKANzISj0BAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKANzISj0=/docs/6QEKANzISj0BAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-7b2b439701d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163705
}
```

Dahili OLARAK EF Core'un izlenen tüm varlıklar için her zaman benzersiz anahtar değerlere sahip olması gerekir. Sahip olunan türlerin koleksiyonları için varsayılan olarak oluşturulan birincil anahtar, sahibini `int` gösteren yabancı anahtar özellikleri ve JSON dizisinde dizideki diziye karşılık gelen bir özellikten oluşur. Bu değerler giriş API almak için kullanılabilir:

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> Gerektiğinde sahip olunan varlık türleri için varsayılan birincil anahtar değiştirilebilir, ancak daha sonra anahtar değerleri açıkça sağlanmalıdır.

## <a name="working-with-disconnected-entities"></a>Bağlantısı kesilen varlıklarla çalışma

Her öğenin verilen `id` bölüm anahtarı için benzersiz bir değere sahip olması gerekir. Varsayılan olarak EF Core, ayırıcıyı ve birincil anahtar değerlerini bir sınırlayıcı olarak '|' kullanarak bir değeri biraraya gelir. Anahtar değerler yalnızca bir varlık `Added` duruma girdiğinde oluşturulur. Değeri depolamak için .NET türünde bir `id` özelliği [yoksa, varlıkları takarken](../../saving/disconnected-entities.md) sorun yaratabilir.

Bu sınırlamayı `id` aşmak için değer el ile oluşturulabilir ve ayarlanabilir veya varlığı önce eklenen olarak işaretleyebilir, sonra istenilen duruma göre değiştirebiliriz:

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

Bu, ortaya çıkan JSON:

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Street": "500 S 48th Street"
        }
    ],
    "_rid": "JBwtAN8oNYEBAAAAAAAAAA==",
    "_self": "dbs/JBwtAA==/colls/JBwtAN8oNYE=/docs/JBwtAN8oNYEBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-9377-d7a1ae7c01d5\"",
    "_attachments": "attachments/",
    "_ts": 1572917100
}
```
