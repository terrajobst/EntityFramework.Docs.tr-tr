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
<a name="custom-migrations-history-table"></a>Özel geçişleri geçmiş tablosu
===============================
Varsayılan olarak EF Core adlı bir tabloda kaydederek veritabanına hangi geçişleri uygulanmış izler `__EFMigrationsHistory`. Çeşitli nedenlerden dolayı bu tablo, gereksinimlerinize daha iyi uyacak şekilde özelleştirmek isteyebilirsiniz.

> [!IMPORTANT]
> Geçişleri geçmiş tablosu özelleştirirseniz *sonra* geçişler uygulama, var olan tablo veritabanında güncelleştirmek için sorumluluğu size aittir.

<a name="schema-and-table-name"></a>Şema ve tablo adı
----------------------
Şema ve tablo adıyla değiştirebilirsiniz `MigrationsHistoryTable()` yönteminde `OnConfiguring()` (veya `ConfigureServices()` ASP.NET Core üzerinde). SQL Server EF Core Sağlayıcısı'nı kullanarak bir örnek aşağıda verilmiştir.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Diğer değişiklikler
-------------
Tablo ek yönlerini yapılandırmak için geçersiz kılın ve sağlayıcıya özgü değiştirin `IHistoryRepository` hizmeti. İşte bir örnek MigrationId sütunun adı değiştirme *kimliği* SQL Server'da.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` içinde bir iç ad alanı ve gelecek sürümlerde değişebilir.

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
