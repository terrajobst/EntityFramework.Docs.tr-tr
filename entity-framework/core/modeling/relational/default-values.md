---
title: Varsayılan değerler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: 0d3613606f21a78e22cfe0ee752ea982a6a17f93
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196991"
---
# <a name="default-values"></a><span data-ttu-id="85deb-102">Varsayılan Değerler</span><span class="sxs-lookup"><span data-stu-id="85deb-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="85deb-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="85deb-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="85deb-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="85deb-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="85deb-105">Bir sütunun varsayılan değeri, yeni bir satır eklenirse, ancak sütun için hiçbir değer belirtilmemişse eklenecek değerdir.</span><span class="sxs-lookup"><span data-stu-id="85deb-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="85deb-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="85deb-106">Conventions</span></span>

<span data-ttu-id="85deb-107">Kurala göre, varsayılan bir değer yapılandırılmaz.</span><span class="sxs-lookup"><span data-stu-id="85deb-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="85deb-108">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="85deb-108">Data Annotations</span></span>

<span data-ttu-id="85deb-109">Veri ek açıklamalarını kullanarak varsayılan bir değer ayarlayamazsınız.</span><span class="sxs-lookup"><span data-stu-id="85deb-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="85deb-110">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="85deb-110">Fluent API</span></span>

<span data-ttu-id="85deb-111">Bir özellik için varsayılan değeri belirtmek üzere Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85deb-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultValue.cs?highlight=9)] -->
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

<span data-ttu-id="85deb-112">Varsayılan değeri hesaplamak için kullanılan bir SQL parçası de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85deb-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultValueSql.cs?highlight=9)] -->
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
