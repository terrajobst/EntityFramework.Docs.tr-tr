---
title: Birincil anahtarlar - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
ms.technology: entity-framework-core
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: fcb1871149c0f20a2576864028b4171904de1982
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054180"
---
# <a name="primary-keys"></a><span data-ttu-id="b4ad0-102">Birincil anahtarlar</span><span class="sxs-lookup"><span data-stu-id="b4ad0-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="b4ad0-103">Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b4ad0-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="b4ad0-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="b4ad0-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="b4ad0-105">Bir birincil anahtar kısıtlaması her bir varlık türü anahtarı için sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="b4ad0-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="b4ad0-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="b4ad0-106">Conventions</span></span>

<span data-ttu-id="b4ad0-107">Kurala göre veritabanı birincil anahtarda adlandırılacağını `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="b4ad0-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b4ad0-108">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="b4ad0-108">Data Annotations</span></span>

<span data-ttu-id="b4ad0-109">Bir birincil anahtar yok ilişkisel veritabanı belirli yönlerini veri ek açıklamaları kullanılarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b4ad0-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b4ad0-110">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="b4ad0-110">Fluent API</span></span>

<span data-ttu-id="b4ad0-111">Fluent API veritabanında birincil anahtar kısıtlaması adını yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b4ad0-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

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
