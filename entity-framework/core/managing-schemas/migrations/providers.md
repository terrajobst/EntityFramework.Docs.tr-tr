---
title: Birden çok sağlayıcı ile geçişler-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: c764a7c7858dc46a5e478f79476f55a34e0fdabd
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655584"
---
# <a name="migrations-with-multiple-providers"></a><span data-ttu-id="6ef22-102">Birden çok sağlayıcı ile geçişler</span><span class="sxs-lookup"><span data-stu-id="6ef22-102">Migrations with Multiple Providers</span></span>

<span data-ttu-id="6ef22-103">[EF Core araçları][1] , etkin sağlayıcı için yalnızca iskele geçişlerini.</span><span class="sxs-lookup"><span data-stu-id="6ef22-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="6ef22-104">Ancak bazen, DbContext ile birden fazla sağlayıcı (örneğin Microsoft SQL Server ve SQLite) kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6ef22-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="6ef22-105">Geçişlerle bunu işlemenin iki yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="6ef22-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="6ef22-106">Her bir sağlayıcı için bir tane olmak üzere iki geçiş kümesini koruyabilir veya bunları her ikisi üzerinde çalışan tek bir küme halinde birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6ef22-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

## <a name="two-migration-sets"></a><span data-ttu-id="6ef22-107">İki geçiş kümesi</span><span class="sxs-lookup"><span data-stu-id="6ef22-107">Two migration sets</span></span>

<span data-ttu-id="6ef22-108">İlk yaklaşımda, her model değişikliği için iki geçiş oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="6ef22-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="6ef22-109">Bunu yapmanın bir yolu, her bir geçiş kümesini [ayrı bir derlemede][2] koymaktır ve iki geçiş ekleme arasında etkin sağlayıcıyı (ve geçişler derlemesini) el ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6ef22-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="6ef22-110">Araçlarla çalışmayı kolaylaştıran başka bir yaklaşım da, DbContext 'ınızdan türeyen yeni bir tür oluşturmaktır ve etkin sağlayıcıyı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="6ef22-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="6ef22-111">Bu tür, geçişler eklenirken veya uygulanırken tasarım zamanında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6ef22-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="6ef22-112">Her geçiş kümesi kendi DbContext türlerini kullandığından, bu yaklaşım ayrı bir geçiş derlemesi kullanılmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="6ef22-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="6ef22-113">Yeni geçiş eklerken bağlam türlerini belirtin.</span><span class="sxs-lookup"><span data-stu-id="6ef22-113">When adding new migration, specify the context types.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="6ef22-114">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6ef22-114">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="6ef22-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ef22-115">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

***

> [!TIP]
> <span data-ttu-id="6ef22-116">Son bir için eşdüzey öğe olarak oluşturuldıklarından sonraki geçişler için çıkış dizinini belirtmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="6ef22-116">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

## <a name="one-migration-set"></a><span data-ttu-id="6ef22-117">Bir geçiş kümesi</span><span class="sxs-lookup"><span data-stu-id="6ef22-117">One migration set</span></span>

<span data-ttu-id="6ef22-118">İki geçiş kümesi yoksa, bunları her iki sağlayıcıya da uygulanabilen tek bir küme halinde el ile birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6ef22-118">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="6ef22-119">Sağlayıcı anladığı tüm ek açıklamaları yoksaydığından, ek açıklamalar birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6ef22-119">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="6ef22-120">Örneğin, hem Microsoft SQL Server hem de SQLite ile birlikte çalışarak birincil anahtar sütunu şöyle görünebilir.</span><span class="sxs-lookup"><span data-stu-id="6ef22-120">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="6ef22-121">İşlemler yalnızca bir sağlayıcıya uygulanabileceği (veya sağlayıcılar arasında farklı olan), hangi sağlayıcının etkin olduğunu söylemek için `ActiveProvider` özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="6ef22-121">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
