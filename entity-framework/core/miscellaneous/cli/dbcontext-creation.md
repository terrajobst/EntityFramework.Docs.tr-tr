---
title: Tasarım zamanı DbContext oluşturma - EF çekirdek
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 8b38d300d31038bdf5cd877aa3c42b7f5f97eabc
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
<a name="design-time-dbcontext-creation"></a>Tasarım zamanı DbContext oluşturma
==============================
Bazı EF çekirdek araçları komutlar (örneğin, [geçişler] [ 1] komutları) türetilmiş gerektiren `DbContext` uygulamanın ayrıntılarını toplama amacıyla tasarım zamanında oluşturulacak örneği varlık türlerini ve bunların bir veritabanı şeması nasıl eşleneceğine. Çoğu durumda, tercih edilir, `DbContext` böylece oluşturulan nasıl olacaktır için benzer bir şekilde yapılandırılmış [çalışma zamanında yapılandırılmış][2].

Araçlar deneyin oluşturmak için çeşitli yolları vardır `DbContext`:

<a name="from-application-services"></a>Uygulama hizmetlerinden
-------------------------
Başlangıç projesi ASP.NET Core uygulama ise, uygulamanın hizmet sağlayıcısı'ndan DbContext nesnesi edinmek için Araçlar deneyin.

Aracı ilk deneyin çağırarak hizmet sağlayıcısı edinmek `Program.BuildWebHost()` ve erişme `IWebHost.Services` özelliği.

> [!NOTE]
> Yeni bir ASP.NET Core 2.0 uygulaması oluşturduğunuzda, bu kanca varsayılan olarak dahil edilir. Önceki sürümlerinde EF Core ve ASP.NET Core, çağrılacak araçları deneyin `Startup.ConfigureServices` doğrudan uygulamanın hizmet sağlayıcısı, ancak bu deseni artık almak için düzgün şekilde ASP.NET Core 2.0 uygulamalarda çalışır. ASP.NET Core 1.x uygulamaya 2.0 yükseltiyorsanız, şunları yapabilirsiniz [değiştirmek, `Program` yeni deseni izlemesini sınıfı][3].

`DbContext` Kendisi ve bağımlılıkları kendi oluşturucusuna uygulamanın hizmet sağlayıcısı'nda Hizmetleri olarak kayıtlı olması gerekir. Bu kolayca sağlanarak elde edilebilir [bir oluşturucu `DbContext` örneği alır `DbContextOptions<TContext>` bağımsız değişkeni olarak] [ 4] ve kullanarak [ `AddDbContext<TContext>` yöntemi] [5].

<a name="using-a-constructor-with-no-parameters"></a>Parametresiz bir oluşturucu kullanma
--------------------------------------
DbContext uygulama hizmet sağlayıcısından alınamıyorsa araçları türetilmiş için Ara `DbContext` projesi içinde türü. Ardından parametresiz bir Oluşturucu kullanarak bir örneğini oluşturmak deneyin. Bu varsayılan oluşturucu olabilir `DbContext` kullanılarak yapılandırılmış [ `OnConfiguring` ] [ 6] yöntemi.

<a name="from-a-design-time-factory"></a>Tasarım zamanı fabrikası'ndan
--------------------------
Araçlar uygulayarak, DbContext oluşturmak nasıl anlayabilirsiniz `IDesignTimeDbContextFactory<TContext>` arabirimi: Bu arabirimi uygulayan bir sınıf ya da aynı projesi türetilmiş olarak bulunursa `DbContext` veya uygulamanın başlangıç projesi araçları atla Bunun yerine DbContext ve kullanım tasarım zamanı Fabrika oluşturma diğer yolları.

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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
> `args` Parametresi şu anda kullanılmayan bağlıdır. Yoktur [bir sorun] [ 7] araçları tasarım zamanı bağımsız değişkenlerini belirtme olanağı izleme.

Tasarım zamanı Fabrika DbContext farklı çalışma zamanında tasarım zamanından yapılandırmak, gerekirse çok kullanışlı olabilir `DbContext` ek parametreler kaydedilmedi dı içinde dı hiç kullanmıyorsanız Oluşturucusu alır ya da Eğer bazı için tercih ettiğiniz yüklüyse neden bir `BuildWebHost` ASP.NET Core uygulamanızın yöntemi  
`Main` Sınıf.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
