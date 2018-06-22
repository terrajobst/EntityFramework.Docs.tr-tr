---
title: İstemci vs. Sunucu değerlendirmesi - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
ms.technology: entity-framework-core
uid: core/querying/client-eval
ms.openlocfilehash: e1852b780041e9e92fb4d25129175346e3a601a3
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054159"
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="a6574-102">İstemci vs. Sunucu değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="a6574-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="a6574-103">Entity Framework Çekirdek istemci ve veritabanına iletilmesini bölümleri değerlendirilen sorgunun bölümlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="a6574-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="a6574-104">Bu sorgu hangi kısımlarının veritabanında değerlendirilecek belirlemek için veritabanı sağlayıcısı kadar olur.</span><span class="sxs-lookup"><span data-stu-id="a6574-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="a6574-105">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) github'da.</span><span class="sxs-lookup"><span data-stu-id="a6574-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="a6574-106">İstemci değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="a6574-106">Client evaluation</span></span>

<span data-ttu-id="a6574-107">Aşağıdaki örnekte yardımcı bir yöntem için bir SQL Server veritabanı tarafından döndürülen bloglar URL'leri standart hale getirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a6574-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="a6574-108">SQL Server sağlayıcısı bu yöntem nasıl uygulandığını içine hiçbir Insight olduğundan SQL'e Çevir mümkün değil.</span><span class="sxs-lookup"><span data-stu-id="a6574-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="a6574-109">Tüm sorgu yönlerini değerlendirilir veritabanında ancak geçirme döndürülen `URL` bu yöntemle istemci üzerinde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a6574-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs?highlight=6)] -->
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

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
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

## <a name="disabling-client-evaluation"></a><span data-ttu-id="a6574-110">İstemci değerlendirmesi devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="a6574-110">Disabling client evaluation</span></span>

<span data-ttu-id="a6574-111">İstemci değerlendirme çok kullanışlı olabilirler, ancak bazı durumlarda, performansın düşmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="a6574-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="a6574-112">Burada bir filtrede kullanılan yardımcı yöntemi aşağıdaki sorgu, göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="a6574-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="a6574-113">Bu veritabanında gerçekleştirilemediğinden tüm verileri bellek ve ardından filtre çekilir istemcide uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a6574-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="a6574-114">Veri ve bu verilerin ne kadar filtre miktarına bağlı olarak, bu performansın düşmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="a6574-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="a6574-115">İstemci değerlendirme gerçekleştirildiğinde varsayılan olarak, bir uyarı EF çekirdek günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="a6574-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="a6574-116">Bkz: [günlüğü](../miscellaneous/logging.md) günlük çıktısı görüntüleme hakkında daha fazla bilgi.</span><span class="sxs-lookup"><span data-stu-id="a6574-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="a6574-117">İstemci değerlendirme throw veya hiçbir şey yapma oluştuğunda davranışı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a6574-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="a6574-118">Bu genellikle, içeriğiniz - seçeneklerini ayarlama zaman yapılır `DbContext.OnConfiguring`, veya `Startup.cs` ASP.NET Core kullanıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="a6574-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
