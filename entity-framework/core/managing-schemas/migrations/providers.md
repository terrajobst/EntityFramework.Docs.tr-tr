---
title: Birden çok sağlayıcı ile geçişler-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: efe95893f7dbfc8e5c4775e86d58abb32eee3c83
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416789"
---
# <a name="migrations-with-multiple-providers"></a>Birden çok sağlayıcı ile geçişler

[EF Core araçları][1] , etkin sağlayıcı için yalnızca iskele geçişlerini. Ancak bazen, DbContext ile birden fazla sağlayıcı (örneğin Microsoft SQL Server ve SQLite) kullanmak isteyebilirsiniz. Geçişlerle bunu işlemenin iki yolu vardır. Her bir sağlayıcı için bir tane olmak üzere iki geçiş kümesini koruyabilir veya bunları her ikisi üzerinde çalışan tek bir küme halinde birleştirebilirsiniz.

## <a name="two-migration-sets"></a>İki geçiş kümesi

İlk yaklaşımda, her model değişikliği için iki geçiş oluşturursunuz.

Bunu yapmanın bir yolu, her bir geçiş kümesini [ayrı bir derlemede][2] koymaktır ve iki geçiş ekleme arasında etkin sağlayıcıyı (ve geçişler derlemesini) el ile değiştirin.

Araçlarla çalışmayı kolaylaştıran başka bir yaklaşım da, DbContext 'ınızdan türeyen yeni bir tür oluşturmaktır ve etkin sağlayıcıyı geçersiz kılar. Bu tür, geçişler eklenirken veya uygulanırken tasarım zamanında kullanılır.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Her geçiş kümesi kendi DbContext türlerini kullandığından, bu yaklaşım ayrı bir geçiş derlemesi kullanılmasını gerektirmez.

Yeni geçiş eklerken bağlam türlerini belirtin.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

***

> [!TIP]
> Son bir için eşdüzey öğe olarak oluşturuldıklarından sonraki geçişler için çıkış dizinini belirtmeniz gerekmez.

## <a name="one-migration-set"></a>Bir geçiş kümesi

İki geçiş kümesi yoksa, bunları her iki sağlayıcıya da uygulanabilen tek bir küme halinde el ile birleştirebilirsiniz.

Sağlayıcı anladığı tüm ek açıklamaları yoksaydığından, ek açıklamalar birlikte kullanılabilir. Örneğin, hem Microsoft SQL Server hem de SQLite ile birlikte çalışarak birincil anahtar sütunu şöyle görünebilir.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

İşlemler yalnızca bir sağlayıcıya uygulanabileceği (veya sağlayıcılar arasında farklı olan), hangi sağlayıcının etkin olduğunu söylemek için `ActiveProvider` özelliğini kullanın.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
