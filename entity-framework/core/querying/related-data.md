---
title: İlgili verileri - EF Core yükleme
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 590d16902329ffb3fff8026f8dfdcfc887f6dea3
ms.sourcegitcommit: eefcab31142f61a7aaeac03ea90dcd39f158b8b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64873199"
---
# <a name="loading-related-data"></a>İlgili verileri yükleme

Entity Framework Core ilgili varlıkları yükleme için modelinizde gezinme özelliklerini kullanmanıza olanak tanır. İlgili verileri yüklemek için kullanılan üç genel O/RM Düzen vardır.
* **İstekli yükleme** ilgili verileri veritabanından ilk sorgunun bir parçası yüklendiğini anlamına gelir.
* **Açık yükleme** daha sonraki bir zamanda veritabanından, ilgili veriler açıkça yüklendikten anlamına gelir.
* **Yavaş Yükleniyor** gezinme özelliğini erişildiğinde ilgili verileri veritabanından şeffaf bir şekilde yüklendiğini anlamına gelir.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.

## <a name="eager-loading"></a>İstekli yükleme

Kullanabileceğiniz `Include` sorgu sonuçlarında eklenecek ilgili verileri belirtmek için yöntemi. Aşağıdaki örnekte, döndürülen sonuçlarda blogları olacaktır kendi `Posts` özelliği ile ilgili gönderileri doldurulur.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core olacak otomatik olarak düzeltme yukarı Gezinti özellikleri bağlam örneğine önceden yüklenmiş herhangi bir varlık için. Bu nedenle açıkça bir gezinme özelliği için bir veri içermez olsa bile, özellik hala bazıları doldurulabilir veya tüm ilişkili varlıkları önceden yüklenen.


Tek bir sorguda birden çok ilişki ilgili verileri içerebilir.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Birden çok düzeyleri dahil

İlgili verileri kullanarak, birden çok düzeyi içerecek şekilde ilişkileri detaya gidebilirsiniz `ThenInclude` yöntemi. Aşağıdaki örnek, tüm blogları, kendi ilgili gönderileri ve her bir gönderi yazarı yükler.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Visual Studio'nun geçerli sürümleri yanlış kod tamamlama seçenekleri sunar ve söz dizimi hataları ile kullanırken işaretlenmesini doğru ifadeleri neden olabilir `ThenInclude` sonrasına bir koleksiyon gezinme özelliği. Bu, izlenen bir IntelliSense hatanın belirtisidir https://github.com/dotnet/roslyn/issues/8237. Kod doğru olduğundan ve başarıyla derlenen sürece bu alacaklardır söz dizimi hataları yoksaymak güvenlidir. 

Birden çok çağrı zincirleyebilirsiniz `ThenInclude` başka ilgili verileri düzeyini de dahil olmak üzere devam etmek için.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Tüm bunlar aynı sorguda birden çok düzeyi ve birden çok kök ilgili verileri içerecek şekilde birleştirebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Dahil varlıklardan biri için birden çok ilişkili varlıkları içermesi isteyebilirsiniz. Örneğin, sorgulanırken `Blogs`, dahil `Posts` ve ardından her ikisini dahil etmek istediğiniz `Author` ve `Tags` , `Posts`. Bunu yapmak için her ekleme kök dizininde başlangıç yolu belirtmeniz gerekir. Örneğin, `Blog -> Posts -> Author` ve `Blog -> Posts -> Tags`. Bu, yedekli birleştirmeler erişmenizi sağlayacak gelmez; Çoğu durumda, SQL oluştururken, birleştirmeler EF birleştirecek.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Türetilmiş türler

Yalnızca bir türetilmiş bir tür kullanarak tanımlanan gezintiler ilgili verileri içerebilir `Include` ve `ThenInclude`. 

Şu model verilen:

```csharp
public class SchoolContext : DbContext
{
    public DbSet<Person> People { get; set; }
    public DbSet<School> Schools { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
    }
}

public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Student : Person
{
    public School School { get; set; }
}

public class School
{
    public int Id { get; set; }
    public string Name { get; set; }

    public List<Student> Students { get; set; }
}
```

İçeriğini `School` Öğrenciler tüm kişilerin Gezinti eagerly yüklenebilir bir desenlerinin kullanarak:

- Cast kullanma
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- kullanarak `as` işleci
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- aşırı yüklemesini kullanarak `Include` türünde parametre almayan `string`
  ```csharp
  context.People.Include("School").ToList()
  ```

### <a name="ignored-includes"></a>Yoksayılan içerir

Sorgu değiştirirseniz, böylece artık sorgu başlamış varlık türü örneklerini döndürür içerme işleçleri göz ardı edilir.

