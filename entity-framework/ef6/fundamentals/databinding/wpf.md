---
title: WPF ile Veri Bağlama - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639139"
---
> [!IMPORTANT]
> <span data-ttu-id="2e8ba-102">**Bu belge sadece .NET Framework'de WPF için geçerlidir**</span><span class="sxs-lookup"><span data-stu-id="2e8ba-102">**This document is valid for WPF on the .NET Framework only**</span></span>
>
> <span data-ttu-id="2e8ba-103">Bu belge, .NET Framework üzerinde WPF için veri bağlama açıklar.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-103">This document describes databinding for WPF on the .NET Framework.</span></span> <span data-ttu-id="2e8ba-104">Yeni .NET Core projeleri için Varlık Çerçevesi 6 yerine [EF Core'u](/ef/core) kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-104">For new .NET Core projects, we recommend you use [EF Core](/ef/core) instead of Entity Framework 6.</span></span> <span data-ttu-id="2e8ba-105">EF Core'da veri bağlama için gerekli belgeler [Sayı #778'da](https://github.com/dotnet/EntityFramework.Docs/issues/778)izlenir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-105">The documentation for databinding in EF Core is tracked in [Issue #778](https://github.com/dotnet/EntityFramework.Docs/issues/778).</span></span>

# <a name="databinding-with-wpf"></a><span data-ttu-id="2e8ba-106">WPF ile veri bağlama</span><span class="sxs-lookup"><span data-stu-id="2e8ba-106">Databinding with WPF</span></span>
<span data-ttu-id="2e8ba-107">Bu adım adım gözden geçirme, POCO türlerinin WPF denetimlerine nasıl "ana ayrıntı" biçiminde bağlanılabildiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-107">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="2e8ba-108">Uygulama, nesneleri veritabanındaki verilerle doldurmak, değişiklikleri izlemek ve verileri veritabanında kalıcı olarak sürdürmek için Varlık Çerçeve API'lerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-108">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="2e8ba-109">Model, bir-çok ilişkisine katılan iki tür **Category** tanımlar:\\Kategori (asıl asıl) ve **Ürün** (bağımlı\\ayrıntı).</span><span class="sxs-lookup"><span data-stu-id="2e8ba-109">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="2e8ba-110">Daha sonra, Visual Studio araçları wpf denetimleri için modelde tanımlanan türleri bağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-110">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="2e8ba-111">WPF veri bağlama çerçevesi ilgili nesneler arasında gezinmeyi sağlar: ana görünümdeki satırları seçmek ayrıntı görünümünün ilgili alt verilerle güncelleştirilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-111">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="2e8ba-112">Bu iznin ekran görüntüleri ve kod listeleri Visual Studio 2013'ten alınmıştır ancak Visual Studio 2012 veya Visual Studio 2010 ile bu walkthrough'u tamamlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-112">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="2e8ba-113">WPF Veri Kaynakları Oluşturmak için 'Nesne' Seçeneğini Kullanma</span><span class="sxs-lookup"><span data-stu-id="2e8ba-113">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="2e8ba-114">Varlık Çerçevesi'nin önceki sürümünde, EF Tasarımcısı ile oluşturulan bir modele dayalı yeni bir Veri Kaynağı oluştururken **Veritabanı** seçeneğini kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-114">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="2e8ba-115">Bunun nedeni, tasarımcının ObjectContext'tan türetilen bir bağlam ve EntityObject'ten türetilen varlık sınıfları oluşturmasıdır.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-115">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="2e8ba-116">Veritabanı seçeneğini kullanmak, bu API yüzeyi ile etkileşim için en iyi kodu yazmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-116">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="2e8ba-117">Visual Studio 2012 ve Visual Studio 2013 için EF Designers basit POCO varlık sınıfları ile birlikte DbContext türetilen bir bağlam oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-117">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="2e8ba-118">Visual Studio 2010 ile, bu iznin ilerleyen saatlerinde açıklandığı gibi DbContext kullanan bir kod oluşturma şablonuna geçiş yapmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-118">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="2e8ba-119">DbContext API yüzeyini kullanırken, bu izbarada gösterildiği gibi yeni bir Veri Kaynağı oluştururken **Nesne** seçeneğini kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-119">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="2e8ba-120">Gerekirse, EF Designer ile oluşturulan modeller için [ObjectContext tabanlı kod oluşturma](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) geri dönebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-120">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="2e8ba-121">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="2e8ba-121">Pre-Requisites</span></span>

<span data-ttu-id="2e8ba-122">Bu walkthrough tamamlamak için Visual Studio 2013, Visual Studio 2012 veya Visual Studio 2010 yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-122">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="2e8ba-123">Visual Studio 2010 kullanıyorsanız, NuGet'i de yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-123">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="2e8ba-124">Daha fazla bilgi için [NuGet'i Yükleme'ye](https://docs.microsoft.com/nuget/install-nuget-client-tools)bakın.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-124">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="2e8ba-125">Uygulamayı Oluştur</span><span class="sxs-lookup"><span data-stu-id="2e8ba-125">Create the Application</span></span>

-   <span data-ttu-id="2e8ba-126">Visual Studio’yu açın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-126">Open Visual Studio</span></span>
-   <span data-ttu-id="2e8ba-127">**Dosya&gt; -&gt; Yeni - Proje....**</span><span class="sxs-lookup"><span data-stu-id="2e8ba-127">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="2e8ba-128">Sol bölmede **Windows'u** ve sağ bölmede **WPFApplication'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="2e8ba-128">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="2e8ba-129"> **WPFwithEFSample'ı** ad olarak girin</span><span class="sxs-lookup"><span data-stu-id="2e8ba-129">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="2e8ba-130"> **Tamam'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="2e8ba-130">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="2e8ba-131">Varlık Çerçevesi NuGet paketini yükleyin</span><span class="sxs-lookup"><span data-stu-id="2e8ba-131">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="2e8ba-132">Solution Explorer'da **WinFormswithEFSample** projesine sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-132">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="2e8ba-133">**NuGet Paketlerini Yönet'i seçin...**</span><span class="sxs-lookup"><span data-stu-id="2e8ba-133">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="2e8ba-134">NuGet Paketlerini Yönet iletişim **kutusunda, Çevrimiçi** sekmesini seçin ve **EntityFramework** paketini seçin</span><span class="sxs-lookup"><span data-stu-id="2e8ba-134">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="2e8ba-135">**Yükle'yi** tıklatın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-135">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="2e8ba-136">EntityFramework derlemesine ek olarak System.ComponentModel.DataAnnotations'a bir başvuru da eklenir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-136">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="2e8ba-137">Projenin System.Data.Entity'e bir başvurusu varsa, EntityFramework paketi yüklendiğinde kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-137">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="2e8ba-138">System.Data.Entity derlemesi artık Entity Framework 6 uygulamaları için kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-138">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="2e8ba-139">Model Tanımla</span><span class="sxs-lookup"><span data-stu-id="2e8ba-139">Define a Model</span></span>

<span data-ttu-id="2e8ba-140">Bu izne Code First veya EF Designer kullanarak bir model uygulamayı seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-140">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="2e8ba-141">Aşağıdaki iki bölümden birini tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-141">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="2e8ba-142">Seçenek 1: Önce Kodu Kullanarak Model Tanımlayın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-142">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="2e8ba-143">Bu bölümde, Önce Kod'u kullanarak bir modelin ve ilişkili veritabanının nasıl oluşturulutamamolduğu gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-143">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="2e8ba-144">MODELINIZI EF tasarımcısını kullanarak bir veritabanından tersine mühendislik yapmak için Önce Veritabanı'nı kullanarak bir model tanımlayın **(Seçenek 2: Önce Veritabanı'nı kullanarak bir model tanımlayın)**</span><span class="sxs-lookup"><span data-stu-id="2e8ba-144">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="2e8ba-145">Kod İlk geliştirme kullanırken genellikle kavramsal (etki alanı) modelinizi tanımlayan .NET Framework sınıfları yazarak başlarsınız.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-145">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="2e8ba-146"> **WPFwithEFSample'a** yeni bir sınıf ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2e8ba-146">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="2e8ba-147">Proje adına sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-147">Right-click on the project name</span></span>
    -   <span data-ttu-id="2e8ba-148">**Ekle'yi**seçin, ardından **Yeni Öğe**</span><span class="sxs-lookup"><span data-stu-id="2e8ba-148">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="2e8ba-149">**Sınıf'ı** seçin ve sınıf adı için **Ürün** girin</span><span class="sxs-lookup"><span data-stu-id="2e8ba-149">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="2e8ba-150"> **Ürün** sınıf tanımını aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="2e8ba-150">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="2e8ba-151">**Ürün** sınıfındaki **Kategori** sınıfı ve **Kategori** özelliğindeki **Ürünler** özelliği gezinti özellikleridir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-151">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="2e8ba-152">Varlık Çerçevesi'nde, gezinti özellikleri iki varlık türü arasındaki ilişkiyi gezinmenin bir yolunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-152">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="2e8ba-153">Varlıkları tanımlamaya ek olarak, DbContext'dan türetilen ve DbSet&lt;TEntity&gt; özelliklerini ortaya çıkaran bir sınıf tanımlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-153">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="2e8ba-154">DbSet&lt;TEntity&gt; özellikleri, içeriğe modele hangi türleri eklemek istediğinizi bildirin.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-154">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="2e8ba-155">DbContext türetilmiş türünün bir örneği, nesneleri veritabanındaki verilerle doldurmayı, izlemeyi değiştirmeyi ve verileri veritabanına kalıcı olarak dahil etmeyi içeren çalışma süresi sırasında varlık nesnelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-155">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="2e8ba-156">Projeye aşağıdaki tanımla yeni bir **ProductContext** sınıfı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2e8ba-156">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="2e8ba-157">Projeyi derle.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-157">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="2e8ba-158">Seçenek 2: Önce Veritabanı'nı kullanarak bir model tanımlayın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-158">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="2e8ba-159">Bu bölümde, MODELINIZI EF tasarımcısını kullanarak bir veritabanından tersine mühendislik yapmak için Veritabanı İlk'in nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-159">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="2e8ba-160">Önceki bölümü tamamladıysanız **(Seçenek 1: Önce Kod'u kullanarak bir model tanımlayın)** sonra bu bölümü atlayın ve doğrudan **Tembel Yükleme** bölümüne gidin.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-160">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="2e8ba-161">Varolan Veritabanı Oluşturma</span><span class="sxs-lookup"><span data-stu-id="2e8ba-161">Create an Existing Database</span></span>

<span data-ttu-id="2e8ba-162">Genellikle varolan bir veritabanını hedefliyorsanız zaten oluşturulur, ancak bu izlenecek yol için erişmek için bir veritabanı oluşturmamız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-162">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="2e8ba-163">Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklıdır:</span><span class="sxs-lookup"><span data-stu-id="2e8ba-163">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="2e8ba-164">Visual Studio 2010 kullanıyorsanız bir SQL Express veritabanı oluşturuyor olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-164">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="2e8ba-165">Visual Studio 2012 kullanıyorsanız, bir [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) veritabanı oluşturuyor olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-165">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="2e8ba-166">Devam edelim ve veritabanını oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-166">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="2e8ba-167">**Görünüm&gt; - Sunucu Gezgini**</span><span class="sxs-lookup"><span data-stu-id="2e8ba-167">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="2e8ba-168">**Veri Bağlantıları -&gt; Bağlantı Ekle'ye** sağ tıklayın...</span><span class="sxs-lookup"><span data-stu-id="2e8ba-168">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="2e8ba-169">Veri kaynağı olarak Microsoft SQL Server'ı seçmeniz gerekirken, sunucu gezgininden bir veritabanına bağlanmadıysanız</span><span class="sxs-lookup"><span data-stu-id="2e8ba-169">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Veri Kaynağını Değiştir](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="2e8ba-171">Hangisini yüklediğinize bağlı olarak LocalDB veya SQL Express'e bağlanın ve **Ürünleri** veritabanı adı olarak girin</span><span class="sxs-lookup"><span data-stu-id="2e8ba-171">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Bağlantı Ekle LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Bağlantı Ekspresi Ekle](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="2e8ba-174">**Tamam'ı** seçin ve yeni bir veritabanı oluşturmak isteyip istemediğiniz sorulur, **Evet'i** seçin</span><span class="sxs-lookup"><span data-stu-id="2e8ba-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Veritabanı Oluşturma](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="2e8ba-176">Yeni veritabanı artık Server Explorer'da görünecek, üzerine sağ tıklanacak ve **Yeni Sorgu'yu** seçecektir</span><span class="sxs-lookup"><span data-stu-id="2e8ba-176">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="2e8ba-177">Aşağıdaki SQL'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayın ve **Yürüt''''ün**</span><span class="sxs-lookup"><span data-stu-id="2e8ba-177">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="2e8ba-178">Ters Mühendis Modeli</span><span class="sxs-lookup"><span data-stu-id="2e8ba-178">Reverse Engineer Model</span></span>

<span data-ttu-id="2e8ba-179">Modelimizi oluşturmak için Visual Studio'nun bir parçası olarak yer alan Entity Framework Designer'ı kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-179">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="2e8ba-180">**Proje&gt; - Yeni Öğe Ekle...**</span><span class="sxs-lookup"><span data-stu-id="2e8ba-180">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="2e8ba-181">Sol menüden **Veri'yi** seçin ve ardından **Varlık Veri Modeli ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="2e8ba-181">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="2e8ba-182">Ürün **Model'i** ad olarak girin ve **Tamam'ı** tıklatın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-182">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="2e8ba-183">Bu, **Varlık Veri Modeli Sihirbazı'nı** başlattı</span><span class="sxs-lookup"><span data-stu-id="2e8ba-183">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="2e8ba-184">**Veritabanından Oluştur'u** seçin ve **İleri'yi** tıklatın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-184">Select **Generate from Database** and click **Next**</span></span>

    ![Model İçeriklerini Seçin](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="2e8ba-186">İlk bölümde oluşturduğunuz veritabanına bağlantıyı seçin, bağlantı dizesinin adı olarak **ProductContext'ı** girin ve **İleri'yi** tıklatın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-186">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Bağlantınızı Seçin](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="2e8ba-188">Tüm tabloları almak için 'Tablolar'ın yanındaki onay kutusunu tıklatın ve 'Finish' düğmesini tıklatın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-188">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Nesnelerinizi Seçin](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="2e8ba-190">Ters mühendislik işlemi tamamlandıktan sonra yeni model projenize eklenir ve Entity Framework Designer'da görüntülemeniz için açılır.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-190">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="2e8ba-191">Veritabanının bağlantı ayrıntılarıyla birlikte projenize bir App.config dosyası da eklendi.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-191">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="2e8ba-192">Visual Studio 2010'da Ek Adımlar</span><span class="sxs-lookup"><span data-stu-id="2e8ba-192">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="2e8ba-193">Visual Studio 2010'da çalışıyorsanız, EF6 kod oluşturmayı kullanmak için EF tasarımcısını güncellemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-193">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="2e8ba-194">EF Designer'da modelinizin boş bir noktasına sağ tıklayın ve **Kod Oluşturma Öğesi Ekle'yi seçin...**</span><span class="sxs-lookup"><span data-stu-id="2e8ba-194">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="2e8ba-195">Sol menüden **Çevrimiçi Şablonlar'ı** seçin ve **DbContext'ı** arayın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-195">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="2e8ba-196">**C\#için EF 6.x DbContext Jeneratörünü seçin,** ürün adı olarak **ProductsModel'i** girin ve Ekle'yi tıklatın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-196">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="2e8ba-197">Veri bağlama için kod oluşturma yı güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="2e8ba-197">Updating code generation for data binding</span></span>

<span data-ttu-id="2e8ba-198">EF, T4 şablonlarını kullanarak modelinizden kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-198">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="2e8ba-199">Visual Studio ile gönderilen veya Visual Studio galerisinden indirilen şablonlar genel amaçlı kullanım için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-199">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="2e8ba-200">Bu, bu şablonlardan oluşturulan varlıkların basit&lt;ICollection&gt; T özelliklerine sahip olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-200">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="2e8ba-201">Ancak, WPF kullanarak veri bağlama yaparken, WPF'nin koleksiyonlarda yapılan değişiklikleri takip edebilmesi için toplama özellikleri için **ObservableCollection'ı** kullanmak istenir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-201">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="2e8ba-202">Bu amaçla, observableCollection kullanmak için şablonları değiştirmek olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-202">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="2e8ba-203">Solution **Explorer'ı** açın ve **ProductModel.edmx** dosyasını bulun</span><span class="sxs-lookup"><span data-stu-id="2e8ba-203">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="2e8ba-204">ProductModel.edmx dosyasının altına yer alacak **ProductModel.tt** dosyayı bulun</span><span class="sxs-lookup"><span data-stu-id="2e8ba-204">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![WPF Ürün Modeli Şablonu](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="2e8ba-206">Visual Studio editöründe açmak için ProductModel.tt dosyasına çift tıklayın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-206">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="2e8ba-207">"**ICollection**" un iki olayını "**ObservableCollection**" ile bulun ve değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-207">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="2e8ba-208">Bunlar yaklaşık olarak 296 ve 484.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-208">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="2e8ba-209">"**HashSet**" in ilk oluşumunu "**ObservableCollection**" ile bulun ve değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-209">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="2e8ba-210">Bu oluşum yaklaşık 50 satırında yer alır.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-210">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="2e8ba-211">**Do not** Daha sonra kodda bulunan HashSet'in ikinci oluşumunu değiştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-211">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="2e8ba-212">"**System.Collections.Generic**" in tek oluşumunu "**System.Collections.ObjectModel**" ile bulun ve değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-212">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="2e8ba-213">Bu yaklaşık 424 hattında yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-213">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="2e8ba-214">ProductModel.tt dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-214">Save the ProductModel.tt file.</span></span> <span data-ttu-id="2e8ba-215">Bu, varlıkların yeniden oluşturulması için kodun neden olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-215">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="2e8ba-216">Kod otomatik olarak yenilenmezse, ProductModel.tt sağ tıklayın ve "Özel Aracı Çalıştır"ı seçin.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-216">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="2e8ba-217">Şimdi Category.cs dosyayı açarsanız (ProductModel.tt altında iç içe dir) o zaman Ürünler koleksiyonunda **ObservableCollection&lt;&gt;Ürün**türüne sahip olduğunu görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-217">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="2e8ba-218">Projeyi derle.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-218">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="2e8ba-219">Tembel Yükleme</span><span class="sxs-lookup"><span data-stu-id="2e8ba-219">Lazy Loading</span></span>

<span data-ttu-id="2e8ba-220">**Ürün** sınıfındaki **Kategori** sınıfı ve **Kategori** özelliğindeki **Ürünler** özelliği gezinti özellikleridir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-220">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="2e8ba-221">Varlık Çerçevesi'nde, gezinti özellikleri iki varlık türü arasındaki ilişkiyi gezinmenin bir yolunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-221">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="2e8ba-222">EF, gezinti özelliğine ilk erişimenizden itibaren ilgili varlıkları veritabanından otomatik olarak yükleme seçeneği sunar.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-222">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="2e8ba-223">Bu tür yüklemelerle (tembel yükleme olarak adlandırılır), her gezinti özelliğine ilk kez erişirdiğinizde, içerik bağlamında değilse veritabanına karşı ayrı bir sorgu yürütüleceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-223">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="2e8ba-224">POCO varlık türlerini kullanırken, EF çalışma süresi sırasında türetilmiş proxy türlerinin örnekleri oluşturarak ve ardından yükleme kancasını eklemek için sınıflarınızdaki sanal özellikleri geçersiz kılarak tembel yükleme elde eder.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-224">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="2e8ba-225">İlgili nesnelerin tembel cereyan etmesini sağlamak için, gezinti özelliği nial **ve** **sanal** (Visual Basic'te**geçersiz kılınabilir)** olarak beyan etmeniz gerekir ve sınıf **mühürlenmemelidir** (Visual Basic'te**Geçersiz kılınmaz).**</span><span class="sxs-lookup"><span data-stu-id="2e8ba-225">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="2e8ba-226">Veritabanı kullanırken İlk gezinme özellikleri otomatik olarak tembel yükleme sağlamak için sanal yapılır.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-226">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="2e8ba-227">Kod İlk bölümünde biz aynı nedenle navigasyon özellikleri sanal yapmak için seçti</span><span class="sxs-lookup"><span data-stu-id="2e8ba-227">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="2e8ba-228">Nesneyi Denetimlere Bağlama</span><span class="sxs-lookup"><span data-stu-id="2e8ba-228">Bind Object to Controls</span></span>

<span data-ttu-id="2e8ba-229">Modelde tanımlanan sınıfları bu WPF uygulaması için veri kaynağı olarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-229">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="2e8ba-230">Ana formu açmak için Solution Explorer'da **MainWindow.xaml'a** çift tıklayın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-230">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="2e8ba-231">Ana menüden **Project -&gt; Yeni Veri Kaynağı Ekle'yi** seçin ...</span><span class="sxs-lookup"><span data-stu-id="2e8ba-231">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="2e8ba-232">(Visual Studio 2010'da Veri - **Yeni Veri Kaynağı&gt; Ekle...**)</span><span class="sxs-lookup"><span data-stu-id="2e8ba-232">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="2e8ba-233">Veri Kaynağı Yazı Penceresini Seç'te **Nesne'yi** seçin ve **İleri'yi** tıklatın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-233">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="2e8ba-234">Veri Nesnelerini Seç iletişim kutusunda, **WPFwithEFSample'ı** iki kez açın ve **Kategori'yi** seçin</span><span class="sxs-lookup"><span data-stu-id="2e8ba-234">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="2e8ba-235">***Ürün** veri kaynağını seçmenize gerek yoktur, çünkü ürüne **Kategori**veri kaynağındaki **Category** Ürünün özelliği üzerinden ulaşacağız*</span><span class="sxs-lookup"><span data-stu-id="2e8ba-235">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Veri Nesnelerini Seçin](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="2e8ba-237">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-237">Click **Finish.**</span></span>
-   <span data-ttu-id="2e8ba-238">Veri Kaynakları penceresi MainWindow.xaml penceresinin yanında açılır \*Veri Kaynakları penceresi görünmüyorsa, **Görünüm -&gt; Diğer Windows-&gt; Veri Kaynakları** \*</span><span class="sxs-lookup"><span data-stu-id="2e8ba-238">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="2e8ba-239">Pin simgesine bastığımda, Veri Kaynakları penceresi otomatik gizlemez.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-239">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="2e8ba-240">Pencere zaten görünüyorsa yenileme düğmesine basmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-240">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Veri Kaynakları](~/ef6/media/datasources.png)

-   <span data-ttu-id="2e8ba-242"> **Kategori** veri kaynağını seçin ve forma sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-242">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="2e8ba-243">Bu kaynağı sürüklediğimizde şunlar oldu:</span><span class="sxs-lookup"><span data-stu-id="2e8ba-243">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="2e8ba-244">**CategoryViewSource** kaynak ve **kategoriDataGrid** kontrolü XAML eklendi</span><span class="sxs-lookup"><span data-stu-id="2e8ba-244">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="2e8ba-245">Ana Izgara öğesindeki DataContext özelliği "{StaticResource **categoryViewSource** }" olarak ayarlandı.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-245">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="2e8ba-246">**categoryViewSource** kaynağı dış\\üst Izgara öğesi için bağlayıcı bir kaynak olarak hizmet vermektedir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-246"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="2e8ba-247">İç Izgara öğeleri daha sonra ana Kılavuz'dan Veri Bağlamı değerini devralır (CategoryDataGrid'in ItemsSource özelliği "{Binding}" olarak ayarlanır)</span><span class="sxs-lookup"><span data-stu-id="2e8ba-247">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="2e8ba-248">Ayrıntı Izgara ekleme</span><span class="sxs-lookup"><span data-stu-id="2e8ba-248">Adding a Details Grid</span></span>

<span data-ttu-id="2e8ba-249">Kategorileri görüntülemek için bir ızgaramız olduğuna göre, ilişkili Ürünleri görüntülemek için bir ayrıntı ızgarası ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-249">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="2e8ba-250"> **Kategori** veri kaynağının altından **Ürünler** özelliğini seçin ve forma sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-250">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="2e8ba-251">**CategoryProductsViewSource** kaynak ve **ürünDataGrid** ızgara XAML eklenir</span><span class="sxs-lookup"><span data-stu-id="2e8ba-251">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="2e8ba-252">Bu kaynak için bağlayıcı yol Ürünler olarak ayarlanır</span><span class="sxs-lookup"><span data-stu-id="2e8ba-252">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="2e8ba-253">WPF veri bağlama çerçevesi, yalnızca seçili Kategoriile ilgili Ürünlerin **productDataGrid'de** gösterilmesini sağlar</span><span class="sxs-lookup"><span data-stu-id="2e8ba-253">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="2e8ba-254">Araç Kutusundan **Düğmeyi** forma sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-254">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="2e8ba-255">**Ad** özelliğini **kaydet düğmesine** ve **Kaydet'e İçerik** özelliğini ayarla. **Save**</span><span class="sxs-lookup"><span data-stu-id="2e8ba-255">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="2e8ba-256">Form buna benzer olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="2e8ba-256">The form should look similar to this:</span></span>

![Tasarımcı](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="2e8ba-258">Veri Etkileşimini İşleyen Kod Ekleme</span><span class="sxs-lookup"><span data-stu-id="2e8ba-258">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="2e8ba-259">Ana pencereye bazı olay işleyicileri eklemenin zamanı doldu.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-259">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="2e8ba-260">XAML penceresinde, \*\* &lt;Pencere\*\* öğesini tıklatın, bu ana pencereyi seçer</span><span class="sxs-lookup"><span data-stu-id="2e8ba-260">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="2e8ba-261">**Özellikler** penceresinde sağ üstteki **Etkinlikler'i** seçin ve **ardından Yüklenen** etiketin sağındaki metin kutusunu çift tıklatın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-261">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Ana Pencere Özellikleri](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="2e8ba-263">Ayrıca **tasarımcıda** Kaydet düğmesini çift tıklatarak **Kaydet** düğmesine tıklayın olayını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-263">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="2e8ba-264">Bu form için arkasındaki kod getiriyor, şimdi veri erişimi gerçekleştirmek için ProductContext kullanmak için kodu edeceğiz.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-264">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="2e8ba-265">MainWindow kodunu aşağıda gösterildiği gibi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-265">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="2e8ba-266">Kod, **ProductContext'ın**uzun soluklu bir örneğini bildirir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-266">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="2e8ba-267">**ProductContext** nesnesi verileri sorgulamak ve veritabanına kaydetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-267">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="2e8ba-268">**ProductContext** örneğindeki **Elden Çıkarma()** daha sonra geçersiz kılınan **OnClosing** yönteminden çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-268">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="2e8ba-269">Kod açıklamaları, kodun ne yaptığı hakkında ayrıntılı bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-269"> The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="2e8ba-270">WPF Uygulamasını Test Edin</span><span class="sxs-lookup"><span data-stu-id="2e8ba-270">Test the WPF Application</span></span>

-   <span data-ttu-id="2e8ba-271">Uygulamayı derle ve çalıştır.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-271">Compile and run the application.</span></span> <span data-ttu-id="2e8ba-272">Önce Kod'u kullandıysanız, sizin için bir **WPFwithEFSample.ProductContext** veritabanı oluşturulduğunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-272">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="2e8ba-273">*Birincil anahtar veritabanı tarafından oluşturulduğundan,* üst ızgaraya bir kategori adı ve alt ızgaraya ürün adları girin Kimlik sütunlarında hiçbir şey girmeyin</span><span class="sxs-lookup"><span data-stu-id="2e8ba-273">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Yeni kategoriler ve ürünlerle Ana Pencere](~/ef6/media/screen1.png)

-   <span data-ttu-id="2e8ba-275">Verileri veritabanına kaydetmek için **Kaydet** düğmesine basın</span><span class="sxs-lookup"><span data-stu-id="2e8ba-275">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="2e8ba-276">DbContext'S **SaveChanges()** için aramadan sonra, d's veritabanı oluşturulan değerleri ile doldurulur.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-276">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="2e8ba-277">**SaveChanges()** den sonra **Yenile()** adını verdiğimiz için **DataGrid** denetimleri yeni değerlerle de güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-277">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Doldurulan doldurulmuş doldurulmuş ana pencere](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="2e8ba-279">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2e8ba-279">Additional Resources</span></span>

<span data-ttu-id="2e8ba-280">WPF kullanarak koleksiyonlara veri bağlama hakkında daha fazla bilgi edinmek için [bu konuya](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) WPF belgelerinde bakın.</span><span class="sxs-lookup"><span data-stu-id="2e8ba-280">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  
