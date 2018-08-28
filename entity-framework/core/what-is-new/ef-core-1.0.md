---
title: EF Core 1.0 - EF Core yenilikleri
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 409e16d762bca3ecd083ea191ad7b42aa0a6a275
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996857"
---
# <a name="features-included-in-ef-core-10"></a>EF Core 1.0 dahil olan özellikler

## <a name="platforms"></a>Platformlar
### <a name="net-framework-451"></a>.NET Framework 4.5.1
Konsolu, WPF, WinForms, ASP.NET 4, vb. içerir.
### <a name="net-standard-13"></a>.NET standard 1.3
Hem .NET Framework ve .NET Core Windows, OSX ve Linux'ta hedefleyen ASP.NET Core dahil.

## <a name="modelling"></a>Modelleme
### <a name="basic-modelling"></a>Temel modelleme
POCO varlık alma/ayarlama özelliklerini genel skaler türleri ile temel (`int`, `string`vb..).
### <a name="relationships-and-navigation-properties"></a>İlişkileri ve gezinti özellikleri
Bire çok ve bir sıfır-veya-bire ilişkileri bir yabancı anahtar dayalı modeli belirtilebilir. Gezinti özellikleri basit koleksiyonu veya başvuru türleri bu ilişkileri ile ilişkilendirilebilir.
### <a name="built-in-conventions"></a>Yerleşik kuralları
Bu varlık sınıflarının şekli üzerinde bir ilk modeline oluşturun.
### <a name="fluent-api"></a>Fluent API'si
Geçersiz kılmanıza da olanak tanır `OnModelCreating` Bağlamınızı daha fazla kuralı tarafından bulunan modeli yapılandırmak için yöntemi.
### <a name="data-annotations"></a>Veri açıklamaları
Varlık sınıfları/özelliklerinizi eklenebilir ve EF modeli etkiler öznitelikleridir. Örneğin, ekleme `[Required]` EF bir özelliğin gerekli olduğunu bilmesini sağlayacaktır.
### <a name="relational-table-mapping"></a>İlişkisel Tablo eşleme
Tabloları/sütunları eşlenmesi varlıklar sağlar.
### <a name="key-value-generation"></a>Anahtar değeri oluşturma
İstemci tarafında oluşturma ve veritabanı oluşturma da dahil olmak üzere.
### <a name="database-generated-values"></a>Veritabanı üretilmiş değerler
Veritabanı ekleme (varsayılan değerler) veya güncelleştirme (hesaplanan sütun) tarafından oluşturulan değerleri sağlar.
### <a name="sequences-in-sql-server"></a>SQL Server'da dizileri
Modelde tanımlanması nesneleri dizisi sağlar.
### <a name="unique-constraints"></a>Benzersiz kısıtlamalar
Alternatif anahtarlar ve ilişkileri tanımlama yeteneği tanımı için bu anahtara hedefleyen sağlar.
### <a name="indexes"></a>Dizinleri
Modelde, otomatik olarak dizinler tanımlama veritabanında tanıtır. Benzersiz dizinler de desteklenir.
### <a name="shadow-state-properties"></a>Gölge durumu özellikleri
Bildirilmemiş .NET sınıfında depolanmaz, ancak izlenen ve EF Core tarafından güncelleştirilmiş modelde tanımlanması özellikleri sağlar. Bu nesne gösterme istenmediğinde yabancı anahtar özelliklerini yaygın olarak kullanılır.
### <a name="table-per-hierarchy-inheritance-pattern"></a>Tablo başına hiyerarşi devralma deseni
Bunlar veritabanında belirli bir kaydı için varlık türü tanımlamak için bir ayrıştırıcı sütunu kullanarak tek bir tabloyu kaydedilecek Devralma Hiyerarşisi varlıklar sağlar.
### <a name="model-validation"></a>Model doğrulama
Algılar. geçersiz modelde desenleri ve kullanışlı hata iletileri sağlar.

## <a name="change-tracking"></a>Change tracking
### <a name="snapshot-change-tracking"></a>Anlık görüntü değişiklik izleme
Geçerli durumunu özgün durumuna karşı bir kopyasını (snapshot) karşılaştırarak otomatik olarak algılanamayacak kadar varlıklarda değişiklikler sağlar.
### <a name="notification-change-tracking"></a>Bildirim değişiklik izleme
Özellik değerleri değiştirildiğinde değişiklik İzleyici bildirmek varlıklarınızı sağlar.
### <a name="accessing-tracked-state"></a>İzlenen durumuna erişme
Aracılığıyla `DbContext.Entry` ve `DbContext.ChangeTracker`.
### <a name="attaching-detached-entitiesgraphs"></a>Ayrılmış varlıklar/grafik ekleme
Yeni `DbContext.AttachGraph` API yardımcı olan bir bağlam varlıklara yeni/değiştirilmiş varlıklar kaydetmek için yeniden ekler.

