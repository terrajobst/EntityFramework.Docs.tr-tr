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
# <a name="including--excluding-properties"></a>Dahil olan ve dışlanan Özellikler

Model bir özelliği dahil olmak üzere EF bu özellik hakkındaki meta veriler olduğunu gösterir ve okuma ve yazma veritabanı / için değerleri dener.

## <a name="conventions"></a>Kurallar

Kural gereği, genel özellikleri bir alıcı ve ayarlayıcı ile modele dahil edilir.

## <a name="data-annotations"></a>Veri ek açıklamaları

Modelden bir özelliği dışarıda için veri ek açıklamaları kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>Fluent API'si

Bir özellik modelden dışlanacak Fluent API'sini kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
