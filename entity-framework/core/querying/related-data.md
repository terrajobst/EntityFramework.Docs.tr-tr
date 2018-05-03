---
title: Yükleme ilgili verileri - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 5f1fb9376300739ab0e306d9d60e7ec71aa2d2e7
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
# <a name="loading-related-data"></a>İlgili verileri yükleniyor

Entity Framework Çekirdek, gezinti özellikleri ilgili varlıklar yüklemek için modelinizde kullanmanıza olanak sağlar. İlgili verileri yüklemek için kullanılan üç ortak O/RM desenler vardır.
* **İstekli yükleme** ilgili verileri ilk sorgu bir parçası olarak veritabanından yüklenen anlamına gelir.
* **Açık yükleme** ilgili verileri veritabanından açıkça daha sonraki bir zamanda yüklenip yüklenmediğine anlamına gelir.
* **Yavaş Yükleniyor** gezinti özelliği erişildiğinde ilgili verileri veritabanından saydam yüklenen anlamına gelir.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) github'da.

## <a name="eager-loading"></a>istekli yükleniyor

Kullanabileceğiniz `Include` yöntemi sorgu sonuçlarında dahil edilecek ilgili verileri belirtin. Aşağıdaki örnekte, döndürülen sonuçlarda bloglar olacaktır kendi `Posts` özelliği ile ilgili gönderileri doldurulur.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Çekirdek otomatik olarak düzeltme yukarı Gezinti özellikleri bağlam örneğine önceden yüklenmiş herhangi bir varlık için. Bu nedenle bir gezinme özelliği için veri açıkça içerme olsa bile, özellik hala bazıları doldurulmuş veya tüm ilgili varlıklar daha önce yüklenen.


Tek bir sorguda birden çok ilişki ilgili verileri içerebilir.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Birden çok düzeyi de dahil olmak üzere

Birden çok düzeyinden birini kullanarak ilgili verileri içerecek şekilde ilişkileri ayrıntıya girebilirsiniz `ThenInclude` yöntemi. Aşağıdaki örnek, tüm blogları, kendi ilgili gönderileri ve her posta yazarı yükler.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Visual Studio'nun geçerli sürümleri yanlış kod tamamlama seçenekleri sunar ve sözdizimi hataları ile kullanırken işaretlenmesini doğru ifadeleri neden olabilir `ThenInclude` yöntemi sonra bir koleksiyon gezinme özelliği. Bu bir adresindeki izlenen bir IntelliSense hatanın belirtisidir https://github.com/dotnet/roslyn/issues/8237. Kod doğru olduğundan ve başarıyla derlenen sürece bu alacaklardır sözdizimi hataları yoksaymak güvenlidir. 

Birden fazla çağrı zincir `ThenInclude` daha ilgili verileri düzeylerini dahil olmak üzere devam etmek için.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Bu aynı sorguda birden çok düzeyi ve birden çok kök ilgili verileri içerecek şekilde tüm birleştirebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Dahil varlıklar biri için birden çok ilgili varlık eklemek isteyebilirsiniz. Örneğin, sorgulanırken `Blog`s, eklediğiniz `Posts` ve ardından her ikisi de dahil etmek istediğiniz `Author` ve `Tags` , `Posts`. Bunu yapmak için her dahil kökte başlayan yolunu belirtmeniz gerekir. Örneğin, `Blog -> Posts -> Author` ve `Blog -> Posts -> Tags`. Bu, çoğu durumda EF birleştirmek yedekli birleştirmeler SQL oluştururken birleştirmeler alırsınız gelmez.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Türetilen türler

Yalnızca bir türetilmiş bir tür kullanarak tanımlanan gezintilerini ilgili verileri içerebilir `Include` ve `ThenInclude`. 

Aşağıdaki model verilen:

```Csharp
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

İçeriği `School` Gezinti Öğrenciler tüm kişilerin isteğini önleyebiliriz yüklenebilir bir desenlerinin kullanarak:

- Cast kullanma
  ```Csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- kullanarak `as` işleci
  ```Csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- aşırı yüklemesini kullanarak `Include` türünde bir parametre alır `string`
  ```Csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a>Göz ardı içerir

Böylece artık sorgu ile başlamıştır varlık türünün örneklerini döndürür sorgu değiştirirseniz, INCLUDE işleçleri göz ardı edilir.

INCLUDE işleçleri aşağıdaki örnekte, temel alan `Blog`, ancak ardından `Select` işleci anonim bir tür döndürmek için sorguyu değiştirmek için kullanılır. Bu durumda, INCLUDE işleçleri hiçbir etkisi yoktur.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Varsayılan olarak, bir uyarı EF çekirdek oturum zaman dahil işleçleri yok sayılır. Bkz: [günlüğü](../miscellaneous/logging.md) günlük çıktısı görüntüleme hakkında daha fazla bilgi. Ekle işleci throw veya hiçbir şey yapma göz ardı davranışı değiştirebilirsiniz. Bu genellikle, içeriğiniz - seçeneklerini ayarlama zaman yapılır `DbContext.OnConfiguring`, veya `Startup.cs` ASP.NET Core kullanıyorsanız.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Açık yükleniyor

