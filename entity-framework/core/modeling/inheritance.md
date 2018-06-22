---
title: Devralma - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054198"
---
# <a name="inheritance"></a>Devralma

Devralma EF modeldeki varlık sınıflarını devralma veritabanında nasıl temsil denetlemek için kullanılır.

## <a name="conventions"></a>Kurallar

Kurala göre bu veritabanında devralma nasıl gösterileceğini belirlemek için veritabanı sağlayıcısı kadar olur. Bkz: [devralma (ilişkisel veritabanı)](relational/inheritance.md) için bu bir ilişkisel veritabanı sağlayıcısı ile nasıl işlenir.

İki veya daha fazla devralınan türleri açıkça modelde varsa EF yalnızca devralma ayarlayın. EF modelinde bulunmayan temel veya türetilmiş türler için taramaz. Göstererek modelde türleri içerebilir bir *DbSet<TEntity>*  her türünün Devralma Hiyerarşisi.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Kullanıma istemiyorsanız, bir *DbSet<TEntity>*  hiyerarşideki bir veya daha fazla varlıklar için Fluent API modele dahil bunlar emin olmak için kullanabilirsiniz.
Ve kurallarına güvenmeyin, açıkça kullanarak temel türünü belirtebilirsiniz `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Kullanabileceğiniz `.HasBaseType((Type)null)` hiyerarşisinden bir varlık türünü kaldırmak için.

## <a name="data-annotations"></a>Veri ek açıklamaları

Devralma yapılandırmak için veri ek açıklamaları kullanamazsınız.

## <a name="fluent-api"></a>Fluent API'si

Devralma Fluent API'si kullandığınız veritabanı sağlayıcısına bağlıdır. Bkz: [devralma (ilişkisel veritabanı)](relational/inheritance.md) gerçekleştirmek için bir ilişkisel veritabanı sağlayıcı yapılandırması için.
