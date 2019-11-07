---
title: Türler hariç & içerme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655735"
---
# <a name="including--excluding-types"></a>Türleri Dahil Etme ve Dışlama

Modele bir tür eklemek, EF 'in bu tür hakkında meta verilere sahip olduğu ve veritabanından örnekleri okumaya ve veritabanına yazmayı deneyeceği anlamına gelir.

## <a name="conventions"></a>Kurallar

Kurala göre, bağlamınızın `DbSet` özelliklerinde gösterilen türler modelinize dahildir. Ayrıca, `OnModelCreating` yönteminde bahsedilen türler de dahildir. Son olarak, bulunan türlerin gezinti özelliklerini yinelemeli olarak inceleyerek bulunan türler da modele dahil edilir.

**Örneğin, aşağıdaki kod listesinde, tüm üç tür bulunur:**

* bağlamda bir `DbSet` özelliğinde kullanıma sunulduğundan `Blog`

* `Blog.Posts` gezinti özelliği aracılığıyla bulunduğundan `Post`

* `AuditEntry` `OnModelCreating` belirtildiği için

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a>Veri Açıklamaları

Bir türü modelden dışlamak için veri açıklamalarını kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>Akıcı API

Bir tür modelden dışlamak için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
