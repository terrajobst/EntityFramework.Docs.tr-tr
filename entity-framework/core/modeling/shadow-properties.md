---
title: Gölge özellikleri-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 5fdc4c50c295f73d0fa5eef3518adf4d3eb95599
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197709"
---
# <a name="shadow-properties"></a><span data-ttu-id="ab689-102">Gölge Özellikler</span><span class="sxs-lookup"><span data-stu-id="ab689-102">Shadow Properties</span></span>

<span data-ttu-id="ab689-103">Gölge özellikleri, .NET varlık sınıfınızda tanımlanmayan ancak EF Core modelinde bu varlık türü için tanımlanan özelliklerdir.</span><span class="sxs-lookup"><span data-stu-id="ab689-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="ab689-104">Bu özelliklerin değeri ve durumu yalnızca değişiklik Izleyicide korunur.</span><span class="sxs-lookup"><span data-stu-id="ab689-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="ab689-105">Veritabanında, eşlenmiş varlık türlerinde gösterilmemesi gereken veriler olduğunda, gölge özellikleri faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="ab689-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="ab689-106">Bunlar, iki varlık arasındaki ilişkinin veritabanındaki bir yabancı anahtar değeri ile temsil edildiği, ancak ilişki varlık türleri arasında, varlık türleri arasında yönetildiğinde, yabancı anahtar özellikleri için en sık kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ab689-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="ab689-107">Gölge özellik değerleri, `ChangeTracker` API aracılığıyla elde edilebilir ve değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ab689-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="ab689-108">`EF.Property` Statik yöntem aracılığıyla, LINQ sorgularında gölge özelliklerine başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="ab689-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="ab689-109">Kurallar</span><span class="sxs-lookup"><span data-stu-id="ab689-109">Conventions</span></span>

<span data-ttu-id="ab689-110">Bir ilişki keşfedildiğinde, ancak bağımlı varlık sınıfında yabancı anahtar özelliği bulunamadığında, bu kural tarafından gölge özellikleri oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="ab689-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="ab689-111">Bu durumda, bir gölge yabancı anahtar özelliği tanıtılacaktır.</span><span class="sxs-lookup"><span data-stu-id="ab689-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="ab689-112">Gölge yabancı anahtar özelliği adlandırılacak `<navigation property name><principal key property name>` (birincil varlığa işaret eden, adlandırma için kullanılan bağımlı varlık üzerindeki gezinti).</span><span class="sxs-lookup"><span data-stu-id="ab689-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="ab689-113">Asıl anahtar özellik adı Gezinti özelliğinin adını içeriyorsa, ad yalnızca `<principal key property name>`siz olur.</span><span class="sxs-lookup"><span data-stu-id="ab689-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="ab689-114">Bağımlı varlık üzerinde hiçbir gezinti özelliği yoksa, asıl tür adı bunun yerine kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ab689-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="ab689-115">Örneğin, aşağıdaki kod listesi, `BlogId` `Post` varlığa bir gölge özelliğin tanıtılmasıyla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="ab689-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="ab689-116">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="ab689-116">Data Annotations</span></span>

<span data-ttu-id="ab689-117">Gölge özellikleri, veri ek açıklamalarıyla oluşturulamaz.</span><span class="sxs-lookup"><span data-stu-id="ab689-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ab689-118">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="ab689-118">Fluent API</span></span>

<span data-ttu-id="ab689-119">Gölge özelliklerini yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ab689-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="ab689-120">' Nin `Property` dize aşırı yüklemesini çağırdıktan sonra, diğer özellikler için yaptığınız herhangi bir yapılandırma çağrısının herhangi birini zincirleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ab689-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="ab689-121">`Property` Yöntemi için sağlanan ad, var olan bir özelliğin adıyla eşleşiyorsa (bir gölge özellik veya varlık sınıfında tanımlı bir tane), kod, yeni bir gölge özellik tanıtımı yerine mevcut özelliği yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ab689-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
