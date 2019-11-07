---
title: Anahtarlar (birincil)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655941"
---
# <a name="keys-primary"></a><span data-ttu-id="0cf1e-102">Anahtarlar (birincil)</span><span class="sxs-lookup"><span data-stu-id="0cf1e-102">Keys (primary)</span></span>

<span data-ttu-id="0cf1e-103">Bir anahtar, her varlık örneği için birincil benzersiz tanımlayıcı işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="0cf1e-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="0cf1e-104">İlişkisel bir veritabanı kullanılırken bu, *birincil anahtar*kavramıyla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="0cf1e-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="0cf1e-105">Birincil anahtar olmayan benzersiz bir tanımlayıcı da yapılandırabilirsiniz (daha fazla bilgi için bkz. [Alternatif anahtarlar](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="0cf1e-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="0cf1e-106">Aşağıdaki yöntemlerden biri, bir birincil anahtar ayarlamak/oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0cf1e-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="0cf1e-107">Kurallar</span><span class="sxs-lookup"><span data-stu-id="0cf1e-107">Conventions</span></span>

<span data-ttu-id="0cf1e-108">Kurala göre, `Id` veya `<type name>Id` adlı bir özellik bir varlığın anahtarı olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="0cf1e-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a><span data-ttu-id="0cf1e-109">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="0cf1e-109">Data Annotations</span></span>

<span data-ttu-id="0cf1e-110">Tek bir özelliği bir varlığın anahtarı olacak şekilde yapılandırmak için, veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0cf1e-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="0cf1e-111">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="0cf1e-111">Fluent API</span></span>

<span data-ttu-id="0cf1e-112">Tek bir özelliği bir varlığın anahtarı olacak şekilde yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0cf1e-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="0cf1e-113">Ayrıca, birden çok özelliği bir varlığın anahtarı olacak şekilde (bileşik anahtar olarak bilinir) yapılandırmak için akıcı API 'yi de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0cf1e-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="0cf1e-114">Bileşik anahtarlar yalnızca akıcı API kullanılarak yapılandırılabilir-kurallar hiçbir şekilde bir bileşik anahtar kurmayacaktır ve veri açıklamalarını yapılandırmak için kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="0cf1e-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
