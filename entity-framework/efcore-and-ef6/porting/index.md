---
title: EF6'dan EF Core'a taşıma - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 77096b9bffba6b8c2a3d7bfb0c2e41e2d170a7db
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416952"
---
# <a name="porting-from-ef6-to-ef-core"></a>EF6’dan EF Core’a taşıma

EF Core'daki temel değişiklikler nedeniyle, değişikliği yapmak için zorlayıcı bir nedeniniz yoksa bir EF6 uygulamasını EF Core'a taşımayı denemenizi önermiyoruz.
EF6'dan EF Core'a geçişi yükseltme yerine bir bağlantı noktası olarak görüntülemeniz gerekir.

> [!IMPORTANT]
> Taşıma işlemine başlamadan önce, EF Core'un uygulamanız için veri erişim gereksinimlerini karşıladığını doğrulamak önemlidir.

## <a name="missing-features"></a>Eksik özellikler

EF Core'un uygulamanızda kullanmanız gereken tüm özelliklere sahip olduğundan emin olun. EF Core'da ayarlanan özelliğin EF6 ile karşılaştırıldığında nasıl olduğunu ayrıntılı bir karşılaştırma için [Özellik Karşılaştırması'na](xref:efcore-and-ef6/index) bakın. Gerekli özellikler yoksa, EF Core'a geçmeden önce bu özelliklerin eksikliğini telafi edebileceğinden emin olun.

## <a name="behavior-changes"></a>Davranış değişiklikleri

Bu, EF6 ve EF Core arasındaki davranış değişikliklerinin kapsamlı olmayan bir listesidir. Uygulamanızın nasıl bir hareket olduğunu değiştirebileceğinden, ancak EF Core'a geçtikten sonra derleme hatası olarak gösterilmeyeceğine dikkat etmek önemlidir.

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet.Ekle/Ekle ve grafik davranışı

EF6'da, `DbSet.Add()` bir varlığı çağırmak, gezinti özelliklerinde başvurulan tüm varlıklar için özyinelemeli bir arama yla sonuçlanır. Bulunan ve bağlam tarafından zaten izlenmeyen tüm varlıklar da eklenmiş olarak işaretlenir. `DbSet.Attach()`tüm varlıklar değişmeden işaretlenmiş olması dışında aynı şekilde kalır.

**EF Core benzer özyinelemeli arama yapar, ancak bazı biraz farklı kurallar ile.**

*  Kök varlık her zaman istenen durumdadır `DbSet.Add` (için `DbSet.Attach`eklenen ve değiştirilmemiş).

*  **Gezinti özelliklerinin özyinelemeli araması sırasında bulunan varlıklar için:**

    *  **Varlığın birincil anahtarı depo oluşturulursa**

        * Birincil anahtar bir değere ayarlanmazsa, durum eklenecek şekilde ayarlanır. Özellik türü için CLR varsayılan değeri atanırsa birincil anahtar değeri "ayarlanmaz" `null` `string`olarak kabul edilir (örneğin, `0` için, `int`vb.).

        * Birincil anahtar bir değere ayarlanmışsa, durum değişmeden ayarlanır.

    *  Birincil anahtar veritabanı oluşturulmazsa, varlık kökle aynı duruma sokulur.

### <a name="code-first-database-initialization"></a>Kod İlk veritabanı başlatma

**EF6, veritabanı bağlantısını seçme ve veritabanını başlatma etrafında gerçekleştirdiği önemli miktarda büyüye sahiptir. Bu kurallardan bazıları şunlardır:**

* Yapılandırma yapılmazsa, EF6 SQL Express veya LocalDb'da bir veritabanı seçer.

* Uygulamalar `App/Web.config` dosyasında bağlamla aynı ada sahip bir bağlantı dizesi varsa, bu bağlantı kullanılır.

* Veritabanı yoksa, oluşturulur.

* Modeldeki tablolardan hiçbiri veritabanında yoksa, geçerli modelin şeması veritabanına eklenir. Geçişler etkinse, veritabanını oluşturmak için kullanılır.

* Veritabanı varsa ve EF6 şemayı daha önce oluşturmuşsa, şema geçerli modelle uyumluluk için denetlenir. Şema oluşturulduğundan beri model değiştiyse bir özel durum atılır.

**EF Core bu sihri gerçekleştirmez.**

* Veritabanı bağlantısı açıkça kod olarak yapılandırılmalıdır.

* Başlatma işlemi yapılmaz. Geçişleri `DbContext.Database.Migrate()` uygulamak (veya `DbContext.Database.EnsureCreated()` geçişleri `EnsureDeleted()` kullanmadan veritabanını oluşturmak/silmek) için kullanmanız gerekir.

### <a name="code-first-table-naming-convention"></a>Kod İlk tablo adlandırma kuralı

EF6, varlığın eşlenen varsayılan tablo adını hesaplamak için varlık sınıf adını çoğullaştırma hizmeti aracılığıyla çalıştırAr.

EF Core, varlığın `DbSet` türetilmiş bağlamda maruz kalandığı özelliğin adını kullanır. Varlığın bir `DbSet` özelliği yoksa, sınıf adı kullanılır.
