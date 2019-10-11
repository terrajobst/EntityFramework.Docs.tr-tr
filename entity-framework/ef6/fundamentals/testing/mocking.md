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
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="55afc-102">Bir sahte işlem çerçevesiyle test etme</span><span class="sxs-lookup"><span data-stu-id="55afc-102">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="55afc-103">**Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="55afc-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="55afc-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="55afc-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="55afc-105">Uygulamanız için testler yazarken, genellikle veritabanına ulaşmaktan kaçınmak için bu durum tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="55afc-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="55afc-106">Entity Framework, bir bağlam oluşturarak elde etmenizi sağlar. bu sayede, testleriniz tarafından tanımlanan davranışla birlikte bellek içi verileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="55afc-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="55afc-107">Test Double değerleri oluşturma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="55afc-107">Options for creating test doubles</span></span>  

<span data-ttu-id="55afc-108">Bağlamınızın bellek içi bir sürümünü oluşturmak için kullanılabilecek iki farklı yaklaşım vardır.</span><span class="sxs-lookup"><span data-stu-id="55afc-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="55afc-109">**Kendi testinizi oluşturun** – bu yaklaşım, bağlam ve dbsets kendi bellek içi uygulamanızın yazılmasını içerir.</span><span class="sxs-lookup"><span data-stu-id="55afc-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="55afc-110">Bu, sınıfların nasıl davrandığına ilişkin çok fazla denetim sağlar ancak makul miktarda kodu yazmayı ve sahip olduğunu içerebilir.</span><span class="sxs-lookup"><span data-stu-id="55afc-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="55afc-111">**Test Double değerleri oluşturmak için bir sahte işlem çerçevesi kullanma** : bir sahte işlem çerçevesi (moq gibi) kullanarak, bağlamınızın bellek içi uygulamalarına ve çalışma zamanında dinamik olarak oluşturulan kümelerine sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55afc-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of your context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="55afc-112">Bu makale, bir sahte işlem çerçevesi kullanmaya yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="55afc-112">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="55afc-113">Kendi test Double 'larınızı oluşturmak için [, kendi testlerinizi kullanarak test edin](writing-test-doubles.md).</span><span class="sxs-lookup"><span data-stu-id="55afc-113">For creating your own test doubles see [Testing with Your Own Test Doubles](writing-test-doubles.md).</span></span>  

