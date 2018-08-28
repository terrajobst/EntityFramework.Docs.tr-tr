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
# <a name="client-vs-server-evaluation"></a>İstemci vs. Server değerlendirmesi

Entity Framework Core istemcisi ve bunun veritabanına gönderilen parçalarını değerlendirilen sorgunun bölümlerini destekler. Bu veritabanında sorgu hangi parçalarının değerlendirileceğini belirlemek için veritabanı sağlayıcısı kadar olur.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.

## <a name="client-evaluation"></a>İstemci değerlendirmesi

Aşağıdaki örnekte, bir yardımcı yöntem URL'leri farklı bir SQL Server veritabanından döndürülen Web günlükleri için standart hale getirmek için kullanılır. SQL Server sağlayıcısı bu yöntemin nasıl uygulandığını hiçbir Öngörüler olduğundan, SQL'e çevirmek mümkün değildir. Sorgu diğer tüm yönleri değerlendirilir veritabanındaki ancak geçirme döndürülen `URL` istemci üzerinde bu yöntem kullanılarak gerçekleştirilir.

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

## <a name="disabling-client-evaluation"></a>İstemci değerlendirmesi devre dışı bırakma

İstemci değerlendirmesi çok kullanışlı olabilir, ancak bazı durumlarda, performansın düşmesine neden olabilir. Aşağıdaki sorgu, bir filtre yardımcı yöntemi artık kullanıldığı göz önünde bulundurun. Bu veritabanında gerçekleştirilemiyor çünkü tüm verileri, bellek ve ardından filtre çekilir istemcide uygulanır. Veri ve bu verilerin ne kadar filtrenin dışında kaldı miktarına bağlı olarak, bu performansın düşmesine neden olabilir.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

İstemci değerlendirmesi gerçekleştirildiğinde varsayılan olarak, bir uyarı EF Core günlüğe kaydeder. Bkz: [günlüğü](../miscellaneous/logging.md) günlük çıktısı görüntüleme hakkında daha fazla bilgi. İstemci değerlendirmesi durum veya hiçbir şey yapma oluştuğunda davranışını değiştirebilirsiniz. Bağlamınızı - seçeneklerini normalde ayarlarken yapıldığını `DbContext.OnConfiguring`, veya `Startup.cs` ASP.NET Core kullanıyorsanız.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
