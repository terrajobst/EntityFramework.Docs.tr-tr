---
title: Entity Framework Core genel bakış EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655618"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core, popüler Entity Framework veri erişim teknolojisinin basit, Genişletilebilir, [Açık kaynaklı](https://github.com/aspnet/EntityFrameworkCore) ve platformlar arası bir sürümüdür.

EF Core, nesne ilişkisel Eşleyici (O/RM) olarak görev yapabilir, .NET geliştiricilerin .NET nesnelerini kullanarak bir veritabanıyla çalışmasını sağlar ve genellikle yazması gereken veri erişimi kodunun çoğunu ortadan kaldırır.

EF Core birçok veritabanı altyapısını destekler, Ayrıntılar için bkz. [veritabanı sağlayıcıları](providers/index.md) .

## <a name="the-model"></a>Model

EF Core, veri erişimi bir model kullanılarak gerçekleştirilir. Bir model, varlık sınıflarından ve veritabanı ile bir oturumu temsil eden bir bağlam nesnesinden oluşur ve verileri sorgulayıp kaydetmenizi sağlar. Daha fazla bilgi için bkz. [model oluşturma](modeling/index.md) .

Var olan bir veritabanından bir model oluşturabilir, bir modeli veritabanınızdan eşlemek üzere kodlayabilir veya modelinizden bir veritabanı oluşturmak için [EF geçişleri](managing-schemas/migrations/index.md) kullanabilirsiniz.

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a>Oluştu

Varlık sınıflarınızın örnekleri, dil ile tümleşik sorgu (LINQ) kullanılarak veritabanından alınır. Daha fazla bilgi için bkz. [veri sorgulama](querying/index.md) .

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a>Verileri Kaydetme

Veri, varlık sınıflarınızın örnekleri kullanılarak oluşturulur, silinir ve değiştirilir. Daha fazla bilgi edinmek için [verileri kaydetme](saving/index.md) konusuna bakın.

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a>Sonraki adımlar

Tanıtım öğreticileri için bkz. [Entity Framework Core kullanmaya](get-started/index.md)başlama.
