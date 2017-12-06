---
title: "SQLite veritabanı sağlayıcısı - sınırlamalar - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>SQLite EF çekirdek veritabanı sağlayıcısı sınırlamaları

SQLite sağlayıcısı bir dizi geçişler sınırlama vardır. Bu sınırlamalara çoğu sınırlamaları temel SQLite Veritabanı Altyapısı'ndaki bir sonucudur ve EF için özel değildir.

## <a name="modeling-limitations"></a>Modelleme sınırlamaları

(Entity Framework ilişkisel veritabanı sağlayıcıları tarafından paylaşılan) ortak ilişkisel kitaplığı API'leri, çoğu ilişkisel veritabanı motoru için ortak olan kavramları modelleme için tanımlar. Bu kavramlar birkaç SQLite sağlayıcı tarafından desteklenmiyor.

* Şemaları
* Diziler

## <a name="migrations-limitations"></a>Geçiş sınırlamaları

SQLite veritabanı altyapısı diğer ilişkisel veritabanları çoğunluğu tarafından desteklenen şema işlemlerinin sayısı desteklemez. Desteklenmeyen işlemlerden biri, bir SQLite veritabanı için geçerli çalışırsanız sonra bir `NotSupportedException` oluşturulur.

| İşlemi            | Destekleniyor mu? |
| -------------------- | ---------- |
| AddColumn            | ✔          |
| AddForeignKey        | ✗          |
| AddPrimaryKey        | ✗          |
| AddUniqueConstraint  | ✗          |
| AlterColumn          | ✗          |
| CreateIndex          | ✔          |
| CreateTable          | ✔          |
| DropColumn           | ✗          |
| DropForeignKey       | ✗          |
| DropIndex            | ✔          |
| DropPrimaryKey       | ✗          |
| DropTable            | ✔          |
| DropUniqueConstraint | ✗          |
| RenameColumn         | ✗          |
| RenameIndex          | ✗          |
| RenameTable          | ✔          |

## <a name="migrations-limitations-workaround"></a>Geçiş sınırlamaları geçici çözüm

Geçici çözüm bazı yapabilecekleriniz sınırlamalara tablo gerçekleştirmek için geçiş kodu el ile yazarak, yeniden oluşturun. Bir tablo yeniden oluşturma, varolan bir tabloyu yeniden adlandırma, yeni bir tablo oluşturma, yeni tabloya veri kopyalama ve eski tablo bırakma içerir. Kullanmanız gerekecektir `Sql(string)` bazı adımları gerçekleştirmek için yöntem.

Bkz: [yapmadan diğer türleri, tablo şema değişiklikleri](http://sqlite.org/lang_altertable.html#otheralter) daha fazla ayrıntı için SQLite belgelerinde.

Gelecekte, EF bazı işlemlerini kapsar altında tablo yeniden yaklaşımı kullanarak destekleyebilir. Yapabilecekleriniz [GitHub Projemizin bu özellik izlemek](https://github.com/aspnet/EntityFramework/issues/329).
