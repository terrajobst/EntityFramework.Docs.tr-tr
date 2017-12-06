---
title: "EF çekirdek 1.0 - EF çekirdek yenilikler nelerdir?"
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: e5b9e57a01ff302b1d7bd0fc5419aa5b8213865e
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="features-included-in-ef-core-10"></a>EF çekirdek 1.0 içinde bulunan özellikler

## <a name="platforms"></a>Platformlar
### <a name="net-framework-451"></a>.NET Framework 4.5.1
Konsolu, WPF, WinForms, ASP.NET 4, vb. içerir.
### <a name="net-standard-13"></a>.NET standart 1.3
.NET Framework ve Windows, OSX ve Linux .NET Core hedefleme dahil olmak üzere ASP.NET Core.

## <a name="modelling"></a>Model oluşturma
### <a name="basic-modelling"></a>Temel modelleme
Genel skaler türleri get/set özellikleriyle POCO varlıklar göre (`int`, `string`vb..).
### <a name="relationships-and-navigation-properties"></a>İlişkileri ve gezinti özellikleri
Bir yabancı anahtar tabanlı modelinde bir çok ve tek sıfır-veya-bire ilişkileri belirtilebilir. Gezinti özellikleri basit koleksiyon veya başvuru türleri bu ilişkileri ile ilişkili olabilir.
### <a name="built-in-conventions"></a>Yerleşik kuralları
Bu varlık sınıflarını şekil üzerinde bir ilk modeline oluşturun.
### <a name="fluent-api"></a>Fluent API'si
Geçersiz kılmanıza olanak tanır `OnModelCreating` yöntemi hakkında daha fazla kural tarafından bulunan modeli yapılandırmak için bağlamı.
### <a name="data-annotations"></a>Veri açıklamaları
Varlık sınıfları/özellikleri ve işlem için eklenen öznitelikler (örn. bir özelliğin gerekli olduğunu bilmeniz EF [gerekli] izin ekleme) EF modeli etkilemek var.
### <a name="relational-table-mapping"></a>İlişkisel Tablo eşleme
Tablolar/sütunlarına eşlenmesi için varlıkları sağlar.
### <a name="key-value-generation"></a>Anahtar değeri oluşturma
İstemci tarafında oluşturma ve veritabanı oluşturma dahil olmak üzere.
### <a name="database-generated-values"></a>Veritabanı üretilmiş değerler
Veritabanında (varsayılan değerler) INSERT veya update (hesaplanan sütunlar) tarafından oluşturulan için değerler sağlar.
### <a name="sequences-in-sql-server"></a>SQL Server'da sıraları
Modelde tanımlanacak dizisi nesneleri sağlar.
### <a name="unique-constraints"></a>Benzersiz kısıtlamalar
Alternatif anahtarları ve ilişkileri tanımlama yeteneği tanımı için bu anahtar hedefleyen sağlar.
### <a name="indexes"></a>Dizinler
Dizinleri otomatik olarak modeldeki tanımlama dizinleri veritabanında tanıtır. Benzersiz dizinler de desteklenir.
### <a name="shadow-state-properties"></a>Gölge durumu özellikleri
Bildirilmemiş ve .NET sınıfında depolanmaz, ancak izlenen ve EF çekirdek tarafından güncelleştirilen modelde tanımlanacak özellikleri sağlar. Bu nesnedeki gösterme istenmediğinde yabancı anahtar özellikleri için yaygın olarak kullanılır.
### <a name="table-per-hierarchy-inheritance-pattern"></a>Tablo başına hiyerarşisi devralma deseni
Bunlar varlık türü veritabanında belirli bir kayıt için tanımlamak için ayrıştırıcı sütunun kullanarak tek bir tabloya kaydedilmesi için bir devralma hiyerarşisinde varlıklar sağlar.
### <a name="model-validation"></a>Model doğrulama
Geçersiz modelde düzenleri ve yararlı hata iletileri sağlar algılar.

## <a name="change-tracking"></a>Change tracking
### <a name="snapshot-change-tracking"></a>Anlık görüntü değişiklik izleme
Geçerli durumunu özgün durumuna karşı bir kopyasını (snapshot) karşılaştırarak otomatik olarak algılanması varlıklardaki değişiklikleri sağlar.
### <a name="notification-change-tracking"></a>Bildirim değişiklik izleme
Özellik değerlerini değiştirildiğinde değişikliği İzleyicisi bildirmek varlıklarınızı sağlar.
### <a name="accessing-tracked-state"></a>İzlenen durum erişme
Aracılığıyla `DbContext.Entry` ve `DbContext.ChangeTracker`.
### <a name="attaching-detached-entitiesgraphs"></a>Ayrılmış varlıklar/grafikler ekleme
Yeni `DbContext.AttachGraph` API yardımcı olan bir bağlam varlıklara yeni/değiştirilmiş varlıklar kaydetmek için yeniden bağlayın.

