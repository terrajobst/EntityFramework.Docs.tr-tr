---
title: Tablo bölme-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 684fcfbb66debfd1b89e23c8aaf0a32909378c6b
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149195"
---
# <a name="table-splitting"></a>Tablo bölme

>[!NOTE]
> Bu özellik EF Core 2,0 ' de yenidir.

EF Core iki veya daha fazla varlığı tek bir satıra eşlemeye izin verir. Bu, _tablo bölme_ veya _tablo paylaşma_olarak adlandırılır.

## <a name="configuration"></a>Yapılandırma

Varlık türlerinin tablo bölmeyi kullanmak için aynı tabloya eşlenmesi gerekir, birincil anahtarların aynı sütunlara ve bir varlık türünün birincil anahtarı ile aynı tabloda diğeri arasında yapılandırılmış en az bir ilişkiye sahip olması gerekir.

Tablo bölme için yaygın bir senaryo, daha fazla performans veya kapsülleme için tablodaki sütunların yalnızca bir alt kümesini kullanmaktır.

Bu örnekte `Order` , `DetailedOrder`bir alt kümesini temsil eder.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Gereken yapılandırmaya ek olarak, ile aynı sütuna `Property(o => o.Status).HasColumnName("Status")` `Order.Status`eşleme `DetailedOrder.Status` çağrısı yaptık.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> Daha fazla bağlam için bkz. [tam örnek proje](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .

## <a name="usage"></a>Kullanım

Tablo bölme kullanarak varlıkları kaydetme ve sorgulama, diğer varlıklarla aynı şekilde yapılır. EF Core 3,0 ' den başlayarak, bağımlı varlık başvurusu olabilir `null`. Bağımlı varlık `NULL` tarafından kullanılan tüm sütunlar veritabanı ise, sorgulanan sırada bir örnek oluşturulmaz. Bu, tüm özelliklerin isteğe bağlı ve olarak `null`ayarlandığı anlamına gelir, bu da beklenmeyebilir.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Eşzamanlılık belirteçleri

Bir tabloyu paylaşan varlık türlerinden herhangi birinde bir eşzamanlılık belirteci varsa, aynı tabloyla eşlenmiş varlıkların yalnızca biri güncelleştirildiği zaman eski bir eşzamanlılık belirteci değerinden kaçınmak için diğer tüm varlık türlerine dahil olması gerekir.

Bunu, tüketim kodunda ortaya çıkarmamak için gölge durumundaki bir oluşturma olasılığı vardır.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]