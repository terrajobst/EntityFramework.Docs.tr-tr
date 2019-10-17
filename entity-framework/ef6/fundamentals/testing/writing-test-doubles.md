---
title: Kendi testinizi test edin-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 3d8933fb5e17f8c01f3971495a1fcdb5b8cfab57
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72446031"
---
# <a name="testing-with-your-own-test-doubles"></a>Kendi testinizde test Double
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.  

Uygulamanız için testler yazarken, genellikle veritabanına ulaşmaktan kaçınmak için bu durum tercih edilir.  Entity Framework, bir bağlam oluşturarak elde etmenizi sağlar. bu sayede, testleriniz tarafından tanımlanan davranışla birlikte bellek içi verileri kullanır.  

## <a name="options-for-creating-test-doubles"></a>Test Double değerleri oluşturma seçenekleri  

Bağlamınızın bellek içi bir sürümünü oluşturmak için kullanılabilecek iki farklı yaklaşım vardır.  

- **Kendi testinizi oluşturun** – bu yaklaşım, bağlam ve dbsets kendi bellek içi uygulamanızın yazılmasını içerir. Bu, sınıfların nasıl davrandığına ilişkin çok fazla denetim sağlar ancak makul miktarda kodu yazmayı ve sahip olduğunu içerebilir.  
- **Test Double değerleri oluşturmak için bir sahte işlem çerçevesi kullanma** : bir sahte işlem çerçevesi kullanarak (moq gibi), sizin için çalışma zamanında sizin için bağlam içi ve dinamik olarak oluşturulan kümeler için, içeriğiniz için bir işlem yapın.  

Bu makale, kendi test Double 'nizi oluşturmaya yöneliktir. Sahte işlem çerçevesini kullanma hakkında bilgi için bkz. [bir sahte işlem çerçevesi ile test etme](mocking.md).  

## <a name="testing-with-pre-ef6-versions"></a>EF6 öncesi sürümlerle test etme  

