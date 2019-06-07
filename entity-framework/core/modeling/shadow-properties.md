---
title: Gölge özellikler - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 4029539f3642f539a427f5901577d4df96c00f30
ms.sourcegitcommit: 119058fefd7f35952048f783ada68be9aa612256
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749698"
---
# <a name="shadow-properties"></a><span data-ttu-id="ca32d-102">Gölge Özellikler</span><span class="sxs-lookup"><span data-stu-id="ca32d-102">Shadow Properties</span></span>

<span data-ttu-id="ca32d-103">Gölge özellikler .NET varlık Sınıfınız içinde tanımlı değil, ancak bu varlık türünün EF Core modelinde tanımlanmış özelliklerdir.</span><span class="sxs-lookup"><span data-stu-id="ca32d-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="ca32d-104">Sadece değişiklik İzleyici ' değeri ve bu özelliklerin durumu saklanır.</span><span class="sxs-lookup"><span data-stu-id="ca32d-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="ca32d-105">Gölge özellikler eşlenen varlık türlerinde sunulmamalıdır veritabanında veri olduğunda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="ca32d-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="ca32d-106">Burada iki varlık arasında ilişki bir veritabanındaki yabancı anahtar değeri tarafından temsil edilen, ancak ilişki varlık türleri arasında gezinti özelliklerini kullanarak varlık türleri üzerinde yönetilen yabancı anahtar özellikleri için en sık kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ca32d-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="ca32d-107">Gölge özellik değerlerini alınabilir ve aracılığıyla değiştirilmiş `ChangeTracker` API.</span><span class="sxs-lookup"><span data-stu-id="ca32d-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="ca32d-108">Gölge özellikler, LINQ sorgularında başvurulabilir `EF.Property` statik yöntem.</span><span class="sxs-lookup"><span data-stu-id="ca32d-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="ca32d-109">Kurallar</span><span class="sxs-lookup"><span data-stu-id="ca32d-109">Conventions</span></span>

<span data-ttu-id="ca32d-110">Gölge özellikler, bir ilişki bulundu ancak bağımlı varlık sınıfı bulunamadı yabancı anahtar özellik kuralı tarafından oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="ca32d-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="ca32d-111">Bu durumda, bir gölge yabancı anahtar özellik sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="ca32d-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="ca32d-112">Gölge yabancı anahtar özelliği adlı `<navigation property name><principal key property name>` (asıl varlığa işaret bağımlı varlık üzerinde Gezinti adlandırmak için kullanılır).</span><span class="sxs-lookup"><span data-stu-id="ca32d-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="ca32d-113">Gezinme özelliğinin adını asıl anahtar özellik adını içeren sonra adı yalnızca olacaktır `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="ca32d-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="ca32d-114">Sonra bağımlı varlık üzerinde bir gezinti özelliği yok ise, asıl tür adı yerine kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ca32d-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="ca32d-115">Örneğin, aşağıdaki kod listesi sonuçlanır bir `BlogId` gölge özelliği için sunulan `Post` varlık.</span><span class="sxs-lookup"><span data-stu-id="ca32d-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="ca32d-116">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="ca32d-116">Data Annotations</span></span>

<span data-ttu-id="ca32d-117">Gölge özellikler veri ek açıklamaları ile oluşturulamaz.</span><span class="sxs-lookup"><span data-stu-id="ca32d-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ca32d-118">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="ca32d-118">Fluent API</span></span>

<span data-ttu-id="ca32d-119">Fluent API'si gölge özelliklerini yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ca32d-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="ca32d-120">Dize aşırı yüklemesini çağrıldığında `Property` , yaptığınız diğer özellikler için yapılandırma çağrılarının zincirleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ca32d-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="ca32d-121">Ad için sağlandıysa `Property` yöntemi (gölge özelliği veya bir varlık sınıf üzerinde tanımlanan) varolan bir özellik adı ile eşleşen ve ardından yeni bir gölge özelliği sunmak yerine, varolan bir özellik kod yapılandıracaksınız.</span><span class="sxs-lookup"><span data-stu-id="ca32d-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
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
