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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>SQL Server EF Core veritabanı sağlayıcısında bellek için Iyileştirilmiş tablo desteği

[Bellek Için Iyileştirilmiş tablolar](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) , tüm tablonun bellekte bulunduğu SQL Server bir özelliktir. Tablo verilerinin ikinci bir kopyası diskte tutulur, ancak yalnızca dayanıklılık amaçlıdır. Bellek için iyileştirilmiş tablolardaki veriler yalnızca veritabanı kurtarma sırasında diskten okunurdur. Örneğin, bir sunucu yeniden başlatıldıktan sonra.

## <a name="configuring-a-memory-optimized-table"></a>Bellek için iyileştirilmiş bir tablo yapılandırma

Bir varlığın eşlendiği tablonun bellek için iyileştirilmiş olduğunu belirtebilirsiniz. Modelinize dayalı bir veritabanı oluşturmak ve korumak için EF Core kullanırken ( [geçişler](xref:core/managing-schemas/migrations/index) [veya yeniden](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)oluşturma ile), bu varlıklar için bellek için iyileştirilmiş bir tablo oluşturulacaktır.

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
