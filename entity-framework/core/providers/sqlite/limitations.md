---
title: SQLite veritabanı sağlayıcısı - kısıtlamaları - EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: eaa7d5b1496172e4f3821433a1cd098ee7e8b737
ms.sourcegitcommit: 9bd64a1a71b7f7aeb044aeecc7c4785b57db1ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394795"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>SQLite EF Core veritabanı sağlayıcısı sınırlamaları

SQLite sağlayıcısı, bir dizi geçişleri sınırlandırması vardır. Bu sınırlamaların çoğunu sınırlamaları temel SQLite Veritabanı Altyapısı'ndaki bir sonucudur ve EF için özel değildir.

## <a name="modeling-limitations"></a>Modelleme sınırlamaları

(Entity Framework ilişkisel veritabanı sağlayıcıları tarafından paylaşılan) ortak ilişkisel kitaplığı, çoğu ilişkisel veritabanı motoru için ortak olan kavramları modelleme için API tanımlar. Bu kavramlar birkaç SQLite sağlayıcı tarafından desteklenmiyor.

* Şemaları
* Diziler
* Hesaplanan sütunlar

## <a name="query-limitations"></a>Sorgu sınırlamaları

SQLite yerel olarak aşağıdaki veri türlerini desteklemiyor. EF Core okuma ve yazma değerlerini ve bu tür, eşitlik için sorgulama (`where e.Property == value`) de desteğidir. Diğer işlemler, ancak karşılaştırma ister ve sıralama istemcide değerlendirme gerektirir.

* DateTimeOffset
* Ondalık
* TimeSpan
* UInt64

Yerine `DateTimeOffset`, DateTime değerleri kullanmanızı öneririz. Birden fazla saat dilimlerini işlerken, kaydetme ve ardından uygun saat dilimine dönüştürme önce UTC'ye değerleri dönüştürmenizi öneririz.

`Decimal` Türü, yüksek düzeyde bir duyarlık sağlar. Ancak, duyarlık düzeyini gerekmiyorsa, çift kullanmayı öneririz. Kullanabileceğiniz bir [değer dönüştürücü](../../modeling/value-conversions.md) ondalık sınıflarınızdaki kullanmaya devam etmek için.

``` csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

## <a name="migrations-limitations"></a>Geçiş sınırlamaları

SQLite veritabanı altyapısı, bir dizi diğer ilişkisel veritabanlarını çoğunluğu tarafından desteklenen şema işlemi desteklemiyor. Desteklenmeyen işlemlerden biri bir SQLite veritabanı için geçerli çalışırsanız bir `NotSupportedException` oluşturulur.

| Çalışma            | Destekleniyor mu? | Sürümünü gerektirir |
|:---------------------|:-----------|:-----------------|
| AddColumn            | ✔          | 1.0              |
| AddForeignKey        | ✗          |                  |
| AddPrimaryKey        | ✗          |                  |
| AddUniqueConstraint  | ✗          |                  |
| AlterColumn          | ✗          |                  |
| CreateIndex          | ✔          | 1.0              |
| CreateTable          | ✔          | 1.0              |
| DropColumn           | ✗          |                  |
| DropForeignKey       | ✗          |                  |
| DropIndex            | ✔          | 1.0              |
| DropPrimaryKey       | ✗          |                  |
| DropTable            | ✔          | 1.0              |
| DropUniqueConstraint | ✗          |                  |
| RenameColumn         | ✔          | 2.2.2            |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (İşlemsiz)  | 2,0              |
| DropSchema           | ✔ (İşlemsiz)  | 2,0              |
| Ekleme               | ✔          | 2,0              |
| Güncelleştirme               | ✔          | 2,0              |
| Sil               | ✔          | 2,0              |

## <a name="migrations-limitations-workaround"></a>Geçiş sınırlamaları geçici çözüm

Geçici çözüm bazıları için el ile bir tablo gerçekleştirmek için geçiş kodu yazarak bu sınırlamalardan yeniden oluşturun. Bir tablo yeniden oluşturma, varolan bir tabloyu yeniden adlandırma, yeni bir tablo oluşturma, yeni tabloya veri kopyalama ve eski tablo bırakılırken içerir. Kullanmanız gerekecektir `Sql(string)` bazı adımları gerçekleştirmek için yöntemi.

Bkz: [yapmadan diğer tür, tablo şema değişiklikleri](http://sqlite.org/lang_altertable.html#otheralter) SQLite belgelerinde daha fazla ayrıntı için.

Gelecekte EF bazıları bu işlemleri arka planda altında tablo yeniden oluşturma yaklaşımı kullanarak destekleyebilir. Yapabilecekleriniz [bizim GitHub üzerinde bu özelliği izlemek](https://github.com/aspnet/EntityFrameworkCore/issues/329).
