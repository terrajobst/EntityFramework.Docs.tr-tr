---
title: Gölge özellikler - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: b7b7b10642564dfa3dbc05755188b5b5c63e0d03
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993808"
---
# <a name="shadow-properties"></a>Gölge Özellikler

Gölge özellikler .NET varlık Sınıfınız içinde tanımlı değil, ancak bu varlık türünün EF Core modelinde tanımlanmış özelliklerdir. Sadece değişiklik İzleyici ' değeri ve bu özelliklerin durumu saklanır.

Gölge özellikler eşlenen varlık türlerinde sunulmamalıdır veritabanında veri olduğunda yararlıdır. Burada iki varlık arasında ilişki bir veritabanındaki yabancı anahtar değeri tarafından temsil edilen, ancak ilişki varlık türleri arasında gezinti özelliklerini kullanarak varlık türleri üzerinde yönetilen yabancı anahtar özellikleri için en sık kullanılır.

Gölge özellik değerlerini alınabilir ve aracılığıyla değiştirilmiş `ChangeTracker` API.

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Gölge özellikler, LINQ sorgularında başvurulabilir `EF.Property` statik yöntem.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Kurallar

Gölge özellikler, bir ilişki bulundu ancak bağımlı varlık sınıfı bulunamadı yabancı anahtar özellik kuralı tarafından oluşturulabilir. Bu durumda, bir gölge yabancı anahtar özellik sunulacaktır. Gölge yabancı anahtar özelliği adlı `<navigation property name><principal key property name>` (asıl varlığa işaret bağımlı varlık üzerinde Gezinti adlandırmak için kullanılır). Gezinme özelliğinin adını asıl anahtar özellik adını içeren sonra adı yalnızca olacaktır `<principal key property name>`. Sonra bağımlı varlık üzerinde bir gezinti özelliği yok ise, asıl tür adı yerine kullanılır.

Örneğin, aşağıdaki kod listesi sonuçlanır bir `BlogId` gölge özelliği için sunulan `Post` varlık.

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

## <a name="data-annotations"></a>Veri ek açıklamaları

Gölge özellikler veri ek açıklamaları ile oluşturulamaz.

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si gölge özelliklerini yapılandırmak için kullanabilirsiniz. Dize aşırı yüklemesini çağrıldığında `Property` , yaptığınız diğer özellikler için yapılandırma çağrılarının zincirleyebilirsiniz.

Ad için sağlandıysa `Property` yöntemi (gölge özelliği veya bir varlık sınıf üzerinde tanımlanan) varolan bir özellik adı ile eşleşen ve ardından yeni bir gölge özelliği sunmak yerine, varolan bir özellik kod yapılandıracaksınız.

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
