---
title: Microsoft SQL Server veritabanı sağlayıcısı - bellek için iyileştirilmiş tablolar - EF çekirdek
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
ms.locfileid: "26054147"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="59f18-102">SQL Server EF çekirdek veritabanı sağlayıcısı bellek için iyileştirilmiş tablolar desteği</span><span class="sxs-lookup"><span data-stu-id="59f18-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="59f18-103">Bu özellik EF çekirdek 1.1 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="59f18-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="59f18-104">[Bellek için iyileştirilmiş tablolar](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) tüm tablo bellekte bulunduğu SQL Server'ın bir özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="59f18-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="59f18-105">Tablo verisi ikinci bir kopyası diskte, ancak yalnızca dayanıklılık amacıyla korunur.</span><span class="sxs-lookup"><span data-stu-id="59f18-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="59f18-106">Bellek için iyileştirilmiş tablolardaki verileri diskten veritabanı kurtarma sırasında salt okunur.</span><span class="sxs-lookup"><span data-stu-id="59f18-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="59f18-107">Örneğin, sonra bir sunucuyu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="59f18-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="59f18-108">Bellek için iyileştirilmiş tablo yapılandırma</span><span class="sxs-lookup"><span data-stu-id="59f18-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="59f18-109">Bir varlık eşlenmiş tablosu bellek için iyileştirilmiş olduğunu belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="59f18-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="59f18-110">Ne zaman EF çekirdek bir veritabanını oluşturmak ve korumak için tabanlı kullanarak modelinize göre (geçişler biriyle veya `Database.EnsureCreated()`), bir bellek için iyileştirilmiş tablo bu varlıklar için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="59f18-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
