---
title: Mevcut bir veritabanına - EF6 öncelikle kod
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
caps.latest.revision: 3
ms.openlocfilehash: 47581a7ae9ff534d26ce82365bcbe2b254247495
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912726"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="1bc11-102">Mevcut bir veritabanına ilk kod</span><span class="sxs-lookup"><span data-stu-id="1bc11-102">Code First to an Existing Database</span></span>
<span data-ttu-id="1bc11-103">Bu video ve adım adım izlenecek yol, var olan bir veritabanını hedefleyen Code First geliştirmeye giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="1bc11-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="1bc11-104">Kod ilk sağlar, C kullanarak modelinizi tanımlamanızı\# veya VB.Net sınıflar.</span><span class="sxs-lookup"><span data-stu-id="1bc11-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="1bc11-105">İsteğe bağlı olarak ek yapılandırma, sınıfları ve özellikleri ya da fluent API'sini kullanarak öznitelikleri kullanılarak gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="1bc11-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="1bc11-106">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="1bc11-106">Watch the video</span></span>
<span data-ttu-id="1bc11-107">Bu video [Channel 9 sunuldu](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="1bc11-107">This video is [now available on Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="1bc11-108">Ön koşullar</span><span class="sxs-lookup"><span data-stu-id="1bc11-108">Pre-Requisites</span></span>

<span data-ttu-id="1bc11-109">Sahip olması gerekir **Visual Studio 2012** veya **Visual Studio 2013** Bu izlenecek yolu tamamlamak için yüklü.</span><span class="sxs-lookup"><span data-stu-id="1bc11-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="1bc11-110">Ayrıca sürüm gerekir **6.1** (veya üzeri), **Visual Studio için Entity Framework Tools** yüklü.</span><span class="sxs-lookup"><span data-stu-id="1bc11-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="1bc11-111">Bkz: [alma Entity Framework](~/ef6/fundamentals/install.md) Entity Framework Araçları'nın en son sürümü yükleme hakkında bilgi.</span><span class="sxs-lookup"><span data-stu-id="1bc11-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="1bc11-112">1. Mevcut bir veritabanı oluşturun</span><span class="sxs-lookup"><span data-stu-id="1bc11-112">1. Create an Existing Database</span></span>

<span data-ttu-id="1bc11-113">Genellikle zaten oluşturulur varolan bir veritabanını hedeflerken, ancak bu kılavuz erişmek için bir veritabanı oluşturmak ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="1bc11-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="1bc11-114">Yeni bir ubuntu ve veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1bc11-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="1bc11-115">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="1bc11-115">Open Visual Studio</span></span>
-   <span data-ttu-id="1bc11-116">**Görünüm -&gt; Sunucu Gezgini**</span><span class="sxs-lookup"><span data-stu-id="1bc11-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="1bc11-117">Sağ tıklayın **veri bağlantıları -&gt; bağlantı ekle...**</span><span class="sxs-lookup"><span data-stu-id="1bc11-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="1bc11-118">Bir veritabanına bağlamadıysanız **Sunucu Gezgini** seçmeniz gerekir önce **Microsoft SQL Server** veri kaynağı</span><span class="sxs-lookup"><span data-stu-id="1bc11-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="1bc11-120">LocalDB Örneğinize bağlanmak ve girin **blog** veritabanı adı</span><span class="sxs-lookup"><span data-stu-id="1bc11-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![LocalDBConnection](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="1bc11-122">Seçin **Tamam** ve bir yeni bir veritabanı oluşturmak istiyorsanız istenir **Evet**</span><span class="sxs-lookup"><span data-stu-id="1bc11-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabaseDialog](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="1bc11-124">Yeni veritabanı sunucu Gezgini'nde artık görünecek üzerinde sağ tıklayıp **yeni sorgu**</span><span class="sxs-lookup"><span data-stu-id="1bc11-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="1bc11-125">Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **Yürüt**</span><span class="sxs-lookup"><span data-stu-id="1bc11-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a><span data-ttu-id="1bc11-126">2. Uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="1bc11-126">2. Create the Application</span></span>

<span data-ttu-id="1bc11-127">Örneği basit tutmak için yapı veri erişimi gerçekleştirdiği Code First kullanan temel bir konsol uygulaması oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="1bc11-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="1bc11-128">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="1bc11-128">Open Visual Studio</span></span>
-   <span data-ttu-id="1bc11-129">**Dosya -&gt; yeni -&gt; proje...**</span><span class="sxs-lookup"><span data-stu-id="1bc11-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="1bc11-130">Seçin **Windows** sol menüden ve **konsol uygulaması**</span><span class="sxs-lookup"><span data-stu-id="1bc11-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="1bc11-131">Girin **CodeFirstExistingDatabaseSample** adı</span><span class="sxs-lookup"><span data-stu-id="1bc11-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="1bc11-132">Seçin **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1bc11-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="1bc11-133">3. Ters mühendislik modeli</span><span class="sxs-lookup"><span data-stu-id="1bc11-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="1bc11-134">Visual Studio için Entity Framework Araçları, veritabanı için ilk kod oluşturmamızı sağlayacak yararlanması oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="1bc11-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="1bc11-135">Bu araçlar, yalnızca tercih ederseniz, ayrıca, el ile yazabilirsiniz kod oluşturma.</span><span class="sxs-lookup"><span data-stu-id="1bc11-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="1bc11-136">**Takım projesi -&gt; Yeni Öğe Ekle...**</span><span class="sxs-lookup"><span data-stu-id="1bc11-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="1bc11-137">Seçin **veri** sol menüden ardından **ADO.NET varlık veri modeli**</span><span class="sxs-lookup"><span data-stu-id="1bc11-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="1bc11-138">Girin **BloggingContext** tıklayın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1bc11-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="1bc11-139">Böylece **varlık veri modeli Sihirbazı**</span><span class="sxs-lookup"><span data-stu-id="1bc11-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="1bc11-140">Seçin **Code First veritabanından** tıklatıp **İleri**</span><span class="sxs-lookup"><span data-stu-id="1bc11-140">Select **Code First from Database** and click **Next**</span></span>

    ![WizardOneCFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="1bc11-142">İlk bölümde oluşturduğunuz veritabanı bağlantısı seçin ve tıklayın **İleri**</span><span class="sxs-lookup"><span data-stu-id="1bc11-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![WizardTwoCFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="1bc11-144">Yanındaki onay kutusuna tıklayın **tabloları** tüm tabloları içeri aktarma ve tıklayın **son**</span><span class="sxs-lookup"><span data-stu-id="1bc11-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![WizardThreeCFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="1bc11-146">Öğe sayısı ters mühendislik işlemi tamamlandıktan sonra projeye şimdi eklenecek ne eklenmiş bir göz atalım.</span><span class="sxs-lookup"><span data-stu-id="1bc11-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="1bc11-147">Yapılandırma dosyası</span><span class="sxs-lookup"><span data-stu-id="1bc11-147">Configuration file</span></span>

<span data-ttu-id="1bc11-148">Bir App.config dosyası eklendi projeye, bu dosya, var olan veritabanına bağlantı dizesi içerir.</span><span class="sxs-lookup"><span data-stu-id="1bc11-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="1bc11-149">*Diğer ayarlarında bir yapılandırma dosyası çok fark edeceksiniz, Code First veritabanları oluşturma yeri bildirme varsayılan EF ayarları şunlardır. Bu ayar yoksayılacak varolan bir veritabanına uygulamamız içinde eşleyeceğiniz olduğundan.*</span><span class="sxs-lookup"><span data-stu-id="1bc11-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="1bc11-150">Türetilen içerik</span><span class="sxs-lookup"><span data-stu-id="1bc11-150">Derived Context</span></span>

<span data-ttu-id="1bc11-151">A **BloggingContext** sınıfı projeye eklendi.</span><span class="sxs-lookup"><span data-stu-id="1bc11-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="1bc11-152">Bağlam, sorgu ve veri kaydetmek bize izin vererek, veritabanı ile bir oturumu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="1bc11-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="1bc11-153">İçerik sunan bir **olan DB&lt;TEntity&gt;**  modelimiz, her tür için.</span><span class="sxs-lookup"><span data-stu-id="1bc11-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="1bc11-154">Varsayılan Oluşturucu kullanarak temel oluşturucu çağrısı da fark edeceksiniz **adı =** söz dizimi.</span><span class="sxs-lookup"><span data-stu-id="1bc11-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="1bc11-155">Bu kod ilk bu bağlam için kullanılacak bağlantı dizesini yapılandırma dosyasından yüklenmesi gerektiğini bildirir.</span><span class="sxs-lookup"><span data-stu-id="1bc11-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

<span data-ttu-id="1bc11-156">*Her zaman kullanmalısınız **adı =** yapılandırma dosyasında bir bağlantı dizesi kullanırken söz dizimi. Bu bağlantı dizesi, mevcut değilse, Entity Framework ardından oluşturmaz sağlar kurala göre yeni bir veritabanı oluşturmak yerine.*</span><span class="sxs-lookup"><span data-stu-id="1bc11-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="1bc11-157">Model sınıfları</span><span class="sxs-lookup"><span data-stu-id="1bc11-157">Model classes</span></span>

<span data-ttu-id="1bc11-158">Son olarak, bir **Blog** ve **Post** sınıfı da projeye eklendi.</span><span class="sxs-lookup"><span data-stu-id="1bc11-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="1bc11-159">Modelimiz yapmak alan sınıfları şunlardır.</span><span class="sxs-lookup"><span data-stu-id="1bc11-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="1bc11-160">Burada kod öncelikli kurallar var olan veritabanının yapısını hizalama değil, yapılandırmayı belirtmek için veri sınıfları için uygulanan ek görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="1bc11-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="1bc11-161">Örneğin, gördüğünüz **StringLength** üzerindeki ek açıklama **Blog.Name** ve **Blog.Url** bunlar en fazla olduğundan **200** içinde Veritabanı (veritabanı sağlayıcısı tarafından - desteklenen izin verilen en fazla uzunluk kullanmak için Code First varsayılandır **nvarchar(max)** SQL Server).</span><span class="sxs-lookup"><span data-stu-id="1bc11-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a><span data-ttu-id="1bc11-162">4. Okuma ve yazma veri</span><span class="sxs-lookup"><span data-stu-id="1bc11-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="1bc11-163">Biz bir modeliniz olduğuna göre bazı verilere erişmek için kullanılacak zaman var.</span><span class="sxs-lookup"><span data-stu-id="1bc11-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="1bc11-164">Uygulama **ana** yönteminde **Program.cs** aşağıda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="1bc11-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="1bc11-165">Bu kod, bizim bağlam yeni bir örneğini oluşturur ve yeni bir eklemek için kullandığı **Blog**.</span><span class="sxs-lookup"><span data-stu-id="1bc11-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="1bc11-166">Tüm almak için bir LINQ Sorgu kullanıyorsa **blogları** göre alfabetik olarak sıralanmış veritabanından **başlık**.</span><span class="sxs-lookup"><span data-stu-id="1bc11-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

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

<span data-ttu-id="1bc11-167">Şimdi uygulamayı çalıştırmak ve test etmek.</span><span class="sxs-lookup"><span data-stu-id="1bc11-167">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="1bc11-168">Veritabanım varsa ne değiştirir?</span><span class="sxs-lookup"><span data-stu-id="1bc11-168">What if My Database Changes?</span></span>

<span data-ttu-id="1bc11-169">Code First Veritabanı Sihirbazı'nı, ardından ince ve değiştirebileceği sınıf bir başlangıç noktası kümesi oluşturmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="1bc11-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="1bc11-170">Veritabanı şemanızı değişirse el ile düzenleme sınıfları veya sınıflar üzerine yazmak için başka bir ters mühendislik gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="1bc11-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="1bc11-171">Varolan bir veritabanına Code First geçişleri kullanma</span><span class="sxs-lookup"><span data-stu-id="1bc11-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="1bc11-172">Var olan bir veritabanına Code First Migrations'ı kullanmak istiyorsanız, bkz. [var olan bir veritabanına Code First Migrations](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="1bc11-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="1bc11-173">Özet</span><span class="sxs-lookup"><span data-stu-id="1bc11-173">Summary</span></span>

<span data-ttu-id="1bc11-174">Bu izlenecek yolda Code First geliştirme varolan bir veritabanını kullanma incelemiştik.</span><span class="sxs-lookup"><span data-stu-id="1bc11-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="1bc11-175">Ters mühendislik veritabanına eşlediğiniz ve veri depolayıp almak için kullanılabilir sınıf kümesi için Entity Framework Araçları Visual Studio için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1bc11-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
