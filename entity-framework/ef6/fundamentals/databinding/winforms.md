---
title: WinForms - EF6 ile veri bağlama
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
caps.latest.revision: 3
ms.openlocfilehash: b17bc91fd7d665f6d75bf5f1e5798ddd16aa345d
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913443"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="d98ff-102">WinForms ile veri bağlama</span><span class="sxs-lookup"><span data-stu-id="d98ff-102">Databinding with WinForms</span></span>
<span data-ttu-id="d98ff-103">Bu adım adım kılavuzda, POCO türleri "ana öğe-ayrıntı" formunda (WinForms) Windows Formları denetimleri bağlama işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d98ff-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="d98ff-104">Uygulama, verileri veritabanından nesnelerle doldurmak, değişiklikleri izlemek ve veritabanına verileri kalıcı hale getirmek için Entity Framework kullanır.</span><span class="sxs-lookup"><span data-stu-id="d98ff-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="d98ff-105">Model-çok ilişkisini katılan iki tür tanımlar: Kategori (asıl\\ana) ve ürün (bağımlı\\ayrıntı).</span><span class="sxs-lookup"><span data-stu-id="d98ff-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="d98ff-106">Ardından, Visual Studio Araçları, WinForms denetimlere modelde tanımlanan türleri bağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d98ff-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="d98ff-107">İlgili nesneler arasında gezinmeyi WinForms Veri bağlama çerçeve sağlar: ana görünümünde satırları seçme ile ilgili alt verileri güncelleştirmek ayrıntılı görünüm neden olur.</span><span class="sxs-lookup"><span data-stu-id="d98ff-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="d98ff-108">Bu izlenecek yolda kod listeleri ve ekran görüntüleri, Visual Studio 2013'ten alınır ancak bu kılavuzda Visual Studio 2012 veya Visual Studio 2010 ile tamamlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d98ff-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="d98ff-109">Ön koşullar</span><span class="sxs-lookup"><span data-stu-id="d98ff-109">Pre-Requisites</span></span>

<span data-ttu-id="d98ff-110">Visual Studio 2013 olması gerekir, bu izlenecek yolu tamamlamak için Visual Studio 2012 veya Visual Studio 2010 'un yüklü.</span><span class="sxs-lookup"><span data-stu-id="d98ff-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="d98ff-111">Visual Studio 2010 kullanıyorsanız, aynı zamanda NuGet yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d98ff-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="d98ff-112">Daha fazla bilgi için [NuGet yükleme](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="d98ff-112">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="d98ff-113">Uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="d98ff-113">Create the Application</span></span>

