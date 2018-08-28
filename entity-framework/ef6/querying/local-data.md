---
title: Yerel veriler - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: dac1a1de20398501c706b118443743d47970df17
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994280"
---
# <a name="local-data"></a>Yerel veriler
Doğrudan olan DB karşı çalışan bir LINQ Sorgu her zaman bir sorgu için veritabanı gönderir, ancak şu anda DbSet.Local özelliğini kullanarak bellek içi verileri erişebilirsiniz. Varlıklarınızı DbContext.Entry ve DbContext.ChangeTracker.Entries yöntemleri kullanma hakkında ek bilgi EF izleme de erişebilirsiniz. Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.  

## <a name="using-local-to-look-at-local-data"></a>Yerel verilere bakmak için yerel kullanma  

Yerel bir özellik olan DB, şu anda bağlam izlendiğini ve silinmiş ' işaretlenmemiş varlıkları kümesinin basit erişim sağlar. Yerel özellik erişim asla veritabanına gönderilmek üzere bir sorgu neden olur. Bu, bir sorgu zaten gerçekleştirildikten sonra genellikle kullanıldığını anlamına gelir. Yük genişletme yöntemi, böylece sonuçları içerik izleyen bir sorgu yürütmek için kullanılabilir. Örneğin:  

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

-'ADO.NET Blog' bir BlogId 1 ile - ve 'Visual Studio Blog' 2'in bir BlogId ile veritabanında iki blog vardı, aşağıdaki çıktıyı bekliyoruz:  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

Bu, üç nokta gösterilmektedir:  

- Bunu henüz veritabanına kaydedilmedi olsa bile yeni blog 'My Yeni Blog' yerel koleksiyona dahil edilir. Veritabanı henüz varlık için gerçek bir anahtar oluşturmamıştır için bu blog sıfır birincil bir anahtar içeriyor.  
- Hala bağlam tarafından izleniyor olsa bile 'ADO.NET Blog' yerel koleksiyona dahil edilmez. Böylece bu silindi olarak işaretleme olan DB kaldırdık olmasıdır.  
- Bir sorguyu gerçekleştirmek için olan DB kullanıldığında (ADO.NET Blog) silinmek üzere işaretlenmiş blog sonuçlarda yer ve veritabanına kaydedilmedi yeni blog (My Yeni Blog) sonuçlarda yer almaz. Olan DB veritabanında bir sorgu gerçekleştiriyor ve her zaman döndürülen sonuçları veritabanında nedir yansıtmak için budur.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>Varlıkları bağlamdan ekleyip için yerel kullanma  

Yerel özelliği olan DB döndürür bir [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) eşitleme bağlamı içeriğini kalacak şekilde ölçekledikçe olayları. Bu varlıklar eklenebilir veya yerel koleksiyon veya olan DB kaldırılması gerektiğini anlamına gelir. Ayrıca, bu varlıklarla güncelleştirilen yerel koleksiyonu bağlamına yeni varlıkları getiren sorgular sonuçlanır anlamına gelir. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework").Load();  

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

'Entity framework' ve 'asp.net' çıkışı ile etiketlenmiş birkaç gönderileri vardı varsayarsak şunun gibi görünebilir:  

