---
title: API - EF Core oluşturma ve bırakma
author: bricelam
ms.author: bricelam
ms.date: 11/10/2017
ms.openlocfilehash: 336f6fd655603a2474a58dfef377e121d9b04c3a
ms.sourcegitcommit: a088421ecac4f5dc5213208170490181ae2f5f0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285645"
---
# <a name="create-and-drop-apis"></a>API oluşturma ve bırakma

Basit bir alternatif EnsureCreated ve EnsureDeleted yöntemleri sağlaması [geçişler](migrations/index.md) veritabanı şemasını yönetme. Veriler geçici ve şema değiştiğinde bırakılan bu senaryolarda yararlı olur. Örneğin prototip testleri veya yerel sırasında.

Bazı sağlayıcıları (özellikle, ilişkisel olmayan olanlar) geçişleri desteklemez. Bunlar için EnsureCreated genellikle veritabanı şemasına başlatmak için en kolay yoludur.

> [!WARNING]
> EnsureCreated ve geçişleri birlikte düzgün çalışmaz. Geçişleri kullanıyorsanız EnsureCreated şema başlatmak için kullanmayın.

Geçişleri EnsureCreated geçiş, sorunsuz bir deneyim değil. Bunu başarmanın simpelest veritabanını bırakın ve geçişleri kullanarak yeniden oluşturmak için yoludur. Geçişleri gelecekte kullanarak öngörüyorsanız, EnsureCreated kullanmak yerine Migrations ile hemen başlatmak idealdir.

## <a name="ensuredeleted"></a>EnsureDeleted

Varsa EnsureDeleted yöntemi veritabanı kaldıracağız. Uygun izinleriniz yoksa, bir özel durum oluşturulur.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

Mevcut değil ve veritabanı şeması başlatmak EnsureCreated veritabanı oluşturur. Herhangi bir tablo olması durumunda (başka bir DbContext sınıfı için tablolar dahil), şema başlatılması olmaz.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> Bu yöntemlerin zaman uyumsuz sürümleri de mevcuttur.

## <a name="sql-script"></a>SQL betiği

EnsureCreated tarafından kullanılan SQL almak için GenerateCreateScript yöntemi kullanabilirsiniz.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Birden çok DbContext sınıfı

Tablo veritabanında mevcut olduğunda EnsureCreated yalnızca çalışır. Gerekirse, şema başlatılması gerekip gerekmediğini görmek için kendi denetiminizi yazabilir ve temel alınan IRelationalDatabaseCreator hizmet Şeması'nı başlatmak için kullanın.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
