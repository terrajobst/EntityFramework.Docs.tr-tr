---
title: Anahtarlar (birincil)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 8b32bf6417890a954c933a5973a2c90c609beeca
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197274"
---
# <a name="keys-primary"></a><span data-ttu-id="3ef79-102">Anahtarlar (birincil)</span><span class="sxs-lookup"><span data-stu-id="3ef79-102">Keys (primary)</span></span>

<span data-ttu-id="3ef79-103">Bir anahtar, her varlık örneği için birincil benzersiz tanımlayıcı işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="3ef79-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="3ef79-104">İlişkisel bir veritabanı kullanılırken bu, *birincil anahtar*kavramıyla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="3ef79-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="3ef79-105">Birincil anahtar olmayan benzersiz bir tanımlayıcı da yapılandırabilirsiniz (daha fazla bilgi için bkz. [Alternatif anahtarlar](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="3ef79-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="3ef79-106">Aşağıdaki yöntemlerden biri, bir birincil anahtar ayarlamak/oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3ef79-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="3ef79-107">Kurallar</span><span class="sxs-lookup"><span data-stu-id="3ef79-107">Conventions</span></span>

<span data-ttu-id="3ef79-108">Kurala göre, veya `Id` `<type name>Id` adlı bir özellik bir varlığın anahtarı olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="3ef79-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="3ef79-109">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="3ef79-109">Data Annotations</span></span>

<span data-ttu-id="3ef79-110">Tek bir özelliği bir varlığın anahtarı olacak şekilde yapılandırmak için, veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3ef79-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="3ef79-111">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="3ef79-111">Fluent API</span></span>

<span data-ttu-id="3ef79-112">Tek bir özelliği bir varlığın anahtarı olacak şekilde yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3ef79-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="3ef79-113">Ayrıca, birden çok özelliği bir varlığın anahtarı olacak şekilde (bileşik anahtar olarak bilinir) yapılandırmak için akıcı API 'yi de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3ef79-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="3ef79-114">Bileşik anahtarlar yalnızca akıcı API kullanılarak yapılandırılabilir-kurallar hiçbir şekilde bir bileşik anahtar kurmayacaktır ve veri açıklamalarını yapılandırmak için kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="3ef79-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