Ekleme işleçleri dayalı aşağıdaki örnekte, `Blog`, ancak ardından `Select` işleci, anonim bir tür sorguyu değiştirmek için kullanılır. Bu durumda, INCLUDE işleçleri hiçbir etkisi yoktur.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Varsayılan olarak, bir uyarı EF Core başlar.SSH zaman dahil işleçleri yok sayılır. Bkz: [günlüğü](../miscellaneous/logging.md) günlük çıktısı görüntüleme hakkında daha fazla bilgi. Ekle işleci throw veya hiçbir şey yapma göz ardı edilir olduğunda davranışı değiştirebilirsiniz. Bağlamınızı - seçeneklerini normalde ayarlarken yapıldığını `DbContext.OnConfiguring`, veya `Startup.cs` ASP.NET Core kullanıyorsanız.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>açık yükleme

> [!NOTE]  
> Bu özellik, EF Core 1.1 içinde kullanılmaya başlandı.

Bir gezinme özelliği aracılığıyla açıkça yüklemek `DbContext.Entry(...)` API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Bir gezinme özelliği, ilgili varlıkları döndürür, ayrı bir sorgu yürüterek de açıkça yükleyebilirsiniz. Değişiklik izleme etkin sonra bir varlık yüklenirken, EF Core otomatik olarak önceden yüklenmiş tüm varlıklara başvurmak için yeni yüklenen varlık Gezinti özelliklerini ayarlayın ve başvurmak için önceden yüklenmiş varlıkları Gezinti özelliklerini ayarlama Yeni yüklenen varlık.

### <a name="querying-related-entities"></a>İlgili varlıkları sorgulama

Ayrıca, bir gezinti özelliği içeriğini temsil eden bir LINQ Sorgu alabilirsiniz.

Bu belleğe yüklemeden ilgili varlıklar üzerinde bir toplama işleci çalıştırma gibi şeyleri yapmanıza olanak sağlar.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Ayrıca, hangi ilgili varlıkları belleğe yüklenen filtreleyebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Yavaş yükleniyor

> [!NOTE]  
> Bu özellik, EF Core 2.1 içinde kullanılmaya başlandı.

Yavaş yükleniyor kullanmanın en basit yolu yüklemektir [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paket ve çağrısıyla etkinleştirme `UseLazyLoadingProxies`. Örneğin:
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
Veya AddDbContext kullanırken:
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
EF Core olacak etkinleştir--diğer bir deyişle, geçersiz kılınabilir herhangi bir gezinti özelliği için yükleme yavaş olması gerektiğini sonra `virtual` ve öğesinden devralınan bir sınıf. Örneğin, aşağıdaki varlıkların içinde `Post.Blog` ve `Blog.Posts` Gezinti özellikleri, yüklenen yavaş olacaktır.
```csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```
### <a name="lazy-loading-without-proxies"></a>Lazy proxy'si yükleme

Lazy yükleme proxy'leri iş ekleyerek `ILazyLoader` açıklandığı bir varlığa hizmet [varlık türü oluşturucuları](../modeling/constructors.md). Örneğin:
```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
Bu devralınan için varlık türleri veya gezinti özelliklerini sanal gerektirmez ve ile oluşturulan varlık örnekleri sağlar `new` yavaş bir kez yük için bir bağlamına bağlı. Ancak, bir başvuru gerektirir `ILazyLoader` tanımlanan hizmet [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) paket. Çok az etkisi bağlı olarak, yani bu paket türleri en az bir kümesini içerir. Ancak, tüm varlık türlerini EF Core paketlerinde bağlı olarak tamamen önlemek için eklemesine mümkündür `ILazyLoader.Load` yöntemi temsilci olarak. Örneğin:
```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
Yukarıdaki kullanan kodu bir `Load` temsilci biraz Temizleyicisi hale getirmek için genişletme yöntemi:
```csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```
> [!NOTE]  
> Yavaş yükleniyor temsilci Oluşturucu parametresi "lazyLoader" çağrılmalıdır. Bu, gelecekteki sürümlerde sunulması planlanmaktadır olandan farklı bir ad kullanmak için yapılandırma.

## <a name="related-data-and-serialization"></a>İlgili verileri ve Serileştirme

EF Core otomatik olarak düzeltme yukarı Gezinti özelliklerini, döngüleriyle nesne graftaki kalabilirsiniz olduğundan. Örneğin, blog ve kendi ilgili gönderileri yükleme gönderileri koleksiyonunu başvuran bir blog nesneyle sonuçlanacak. Bu gönderilerin her blog geri başvuru gerekir.

Bazı serileştirme çerçeveleri gibi döngüleri izin vermez. Örneğin, bir döngüye ile karşılaşılırsa Json.NET şu özel durum oluşturur.

> Newtonsoft.Json.JsonSerializationException: Kendi kendine başvuran döngü 'Blog' özelliği için 'MyApplication.Models.Blog' türüyle algılandı.

ASP.NET Core kullanıyorsanız, nesne grafiğinde bulduğu döngüleri yok saymak için Json.NET yapılandırabilirsiniz. Bu yapılır `ConfigureServices(...)` yönteminde `Startup.cs`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```

Gezinti özellikleri ile birini donatmak için başka bir alternatiftir `[JsonIgnore]` özniteliği, bu gezinme özelliğinin serileştirirken gezeceği değil Json.NET bildirir.
