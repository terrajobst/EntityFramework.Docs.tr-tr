---
title: Tasarım zamanı DbContext oluşturma-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f44f0648678af5a70e5171d69692bde1c1d5e0eb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655533"
---
# <a name="design-time-dbcontext-creation"></a>Tasarım Zamanında DbContext Oluşturma

EF Core araçları komutlarından bazıları (örneğin, [geçişler][1] komutları), uygulamanın varlık türleriyle ilgili ayrıntıların toplanması ve bir veritabanı şemasına nasıl eşlendikleri hakkında, tasarım zamanında oluşturulacak türetilmiş bir `DbContext` örneği gerektirir. Çoğu durumda, bu nedenle oluşturulan `DbContext`, [çalışma zamanında nasıl yapılandırılacağından][2]benzer bir şekilde yapılandırılmalıdır.

Araçların `DbContext`oluşturmaya ne kadar farklı şekilde deneme vardır:

## <a name="from-application-services"></a>Uygulama hizmetlerinden

Başlangıç projeniz [ASP.NET Core Web konağını][3] veya [.NET Core genel konağını][4]kullanıyorsa, Araçlar DbContext nesnesini uygulamanın hizmet sağlayıcısından almaya çalışır.

Araçlar önce `Program.CreateHostBuilder()`çağırarak, `Build()`çağırarak `Services` özelliğine erişerek hizmet sağlayıcısını edinmeye çalışır.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs)]

> [!NOTE]
> Yeni bir ASP.NET Core uygulaması oluşturduğunuzda, bu kanca varsayılan olarak dahil edilir.

`DbContext` kendisi ve oluşturucudaki tüm bağımlılıklar uygulamanın hizmet sağlayıcısında hizmet olarak kaydedilmelidir. Bu, bir [`DbContextOptions<TContext>` örneğini bir bağımsız değişken olarak alan][5] ve [`AddDbContext<TContext>` metodunu][6]kullanan `DbContext` bir Oluşturucu bulundurarak kolayca elde edilebilir.

## <a name="using-a-constructor-with-no-parameters"></a>Parametresiz bir Oluşturucu kullanma

DbContext uygulama hizmeti sağlayıcısından alınamıyorsa, Araçlar proje içinde türetilmiş `DbContext` türünü arar. Daha sonra parametresiz bir Oluşturucu kullanarak bir örnek oluşturmaya çalışır. Bu, `DbContext` [`OnConfiguring`][7] yöntemi kullanılarak yapılandırıldıysa varsayılan Oluşturucu olabilir.

## <a name="from-a-design-time-factory"></a>Tasarım zamanı fabrikasından

Ayrıca, `IDesignTimeDbContextFactory<TContext>` arabirimini uygulayarak DbContext 'in nasıl oluşturulacağını de söyleyebilirsiniz: Bu arabirimi uygulayan bir sınıf türetilen `DbContext` aynı projede ya da uygulamanın başlangıç projesinde bulunursa, Araçlar diğerini atlar DbContext oluşturma ve bunun yerine tasarım zamanı fabrikasını kullanma yolları.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs)]

> [!NOTE]
> `args` parametresi şu anda kullanılmıyor. Araçlardan tasarım zamanı bağımsız değişkenlerini belirtme yeteneği izlenirken [bir sorun][8] oluştu.

Tasarım zamanı fabrikası, çalışma zamanından farklı olarak DbContext 'i tasarım zamanı için farklı şekilde yapılandırmanız gerektiğinde kullanışlı olabilir. `DbContext` Oluşturucu ek parametreler alırsa, bu durumda DI 'yi kullanmıyorsanız veya bazı nedenlerden dolayı ASP.NET Core uygulamanızın `Main` sınıfında bir `BuildWebHost` yöntemi bulundurmayı tercih edin.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
