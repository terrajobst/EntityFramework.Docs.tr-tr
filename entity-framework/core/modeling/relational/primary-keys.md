---
title: Birincil anahtarlar-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: bdb31964d717c64bddc28e7f1ce55b261e285a9b
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196951"
---
# <a name="primary-keys"></a><span data-ttu-id="84e1e-102">Birincil Anahtarlar</span><span class="sxs-lookup"><span data-stu-id="84e1e-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="84e1e-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="84e1e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="84e1e-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="84e1e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="84e1e-105">Her varlık türünün anahtarı için bir birincil anahtar kısıtlaması tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="84e1e-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="84e1e-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="84e1e-106">Conventions</span></span>

<span data-ttu-id="84e1e-107">Kurala göre, veritabanındaki birincil anahtar olarak adlandırılır `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="84e1e-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="84e1e-108">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="84e1e-108">Data Annotations</span></span>

<span data-ttu-id="84e1e-109">Birincil anahtarın ilişkisel veritabanı özel yönleri, veri ek açıklamaları kullanılarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="84e1e-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="84e1e-110">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="84e1e-110">Fluent API</span></span>

<span data-ttu-id="84e1e-111">Akıcı API 'YI, veritabanında birincil anahtar kısıtlamasının adını yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84e1e-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
