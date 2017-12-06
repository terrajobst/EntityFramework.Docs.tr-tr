---
title: "Dizinler - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a><span data-ttu-id="6d54d-102">Dizinler</span><span class="sxs-lookup"><span data-stu-id="6d54d-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="6d54d-103">Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="6d54d-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="6d54d-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="6d54d-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="6d54d-105">İlişkisel bir veritabanındaki bir dizin aynı kavram çekirdek Entity Framework'ün bir dizin olarak eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="6d54d-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="6d54d-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="6d54d-106">Conventions</span></span>

<span data-ttu-id="6d54d-107">Kurala göre dizinleri adlı `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="6d54d-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="6d54d-108">Bileşik dizinler için `<property name>` özellik adları alt çizgi ayrılmış listesini olur.</span><span class="sxs-lookup"><span data-stu-id="6d54d-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="6d54d-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="6d54d-109">Data Annotations</span></span>

<span data-ttu-id="6d54d-110">Dizinleri veri ek açıklamaları kullanılarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="6d54d-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="6d54d-111">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="6d54d-111">Fluent API</span></span>

<span data-ttu-id="6d54d-112">Bir dizinin adını yapılandırmak için Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6d54d-112">You can use the Fluent API to configure the name of an index.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
