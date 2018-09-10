---
title: Veri bağlama WPF - EF6 ile
author: divega
ms.date: 2016-10-23
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: e6df90db17d39d3aa91275800a6414fed40fb5db
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251160"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="bb8fa-102">Veri bağlama WPF ile</span><span class="sxs-lookup"><span data-stu-id="bb8fa-102">Databinding with WPF</span></span>
<span data-ttu-id="bb8fa-103">Bu adım adım kılavuzda, POCO türleri "ana öğe-ayrıntı" formunda WPF denetimleri bağlama işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="bb8fa-104">Uygulama, verileri veritabanından nesnelerle doldurmak, değişiklikleri izlemek ve veritabanına verileri kalıcı hale getirmek için Entity Framework API'leri kullanır.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="bb8fa-105">Model-çok ilişkisini katılan iki tür tanımlar: **kategori** (asıl\\ana) ve **ürün** (bağımlı\\ayrıntı).</span><span class="sxs-lookup"><span data-stu-id="bb8fa-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="bb8fa-106">Ardından, Visual Studio Araçları, WPF denetimleri için modelde tanımlanan türleri bağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="bb8fa-107">WPF veri bağlama çerçeve ilgili nesneler arasında gezinme sağlar: ana görünümünde satırları seçme ile ilgili alt verileri güncelleştirmek ayrıntılı görünüm neden olur.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="bb8fa-108">Bu izlenecek yolda kod listeleri ve ekran görüntüleri, Visual Studio 2013'ten alınır ancak bu kılavuzda Visual Studio 2012 veya Visual Studio 2010 ile tamamlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="bb8fa-109">WPF veri kaynakları oluşturmak için 'Object' seçeneğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="bb8fa-110">Kullanılması için kullandığımız Entity Framework'ın önceki sürümüyle **veritabanı** temel EF Designer ile oluşturulan bir model üzerinde yeni bir veri kaynağı oluşturma seçeneği.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="bb8fa-111">Tasarımcı ObjectContext türetilmiş bir bağlam ve EntityObject türetilen varlık sınıfları oluşturur, çünkü bu değildi.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="bb8fa-112">Veritabanı seçeneğini kullanarak bu API yüzeyi ile etkileşim kurmak için en iyi kod yazmanıza yardımcı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="bb8fa-113">Visual Studio 2012 ve Visual Studio 2013 için EF tasarımcıları basit POCO varlık sınıfları birlikte DbContext türeyen bir bağlam oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="bb8fa-114">Visual Studio 2010 ile bu anlatımın ilerleyen kısımlarında açıklandığı gibi DbContext kullanan kod oluşturma şablonuna takas öneririz.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="bb8fa-115">DbContext API yüzeyi kullanırken kullanmalısınız **nesne** seçenek yeni bir veri kaynağı oluşturulurken bu örneklerde gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="bb8fa-116">Gerekirse, [ObjectContext göre kod oluşturma için geri](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) EF Designer ile oluşturulan modeller için.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="bb8fa-117">Ön koşullar</span><span class="sxs-lookup"><span data-stu-id="bb8fa-117">Pre-Requisites</span></span>

<span data-ttu-id="bb8fa-118">Visual Studio 2013 olması gerekir, bu izlenecek yolu tamamlamak için Visual Studio 2012 veya Visual Studio 2010 'un yüklü.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="bb8fa-119">Visual Studio 2010 kullanıyorsanız, aynı zamanda NuGet yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="bb8fa-120">Daha fazla bilgi için [NuGet yükleme](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="bb8fa-120">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="bb8fa-121">Uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="bb8fa-121">Create the Application</span></span>

