---
title: Database First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: d40cff4ddccf43a394ef4f244653372a5a89b05a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418359"
---
# <a name="database-first"></a><span data-ttu-id="236c9-102">Database First</span><span class="sxs-lookup"><span data-stu-id="236c9-102">Database First</span></span>
<span data-ttu-id="236c9-103">Bu video ve adım adım yönergeler, Entity Framework kullanarak Database First geliştirmeye yönelik bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="236c9-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="236c9-104">Database First, varolan bir veritabanından bir modele ters mühendislik yapmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="236c9-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="236c9-105">Model bir EDMX dosyasında (. edmx uzantılı) depolanır ve Entity Framework Designer görüntülenebilir ve düzenlenebilir.</span><span class="sxs-lookup"><span data-stu-id="236c9-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="236c9-106">Uygulamanızda etkileşimde bulunan sınıflar, EDMX dosyasından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="236c9-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="236c9-107">Videoyu izleme</span><span class="sxs-lookup"><span data-stu-id="236c9-107">Watch the video</span></span>
<span data-ttu-id="236c9-108">Bu videoda Entity Framework kullanarak Database First geliştirmeye bir giriş sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="236c9-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="236c9-109">Database First, varolan bir veritabanından bir modele ters mühendislik yapmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="236c9-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="236c9-110">Model bir EDMX dosyasında (. edmx uzantılı) depolanır ve Entity Framework Designer görüntülenebilir ve düzenlenebilir.</span><span class="sxs-lookup"><span data-stu-id="236c9-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="236c9-111">Uygulamanızda etkileşimde bulunan sınıflar, EDMX dosyasından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="236c9-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="236c9-112">**Sunulma ölçütü**: [Rowa Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="236c9-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="236c9-113">**Video**: [wmv](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="236c9-113">**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="236c9-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="236c9-114">Pre-Requisites</span></span>

<span data-ttu-id="236c9-115">Bu izlenecek yolu tamamlamak için en az Visual Studio 2010 veya Visual Studio 2012 yüklü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="236c9-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="236c9-116">Visual Studio 2010 kullanıyorsanız, [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) ' in yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="236c9-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="236c9-117">1. var olan bir veritabanını oluşturun</span><span class="sxs-lookup"><span data-stu-id="236c9-117">1. Create an Existing Database</span></span>

<span data-ttu-id="236c9-118">Genellikle, var olan bir veritabanını hedeflerken zaten oluşturulur, ancak bu izlenecek yol için, erişmek üzere bir veritabanı oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="236c9-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="236c9-119">Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösteren bir sürümdür:</span><span class="sxs-lookup"><span data-stu-id="236c9-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="236c9-120">Visual Studio 2010 kullanıyorsanız, bir SQL Express veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="236c9-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="236c9-121">Visual Studio 2012 kullanıyorsanız, [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="236c9-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="236c9-122">Şimdi veritabanını oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="236c9-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="236c9-123">Visual Studio’yu açın</span><span class="sxs-lookup"><span data-stu-id="236c9-123">Open Visual Studio</span></span>
-   <span data-ttu-id="236c9-124">**&gt; Sunucu Gezgini görüntüle**</span><span class="sxs-lookup"><span data-stu-id="236c9-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="236c9-125">Veri bağlantıları ' na sağ tıklayın **&gt; bağlantı ekle...**</span><span class="sxs-lookup"><span data-stu-id="236c9-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="236c9-126">Sunucu Gezgini bir veritabanına bağlı değilseniz, veri kaynağı olarak Microsoft SQL Server seçmeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="236c9-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Veri Kaynağı Seç](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="236c9-128">Hangisini yüklediğinize bağlı olarak LocalDB 'ye veya SQL Express 'e bağlanın ve veritabanı adı olarak **Databasefirst. blog** yazın</span><span class="sxs-lookup"><span data-stu-id="236c9-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![SQL Express bağlantı DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDB bağlantısı DF](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="236c9-131">**Tamam** ' ı seçtiğinizde, yeni bir veritabanı oluşturmak isteyip istemediğiniz sorulur, **Evet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="236c9-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Veritabanı oluştur Iletişim kutusu](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="236c9-133">Yeni veritabanı artık Sunucu Gezgini görüntülenir, sağ tıklayıp **Yeni sorgu** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="236c9-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="236c9-134">Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **Yürüt** ' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="236c9-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="236c9-135">2. uygulamayı oluşturun</span><span class="sxs-lookup"><span data-stu-id="236c9-135">2. Create the Application</span></span>

<span data-ttu-id="236c9-136">Şeyleri basit tutmak için, veri erişimi gerçekleştirmek üzere Database First kullanan temel bir konsol uygulaması oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="236c9-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="236c9-137">Visual Studio’yu açın</span><span class="sxs-lookup"><span data-stu-id="236c9-137">Open Visual Studio</span></span>
-   <span data-ttu-id="236c9-138">**Dosya-&gt; yeni&gt; projesi...**</span><span class="sxs-lookup"><span data-stu-id="236c9-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="236c9-139">Sol taraftaki menüden ve **konsol uygulamasından** **Windows** ' u seçin</span><span class="sxs-lookup"><span data-stu-id="236c9-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="236c9-140">Ad olarak **Databasefirstsample** yazın</span><span class="sxs-lookup"><span data-stu-id="236c9-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="236c9-141">**Tamam**’ı seçin</span><span class="sxs-lookup"><span data-stu-id="236c9-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="236c9-142">3. tersine mühendislik modeli</span><span class="sxs-lookup"><span data-stu-id="236c9-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="236c9-143">Modelimizi oluşturmak için Visual Studio 'nun bir parçası olarak dahil edilen Entity Framework Designer kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="236c9-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="236c9-144">**Proje-&gt; yeni öğe Ekle...**</span><span class="sxs-lookup"><span data-stu-id="236c9-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="236c9-145">Sol menüden **verileri** seçin ve ardından **ADO.net varlık veri modeli**</span><span class="sxs-lookup"><span data-stu-id="236c9-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="236c9-146">Ad olarak **BloggingModel** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="236c9-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="236c9-147">Bu, **varlık veri modeli Sihirbazı 'nı** başlatır</span><span class="sxs-lookup"><span data-stu-id="236c9-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="236c9-148">**Veritabanından oluştur** ' u seçin ve **İleri** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="236c9-148">Select **Generate from Database** and click **Next**</span></span>

    ![Sihirbaz 1. adım](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="236c9-150">İlk bölümde oluşturduğunuz veritabanına bağlantıyı seçin, bağlantı dizesinin adı olarak **BloggingContext** girin ve **İleri** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="236c9-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![Sihirbaz 2. adım](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="236c9-152">Tüm tabloları içeri aktarmak için ' tablolar ' seçeneğinin yanındaki onay kutusuna tıklayın ve ' son ' a tıklayın</span><span class="sxs-lookup"><span data-stu-id="236c9-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Sihirbaz 3. adım](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="236c9-154">Tersine mühendislik işlemi tamamlandıktan sonra, yeni model projenize eklenir ve Entity Framework Designer görüntülemeniz için açılır.</span><span class="sxs-lookup"><span data-stu-id="236c9-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="236c9-155">Veritabanına yönelik bağlantı ayrıntılarına sahip bir App. config dosyası da projenize eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="236c9-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![Ilk model](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="236c9-157">Visual Studio 2010 ' de ek adımlar</span><span class="sxs-lookup"><span data-stu-id="236c9-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="236c9-158">Visual Studio 2010 ' de çalışıyorsanız, en son Entity Framework sürümüne yükseltmek için izlemeniz gereken bazı ek adımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="236c9-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="236c9-159">' Nin, kullanımı çok daha kolay olan gelişmiş bir API yüzeyine erişim sağladığından ve en son hata düzeltmelerinin yanı sıra yükseltme önemlidir.</span><span class="sxs-lookup"><span data-stu-id="236c9-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="236c9-160">İlk olarak, Entity Framework NuGet 'den en son sürümü alması gerekir.</span><span class="sxs-lookup"><span data-stu-id="236c9-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="236c9-161">**Proje –&gt; NuGet Paketlerini Yönet...** \* **NuGet Paketlerini Yönet...** seçeneğine sahipseniz
    [NuGet 'in en son sürümünü](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) yüklemelisiniz\*</span><span class="sxs-lookup"><span data-stu-id="236c9-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="236c9-162">**Çevrimiçi** sekmesini seçin</span><span class="sxs-lookup"><span data-stu-id="236c9-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="236c9-163">**EntityFramework** paketini seçin</span><span class="sxs-lookup"><span data-stu-id="236c9-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="236c9-164">**Install** 'a tıklayın</span><span class="sxs-lookup"><span data-stu-id="236c9-164">Click **Install**</span></span>

<span data-ttu-id="236c9-165">Bundan sonra, Entity Framework sonraki sürümlerinde tanıtılan DbContext API 'sini kullanan kodu oluşturmak için modelimizi değiştirmemiz gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="236c9-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="236c9-166">EF Designer 'daki modelinizin boş bir noktasına sağ tıklayıp **kod oluşturma öğesi Ekle...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="236c9-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="236c9-167">Sol menüden **çevrimiçi şablonlar** ' ı seçin ve **DbContext** ' i arayın</span><span class="sxs-lookup"><span data-stu-id="236c9-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="236c9-168">**C\#IÇIN EF 5. x DbContext oluşturucusunu** seçin, ad olarak **BloggingModel** girin ve **Ekle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="236c9-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![DbContext şablonu](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="236c9-170">4. verileri okuma & yazma</span><span class="sxs-lookup"><span data-stu-id="236c9-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="236c9-171">Artık bir modelimiz olduğuna göre, bazı verilere erişmek için bunu kullanmanın zamanı.</span><span class="sxs-lookup"><span data-stu-id="236c9-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="236c9-172">Verilere erişmek için kullanacağınız sınıflar, EDMX dosyasına bağlı olarak sizin için otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="236c9-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="236c9-173">*Bu ekran görüntüsü Visual Studio 2012 ' den, Visual Studio 2010 kullanıyorsanız BloggingModel.tt ve BloggingModel.Context.tt dosyaları, EDMX dosyasının altında iç içe değil doğrudan projenizin altında olacaktır.*</span><span class="sxs-lookup"><span data-stu-id="236c9-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Oluşturulan sınıflar DF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="236c9-175">Ana yöntemi aşağıda gösterildiği gibi Program.cs ' de uygulayın.</span><span class="sxs-lookup"><span data-stu-id="236c9-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="236c9-176">Bu kod, bağlamımız yeni bir örnek oluşturur ve yeni bir blog eklemek için onu kullanır.</span><span class="sxs-lookup"><span data-stu-id="236c9-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="236c9-177">Daha sonra, bir LINQ sorgusu kullanarak, veritabanındaki tüm blogları başlık sırasına göre sıralanmış olarak alır.</span><span class="sxs-lookup"><span data-stu-id="236c9-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="236c9-178">Şimdi uygulamayı çalıştırabilir ve test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="236c9-178">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="236c9-179">5. veritabanı değişiklikleriyle ilgilenme</span><span class="sxs-lookup"><span data-stu-id="236c9-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="236c9-180">Artık veritabanı şemanızda bazı değişiklikler yapmanız zaman alabilir. bu değişiklikleri yaparken, bu değişiklikleri yansıtacak şekilde modelimizi de güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="236c9-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="236c9-181">İlk adım, veritabanı şemasında bazı değişiklikler yapmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="236c9-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="236c9-182">Şemaya bir kullanıcı tablosu ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="236c9-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="236c9-183">Sunucu Gezgini ve **Yeni sorgu** ' yı seçin **.**</span><span class="sxs-lookup"><span data-stu-id="236c9-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="236c9-184">Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **Yürüt** ' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="236c9-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="236c9-185">Artık şema güncellendiğinden, modelin bu değişikliklerle güncelleştirilmesi zaman atalım.</span><span class="sxs-lookup"><span data-stu-id="236c9-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="236c9-186">EF Designer 'daki modelinizin boş bir noktasına sağ tıklayın ve ' veritabanını güncelleştir... ' seçeneğini belirleyin, bu işlem güncelleştirme sihirbazını başlatır</span><span class="sxs-lookup"><span data-stu-id="236c9-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="236c9-187">Güncelleştirme sihirbazının Ekle sekmesinde tablolar ' ın yanındaki kutuyu işaretleyin, bundan sonra şemadan yeni tablolar eklemek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="236c9-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="236c9-188">*Yenile sekmesi, modelde, güncelleştirme sırasında değişiklikler için denetlenecek tüm mevcut tabloları gösterir. Silme sekmeleri şemadan kaldırılan tüm tabloları gösterir ve güncelleştirmenin bir parçası olarak modelden de kaldırılır. Bu iki sekmede bulunan bilgiler otomatik olarak algılanır ve yalnızca bilgilendirme amacıyla sağlanır, hiçbir ayarı değiştiremezsiniz.*</span><span class="sxs-lookup"><span data-stu-id="236c9-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![Sihirbazı Yenile](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="236c9-190">Güncelleştirme sihirbazında son ' a tıklayın</span><span class="sxs-lookup"><span data-stu-id="236c9-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="236c9-191">Model artık veritabanına eklediğimiz Kullanıcı tablosuyla eşleşen yeni bir kullanıcı varlığı içerecek şekilde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="236c9-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![Model güncelleştirildi](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="236c9-193">Özet</span><span class="sxs-lookup"><span data-stu-id="236c9-193">Summary</span></span>

<span data-ttu-id="236c9-194">Bu kılavuzda, var olan bir veritabanını temel alan EF tasarımcısında bir model oluşturmamızı sağlayan Database First geliştirmeyi inceledik.</span><span class="sxs-lookup"><span data-stu-id="236c9-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="236c9-195">Daha sonra bu modeli veritabanından bazı verileri okumak ve yazmak için kullandık.</span><span class="sxs-lookup"><span data-stu-id="236c9-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="236c9-196">Son olarak, modeli veritabanı şemasında yaptığımız değişiklikleri yansıtacak şekilde güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="236c9-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
