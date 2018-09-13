---
title: Birden çok sağlayıcı - EF Core ile geçişleri
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
ms.openlocfilehash: 75c055221609679db3f00016b9cb44c6c8c6e473
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488783"
---
<a name="migrations-with-multiple-providers"></a>Birden çok sağlayıcı ile geçişleri
==================================
[EF Core Araçları] [ 1] yalnızca geçişler etkin sağlayıcı için iskele. Bazı durumlarda, ancak birden fazla sağlayıcı (örneğin, Microsoft SQL Server ve SQLite) ile DbContext kullanmak isteyebilirsiniz. Migrations ile bu durumu çözmek için iki yolu vardır. İki koruyabilirsiniz geçişini--her sağlayıcısı--veya birleştirme bunları tek bir kümesi için bir tane hem de çalışabilir.

<a name="two-migration-sets"></a>İki geçiş kümesi
------------------
İçinde bir ilk yaklaşım, her model değişikliği için iki geçiş oluşturur.

Yapmanın bir yolu bu her geçiş kümesi eklemektir [ayrı bir derlemede] [ 2] ve iki geçiş ekleme arasında etkin sağlayıcısı (ve geçişleri derleme) el ile geçiş yapın.

Araçları ile çalışmayı kolaylaştırır başka bir yaklaşım, DbContext türetilir ve etkin sağlayıcı geçersiz kılar. yeni bir tür oluşturmaktır. Bu tür, tasarım sırasında kullanılan saat eklerken veya geçişler uygulanıyor.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Her bir geçiş kümesi kendi DbContext türleri kullandığından, bu yaklaşım ayrı geçişleri derlemeyi kullanarak gerektirmez.

Yeni geçiş eklerken, içerik türlerini belirtin.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> Bir kardeş olarak oluşturulduğundan, sonraki geçişler için çıktı dizini belirtmeniz gerekmez.

<a name="one-migration-set"></a>Bir geçiş kümesi
-----------------
Geçişlerin iki sahip beğenmezseniz el ile bunları her iki sağlayıcıları için uygulanabilir tek bir kümesine birleştirebilirsiniz.

Bir sağlayıcı, anlamıyor herhangi bir ek açıklamaları yok sayar olduğundan, ek açıklamalar bulunabilir. Örneğin, Microsoft SQL Server ve SQLite ile çalışan bir birincil anahtar sütunu aşağıdaki gibi görünebilir.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

İşlem yalnızca bir sağlayıcısında uygulanabilir (veya farklı sağlayıcıları arasında oldukları), kullanın `ActiveProvider` özelliği sağlayıcının etkin olduğunu söylemek için.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
