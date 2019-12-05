---
title: Microsoft SQL Server veritabanı sağlayıcısı-Azure SQL veritabanı seçenekleri-EF Core
description: Azure SQL veritabanı için hizmet katmanı ve performans düzeyini SQL Server Entity Framework Core veritabanı sağlayıcısıyla belirleme
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/azure-sql-database
ms.openlocfilehash: c4f7b91110a0e700ed06130661e611cf45bee05f
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824794"
---
# <a name="specifying-azure-sql-database-options"></a>Azure SQL veritabanı seçeneklerini belirtme

>[!NOTE]
> Bu API EF Core 3,1 ' de yenidir.

Azure SQL veritabanı, genellikle Azure portalı aracılığıyla yapılandırılmış [çeşitli fiyatlandırma seçenekleri](https://azure.microsoft.com/pricing/details/sql-database/single/) sağlar. Ancak [EF Core geçişleri](xref:core/managing-schemas/migrations/index) kullanarak şemayı yönetiyorsanız, istenen seçenekleri modelin kendisinde belirtebilirsiniz.

[Hasservicetier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier)kullanarak VERITABANıNıN (sürüm) hizmet katmanını belirtebilirsiniz:

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

[Hasdatabasemaxsize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize)kullanarak veritabanının en büyük boyutunu belirtebilirsiniz:

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

[Hasperformancelevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel)kullanarak veritabanının performans düzeyini (SERVICE_OBJECTIVE) belirtebilirsiniz:

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

Değer bir dize sabit değeri olmadığından, elastik havuzu yapılandırmak için [Hasperformancelevelsql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) kullanın:

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]


>[!TIP]
> Desteklenen tüm değerleri [alter database belgelerinde](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current)bulabilirsiniz.