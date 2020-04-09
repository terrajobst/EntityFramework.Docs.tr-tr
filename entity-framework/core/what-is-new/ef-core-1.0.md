---
title: EF Core 1.0'daki yenilikler - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417526"
---
# <a name="features-included-in-ef-core-10"></a>EF Core 1.0'da yer alan özellikler

## <a name="platforms"></a>Platformlar

### <a name="net-framework-451"></a>.NET Framework 4.5.1

Konsol, WPF, WinForms, ASP.NET 4, vb içerir

### <a name="net-standard-13"></a>.NET Standart 1.3

Windows, OSX ve Linux'ta .NET Framework ve .NET Core'u hedefleyen ASP.NET Core dahil.

## <a name="modelling"></a>Modelleme

### <a name="basic-modelling"></a>Temel modelleme

Ortak skaler türlerinin (,`int`vb.) `string`get/set özelliklerine sahip POCO varlıklarına dayanmaktadır.

### <a name="relationships-and-navigation-properties"></a>İlişkiler ve gezinme özellikleri

Modelde yabancı bir anahtara dayalı bire bir ve bire sıfır veya bir ilişkileri belirtilebilir. Basit toplama veya başvuru türlerinin gezinme özellikleri bu ilişkilerle ilişkilendirilebilir.

### <a name="built-in-conventions"></a>Yerleşik sözleşmeler

Bunlar, varlık sınıflarının şekline dayalı bir başlangıç modeli oluşturur.

### <a name="fluent-api"></a>Akıcı API

Kuralı tarafından keşfedilen `OnModelCreating` modeli daha da yapılandırmak için bağlamınızdaki yöntemi geçersiz kılmanızı sağlar.

### <a name="data-annotations"></a>Veri açıklamaları

Varlık sınıflarınıza/özelliklerinize eklenebilecek ve EF modelini etkileyecek özniteliklerdir. Örneğin, ekleme, `[Required]` EF'ye bir özelliğin gerekli olduğunu bilmesini sağlar.

### <a name="relational-table-mapping"></a>İlişkisel Tablo eşleme

Varlıkların tablolara/sütunlara eşlenemesini sağlar.

### <a name="key-value-generation"></a>Anahtar değer oluşturma

İstemci tarafı oluşturma ve veritabanı oluşturma dahil.

### <a name="database-generated-values"></a>Veritabanı oluşturulan değerler

Ekler (varsayılan değerler) veya güncelleştirme (hesaplanmış sütunlar) veritabanı tarafından oluşturulacak değerlerin sağlar.

### <a name="sequences-in-sql-server"></a>SQL Server'da Diziler

Dizi nesnelerinin modelde tanımlanmasına izin verir.

### <a name="unique-constraints"></a>Benzersiz kısıtlamalar

Alternatif anahtarların tanımına ve bu anahtarı hedefleyen ilişkileri tanımlama yeteneğine olanak tanır.

### <a name="indexes"></a>Dizinler

Modeldeki dizinleri tanımlamak veritabanında dizinleri otomatik olarak tanıtıyor. Benzersiz dizinler de desteklenir.

### <a name="shadow-state-properties"></a>Gölge durumu özellikleri

Modelde belirtilmeyen ve .NET sınıfında depolanmayan ancak EF Core tarafından izlenebilir ve güncelleştirilebilen özelliklerin tanımlanmasına izin verir. Bunları nesnede teşhir ederken yabancı anahtar özellikleri için yaygın olarak kullanılır.

### <a name="table-per-hierarchy-inheritance-pattern"></a>Tablo-Hiyerarşi Başına kalıtım deseni

Devralma hiyerarşisindeki varlıkların, veritabanında belirli bir kayıt için varlık türünü tanımlamak için ayırıcı sütun kullanarak tek bir tabloya kaydedilmesine izin verir.

### <a name="model-validation"></a>Model doğrulaması

Modeldeki geçersiz desenleri algılar ve yararlı hata iletileri sağlar.

## <a name="change-tracking"></a>Değişiklik izleme

### <a name="snapshot-change-tracking"></a>Anlık görüntü değiştirme izleme

Geçerli durumu özgün durumu kopyalayan (anlık görüntü) ile karşılaştırarak varlıklardaki değişikliklerin otomatik olarak algılanmasına izin verir.

### <a name="notification-change-tracking"></a>Bildirim değişikliği izleme

Özellik değerleri değiştirildiğinde, varlıklarınızın değişiklik izleyicisini bildirmesine olanak tanır.

