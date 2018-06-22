---
title: SQLite veritabanı sağlayıcısı - sınırlamalar - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 8a60ccfc61a5757df8ebedf257379d4d3dbffbf6
ms.sourcegitcommit: 60b831318c4f5ec99061e8af6a7c9e7c03b3469c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
ms.locfileid: "29719491"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>SQLite EF çekirdek veritabanı sağlayıcısı sınırlamaları

SQLite sağlayıcısı bir dizi geçişler sınırlama vardır. Bu sınırlamalara çoğu sınırlamaları temel SQLite Veritabanı Altyapısı'ndaki bir sonucudur ve EF için özel değildir.

## <a name="modeling-limitations"></a>Modelleme sınırlamaları

(Entity Framework ilişkisel veritabanı sağlayıcıları tarafından paylaşılan) ortak ilişkisel kitaplığı API'leri, çoğu ilişkisel veritabanı motoru için ortak olan kavramları modelleme için tanımlar. Bu kavramlar birkaç SQLite sağlayıcı tarafından desteklenmiyor.

* Şemaları
* Diziler

## <a name="migrations-limitations"></a>Geçiş sınırlamaları

SQLite veritabanı altyapısı diğer ilişkisel veritabanları çoğunluğu tarafından desteklenen şema işlemlerinin sayısı desteklemez. Desteklenmeyen işlemlerden biri, bir SQLite veritabanı için geçerli çalışırsanız sonra bir `NotSupportedException` oluşturulur.

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
| RenameColumn         | ✗          |                  |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (no-op)  | 2,0              |
| DropSchema           | ✔ (no-op)  | 2,0              |
| Ekleme               | ✔          | 2,0              |
| Güncelleştirme               | ✔          | 2,0              |
| Sil               | ✔          | 2,0              |

## <a name="migrations-limitations-workaround"></a>Geçiş sınırlamaları geçici çözüm

Geçici çözüm bazı yapabilecekleriniz sınırlamalara tablo gerçekleştirmek için geçiş kodu el ile yazarak, yeniden oluşturun. Bir tablo yeniden oluşturma, varolan bir tabloyu yeniden adlandırma, yeni bir tablo oluşturma, yeni tabloya veri kopyalama ve eski tablo bırakma içerir. Kullanmanız gerekecektir `Sql(string)` bazı adımları gerçekleştirmek için yöntem.

Bkz: [yapmadan diğer türleri, tablo şema değişiklikleri](http://sqlite.org/lang_altertable.html#otheralter) daha fazla ayrıntı için SQLite belgelerinde.

Gelecekte, EF bazı işlemlerini kapsar altında tablo yeniden yaklaşımı kullanarak destekleyebilir. Yapabilecekleriniz [GitHub Projemizin bu özellik izlemek](https://github.com/aspnet/EntityFrameworkCore/issues/329).
