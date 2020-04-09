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
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="9ceac-103">EF Core Azure Cosmos DB Sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="9ceac-103">EF Core Azure Cosmos DB Provider</span></span>

> [!NOTE]
> <span data-ttu-id="9ceac-104">Bu sağlayıcı EF Core 3.0'da yenidir.</span><span class="sxs-lookup"><span data-stu-id="9ceac-104">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="9ceac-105">Bu veritabanı sağlayıcısı, Entity Framework Core'un Azure Cosmos DB ile kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="9ceac-105">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="9ceac-106">Sağlayıcı, Varlık Çerçeve Çekirdek [Projesi'nin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak korunur.</span><span class="sxs-lookup"><span data-stu-id="9ceac-106">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="9ceac-107">Bu bölümü okumadan önce Azure [Cosmos DB belgelerine](/azure/cosmos-db/introduction) alışmanız önemle tavsiye edilir.</span><span class="sxs-lookup"><span data-stu-id="9ceac-107">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](/azure/cosmos-db/introduction) before reading this section.</span></span>

> [!NOTE]
> <span data-ttu-id="9ceac-108">Bu sağlayıcı yalnızca Azure Cosmos DB'nin SQL API'si ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="9ceac-108">This provider only works with the SQL API of Azure Cosmos DB.</span></span>

## <a name="install"></a><span data-ttu-id="9ceac-109">Yükleme</span><span class="sxs-lookup"><span data-stu-id="9ceac-109">Install</span></span>

<span data-ttu-id="9ceac-110">[Microsoft.EntityFrameworkCore.Cosmos NuGet paketini](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/)yükleyin.</span><span class="sxs-lookup"><span data-stu-id="9ceac-110">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9ceac-111">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9ceac-111">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

### <a name="visual-studio"></a>[<span data-ttu-id="9ceac-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ceac-112">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a><span data-ttu-id="9ceac-113">başlarken</span><span class="sxs-lookup"><span data-stu-id="9ceac-113">Get started</span></span>

> [!TIP]  
> <span data-ttu-id="9ceac-114">Bu makalenin [örneğini GitHub'da](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Cosmos)görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9ceac-114">You can view this article's [sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="9ceac-115">Diğer sağlayıcılar için olduğu gibi ilk adım [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos)aramaktır:</span><span class="sxs-lookup"><span data-stu-id="9ceac-115">Like for other providers the first step is to call [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="9ceac-116">Uç nokta ve anahtar basitlik için burada kodlanır, ancak bir üretim uygulamasında bunlar [güvenli bir şekilde depolanmalıdır.](/aspnet/core/security/app-secrets#secret-manager)</span><span class="sxs-lookup"><span data-stu-id="9ceac-116">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securely](/aspnet/core/security/app-secrets#secret-manager).</span></span>

<span data-ttu-id="9ceac-117">Bu `Order` örnekte, [sahip olunan türe](../../modeling/owned-entities.md) `StreetAddress`atıfta bulunan basit bir varlık yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="9ceac-117">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="9ceac-118">Veri kaydetme ve quering normal EF deseni izler:</span><span class="sxs-lookup"><span data-stu-id="9ceac-118">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="9ceac-119">Gerekli kapsayıcıları oluşturmak ve modelde varsa tohum [verilerini](../../modeling/data-seeding.md) eklemek için [EnsureCreatedAsync'i](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) aramak gereklidir.</span><span class="sxs-lookup"><span data-stu-id="9ceac-119">Calling [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) is necessary to create the required containers and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="9ceac-120">Ancak, `EnsureCreatedAsync` performans sorunlarına neden olabileceğiiçin, normal çalışma sırasında değil, yalnızca dağıtım sırasında çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9ceac-120">However `EnsureCreatedAsync` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="9ceac-121">Cosmos'a özel model özelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="9ceac-121">Cosmos-specific model customization</span></span>

<span data-ttu-id="9ceac-122">Varsayılan olarak tüm varlık türleri, türetilmiş bağlamdan (bu`"OrderContext"` durumda) sonra adlandırılmış aynı kapsayıcıya eşlenir.</span><span class="sxs-lookup"><span data-stu-id="9ceac-122">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="9ceac-123">Varsayılan kapsayıcı adını değiştirmek için [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer)kullanın:</span><span class="sxs-lookup"><span data-stu-id="9ceac-123">To change the default container name use [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="9ceac-124">Bir varlık türünü farklı bir kapsayıcıyla eşlemek için [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer)kullanın:</span><span class="sxs-lookup"><span data-stu-id="9ceac-124">To map an entity type to a different container use [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="9ceac-125">Belirli bir öğenin EF Core'u temsil ettiği varlık türünü tanımlamak için, türetilmiş varlık türü olmasa bile ayrımcı bir değer ekler.</span><span class="sxs-lookup"><span data-stu-id="9ceac-125">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="9ceac-126">Ayırıcının adı ve değeri [değiştirilebilir.](../../modeling/inheritance.md)</span><span class="sxs-lookup"><span data-stu-id="9ceac-126">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

<span data-ttu-id="9ceac-127">Başka bir varlık türü aynı kapta depolanmayacaksa, ayırıcı [HasNoDiscriminator'u](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator)arayarak kaldırılabilir:</span><span class="sxs-lookup"><span data-stu-id="9ceac-127">If no other entity type will ever be stored in the same container the discriminator can be removed by calling [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):</span></span>

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a><span data-ttu-id="9ceac-128">Bölüm tuşları</span><span class="sxs-lookup"><span data-stu-id="9ceac-128">Partition keys</span></span>

<span data-ttu-id="9ceac-129">Varsayılan olarak EF Core, öğeleri eklerken `"__partitionKey"` bunun için herhangi bir değer sağlamadan bölüm anahtarı ayarlanmış kapsayıcılar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9ceac-129">By default EF Core will create containers with the partition key set to `"__partitionKey"` without supplying any value for it when inserting items.</span></span> <span data-ttu-id="9ceac-130">Ancak Azure Cosmos'un performans özelliklerinden tam olarak yararlanmak için özenle seçilmiş bir [bölüm anahtarı](/azure/cosmos-db/partition-data) kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9ceac-130">But to fully leverage the performance capabilities of Azure Cosmos a [carefully selected partition key](/azure/cosmos-db/partition-data) should be used.</span></span> <span data-ttu-id="9ceac-131">[Bu HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey)arayarak yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="9ceac-131">It can be configured by calling [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
><span data-ttu-id="9ceac-132">Bölüm anahtar özelliği [dize dönüştürüldüğü](xref:core/modeling/value-conversions)sürece herhangi bir tür olabilir.</span><span class="sxs-lookup"><span data-stu-id="9ceac-132">The partition key property can be of any type as long as it is [converted to string](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="9ceac-133">Bir kez yapılandırılan bölüm anahtar özelliği her zaman null olmayan bir değere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9ceac-133">Once configured the partition key property should always have a non-null value.</span></span> <span data-ttu-id="9ceac-134">Sorgu verirken, tek bölümyapmak için bir koşul eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="9ceac-134">When issuing a query a condition can be added to make it single-partition.</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a><span data-ttu-id="9ceac-135">Gömülü varlıklar</span><span class="sxs-lookup"><span data-stu-id="9ceac-135">Embedded entities</span></span>

<span data-ttu-id="9ceac-136">Çünkü Cosmos'a ait varlıklar sahibiyle aynı öğeye gömülür.</span><span class="sxs-lookup"><span data-stu-id="9ceac-136">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="9ceac-137">Bir özellik adını değiştirmek için [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty)kullanın:</span><span class="sxs-lookup"><span data-stu-id="9ceac-137">To change a property name use [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="9ceac-138">Bu yapılandırma ile yukarıdaki örnekten gelen sipariş aşağıdaki gibi depolanır:</span><span class="sxs-lookup"><span data-stu-id="9ceac-138">With this configuration the order from the example above is stored like this:</span></span>

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

<span data-ttu-id="9ceac-139">Sahip olunan varlıkların koleksiyonları da gömülür.</span><span class="sxs-lookup"><span data-stu-id="9ceac-139">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="9ceac-140">Bir sonraki örnek için `Distributor` bir koleksiyon ile `StreetAddress`sınıf kullanacağız:</span><span class="sxs-lookup"><span data-stu-id="9ceac-140">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="9ceac-141">Sahip olunan varlıkların depolanacak açık anahtar değerleri sağlaması gerekmez:</span><span class="sxs-lookup"><span data-stu-id="9ceac-141">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="9ceac-142">Bu şekilde devam edeceklerdir:</span><span class="sxs-lookup"><span data-stu-id="9ceac-142">They will be persisted in this way:</span></span>

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

<span data-ttu-id="9ceac-143">Dahili OLARAK EF Core'un izlenen tüm varlıklar için her zaman benzersiz anahtar değerlere sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9ceac-143">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="9ceac-144">Sahip olunan türlerin koleksiyonları için varsayılan olarak oluşturulan birincil anahtar, sahibini `int` gösteren yabancı anahtar özellikleri ve JSON dizisinde dizideki diziye karşılık gelen bir özellikten oluşur.</span><span class="sxs-lookup"><span data-stu-id="9ceac-144">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="9ceac-145">Bu değerler giriş API almak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="9ceac-145">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="9ceac-146">Gerektiğinde sahip olunan varlık türleri için varsayılan birincil anahtar değiştirilebilir, ancak daha sonra anahtar değerleri açıkça sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9ceac-146">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="9ceac-147">Bağlantısı kesilen varlıklarla çalışma</span><span class="sxs-lookup"><span data-stu-id="9ceac-147">Working with disconnected entities</span></span>

<span data-ttu-id="9ceac-148">Her öğenin verilen `id` bölüm anahtarı için benzersiz bir değere sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9ceac-148">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="9ceac-149">Varsayılan olarak EF Core, ayırıcıyı ve birincil anahtar değerlerini bir sınırlayıcı olarak '|' kullanarak bir değeri biraraya gelir.</span><span class="sxs-lookup"><span data-stu-id="9ceac-149">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="9ceac-150">Anahtar değerler yalnızca bir varlık `Added` duruma girdiğinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9ceac-150">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="9ceac-151">Değeri depolamak için .NET türünde bir `id` özelliği [yoksa, varlıkları takarken](../../saving/disconnected-entities.md) sorun yaratabilir.</span><span class="sxs-lookup"><span data-stu-id="9ceac-151">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="9ceac-152">Bu sınırlamayı `id` aşmak için değer el ile oluşturulabilir ve ayarlanabilir veya varlığı önce eklenen olarak işaretleyebilir, sonra istenilen duruma göre değiştirebiliriz:</span><span class="sxs-lookup"><span data-stu-id="9ceac-152">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="9ceac-153">Bu, ortaya çıkan JSON:</span><span class="sxs-lookup"><span data-stu-id="9ceac-153">This is the resulting JSON:</span></span>

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
