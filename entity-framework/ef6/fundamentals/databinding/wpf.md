---
title: WPF ile veri bağlama-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 1933988277d3be8fecc02fced3293f2b7f80c901
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416598"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="fb1f6-102">WPF ile veri bağlama</span><span class="sxs-lookup"><span data-stu-id="fb1f6-102">Databinding with WPF</span></span>
<span data-ttu-id="fb1f6-103">Bu adım adım kılavuzda, POCO türlerinin bir "ana-ayrıntı" formunda WPF denetimlerine nasıl bağlanacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="fb1f6-104">Uygulama, nesneleri veritabanındaki verilerle doldurmak, değişiklikleri izlemek ve verileri veritabanına kalıcı hale getirmek için Entity Framework API 'Lerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="fb1f6-105">Model bire çok ilişkiye katılan iki tür tanımlar: **Kategori** (asıl\\ana) ve **ürün** (bağımlı\\ayrıntısı).</span><span class="sxs-lookup"><span data-stu-id="fb1f6-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="fb1f6-106">Daha sonra, Visual Studio Araçları modelde tanımlanan türleri WPF denetimlerine bağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="fb1f6-107">WPF veri bağlama çerçevesi ilgili nesneler arasında gezinmeyi sağlar: ana görünümdeki satırları seçme, ayrıntı görünümünün karşılık gelen alt verilerle güncelleştirilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="fb1f6-108">Bu izlenecek yolda ekran görüntüleri ve kod listeleri Visual Studio 2013 alınmıştır ancak Visual Studio 2012 veya Visual Studio 2010 ile bu yönergeyi tamamlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="fb1f6-109">WPF veri kaynakları oluşturmak için ' nesne ' seçeneğini kullanın</span><span class="sxs-lookup"><span data-stu-id="fb1f6-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="fb1f6-110">Entity Framework önceki sürümüyle, EF Designer ile oluşturulmuş bir modele dayalı yeni bir veri kaynağı oluştururken **veritabanı** seçeneğini kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="fb1f6-111">Bunun nedeni, tasarımcının ObjectContext 'ten türetilen ve EntityObject öğesinden türetilen varlık sınıflarından bir bağlam üretmesidir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="fb1f6-112">Veritabanı seçeneğini kullanmak, bu API yüzeyine etkileşimde bulunmak için en iyi kodu yazmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="fb1f6-113">Visual Studio 2012 ve Visual Studio 2013 için EF tasarımcıları, basit POCO varlık sınıflarıyla birlikte DbContext 'ten türeten bir bağlam oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="fb1f6-114">Visual Studio 2010 ile, bu kılavuzda daha sonra açıklandığı gibi DbContext kullanan bir kod oluşturma şablonuna değiştirmeyi öneririz.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="fb1f6-115">DbContext API yüzeyini kullanırken, bu kılavuzda gösterildiği gibi yeni bir veri kaynağı oluştururken **nesne** seçeneğini kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="fb1f6-116">Gerekirse, EF Designer ile oluşturulan modeller için [ObjectContext tabanlı kod oluşturmaya döndürebilirsiniz](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) .</span><span class="sxs-lookup"><span data-stu-id="fb1f6-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="fb1f6-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="fb1f6-117">Pre-Requisites</span></span>

