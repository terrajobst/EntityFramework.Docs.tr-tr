---
title: Gerekli/isteğe bağlı özellikleri - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: b6716a5b03e1afc2933e317d606ef50f986c22c7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995503"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="d3bec-102">Gerekli ve isteğe bağlı özellikler</span><span class="sxs-lookup"><span data-stu-id="d3bec-102">Required and Optional Properties</span></span>

<span data-ttu-id="d3bec-103">Bir özellik içeren için geçerli ise isteğe bağlı olarak sayılır `null`.</span><span class="sxs-lookup"><span data-stu-id="d3bec-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="d3bec-104">Varsa `null` gerekli özelliği olduğu düşünülür sonra bir özelliğe atanacak geçerli bir değer değil.</span><span class="sxs-lookup"><span data-stu-id="d3bec-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="d3bec-105">Kurallar</span><span class="sxs-lookup"><span data-stu-id="d3bec-105">Conventions</span></span>

<span data-ttu-id="d3bec-106">Kural gereği, CLR türü, null içerebilir bir özellik isteğe bağlı olarak yapılandırılır (`string`, `int?`, `byte[]`vb..).</span><span class="sxs-lookup"><span data-stu-id="d3bec-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="d3bec-107">CLR türü, null içeremez özellikleri yapılandırılması gerektiği gibi (`int`, `decimal`, `bool`vb..).</span><span class="sxs-lookup"><span data-stu-id="d3bec-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="d3bec-108">Bir özelliğin CLR türü null içeremez, isteğe bağlı olarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="d3bec-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="d3bec-109">Entity Framework tarafından gerekli özellik her zaman değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="d3bec-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d3bec-110">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="d3bec-110">Data Annotations</span></span>

<span data-ttu-id="d3bec-111">Bir özelliğin gerekli olduğunu belirtmek için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3bec-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="d3bec-112">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="d3bec-112">Fluent API</span></span>

<span data-ttu-id="d3bec-113">Fluent API'si, bir özelliğin gerekli olduğunu belirtmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3bec-113">You can use the Fluent API to indicate that a property is required.</span></span>

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
