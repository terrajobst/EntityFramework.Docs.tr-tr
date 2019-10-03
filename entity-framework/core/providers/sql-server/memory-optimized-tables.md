---
title: Microsoft SQL Server veritabanı sağlayıcısı-bellek için Iyileştirilmiş tablolar-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 7383e74b3f83172f9b8e0eaf9bd09d4e187e87f8
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813491"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="ce86b-102">SQL Server EF Core veritabanı sağlayıcısında bellek için Iyileştirilmiş tablo desteği</span><span class="sxs-lookup"><span data-stu-id="ce86b-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="ce86b-103">[Bellek Için Iyileştirilmiş tablolar](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) , tüm tablonun bellekte bulunduğu SQL Server bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="ce86b-103">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="ce86b-104">Tablo verilerinin ikinci bir kopyası diskte tutulur, ancak yalnızca dayanıklılık amaçlıdır.</span><span class="sxs-lookup"><span data-stu-id="ce86b-104">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="ce86b-105">Bellek için iyileştirilmiş tablolardaki veriler yalnızca veritabanı kurtarma sırasında diskten okunurdur.</span><span class="sxs-lookup"><span data-stu-id="ce86b-105">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="ce86b-106">Örneğin, bir sunucu yeniden başlatıldıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="ce86b-106">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="ce86b-107">Bellek için iyileştirilmiş bir tablo yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ce86b-107">Configuring a memory-optimized table</span></span>

<span data-ttu-id="ce86b-108">Bir varlığın eşlendiği tablonun bellek için iyileştirilmiş olduğunu belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ce86b-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="ce86b-109">Modelinize dayalı bir veritabanı oluşturmak ve korumak için EF Core kullanılırken (geçişlerle veya `Database.EnsureCreated()`), bu varlıklar için bellek için iyileştirilmiş bir tablo oluşturulacaktır.</span><span class="sxs-lookup"><span data-stu-id="ce86b-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
