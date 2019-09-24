---
title: EF6’dan EF Core’a taşıma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 42e40ce769a67a987883027e1807ec7eaeb4ad7a
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198032"
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

EF6 ' de, `DbSet.Add()` bir varlığa çağırmak, gezinti özelliklerinde başvurulan tüm varlıkların özyinelemeli aramasına neden olur. Bulunan ve bağlam tarafından henüz izlenmeyen varlıklar de eklenmiş olarak işaretlenir. `DbSet.Attach()`tüm varlıkların değiştirilmemiş olarak işaretlenmesi dışında, aynı şekilde davranır.

**EF Core benzer özyinelemeli arama gerçekleştirir, ancak biraz farklı kurallara sahiptir.**

*  Kök varlık her zaman istenen durumda (için eklendi `DbSet.Add` ve `DbSet.Attach`değiştirilmez).

*  **Gezinti özelliklerinin özyinelemeli araması sırasında bulunan varlıklar için:**

    *  **Varlığın birincil anahtarı mağaza oluşturulduysa**

        * Birincil anahtar bir değere ayarlanmamışsa, durum eklendi olarak ayarlanır. Özellik türü için clr varsayılan değeri atanırsa, birincil anahtar değeri "ayarlanmadı" `0` olarak değerlendirilir (örneğin `int` `null` `string`, için, vb.).

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

* Başlatma yapılmaz. Geçişleri uygulamak için `DbContext.Database.Migrate()` kullanmanız gerekir ( `EnsureDeleted()` veya `DbContext.Database.EnsureCreated()` , geçişleri kullanmadan veritabanını oluşturmak/silmek için).

### <a name="code-first-table-naming-convention"></a>Code First tablo adlandırma kuralı

EF6, varlığın eşlendiği varsayılan tablo adını hesaplamak için bir çoğullaştırma hizmeti aracılığıyla varlık sınıfı adını çalıştırır.

EF Core, varlığın türetilmiş bağlamda kullanıma `DbSet` sunulmuş olduğu özelliğin adını kullanır. Varlığın bir `DbSet` özelliği yoksa, sınıf adı kullanılır.
