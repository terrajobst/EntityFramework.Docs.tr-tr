---
title: Temel Kaydet - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 066d67d6104316832a33f5a3648f1f2fa6cc9c50
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417637"
---
# <a name="basic-save"></a>Temel Kaydetme

Bağlam ve varlık sınıflarınızı kullanarak veri eklemeyi, değiştirmeyi ve kaldırmayı öğrenin.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) GitHub'da görüntüleyebilirsiniz.

## <a name="adding-data"></a>Veri Ekleme

Varlık sınıflarınızın yeni örneklerini eklemek için *DbSet.Add* yöntemini kullanın. *SaveChanges'ı*aradiğinizde veriler veritabanına eklenir.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Ekle, Ekle ve Güncelleştir yöntemleri, [İlgili Veriler](related-data.md) bölümünde açıklandığı gibi, kendilerine geçen varlıkların tam grafiğinde çalışır. Alternatif olarak, EntityEntry.State özelliği sadece tek bir varlığın durumunu ayarlamak için kullanılabilir. Örneğin, `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Verileri Güncelleme

EF, bağlam tarafından izlenen varolan bir varlıkta yapılan değişiklikleri otomatik olarak algılar. Bu, veritabanından yüklediğiniz/sorguladığınız varlıkları ve daha önce eklenen ve veritabanına kaydedilmiş varlıkları içerir.

Yalnızca özelliklere atanan değerleri değiştirin ve ardından *SaveChanges'ı*arayın.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Veri silme

Varlık sınıflarınızın örneklerini silmek için *DbSet.Remove* yöntemini kullanın.

Varlık veritabanında zaten varsa, *SaveChanges*sırasında silinir. Varlık henüz veritabanına kaydedilmemişse (yani eklendikçe izlenir) o zaman bağlamdan kaldırılır ve *SaveChanges* çağrıldığında artık eklenmez.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Tek bir SaveChanges'ta Birden Çok İşlem

Birden çok Ekle/Güncelleştir/Kaldır işlemlerini *SaveChanges*için tek bir çağrıda birleştirebilirsiniz.

> [!NOTE]  
> Çoğu veritabanı sağlayıcısı için *SaveChanges* işlemseldir. Bu, tüm işlemlerin başarılı olacağı veya başarısız olacağı ve işlemlerin hiçbir zaman kısmen uygulanamayacağı anlamına gelir.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