## <a name="saving-data"></a>Verileri kaydetme
### <a name="basic-save-functionality"></a>Temel işlevleri kaydetme
Varlık örneklerini veritabanına kalıcı değişiklikler sağlar.
### <a name="optimistic-concurrency"></a>İyimser eşzamanlılık
Verileri veritabanından getirildikten sonra başka bir kullanıcı tarafından yapılan değişikliklerin üzerine karşı korur.
### <a name="async-savechanges"></a>Zaman uyumsuz SaveChanges
Veritabanı verildiği komutları işlerken diğer isteklerini işlemek için geçerli iş parçacığının boşaltmak `SaveChanges`.
### <a name="database-transactions"></a>Veritabanı işlemleri
Anlamına `SaveChanges` her zaman (ya da tamamen başarılı ya da herhangi bir değişiklik veritabanına yapılan anlamına gelir) atomik olur. Ayrıca vardır işlem ilgili işlemler bağlam örnekleri vb. arasında paylaşılmasına izin verecek şekilde API'ler.
### <a name="relational-batching-of-statements"></a>İlişkisel: deyimleri toplu işleme
Veritabanı tek bir gidiş dönüş içine birden çok ekleme/güncelleştirme/silme komutlarını yığınlama daha iyi performans sağlar.

## <a name="query"></a>Sorgu
### <a name="basic-linq-support"></a>Temel LINQ desteği
Veritabanından veri almak için LINQ kullanma olanağı sağlar.
### <a name="mixed-clientserver-evaluation"></a>Karma istemci/sunucu değerlendirme
Veritabanında değerlendirilemez ve bu nedenle veri belleğe alındıktan sonra değerlendirilmelidir mantığı içerecek şekilde sorguları sağlar.
### <a name="notracking"></a>NoTracking
Sorguları daha hızlı sorgu yürütme bağlamı (yani sonuçları salt okunur) varlık örneklerini değişiklikleri izlemek gerekmez, sağlar.
### <a name="eager-loading"></a>istekli yükleniyor
Sağlar `Include` ve `ThenInclude` de sorgulanırken getirileceği ilgili verileri tanımlamak için yöntemleri.
### <a name="async-query"></a>Zaman uyumsuz sorgu
Boş veritabanı sırasında diğer isteklerini işlemek için geçerli iş parçacığının (ve onun ilişkili kaynakları) işler sorgu.
### <a name="raw-sql-queries"></a>Ham SQL sorguları
Sağlar `DbSet.FromSql` ham SQL kullanılacak yöntemi sorgular veri getirilemiyor. Bu sorguları LINQ kullanarak da oluşabilir.

## <a name="database-schema-management"></a>Veritabanı şeması Yönetimi       
### <a name="database-creationdeletion-apis"></a>Veritabanı oluşturma/silme API'leri
Çoğunlukla hızla oluşturun / veritabanı geçişler kullanmadan silmek istediğiniz test etmek için tasarlanmıştır.
### <a name="relational-database-migrations"></a>İlişkisel veritabanı geçişleri
Model değişikliklerinizi olarak mesai gelişmesi bir ilişkisel veritabanı şeması izin verir.
### <a name="reverse-engineer-from-database"></a>Veritabanından ters mühendislik
Varolan bir ilişkisel veritabanı şemasını temel alarak bir EF model iskelesini kurar.

## <a name="database-providers"></a>Veritabanı sağlayıcıları
### <a name="sql-server"></a>SQL Server
Microsoft SQL Server 2008 veya sonraki sürümleri bağlanır.
### <a name="sqlite"></a>SQLite
SQLite 3 veritabanına bağlar.
### <a name="in-memory"></a>Bellek içi
Gerçek bir veritabanına bağlanmadan sınama kolayca tanımak için tasarlanmıştır.
### <a name="3rd-party-providers"></a>3. taraf sağlayıcılar
Birkaç sağlayıcıları için diğer veritabanı altyapısı kullanılabilir. Bkz: [veritabanı sağlayıcıları](../providers/index.md) tam listesi için.
