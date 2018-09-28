---
title: Varlıklar - EF6 yükleme ilgili
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: 2d33d9db8acc61f7d556e3eca46b1ea90198723e
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415763"
---
# <a name="loading-related-entities"></a>İlgili varlıklar yükleniyor
Entity Framework, ilgili verileri - yüklenirken eager, yavaş yükleniyor ve açık yükleme yüklemek için üç yol destekler. Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.  

## <a name="eagerly-loading"></a>Eagerly yükleniyor  

İstekli yükleme alınabildiği bir varlık türü için bir sorgu ayrıca ilgili varlıkları sorgunun bir parçası yükleyen bir işlemdir. İstekli yükleme dahil etme yöntemini kullanarak elde edilir. Örneğin, aşağıdaki sorguları, bloglar ve her blog için ilgili tüm gönderileri yükler.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blogs and its related posts
    var blog1 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include(b => b.Posts)
                       .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                        .Include("Posts")
                        .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include("Posts")
                       .FirstOrDefault();
}
```  

INCLUDE System.Data.Entity ad alanındaki bir genişletme yöntemi olduğunu unutmayın kadar bu ad alanı kullanıyorsanız emin olun.  

### <a name="eagerly-loading-multiple-levels"></a>Birden çok düzeyi eagerly yükleniyor  

İlgili varlıkları birden fazla düzeyde eagerly yüklemek mümkündür. Aşağıdaki sorgu, toplama ve başvuru Gezinti özellikleri için bunun nasıl yapılacağını örnekleri gösterir.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                        .Include(b => b.Posts.Select(p => p.Comments))
                        .ToList();

    // Load all users, their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                        .Include("Posts.Comments")
                        .ToList();

    // Load all users, their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

Şu anda hangi ilgili varlıkları yüklenen filtre mümkün olmadığı anlamına unutmayın. Dahil ilgili tüm varlıklar her zaman getirin.  

## <a name="lazy-loading"></a>Yavaş yükleniyor  

Yavaş yükleniyor verebileceğiniz bir varlık veya varlık koleksiyonunu otomatik olarak veritabanından varlık/varlıkları başvuran bir özelliğine erişinceye ilk kez yüklenen bir işlemdir. POCO varlık türleri kullanırken, yavaş yükleniyor proxy türetilen türlerin örneklerini oluşturmak ve ardından yükleme kanca eklemek için sanal özellikleri geçersiz kılma tarafından sağlanır. Örneğin, aşağıda tanımlanan Blog varlık sınıfı kullanırken gönderileri Gezinti özelliğine erişinceye ilk kez ilgili gönderileri yüklenecektir:  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public virtual ICollection<Post> Posts { get; set; }  
}
```  

### <a name="turn-lazy-loading-off-for-serialization"></a>Kapatmak için serileştirme yüklenirken yavaş Aç  

Yavaş yükleniyor ve Serileştirme iyi karıştırmayın ve dikkatli olmazsanız veritabanınız için sorgulama yavaş yükleniyor yalnızca etkin olduğundan yukarı sonlandırabilirsiniz. Çoğu seri hale getiricileri genişletme türünün bir örneğinde her bir özellik erişerek çalışır. Daha fazla varlık serileştirilen şekilde yavaş yükleniyor, özellik erişimi tetikler. Daha fazla varlık yüklendi ve bu varlıkların özellikleri erişilir. Kapalı bir varlık seri hale getirme önce yükleme yavaş etkinleştirmek için iyi bir uygulamadır. Aşağıdaki bölümlerde bunun nasıl yapılacağı gösterilmektedir.  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>Belirli bir gezinti özellikleri yükleniyor yavaş kapatma  

Yavaş yükleniyor gönderileri koleksiyonun gönderileri özelliği sanal olmayan hale getirerek kapatılabilir:  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public ICollection<Post> Posts { get; set; }  
}
```  

Yükleniyor gönderiler koleksiyon hala istekli yükleme kullanarak gerçekleştirilebilir (bkz *Eagerly Yükleniyor* yukarıda) veya Load yöntemi (bkz *açıkça Yükleniyor* aşağıda).  

### <a name="turn-off-lazy-loading-for-all-entities"></a>Tüm varlıklar için yükleme yavaş Kapat  

Gecikmeli yükleme bağlamı tüm varlıklar için yapılandırma özelliği bir bayrak ayarlayarak kapatılabilir. Örneğin:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

İlgili varlıkları yükleme yine de gerçekleştirilebilir istekli yükleme kullanarak (bkz *Eagerly Yükleniyor* yukarıda) veya Load yöntemi (bkz *açıkça yüklenirken* aşağıda).  

## <a name="explicitly-loading"></a>Açıkça yükleniyor  

Devre dışı bile yavaş yükleyerek, gevşek ilgili varlıkları yükleme yine de mümkündür, ancak açık bir çağrı ile yapılmalıdır. Bunu yapmak için ilgili varlığın girişinde yükleme yöntemi kullanın. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

Bir varlık başka bir tek bir varlık Gezinti özelliğine sahip olduğunda başvuru yöntemi kullanılması gerektiğini unutmayın. Öte yandan, bir varlığın bir gezinti özelliği için diğer varlıklar koleksiyonu olduğunda koleksiyon yöntemi kullanılmalıdır.  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>Açıkça ilgili varlıkları yüklenirken uygulanan filtreler  

Sorgu yöntemine, Entity Framework, ilgili varlıkları yüklerken kullanacağınız temel alınan sorgunun erişim sağlar. Ardından LINQ ToList, yük, vb. gibi LINQ genişletme yöntemine bir çağrıyla yürütmeden önce sorguya filtre uygulamak için de kullanabilirsiniz. Sorgu yöntemi ile başvuru hem koleksiyon Gezinti özellikleri kullanılabilir, ancak burada bu koleksiyonun parçası yalnızca yüklenecek kullanılabilir koleksiyonlar için en kullanışlıdır. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
           .Collection(b => b.Posts)
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
           .Collection("Posts")
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();
}
```  

Sorgu yöntemini kullanırken gezinti özelliği için yükleme yavaş kapatmak en iyisidir. Bu durum, aksi takdirde tüm koleksiyon otomatik olarak yavaş yükleme mekanizması tarafından önce veya filtrelenmiş sorgu yürütüldükten sonra yüklenir çünkü.  

İlişki yerine bir lambda ifadesi bir dize olarak belirtilebilir, ancak döndürülen Iqueryable dize kullanıldığında ve faydalı bir şey ile yapılabilir önce Cast yöntemi genellikle gerekli genel olmadığına dikkat edin.  

## <a name="using-query-to-count-related-entities-without-loading-them"></a>İlgili varlıkları yükleme olmadan saymak için sorgu kullanma  

Bazen kaç varlıkları veritabanında başka bir varlık yüklenirken bu varlıkların maliyeti olmaksızın ilgili bilmek de yararlı olabilir. Bunu yapmak için sorgu yöntemi ile LINQ Count yöntemi kullanılabilir. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                           .Collection(b => b.Posts)
                           .Query()
                           .Count();
}
```  
