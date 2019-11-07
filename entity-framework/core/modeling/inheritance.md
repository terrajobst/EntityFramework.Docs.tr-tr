---
title: Devralma-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: abc1caa4d3839b7cdb52b316bcfc8f648b609b70
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655681"
---
# <a name="inheritance"></a>Devralma

EF modelindeki devralma, varlık sınıflarında devralmanın veritabanında nasıl temsil edileceğini denetlemek için kullanılır.

## <a name="conventions"></a>Kurallar

Kurala göre, devralmanın veritabanında nasıl temsil edileceğini belirleme veritabanı sağlayıcısına kadar olur. Bunun ilişkisel veritabanı sağlayıcısıyla nasıl işlendiği hakkında bilgi için bkz. [Devralma (Ilişkisel veritabanı)](relational/inheritance.md) .

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
