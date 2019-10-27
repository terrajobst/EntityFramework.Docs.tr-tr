---
title: API 'Leri oluşturma ve bırakma-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
uid: core/managing-schemas/ensure-created
ms.openlocfilehash: 32ac6cd043df73cd041780ec4c8805675adc5ab1
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811792"
---
# <a name="create-and-drop-apis"></a>API Oluşturma ve Bırakma

Ensuyeniden oluşturma ve EnsureDeleted yöntemleri, veritabanı şemasını yönetmeye yönelik [geçişlere](migrations/index.md) hafif bir alternatif sağlar. Bu yöntemler, verilerin geçici olduğu ve şema değiştiğinde bırakılan senaryolarda faydalıdır. Örneğin, prototipleme sırasında, testlerde veya yerel önbellekler için.

Bazı sağlayıcılar (özellikle ilişkisel olmayan) geçişleri desteklemez. Bu sağlayıcılar için, veritabanı şemasını başlatmanın en kolay yolu genellikle yeniden oluşturulur.

> [!WARNING]
> Yeniden oluşturulup geçişler birlikte iyi çalışmaz. Geçişleri kullanıyorsanız şemayı başlatmak için yeniden oluşturulması kullanmayın.

Geçişlere geçiş aşamasından geçiş, sorunsuz bir deneyim değildir. Bunu yapmanın en kolay yolu, veritabanını bırakıp geçişleri kullanarak yeniden oluşturmayı kullanmaktır. Daha sonra geçişlerde geçiş yapmayı düşünüyorsanız, en iyi şekilde, yeniden oluşturulması yerine geçişle başlamak yeterlidir.

## <a name="ensuredeleted"></a>EnsureDeleted

EnsureDeleted yöntemi, varsa veritabanını de bırakacak. Uygun izinleriniz yoksa bir özel durum oluşturulur.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>Yeniden oluşturuldu

Yeniden oluşturma, mevcut değilse veritabanını oluşturur ve veritabanı şemasını başlatabilir. Herhangi bir tablo varsa (başka bir DbContext sınıfı için tablolar dahil), şema başlatılmaz.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> Bu yöntemlerin zaman uyumsuz sürümleri de mevcuttur.

## <a name="sql-script"></a>SQL betiği

Yeniden oluştururken kullanılan SQL 'i almak için GenerateCreateScript yöntemini kullanabilirsiniz.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Birden çok DbContext sınıfı

Ensuyeniden oluşturulması yalnızca veritabanında hiçbir tablo yoksa işe yarar. Gerekirse, şemanın başlatılmış olması gerekip gerekmediğini görmek için kendi kontrol etmeniz yazabilir ve şemayı başlatmak için temeldeki ırelationaldatabasecreator hizmetini kullanın.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
