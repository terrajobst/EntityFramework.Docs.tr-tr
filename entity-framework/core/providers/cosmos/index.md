---
title: Azure Cosmos DB sağlayıcı-EF Core
description: Entity Framework Core Azure Cosmos DB SQL API 'siyle kullanılmasına izin veren veritabanı sağlayıcısına yönelik belgeler
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/index
ms.openlocfilehash: 7451ce6e8d5d7078b3f56a6865aa7698e6fc63ca
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888128"
---
# <a name="ef-core-azure-cosmos-db-provider"></a>EF Core Azure Cosmos DB sağlayıcısı

> [!NOTE]
> Bu sağlayıcı EF Core 3,0 ' de yenidir.

Bu veritabanı sağlayıcısı, Azure Cosmos DB birlikte Entity Framework Core kullanılmasına izin verir. Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.

Bu bölümü okumadan önce [Azure Cosmos DB belgelerini](/azure/cosmos-db/introduction) öğrenmeniz önemle tavsiye edilir.

> [!NOTE]
> Bu sağlayıcı yalnızca Azure Cosmos DB SQL API 'SI ile birlikte kullanılabilir.

## <a name="install"></a>yükleme

[Microsoft. EntityFrameworkCore. Cosmos NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/)yükler.

### <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a>Başlangıç

> [!TIP]  
> Bu makalenin [örneğini GitHub '](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos)da görebilirsiniz.

Diğer sağlayıcılar gibi, ilk adım [Usecosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos)'yi çağırayöneliktir:

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> Uç nokta ve anahtar kolaylık sağlaması için burada kodlanmıştır, ancak bir üretim uygulamasında bunlar [güvenli bir şekilde depolanmalıdır](/aspnet/core/security/app-secrets#secret-manager).

Bu örnekte `Order`, [sahip olduğu `StreetAddress`türüne](../../modeling/owned-entities.md) başvuran basit bir varlıktır.

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

Verileri kaydetme ve sorgulama normal EF düzenine uyar:

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) çağırma, gerekli kapsayıcıları oluşturmak ve modelde varsa [çekirdek verileri](../../modeling/data-seeding.md) eklemek için gereklidir. Ancak `EnsureCreatedAsync`, normal işlem değil, yalnızca dağıtım sırasında çağrılmalıdır, çünkü performans sorunlarına neden olabilir.

## <a name="cosmos-specific-model-customization"></a>Cosmos 'e özgü model özelleştirmesi

Varsayılan olarak, tüm varlık türleri türetilmiş bağlamdan sonra adlandırılan aynı kapsayıcıya eşlenir (Bu durumda`"OrderContext"`). Varsayılan kapsayıcı adını değiştirmek için [Hasdefaultcontainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer)kullanın:

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Bir varlık türünü farklı bir kapsayıcıya eşlemek için [Tocontainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer)kullanın:

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Belirli bir öğenin temsil ettiği varlık türünü belirlemek için EF Core türetilmiş varlık türü olmasa bile bir Ayrıştırıcı değeri ekler. Ayrıştırıcı adı ve değeri [değiştirilebilir](../../modeling/inheritance.md).

Aynı kapsayıcıda hiçbir zaman başka bir varlık türü depolanmıyorsa, bu ayrıştırıcı [Hasnoayrıştırıcı](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator)çağırarak kaldırılabilir:

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a>Bölüm anahtarları

Varsayılan olarak EF Core, bölüm anahtarı, öğe eklerken herhangi bir değer sağlamadan `"__partitionKey"` olarak ayarlanmış kapsayıcılar oluşturacaktır. Ancak Azure Cosmos 'nin performans özelliklerinden tamamen yararlanmak için [dikkatle seçilen bir bölüm anahtarı](/azure/cosmos-db/partition-data) kullanılmalıdır. [Haspartitionkey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey)çağırarak yapılandırılabilir:

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
>Bölüm anahtarı özelliği, [dizeye dönüştürüldüğü](xref:core/modeling/value-conversions)sürece herhangi bir türde olabilir.

Bir kez yapılandırıldıktan sonra, bölüm anahtarı özelliği her zaman null olmayan bir değere sahip olmalıdır. Bir sorgu verirken, tek bölüm oluşturmak için bir koşul eklenebilir.

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a>Katıştırılmış varlıklar

Cosmos 'ye ait varlıklar, sahibiyle aynı öğeye katıştırılır. Bir özellik adını değiştirmek için [Tojsonproperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty)kullanın:

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

Bu yapılandırmayla yukarıdaki örnekteki sıralama şöyle saklanır:

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

Sahip olunan varlıkların koleksiyonları da katıştırılır. Bir sonraki örnek için `Distributor` sınıfını bir `StreetAddress`koleksiyonuyla kullanacağız:

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

Sahip olunan varlıkların depolanacak açık anahtar değerleri sağlaması gerekmez:

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

Bu şekilde kalıcı olacak:

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

Dahili EF Core her zaman izlenen tüm varlıklar için benzersiz anahtar değerlerine sahip olması gerekir. Sahip olunan türlerin koleksiyonları için varsayılan olarak oluşturulan birincil anahtar, sahibi ve JSON dizisindeki dizine karşılık gelen bir `int` özelliğine işaret eden yabancı anahtar özelliklerinden oluşur. Bu değerleri almak için giriş API 'SI kullanılabilir:

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> Gerektiğinde, sahip olan varlık türleri için varsayılan birincil anahtar değiştirilebilir, ancak anahtar değerleri açıkça sağlanmalıdır.

## <a name="working-with-disconnected-entities"></a>Bağlantısı kesilmiş varlıklarla çalışma

Her öğenin verilen bölüm anahtarı için benzersiz bir `id` değere sahip olması gerekir. Varsayılan olarak EF Core, bir sınırlayıcı olarak ' | ' kullanarak Ayrıştırıcı ve birincil anahtar değerlerini birleştirerek değeri oluşturur. Anahtar değerleri yalnızca bir varlık `Added` durumuna girdiğinde oluşturulur. Bu, değeri depolamak için .NET türünde bir `id` özelliği yoksa [varlık eklenirken](../../saving/disconnected-entities.md) bir sorun oluşturabilir.

Bu sınırlamaya geçici bir çözüm bulmak için, `id` değerini el ile oluşturup ayarlayabilir ya da önce varlığı önce eklenmiş olarak işaretleyebilir, sonra da istediğiniz durumla değiştirerek:

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

Bu, sonuçta elde edilen JSON ' dır:

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
