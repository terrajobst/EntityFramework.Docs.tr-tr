---
title: İlgili Verilerin Yüklenmesi - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 915aaa41beb495a046f2d6260e9c3b174d5f3031
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417679"
---
# <a name="loading-related-data"></a>İlgili Verileri Yükleme

Entity Framework Core, ilgili varlıkları yüklemek için modelinizdeki gezinti özelliklerini kullanmanıza olanak tanır. İlgili verileri yüklemek için kullanılan üç yaygın O/RM delemi vardır.

* **Eager yükleme,** ilgili verilerin ilk sorgunun bir parçası olarak veritabanından yüklendiği anlamına gelir.
* **Açık yükleme,** ilgili verilerin daha sonra veritabanından açıkça yüklendiği anlamına gelir.
* **Tembel yükleme,** gezinti özelliğine erişildiğinde ilgili verilerin veritabanından saydam olarak yüklendiği anlamına gelir.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub'da görüntüleyebilirsiniz.

## <a name="eager-loading"></a>Istekli yükleme

Sorgu sonuçlarına `Include` dahil edilecek ilgili verileri belirtmek için yöntemi kullanabilirsiniz. Aşağıdaki örnekte, sonuçlarda döndürülen blogların `Posts` özelliği ilgili gönderilerle doldurulur.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core, daha önce bağlam örneğine yüklenen diğer varlıklara gezinme özelliklerini otomatik olarak sabitler. Bu nedenle, bir gezinti özelliğinin verilerini açıkça eklemeseniz bile, ilgili varlıkların bazıları veya tümü daha önce yüklenmişse, özellik yine de doldurulabilir.

Tek bir sorguda birden çok ilişkiden ilgili verileri ekleyebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Birden çok düzey dahil

Yöntemi kullanarak ilişkili verilerin birden çok düzeylerini içerecek `ThenInclude` şekilde ilişkiler arasında ayrıntılı bilgi edinebilirsiniz. Aşağıdaki örnek, tüm blogları, ilgili gönderileri ve her gönderinin yazarını yükler.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

İlgili verilerin daha `ThenInclude` fazla düzeyleri de dahil olmak üzere devam etmek için birden çok çağrı zincirleyebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Tüm bunları, aynı sorguya birden çok düzeyden ve birden çok kökten ilgili verileri içerecek şekilde birleştirebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

Dahil edilen varlıklardan biri için birden çok ilgili varlık eklemek isteyebilirsiniz. Örneğin, `Blogs`sorgularken, `Posts` hem de `Author` `Tags` . `Posts` Bunu yapmak için, kökten başlayan her bir include yolu belirtmeniz gerekir. Örneğin, `Blog -> Posts -> Author` ve `Blog -> Posts -> Tags`. Bu, gereksiz birleştirmeler alacağınız anlamına gelmez; çoğu durumda, EF SQL oluştururken birleştirmeleri birleştirir.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

> [!CAUTION]
> Sürüm 3.0.0'dan `Include` bu yana, her biri ilişkisel sağlayıcılar tarafından üretilen SQL sorgularına ek bir JOIN eklenmesine neden olurken, önceki sürümler ek SQL sorguları oluşturacaktır. Bu, sorgularınızın performansını iyi veya kötü önemli ölçüde değiştirebilir. Özellikle, çok yüksek sayıda `Include` işleç içeren LINQ sorgularının kartezyen patlama sorununu önlemek için birden çok ayrı LINQ sorgusuna ayrılması gerekebilir.

### <a name="include-on-derived-types"></a>Türemiş türlere ekleme

Yalnızca türetilmiş bir türde tanımlanan gezintilerden `Include` `ThenInclude`ilgili verileri ekleyebilirsiniz ve.

Aşağıdaki model göz önüne alındığında:

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

Öğrenci `School` olan tüm kişilerin gezinme içeriği bir dizi desen kullanılarak hevesle yüklenebilir:

* döküm kullanma

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* operatör `as` kullanma

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* bu tür `Include` parametre alır aşırı yük kullanarak`string`

  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a>Açık yükleme

API üzerinden bir gezinti özelliğini `DbContext.Entry(...)` açıkça yükleyebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

