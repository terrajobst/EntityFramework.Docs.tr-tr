---
title: Varlık Çerçeve Çekirdeğine Genel Bakış - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416848"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core, popüler Entity Framework veri erişim teknolojisinin hafif, genişletilebilir, [açık kaynak](https://github.com/aspnet/EntityFrameworkCore) kodlu ve çapraz platformlu bir sürümüdür.

EF Core, .NET geliştiricilerin .NET nesnelerini kullanarak bir veritabanıyla çalışmasını sağlayan ve genellikle yazmaları gereken veri erişim kodunun çoğuna olan gereksinimi ortadan kaldıran bir nesne ilişkisisel mapper (O/RM) olarak hizmet verebilir.

EF Core birçok veritabanı altyapılarını destekler, ayrıntılar için [Veritabanı Sağlayıcıları'na](providers/index.md) bakın.

## <a name="the-model"></a>The Model

EF Core ile veri erişimi bir model kullanılarak gerçekleştirilir. Bir model, varlık sınıfları ve veritabanıyla bir oturumu temsil eden ve verileri sorgulayıp kaydetmenize olanak tanıyan bir bağlam nesnesi oluşur. Bkz. Daha fazla bilgi edinmek için [Model Oluşturma.](modeling/index.md)

Varolan bir veritabanından bir model oluşturabilir, veritabanınızla eşleşecek bir modeli el koduyla kodlayabilir veya modelinizden bir veritabanı oluşturmak için [EF Geçişleri'ni](managing-schemas/migrations/index.md) kullanabilir ve modeliniz zaman içinde değiştikçe bu modeli geliştirebilirsiniz.

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a>Sorgulama

Varlık sınıflarınızın örnekleri, Language Integrated Query (LINQ) kullanılarak veritabanından alınır. Daha fazla bilgi edinmek için [Verileri Sorgulama'ya](querying/index.md) bakın.

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a>Verileri Kaydetme

Veriler, varlık sınıflarınızın örnekleri kullanılarak veritabanında oluşturulur, silinir ve değiştirilir. Daha fazla bilgi edinmek için [Verileri Kaydetme'ye](saving/index.md) bakın.

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a>Sonraki adımlar

Giriş eğitimleri için Entity [Framework Core ile Başlarken'e](get-started/index.md)bakın.
