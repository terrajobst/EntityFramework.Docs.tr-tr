---
title: Devralma-EF Core
description: Entity Framework Core kullanarak varlık türü devralmayı yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 4d43a432174c92ab7f3f9d78a234aefb0a4a17e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824683"
---
# <a name="inheritance"></a>Devralma

EF modelindeki devralma, varlık sınıflarında devralmanın veritabanında nasıl temsil edileceğini denetlemek için kullanılır.

## <a name="conventions"></a>Kurallar

Varsayılan olarak, devralmanın veritabanında nasıl temsil edileceğini belirleme veritabanı sağlayıcısına ait olur. Bunun ilişkisel veritabanı sağlayıcısıyla nasıl işlendiği hakkında bilgi için bkz. [Devralma (Ilişkisel veritabanı)](relational/inheritance.md) .

EF yalnızca, modele iki veya daha fazla devralınan tür açık olarak dahil edil, devralma işlemini ayarlar. EF, başka türlü modele dahil olmayan temel veya türetilmiş türler için tarama olmayacaktır. Devralma hiyerarşisindeki her tür için bir *Dbset\<TEntity >* sunarak modele türler ekleyebilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Hiyerarşide bir veya daha fazla varlık için bir *Dbset\<TEntity >* göstermek istemiyorsanız, bu API 'lerin modele dahil olduklarından emin olmak IÇIN akıcı API 'yi kullanabilirsiniz.
Kurallara dayanmıyorsanız, `HasBaseType`kullanarak temel türü açıkça belirtebilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Hiyerarşisinden bir varlık türünü kaldırmak için `.HasBaseType((Type)null)` kullanabilirsiniz.

## <a name="data-annotations"></a>Veri Açıklamaları

Devralma yapılandırmak için veri ek açıklamalarını kullanamazsınız.

## <a name="fluent-api"></a>Akıcı API

Devralma için akıcı API, kullanmakta olduğunuz veritabanı sağlayıcısına bağlıdır. İlişkisel bir veritabanı sağlayıcısı için gerçekleştirebileceğiniz yapılandırma için bkz. [Devralma (Ilişkisel veritabanı)](relational/inheritance.md) .
