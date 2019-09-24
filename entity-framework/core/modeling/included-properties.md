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
# <a name="including--excluding-properties"></a>Özellikleri Dahil Etme ve Dışlama

Modeldeki bir özelliğin dahil edilmesi, EF 'in bu özellik hakkında meta verilere sahip olması ve veritabanından/öğesinden değer okumaya ve yazmaya çalışacağı anlamına gelir.

## <a name="conventions"></a>Kurallar

Kurala göre, bir alıcı ve ayarlayıcı içeren ortak özellikler modele dahil edilir.

## <a name="data-annotations"></a>Veri Açıklamaları

Bir özelliği modelden dışlamak için, veri açıklamalarını kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>Akıcı API

Bir özelliği modelden dışlamak için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
