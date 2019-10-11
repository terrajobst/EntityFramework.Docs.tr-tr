---
title: Bir sahte işlem çerçevesi ile test etme-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: 790e077c5b30c4a68a96b3c1a99b40893b2bbe55
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181566"
---
# <a name="testing-with-a-mocking-framework"></a>Bir sahte işlem çerçevesiyle test etme
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.  

Uygulamanız için testler yazarken, genellikle veritabanına ulaşmaktan kaçınmak için bu durum tercih edilir.  Entity Framework, bir bağlam oluşturarak elde etmenizi sağlar. bu sayede, testleriniz tarafından tanımlanan davranışla birlikte bellek içi verileri kullanır.  

## <a name="options-for-creating-test-doubles"></a>Test Double değerleri oluşturma seçenekleri  

Bağlamınızın bellek içi bir sürümünü oluşturmak için kullanılabilecek iki farklı yaklaşım vardır.  

- **Kendi testinizi oluşturun** – bu yaklaşım, bağlam ve dbsets kendi bellek içi uygulamanızın yazılmasını içerir. Bu, sınıfların nasıl davrandığına ilişkin çok fazla denetim sağlar ancak makul miktarda kodu yazmayı ve sahip olduğunu içerebilir.  
- **Test Double değerleri oluşturmak için bir sahte işlem çerçevesi kullanma** : bir sahte işlem çerçevesi (moq gibi) kullanarak, bağlamınızın bellek içi uygulamalarına ve çalışma zamanında dinamik olarak oluşturulan kümelerine sahip olabilirsiniz.  

Bu makale, bir sahte işlem çerçevesi kullanmaya yöneliktir. Kendi test Double 'larınızı oluşturmak için [, kendi testlerinizi kullanarak test edin](writing-test-doubles.md).  

