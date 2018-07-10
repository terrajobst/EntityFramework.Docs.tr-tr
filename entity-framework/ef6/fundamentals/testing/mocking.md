---
title: -EF6 gibi sahte bir çerçeve ile test etme
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
caps.latest.revision: 3
ms.openlocfilehash: 7529929a3ed3906e1201c0899f2fb8b959ec9ed2
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914135"
---
# <a name="testing-with-a-mocking-framework"></a>Sahte bir framework ile test etme
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.  

Uygulamanız için testleri yazarken genellikle veritabanı ulaşmaktan kaçınmak için tercih edilir.  Entity Framework, bir bağlam oluşturarak – – testleriniz tarafından tanımlanan davranışı ile bunu başarmak için bellek içi verileri kullanır, sağlar.  

## <a name="options-for-creating-test-doubles"></a>Test double oluşturmak için Seçenekler  

Bağlamınızı bir bellek içi sürümünü oluşturmak için kullanılan iki farklı yaklaşım vardır.  

- **Kendi test double oluşturma** – bu yaklaşım, kendi bellek içi uygulama içeriği ve DbSets yazma içerir. Bu, çok nasıl sınıfları davranır ancak yazmayı ve makul bir kod sahip olan içerebilir denetim sağlar.  
- **Sahte bir çerçeve test double oluşturulacağı** – sahte bir çerçeve (örneğin, Moq) kullanarak, bellek içi uygulamaları, içerik ve çalışma zamanında dinamik olarak için oluşturulan kümeleri olabilir.  

Bu makalede sahte bir çerçeve kullanma ile ilgilenecektir. Kendi test double oluşturmak için bkz: [ile bilgisayarınızı kendi Test double test](writing-test-doubles.md).  

