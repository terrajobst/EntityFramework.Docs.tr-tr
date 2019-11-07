---
title: Özel geçişler geçmiş tablosu-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655712"
---
# <a name="custom-migrations-history-table"></a><span data-ttu-id="c7c3a-102">Özel geçişler geçmiş tablosu</span><span class="sxs-lookup"><span data-stu-id="c7c3a-102">Custom Migrations History Table</span></span>

<span data-ttu-id="c7c3a-103">Varsayılan olarak EF Core, veritabanını `__EFMigrationsHistory`adlı bir tabloya kaydederek hangi geçişlerin uygulanmış olduğunu izler.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="c7c3a-104">Çeşitli nedenlerle bu tabloyu gereksinimlerinize daha uygun olacak şekilde özelleştirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c7c3a-105">Geçişleri uyguladıktan *sonra* geçişleri geçmiş tablosunu özelleştirirseniz, veritabanında var olan tabloyu güncelleştirmekten siz sorumlusunuz.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="c7c3a-106">Şema ve tablo adı</span><span class="sxs-lookup"><span data-stu-id="c7c3a-106">Schema and table name</span></span>

<span data-ttu-id="c7c3a-107">Şema ve tablo adını, `OnConfiguring()` (veya ASP.NET Core üzerinde `ConfigureServices()`) `MigrationsHistoryTable()` yöntemini kullanarak değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="c7c3a-108">SQL Server EF Core sağlayıcısını kullanan bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-108">Here is an example using the SQL Server EF Core provider.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a><span data-ttu-id="c7c3a-109">Diğer değişiklikler</span><span class="sxs-lookup"><span data-stu-id="c7c3a-109">Other changes</span></span>

<span data-ttu-id="c7c3a-110">Tablonun ek yönlerini yapılandırmak için sağlayıcıya özgü `IHistoryRepository` hizmetini geçersiz kılın ve değiştirin.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="c7c3a-111">SQL Server MigrationID sütununun adını *kimlik* olarak değiştirme örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> <span data-ttu-id="c7c3a-112">`SqlServerHistoryRepository` iç ad alanı içindedir ve gelecek sürümlerde değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
