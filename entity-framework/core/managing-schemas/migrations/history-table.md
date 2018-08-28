---
title: Özel geçişleri geçmiş tablosu - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.openlocfilehash: 7ee76cadd6fac4ec403918e88460e43067ae5815
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995702"
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
