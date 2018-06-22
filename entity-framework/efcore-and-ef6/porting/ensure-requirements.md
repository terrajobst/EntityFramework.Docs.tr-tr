---
title: EF çekirdek - EF6 bağlantı noktası oluşturma gereksinimlerini doğrulayın
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054255"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>EF çekirdek EF6 bağlantı noktası oluşturma önce: uygulamanızın gereksinimlerini doğrulayın

Taşıma işlemine başlamadan önce EF çekirdek uygulamanız için veri erişim gereksinimlerini karşıladığını doğrulamak önemlidir.

## <a name="missing-features"></a>Eksik Özellikler

EF çekirdek, uygulamanızda kullanmak için gereken tüm özellikler sahip olduğundan emin olun. Bkz: [özellik karşılaştırması](../features.md) nasıl EF6 için özellik EF çekirdek kümesini karşılaştırır, ayrıntılı bir karşılaştırma için. Tüm gerekli özellikleri eksikse, bu özellikler olmaması için EF çekirdek taşıma önce dengeleyebilirsiniz emin olun.

## <a name="behavior-changes"></a>Davranış değişiklikleri

Bu davranış EF6 EF çekirdek arasındaki bazı değişiklikler kapsamlı olmayan bir listesidir. Bunlar uygulamanızı davranır, ancak derleme hataları takas sonra EF çekirdek gösterilmez şekilde değişiklik gösterebileceği için bağlantı noktası olarak uygulamanız bu aklınızda tutmak önemlidir.

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet.Add/Attach ve grafik davranışı

EF6 içinde arama `DbSet.Add()` Gezinti özelliklerini başvurulan tüm varlıklar için yinelemeli bir aramasını varlık sonuçlarında üzerinde. Bulunan ve bağlam tarafından izlenmez herhangi bir varlık da olduğu eklenmiş olarak işaretlenmelidir. `DbSet.Attach()`Tüm varlıklar işaretlenmiş dışında aynı şekilde davranır olarak değişmez.

**EF çekirdek benzer bir özyinelemeli arama gerçekleştirir, ancak bazı biraz farklı kuralları ile.**

*  Kök varlık her zaman istenen durumda (için eklenen `DbSet.Add` ve için değişmemiştir `DbSet.Attach`).

*  **Gezinti özellikleri özyinelemeli arama sırasında bulunan varlıklar için:**

    *  **Varlığın birincil anahtarı oluşturulan depo ise**

        * Birincil anahtar değerine ayarlı değilse durumu için eklenen ayarlanır. Özellik türü için CLR varsayılan değer atanmışsa birincil anahtar değerine "ayarlı değil" olarak kabul edilir (yani `0` için `int`, `null` için `string`, vb..).

        * Birincil anahtar değerine ayarlı ise, durumu değişmeden ayarlanır.

    *  Birincil anahtar oluşturulan veritabanı değilse, varlık kök ile aynı duruma getirilir.

### <a name="code-first-database-initialization"></a>İlk veritabanı başlatma kod

**EF6 veritabanı bağlantısını seçerek ve veritabanı başlatılırken geçici gerçekleştirir Sihirli önemli miktarda sahiptir. Bu kurallar şunlardır:**

* Hiçbir yapılandırma gerçekleştirilirse, EF6 SQL Express veya yerel veritabanı bir veritabanı seçin.

* Bağlam aynı ada sahip bir bağlantı dizesi uygulamalarda ise `App/Web.config` dosyası, bu bağlantısı kullanılır.

* Veritabanı mevcut değilse oluşturulur.

* Model tablolardan hiçbiri veritabanında yoksa, geçerli model için şema veritabanına eklenir. Geçişler etkinleştirilirse, ardından bunlar veritabanı oluşturmak için kullanılır.

* Veritabanı varsa ve EF6 daha önce şema oluşturduysanız, geçerli model ile uyumluluk için şema denetlenir. Model şeması oluşturulmasından bu yana değiştiyse özel durum oluşur.

**EF çekirdek bu Sihirli gerçekleştirmez.**

* Veritabanı bağlantısı kodda açıkça yapılandırılmalıdır.

* Hiçbir başlatma gerçekleştirilir. Kullanmalısınız `DbContext.Database.Migrate()` geçişleri uygulamak için (veya `DbContext.Database.EnsureCreated()` ve `EnsureDeleted()` oluşturma / veritabanı geçişler kullanmadan silme için).

### <a name="code-first-table-naming-convention"></a>Kod ilk tablonun adlandırma

EF6 varlık sınıfı adı varlık eşlenmiş varsayılan tablo adı hesaplamak için çoğullaştırma hizmeti ile çalışır.

EF çekirdek adını kullanan `DbSet` varlık de türetilmiş bağlamda sağlanmaktadır özelliği. Varlık yoksa bir `DbSet` özelliği, ardından sınıf adı kullanılır.
