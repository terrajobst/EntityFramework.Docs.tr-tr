---
title: Devralma (Ilişkisel veritabanı)-EF Core
description: Entity Framework Core kullanarak ilişkisel bir veritabanında varlık türü devralmayı yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 30e25aa2968ceab03404baddb46e0ae59fc3ea6b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824750"
---
# <a name="inheritance-relational-database"></a>Devralma (İlişkisel Veritabanı)

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

EF modelindeki devralma, varlık sınıflarında devralmanın veritabanında nasıl temsil edileceğini denetlemek için kullanılır.

> [!NOTE]  
> Şu anda EF Core ' de yalnızca hiyerarşi başına tablo (TPH) düzeni uygulanır. Tablo/tür (TPT) ve tablo başına somut-tür (TPC) gibi diğer yaygın desenler henüz kullanılamamaktadır.

## <a name="conventions"></a>Kurallar

Devralma varsayılan olarak, hiyerarşi başına tablo (TPH) düzeni kullanılarak eşleştirilir. TPH, hiyerarşideki tüm türlerin verilerini depolamak için tek bir tablo kullanır. Bir Ayrıştırıcı sütunu, her bir satırın hangi tür temsil ettiğini belirlemek için kullanılır.

EF Core, yalnızca modele açıkça iki veya daha fazla devralınmış tür varsa devralma ayarı yapılır (daha fazla ayrıntı için bkz. [Devralma](../inheritance.md) ).

Aşağıda, bir basit devralma senaryosunu ve TPH deseninin kullanıldığı bir ilişkisel veritabanı tablosunda depolanan verileri gösteren bir örnek verilmiştir. *Ayrıştırıcı* sütunu, her satırda hangi *Blog* türünün depolandığını tanımlar.

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![görüntü](_static/inheritance-tph-data.png)

>[!NOTE]
> Veritabanı sütunları, TPH eşlemesi kullanılırken gerektiğinde otomatik olarak null yapılabilir hale getirilir.

## <a name="data-annotations"></a>Veri Açıklamaları

Devralma yapılandırmak için veri ek açıklamalarını kullanamazsınız.

## <a name="fluent-api"></a>Akıcı API

Ayrıştırıcı sütununun adını ve türünü ve hiyerarşideki her türü tanımlamak için kullanılan değerleri yapılandırmak için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a>Ayrıştırıcı özelliğini yapılandırma

Yukarıdaki örneklerde, ayrıştırıcı hiyerarşinin temel varlığında bir [Gölge Özellik](xref:core/modeling/shadow-properties) olarak oluşturulur. Modeldeki bir özellik olduğu için, diğer özellikler gibi yapılandırılabilir. Örneğin, varsayılan, kural olarak ayrıştırıcı kullanılıyorsa en fazla uzunluğu ayarlamak için:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

Ayrıştırıcı, varlığınızda bir .NET özelliğine de eşleştirilebilir ve bunu yapılandırabilir. Örneğin:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a>Paylaşılan sütunlar

İki eşdüzey varlık türünün aynı ada sahip bir özelliği olduğunda, varsayılan olarak iki ayrı sütuna eşlenir. Ancak uyumluysa, aynı sütunla eşleştirilebilir:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]