Sahte işlem çerçevesiyle EF kullanan göstermek için Moq kullanmak için kullanacağız. Moq almak için en kolay yolu yüklemektir [Moq NuGet paketinden](http://nuget.org/packages/Moq/).  

## <a name="testing-with-pre-ef6-versions"></a>EF6 öncesi sürümler ile test etme  

Bu makalede gösterilen senaryoyu EF6 içinde olan DB için sunulan bazı değişikliklere bağlıdır. EF5 ve önceki sürümü ile test etmek için bkz: [sahte bir bağlamla test](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>EF bellek içi test çiftten oluşan sınırlamaları  

Bellek içi test double birim testi, uygulamanızın EF kullanan bit düzeyi kapsamını sağlamanın iyi bir yolu olabilir. Ancak, bunun yapılması, LINQ to Objects'in bellek içi veri sorguları yürütmek için kullanırsınız. Bu sorgular, veritabanınızda çalıştırın SQL küçültmesini EF'ın LINQ sağlayıcısı (LINQ to Entities) kullanmaktan farklı bir davranış neden olabilir.  

Böyle bir fark örneği ilgili verileri yükleniyor. Bir dizi blog oluşturursanız her ilgili gönderileri ve bellek içi verileri kullanırken ilgili postaların her zaman her Blog için yüklenecek. Dahil etme yöntemini kullanırsanız, ancak bir veritabanıyla çalışırken veriler yalnızca yüklenir.  

Bu nedenle, her zaman doğru bir veritabanında, uygulama çalışır emin olmak için uçtan uca (ek olarak, birim testleri) test belirli bir düzeyde dahil etmek için önerilir.  

## <a name="following-along-with-this-article"></a>Bu makaleyi izleyerek  

Bu makalede istiyorsanız takip etmek için Visual Studio'ya kopyalayabilirsiniz tam kod listeleri sağlar. Oluşturmak en kolayıdır bir **birim testi projesi** ihtiyacınız ve hedef **.NET Framework 4.5** kullanan zaman uyumsuz bölümlerin tamamlanması.  

## <a name="the-ef-model"></a>EF modeli  

Test etmek için yapacağımız hizmeti bir EF yararlanır modeli BloggingContext ve Blog ve gönderi sınıfları oluşur. Bu kod EF Designer tarafından oluşturulmuş olabilir veya bir Code First modeli.  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a>EF Designer ile sanal olan DB özellikleri  

Bağlam olan DB özellikleri olarak işaretlenmiş unutmayın. Bu, bizim bağlamını ve sahte bir uygulama bu özelliklerini geçersiz kılma türetmek için sahte framework olanak tanır.  

Ardından kod ilk kullanıyorsanız sınıflarınızı doğrudan düzenleyebilirsiniz. EF Designer kullanıyorsanız Bağlamınızı oluşturan T4 şablonu düzenlemeniz gerekir. Açık yukarı \<model_adı\>. Edmx dosyası altında iç içe Context.tt dosya, aşağıdaki kod parçası bulun ve gösterildiği gibi sanal anahtar sözcük ekleyin.  

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

Tüm sorgu olmayan yöntemleri testi başlatmak için yapmanız gereken budur. Şu test Moq bağlam oluşturmak için kullanır. Ardından bir olan DB oluşturur\<Blog\> ve bu bağlamı'nın blogları özelliğinden döndürülecek bağlayan ayarlama. Ardından, bağlamı, ardından AddBlog yöntemi kullanarak yeni bir blog – oluşturmak için kullanılan yeni bir BlogService oluşturmak için kullanılır. Son olarak, test, hizmet eklenen yeni bir Blog ve bağlamda SaveChanges çağırmışsa doğrular.  

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

## <a name="testing-query-scenarios"></a>Sorgu senaryoları test etme  

Çift bizim olan DB test sorguları yürütmek için biz Iqueryable uygulaması zamanlaması oluşturmanız gerekir. Bazı bellek içi verileri oluşturmak için ilk adımıdır – listesini kullanıyoruz\<Blog\>. Ardından, biz bir bağlam ve olan DB oluşturun\<Blog\> ardından wire olan DB – Iqueryable uygulamasını ayarlama bunlar yalnızca temsilci seçme listesiyle birlikte çalışan nesnelerin sağlayıcı için LINQ için\<T\>.  

Ardından bizim test çiftler hakkında temel bir BlogService oluşturabilmek ve geri GetAllBlogs aldığımız verileri adına göre sıralanır emin olun.  

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
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(0 => data.GetEnumerator());

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

### <a name="testing-with-async-queries"></a>Zaman uyumsuz sorgular ile test etme

Entity Framework 6 zaman uyumsuz olarak bir sorgu yürütmek için kullanılan genişletme yöntemleri kümesini kullanıma sunuldu. Bu yöntemleri örnekleri ToListAsync, FirstAsync, ForEachAsync vb. içerir.  

Entity Framework sorguları LINQ yararlanması için genişletme yöntemleri Iqueryable ve IEnumerable tanımlanır. Ancak, yalnızca Entity Framework ile kullanılmak üzere tasarlandığından bir Entity Framework sorgu olmayan bir LINQ sorgusu kullanmayı denerseniz şu hatayı alabilirsiniz:

> Iqueryable kaynak IDbAsyncEnumerable uygulamayan{0}. IDbAsyncEnumerable uygulayan kaynakları Entity Framework zaman uyumsuz işlemler için kullanılabilir. Daha fazla ayrıntı için bkz: [ http://go.microsoft.com/fwlink/?LinkId=287068 ](http://go.microsoft.com/fwlink/?LinkId=287068).  

Zaman uyumsuz yöntemler, yalnızca bir EF sorgusu çalıştırılırken desteklenir artırabileceksiniz çalışan bellek içi karşı test çift olan bir DB, birim testiniz kullanmak isteyebilirsiniz.  

Async metotlarını kullanmak için size zaman uyumsuz sorguları işlemek üzere bir bellek içi DbAsyncQueryProvider oluşturmanız gerekir. Moq kullanarak bir sorgu sağlayıcısı ayarlanabilir artırabileceksiniz kodda test çift uygulaması oluşturmak çok daha kolaydır. Bu uygulama için kod aşağıdaki gibidir:  

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

Biz bir zaman uyumsuz sorgu sağlayıcısı olduğuna göre bir birim testi için sunduğumuz yeni GetAllBlogsAsync yöntemi yazabilirsiniz.  

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
