---
title: "Microsoft SQL Server veritabanı sağlayıcısı - bellek için iyileştirilmiş tablolar - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 83a0decffc5946d036903b8b8add59f0ea31b21f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>SQL Server EF çekirdek veritabanı sağlayıcısı bellek için iyileştirilmiş tablolar desteği

> [!NOTE]  
>
> Bu özellik EF çekirdek 1.1 sunulmuştur.

[Bellek için iyileştirilmiş tablolar](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) tüm tablo bellekte bulunduğu SQL Server'ın bir özelliğidir. Tablo verisi ikinci bir kopyası diskte, ancak yalnızca dayanıklılık amacıyla korunur. Bellek için iyileştirilmiş tablolardaki verileri diskten veritabanı kurtarma sırasında salt okunur. Örneğin, sonra bir sunucuyu yeniden başlatın.

## <a name="configuring-a-memory-optimized-table"></a>Bellek için iyileştirilmiş tablo yapılandırma

Bir varlık eşlenmiş tablosu bellek için iyileştirilmiş olduğunu belirtebilirsiniz. Ne zaman EF çekirdek bir veritabanını oluşturmak ve korumak için tabanlı kullanarak modelinize göre (geçişler biriyle veya `Database.EnsureCreated()`), bir bellek için iyileştirilmiş tablo bu varlıklar için oluşturulur.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
