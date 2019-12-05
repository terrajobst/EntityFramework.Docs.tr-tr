---
title: Azure Cosmos DB sağlayıcı-EF Core
description: Entity Framework Core Azure Cosmos DB SQL API 'siyle kullanılmasına izin veren veritabanı sağlayıcısına yönelik belgeler
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/index
ms.openlocfilehash: 162e5d387bcbfbf1e90baf27fc62dd2ed562ae58
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824552"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="f5dbd-103">EF Core Azure Cosmos DB sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="f5dbd-103">EF Core Azure Cosmos DB Provider</span></span>

> [!NOTE]
> <span data-ttu-id="f5dbd-104">Bu sağlayıcı EF Core 3,0 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-104">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="f5dbd-105">Bu veritabanı sağlayıcısı, Azure Cosmos DB birlikte Entity Framework Core kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-105">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="f5dbd-106">Sağlayıcı [Entity Framework Core projenin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak tutulur.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-106">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="f5dbd-107">Bu bölümü okumadan önce [Azure Cosmos DB belgelerini](/azure/cosmos-db/introduction) öğrenmeniz önemle tavsiye edilir.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-107">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](/azure/cosmos-db/introduction) before reading this section.</span></span>

> [!NOTE]
> <span data-ttu-id="f5dbd-108">Bu sağlayıcı yalnızca Azure Cosmos DB SQL API 'SI ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-108">This provider only works with the SQL API of Azure Cosmos DB.</span></span>

## <a name="install"></a><span data-ttu-id="f5dbd-109">yükleme</span><span class="sxs-lookup"><span data-stu-id="f5dbd-109">Install</span></span>

<span data-ttu-id="f5dbd-110">[Microsoft. EntityFrameworkCore. Cosmos NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/)yükler.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-110">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="f5dbd-111">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f5dbd-111">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="f5dbd-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5dbd-112">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a><span data-ttu-id="f5dbd-113">Kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="f5dbd-113">Get started</span></span>

