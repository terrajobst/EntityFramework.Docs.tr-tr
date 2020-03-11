---
title: EF Core 1,0 ' deki yenilikler-EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417526"
---
# <a name="features-included-in-ef-core-10"></a>EF Core 1,0 ' de bulunan özellikler

## <a name="platforms"></a>Platformlar

### <a name="net-framework-451"></a>.NET Framework 4.5.1

Konsol, WPF, WinForms, ASP.NET 4 vb. içerir.

### <a name="net-standard-13"></a>.NET Standard 1,3

Hem .NET Framework hem de .NET Core 'u Windows, OSX ve Linux üzerinde hedefleme ASP.NET Core dahil.

## <a name="modelling"></a>Modelleme

### <a name="basic-modelling"></a>Temel modelleme

Ortak skaler türlerin (`int`, `string`, vb.) Get/Set özellikleriyle POCO varlıklarını temel alır.

### <a name="relationships-and-navigation-properties"></a>İlişkiler ve gezinti özellikleri

Bire çok ve bire sıfır veya-bir ilişki yabancı anahtara göre modelde belirtilebilir. Basit koleksiyon veya başvuru türlerinin gezinti özellikleri, bu ilişkilerle ilişkilendirilebilir.

### <a name="built-in-conventions"></a>Yerleşik kurallar

Bu, varlık sınıflarının şekline göre bir başlangıç modeli oluşturur.

### <a name="fluent-api"></a>Akıcı API

Kural tarafından bulunan modeli daha fazla yapılandırmak için, bağlamdaki `OnModelCreating` yöntemi geçersiz kılmanızı sağlar.

### <a name="data-annotations"></a>Veri açıklamaları

, Varlık sınıflarınıza/özelliklerine eklenebilen özniteliklerdir ve EF modelini etkiler. Örneğin, `[Required]` eklemek EF 'in bir özelliğin gerekli olduğunu bilmesini sağlar.

### <a name="relational-table-mapping"></a>İlişkisel tablo eşleme

Varlıkların tablolarla/sütunlarla eşleştirilmesini sağlar.

### <a name="key-value-generation"></a>Anahtar değeri oluşturma

İstemci tarafı oluşturma ve veritabanı oluşturma dahil.

### <a name="database-generated-values"></a>Veritabanı tarafından oluşturulan değerler

Ekleme (varsayılan değerler) veya Update (hesaplanan sütunlar) üzerinde veritabanı tarafından oluşturulmasına izin verir.

### <a name="sequences-in-sql-server"></a>SQL Server diziler

Model içinde sıralama nesnelerinin tanımlanmasını sağlar.

### <a name="unique-constraints"></a>Benzersiz kısıtlamalar

Alternatif anahtarların tanımına ve bu anahtarı hedefleyen ilişkiler tanımlamasına olanak sağlar.

### <a name="indexes"></a>Dizinler

Modeldeki dizinlerin tanımlanması veritabanındaki dizinleri otomatik olarak tanıtır. Benzersiz dizinler de desteklenir.

### <a name="shadow-state-properties"></a>Gölge durumu özellikleri

Özelliklerin bildirilmemiş ve .NET sınıfında depolanmayan, ancak EF Core tarafından izlenip güncelleştirilebilecek modelde tanımlanmasına izin verir. Genellikle yabancı anahtar özellikleri için kullanılır.

### <a name="table-per-hierarchy-inheritance-pattern"></a>Tablo başına devralma düzeni

Bir devralma hiyerarşisindeki varlıkların veritabanındaki belirli bir kayıt için varlık türünü belirlemek üzere bir Ayrıştırıcı sütunu kullanılarak tek bir tabloya kaydedilmesine izin verir.

### <a name="model-validation"></a>Model doğrulaması

Modelde geçersiz desenler algılar ve yararlı hata iletileri sağlar.

## <a name="change-tracking"></a>Değişiklik izleme

### <a name="snapshot-change-tracking"></a>Anlık görüntü değişiklik izleme

Geçerli durum özgün durumun bir kopyasına (anlık görüntü) göre karşılaştırılırken varlıklarda yapılan değişikliklerin otomatik olarak algılanabilmesi için izin verir.

### <a name="notification-change-tracking"></a>Bildirim değişikliği izleme

Özellik değerleri değiştirildiğinde varlıklarınızın değişiklik izleyicide bilgilendirmesini sağlar.

### <a name="accessing-tracked-state"></a>İzlenen duruma erişme

`DbContext.Entry` ve `DbContext.ChangeTracker`aracılığıyla.

