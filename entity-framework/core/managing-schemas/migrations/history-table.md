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
<a name="custom-migrations-history-table"></a>Özel geçişler geçmiş tablosu
===============================
Varsayılan olarak, EF çekirdek adlı bir tablo kaydederek veritabanına hangi geçiş uygulanmadı izler `__EFMigrationsHistory`. Çeşitli nedenlerle Bu tablo, gereksinimlerinize daha iyi uyacak şekilde özelleştirmek isteyebilirsiniz.

> [!IMPORTANT]
> Geçmiş tablosunun özelleştirirseniz *sonra* geçişler uygulama, veritabanında var olan tablo güncelleştirmek için sorumluluğu size aittir.

<a name="schema-and-table-name"></a>Şema ve tablo adı
----------------------
Şema ve tablo adı kullanarak değiştirebilirsiniz `MigrationsHistoryTable()` yönteminde `OnConfiguring()` (veya `ConfigureServices()` ASP.NET Core üzerinde). Burada, SQL Server EF çekirdek sağlayıcısını kullanarak bir örnek verilmiştir.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Diğer değişiklikler
-------------
Tablonun ek yönlerini yapılandırmak için geçersiz kılın ve sağlayıcıya özgü Değiştir `IHistoryRepository` hizmet. MigrationId sütun adını değiştirmenin örneği *kimliği* SQL Server'da.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository`içinde bir iç ad alanıdır ve gelecek sürümlerde değişebilir.

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