<span data-ttu-id="fb1f6-118">Bu izlenecek yolu tamamlamak için Visual Studio 2013, Visual Studio 2012 veya Visual Studio 2010 yüklü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="fb1f6-119">Visual Studio 2010 kullanıyorsanız, NuGet ' i de yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="fb1f6-120">Daha fazla bilgi için bkz. [NuGet 'ı yükleme](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span><span class="sxs-lookup"><span data-stu-id="fb1f6-120">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="fb1f6-121">Uygulamayı oluşturma</span><span class="sxs-lookup"><span data-stu-id="fb1f6-121">Create the Application</span></span>

-   <span data-ttu-id="fb1f6-122">Visual Studio’yu açın</span><span class="sxs-lookup"><span data-stu-id="fb1f6-122">Open Visual Studio</span></span>
-   <span data-ttu-id="fb1f6-123">**Dosya-&gt; yeni&gt; projesi....**</span><span class="sxs-lookup"><span data-stu-id="fb1f6-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="fb1f6-124">Sol bölmedeki **Windows** ve sağ bölmedeki **WPFApplication** seçin</span><span class="sxs-lookup"><span data-stu-id="fb1f6-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="fb1f6-125">Ad olarak **WPFwithEFSample** girin</span><span class="sxs-lookup"><span data-stu-id="fb1f6-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="fb1f6-126"> **Tamam 'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="fb1f6-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="fb1f6-127">Entity Framework NuGet paketini yükler</span><span class="sxs-lookup"><span data-stu-id="fb1f6-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="fb1f6-128">Çözüm Gezgini, **Winformyüzthefsample** projesine sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="fb1f6-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="fb1f6-129">**NuGet Paketlerini Yönet...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="fb1f6-130">NuGet Paketlerini Yönet iletişim kutusunda **çevrimiçi** sekmesini seçin ve **EntityFramework** paketini seçin</span><span class="sxs-lookup"><span data-stu-id="fb1f6-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="fb1f6-131">**Install** 'a tıklayın</span><span class="sxs-lookup"><span data-stu-id="fb1f6-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="fb1f6-132">EntityFramework derlemesine ek olarak, System. ComponentModel. Dataaçıklamalarda bir başvuru de eklenir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="fb1f6-133">Projenin System. Data. Entity başvurusu varsa, EntityFramework paketi yüklendiğinde kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="fb1f6-134">System. Data. Entity derlemesi artık Entity Framework 6 uygulamaları için kullanılmıyor.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="fb1f6-135">Model tanımlama</span><span class="sxs-lookup"><span data-stu-id="fb1f6-135">Define a Model</span></span>

<span data-ttu-id="fb1f6-136">Bu izlenecek yolda, Code First veya EF tasarımcısını kullanarak bir model uygulamayı seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="fb1f6-137">Aşağıdaki iki bölümden birini doldurun.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="fb1f6-138">Seçenek 1: Code First kullanarak model tanımlama</span><span class="sxs-lookup"><span data-stu-id="fb1f6-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="fb1f6-139">Bu bölümde, Code First kullanarak bir modelin ve ilişkili veritabanının nasıl oluşturulacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="fb1f6-140">Bir sonraki bölüme atlayın (**seçenek 2: Database First kullanarak bir model tanımlayın)** , bir VERITABANıNDAN, EF Designer kullanarak modelinize ters mühendislik uygulamak için Database First kullanmayı tercih ediyorsanız</span><span class="sxs-lookup"><span data-stu-id="fb1f6-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="fb1f6-141">Code First geliştirme kullanılırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan .NET Framework sınıfları yazarak başlarsınız.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="fb1f6-142">WPFwithEFSample için yeni bir sınıf ekleyin **:**</span><span class="sxs-lookup"><span data-stu-id="fb1f6-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="fb1f6-143">Proje adına sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="fb1f6-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="fb1f6-144">**Ekle**' yi ve ardından **Yeni öğe** ' yi seçin</span><span class="sxs-lookup"><span data-stu-id="fb1f6-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="fb1f6-145">Sınıf adı için **sınıf** ' ı seçin ve **ürün** girin</span><span class="sxs-lookup"><span data-stu-id="fb1f6-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="fb1f6-146"> **Ürün** sınıfı tanımını aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="fb1f6-146">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="fb1f6-147">**Product** sınıfında **category** sınıfı ve **category** özelliğindeki **Products** özelliği gezinti özellikleridir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="fb1f6-148">Entity Framework, gezinti özellikleri iki varlık türü arasında bir ilişkiye gitmek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="fb1f6-149">Varlıkları tanımlamaya ek olarak, DbContext 'ten türeten bir sınıf tanımlamanız ve DbSet&lt;TEntity&gt; özelliklerini kullanıma sunabilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="fb1f6-150">DbSet&lt;TEntity&gt; özellikleri, bağlamın modele hangi türleri dahil etmek istediğinizi bilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="fb1f6-151">DbContext türetilmiş türünün bir örneği, çalışma zamanı sırasında varlık nesnelerini yönetir, bu da nesneleri bir veritabanındaki verilerle doldurmayı, değişiklik izlemeyi ve kalıcı verileri veritabanına getirmeyi içerir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="fb1f6-152">Aşağıdaki tanıma sahip projeye yeni bir **Productcontext** sınıfı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="fb1f6-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="fb1f6-153">Projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="fb1f6-154">Seçenek 2: Database First kullanarak model tanımlama</span><span class="sxs-lookup"><span data-stu-id="fb1f6-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="fb1f6-155">Bu bölümde, EF Designer kullanarak bir veritabanından modelinize ters mühendislik uygulamak için Database First nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="fb1f6-156">Önceki bölümü (**1. seçenek: Code First kullanarak model tanımlama)** tamamladıysanız, bu bölümü atlayın ve doğrudan **geç yükleme** bölümüne gidin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="fb1f6-157">Var olan bir veritabanı oluştur</span><span class="sxs-lookup"><span data-stu-id="fb1f6-157">Create an Existing Database</span></span>

<span data-ttu-id="fb1f6-158">Genellikle, var olan bir veritabanını hedeflerken zaten oluşturulur, ancak bu izlenecek yol için, erişmek üzere bir veritabanı oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="fb1f6-159">Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösteren bir sürümdür:</span><span class="sxs-lookup"><span data-stu-id="fb1f6-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="fb1f6-160">Visual Studio 2010 kullanıyorsanız, bir SQL Express veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="fb1f6-161">Visual Studio 2012 kullanıyorsanız, [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="fb1f6-162">Şimdi veritabanını oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="fb1f6-163">**&gt; Sunucu Gezgini görüntüle**</span><span class="sxs-lookup"><span data-stu-id="fb1f6-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="fb1f6-164">Veri bağlantıları ' na sağ tıklayın **&gt; bağlantı ekle...**</span><span class="sxs-lookup"><span data-stu-id="fb1f6-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="fb1f6-165">Sunucu Gezgini bir veritabanına bağlı değilseniz, veri kaynağı olarak Microsoft SQL Server seçmeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="fb1f6-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Veri kaynağını Değiştir](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="fb1f6-167">Hangisini yüklediğinize bağlı olarak LocalDB 'ye veya SQL Express 'e bağlanın ve veritabanı adı olarak **ürünleri** girin</span><span class="sxs-lookup"><span data-stu-id="fb1f6-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Bağlantı LocalDB Ekle](~/ef6/media/addconnectionlocaldb.png)

    ![Bağlantı Express ekleme](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="fb1f6-170">**Tamam** ' ı seçtiğinizde, yeni bir veritabanı oluşturmak isteyip istemediğiniz sorulur, **Evet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Veritabanı Oluşturma](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="fb1f6-172">Yeni veritabanı artık Sunucu Gezgini görüntülenir, sağ tıklayıp **Yeni sorgu** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="fb1f6-173">Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **Yürüt** ' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="fb1f6-174">Tersine mühendislik modeli</span><span class="sxs-lookup"><span data-stu-id="fb1f6-174">Reverse Engineer Model</span></span>

<span data-ttu-id="fb1f6-175">Modelimizi oluşturmak için Visual Studio 'nun bir parçası olarak dahil edilen Entity Framework Designer kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="fb1f6-176">**Proje-&gt; yeni öğe Ekle...**</span><span class="sxs-lookup"><span data-stu-id="fb1f6-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="fb1f6-177">Sol menüden **verileri** seçin ve ardından **ADO.net varlık veri modeli**</span><span class="sxs-lookup"><span data-stu-id="fb1f6-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="fb1f6-178">Ad olarak **ProductModel** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="fb1f6-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="fb1f6-179">Bu, **varlık veri modeli Sihirbazı 'nı** başlatır</span><span class="sxs-lookup"><span data-stu-id="fb1f6-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="fb1f6-180">**Veritabanından oluştur** ' u seçin ve **İleri** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-180">Select **Generate from Database** and click **Next**</span></span>

    ![Model Içeriğini seçin](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="fb1f6-182">İlk bölümde oluşturduğunuz veritabanına bağlantıyı seçin, bağlantı dizesinin adı olarak **Productcontext** girin ve **İleri** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Bağlantınızı seçin](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="fb1f6-184">Tüm tabloları içeri aktarmak için ' tablolar ' seçeneğinin yanındaki onay kutusuna tıklayın ve ' son ' a tıklayın</span><span class="sxs-lookup"><span data-stu-id="fb1f6-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Nesnelerinizi seçin](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="fb1f6-186">Tersine mühendislik işlemi tamamlandıktan sonra, yeni model projenize eklenir ve Entity Framework Designer görüntülemeniz için açılır.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="fb1f6-187">Veritabanına yönelik bağlantı ayrıntılarına sahip bir App. config dosyası da projenize eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="fb1f6-188">Visual Studio 2010 ' de ek adımlar</span><span class="sxs-lookup"><span data-stu-id="fb1f6-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="fb1f6-189">Visual Studio 2010 ' de çalışıyorsanız, EF6 kod oluşturmayı kullanmak için EF tasarımcısını güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="fb1f6-190">EF Designer 'daki modelinizin boş bir noktasına sağ tıklayıp **kod oluşturma öğesi Ekle...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="fb1f6-191">Sol menüden **çevrimiçi şablonlar** ' ı seçin ve **DbContext** ' i arayın</span><span class="sxs-lookup"><span data-stu-id="fb1f6-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="fb1f6-192">**C\#Için EF 6. x DbContext oluşturucusunu seçin,** ad olarak **productsmodel** girin ve Ekle ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="fb1f6-193">Veri bağlama için kod üretimi güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="fb1f6-193">Updating code generation for data binding</span></span>

<span data-ttu-id="fb1f6-194">EF, modelden T4 şablonları kullanarak kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="fb1f6-195">Visual Studio ile birlikte gelen veya Visual Studio galerisinden indirilen şablonlar genel amaçlı kullanım amaçlıdır.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="fb1f6-196">Bu, bu şablonlardan oluşturulan varlıkların basit ICollection&lt;T&gt; özelliklerine sahip olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="fb1f6-197">Bununla birlikte, WPF kullanarak veri bağlama yaparken, WPF 'in koleksiyonlarda yapılan değişiklikleri izleyebilmesi için koleksiyon özellikleri için **ObservableCollection** kullanılması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="fb1f6-198">Bu uçta, ObservableCollection kullanmak için şablonları değiştiririz.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="fb1f6-199">**Çözüm Gezgini** açın ve **ProductModel. edmx** dosyasını bulun</span><span class="sxs-lookup"><span data-stu-id="fb1f6-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="fb1f6-200">ProductModel. edmx dosyasının altında iç içe olacak **ProductModel.tt** dosyasını bulun</span><span class="sxs-lookup"><span data-stu-id="fb1f6-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![WPF ürün modeli şablonu](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="fb1f6-202">Visual Studio Düzenleyicisi 'nde açmak için ProductModel.tt dosyasına çift tıklayın</span><span class="sxs-lookup"><span data-stu-id="fb1f6-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="fb1f6-203">"**ICollection**" sözcüğünün iki yinelemesini "**ObservableCollection**" ile bulur ve değiştirin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="fb1f6-204">Bunlar, 296 ve 484 satırlarında yaklaşık olarak bulunur.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="fb1f6-205">İlk "**HashSet**" oluşumunu "**ObservableCollection**" ile bulur ve değiştirin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="fb1f6-206">Bu oluşum yaklaşık olarak 50. satırda bulunur.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="fb1f6-207">Kodda daha sonra bulunan ikinci diyez kümesi **oluşumunu değiştirmeyin.**</span><span class="sxs-lookup"><span data-stu-id="fb1f6-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="fb1f6-208">Tek "**System. Collections. Generic**" oluşumunu "**System. Collections. ObjectModel**" ile bulur ve değiştirir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="fb1f6-209">Bu yaklaşık 424. satırda bulunur.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="fb1f6-210">ProductModel.tt dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="fb1f6-211">Bu, varlıkların kodunun yeniden üretilmesine neden olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="fb1f6-212">Kod otomatik olarak yeniden oluşturmaz, ProductModel.tt 'e sağ tıklayın ve "özel araç Çalıştır" seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="fb1f6-213">Artık Category.cs dosyasını (ProductModel.tt altında iç içe yerleştirilmiş) açarsanız, Products koleksiyonunun **ObservableCollection&lt;ürün&gt;** türünde olduğunu görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="fb1f6-214">Projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="fb1f6-215">Geç yükleme</span><span class="sxs-lookup"><span data-stu-id="fb1f6-215">Lazy Loading</span></span>

<span data-ttu-id="fb1f6-216">**Product** sınıfında **category** sınıfı ve **category** özelliğindeki **Products** özelliği gezinti özellikleridir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="fb1f6-217">Entity Framework, gezinti özellikleri iki varlık türü arasında bir ilişkiye gitmek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="fb1f6-218">EF, gezinti özelliğine ilk kez eriştiğinizde ilgili varlıkları veritabanından otomatik olarak yükleme seçeneği sunar.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="fb1f6-219">Bu yükleme türü (yavaş yükleme olarak adlandırılır) ile, her gezinti özelliğine ilk kez eriştiğinizde, içerik bağlamda değilse veritabanına karşı ayrı bir sorgu yürütülecektir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="fb1f6-220">POCO varlık türleri kullanılırken, EF, çalışma zamanı sırasında türetilmiş proxy türlerinin örneklerini oluşturarak ve sonra yükleme kancasını eklemek için sınıflarınızda sanal özellikleri geçersiz kılarak geç yüklemeye erişir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="fb1f6-221">İlgili nesnelerin geç yüklemesini almak için, gezinti özelliği alıcıları ' ı **Public** ve **Virtual** (Visual Basic) olarak bildirmeniz ve sınıfınız **mühürlenmemelidir** **(Visual Basic** içinde**NotOverridable** ).</span><span class="sxs-lookup"><span data-stu-id="fb1f6-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="fb1f6-222">Database First gezinti özelliklerinin kullanılması, yavaş yüklemeyi etkinleştirmek için otomatik olarak sanal hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="fb1f6-223">Code First bölümünde, aynı nedenden dolayı gezinti özelliklerini sanal hale getirmeyi seçtik</span><span class="sxs-lookup"><span data-stu-id="fb1f6-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="fb1f6-224">Nesneyi denetimlere bağlama</span><span class="sxs-lookup"><span data-stu-id="fb1f6-224">Bind Object to Controls</span></span>

<span data-ttu-id="fb1f6-225">Modelde tanımlanan sınıfları bu WPF uygulaması için veri kaynağı olarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="fb1f6-226">Ana formu açmak için Çözüm Gezgini **MainWindow. xaml** öğesine çift tıklayın</span><span class="sxs-lookup"><span data-stu-id="fb1f6-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="fb1f6-227">Ana menüden **Proje-&gt; yeni veri kaynağı Ekle...** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="fb1f6-228">(Visual Studio 2010 ' de, **veri&gt; yeni veri kaynağı Ekle...** ' i seçmeniz gerekir.)</span><span class="sxs-lookup"><span data-stu-id="fb1f6-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="fb1f6-229">Bir veri kaynağı türü seçin penceresinde, **nesne** ' yi seçin ve **İleri** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="fb1f6-230">Veri nesnelerini seçin iletişim kutusunda, **WPFwithEFSample** iki kez katlamayı kaldırın ve **Kategori** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="fb1f6-231">***Ürün veri kaynağını** seçme gereksinimi yoktur, çünkü bu Işlem, **Kategori** veri kaynağındaki **ürünün**özelliği aracılığıyla sunulacaktır*</span><span class="sxs-lookup"><span data-stu-id="fb1f6-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Veri nesnelerini seçin](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="fb1f6-233">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-233">Click **Finish.**</span></span>
-   <span data-ttu-id="fb1f6-234">Veri kaynakları penceresi açık değilse, MainWindow. xaml penceresinin yanında veri kaynakları penceresi açılır.  ***&gt; diğer Windows-&gt; veri kaynaklarını görüntüle** ' yi seçin*</span><span class="sxs-lookup"><span data-stu-id="fb1f6-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="fb1f6-235">Veri kaynakları penceresi otomatik olarak gizlenmemesi için Sabitle simgesine basın.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="fb1f6-236">Pencere zaten görünür durumdaysa Yenile düğmesine vurması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Veri Kaynakları](~/ef6/media/datasources.png)

-   <span data-ttu-id="fb1f6-238"> **Kategori** veri kaynağını seçin ve form üzerine sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-238">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="fb1f6-239">Bu kaynağı sürüklediğiniz sırada şunlar oldu:</span><span class="sxs-lookup"><span data-stu-id="fb1f6-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="fb1f6-240">**Categoryviewsource** kaynağı ve **CATEGORYDATAGRID** denetimi xaml 'ye eklendi</span><span class="sxs-lookup"><span data-stu-id="fb1f6-240">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="fb1f6-241">Üst kılavuz öğesindeki DataContext özelliği "{StaticResource **Categoryviewsource** }" olarak ayarlandı.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-241">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="fb1f6-242"> **Categoryviewsource** kaynağı, dış\\üst kılavuz öğesi için bir bağlama kaynağı işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-242"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="fb1f6-243">İç kılavuz öğeleri daha sonra üst ızgaraya DataContext değerini devralınır (categoryDataGrid 'in ItemId özelliği "{Binding}" olarak ayarlanır)</span><span class="sxs-lookup"><span data-stu-id="fb1f6-243">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="fb1f6-244">Ayrıntı Kılavuzu ekleme</span><span class="sxs-lookup"><span data-stu-id="fb1f6-244">Adding a Details Grid</span></span>

<span data-ttu-id="fb1f6-245">Şimdi Kategoriler görüntüleyen bir kılavuza sahip olduğumuz ilgili ürünleri göstermek için bir ayrıntı Kılavuzu ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-245">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="fb1f6-246"> **Kategori** veri kaynağı altında **Products** özelliğini seçin ve forma sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-246">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="fb1f6-247">**Categoryproductsviewsource** kaynağı ve **PRODUCTDATAGRID** Kılavuzu xaml 'ye eklenir</span><span class="sxs-lookup"><span data-stu-id="fb1f6-247">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="fb1f6-248">Bu kaynak için bağlama yolu ürünler olarak ayarlandı</span><span class="sxs-lookup"><span data-stu-id="fb1f6-248">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="fb1f6-249">WPF veri bağlama çerçevesi yalnızca seçili kategoriyle ilgili ürünlerin **Productdatagrid** 'de görünmesini sağlar</span><span class="sxs-lookup"><span data-stu-id="fb1f6-249">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="fb1f6-250">Araç kutusu ' ndan **düğme** ' ye sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-250">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="fb1f6-251">**Name** özelliğini **Buttonsave** , **içerik** özelliği ise **Save**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-251">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="fb1f6-252">Form şuna benzer görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="fb1f6-252">The form should look similar to this:</span></span>

![Tasarımcı](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="fb1f6-254">Veri etkileşimini Işleyen kod ekleme</span><span class="sxs-lookup"><span data-stu-id="fb1f6-254">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="fb1f6-255">Ana pencereye bazı olay işleyicileri ekleme zamanı.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-255">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="fb1f6-256">XAML penceresinde **&lt;pencere** öğesine tıklayın, bu, ana pencereyi seçer</span><span class="sxs-lookup"><span data-stu-id="fb1f6-256">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="fb1f6-257">**Özellikler** penceresinde, sağ üst köşedeki **Olaylar** ' ı seçin ve ardından **yüklenen** etiketin sağ tarafındaki metin kutusunu çift tıklatın</span><span class="sxs-lookup"><span data-stu-id="fb1f6-257">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Ana pencere özellikleri](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="fb1f6-259">Ayrıca, tasarımcıda Kaydet düğmesine çift tıklayarak **Kaydet** düğmesine ait **Click** olayını da ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-259">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="fb1f6-260">Bu, sizi form için arka plan koduna getirir, artık kodu, veri erişimi gerçekleştirmek için ProductContext 'i kullanacak şekilde düzenliyoruz.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-260">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="fb1f6-261">MainWindow için kodu aşağıda gösterildiği gibi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-261">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="fb1f6-262">Kod, **Productcontext**'in uzun süre çalışan bir örneğini bildirir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-262">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="fb1f6-263">Verileri sorgulamak ve veritabanına kaydetmek için **Productcontext** nesnesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-263">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="fb1f6-264">**Productcontext** örneğindeki **Dispose ()** , geçersiz kılınan **OnClosing** yönteminden çağırılır.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-264">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="fb1f6-265"> Kod açıklamaları kodun ne yaptığını hakkında ayrıntılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-265"> The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="fb1f6-266">WPF uygulamasını test etme</span><span class="sxs-lookup"><span data-stu-id="fb1f6-266">Test the WPF Application</span></span>

-   <span data-ttu-id="fb1f6-267">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-267">Compile and run the application.</span></span> <span data-ttu-id="fb1f6-268">Code First kullandıysanız, sizin için bir **WPFwithEFSample. ProductContext** veritabanının oluşturulduğunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-268">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="fb1f6-269">Üst kılavuza bir kategori adı girin ve *birincil anahtar veritabanı tarafından oluşturulduğundan alt kılavuzdaki ürün adları ID sütunlarında hiçbir şey girmeyin*</span><span class="sxs-lookup"><span data-stu-id="fb1f6-269">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Yeni kategoriler ve ürünlerle ana pencere](~/ef6/media/screen1.png)

-   <span data-ttu-id="fb1f6-271">Verileri veritabanına kaydetmek için **Kaydet** düğmesine basın</span><span class="sxs-lookup"><span data-stu-id="fb1f6-271">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="fb1f6-272">DbContext 'in **SaveChanges ()** çağrısından sonra, kimlikler veritabanı tarafından oluşturulan değerlerle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-272">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="fb1f6-273">**SaveChanges** () sonrasında **Refresh (** ) çağırdığımız için **DataGrid** denetimleri yeni değerlerle de güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-273">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Doldurulan kimlikleri içeren ana pencere](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="fb1f6-275">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fb1f6-275">Additional Resources</span></span>

<span data-ttu-id="fb1f6-276">WPF kullanarak koleksiyonlara veri bağlama hakkında daha fazla bilgi edinmek için WPF belgelerindeki [Bu konuya](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) bakın.</span><span class="sxs-lookup"><span data-stu-id="fb1f6-276">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  
