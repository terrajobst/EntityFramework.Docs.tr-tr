---
title: Ilgili verileri yükleme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 4bf9598f9b7e74c2835d3926215de9a7ef4e6f96
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921799"
---
# <a name="loading-related-data"></a>Ilgili verileri yükleme

Entity Framework Core, ilişkili varlıkları yüklemek için modelinizdeki gezinti özelliklerini kullanmanıza olanak sağlar. İlgili verileri yüklemek için kullanılan üç ortak O/RM deseni vardır.
* **Eager yüklemesi** , ilgili verilerin ilk sorgunun parçası olarak veritabanından yüklendiği anlamına gelir.
* **Açık yükleme** , ilgili verilerin daha sonra veritabanından açıkça yüklendiği anlamına gelir.
* **Yavaş yükleme** , gezinti özelliğine erişildiğinde ilgili verilerin veritabanından saydam olarak yüklendiği anlamına gelir.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.

## <a name="eager-loading"></a>ekip yükleme

Sorgu sonuçlarına dahil edilecek `Include` ilgili verileri belirtmek için yöntemini kullanabilirsiniz. Aşağıdaki örnekte, sonuçlarda döndürülen blogların `Posts` özelliği ilgili gönderileriyle doldurulmuş olacaktır.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core, gezinti özelliklerini daha önce bağlam örneğine yüklenmiş diğer varlıklara otomatik olarak düzeltir. Bu nedenle, bir gezinti özelliği için verileri açıkça bulundurmasanız bile, ilgili varlıkların bazıları veya tümü daha önce yüklenmişse Özellik yine de doldurulmuş olabilir.


Tek bir sorgudaki birden fazla ilişkilerden ilgili verileri dahil edebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Birden çok düzey dahil

`ThenInclude` Yöntemini kullanarak ilgili verilerin birden fazla düzeyini dahil etmek için ilişkilerde ayrıntıya gidebilirsiniz. Aşağıdaki örnek tüm blogları, ilgili yayınlarını ve her gönderiye ait yazarı yükler.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Visual Studio 'nun geçerli sürümleri yanlış kod tamamlama seçenekleri sunar ve bir koleksiyon gezintisi özelliğinden sonra `ThenInclude` yöntemi kullanılırken, doğru ifadelerin sözdizimi hatalarıyla işaretlenmesine neden olabilir. Bu, tarihinde https://github.com/dotnet/roslyn/issues/8237 izlenen bir IntelliSense hata belirtisidir. Kodun doğru olduğu ve başarıyla derlenemediği sürece bu, söz dizimi hatalarının yoksayılıp yoksayılması güvenlidir. 

Daha fazla ilgili veri düzeyi dahil `ThenInclude` devam etmek için birden çok çağrısı zincirleyebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Aynı sorguda birden çok düzeyden ve birden çok kökten ilgili verileri dahil etmek için tüm bunları birleştirebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

Dahil edilen varlıklardan biri için birden fazla ilgili varlık eklemek isteyebilirsiniz. Örneğin `Blogs`, sorgulama `Author` `Tags` yaparken, ve ' nin`Posts`her ikisini de dahil etmek istersiniz. `Posts` Bunu yapmak için, kökden başlayarak her bir içerme yolunu belirtmeniz gerekir. Örneğin, `Blog -> Posts -> Author` ve `Blog -> Posts -> Tags`. Bu, gereksiz birleşimler alacağınız anlamına gelmez; Çoğu durumda, SQL oluştururken EF birleştirmeleri birleştirecek.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Türetilmiş türlere dahil et

Yalnızca ve `Include` `ThenInclude`kullanarak türetilmiş bir tür üzerinde tanımlanan gezintilerden ilgili verileri dahil edebilirsiniz. 

Aşağıdaki model veriliyor:

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

Öğrenciler olan tüm kişilerin gezinmesininiçerikleri,çeşitlidesenlerkullanılarakoluşturulabilir:`School`

- cast kullanma
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- Using `as` işleci
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- türü parametre alan `Include` aşırı yüklemesini kullanma`string`
  ```csharp
  context.People.Include("School").ToList()
  ```

### <a name="ignored-includes"></a>Yoksayılan ekler

Sorguyu artık sorgunun başladığı varlık türünün örneklerini döndürmez şekilde değiştirirseniz, içerme işleçleri yok sayılır.

Aşağıdaki örnekte, Include işleçleri öğesine dayalıdır `Blog`, ancak `Select` sonra işleci, anonim bir tür döndürecek şekilde sorgu değiştirmek için kullanılır. Bu durumda, içerme işleçlerinin hiçbir etkisi yoktur.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Varsayılan olarak, içerme işleçleri göz ardı edildiğinde EF Core bir uyarı kaydeder. Günlüğe kaydetme çıkışını görüntüleme hakkında daha fazla bilgi için [günlüğe](../miscellaneous/logging.md) bakın. Ekleme veya hiçbir şey yapma için bir içerme işlecinin yoksayılması durumunda davranışı değiştirebilirsiniz. Bu işlem, bağlam için seçenekleri ayarlarken yapılır-içinde `DbContext.OnConfiguring` `Startup.cs` veya ASP.NET Core kullanıyorsanız.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>açık yükleme

