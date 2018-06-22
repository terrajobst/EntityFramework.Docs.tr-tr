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
# <a name="client-vs-server-evaluation"></a>İstemci vs. Sunucu değerlendirmesi

Entity Framework Çekirdek istemci ve veritabanına iletilmesini bölümleri değerlendirilen sorgunun bölümlerini destekler. Bu sorgu hangi kısımlarının veritabanında değerlendirilecek belirlemek için veritabanı sağlayıcısı kadar olur.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) github'da.

## <a name="client-evaluation"></a>İstemci değerlendirmesi

Aşağıdaki örnekte yardımcı bir yöntem için bir SQL Server veritabanı tarafından döndürülen bloglar URL'leri standart hale getirmek için kullanılır. SQL Server sağlayıcısı bu yöntem nasıl uygulandığını içine hiçbir Insight olduğundan SQL'e Çevir mümkün değil. Tüm sorgu yönlerini değerlendirilir veritabanında ancak geçirme döndürülen `URL` bu yöntemle istemci üzerinde gerçekleştirilir.

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

İstemci değerlendirme çok kullanışlı olabilirler, ancak bazı durumlarda, performansın düşmesine neden olabilir. Burada bir filtrede kullanılan yardımcı yöntemi aşağıdaki sorgu, göz önünde bulundurun. Bu veritabanında gerçekleştirilemediğinden tüm verileri bellek ve ardından filtre çekilir istemcide uygulanır. Veri ve bu verilerin ne kadar filtre miktarına bağlı olarak, bu performansın düşmesine neden olabilir.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

İstemci değerlendirme gerçekleştirildiğinde varsayılan olarak, bir uyarı EF çekirdek günlüğe kaydeder. Bkz: [günlüğü](../miscellaneous/logging.md) günlük çıktısı görüntüleme hakkında daha fazla bilgi. İstemci değerlendirme throw veya hiçbir şey yapma oluştuğunda davranışı değiştirebilirsiniz. Bu genellikle, içeriğiniz - seçeneklerini ayarlama zaman yapılır `DbContext.OnConfiguring`, veya `Startup.cs` ASP.NET Core kullanıyorsanız.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
