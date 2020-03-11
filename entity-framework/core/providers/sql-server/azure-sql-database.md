---
title: Microsoft SQL Server veritabanı sağlayıcısı-Azure SQL veritabanı seçenekleri-EF Core
description: Azure SQL veritabanı için hizmet katmanı ve performans düzeyini SQL Server Entity Framework Core veritabanı sağlayıcısıyla belirleme
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/azure-sql-database
ms.openlocfilehash: c4f7b91110a0e700ed06130661e611cf45bee05f
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417899"
---
# <a name="specifying-azure-sql-database-options"></a><span data-ttu-id="9133c-103">Azure SQL Veritabanı Seçeneklerini Belirtme</span><span class="sxs-lookup"><span data-stu-id="9133c-103">Specifying Azure SQL Database Options</span></span>

>[!NOTE]
> <span data-ttu-id="9133c-104">Bu API EF Core 3,1 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="9133c-104">This API is new in EF Core 3.1.</span></span>

<span data-ttu-id="9133c-105">Azure SQL veritabanı, genellikle Azure portalı aracılığıyla yapılandırılmış [çeşitli fiyatlandırma seçenekleri](https://azure.microsoft.com/pricing/details/sql-database/single/) sağlar.</span><span class="sxs-lookup"><span data-stu-id="9133c-105">Azure SQL Database provides [a variety of pricing options](https://azure.microsoft.com/pricing/details/sql-database/single/) that are usually configured through the Azure Portal.</span></span> <span data-ttu-id="9133c-106">Ancak [EF Core geçişleri](xref:core/managing-schemas/migrations/index) kullanarak şemayı yönetiyorsanız, istenen seçenekleri modelin kendisinde belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9133c-106">However if you are managing the schema using [EF Core migrations](xref:core/managing-schemas/migrations/index) you can specify the desired options in the model itself.</span></span>

<span data-ttu-id="9133c-107">[Hasservicetier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier)kullanarak VERITABANıNıN (sürüm) hizmet katmanını belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9133c-107">You can specify the service tier of the database (EDITION) using [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span></span>

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

<span data-ttu-id="9133c-108">[Hasdatabasemaxsize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize)kullanarak veritabanının en büyük boyutunu belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9133c-108">You can specify the maximum size of the database using [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span></span>

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

<span data-ttu-id="9133c-109">[Hasperformancelevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel)kullanarak veritabanının performans düzeyini (SERVICE_OBJECTIVE) belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9133c-109">You can specify the performance level of the database (SERVICE_OBJECTIVE) using [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

<span data-ttu-id="9133c-110">Değer bir dize sabit değeri olmadığından, elastik havuzu yapılandırmak için [Hasperformancelevelsql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) kullanın:</span><span class="sxs-lookup"><span data-stu-id="9133c-110">Use [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) to configure the elastic pool, since the value is not a string literal:</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]


>[!TIP]
> <span data-ttu-id="9133c-111">Desteklenen tüm değerleri [alter database belgelerinde](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current)bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9133c-111">You can find all the supported values in the [ALTER DATABASE documentation](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).</span></span>