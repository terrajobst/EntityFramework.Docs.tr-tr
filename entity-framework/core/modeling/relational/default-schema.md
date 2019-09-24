---
title: Varsayılan şema-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: ae903ed7200859430aecc55073651236759bc6ce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197136"
---
# <a name="default-schema"></a><span data-ttu-id="0a8da-102">Varsayılan Şema</span><span class="sxs-lookup"><span data-stu-id="0a8da-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="0a8da-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0a8da-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="0a8da-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="0a8da-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="0a8da-105">Varsayılan şema, bu nesne için bir şema açıkça yapılandırılmamışsa nesnelerin oluşturulacağı veritabanı şemadır.</span><span class="sxs-lookup"><span data-stu-id="0a8da-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="0a8da-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="0a8da-106">Conventions</span></span>

<span data-ttu-id="0a8da-107">Kural gereği, veritabanı sağlayıcısı en uygun varsayılan şemayı seçer.</span><span class="sxs-lookup"><span data-stu-id="0a8da-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="0a8da-108">Örneğin, Microsoft SQL Server `dbo` şemayı kullanacaktır ve SQLite bir şema kullanmaz (çünkü şemalar SQLite ' de desteklenmez).</span><span class="sxs-lookup"><span data-stu-id="0a8da-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0a8da-109">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="0a8da-109">Data Annotations</span></span>

<span data-ttu-id="0a8da-110">Veri ek açıklamalarını kullanarak varsayılan şemayı ayarlayamazsınız.</span><span class="sxs-lookup"><span data-stu-id="0a8da-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="0a8da-111">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="0a8da-111">Fluent API</span></span>

<span data-ttu-id="0a8da-112">Varsayılan bir şema belirtmek için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0a8da-112">You can use the Fluent API to specify a default schema.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultSchema.cs?highlight=7)] -->
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
