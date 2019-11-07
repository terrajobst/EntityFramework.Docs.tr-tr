---
title: Dizinler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655692"
---
# <a name="indexes"></a>Dizinlerde

Dizinler birçok veri deposu genelinde ortak bir kavramdır. Veri deposundaki uygulamaları farklılık gösterebilir, ancak bir sütuna (veya sütun kümesine) göre aramalar yapmak için kullanılır.

## <a name="conventions"></a>Kurallar

Kurala göre, yabancı anahtar olarak kullanılan her bir özellikte (veya özellik kümesinde) bir dizin oluşturulur.

## <a name="data-annotations"></a>Veri Açıklamaları

Dizinler, veri açıklamaları kullanılarak oluşturulamaz.

## <a name="fluent-api"></a>Akıcı API

Tek bir özellikte dizin belirtmek için Floent API 'sini kullanabilirsiniz. Dizinler, varsayılan olarak benzersiz değildir.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

Ayrıca, bir dizinin benzersiz olması gerektiğini de belirtebilirsiniz. Bu, iki varlığın verilen Özellik (ler) için aynı değere sahip olamaz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

Ayrıca, birden fazla sütundan oluşan bir dizin belirtebilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> Ayrı özellik kümesi başına yalnızca bir dizin vardır. Zaten bir dizini tanımlanmış, kural veya önceki yapılandırma ile tanımlanmış bir dizi özellik kümesi üzerinde bir dizin yapılandırmak için akıcı API kullanıyorsanız, bu dizinin tanımını değiştirmiş olursunuz. Kural tarafından oluşturulan bir dizini daha fazla yapılandırmak istiyorsanız bu yararlı olur.
