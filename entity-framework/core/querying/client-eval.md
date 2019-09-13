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
# <a name="client-vs-server-evaluation"></a>İstemci ile Sunucu değerlendirmesi

Entity Framework Core, sorgunun istemci üzerinde değerlendirilmekte olduğunu ve veritabanına itilmekte olan parçalarını destekler. Bu, sorgunun hangi bölümlerinin veritabanında değerlendirileceğini belirleyen veritabanı sağlayıcısına ait olacaktır.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.

## <a name="client-evaluation"></a>İstemci değerlendirmesi

Aşağıdaki örnekte, bir SQL Server veritabanından döndürülen blogların URL 'Lerini standartlaştırmak için bir yardımcı yöntem kullanılır. SQL Server sağlayıcının bu yöntemin nasıl uygulandığı hakkında öngörü olmadığından, SQL 'e çevirmek mümkün değildir. Sorgunun diğer tüm yönleri veritabanında değerlendirilir, ancak bu yöntem aracılığıyla döndürülen `URL` bu yöntemin geçirilmesi istemcisinde yapılır.

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

## <a name="client-evaluation-performance-issues"></a>İstemci değerlendirmesi performans sorunları

İstemci değerlendirmesi çok faydalı olabilir, bazı durumlarda performansı düşük olabilir. Yardımcı yöntemin artık bir filtrede kullanıldığı aşağıdaki sorguyu göz önünde bulundurun. Bu, veritabanında gerçekleştirilemediğinden, tüm veriler belleğe çekilir ve sonra filtre istemciye uygulanır. Veri miktarına ve bu verilerin ne kadarının filtrelendiğine bağlı olarak, bu, performansın düşmesine neden olabilir.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a>İstemci değerlendirme günlüğü

Varsayılan olarak, EF Core istemci değerlendirmesi gerçekleştirildiğinde bir uyarı günlüğe kaydedilir. Günlüğe kaydetme çıkışını görüntüleme hakkında daha fazla bilgi için [günlüğe](../miscellaneous/logging.md) bakın. 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a>İsteğe bağlı davranış: istemci değerlendirmesi için bir özel durum oluşturun

İstemci değerlendirmesi yapıldığında ya da hiçbir şey yapmayan davranışı değiştirebilirsiniz. Bu işlem, bağlam için seçenekleri ayarlarken yapılır-içinde `DbContext.OnConfiguring` `Startup.cs` veya ASP.NET Core kullanıyorsanız.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
