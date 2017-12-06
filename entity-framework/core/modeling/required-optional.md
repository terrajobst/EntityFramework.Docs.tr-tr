---
title: "Gerekli/isteğe bağlı özellikleri - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
ms.technology: entity-framework-core
uid: core/modeling/required-optional
ms.openlocfilehash: 2af1d49e12ef980f81cb9c00556dee471673ccae
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="8ab8d-102">Gerekli ve isteğe bağlı özellikler</span><span class="sxs-lookup"><span data-stu-id="8ab8d-102">Required and Optional Properties</span></span>

<span data-ttu-id="8ab8d-103">Bir özellik içeren için geçerli olup olmadığını isteğe bağlı olarak kabul edilir `null`.</span><span class="sxs-lookup"><span data-stu-id="8ab8d-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="8ab8d-104">Varsa `null` gerekli bir özellik olarak kabul edilir sonra bir özelliğe atanmak için geçerli bir değer değil.</span><span class="sxs-lookup"><span data-stu-id="8ab8d-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="8ab8d-105">Kurallar</span><span class="sxs-lookup"><span data-stu-id="8ab8d-105">Conventions</span></span>

<span data-ttu-id="8ab8d-106">Kurala göre CLR türü, null içerebilir bir özellik isteğe bağlı olarak yapılandırılacak (`string`, `int?`, `byte[]`vb..).</span><span class="sxs-lookup"><span data-stu-id="8ab8d-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="8ab8d-107">Özellikler, CLR türü, null içeremez yapılandırılması gerektiği gibi (`int`, `decimal`, `bool`vb..).</span><span class="sxs-lookup"><span data-stu-id="8ab8d-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="8ab8d-108">Bir özellik, CLR türü null içeremez, isteğe bağlı olarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="8ab8d-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="8ab8d-109">Özellik her zaman gerekli Entity Framework tarafından kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="8ab8d-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8ab8d-110">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="8ab8d-110">Data Annotations</span></span>

<span data-ttu-id="8ab8d-111">Bir özelliğin gerekli olduğunu belirtmek için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8ab8d-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="8ab8d-112">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="8ab8d-112">Fluent API</span></span>

<span data-ttu-id="8ab8d-113">Bir özelliğin gerekli olduğunu belirtmek için Fluent API'sini kullanın.</span><span class="sxs-lookup"><span data-stu-id="8ab8d-113">You can use the Fluent API to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
