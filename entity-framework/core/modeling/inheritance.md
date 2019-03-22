---
title: Devralma - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: f6b5c8f5a398ac1e28e29bc17f0674c5b76d7b20
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319133"
---
# <a name="inheritance"></a>Devralma

Devralma EF modeli, varlık sınıflarının Devralmada veritabanında nasıl temsil edildiğini denetlemek için kullanılır.

## <a name="conventions"></a>Kurallar

Kural olarak, bu devralma veritabanında nasıl gösterileceğini belirlemek için veritabanı sağlayıcısı kadar olur. Bkz: [devralma (ilişkisel veritabanı)](relational/inheritance.md) için bu bir ilişkisel veritabanı sağlayıcısı ile nasıl işlenir.

İki veya daha fazla devralınan türlerin açıkça modelde varsa EF yalnızca devralma ayarlayın. EF modeli bulunmayan temel veya türetilmiş türleri için taramaz. Göstererek modelde türleri içerebilir bir *olan DB<TEntity>*  devralma hiyerarşisinde her türü için.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Kullanıma sunmak istemiyorsanız bir *olan DB<TEntity>*  hiyerarşideki bir veya daha fazla varlık için modele dahil oldukları emin olmak için Fluent API'sini kullanabilirsiniz.
Ve kurallarını güvenmeyin, açıkça kullanarak temel tür belirtebilirsiniz `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Kullanabileceğiniz `.HasBaseType((Type)null)` bu hiyerarşisinden bir varlık türü kaldırılamadı.

## <a name="data-annotations"></a>Veri ek açıklamaları

Veri ek açıklamaları, devralmayı yapılandırmak için kullanamazsınız.

## <a name="fluent-api"></a>Fluent API'si

Devralma Fluent API'si, kullanmakta olduğunuz veritabanı sağlayıcısına bağlıdır. Bkz: [devralma (ilişkisel veritabanı)](relational/inheritance.md) gerçekleştirmek için ilişkisel veritabanı sağlayıcısı yapılandırması.
