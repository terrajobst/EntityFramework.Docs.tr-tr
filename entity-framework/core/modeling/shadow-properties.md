---
title: Gölge özellikleri - EF çekirdek
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
ms.locfileid: "26054564"
---
# <a name="shadow-properties"></a>Gölge Özellikleri

Gölge Özellikleri .NET varlık sınıfınızda tanımlı değil ancak EF çekirdek modeldeki varlık türü için tanımlanmış özelliklerdir. Tamamen değişiklik İzleyici'değeri ve bu özelliklerin durumu saklanır.

Gölge özellikleri eşlenen varlık türlerinde gösterilmemesi veritabanında veri olduğunda faydalıdır. Bunlar çoğunlukla burada iki varlık arasındaki ilişki veritabanındaki yabancı anahtar değeri tarafından temsil edilen, ancak ilişki varlık türleri arasındaki Gezinti özellikleri kullanarak varlık türleri üzerinde yönetilen yabancı anahtar özellikleri için kullanılır.

Gölge özellik değerlerini alınabilir ve aracılığıyla değiştirilmiş `ChangeTracker` API.

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

LINQ sorgularında gölge özellikleri başvurulabilir `EF.Property` statik yöntemi.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Kurallar

Gölge özellikleri kurala göre bir ilişkisi bulunan ancak hiçbir yabancı anahtar özelliği bağımlı varlık sınıfında bulunan oluşturulabilir. Bu durumda, bir gölge yabancı anahtar özelliği görülecektir. Gölge yabancı anahtar özellik olarak adlandırılacaktır `<navigation property name><principal key property name>` (asıl varlığa işaret, bağımlı varlık üzerinde Gezinti adlandırmak için kullanılır). Asıl anahtar özellik adı gezinme özelliğinin adını içeren sonra adı yalnızca olacaktır `<principal key property name>`. Bağımlı varlığa gezinti özelliği yok ise, asıl tür adı onun yerine kullanılır.

Örneğin, aşağıdaki kod listesi sonuçlanır bir `BlogId` gölge özelliği için eklenen `Post` varlık.

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

Gölge özellikleri ile veri ek açıklamaları oluşturulamaz.

## <a name="fluent-api"></a>Fluent API'si

Fluent API gölge özelliklerini yapılandırmak için kullanabilirsiniz. Dize aşırı yüklemesini çağrıldığında `Property` herhangi diğer özellikler için yaptığınız yapılandırma çağrılarının zincir.

Ad için sağlandıysa `Property` yöntem eşleşiyor (bir gölge özellik veya bir varlık sınıfı üzerinde tanımlanan) mevcut bir özellik adını, sonra kod yeni bir gölge özelliği giriş yerine o mevcut özellik yapılandırır.

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
