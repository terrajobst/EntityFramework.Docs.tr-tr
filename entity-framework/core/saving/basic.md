---
title: Temel kaydetme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 066d67d6104316832a33f5a3648f1f2fa6cc9c50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417637"
---
# <a name="basic-save"></a>Temel Kaydetme

Bağlam ve varlık sınıflarınızı kullanarak verileri ekleme, değiştirme ve kaldırma hakkında bilgi edinin.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) GitHub ' da görebilirsiniz.

## <a name="adding-data"></a>Veri ekleme

Varlık sınıflarınızın yeni örneklerini eklemek için *Dbset. Add* metodunu kullanın. *SaveChanges*' i çağırdığınızda veriler veritabanına eklenecektir.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Add, Attach ve Update yöntemleri, [Ilişkili veri](related-data.md) bölümünde açıklandığı gibi, bunlara geçirilen varlıkların tam grafiğinde çalışır. Alternatif olarak, EntityEntry. State özelliği yalnızca tek bir varlığın durumunu ayarlamak için kullanılabilir. Örneğin, `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Verileri güncelleştirme

EF, bağlam tarafından izlenen mevcut bir varlıkta yapılan değişiklikleri otomatik olarak algılar. Bu, veritabanından yüklediğiniz/sorgulayan varlıkları ve daha önce eklenen ve veritabanına kaydedilen varlıkları içerir.

Yalnızca özelliklere atanan değerleri değiştirin ve ardından *SaveChanges*öğesini çağırın.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Verileri silme

Varlık sınıflarınızın örneklerini silmek için *Dbset. Remove* metodunu kullanın.

Varlık veritabanında zaten mevcutsa, *SaveChanges*sırasında silinir. Varlık henüz veritabanına kaydedilmediyse (yani, eklenen olarak izleniyorsa), içerikten kaldırılır ve *SaveChanges* çağrıldığında artık eklenmeyecektir.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Tek bir SaveChanges içindeki birden çok Işlem

Birden çok ekleme/güncelleştirme/kaldırma işlemini tek bir *SaveChanges*çağrısıyla birleştirebilirsiniz.

> [!NOTE]  
> Çoğu veritabanı sağlayıcısı için *SaveChanges* işlem. Bu, tüm işlemlerin başarılı veya başarısız olduğu anlamına gelir ve işlemler hiçbir durumda kısmen uygulanmaz.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
