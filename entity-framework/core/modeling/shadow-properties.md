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
# <a name="shadow-properties"></a>Gölge Özellikler

Gölge özellikleri, .NET varlık sınıfınızda tanımlanmayan ancak EF Core modelinde bu varlık türü için tanımlanan özelliklerdir. Bu özelliklerin değeri ve durumu yalnızca değişiklik Izleyicide korunur.

Veritabanında, eşlenmiş varlık türlerinde gösterilmemesi gereken veriler olduğunda, gölge özellikleri faydalıdır. Bunlar, iki varlık arasındaki ilişkinin veritabanındaki bir yabancı anahtar değeri ile temsil edildiği, ancak ilişki varlık türleri arasında, varlık türleri arasında yönetildiğinde, yabancı anahtar özellikleri için en sık kullanılır.

Gölge özellik değerleri, `ChangeTracker` API aracılığıyla elde edilebilir ve değiştirilebilir.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

`EF.Property` Statik yöntem aracılığıyla, LINQ sorgularında gölge özelliklerine başvurulabilir.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Kurallar

Bir ilişki keşfedildiğinde, ancak bağımlı varlık sınıfında yabancı anahtar özelliği bulunamadığında, bu kural tarafından gölge özellikleri oluşturulabilir. Bu durumda, bir gölge yabancı anahtar özelliği tanıtılacaktır. Gölge yabancı anahtar özelliği adlandırılacak `<navigation property name><principal key property name>` (birincil varlığa işaret eden, adlandırma için kullanılan bağımlı varlık üzerindeki gezinti). Asıl anahtar özellik adı Gezinti özelliğinin adını içeriyorsa, ad yalnızca `<principal key property name>`siz olur. Bağımlı varlık üzerinde hiçbir gezinti özelliği yoksa, asıl tür adı bunun yerine kullanılır.

Örneğin, aşağıdaki kod listesi, `BlogId` `Post` varlığa bir gölge özelliğin tanıtılmasıyla sonuçlanır.

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

## <a name="data-annotations"></a>Veri Açıklamaları

Gölge özellikleri, veri ek açıklamalarıyla oluşturulamaz.

## <a name="fluent-api"></a>Akıcı API

Gölge özelliklerini yapılandırmak için Floent API 'sini kullanabilirsiniz. ' Nin `Property` dize aşırı yüklemesini çağırdıktan sonra, diğer özellikler için yaptığınız herhangi bir yapılandırma çağrısının herhangi birini zincirleyebilirsiniz.

`Property` Yöntemi için sağlanan ad, var olan bir özelliğin adıyla eşleşiyorsa (bir gölge özellik veya varlık sınıfında tanımlı bir tane), kod, yeni bir gölge özellik tanıtımı yerine mevcut özelliği yapılandırır.

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
