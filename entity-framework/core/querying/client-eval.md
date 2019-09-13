---
title: İstemci ile Sunucu değerlendirmesi-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: cb207d9e1b1004a4084dd6fc66712183b5bdd5dc
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921710"
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="46c44-102">İstemci ile Sunucu değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="46c44-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="46c44-103">Entity Framework Core, sorgunun istemci üzerinde değerlendirilmekte olduğunu ve veritabanına itilmekte olan parçalarını destekler.</span><span class="sxs-lookup"><span data-stu-id="46c44-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="46c44-104">Bu, sorgunun hangi bölümlerinin veritabanında değerlendirileceğini belirleyen veritabanı sağlayıcısına ait olacaktır.</span><span class="sxs-lookup"><span data-stu-id="46c44-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="46c44-105">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="46c44-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="46c44-106">İstemci değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="46c44-106">Client evaluation</span></span>

<span data-ttu-id="46c44-107">Aşağıdaki örnekte, bir SQL Server veritabanından döndürülen blogların URL 'Lerini standartlaştırmak için bir yardımcı yöntem kullanılır.</span><span class="sxs-lookup"><span data-stu-id="46c44-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="46c44-108">SQL Server sağlayıcının bu yöntemin nasıl uygulandığı hakkında öngörü olmadığından, SQL 'e çevirmek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="46c44-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="46c44-109">Sorgunun diğer tüm yönleri veritabanında değerlendirilir, ancak bu yöntem aracılığıyla döndürülen `URL` bu yöntemin geçirilmesi istemcisinde yapılır.</span><span class="sxs-lookup"><span data-stu-id="46c44-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs?highlight=6)] -->
``` csharp
var blogs = context.Blogs
    .OrderByDescending(blog => blog.Rating)
    .Select(blog => new
    {
        Id = blog.BlogId,
        Url = StandardizeUrl(blog.Url)
    })
    .ToList();
```

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
public static string StandardizeUrl(string url)
{
    url = url.ToLower();

    if (!url.StartsWith("http://"))
    {
        url = string.Concat("http://", url);
    }

    return url;
}
```

## <a name="client-evaluation-performance-issues"></a><span data-ttu-id="46c44-110">İstemci değerlendirmesi performans sorunları</span><span class="sxs-lookup"><span data-stu-id="46c44-110">Client evaluation performance issues</span></span>

<span data-ttu-id="46c44-111">İstemci değerlendirmesi çok faydalı olabilir, bazı durumlarda performansı düşük olabilir.</span><span class="sxs-lookup"><span data-stu-id="46c44-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="46c44-112">Yardımcı yöntemin artık bir filtrede kullanıldığı aşağıdaki sorguyu göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="46c44-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="46c44-113">Bu, veritabanında gerçekleştirilemediğinden, tüm veriler belleğe çekilir ve sonra filtre istemciye uygulanır.</span><span class="sxs-lookup"><span data-stu-id="46c44-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="46c44-114">Veri miktarına ve bu verilerin ne kadarının filtrelendiğine bağlı olarak, bu, performansın düşmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="46c44-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a><span data-ttu-id="46c44-115">İstemci değerlendirme günlüğü</span><span class="sxs-lookup"><span data-stu-id="46c44-115">Client evaluation logging</span></span>

<span data-ttu-id="46c44-116">Varsayılan olarak, EF Core istemci değerlendirmesi gerçekleştirildiğinde bir uyarı günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="46c44-116">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="46c44-117">Günlüğe kaydetme çıkışını görüntüleme hakkında daha fazla bilgi için [günlüğe](../miscellaneous/logging.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="46c44-117">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a><span data-ttu-id="46c44-118">İsteğe bağlı davranış: istemci değerlendirmesi için bir özel durum oluşturun</span><span class="sxs-lookup"><span data-stu-id="46c44-118">Optional behavior: throw an exception for client evaluation</span></span>

<span data-ttu-id="46c44-119">İstemci değerlendirmesi yapıldığında ya da hiçbir şey yapmayan davranışı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46c44-119">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="46c44-120">Bu işlem, bağlam için seçenekleri ayarlarken yapılır-içinde `DbContext.OnConfiguring` `Startup.cs` veya ASP.NET Core kullanıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="46c44-120">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
