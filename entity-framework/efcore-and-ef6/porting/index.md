---
title: EF6 'den EF Core-EF 'e taşıma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 77096b9bffba6b8c2a3d7bfb0c2e41e2d170a7db
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182085"
---
# <a name="porting-from-ef6-to-ef-core"></a>EF6’dan EF Core’a taşıma

EF Core temel değişiklikler nedeniyle, değişikliği yapmak için etkileyici bir nedeniniz olmadığı takdirde, bir EF6 uygulamasını EF Core taşımaya çalışmak önerilmez.
EF6 ' dan EF Core bir yükseltme yerine bir bağlantı noktası olarak ilerletirsiniz.

> [!IMPORTANT]
> Taşıma işlemine başlamadan önce, EF Core uygulamanızın veri erişim gereksinimlerini karşıladığını doğrulamak önemlidir.

## <a name="missing-features"></a>Eksik Özellikler

EF Core uygulamanızda kullanmak için gereken tüm özelliklere sahip olduğundan emin olun. EF Core özellik kümesinin EF6 ile nasıl Karşılaştırıldığı hakkında ayrıntılı bir karşılaştırma için bkz. [Özellik Karşılaştırması](xref:efcore-and-ef6/index) . Gerekli özelliklerden herhangi biri eksikse, EF Core 'e geçmeden önce bu özelliklerin eksik olup olmadığını dengelemediğinizden emin olun.

## <a name="behavior-changes"></a>Davranış değişiklikleri

Bu, EF6 ve EF Core arasındaki davranıştaki bazı değişikliklerin ayrıntılı olmayan bir listesidir. Uygulamanızın davranış şeklini değiştirebilecekleri, ancak EF Core takas edildikten sonra derleme hatası olarak göstermeyecek olan bağlantı noktası olarak bunları aklınızda bulundurmanız önemlidir.

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet. Add/Attach ve Graph davranışı

EF6 ' de, bir varlık üzerinde `DbSet.Add()` çağırmak, gezinti özelliklerinde başvurulan tüm varlıkların özyinelemeli aramasına neden olur. Bulunan ve bağlam tarafından henüz izlenmeyen varlıklar de eklenmiş olarak işaretlenir. `DbSet.Attach()`, tüm varlıkların değiştirilmemiş olarak işaretlenmediği sürece aynı şekilde davranır.

**EF Core benzer özyinelemeli arama gerçekleştirir, ancak biraz farklı kurallara sahiptir.**

*  Kök varlık her zaman istenen durumda (`DbSet.Add` için eklenmiştir ve `DbSet.Attach`için değiştirilmez).

*  **Gezinti özelliklerinin özyinelemeli araması sırasında bulunan varlıklar için:**

    *  **Varlığın birincil anahtarı mağaza oluşturulduysa**

        * Birincil anahtar bir değere ayarlanmamışsa, durum eklendi olarak ayarlanır. Özellik türü için CLR varsayılan değeri atanmışsa, birincil anahtar değeri "ayarlanmadı" olarak değerlendirilir (örneğin, `int`için `0`, `string`için `null` vb.).

        * Birincil anahtar bir değere ayarlanmışsa durum Unchanged olarak ayarlanır.

    *  Birincil anahtar veritabanı oluşturulmadığından, varlık kökle aynı duruma konur.

### <a name="code-first-database-initialization"></a>Code First veritabanı başlatma

**EF6, veritabanı bağlantısını seçip veritabanını başlatırken önemli miktarda Magic 'e sahiptir. Bu kurallardan bazıları şunlardır:**

* Hiçbir yapılandırma gerçekleştirildiyse, EF6 SQL Express veya LocalDb üzerinde bir veritabanı seçer.

* Bağlamla aynı ada sahip bir bağlantı dizesi uygulamalar `App/Web.config` dosyasında ise, bu bağlantı kullanılacaktır.

* Veritabanı yoksa, oluşturulur.

* Modeldeki tablolardan hiçbiri veritabanında yoksa, geçerli modelin şeması veritabanına eklenir. Geçişler etkinse, veritabanını oluşturmak için kullanılır.

* Veritabanı varsa ve EF6 daha önce şemayı oluşturduysanız, şema geçerli modelle uyumluluk için denetlenir. Şema oluşturulduktan sonra model değiştiyse bir özel durum oluşturulur.

**EF Core Bu Magic 'in hiçbirini gerçekleştirmez.**

* Veritabanı bağlantısı kodda açıkça yapılandırılmalıdır.

* Başlatma yapılmaz. Geçişleri uygulamak için `DbContext.Database.Migrate()` kullanmanız gerekir (veya `DbContext.Database.EnsureCreated()` ve `EnsureDeleted()` geçişleri kullanmadan veritabanını oluşturmak/silmek için).

### <a name="code-first-table-naming-convention"></a>Code First tablo adlandırma kuralı

EF6, varlığın eşlendiği varsayılan tablo adını hesaplamak için bir çoğullaştırma hizmeti aracılığıyla varlık sınıfı adını çalıştırır.

EF Core, türetilen bağlamda varlığın içinde kullanıma sunulmuş `DbSet` özelliğinin adını kullanır. Varlığın bir `DbSet` özelliği yoksa, sınıf adı kullanılır.
