---
title: Özel bir geçiş işlemleri - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 510d585534b4809179c905ee5b77cab4209a2b8f
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614291"
---
<a name="custom-migrations-operations"></a><span data-ttu-id="ebfac-102">Özel bir geçiş işlemleri</span><span class="sxs-lookup"><span data-stu-id="ebfac-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="ebfac-103">MigrationBuilder API geçiş sırasında birçok farklı türde işlemleri yapmanıza olanak tanır, ancak kapsamlı gölgeden uzak olan.</span><span class="sxs-lookup"><span data-stu-id="ebfac-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="ebfac-104">Ancak, API da Genişletilebilir kendi işlemleri tanımlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ebfac-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="ebfac-105">API genişletmek için iki yolu vardır: kullanarak `Sql()` yöntemi veya özel tanımlayarak `MigrationOperation` nesneleri.</span><span class="sxs-lookup"><span data-stu-id="ebfac-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="ebfac-106">Göstermek için her bir yaklaşım kullanarak bir veritabanı kullanıcısı oluşturan bir işlem uygulanmasına göz atalım.</span><span class="sxs-lookup"><span data-stu-id="ebfac-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="ebfac-107">Bizim geçişlerde, aşağıdaki kod yazma etkinleştirmek istiyoruz:</span><span class="sxs-lookup"><span data-stu-id="ebfac-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="ebfac-108">MigrationBuilder.Sql() kullanma</span><span class="sxs-lookup"><span data-stu-id="ebfac-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="ebfac-109">Çağıran bir genişletme yöntemi tanımlamak için özel bir işlemi uygulamak için en kolay yolu olan `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="ebfac-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="ebfac-110">Uygun Transact-SQL oluşturan bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ebfac-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="ebfac-111">Geçiş birden çok veritabanı sağlayıcısı desteklemeniz gerekiyorsa, kullanabileceğiniz `MigrationBuilder.ActiveProvider` özelliği.</span><span class="sxs-lookup"><span data-stu-id="ebfac-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="ebfac-112">Hem Microsoft SQL Server hem de PostgreSQL destekleyen bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ebfac-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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

<span data-ttu-id="ebfac-113">Her sağlayıcının biliyorsanız, bu yaklaşım yalnızca özel işlemi burada uygulanacak çalışır.</span><span class="sxs-lookup"><span data-stu-id="ebfac-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="ebfac-114">Bir MigrationOperation kullanma</span><span class="sxs-lookup"><span data-stu-id="ebfac-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="ebfac-115">SQL özel işlemden ayırmak için kendi tanımlayabileceğiniz `MigrationOperation` bunu gösterecek.</span><span class="sxs-lookup"><span data-stu-id="ebfac-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="ebfac-116">Böylece oluşturmak için uygun SQL belirleyebilir işlemi daha sonra sağlayıcıya geçirilir.</span><span class="sxs-lookup"><span data-stu-id="ebfac-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="ebfac-117">Bu yaklaşımda, genişletme yöntemi yalnızca bu işlemler için birini eklemek gereken `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="ebfac-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="ebfac-118">Bu yaklaşım bu işlem için SQL oluşturmayı öğrenmek her bir sağlayıcı gerektirir, `IMigrationsSqlGenerator` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="ebfac-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="ebfac-119">Yeni işlemi işlemek için SQL Server'ın Oluşturucu geçersiz kılan bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ebfac-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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

<span data-ttu-id="ebfac-120">Varsayılan geçiş sql Oluşturucu hizmeti güncelleştirilmiş biriyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ebfac-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
