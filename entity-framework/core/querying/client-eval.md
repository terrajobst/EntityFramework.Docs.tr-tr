---
title: İstemci vs. Server değerlendirmesi - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: 78f8d9576748a725634665f915def80b5a13820c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997883"
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="f48f5-102">İstemci vs. Server değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="f48f5-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="f48f5-103">Entity Framework Core istemcisi ve bunun veritabanına gönderilen parçalarını değerlendirilen sorgunun bölümlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="f48f5-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="f48f5-104">Bu veritabanında sorgu hangi parçalarının değerlendirileceğini belirlemek için veritabanı sağlayıcısı kadar olur.</span><span class="sxs-lookup"><span data-stu-id="f48f5-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="f48f5-105">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="f48f5-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="f48f5-106">İstemci değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="f48f5-106">Client evaluation</span></span>

<span data-ttu-id="f48f5-107">Aşağıdaki örnekte, bir yardımcı yöntem URL'leri farklı bir SQL Server veritabanından döndürülen Web günlükleri için standart hale getirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f48f5-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="f48f5-108">SQL Server sağlayıcısı bu yöntemin nasıl uygulandığını hiçbir Öngörüler olduğundan, SQL'e çevirmek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="f48f5-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="f48f5-109">Sorgu diğer tüm yönleri değerlendirilir veritabanındaki ancak geçirme döndürülen `URL` istemci üzerinde bu yöntem kullanılarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f48f5-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

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

## <a name="disabling-client-evaluation"></a><span data-ttu-id="f48f5-110">İstemci değerlendirmesi devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="f48f5-110">Disabling client evaluation</span></span>

<span data-ttu-id="f48f5-111">İstemci değerlendirmesi çok kullanışlı olabilir, ancak bazı durumlarda, performansın düşmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="f48f5-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="f48f5-112">Aşağıdaki sorgu, bir filtre yardımcı yöntemi artık kullanıldığı göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="f48f5-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="f48f5-113">Bu veritabanında gerçekleştirilemiyor çünkü tüm verileri, bellek ve ardından filtre çekilir istemcide uygulanır.</span><span class="sxs-lookup"><span data-stu-id="f48f5-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="f48f5-114">Veri ve bu verilerin ne kadar filtrenin dışında kaldı miktarına bağlı olarak, bu performansın düşmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="f48f5-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="f48f5-115">İstemci değerlendirmesi gerçekleştirildiğinde varsayılan olarak, bir uyarı EF Core günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="f48f5-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="f48f5-116">Bkz: [günlüğü](../miscellaneous/logging.md) günlük çıktısı görüntüleme hakkında daha fazla bilgi.</span><span class="sxs-lookup"><span data-stu-id="f48f5-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="f48f5-117">İstemci değerlendirmesi durum veya hiçbir şey yapma oluştuğunda davranışını değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f48f5-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="f48f5-118">Bağlamınızı - seçeneklerini normalde ayarlarken yapıldığını `DbContext.OnConfiguring`, veya `Startup.cs` ASP.NET Core kullanıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="f48f5-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
