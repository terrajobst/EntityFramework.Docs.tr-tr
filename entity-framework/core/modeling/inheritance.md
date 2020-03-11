---
title: Devralma-EF Core
description: Entity Framework Core kullanarak varlık türü devralmayı yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 507854e3acc0347adee612e516b3e2e0b10f55cf
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417297"
---
# <a name="inheritance"></a>Devralma

EF bir .NET tür hiyerarşisini bir veritabanıyla eşleyebilir. Bu, temel ve türetilmiş türleri kullanarak .NET varlıklarınızı kodda her zamanki gibi yazmanızı sağlar ve uygun veritabanı şemasını, sorun sorgularını vb. kullanarak sorunsuz bir şekilde oluşturabilirsiniz. Bir tür hiyerarşisinin nasıl eşlendiğine ilişkin gerçek Ayrıntılar sağlayıcıya bağımlıdır; Bu sayfa, bir ilişkisel veritabanı bağlamında devralma desteğini açıklar.

Şu anda EF Core yalnızca hiyerarşi başına tablo (TPH) modelini destekler. TPH, hiyerarşideki tüm türlerin verilerini depolamak için tek bir tablo kullanır ve her bir satırın hangi türü temsil ettiğini belirlemek için bir Ayrıştırıcı sütunu kullanılır.

> [!NOTE]
> EF6 tarafından desteklenen tablo/tür (TPT) ve tablo-somut türü (TPC) EF Core tarafından henüz desteklenmemektedir. TPT EF Core 5,0 için planlanmış bir ana özelliktir.

## <a name="entity-type-hierarchy-mapping"></a>Varlık türü hiyerarşisi eşleme

Kurala göre, yalnızca modele iki veya daha fazla devralınmış tür açık olarak dahil ediliyorsa, EF yalnızca devralmayı ayarlar. EF, modele dahil olmayan temel veya türetilmiş türler için otomatik olarak taramayacaktır.

Devralma hiyerarşisindeki her bir tür için bir DbSet sunarak modele türler ekleyebilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

Bu model aşağıdaki veritabanı şemasına eşlendi (her satırda hangi *Blog* türünün depolandığını belirleyen örtük olarak oluşturulmuş *ayrıştırıcı* sütununu aklınızda bulunur):

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> Veritabanı sütunları, TPH eşlemesi kullanılırken gerektiğinde otomatik olarak null yapılabilir hale getirilir. Örneğin, normal *Blog* örnekleri bu özelliğe sahip olmadığından *rssurl* sütunu null yapılabilir.

Hiyerarşide bir veya daha fazla varlık için bir DbSet göstermek istemiyorsanız, bu API 'lerin modele dahil olduklarından emin olmak için akıcı API 'yi de kullanabilirsiniz.

> [!TIP]
> Kurallara dayanmıyorsanız, `HasBaseType`kullanarak temel türü açıkça belirtebilirsiniz. Bir varlık türünü hiyerarşiden kaldırmak için `.HasBaseType((Type)null)` de kullanabilirsiniz.

## <a name="discriminator-configuration"></a>Ayrıştırıcı yapılandırması

Ayrıştırıcı sütununun adını ve türünü ve hiyerarşideki her türü tanımlamak için kullanılan değerleri yapılandırabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

Yukarıdaki örneklerde EF, Ayrıştırıcıyı hiyerarşinin temel varlığına örtük bir şekilde bir [Gölge Özellik](xref:core/modeling/shadow-properties) olarak eklemiştir. Bu özellik, herhangi bir şekilde yapılandırılabilir:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

Son olarak, ayrıştırıcı, varlığınızda düzenli bir .NET özelliğine de eşleştirilebilir:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

## <a name="shared-columns"></a>Paylaşılan sütunlar

Varsayılan olarak, hiyerarşideki iki eşdüzey varlık türünün aynı ada sahip bir özelliği varsa, bunlar iki ayrı sütuna eşleştirilir. Ancak, türleri aynıysa aynı veritabanı sütunuyla eşleştirilebilir:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]
