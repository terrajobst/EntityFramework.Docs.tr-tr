---
title: Birden çok sağlayıcı - EF Core ile geçişleri
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.openlocfilehash: 7ae695037992323337a780cda29d8c8ed8a13458
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997979"
---
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="22dbc-102">Birden çok sağlayıcı ile geçişleri</span><span class="sxs-lookup"><span data-stu-id="22dbc-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="22dbc-103">[EF Core Araçları] [ 1] yalnızca geçişler etkin sağlayıcı için iskele.</span><span class="sxs-lookup"><span data-stu-id="22dbc-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="22dbc-104">Bazı durumlarda, ancak birden fazla sağlayıcı (örneğin, Microsoft SQL Server ve SQLite) ile DbContext kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22dbc-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="22dbc-105">Migrations ile bu durumu çözmek için iki yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="22dbc-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="22dbc-106">İki koruyabilirsiniz geçişini--her sağlayıcısı--veya birleştirme bunları tek bir kümesi için bir tane hem de çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="22dbc-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="22dbc-107">İki geçiş kümesi</span><span class="sxs-lookup"><span data-stu-id="22dbc-107">Two migration sets</span></span>
------------------
<span data-ttu-id="22dbc-108">İçinde bir ilk yaklaşım, her model değişikliği için iki geçiş oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22dbc-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="22dbc-109">Yapmanın bir yolu bu her geçiş kümesi eklemektir [ayrı bir derlemede] [ 2] ve iki geçiş ekleme arasında etkin sağlayıcısı (ve geçişleri derleme) el ile geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="22dbc-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="22dbc-110">Araçları ile çalışmayı kolaylaştırır başka bir yaklaşım, DbContext türetilir ve etkin sağlayıcı geçersiz kılar. yeni bir tür oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="22dbc-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="22dbc-111">Bu tür, tasarım sırasında kullanılan saat eklerken veya geçişler uygulanıyor.</span><span class="sxs-lookup"><span data-stu-id="22dbc-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="22dbc-112">Her bir geçiş kümesi kendi DbContext türleri kullandığından, bu yaklaşım ayrı geçişleri derlemeyi kullanarak gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="22dbc-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="22dbc-113">Yeni geçiş eklerken, içerik türlerini belirtin.</span><span class="sxs-lookup"><span data-stu-id="22dbc-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="22dbc-114">Bir kardeş olarak oluşturulduğundan, sonraki geçişler için çıktı dizini belirtmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="22dbc-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="22dbc-115">Bir geçiş kümesi</span><span class="sxs-lookup"><span data-stu-id="22dbc-115">One migration set</span></span>
-----------------
<span data-ttu-id="22dbc-116">Geçişlerin iki sahip beğenmezseniz el ile bunları her iki sağlayıcıları için uygulanabilir tek bir kümesine birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22dbc-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="22dbc-117">Bir sağlayıcı, anlamıyor herhangi bir ek açıklamaları yok sayar olduğundan, ek açıklamalar bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="22dbc-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="22dbc-118">Örneğin, Microsoft SQL Server ve SQLite ile çalışan bir birincil anahtar sütunu aşağıdaki gibi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="22dbc-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="22dbc-119">İşlem yalnızca bir sağlayıcısında uygulanabilir (veya farklı sağlayıcıları arasında oldukları), kullanın `ActiveProvider` özelliği sağlayıcının etkin olduğunu söylemek için.</span><span class="sxs-lookup"><span data-stu-id="22dbc-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
