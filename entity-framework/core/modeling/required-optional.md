---
title: Gerekli/isteğe bağlı özellikler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149152"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="48b2e-102">Gerekli ve Isteğe bağlı özellikler</span><span class="sxs-lookup"><span data-stu-id="48b2e-102">Required and Optional Properties</span></span>

<span data-ttu-id="48b2e-103">Özelliği, içermesi `null`için geçerliyse, isteğe bağlı olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="48b2e-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="48b2e-104">`null` Bir özelliğe atanacak geçerli bir değer değilse, gerekli bir özellik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="48b2e-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="48b2e-105">Kurallar</span><span class="sxs-lookup"><span data-stu-id="48b2e-105">Conventions</span></span>

<span data-ttu-id="48b2e-106">Kurala göre, .NET türü null içerebilen bir özellik isteğe bağlı (`string` `byte[]`, `int?`,, vb.) olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="48b2e-106">By convention, a property whose .NET type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="48b2e-107">CLR türü null değer içermeyen özellikler, gerekli (`int`, `decimal`, `bool`vb.) olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="48b2e-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="48b2e-108">.NET türü null değer içermeyen bir özellik isteğe bağlı olarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="48b2e-108">A property whose .NET type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="48b2e-109">Özellik her zaman Entity Framework için gerekli kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="48b2e-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="48b2e-110">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="48b2e-110">Data Annotations</span></span>

<span data-ttu-id="48b2e-111">Bir özelliğin gerekli olduğunu göstermek için, veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="48b2e-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="48b2e-112">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="48b2e-112">Fluent API</span></span>

<span data-ttu-id="48b2e-113">Bir özelliğin gerekli olduğunu göstermek için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="48b2e-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