Bu makalede gösterilen kod, EF6 ile uyumludur. EF5 ve önceki sürümleri test etmek için bkz. [sahte bağlamla test etme](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Bellek içindeki EF ile test arasındaki sınırlamalar  

Bellek içi test Double değerleri, uygulamanızın EF kullanan bit bitlerinin birim test düzeyi kapsamını sağlamak için iyi bir yoldur. Ancak bunu yaparken, bellek içi verilerde sorgu yürütmek için LINQ to Objects kullanırsınız. Bu, SQL 'e sorguları veritabanınıza karşı çalıştırılan SQL 'e çevirmek için EF 'in LINQ sağlayıcısı (LINQ to Entities) kullanmaktan farklı davranışa neden olabilir.  

Bu türden bir örnek ilgili verileri yüklüyor. Her birinin ilgili gönderileri olan bir dizi blog oluşturursanız, bellek içi veriler kullanılırken ilgili gönderiler her blog için her zaman yüklenir. Ancak, bir veritabanına karşı çalıştırıldığında, veriler yalnızca Include metodunu kullanırsanız yüklenir.  

Bu nedenle, uygulamanızın bir veritabanına karşı doğru şekilde çalıştığından emin olmak için her zaman uçtan uca testlerin bir düzeyi (birim testlerinize ek olarak) dahil edilmesi önerilir.  

## <a name="following-along-with-this-article"></a>Bu makaleyle birlikte aşağıdaki  

Bu makale, daha sonra izlemek üzere Visual Studio 'ya kopyalayabileceğiniz tüm kod listelerini sağlar. Bir **birim testi projesi** oluşturmak en kolayıdır ve zaman uyumsuz kullanan bölümleri tamamlayabilmeniz için **.NET Framework 4,5** ' i hedefleyebilirsiniz.  

## <a name="creating-a-context-interface"></a>Bağlam arabirimi oluşturma  

EF modelini kullanan bir hizmeti test etmeye bakacağız. Test için EF bağlamımızı bir bellek içi sürüm ile değiştirmek için, EF bağlamımız (ve bellek içi çift) uygulanacak bir arabirim tanımlayacağız.

Test edilecek hizmet, bağlamımız DbSet özelliklerini kullanarak verileri sorgular ve değiştirir ve değişiklikleri veritabanına göndermek için de SaveChanges 'ı çağırır. Bu nedenle bu üyeleri arabirime dahil ediyoruz.  

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

Test edilecek hizmet, BloggingContext ve blog ve post sınıflarından oluşan bir EF modelinin kullanımını sağlar. Bu kod, EF Designer tarafından oluşturulmuş veya bir Code First modeli olabilir.  

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

Bağlamımızın IBloggingContext arabirimini uyguladığını unutmayın.  

Code First kullanıyorsanız, arabirimi uygulamak için bağlamını doğrudan düzenleyebilirsiniz. EF Designer kullanıyorsanız, bağlamını oluşturan T4 şablonunu düzenlemeniz gerekir. @No__t-0model_adı @ no__t-1 ' i açın. Context.tt dosyası, edmx dosyasının altında bulunan, aşağıdaki kod parçasını bulun ve arabirimde gösterildiği gibi ekleyin.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a>Sınanacak hizmet  

Bellek içi test ile testi göstermek için bir BlogService için birkaç test yazacağız. Hizmet, yeni blogların (AddBlog) oluşturulmasına ve ada göre sıralanmış tüm blogların (Getallblogları) döndürüliliğine sahiptir. Getallbloglara ek olarak, ada (GetAllBlogsAsync) göre sıralanan tüm blogları zaman uyumsuz olarak alacak bir yöntem de sağladık.  

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

## <a name="creating-the-in-memory-test-doubles"></a>Bellek içi test Double değerlerini oluşturma  

Artık gerçek EF modeline ve bu hizmeti kullandığımıza göre, test için kullanabilmemiz için bellek içi test Double 'u oluşturmak zaman atalım. Bağlamımız için bir TestContext test Double oluşturduk. Test Double 'ta çalıştırdığımız testleri desteklemek için istediğimiz davranışı seçeceğiz. Bu örnekte, SaveChanges kaç kez çağrıldığını yakalıyoruz, ancak sınadığınız senaryoyu doğrulamak için gereken mantığı dahil edebilirsiniz.  

Ayrıca, DbSet 'in bellek içi uygulamasını sağlayan bir TestDbSet oluşturduk. DbSet üzerindeki tüm yöntemler için (bul dışında) bir yürütme sağladık, ancak yalnızca test senaryonuz tarafından kullanılacak üyeleri uygulamanız gerekir.  

TestDbSet, zaman uyumsuz sorguların işlenebilmesi için dahil ettiğimiz bazı başka altyapı sınıflarının kullanımını sağlar.  

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

### <a name="implementing-find"></a>Bulma uygulama  

Find yönteminin, genel bir biçimde uygulanması zordur. Find metodunu kullanan kodu test etmeniz gerekiyorsa, bulmayı desteklemesi gereken her varlık türü için bir test DbSet oluşturmak en kolay yoldur. Daha sonra, aşağıda gösterildiği gibi belirli varlık türünü bulmak için mantık yazabilirsiniz.  

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

## <a name="writing-some-tests"></a>Bazı testler yazma  

Teste başlamak için yapmanız gereken tek şey vardır. Aşağıdaki test bir test bağlamı ve bu bağlamı temel alan bir hizmet oluşturur. Daha sonra bu hizmet, AddBlog yöntemi kullanılarak yeni bir blog oluşturmak için kullanılır. Son olarak, test, hizmetin, içeriğin blogları özelliğine yeni bir blog eklediğini ve bağlam üzerinde SaveChanges olarak adlandırdığını doğrular.  

Bu yalnızca, bellek içi test Double ile test yapabileceğiniz işlem türlerine bir örnektir ve test Double değerlerini ve doğrulama mantığını gereksinimlerinize uyacak şekilde ayarlayabilirsiniz.  

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

İşte bir test, bu kez sorgu gerçekleştiren bir örnektir. Test, blog özelliğindeki bazı verilerle bir test bağlamı oluşturarak başlar; verilerin alfabetik sırada olmadığına unutmayın. Daha sonra test bağlamımızı temel alan bir BlogService oluşturabilir ve Getallbloglarından geri aldığımız verilerin ada göre sipariş aldığından emin olabilirsiniz.  

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

Son olarak, [Testdbset](#creating-the-in-memory-test-doubles) 'e dahil ettiğimiz zaman uyumsuz altyapının çalıştığından emin olmak için zaman uyumsuz yönteminizin kullanıldığı bir daha test yazacağız.  

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
