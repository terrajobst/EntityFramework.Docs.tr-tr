---
title: Kendi test double - EF6 ile test etme
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 2158dc73585c2720e7293096b0478c73edf522d9
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490915"
---
# <a name="testing-with-your-own-test-doubles"></a>Kendi test Double ile test etme
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.  

Uygulamanız için testleri yazarken genellikle veritabanı ulaşmaktan kaçınmak için tercih edilir.  Entity Framework, bir bağlam oluşturarak – – testleriniz tarafından tanımlanan davranışı ile bunu başarmak için bellek içi verileri kullanır, sağlar.  

## <a name="options-for-creating-test-doubles"></a>Test double oluşturmak için Seçenekler  

Bağlamınızı bir bellek içi sürümünü oluşturmak için kullanılan iki farklı yaklaşım vardır.  

- **Kendi test double oluşturma** – bu yaklaşım, kendi bellek içi uygulama içeriği ve DbSets yazma içerir. Bu, çok nasıl sınıfları davranır ancak yazmayı ve makul bir kod sahip olan içerebilir denetim sağlar.  
- **Sahte bir çerçeve test double oluşturulacağı** – sahte bir çerçeve (örneğin, Moq) kullanarak, bellek içi uygulamaları, içerik ve çalışma zamanında dinamik olarak için oluşturulan kümeleri olabilir.  

Bu makale, kendi test çift oluşturma ile ilgilenecektir. Sahte bir çerçeve kullanma hakkında bilgi için bkz. [sahte bir Framework ile test](mocking.md).  

## <a name="testing-with-pre-ef6-versions"></a>EF6 öncesi sürümler ile test etme  

Bu makalede gösterilen kod EF6 ile uyumludur. EF5 ve önceki sürümü ile test etmek için bkz: [sahte bir bağlamla test](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>EF bellek içi test çiftten oluşan sınırlamaları  

Bellek içi test double birim testi, uygulamanızın EF kullanan bit düzeyi kapsamını sağlamanın iyi bir yolu olabilir. Ancak, bunun yapılması, LINQ to Objects'in bellek içi veri sorguları yürütmek için kullanırsınız. Bu sorgular, veritabanınızda çalıştırın SQL küçültmesini EF'ın LINQ sağlayıcısı (LINQ to Entities) kullanmaktan farklı bir davranış neden olabilir.  

Böyle bir fark örneği ilgili verileri yükleniyor. Bir dizi blog oluşturursanız her ilgili gönderileri ve bellek içi verileri kullanırken ilgili postaların her zaman her Blog için yüklenecek. Dahil etme yöntemini kullanırsanız, ancak bir veritabanıyla çalışırken veriler yalnızca yüklenir.  

Bu nedenle, her zaman doğru bir veritabanında, uygulama çalışır emin olmak için uçtan uca (ek olarak, birim testleri) test belirli bir düzeyde dahil etmek için önerilir.  

## <a name="following-along-with-this-article"></a>Bu makaleyi izleyerek  

Bu makalede istiyorsanız takip etmek için Visual Studio'ya kopyalayabilirsiniz tam kod listeleri sağlar. Oluşturmak en kolayıdır bir **birim testi projesi** ihtiyacınız ve hedef **.NET Framework 4.5** kullanan zaman uyumsuz bölümlerin tamamlanması.  

## <a name="creating-a-context-interface"></a>Bir bağlam arabirimi oluşturma  

Bunu bir EF kullanan bir hizmet sınama sırasında aranacak dağıtacağız modeli. Test etmek için bir bellek içi sürüm bizim EF bağlam değiştirmek mümkün olması için size bir arabirim tanımlarsınız imeplement bizim EF bağlam (ve onun bellek içi çift) olur.  

Test kullanacağız Hizmeti sorgu ve bizim bağlam olan DB özelliklerini kullanarak verileri değiştirme ve veritabanına değişiklikleri gönderme SaveChanges de çağırır. Biz bu üyeler arabirimde şekilde dahil.  

``` csharp
using System.Data.Entity;

namespace TestingDemo
{
    public interface IBloggingContext
    {
        DbSet<Blog> Blogs { get; }
        DbSet<Post> Posts { get; }
        int SaveChanges();
    }
}
```  

## <a name="the-ef-model"></a>EF modeli  

Test etmek için yapacağımız hizmeti bir EF yararlanır modeli BloggingContext ve Blog ve gönderi sınıfları oluşur. Bu kod EF Designer tarafından oluşturulmuş olabilir veya bir Code First modeli.  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext, IBloggingContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }
        public string Url { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }
}
```  

### <a name="implementing-the-context-interface-with-the-ef-designer"></a>EF Designer ile bağlam arabirimini uygulama  

Bizim bağlam IBloggingContext arabirim uyguladığını unutmayın.  

Code First kullanıyorsanız, doğrudan arabirim uygulamak için Bağlamınızı düzenleyebilirsiniz. EF Designer kullanıyorsanız Bağlamınızı oluşturan T4 şablonu düzenlemeniz gerekir. Açık yukarı \<model_adı\>. Edmx dosyası altında iç içe Context.tt dosya, aşağıdaki kod parçası bulun ve arabiriminde gösterildiği gibi ekleyin.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a>Test edilecek hizmeti  

Bellek içi test Double ile test göstermek için birkaç test için bir BlogService sağladığım için kullanacağız. (GetAllBlogs) adına göre sıralanmış tüm blogları döndüren ve yeni blogları (AddBlog) istemcilerinizle bir hizmettir. GetAllBlogs yanı sıra, ayrıca zaman uyumsuz olarak (GetAllBlogsAsync) adına göre sıralanmış tüm blogları alacak bir yöntem sağladık.  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private IBloggingContext _context;

        public BlogService(IBloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = new Blog { Name = name, Url = url };
            _context.Blogs.Add(blog);
            _context.SaveChanges();

            return blog;
        }

        public List<Blog> GetAllBlogs()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return query.ToList();
        }

        public async Task<List<Blog>> GetAllBlogsAsync()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return await query.ToListAsync();
        }
    }
}
```  

