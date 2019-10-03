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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>SQL Server EF Core veritabanı sağlayıcısında bellek için Iyileştirilmiş tablo desteği

[Bellek Için Iyileştirilmiş tablolar](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) , tüm tablonun bellekte bulunduğu SQL Server bir özelliktir. Tablo verilerinin ikinci bir kopyası diskte tutulur, ancak yalnızca dayanıklılık amaçlıdır. Bellek için iyileştirilmiş tablolardaki veriler yalnızca veritabanı kurtarma sırasında diskten okunurdur. Örneğin, bir sunucu yeniden başlatıldıktan sonra.

## <a name="configuring-a-memory-optimized-table"></a>Bellek için iyileştirilmiş bir tablo yapılandırma

Bir varlığın eşlendiği tablonun bellek için iyileştirilmiş olduğunu belirtebilirsiniz. Modelinize dayalı bir veritabanı oluşturmak ve korumak için EF Core kullanılırken (geçişlerle veya `Database.EnsureCreated()`), bu varlıklar için bellek için iyileştirilmiş bir tablo oluşturulacaktır.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
