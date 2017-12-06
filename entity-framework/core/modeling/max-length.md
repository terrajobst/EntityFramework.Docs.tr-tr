---
title: "En fazla uzunluk - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
ms.technology: entity-framework-core
uid: core/modeling/max-length
ms.openlocfilehash: 7325c0c3328477473392bf9e7c82f1696bb4f424
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="maximum-length"></a><span data-ttu-id="f077e-102">En fazla uzunluğu</span><span class="sxs-lookup"><span data-stu-id="f077e-102">Maximum Length</span></span>

<span data-ttu-id="f077e-103">En fazla yapılandırma veri deposu belirli bir özellik için kullanılacak uygun veri türü hakkında bir ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="f077e-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="f077e-104">En fazla uzunluk yalnızca geçerlidir dizi veri türleri için gibi `string` ve `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="f077e-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="f077e-105">Entity Framework veri sağlayıcısına geçirmeden önce tüm doğrulama en fazla uzunluğu yapmaz.</span><span class="sxs-lookup"><span data-stu-id="f077e-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="f077e-106">Uygunsa doğrulamak için sağlayıcısı veya veri deposunu kadar olan.</span><span class="sxs-lookup"><span data-stu-id="f077e-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="f077e-107">Örneğin, SQL Server, en fazla uzunluğu aşan hedefleyen bir özel durum temel sütununun veri türü olarak neden olması durumunda depolanan veri fazlasını izin vermez.</span><span class="sxs-lookup"><span data-stu-id="f077e-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="f077e-108">Kurallar</span><span class="sxs-lookup"><span data-stu-id="f077e-108">Conventions</span></span>

<span data-ttu-id="f077e-109">Kurala göre uygun bir veri türü özellikleri seçmek için veritabanı sağlayıcısı kadar bırakılır.</span><span class="sxs-lookup"><span data-stu-id="f077e-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="f077e-110">Bir uzunluğa sahip özellikler için veritabanı sağlayıcısı genellikle uzun veri uzunluğu için izin veren bir veri türü seçeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="f077e-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="f077e-111">Örneğin, Microsoft SQL Server kullanacak `nvarchar(max)` için `string` özellikleri (veya `nvarchar(450)` sütunun bir anahtar olarak kullanılıyorsa).</span><span class="sxs-lookup"><span data-stu-id="f077e-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f077e-112">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="f077e-112">Data Annotations</span></span>

<span data-ttu-id="f077e-113">Bir özellik için en fazla uzunluk yapılandırmak için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f077e-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="f077e-114">Bu örnekte, SQL Server'ın bu sonuçlanacak hedefleme `nvarchar(500)` veri türü.</span><span class="sxs-lookup"><span data-stu-id="f077e-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="f077e-115">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="f077e-115">Fluent API</span></span>

<span data-ttu-id="f077e-116">Bir özellik için en fazla uzunluk yapılandırmak için Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f077e-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="f077e-117">Bu örnekte, SQL Server'ın bu sonuçlanacak hedefleme `nvarchar(500)` veri türü.</span><span class="sxs-lookup"><span data-stu-id="f077e-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
