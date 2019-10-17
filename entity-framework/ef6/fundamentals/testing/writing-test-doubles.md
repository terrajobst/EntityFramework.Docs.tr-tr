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
# <a name="testing-with-your-own-test-doubles"></a><span data-ttu-id="92f49-102">Kendi testinizde test Double</span><span class="sxs-lookup"><span data-stu-id="92f49-102">Testing with your own test doubles</span></span>
> [!NOTE]
> <span data-ttu-id="92f49-103">**Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="92f49-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="92f49-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="92f49-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="92f49-105">Uygulamanız için testler yazarken, genellikle veritabanına ulaşmaktan kaçınmak için bu durum tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="92f49-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="92f49-106">Entity Framework, bir bağlam oluşturarak elde etmenizi sağlar. bu sayede, testleriniz tarafından tanımlanan davranışla birlikte bellek içi verileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="92f49-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="92f49-107">Test Double değerleri oluşturma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="92f49-107">Options for creating test doubles</span></span>  

<span data-ttu-id="92f49-108">Bağlamınızın bellek içi bir sürümünü oluşturmak için kullanılabilecek iki farklı yaklaşım vardır.</span><span class="sxs-lookup"><span data-stu-id="92f49-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="92f49-109">**Kendi testinizi oluşturun** – bu yaklaşım, bağlam ve dbsets kendi bellek içi uygulamanızın yazılmasını içerir.</span><span class="sxs-lookup"><span data-stu-id="92f49-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="92f49-110">Bu, sınıfların nasıl davrandığına ilişkin çok fazla denetim sağlar ancak makul miktarda kodu yazmayı ve sahip olduğunu içerebilir.</span><span class="sxs-lookup"><span data-stu-id="92f49-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="92f49-111">**Test Double değerleri oluşturmak için bir sahte işlem çerçevesi kullanma** : bir sahte işlem çerçevesi kullanarak (moq gibi), sizin için çalışma zamanında sizin için bağlam içi ve dinamik olarak oluşturulan kümeler için, içeriğiniz için bir işlem yapın.</span><span class="sxs-lookup"><span data-stu-id="92f49-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="92f49-112">Bu makale, kendi test Double 'nizi oluşturmaya yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="92f49-112">This article will deal with creating your own test double.</span></span> <span data-ttu-id="92f49-113">Sahte işlem çerçevesini kullanma hakkında bilgi için bkz. [bir sahte işlem çerçevesi ile test etme](mocking.md).</span><span class="sxs-lookup"><span data-stu-id="92f49-113">For information on using a mocking framework see [Testing with a Mocking Framework](mocking.md).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="92f49-114">EF6 öncesi sürümlerle test etme</span><span class="sxs-lookup"><span data-stu-id="92f49-114">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="92f49-115">Bu makalede gösterilen kod, EF6 ile uyumludur.</span><span class="sxs-lookup"><span data-stu-id="92f49-115">The code shown in this article is compatible with EF6.</span></span> <span data-ttu-id="92f49-116">EF5 ve önceki sürümleri test etmek için bkz. [sahte bağlamla test etme](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="92f49-116">For testing with EF5 and earlier version see [Testing with a Fake Context](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="92f49-117">Bellek içindeki EF ile test arasındaki sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="92f49-117">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="92f49-118">Bellek içi test Double değerleri, uygulamanızın EF kullanan bit bitlerinin birim test düzeyi kapsamını sağlamak için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="92f49-118">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="92f49-119">Ancak bunu yaparken, bellek içi verilerde sorgu yürütmek için LINQ to Objects kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="92f49-119">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="92f49-120">Bu, SQL 'e sorguları veritabanınıza karşı çalıştırılan SQL 'e çevirmek için EF 'in LINQ sağlayıcısı (LINQ to Entities) kullanmaktan farklı davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="92f49-120">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="92f49-121">Bu türden bir örnek ilgili verileri yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="92f49-121">One example of such a difference is loading related data.</span></span> <span data-ttu-id="92f49-122">Her birinin ilgili gönderileri olan bir dizi blog oluşturursanız, bellek içi veriler kullanılırken ilgili gönderiler her blog için her zaman yüklenir.</span><span class="sxs-lookup"><span data-stu-id="92f49-122">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="92f49-123">Ancak, bir veritabanına karşı çalıştırıldığında, veriler yalnızca Include metodunu kullanırsanız yüklenir.</span><span class="sxs-lookup"><span data-stu-id="92f49-123">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="92f49-124">Bu nedenle, uygulamanızın bir veritabanına karşı doğru şekilde çalıştığından emin olmak için her zaman uçtan uca testlerin bir düzeyi (birim testlerinize ek olarak) dahil edilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="92f49-124">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="92f49-125">Bu makaleyle birlikte aşağıdaki</span><span class="sxs-lookup"><span data-stu-id="92f49-125">Following along with this article</span></span>  

