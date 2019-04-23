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
# <a name="required-and-optional-properties"></a>Gerekli ve isteğe bağlı özellikler

Bir özellik içeren için geçerli ise isteğe bağlı olarak sayılır `null`. Varsa `null` gerekli özelliği olduğu düşünülür sonra bir özelliğe atanacak geçerli bir değer değil.

## <a name="conventions"></a>Kurallar

Kural gereği, CLR türü, null içerebilir bir özellik isteğe bağlı olarak yapılandırılır (`string`, `int?`, `byte[]`vb..). CLR türü, null içeremez özellikleri yapılandırılması gerektiği gibi (`int`, `decimal`, `bool`vb..).

> [!NOTE]  
> Bir özelliğin CLR türü null içeremez, isteğe bağlı olarak yapılandırılamaz. Entity Framework tarafından gerekli özellik her zaman değerlendirilir.

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir özelliğin gerekli olduğunu belirtmek için veri ek açıklamaları kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si, bir özelliğin gerekli olduğunu belirtmek için kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

