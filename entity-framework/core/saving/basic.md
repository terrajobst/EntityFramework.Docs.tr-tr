---
title: Temel kaydetme - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: ecf8f344a5baae37a5e7255a4affb1085f1b3ff3
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949404"
---
# <a name="basic-save"></a>Temel kaydetme

Eklemek, değiştirmek ve bağlam ve varlık sınıfları kullanarak verileri kaldırma hakkında bilgi edinin.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) GitHub üzerinde.

## <a name="adding-data"></a>Veri ekleme

Kullanım *DbSet.Add* yeni örnekleri, varlık sınıfları eklemek için yöntemi. Çağırdığınızda veriler veritabanında eklenir *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Varlıkların tam grafikte tüm iş Ekle, ekleme ve güncelleştirme yöntemlerini geçirilen, açıklandığı [ilgili verileri](related-data.md) bölümü. Alternatif olarak, EntityEntry.State özelliği yalnızca tek bir varlık durumunu ayarlamak için kullanılabilir. Örneğin, `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Verileri güncelleştirme

EF bağlam tarafından izlenen var olan bir varlığa yapılan değişiklikleri otomatik olarak algılar. Bu, yük/sorgu veritabanından varlıklar ve daha önce eklediğiniz ve veritabanına kaydedilen varlıkları içerir.

Yalnızca özelliklerine atanan değerleri değiştirin ve sonra çağrı *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Verileri silme

Kullanım *DbSet.Remove* , varlık sınıflarının örneklerini silmek için yöntemi.

Varlık veritabanında zaten varsa sırasında silinecek *SaveChanges*. Varlık henüz veritabanına kaydedilmedi varsa (diğer bir deyişle, eklenen izlenen) bağlamdan kaldırılacak ve artık ne zaman eklenen *SaveChanges* çağrılır.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Tek bir SaveChanges içinde birden çok işlem

Tek bir çağrı ile birden fazla ekleme/güncelleştirme/kaldırma işlemi birleştirebilirsiniz *SaveChanges*.

> [!NOTE]  
> Çoğu veritabanı sağlayıcısı için *SaveChanges* işlem. Başka bir deyişle, tüm işlemleri başarılı başarısız veya ve işlemleri hiçbir zaman sola kısmen uygulanır.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