### <a name="attaching-detached-entitiesgraphs"></a>Ayrılmış varlıkları/grafikleri iliştirme

Yeni `DbContext.AttachGraph` API 'SI, yeni/değiştirilmiş varlıkların kaydedileceği varlıkları bir bağlama yeniden iliştirmenize yardımcı olur.

## <a name="saving-data"></a>Verileri kaydetme

### <a name="basic-save-functionality"></a>Temel kaydetme işlevselliği

Varlık örneklerindeki değişikliklerin veritabanına kalıcı olmasını sağlar.

### <a name="optimistic-concurrency"></a>İyimser Eşzamanlılık

Verilerin veritabanından getirilmesi bu yana başka bir kullanıcı tarafından yapılan değişikliklerin üzerine yazılmasına karşı koruma sağlar.

### <a name="async-savechanges"></a>Zaman uyumsuz SaveChanges

, Veritabanı `SaveChanges`verilen komutları işlerken diğer istekleri işlemek için geçerli iş parçacığını serbest bırakabilirsiniz.

### <a name="database-transactions"></a>Veritabanı Işlemleri

`SaveChanges` her zaman atomik olduğu anlamına gelir (yani tamamen başarılı olur ya da veritabanında değişiklik yapılmaz). Ayrıca bağlam örnekleri arasında işleme işlemlerine izin veren işlem ile ilgili API 'Ler vardır.

### <a name="relational-batching-of-statements"></a>İlişkisel: deyimlerin toplu işi

Birden fazla ekleme/GÜNCELLEŞTIRME/SILME komutunu veritabanına tek bir gidiş dönüş halinde toplu olarak oluşturarak daha iyi performans sağlar.

## <a name="query"></a>Sorgu

### <a name="basic-linq-support"></a>Temel LINQ desteği

Veritabanından veri almak için LINQ kullanma özelliği sağlar.

### <a name="mixed-clientserver-evaluation"></a>Karma istemci/sunucu değerlendirmesi

Sorguların veritabanında değerlendirilemeyen mantığı içermesini sağlar ve bu nedenle veriler belleğe alındıktan sonra değerlendirilmelidir.

### <a name="notracking"></a>NoTracking

Bağlam, varlık örneklerine yapılan değişiklikleri izlemeye gerek olmadığında daha hızlı sorgu yürütmeye olanak sağlar (sonuçlar salt okunurdur, bu faydalıdır).

### <a name="eager-loading"></a>ekip yükleme

, ' İ sorgularken de getirilmesi gereken ilgili verileri belirlemek için `Include` ve `ThenInclude` yöntemleri sağlar.

### <a name="async-query"></a>Zaman uyumsuz sorgu

, Veritabanı sorguyu işlerken diğer istekleri işlemek için geçerli iş parçacığını (ve ilişkili kaynakları) serbest bırakabilirsiniz.

### <a name="raw-sql-queries"></a>Ham SQL sorguları

Veri getirmek için ham SQL sorguları kullanmak üzere `DbSet.FromSql` yöntemi sağlar. Bu sorgular, LINQ kullanılarak da oluşturulabilir.

## <a name="database-schema-management"></a>Veritabanı şeması yönetimi

### <a name="database-creationdeletion-apis"></a>Veritabanı oluşturma/silme API 'Leri

Genellikle, geçişleri kullanmadan veritabanını hızlı bir şekilde oluşturmak/silmek istediğiniz test için tasarlanmıştır.

### <a name="relational-database-migrations"></a>İlişkisel veritabanı geçişleri

İlişkisel bir veritabanı şemasının, modelinizde değişiklik yaptığı fazla mesaiyi geliştirmeye izin verin.

### <a name="reverse-engineer-from-database"></a>Veritabanından tersine mühendislik

Var olan bir ilişkisel veritabanı şemasını temel alan bir EF modeli yapı iskelesi.

## <a name="database-providers"></a>Veritabanı sağlayıcıları

### <a name="sql-server"></a>SQL Server

Microsoft SQL Server 2008 ve sonraki sürümleri bağlanır.

### <a name="sqlite"></a>SQLite

SQLite 3 veritabanına bağlanır.

### <a name="in-memory"></a>Bellek içi

, Gerçek bir veritabanına bağlanmadan test etmeyi kolayca etkinleştirmek üzere tasarlanmıştır.

### <a name="3rd-party-providers"></a>3\. taraf sağlayıcılar

Diğer veritabanı motorları için çeşitli sağlayıcılar mevcuttur. Tüm liste için bkz. [veritabanı sağlayıcıları](../providers/index.md) .
