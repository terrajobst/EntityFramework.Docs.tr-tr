---
title: Microsoft SQL Server veritabanı sağlayıcısı - bellek için iyileştirilmiş tablolar - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 63d2cbf8b69e4f1945ad60914e284fb42c48e8db
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995808"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>SQL Server EF Core veritabanı sağlayıcısı bellek için iyileştirilmiş tablolar desteği

> [!NOTE]  
>
> Bu özellik, EF Core 1.1 içinde kullanılmaya başlandı.

[Bellek için iyileştirilmiş tablolar](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) tablonun tamamını bellekte bulunduğu SQL Server'ın bir özelliğidir. Tablo verilerini ikinci bir kopyası, disk, ancak dayanıklılık amacıyla yalnızca korunur. Bellek için iyileştirilmiş tablolardaki verileri veritabanı kurtarma sırasında diskten salt okunur. Örneğin, bir sunucu sonra yeniden başlatın.

## <a name="configuring-a-memory-optimized-table"></a>Bellek için iyileştirilmiş bir tablo yapılandırma

Bir varlığa eşlenmiş tablosu bellek için iyileştirilmiş olduğunu belirtebilirsiniz. Ne zaman EF Core oluşturmak ve bir veritabanını korumak için tabanlı kullanarak modelinize göre (geçişleri ile veya `Database.EnsureCreated()`), bu varlıklar için bellek için iyileştirilmiş bir tablo oluşturulur.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