-   <span data-ttu-id="bb8fa-122">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="bb8fa-122">Open Visual Studio</span></span>
-   <span data-ttu-id="bb8fa-123">**Dosya -&gt; yeni -&gt; proje...**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="bb8fa-124">Seçin **Windows** sol bölmede ve **WPFApplication** sağ bölmede</span><span class="sxs-lookup"><span data-stu-id="bb8fa-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="bb8fa-125">Girin **WPFwithEFSample** adı</span><span class="sxs-lookup"><span data-stu-id="bb8fa-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="bb8fa-126">Seçin **Tamam**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="bb8fa-127">Entity Framework NuGet paketini yükle</span><span class="sxs-lookup"><span data-stu-id="bb8fa-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="bb8fa-128">Çözüm Gezgini'nde sağ **WinFormswithEFSample** proje</span><span class="sxs-lookup"><span data-stu-id="bb8fa-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="bb8fa-129">Seçin **NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="bb8fa-130">NuGet paketlerini Yönet iletişim kutusunda, seçmek **çevrimiçi** sekmesini **EntityFramework** paket</span><span class="sxs-lookup"><span data-stu-id="bb8fa-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="bb8fa-131">Tıklayın **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="bb8fa-132">EntityFramework derlemeye ek olarak bir başvuru System.ComponentModel.DataAnnotations de eklenir.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="bb8fa-133">Projeyi bir başvuru System.Data.Entity varsa, EntityFramework paketi yüklendiğinde, sonra da kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="bb8fa-134">System.Data.Entity derleme için Entity Framework 6 uygulamaları artık kullanılmamaktadır.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="bb8fa-135">Bir modeli tanımlamak</span><span class="sxs-lookup"><span data-stu-id="bb8fa-135">Define a Model</span></span>

<span data-ttu-id="bb8fa-136">Code First veya EF Designer kullanarak bir model uygulamak aşağıdakileri yapabilirsiniz, bu izlenecek yolda seçtiniz.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="bb8fa-137">İki bölümden birini tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="bb8fa-138">1. seçenek: Code First kullanarak bir Model tanımlayın</span><span class="sxs-lookup"><span data-stu-id="bb8fa-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="bb8fa-139">Bu bölümde, bir model ve Code First kullanarak ilişkili veritabanı oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="bb8fa-140">Sonraki bölümde Atla (**seçeneği 2: veritabanı ilk kullanarak bir modeli tanımlamak)** modelinizi EF designer kullanarak bir veritabanından, bunun yerine veritabanı ilk tersine çevirmek için kullanacağınız, mühendislik</span><span class="sxs-lookup"><span data-stu-id="bb8fa-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="bb8fa-141">Code First geliştirme kullanırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan bir .NET Framework sınıf yazarak başlayın.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="bb8fa-142">Yeni bir sınıfa eklemek **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="bb8fa-143">Proje adına sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="bb8fa-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="bb8fa-144">Seçin **ekleme**, ardından **yeni öğe**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="bb8fa-145">Seçin **sınıfı** girin **ürün** sınıfı adı</span><span class="sxs-lookup"><span data-stu-id="bb8fa-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="bb8fa-146">Değiştirin **ürün** sınıf tanımını aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="bb8fa-146">Replace the **Product** class definition with the following code:</span></span>

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

<span data-ttu-id="bb8fa-147">**Ürünleri** özelliği **kategori** sınıfı ve **kategori** özelliği **ürün** Gezinti özellikleri sınıfı bulunur.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="bb8fa-148">Varlık Çerçevesi'nde, gezinti özellikleri iki varlık türleri arasındaki bir ilişki gitmek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="bb8fa-149">Varlıklar tanımlayan yanı sıra olan DB kullanıma sunar ve DbContext türeyen bir sınıfı tanımlamanız gereken&lt;TEntity&gt; özellikleri.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="bb8fa-150">Olan DB&lt;TEntity&gt; özellikleri modele dahil etmek istediğiniz hangi türlerin bilmeniz bağlam olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="bb8fa-151">Nesneleri bir veritabanındaki verilerle doldurma içeren çalışma zamanı sırasında varlık nesnesi DbContext türetilmiş türün bir örneğini yönetir, izleme ve kalıcı veri veritabanına değiştirin.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="bb8fa-152">Yeni bir **ProductContext** projeye aşağıdaki tanımıyla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="bb8fa-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="bb8fa-153">Projeyi derle.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="bb8fa-154">2. seçenek: Veritabanı ilk kullanarak bir model tanımlayın</span><span class="sxs-lookup"><span data-stu-id="bb8fa-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="bb8fa-155">Bu bölümde, modelinizi EF designer kullanarak bir veritabanındaki veritabanı ilk tersine mühendislik için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="bb8fa-156">Önceki bölümde tamamladıysanız (**1. seçenek: Code First kullanarak bir modeli tanımlamak)**, ardından bu bölümü atlayın ve gidebilirsiniz **yavaş Yükleniyor** bölümü.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="bb8fa-157">Mevcut bir veritabanı oluşturun</span><span class="sxs-lookup"><span data-stu-id="bb8fa-157">Create an Existing Database</span></span>

