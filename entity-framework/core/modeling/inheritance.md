---
title: Devralma-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: 1f20c455176b5922364584f8c7688c15a4c3f0f9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197291"
---
# <a name="inheritance"></a>Devralma

EF modelindeki devralma, varlık sınıflarında devralmanın veritabanında nasıl temsil edileceğini denetlemek için kullanılır.

## <a name="conventions"></a>Kurallar

Kurala göre, devralmanın veritabanında nasıl temsil edileceğini belirleme veritabanı sağlayıcısına kadar olur. Bunun ilişkisel veritabanı sağlayıcısıyla nasıl işlendiği hakkında bilgi için bkz. [Devralma (Ilişkisel veritabanı)](relational/inheritance.md) .

EF yalnızca, modele iki veya daha fazla devralınan tür açık olarak dahil edil, devralma işlemini ayarlar. EF, başka türlü modele dahil olmayan temel veya türetilmiş türler için tarama olmayacaktır. Devralma hiyerarşisindeki her bir tür için bir *dbset<TEntity>*  sunarak modele türler ekleyebilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Hiyerarşide bir veya daha fazla varlık için bir *dbset<TEntity>*  göstermek istemiyorsanız, bu API 'lerin modele dahil olduklarından emin olmak için akıcı API 'yi kullanabilirsiniz.
Kurallara bağlı değilseniz, temel türü açıkça kullanarak `HasBaseType`belirtebilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Hiyerarşisinden bir varlık `.HasBaseType((Type)null)` türünü kaldırmak için ' i kullanabilirsiniz.

## <a name="data-annotations"></a>Veri Açıklamaları

Devralma yapılandırmak için veri ek açıklamalarını kullanamazsınız.

## <a name="fluent-api"></a>Akıcı API

Devralma için akıcı API, kullanmakta olduğunuz veritabanı sağlayıcısına bağlıdır. İlişkisel bir veritabanı sağlayıcısı için gerçekleştirebileceğiniz yapılandırma için bkz. [Devralma (Ilişkisel veritabanı)](relational/inheritance.md) .