> [!TIP]  
> <span data-ttu-id="f5dbd-114">Bu makalenin [örneğini GitHub '](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos)da görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-114">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="f5dbd-115">Diğer sağlayıcılar gibi, ilk adım [Usecosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos)'yi çağırayöneliktir:</span><span class="sxs-lookup"><span data-stu-id="f5dbd-115">Like for other providers the first step is to call [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="f5dbd-116">Uç nokta ve anahtar kolaylık sağlaması için burada kodlanmıştır, ancak bir üretim uygulamasında bu uygulamalar güvenli bir şekilde [depolanmalıdır](/aspnet/core/security/app-secrets#secret-manager)</span><span class="sxs-lookup"><span data-stu-id="f5dbd-116">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="f5dbd-117">Bu örnekte `Order`, [sahip olduğu `StreetAddress`türüne](../../modeling/owned-entities.md) başvuran basit bir varlıktır.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-117">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="f5dbd-118">Verileri kaydetme ve sorgulama normal EF düzenine uyar:</span><span class="sxs-lookup"><span data-stu-id="f5dbd-118">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="f5dbd-119">[EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) çağırma, gerekli kapsayıcıları oluşturmak ve modelde varsa [çekirdek verileri](../../modeling/data-seeding.md) eklemek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-119">Calling [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) is necessary to create the required containers and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="f5dbd-120">Ancak `EnsureCreatedAsync`, normal işlem değil, yalnızca dağıtım sırasında çağrılmalıdır, çünkü performans sorunlarına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-120">However `EnsureCreatedAsync` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="f5dbd-121">Cosmos 'e özgü model özelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="f5dbd-121">Cosmos-specific model customization</span></span>

<span data-ttu-id="f5dbd-122">Varsayılan olarak, tüm varlık türleri türetilmiş bağlamdan sonra adlandırılan aynı kapsayıcıya eşlenir (Bu durumda`"OrderContext"`).</span><span class="sxs-lookup"><span data-stu-id="f5dbd-122">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="f5dbd-123">Varsayılan kapsayıcı adını değiştirmek için [Hasdefaultcontainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer)kullanın:</span><span class="sxs-lookup"><span data-stu-id="f5dbd-123">To change the default container name use [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="f5dbd-124">Bir varlık türünü farklı bir kapsayıcıya eşlemek için [Tocontainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer)kullanın:</span><span class="sxs-lookup"><span data-stu-id="f5dbd-124">To map an entity type to a different container use [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="f5dbd-125">Belirli bir öğenin temsil ettiği varlık türünü belirlemek için EF Core türetilmiş varlık türü olmasa bile bir Ayrıştırıcı değeri ekler.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-125">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="f5dbd-126">Ayrıştırıcı adı ve değeri [değiştirilebilir](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="f5dbd-126">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

<span data-ttu-id="f5dbd-127">Aynı kapsayıcıda hiçbir zaman başka bir varlık türü depolanmıyorsa, bu ayrıştırıcı [Hasnoayrıştırıcı](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator)çağırarak kaldırılabilir:</span><span class="sxs-lookup"><span data-stu-id="f5dbd-127">If no other entity type will ever be stored in the same container the discriminator can be removed by calling [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):</span></span>

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a><span data-ttu-id="f5dbd-128">Bölüm anahtarları</span><span class="sxs-lookup"><span data-stu-id="f5dbd-128">Partition keys</span></span>

<span data-ttu-id="f5dbd-129">Varsayılan olarak EF Core, bölüm anahtarı, öğe eklerken herhangi bir değer sağlamadan `"__partitionKey"` olarak ayarlanmış kapsayıcılar oluşturacaktır.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-129">By default EF Core will create containers with the partition key set to `"__partitionKey"` without supplying any value for it when inserting items.</span></span> <span data-ttu-id="f5dbd-130">Ancak Azure Cosmos 'nin performans özelliklerinden tamamen yararlanmak için [dikkatle seçilen bir bölüm anahtarı](/azure/cosmos-db/partition-data) kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-130">But to fully leverage the performance capabilities of Azure Cosmos a [carefully selected partition key](/azure/cosmos-db/partition-data) should be used.</span></span> <span data-ttu-id="f5dbd-131">[Haspartitionkey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey)çağırarak yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="f5dbd-131">It can be configured by calling [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
><span data-ttu-id="f5dbd-132">Bölüm anahtarı özelliği, [dizeye dönüştürüldüğü](xref:core/modeling/value-conversions)sürece herhangi bir türde olabilir.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-132">The partition key property can be of any type as long as it is [converted to string](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="f5dbd-133">Bir kez yapılandırıldıktan sonra, bölüm anahtarı özelliği her zaman null olmayan bir değere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-133">Once configured the partition key property should always have a non-null value.</span></span> <span data-ttu-id="f5dbd-134">Bir sorgu verirken, tek bölüm oluşturmak için bir koşul eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-134">When issuing a query a condition can be added to make it single-partition.</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a><span data-ttu-id="f5dbd-135">Katıştırılmış varlıklar</span><span class="sxs-lookup"><span data-stu-id="f5dbd-135">Embedded entities</span></span>

<span data-ttu-id="f5dbd-136">Cosmos 'ye ait varlıklar, sahibiyle aynı öğeye katıştırılır.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-136">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="f5dbd-137">Bir özellik adını değiştirmek için [Tojsonproperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty)kullanın:</span><span class="sxs-lookup"><span data-stu-id="f5dbd-137">To change a property name use [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="f5dbd-138">Bu yapılandırmayla yukarıdaki örnekteki sıralama şöyle saklanır:</span><span class="sxs-lookup"><span data-stu-id="f5dbd-138">With this configuration the order from the example above is stored like this:</span></span>

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

<span data-ttu-id="f5dbd-139">Sahip olunan varlıkların koleksiyonları da katıştırılır.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-139">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="f5dbd-140">Bir sonraki örnek için `Distributor` sınıfını bir `StreetAddress`koleksiyonuyla kullanacağız:</span><span class="sxs-lookup"><span data-stu-id="f5dbd-140">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="f5dbd-141">Sahip olunan varlıkların depolanacak açık anahtar değerleri sağlaması gerekmez:</span><span class="sxs-lookup"><span data-stu-id="f5dbd-141">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="f5dbd-142">Bu şekilde kalıcı olacak:</span><span class="sxs-lookup"><span data-stu-id="f5dbd-142">They will be persisted in this way:</span></span>

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

<span data-ttu-id="f5dbd-143">Dahili EF Core her zaman izlenen tüm varlıklar için benzersiz anahtar değerlerine sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-143">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="f5dbd-144">Sahip olunan türlerin koleksiyonları için varsayılan olarak oluşturulan birincil anahtar, sahibi ve JSON dizisindeki dizine karşılık gelen bir `int` özelliğine işaret eden yabancı anahtar özelliklerinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-144">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="f5dbd-145">Bu değerleri almak için giriş API 'SI kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="f5dbd-145">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="f5dbd-146">Gerektiğinde, sahip olan varlık türleri için varsayılan birincil anahtar değiştirilebilir, ancak anahtar değerleri açıkça sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-146">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="f5dbd-147">Bağlantısı kesilmiş varlıklarla çalışma</span><span class="sxs-lookup"><span data-stu-id="f5dbd-147">Working with disconnected entities</span></span>

<span data-ttu-id="f5dbd-148">Her öğenin verilen bölüm anahtarı için benzersiz bir `id` değere sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-148">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="f5dbd-149">Varsayılan olarak EF Core, bir sınırlayıcı olarak ' | ' kullanarak Ayrıştırıcı ve birincil anahtar değerlerini birleştirerek değeri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-149">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="f5dbd-150">Anahtar değerleri yalnızca bir varlık `Added` durumuna girdiğinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-150">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="f5dbd-151">Bu, değeri depolamak için .NET türünde bir `id` özelliği yoksa [varlık eklenirken](../../saving/disconnected-entities.md) bir sorun oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="f5dbd-151">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="f5dbd-152">Bu sınırlamaya geçici bir çözüm bulmak için, `id` değerini el ile oluşturup ayarlayabilir ya da önce varlığı önce eklenmiş olarak işaretleyebilir, sonra da istediğiniz durumla değiştirerek:</span><span class="sxs-lookup"><span data-stu-id="f5dbd-152">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="f5dbd-153">Bu, sonuçta elde edilen JSON ' dır:</span><span class="sxs-lookup"><span data-stu-id="f5dbd-153">This is the resulting JSON:</span></span>

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
