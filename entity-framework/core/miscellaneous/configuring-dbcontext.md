---
title: DbContext yapılandırma EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: e04c1b65df96819f3493e0ed34ccf26609511f6a
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445917"
---
# <a name="configuring-a-dbcontext"></a>DbContext yapılandırma

Bu makalede, belirli bir EF Core sağlayıcısı ve isteğe bağlı davranışları kullanarak bir veritabanına bağlanmak için bir `DbContextOptions` aracılığıyla `DbContext` yapılandırmasına yönelik temel desenler gösterilmektedir.

## <a name="design-time-dbcontext-configuration"></a>Tasarım zamanı DbContext yapılandırması

[Geçiş](xref:core/managing-schemas/migrations/index) gibi EF Core tasarım zamanı araçlarının, uygulamanın varlık türleriyle ilgili ayrıntıları ve bir veritabanı şemasına nasıl eşlendikleri hakkında bilgi toplamak için bir `DbContext` türünün bir çalışma örneğini bulması ve oluşturabilmeleri gerekir. Bu işlem, araç `DbContext` ' y i, çalışma zamanında nasıl yapılandırılacağından benzer şekilde yapılandırılacak şekilde kolayca oluşturabileceğiniz sürece otomatik olabilir.

@No__t-0 ' a gerekli yapılandırma bilgilerini sağlayan herhangi bir desen çalışma zamanında çalışsa da, tasarım zamanında `DbContext` kullanılması gereken araçlar yalnızca sınırlı sayıda desenle çalışabilir. Bunlar [Tasarım zamanı bağlam oluşturma](xref:core/miscellaneous/cli/dbcontext-creation) bölümünde daha ayrıntılı olarak ele alınmıştır.

## <a name="configuring-dbcontextoptions"></a>DbContextOptions 'ı yapılandırma

`DbContext` ' ın herhangi bir çalışmayı gerçekleştirmek için bir `DbContextOptions` örneği olmalıdır. @No__t-0 örneği, şu gibi yapılandırma bilgilerini taşır:

- Kullanılacak veritabanı sağlayıcısı, genellikle `UseSqlServer` veya `UseSqlite` gibi bir yöntem çağırarak seçilir. Bu uzantı yöntemleri, `Microsoft.EntityFrameworkCore.SqlServer` veya `Microsoft.EntityFrameworkCore.Sqlite` gibi karşılık gelen sağlayıcı paketini gerektirir. Yöntemler `Microsoft.EntityFrameworkCore` ad alanında tanımlanır.
- Genellikle yukarıda bahsedilen sağlayıcı seçim yöntemine bir bağımsız değişken olarak geçirilen veritabanı örneğinin gerekli bağlantı dizesi veya tanımlayıcısı
- Tüm sağlayıcı düzeyi isteğe bağlı davranış seçicileri, genellikle sağlayıcı seçim yöntemine yapılan çağrının içinde de zincirleme
- Genel EF Core davranış seçicileri, genellikle sağlayıcı Seçicisi yönteminden sonra veya önce zincirleme