Bir sahte işlem çerçevesiyle EF kullanmayı göstermek için moq kullanacağız. Moq almanın en kolay yolu [NuGet 'Den moq paketini](https://nuget.org/packages/Moq/)yüklemektir.  

## <a name="testing-with-pre-ef6-versions"></a>EF6 öncesi sürümlerle test etme  

Bu makalede gösterilen senaryo, EF6 içinde DbSet 'e yaptığımız bazı değişikliklere bağımlıdır. EF5 ve önceki sürümleri test etmek için bkz. [sahte bağlamla test etme](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Bellek içindeki EF ile test arasındaki sınırlamalar  

Bellek içi test Double değerleri, uygulamanızın EF kullanan bit bitlerinin birim test düzeyi kapsamını sağlamak için iyi bir yoldur. Ancak bunu yaparken, bellek içi verilerde sorgu yürütmek için LINQ to Objects kullanırsınız. Bu, SQL 'e sorguları veritabanınıza karşı çalıştırılan SQL 'e çevirmek için EF 'in LINQ sağlayıcısı (LINQ to Entities) kullanmaktan farklı davranışa neden olabilir.  

Bu türden bir örnek ilgili verileri yüklüyor. Her birinin ilgili gönderileri olan bir dizi blog oluşturursanız, bellek içi veriler kullanılırken ilgili gönderiler her blog için her zaman yüklenir. Ancak, bir veritabanına karşı çalıştırıldığında, veriler yalnızca Include metodunu kullanırsanız yüklenir.  

Bu nedenle, uygulamanızın bir veritabanına karşı doğru şekilde çalıştığından emin olmak için her zaman uçtan uca testlerin bir düzeyi (birim testlerinize ek olarak) dahil edilmesi önerilir.  

## <a name="following-along-with-this-article"></a>Bu makaleyle birlikte aşağıdaki  

Bu makale, daha sonra izlemek üzere Visual Studio 'ya kopyalayabileceğiniz tüm kod listelerini sağlar. Bir **birim testi projesi** oluşturmak en kolayıdır ve zaman uyumsuz kullanan bölümleri tamamlayabilmeniz için **.NET Framework 4,5** ' i hedefleyebilirsiniz.  

## <a name="the-ef-model"></a>EF modeli  

Test edilecek hizmet, BloggingContext ve blog ve post sınıflarından oluşan bir EF modelinin kullanımını sağlar. Bu kod, EF Designer tarafından oluşturulmuş veya bir Code First modeli olabilir.  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext
    {
        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }
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

### <a name="virtual-dbset-properties-with-ef-designer"></a>EF Designer ile sanal DbSet özellikleri  

Bağlamdaki DbSet özelliklerinin sanal olarak işaretlendiğini unutmayın. Bu, sahte işlem çerçevesinin bağlamımızdan türemesini ve bu özellikleri bir mocılenmiş uygulamayla geçersiz kılmasını sağlar.  

Code First kullanıyorsanız, sınıflarınızı doğrudan düzenleyebilirsiniz. EF Designer kullanıyorsanız, bağlamını oluşturan T4 şablonunu düzenlemeniz gerekir. @No__t-0model_adı @ no__t-1 ' i açın. Context.tt dosyası, edmx dosyasının altında bulunan, aşağıdaki kod parçasını bulup gösterildiği gibi sanal anahtar sözcüğe ekleyin.  

``` csharp
public string DbSet(EntitySet entitySet)
{
    return string.Format(
        CultureInfo.InvariantCulture,
        "{0} virtual DbSet\<{1}> {2} {{ get; set; }}",
        Accessibility.ForReadOnlyProperty(entitySet),
        _typeMapper.GetTypeName(entitySet.ElementType),
        _code.Escape(entitySet));
}
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
        private BloggingContext _context;

        public BlogService(BloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = _context.Blogs.Add(new Blog { Name = name, Url = url });
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

## <a name="testing-non-query-scenarios"></a>Sorgu olmayan senaryoları test etme  

Sorgu olmayan yöntemlerin test etmeye başlamak için yapmanız gereken tek şey var. Aşağıdaki test, bir bağlam oluşturmak için moq kullanır. Daha sonra bir DbSet @ no__t-0Blog @ no__t-1 oluşturur ve içeriğin blogları özelliğinden döndürülecek şekilde kablolar olur. Daha sonra bağlam, yeni bir blog oluşturmak için kullanılan yeni bir BlogService oluşturmak için kullanılır. AddBlog yöntemi kullanılarak. Son olarak, test, hizmetin yeni bir blog eklediğini ve bağlam üzerinde SaveChanges çağırdı olduğunu doğrular.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Data.Entity;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var mockSet = new Mock<DbSet<Blog>>();

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(m => m.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            mockSet.Verify(m => m.Add(It.IsAny<Blog>()), Times.Once());
            mockContext.Verify(m => m.SaveChanges(), Times.Once());
        }
    }
}
```  

## <a name="testing-query-scenarios"></a>Sorgu senaryolarını test etme  

DbSet test Double 'imize karşı sorgu yürütebilmek için bir IQueryable uygulaması ayarlamanız gerekir. İlk adım, bazı bellek içi veriler oluşturmaktır: @ no__t-0Blog @ no__t-1 listesini kullanıyoruz. Daha sonra, bir bağlam oluşturur ve DBSet @ no__t-0Blogu @ no__t-1 ' i daha sonra DbSet için IQueryable uygulamasını yedekliyoruz; bu, yalnızca @ no__t-2T @ no__t-3 listesi ile birlikte çalışır LINQ to Objects sağlayıcıya temsilci olarak oluşturulur.  

Daha sonra test Double 'lerimizi temel alan bir BlogService oluşturabilir ve Getallbloglarından geri aldığımız verilerin ada göre sipariş aldığından emin olabilirsiniz.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Provider).Returns(data.Provider);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

### <a name="testing-with-async-queries"></a>Zaman uyumsuz sorgularla test etme

Entity Framework 6, bir sorguyu zaman uyumsuz olarak yürütmek için kullanılabilecek bir genişletme yöntemleri kümesi sunmuştur. Bu yöntemlerin örnekleri ToListAsync, FirstAsync, ForEachAsync vb. içerir.  

Entity Framework sorguları LINQ kullandığından, uzantı yöntemleri IQueryable ve IEnumerable üzerinde tanımlanmıştır. Ancak Entity Framework yalnızca ile kullanılmak üzere tasarlandıklarından, bunları Entity Framework sorgusu olmayan bir LINQ sorgusunda kullanmayı denerseniz aşağıdaki hatayı alabilirsiniz:

> Kaynak IQueryable, ıdbasyncenumerable @ no__t-0 uygulamıyor. Yalnızca ıdbasyncenumerable uygulayan kaynaklar Entity Framework zaman uyumsuz işlemler için kullanılabilir. Daha fazla ayrıntı için bkz. [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).  

Zaman uyumsuz yöntemler yalnızca bir EF sorgusuna karşı çalıştırılırken desteklenir, bir DbSet 'in bellek içi test çift tarafında çalışırken bunları birim testinizde kullanmak isteyebilirsiniz.  

Zaman uyumsuz yöntemleri kullanabilmeniz için, zaman uyumsuz sorguyu işlemek üzere bellek içi DbAsyncQueryProvider oluşturma gereksinimimiz vardır. Moq kullanarak bir sorgu sağlayıcısı kurmak mümkün olduğunda, kodda bir test Double uygulaması oluşturmak çok daha kolay. Bu uygulamanın kodu aşağıdaki gibidir:  

``` csharp
using System.Collections.Generic;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
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

Artık zaman uyumsuz bir sorgu sağlayıcımız olduğuna göre, yeni GetAllBlogsAsync yönteminizin bir birim testini yazabiliriz.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
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

            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IDbAsyncEnumerable<Blog>>()
                .Setup(m => m.GetAsyncEnumerator())
                .Returns(new TestDbAsyncEnumerator<Blog>(data.GetEnumerator()));

            mockSet.As<IQueryable<Blog>>()
                .Setup(m => m.Provider)
                .Returns(new TestDbAsyncQueryProvider<Blog>(data.Provider));

            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
