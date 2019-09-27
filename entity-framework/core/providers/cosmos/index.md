---
title: Azure Cosmos DB sağlayıcı-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: 683436aa485d2fef9aa8bf6c6ff02b00dfeb28cf
ms.sourcegitcommit: 2caec1e63f2ce1d9439ef6193df5a77da2fedd0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317569"
---
# <a name="ef-core-azure-cosmos-db-provider"></a>EF Core Azure Cosmos DB sağlayıcısı

>[!NOTE]
> Bu sağlayıcı EF Core 3,0 ' de yenidir.

Bu veritabanı sağlayıcısı, Azure Cosmos DB birlikte Entity Framework Core kullanılmasına izin verir. Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.

Bu bölümü okumadan önce [Azure Cosmos DB belgelerini](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) öğrenmeniz önemle tavsiye edilir.

## <a name="install"></a>Yükleme

[Microsoft. EntityFrameworkCore. Cosmos NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/)yükler.

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

## <a name="get-started"></a>Başlarken

> [!TIP]  
> Bu makalenin [örneğini GitHub '](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos)da görebilirsiniz.

Diğer sağlayıcılarda olduğu gibi, ilk adım çağrlandır `UseCosmos`:

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> Uç nokta ve anahtar kolaylık sağlaması için burada kodlanmıştır, ancak bir üretim uygulamasında bu uygulamalar güvenli bir şekilde [depolanmalıdır](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)

Bu örnekte `Order` , [sahip olunan türe](../../modeling/owned-entities.md) `StreetAddress`başvuru içeren basit bir varlıktır.

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

Verileri kaydetme ve sorgulama normal EF düzenine uyar:

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> Çağırma `EnsureCreated` , gerekli koleksiyonları oluşturmak ve modelde varsa [çekirdek verileri](../../modeling/data-seeding.md) eklemek için gereklidir. Ancak `EnsureCreated` , performans sorunlarına neden olabileceğinden, yalnızca dağıtım sırasında, normal işlem değil çağrılmalıdır.

## <a name="cosmos-specific-model-customization"></a>Cosmos 'e özgü model özelleştirmesi

Varsayılan olarak, tüm varlık türleri türetilmiş bağlamdan sonra adlandırılan aynı kapsayıcıya eşlenir (`"OrderContext"` bu durumda). Varsayılan kapsayıcı adını değiştirmek için şunu kullanın `HasDefaultContainer`:

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Bir varlık türünü farklı bir kapsayıcı kullanımına `ToContainer`eşlemek için:

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Belirli bir öğenin temsil ettiği varlık türünü belirlemek için EF Core türetilmiş varlık türü olmasa bile bir Ayrıştırıcı değeri ekler. Ayrıştırıcı adı ve değeri [değiştirilebilir](../../modeling/inheritance.md).

## <a name="embedded-entities"></a>Katıştırılmış varlıklar

Cosmos 'ye ait varlıklar, sahibiyle aynı öğeye katıştırılır. Bir özellik adını değiştirmek için şunu `ToJsonProperty`kullanın:

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

Bu yapılandırmayla yukarıdaki örnekteki sıralama şöyle saklanır:

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

Sahip olunan varlıkların koleksiyonları da katıştırılır. Bir sonraki örnekte `Distributor` sınıfını bir `StreetAddress`koleksiyonuyla kullanacağız:

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
            "Discriminator": "StreetAddress",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
            "Discriminator": "StreetAddress",
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

Dahili EF Core her zaman izlenen tüm varlıklar için benzersiz anahtar değerlerine sahip olması gerekir. Sahip olunan türlerin koleksiyonları için varsayılan olarak oluşturulan birincil anahtar, sahibine ve JSON dizisindeki dizine karşılık gelen bir `int` özelliğe işaret eden yabancı anahtar özelliklerinden oluşur. Bu değerleri almak için giriş API 'SI kullanılabilir:

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> Gerektiğinde, sahip olan varlık türleri için varsayılan birincil anahtar değiştirilebilir, ancak anahtar değerleri açıkça sağlanmalıdır.

## <a name="working-with-disconnected-entities"></a>Bağlantısı kesilmiş varlıklarla çalışma

Her öğenin, belirtilen bölüm anahtarı `id` için benzersiz bir değere sahip olması gerekir. Varsayılan olarak EF Core, bir sınırlayıcı olarak ' | ' kullanarak Ayrıştırıcı ve birincil anahtar değerlerini birleştirerek değeri oluşturur. Anahtar değerleri yalnızca bir varlık `Added` duruma girdiğinde oluşturulur. Bu, [varlık eklenirken](../../saving/disconnected-entities.md) bir sorun oluşturabilir ve bu, değeri depolamak için `id` .net türünde bir özelliğe sahip değildir.

Bu sınırlamaya geçici bir çözüm bulmak için, `id` değeri el ile oluşturup ayarlayabilir ya da önce varlığı önce eklenmiş olarak işaretleyebilir, sonra da istediğiniz durumla değiştirerek:

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

Bu, sonuçta elde edilen JSON ' dır:

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "3 Abbey Road"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-8f7ac48f01d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163739
}
```
