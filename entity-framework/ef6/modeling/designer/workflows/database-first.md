---
title: EF6 öncelikle - veritabanı
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: c81025fe7c3ad6398f003f7be2a3f9f072eec327
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284089"
---
# <a name="database-first"></a><span data-ttu-id="03328-102">İlk veritabanı</span><span class="sxs-lookup"><span data-stu-id="03328-102">Database First</span></span>
<span data-ttu-id="03328-103">Bu video ve adım adım izlenecek yol, Entity Framework kullanarak veritabanı First geliştirmeye giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="03328-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="03328-104">Veritabanı ilk ters mühendislik varolan bir veritabanını modelden sağlar.</span><span class="sxs-lookup"><span data-stu-id="03328-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="03328-105">Model bir EDMX dosyası (.edmx uzantısı) depolanır ve görüntülenebilir ve Entity Framework Tasarımcısı'nda düzenlenebilir.</span><span class="sxs-lookup"><span data-stu-id="03328-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="03328-106">Uygulamanıza etkileşim sınıfları EDMX dosyasından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="03328-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="03328-107">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="03328-107">Watch the video</span></span>
<span data-ttu-id="03328-108">Bu videoda, Entity Framework kullanarak veritabanı ilk geliştirme tanıtılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="03328-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="03328-109">Veritabanı ilk ters mühendislik varolan bir veritabanını modelden sağlar.</span><span class="sxs-lookup"><span data-stu-id="03328-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="03328-110">Model bir EDMX dosyası (.edmx uzantısı) depolanır ve görüntülenebilir ve Entity Framework Tasarımcısı'nda düzenlenebilir.</span><span class="sxs-lookup"><span data-stu-id="03328-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="03328-111">Uygulamanıza etkileşim sınıfları EDMX dosyasından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="03328-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="03328-112">**Tarafından sunulan**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="03328-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="03328-113">**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="03328-113">**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="03328-114">Ön koşullar</span><span class="sxs-lookup"><span data-stu-id="03328-114">Pre-Requisites</span></span>

<span data-ttu-id="03328-115">Bu izlenecek yolu tamamlamak için Visual Studio 2012 yüklü veya en az Visual Studio 2010 olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="03328-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="03328-116">Visual Studio 2010 kullanıyorsanız, aynı zamanda sahip gerekecektir [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) yüklü.</span><span class="sxs-lookup"><span data-stu-id="03328-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="03328-117">1. Mevcut bir veritabanı oluşturun</span><span class="sxs-lookup"><span data-stu-id="03328-117">1. Create an Existing Database</span></span>

