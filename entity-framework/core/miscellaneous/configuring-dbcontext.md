---
title: DbContext yapılandırma EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 734acad86e364abbfd1522fe79d4a847b1acfb52
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149027"
---
# <a name="configuring-a-dbcontext"></a>DbContext yapılandırma

Bu makalede belirli bir EF Core sağlayıcısı ve isteğe `DbContext` bağlı davranışları `DbContextOptions` kullanarak bir veritabanına bağlanmak için bir ile yapılandırma için temel desenler gösterilmektedir.

## <a name="design-time-dbcontext-configuration"></a>Tasarım zamanı DbContext yapılandırması

EF Core tasarım zamanı araçlarının, uygulamanın varlık türleriyle ilgili ayrıntıları ve bir veritabanı şemasına nasıl eşlendikleri hakkında bilgi toplamak için `DbContext` bir türün çalışma örneğini bulması ve oluşturabilmeleri [gerekir.](xref:core/managing-schemas/migrations/index) Bu işlem, araç, `DbContext` çalışma zamanında nasıl yapılandırılacağından benzer şekilde yapılandırılacak şekilde kolayca oluşturabileceğiniz sürece otomatik olabilir.

İçin gerekli yapılandırma bilgilerini sağlayan herhangi bir `DbContext` `DbContext` desen çalışma zamanında çalışsa da, tasarım zamanı için kullanılması gereken araçlar yalnızca sınırlı sayıda desenle çalışabilir. Bunlar [Tasarım zamanı bağlam oluşturma](xref:core/miscellaneous/cli/dbcontext-creation) bölümünde daha ayrıntılı olarak ele alınmıştır.

## <a name="configuring-dbcontextoptions"></a>DbContextOptions 'ı yapılandırma

`DbContext`herhangi bir çalışmayı gerçekleştirmek için `DbContextOptions` bir örneğine sahip olmalıdır. Örnek `DbContextOptions` , şu şekilde yapılandırma bilgilerini taşır:

- Kullanılacak veritabanı sağlayıcısı, genellikle `UseSqlServer` veya `UseSqlite`gibi bir yöntemi çağırarak seçilir. Bu uzantı yöntemleri, `Microsoft.EntityFrameworkCore.SqlServer` veya `Microsoft.EntityFrameworkCore.Sqlite`gibi karşılık gelen sağlayıcı paketini gerektirir. Yöntemler `Microsoft.EntityFrameworkCore` ad alanında tanımlanmıştır.
- Genellikle yukarıda bahsedilen sağlayıcı seçim yöntemine bir bağımsız değişken olarak geçirilen veritabanı örneğinin gerekli bağlantı dizesi veya tanımlayıcısı
- Tüm sağlayıcı düzeyi isteğe bağlı davranış seçicileri, genellikle sağlayıcı seçim yöntemine yapılan çağrının içinde de zincirleme
- Genel EF Core davranış seçicileri, genellikle sağlayıcı Seçicisi yönteminden sonra veya önce zincirleme

Aşağıdaki örnek, SQL Server sağlayıcıyı `DbContextOptions` , `connectionString` değişkende yer alan bir bağlantıyı, sağlayıcı düzeyi komut zaman aşımını ve `DbContext` tüm sorguları ' de yürütülen bir EF Core davranış seçicisini kullanacak şekilde yapılandırır. Varsayılan olarak [izleme yok](xref:core/querying/tracking#no-tracking-queries) :

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Yukarıda bahsedilen sağlayıcı Seçicisi yöntemleri ve diğer davranış Seçicisi yöntemleri, uzantı yöntemleri `DbContextOptions` veya sağlayıcıya özgü seçenek sınıflarıdır. Bu uzantı yöntemlerine erişebilmek için, kapsamda bir ad alanına (genellikle `Microsoft.EntityFrameworkCore`) sahip olmanız ve ek paket bağımlılıklarını projeye eklemeniz gerekebilir.

, Yöntemi geçersiz kılarak `DbContext` veya dışarıdan bir Oluşturucu bağımsız değişkeni aracılığıyla sağlanabilir. `OnConfiguring` `DbContextOptions`

Her ikisi de kullanılırsa, `OnConfiguring` en son uygulanır ve Oluşturucu bağımsız değişkenine sağlanan seçeneklerin üzerine yazabilir.

### <a name="constructor-argument"></a>Oluşturucu bağımsız değişkeni

Oluşturucuya sahip bağlam kodu:

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
> DbContext 'in temel Oluşturucusu, genel `DbContextOptions`olmayan sürümünü de kabul eder, ancak birden çok bağlam türüne sahip uygulamalar için genel olmayan sürüm kullanılması önerilmez.

