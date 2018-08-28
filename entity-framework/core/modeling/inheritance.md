---
title: Devralma - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: c5fa9d13dec8cfc3e1cac69e471f509cbbb9e4c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995902"
---
# <a name="inheritance"></a>Devralma

Devralma EF modeli, varlık sınıflarının Devralmada veritabanında nasıl temsil edildiğini denetlemek için kullanılır.

## <a name="conventions"></a>Kurallar

Kural olarak, bu devralma veritabanında nasıl gösterileceğini belirlemek için veritabanı sağlayıcısı kadar olur. Bkz: [devralma (ilişkisel veritabanı)](relational/inheritance.md) için bu bir ilişkisel veritabanı sağlayıcısı ile nasıl işlenir.

İki veya daha fazla devralınan türlerin açıkça modelde varsa EF yalnızca devralma ayarlayın. EF modeli bulunmayan temel veya türetilmiş türleri için taramaz. Göstererek modelde türleri içerebilir bir *olan DB<TEntity>*  devralma hiyerarşisinde her türü için.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Kullanıma sunmak istemiyorsanız bir *olan DB<TEntity>*  hiyerarşideki bir veya daha fazla varlık için modele dahil oldukları emin olmak için Fluent API'sini kullanabilirsiniz.
Ve kurallarını güvenmeyin durumunda kullanarak açıkça temel tür belirtebilirsiniz `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Kullanabileceğiniz `.HasBaseType((Type)null)` bu hiyerarşisinden bir varlık türü kaldırılamadı.

## <a name="data-annotations"></a>Veri ek açıklamaları

Veri ek açıklamaları, devralmayı yapılandırmak için kullanamazsınız.

## <a name="fluent-api"></a>Fluent API'si

Devralma Fluent API'si, kullanmakta olduğunuz veritabanı sağlayıcısına bağlıdır. Bkz: [devralma (ilişkisel veritabanı)](relational/inheritance.md) gerçekleştirmek için ilişkisel veritabanı sağlayıcısı yapılandırması.
