---
title: Yeni bir veritabanı - EF6 öncelikle kod
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
caps.latest.revision: 3
ms.openlocfilehash: 75057fc73b082f4c745171ed77cddc358229a130
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912123"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="09f43-102">Yeni bir veritabanına ilk kod</span><span class="sxs-lookup"><span data-stu-id="09f43-102">Code First to a New Database</span></span>
<span data-ttu-id="09f43-103">Bu video ve adım adım kılavuz, yeni bir veritabanını hedefleyen Code First geliştirmeye giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="09f43-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="09f43-104">Bu senaryo, mevcut bir veritabanını hedefleyen içerir ve Code First oluşturur veya çok Code First yeni ekler için boş bir veritabanı tabloları.</span><span class="sxs-lookup"><span data-stu-id="09f43-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables too.</span></span> <span data-ttu-id="09f43-105">Kod ilk sağlar, C kullanarak modelinizi tanımlamanızı\# veya VB.Net sınıflar.</span><span class="sxs-lookup"><span data-stu-id="09f43-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="09f43-106">Ek yapılandırma, sınıfları ve özellikleri ya da fluent API'sini kullanarak özniteliklerini kullanarak isteğe bağlı olarak gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="09f43-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="09f43-107">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="09f43-107">Watch the video</span></span>
<span data-ttu-id="09f43-108">Bu videoda yeni bir veritabanını hedefleyen Code First geliştirmeye giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="09f43-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="09f43-109">Bu senaryo, mevcut bir veritabanını hedefleyen içerir ve Code First oluşturur veya çok Code First yeni ekler için boş bir veritabanı tabloları.</span><span class="sxs-lookup"><span data-stu-id="09f43-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables too.</span></span> <span data-ttu-id="09f43-110">Kod ilk C# veya VB.Net sınıflarını kullanarak modelinizi tanımlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="09f43-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="09f43-111">Ek yapılandırma, sınıfları ve özellikleri ya da fluent API'sini kullanarak özniteliklerini kullanarak isteğe bağlı olarak gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="09f43-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="09f43-112">**Tarafından sunulan**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="09f43-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="09f43-113">**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="09f43-113">**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

# <a name="pre-requisites"></a><span data-ttu-id="09f43-114">Ön koşullar</span><span class="sxs-lookup"><span data-stu-id="09f43-114">Pre-Requisites</span></span>

<span data-ttu-id="09f43-115">Bu izlenecek yolu tamamlamak için Visual Studio 2012 yüklü veya en az Visual Studio 2010 olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="09f43-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="09f43-116">Visual Studio 2010 kullanıyorsanız, aynı zamanda sahip gerekecektir [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) yüklü.</span><span class="sxs-lookup"><span data-stu-id="09f43-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="09f43-117">1. Uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="09f43-117">1. Create the Application</span></span>

<span data-ttu-id="09f43-118">Örneği basit tutmak için bunu bir veri erişimi gerçekleştirdiği Code First kullanan temel bir konsol uygulaması oluşturmak için dağıtacağız.</span><span class="sxs-lookup"><span data-stu-id="09f43-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="09f43-119">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="09f43-119">Open Visual Studio</span></span>
-   <span data-ttu-id="09f43-120">**Dosya -&gt; yeni -&gt; proje...**</span><span class="sxs-lookup"><span data-stu-id="09f43-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="09f43-121">Seçin **Windows** sol menüden ve **konsol uygulaması**</span><span class="sxs-lookup"><span data-stu-id="09f43-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="09f43-122">Girin **CodeFirstNewDatabaseSample** adı</span><span class="sxs-lookup"><span data-stu-id="09f43-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="09f43-123">Seçin **Tamam**</span><span class="sxs-lookup"><span data-stu-id="09f43-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="09f43-124">2. Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="09f43-124">2. Create the Model</span></span>

<span data-ttu-id="09f43-125">Sınıfları kullanarak basit bir model tanımlayalım.</span><span class="sxs-lookup"><span data-stu-id="09f43-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="09f43-126">Biz yalnızca bunları Program.cs dosyasındaki ancak ayrı dosyalar ve büyük olasılıkla ayrı proje sınıflarınızı kullanıma ayırırsınız gerçek dünya uygulamasında tanımlayacağınız.</span><span class="sxs-lookup"><span data-stu-id="09f43-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="09f43-127">Program.cs Program sınıf tanımında altına aşağıdaki iki sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="09f43-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

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

