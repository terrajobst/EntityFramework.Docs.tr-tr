---
title: Tasarım zamanında DbContext oluşturma - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 7c16017d3b97d115841050fe6ac0fdbeb5e71d94
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067537"
---
<a name="design-time-dbcontext-creation"></a>Tasarım zamanında DbContext oluşturma
==============================
EF Core araçları komutlardan bazıları (örneğin, [geçişler] [ 1] komutları) türetilmiş gerektiren `DbContext` uygulamanın ayrıntılarını toplamak için tasarım zamanında oluşturulacak örnek varlık türleri ve bunların veritabanı şemasına nasıl eşleneceğine. Çoğu durumda, tercih edilir, `DbContext` böylece oluşturulan nasıl olacaktır benzer bir biçimde yapılandırılmış [çalışma zamanında yapılandırılmış][2].

Araçları deneyebilirsiniz oluşturmak için çeşitli yollar vardır `DbContext`:

<a name="from-application-services"></a>Uygulama Hizmetleri'nden
-------------------------
Başlangıç projeniz bir ASP.NET Core uygulaması ise, uygulamanın hizmet sağlayıcısından DbContext nesnesini almak Araçlar'ı deneyin.

Aracı ilk deneyin çağırarak hizmet sağlayıcısı edinmek `Program.BuildWebHost()` erişerek `IWebHost.Services` özelliği.

> [!NOTE]
> Yeni bir ASP.NET Core 2.0 uygulama oluşturduğunuzda, bu kanca varsayılan olarak dahil edilir. Önceki sürümlerinde EF Core ve ASP.NET Core, çağrılacak araçları deneyebilirsiniz `Startup.ConfigureServices` doğrudan uygulamanın hizmet sağlayıcısı, ancak bu düzen artık almak için düzgün şekilde ASP.NET Core 2.0 uygulamaları çalışır. ASP.NET Core 1.x uygulamaya 2.0 yükseltiyorsanız, şunları yapabilirsiniz [değiştirmek, `Program` yeni desende sınıfı][3].

`DbContext` Kendisi ve herhangi bir bağımlılığın oluşturucusuna Hizmetleri'nde uygulamanın hizmet sağlayıcısı olarak kaydedilmesi gerekir. Bu kolayca sağlanarak elde edilebilir [bir oluşturucu `DbContext` örneğini alır `DbContextOptions<TContext>` bir bağımsız değişken olarak] [ 4] ve kullanarak [ `AddDbContext<TContext>` yöntemi] [5].

<a name="using-a-constructor-with-no-parameters"></a>Parametresiz bir oluşturucu kullanılarak
--------------------------------------
Uygulama hizmet sağlayıcısından DbContext alınamıyorsa araçları için türetilmiş Ara `DbContext` türü proje içinde. Ardından parametresiz bir Oluşturucusu kullanarak örneği oluşturmayı deneyin. Bu varsayılan oluşturucu olabilir `DbContext` kullanılarak yapılandırılan [ `OnConfiguring` ] [ 6] yöntemi.

<a name="from-a-design-time-factory"></a>Tasarım zamanı fabrikadan
--------------------------
Uygulayarak, DbContext oluşturma araçları da söyleyebilirsiniz `IDesignTimeDbContextFactory<TContext>` arabirimi: Bu arabirimi uygulayan bir sınıfa veya türetilmiş projenin bulunursa `DbContext` veya uygulamanın başlangıç projesi araçları atlama DbContext ve tasarım zamanı Fabrika kullanmak yerine oluşturmanın diğer yolu.

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
> `args` Parametresi, şu anda kullanılmayan. Var. [soruna] [ 7] izleme araçları tasarım zamanı bağımsız değişkenleri belirtme olanağı.

Tasarım zamanı Fabrika DbContext çalışma zamanında tasarım zamanından farklı şekilde yapılandırmak, gerekirse özellikle kullanışlı olabilir `DbContext` ek parametreler kaydedilmedi DI içinde DI hiç kullanmıyorsanız Oluşturucusu alır ya da Eğer bazı için tercih ettiğiniz yüklüyse neden bir `BuildWebHost` ASP.NET Core uygulamanızın yönteminde `Main` sınıfı.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
