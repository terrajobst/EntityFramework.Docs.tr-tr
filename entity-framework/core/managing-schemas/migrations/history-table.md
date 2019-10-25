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
# <a name="custom-migrations-history-table"></a>Özel geçişler geçmiş tablosu

Varsayılan olarak EF Core, veritabanını `__EFMigrationsHistory`adlı bir tabloya kaydederek hangi geçişlerin uygulanmış olduğunu izler. Çeşitli nedenlerle bu tabloyu gereksinimlerinize daha uygun olacak şekilde özelleştirmek isteyebilirsiniz.

> [!IMPORTANT]
> Geçişleri uyguladıktan *sonra* geçişleri geçmiş tablosunu özelleştirirseniz, veritabanında var olan tabloyu güncelleştirmekten siz sorumlusunuz.

## <a name="schema-and-table-name"></a>Şema ve tablo adı

Şema ve tablo adını, `OnConfiguring()` (veya ASP.NET Core üzerinde `ConfigureServices()`) `MigrationsHistoryTable()` yöntemini kullanarak değiştirebilirsiniz. SQL Server EF Core sağlayıcısını kullanan bir örnek aşağıda verilmiştir.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

## <a name="other-changes"></a>Diğer değişiklikler

Tablonun ek yönlerini yapılandırmak için sağlayıcıya özgü `IHistoryRepository` hizmetini geçersiz kılın ve değiştirin. SQL Server MigrationID sütununun adını *kimlik* olarak değiştirme örneği aşağıda verilmiştir.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` iç ad alanı içindedir ve gelecek sürümlerde değiştirilebilir.

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