<span data-ttu-id="09f43-128">İki Gezinti özellikleri (Blog.Posts ve Post.Blog) sanal yapıyoruz olduğunu fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="09f43-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="09f43-129">Bu, Entity Framework'ün yavaş Yükleme özelliği sağlar.</span><span class="sxs-lookup"><span data-stu-id="09f43-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="09f43-130">Yavaş yükleniyor, bunları erişmeye çalışırken bu özellikleri içeriğini otomatik olarak veritabanından yükleneceğini belirten anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="09f43-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="09f43-131">3. Bir bağlam oluşturma</span><span class="sxs-lookup"><span data-stu-id="09f43-131">3. Create a Context</span></span>

<span data-ttu-id="09f43-132">Artık sorgu ve veri kaydetmek bize izin vererek, veritabanı ile bir oturumu temsil eden bir türetilmiş içeriği tanımlamak için zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="09f43-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="09f43-133">System.Data.Entity.DbContext türetilir ve bir türü belirtilmiş olan DB sunan bir bağlam tanımlarız&lt;TEntity&gt; modelimizi her sınıf için.</span><span class="sxs-lookup"><span data-stu-id="09f43-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="09f43-134">Şu anda yüzden EntityFramework NuGet paketini eklemek Entity Framework türlerinden kullanılacak başlatılıyor.</span><span class="sxs-lookup"><span data-stu-id="09f43-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="09f43-135">**Proje –&gt; NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="09f43-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="09f43-136">Not: yoksa **NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="09f43-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="09f43-137">seçeneğini yüklemelisiniz [en son NuGet sürümünü](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="09f43-137">option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="09f43-138">Seçin **çevrimiçi** sekmesi</span><span class="sxs-lookup"><span data-stu-id="09f43-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="09f43-139">Seçin **EntityFramework** paket</span><span class="sxs-lookup"><span data-stu-id="09f43-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="09f43-140">Tıklayın **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="09f43-140">Click **Install**</span></span>

<span data-ttu-id="09f43-141">Kullanarak bir ekleme deyimi Program.cs dosyasının üst System.Data.Entity için.</span><span class="sxs-lookup"><span data-stu-id="09f43-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="09f43-142">Program.cs sınıfında postanın aşağıdaki türetilmiş bağlam ekleyin.</span><span class="sxs-lookup"><span data-stu-id="09f43-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="09f43-143">Ne Program.cs artık içermelidir, tam bir listesi aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="09f43-143">Here is a complete listing of what Program.cs should now contain.</span></span>

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

<span data-ttu-id="09f43-144">Biz depolamak ve almak veri başlatmak için ihtiyacınız olan tüm kod olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="09f43-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="09f43-145">Kuşkusuz arka planda geçmeden oldukça bit yoktur ve ilk ancak bir dakika içinde şimdi nasıl çalıştığını görün, biz göz atacağız.</span><span class="sxs-lookup"><span data-stu-id="09f43-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="09f43-146">4. Okuma ve yazma veri</span><span class="sxs-lookup"><span data-stu-id="09f43-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="09f43-147">Main yöntemi program.cs'ye aşağıda gösterildiği gibi uygulayın.</span><span class="sxs-lookup"><span data-stu-id="09f43-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="09f43-148">Bu kod, bizim bağlam yeni bir örneğini oluşturur ve yeni bir Blog eklemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="09f43-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="09f43-149">Ardından veritabanından başlığa göre alfabetik olarak sıralanmış tüm blogları almak için LINQ sorgusu kullanır.</span><span class="sxs-lookup"><span data-stu-id="09f43-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="09f43-150">Şimdi uygulamayı çalıştırmak ve test etmek.</span><span class="sxs-lookup"><span data-stu-id="09f43-150">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="09f43-151">Verilerim nerede?</span><span class="sxs-lookup"><span data-stu-id="09f43-151">Where’s My Data?</span></span>

