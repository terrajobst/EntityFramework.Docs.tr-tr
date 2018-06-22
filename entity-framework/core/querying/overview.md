---
title: İş - EF çekirdek nasıl sorgular
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054258"
---
# <a name="how-queries-work"></a>Sorguları nasıl çalışır

Entity Framework Çekirdek dil tümleştirmek sorgu (LINQ) veritabanından verileri sorgulamak kullanır. LINQ, C# (veya .NET dilinizi tercih) türetilen bağlamını ve varlık sınıfları esas alarak kesin türü belirtilmiş sorguları yazmaya kullanmanıza olanak sağlar.

## <a name="the-life-of-a-query"></a>Bir sorgu ömrü

Her sorgu geçtiği işlemi üst düzey bir genel bakış verilmiştir.

1. LINQ Sorgu sağlayıcısı tarafından işlenmek üzere hazır bir temsilini oluşturmak için Entity Framework Çekirdek tarafından işlenir
   1. Sonuç önbelleğe alınan bu işlem, sorgu her çalıştırılışında yapılması gerekmez.
2. Sonuç veritabanı sağlayıcıya geçirilir
   1. Veritabanı sağlayıcısı sorgu hangi kısımlarının veritabanında değerlendirilebilir tanımlar
   2. Bu sorgunun bölümlerini veritabanı belirli sorgu dili (örneğin bir ilişkisel veritabanı için SQL) için çevrilir
   3. Bir veya daha fazla sorgular ve veritabanı döndürülen sonuç gönderilir (sonucu olan değerleri veritabanından varlık örnekleri)
3. Her öğe için sonuç kümesinde
   1. Bu izleme sorgu ise, veri bağlamı örneği için değişiklik İzleyicisi'ni zaten bir varlığı temsil edip etmediğini EF denetler
      * Bu durumda, var olan varlık döndürülür
      * Aksi durumda, yeni bir varlık oluşturulur, değişiklik izleme kurulması ve yeni bir varlık döndürdü
   2. Hayır-izleme sorgu varsa EF verileri zaten bu sorgu için sonuç bir varlığı temsil edip etmediğini denetler
      * Bu nedenle, varolan varlık döndürülürse <sup>(1)</sup>
      * Aksi halde yeni bir varlık oluşturulur ve döndürdü

<sup>(1) </sup> Zaten döndürülen varlıkları izlemek için zayıf başvurular hiçbir izleme sorguları kullanın. Aynı kimliğe sahip bir önceki sonuç kapsamının dışına gider ve atık toplama devam, yeni bir varlık örneği elde edebilirsiniz.

## <a name="when-queries-are-executed"></a>Sorguları ne zaman çalıştırılır

LINQ işleçleri çağırdığınızda, sorgu bir bellek içi temsili yalnızca oluşturmakta olduğunuz. Sorgu veritabanına sonuçları tüketilen yalnızca gönderilir.

Veritabanına gönderilen sorgu sonucunda en yaygın işlemleri şunlardır:
* Sonuçlarda yineleme bir `for` döngü
* Bir işleç gibi kullanılarak `ToList`, `ToArray`, `Single`, `Count`
* Veri bağlama bir kullanıcı Arabirimi için bir sorgu sonuçları

> [!WARNING]  
> **Kullanıcı girişi her zaman doğrulayın:** sırada EF SQL ekleme saldırılarına karşı koruma sağlar, tüm genel doğrulama giriş yapın. LINQ sorgularını, atanmış varlık özellikleri, vb. için kullanılan API'leri, geçirilen değerleri güvenilmeyen kaynak sonra uygulama gereksinimlerinizi başına uygun doğrulama alınması, bu nedenle gerçekleştirilmelidir. Bu, dinamik sorgular oluşturmak için kullanılan herhangi bir kullanıcı giriş içerir. Yalnızca hedeflenen ifadeleri emin olmanız gerekir ifadeleri oluşturmak için oluşturulan kullanıcı girişi kabul ediyorsunuz bile LINQ, kullanırken.
