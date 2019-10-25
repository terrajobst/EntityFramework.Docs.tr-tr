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
# <a name="custom-migrations-operations"></a><span data-ttu-id="343d1-102">Özel geçiş Işlemleri</span><span class="sxs-lookup"><span data-stu-id="343d1-102">Custom Migrations Operations</span></span>

<span data-ttu-id="343d1-103">MigrationBuilder API 'SI, geçiş sırasında birçok farklı türde işlem gerçekleştirmenize olanak tanır, ancak bu çok yoğun bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="343d1-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="343d1-104">Ancak, API ayrıca kendi işlemlerinizi tanımlamanızı sağlayan genişletilebilir.</span><span class="sxs-lookup"><span data-stu-id="343d1-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="343d1-105">API 'yi genişletmek için iki yol vardır: `Sql()` yöntemini kullanma veya özel `MigrationOperation` nesneleri tanımlama.</span><span class="sxs-lookup"><span data-stu-id="343d1-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="343d1-106">Göstermek için, her yaklaşımı kullanarak bir veritabanı kullanıcısı oluşturan bir işlem uygulamaya bakalım.</span><span class="sxs-lookup"><span data-stu-id="343d1-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="343d1-107">Geçişlerimiz bölümünde aşağıdaki kodun yazılmasını etkinleştirmek istiyoruz:</span><span class="sxs-lookup"><span data-stu-id="343d1-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a><span data-ttu-id="343d1-108">MigrationBuilder. SQL () kullanma</span><span class="sxs-lookup"><span data-stu-id="343d1-108">Using MigrationBuilder.Sql()</span></span>

<span data-ttu-id="343d1-109">Özel bir işlem uygulamanın en kolay yolu, `MigrationBuilder.Sql()`çağıran bir genişletme yöntemi tanımlamaktır.</span><span class="sxs-lookup"><span data-stu-id="343d1-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span> <span data-ttu-id="343d1-110">Uygun Transact-SQL üreten bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="343d1-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="343d1-111">Geçişlerinizin birden çok veritabanı sağlayıcısını desteklemesi gerekiyorsa `MigrationBuilder.ActiveProvider` özelliğini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="343d1-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="343d1-112">Hem Microsoft SQL Server hem de PostgreSQL destekleyen bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="343d1-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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

<span data-ttu-id="343d1-113">Bu yaklaşım yalnızca özel işlemin uygulanacağı her sağlayıcıyı biliyorsanız çalışır.</span><span class="sxs-lookup"><span data-stu-id="343d1-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

## <a name="using-a-migrationoperation"></a><span data-ttu-id="343d1-114">MigrationOperation kullanma</span><span class="sxs-lookup"><span data-stu-id="343d1-114">Using a MigrationOperation</span></span>

<span data-ttu-id="343d1-115">Özel işlemi SQL 'den ayırmak için, bunu temsil etmek üzere kendi `MigrationOperation` tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="343d1-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="343d1-116">Daha sonra işlem sağlayıcıya geçirilir, böylece oluşturulacak uygun SQL 'i tespit edebilir.</span><span class="sxs-lookup"><span data-stu-id="343d1-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="343d1-117">Bu yaklaşımla, Uzantı yönteminin `MigrationBuilder.Operations`için bu işlemlerden birini eklemesi yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="343d1-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="343d1-118">Bu yaklaşım, her sağlayıcının `IMigrationsSqlGenerator` hizmetinde bu işlem için nasıl SQL üreteceğimizi bilmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="343d1-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="343d1-119">İşte, yeni işlemi işlemek için SQL Server üreticisini geçersiz kılan bir örnek.</span><span class="sxs-lookup"><span data-stu-id="343d1-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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

<span data-ttu-id="343d1-120">Varsayılan geçişleri SQL Oluşturucu hizmetini güncelleştirilmiş bir ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="343d1-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
