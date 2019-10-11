---
title: Yerel veri-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: efd646348d8a18bbeed2d0a0e708d4d36eb26eac
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182426"
---
# <a name="local-data"></a>Yerel Veriler
LINQ sorgusunun doğrudan bir DbSet 'e karşı çalıştırılması veritabanına her zaman bir sorgu gönderir, ancak şu anda DbSet. local özelliğini kullanarak bellekteki verilere erişebilirsiniz. DbContext. Entry ve DbContext. ChangeTracker. Entries yöntemlerini kullanarak varlıklarınız hakkında daha fazla bilgi için EF 'e de erişebilirsiniz. Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.  

## <a name="using-local-to-look-at-local-data"></a>Yerel verilere bakmak için yerel kullanma  

DbSet 'in yerel özelliği, şu anda bağlam tarafından izlenmekte olan ve silinmiş olarak işaretlenmeyen, küme varlıklarına basit erişim sağlar. Yerel özelliğe erişim hiçbir şekilde bir sorgunun veritabanına gönderilmesine neden olmaz. Bu, genellikle bir sorgu daha önce gerçekleştirildikten sonra kullanıldığı anlamına gelir. Yük uzantısı metodu, içeriğin sonuçları izlemesi için bir sorgu yürütmek üzere kullanılabilir. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs from the database into the context
    context.Blogs.Load();

    // Add a new blog to the context
    context.Blogs.Add(new Blog { Name = "My New Blog" });

    // Mark one of the existing blogs as Deleted
    context.Blogs.Remove(context.Blogs.Find(1));

    // Loop over the blogs in the context.
    Console.WriteLine("In Local: ");
    foreach (var blog in context.Blogs.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }

    // Perform a query against the database.
    Console.WriteLine("\nIn DbSet query: ");
    foreach (var blog in context.Blogs)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }
}
```  

' ADO.NET blog ' veritabanında iki blogumuz varsa blogID 1 ve ' a Visual Studio blogu ' ile bir blogID 2-şu çıktıyı bekleyebilir:  

```console
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

Bu üç noktayı gösterir:  

- Yeni blog ' yeni blogum ', henüz veritabanına kaydedilmese bile yerel koleksiyona dahil edilir. Veritabanı henüz varlık için gerçek bir anahtar oluşturulmadığı için bu blogun birincil anahtarı sıfır.  
- ' ADO.NET blogu ', bağlam tarafından izlenmekte olmasına rağmen yerel koleksiyona dahil edilmez. Bunun nedeni, bunu DbSet 'ten kaldırdığımız ve bu nedenle silinmiş olarak işaretliyoruz.  
- DbSet bir sorgu gerçekleştirmek için kullanıldığında, silinmek üzere işaretlenen blog (ADO.NET blogu) sonuçlara dahil edilir ve henüz veritabanına kaydedilmemiş olan yeni blog (yeni blogum) sonuçlara dahil edilmez. Bunun nedeni, DbSet 'in veritabanına karşı bir sorgu gerçekleştirme ve döndürülen sonuçların her zaman veritabanında neler olduğunu yansıtır.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>Bağlamdan varlık eklemek ve kaldırmak için yerel kullanma  

