---
title: Nasıl iş - EF Core sorgular
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/overview
ms.openlocfilehash: 23d26f9c0ac17fc0df744f5339946947ea366911
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415737"
---
# <a name="how-queries-work"></a>Sorguları nasıl çalışır

Entity Framework Core dil tümleşik sorgu (LINQ) veritabanından verileri sorgulamak için kullanır. LINQ C# (veya tercih ettiğiniz .NET dilinizi) türetilen bağlamını ve varlık sınıflarında temel alan türü kesin belirlenmiş sorgular yazmak için kullanmanıza olanak tanır.

## <a name="the-life-of-a-query"></a>Bir sorgu ömrü

Her sorgu geçtiği işlemine yüksek düzeyli bir genel bakış aşağıda verilmiştir.

1. LINQ Sorgu sağlayıcısı tarafından işlenmek üzere hazır olan bir gösterim oluşturmak için Entity Framework Core tarafından işlenen
   1. Sonuç önbelleğe alınan bu işleme, sorgunun her çalıştırılışında yapılması gerekmez.
2. Sonuç veritabanı sağlayıcıya geçirilir.
   1. Veritabanı sağlayıcısı veritabanında sorgu hangi parçalarının değerlendirilebilir tanımlar.
   2. Bu sorgunun bölümlerini veritabanı belirli bir sorgu dili (örneğin, ilişkisel bir veritabanı için SQL) için bunların çevirisi
   3. Bir veya daha fazla sorgu için veritabanı ve sonuç kümesini döndürülen gönderilir (sonucu olan varlık örneklerini veritabanından alınan değerlerin)
3. Her öğe için sonuç kümesinde
   1. Bir izleme sorguya varsa EF veri değişikliği İzleyicisi bağlam örneği için zaten bir varlığı temsil edip etmediğini denetler
      * Bu durumda, var olan bir varlığa döndürülür
      * Aksi halde yeni bir varlık oluşturulur, değişiklik izleme kurulduğundan ve yeni bir varlık döndürdü
   2. Hayır-izleme sorgu varsa EF veriyi bir varlık zaten bu sorgu için sonuç temsil edip etmediğini denetler
      * Bu nedenle, var olan bir varlığa döndürülürse <sup>(1)</sup>
      * Aksi halde yeni bir varlık oluşturulur ve döndürülen

<sup>(1) </sup> Zaten döndürülen varlıkları izlemek için zayıf başvurular hiçbir izleme sorguları kullanın. Aynı kimliğe sahip bir önceki sonucu kapsam dışına gider ve atık toplama devam, yeni bir varlık örneği alabilirsiniz.

## <a name="when-queries-are-executed"></a>Sorguları ne zaman çalıştırılır

LINQ işleçleri çağırdığınızda, bir sorgunun bellek içi temsili yalnızca oluşturuyorsunuz. Sonuçları tüketilen olduğunda sorguyu yalnızca veritabanına gönderilir.

Veritabanına gönderilen sorgu sonucunda en yaygın işlemler şunlardır:
* Yineleme sonuçları bir `for` döngü
* Bir işleç gibi kullanarak `ToList`, `ToArray`, `Single`, `Count`
* Veri bağlama için bir kullanıcı Arabirimi bir sorgunun sonuçlarını

> [!WARNING]  
> **Her zaman kullanıcı girişini doğrulama:** sırada EF Core parametreleri kullanarak SQL ekleme saldırılarına karşı korur ve sorgularda değişmez değerleri kaçış, girişleri doğrulamaz. Güvenilir olmayan kaynaklardan gelen değerleri LINQ sorgularında kullanılan, varlık özelliklerine atanan veya geçirilen diğer EF Core API'leri için önce başına uygulama gereksinimlerine uygun doğrulama gerçekleştirilmesi gerekir. Bu sorguları dinamik olarak oluşturmak için kullanılan herhangi bir kullanıcı girişi içerir. İfadeleri oluşturmak için kullanıcı girişi kabul ediyorsanız, LINQ kullanırken bile, yalnızca hedeflenen ifadeleri oluşturulabilir emin olmanız gerekir.
