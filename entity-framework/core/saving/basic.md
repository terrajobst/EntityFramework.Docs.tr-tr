---
title: Temel kaydetme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 6f72458504a9dbe99038af7cfd23b6991258f6b8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197782"
---
# <a name="basic-save"></a>Temel Kaydetme

Bağlam ve varlık sınıflarınızı kullanarak verileri ekleme, değiştirme ve kaldırma hakkında bilgi edinin.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) GitHub üzerinde.

## <a name="adding-data"></a>Veri ekleme

Varlık sınıflarınızın yeni örneklerini eklemek için *Dbset. Add* metodunu kullanın. *SaveChanges*' i çağırdığınızda veriler veritabanına eklenecektir.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Add, Attach ve Update yöntemleri, [Ilişkili veri](related-data.md) bölümünde açıklandığı gibi, bunlara geçirilen varlıkların tam grafiğinde çalışır. Alternatif olarak, EntityEntry. State özelliği yalnızca tek bir varlığın durumunu ayarlamak için kullanılabilir. Örneğin, uygulamasında yönetilen Hyper-V konakları olarak eklemek için aşağıdaki yordamı kullanabilirsiniz.

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