Aşağıdaki örnek, SQL Server sağlayıcıyı kullanmak için @no__t, `connectionString` değişkeninde yer alan bir bağlantı, sağlayıcı düzeyi komut zaman aşımı ve tüm sorguları `DbContext` olmayan bir [izleme](xref:core/querying/tracking#no-tracking-queries) içinde yürüten bir EF Core davranış seçicisine göre yapılandırır. Varsayılan olarak:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Yukarıda bahsedilen sağlayıcı seçici yöntemleri ve diğer davranış Seçicisi yöntemleri `DbContextOptions` veya sağlayıcıya özgü seçenek sınıflarında uzantı yöntemleridir. Bu uzantı yöntemlerine erişebilmek için kapsamda bir ad alanına (genellikle `Microsoft.EntityFrameworkCore`) sahip olmanız ve ek paket bağımlılıklarını projeye eklemeniz gerekebilir.

@No__t-0, `OnConfiguring` yöntemini geçersiz kılarak veya bir Oluşturucu bağımsız değişkeni aracılığıyla `DbContext` ' e sağlanabilir.

Her ikisi de kullanılırsa, `OnConfiguring` en son uygulanır ve Oluşturucu bağımsız değişkenine sağlanan seçeneklerin üzerine yazabilir.

### <a name="constructor-argument"></a>Oluşturucu bağımsız değişkeni

Oluşturucunuzu aşağıdaki şekilde `DbContextOptions` kabul edebilir:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> DbContext 'in temel Oluşturucusu, `DbContextOptions` ' ın genel olmayan sürümünü de kabul eder, ancak birden çok bağlam türüne sahip uygulamalar için genel olmayan sürüm kullanılması önerilmez.

Uygulamanız artık bağlamı örneklediğinde `DbContextOptions` ' ı şu şekilde geçirebilir:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>Onyapılandırıyor

Ayrıca, bağlamı içinde `DbContextOptions` ' yı da başlatabilirsiniz. Temel yapılandırma için bu tekniği kullanabilseniz de, genellikle bir veritabanı bağlantı dizesi gibi, dışarıdaki bazı yapılandırma ayrıntılarını almanız gerekecektir. Bu, bir yapılandırma API 'SI veya başka yollarla yapılabilir.

Bağlam içinde `DbContextOptions` ' ı başlatmak için, `OnConfiguring` yöntemini geçersiz kılın ve belirtilen `DbContextOptionsBuilder` ' deki Yöntemleri çağırın:

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

Bir uygulama, oluşturucusuna hiçbir şey geçirmeden bu bağlamı basit bir şekilde oluşturabilir:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Testler tam veritabanını hedeflemez değilse, bu yaklaşım kendisini teste vermez.

### <a name="using-dbcontext-with-dependency-injection"></a>Bağımlılık ekleme ile DbContext kullanma

EF Core, bağımlılık ekleme kapsayıcısı ile `DbContext` kullanımını destekler. DbContext türü, `AddDbContext<TContext>` yöntemi kullanılarak hizmet kapsayıcısına eklenebilir.

`AddDbContext<TContext>` hem DbContext türünü, `TContext` ' i hem de hizmet kapsayıcısından ekleme için kullanılan `DbContextOptions<TContext>` ' yi kullanılabilir hale getirir.

Bağımlılık ekleme hakkında ek bilgi için aşağıda [daha fazla okuma](#more-reading) bölümüne bakın.

@No__t-0 ' yı bağımlılık eklenmesine ekleme:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Bu, DbContext türüne `DbContextOptions<TContext>` kabul eden bir [Oluşturucu bağımsız değişkeni](#constructor-argument) eklenmesini gerektirir.

Bağlam kodu:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Uygulama kodu (ASP.NET Core):

``` csharp
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

Uygulama kodu (doğrudan ServiceProvider kullanılarak, daha az ortak):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a>DbContext iş parçacığı sorunlarından kaçınma

Entity Framework Core, aynı `DbContext` örneğinde çalışan birden çok paralel işlemi desteklemez. Bu, zaman uyumsuz sorguların paralel yürütmesini ve birden çok iş parçacığından açık olan eşzamanlı kullanımı içerir. Bu nedenle, her zaman `await` zaman uyumsuz çağrı hemen yürütülür veya paralel olarak yürütülen işlemler için ayrı `DbContext` örnekleri kullanır.

EF Core aynı anda `DbContext` örneğini kullanma girişimi algıladığında, şöyle bir ileti içeren bir `InvalidOperationException` görürsünüz: 

> Önceki bir işlem tamamlanmadan önce bu bağlamda ikinci bir işlem başlatıldı. Bu genellikle aynı DbContext örneğini kullanan farklı iş parçacıklarından kaynaklanır, ancak örnek üyelerin iş parçacığı güvenli olduğu garanti edilmez.

Eşzamanlı erişim algılanmadıysa, tanımsız davranışa, uygulama kilitlenmelerine ve veri bozulmasına neden olabilir.

Aynı `DbContext` örneğinde eşzamanlı erişime yanlışlıkla neden olabilecek yaygın hatalar vardır:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Aynı DbContext üzerinde başka bir işlem başlatmadan önce zaman uyumsuz bir işlemin tamamlanmasını beklemek

Zaman uyumsuz yöntemler, EF Core engellenmeyen bir şekilde veritabanına erişen işlemler başlatmasını etkinleştirir. Ancak bir arayan, bu yöntemlerin birinin tamamlanmasını beklemez ve `DbContext` ' da başka işlemler gerçekleştirmeye devam etmez, `DbContext` durumu bozulmuş olabilir (ve büyük olasılıkla). 

Her zaman zaman uyumsuz yöntemleri hemen EF Core.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Bağımlılık ekleme aracılığıyla birden çok iş parçacığında DbContext örneklerini örtülü olarak paylaşma

[@No__t-1](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) uzantı yöntemi, varsayılan olarak [kapsamlı ömrü](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) olan `DbContext` türlerini kaydeder. 

Belirli bir zamanda her istemci isteğini yürüten tek bir iş parçacığı olduğundan ve her istek ayrı bir bağımlılık ekleme kapsamı (ve dolayısıyla ayrı bir `DbContext` örneği) aldığından, bu, ASP.NET Core uygulamalardaki eşzamanlı erişim sorunlarından daha güvenlidir.

Ancak birden çok iş parçacığını paralel olarak doğrudan yürüten kod, `DbContext` örneklerine aynı anda erişilmemesini sağlamalıdır.

Bağımlılık ekleme 'yi kullanarak, bu, her bir iş parçacığı için bağlamı kapsam olarak kaydederek (`IServiceScopeFactory` kullanarak) veya `DbContext` ' i geçici olarak (`ServiceLifetime` parametresi alan `AddDbContext` ' yi kullanarak) kaydederek elde edilebilir.

## <a name="more-reading"></a>Daha fazla okuma

* Dı kullanma hakkında daha fazla bilgi edinmek için [bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) konusunu okuyun.
* Daha fazla bilgi için [testi](testing/index.md) okuyun.
