---
title: Alternatif anahtarlar-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: e5bb0602adb465435c8e88d045395a9424b2d9a3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655770"
---
# <a name="alternate-keys"></a><span data-ttu-id="d0b55-102">Alternatif Anahtarlar</span><span class="sxs-lookup"><span data-stu-id="d0b55-102">Alternate Keys</span></span>

<span data-ttu-id="d0b55-103">Alternatif bir anahtar, birincil anahtara ek olarak her bir varlık örneği için alternatif benzersiz bir tanımlayıcı işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="d0b55-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="d0b55-104">Alternatif anahtarlar bir ilişkinin hedefi olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d0b55-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="d0b55-105">İlişkisel bir veritabanı kullanırken, bu, diğer anahtar sütunları üzerinde benzersiz bir dizin/kısıtlama kavramına eşlenir ve sütunlara (ler) başvuran bir veya daha fazla yabancı anahtar kısıtlamasıdır.</span><span class="sxs-lookup"><span data-stu-id="d0b55-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="d0b55-106">Yalnızca bir sütunun benzersizlik düzeyini zorlamak isterseniz, alternatif bir anahtar yerine benzersiz bir dizin isterseniz bkz. [dizinler](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="d0b55-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="d0b55-107">EF 'de, alternatif anahtarlar yabancı bir anahtarın hedefi olarak kullanılabildiğinden benzersiz dizinlerden daha fazla işlevsellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="d0b55-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="d0b55-108">Diğer anahtarlar genellikle gerektiğinde sizin için sunulmuştur ve bunları el ile yapılandırmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="d0b55-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="d0b55-109">Daha fazla ayrıntı için bkz. [kurallar](#conventions) .</span><span class="sxs-lookup"><span data-stu-id="d0b55-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="d0b55-110">Kurallar</span><span class="sxs-lookup"><span data-stu-id="d0b55-110">Conventions</span></span>

<span data-ttu-id="d0b55-111">Kural gereği, bir ilişkinin hedefi olarak birincil anahtar olmayan bir özelliği tanımlarken sizin için alternatif bir anahtar sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="d0b55-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

## <a name="data-annotations"></a><span data-ttu-id="d0b55-112">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="d0b55-112">Data Annotations</span></span>

<span data-ttu-id="d0b55-113">Alternatif anahtarlar, veri ek açıklamaları kullanılarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="d0b55-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d0b55-114">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="d0b55-114">Fluent API</span></span>

<span data-ttu-id="d0b55-115">Tek bir özelliği alternatif bir anahtar olacak şekilde yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0b55-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=7,8)]

<span data-ttu-id="d0b55-116">Ayrıca, birden çok özelliği farklı bir anahtar olacak şekilde (bileşik alternatif anahtar olarak bilinir) yapılandırmak için akıcı API 'yi de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0b55-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=7,8)]