> [!NOTE]  
> Bu özellik EF Core 1,1 ' de tanıtılmıştı.

`DbContext.Entry(...)` API aracılığıyla bir gezinti özelliğini açıkça yükleyebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

Ayrıca, ilişkili varlıkları döndüren ayrı bir sorgu yürüterek bir gezinti özelliğini açıkça yükleyebilirsiniz. Değişiklik izleme etkinse, bir varlık yüklenirken EF Core, önceden yüklenmiş olan varlıkların gezinti özelliklerini otomatik olarak ayarlar ve bu, önceden yüklenmiş varlıkların gezinti özelliklerini, daha önce yüklenmiş varlıklara başvuracak şekilde ayarlar. Yeni yüklenen varlık.

### <a name="querying-related-entities"></a>İlgili varlıkları sorgulama

Ayrıca, bir gezinti özelliğinin içeriğini temsil eden bir LINQ sorgusu da edinebilirsiniz.

Bu, ilgili varlıklar üzerinde bir toplama işlecini belleğe yüklemeden çalıştırmak gibi işlemleri yapmanızı sağlar.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Ayrıca, hangi ilgili varlıkların belleğe yükleneceğini de filtreleyebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>geç yükleme

> [!NOTE]  
> Bu özellik EF Core 2,1 ' de tanıtılmıştı.

Geç yüklemeyi kullanmanın en basit yolu, [Microsoft. EntityFrameworkCore. proxy](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paketini yüklemek ve ' a çağrı `UseLazyLoadingProxies`yaparak bunu yapmanızı sağlar. Örneğin:
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
AddDbContext kullanırken:
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
EF Core, geçersiz kılınabilen herhangi bir gezinti özelliği için yavaş yüklemeyi etkinleştirir-Yani, `virtual` ve öğesinden devralınabilen bir sınıf üzerinde olmalıdır. Örneğin, aşağıdaki varlıklarda `Post.Blog` ve `Blog.Posts` gezinti özellikleri yavaş yüklenir.
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
### <a name="lazy-loading-without-proxies"></a>Proxy olmadan yavaş yükleme

Yavaş yükleme proxy 'leri, `ILazyLoader` [varlık türü oluşturucuları](../modeling/constructors.md)bölümünde açıklandığı gibi hizmeti bir varlığa ekleme. Örneğin:
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
Bu, varlık türlerinin veya gezinti özelliklerinden devralınmasını gerektirmez ve ile `new` oluşturulan varlık örneklerinin bir içeriğe eklendikten sonra geç yükleme yapmasına izin verir. Ancak, `ILazyLoader` [Microsoft. entityframeworkcore. soyutlamalar](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) paketinde tanımlanan hizmete bir başvuru gerektirir. Bu paket, buna bağlı olarak çok az etkisi olması için en az bir tür kümesi içerir. Ancak, varlık türlerindeki tüm EF Core paketlerine bağlı olarak tamamen kaçınmak için, `ILazyLoader.Load` yöntemi bir temsilci olarak eklemek mümkündür. Örneğin:
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
Yukarıdaki kod, temsilciyi bir `Load` bit temizleyici kullanarak yapmak için bir genişletme yöntemi kullanır:
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
> Yavaş yükleme temsilcisinin Oluşturucu parametresine "lazyLoader" adı verilmelidir. Daha sonraki bir sürüm için bu değerden farklı bir ad kullanacak şekilde yapılandırma.

## <a name="related-data-and-serialization"></a>İlgili veriler ve serileştirme

EF Core, gezinti özelliklerini otomatik olarak düzeltiğinden, nesne grafiğinizde döngülerle bitilecektir. Örneğin, bir blog ve ilgili gönderimler yükleme, bir gönderi koleksiyonuna başvuran bir blog nesnesine neden olur. Bu gönderilerin her birinin bloguna bir başvurusu olacaktır.

Bazı serileştirme çerçeveleri bu tür döngülerine izin vermez. Örneğin, bir döngüyle karşılaşılırsa Json.NET aşağıdaki özel durumu oluşturur.

> Newtonsoft. JSON. JsonSerializationException: ' MyApplication. modeller. blog ' türündeki ' blog ' özelliği için kendine başvuran bir döngü algılandı.

ASP.NET Core kullanıyorsanız, Json.NET 'ı nesne grafiğinde bulduğu döngüleri yoksayacak şekilde yapılandırabilirsiniz. Bu, içindeki `ConfigureServices(...)` `Startup.cs`yönteminde yapılır.

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

Diğer bir seçenek de, bir gezinti özelliklerinden `[JsonIgnore]` birini, JSON.net, serileştirilirken Bu gezinti özelliğinde çapraz geçiş yapmasına yönlendiren özniteliğiyle süslemesine yönelik bir alternatiftir.
