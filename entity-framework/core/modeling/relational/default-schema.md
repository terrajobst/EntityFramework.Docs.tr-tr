---
title: "Varsayılan şema - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
ms.technology: entity-framework-core
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 26106deb2d4e35ecf33e97790a83f9af77991aed
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="default-schema"></a><span data-ttu-id="8c449-102">Varsayılan şema</span><span class="sxs-lookup"><span data-stu-id="8c449-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="8c449-103">Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8c449-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="8c449-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="8c449-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="8c449-105">Varsayılan şema, bu nesne için bir şema açıkça yapılandırılmamışsa nesneleri içinde oluşturulan veritabanı şemasının ' dir.</span><span class="sxs-lookup"><span data-stu-id="8c449-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="8c449-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="8c449-106">Conventions</span></span>

<span data-ttu-id="8c449-107">Kurala göre en uygun varsayılan şema veritabanı sağlayıcısı seçeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="8c449-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="8c449-108">Örneğin, Microsoft SQL Server kullanacak `dbo` (şemaları SQLite içinde desteklenmediği) şema ve SQLite bir şema kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="8c449-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8c449-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="8c449-109">Data Annotations</span></span>

<span data-ttu-id="8c449-110">Veri ek açıklamaları kullanılarak varsayılan şema ayarlayamazsınız.</span><span class="sxs-lookup"><span data-stu-id="8c449-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8c449-111">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="8c449-111">Fluent API</span></span>

<span data-ttu-id="8c449-112">Varsayılan şema belirtmek için Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c449-112">You can use the Fluent API to specify a default schema.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
