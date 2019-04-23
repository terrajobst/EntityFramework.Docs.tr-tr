---
title: Gerekli/isteğe bağlı özellikleri - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 564d9e62e2ed4f1a52b569630ed4994529e31dc1
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929816"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="2ba8a-102">Gerekli ve isteğe bağlı özellikler</span><span class="sxs-lookup"><span data-stu-id="2ba8a-102">Required and Optional Properties</span></span>

<span data-ttu-id="2ba8a-103">Bir özellik içeren için geçerli ise isteğe bağlı olarak sayılır `null`.</span><span class="sxs-lookup"><span data-stu-id="2ba8a-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="2ba8a-104">Varsa `null` gerekli özelliği olduğu düşünülür sonra bir özelliğe atanacak geçerli bir değer değil.</span><span class="sxs-lookup"><span data-stu-id="2ba8a-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="2ba8a-105">Kurallar</span><span class="sxs-lookup"><span data-stu-id="2ba8a-105">Conventions</span></span>

<span data-ttu-id="2ba8a-106">Kural gereği, CLR türü, null içerebilir bir özellik isteğe bağlı olarak yapılandırılır (`string`, `int?`, `byte[]`vb..).</span><span class="sxs-lookup"><span data-stu-id="2ba8a-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="2ba8a-107">CLR türü, null içeremez özellikleri yapılandırılması gerektiği gibi (`int`, `decimal`, `bool`vb..).</span><span class="sxs-lookup"><span data-stu-id="2ba8a-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="2ba8a-108">Bir özelliğin CLR türü null içeremez, isteğe bağlı olarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="2ba8a-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="2ba8a-109">Entity Framework tarafından gerekli özellik her zaman değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2ba8a-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2ba8a-110">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="2ba8a-110">Data Annotations</span></span>

<span data-ttu-id="2ba8a-111">Bir özelliğin gerekli olduğunu belirtmek için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2ba8a-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="2ba8a-112">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="2ba8a-112">Fluent API</span></span>

<span data-ttu-id="2ba8a-113">Fluent API'si, bir özelliğin gerekli olduğunu belirtmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2ba8a-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

