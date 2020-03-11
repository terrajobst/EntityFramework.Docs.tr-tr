---
title: Tablo bölme-EF Core
description: Entity Framework Core kullanarak tablo bölmeyi yapılandırma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 01/03/2020
uid: core/modeling/table-splitting
ms.openlocfilehash: de24f8903af79ebd7f68e6b74288257883c1fa8d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417400"
---
# <a name="table-splitting"></a>Tablo Bölme

EF Core iki veya daha fazla varlığı tek bir satıra eşlemeye izin verir. Bu, _tablo bölme_ veya _tablo paylaşma_olarak adlandırılır.

## <a name="configuration"></a>Yapılandırma

Varlık türlerinin tablo bölmeyi kullanmak için aynı tabloya eşlenmesi gerekir, birincil anahtarların aynı sütunlara ve bir varlık türünün birincil anahtarı ile aynı tabloda diğeri arasında yapılandırılmış en az bir ilişkiye sahip olması gerekir.

Tablo bölme için yaygın bir senaryo, daha fazla performans veya kapsülleme için tablodaki sütunların yalnızca bir alt kümesini kullanmaktır.

Bu örnekte `Order` bir `DetailedOrder`alt kümesini temsil eder.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Gerekli yapılandırmaya ek olarak, `DetailedOrder.Status` `Order.Status`ile aynı sütuna eşlemek için `Property(o => o.Status).HasColumnName("Status")` çağırıyoruz.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting)]

> [!TIP]
> Daha fazla bağlam için bkz. [tam örnek proje](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .

## <a name="usage"></a>Kullanım

Tablo bölme kullanarak varlıkları kaydetme ve sorgulama, diğer varlıklarla aynı şekilde yapılır:

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="optional-dependent-entity"></a>İsteğe bağlı bağımlı varlık

> [!NOTE]
> Bu özellik EF Core 3,0 ' de tanıtılmıştı.

Bağımlı bir varlık tarafından kullanılan tüm sütunlar veritabanında `NULL`, bu durumda sorgulanan bir örnek oluşturulmaz. Bu, Principal 'ın ilişki özelliğinin null olacağı, isteğe bağlı bir bağımlı varlığın modellemesini sağlar. Bunun aynı zamanda bağımlı olduğu tüm özelliklerin tümünün de olacağını ve `null`olarak ayarlandığını unutmayın.

## <a name="concurrency-tokens"></a>Eşzamanlılık belirteçleri

Bir tabloyu paylaşan varlık türlerinden herhangi birinde eşzamanlılık belirteci varsa, bu, diğer tüm varlık türlerine de dahil olmalıdır. Aynı tabloyla eşlenmiş varlıkların yalnızca biri güncelleştirildiği zaman eski bir eşzamanlılık belirteci değerinden kaçınmak için bu gereklidir.

Eşzamanlılık belirtecini, tüketim kodunda açığa çıkarmamak için bir [gölge özelliği](xref:core/modeling/shadow-properties)olarak oluştur mümkündür:

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
