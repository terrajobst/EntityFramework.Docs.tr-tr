---
title: Türler hariç & içerme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655735"
---
# <a name="including--excluding-types"></a><span data-ttu-id="93645-102">Türleri Dahil Etme ve Dışlama</span><span class="sxs-lookup"><span data-stu-id="93645-102">Including & Excluding Types</span></span>

<span data-ttu-id="93645-103">Modele bir tür eklemek, EF 'in bu tür hakkında meta verilere sahip olduğu ve veritabanından örnekleri okumaya ve veritabanına yazmayı deneyeceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="93645-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="93645-104">Kurallar</span><span class="sxs-lookup"><span data-stu-id="93645-104">Conventions</span></span>

<span data-ttu-id="93645-105">Kurala göre, bağlamınızın `DbSet` özelliklerinde gösterilen türler modelinize dahildir.</span><span class="sxs-lookup"><span data-stu-id="93645-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="93645-106">Ayrıca, `OnModelCreating` yönteminde bahsedilen türler de dahildir.</span><span class="sxs-lookup"><span data-stu-id="93645-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="93645-107">Son olarak, bulunan türlerin gezinti özelliklerini yinelemeli olarak inceleyerek bulunan türler da modele dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="93645-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="93645-108">**Örneğin, aşağıdaki kod listesinde, tüm üç tür bulunur:**</span><span class="sxs-lookup"><span data-stu-id="93645-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="93645-109">bağlamda bir `DbSet` özelliğinde kullanıma sunulduğundan `Blog`</span><span class="sxs-lookup"><span data-stu-id="93645-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="93645-110">`Blog.Posts` gezinti özelliği aracılığıyla bulunduğundan `Post`</span><span class="sxs-lookup"><span data-stu-id="93645-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="93645-111">`AuditEntry` `OnModelCreating` belirtildiği için</span><span class="sxs-lookup"><span data-stu-id="93645-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a><span data-ttu-id="93645-112">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="93645-112">Data Annotations</span></span>

<span data-ttu-id="93645-113">Bir türü modelden dışlamak için veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93645-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="93645-114">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="93645-114">Fluent API</span></span>

<span data-ttu-id="93645-115">Bir tür modelden dışlamak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93645-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
