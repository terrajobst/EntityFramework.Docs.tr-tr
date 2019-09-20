---
title: Azure Cosmos DB sağlayıcı-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: c753bb71089c91cbb26b970cddd118645fb18d56
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150821"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="e41a2-102">EF Core Azure Cosmos DB sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="e41a2-102">EF Core Azure Cosmos DB Provider</span></span>

>[!NOTE]
> <span data-ttu-id="e41a2-103">Bu sağlayıcı EF Core 3,0 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="e41a2-103">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="e41a2-104">Bu veritabanı sağlayıcısı, Azure Cosmos DB birlikte Entity Framework Core kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="e41a2-104">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="e41a2-105">Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.</span><span class="sxs-lookup"><span data-stu-id="e41a2-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="e41a2-106">Bu bölümü okumadan önce [Azure Cosmos DB belgelerini](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) öğrenmeniz önemle tavsiye edilir.</span><span class="sxs-lookup"><span data-stu-id="e41a2-106">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) before reading this section.</span></span>

## <a name="install"></a><span data-ttu-id="e41a2-107">Yükleme</span><span class="sxs-lookup"><span data-stu-id="e41a2-107">Install</span></span>

<span data-ttu-id="e41a2-108">[Microsoft. EntityFrameworkCore. Cosmos NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/)yükler.</span><span class="sxs-lookup"><span data-stu-id="e41a2-108">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

## <a name="get-started"></a><span data-ttu-id="e41a2-109">Başlarken</span><span class="sxs-lookup"><span data-stu-id="e41a2-109">Get Started</span></span>

