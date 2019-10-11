---
title: Var olan bir veritabanına Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 61980bbd1f236f496a9d4fd92aa52264f1454615
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182626"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="ef43d-102">Var olan bir veritabanına Code First</span><span class="sxs-lookup"><span data-stu-id="ef43d-102">Code First to an Existing Database</span></span>
<span data-ttu-id="ef43d-103">Bu video ve adım adım yönergeler, var olan bir veritabanını hedefleyen Code First geliştirmeye yönelik bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="ef43d-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="ef43d-104">Code First, modelinizi C @ no__t-0 veya VB.Net sınıfları kullanarak tanımlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ef43d-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="ef43d-105">İsteğe bağlı olarak, sınıflarınızda ve özelliklerde öznitelikler kullanılarak veya bir Fluent API kullanarak ek yapılandırma gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ef43d-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="ef43d-106">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="ef43d-106">Watch the video</span></span>
<span data-ttu-id="ef43d-107">Bu video [artık Channel 9 ' da kullanılabilir](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="ef43d-107">This video is [now available on Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="ef43d-108">Önkoşulların önkoşulları</span><span class="sxs-lookup"><span data-stu-id="ef43d-108">Pre-Requisites</span></span>

<span data-ttu-id="ef43d-109">Bu izlenecek yolu tamamlamak için **Visual Studio 2012** veya **Visual Studio 2013** yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ef43d-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="ef43d-110">Ayrıca, **Visual Studio için Entity Framework Tools** sürüm **6,1** (veya üzeri) yüklü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef43d-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="ef43d-111">Entity Framework Tools 'ın en son sürümünü yükleme hakkında bilgi için bkz. [Get Entity Framework](~/ef6/fundamentals/install.md) .</span><span class="sxs-lookup"><span data-stu-id="ef43d-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="ef43d-112">1. Var olan bir veritabanı oluştur</span><span class="sxs-lookup"><span data-stu-id="ef43d-112">1. Create an Existing Database</span></span>

<span data-ttu-id="ef43d-113">Genellikle, var olan bir veritabanını hedeflerken zaten oluşturulur, ancak bu izlenecek yol için, erişmek üzere bir veritabanı oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ef43d-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="ef43d-114">Şimdi veritabanını oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="ef43d-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="ef43d-115">Visual Studio 'Yu aç</span><span class="sxs-lookup"><span data-stu-id="ef43d-115">Open Visual Studio</span></span>
-   <span data-ttu-id="ef43d-116">**@No__t-1 Sunucu Gezgini görüntüle**</span><span class="sxs-lookup"><span data-stu-id="ef43d-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="ef43d-117">Veri bağlantıları ' na sağ tıklayın **-&gt; bağlantı ekle...**</span><span class="sxs-lookup"><span data-stu-id="ef43d-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="ef43d-118">**Sunucu Gezgini** bir veritabanına bağlı değilseniz, veri kaynağı olarak **Microsoft SQL Server** seçmeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="ef43d-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![Veri kaynağını seçin](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="ef43d-120">LocalDB örneğinize bağlanın ve veritabanı adı olarak **Blog** girin</span><span class="sxs-lookup"><span data-stu-id="ef43d-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![LocalDB bağlantısı](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="ef43d-122">**Tamam** ' ı seçtiğinizde, yeni bir veritabanı oluşturmak isteyip istemediğiniz sorulur, **Evet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="ef43d-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Veritabanı oluştur Iletişim kutusu](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="ef43d-124">Yeni veritabanı artık Sunucu Gezgini görüntülenir, sağ tıklayıp **Yeni sorgu** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="ef43d-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="ef43d-125">Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **Yürüt** ' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="ef43d-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="ef43d-126">2. Uygulamayı oluşturma</span><span class="sxs-lookup"><span data-stu-id="ef43d-126">2. Create the Application</span></span>

<span data-ttu-id="ef43d-127">Şeyleri basit tutmak için veri erişimi gerçekleştirmek üzere Code First kullanan temel bir konsol uygulaması oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="ef43d-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="ef43d-128">Visual Studio 'Yu aç</span><span class="sxs-lookup"><span data-stu-id="ef43d-128">Open Visual Studio</span></span>
-   <span data-ttu-id="ef43d-129">**Dosya-&gt; yeni-&gt; proje...**</span><span class="sxs-lookup"><span data-stu-id="ef43d-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="ef43d-130">Sol taraftaki menüden ve **konsol uygulamasından** **Windows** ' u seçin</span><span class="sxs-lookup"><span data-stu-id="ef43d-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="ef43d-131">Ad olarak **Codefırstexistingdatabasesample** girin</span><span class="sxs-lookup"><span data-stu-id="ef43d-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="ef43d-132">**Tamam 'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="ef43d-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="ef43d-133">3. Tersine mühendislik modeli</span><span class="sxs-lookup"><span data-stu-id="ef43d-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="ef43d-134">Veritabanına eşlemek için bazı ilk kod oluşturmamıza yardımcı olmak üzere Visual Studio için Entity Framework Tools kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="ef43d-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="ef43d-135">Bu araçlar, isterseniz, el ile de yazabileceğiniz bir kod oluşturuyor.</span><span class="sxs-lookup"><span data-stu-id="ef43d-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="ef43d-136">**Proje-&gt; yeni öğe Ekle...**</span><span class="sxs-lookup"><span data-stu-id="ef43d-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="ef43d-137">Sol menüden **verileri** seçin ve ardından **ADO.net varlık veri modeli**</span><span class="sxs-lookup"><span data-stu-id="ef43d-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="ef43d-138">Ad olarak **BloggingContext** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="ef43d-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="ef43d-139">Bu, **varlık veri modeli Sihirbazı 'nı** başlatır</span><span class="sxs-lookup"><span data-stu-id="ef43d-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="ef43d-140">**Veritabanından Code First** seçin ve **İleri** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ef43d-140">Select **Code First from Database** and click **Next**</span></span>

    ![Sihirbaz bir CFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="ef43d-142">İlk bölümde oluşturduğunuz veritabanına bağlantıyı seçin ve **İleri** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ef43d-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![Sihirbaz Iki CFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="ef43d-144">**Tablolar** ' ın yanındaki onay kutusuna tıklayarak tüm tabloları Içeri aktarıp **son** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ef43d-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![Sihirbaz üç CFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="ef43d-146">Tersine mühendislik işlemi tamamlandıktan sonra projeye bir dizi öğe eklendiğinde, nelerin eklendiğine göz atalım.</span><span class="sxs-lookup"><span data-stu-id="ef43d-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="ef43d-147">Yapılandırma dosyası</span><span class="sxs-lookup"><span data-stu-id="ef43d-147">Configuration file</span></span>

<span data-ttu-id="ef43d-148">Projeye bir App. config dosyası eklenmiştir, bu dosya mevcut veritabanına yönelik bağlantı dizesini içerir.</span><span class="sxs-lookup"><span data-stu-id="ef43d-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="ef43d-149">*yapılandırma dosyasında başka bazı ayarlar da fark edersiniz. Bunlar, veritabanlarının nerede oluşturulacağını Code First söyleyen varsayılan EF ayarlarıdır. Mevcut bir veritabanıyla eşlendiğimiz için bu ayar uygulamamızda yok sayılacak.*</span><span class="sxs-lookup"><span data-stu-id="ef43d-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="ef43d-150">Türetilmiş bağlam</span><span class="sxs-lookup"><span data-stu-id="ef43d-150">Derived Context</span></span>

<span data-ttu-id="ef43d-151">Projeye bir **BloggingContext** sınıfı eklendi.</span><span class="sxs-lookup"><span data-stu-id="ef43d-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="ef43d-152">Bağlam, veritabanı ile bir oturumu temsil eder ve verileri sorgulamanızı ve kaydetmemizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="ef43d-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="ef43d-153">Bağlam, modelimizin her türü için bir **Dbset @ no__t-1TEntity @ no__t-2** gösterir.</span><span class="sxs-lookup"><span data-stu-id="ef43d-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="ef43d-154">Ayrıca, varsayılan oluşturucunun **Name =** söz dizimini kullanarak bir temel Oluşturucu çağırdığına de dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ef43d-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="ef43d-155">Bu, bu bağlam için kullanılacak bağlantı dizesinin yapılandırma dosyasından yüklü olması gerektiğini Code First söyler.</span><span class="sxs-lookup"><span data-stu-id="ef43d-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

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

<span data-ttu-id="ef43d-156">*Yapılandırma dosyasında bir bağlantı dizesi kullanırken her zaman **Name =** sözdizimini kullanmanız gerekir. Bu, bağlantı dizesinin mevcut olmamasını sağlar ve Entity Framework kurala göre yeni bir veritabanı oluşturmak yerine oluşturulacak.*</span><span class="sxs-lookup"><span data-stu-id="ef43d-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="ef43d-157">Model sınıfları</span><span class="sxs-lookup"><span data-stu-id="ef43d-157">Model classes</span></span>

<span data-ttu-id="ef43d-158">Son olarak, projeye bir **Blog** ve **Post** sınıfı de eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="ef43d-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="ef43d-159">Bunlar, modelimizi oluşturan etki alanı sınıflarıdır.</span><span class="sxs-lookup"><span data-stu-id="ef43d-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="ef43d-160">Code First kurallarının mevcut veritabanının yapısıyla hizalanmayan yapılandırmayı belirtmek için sınıflara uygulanan veri ek açıklamalarını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="ef43d-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="ef43d-161">Örneğin, veritabanında maksimum **200** uzunluğuna sahip olduklarından **blog.Name** ve **blog. URL** ' de **StringLength** ek açıklamasını görürsünüz (Code First varsayılan değer veritabanı sağlayıcısı tarafından desteklenen en yüksek uzunluğu kullanmaktır- SQL Server) için **nvarchar (max)** ).</span><span class="sxs-lookup"><span data-stu-id="ef43d-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

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

## <a name="4-reading--writing-data"></a><span data-ttu-id="ef43d-162">4. Verileri okuma & yazma</span><span class="sxs-lookup"><span data-stu-id="ef43d-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="ef43d-163">Artık bir modelimiz olduğuna göre, bazı verilere erişmek için bunu kullanmanın zamanı.</span><span class="sxs-lookup"><span data-stu-id="ef43d-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="ef43d-164">**Ana** yöntemi aşağıda gösterildiği gibi **program.cs** ' de uygulayın.</span><span class="sxs-lookup"><span data-stu-id="ef43d-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="ef43d-165">Bu kod, bağlamımız yeni bir örnek oluşturur ve yeni bir **Blog**eklemek için onu kullanır.</span><span class="sxs-lookup"><span data-stu-id="ef43d-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="ef43d-166">Daha sonra, bir LINQ sorgusu kullanarak, veritabanındaki tüm **blogları** **başlık**sırasına göre sıralanmış olarak alır.</span><span class="sxs-lookup"><span data-stu-id="ef43d-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

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

<span data-ttu-id="ef43d-167">Şimdi uygulamayı çalıştırabilir ve test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef43d-167">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="ef43d-168">Veritabanım değişirse ne olacak?</span><span class="sxs-lookup"><span data-stu-id="ef43d-168">What if My Database Changes?</span></span>

<span data-ttu-id="ef43d-169">Veritabanına Code First Sihirbazı, daha sonra ince ayar ve değiştirebileceğiniz bir başlangıç noktası kümesi oluşturmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="ef43d-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="ef43d-170">Veritabanı şemanız değişirse, sınıfları el ile düzenleyebilir veya sınıfların üzerine yazmak için başka bir geri mühendis gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef43d-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="ef43d-171">Mevcut bir veritabanına Code First Migrations kullanma</span><span class="sxs-lookup"><span data-stu-id="ef43d-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="ef43d-172">Mevcut bir veritabanıyla Code First Migrations kullanmak istiyorsanız, bkz. [Code First Migrations var olan bir veritabanına](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="ef43d-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="ef43d-173">Özet</span><span class="sxs-lookup"><span data-stu-id="ef43d-173">Summary</span></span>

<span data-ttu-id="ef43d-174">Bu kılavuzda, var olan bir veritabanını kullanarak Code First geliştirmeye baktık.</span><span class="sxs-lookup"><span data-stu-id="ef43d-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="ef43d-175">Veritabanına eşlenen ve veri depolamak ve almak için kullanılabilecek bir sınıf kümesine ters mühendislik uygulamak için Visual Studio için Entity Framework Tools kullandık.</span><span class="sxs-lookup"><span data-stu-id="ef43d-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
