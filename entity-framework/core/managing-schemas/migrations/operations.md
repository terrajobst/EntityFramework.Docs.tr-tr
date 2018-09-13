---
title: Özel bir geçiş işlemleri - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: dcf11c44dcc9f6008b8290a89dd8c042e5ec5771
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489147"
---
<a name="custom-migrations-operations"></a>Özel bir geçiş işlemleri
============================
MigrationBuilder API geçiş sırasında birçok farklı türde işlemleri yapmanıza olanak tanır, ancak kapsamlı gölgeden uzak olan. Ancak, API da Genişletilebilir kendi işlemleri tanımlamanızı sağlar. API genişletmek için iki yolu vardır: kullanarak `Sql()` yöntemi veya özel tanımlayarak `MigrationOperation` nesneleri.

Göstermek için her bir yaklaşım kullanarak bir veritabanı kullanıcısı oluşturan bir işlem uygulanmasına göz atalım. Bizim geçişlerde, aşağıdaki kod yazma etkinleştirmek istiyoruz:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>MigrationBuilder.Sql() kullanma
----------------------------
Çağıran bir genişletme yöntemi tanımlamak için özel bir işlemi uygulamak için en kolay yolu olan `MigrationBuilder.Sql()`.
Uygun Transact-SQL oluşturan bir örnek aşağıda verilmiştir.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Geçiş birden çok veritabanı sağlayıcısı desteklemeniz gerekiyorsa, kullanabileceğiniz `MigrationBuilder.ActiveProvider` özelliği. Hem Microsoft SQL Server hem de PostgreSQL destekleyen bir örnek aşağıda verilmiştir.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    switch (migrationBuilder.ActiveProvider)
    {
        case "Npgsql.EntityFrameworkCore.PostgreSQL":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD '{password}';");

        case "Microsoft.EntityFrameworkCore.SqlServer":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD = '{password}';");
    }

    return migrationBuilder;
}
```

Her sağlayıcının biliyorsanız, bu yaklaşım yalnızca özel işlemi burada uygulanacak çalışır.

<a name="using-a-migrationoperation"></a>Bir MigrationOperation kullanma
---------------------------
SQL özel işlemden ayırmak için kendi tanımlayabileceğiniz `MigrationOperation` bunu gösterecek. Böylece oluşturmak için uygun SQL belirleyebilir işlemi daha sonra sağlayıcıya geçirilir.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

Bu yaklaşımda, genişletme yöntemi yalnızca bu işlemler için birini eklemek gereken `MigrationBuilder.Operations`.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    migrationBuilder.Operations.Add(
        new CreateUserOperation
        {
            Name = name,
            Password = password
        });

    return migrationBuilder;
}
```

Bu yaklaşım bu işlem için SQL oluşturmayı öğrenmek her bir sağlayıcı gerektirir, `IMigrationsSqlGenerator` hizmeti. Yeni işlemi işlemek için SQL Server'ın Oluşturucu geçersiz kılan bir örnek aşağıda verilmiştir.

``` csharp
class MyMigrationsSqlGenerator : SqlServerMigrationsSqlGenerator
{
    public MyMigrationsSqlGenerator(
        MigrationsSqlGeneratorDependencies dependencies,
        IMigrationsAnnotationProvider migrationsAnnotations)
        : base(dependencies, migrationsAnnotations)
    {
    }

    protected override void Generate(
        MigrationOperation operation,
        IModel model,
        MigrationCommandListBuilder builder)
    {
        if (operation is CreateUserOperation createUserOperation)
        {
            Generate(createUserOperation, builder);
        }
        else
        {
            base.Generate(operation, model, builder);
        }
    }

    private void Generate(
        CreateUserOperation operation,
        MigrationCommandListBuilder builder)
    {
        var sqlHelper = Dependencies.SqlGenerationHelper;
        var stringMapping = Dependencies.TypeMapper.GetMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

Varsayılan geçiş sql Oluşturucu hizmeti güncelleştirilmiş biriyle değiştirin.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