### <a name="accessing-tracked-state"></a>İzlenen duruma erişme

Via `DbContext.Entry` `DbContext.ChangeTracker`ve .

### <a name="attaching-detached-entitiesgraphs"></a>Ayrılmış varlıkları/grafikleri ekleme

Yeni `DbContext.AttachGraph` API, yeni/değiştirilmiş varlıkları kaydetmek için varlıkları bir içeriğe yeniden eklemeye yardımcı olur.

## <a name="saving-data"></a>Verileri kaydetme

### <a name="basic-save-functionality"></a>Temel kaydetme işlevi

Varlık örneklerindeki değişikliklerin veritabanında kalıcı olmasını sağlar.

### <a name="optimistic-concurrency"></a>İyimser Eşzamanlılık

Veriler veritabanından getirildiğinden, başka bir kullanıcı tarafından yapılan aşırı yazım değişikliklerine karşı korur.

### <a name="async-savechanges"></a>Async SaveChanges

Veritabanı' dan verilen komutları işlerken, diğer istekleri işlemek `SaveChanges`için geçerli iş parçacığı serbest

### <a name="database-transactions"></a>Veritabanı İşlemleri

Her `SaveChanges` zaman atomik olduğu anlamına gelir (ya tamamen başarılı olur veya veritabanında herhangi bir değişiklik yapılmaz). İçerik örnekleri vb. arasında işlem paylaşımına izin vermek için işlemle ilgili API'ler de vardır.

### <a name="relational-batching-of-statements"></a>İlişkisel: İfadelerin toplu olarak gruplanması

Birden çok INSERT/UPDATE/DELETE komutunu veritabanına tek bir gidiş dönüşe ekleyerek daha iyi performans sağlar.

## <a name="query"></a>Sorgu

### <a name="basic-linq-support"></a>Temel LINQ desteği

Veritabanından veri almak için LINQ'yi kullanma olanağı sağlar.

### <a name="mixed-clientserver-evaluation"></a>Karışık istemci/sunucu değerlendirmesi

Sorguların veritabanında değerlendirilemeyen bir mantık içermesini sağlar ve bu nedenle veriler belleğe alındıktan sonra değerlendirilmesi gerekir.

### <a name="notracking"></a>Notracking

Bağlam varlık örneklerideğişiklikleri için izlemek gerekmez (sonuçlar salt okunursa bu yararlıdır) sorgular daha hızlı sorgu yürütme sağlar.

### <a name="eager-loading"></a>Istekli yükleme

`Include` Sorguyaparken `ThenInclude` alınması gereken ilgili verileri tanımlamak için ve yöntemler sağlar.

### <a name="async-query"></a>Async sorgusu

Veritabanı sorguyu işlerken diğer istekleri işlemek için geçerli iş parçacığı (ve ilişkili kaynaklar) kadar serbest olabilir.

### <a name="raw-sql-queries"></a>Ham SQL sorguları

Verileri `DbSet.FromSql` almak için ham SQL sorgularını kullanma yöntemini sağlar. Bu sorgular LINQ kullanılarak da oluşturulabilir.

## <a name="database-schema-management"></a>Veritabanı şema yönetimi

### <a name="database-creationdeletion-apis"></a>Veritabanı oluşturma/silme API'leri

Geçişleri kullanmadan veritabanını hızla oluşturmak/silmek istediğiniz leri sınamak için tasarlanmıştır.

### <a name="relational-database-migrations"></a>İlişkisel veritabanı geçişleri

İlişkisel veritabanı şemasının modeliniz değiştikçe fazla mesaiye dönüşmesine izin verin.

### <a name="reverse-engineer-from-database"></a>Veritabanından ters mühendis

İskele, varolan bir ilişkisel veritabanı şemasına dayalı bir EF modelini şekillendiricidir.

## <a name="database-providers"></a>Veritabanı sağlayıcıları

### <a name="sql-server"></a>SQL Server

Microsoft SQL Server 2008'e bağlanır.

### <a name="sqlite"></a>SQLite

Bir SQLite 3 veritabanına bağlanır.

### <a name="in-memory"></a>Bellek Içi

Gerçek bir veritabanına bağlanmadan testi kolayca etkinleştirmek için tasarlanmıştır.

### <a name="3rd-party-providers"></a>3. taraf sağlayıcılar

Diğer veritabanı motorları için çeşitli sağlayıcılar kullanılabilir. Tam liste için [Veritabanı Sağlayıcıları'na](../providers/index.md) bakın.
