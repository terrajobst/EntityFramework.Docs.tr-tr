---
title: Özel geçişler işlemleri - EF çekirdek
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 84d80175e719c950844b13688e1a4992614f25d8
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
<a name="custom-migrations-operations"></a>Özel geçişler işlemleri
============================
MigrationBuilder API işlemleri birçok farklı türde bir geçiş sırasında işlemleri yapmanıza olanak tanır ancak kapsamlı gölgeden uzak olan. Ancak, ayrıca kendi işlemleri tanımlamanızı sağlayan genişletilebilir bir API'dir. API genişletmek için iki yolu vardır: kullanarak `Sql()` yöntemini veya özel tanımlayarak `MigrationOperation` nesneleri.

Anlamak için her bir yaklaşım kullanarak bir veritabanı kullanıcısı oluşturan bir işlem uygulanmasına bakalım. Bizim geçişlerde, aşağıdaki kod yazmayı etkinleştirmek istiyoruz:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>MigrationBuilder.Sql() kullanma
----------------------------
Çağıran bir genişletme yöntemi tanımlamak için özel bir işlemi uygulamak için en kolay yolu olan `MigrationBuilder.Sql()`.
Uygun Transact-SQL oluşturan örnek aşağıda verilmiştir.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Geçişler birden çok veritabanı sağlayıcısı desteklemeniz gerekiyorsa, kullanabileceğiniz `MigrationBuilder.ActiveProvider` özelliği. Burada, Microsoft SQL Server ve PostgreSQL destekleyen bir örnek verilmiştir.

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
}
```

Her sağlayıcının biliyorsanız, bu yaklaşım yalnızca özel işleminizi nereye uygulanacağını çalışır.

<a name="using-a-migrationoperation"></a>Bir MigrationOperation kullanma
---------------------------
SQL özel işlemi aynı şekilde, kendi tanımlayabilirsiniz `MigrationOperation` onu temsil etmek için. Böylece oluşturmak için uygun SQL belirleyebilir işlemi daha sonra sağlayıcıya geçirilir.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

Bu yaklaşımda, genişletme yöntemi yalnızca bu işlemlerden birini eklemek gereken `MigrationBuilder.Operations`.

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

Bu işlem için SQL oluşturmak nasıl bilmek her bir sağlayıcı bu yaklaşımı gerektirir kendi `IMigrationsSqlGenerator` hizmet. Yeni işlem işlemek için SQL Server'ın Oluşturucu geçersiz kılma örnek aşağıda verilmiştir.

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

Varsayılan geçiş sql Oluşturucu hizmet güncelleştirilmiş adla değiştirin.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
