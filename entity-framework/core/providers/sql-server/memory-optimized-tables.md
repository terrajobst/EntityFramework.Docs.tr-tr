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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="7d8bc-102">SQL Server EF Core veritabanı sağlayıcısı bellek için iyileştirilmiş tablolar desteği</span><span class="sxs-lookup"><span data-stu-id="7d8bc-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="7d8bc-103">Bu özellik, EF Core 1.1 içinde kullanılmaya başlandı.</span><span class="sxs-lookup"><span data-stu-id="7d8bc-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="7d8bc-104">[Bellek için iyileştirilmiş tablolar](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) tablonun tamamını bellekte bulunduğu SQL Server'ın bir özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="7d8bc-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="7d8bc-105">Tablo verilerini ikinci bir kopyası, disk, ancak dayanıklılık amacıyla yalnızca korunur.</span><span class="sxs-lookup"><span data-stu-id="7d8bc-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="7d8bc-106">Bellek için iyileştirilmiş tablolardaki verileri veritabanı kurtarma sırasında diskten salt okunur.</span><span class="sxs-lookup"><span data-stu-id="7d8bc-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="7d8bc-107">Örneğin, bir sunucu sonra yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="7d8bc-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="7d8bc-108">Bellek için iyileştirilmiş bir tablo yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7d8bc-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="7d8bc-109">Bir varlığa eşlenmiş tablosu bellek için iyileştirilmiş olduğunu belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7d8bc-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="7d8bc-110">Ne zaman EF Core oluşturmak ve bir veritabanını korumak için tabanlı kullanarak modelinize göre (geçişleri ile veya `Database.EnsureCreated()`), bu varlıklar için bellek için iyileştirilmiş bir tablo oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7d8bc-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