-   <span data-ttu-id="d98ff-114">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="d98ff-114">Open Visual Studio</span></span>
-   <span data-ttu-id="d98ff-115">**Dosya -&gt; yeni -&gt; proje...**</span><span class="sxs-lookup"><span data-stu-id="d98ff-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="d98ff-116">Seçin **Windows** sol bölmede ve **Windows FormsApplication** sağ bölmede</span><span class="sxs-lookup"><span data-stu-id="d98ff-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="d98ff-117">Girin **WinFormswithEFSample** adı</span><span class="sxs-lookup"><span data-stu-id="d98ff-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="d98ff-118">Seçin **Tamam**</span><span class="sxs-lookup"><span data-stu-id="d98ff-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="d98ff-119">Entity Framework NuGet paketini yükle</span><span class="sxs-lookup"><span data-stu-id="d98ff-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="d98ff-120">Çözüm Gezgini'nde sağ **WinFormswithEFSample** proje</span><span class="sxs-lookup"><span data-stu-id="d98ff-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="d98ff-121">Seçin **NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="d98ff-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="d98ff-122">NuGet paketlerini Yönet iletişim kutusunda, seçmek **çevrimiçi** sekmesini **EntityFramework** paket</span><span class="sxs-lookup"><span data-stu-id="d98ff-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="d98ff-123">Tıklayın **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="d98ff-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="d98ff-124">EntityFramework derlemeye ek olarak bir başvuru System.ComponentModel.DataAnnotations de eklenir.</span><span class="sxs-lookup"><span data-stu-id="d98ff-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="d98ff-125">Projeyi bir başvuru System.Data.Entity varsa, EntityFramework paketi yüklendiğinde, sonra da kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="d98ff-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="d98ff-126">System.Data.Entity derleme için Entity Framework 6 uygulamaları artık kullanılmamaktadır.</span><span class="sxs-lookup"><span data-stu-id="d98ff-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="d98ff-127">IListSource koleksiyonlar için uygulama</span><span class="sxs-lookup"><span data-stu-id="d98ff-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="d98ff-128">Koleksiyon Özellikleri, Windows Forms kullanırken sıralama ile iki yönlü veri bağlamayı etkinleştirmek için IListSource arabirimini uygulamalıdır.</span><span class="sxs-lookup"><span data-stu-id="d98ff-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="d98ff-129">Bunu yapmak için IListSource işlevselliği eklemek için ObservableCollection genişletmek için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="d98ff-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="d98ff-130">Ekleme bir **ObservableListSource** projeye sınıfı:</span><span class="sxs-lookup"><span data-stu-id="d98ff-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="d98ff-131">Proje adına sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="d98ff-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="d98ff-132">Seçin **Ekle -&gt; yeni öğe**</span><span class="sxs-lookup"><span data-stu-id="d98ff-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="d98ff-133">Seçin **sınıfı** girin **ObservableListSource** sınıfı adı</span><span class="sxs-lookup"><span data-stu-id="d98ff-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="d98ff-134">Aşağıdaki kod ile varsayılan olarak oluşturulan kodu değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d98ff-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="d98ff-135">*Bu sınıf, bağlama yanı sıra sıralama çift yönlü veri sağlar. ObservableCollection sınıfı türetilen&lt;T&gt; ve IListSource açık uygulaması ekler. IListSource GetList() yöntemi ObservableCollection ile eşitlenmiş durumda kalmasını IBindingList uygulamaya geri dönmek için uygulanır. Sıralama ToBindingList tarafından oluşturulan IBindingList uygulanmasını destekler. ToBindingList uzantı yöntemi, EntityFramework derlemede tanımlandı.*</span><span class="sxs-lookup"><span data-stu-id="d98ff-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extention method is defined in the EntityFramework assembly.*</span></span>

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList  (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="d98ff-136">Bir modeli tanımlamak</span><span class="sxs-lookup"><span data-stu-id="d98ff-136">Define a Model</span></span>

<span data-ttu-id="d98ff-137">Code First veya EF Designer kullanarak bir model uygulamak aşağıdakileri yapabilirsiniz, bu izlenecek yolda seçtiniz.</span><span class="sxs-lookup"><span data-stu-id="d98ff-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="d98ff-138">İki bölümden birini tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="d98ff-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="d98ff-139">1. seçenek: Code First kullanarak bir Model tanımlayın</span><span class="sxs-lookup"><span data-stu-id="d98ff-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="d98ff-140">Bu bölümde, bir model ve Code First kullanarak ilişkili veritabanı oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d98ff-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="d98ff-141">Sonraki bölümde Atla (**seçeneği 2: veritabanı ilk kullanarak bir modeli tanımlamak)** modelinizi EF designer kullanarak bir veritabanından, bunun yerine veritabanı ilk tersine çevirmek için kullanacağınız, mühendislik</span><span class="sxs-lookup"><span data-stu-id="d98ff-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="d98ff-142">Code First geliştirme kullanırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan bir .NET Framework sınıf yazarak başlayın.</span><span class="sxs-lookup"><span data-stu-id="d98ff-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="d98ff-143">Yeni bir **ürün** projesine sınıfı</span><span class="sxs-lookup"><span data-stu-id="d98ff-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="d98ff-144">Aşağıdaki kod ile varsayılan olarak oluşturulan kodu değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d98ff-144">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   <span data-ttu-id="d98ff-145">Ekleme bir **kategori** projeye sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d98ff-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="d98ff-146">Aşağıdaki kod ile varsayılan olarak oluşturulan kodu değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d98ff-146">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

<span data-ttu-id="d98ff-147">Varlıklar tanımlayan ek olarak, türetilen bir sınıf tanımlamanız gereken **DbContext** ve ortaya çıkaran **olan DB&lt;TEntity&gt;**  özellikleri.</span><span class="sxs-lookup"><span data-stu-id="d98ff-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="d98ff-148">**Olan DB** özellikleri modele dahil etmek istediğiniz hangi türlerin bilmeniz bağlam olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="d98ff-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="d98ff-149">**DbContext** ve **olan DB** türleri EntityFramework derlemesinde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d98ff-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="d98ff-150">Nesneleri bir veritabanındaki verilerle doldurma içeren çalışma zamanı sırasında varlık nesnesi DbContext türetilmiş türün bir örneğini yönetir, izleme ve kalıcı veri veritabanına değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d98ff-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="d98ff-151">Yeni bir **ProductContext** projeye sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d98ff-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="d98ff-152">Aşağıdaki kod ile varsayılan olarak oluşturulan kodu değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d98ff-152">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="d98ff-153">Projeyi derle.</span><span class="sxs-lookup"><span data-stu-id="d98ff-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="d98ff-154">2. seçenek: Veritabanı ilk kullanarak bir model tanımlayın</span><span class="sxs-lookup"><span data-stu-id="d98ff-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="d98ff-155">Bu bölümde, modelinizi EF designer kullanarak bir veritabanındaki veritabanı ilk tersine mühendislik için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="d98ff-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="d98ff-156">Önceki bölümde tamamladıysanız (**1. seçenek: Code First kullanarak bir modeli tanımlamak)**, ardından bu bölümü atlayın ve gidebilirsiniz **yavaş Yükleniyor** bölümü.</span><span class="sxs-lookup"><span data-stu-id="d98ff-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="d98ff-157">Mevcut bir veritabanı oluşturun</span><span class="sxs-lookup"><span data-stu-id="d98ff-157">Create an Existing Database</span></span>

<span data-ttu-id="d98ff-158">Genellikle zaten oluşturulur varolan bir veritabanını hedeflerken, ancak bu kılavuz erişmek için bir veritabanı oluşturmak ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="d98ff-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="d98ff-159">Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösterir:</span><span class="sxs-lookup"><span data-stu-id="d98ff-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="d98ff-160">Visual Studio 2010 kullanıyorsanız, SQL Express veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="d98ff-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="d98ff-161">Visual Studio 2012 kullanıyorsanız sonra, oluşturduğunuz bir [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="d98ff-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="d98ff-162">Yeni bir ubuntu ve veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d98ff-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="d98ff-163">**Görünüm -&gt; Sunucu Gezgini**</span><span class="sxs-lookup"><span data-stu-id="d98ff-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="d98ff-164">Sağ tıklayın **veri bağlantıları -&gt; bağlantı ekle...**</span><span class="sxs-lookup"><span data-stu-id="d98ff-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="d98ff-165">Sunucu gezgininden veritabanına bağlamadıysanız önce Microsoft SQL Server veri kaynağı olarak seçmeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="d98ff-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="d98ff-167">LocalDB veya hangisinin bağlı olarak, yüklü, SQL Express için bağlanın ve girin **ürünleri** veritabanı adı</span><span class="sxs-lookup"><span data-stu-id="d98ff-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="d98ff-170">Seçin **Tamam** ve bir yeni bir veritabanı oluşturmak istiyorsanız istenir **Evet**</span><span class="sxs-lookup"><span data-stu-id="d98ff-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="d98ff-172">Yeni veritabanı sunucu Gezgini'nde artık görünecek üzerinde sağ tıklayıp **yeni sorgu**</span><span class="sxs-lookup"><span data-stu-id="d98ff-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="d98ff-173">Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **Yürüt**</span><span class="sxs-lookup"><span data-stu-id="d98ff-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="d98ff-174">Ters mühendislik modeli</span><span class="sxs-lookup"><span data-stu-id="d98ff-174">Reverse Engineer Model</span></span>

<span data-ttu-id="d98ff-175">Modelimiz oluşturmak için Entity Framework Visual Studio'nun bir parçası olarak dahil olan Designer'ın, kullanan oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="d98ff-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="d98ff-176">**Takım projesi -&gt; Yeni Öğe Ekle...**</span><span class="sxs-lookup"><span data-stu-id="d98ff-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="d98ff-177">Seçin **veri** sol menüden ardından **ADO.NET varlık veri modeli**</span><span class="sxs-lookup"><span data-stu-id="d98ff-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="d98ff-178">Girin **ProductModel** tıklayın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="d98ff-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="d98ff-179">Böylece **varlık veri modeli Sihirbazı**</span><span class="sxs-lookup"><span data-stu-id="d98ff-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="d98ff-180">Seçin **veritabanından Oluştur** tıklatıp **İleri**</span><span class="sxs-lookup"><span data-stu-id="d98ff-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="d98ff-182">İlk bölümde oluşturduğunuz veritabanı bağlantısı seçin, girin **ProductContext** tıklayın ve bağlantı dizesi adı olarak **İleri**</span><span class="sxs-lookup"><span data-stu-id="d98ff-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="d98ff-184">'Tüm tabloları Al ve 'Son' tablolar' yanındaki onay kutusuna tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d98ff-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="d98ff-186">Ters mühendislik işlemi tamamlandıktan sonra yeni modeli projenize eklenir ve Entity Framework Tasarımcısı'nda görüntülemeniz için açıldı.</span><span class="sxs-lookup"><span data-stu-id="d98ff-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="d98ff-187">Bir App.config dosyası ayrıca veritabanı bağlantı ayrıntıları ile projenize eklendi.</span><span class="sxs-lookup"><span data-stu-id="d98ff-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="d98ff-188">Visual Studio 2010'daki ek adımlar</span><span class="sxs-lookup"><span data-stu-id="d98ff-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="d98ff-189">Ardından Visual Studio 2010'da çalışıyorsanız EF designer EF6 kod oluşturmayı kullan güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d98ff-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="d98ff-190">Modelinizin EF Tasarımcısı'nda boş bir nokta üzerinde sağ tıklayıp **kod oluşturma öğesi Ekle...**</span><span class="sxs-lookup"><span data-stu-id="d98ff-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="d98ff-191">Seçin **çevrimiçi şablonlar** arayın ve sol menüden **DbContext**</span><span class="sxs-lookup"><span data-stu-id="d98ff-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="d98ff-192">Seçin **EF 6.x DbContext Oluşturucu c\#,** girin **ProductsModel** adıyla ve Ekle'ye tıklayın</span><span class="sxs-lookup"><span data-stu-id="d98ff-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="d98ff-193">Kod oluşturma için veri bağlamayı güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="d98ff-193">Updating code generation for data binding</span></span>

<span data-ttu-id="d98ff-194">EF modelinizden T4 şablonlarını kullanarak kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d98ff-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="d98ff-195">Şablonları Visual Studio ile birlikte gelen veya Visual Studio Galerisi'nden indirilir, genel amaçlı kullanım için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d98ff-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="d98ff-196">Bu bu şablonlardan oluşturulan varlıkları basit ICollection olduğu anlamına gelir&lt;T&gt; özellikleri.</span><span class="sxs-lookup"><span data-stu-id="d98ff-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="d98ff-197">Ancak, veri bağlama yaparken IListSource uygulayan koleksiyon özellikleri sağlamak için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="d98ff-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="d98ff-198">Bu nedenle yukarıdaki ObservableListSource sınıfı oluşturduk ve artık hale getirmek için şablonları değiştirme kullanacağız, bu sınıfı kullanın.</span><span class="sxs-lookup"><span data-stu-id="d98ff-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="d98ff-199">Açık **Çözüm Gezgini** ve bulma **ProductModel.edmx** dosyası</span><span class="sxs-lookup"><span data-stu-id="d98ff-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="d98ff-200">Bulma **ProductModel.tt** ProductModel.edmx dosyayı içe dosyası</span><span class="sxs-lookup"><span data-stu-id="d98ff-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![ProductModelTemplate](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="d98ff-202">Visual Studio Düzenleyicisi'nde açmak için ProductModel.tt dosyaya çift tıklayın</span><span class="sxs-lookup"><span data-stu-id="d98ff-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="d98ff-203">Bul ve Değiştir iki örneği "**ICollection**"ile"**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="d98ff-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="d98ff-204">Bu yaklaşık satırlar 296 ve 484 yer alır.</span><span class="sxs-lookup"><span data-stu-id="d98ff-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="d98ff-205">Bul ve Değiştir ilk geçtiği "**HashSet**"ile"**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="d98ff-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="d98ff-206">Bu durum, yaklaşık 50 satırında bulunur.</span><span class="sxs-lookup"><span data-stu-id="d98ff-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="d98ff-207">**Sağlamadığı** HashSet kod içinde bulunan ikinci oluşum değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d98ff-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="d98ff-208">ProductModel.tt dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="d98ff-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="d98ff-209">Bu, kodu yeniden oluşturulması varlıklar için neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="d98ff-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="d98ff-210">Kodu otomatik olarak yeniden oluşturmaz, ProductModel.tt üzerinde sağ tıklayın ve "Özel aracı çalıştır" ı seçin.</span><span class="sxs-lookup"><span data-stu-id="d98ff-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="d98ff-211">ProductModel.tt altında iç içe) Category.cs dosyasını (artık açın ardından ürünleri koleksiyon türlerinin olduğunu görmeniz gerekir **ObservableListSource&lt;ürün&gt;**.</span><span class="sxs-lookup"><span data-stu-id="d98ff-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="d98ff-212">Projeyi derle.</span><span class="sxs-lookup"><span data-stu-id="d98ff-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="d98ff-213">Yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="d98ff-213">Lazy Loading</span></span>

<span data-ttu-id="d98ff-214">**Ürünleri** özelliği **kategori** sınıfı ve **kategori** özelliği **ürün** Gezinti özellikleri sınıfı bulunur.</span><span class="sxs-lookup"><span data-stu-id="d98ff-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="d98ff-215">Varlık Çerçevesi'nde, gezinti özellikleri iki varlık türleri arasındaki bir ilişki gitmek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="d98ff-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="d98ff-216">EF ilgili varlıkları veritabanından otomatik olarak, gezinti özelliği ilk kez yüklenirken bir seçeneği sunar.</span><span class="sxs-lookup"><span data-stu-id="d98ff-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="d98ff-217">Bu (Gecikmeli yükleme olarak adlandırılır) yükleme türüyle içeriği değil zaten bir bağlamda ise, her gezinti özelliği ilk kez ayrı bir sorgu veritabanına karşı yürütülecek olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="d98ff-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="d98ff-218">EF yavaş yükleniyor POCO varlık türleri kullanırken, çalışma zamanı sırasında proxy türetilen türlerin örneklerini oluşturarak ve sonra da, sınıflarda yükleme kanca eklemek için sanal özelliklerini geçersiz kılma ulaşır.</span><span class="sxs-lookup"><span data-stu-id="d98ff-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="d98ff-219">İlgili nesnelerin yavaş yükleniyor almak için özellik alıcıları olarak Gezinti bildirmelidir **genel** ve **sanal** (**Overridable** Visual Basic'te), ve, sınıfı olmalıdır **korumalı** (**NotOverridable** Visual Basic'te).</span><span class="sxs-lookup"><span data-stu-id="d98ff-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="d98ff-220">Kullanarak veritabanı ilk Gezinti özellikleri otomatik olarak yavaş yükleniyor etkinleştirmek için sanal hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="d98ff-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="d98ff-221">Gezinti özellikleri aynı nedenden dolayı sanal yapmak seçtik kod Birinci bölümde</span><span class="sxs-lookup"><span data-stu-id="d98ff-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="d98ff-222">Denetimlerine nesne bağlama</span><span class="sxs-lookup"><span data-stu-id="d98ff-222">Bind Object to Controls</span></span>

<span data-ttu-id="d98ff-223">Aplikace WinForms için veri kaynağı olarak modelde tanımlı sınıfları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d98ff-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="d98ff-224">Ana menüden **proje -&gt; yeni veri kaynağı Ekle...**</span><span class="sxs-lookup"><span data-stu-id="d98ff-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="d98ff-225">(Visual Studio 2010'da seçmeniz gerekir. **veri -&gt; yeni veri kaynağı Ekle...** )</span><span class="sxs-lookup"><span data-stu-id="d98ff-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="d98ff-226">Bir veri kaynağı türü penceresi Seç seçin **nesne** tıklatıp **İleri**</span><span class="sxs-lookup"><span data-stu-id="d98ff-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="d98ff-227">Veri nesneleri iletişim seçin unfold **WinFormswithEFSample** seçin ve iki kez **kategori** ürün veri kaynağı seçmek için gerek yoktur çünkü biz için ürünün alırsınız Kategori veri kaynağı özelliği.</span><span class="sxs-lookup"><span data-stu-id="d98ff-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![Veri kaynağı](~/ef6/media/datasource.png)

-   <span data-ttu-id="d98ff-229">Tıklayın **son.** 
     *Veri kaynakları penceresinden görüntülenmiyor, seçin *** görünümü -&gt; diğer Windows -&gt; veri kaynakları**</span><span class="sxs-lookup"><span data-stu-id="d98ff-229">Click **Finish.**
*If the Data Sources window is not showing up, select***View -&gt; Other Windows-&gt; Data Sources**</span></span>
-   <span data-ttu-id="d98ff-230">Raptiye simgesini, böylece veri kaynakları penceresi otomatik gizleme tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="d98ff-230">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="d98ff-231">Pencere zaten görülebiliyorsa yenile düğmesine tıklama gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="d98ff-231">You may need to hit the refresh button if the window was already visible.</span></span>

    ![DataSource2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="d98ff-233">Çözüm Gezgini'nde çift tıklayarak **Form1.cs** dosyayı ana form Tasarımcısı'nda açın.</span><span class="sxs-lookup"><span data-stu-id="d98ff-233">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="d98ff-234">Seçin **kategori** veri kaynağı ve formda sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="d98ff-234">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="d98ff-235">Varsayılan olarak, yeni DataGridView (**categoryDataGridView**) ve gezinti araç çubuğu denetimleri tasarımcıya eklendi.</span><span class="sxs-lookup"><span data-stu-id="d98ff-235">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="d98ff-236">Bu denetimler için BindingSource bağlıdır (**categoryBindingSource**) ve Gezgin bağlama (**categoryBindingNavigator**) bileşenleri de oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d98ff-236">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="d98ff-237">Sütunları düzenleme **categoryDataGridView**.</span><span class="sxs-lookup"><span data-stu-id="d98ff-237">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="d98ff-238">Ayarlamak istediğiniz **CategoryID** sütun salt okunur.</span><span class="sxs-lookup"><span data-stu-id="d98ff-238">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="d98ff-239">Değeri **CategoryID** özelliği, biz verileri kaydettikten sonra veritabanı tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d98ff-239">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="d98ff-240">DataGridView denetimine sağ tıklayın ve sütunları Düzenle...</span><span class="sxs-lookup"><span data-stu-id="d98ff-240">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="d98ff-241">CategoryID sütunu seçin ve salt okunur True olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d98ff-241">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="d98ff-242">Tamam'a basın</span><span class="sxs-lookup"><span data-stu-id="d98ff-242">Press OK</span></span>
-   <span data-ttu-id="d98ff-243">Kategori veri kaynağı gezintisinde ürünleri seçin ve formda sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="d98ff-243">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="d98ff-244">ProductDataGridView ve productBindingSource formuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="d98ff-244">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="d98ff-245">ProductDataGridView sütunlarda düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="d98ff-245">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="d98ff-246">Kullanıcı, Categoryıd'si ve Kategori sütunları gizleyin ve ProductID salt okunur olarak ayarlamak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="d98ff-246">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="d98ff-247">ProductID özelliğinin değeri, biz verileri kaydettikten sonra veritabanı tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d98ff-247">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="d98ff-248">DataGridView denetimi sağ tıklayıp **sütunları Düzenle...** .</span><span class="sxs-lookup"><span data-stu-id="d98ff-248">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="d98ff-249">Seçin **ProductID** sütun ve kümesi **salt okunur** için **True**.</span><span class="sxs-lookup"><span data-stu-id="d98ff-249">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="d98ff-250">Seçin **CategoryID** sütun ve ENTER tuşuna **Kaldır** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d98ff-250">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="d98ff-251">İle aynı yapmak **kategori** sütun.</span><span class="sxs-lookup"><span data-stu-id="d98ff-251">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="d98ff-252">Tuşuna **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="d98ff-252">Press **OK**.</span></span>

    <span data-ttu-id="d98ff-253">Şu ana kadar biz Tasarımcısı'nda BindingSource bileşenleri ile bizim DataGridView denetimi ilişkili.</span><span class="sxs-lookup"><span data-stu-id="d98ff-253">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="d98ff-254">Sonraki bölümde kod arkasındaki kodda şu anda DbContext tarafından izlenen varlık koleksiyonunu categoryBindingSource.DataSource koymak için ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="d98ff-254">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="d98ff-255">Ne zaman biz sürüklediğiniz ve bırakılan ürün kategorisi, WinForms gezintisinde sürdü ürünleri categoryBindingSource ve productsBindingSource.DataMember özelliğini productsBindingSource.DataSource özelliğini ayarlama dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="d98ff-255">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="d98ff-256">Bu bağlama nedeniyle şu anda seçili kategoriye ait olan ürünleri productDataGridView içinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d98ff-256">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="d98ff-257">Etkinleştirme **Kaydet** sağ fare düğmesine tıklayıp seçerek gezinti araç çubuğunda **etkin**.</span><span class="sxs-lookup"><span data-stu-id="d98ff-257">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Form1 Tasarımcısı](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="d98ff-259">Kayıt için olay işleyicisi ekleme düğmesi düğmesine çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d98ff-259">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="d98ff-260">Olay işleyicisi eklemek ve form için arka plan kod için bilgisayarınızı getirin.</span><span class="sxs-lookup"><span data-stu-id="d98ff-260">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="d98ff-261">Kodu **categoryBindingNavigatorSaveItem\_tıklayın** olay işleyicisi, sonraki bölümde eklenir.</span><span class="sxs-lookup"><span data-stu-id="d98ff-261">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="d98ff-262">Veri etkileşimini işleyen kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="d98ff-262">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="d98ff-263">Veri erişimi gerçekleştirdiği ProductContext kullanmak için kodu artık ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="d98ff-263">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="d98ff-264">Kodu ana form penceresi, aşağıda gösterildiği gibi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="d98ff-264">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="d98ff-265">Kod ProductContext uzun süre çalışan bir örneğini bildirir.</span><span class="sxs-lookup"><span data-stu-id="d98ff-265">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="d98ff-266">ProductContext nesnesi, sorgu ve veri veritabanına kaydetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d98ff-266">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="d98ff-267">Dispose() yöntemini ProductContext örneğinde geçersiz kılınan OnClosing yönteminden sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d98ff-267">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="d98ff-268">Kod açıklamaları ne yaptığını hakkında ayrıntılar sağlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d98ff-268">The code comments provide details about what the code does.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="d98ff-269">Windows Forms uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="d98ff-269">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="d98ff-270">Derleme ve çalıştırma ve uygulama işlevini test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d98ff-270">Compile and run the application and you can test out the functionality.</span></span>

    ![Form1BeforeSave](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="d98ff-272">Kaydettikten sonra ekranda oluşturulan depolama anahtarları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d98ff-272">After saving the store generated keys are shown on the screen.</span></span>

    ![Form1AfterSave](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="d98ff-274">Code First kullanılan sonra da göreceksiniz bir **WinFormswithEFSample.ProductContext** veritabanı sizin için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d98ff-274">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![ServerObjExplorer](~/ef6/media/serverobjexplorer.png)
