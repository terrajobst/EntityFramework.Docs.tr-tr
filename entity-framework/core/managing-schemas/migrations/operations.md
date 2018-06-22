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
ms.locfileid: "34163148"
---
<a name="custom-migrations-operations"></a><span data-ttu-id="9311b-102">Özel geçişler işlemleri</span><span class="sxs-lookup"><span data-stu-id="9311b-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="9311b-103">MigrationBuilder API işlemleri birçok farklı türde bir geçiş sırasında işlemleri yapmanıza olanak tanır ancak kapsamlı gölgeden uzak olan.</span><span class="sxs-lookup"><span data-stu-id="9311b-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="9311b-104">Ancak, ayrıca kendi işlemleri tanımlamanızı sağlayan genişletilebilir bir API'dir.</span><span class="sxs-lookup"><span data-stu-id="9311b-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="9311b-105">API genişletmek için iki yolu vardır: kullanarak `Sql()` yöntemini veya özel tanımlayarak `MigrationOperation` nesneleri.</span><span class="sxs-lookup"><span data-stu-id="9311b-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="9311b-106">Anlamak için her bir yaklaşım kullanarak bir veritabanı kullanıcısı oluşturan bir işlem uygulanmasına bakalım.</span><span class="sxs-lookup"><span data-stu-id="9311b-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="9311b-107">Bizim geçişlerde, aşağıdaki kod yazmayı etkinleştirmek istiyoruz:</span><span class="sxs-lookup"><span data-stu-id="9311b-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="9311b-108">MigrationBuilder.Sql() kullanma</span><span class="sxs-lookup"><span data-stu-id="9311b-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="9311b-109">Çağıran bir genişletme yöntemi tanımlamak için özel bir işlemi uygulamak için en kolay yolu olan `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="9311b-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="9311b-110">Uygun Transact-SQL oluşturan örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="9311b-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="9311b-111">Geçişler birden çok veritabanı sağlayıcısı desteklemeniz gerekiyorsa, kullanabileceğiniz `MigrationBuilder.ActiveProvider` özelliği.</span><span class="sxs-lookup"><span data-stu-id="9311b-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="9311b-112">Burada, Microsoft SQL Server ve PostgreSQL destekleyen bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="9311b-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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

<span data-ttu-id="9311b-113">Her sağlayıcının biliyorsanız, bu yaklaşım yalnızca özel işleminizi nereye uygulanacağını çalışır.</span><span class="sxs-lookup"><span data-stu-id="9311b-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="9311b-114">Bir MigrationOperation kullanma</span><span class="sxs-lookup"><span data-stu-id="9311b-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="9311b-115">SQL özel işlemi aynı şekilde, kendi tanımlayabilirsiniz `MigrationOperation` onu temsil etmek için.</span><span class="sxs-lookup"><span data-stu-id="9311b-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="9311b-116">Böylece oluşturmak için uygun SQL belirleyebilir işlemi daha sonra sağlayıcıya geçirilir.</span><span class="sxs-lookup"><span data-stu-id="9311b-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="9311b-117">Bu yaklaşımda, genişletme yöntemi yalnızca bu işlemlerden birini eklemek gereken `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="9311b-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="9311b-118">Bu işlem için SQL oluşturmak nasıl bilmek her bir sağlayıcı bu yaklaşımı gerektirir kendi `IMigrationsSqlGenerator` hizmet.</span><span class="sxs-lookup"><span data-stu-id="9311b-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="9311b-119">Yeni işlem işlemek için SQL Server'ın Oluşturucu geçersiz kılma örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="9311b-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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

<span data-ttu-id="9311b-120">Varsayılan geçiş sql Oluşturucu hizmet güncelleştirilmiş adla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9311b-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
