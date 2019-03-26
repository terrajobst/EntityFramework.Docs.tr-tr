---
title: Kendi test double - EF6 ile test etme
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 9db56e28cd89084fece36c3e5a2c1b4495991d01
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419737"
---
# <a name="testing-with-your-own-test-doubles"></a><span data-ttu-id="5148e-102">Kendi test Double ile test etme</span><span class="sxs-lookup"><span data-stu-id="5148e-102">Testing with your own test doubles</span></span>
> [!NOTE]
> <span data-ttu-id="5148e-103">**EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="5148e-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="5148e-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="5148e-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="5148e-105">Uygulamanız için testleri yazarken genellikle veritabanı ulaşmaktan kaçınmak için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="5148e-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="5148e-106">Entity Framework, bir bağlam oluşturarak – – testleriniz tarafından tanımlanan davranışı ile bunu başarmak için bellek içi verileri kullanır, sağlar.</span><span class="sxs-lookup"><span data-stu-id="5148e-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="5148e-107">Test double oluşturmak için Seçenekler</span><span class="sxs-lookup"><span data-stu-id="5148e-107">Options for creating test doubles</span></span>  

<span data-ttu-id="5148e-108">Bağlamınızı bir bellek içi sürümünü oluşturmak için kullanılan iki farklı yaklaşım vardır.</span><span class="sxs-lookup"><span data-stu-id="5148e-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="5148e-109">**Kendi test double oluşturma** – bu yaklaşım, kendi bellek içi uygulama içeriği ve DbSets yazma içerir.</span><span class="sxs-lookup"><span data-stu-id="5148e-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="5148e-110">Bu, çok nasıl sınıfları davranır ancak yazmayı ve makul bir kod sahip olan içerebilir denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="5148e-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="5148e-111">**Sahte bir çerçeve test double oluşturulacağı** – sahte bir çerçeve (örneğin, Moq) kullanarak, bellek içi uygulamaları, içerik ve çalışma zamanında dinamik olarak için oluşturulan kümeleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="5148e-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="5148e-112">Bu makale, kendi test çift oluşturma ile ilgilenecektir.</span><span class="sxs-lookup"><span data-stu-id="5148e-112">This article will deal with creating your own test double.</span></span> <span data-ttu-id="5148e-113">Sahte bir çerçeve kullanma hakkında bilgi için bkz. [sahte bir Framework ile test](mocking.md).</span><span class="sxs-lookup"><span data-stu-id="5148e-113">For information on using a mocking framework see [Testing with a Mocking Framework](mocking.md).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="5148e-114">EF6 öncesi sürümler ile test etme</span><span class="sxs-lookup"><span data-stu-id="5148e-114">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="5148e-115">Bu makalede gösterilen kod EF6 ile uyumludur.</span><span class="sxs-lookup"><span data-stu-id="5148e-115">The code shown in this article is compatible with EF6.</span></span> <span data-ttu-id="5148e-116">EF5 ve önceki sürümü ile test etmek için bkz: [sahte bir bağlamla test](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="5148e-116">For testing with EF5 and earlier version see [Testing with a Fake Context](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="5148e-117">EF bellek içi test çiftten oluşan sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="5148e-117">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="5148e-118">Bellek içi test double birim testi, uygulamanızın EF kullanan bit düzeyi kapsamını sağlamanın iyi bir yolu olabilir.</span><span class="sxs-lookup"><span data-stu-id="5148e-118">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="5148e-119">Ancak, bunun yapılması, LINQ to Objects'in bellek içi veri sorguları yürütmek için kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="5148e-119">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="5148e-120">Bu sorgular, veritabanınızda çalıştırın SQL küçültmesini EF'ın LINQ sağlayıcısı (LINQ to Entities) kullanmaktan farklı bir davranış neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="5148e-120">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="5148e-121">Böyle bir fark örneği ilgili verileri yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="5148e-121">One example of such a difference is loading related data.</span></span> <span data-ttu-id="5148e-122">Bir dizi blog oluşturursanız her ilgili gönderileri ve bellek içi verileri kullanırken ilgili postaların her zaman her Blog için yüklenecek.</span><span class="sxs-lookup"><span data-stu-id="5148e-122">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="5148e-123">Dahil etme yöntemini kullanırsanız, ancak bir veritabanıyla çalışırken veriler yalnızca yüklenir.</span><span class="sxs-lookup"><span data-stu-id="5148e-123">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="5148e-124">Bu nedenle, her zaman doğru bir veritabanında, uygulama çalışır emin olmak için uçtan uca (ek olarak, birim testleri) test belirli bir düzeyde dahil etmek için önerilir.</span><span class="sxs-lookup"><span data-stu-id="5148e-124">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="5148e-125">Bu makaleyi izleyerek</span><span class="sxs-lookup"><span data-stu-id="5148e-125">Following along with this article</span></span>  

<span data-ttu-id="5148e-126">Bu makalede istiyorsanız takip etmek için Visual Studio'ya kopyalayabilirsiniz tam kod listeleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="5148e-126">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="5148e-127">Oluşturmak en kolayıdır bir **birim testi projesi** ihtiyacınız ve hedef **.NET Framework 4.5** kullanan zaman uyumsuz bölümlerin tamamlanması.</span><span class="sxs-lookup"><span data-stu-id="5148e-127">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="creating-a-context-interface"></a><span data-ttu-id="5148e-128">Bir bağlam arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="5148e-128">Creating a context interface</span></span>  

<span data-ttu-id="5148e-129">Bunu bir EF kullanan bir hizmet sınama sırasında aranacak dağıtacağız modeli.</span><span class="sxs-lookup"><span data-stu-id="5148e-129">We're going to look at testing a service that makes use of an EF model.</span></span> <span data-ttu-id="5148e-130">Test etmek için bir bellek içi sürüm bizim EF bağlam yerine oluşturabilmek, bizim EF bağlam (ve onun bellek içi çift) uygulayan bir arabirim tanımlarsınız.</span><span class="sxs-lookup"><span data-stu-id="5148e-130">In order to be able to replace our EF context with an in-memory version for testing, we'll define an interface that our EF context (and it's in-memory double) will implement.</span></span>

<span data-ttu-id="5148e-131">Test kullanacağız Hizmeti sorgu ve bizim bağlam olan DB özelliklerini kullanarak verileri değiştirme ve veritabanına değişiklikleri gönderme SaveChanges de çağırır.</span><span class="sxs-lookup"><span data-stu-id="5148e-131">The service we are going to test will query and modify data using the DbSet properties of our context and also call SaveChanges to push changes to the database.</span></span> <span data-ttu-id="5148e-132">Biz bu üyeler arabirimde şekilde dahil.</span><span class="sxs-lookup"><span data-stu-id="5148e-132">So we're including these members on the interface.</span></span>  

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

## <a name="the-ef-model"></a><span data-ttu-id="5148e-133">EF modeli</span><span class="sxs-lookup"><span data-stu-id="5148e-133">The EF model</span></span>  

<span data-ttu-id="5148e-134">Test etmek için yapacağımız hizmeti bir EF yararlanır modeli BloggingContext ve Blog ve gönderi sınıfları oluşur.</span><span class="sxs-lookup"><span data-stu-id="5148e-134">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="5148e-135">Bu kod EF Designer tarafından oluşturulmuş olabilir veya bir Code First modeli.</span><span class="sxs-lookup"><span data-stu-id="5148e-135">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a><span data-ttu-id="5148e-136">EF Designer ile bağlam arabirimini uygulama</span><span class="sxs-lookup"><span data-stu-id="5148e-136">Implementing the context interface with the EF Designer</span></span>  

<span data-ttu-id="5148e-137">Bizim bağlam IBloggingContext arabirim uyguladığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="5148e-137">Note that our context implements the IBloggingContext interface.</span></span>  

<span data-ttu-id="5148e-138">Code First kullanıyorsanız, doğrudan arabirim uygulamak için Bağlamınızı düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5148e-138">If you are using Code First then you can edit your context directly to implement the interface.</span></span> <span data-ttu-id="5148e-139">EF Designer kullanıyorsanız Bağlamınızı oluşturan T4 şablonu düzenlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5148e-139">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="5148e-140">Açık yukarı \<model_adı\>. Edmx dosyası altında iç içe Context.tt dosya, aşağıdaki kod parçası bulun ve arabiriminde gösterildiği gibi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5148e-140">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the interface as shown.</span></span>  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="5148e-141">Test edilecek hizmeti</span><span class="sxs-lookup"><span data-stu-id="5148e-141">Service to be tested</span></span>  

<span data-ttu-id="5148e-142">Bellek içi test Double ile test göstermek için birkaç test için bir BlogService sağladığım için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="5148e-142">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="5148e-143">(GetAllBlogs) adına göre sıralanmış tüm blogları döndüren ve yeni blogları (AddBlog) istemcilerinizle bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="5148e-143">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="5148e-144">GetAllBlogs yanı sıra, ayrıca zaman uyumsuz olarak (GetAllBlogsAsync) adına göre sıralanmış tüm blogları alacak bir yöntem sağladık.</span><span class="sxs-lookup"><span data-stu-id="5148e-144">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

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

<span data-ttu-id="5148e-145"><a name="creating-the-in-memory-test-doubles"/> ## Bellek içi test oluşturma iki katına çıkar</span><span class="sxs-lookup"><span data-stu-id="5148e-145"><a name="creating-the-in-memory-test-doubles"/> ## Creating the in-memory test doubles</span></span>  

<span data-ttu-id="5148e-146">Gerçek EF modeli ve kullanabileceği bir hizmet sunuyoruz, bellek içi test ediyoruz test için kullanabileceğiniz çift oluşturmak zaman var.</span><span class="sxs-lookup"><span data-stu-id="5148e-146">Now that we have the real EF model and the service that can use it, it's time to create the in-memory test double that we can use for testing.</span></span> <span data-ttu-id="5148e-147">TestContext test çift bizim bağlam için oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="5148e-147">We've created a TestContext test double for our context.</span></span> <span data-ttu-id="5148e-148">Testleri desteklemek için istediğimiz davranış seçmek için aldığımız test double içinde çalıştırmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="5148e-148">In test doubles we get to choose the behavior we want in order to support the tests we are going to run.</span></span> <span data-ttu-id="5148e-149">Bu örnekte biz yalnızca SaveChanges adlı kaç kez yakalayacağınızı, ancak Test senaryosu doğrulamak için gerekli mantığı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5148e-149">In this example we're just capturing the number of times SaveChanges is called, but you can include whatever logic is needed to verify the scenario you are testing.</span></span>  

<span data-ttu-id="5148e-150">Ayrıca, bellek içi uygulaması olan DB sağlayan bir TestDbSet oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="5148e-150">We've also created a TestDbSet that provides an in-memory implementation of DbSet.</span></span> <span data-ttu-id="5148e-151">Tüm yöntemleri için eksiksiz bir düzeyi (bulma dışında) olan DB üzerinde sunduk ancak test senaryonuz kullanacağı üyeleri uygulamak yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="5148e-151">We've provided a complete implemention for all the methods on DbSet (except for Find), but you only need to implement the members that your test scenario will use.</span></span>  

<span data-ttu-id="5148e-152">TestDbSet, zaman uyumsuz sorgular işlenebilir emin olmak için ekledik bazı diğer altyapı sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="5148e-152">TestDbSet makes use of some other infrastructure classes that we've included to ensure that async queries can be processed.</span></span>  

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

### <a name="implementing-find"></a><span data-ttu-id="5148e-153">Uygulama Bul</span><span class="sxs-lookup"><span data-stu-id="5148e-153">Implementing Find</span></span>  

<span data-ttu-id="5148e-154">Find yöntemi genel bir şekilde uygulanması zordur.</span><span class="sxs-lookup"><span data-stu-id="5148e-154">The Find method is difficult to implement in a generic fashion.</span></span> <span data-ttu-id="5148e-155">Test etmek gerekiyorsa yapan kodu desteklemek için gereken Varlık türlerinin her biri için olan DB bulma bir test oluşturmak en kolayıdır bulma yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="5148e-155">If you need to test code that makes use of the Find method it is easiest to create a test DbSet for each of the entity types that need to support find.</span></span> <span data-ttu-id="5148e-156">Ardından, aşağıda gösterildiği gibi bu türdeki varlığın bulmak için mantıksal yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5148e-156">You can then write logic to find that particular type of entity, as shown below.</span></span>  

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

## <a name="writing-some-tests"></a><span data-ttu-id="5148e-157">Bazı testleri yazma</span><span class="sxs-lookup"><span data-stu-id="5148e-157">Writing some tests</span></span>  

<span data-ttu-id="5148e-158">Tüm testi başlatmak için yapmanız gereken budur.</span><span class="sxs-lookup"><span data-stu-id="5148e-158">That’s all we need to do to start testing.</span></span> <span data-ttu-id="5148e-159">Şu test bir TestContext ve bu bağlamda alan bir hizmeti oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5148e-159">The following test creates a TestContext and then a service based on this context.</span></span> <span data-ttu-id="5148e-160">Hizmet, ardından AddBlog yöntemi kullanarak yeni bir blog – oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5148e-160">The service is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="5148e-161">Son olarak, test, hizmet bağlamı'nın blogları özelliğine yeni bir Blog eklenmiş ve bağlamda SaveChanges çağırmışsa doğrular.</span><span class="sxs-lookup"><span data-stu-id="5148e-161">Finally, the test verifies that the service added a new Blog to the context's Blogs property and called SaveChanges on the context.</span></span>  

<span data-ttu-id="5148e-162">Bu yalnızca bir bellek içi test çift sınayabilirsiniz şeyler tür örnek ve test double ve gereksinimlerinizi karşılamak için doğrulama mantığını ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5148e-162">This is just an example of the types of things you can test with an in-memory test double and you can adjust the logic of the test doubles and the verification to meet your requirements.</span></span>  

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

<span data-ttu-id="5148e-163">Test - bu kez, bir sorgu gerçekleştiren bir başka bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5148e-163">Here is another example of a test - this time one that performs a query.</span></span> <span data-ttu-id="5148e-164">Test - veri alfabetik sırada olmadığına dikkat edin, Blog özelliğinde bazı verilerle bir test bağlam oluşturarak başlar.</span><span class="sxs-lookup"><span data-stu-id="5148e-164">The test starts by creating a test context with some data in its Blog property - note that the data is not in alphabetical order.</span></span> <span data-ttu-id="5148e-165">Ardından bizim test bağlamını temel alan bir BlogService oluşturabilmek ve geri GetAllBlogs aldığımız verileri adına göre sıralanır emin olun.</span><span class="sxs-lookup"><span data-stu-id="5148e-165">We can then create a BlogService based on our test context and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

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

<span data-ttu-id="5148e-166">Son olarak, sizi eklediğimiz içinde zaman uyumsuz altyapı emin olmak için sunduğumuz zaman uyumsuz yöntemini kullanan bir daha fazla test yazacaksınız [TestDbSet](#creating-the-in-memory-test-doubles) çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5148e-166">Finally, we'll write one more test that uses our async method to ensure that the async infrastructure we included in [TestDbSet](#creating-the-in-memory-test-doubles) is working.</span></span>  

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
