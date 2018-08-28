---
title: Varsayılan şema - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995372"
---
# <a name="default-schema"></a><span data-ttu-id="122a0-102">Varsayılan şema</span><span class="sxs-lookup"><span data-stu-id="122a0-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="122a0-103">Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="122a0-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="122a0-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="122a0-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="122a0-105">Varsayılan şema, bu nesne için bir şema açıkça yapılandırılmamışsa, nesneleri oluşturulacak veritabanı şeması ' dir.</span><span class="sxs-lookup"><span data-stu-id="122a0-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="122a0-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="122a0-106">Conventions</span></span>

<span data-ttu-id="122a0-107">Kural gereği, veritabanı sağlayıcısı en uygun varsayılan şema seçersiniz.</span><span class="sxs-lookup"><span data-stu-id="122a0-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="122a0-108">Örneğin, Microsoft SQL Server kullanacak `dbo` (şemaları SQLite desteklenmediği) şema ve SQLite bir şema kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="122a0-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="122a0-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="122a0-109">Data Annotations</span></span>

<span data-ttu-id="122a0-110">Veri ek açıklamalarını kullanma varsayılan şema ayarlayamazsınız.</span><span class="sxs-lookup"><span data-stu-id="122a0-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="122a0-111">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="122a0-111">Fluent API</span></span>

<span data-ttu-id="122a0-112">Varsayılan şema belirtmek için Fluent API'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="122a0-112">You can use the Fluent API to specify a default schema.</span></span>

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
