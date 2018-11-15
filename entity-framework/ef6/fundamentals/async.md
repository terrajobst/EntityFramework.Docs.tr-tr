---
title: Zaman uyumsuz sorgulama ve Kaydet - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 4ed4f5c13341f33ccff8325a5ddacd8f7b195a76
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283829"
---
# <a name="async-query-and-save"></a><span data-ttu-id="fb75d-102">Zaman uyumsuz sorgulama ve kaydedin</span><span class="sxs-lookup"><span data-stu-id="fb75d-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="fb75d-103">**EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="fb75d-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="fb75d-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="fb75d-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="fb75d-105">EF6 kullanarak kaydetmek ve zaman uyumsuz sorgu için destek sunmuştur [async ve await anahtar sözcükleri](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) .NET 4. 5 ' sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="fb75d-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="fb75d-106">Tüm uygulamalar için faaliyetler avantajlı olabilir ancak uzun süre çalışan, ağ veya miyim/O-bağlı görevler ele alırken istemci yanıt verme becerisini ve sunucu ölçeklenebilirliğini geliştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb75d-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="fb75d-107">Zaman uyumsuz gerçekten kullanan</span><span class="sxs-lookup"><span data-stu-id="fb75d-107">When to really use async</span></span>

<span data-ttu-id="fb75d-108">Bu kılavuzun amacı, zaman uyumsuz ve zaman uyumlu programın yürütülmesi arasındaki farkı görmek kolay bir şekilde zaman uyumsuz kavramları tanıtan sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="fb75d-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="fb75d-109">Bu izlenecek yol herhangi birini senaryoları göstermek için zaman uyumsuz programlama avantajları burada tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="fb75d-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="fb75d-110">Zaman uyumsuz programlama öncelikle herhangi bir işlem süresi gerektirmeyen bir işlem için beklediği sırada başka işleri yapmak için geçerli yönetilen iş parçacığı (iş parçacığı çalışan .NET kodu) boşaltma üzerinde yönetilen bir iş parçacığından odaklanır.</span><span class="sxs-lookup"><span data-stu-id="fb75d-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="fb75d-111">Örneğin, veritabanı motoru sorgu işlenirken bir şey yok .NET kodu tarafından yapılacak.</span><span class="sxs-lookup"><span data-stu-id="fb75d-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="fb75d-112">İstemci uygulamalarında (WinForms, WPF vb.), geçerli iş parçacığı UI esnek, zaman uyumsuz işlem gerçekleştirildiği sırada tutmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb75d-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="fb75d-113">Diğer gelen istekleri - iş parçacığı kullanılabilir sunucu uygulamalarında (ASP.NET, vb.) bu bellek kullanımını azaltmak ve/veya sunucusunun aktarım hızını artırın.</span><span class="sxs-lookup"><span data-stu-id="fb75d-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="fb75d-114">Zaman uyumsuz kullanarak çoğu uygulamada belirgin faydalı olacaktır ve hatta zarar.</span><span class="sxs-lookup"><span data-stu-id="fb75d-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="fb75d-115">Testler, profil oluşturma ve sağduyunuzu yapmadan önce zaman uyumsuz belirli senaryonuza etkisini ölçmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="fb75d-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="fb75d-116">Zaman uyumsuz hakkında bilgi edinmek için daha fazla bazı kaynaklar aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fb75d-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="fb75d-117">.NET 4.5 içinde async/await Brandon Bray'nın genel bakış</span><span class="sxs-lookup"><span data-stu-id="fb75d-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="fb75d-118">[Zaman uyumsuz programlama](https://msdn.microsoft.com/library/hh191443.aspx) MSDN Kitaplığı'nda sayfaları</span><span class="sxs-lookup"><span data-stu-id="fb75d-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="fb75d-119">[ASP.NET Web uygulamaları kullanarak Async yapı nasıl](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (artan sunucusu verimliliği gösterimini içerir)</span><span class="sxs-lookup"><span data-stu-id="fb75d-119">[How to Build ASP.NET Web Applications Using Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="fb75d-120">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="fb75d-120">Create the model</span></span>

<span data-ttu-id="fb75d-121">Kullanacağız [kod ilk iş akışınızı](~/ef6/modeling/code-first/workflows/new-database.md) modelimizi oluşturun ve zaman uyumsuz işlevleri ile EF Designer oluşturulanlar dahil olmak üzere tüm EF modelleri ile çalışır ancak veritabanı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="fb75d-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="fb75d-122">Bir konsol uygulaması oluşturun ve adlandırın **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="fb75d-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="fb75d-123">EntityFramework NuGet paketi ekleme</span><span class="sxs-lookup"><span data-stu-id="fb75d-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="fb75d-124">Çözüm Gezgini'nde sağ **AsyncDemo** proje</span><span class="sxs-lookup"><span data-stu-id="fb75d-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="fb75d-125">Seçin **NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="fb75d-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="fb75d-126">NuGet paketlerini Yönet iletişim kutusunda, seçmek **çevrimiçi** sekmesini **EntityFramework** paket</span><span class="sxs-lookup"><span data-stu-id="fb75d-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="fb75d-127">Tıklayın **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="fb75d-127">Click **Install**</span></span>
-   <span data-ttu-id="fb75d-128">Ekleme bir **Model.cs** aşağıdaki uygulama ile sınıfı</span><span class="sxs-lookup"><span data-stu-id="fb75d-128">Add a **Model.cs** class with the following implementation</span></span>

``` csharp
    using System.Collections.Generic;
    using System.Data.Entity;

    namespace AsyncDemo
    {
        public class BloggingContext : DbContext
        {
            public DbSet<Blog> Blogs { get; set; }
            public DbSet<Post> Posts { get; set; }
        }

        public class Blog
        {
            public int BlogId { get; set; }
            public string Name { get; set; }

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

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="fb75d-129">Zaman uyumlu bir program oluşturma</span><span class="sxs-lookup"><span data-stu-id="fb75d-129">Create a synchronous program</span></span>

<span data-ttu-id="fb75d-130">EF modeli sahibiz, bazı veri erişimi gerçekleştirdiği kullanan biraz kod yazalım.</span><span class="sxs-lookup"><span data-stu-id="fb75d-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="fb75d-131">Öğesinin içeriğini değiştirin **Program.cs** aşağıdaki kodla</span><span class="sxs-lookup"><span data-stu-id="fb75d-131">Replace the contents of **Program.cs** with the following code</span></span>

``` csharp
    using System;
    using System.Linq;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                PerformDatabaseOperations();

                Console.WriteLine();
                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static void PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    db.SaveChanges();

                    // Query for all blogs ordered by name
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine();
                    Console.WriteLine("All blogs:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="fb75d-132">Bu kod **PerformDatabaseOperations** yeni kaydedileceği yöntemi **Blog** veritabanına ve ardından alır tüm **blogları** veritabanından ve bunlara yazdırır **Konsol**.</span><span class="sxs-lookup"><span data-stu-id="fb75d-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="fb75d-133">Bundan sonra programı için günün bir teklif Yazar **konsol**.</span><span class="sxs-lookup"><span data-stu-id="fb75d-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="fb75d-134">Kod zaman uyumlu olduğundan, biz programını çalıştırdığınızda, biz aşağıdaki yürütme akış görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fb75d-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="fb75d-135">**SaveChanges** yeni göndermeye başlar **Blog** veritabanı</span><span class="sxs-lookup"><span data-stu-id="fb75d-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="fb75d-136">**SaveChanges** tamamlar</span><span class="sxs-lookup"><span data-stu-id="fb75d-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="fb75d-137">Tüm sorgu **blogları** veritabanına gönderilir</span><span class="sxs-lookup"><span data-stu-id="fb75d-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="fb75d-138">Sorgu döndürür ve sonuçları yazılır **Konsolu**</span><span class="sxs-lookup"><span data-stu-id="fb75d-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="fb75d-139">Günün yazılır **Konsolu**</span><span class="sxs-lookup"><span data-stu-id="fb75d-139">Quote of the day is written to **Console**</span></span>

![Eşitleme çıkışı](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="fb75d-141">Zaman uyumsuz hale getirme</span><span class="sxs-lookup"><span data-stu-id="fb75d-141">Making it asynchronous</span></span>

<span data-ttu-id="fb75d-142">Programımız çalıştırmaya sahibiz, biz yeni async ve await anahtar sözcükleri yapmadan başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb75d-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="fb75d-143">Program.cs'ye aşağıdaki değişiklikleri yaptık</span><span class="sxs-lookup"><span data-stu-id="fb75d-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="fb75d-144">2. satır: Kullanma bildirimi **System.Data.Entity** ad alanı sağladığı erişim için EF zaman uyumsuz genişletme yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="fb75d-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="fb75d-145">4. satırı: Kullanma bildirimi **System.Threading.Tasks** bize kullanmak ad alanı sağlar **görev** türü.</span><span class="sxs-lookup"><span data-stu-id="fb75d-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="fb75d-146">Satır 12 & 18: biz ilerleme durumunu izleyen Görev yakalama **PerformSomeDatabaseOperations** (satır, 12) ve sonra bu program yürütme engelleme görev tam bir kez tüm işler için program (18. satır) gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="fb75d-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="fb75d-147">Satırı 25: Güncelleştirme yaptıklarımız **PerformSomeDatabaseOperations** olarak işaretlenecek **zaman uyumsuz** ve dönüş bir **görev**.</span><span class="sxs-lookup"><span data-stu-id="fb75d-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="fb75d-148">Satırı 35: Biz artık SaveChanges zaman uyumsuz sürümü çağırma ve onun tamamlanmasını bekleme.</span><span class="sxs-lookup"><span data-stu-id="fb75d-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="fb75d-149">Satırı 42: Biz artık ToList zaman uyumsuz sürümü çağırma ve sonucuna bekleniyor.</span><span class="sxs-lookup"><span data-stu-id="fb75d-149">Line 42: We're now calling hte Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="fb75d-150">System.Data.Entity ad alanında kullanılabilir uzantı yöntemlerini kapsamlı bir listesi için QueryableExtensions sınıfına bakın.</span><span class="sxs-lookup"><span data-stu-id="fb75d-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="fb75d-151">*Ayrıca "System.Data.Entity kullanma" eklemek kullanarak, gerekir deyimleri.*</span><span class="sxs-lookup"><span data-stu-id="fb75d-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

``` csharp
    using System;
    using System.Data.Entity;
    using System.Linq;
    using System.Threading.Tasks;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                var task = PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                task.Wait();

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static async Task PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    await db.SaveChangesAsync();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = await (from b in db.Blogs
                                orderby b.Name
                                select b).ToListAsync();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" - " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="fb75d-152">Kod uyumsuz olduğuna göre biz programını çalıştırdığınızda, biz farklı yürütme akış görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fb75d-152">Now that the code is asyncronous, we can observe a different execution flow when we run the program:</span></span>

1.  <span data-ttu-id="fb75d-153">**SaveChanges** yeni göndermeye başlar **Blog** veritabanına *komutu artık işlem saati geçerli bir yönetilen iş parçacığı üzerinde gerekli veritabanı gönderildikten sonra. **PerformDatabaseOperations** yöntemi döndürür (Bu yürütme işlemi tamamlanmadı olsa bile) ve ana yöntem program akışında devam eder.*</span><span class="sxs-lookup"><span data-stu-id="fb75d-153">**SaveChanges** begins to push the new **Blog** to the database *Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2.  <span data-ttu-id="fb75d-154">**Günün konsoluna yazılan**
    \*Main yöntemine yapmak için daha fazla iş olduğundan, yönetilen iş parçacığı bekleme engellenir veritabanı işlemi tamamlanana kadar çağırın. İşlem tamamlandıktan sonra geri kalanında bizim **PerformDatabaseOperations** \* yürütülür.</span><span class="sxs-lookup"><span data-stu-id="fb75d-154">**Quote of the day is written to Console**
*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations*** will be executed.</span></span>
3.  <span data-ttu-id="fb75d-155">**SaveChanges** tamamlar</span><span class="sxs-lookup"><span data-stu-id="fb75d-155">**SaveChanges** completes</span></span>
4.  <span data-ttu-id="fb75d-156">Tüm sorgu **blogları** veritabanına gönderilen *yeniden yönetilen iş parçacığı veritabanında sorgu işlenirken başka işleri yapmak ücretsizdir. Diğer tüm yürütme tamamlandı olduğundan, iş parçacığı yalnızca bekleme çağrıda ancak durdurulur.*</span><span class="sxs-lookup"><span data-stu-id="fb75d-156">Query for all **Blogs** is sent to the database *Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="fb75d-157">Sorgu döndürür ve sonuçları yazılır **Konsolu**</span><span class="sxs-lookup"><span data-stu-id="fb75d-157">Query returns and results are written to **Console**</span></span>

![Zaman uyumsuz çıkış](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="fb75d-159">Takeaway</span><span class="sxs-lookup"><span data-stu-id="fb75d-159">The takeaway</span></span>

<span data-ttu-id="fb75d-160">Artık hale getirmek için ne kadar kolay olduğunu gördük EF'ın zaman uyumsuz yöntemleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="fb75d-160">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="fb75d-161">Zaman uyumsuz avantajları ile basit bir konsol uygulaması oldukça belirgin olmayabilir rağmen bu aynı stratejiler burada uzun süreli veya ağa bağlı etkinlik Aksi takdirde uygulama önleyebilir, veya çok sayıda iş parçacığı neden durumlarda uygulanabilir bellek Ayak izi artırın.</span><span class="sxs-lookup"><span data-stu-id="fb75d-161">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