Oluşturucu bağımsız değişkeninden başlatılacak uygulama kodu:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>Onyapılandırıyor

Bağlam kodu `OnConfiguring`:

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

Öğesini başlatmak `DbContext` `OnConfiguring`için kullanılan uygulama kodu:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Testler tam veritabanını hedeflemez değilse, bu yaklaşım kendisini teste vermez.

### <a name="using-dbcontext-with-dependency-injection"></a>Bağımlılık ekleme ile DbContext kullanma

EF Core, bağımlılık `DbContext` ekleme kapsayıcısı ile kullanmayı destekler. DbContext türü, `AddDbContext<TContext>` yöntemi kullanılarak hizmet kapsayıcısına eklenebilir.

`AddDbContext<TContext>`hem DbContext türü, `TContext` `DbContextOptions<TContext>` hem de hizmet kapsayıcısından ekleme için kullanılabilir duruma gelir.

Bağımlılık ekleme hakkında ek bilgi için aşağıda [daha fazla okuma](#more-reading) bölümüne bakın.

`DbContext` To bağımlılığı ekleme:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Bu, kabul `DbContextOptions<TContext>`eden DbContext türüne bir [Oluşturucu bağımsız değişkeni](#constructor-argument) eklenmesini gerektirir.

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

Entity Framework Core, aynı `DbContext` örnekte çalıştırılan birden çok paralel işlemi desteklemez. Bu, zaman uyumsuz sorguların paralel yürütmesini ve birden çok iş parçacığından açık olan eşzamanlı kullanımı içerir. Bu nedenle, `await` her zaman zaman uyumsuz çağrılar hemen, `DbContext` paralel olarak yürütülen işlemler için ayrı örnekler kullanır.

EF Core aynı anda bir `DbContext` örneği kullanma girişimi algıladığında şöyle bir ileti `InvalidOperationException` görürsünüz: 

> Önceki bir işlem tamamlanmadan önce bu bağlamda ikinci bir işlem başlatıldı. Bu genellikle aynı DbContext örneğini kullanan farklı iş parçacıklarından kaynaklanır, ancak örnek üyelerin iş parçacığı güvenli olduğu garanti edilmez.

Eşzamanlı erişim algılanmadıysa, tanımsız davranışa, uygulama kilitlenmelerine ve veri bozulmasına neden olabilir.

Aynı `DbContext` örnekte yanlışlıkla eşzamanlı erişime neden olabilecek yaygın hatalar vardır:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Aynı DbContext üzerinde başka bir işlem başlatmadan önce zaman uyumsuz bir işlemin tamamlanmasını beklemek

Zaman uyumsuz yöntemler, EF Core engellenmeyen bir şekilde veritabanına erişen işlemler başlatmasını etkinleştirir. Ancak bir arayan, bu yöntemlerin birinin tamamlanmasını beklemez ve üzerinde `DbContext`başka işlemler gerçekleştirmeye devam etmez, durumu `DbContext` (ve büyük olasılıkla) bozulmuş olabilir. 

Her zaman zaman uyumsuz yöntemleri hemen EF Core.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Bağımlılık ekleme aracılığıyla birden çok iş parçacığında DbContext örneklerini örtülü olarak paylaşma

Genişletme [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) yöntemi, varsayılan `DbContext` olarak [kapsamlı yaşam süresine](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) sahip türleri kaydeder. 

Bu, belirli bir zamanda her istemci isteğini yürüten tek bir iş parçacığı olduğundan ve her istek ayrı bir bağımlılık ekleme kapsamı (ve dolayısıyla ayrı `DbContext` bir bağımlılık ekleme kapsamı) aldığından, ASP.NET Core uygulamalardaki eşzamanlı erişim sorunlarından güvenlidir. örnek).

Ancak birden çok iş parçacığını paralel olarak yürüten herhangi bir kod, `DbContext` örneklere hiçbir zaman eş zamanlı olarak erişilmemesini sağlamalıdır.

Bağımlılık ekleme 'yi kullanarak bu, her bir `IServiceScopeFactory` `DbContext` `AddDbContext` işparçacığıiçinbağlamıkapsamolarakkaydederek(kullanarak)yadageçiciolarakkaydederekeldeedilebilir.`ServiceLifetime` parametresi).

## <a name="more-reading"></a>Daha fazla okuma

* Dı kullanma hakkında daha fazla bilgi edinmek için [bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) konusunu okuyun.
* Daha fazla bilgi için [testi](testing/index.md) okuyun.