Ayrıca, ilgili varlıkları döndüren ayrı bir sorgu gerçekleştirerek bir gezinti özelliğini açıkça yükleyebilirsiniz. Değişiklik izleme etkinse, bir varlık yüklenirken, EF Core otomatik olarak yeni yüklenen entitiy'nin gezinme özelliklerini zaten yüklenmiş olan varlıklara başvurmak üzere ayarlar ve zaten yüklenmiş varlıkların gezinti özelliklerini yeni yüklenen varlığa başvurmak üzere ayarlar.

### <a name="querying-related-entities"></a>İlgili varlıkları sorgulama

Ayrıca, bir gezinti özelliğinin içeriğini temsil eden bir LINQ sorgusu da alabilirsiniz.

Bu, ilgili varlıklar üzerinde bir toplu işleç çalıştırma gibi şeyleri belleğe yüklemeden yapmanızı sağlar.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Ayrıca, hangi ilgili varlıkların belleğe yüklendiğini de filtreleyebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Tembel yükleme

Tembel yükleme kullanmak için en basit yolu [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paketi yükleyerek ve `UseLazyLoadingProxies`bir çağrı ile etkinleştirerek. Örneğin:

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

EF Core daha sonra geçersiz kılınabilen herhangi bir gezinme özelliği için `virtual` tembel yüklemeye olanak sağlayacaktır- yani, olması gereken ve devralınabilecek bir sınıf üzerinde. Örneğin, aşağıdaki varlıklarda, `Post.Blog` ve `Blog.Posts` gezinti özellikleri tembel yüklü olacaktır.

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

### <a name="lazy-loading-without-proxies"></a>Vekiller olmadan tembel yükleme

Tembel yükleme vekilleri, Entity Type `ILazyLoader` [Constructors'da](../modeling/constructors.md)açıklandığı gibi hizmeti bir varlığa enjekte ederek çalışır. Örneğin:

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

Bu, varlık türlerinin devralınmasını veya gezinme özelliklerinin sanal olmasını gerektirmez ve `new` bir bağlama iliştirildikten sonra tembel liğe bağlı olarak oluşturulan varlık örneklerine izin verir. Ancak, `ILazyLoader` [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) paketinde tanımlanan hizmete başvuru gerektirir. Bu paket, bağlı olarak çok az etkisi olacak şekilde en az sayıda tür içerir. Ancak, varlık türlerindeki EF Core paketlerine bağlı kalmaktan tamamen kaçınmak `ILazyLoader.Load` için, yöntemi temsilci olarak enjekte etmek mümkündür. Örneğin:

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

Yukarıdaki kod, `Load` temsilciyi biraz daha temiz kullanmak için bir uzantı yöntemi kullanır:

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
> Tembel yükleme temsilcisi için yapıcı parametre "lazyLoader" olarak adlandırılmalıdır. Yapılandırma bundan farklı bir ad kullanmak için gelecekteki bir sürüm için planlanmıştır.

## <a name="related-data-and-serialization"></a>İlgili veriler ve serileştirme

EF Core navigasyon özelliklerini otomatik olarak düzelteceği için, nesne grafiğinizde döngüler olabilir. Örneğin, bir blogun ve ilgili gönderilerin yüklenmesi, bir gönderi koleksiyonuna başvuran bir blog nesnesine neden olur. Bu mesajların her biri geri bloga bir referans olacaktır.

Bazı serileştirme çerçeveleri bu tür döngülere izin vermez. Örneğin, bir döngü yle karşılaşılırsa, Json.NET aşağıdaki özel durumu oluşturur.

> Newtonsoft.Json.JsonSerializationException: 'MyApplication.Models.Blog' türü ile özellik 'Blog' için algılanan Self referencing döngüsü.

ASP.NET Core kullanıyorsanız, Json.NET nesne grafiğinde bulduğu döngüleri yoksayacak şekilde yapılandırabilirsiniz. Bu `ConfigureServices(...)` yöntemde `Startup.cs`yapılır.

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

Başka bir alternatif, Json.NET serileştirme sırasında `[JsonIgnore]` bu gezinti özelliği ni geçmeyecek şekilde talimat veren öznitelik ile gezinti özelliklerinden birini süslemektir.
