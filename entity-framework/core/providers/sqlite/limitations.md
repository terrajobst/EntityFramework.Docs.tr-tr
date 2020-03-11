---
title: SQLite veritabanı sağlayıcısı-sınırlamalar-EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 2f80dc195265787318ac4925dd937da45ffad011
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417776"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>SQLite EF Core veritabanı sağlayıcısı sınırlamaları

SQLite sağlayıcının bir dizi geçiş sınırlaması vardır. Bu sınırlamaların çoğu, temel alınan SQLite veritabanı altyapısındaki kısıtlamaların bir sonucudur ve EF 'e özgü değildir.

## <a name="modeling-limitations"></a>Modelleme sınırlamaları

Ortak ilişkisel kitaplık (Entity Framework ilişkisel veritabanı sağlayıcıları tarafından paylaşılır), en ilişkisel veritabanı altyapılarında ortak olan modelleme kavramları için API 'Leri tanımlar. Bu kavramların birkaç ikisi SQLite sağlayıcısı tarafından desteklenmez.

* Şemalar
* Diziler
* Hesaplanan sütunlar

## <a name="query-limitations"></a>Sorgu sınırlamaları

SQLite, aşağıdaki veri türlerini yerel olarak desteklemez. EF Core bu türlerin değerlerini okuyup yazabilir ve eşitlik için sorgulama (`where e.Property == value`) de desteklenir. Bununla birlikte, karşılaştırma ve sıralama gibi diğer işlemler de istemci üzerinde değerlendirme gerektirir.

* DateTimeOffset
* Ondalık
* TimeSpan
* UInt64

`DateTimeOffset`yerine, DateTime değerlerini kullanmanızı öneririz. Birden çok saat dilimini işlerken, kaydetmeden önce değerleri UTC 'ye dönüştürmenizi ve sonra uygun saat dilimine geri dönüştürmeyi öneririz.

`Decimal` türü yüksek düzeyde bir duyarlık sağlar. Ancak bu duyarlık düzeyine ihtiyacınız yoksa, bunun yerine Double kullanmanızı öneririz. Sınıflarınızda ondalık olarak kullanmaya devam etmek için bir [değer Dönüştürücüsü](../../modeling/value-conversions.md) kullanabilirsiniz.

``` csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

## <a name="migrations-limitations"></a>Geçiş sınırlamaları

SQLite veritabanı altyapısı, diğer ilişkisel veritabanlarının çoğunluğu tarafından desteklenen bir dizi şema işlemini desteklemez. Desteklenmeyen işlemlerden birini bir SQLite veritabanına uygulamaya çalışırsanız, `NotSupportedException` oluşturulur.

| İşlem            | Destekleniyor mu? | Sürüm gerektirir |
|:---------------------|:-----------|:-----------------|
| AddColumn            | ✔          | 1.0              |
| AddForeignKey        | ✗          |                  |
| AddPrimaryKey        | ✗          |                  |
| AddUniqueConstraint kısıtlaması  | ✗          |                  |
| AlterColumn          | ✗          |                  |
| CreateIndex          | ✔          | 1.0              |
| CreateTable          | ✔          | 1.0              |
| Açılan sütun           | ✗          |                  |
| Dropyabancıanahtarı       | ✗          |                  |
| Açılan Dizin            | ✔          | 1.0              |
| DropPrimaryKey       | ✗          |                  |
| Açılan tablo            | ✔          | 1.0              |
| DropUniqueConstraint kısıtlaması | ✗          |                  |
| RenameColumn         | ✔          | 2.2.2            |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (-OP)  | 2,0              |
| DropSchema           | ✔ (-OP)  | 2,0              |
| Ekle               | ✔          | 2,0              |
| Güncelleştir               | ✔          | 2,0              |
| Sil               | ✔          | 2,0              |

## <a name="migrations-limitations-workaround"></a>Geçiş kısıtlamaları geçici çözümü

Tablo yeniden oluşturma işlemi gerçekleştirmek için geçişlerinizi el ile yazarak bu sınırlamaların bazılarını geçici olarak yapabilirsiniz. Tablo yeniden oluşturma, var olan tabloyu yeniden adlandırmayı, yeni bir tablo oluşturmayı, yeni tabloya veri kopyalamayı ve eski tabloyu bırakmayı içerir. Bu adımlardan bazılarını gerçekleştirmek için `Sql(string)` yöntemini kullanmanız gerekir.

Daha fazla ayrıntı için bkz. SQLite belgelerinde [diğer tür tablo şeması değişiklikleri yapma](https://sqlite.org/lang_altertable.html#otheralter) .

Daha sonra, EF 'in altındaki tablo yeniden oluşturma yaklaşımını kullanarak bu işlemlerden bazılarını destekleyebilir. [Bu özelliği GitHub projemizdeki](https://github.com/aspnet/EntityFrameworkCore/issues/329)bir şekilde izleyebilirsiniz.