<span data-ttu-id="03328-118">Genellikle zaten oluşturulur varolan bir veritabanını hedeflerken, ancak bu kılavuz erişmek için bir veritabanı oluşturmak ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="03328-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="03328-119">Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösterir:</span><span class="sxs-lookup"><span data-stu-id="03328-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="03328-120">Visual Studio 2010 kullanıyorsanız, SQL Express veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="03328-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="03328-121">Visual Studio 2012 kullanıyorsanız sonra, oluşturduğunuz bir [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="03328-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="03328-122">Yeni bir ubuntu ve veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="03328-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="03328-123">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="03328-123">Open Visual Studio</span></span>
-   <span data-ttu-id="03328-124">**Görünüm -&gt; Sunucu Gezgini**</span><span class="sxs-lookup"><span data-stu-id="03328-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="03328-125">Sağ tıklayın **veri bağlantıları -&gt; bağlantı ekle...**</span><span class="sxs-lookup"><span data-stu-id="03328-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="03328-126">Sunucu gezgininden veritabanına bağlamadıysanız önce Microsoft SQL Server veri kaynağı olarak seçmeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="03328-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Veri kaynağını seçin](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="03328-128">LocalDB veya hangisinin bağlı olarak, yüklü, SQL Express için bağlanın ve girin **DatabaseFirst.Blogging** veritabanı adı</span><span class="sxs-lookup"><span data-stu-id="03328-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![SQL Express bağlantısı SD](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDB bağlantı SD](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="03328-131">Seçin **Tamam** ve bir yeni bir veritabanı oluşturmak istiyorsanız istenir **Evet**</span><span class="sxs-lookup"><span data-stu-id="03328-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Veritabanı iletişim kutusu oluşturma](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="03328-133">Yeni veritabanı sunucu Gezgini'nde artık görünecek üzerinde sağ tıklayıp **yeni sorgu**</span><span class="sxs-lookup"><span data-stu-id="03328-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="03328-134">Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **Yürüt**</span><span class="sxs-lookup"><span data-stu-id="03328-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);
```

## <a name="2-create-the-application"></a><span data-ttu-id="03328-135">2. Uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="03328-135">2. Create the Application</span></span>

<span data-ttu-id="03328-136">Örneği basit tutmak için veri erişimi gerçekleştirdiği ilk veritabanı kullanan temel bir konsol uygulaması oluşturmak için oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="03328-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="03328-137">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="03328-137">Open Visual Studio</span></span>
-   <span data-ttu-id="03328-138">**Dosya -&gt; yeni -&gt; proje...**</span><span class="sxs-lookup"><span data-stu-id="03328-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="03328-139">Seçin **Windows** sol menüden ve **konsol uygulaması**</span><span class="sxs-lookup"><span data-stu-id="03328-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="03328-140">Girin **DatabaseFirstSample** adı</span><span class="sxs-lookup"><span data-stu-id="03328-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="03328-141">Seçin **Tamam**</span><span class="sxs-lookup"><span data-stu-id="03328-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="03328-142">3. Ters mühendislik modeli</span><span class="sxs-lookup"><span data-stu-id="03328-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="03328-143">Modelimiz oluşturmak için Entity Framework Visual Studio'nun bir parçası olarak dahil olan Designer'ın, kullanan oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="03328-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="03328-144">**Takım projesi -&gt; Yeni Öğe Ekle...**</span><span class="sxs-lookup"><span data-stu-id="03328-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="03328-145">Seçin **veri** sol menüden ardından **ADO.NET varlık veri modeli**</span><span class="sxs-lookup"><span data-stu-id="03328-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="03328-146">Girin **BloggingModel** tıklayın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="03328-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="03328-147">Böylece **varlık veri modeli Sihirbazı**</span><span class="sxs-lookup"><span data-stu-id="03328-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="03328-148">Seçin **veritabanından Oluştur** tıklatıp **İleri**</span><span class="sxs-lookup"><span data-stu-id="03328-148">Select **Generate from Database** and click **Next**</span></span>

    ![Adım 1'in Sihirbazı](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="03328-150">İlk bölümde oluşturduğunuz veritabanı bağlantısı seçin, girin **BloggingContext** tıklayın ve bağlantı dizesi adı olarak **İleri**</span><span class="sxs-lookup"><span data-stu-id="03328-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![Sihirbazı adımı 2](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="03328-152">'Tüm tabloları Al ve 'Son' tablolar' yanındaki onay kutusuna tıklayın.</span><span class="sxs-lookup"><span data-stu-id="03328-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Sihirbazı adımı 3](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="03328-154">Ters mühendislik işlemi tamamlandıktan sonra yeni modeli projenize eklenir ve Entity Framework Tasarımcısı'nda görüntülemeniz için açıldı.</span><span class="sxs-lookup"><span data-stu-id="03328-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="03328-155">Bir App.config dosyası ayrıca veritabanı bağlantı ayrıntıları ile projenize eklendi.</span><span class="sxs-lookup"><span data-stu-id="03328-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![İlk model](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="03328-157">Visual Studio 2010'daki ek adımlar</span><span class="sxs-lookup"><span data-stu-id="03328-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="03328-158">Visual Studio 2010'da çalışıyorsanız Entity Framework'ün en son sürüme yükseltmek için izlemeniz gereken bazı ek adımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="03328-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="03328-159">Yükseltme önemlidir çünkü en son hata düzeltmeleri yanı sıra kullanmak çok daha kolaydır, geliştirilmiş bir API yüzeyi, erişmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="03328-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="03328-160">Öncelikle, Nuget'ten Entity Framework'ün en son sürümünü almak ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="03328-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="03328-161">\*\*Proje –&gt; NuGet paketlerini Yönet... \*\* 
     \*Yoksa \**NuGet paketlerini Yönet... \*\* yüklemelisiniz seçeneği [en son NuGet sürümünü](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="03328-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="03328-162">Seçin **çevrimiçi** sekmesi</span><span class="sxs-lookup"><span data-stu-id="03328-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="03328-163">Seçin **EntityFramework** paket</span><span class="sxs-lookup"><span data-stu-id="03328-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="03328-164">Tıklayın **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="03328-164">Click **Install**</span></span>

<span data-ttu-id="03328-165">Ardından, Entity Framework'ün sonraki sürümlerinde sunulan DbContext API kullanır kod üretmek için modelimizi takas etmek ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="03328-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="03328-166">Modelinizin EF Tasarımcısı'nda boş bir nokta üzerinde sağ tıklayıp **kod oluşturma öğesi Ekle...**</span><span class="sxs-lookup"><span data-stu-id="03328-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="03328-167">Seçin **çevrimiçi şablonlar** arayın ve sol menüden **DbContext**</span><span class="sxs-lookup"><span data-stu-id="03328-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="03328-168">EF seçin **5.x DbContext Oluşturucu c\#**, girin **BloggingModel** tıklayın ve adı olarak **Ekle**</span><span class="sxs-lookup"><span data-stu-id="03328-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![DbContext şablonu](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="03328-170">4. Okuma ve yazma veri</span><span class="sxs-lookup"><span data-stu-id="03328-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="03328-171">Biz bir modeliniz olduğuna göre bazı verilere erişmek için kullanılacak zaman var.</span><span class="sxs-lookup"><span data-stu-id="03328-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="03328-172">Sınıfları kullanacağız EDMX dosyasını temel alan için kullanılacak erişim verilerini otomatik olarak oluşturuluyor.</span><span class="sxs-lookup"><span data-stu-id="03328-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="03328-173">*Visual Studio 2010 kullanıyorsanız bu ekran görüntüsü, Visual Studio 2012'den BloggingModel.tt olduğu ve BloggingModel.Context.tt dosyalarını doğrudan projenize altında yerine EDMX dosyasının altında iç içe geçmiş.*</span><span class="sxs-lookup"><span data-stu-id="03328-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Oluşturulan sınıflar SD](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="03328-175">Main yöntemi program.cs'ye aşağıda gösterildiği gibi uygulayın.</span><span class="sxs-lookup"><span data-stu-id="03328-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="03328-176">Bu kod, bizim bağlam yeni bir örneğini oluşturur ve yeni bir Blog eklemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="03328-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="03328-177">Ardından veritabanından başlığa göre alfabetik olarak sıralanmış tüm blogları almak için LINQ sorgusu kullanır.</span><span class="sxs-lookup"><span data-stu-id="03328-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="03328-178">Şimdi uygulamayı çalıştırmak ve test etmek.</span><span class="sxs-lookup"><span data-stu-id="03328-178">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="03328-179">5. Veritabanı değişikliklerini uğraşmanızı</span><span class="sxs-lookup"><span data-stu-id="03328-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="03328-180">Artık biz de modelimiz, bu değişiklikleri yansıtacak şekilde güncelleştirilecek ihtiyacımız bu değişiklik yaptığınız zaman, bizim veritabanı şeması, bazı değişiklikler yapmanız gerekebileceğini zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="03328-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="03328-181">Veritabanı şemasına bazı değişiklikler yapmanız gerekebileceğini ilk adımdır bakın.</span><span class="sxs-lookup"><span data-stu-id="03328-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="03328-182">Bir kullanıcı tablosu için şema eklemek için ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="03328-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="03328-183">Sağ **DatabaseFirst.Blogging** seçin ve veritabanı sunucu Gezgini'nde **yeni sorgu**</span><span class="sxs-lookup"><span data-stu-id="03328-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="03328-184">Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **Yürüt**</span><span class="sxs-lookup"><span data-stu-id="03328-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="03328-185">Şema güncelleştirildi, modeli bu değişikliklerle güncelleştirmek için zaman var.</span><span class="sxs-lookup"><span data-stu-id="03328-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="03328-186">Boş bir nokta modelinize EF Designer ve Seç 'güncelleştirme modeli veritabanından...' na sağ tıklayın, bu güncelleştirme Sihirbazı başlatılır</span><span class="sxs-lookup"><span data-stu-id="03328-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="03328-187">Güncelleştirme Sihirbazı onay tabloları yanındaki kutuyu Ekle sekmesinde bu şemadan herhangi bir yeni tablolar eklemek istediğimiz gösterir.</span><span class="sxs-lookup"><span data-stu-id="03328-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="03328-188">*Yenileme sekmesi, güncelleştirme sırasında değişiklikleri için denetlenecek modelindeki mevcut tabloların gösterir. Delete sekmeler şemadan kaldırıldı ve modelden güncelleştirmenin bir parçası da kaldırılacak herhangi bir tablo gösterir. Bu iki sekmelerindeki bilgileri otomatik olarak algılanır ve yalnızca bilgilendirme amacıyla sağlanmıştır, herhangi bir ayarı değiştiremezsiniz.*</span><span class="sxs-lookup"><span data-stu-id="03328-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![Yenileme Sihirbazı](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="03328-190">Güncelleştirme Sihirbazı'nı Son'a tıklayın</span><span class="sxs-lookup"><span data-stu-id="03328-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="03328-191">Model veritabanına ekledik kullanıcılar tablo eşleşen yeni bir kullanıcı varlığı eklemek için güncelleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="03328-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![Güncelleştirilmiş model](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="03328-193">Özet</span><span class="sxs-lookup"><span data-stu-id="03328-193">Summary</span></span>

<span data-ttu-id="03328-194">Bu kılavuzda ilk veritabanı geliştirme incelemiştik olduğu EF tasarımcıda varolan bir veritabanını temel alan bir model oluşturmak olduk.</span><span class="sxs-lookup"><span data-stu-id="03328-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="03328-195">Ardından bu modelin bazı verileri veritabanından okuyup kullandık.</span><span class="sxs-lookup"><span data-stu-id="03328-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="03328-196">Son olarak, model veritabanı şemasına yaptık değişiklikleri yansıtacak şekilde güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="03328-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
