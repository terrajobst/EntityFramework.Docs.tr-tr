---
title: Async sorgusu ve Save-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 0642dc13e7aa3906fa1495031c62701fc16f0192
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417982"
---
# <a name="async-query-and-save"></a><span data-ttu-id="9e9c0-102">Zaman uyumsuz sorgu ve Kaydet</span><span class="sxs-lookup"><span data-stu-id="9e9c0-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="9e9c0-103">**Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="9e9c0-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="9e9c0-105">EF6, zaman uyumsuz sorgu için destek sunmuştur ve .NET 4,5 ' de tanıtılan [zaman uyumsuz ve await anahtar sözcüklerini](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) kullanarak kaydeder.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="9e9c0-106">Tüm uygulamalar asynchrony 'den faydalanabilir olsa da, uzun süre çalışan, ağ veya g/ç ile bağlantılı görevleri gerçekleştirirken istemci yanıt hızını ve sunucu ölçeklenebilirliğini artırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="9e9c0-107">Zaman uyumsuz olarak ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="9e9c0-107">When to really use async</span></span>

<span data-ttu-id="9e9c0-108">Bu izlenecek yol, zaman uyumsuz kavramları, zaman uyumsuz ve zaman uyumlu program yürütme arasındaki farkı gözlemlemeye olanak verecek şekilde sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="9e9c0-109">Bu izlenecek yol, zaman uyumsuz programlama avantajlarının sağladığı önemli senaryolardan herhangi birini göstermeye yönelik değildir.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="9e9c0-110">Zaman uyumsuz programlama öncelikle, yönetilen bir iş parçacığından işlem süresi gerektirmeyen bir işlemi beklediği sırada başka çalışma yapmak için geçerli yönetilen iş parçacığını (.NET kodu çalıştıran iş parçacığı) boşaltmaya odaklanır.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="9e9c0-111">Örneğin, veritabanı altyapısı bir sorguyu işliyor, .NET kodu tarafından yapılacak bir şey yoktur.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="9e9c0-112">İstemci uygulamalarında (WinForms, WPF vb.) geçerli iş parçacığı, zaman uyumsuz işlem gerçekleştirilirken Kullanıcı arabirimini yanıt verecek şekilde korumak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="9e9c0-113">Sunucu uygulamalarında (ASP.NET vb.), iş parçacığı diğer gelen istekleri işlemek için kullanılabilir-Bu, bellek kullanımını azaltabilir ve/veya sunucunun verimini artırabilir.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="9e9c0-114">Zaman uyumsuz kullanan çoğu uygulama, dikkat çekici bir avantaja sahip olmayacaktır ve hatta büyük olasılıkla olabilir.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="9e9c0-115">Test, profil oluşturma ve ortak anlamda, zaman uyumsuz olarak belirli senaryonuza uygulamadan önce etkisini ölçmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="9e9c0-116">Zaman uyumsuz hakkında bilgi edinmek için daha fazla kaynak aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="9e9c0-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="9e9c0-117">Brandon Bray, .NET 4,5 ' de Async/Await öğesine genel bakış</span><span class="sxs-lookup"><span data-stu-id="9e9c0-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="9e9c0-118">MSDN kitaplığındaki [zaman uyumsuz programlama](https://msdn.microsoft.com/library/hh191443.aspx) sayfaları</span><span class="sxs-lookup"><span data-stu-id="9e9c0-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="9e9c0-119">[Zaman uyumsuz kullanarak ASP.NET Web uygulamaları oluşturma](https://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (daha fazla sunucu aktarım hızı içerir)</span><span class="sxs-lookup"><span data-stu-id="9e9c0-119">[How to Build ASP.NET Web Applications Using Async](https://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="9e9c0-120">Modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e9c0-120">Create the model</span></span>

<span data-ttu-id="9e9c0-121">Modelimizi oluşturmak ve veritabanını oluşturmak için [Code First iş akışını](~/ef6/modeling/code-first/workflows/new-database.md) kullanacağız, ancak zaman uyumsuz Işlevi EF Designer ile oluşturulanlar dahil olmak üzere tüm EF modelleriyle çalışır.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="9e9c0-122">Bir konsol uygulaması oluşturun ve bunu **AsyncDemo** olarak çağırın</span><span class="sxs-lookup"><span data-stu-id="9e9c0-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="9e9c0-123">EntityFramework NuGet paketini ekleme</span><span class="sxs-lookup"><span data-stu-id="9e9c0-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="9e9c0-124">Çözüm Gezgini, **AsyncDemo** projesine sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="9e9c0-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="9e9c0-125">**NuGet Paketlerini Yönet...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="9e9c0-126">NuGet Paketlerini Yönet iletişim kutusunda **çevrimiçi** sekmesini seçin ve **EntityFramework** paketini seçin</span><span class="sxs-lookup"><span data-stu-id="9e9c0-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="9e9c0-127">**Install** 'a tıklayın</span><span class="sxs-lookup"><span data-stu-id="9e9c0-127">Click **Install**</span></span>
-   <span data-ttu-id="9e9c0-128">Aşağıdaki uygulamayla bir **model.cs** sınıfı ekleyin</span><span class="sxs-lookup"><span data-stu-id="9e9c0-128">Add a **Model.cs** class with the following implementation</span></span>

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

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="9e9c0-129">Zaman uyumlu program oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e9c0-129">Create a synchronous program</span></span>

<span data-ttu-id="9e9c0-130">Artık bir EF modelimiz olduğuna göre, bazı veri erişimi gerçekleştirmek için onu kullanan bir kod yazalım.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="9e9c0-131">**Program.cs** içeriğini aşağıdaki kodla değiştirin</span><span class="sxs-lookup"><span data-stu-id="9e9c0-131">Replace the contents of **Program.cs** with the following code</span></span>

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
                    Console.WriteLine("Calling SaveChanges.");
                    db.SaveChanges();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="9e9c0-132">Bu kod, veritabanına yeni bir **Blog** kaydeden ve sonra tüm **blogları** veritabanından alıp **konsola**yazdıran **performdatabaseoperations** yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="9e9c0-133">Bundan sonra program, günün bir teklifini **konsola**yazar.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="9e9c0-134">Kod zaman uyumlu olduğundan, programı çalıştırdığımızda aşağıdaki yürütme akışını gözlemlebiliriz:</span><span class="sxs-lookup"><span data-stu-id="9e9c0-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="9e9c0-135">**SaveChanges** yeni **blogunuzu** veritabanına göndermeye başlıyor</span><span class="sxs-lookup"><span data-stu-id="9e9c0-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="9e9c0-136">**SaveChanges** tamamlandı</span><span class="sxs-lookup"><span data-stu-id="9e9c0-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="9e9c0-137">Tüm **blogların** sorgusu veritabanına gönderiliyor</span><span class="sxs-lookup"><span data-stu-id="9e9c0-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="9e9c0-138">Sorgu dönüşleri ve sonuçlar **konsola** yazılır</span><span class="sxs-lookup"><span data-stu-id="9e9c0-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="9e9c0-139">Günün teklifi **konsola** yazılır</span><span class="sxs-lookup"><span data-stu-id="9e9c0-139">Quote of the day is written to **Console**</span></span>

![Eşitleme çıkışı](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="9e9c0-141">Zaman uyumsuz hale getirme</span><span class="sxs-lookup"><span data-stu-id="9e9c0-141">Making it asynchronous</span></span>

<span data-ttu-id="9e9c0-142">Programımızın çalışır duruma getirdiğimiz ve bu aşamada, yeni Async ve await anahtar sözcüklerini kullanmaya başlayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="9e9c0-143">Program.cs için aşağıdaki değişiklikleri yaptık</span><span class="sxs-lookup"><span data-stu-id="9e9c0-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="9e9c0-144">2\. satır: **System. Data. Entity** ad alanı için using metodu, EF Async Extension yöntemlerine bize erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="9e9c0-145">Satır 4: **System. Threading. Tasks** ad alanı için using ifadesinin **görev** türünü kullanmamızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="9e9c0-146">Satır 12 & 18: **Performsomedatabaseoperations** (12. satır) ilerlemesini izleyen bir görev olarak yakalıyoruz ve sonra programa yönelik tüm işler yapıldıktan sonra bu görevin tamamlanması için program yürütmesini engelliyor (satır 18).</span><span class="sxs-lookup"><span data-stu-id="9e9c0-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="9e9c0-147">25. satır: **zaman uyumsuz** olarak işaretlenecek ve bir **görev**döndüren **performsomedatabaseoperations** 'ı güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="9e9c0-148">Satır 35: Şu anda SaveChanges 'un zaman uyumsuz sürümünü çağırıyor ve tamamlanmasını bekliyor.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="9e9c0-149">Satır 42: artık ToList 'in zaman uyumsuz sürümünü çağırıyor ve sonuç üzerinde bekleniyor.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-149">Line 42: We're now calling the Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="9e9c0-150">System. Data. Entity ad alanındaki kullanılabilir uzantı yöntemlerinin kapsamlı bir listesi için, QueryableExtensions sınıfına bakın.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="9e9c0-151">*Ayrıca, using deyimlerine "System. Data. Entity kullanma" eklemeniz gerekecektir.*</span><span class="sxs-lookup"><span data-stu-id="9e9c0-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

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

<span data-ttu-id="9e9c0-152">Artık kod zaman uyumsuz olduğuna göre, programı çalıştırdığımızda farklı bir yürütme akışını gözlemlebiliriz:</span><span class="sxs-lookup"><span data-stu-id="9e9c0-152">Now that the code is asynchronous, we can observe a different execution flow when we run the program:</span></span>

1. <span data-ttu-id="9e9c0-153">**SaveChanges** yeni **blogunuzu** veritabanına göndermeye başlıyor</span><span class="sxs-lookup"><span data-stu-id="9e9c0-153">**SaveChanges** begins to push the new **Blog** to the database</span></span>  
    <span data-ttu-id="9e9c0-154">*Komut veritabanına gönderildikten sonra, geçerli yönetilen iş parçacığında daha fazla işlem süresi gerekmez. **Performdatabaseoperations** yöntemi döndürür (yürütmeyi bitirmemiş olsa da) ve Main yönteminde Program Flow devam eder.*</span><span class="sxs-lookup"><span data-stu-id="9e9c0-154">*Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2. <span data-ttu-id="9e9c0-155">**Günün teklifi konsola yazılır**</span><span class="sxs-lookup"><span data-stu-id="9e9c0-155">**Quote of the day is written to Console**</span></span>  
    <span data-ttu-id="9e9c0-156">*Main yönteminde yapılacak başka bir iş olmadığından, veritabanı işlemi tamamlanana kadar, yönetilen iş parçacığı bekleme çağrısında engellenir. Tamamlandıktan sonra, **Performdatabaseoperations** 'imizin geri kalanı yürütülür.*</span><span class="sxs-lookup"><span data-stu-id="9e9c0-156">*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations** will be executed.*</span></span>
3.  <span data-ttu-id="9e9c0-157">**SaveChanges** tamamlandı</span><span class="sxs-lookup"><span data-stu-id="9e9c0-157">**SaveChanges** completes</span></span>  
4.  <span data-ttu-id="9e9c0-158">Tüm **blogların** sorgusu veritabanına gönderiliyor</span><span class="sxs-lookup"><span data-stu-id="9e9c0-158">Query for all **Blogs** is sent to the database</span></span>  
    <span data-ttu-id="9e9c0-159">*Bu durumda, sorgu veritabanında işlendiği sırada yönetilen iş parçacığı başka bir iş yapmak için ücretsizdir. Diğer tüm yürütme tamamlandığından iş parçacığı de yalnızca bekleme çağrısında durabilir.*</span><span class="sxs-lookup"><span data-stu-id="9e9c0-159">*Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="9e9c0-160">Sorgu dönüşleri ve sonuçlar **konsola** yazılır</span><span class="sxs-lookup"><span data-stu-id="9e9c0-160">Query returns and results are written to **Console**</span></span>  

![Zaman uyumsuz çıkış](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="9e9c0-162">Kalkış</span><span class="sxs-lookup"><span data-stu-id="9e9c0-162">The takeaway</span></span>

<span data-ttu-id="9e9c0-163">Artık EF 'in zaman uyumsuz yöntemlerinin ne kadar kolay olduğunu gördük.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-163">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="9e9c0-164">Zaman uyumsuz 'nin avantajları basit bir konsol uygulamasıyla çok açık olmayabilir, ancak uzun süreli veya ağ bağlantılı etkinliklerin uygulamayı engelleyebileceği veya çok sayıda iş parçacığına neden olabileceği durumlarda aynı stratejiler uygulanabilir. bellek parmak izini artırın.</span><span class="sxs-lookup"><span data-stu-id="9e9c0-164">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
