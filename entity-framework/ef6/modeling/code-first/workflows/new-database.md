---
title: Yeni bir veritabanına Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: d540fc6e84049f345ae22998f94c309e0be73fc3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418814"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="4893c-102">Yeni bir veritabanına Code First</span><span class="sxs-lookup"><span data-stu-id="4893c-102">Code First to a New Database</span></span>
<span data-ttu-id="4893c-103">Bu video ve adım adım yönergeler, yeni bir veritabanını hedefleyen Code First geliştirmeye yönelik bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="4893c-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="4893c-104">Bu senaryo, mevcut olmayan bir veritabanının hedeflenmesini ve Code First oluşturulacağını ve Code First yeni tablolar ekleyecek boş bir veritabanı içerir.</span><span class="sxs-lookup"><span data-stu-id="4893c-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="4893c-105">Code First, modelinizi C\# veya VB.Net sınıfları kullanarak tanımlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="4893c-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="4893c-106">Ek yapılandırma, isteğe bağlı olarak sınıflarınızda ve özelliklerde öznitelikler kullanılarak veya bir Fluent API kullanılarak gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4893c-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="4893c-107">Videoyu izleme</span><span class="sxs-lookup"><span data-stu-id="4893c-107">Watch the video</span></span>
<span data-ttu-id="4893c-108">Bu videoda yeni bir veritabanını hedefleyen Code First geliştirmeye yönelik bir giriş sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4893c-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="4893c-109">Bu senaryo, mevcut olmayan bir veritabanının hedeflenmesini ve Code First oluşturulacağını ve Code First yeni tablolar ekleyecek boş bir veritabanı içerir.</span><span class="sxs-lookup"><span data-stu-id="4893c-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="4893c-110">Code First, veya VB.Net sınıfları kullanarak C# modelinizi tanımlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="4893c-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="4893c-111">Ek yapılandırma, isteğe bağlı olarak sınıflarınızda ve özelliklerde öznitelikler kullanılarak veya bir Fluent API kullanılarak gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4893c-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="4893c-112">**Sunulma ölçütü**: [Rowa Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="4893c-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="4893c-113">**Video**: [wmv](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="4893c-113">**Video**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="4893c-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="4893c-114">Pre-Requisites</span></span>

<span data-ttu-id="4893c-115">Bu izlenecek yolu tamamlamak için en az Visual Studio 2010 veya Visual Studio 2012 yüklü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4893c-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="4893c-116">Visual Studio 2010 kullanıyorsanız, [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) ' in yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4893c-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="4893c-117">1. uygulamayı oluşturun</span><span class="sxs-lookup"><span data-stu-id="4893c-117">1. Create the Application</span></span>

<span data-ttu-id="4893c-118">Şeyleri basit tutmak için veri erişimi gerçekleştirmek üzere Code First kullanan temel bir konsol uygulaması oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="4893c-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="4893c-119">Visual Studio’yu açın</span><span class="sxs-lookup"><span data-stu-id="4893c-119">Open Visual Studio</span></span>
-   <span data-ttu-id="4893c-120">**Dosya-&gt; yeni&gt; projesi...**</span><span class="sxs-lookup"><span data-stu-id="4893c-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="4893c-121">Sol taraftaki menüden ve **konsol uygulamasından** **Windows** ' u seçin</span><span class="sxs-lookup"><span data-stu-id="4893c-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="4893c-122">Ad olarak **Codefırstnewdatabasesample** girin</span><span class="sxs-lookup"><span data-stu-id="4893c-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="4893c-123">**Tamam**’ı seçin</span><span class="sxs-lookup"><span data-stu-id="4893c-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="4893c-124">2. model oluşturma</span><span class="sxs-lookup"><span data-stu-id="4893c-124">2. Create the Model</span></span>

<span data-ttu-id="4893c-125">Sınıfları kullanarak çok basit bir model tanımlayalim.</span><span class="sxs-lookup"><span data-stu-id="4893c-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="4893c-126">Bunları Program.cs dosyasında, ancak gerçek bir dünya uygulamasında tanımladıktan sonra, sınıflarınızı ayrı dosyalara ve potansiyel olarak ayrı bir projeye böyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="4893c-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="4893c-127">Program.cs içindeki program sınıfı tanımının altında aşağıdaki iki sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4893c-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

``` csharp
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
```

<span data-ttu-id="4893c-128">İki gezinti özelliğini (blog. Post ve post. blog) sanal hale getiriyoruz.</span><span class="sxs-lookup"><span data-stu-id="4893c-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="4893c-129">Bu, Entity Framework yavaş yükleme özelliğini sunar.</span><span class="sxs-lookup"><span data-stu-id="4893c-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="4893c-130">Yavaş yükleme, bu özelliklerin içeriğinin, erişmeye çalıştığınızda veritabanından otomatik olarak yükleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="4893c-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="4893c-131">3. bağlam oluşturma</span><span class="sxs-lookup"><span data-stu-id="4893c-131">3. Create a Context</span></span>

<span data-ttu-id="4893c-132">Şimdi, veritabanı ile bir oturumu temsil eden ve verileri sorgulayıp kaydedebileceğimizi sağlayan bir türetilmiş bağlam tanımlama zamanı.</span><span class="sxs-lookup"><span data-stu-id="4893c-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="4893c-133">System. Data. Entity. DbContext öğesinden türeten bir bağlam tanımladık ve modelimizin her bir sınıfı için tür bir DbSet&lt;TEntity&gt; kullanıma sunuyor.</span><span class="sxs-lookup"><span data-stu-id="4893c-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="4893c-134">Artık Entity Framework türleri kullanmaya başladık, bu yüzden EntityFramework NuGet paketini eklememiz gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="4893c-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="4893c-135">**Proje –&gt; NuGet Paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="4893c-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="4893c-136">Note: **NuGet Paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="4893c-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="4893c-137">[en son NuGet sürümünü](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="4893c-137">option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="4893c-138">**Çevrimiçi** sekmesini seçin</span><span class="sxs-lookup"><span data-stu-id="4893c-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="4893c-139">**EntityFramework** paketini seçin</span><span class="sxs-lookup"><span data-stu-id="4893c-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="4893c-140">**Install** 'a tıklayın</span><span class="sxs-lookup"><span data-stu-id="4893c-140">Click **Install**</span></span>

<span data-ttu-id="4893c-141">Program.cs 'in en üstünde System. Data. Entity için bir using açıklaması ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4893c-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="4893c-142">Program.cs ' deki Post sınıfının altında aşağıdaki türetilmiş bağlamı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4893c-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="4893c-143">İşte Program.cs 'in neleri içermesi gerektiğine ilişkin ayrıntılı bir liste aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4893c-143">Here is a complete listing of what Program.cs should now contain.</span></span>

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
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

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

<span data-ttu-id="4893c-144">Bu, veri depolamayı ve almayı başlatmak için gerekli olan tüm kodlarda bulunur.</span><span class="sxs-lookup"><span data-stu-id="4893c-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="4893c-145">Arka planda oldukça bir bit olduğu açıktır, ancak kısa bir süre içinde göz atacağız, ancak önce bunu eylemde görlim.</span><span class="sxs-lookup"><span data-stu-id="4893c-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="4893c-146">4. verileri okuma & yazma</span><span class="sxs-lookup"><span data-stu-id="4893c-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="4893c-147">Ana yöntemi aşağıda gösterildiği gibi Program.cs ' de uygulayın.</span><span class="sxs-lookup"><span data-stu-id="4893c-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="4893c-148">Bu kod, bağlamımız yeni bir örnek oluşturur ve yeni bir blog eklemek için onu kullanır.</span><span class="sxs-lookup"><span data-stu-id="4893c-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="4893c-149">Daha sonra, bir LINQ sorgusu kullanarak, veritabanındaki tüm blogları başlık sırasına göre sıralanmış olarak alır.</span><span class="sxs-lookup"><span data-stu-id="4893c-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="4893c-150">Şimdi uygulamayı çalıştırabilir ve test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4893c-150">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="4893c-151">Verilerim nerede?</span><span class="sxs-lookup"><span data-stu-id="4893c-151">Where’s My Data?</span></span>

<span data-ttu-id="4893c-152">Kurala göre DbContext sizin için bir veritabanı oluşturdu.</span><span class="sxs-lookup"><span data-stu-id="4893c-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="4893c-153">Yerel bir SQL Express örneği varsa (Visual Studio 2010 ile varsayılan olarak yüklenir) Code First, bu örnekte veritabanını oluşturmuştur</span><span class="sxs-lookup"><span data-stu-id="4893c-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="4893c-154">SQL Express kullanılamazsa Code First, [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) 'yi dener ve kullanır (Visual Studio 2012 ile varsayılan olarak yüklenir)</span><span class="sxs-lookup"><span data-stu-id="4893c-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="4893c-155">Veritabanı, türetilmiş bağlamın tam adından sonra, **Codefırstnewdatabasesample. BloggingContext** ile adlandırılır</span><span class="sxs-lookup"><span data-stu-id="4893c-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="4893c-156">Bunlar yalnızca varsayılan kurallardır ve Code First kullanan veritabanını değiştirmek için çeşitli yollar vardır. **DbContext 'In model ve veritabanı bağlantısını nasıl bulduğu hakkında** daha fazla bilgi bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4893c-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="4893c-157">Visual Studio 'da Sunucu Gezgini kullanarak bu veritabanına bağlanabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="4893c-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="4893c-158">**&gt; Sunucu Gezgini görüntüle**</span><span class="sxs-lookup"><span data-stu-id="4893c-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="4893c-159">**Veri bağlantıları** ' na sağ tıklayın ve **bağlantı ekle...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="4893c-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="4893c-160">Sunucu Gezgini bir veritabanına bağlı değilseniz, veri kaynağı olarak Microsoft SQL Server seçmeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="4893c-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Veri Kaynağı Seç](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="4893c-162">Hangi hangisinin yüklü olduğuna bağlı olarak, LocalDB veya SQL Express 'e bağlanın</span><span class="sxs-lookup"><span data-stu-id="4893c-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="4893c-163">Artık Code First oluşturulan şemayı inceleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4893c-163">We can now inspect the schema that Code First created.</span></span>

![Ilk şema](~/ef6/media/schemainitial.png)

<span data-ttu-id="4893c-165">DbContext, tanımladığımız DbSet özelliklerine bakarak modele hangi sınıfların dahil edileceğini çalıştı.</span><span class="sxs-lookup"><span data-stu-id="4893c-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="4893c-166">Daha sonra tablo ve sütun adlarını belirleme, veri türlerini belirleme, birincil anahtarları bulma vb. için varsayılan Code First kuralları kümesini kullanır.</span><span class="sxs-lookup"><span data-stu-id="4893c-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="4893c-167">Bu izlenecek yolda daha sonra bu kuralları nasıl geçersiz kılabileceğiniz hakkında bakacağız.</span><span class="sxs-lookup"><span data-stu-id="4893c-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="4893c-168">5. model değişiklikleriyle ilgilenme</span><span class="sxs-lookup"><span data-stu-id="4893c-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="4893c-169">Artık modelinizde bazı değişiklikler yapma zamanı, bu değişiklikleri yaptığımız için de veritabanı şemasını güncelleştirmemiz gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="4893c-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="4893c-170">Bunu yapmak için Code First Migrations veya kısa geçişler adlı bir özellik kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="4893c-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="4893c-171">Geçişler, veritabanı Şemamızı yükseltme (ve düşürme) hakkında sıralı bir adım kümesine sahip olmamızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="4893c-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="4893c-172">Geçiş olarak bilinen bu adımların her biri, uygulanacak değişiklikleri açıklayan bir kod içerir.</span><span class="sxs-lookup"><span data-stu-id="4893c-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="4893c-173">İlk adım, BloggingContext için Code First Migrations etkinleştirmektir.</span><span class="sxs-lookup"><span data-stu-id="4893c-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="4893c-174">**Araçlar-&gt; kitaplığı Paket Yöneticisi-&gt; Paket Yöneticisi konsolu**</span><span class="sxs-lookup"><span data-stu-id="4893c-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="4893c-175">Paket Yöneticisi konsolunda **Etkinleştir-geçişleri** komutunu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="4893c-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="4893c-176">Projenize iki öğe içeren yeni bir geçişler klasörü eklenmiştir:</span><span class="sxs-lookup"><span data-stu-id="4893c-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="4893c-177">**Configuration.cs** – bu dosya geçişleri BloggingContext geçirmek için kullanılacak ayarları içerir.</span><span class="sxs-lookup"><span data-stu-id="4893c-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="4893c-178">Bu izlenecek yol için herhangi bir şeyi değiştirmemiz gerekmez, ancak burada çekirdek verileri belirtebileceğiniz, diğer veritabanları için sağlayıcıları kaydedebileceğiniz, geçişlerin de oluşturulduğu ad alanını değiştiren yer alan yer verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4893c-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="4893c-179">**&lt;zaman damgası&gt;\_InitialCreate.cs** – bu ilk geçişinizin olması, veritabanını, blogların ve gönderi tablolarının bulunduğu boş bir veritabanı olmasını sağlamak üzere veritabanına uygulanmış olan değişiklikleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4893c-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="4893c-180">Code First, artık bu tabloları bir geçişe dönüştürüldüğünü kabul ettiğimiz için bu tabloları otomatik olarak oluşturmamıza izin veririz.</span><span class="sxs-lookup"><span data-stu-id="4893c-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="4893c-181">Bu geçişin zaten uygulanmış olduğu yerel veritabanımızda Code First de kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4893c-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="4893c-182">Dosya adının zaman damgası, sıralama amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4893c-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="4893c-183">Şimdi modelinizde bir değişiklik yapalim, blog sınıfına bir URL özelliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4893c-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="4893c-184">Paket Yöneticisi konsolundaki **Add-Migration AddUrl** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4893c-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="4893c-185">Ekle-geçiş komutu, son geçişinizin sonrasında yapılan değişiklikleri denetler ve bulunan değişikliklerle yeni bir geçiş gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="4893c-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="4893c-186">Geçişlerde bir ad verebiliriz; Bu durumda, ' AddUrl ' geçişini arıyoruz.</span><span class="sxs-lookup"><span data-stu-id="4893c-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="4893c-187">Yapı iskelesi kodu, dize verilerini tutan, dbo 'ya bir URL sütunu eklememiz gerektiğini bildiriyor. Bloglar tablosu.</span><span class="sxs-lookup"><span data-stu-id="4893c-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="4893c-188">Gerekirse, yapı iskelesi kodunu düzenleyebiliyoruz ancak bu durumda gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="4893c-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   <span data-ttu-id="4893c-189">Package Manager konsolunda **Update-Database** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4893c-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="4893c-190">Bu komut, bekleyen tüm geçişleri veritabanına uygular.</span><span class="sxs-lookup"><span data-stu-id="4893c-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="4893c-191">Yalnızca yeni AddUrl geçişimizi uygulamak için, ınitialcreate geçişimiz zaten uygulandı.</span><span class="sxs-lookup"><span data-stu-id="4893c-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="4893c-192">İpucu: veritabanına göre yürütülmekte olan SQL 'i görmek için Update-Database ' i çağırırken **– verbose** anahtarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4893c-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="4893c-193">Yeni URL sütunu artık veritabanındaki Bloglar tablosuna eklenir:</span><span class="sxs-lookup"><span data-stu-id="4893c-193">The new Url column is now added to the Blogs table in the database:</span></span>

![URL Ile şema](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="4893c-195">6. veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="4893c-195">6. Data Annotations</span></span>

<span data-ttu-id="4893c-196">Şu ana kadar, en az bir deyişle, varsayılan kurallarını kullanarak modeli keşfeder, ancak sınıflarımızın kuralları takip etmemesi ve daha fazla yapılandırma gerçekleştirebilmemiz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4893c-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="4893c-197">Bunun için iki seçenek vardır; Bu bölümdeki veri ek açıklamalarına ve ardından sonraki bölümde Fluent API bakacağız.</span><span class="sxs-lookup"><span data-stu-id="4893c-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="4893c-198">Modelinize bir Kullanıcı sınıfı ekleyelim</span><span class="sxs-lookup"><span data-stu-id="4893c-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="4893c-199">Ayrıca, türetilmiş bağlamımız için bir küme eklememiz gerekir</span><span class="sxs-lookup"><span data-stu-id="4893c-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="4893c-200">Bir geçiş eklemeye çalıştık, "*EntityType ' kullanıcısının" tanımlı anahtarı olmadığını belirten bir hata alırız. Bu EntityType için anahtarı tanımlayın. "*</span><span class="sxs-lookup"><span data-stu-id="4893c-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="4893c-201">EF, bu kullanıcı adını bilmenin bir yolu olmadığından Kullanıcı için birincil anahtar olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4893c-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="4893c-202">Şimdi veri ek açıklamalarını kullanacağız. bu nedenle, Program.cs üst kısmına bir using ifadesini eklememiz gerekir</span><span class="sxs-lookup"><span data-stu-id="4893c-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="4893c-203">Şimdi, birincil anahtar olduğunu belirlemek için username özelliğine not ekleyin</span><span class="sxs-lookup"><span data-stu-id="4893c-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="4893c-204">Bu değişiklikleri veritabanına uygulamak için geçişi bir geçişe bağlamak üzere **Add-Migration AddUser** komutunu kullanın</span><span class="sxs-lookup"><span data-stu-id="4893c-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="4893c-205">Veritabanına yeni geçişi uygulamak için **Update-Database** komutunu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="4893c-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="4893c-206">Yeni tablo artık veritabanına eklenir:</span><span class="sxs-lookup"><span data-stu-id="4893c-206">The new table is now added to the database:</span></span>

![Kullanıcılar Ile şema](~/ef6/media/schemawithusers.png)

<span data-ttu-id="4893c-208">EF tarafından desteklenen ek açıklamaların tam listesi:</span><span class="sxs-lookup"><span data-stu-id="4893c-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="4893c-209">KeyAttribute</span><span class="sxs-lookup"><span data-stu-id="4893c-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="4893c-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="4893c-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="4893c-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="4893c-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="4893c-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="4893c-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="4893c-213">RequiredAttribute özniteliğine</span><span class="sxs-lookup"><span data-stu-id="4893c-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="4893c-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="4893c-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="4893c-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="4893c-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="4893c-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="4893c-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="4893c-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="4893c-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="4893c-218">Ters çevir Sepropertyattribute</span><span class="sxs-lookup"><span data-stu-id="4893c-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="4893c-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="4893c-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="4893c-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="4893c-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="4893c-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="4893c-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="4893c-222">7. floent API 'SI</span><span class="sxs-lookup"><span data-stu-id="4893c-222">7. Fluent API</span></span>

<span data-ttu-id="4893c-223">Önceki bölümde, kurala göre algılanıp algılanmadığını tamamlamak veya geçersiz kılmak için veri açıklamalarını kullanma hakkında baktık.</span><span class="sxs-lookup"><span data-stu-id="4893c-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="4893c-224">Modeli yapılandırmanın diğer yolu Code First Fluent API kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="4893c-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="4893c-225">Çoğu model yapılandırması, basit veri açıklamaları kullanılarak yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="4893c-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="4893c-226">Fluent API, veri ek açıklamalarıyla ilgili bazı gelişmiş yapılandırmaya ek olarak veri ek açıklamalarının yapabileceği her şeyi içeren model yapılandırması belirtmenin daha gelişmiş bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="4893c-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="4893c-227">Veri ek açıklamaları ve Fluent API birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4893c-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="4893c-228">Fluent API erişmek için DbContext içinde Onmodeloluþturma yöntemini geçersiz kılarsınız.</span><span class="sxs-lookup"><span data-stu-id="4893c-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="4893c-229">\_adı göstermek için User. DisplayName 'in içinde depolandığı sütunu yeniden adlandırmanızı istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="4893c-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="4893c-230">Aşağıdaki kodla BloggingContext üzerinde Onmodeloluþturma yöntemini geçersiz kılın</span><span class="sxs-lookup"><span data-stu-id="4893c-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   <span data-ttu-id="4893c-231">Bu değişiklikleri veritabanına uygulamak için geçişi bir geçişe bağlamak üzere **Add-Migration ChangeDisplayName** komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="4893c-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="4893c-232">Yeni geçişi veritabanına uygulamak için **Update-Database** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4893c-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="4893c-233">DisplayName sütunu artık\_adı görüntülenecek şekilde yeniden adlandırıldı:</span><span class="sxs-lookup"><span data-stu-id="4893c-233">The DisplayName column is now renamed to display\_name:</span></span>

![Görünen adı olan şema yeniden adlandırıldı](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="4893c-235">Özet</span><span class="sxs-lookup"><span data-stu-id="4893c-235">Summary</span></span>

<span data-ttu-id="4893c-236">Bu izlenecek yolda, yeni bir veritabanı kullanarak Code First geliştirmeye baktık.</span><span class="sxs-lookup"><span data-stu-id="4893c-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="4893c-237">Sınıfları kullanarak bir model tanımladık ve bu modeli bir veritabanı oluşturmak ve verileri depolamak ve almak için kullandınız.</span><span class="sxs-lookup"><span data-stu-id="4893c-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="4893c-238">Veritabanı oluşturulduktan sonra, modelimiz gibi şemayı değiştirmek için Code First Migrations kullandınız.</span><span class="sxs-lookup"><span data-stu-id="4893c-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="4893c-239">Ayrıca, veri ek açıklamalarını ve akıcı API 'YI kullanarak bir modelin nasıl yapılandırılacağını da gördük.</span><span class="sxs-lookup"><span data-stu-id="4893c-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
