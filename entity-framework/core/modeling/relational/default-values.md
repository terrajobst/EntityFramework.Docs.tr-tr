---
title: Varsayılan değerleri - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: 341f243ddddc345bb4236e5c34f814694b71e32a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996258"
---
# <a name="default-values"></a><span data-ttu-id="8485e-102">Varsayılan değerler</span><span class="sxs-lookup"><span data-stu-id="8485e-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="8485e-103">Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8485e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="8485e-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="8485e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="8485e-105">Yeni bir satır eklenir, ancak hiçbir değer belirtilen sütun için eklenecek değer sütunun varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="8485e-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="8485e-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="8485e-106">Conventions</span></span>

<span data-ttu-id="8485e-107">Kural gereği, varsayılan değer yapılandırılmadı.</span><span class="sxs-lookup"><span data-stu-id="8485e-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8485e-108">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="8485e-108">Data Annotations</span></span>

<span data-ttu-id="8485e-109">Veri ek açıklamalarını kullanma, varsayılan bir değer ayarlayamazsınız.</span><span class="sxs-lookup"><span data-stu-id="8485e-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8485e-110">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="8485e-110">Fluent API</span></span>

<span data-ttu-id="8485e-111">Fluent API'si, bir özellik için varsayılan değer belirtmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8485e-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValue.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Rating)
            .HasDefaultValue(3);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
}
```

<span data-ttu-id="8485e-112">Varsayılan değer hesaplamak için kullanılan bir SQL parça de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8485e-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValueSql.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Created)
            .HasDefaultValueSql("getdate()");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public DateTime Created { get; set; }
}
```
