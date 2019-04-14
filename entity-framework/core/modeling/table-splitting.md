---
title: Tablo bölme - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562602"
---
# <a name="table-splitting"></a>Tablo bölme

>[!NOTE]
> Bu özellik, EF Core 2.0 sürümünde yenidir.

EF Core tek bir satır için iki veya daha fazla varlık eşlemeye izin verir. Bu adlandırılır _tablo bölme_ veya _tablo paylaşımı_.

## <a name="configuration"></a>Yapılandırma

Tablo varlık türleri aynı tabloya eşlenmesi gerekir bölme kullanmak için aynı sütunlarıyla eşlenen birincil anahtarlar ve bir varlık türünün birincil anahtarı ile aynı tablodaki başka bir arasındaki yapılandırılmış en az bir ilişki var.

Tablo bölmek için yaygın bir senaryo büyük performans veya saklama için tabloda yalnızca bir sütun alt kümesi kullanıyor.

Bu örnekte `Order` bir alt kümesini temsil eden `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Ek gerekli yapılandırma diyoruz `HasBaseType((string)null)` eşleme önlemek için `DetailedOrder` aynı hiyerarşide `Order`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

Bkz: [tam örnek proje](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) daha fazla bağlam için.

## <a name="usage"></a>Kullanım

Kaydetme ve tablo bölmeyi kullanarak varlıkları sorgulama gibi diğer varlıklar aynı şekilde yapılır, tek fark bir satır paylaşma tüm varlıklar için INSERT izlenmesi gerekir.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Eşzamanlılık belirteçleri

Ardından herhangi bir tablo paylaşımı varlık türleri varsa eşzamanlı bir simge eski eşzamanlılık belirteç değeri aynı tabloya eşlenen varlıkları yalnızca biri güncelleştirildiğinde önlemek için diğer tüm varlık türleri eklenmelidir.

Kullanan kodu için kullanılmasını önlemek için mümkündür oluşturma bir gölge durumu.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]