> [!NOTE]  
> Bu özellik EF çekirdek 1.1 sunulmuştur.

Bir gezinti özelliği aracılığıyla açıkça yükleyebilir `DbContext.Entry(...)` API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

İlgili varlıklar döndürüyor ayrı bir sorgu yürüterek bir gezinti özelliği de açıkça yükleyebilirsiniz. Değişiklik izleme etkinleştirildi, varsa bir varlık yüklenirken EF çekirdek otomatik olarak zaten yüklenmiş herhangi bir varlık başvurmak için yeni yüklenen entitiy Gezinti özelliklerini ayarlamak ve başvurmak için daha önce yüklenmiş varlıklar Gezinti özelliklerini ayarlama Yeni yüklenen varlık.

### <a name="querying-related-entities"></a>İlgili varlıklar sorgulama

Ayrıca bir gezinti özelliği içeriğini temsil eden bir LINQ Sorgu alabilirsiniz.

Bu belleğe yüklemeden ilgili varlıklar üzerinde bir toplama işleci çalıştırma gibi şeyler olanak sağlar.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Ayrıca, hangi ilgili varlıklar belleğe yüklenen filtre uygulayabilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>yavaş yükleniyor

> [!NOTE]  
> Bu özellik EF çekirdek 2.1 içinde sunulmuştur.

Yükleme yavaş kullanmak için en basit yolu yüklemektir [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paket ve çağrısıyla etkinleştirme `UseLazyLoadingProxies`. Örneğin:
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
Ya da AddDbContext kullanırken:
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
EF çekirdek sonra yükleme yavaş olan kılınabilen--herhangi gezinme özelliği için etkinleştir, olmalıdır `virtual` ve öğesinden devralınan bir sınıf. Örneğin, aşağıdaki varlıklar olarak `Post.Blog` ve `Blog.Posts` Gezinti özellikleri yüklenen yavaş olacaktır.
```Csharp
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
### <a name="lazy-loading-without-proxies"></a>Lazy yükleme proxy'leri olmadan

Lazy yükleme proxy'leri iş ekleyerek `ILazyLoader` açıklandığı gibi bir varlık kümesine hizmet [varlık türü oluşturucuları](../modeling/constructors.md). Örneğin:
```Csharp
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
        get => LazyLoader?.Load(this, ref _posts);
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
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
Bu varlık türleri kaynağından devralındı ya da gezinti özellikleri sanal olmasını gerektirmez ve ile oluşturulan varlık örnekleri sağlar `new` yavaş bir kez yük bağlı bir bağlam. Ancak, bir başvuru gerektirir `ILazyLoader` , varlık türleri EF çekirdek derlemeye tüm çiftler hizmetini. Bu EF çekirdek önlemek için verir `ILazyLoader.Load` temsilci olarak eklenemeyebilir yöntemi. Örneğin:
```Csharp
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
        get => LazyLoader?.Load(this, ref _posts);
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
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
Yukarıdaki kullanan kodu bir `Load` genişletme yöntemi temsilci biraz temizleyici kullanarak yapmak için:
```Csharp
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
> Yükleme yavaş temsilci Oluşturucu parametresi "lazyLoader" çağrılmalıdır. Gelecekteki bir sürümde bu planlanan farklı bir ad kullanmak üzere yapılandırma.

## <a name="related-data-and-serialization"></a>İlgili tüm verileri ve seri hale getirme

EF çekirdek işlem otomatik olarak düzeltme yukarı Gezinti özellikleri, döngü ile nesne grafiğinde düşebilir olduğundan. Örneğin, bir blog ve yükleme gönderileri gönderileri koleksiyonu başvuruda bulunan bir blog nesnesinde sonuçlanır ilişkilidir. Bu gönderileri her bir başvuru geri blog sahip olur.

Bazı serileştirme çerçeveler gibi döngüleri izin vermez. Örneğin, bir döngü karşılaştıysanız Json.NET şu özel durum oluşturur.

> Newtonsoft.Json.JsonSerializationException: Kendi kendine başvuran döngü 'Blog' özelliği için 'MyApplication.Models.Blog' türüyle algılandı.

ASP.NET Core kullanıyorsanız, nesne grafiğinde bulduğu döngüleri yoksaymak için Json.NET yapılandırabilirsiniz. Bu yapılır `ConfigureServices(...)` yönteminde `Startup.cs`.

``` csharp
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