```  
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

Bu, üç nokta gösterilmektedir:  

- 'EF yenilikler' yeni gönderiye eklenen yerel koleksiyonu eklendi durumu bağlamda tarafından izlenen olur. SaveChanges çağrıldığında bu nedenle veritabanına eklenir.  
- (EF Başlangıç Kılavuzu) yerel koleksiyondan kaldırıldı post artık bağlam içinde silindi olarak işaretlenir. SaveChanges çağrıldığında bu nedenle veritabanından silinir.  
- İkinci sorgu bağlamla yüklenen ek post (ASP.NET Başlangıç Kılavuzu) otomatik olarak yerel koleksiyona eklenir.  

ObservableCollection performans varlıklar çok sayıda harika değil, çünkü yerel hakkında dikkat edilecek son bir şey olmasıdır. Bu nedenle, bağlam içinde binlerce varlığın ile uğraşıyorsanız yerel bildirilmesi olmayabilir.  

## <a name="using-local-for-wpf-data-binding"></a>WPF veri bağlama için yerel kullanma  

ObservableCollection örneği olduğundan doğrudan WPF uygulamasında veri bağlama için olan DB yerel özelliği kullanılabilir. Bu otomatik olarak kullanacağı anlamına gelir önceki bölümlerde açıklandığı gibi eşitleme bağlamı içeriğinden haberdar olun ve bağlama içeriğini otomatik olarak ile eşitlenmiş durumda kalır. Yerel koleksiyon için yerel veritabanı sorgusu hiçbir zaman neden olduğundan bağlamak için herhangi bir şey olması burada verilerle önceden doldurmak gereken unutmayın.  

Bu tam bir WPF verilerini bağlama örneği için uygun bir yere değildir ancak temel öğeleri şunlardır:  

- Kurulum bağlama kaynağı  
- Yerel kümenize özelliği için bağlama  
- Yerel veritabanına bir sorgu kullanarak doldurun.  

## <a name="wpf-binding-to-navigation-properties"></a>WPF bağlama için Gezinti özellikleri  

Sizin yapmanız durumunda, ana/ayrıntı veri ayrıntılı görünüm birinin varlıklarınızdaki her gezinti özelliği için bağlama isteyebilirsiniz. Bunun çalışmasını sağlamak için kolay bir yolu, gezinti özelliği için ObservableCollection kullanmaktır. Örneğin:  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>SaveChanges varlıklarda temizlemek için yerel kullanma  

Çoğu durumda bir gezinti özelliği kaldırıldı varlıkları otomatik olarak bağlamında silindi olarak işaretlenmez. Gönderin, ardından bir Post nesnesi Blog.Posts koleksiyondan kaldırırsanız SaveChanges çağrıldığında Örneğin, otomatik olarak silinmez. Ardından silinmesi gerekiyorsa bu Sallantıdaki varlıkları bulun ve bunları SaveChanges çağırmadan önce veya geçersiz kılınan bir SaveChanges bir parçası olarak silindi olarak işaretle gerekebilir. Örneğin:  

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

Yukarıdaki kod, tüm gönderileri ve herhangi bir blog başvurusu silindi olarak yok işaretleri bulmak için yerel koleksiyon kullanır. ToList çağrı gereklidir çünkü koleksiyon Kaldır tarafından aksi durumda değiştirilecek numaralandırılan çağırın. Diğer çoğu durumda ToList kullanmadan önce yerel özellik karşı doğrudan sorgulayabilirsiniz.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>Yerel ve ToBindingList için Windows Forms veri bağlama işlemini kullanma  

Windows Forms kullanarak doğrudan ObservableCollection tam uygunlukta veri bağlamayı desteklemez. Ancak, önceki bölümlerde açıklanan tüm avantajlarından yararlanabilmek için veri bağlama için olan DB yerel özelliği kullanabilirsiniz. Bu oluşturan ToBindingList uzantı yöntemiyle yaratılabilir sağlanır bir [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) alacağından yerel ObservableCollection tarafından.  

Bu tam bir Windows Forms veri bağlama örneği için uygun bir yere değildir, ancak temel öğeleri şunlardır:  

- Bir nesne bağlama kaynağı ayarla  
- Yerel kümenize Local.ToBindingList() özelliğine bağlama  
- Veritabanına bir sorgu kullanarak yerel Doldur  

## <a name="getting-detailed-information-about-tracked-entities"></a>İzlenen varlıkları hakkında ayrıntılı bilgi alma  

Bu serideki örneklerin çoğu DbEntityEntry örneği bir varlık için döndürülecek giriş yöntemi kullanın. Bu giriş nesnesi, ardından geçerli durumu gibi varlık hakkında bilgi toplamak için yanı sıra bir ilgili varlığa açıkça yükleme gibi varlık üzerinde işlem gerçekleştirmek için başlangıç noktası olarak görev yapar.  

Giriş yöntemleri, bağlam tarafından izleniyor çoğu veya tüm varlıklar için DbEntityEntry nesneleri döndürür. Bu, bilgi toplamak veya birçok varlığın yalnızca tek bir giriş yazmak yerine işlemleri sağlar. Örneğin:  

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

Bir yazar ve okuyucu sınıfı örnek sunuyoruz - bu sınıfların her ikisi de IPerson arabirimini uygulayan göreceksiniz.  

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

Aşağıdaki veriler bir veritabanında sahibiz varsayalım:

Blog BlogId ile = 1 ve adı 'ADO.NET Blog' =  
Blog BlogId ile = 2 ve adı 'Visual Studio Blog' =  
Blog BlogId ile 3 ve adı = '.NET Framework blogu' =  
Yazar ile AuthorId = 1 ve adı 'ALi Bloggs' =  
Okuyucu ile ReaderId = 1 ve ad = 'Turgay elmas'  

Bir kod çalıştırmasını çıktı aşağıdaki gibi olur:  

```  
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

Bu örnekler birkaç nokta gösterir:  

- Giriş yöntemleri girişleri varlıklar için silinen dahil olmak üzere tüm durumlarda döndürür. Karşılaştırmak için bu dışlar yerel varlıkları silindi.  
- Genel olmayan girişler yöntemi kullanıldığında tüm varlık türlerini girişlerinde döndürülür. Genel girişleri yöntemi kullanıldığında girişleri genel tür örneği olan varlıklar için yalnızca döndürülür. Bu yukarıdaki tüm blogları girişlerini almak için kullanıldı. Ayrıca, girişlerini IPerson uygulamak için tüm varlıkları almak için kullanıldı. Bu, genel tür, gerçek varlık türü yok gösterir.  
- Nesnelere LINQ, döndürülen sonuçları filtrelemek için kullanılabilir. Bunu yukarıda değiştirilmeden sürece herhangi bir türde varlıkları bulmak için kullanıldı.  

DbEntityEntry örneklerinin her zaman null olmayan bir varlık içerdiğini unutmayın. Bu filtre gerekmez. Bu nedenle ilişki girişlerinin ve saplama girdileri DbEntityEntry örnekleri olarak temsil edilmez.
