---
title: Özellikleri hariç & içerme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197419"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="ebe47-102">Özellikleri Dahil Etme ve Dışlama</span><span class="sxs-lookup"><span data-stu-id="ebe47-102">Including & Excluding Properties</span></span>

<span data-ttu-id="ebe47-103">Modeldeki bir özelliğin dahil edilmesi, EF 'in bu özellik hakkında meta verilere sahip olması ve veritabanından/öğesinden değer okumaya ve yazmaya çalışacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ebe47-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="ebe47-104">Kurallar</span><span class="sxs-lookup"><span data-stu-id="ebe47-104">Conventions</span></span>

<span data-ttu-id="ebe47-105">Kurala göre, bir alıcı ve ayarlayıcı içeren ortak özellikler modele dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="ebe47-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ebe47-106">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="ebe47-106">Data Annotations</span></span>

<span data-ttu-id="ebe47-107">Bir özelliği modelden dışlamak için, veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ebe47-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="ebe47-108">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="ebe47-108">Fluent API</span></span>

<span data-ttu-id="ebe47-109">Bir özelliği modelden dışlamak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ebe47-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