<a name="creating-the-in-memory-test-doubles"/> ## Bellek içi test oluşturma iki katına çıkar  

Gerçek EF modeli ve kullanabileceği bir hizmet sunuyoruz, bellek içi test ediyoruz test için kullanabileceğiniz çift oluşturmak zaman var. TestContext test çift bizim bağlam için oluşturduk. Testleri desteklemek için istediğimiz davranış seçmek için aldığımız test double içinde çalıştırmak için kullanacağız. Bu örnekte biz yalnızca SaveChanges adlı kaç kez yakalayacağınızı, ancak Test senaryosu doğrulamak için gerekli mantığı ekleyebilirsiniz.  

Ayrıca, bellek içi uygulaması olan DB sağlayan bir TestDbSet oluşturduk. Tüm yöntemleri için eksiksiz bir düzeyi (bulma dışında) olan DB üzerinde sunduk ancak test senaryonuz kullanacağı üyeleri uygulamak yeterlidir.  

TestDbSet, zaman uyumsuz sorgular işlenebilir emin olmak için ekledik bazı diğer altyapı sınıfları kullanır.  

``` csharp
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class TestContext : IBloggingContext
    {
        public TestContext()
        {
            this.Blogs = new TestDbSet<Blog>();
            this.Posts = new TestDbSet<Post>();
        }

        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
        public int SaveChangesCount { get; private set; }
        public int SaveChanges()
        {
            this.SaveChangesCount++;
            return 1;
        }
    }

    public class TestDbSet<TEntity> : DbSet<TEntity>, IQueryable, IEnumerable<TEntity>, IDbAsyncEnumerable<TEntity>
        where TEntity : class
    {
        ObservableCollection<TEntity> _data;
        IQueryable _query;

        public TestDbSet()
        {
            _data = new ObservableCollection<TEntity>();
            _query = _data.AsQueryable();
        }

        public override TEntity Add(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Remove(TEntity item)
        {
            _data.Remove(item);
            return item;
        }

        public override TEntity Attach(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Create()
        {
            return Activator.CreateInstance<TEntity>();
        }

        public override TDerivedEntity Create<TDerivedEntity>()
        {
            return Activator.CreateInstance<TDerivedEntity>();
        }

        public override ObservableCollection<TEntity> Local
        {
            get { return _data; }
        }

        Type IQueryable.ElementType
        {
            get { return _query.ElementType; }
        }

        Expression IQueryable.Expression
        {
            get { return _query.Expression; }
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<TEntity>(_query.Provider); }
        }

        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IEnumerator<TEntity> IEnumerable<TEntity>.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IDbAsyncEnumerator<TEntity> IDbAsyncEnumerable<TEntity>.GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<TEntity>(_data.GetEnumerator());
        }
    }

    internal class TestDbAsyncQueryProvider<TEntity> : IDbAsyncQueryProvider
    {
        private readonly IQueryProvider _inner;

        internal TestDbAsyncQueryProvider(IQueryProvider inner)
        {
            _inner = inner;
        }

        public IQueryable CreateQuery(Expression expression)
        {
            return new TestDbAsyncEnumerable<TEntity>(expression);
        }

        public IQueryable<TElement> CreateQuery<TElement>(Expression expression)
        {
            return new TestDbAsyncEnumerable<TElement>(expression);
        }

        public object Execute(Expression expression)
        {
            return _inner.Execute(expression);
        }

        public TResult Execute<TResult>(Expression expression)
        {
            return _inner.Execute<TResult>(expression);
        }

        public Task<object> ExecuteAsync(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute(expression));
        }

        public Task<TResult> ExecuteAsync<TResult>(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute<TResult>(expression));
        }
    }

    internal class TestDbAsyncEnumerable<T> : EnumerableQuery<T>, IDbAsyncEnumerable<T>, IQueryable<T>
    {
        public TestDbAsyncEnumerable(IEnumerable<T> enumerable)
            : base(enumerable)
        { }

        public TestDbAsyncEnumerable(Expression expression)
            : base(expression)
        { }

        public IDbAsyncEnumerator<T> GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<T>(this.AsEnumerable().GetEnumerator());
        }

        IDbAsyncEnumerator IDbAsyncEnumerable.GetAsyncEnumerator()
        {
            return GetAsyncEnumerator();
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<T>(this); }
        }
    }

    internal class TestDbAsyncEnumerator<T> : IDbAsyncEnumerator<T>
    {
        private readonly IEnumerator<T> _inner;

        public TestDbAsyncEnumerator(IEnumerator<T> inner)
        {
            _inner = inner;
        }

        public void Dispose()
        {
            _inner.Dispose();
        }

        public Task<bool> MoveNextAsync(CancellationToken cancellationToken)
        {
            return Task.FromResult(_inner.MoveNext());
        }

        public T Current
        {
            get { return _inner.Current; }
        }

        object IDbAsyncEnumerator.Current
        {
            get { return Current; }
        }
    }
}
```  

