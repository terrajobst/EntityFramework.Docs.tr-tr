---
title: "Tasarım zamanı DbContext oluşturma - EF çekirdek"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a>Tasarım zamanı DbContext oluşturma
==============================
EF tasarım sırasında oluşturulacak bir DbContext örnek komutları gerektiren araçların bazıları (örneğin, geçişler komutları çalışırken) süresi. Oluşturmak için Araçlar deneyin çeşitli yolları vardır.

<a name="from-application-services"></a>Uygulama hizmetlerinden
-------------------------
Başlangıç projesi ASP.NET Core uygulama ise, uygulamanın hizmet sağlayıcısı'ndan DbContext nesnesi edinmek için Araçlar deneyin. Bunlar çağırarak elde `Program.BuildWebHost()` ve erişme `IWebHost.Services` özelliği. Tüm DbContext kullanarak kayıtlı `IServiceCollection.AddDbContext<TContext>()` bulundu ve bu şekilde oluşturulur. Bu desen edildi [ASP.NET Core 2. 0 ' sunulan][1]

<a name="using-the-default-constructor"></a>Varsayılan Oluşturucu kullanma
-----------------------------
DbContext uygulama hizmet sağlayıcısından alınamıyorsa araçları projesi içinde DbContext türünü arayın. Bunlar, kendi varsayılan oluşturucusunu kullanarak oluşturmayı deneyin.

<a name="from-a-design-time-factory"></a>Tasarım zamanı fabrikası'ndan
--------------------------
Araçlar uygulayarak, DbContext oluşturmak nasıl anlayabilirsiniz `IDesignTimeDbContextFactory`. Bu arabirimi uygulayan bir sınıf projenizi içinde bulunursa, Araçlar DbContext oluşturma diğer yolları atlama.
Her zaman Fabrika tasarım zamanında kullanırlar. Bir Fabrika DbContext farklı çalışma zamanında tasarım zamanından yapılandırmak gerekirse özellikle yararlı olur.

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

> [!NOTE]
> `args` Parametresi şu anda kullanılmayan bağlıdır. Yoktur [bir sorun] [ 2] araçları tasarım zamanı bağımsız değişkenlerini belirtme olanağı izleme.

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
