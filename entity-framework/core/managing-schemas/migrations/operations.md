---
title: Özel geçiş Işlemleri-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: bd2bfdc24977a47eaf7a6756a88b758b563d818a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812050"
---
# <a name="custom-migrations-operations"></a>Özel geçiş Işlemleri

MigrationBuilder API 'SI, geçiş sırasında birçok farklı türde işlem gerçekleştirmenize olanak tanır, ancak bu çok yoğun bir şekilde yapılır. Ancak, API ayrıca kendi işlemlerinizi tanımlamanızı sağlayan genişletilebilir. API 'yi genişletmek için iki yol vardır: `Sql()` yöntemini kullanma veya özel `MigrationOperation` nesneleri tanımlama.

Göstermek için, her yaklaşımı kullanarak bir veritabanı kullanıcısı oluşturan bir işlem uygulamaya bakalım. Geçişlerimiz bölümünde aşağıdaki kodun yazılmasını etkinleştirmek istiyoruz:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a>MigrationBuilder. SQL () kullanma

Özel bir işlem uygulamanın en kolay yolu, `MigrationBuilder.Sql()`çağıran bir genişletme yöntemi tanımlamaktır. Uygun Transact-SQL üreten bir örnek aşağıda verilmiştir.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Geçişlerinizin birden çok veritabanı sağlayıcısını desteklemesi gerekiyorsa `MigrationBuilder.ActiveProvider` özelliğini kullanabilirsiniz. Hem Microsoft SQL Server hem de PostgreSQL destekleyen bir örnek aşağıda verilmiştir.

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

Bu yaklaşım yalnızca özel işlemin uygulanacağı her sağlayıcıyı biliyorsanız çalışır.

## <a name="using-a-migrationoperation"></a>MigrationOperation kullanma

Özel işlemi SQL 'den ayırmak için, bunu temsil etmek üzere kendi `MigrationOperation` tanımlayabilirsiniz. Daha sonra işlem sağlayıcıya geçirilir, böylece oluşturulacak uygun SQL 'i tespit edebilir.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

Bu yaklaşımla, Uzantı yönteminin `MigrationBuilder.Operations`için bu işlemlerden birini eklemesi yeterlidir.

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

Bu yaklaşım, her sağlayıcının `IMigrationsSqlGenerator` hizmetinde bu işlem için nasıl SQL üreteceğimizi bilmesini gerektirir. İşte, yeni işlemi işlemek için SQL Server üreticisini geçersiz kılan bir örnek.

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
        var stringMapping = Dependencies.TypeMappingSource.FindMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(operation.Name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(operation.Password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

Varsayılan geçişleri SQL Oluşturucu hizmetini güncelleştirilmiş bir ile değiştirin.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
