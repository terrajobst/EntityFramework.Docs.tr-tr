---
title: EF6'dan EF Core'a - taşıma gereksinimleri doğrulama
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 65bdc8bb9574d37db697aa47c8e8c480cefcb4f7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949120"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>EF6'dan EF Core'a taşıma önce: uygulamanızın gereksinimlerini doğrulayın

Taşıma işlemine başlamadan önce EF Core uygulamanız için veri erişim gereksinimleri karşıladığını doğrulamak önemlidir.

## <a name="missing-features"></a>Eksik özellikleri

EF Core uygulamanızda kullanmak için ihtiyacınız olan tüm özellikleri sahip olduğundan emin olun. Bkz: [özellik karşılaştırması](../features.md) nasıl EF6 için özellik EF Core kümesini karşılaştırır, ayrıntılı bir karşılaştırması için. Tüm gerekli özellikleri yoksa, bu özelliklerin olmaması için önce EF Core'a taşıma dengeleyebilir emin olun.

## <a name="behavior-changes"></a>Davranış değişiklikleri

Kapsamlı olmayan bazı değişiklikler EF6 ve EF Core arasındaki davranış listesini budur. Bunlar uygulamanızı davranır, ancak derleme değiştirdikten sonra EF Core gösterilmez şekilde değişiklik gösterebileceği için bağlantı noktası olarak uygulamanız bu aklınızda tutmanız önemlidir.

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet.Add/Attach ve grafik davranışı

EF6 içinde arama `DbSet.Add()` yinelemeli arama Gezinti özelliklerini başvurulan tüm varlıklar için bir varlık sonuçları üzerinde. Aynı zamanda bulunur ve bağlam tarafından izlenmez herhangi bir varlık olan eklenen olarak işaretlenmelidir. `DbSet.Attach()` Tüm varlıklar işaretlenmiş dışında aynı şekilde davranır ancak olarak değişmez.

**EF Core benzer bir yinelemeli arama gerçekleştirir ancak bazı biraz farklı kuralları.**

*  Kök varlığın her zaman istenen durumda (eklenen `DbSet.Add` ve için değişmemiştir `DbSet.Attach`).

*  **Gezinti özellikleri yinelemeli arama sırasında bulunan varlıkları için:**

    *  **Varlığın birincil anahtarı oluşturulan depo ise**

        * Birincil anahtar değerine ayarlanmazsa durum için eklenen ayarlanır. Özellik türü için CLR varsayılan değer atanmışsa birincil anahtar değeri "ayarlı değil" olarak kabul edilir (örneğin, `0` için `int`, `null` için `string`vb..).

        * Birincil anahtarı bir değere ayarlarsanız, durumu değişmeden ayarlanır.

    *  Birincil anahtarı oluşturulan veritabanı değil, varlığın kök ile aynı duruma konur.

### <a name="code-first-database-initialization"></a>İlk veritabanı başlatma kodu

**EF6 veritabanı bağlantısını seçerek ve veritabanı başlatma etrafında gerçekleştirir Sihirli önemli miktarda sahiptir. Bu kuralların bazıları şunlardır:**

* Yapılandırma gerçekleştirilirse, bir veritabanında SQL Express ya da LocalDb EF6 seçer.

* İçerik olarak aynı ada sahip bir bağlantı dizesi uygulamalar ise `App/Web.config` dosya, bu bağlantı kullanılacak.

* Veritabanı yoksa, oluşturulur.

* Modelden tablolardan veritabanında varsa, geçerli model için şema veritabanına eklenir. Geçişleri etkinse, sonra bunlar veritabanını oluşturmak için kullanılır.

* Veritabanı varsa ve EF6 şemayı daha önce oluşturmuştunuz şema geçerli modeli ile uyumluluk için denetlenir. Model şeması oluşturulmasından bu yana değişmişse bir özel durum oluşturulur.

**EF Core bu Sihirli gerçekleştirmez.**

* Veritabanı bağlantısı kodda açıkça yapılandırılması gerekir.

* Hiçbir başlatma gerçekleştirilmez. Kullanmalısınız `DbContext.Database.Migrate()` geçişler uygulamak için (veya `DbContext.Database.EnsureCreated()` ve `EnsureDeleted()` oluşturma / veritabanı geçişleri kullanmadan silme için).

### <a name="code-first-table-naming-convention"></a>Kod ilk tabloda adlandırma kuralı

EF6 varlık sınıfı adı varlığa eşlendi varsayılan tablo adı hesaplamak için çoğullaştırma hizmeti aracılığıyla çalıştırılır.

EF Core kullanan adını `DbSet` varlık de türetilmiş bağlamda sağlanmaktadır özelliği. Varlık yoksa bir `DbSet` özelliği ve ardından sınıf adı kullanılır.
