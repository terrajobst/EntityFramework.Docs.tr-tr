---
title: Özel geçişleri geçmiş tablosu - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: 1a253972a8f4e410421ec8a77c079e588d368819
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488822"
---
<a name="custom-migrations-history-table"></a><span data-ttu-id="5d3ff-102">Özel geçişleri geçmiş tablosu</span><span class="sxs-lookup"><span data-stu-id="5d3ff-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="5d3ff-103">Varsayılan olarak EF Core adlı bir tabloda kaydederek veritabanına hangi geçişleri uygulanmış izler `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="5d3ff-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="5d3ff-104">Çeşitli nedenlerden dolayı bu tablo, gereksinimlerinize daha iyi uyacak şekilde özelleştirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5d3ff-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5d3ff-105">Geçişleri geçmiş tablosu özelleştirirseniz *sonra* geçişler uygulama, var olan tablo veritabanında güncelleştirmek için sorumluluğu size aittir.</span><span class="sxs-lookup"><span data-stu-id="5d3ff-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="5d3ff-106">Şema ve tablo adı</span><span class="sxs-lookup"><span data-stu-id="5d3ff-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="5d3ff-107">Şema ve tablo adıyla değiştirebilirsiniz `MigrationsHistoryTable()` yönteminde `OnConfiguring()` (veya `ConfigureServices()` ASP.NET Core üzerinde).</span><span class="sxs-lookup"><span data-stu-id="5d3ff-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="5d3ff-108">SQL Server EF Core Sağlayıcısı'nı kullanarak bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5d3ff-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="5d3ff-109">Diğer değişiklikler</span><span class="sxs-lookup"><span data-stu-id="5d3ff-109">Other changes</span></span>
-------------
<span data-ttu-id="5d3ff-110">Tablo ek yönlerini yapılandırmak için geçersiz kılın ve sağlayıcıya özgü değiştirin `IHistoryRepository` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="5d3ff-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="5d3ff-111">İşte bir örnek MigrationId sütunun adı değiştirme *kimliği* SQL Server'da.</span><span class="sxs-lookup"><span data-stu-id="5d3ff-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="5d3ff-112">`SqlServerHistoryRepository` içinde bir iç ad alanı ve gelecek sürümlerde değişebilir.</span><span class="sxs-lookup"><span data-stu-id="5d3ff-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

``` csharp
class MyHistoryRepository : SqlServerHistoryRepository
{
    public MyHistoryRepository(HistoryRepositoryDependencies dependencies)
        : base(dependencies)
    {
    }

    protected override void ConfigureTable(EntityTypeBuilder<HistoryRow> history)
    {
        base.ConfigureTable(history);

        history.Property(h => h.MigrationId).HasColumnName("Id");
    }
}
```
