---
title: Özel geçişler geçmiş tablosu-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0db393ff3101564f8d8081d0a57b264c2c459df7
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812076"
---
# <a name="custom-migrations-history-table"></a><span data-ttu-id="05fb1-102">Özel geçişler geçmiş tablosu</span><span class="sxs-lookup"><span data-stu-id="05fb1-102">Custom Migrations History Table</span></span>

<span data-ttu-id="05fb1-103">Varsayılan olarak EF Core, veritabanını `__EFMigrationsHistory`adlı bir tabloya kaydederek hangi geçişlerin uygulanmış olduğunu izler.</span><span class="sxs-lookup"><span data-stu-id="05fb1-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="05fb1-104">Çeşitli nedenlerle bu tabloyu gereksinimlerinize daha uygun olacak şekilde özelleştirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="05fb1-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="05fb1-105">Geçişleri uyguladıktan *sonra* geçişleri geçmiş tablosunu özelleştirirseniz, veritabanında var olan tabloyu güncelleştirmekten siz sorumlusunuz.</span><span class="sxs-lookup"><span data-stu-id="05fb1-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="05fb1-106">Şema ve tablo adı</span><span class="sxs-lookup"><span data-stu-id="05fb1-106">Schema and table name</span></span>

<span data-ttu-id="05fb1-107">Şema ve tablo adını, `OnConfiguring()` (veya ASP.NET Core üzerinde `ConfigureServices()`) `MigrationsHistoryTable()` yöntemini kullanarak değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="05fb1-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="05fb1-108">SQL Server EF Core sağlayıcısını kullanan bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="05fb1-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

## <a name="other-changes"></a><span data-ttu-id="05fb1-109">Diğer değişiklikler</span><span class="sxs-lookup"><span data-stu-id="05fb1-109">Other changes</span></span>

<span data-ttu-id="05fb1-110">Tablonun ek yönlerini yapılandırmak için sağlayıcıya özgü `IHistoryRepository` hizmetini geçersiz kılın ve değiştirin.</span><span class="sxs-lookup"><span data-stu-id="05fb1-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="05fb1-111">SQL Server MigrationID sütununun adını *kimlik* olarak değiştirme örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="05fb1-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="05fb1-112">`SqlServerHistoryRepository` iç ad alanı içindedir ve gelecek sürümlerde değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="05fb1-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
