---
title: -EF6 gibi sahte bir çerçeve ile test etme
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: 3d39b41018beb70b72105dfb2fe4d61afc0b0525
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319211"
---
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="fa91e-102">Sahte bir framework ile test etme</span><span class="sxs-lookup"><span data-stu-id="fa91e-102">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="fa91e-103">**EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="fa91e-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="fa91e-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="fa91e-105">Uygulamanız için testleri yazarken genellikle veritabanı ulaşmaktan kaçınmak için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="fa91e-106">Entity Framework, bir bağlam oluşturarak – – testleriniz tarafından tanımlanan davranışı ile bunu başarmak için bellek içi verileri kullanır, sağlar.</span><span class="sxs-lookup"><span data-stu-id="fa91e-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="fa91e-107">Test double oluşturmak için Seçenekler</span><span class="sxs-lookup"><span data-stu-id="fa91e-107">Options for creating test doubles</span></span>  

<span data-ttu-id="fa91e-108">Bağlamınızı bir bellek içi sürümünü oluşturmak için kullanılan iki farklı yaklaşım vardır.</span><span class="sxs-lookup"><span data-stu-id="fa91e-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="fa91e-109">**Kendi test double oluşturma** – bu yaklaşım, kendi bellek içi uygulama içeriği ve DbSets yazma içerir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="fa91e-110">Bu, çok nasıl sınıfları davranır ancak yazmayı ve makul bir kod sahip olan içerebilir denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="fa91e-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="fa91e-111">**Sahte bir çerçeve test double oluşturulacağı** – sahte bir çerçeve (örneğin, Moq) kullanarak bağlamı ve çalışma zamanında dinamik olarak için oluşturulan kümeleri bellek içi uygulamalara sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of your context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="fa91e-112">Bu makalede sahte bir çerçeve kullanma ile ilgilenecektir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-112">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="fa91e-113">Kendi test double oluşturmak için bkz: [ile bilgisayarınızı kendi Test double test](writing-test-doubles.md).</span><span class="sxs-lookup"><span data-stu-id="fa91e-113">For creating your own test doubles see [Testing with Your Own Test Doubles](writing-test-doubles.md).</span></span>  

