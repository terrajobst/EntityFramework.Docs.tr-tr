---
title: Tablo bölme-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: a3a2e5842a6c6b4b490084d205a0d44bb46c17ee
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656036"
---
# <a name="table-splitting"></a>Tablo Bölme

>[!NOTE]
> Bu özellik EF Core 2,0 ' de yenidir.

EF Core iki veya daha fazla varlığı tek bir satıra eşlemeye izin verir. Bu, _tablo bölme_ veya _tablo paylaşma_olarak adlandırılır.

## <a name="configuration"></a>Yapılandırma

Varlık türlerinin tablo bölmeyi kullanmak için aynı tabloya eşlenmesi gerekir, birincil anahtarların aynı sütunlara ve bir varlık türünün birincil anahtarı ile aynı tabloda diğeri arasında yapılandırılmış en az bir ilişkiye sahip olması gerekir.

Tablo bölme için yaygın bir senaryo, daha fazla performans veya kapsülleme için tablodaki sütunların yalnızca bir alt kümesini kullanmaktır.

Bu örnekte `Order` bir `DetailedOrder`alt kümesini temsil eder.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Gerekli yapılandırmaya ek olarak, `DetailedOrder.Status` `Order.Status`ile aynı sütuna eşlemek için `Property(o => o.Status).HasColumnName("Status")` çağırıyoruz.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> Daha fazla bağlam için bkz. [tam örnek proje](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .

## <a name="usage"></a>Kullanım

Tablo bölme kullanarak varlıkları kaydetme ve sorgulama, diğer varlıklarla aynı şekilde yapılır. EF Core 3,0 ile başlayarak, bağımlı varlık başvurusu `null`olabilir. Bağımlı varlık tarafından kullanılan tüm sütunlar veritabanı `NULL`, sorgulandığında hiçbir örnek oluşturulmaz. Ayrıca, tüm özellikler isteğe bağlıdır ve `null`olarak ayarlanır ve bu da beklenmeyebilir.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Eşzamanlılık belirteçleri

Bir tabloyu paylaşan varlık türlerinden herhangi birinde bir eşzamanlılık belirteci varsa, aynı tabloyla eşlenmiş varlıkların yalnızca biri güncelleştirildiği zaman eski bir eşzamanlılık belirteci değerinden kaçınmak için diğer tüm varlık türlerine dahil olması gerekir.

Bunu, tüketim kodunda ortaya çıkarmamak için gölge durumundaki bir oluşturma olasılığı vardır.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