## <a name="saving-data"></a>Verileri kaydetme
### <a name="basic-save-functionality"></a>Temel işlevleri kaydetme
Değişiklikler veritabanına kalıcı için varlık örneklerini sağlar.
### <a name="optimistic-concurrency"></a>İyimser eşzamanlılık
Verileri veritabanından getirildikten sonra başka bir kullanıcı tarafından yapılan değişikliklerin üzerine karşı korur.
### <a name="async-savechanges"></a>Zaman uyumsuz SaveChanges
Veritabanı tarafından verilen komutları işlerken diğer istekleri işlemek için geçerli iş parçacığı boşaltmak `SaveChanges`.
### <a name="database-transactions"></a>Veritabanı işlemleri
Anlamına `SaveChanges` her zaman (ya da tamamen başarılı ya da değişiklik veritabanına yapılan anlamına gelir) atomik olduğu. Ayrıca vardır işlem ilgili API'ler, işlemler vb. bağlam örnekleri arasında paylaşılmasına izin verecek şekilde.
### <a name="relational-batching-of-statements"></a>İlişkisel: deyimleri toplu işleme
Veritabanı tek bir gidiş dönüş içinde birden çok ekleme/güncelleştirme/silme komutlarını yığınlama daha iyi performans sağlar.

## <a name="query"></a>Sorgu
### <a name="basic-linq-support"></a>Temel LINQ desteği
Veritabanından veri almak için LINQ kullanma olanağı sağlar.
### <a name="mixed-clientserver-evaluation"></a>Karma istemci/sunucu değerlendirme
Veritabanında değerlendirilemiyor ve bu nedenle veri belleğe alındıktan sonra değerlendirilmelidir mantığı içerecek şekilde sorguları mümkün kılar.
### <a name="notracking"></a>NoTracking
Sorguları daha hızlı sorgu yürütme bağlamı (Bu sonuçları salt okunur yararlıdır) varlık örneklerini değişiklikleri izleyin gerekmez, sağlar.
### <a name="eager-loading"></a>İstekli yükleme
Sağlar `Include` ve `ThenInclude` ayrıca sorgulanırken getirileceği ilgili verileri belirlemek için yöntemleri.
### <a name="async-query"></a>Zaman uyumsuz sorgu
Veritabanı sırasında diğer istekleri işlemek için geçerli iş parçacığı (ve onun ilişkili kaynakları) ücretsiz işler sorgu.
### <a name="raw-sql-queries"></a>Ham SQL sorguları
Sağlar `DbSet.FromSql` yöntemi ham SQL sorguları verileri getirmek için. Bu sorgular, LINQ kullanarak da oluşabilir.

## <a name="database-schema-management"></a>Veritabanı Şema Yönetimi       
### <a name="database-creationdeletion-apis"></a>Veritabanı oluşturma/silme API'leri
Çoğunlukla, hızlı bir şekilde oluşturma / veritabanı geçişleri kullanmadan silme istediğiniz test etmek için tasarlanmıştır.
### <a name="relational-database-migrations"></a>İlişkisel veritabanı geçişlerini
İlişkisel veritabanı şeması modeli değişikliklerinizi olarak mesai gelişmesi sağlar.
### <a name="reverse-engineer-from-database"></a>Veritabanından ters mühendislik
İskelesini kurar EF modeli varolan bir ilişkisel veritabanı şemasını temel alan.

## <a name="database-providers"></a>Veritabanı sağlayıcıları
### <a name="sql-server"></a>SQL Server
Microsoft SQL Server 2008 ve sonraki sürümlerde bağlanır.
### <a name="sqlite"></a>SQLite
SQLite 3 veritabanına bağlanır.
### <a name="in-memory"></a>Bellek içi
Bir veritabanına bağlanmadan test kolayca sağlamak üzere tasarlanmıştır.
### <a name="3rd-party-providers"></a>3. taraf sağlayıcılar
Çeşitli sağlayıcılar diğer veritabanı altyapıları için kullanılabilir. Bkz: [veritabanı sağlayıcıları](../providers/index.md) tam listesi için.