> [!TIP]  
> <span data-ttu-id="e41a2-110">Bu makalenin [örneğini GitHub '](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos)da görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e41a2-110">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="e41a2-111">Diğer sağlayıcılarda olduğu gibi, ilk adım çağrlandır `UseCosmos`:[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]</span><span class="sxs-lookup"><span data-stu-id="e41a2-111">Like for other providers the first step is to call `UseCosmos`: [!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]</span></span>

> [!WARNING]
> <span data-ttu-id="e41a2-112">Uç nokta ve anahtar kolaylık sağlaması için burada kodlanmıştır, ancak bir üretim uygulamasında bu uygulamalar güvenli bir şekilde [depolanmalıdır](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span><span class="sxs-lookup"><span data-stu-id="e41a2-112">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="e41a2-113">Bu örnekte `Order` , [sahip olunan türe](../../modeling/owned-entities.md) `StreetAddress`başvuru içeren basit bir varlıktır.</span><span class="sxs-lookup"><span data-stu-id="e41a2-113">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="e41a2-114">Verileri kaydetme ve sorgulama normal EF düzenine uyar:[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]</span><span class="sxs-lookup"><span data-stu-id="e41a2-114">Saving and quering data follows the normal EF pattern: [!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e41a2-115">Çağırma `EnsureCreated` , gerekli koleksiyonları oluşturmak ve modelde varsa [çekirdek verileri](../../modeling/data-seeding.md) eklemek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="e41a2-115">Calling `EnsureCreated` is necessary to create the required collections and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="e41a2-116">Ancak `EnsureCreated` , performans sorunlarına neden olabileceğinden, yalnızca dağıtım sırasında, normal işlem değil çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e41a2-116">However `EnsureCreated` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="e41a2-117">Cosmos 'e özgü model özelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="e41a2-117">Cosmos-specific Model Customization</span></span>

<span data-ttu-id="e41a2-118">Varsayılan olarak, tüm varlık türleri türetilmiş bağlamdan sonra adlandırılan aynı kapsayıcıya eşlenir (`"OrderContext"` bu durumda).</span><span class="sxs-lookup"><span data-stu-id="e41a2-118">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="e41a2-119">Varsayılan kapsayıcı adını değiştirmek için şunu kullanın `HasDefaultContainer`:</span><span class="sxs-lookup"><span data-stu-id="e41a2-119">To change the default container name use `HasDefaultContainer`:</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="e41a2-120">Bir varlık türünü farklı bir kapsayıcı kullanımına `ToContainer`eşlemek için:</span><span class="sxs-lookup"><span data-stu-id="e41a2-120">To map an entity type to a different container use `ToContainer`:</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="e41a2-121">Belirli bir öğenin temsil ettiği varlık türünü belirlemek için EF Core türetilmiş varlık türü olmasa bile bir Ayrıştırıcı değeri ekler.</span><span class="sxs-lookup"><span data-stu-id="e41a2-121">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="e41a2-122">Ayrıştırıcı adı ve değeri [değiştirilebilir](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="e41a2-122">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

## <a name="embedded-entities"></a><span data-ttu-id="e41a2-123">Katıştırılmış varlıklar</span><span class="sxs-lookup"><span data-stu-id="e41a2-123">Embedded Entities</span></span>

<span data-ttu-id="e41a2-124">Cosmos 'ye ait varlıklar, sahibiyle aynı öğeye katıştırılır.</span><span class="sxs-lookup"><span data-stu-id="e41a2-124">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="e41a2-125">Bir özellik adını değiştirmek için şunu `ToJsonProperty`kullanın:</span><span class="sxs-lookup"><span data-stu-id="e41a2-125">To change a property name use `ToJsonProperty`:</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="e41a2-126">Bu yapılandırmayla yukarıdaki örnekteki sıralama şöyle saklanır:</span><span class="sxs-lookup"><span data-stu-id="e41a2-126">With this configuration the order from the example above is stored like this:</span></span>

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

<span data-ttu-id="e41a2-127">Sahip olunan varlıkların koleksiyonları da katıştırılır.</span><span class="sxs-lookup"><span data-stu-id="e41a2-127">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="e41a2-128">Bir sonraki örnekte `Distributor` sınıfını bir `StreetAddress`koleksiyonuyla kullanacağız:</span><span class="sxs-lookup"><span data-stu-id="e41a2-128">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="e41a2-129">Sahip olunan varlıkların depolanacak açık anahtar değerleri sağlaması gerekmez:</span><span class="sxs-lookup"><span data-stu-id="e41a2-129">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="e41a2-130">Bu şekilde kalıcı olacak:</span><span class="sxs-lookup"><span data-stu-id="e41a2-130">They will be persisted in this way:</span></span>

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

<span data-ttu-id="e41a2-131">Dahili EF Core her zaman izlenen tüm varlıklar için benzersiz anahtar değerlerine sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e41a2-131">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="e41a2-132">Sahip olunan türlerin koleksiyonları için varsayılan olarak oluşturulan birincil anahtar, sahibine ve JSON dizisindeki dizine karşılık gelen bir `int` özelliğe işaret eden yabancı anahtar özelliklerinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="e41a2-132">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="e41a2-133">Bu değerleri almak için giriş API 'SI kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="e41a2-133">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="e41a2-134">Gerektiğinde, sahip olan varlık türleri için varsayılan birincil anahtar değiştirilebilir, ancak anahtar değerleri açıkça sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e41a2-134">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="e41a2-135">Bağlantısı kesilmiş varlıklarla çalışma</span><span class="sxs-lookup"><span data-stu-id="e41a2-135">Working with Disconnected Entities</span></span>

<span data-ttu-id="e41a2-136">Her öğenin, belirtilen bölüm anahtarı `id` için benzersiz bir değere sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e41a2-136">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="e41a2-137">Varsayılan olarak EF Core, bir sınırlayıcı olarak ' | ' kullanarak Ayrıştırıcı ve birincil anahtar değerlerini birleştirerek değeri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e41a2-137">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="e41a2-138">Anahtar değerleri yalnızca bir varlık `Added` duruma girdiğinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e41a2-138">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="e41a2-139">Bu, [varlık eklenirken](../../saving/disconnected-entities.md) bir sorun oluşturabilir ve bu, değeri depolamak için `id` .net türünde bir özelliğe sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="e41a2-139">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="e41a2-140">Bu sınırlamaya geçici bir çözüm bulmak için, `id` değeri el ile oluşturup ayarlayabilir ya da önce varlığı önce eklenmiş olarak işaretleyebilir, sonra da istediğiniz durumla değiştirerek:</span><span class="sxs-lookup"><span data-stu-id="e41a2-140">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="e41a2-141">Bu, sonuçta elde edilen JSON ' dır:</span><span class="sxs-lookup"><span data-stu-id="e41a2-141">This is the resulting JSON:</span></span>

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