<span data-ttu-id="09f43-152">Kural gereği DbContext sizin için bir veritabanı oluşturdu.</span><span class="sxs-lookup"><span data-stu-id="09f43-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="09f43-153">(Visual Studio 2010 ile varsayılan olarak yüklenir) yerel bir SQL Express örneği varsa ardından Code First veritabanı bu örnekte oluşturdu</span><span class="sxs-lookup"><span data-stu-id="09f43-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="09f43-154">SQL Express kullanılamıyor durumunda Code First çalıştığında ve [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (Visual Studio 2012 ile varsayılan olarak yüklenir)</span><span class="sxs-lookup"><span data-stu-id="09f43-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="09f43-155">Sonra bu örnekte türetilen bağlamı tam olarak nitelenmiş adını adlı veritabanı **CodeFirstNewDatabaseSample.BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="09f43-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="09f43-156">Bunlar yalnızca varsayılan kuralları ve Code First kullandığı veritabanını değiştirmek için çeşitli yolları vardır, daha fazla bilgi kullanılabilir **DbContext Model ve veritabanı bağlantısını nasıl bulur** konu.</span><span class="sxs-lookup"><span data-stu-id="09f43-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="09f43-157">Visual Studio'da Sunucu Gezgini kullanarak bu veritabanına bağlanabilir</span><span class="sxs-lookup"><span data-stu-id="09f43-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="09f43-158">**Görünüm -&gt; Sunucu Gezgini**</span><span class="sxs-lookup"><span data-stu-id="09f43-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="09f43-159">Sağ tıklayın **veri bağlantıları** seçip **bağlantı ekle...**</span><span class="sxs-lookup"><span data-stu-id="09f43-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="09f43-160">Sunucu gezgininden veritabanına bağlamadıysanız önce Microsoft SQL Server veri kaynağı olarak seçmeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="09f43-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="09f43-162">LocalDB veya hangisinin bağlı olarak yüklediğiniz SQL Express için Bağlan</span><span class="sxs-lookup"><span data-stu-id="09f43-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="09f43-163">Biz, artık Code First oluşturulan şema inceleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="09f43-163">We can now inspect the schema that Code First created.</span></span>

![SchemaInitial](~/ef6/media/schemainitial.png)

<span data-ttu-id="09f43-165">Biz tanımlanmış olan DB özelliklerine bakarak tarafından modele dahil edilecek sınıfları dışarı DbContext çalışmıştır.</span><span class="sxs-lookup"><span data-stu-id="09f43-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="09f43-166">Ardından kod öncelikli kurallar varsayılan kümesini kullanır tablo ve sütun adları belirlemek, veri türlerini belirlemek için birincil anahtarlar vb. bulun.</span><span class="sxs-lookup"><span data-stu-id="09f43-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="09f43-167">Bu kuralları nasıl kılabilirsiniz Bu izlenecek yolda inceleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="09f43-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="09f43-168">5. Model değişikliklerle ilgilenme</span><span class="sxs-lookup"><span data-stu-id="09f43-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="09f43-169">Artık size ayrıca veritabanı şemasını güncelleştirmek için ihtiyacımız olan bu değişiklikler yaptığınızda modelimiz için bazı değişiklikler yapmanız gerekebileceğini zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="09f43-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="09f43-170">Bunu yapmak için Code First Migrations veya geçirilmesi için kısa adlı bir özelliği kullanmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="09f43-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="09f43-171">Geçişleri bize (yükseltme ve düşürme), veritabanı şemasını açıklayan adımlar sıralı bir dizi olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="09f43-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="09f43-172">Bir geçiş bilinen, bu adımların her biri, değişikliklerin uygulanması için açıklayan bazı kod içerir.</span><span class="sxs-lookup"><span data-stu-id="09f43-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="09f43-173">İlk adım, bizim BloggingContext için Code First Migrations etkinleştirmektir.</span><span class="sxs-lookup"><span data-stu-id="09f43-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="09f43-174">**Araçlar -&gt; kitaplık Paket Yöneticisi -&gt; Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="09f43-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="09f43-175">Çalıştırma **etkinleştir geçişleri** komutunu Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="09f43-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="09f43-176">Yeni bir geçiş klasör iki öğe içeren bizim projeye eklendi:</span><span class="sxs-lookup"><span data-stu-id="09f43-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="09f43-177">**Configuration.cs** – bu dosya geçişleri geçirmek için kullanacağı ayarları içeren BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="09f43-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="09f43-178">Bu izlenecek yol için herhangi bir ayarı değiştirmek gerekmez, ancak burada kayıt sağlayıcıları diğer veritabanları için çekirdek veri değişiklikleri ad alanı belirleyebileceğiniz geçişler vb. içinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="09f43-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="09f43-179">**&lt;zaman damgası&gt;\_InitialCreate.cs** – Bu, ilk geçiş, blog ve gönderi tablolar içeren bir boş bir veritabanı yüklenmesini gerçekleştirilecek veritabanına zaten uygulanmış olan değişiklikleri gösterir .</span><span class="sxs-lookup"><span data-stu-id="09f43-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="09f43-180">Biz Code First otomatik olarak izin vermesine rağmen bu tablolar bir geçiş dönüştürülmüş geçişler için biz de verdiniz göre bize oluşturun.</span><span class="sxs-lookup"><span data-stu-id="09f43-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="09f43-181">Kod, ayrıca bu geçiş zaten uygulanmış bizim yerel veritabanında ilk kaydetti.</span><span class="sxs-lookup"><span data-stu-id="09f43-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="09f43-182">Zaman damgası dosya çubuğunda amacıyla sıralamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="09f43-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="09f43-183">Şimdi github'dan modelimiz için değişiklik, Blog sınıfa URL'si özelliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="09f43-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="09f43-184">Çalıştırma **Ekle geçiş AddUrl** Paket Yöneticisi konsolunda komutu.</span><span class="sxs-lookup"><span data-stu-id="09f43-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="09f43-185">Geçiş Ekle komutunu son geçişinizi bu yana değişiklik olup olmadığını denetler ve bulunan herhangi bir değişiklik ile yeni bir geçiş iskele oluşturulduğunu.</span><span class="sxs-lookup"><span data-stu-id="09f43-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="09f43-186">Biz geçişleri bir ad verebilirsiniz; Bu durumda biz 'AddUrl' geçiş arıyoruz.</span><span class="sxs-lookup"><span data-stu-id="09f43-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="09f43-187">İskele kurulan kodu dbo dize verileri tutabilecek, bir Url sütun eklemek ihtiyacımız diyor. Bloglar tablo.</span><span class="sxs-lookup"><span data-stu-id="09f43-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="09f43-188">Gerekirse, iskele kurulan kodu düzenleme, ancak bu durumda zorunlu değildir.</span><span class="sxs-lookup"><span data-stu-id="09f43-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

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

-   <span data-ttu-id="09f43-189">Çalıştırma **veritabanını Güncelleştir** Paket Yöneticisi konsolunda komutu.</span><span class="sxs-lookup"><span data-stu-id="09f43-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="09f43-190">Bu komut tüm bekleyen geçişleri veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="09f43-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="09f43-191">Geçişler yalnızca bizim yeni AddUrl geçiş uygulanacak şekilde bizim InitialCreate geçiş zaten uygulanmıştır.</span><span class="sxs-lookup"><span data-stu-id="09f43-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="09f43-192">İpucu: Kullandığınız **– ayrıntılı** veritabanına karşı yürütülen SQL görmek için Update-veritabanı çağırırken geçin.</span><span class="sxs-lookup"><span data-stu-id="09f43-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="09f43-193">Yeni bir Url sütun artık veritabanı blogları tablosuna eklenir:</span><span class="sxs-lookup"><span data-stu-id="09f43-193">The new Url column is now added to the Blogs table in the database:</span></span>

![SchemaWithUrl](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="09f43-195">6. Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="09f43-195">6. Data Annotations</span></span>

<span data-ttu-id="09f43-196">Şu ana kadar biz yalnızca kendi varsayılan kuralları kullanarak model Bul EF bildirdiniz, ancak var olan giderek kez zaman göreceğiniz sınıfların kurallarına uygun olmayan ve daha fazla yapılandırmayı gerçekleştirmek ihtiyacımız olacak.</span><span class="sxs-lookup"><span data-stu-id="09f43-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="09f43-197">Bu iki seçenek vardır; Bu bölümde veri ek açıklamaları ve sonraki bölümde fluent API'si şu konuları inceleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="09f43-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="09f43-198">Kullanıcı sınıfı için modelimizi ekleyelim</span><span class="sxs-lookup"><span data-stu-id="09f43-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="09f43-199">Biz de müşterilerimizin türetilmiş bağlamına kümesi eklemeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="09f43-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="09f43-200">Bir geçiş eklemek çalıştığında bir hata bildiren elde edebileceğiniz "*EntityType 'User' olan anahtar tanımlanmadı. Anahtar için bu Entitytype'a tanımlayın."*</span><span class="sxs-lookup"><span data-stu-id="09f43-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="09f43-201">Kullanıcı adı, kullanıcı için birincil anahtarı olmalıdır olduğunu bilmesinin imkanı EF sahip olduğundan.</span><span class="sxs-lookup"><span data-stu-id="09f43-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="09f43-202">Yüzden kullanarak bir eklemek veri ek açıklamaları kullanmak oluşturacağız deyimini Program.cs dosyasının üst</span><span class="sxs-lookup"><span data-stu-id="09f43-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="09f43-203">Artık birincil anahtarı olduğunu belirlemek için kullanıcı adı özelliği Not Ekle</span><span class="sxs-lookup"><span data-stu-id="09f43-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="09f43-204">Kullanım **Ekle geçiş AddUser** bu uygulama için bir geçiş iskele komut, veritabanına değiştirir</span><span class="sxs-lookup"><span data-stu-id="09f43-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="09f43-205">Çalıştırma **veritabanını Güncelleştir** komut yeni geçiş veritabanına uygulamak için</span><span class="sxs-lookup"><span data-stu-id="09f43-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="09f43-206">Yeni tablo artık veritabanına eklenir:</span><span class="sxs-lookup"><span data-stu-id="09f43-206">The new table is now added to the database:</span></span>

![SchemaWithUsers](~/ef6/media/schemawithusers.png)

<span data-ttu-id="09f43-208">EF tarafından desteklenen ek açıklamaları tam listesi verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="09f43-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="09f43-209">KeyAttribute</span><span class="sxs-lookup"><span data-stu-id="09f43-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="09f43-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="09f43-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="09f43-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="09f43-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="09f43-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="09f43-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="09f43-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="09f43-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="09f43-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="09f43-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="09f43-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="09f43-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="09f43-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="09f43-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="09f43-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="09f43-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="09f43-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="09f43-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="09f43-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="09f43-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="09f43-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="09f43-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="09f43-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="09f43-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="09f43-222">7. Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="09f43-222">7. Fluent API</span></span>

<span data-ttu-id="09f43-223">Önceki bölümde tamamlayabilir veya kurala göre algılandı geçersiz kılmak için veri ek açıklamaları kullanarak incelemiştik.</span><span class="sxs-lookup"><span data-stu-id="09f43-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="09f43-224">Model yapılandırmak için diğer Code First fluent API'si ile yoludur.</span><span class="sxs-lookup"><span data-stu-id="09f43-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="09f43-225">En fazla model yapılandırma yapılabilir basit veri ek açıklamalarını kullanma.</span><span class="sxs-lookup"><span data-stu-id="09f43-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="09f43-226">Fluent API'si veri ek açıklamaları ayrıca veri açıklamalarla mümkün bazı daha gelişmiş yapılandırma için yapabileceğiniz her şeyi kapsayan bir model yapılandırması belirterek daha gelişmiş bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="09f43-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="09f43-227">Veri ek açıklamaları ve fluent API'si birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="09f43-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="09f43-228">Fluent API'sine erişmek için DbContext OnModelCreating yöntemi geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="09f43-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="09f43-229">İstedik User.DisplayName görüntülenecek depolanan sütunu yeniden adlandırmak düşünelim\_adı.</span><span class="sxs-lookup"><span data-stu-id="09f43-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="09f43-230">Aşağıdaki kod ile BloggingContext üzerinde OnModelCreating yöntemi geçersiz kılın</span><span class="sxs-lookup"><span data-stu-id="09f43-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

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

-   <span data-ttu-id="09f43-231">Kullanım **Ekle geçiş ChangeDisplayName** bu uygulama için bir geçiş iskele komut, veritabanına değiştirir.</span><span class="sxs-lookup"><span data-stu-id="09f43-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="09f43-232">Çalıştırma **veritabanını Güncelleştir** veritabanına yeni geçiş uygulamak için komutu.</span><span class="sxs-lookup"><span data-stu-id="09f43-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="09f43-233">DisplayName sütun görüntülemek için şimdi adlandırılır\_adı:</span><span class="sxs-lookup"><span data-stu-id="09f43-233">The DisplayName column is now renamed to display\_name:</span></span>

![SchemaWithDisplayNameRenamed](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="09f43-235">Özet</span><span class="sxs-lookup"><span data-stu-id="09f43-235">Summary</span></span>

<span data-ttu-id="09f43-236">Bu izlenecek yolda Code First geliştirme kullanarak yeni bir veritabanı incelemiştik.</span><span class="sxs-lookup"><span data-stu-id="09f43-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="09f43-237">Biz sınıfları kullanarak bir model tanımlı ardından modelin bir veritabanı oluşturun ve veri depolayıp almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="09f43-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="09f43-238">Veritabanı oluşturulduktan Code First Migrations şema gelişerek modelimizi değiştirmek için kullandık.</span><span class="sxs-lookup"><span data-stu-id="09f43-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="09f43-239">Ayrıca veri ek açıklamaları ve Fluent API'sini kullanarak bir model yapılandırma gördük.</span><span class="sxs-lookup"><span data-stu-id="09f43-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
