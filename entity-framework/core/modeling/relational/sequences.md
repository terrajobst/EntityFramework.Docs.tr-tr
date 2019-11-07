---
title: Diziler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656110"
---
# <a name="sequences"></a>Diziler

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

Dizi, veritabanında ardışık bir sayısal değer oluşturur. Diziler belirli bir tabloyla ilişkilendirilmemiş.

## <a name="conventions"></a>Kurallar

Kurala göre, diziler ' de modele tanıtılmaz.

## <a name="data-annotations"></a>Veri Açıklamaları

Veri ek açıklamalarını kullanarak bir sıra yapılandıramazsınız.

## <a name="fluent-api"></a>Akıcı API

Modelde bir sıra oluşturmak için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

Ayrıca, dizinin şeması, başlangıç değeri ve artışı gibi ek yönlerini de yapılandırabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

Bir sıra alındıktan sonra, modelinizdeki özelliklerin değerlerini oluşturmak için bunu kullanabilirsiniz. Örneğin, sıradaki değeri sırayla eklemek için [varsayılan değerleri](default-values.md) kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