### <a name="implementing-find"></a>Uygulama Bul  

Find yöntemi genel bir şekilde uygulanması zordur. Test etmek gerekiyorsa yapan kodu desteklemek için gereken Varlık türlerinin her biri için olan DB bulma bir test oluşturmak en kolayıdır bulma yöntemini kullanın. Ardından, aşağıda gösterildiği gibi bu türdeki varlığın bulmak için mantıksal yazabilirsiniz.  

``` csharp
using System.Linq;

namespace TestingDemo
{
    class TestBlogDbSet : TestDbSet<Blog>
    {
        public override Blog Find(params object[] keyValues)
        {
            var id = (int)keyValues.Single();
            return this.SingleOrDefault(b => b.BlogId == id);
        }
    }
}
```  

## <a name="writing-some-tests"></a>Bazı testleri yazma  

Tüm testi başlatmak için yapmanız gereken budur. Şu test bir TestContext ve bu bağlamda alan bir hizmeti oluşturur. Hizmet, ardından AddBlog yöntemi kullanarak yeni bir blog – oluşturmak için kullanılır. Son olarak, test, hizmet bağlamı'nın blogları özelliğine yeni bir Blog eklenmiş ve bağlamda SaveChanges çağırmışsa doğrular.  

Bu yalnızca bir bellek içi test çift sınayabilirsiniz şeyler tür örnek ve test double ve gereksinimlerinizi karşılamak için doğrulama mantığını ayarlayabilirsiniz.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var context = new TestContext();

            var service = new BlogService(context);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            Assert.AreEqual(1, context.Blogs.Count());
            Assert.AreEqual("ADO.NET Blog", context.Blogs.Single().Name);
            Assert.AreEqual("http://blogs.msdn.com/adonet", context.Blogs.Single().Url);
            Assert.AreEqual(1, context.SaveChangesCount);
        }
    }
}
```  

Test - bu kez, bir sorgu gerçekleştiren bir başka bir örnek aşağıda verilmiştir. Test - veri alfabetik sırada olmadığına dikkat edin, Blog özelliğinde bazı verilerle bir test bağlam oluşturarak başlar. Ardından bizim test bağlamını temel alan bir BlogService oluşturabilmek ve geri GetAllBlogs aldığımız verileri adına göre sıralanır emin olun.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

Son olarak, sizi eklediğimiz içinde zaman uyumsuz altyapı emin olmak için sunduğumuz zaman uyumsuz yöntemini kullanan bir daha fazla test yazacaksınız [TestDbSet](#creating-the-in-memory-test-doubles) çalışmaktadır.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    [TestClass]
    public class AsyncQueryTests
    {
        [TestMethod]
        public async Task GetAllBlogsAsync_orders_by_name()
        {
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
