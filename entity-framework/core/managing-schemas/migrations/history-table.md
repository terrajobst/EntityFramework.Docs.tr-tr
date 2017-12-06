---
title: "Özel geçişler geçmiş tablosu - EF çekirdek"
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cb9892241f3d7f1fae6293bd60a8a5c3e7120969
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
<a name="custom-migrations-history-table"></a><span data-ttu-id="873bb-102">Özel geçişler geçmiş tablosu</span><span class="sxs-lookup"><span data-stu-id="873bb-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="873bb-103">Varsayılan olarak, EF çekirdek adlı bir tablo kaydederek veritabanına hangi geçiş uygulanmadı izler `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="873bb-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="873bb-104">Çeşitli nedenlerle Bu tablo, gereksinimlerinize daha iyi uyacak şekilde özelleştirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="873bb-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="873bb-105">Geçmiş tablosunun özelleştirirseniz *sonra* geçişler uygulama, veritabanında var olan tablo güncelleştirmek için sorumluluğu size aittir.</span><span class="sxs-lookup"><span data-stu-id="873bb-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="873bb-106">Şema ve tablo adı</span><span class="sxs-lookup"><span data-stu-id="873bb-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="873bb-107">Şema ve tablo adı kullanarak değiştirebilirsiniz `MigrationsHistoryTable()` yönteminde `OnConfiguring()` (veya `ConfigureServices()` ASP.NET Core üzerinde).</span><span class="sxs-lookup"><span data-stu-id="873bb-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="873bb-108">Burada, SQL Server EF çekirdek sağlayıcısını kullanarak bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="873bb-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="873bb-109">Diğer değişiklikler</span><span class="sxs-lookup"><span data-stu-id="873bb-109">Other changes</span></span>
-------------
<span data-ttu-id="873bb-110">Tablonun ek yönlerini yapılandırmak için geçersiz kılın ve sağlayıcıya özgü Değiştir `IHistoryRepository` hizmet.</span><span class="sxs-lookup"><span data-stu-id="873bb-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="873bb-111">MigrationId sütun adını değiştirmenin örneği *kimliği* SQL Server'da.</span><span class="sxs-lookup"><span data-stu-id="873bb-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="873bb-112">`SqlServerHistoryRepository`içinde bir iç ad alanıdır ve gelecek sürümlerde değişebilir.</span><span class="sxs-lookup"><span data-stu-id="873bb-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