<span data-ttu-id="fa91e-114">Sahte işlem çerçevesiyle EF kullanan göstermek için Moq kullanmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="fa91e-114">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="fa91e-115">Moq almak için en kolay yolu yüklemektir [Moq NuGet paketinden](http://nuget.org/packages/Moq/).</span><span class="sxs-lookup"><span data-stu-id="fa91e-115">The easiest way to get Moq is to install the [Moq package from NuGet](http://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="fa91e-116">EF6 öncesi sürümler ile test etme</span><span class="sxs-lookup"><span data-stu-id="fa91e-116">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="fa91e-117">Bu makalede gösterilen senaryoyu EF6 içinde olan DB için sunulan bazı değişikliklere bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="fa91e-117">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="fa91e-118">EF5 ve önceki sürümü ile test etmek için bkz: [sahte bir bağlamla test](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="fa91e-118">For testing with EF5 and earlier version see [Testing with a Fake Context](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="fa91e-119">EF bellek içi test çiftten oluşan sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="fa91e-119">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="fa91e-120">Bellek içi test double birim testi, uygulamanızın EF kullanan bit düzeyi kapsamını sağlamanın iyi bir yolu olabilir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-120">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="fa91e-121">Ancak, bunun yapılması, LINQ to Objects'in bellek içi veri sorguları yürütmek için kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="fa91e-121">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="fa91e-122">Bu sorgular, veritabanınızda çalıştırın SQL küçültmesini EF'ın LINQ sağlayıcısı (LINQ to Entities) kullanmaktan farklı bir davranış neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-122">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="fa91e-123">Böyle bir fark örneği ilgili verileri yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="fa91e-123">One example of such a difference is loading related data.</span></span> <span data-ttu-id="fa91e-124">Bir dizi blog oluşturursanız her ilgili gönderileri ve bellek içi verileri kullanırken ilgili postaların her zaman her Blog için yüklenecek.</span><span class="sxs-lookup"><span data-stu-id="fa91e-124">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="fa91e-125">Dahil etme yöntemini kullanırsanız, ancak bir veritabanıyla çalışırken veriler yalnızca yüklenir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-125">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="fa91e-126">Bu nedenle, her zaman doğru bir veritabanında, uygulama çalışır emin olmak için uçtan uca (ek olarak, birim testleri) test belirli bir düzeyde dahil etmek için önerilir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-126">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="fa91e-127">Bu makaleyi izleyerek</span><span class="sxs-lookup"><span data-stu-id="fa91e-127">Following along with this article</span></span>  

<span data-ttu-id="fa91e-128">Bu makalede istiyorsanız takip etmek için Visual Studio'ya kopyalayabilirsiniz tam kod listeleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="fa91e-128">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="fa91e-129">Oluşturmak en kolayıdır bir **birim testi projesi** ihtiyacınız ve hedef **.NET Framework 4.5** kullanan zaman uyumsuz bölümlerin tamamlanması.</span><span class="sxs-lookup"><span data-stu-id="fa91e-129">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="fa91e-130">EF modeli</span><span class="sxs-lookup"><span data-stu-id="fa91e-130">The EF model</span></span>  

<span data-ttu-id="fa91e-131">Test etmek için yapacağımız hizmeti bir EF yararlanır modeli BloggingContext ve Blog ve gönderi sınıfları oluşur.</span><span class="sxs-lookup"><span data-stu-id="fa91e-131">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="fa91e-132">Bu kod EF Designer tarafından oluşturulmuş olabilir veya bir Code First modeli.</span><span class="sxs-lookup"><span data-stu-id="fa91e-132">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="fa91e-133">EF Designer ile sanal olan DB özellikleri</span><span class="sxs-lookup"><span data-stu-id="fa91e-133">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="fa91e-134">Bağlam olan DB özellikleri olarak işaretlenmiş unutmayın.</span><span class="sxs-lookup"><span data-stu-id="fa91e-134">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="fa91e-135">Bu, bizim bağlamını ve sahte bir uygulama bu özelliklerini geçersiz kılma türetmek için sahte framework olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="fa91e-135">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="fa91e-136">Ardından kod ilk kullanıyorsanız sınıflarınızı doğrudan düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa91e-136">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="fa91e-137">EF Designer kullanıyorsanız Bağlamınızı oluşturan T4 şablonu düzenlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-137">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="fa91e-138">Açık yukarı \<model_adı\>. Edmx dosyası altında iç içe Context.tt dosya, aşağıdaki kod parçası bulun ve gösterildiği gibi sanal anahtar sözcük ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fa91e-138">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

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

## <a name="service-to-be-tested"></a><span data-ttu-id="fa91e-139">Test edilecek hizmeti</span><span class="sxs-lookup"><span data-stu-id="fa91e-139">Service to be tested</span></span>  

<span data-ttu-id="fa91e-140">Bellek içi test Double ile test göstermek için birkaç test için bir BlogService sağladığım için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="fa91e-140">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="fa91e-141">(GetAllBlogs) adına göre sıralanmış tüm blogları döndüren ve yeni blogları (AddBlog) istemcilerinizle bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-141">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="fa91e-142">GetAllBlogs yanı sıra, ayrıca zaman uyumsuz olarak (GetAllBlogsAsync) adına göre sıralanmış tüm blogları alacak bir yöntem sağladık.</span><span class="sxs-lookup"><span data-stu-id="fa91e-142">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

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

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="fa91e-143">Sorgu olmayan senaryoları test etme</span><span class="sxs-lookup"><span data-stu-id="fa91e-143">Testing non-query scenarios</span></span>  

<span data-ttu-id="fa91e-144">Tüm sorgu olmayan yöntemleri testi başlatmak için yapmanız gereken budur.</span><span class="sxs-lookup"><span data-stu-id="fa91e-144">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="fa91e-145">Şu test Moq bağlam oluşturmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="fa91e-145">The following test uses Moq to create a context.</span></span> <span data-ttu-id="fa91e-146">Ardından bir olan DB oluşturur\<Blog\> ve bu bağlamı'nın blogları özelliğinden döndürülecek bağlayan ayarlama.</span><span class="sxs-lookup"><span data-stu-id="fa91e-146">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="fa91e-147">Ardından, bağlamı, ardından AddBlog yöntemi kullanarak yeni bir blog – oluşturmak için kullanılan yeni bir BlogService oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fa91e-147">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="fa91e-148">Son olarak, test, hizmet eklenen yeni bir Blog ve bağlamda SaveChanges çağırmışsa doğrular.</span><span class="sxs-lookup"><span data-stu-id="fa91e-148">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

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

## <a name="testing-query-scenarios"></a><span data-ttu-id="fa91e-149">Sorgu senaryoları test etme</span><span class="sxs-lookup"><span data-stu-id="fa91e-149">Testing query scenarios</span></span>  

<span data-ttu-id="fa91e-150">Çift bizim olan DB test sorguları yürütmek için biz Iqueryable uygulaması zamanlaması oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-150">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="fa91e-151">Bazı bellek içi verileri oluşturmak için ilk adımıdır – listesini kullanıyoruz\<Blog\>.</span><span class="sxs-lookup"><span data-stu-id="fa91e-151">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="fa91e-152">Ardından, biz bir bağlam ve olan DB oluşturun\<Blog\> ardından wire olan DB – Iqueryable uygulamasını ayarlama bunlar yalnızca temsilci seçme listesiyle birlikte çalışan nesnelerin sağlayıcı için LINQ için\<T\>.</span><span class="sxs-lookup"><span data-stu-id="fa91e-152">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="fa91e-153">Ardından bizim test çiftler hakkında temel bir BlogService oluşturabilmek ve geri GetAllBlogs aldığımız verileri adına göre sıralanır emin olun.</span><span class="sxs-lookup"><span data-stu-id="fa91e-153">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

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

### <a name="testing-with-async-queries"></a><span data-ttu-id="fa91e-154">Zaman uyumsuz sorgular ile test etme</span><span class="sxs-lookup"><span data-stu-id="fa91e-154">Testing with async queries</span></span>

<span data-ttu-id="fa91e-155">Entity Framework 6 zaman uyumsuz olarak bir sorgu yürütmek için kullanılan genişletme yöntemleri kümesini kullanıma sunuldu.</span><span class="sxs-lookup"><span data-stu-id="fa91e-155">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="fa91e-156">Bu yöntemleri örnekleri ToListAsync, FirstAsync, ForEachAsync vb. içerir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-156">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="fa91e-157">Entity Framework sorguları LINQ yararlanması için genişletme yöntemleri Iqueryable ve IEnumerable tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="fa91e-157">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="fa91e-158">Ancak, yalnızca Entity Framework ile kullanılmak üzere tasarlandığından bir Entity Framework sorgu olmayan bir LINQ sorgusu kullanmayı denerseniz şu hatayı alabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fa91e-158">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="fa91e-159">Iqueryable kaynak IDbAsyncEnumerable uygulamayan{0}.</span><span class="sxs-lookup"><span data-stu-id="fa91e-159">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="fa91e-160">IDbAsyncEnumerable uygulayan kaynakları Entity Framework zaman uyumsuz işlemler için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-160">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="fa91e-161">Daha fazla ayrıntı için bkz: [ http://go.microsoft.com/fwlink/?LinkId=287068 ](https://go.microsoft.com/fwlink/?LinkId=287068).</span><span class="sxs-lookup"><span data-stu-id="fa91e-161">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="fa91e-162">Zaman uyumsuz yöntemler, yalnızca bir EF sorgusu çalıştırılırken desteklenir artırabileceksiniz çalışan bellek içi karşı test çift olan bir DB, birim testiniz kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa91e-162">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="fa91e-163">Async metotlarını kullanmak için size zaman uyumsuz sorguları işlemek üzere bir bellek içi DbAsyncQueryProvider oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fa91e-163">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="fa91e-164">Moq kullanarak bir sorgu sağlayıcısı ayarlanabilir artırabileceksiniz kodda test çift uygulaması oluşturmak çok daha kolaydır.</span><span class="sxs-lookup"><span data-stu-id="fa91e-164">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="fa91e-165">Bu uygulama için kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="fa91e-165">The code for this implementation is as follows:</span></span>  

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

<span data-ttu-id="fa91e-166">Biz bir zaman uyumsuz sorgu sağlayıcısı olduğuna göre bir birim testi için sunduğumuz yeni GetAllBlogsAsync yöntemi yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa91e-166">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

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
