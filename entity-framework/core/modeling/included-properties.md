---
title: Dahil olan ve dışlanan özellikler - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929831"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="c74eb-102">Dahil olan ve dışlanan Özellikler</span><span class="sxs-lookup"><span data-stu-id="c74eb-102">Including & Excluding Properties</span></span>

<span data-ttu-id="c74eb-103">Model bir özelliği dahil olmak üzere EF bu özellik hakkındaki meta veriler olduğunu gösterir ve okuma ve yazma veritabanı / için değerleri dener.</span><span class="sxs-lookup"><span data-stu-id="c74eb-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="c74eb-104">Kurallar</span><span class="sxs-lookup"><span data-stu-id="c74eb-104">Conventions</span></span>

<span data-ttu-id="c74eb-105">Kural gereği, genel özellikleri bir alıcı ve ayarlayıcı ile modele dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="c74eb-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c74eb-106">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="c74eb-106">Data Annotations</span></span>

<span data-ttu-id="c74eb-107">Modelden bir özelliği dışarıda için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c74eb-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="c74eb-108">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="c74eb-108">Fluent API</span></span>

<span data-ttu-id="c74eb-109">Bir özellik modelden dışlanacak Fluent API'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c74eb-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
