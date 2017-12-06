---
title: "Gölge özellikleri - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
ms.technology: entity-framework-core
uid: core/modeling/shadow-properties
ms.openlocfilehash: 8c7f70789ddc6ebd24f9bfae931069834db593c2
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2017
---
# <a name="shadow-properties"></a><span data-ttu-id="a453c-102">Gölge Özellikleri</span><span class="sxs-lookup"><span data-stu-id="a453c-102">Shadow Properties</span></span>

<span data-ttu-id="a453c-103">Gölge Özellikleri .NET varlık sınıfınızda tanımlı değil ancak EF çekirdek modeldeki varlık türü için tanımlanmış özelliklerdir.</span><span class="sxs-lookup"><span data-stu-id="a453c-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="a453c-104">Tamamen değişiklik İzleyici'değeri ve bu özelliklerin durumu saklanır.</span><span class="sxs-lookup"><span data-stu-id="a453c-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="a453c-105">Gölge özellikleri eşlenen varlık türlerinde gösterilmemesi veritabanında veri olduğunda faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="a453c-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="a453c-106">Bunlar çoğunlukla burada iki varlık arasındaki ilişki veritabanındaki yabancı anahtar değeri tarafından temsil edilen, ancak ilişki varlık türleri arasındaki Gezinti özellikleri kullanarak varlık türleri üzerinde yönetilen yabancı anahtar özellikleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a453c-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="a453c-107">Gölge özellik değerlerini alınabilir ve aracılığıyla değiştirilmiş `ChangeTracker` API.</span><span class="sxs-lookup"><span data-stu-id="a453c-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="a453c-108">LINQ sorgularında gölge özellikleri başvurulabilir `EF.Property` statik yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a453c-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="a453c-109">Kurallar</span><span class="sxs-lookup"><span data-stu-id="a453c-109">Conventions</span></span>

<span data-ttu-id="a453c-110">Gölge özellikleri kurala göre bir ilişkisi bulunan ancak hiçbir yabancı anahtar özelliği bağımlı varlık sınıfında bulunan oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="a453c-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="a453c-111">Bu durumda, bir gölge yabancı anahtar özelliği görülecektir.</span><span class="sxs-lookup"><span data-stu-id="a453c-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="a453c-112">Gölge yabancı anahtar özellik olarak adlandırılacaktır `<navigation property name><principal key property name>` (asıl varlığa işaret, bağımlı varlık üzerinde Gezinti adlandırmak için kullanılır).</span><span class="sxs-lookup"><span data-stu-id="a453c-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="a453c-113">Asıl anahtar özellik adı gezinme özelliğinin adını içeren sonra adı yalnızca olacaktır `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a453c-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="a453c-114">Bağımlı varlığa gezinti özelliği yok ise, asıl tür adı onun yerine kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a453c-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="a453c-115">Örneğin, aşağıdaki kod listesi sonuçlanır bir `BlogId` gölge özelliği için eklenen `Post` varlık.</span><span class="sxs-lookup"><span data-stu-id="a453c-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="a453c-116">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="a453c-116">Data Annotations</span></span>

<span data-ttu-id="a453c-117">Gölge özellikleri ile veri ek açıklamaları oluşturulamaz.</span><span class="sxs-lookup"><span data-stu-id="a453c-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a453c-118">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="a453c-118">Fluent API</span></span>

<span data-ttu-id="a453c-119">Fluent API gölge özelliklerini yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a453c-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="a453c-120">Dize aşırı yüklemesini çağrıldığında `Property` herhangi diğer özellikler için yaptığınız yapılandırma çağrılarının zincir.</span><span class="sxs-lookup"><span data-stu-id="a453c-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="a453c-121">Ad için sağlandıysa `Property` yöntem eşleşiyor (bir gölge özellik veya bir varlık sınıfı üzerinde tanımlanan) mevcut bir özellik adını, sonra kod yeni bir gölge özelliği giriş yerine o mevcut özellik yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a453c-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

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