<span data-ttu-id="92f49-126">Bu makale, daha sonra izlemek üzere Visual Studio 'ya kopyalayabileceğiniz tüm kod listelerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="92f49-126">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="92f49-127">Bir **birim testi projesi** oluşturmak en kolayıdır ve zaman uyumsuz kullanan bölümleri tamamlayabilmeniz için **.NET Framework 4,5** ' i hedefleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="92f49-127">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="creating-a-context-interface"></a><span data-ttu-id="92f49-128">Bağlam arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="92f49-128">Creating a context interface</span></span>  

<span data-ttu-id="92f49-129">EF modelini kullanan bir hizmeti test etmeye bakacağız.</span><span class="sxs-lookup"><span data-stu-id="92f49-129">We're going to look at testing a service that makes use of an EF model.</span></span> <span data-ttu-id="92f49-130">Test için EF bağlamımızı bir bellek içi sürüm ile değiştirmek için, EF bağlamımız (ve bellek içi çift) uygulanacak bir arabirim tanımlayacağız.</span><span class="sxs-lookup"><span data-stu-id="92f49-130">In order to be able to replace our EF context with an in-memory version for testing, we'll define an interface that our EF context (and it's in-memory double) will implement.</span></span>

<span data-ttu-id="92f49-131">Test edilecek hizmet, bağlamımız DbSet özelliklerini kullanarak verileri sorgular ve değiştirir ve değişiklikleri veritabanına göndermek için de SaveChanges 'ı çağırır.</span><span class="sxs-lookup"><span data-stu-id="92f49-131">The service we are going to test will query and modify data using the DbSet properties of our context and also call SaveChanges to push changes to the database.</span></span> <span data-ttu-id="92f49-132">Bu nedenle bu üyeleri arabirime dahil ediyoruz.</span><span class="sxs-lookup"><span data-stu-id="92f49-132">So we're including these members on the interface.</span></span>  

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

## <a name="the-ef-model"></a><span data-ttu-id="92f49-133">EF modeli</span><span class="sxs-lookup"><span data-stu-id="92f49-133">The EF model</span></span>  

<span data-ttu-id="92f49-134">Test edilecek hizmet, BloggingContext ve blog ve post sınıflarından oluşan bir EF modelinin kullanımını sağlar.</span><span class="sxs-lookup"><span data-stu-id="92f49-134">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="92f49-135">Bu kod, EF Designer tarafından oluşturulmuş veya bir Code First modeli olabilir.</span><span class="sxs-lookup"><span data-stu-id="92f49-135">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a><span data-ttu-id="92f49-136">EF Designer ile bağlam arabirimini uygulama</span><span class="sxs-lookup"><span data-stu-id="92f49-136">Implementing the context interface with the EF Designer</span></span>  

<span data-ttu-id="92f49-137">Bağlamımızın IBloggingContext arabirimini uyguladığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="92f49-137">Note that our context implements the IBloggingContext interface.</span></span>  

<span data-ttu-id="92f49-138">Code First kullanıyorsanız, arabirimi uygulamak için bağlamını doğrudan düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="92f49-138">If you are using Code First then you can edit your context directly to implement the interface.</span></span> <span data-ttu-id="92f49-139">EF Designer kullanıyorsanız, bağlamını oluşturan T4 şablonunu düzenlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="92f49-139">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="92f49-140">@No__t-0model_adı @ no__t-1 ' i açın. Context.tt dosyası, edmx dosyasının altında bulunan, aşağıdaki kod parçasını bulun ve arabirimde gösterildiği gibi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="92f49-140">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the interface as shown.</span></span>  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="92f49-141">Sınanacak hizmet</span><span class="sxs-lookup"><span data-stu-id="92f49-141">Service to be tested</span></span>  

<span data-ttu-id="92f49-142">Bellek içi test ile testi göstermek için bir BlogService için birkaç test yazacağız.</span><span class="sxs-lookup"><span data-stu-id="92f49-142">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="92f49-143">Hizmet, yeni blogların (AddBlog) oluşturulmasına ve ada göre sıralanmış tüm blogların (Getallblogları) döndürüliliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="92f49-143">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="92f49-144">Getallbloglara ek olarak, ada (GetAllBlogsAsync) göre sıralanan tüm blogları zaman uyumsuz olarak alacak bir yöntem de sağladık.</span><span class="sxs-lookup"><span data-stu-id="92f49-144">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

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

## <a name="creating-the-in-memory-test-doubles"></a><span data-ttu-id="92f49-145">Bellek içi test Double değerlerini oluşturma</span><span class="sxs-lookup"><span data-stu-id="92f49-145">Creating the in-memory test doubles</span></span>  

<span data-ttu-id="92f49-146">Artık gerçek EF modeline ve bu hizmeti kullandığımıza göre, test için kullanabilmemiz için bellek içi test Double 'u oluşturmak zaman atalım.</span><span class="sxs-lookup"><span data-stu-id="92f49-146">Now that we have the real EF model and the service that can use it, it's time to create the in-memory test double that we can use for testing.</span></span> <span data-ttu-id="92f49-147">Bağlamımız için bir TestContext test Double oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="92f49-147">We've created a TestContext test double for our context.</span></span> <span data-ttu-id="92f49-148">Test Double 'ta çalıştırdığımız testleri desteklemek için istediğimiz davranışı seçeceğiz.</span><span class="sxs-lookup"><span data-stu-id="92f49-148">In test doubles we get to choose the behavior we want in order to support the tests we are going to run.</span></span> <span data-ttu-id="92f49-149">Bu örnekte, SaveChanges kaç kez çağrıldığını yakalıyoruz, ancak sınadığınız senaryoyu doğrulamak için gereken mantığı dahil edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="92f49-149">In this example we're just capturing the number of times SaveChanges is called, but you can include whatever logic is needed to verify the scenario you are testing.</span></span>  

<span data-ttu-id="92f49-150">Ayrıca, DbSet 'in bellek içi uygulamasını sağlayan bir TestDbSet oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="92f49-150">We've also created a TestDbSet that provides an in-memory implementation of DbSet.</span></span> <span data-ttu-id="92f49-151">DbSet üzerindeki tüm yöntemler için (bul dışında) bir yürütme sağladık, ancak yalnızca test senaryonuz tarafından kullanılacak üyeleri uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="92f49-151">We've provided a complete implemention for all the methods on DbSet (except for Find), but you only need to implement the members that your test scenario will use.</span></span>  

<span data-ttu-id="92f49-152">TestDbSet, zaman uyumsuz sorguların işlenebilmesi için dahil ettiğimiz bazı başka altyapı sınıflarının kullanımını sağlar.</span><span class="sxs-lookup"><span data-stu-id="92f49-152">TestDbSet makes use of some other infrastructure classes that we've included to ensure that async queries can be processed.</span></span>  

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

### <a name="implementing-find"></a><span data-ttu-id="92f49-153">Bulma uygulama</span><span class="sxs-lookup"><span data-stu-id="92f49-153">Implementing Find</span></span>  

<span data-ttu-id="92f49-154">Find yönteminin, genel bir biçimde uygulanması zordur.</span><span class="sxs-lookup"><span data-stu-id="92f49-154">The Find method is difficult to implement in a generic fashion.</span></span> <span data-ttu-id="92f49-155">Find metodunu kullanan kodu test etmeniz gerekiyorsa, bulmayı desteklemesi gereken her varlık türü için bir test DbSet oluşturmak en kolay yoldur.</span><span class="sxs-lookup"><span data-stu-id="92f49-155">If you need to test code that makes use of the Find method it is easiest to create a test DbSet for each of the entity types that need to support find.</span></span> <span data-ttu-id="92f49-156">Daha sonra, aşağıda gösterildiği gibi belirli varlık türünü bulmak için mantık yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="92f49-156">You can then write logic to find that particular type of entity, as shown below.</span></span>  

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

## <a name="writing-some-tests"></a><span data-ttu-id="92f49-157">Bazı testler yazma</span><span class="sxs-lookup"><span data-stu-id="92f49-157">Writing some tests</span></span>  

<span data-ttu-id="92f49-158">Teste başlamak için yapmanız gereken tek şey vardır.</span><span class="sxs-lookup"><span data-stu-id="92f49-158">That’s all we need to do to start testing.</span></span> <span data-ttu-id="92f49-159">Aşağıdaki test bir test bağlamı ve bu bağlamı temel alan bir hizmet oluşturur.</span><span class="sxs-lookup"><span data-stu-id="92f49-159">The following test creates a TestContext and then a service based on this context.</span></span> <span data-ttu-id="92f49-160">Daha sonra bu hizmet, AddBlog yöntemi kullanılarak yeni bir blog oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="92f49-160">The service is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="92f49-161">Son olarak, test, hizmetin, içeriğin blogları özelliğine yeni bir blog eklediğini ve bağlam üzerinde SaveChanges olarak adlandırdığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="92f49-161">Finally, the test verifies that the service added a new Blog to the context's Blogs property and called SaveChanges on the context.</span></span>  

<span data-ttu-id="92f49-162">Bu yalnızca, bellek içi test Double ile test yapabileceğiniz işlem türlerine bir örnektir ve test Double değerlerini ve doğrulama mantığını gereksinimlerinize uyacak şekilde ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="92f49-162">This is just an example of the types of things you can test with an in-memory test double and you can adjust the logic of the test doubles and the verification to meet your requirements.</span></span>  

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

<span data-ttu-id="92f49-163">İşte bir test, bu kez sorgu gerçekleştiren bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="92f49-163">Here is another example of a test - this time one that performs a query.</span></span> <span data-ttu-id="92f49-164">Test, blog özelliğindeki bazı verilerle bir test bağlamı oluşturarak başlar; verilerin alfabetik sırada olmadığına unutmayın.</span><span class="sxs-lookup"><span data-stu-id="92f49-164">The test starts by creating a test context with some data in its Blog property - note that the data is not in alphabetical order.</span></span> <span data-ttu-id="92f49-165">Daha sonra test bağlamımızı temel alan bir BlogService oluşturabilir ve Getallbloglarından geri aldığımız verilerin ada göre sipariş aldığından emin olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="92f49-165">We can then create a BlogService based on our test context and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

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

<span data-ttu-id="92f49-166">Son olarak, [Testdbset](#creating-the-in-memory-test-doubles) 'e dahil ettiğimiz zaman uyumsuz altyapının çalıştığından emin olmak için zaman uyumsuz yönteminizin kullanıldığı bir daha test yazacağız.</span><span class="sxs-lookup"><span data-stu-id="92f49-166">Finally, we'll write one more test that uses our async method to ensure that the async infrastructure we included in [TestDbSet](#creating-the-in-memory-test-doubles) is working.</span></span>  

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
