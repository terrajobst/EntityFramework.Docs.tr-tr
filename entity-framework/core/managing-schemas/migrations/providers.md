---
title: "Birden çok sağlayıcı - EF çekirdek ile geçişleri"
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d950e74ed4cef7d4274aabcf3eda7b0b735574c6
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2018
---
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="da463-102">Birden çok sağlayıcı ile geçişleri</span><span class="sxs-lookup"><span data-stu-id="da463-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="da463-103">[EF çekirdek Araçları] [ 1] yalnızca geçişler etkin sağlayıcı için iskele.</span><span class="sxs-lookup"><span data-stu-id="da463-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="da463-104">Bazı durumlarda, ancak, birden fazla sağlayıcı (örneğin, Microsoft SQL Server ve SQLite) ile sizin DbContext kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da463-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="da463-105">Bu geçiş ile işlemek için iki yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="da463-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="da463-106">İki bulundurabilirsiniz geçirilmesi--her sağlayıcısı--veya birleştirme bunları tek bir ayarlamak için bir tane hem de çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="da463-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="da463-107">İki geçiş kümeleri</span><span class="sxs-lookup"><span data-stu-id="da463-107">Two migration sets</span></span>
------------------
<span data-ttu-id="da463-108">İlk yaklaşım, her bir model değişikliği için iki geçiş oluşturur.</span><span class="sxs-lookup"><span data-stu-id="da463-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="da463-109">Yapmanın bir yolu her bir geçiş kümesi yerleştirilecek budur [ayrı bir derleme içindeki] [ 2] ve iki geçiş ekleme arasında etkin sağlayıcısı (ve geçişler derleme) el ile geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="da463-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="da463-110">Araçlar ile çalışmayı kolaylaştırır başka bir yaklaşım active sağlayıcısı geçersiz kılar ve DbContext öğesinden türetilen yeni bir tür oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="da463-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="da463-111">Bu tür tasarım sırasında kullanılan zaman eklerken veya geçişler uygulanıyor.</span><span class="sxs-lookup"><span data-stu-id="da463-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="da463-112">Her bir geçiş kümesi kendi DbContext türleri kullandığından, bu yaklaşım ayrı geçişler derlemeyi kullanarak gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="da463-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="da463-113">Yeni geçiş eklenirken içerik türlerini belirtin.</span><span class="sxs-lookup"><span data-stu-id="da463-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="da463-114">Son eşdüzey olarak oluşturulduğundan sonraki geçişler için çıktı dizini belirtmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="da463-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="da463-115">Bir geçiş ayarlama</span><span class="sxs-lookup"><span data-stu-id="da463-115">One migration set</span></span>
-----------------
<span data-ttu-id="da463-116">Geçişler iki kümesi sahip hoşlanmıyorsanız, el ile bunları hem sağlayıcılarına uygulanabilir tek bir kümesine birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da463-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="da463-117">Bir sağlayıcı, anlamıyor tüm ek açıklamaları yok sayar beri ek açıklamaları bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="da463-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="da463-118">Örneğin, hem Microsoft SQL Server hem de SQLite ile çalışır birincil bir anahtar sütunu şöyle görünebilir.</span><span class="sxs-lookup"><span data-stu-id="da463-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="da463-119">İşlemleri yalnızca bir sağlayıcısında uygulanabilir (veya farklı sağlayıcılar arasında oldukları varsa) kullanmak `ActiveProvider` hangi sağlayıcısı etkin olup özelliği.</span><span class="sxs-lookup"><span data-stu-id="da463-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
