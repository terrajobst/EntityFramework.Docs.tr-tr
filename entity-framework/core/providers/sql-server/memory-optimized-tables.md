---
title: Microsoft SQL Server veritabanı sağlayıcısı-bellek için Iyileştirilmiş tablolar-EF Core
description: SQL Server Entity Framework Core veritabanı sağlayıcısıyla bellek için Iyileştirilmiş tabloları kullanma
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: a504fb3487aea6dd36abf204a7427095e3d29118
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824641"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="95467-103">SQL Server EF Core veritabanı sağlayıcısında bellek için Iyileştirilmiş tablo desteği</span><span class="sxs-lookup"><span data-stu-id="95467-103">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="95467-104">[Bellek Için Iyileştirilmiş tablolar](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) , tüm tablonun bellekte bulunduğu SQL Server bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="95467-104">[Memory-Optimized Tables](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="95467-105">Tablo verilerinin ikinci bir kopyası diskte tutulur, ancak yalnızca dayanıklılık amaçlıdır.</span><span class="sxs-lookup"><span data-stu-id="95467-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="95467-106">Bellek için iyileştirilmiş tablolardaki veriler yalnızca veritabanı kurtarma sırasında diskten okunurdur.</span><span class="sxs-lookup"><span data-stu-id="95467-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="95467-107">Örneğin, bir sunucu yeniden başlatıldıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="95467-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="95467-108">Bellek için iyileştirilmiş bir tablo yapılandırma</span><span class="sxs-lookup"><span data-stu-id="95467-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="95467-109">Bir varlığın eşlendiği tablonun bellek için iyileştirilmiş olduğunu belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="95467-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="95467-110">Modelinize dayalı bir veritabanı oluşturmak ve korumak için EF Core kullanırken ( [geçişler](xref:core/managing-schemas/migrations/index) [veya yeniden](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)oluşturma ile), bu varlıklar için bellek için iyileştirilmiş bir tablo oluşturulacaktır.</span><span class="sxs-lookup"><span data-stu-id="95467-110">When using EF Core to create and maintain a database based on your model (either with [migrations](xref:core/managing-schemas/migrations/index) or [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), a memory-optimized table will be created for these entities.</span></span>

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
