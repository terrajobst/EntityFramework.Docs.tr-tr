---
title: API - EF Core oluşturma ve bırakma
author: bricelam
ms.author: bricelam
ms.date: 11/7/2018
ms.openlocfilehash: 40d9e3aa0aba1bf2bc341f01dd815ed7cb7b48fa
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688635"
---
# <a name="create-and-drop-apis"></a>API oluşturma ve bırakma

Basit bir alternatif EnsureCreated ve EnsureDeleted yöntemleri sağlaması [geçişler](migrations/index.md) veritabanı şemasını yönetme. Veriler geçici ve şema değiştiğinde bırakılan bu yöntemleri senaryolarda yararlıdır. Örneğin prototip testleri veya yerel sırasında.

Bazı sağlayıcıları (özellikle, ilişkisel olmayan olanlar) geçişleri desteklemez. Bu sağlayıcıları için EnsureCreated genellikle veritabanı şemasına başlatmak için en kolay yoludur.

> [!WARNING]
> EnsureCreated ve geçişleri birlikte düzgün çalışmaz. Geçişleri kullanıyorsanız EnsureCreated şema başlatmak için kullanmayın.

Geçişleri EnsureCreated geçiş, sorunsuz bir deneyim değil. Bunu yapmanın en kolay yolu veritabanını bırakın ve geçişleri kullanarak yeniden oluşturmaktır. Geçişleri gelecekte kullanarak öngörüyorsanız, EnsureCreated kullanmak yerine Migrations ile hemen başlatmak idealdir.

## <a name="ensuredeleted"></a>EnsureDeleted

Varsa EnsureDeleted yöntemi veritabanı kaldıracağız. Uygun izinlere sahip değilseniz, bir özel durum oluşturulur.

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
