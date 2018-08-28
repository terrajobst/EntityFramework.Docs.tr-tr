---
title: Birincil anahtarlar - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: 916f3adbcd08cb1037c7fbf68e99630feb321a61
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998074"
---
# <a name="primary-keys"></a><span data-ttu-id="aca59-102">Birincil anahtarlar</span><span class="sxs-lookup"><span data-stu-id="aca59-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="aca59-103">Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="aca59-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="aca59-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="aca59-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="aca59-105">Bir birincil anahtar kısıtlaması her varlık türü için anahtar sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="aca59-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="aca59-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="aca59-106">Conventions</span></span>

<span data-ttu-id="aca59-107">Kural gereği, veritabanının birincil anahtarı yeniden adlandırılacak `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="aca59-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="aca59-108">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="aca59-108">Data Annotations</span></span>

<span data-ttu-id="aca59-109">Bir birincil anahtar yok ilişkisel veritabanı belirli yönlerini veri ek açıklamalarını kullanma yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="aca59-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="aca59-110">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="aca59-110">Fluent API</span></span>

<span data-ttu-id="aca59-111">Fluent API'si, birincil anahtar kısıtlaması adı veritabanında yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="aca59-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
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
