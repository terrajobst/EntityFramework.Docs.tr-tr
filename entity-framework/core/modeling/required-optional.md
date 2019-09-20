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
# <a name="required-and-optional-properties"></a>Gerekli ve Isteğe bağlı özellikler

Özelliği, içermesi `null`için geçerliyse, isteğe bağlı olarak kabul edilir. `null` Bir özelliğe atanacak geçerli bir değer değilse, gerekli bir özellik olarak kabul edilir.

## <a name="conventions"></a>Kurallar

Kurala göre, .NET türü null içerebilen bir özellik isteğe bağlı (`string` `byte[]`, `int?`,, vb.) olarak yapılandırılır. CLR türü null değer içermeyen özellikler, gerekli (`int`, `decimal`, `bool`vb.) olarak yapılandırılır.

> [!NOTE]  
> .NET türü null değer içermeyen bir özellik isteğe bağlı olarak yapılandırılamaz. Özellik her zaman Entity Framework için gerekli kabul edilir.

## <a name="data-annotations"></a>Veri açıklamaları

Bir özelliğin gerekli olduğunu göstermek için, veri açıklamalarını kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>Akıcı API

Bir özelliğin gerekli olduğunu göstermek için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