DbSet üzerindeki yerel özellik, olayların içeriğiyle eşitlenmiş olarak kalacak şekilde, olayları bağlayan bir [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) döndürür. Bu, varlıkların yerel koleksiyona ya da DbSet 'e eklenebileceği veya kaldırılabileceği anlamına gelir. Ayrıca, yeni varlıkları içeriğine getiren sorguların, yerel koleksiyonun bu varlıklarla güncelleştirilmesine neden olacağı anlamına gelir. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework")).Load();  

    // Get the local collection and make some changes to it
    var localPosts = context.Posts.Local;
    localPosts.Add(new Post { Name = "What's New in EF" });
    localPosts.Remove(context.Posts.Find(1));  

    // Loop over the posts in the context.
    Console.WriteLine("In Local after entity-framework query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }

    var post1 = context.Posts.Find(1);
    Console.WriteLine(
        "State of post 1: {0} is {1}",
        post1.Name,  
        context.Entry(post1).State);  

    // Query some more posts from the database
    context.Posts.Where(p => p.Tags.Contains("asp.net").Load();  

    // Loop over the posts in the context again.
    Console.WriteLine("\nIn Local after asp.net query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }
}
```  

' Entity-Framework ' ve ' asp.net ' ile etiketlenmiş birkaç gönderi olduğunu varsayarsak, çıktı şuna benzer olabilir:  

```console
In Local after entity-framework query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
State of post 1: EF Beginners Guide is Deleted

In Local after asp.net query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
Found 4: ASP.NET Beginners Guide with state Unchanged
```  

Bu üç noktayı gösterir:  

- Yerel koleksiyona eklenen yeni gönderi ' EF 'teki yenilikler ', eklenen durumdaki bağlam tarafından izlenir. Bu, SaveChanges çağrıldığında veritabanına eklenecektir.  
- Yerel koleksiyondan kaldırılan gönderi (EF son kullanım kılavuzu) artık bağlamda silinmiş olarak işaretlendi. Bu, SaveChanges çağrıldığında veritabanından silinir.  
- İkinci sorgu ile bağlam içine yüklenen ek gönderi (ASP.NET yeni başlayanlar kılavuzu), yerel koleksiyona otomatik olarak eklenir.  

Yerel hakkında aklınızda bir son şey, ObservableCollection bir performans olduğundan çok sayıda varlık için harika değildir. Bu nedenle, bağlamdaki binlerce varlıkla uğraşıyorsanız yerel kullanılması önerilmez.  

## <a name="using-local-for-wpf-data-binding"></a>WPF veri bağlaması için yerel kullanma  

DbSet üzerindeki Local özelliği, bir WPF uygulamasında bir ObservableCollection örneği olduğundan doğrudan veri bağlama için kullanılabilir. Önceki bölümlerde açıklandığı gibi bu, içeriğin içeriğiyle eşitlenmiş olarak kalacağı ve bağlam içeriğinin otomatik olarak onunla eşitlenmiş kalacağı anlamına gelir. Yerel koleksiyonun, bir veritabanı sorgusuna hiçbir şey neden olmadığı için bağlanacak bir şey olması için verilerle önceden doldurulması gerektiğini unutmayın.  

Bu, tam bir WPF veri bağlama örneği için uygun bir yer değildir ancak anahtar öğeleri şunlardır:  

- Bağlama kaynağı ayarlama  
- Bu dosyayı, kendi ayarladığınız yerel özelliğine bağlayın  
- Veritabanını bir sorgu kullanarak yerel olarak doldurun.  

## <a name="wpf-binding-to-navigation-properties"></a>Gezinti özelliklerine WPF bağlama  

Ana/ayrıntı veri bağlama yapıyorsanız, ayrıntı görünümünü varlıklarınızın bir gezinti özelliğine bağlamak isteyebilirsiniz. Bu işi yapmanın kolay bir yolu, gezinti özelliği için bir ObservableCollection kullanmaktır. Örneğin:  

``` csharp
public class Blog
{
    private readonly ObservableCollection<Post> _posts =
        new ObservableCollection<Post>();

    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual ObservableCollection<Post> Posts
    {
        get { return _posts; }
    }
}
```  

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>SaveChanges 'da varlıkları temizlemek için yerel kullanma  

Çoğu durumda, bir gezinti özelliğinden kaldırılan varlıklar bağlamda silinmiş olarak otomatik olarak işaretlenmez. Örneğin, blog. gönderimleri koleksiyonundan bir post nesnesini kaldırırsanız, SaveChanges çağrıldığında bu gönderi otomatik olarak silinmez. Bunun silinmeli olması gerekiyorsa, bu salgze varlıklarını bulmanız ve SaveChanges 'ı veya geçersiz kılınan SaveChanges 'un bir parçası olarak bu varlıkları silinmek üzere işaretlemeniz gerekebilir. Örneğin:  

``` csharp
public override int SaveChanges()
{
    foreach (var post in this.Posts.Local.ToList())
    {
        if (post.Blog == null)
        {
            this.Posts.Remove(post);
        }
    }

    return base.SaveChanges();
}
```  

Yukarıdaki kod, tüm gönderileri bulmak için yerel koleksiyonu kullanır ve silinen bir blog başvurusu olmayan her türlü işareti işaretler. ToList çağrısı gereklidir çünkü aksi takdirde koleksiyon, numaralandırılmakta olan kaldırma çağrısı tarafından değiştirilir. Çoğu durumda, ToList kullanmadan doğrudan yerel özelliğe karşı sorgulayabilirsiniz.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>Windows Forms veri bağlama için yerel ve ToBindingList kullanma  

Windows Forms doğrudan ObservableCollection kullanarak tam aslına uygunluk verileri bağlamayı desteklemez. Ancak, önceki bölümlerde açıklanan tüm avantajları almak için veri bağlama için DbSet yerel özelliğini kullanmaya devam edebilirsiniz. Bu, yerel ObservableCollection tarafından desteklenen bir [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) uygulamasını oluşturan ToBindingList Extension yöntemi aracılığıyla elde edilir.  

Bu, tam Windows Forms veri bağlama örneği için uygun bir yer değildir ancak anahtar öğeleri şunlardır:  

- Nesne bağlama kaynağı ayarlama  
- Yerel. ToBindingList () kullanarak kendi ayarladığınız Yerel özelliğe bağlayın  
- Veritabanını bir sorgu kullanarak yerel olarak doldur  

## <a name="getting-detailed-information-about-tracked-entities"></a>İzlenen varlıklar hakkında ayrıntılı bilgi alma  

Bu dizideki birçok örnek, bir varlık için DbEntityEntry örneği döndürmek üzere entry metodunu kullanır. Bu giriş nesnesi daha sonra varlık hakkında geçerli durumu gibi bilgi toplamak için başlangıç noktası olarak, varlık üzerinde de ilgili bir varlığı açıkça yükleme gibi işlemler gerçekleştirmek için de çalışır.  

Giriş yöntemleri, bağlam tarafından izlenen birçok veya tüm varlık için DbEntityEntry nesneleri döndürür. Bu, yalnızca tek bir giriş yerine birçok varlık üzerinde bilgi toplamanıza veya işlem gerçekleştirmenize olanak tanır. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    // Load some entities into the context
    context.Blogs.Load();
    context.Authors.Load();
    context.Readers.Load();

    // Make some changes
    context.Blogs.Find(1).Title = "The New ADO.NET Blog";
    context.Blogs.Remove(context.Blogs.Find(2));
    context.Authors.Add(new Author { Name = "Jane Doe" });
    context.Readers.Find(1).Username = "johndoe1987";

    // Look at the state of all entities in the context
    Console.WriteLine("All tracked entities: ");
    foreach (var entry in context.ChangeTracker.Entries())
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Find modified entities of any type
    Console.WriteLine("\nAll modified entities: ");
    foreach (var entry in context.ChangeTracker.Entries()
                              .Where(e => e.State == EntityState.Modified))
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Get some information about just the tracked blogs
    Console.WriteLine("\nTracked blogs: ");
    foreach (var entry in context.ChangeTracker.Entries<Blog>())
    {
        Console.WriteLine(
            "Found Blog {0}: {1} with original Name {2}",
            entry.Entity.BlogId,  
            entry.Entity.Name,
            entry.Property(p => p.Name).OriginalValue);
    }

    // Find all people (author or reader)
    Console.WriteLine("\nPeople: ");
    foreach (var entry in context.ChangeTracker.Entries<IPerson>())
    {
        Console.WriteLine("Found Person {0}", entry.Entity.Name);
    }
}
```  

Örneğin, bu sınıfların her ikisi de IPerson arabirimini uygulayan bir yazar ve okuyucu sınıfı sunuyoruz.  

``` csharp
public class Author : IPerson
{
    public int AuthorId { get; set; }
    public string Name { get; set; }
    public string Biography { get; set; }
}

public class Reader : IPerson
{
    public int ReaderId { get; set; }
    public string Name { get; set; }
    public string Username { get; set; }
}

public interface IPerson
{
    string Name { get; }
}
```  

Veritabanında aşağıdaki verilere sahip olduğunu varsayalım:

BlogID = 1 ve ad = ' ADO.NET blogu ' ile blog  
BlogID = 2 ve ad = ' Visual Studio blogu ' ile blog  
BlogID = 3 ve ad = ' .NET Framework blog ' ile blog  
AuthorId = 1 ve Name = ' ali Bloggs ' olan yazar  
Readerıd = 1 ve Name = ' John tikan ' ile okuyucu  

Kodu çalıştırmanın çıkışı şöyle olacaktır:  

```console
All tracked entities:
Found entity of type Blog with state Modified
Found entity of type Blog with state Deleted
Found entity of type Blog with state Unchanged
Found entity of type Author with state Unchanged
Found entity of type Author with state Added
Found entity of type Reader with state Modified

All modified entities:
Found entity of type Blog with state Modified
Found entity of type Reader with state Modified

Tracked blogs:
Found Blog 1: The New ADO.NET Blog with original Name ADO.NET Blog
Found Blog 2: The Visual Studio Blog with original Name The Visual Studio Blog
Found Blog 3: .NET Framework Blog with original Name .NET Framework Blog

People:
Found Person John Doe
Found Person Joe Bloggs
Found Person Jane Doe
```  

Bu örneklerde çeşitli noktaları gösterilmektedir:  

- Giriş yöntemleri, silinen dahil olmak üzere tüm durumlarda varlıkların girişlerini döndürür. Bunu, silinen varlıkları dışlayan yerel ile karşılaştırın.  
- Tüm varlık türlerinin girişleri, genel olmayan girişler yöntemi kullanıldığında döndürülür. Genel girişler yöntemi kullanılan girişler yalnızca genel türün örnekleri olan varlıklar için döndürülür. Bu, tüm blogların girişlerini almak için yukarıda kullanıldı. Ayrıca, IPerson uygulayan tüm varlıkların girişlerini almak için de kullanılır. Bu, genel türün gerçek bir varlık türü olması gerektiğini gösterir.  
- LINQ to Objects döndürülen sonuçları filtrelemek için kullanılabilir. Bu, değiştirildikleri sürece herhangi bir türdeki varlıkları bulmak için yukarıda kullanılmıştır.  

DbEntityEntry örneklerinin her zaman null olmayan bir varlık içerdiğini unutmayın. İlişki girişleri ve saplama girdileri, DbEntityEntry örnekleri olarak temsil edilmez, bu nedenle bunlara filtre uygulamanız gerekmez.