<span data-ttu-id="55afc-114">Bir sahte işlem çerçevesiyle EF kullanmayı göstermek için moq kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="55afc-114">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="55afc-115">Moq almanın en kolay yolu [NuGet 'Den moq paketini](https://nuget.org/packages/Moq/)yüklemektir.</span><span class="sxs-lookup"><span data-stu-id="55afc-115">The easiest way to get Moq is to install the [Moq package from NuGet](https://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="55afc-116">EF6 öncesi sürümlerle test etme</span><span class="sxs-lookup"><span data-stu-id="55afc-116">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="55afc-117">Bu makalede gösterilen senaryo, EF6 içinde DbSet 'e yaptığımız bazı değişikliklere bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="55afc-117">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="55afc-118">EF5 ve önceki sürümleri test etmek için bkz. [sahte bağlamla test etme](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="55afc-118">For testing with EF5 and earlier version see [Testing with a Fake Context](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="55afc-119">Bellek içindeki EF ile test arasındaki sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="55afc-119">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="55afc-120">Bellek içi test Double değerleri, uygulamanızın EF kullanan bit bitlerinin birim test düzeyi kapsamını sağlamak için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="55afc-120">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="55afc-121">Ancak bunu yaparken, bellek içi verilerde sorgu yürütmek için LINQ to Objects kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="55afc-121">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="55afc-122">Bu, SQL 'e sorguları veritabanınıza karşı çalıştırılan SQL 'e çevirmek için EF 'in LINQ sağlayıcısı (LINQ to Entities) kullanmaktan farklı davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="55afc-122">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="55afc-123">Bu türden bir örnek ilgili verileri yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="55afc-123">One example of such a difference is loading related data.</span></span> <span data-ttu-id="55afc-124">Her birinin ilgili gönderileri olan bir dizi blog oluşturursanız, bellek içi veriler kullanılırken ilgili gönderiler her blog için her zaman yüklenir.</span><span class="sxs-lookup"><span data-stu-id="55afc-124">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="55afc-125">Ancak, bir veritabanına karşı çalıştırıldığında, veriler yalnızca Include metodunu kullanırsanız yüklenir.</span><span class="sxs-lookup"><span data-stu-id="55afc-125">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="55afc-126">Bu nedenle, uygulamanızın bir veritabanına karşı doğru şekilde çalıştığından emin olmak için her zaman uçtan uca testlerin bir düzeyi (birim testlerinize ek olarak) dahil edilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="55afc-126">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="55afc-127">Bu makaleyle birlikte aşağıdaki</span><span class="sxs-lookup"><span data-stu-id="55afc-127">Following along with this article</span></span>  

<span data-ttu-id="55afc-128">Bu makale, daha sonra izlemek üzere Visual Studio 'ya kopyalayabileceğiniz tüm kod listelerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="55afc-128">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="55afc-129">Bir **birim testi projesi** oluşturmak en kolayıdır ve zaman uyumsuz kullanan bölümleri tamamlayabilmeniz için **.NET Framework 4,5** ' i hedefleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55afc-129">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="55afc-130">EF modeli</span><span class="sxs-lookup"><span data-stu-id="55afc-130">The EF model</span></span>  

<span data-ttu-id="55afc-131">Test edilecek hizmet, BloggingContext ve blog ve post sınıflarından oluşan bir EF modelinin kullanımını sağlar.</span><span class="sxs-lookup"><span data-stu-id="55afc-131">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="55afc-132">Bu kod, EF Designer tarafından oluşturulmuş veya bir Code First modeli olabilir.</span><span class="sxs-lookup"><span data-stu-id="55afc-132">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="55afc-133">EF Designer ile sanal DbSet özellikleri</span><span class="sxs-lookup"><span data-stu-id="55afc-133">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="55afc-134">Bağlamdaki DbSet özelliklerinin sanal olarak işaretlendiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="55afc-134">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="55afc-135">Bu, sahte işlem çerçevesinin bağlamımızdan türemesini ve bu özellikleri bir mocılenmiş uygulamayla geçersiz kılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="55afc-135">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="55afc-136">Code First kullanıyorsanız, sınıflarınızı doğrudan düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55afc-136">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="55afc-137">EF Designer kullanıyorsanız, bağlamını oluşturan T4 şablonunu düzenlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="55afc-137">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="55afc-138">@No__t-0model_adı @ no__t-1 ' i açın. Context.tt dosyası, edmx dosyasının altında bulunan, aşağıdaki kod parçasını bulup gösterildiği gibi sanal anahtar sözcüğe ekleyin.</span><span class="sxs-lookup"><span data-stu-id="55afc-138">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

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

## <a name="service-to-be-tested"></a><span data-ttu-id="55afc-139">Sınanacak hizmet</span><span class="sxs-lookup"><span data-stu-id="55afc-139">Service to be tested</span></span>  

<span data-ttu-id="55afc-140">Bellek içi test ile testi göstermek için bir BlogService için birkaç test yazacağız.</span><span class="sxs-lookup"><span data-stu-id="55afc-140">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="55afc-141">Hizmet, yeni blogların (AddBlog) oluşturulmasına ve ada göre sıralanmış tüm blogların (Getallblogları) döndürüliliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="55afc-141">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="55afc-142">Getallbloglara ek olarak, ada (GetAllBlogsAsync) göre sıralanan tüm blogları zaman uyumsuz olarak alacak bir yöntem de sağladık.</span><span class="sxs-lookup"><span data-stu-id="55afc-142">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

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

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="55afc-143">Sorgu olmayan senaryoları test etme</span><span class="sxs-lookup"><span data-stu-id="55afc-143">Testing non-query scenarios</span></span>  

<span data-ttu-id="55afc-144">Sorgu olmayan yöntemlerin test etmeye başlamak için yapmanız gereken tek şey var.</span><span class="sxs-lookup"><span data-stu-id="55afc-144">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="55afc-145">Aşağıdaki test, bir bağlam oluşturmak için moq kullanır.</span><span class="sxs-lookup"><span data-stu-id="55afc-145">The following test uses Moq to create a context.</span></span> <span data-ttu-id="55afc-146">Daha sonra bir DbSet @ no__t-0Blog @ no__t-1 oluşturur ve içeriğin blogları özelliğinden döndürülecek şekilde kablolar olur.</span><span class="sxs-lookup"><span data-stu-id="55afc-146">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="55afc-147">Daha sonra bağlam, yeni bir blog oluşturmak için kullanılan yeni bir BlogService oluşturmak için kullanılır. AddBlog yöntemi kullanılarak.</span><span class="sxs-lookup"><span data-stu-id="55afc-147">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="55afc-148">Son olarak, test, hizmetin yeni bir blog eklediğini ve bağlam üzerinde SaveChanges çağırdı olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="55afc-148">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

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

## <a name="testing-query-scenarios"></a><span data-ttu-id="55afc-149">Sorgu senaryolarını test etme</span><span class="sxs-lookup"><span data-stu-id="55afc-149">Testing query scenarios</span></span>  

<span data-ttu-id="55afc-150">DbSet test Double 'imize karşı sorgu yürütebilmek için bir IQueryable uygulaması ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="55afc-150">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="55afc-151">İlk adım, bazı bellek içi veriler oluşturmaktır: @ no__t-0Blog @ no__t-1 listesini kullanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="55afc-151">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="55afc-152">Daha sonra, bir bağlam oluşturur ve DBSet @ no__t-0Blogu @ no__t-1 ' i daha sonra DbSet için IQueryable uygulamasını yedekliyoruz; bu, yalnızca @ no__t-2T @ no__t-3 listesi ile birlikte çalışır LINQ to Objects sağlayıcıya temsilci olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="55afc-152">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="55afc-153">Daha sonra test Double 'lerimizi temel alan bir BlogService oluşturabilir ve Getallbloglarından geri aldığımız verilerin ada göre sipariş aldığından emin olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55afc-153">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

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

### <a name="testing-with-async-queries"></a><span data-ttu-id="55afc-154">Zaman uyumsuz sorgularla test etme</span><span class="sxs-lookup"><span data-stu-id="55afc-154">Testing with async queries</span></span>

<span data-ttu-id="55afc-155">Entity Framework 6, bir sorguyu zaman uyumsuz olarak yürütmek için kullanılabilecek bir genişletme yöntemleri kümesi sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="55afc-155">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="55afc-156">Bu yöntemlerin örnekleri ToListAsync, FirstAsync, ForEachAsync vb. içerir.</span><span class="sxs-lookup"><span data-stu-id="55afc-156">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="55afc-157">Entity Framework sorguları LINQ kullandığından, uzantı yöntemleri IQueryable ve IEnumerable üzerinde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="55afc-157">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="55afc-158">Ancak Entity Framework yalnızca ile kullanılmak üzere tasarlandıklarından, bunları Entity Framework sorgusu olmayan bir LINQ sorgusunda kullanmayı denerseniz aşağıdaki hatayı alabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="55afc-158">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="55afc-159">Kaynak IQueryable, ıdbasyncenumerable @ no__t-0 uygulamıyor.</span><span class="sxs-lookup"><span data-stu-id="55afc-159">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="55afc-160">Yalnızca ıdbasyncenumerable uygulayan kaynaklar Entity Framework zaman uyumsuz işlemler için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="55afc-160">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="55afc-161">Daha fazla ayrıntı için bkz. [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).</span><span class="sxs-lookup"><span data-stu-id="55afc-161">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="55afc-162">Zaman uyumsuz yöntemler yalnızca bir EF sorgusuna karşı çalıştırılırken desteklenir, bir DbSet 'in bellek içi test çift tarafında çalışırken bunları birim testinizde kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55afc-162">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="55afc-163">Zaman uyumsuz yöntemleri kullanabilmeniz için, zaman uyumsuz sorguyu işlemek üzere bellek içi DbAsyncQueryProvider oluşturma gereksinimimiz vardır.</span><span class="sxs-lookup"><span data-stu-id="55afc-163">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="55afc-164">Moq kullanarak bir sorgu sağlayıcısı kurmak mümkün olduğunda, kodda bir test Double uygulaması oluşturmak çok daha kolay.</span><span class="sxs-lookup"><span data-stu-id="55afc-164">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="55afc-165">Bu uygulamanın kodu aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="55afc-165">The code for this implementation is as follows:</span></span>  

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

<span data-ttu-id="55afc-166">Artık zaman uyumsuz bir sorgu sağlayıcımız olduğuna göre, yeni GetAllBlogsAsync yönteminizin bir birim testini yazabiliriz.</span><span class="sxs-lookup"><span data-stu-id="55afc-166">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

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
