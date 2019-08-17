---
title: Ilgili varlıkları yükleme-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: f40034475ed6659b60ab4317605fd1d802218d69
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565324"
---
# <a name="loading-related-entities"></a>Ilgili varlıkları yükleme
Entity Framework, ilgili veri yükleme, yavaş yükleme ve açık yükleme gibi üç yolu destekler. Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.  

## <a name="eagerly-loading"></a>Ekip yükleme  

Eager yüklemesi, bir varlık türü için bir sorgunun aynı zamanda ilgili varlıkları sorgunun bir parçası olarak yüklediği işlemdir. Eager yüklemesi, Include yönteminin kullanımı ile elde edilir. Örneğin, aşağıdaki sorgular blogların ve her blogun ilgili tüm gönderilerin yükünü yükler.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blog and its related posts
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

Include 'in System. Data. Entity ad alanındaki bir genişletme yöntemi olduğunu unutmayın, bu nedenle bu ad alanını kullandığınızdan emin olun.  

### <a name="eagerly-loading-multiple-levels"></a>Birden çok düzey yükleme  

Ayrıca, birden çok ilgili varlık düzeyini de yüklemek mümkündür. Aşağıdaki sorgularda, hem koleksiyon hem de başvuru gezinti özellikleri için bunu nasıl yapabilinin örnekleri gösterilmektedir.  

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

Şu anda hangi ilgili varlıkların yüklendiğini filtrelemek mümkün değildir. Dahil etme her zaman ilgili varlıkların tümünü alacak.  

## <a name="lazy-loading"></a>Geç yükleme  

Geç yükleme, varlığa/varlıklara başvuran bir özelliğe ilk kez erişildiğinde bir varlık veya varlık koleksiyonunun veritabanından otomatik olarak yüklendiğine yönelik bir işlemdir. POCO varlık türlerini kullanırken, yavaş yükleme türetilmiş proxy türlerinin örnekleri oluşturularak ve sonra yükleme kancasını eklemek için sanal özellikleri geçersiz kılarak elde edilir. Örneğin, aşağıda tanımlanan blog varlık sınıfı kullanılırken, gönderi gezintisi özelliği ilk kez erişildiğinde ilgili postalar yüklenir:  

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

### <a name="turn-lazy-loading-off-for-serialization"></a>Serileştirme için yavaş yüklemeyi kapat  

Geç yükleme ve serileştirme iyi bir şekilde karıştırmayın ve unutmayın, yavaş yükleme etkin olduğundan, veritabanınızın tamamına yönelik sorgulama yapabilirsiniz. Çoğu serileştiriciler, bir tür örneğindeki her özelliğe erişerek çalışır. Özellik erişimi, yavaş yüklemeyi tetikler, bu nedenle daha fazla varlık serileştirilmiş hale alınır. Bu varlıklar özelliklerine erişilir, hatta daha fazla varlık yüklenir. Bir varlığı seri hale gelmeden önce yavaş yüklemeyi kapatmak iyi bir uygulamadır. Aşağıdaki bölümlerde bunun nasıl yapılacağı gösterilmektedir.  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>Belirli gezinti özellikleri için yavaş yüklemeyi kapatma  

Gönderi özelliğinin geç yüklemesi, gönderimler özelliği sanal olmayan bir hale getirilerek kapatılabilir:  

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

Gönderi koleksiyonu yüklemesi yine de Eager yüklemesi (bkz. yukarıya *yükleme* ) veya Load yöntemi (bkz. aşağıda *açıkça yükleme* ) kullanılarak elde edilebilir.  

### <a name="turn-off-lazy-loading-for-all-entities"></a>Tüm varlıklar için yavaş yüklemeyi kapat  

Yapılandırma özelliğinde bir bayrak ayarlanarak, bağlam içindeki tüm varlıklar için yavaş yükleme kapatılabilir. Örneğin:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

İlgili varlıkların yüklenmesi yine de Eager yüklemesi (bkz. yukarıya *yükleme* ) veya Load yöntemi (bkz. aşağıda *açıkça yükleme* ) kullanılarak elde edilebilir.  

## <a name="explicitly-loading"></a>Açıkça yükleme  

Yavaş yükleme devre dışı bırakılmış olsa bile, ilgili varlıkların geç yüklenmeye devam edebilir, ancak açık bir çağrıyla yapılması gerekir. Bunu yapmak için, ilgili varlığın girişinde Load yöntemini kullanırsınız. Örneğin:  

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

Bir varlığın başka bir varlığa bir gezinti özelliği olduğunda başvuru yönteminin kullanılması gerektiğini unutmayın. Diğer taraftan, bir varlık başka varlıkların koleksiyonuna bir gezinti özelliği olduğunda, koleksiyon yöntemi kullanılmalıdır.  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>İlgili varlıkları açıkça yüklerken filtre uygulama  

Sorgu yöntemi, ilgili varlıklar yüklenirken Entity Framework kullanacağı temel sorguya erişim sağlar. Daha sonra, ToList, Load, vb. gibi bir LINQ genişletme yöntemine yapılan bir çağrıyla yürütmeden önce sorguya filtre uygulamak için LINQ kullanabilirsiniz. Sorgu yöntemi hem başvuru hem de koleksiyon gezinme özellikleriyle birlikte kullanılabilir, ancak koleksiyonun yalnızca bir bölümünü yüklemek için kullanılabilecek olan koleksiyonlar için en yararlı seçenektir. Örneğin:  

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

Sorgu yöntemi kullanılırken, gezinti özelliği için yavaş yüklemeyi devre dışı bırakmak genellikle en iyisidir. Bunun nedeni, tersi durumda, filtre uygulanmış Sorgu yürütüldükten önce veya sonra yavaş yükleme mekanizması tarafından otomatik olarak yüklenemeyebilir.  

İlişki bir lambda ifadesi yerine bir dize olarak belirtilebildiği sürece, döndürülen IQueryable bir dize kullanıldığında genel değildir ve bu nedenle, bu durumda herhangi bir şeyin yararlı olması için atama yöntemi gerekir.  

## <a name="using-query-to-count-related-entities-without-loading-them"></a>Sorgu kullanarak ilgili varlıkları yüklemeden Sayın  

Bazen, tüm bu varlıkları yükleme maliyetini karşılamak zorunda kalmadan veritabanında bulunan başka bir varlıkla ilgili kaç varlık olduğunu öğrenmek faydalı olur. Bunu yapmak için LINQ Count yöntemine sahip sorgu yöntemi kullanılabilir. Örneğin:  

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