<span data-ttu-id="bb8fa-158">Genellikle zaten oluşturulur varolan bir veritabanını hedeflerken, ancak bu kılavuz erişmek için bir veritabanı oluşturmak ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="bb8fa-159">Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösterir:</span><span class="sxs-lookup"><span data-stu-id="bb8fa-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="bb8fa-160">Visual Studio 2010 kullanıyorsanız, SQL Express veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="bb8fa-161">Visual Studio 2012 kullanıyorsanız sonra, oluşturduğunuz bir [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="bb8fa-162">Yeni bir ubuntu ve veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="bb8fa-163">**Görünüm -&gt; Sunucu Gezgini**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="bb8fa-164">Sağ tıklayın **veri bağlantıları -&gt; bağlantı ekle...**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="bb8fa-165">Sunucu gezgininden veritabanına bağlamadıysanız önce Microsoft SQL Server veri kaynağı olarak seçmeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="bb8fa-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Veri kaynağını Değiştir](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="bb8fa-167">LocalDB veya hangisinin bağlı olarak, yüklü, SQL Express için bağlanın ve girin **ürünleri** veritabanı adı</span><span class="sxs-lookup"><span data-stu-id="bb8fa-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Bağlantı LocalDB Ekle](~/ef6/media/addconnectionlocaldb.png)

    ![Bağlantı Express Ekle](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="bb8fa-170">Seçin **Tamam** ve bir yeni bir veritabanı oluşturmak istiyorsanız istenir **Evet**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Veritabanı oluşturma](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="bb8fa-172">Yeni veritabanı sunucu Gezgini'nde artık görünecek üzerinde sağ tıklayıp **yeni sorgu**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="bb8fa-173">Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **Yürüt**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a><span data-ttu-id="bb8fa-174">Ters mühendislik modeli</span><span class="sxs-lookup"><span data-stu-id="bb8fa-174">Reverse Engineer Model</span></span>

<span data-ttu-id="bb8fa-175">Modelimiz oluşturmak için Entity Framework Visual Studio'nun bir parçası olarak dahil olan Designer'ın, kullanan oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="bb8fa-176">**Takım projesi -&gt; Yeni Öğe Ekle...**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="bb8fa-177">Seçin **veri** sol menüden ardından **ADO.NET varlık veri modeli**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="bb8fa-178">Girin **ProductModel** tıklayın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="bb8fa-179">Böylece **varlık veri modeli Sihirbazı**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="bb8fa-180">Seçin **veritabanından Oluştur** tıklatıp **İleri**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-180">Select **Generate from Database** and click **Next**</span></span>

    ![Model içeriğinin seçin](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="bb8fa-182">İlk bölümde oluşturduğunuz veritabanı bağlantısı seçin, girin **ProductContext** tıklayın ve bağlantı dizesi adı olarak **İleri**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Bağlantınızı seçin](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="bb8fa-184">'Tüm tabloları Al ve 'Son' tablolar' yanındaki onay kutusuna tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Nesnelerinizi seçin](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="bb8fa-186">Ters mühendislik işlemi tamamlandıktan sonra yeni modeli projenize eklenir ve Entity Framework Tasarımcısı'nda görüntülemeniz için açıldı.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="bb8fa-187">Bir App.config dosyası ayrıca veritabanı bağlantı ayrıntıları ile projenize eklendi.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="bb8fa-188">Visual Studio 2010'daki ek adımlar</span><span class="sxs-lookup"><span data-stu-id="bb8fa-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="bb8fa-189">Ardından Visual Studio 2010'da çalışıyorsanız EF designer EF6 kod oluşturmayı kullan güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="bb8fa-190">Modelinizin EF Tasarımcısı'nda boş bir nokta üzerinde sağ tıklayıp **kod oluşturma öğesi Ekle...**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="bb8fa-191">Seçin **çevrimiçi şablonlar** arayın ve sol menüden **DbContext**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="bb8fa-192">Seçin **EF 6.x DbContext Oluşturucu c\#,** girin **ProductsModel** adıyla ve Ekle'ye tıklayın</span><span class="sxs-lookup"><span data-stu-id="bb8fa-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="bb8fa-193">Kod oluşturma için veri bağlamayı güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="bb8fa-193">Updating code generation for data binding</span></span>

<span data-ttu-id="bb8fa-194">EF modelinizden T4 şablonlarını kullanarak kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="bb8fa-195">Şablonları Visual Studio ile birlikte gelen veya Visual Studio Galerisi'nden indirilir, genel amaçlı kullanım için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="bb8fa-196">Bu bu şablonlardan oluşturulan varlıkları basit ICollection olduğu anlamına gelir&lt;T&gt; özellikleri.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="bb8fa-197">Ancak, veri yaparken binding WPF kullanarak bunu kullanmak için tercih edilir **ObservableCollection** için koleksiyon özelliklerini, WPF koleksiyonlarına yapılan değişiklikleri izlemek için.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="bb8fa-198">Bu amaçla ObservableCollection kullanmak üzere değiştirmek için yapacağız.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="bb8fa-199">Açık **Çözüm Gezgini** ve bulma **ProductModel.edmx** dosyası</span><span class="sxs-lookup"><span data-stu-id="bb8fa-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="bb8fa-200">Bulma **ProductModel.tt** ProductModel.edmx dosyayı içe dosyası</span><span class="sxs-lookup"><span data-stu-id="bb8fa-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![WPF ürün modeli şablonu](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="bb8fa-202">Visual Studio Düzenleyicisi'nde açmak için ProductModel.tt dosyaya çift tıklayın</span><span class="sxs-lookup"><span data-stu-id="bb8fa-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="bb8fa-203">Bul ve Değiştir iki örneği "**ICollection**"ile"**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="bb8fa-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="bb8fa-204">Bu yaklaşık satırlar 296 ve 484 yer alır.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="bb8fa-205">Bul ve Değiştir ilk geçtiği "**HashSet**"ile"**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="bb8fa-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="bb8fa-206">Bu durum, yaklaşık 50 satırında bulunur.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="bb8fa-207">**Sağlamadığı** HashSet kod içinde bulunan ikinci oluşum değiştirin.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="bb8fa-208">Bul ve Değiştir yalnızca oluşumunu "**System.Collections.Generic**"ile"**System.Collections.ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="bb8fa-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="bb8fa-209">Bu işlem yaklaşık satırında 424 de bulunur.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="bb8fa-210">ProductModel.tt dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="bb8fa-211">Bu, kodu yeniden oluşturulması varlıklar için neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="bb8fa-212">Kodu otomatik olarak yeniden oluşturmaz, ProductModel.tt üzerinde sağ tıklayın ve "Özel aracı çalıştır" ı seçin.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="bb8fa-213">ProductModel.tt altında iç içe) Category.cs dosyasını (artık açın ardından ürünleri koleksiyon türlerinin olduğunu görmeniz gerekir **ObservableCollection&lt;ürün&gt;**.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="bb8fa-214">Projeyi derle.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="bb8fa-215">Yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="bb8fa-215">Lazy Loading</span></span>

<span data-ttu-id="bb8fa-216">**Ürünleri** özelliği **kategori** sınıfı ve **kategori** özelliği **ürün** Gezinti özellikleri sınıfı bulunur.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="bb8fa-217">Varlık Çerçevesi'nde, gezinti özellikleri iki varlık türleri arasındaki bir ilişki gitmek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="bb8fa-218">EF ilgili varlıkları veritabanından otomatik olarak, gezinti özelliği ilk kez yüklenirken bir seçeneği sunar.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="bb8fa-219">Bu (Gecikmeli yükleme olarak adlandırılır) yükleme türüyle içeriği değil zaten bir bağlamda ise, her gezinti özelliği ilk kez ayrı bir sorgu veritabanına karşı yürütülecek olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="bb8fa-220">EF yavaş yükleniyor POCO varlık türleri kullanırken, çalışma zamanı sırasında proxy türetilen türlerin örneklerini oluşturarak ve sonra da, sınıflarda yükleme kanca eklemek için sanal özelliklerini geçersiz kılma ulaşır.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="bb8fa-221">İlgili nesnelerin yavaş yükleniyor almak için özellik alıcıları olarak Gezinti bildirmelidir **genel** ve **sanal** (**Overridable** Visual Basic'te), ve, sınıfı olmalıdır **korumalı** (**NotOverridable** Visual Basic'te).</span><span class="sxs-lookup"><span data-stu-id="bb8fa-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="bb8fa-222">Kullanarak veritabanı ilk Gezinti özellikleri otomatik olarak yavaş yükleniyor etkinleştirmek için sanal hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="bb8fa-223">Gezinti özellikleri aynı nedenden dolayı sanal yapmak seçtik kod Birinci bölümde</span><span class="sxs-lookup"><span data-stu-id="bb8fa-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="bb8fa-224">Denetimlerine nesne bağlama</span><span class="sxs-lookup"><span data-stu-id="bb8fa-224">Bind Object to Controls</span></span>

<span data-ttu-id="bb8fa-225">Modeldeki veri kaynağı için bu WPF uygulaması olarak tanımlanan sınıflar ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="bb8fa-226">Çift **MainWindow.xaml** ana formu açmak için Çözüm Gezgini'nde</span><span class="sxs-lookup"><span data-stu-id="bb8fa-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="bb8fa-227">Ana menüden **proje -&gt; yeni veri kaynağı Ekle...**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="bb8fa-228">(Visual Studio 2010'da seçmeniz gerekir. **veri -&gt; yeni veri kaynağı Ekle...** )</span><span class="sxs-lookup"><span data-stu-id="bb8fa-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="bb8fa-229">Bir veri kaynağı Typewindow Seç seçin **nesne** tıklatıp **İleri**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="bb8fa-230">Veri nesneleri iletişim seçin unfold **WPFwithEFSample** seçin ve iki kez **kategorisi**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="bb8fa-231">*Seçilecek gerek yoktur **ürün** veri kaynağı için üzerinden alır çünkü **ürün**'s özelliği **kategori** veri kaynağı*</span><span class="sxs-lookup"><span data-stu-id="bb8fa-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Veri nesnelerini seçin](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="bb8fa-233">Tıklayın **son.**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-233">Click **Finish.**</span></span>
-   <span data-ttu-id="bb8fa-234">Veri kaynakları penceresi yanındaki MainWindow.xaml penceresi açıldığında *veri kaynakları penceresinden görüntülenmiyor, seçin **görünümü -&gt; diğer Windows -&gt; veri kaynakları***</span><span class="sxs-lookup"><span data-stu-id="bb8fa-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="bb8fa-235">Raptiye simgesini, böylece veri kaynakları penceresi otomatik gizleme tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="bb8fa-236">Pencere zaten görülebiliyorsa yenile düğmesine tıklama gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Data Sources](~/ef6/media/datasources.png)

-   <span data-ttu-id="bb8fa-238">Seçin \*\* kategori \*\* veri kaynağı ve formda sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-238">Select the \*\*Category \*\*data source and drag it on the form.</span></span>

<span data-ttu-id="bb8fa-239">Biz bu kaynağı sürüklendiğinde şu oldu:</span><span class="sxs-lookup"><span data-stu-id="bb8fa-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="bb8fa-240">**CategoryViewSource** kaynak ve \*\* categoryDataGrid \*\* denetimi için XAML eklendi.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-240">The **categoryViewSource** resource and the\*\* categoryDataGrid\*\* control were added to XAML.</span></span> <span data-ttu-id="bb8fa-241">DataViewSources hakkında daha fazla bilgi için bkz: http://bea.stollnitz.com/blog/?p=387.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-241">For more information about DataViewSources, see http://bea.stollnitz.com/blog/?p=387.</span></span>
-   <span data-ttu-id="bb8fa-242">Üst kılavuz öğesi DataContext özelliği ayarlandı "{StaticResource **categoryViewSource** }".</span><span class="sxs-lookup"><span data-stu-id="bb8fa-242">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span>  <span data-ttu-id="bb8fa-243">**CategoryViewSource** bağlama kaynağı için dış kaynak görür\\üst kılavuz öğesi.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-243">The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="bb8fa-244">İç kılavuz öğesi DataContext değeri ("{bağlama}" categoryDataGrid'ın ItemsSource özelliği ayarlı) Grid üst öğesinden sonra devralır.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-244">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}").</span></span> 

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a><span data-ttu-id="bb8fa-245">Ayrıntılar Kılavuz ekleme</span><span class="sxs-lookup"><span data-stu-id="bb8fa-245">Adding a Details Grid</span></span>

<span data-ttu-id="bb8fa-246">Biz kategorilerini şimdi görüntülemek için bir kılavuz olduğuna göre ilişkili ürünleri görüntülemek için ayrıntıları kılavuz ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-246">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="bb8fa-247">Seçin \*\* ürünleri \*\* özelliği altındaki \*\* kategori \*\* veri kaynağı ve formda sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-247">Select the \*\*Products \*\*property from under the \*\*Category \*\*data source and drag it on the form.</span></span>
    -   <span data-ttu-id="bb8fa-248">**CategoryProductsViewSource** kaynak ve **productDataGrid** kılavuz için XAML eklendi</span><span class="sxs-lookup"><span data-stu-id="bb8fa-248">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="bb8fa-249">Bu kaynak için bağlama yolunu ürünler için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-249">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="bb8fa-250">WPF veri bağlama çerçevesi sağlar seçilen kategoriye ilgili yalnızca ürünleri gösterilmediğini **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="bb8fa-250">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="bb8fa-251">Araç kutusundan sürükleyin **düğmesi** formu açın.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-251">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="bb8fa-252">Ayarlama **adı** özelliğini **buttonSave** ve **içerik** özelliğini **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-252">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="bb8fa-253">Form şuna benzer görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="bb8fa-253">The form should look similar to this:</span></span>

![Tasarımcı](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="bb8fa-255">Veri etkileşimini işleyen kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="bb8fa-255">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="bb8fa-256">Bu, ana pencereyi bazı olay işleyicileri ekleme zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-256">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="bb8fa-257">XAML penceresinde tıklayarak  **&lt;penceresi** öğesi, ana pencereyi seçer</span><span class="sxs-lookup"><span data-stu-id="bb8fa-257">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="bb8fa-258">İçinde **özellikleri** penceresi seçin **olayları** sağ üst kısımdaki ardından metin kutusunun sağ tarafındaki çift **Loaded** etiketi</span><span class="sxs-lookup"><span data-stu-id="bb8fa-258">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Ana pencere özellikleri](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="bb8fa-260">Ayrıca ekleyin **tıklayın** için olay **Kaydet** Tasarımcısı'nda Kaydet düğmesine çift tıklayarak düğmesi.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-260">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="bb8fa-261">Bu, arka plan kod için form getirir, biz artık veri erişimi gerçekleştirdiği ProductContext kullanmak için kodu düzenleme.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-261">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="bb8fa-262">Kodu MainWindow için aşağıda gösterildiği gibi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-262">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="bb8fa-263">Kod bir uzun süre çalışan örneğini bildirir **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-263">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="bb8fa-264">**ProductContext** nesnesi, sorgu ve veri veritabanına kaydetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-264">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="bb8fa-265">**Dispose**() üzerinde **ProductContext** örneği adlı sonra geçersiz kılınan'den **OnClosing** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-265">The **Dispose**() on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span> <span data-ttu-id="bb8fa-266">Kod açıklamaları ne yaptığını hakkında ayrıntılar sağlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-266">The code comments provide details about what the code does.</span></span>

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a><span data-ttu-id="bb8fa-267">WPF uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="bb8fa-267">Test the WPF Application</span></span>

-   <span data-ttu-id="bb8fa-268">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-268">Compile and run the application.</span></span> <span data-ttu-id="bb8fa-269">Code First kullanılan sonra göreceksiniz bir **WPFwithEFSample.ProductContext** veritabanı sizin için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-269">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="bb8fa-270">Alt kılavuzunda üst kılavuz ve ürün adlarını bir kategori adı girin *birincil anahtarı veritabanı tarafından oluşturulmuş olduğu için hiçbir şey kimliği sütunlarında girmeyin*</span><span class="sxs-lookup"><span data-stu-id="bb8fa-270">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Yeni kategorilerini ve ürünler ile ana penceresi](~/ef6/media/screen1.png)

-   <span data-ttu-id="bb8fa-272">Tuşuna **Kaydet** verileri veritabanına kaydetmek için düğme</span><span class="sxs-lookup"><span data-stu-id="bb8fa-272">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="bb8fa-273">DbContext'ın çağrısından sonra **SaveChanges**(), kimlikler, oluşturulan veritabanı değerleri ile doldurulur.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-273">After the call to DbContext’s **SaveChanges**(), the IDs are populated with the database generated values.</span></span> <span data-ttu-id="bb8fa-274">Biz denir çünkü **Yenile**sonra () **SaveChanges**() **DataGrid** denetimleri, yeni değerleri ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="bb8fa-274">Because we called **Refresh**() after **SaveChanges**() the **DataGrid** controls are updated with the new values as well.</span></span>

![Ana pencere doldurulmuş kimlikleri](~/ef6/media/screen2.png)
