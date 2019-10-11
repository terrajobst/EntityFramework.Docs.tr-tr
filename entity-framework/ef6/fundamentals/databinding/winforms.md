---
title: WinForms ile veri bağlama-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 4b3eee20ff238864b94ef4edfb97c1bae0713300
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181795"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="61dce-102">WinForms ile veri bağlama</span><span class="sxs-lookup"><span data-stu-id="61dce-102">Databinding with WinForms</span></span>
<span data-ttu-id="61dce-103">Bu adım adım izlenecek yol, POCO türlerinin bir "ana-ayrıntı" formunda pencere formları (WinForms) denetimlerine nasıl bağlanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="61dce-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="61dce-104">Uygulama, nesneleri veritabanındaki verilerle doldurmak, değişiklikleri izlemek ve verileri veritabanına kalıcı hale getirmek için Entity Framework kullanır.</span><span class="sxs-lookup"><span data-stu-id="61dce-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="61dce-105">Model bire çok ilişkisine katılan iki tür tanımlar: Kategori (Principal @ no__t-0master) ve ürün (bağımlı @ no__t-1detail).</span><span class="sxs-lookup"><span data-stu-id="61dce-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="61dce-106">Daha sonra, Visual Studio Araçları modelde tanımlanan türleri WinForms denetimlerine bağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="61dce-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="61dce-107">WinForms veri bağlama çerçevesi ilgili nesneler arasında gezinmeyi sağlar: ana görünümdeki satırları seçme, ayrıntı görünümünün karşılık gelen alt verilerle güncelleştirilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="61dce-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="61dce-108">Bu izlenecek yolda ekran görüntüleri ve kod listeleri Visual Studio 2013 alınmıştır ancak Visual Studio 2012 veya Visual Studio 2010 ile bu yönergeyi tamamlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="61dce-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="61dce-109">Önkoşulların önkoşulları</span><span class="sxs-lookup"><span data-stu-id="61dce-109">Pre-Requisites</span></span>

<span data-ttu-id="61dce-110">Bu izlenecek yolu tamamlamak için Visual Studio 2013, Visual Studio 2012 veya Visual Studio 2010 yüklü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="61dce-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="61dce-111">Visual Studio 2010 kullanıyorsanız, NuGet ' i de yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="61dce-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="61dce-112">Daha fazla bilgi için bkz. [NuGet 'ı yükleme](https://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="61dce-112">For more information, see [Installing NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="61dce-113">Uygulamayı oluşturma</span><span class="sxs-lookup"><span data-stu-id="61dce-113">Create the Application</span></span>

-   <span data-ttu-id="61dce-114">Visual Studio 'Yu aç</span><span class="sxs-lookup"><span data-stu-id="61dce-114">Open Visual Studio</span></span>
-   <span data-ttu-id="61dce-115">**Dosya-&gt; yeni-&gt; proje....**</span><span class="sxs-lookup"><span data-stu-id="61dce-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="61dce-116">Sağ bölmedeki sol bölmedeki ve **Windows FormsApplication** penceresinde **Windows** ' u seçin</span><span class="sxs-lookup"><span data-stu-id="61dce-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="61dce-117">Ad olarak **Winformyüzthefsample** girin</span><span class="sxs-lookup"><span data-stu-id="61dce-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="61dce-118">**Tamam 'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="61dce-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="61dce-119">Entity Framework NuGet paketini yükler</span><span class="sxs-lookup"><span data-stu-id="61dce-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="61dce-120">Çözüm Gezgini, **Winformyüzthefsample** projesine sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="61dce-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="61dce-121">**NuGet Paketlerini Yönet...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="61dce-122">NuGet Paketlerini Yönet iletişim kutusunda **çevrimiçi** sekmesini seçin ve **EntityFramework** paketini seçin</span><span class="sxs-lookup"><span data-stu-id="61dce-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="61dce-123">**Install** 'a tıklayın</span><span class="sxs-lookup"><span data-stu-id="61dce-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="61dce-124">EntityFramework derlemesine ek olarak, System. ComponentModel. Dataaçıklamalarda bir başvuru de eklenir.</span><span class="sxs-lookup"><span data-stu-id="61dce-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="61dce-125">Projenin System. Data. Entity başvurusu varsa, EntityFramework paketi yüklendiğinde kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="61dce-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="61dce-126">System. Data. Entity derlemesi artık Entity Framework 6 uygulamaları için kullanılmıyor.</span><span class="sxs-lookup"><span data-stu-id="61dce-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="61dce-127">Koleksiyonlar için ISource uygulama</span><span class="sxs-lookup"><span data-stu-id="61dce-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="61dce-128">Windows Forms kullanılırken sıralama ile iki yönlü veri bağlamayı etkinleştirmek için koleksiyon özelliklerinin ISource arabirimini uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="61dce-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="61dce-129">Bunu yapmak için ObservableCollection ISource işlevselliği eklemek üzere genişletireceğiz.</span><span class="sxs-lookup"><span data-stu-id="61dce-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="61dce-130">Projeye bir **ObservableListSource** sınıfı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="61dce-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="61dce-131">Proje adına sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="61dce-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="61dce-132">**Add-&gt; yeni öğe** seçin</span><span class="sxs-lookup"><span data-stu-id="61dce-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="61dce-133">Sınıf adı için **Class** ' ı seçin ve **ObservableListSource** girin</span><span class="sxs-lookup"><span data-stu-id="61dce-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="61dce-134">Varsayılan olarak oluşturulan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="61dce-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="61dce-135">*bu sınıf iki yönlü veri bağlamayı ve sıralamayı mümkün kılmaktadır. Sınıf, ObservableCollection @ no__t-0T @ no__t-1 ' den türetilir ve ISource 'un açık bir uygulamasını ekler. ISource 'un GetList () yöntemi, ObservableCollection ile eşitlenmiş bir IBindingList uygulamasını döndürecek şekilde uygulanır. ToBindingList tarafından oluşturulan IBindingList uygulamasının sıralamayı destekler. ToBindingList Extension yöntemi EntityFramework derlemesinde tanımlanmıştır.*</span><span class="sxs-lookup"><span data-stu-id="61dce-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extension method is defined in the EntityFramework assembly.*</span></span>

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
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="61dce-136">Model tanımlama</span><span class="sxs-lookup"><span data-stu-id="61dce-136">Define a Model</span></span>

<span data-ttu-id="61dce-137">Bu izlenecek yolda, Code First veya EF tasarımcısını kullanarak bir model uygulamayı seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="61dce-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="61dce-138">Aşağıdaki iki bölümden birini doldurun.</span><span class="sxs-lookup"><span data-stu-id="61dce-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="61dce-139">Seçenek 1: Code First kullanarak model tanımlama</span><span class="sxs-lookup"><span data-stu-id="61dce-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="61dce-140">Bu bölümde, Code First kullanarak bir modelin ve ilişkili veritabanının nasıl oluşturulacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="61dce-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="61dce-141">Sonraki bölüme atlayın (**Seçenek 2: EF Designer kullanarak bir veritabanından modelinize ters mühendislik uygulamak için Database First kullanmayı tercih ediyorsanız, Database First)**  kullanarak bir model tanımlayın</span><span class="sxs-lookup"><span data-stu-id="61dce-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="61dce-142">Code First geliştirme kullanılırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan .NET Framework sınıfları yazarak başlarsınız.</span><span class="sxs-lookup"><span data-stu-id="61dce-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="61dce-143">Projeye yeni bir **ürün** sınıfı ekleyin</span><span class="sxs-lookup"><span data-stu-id="61dce-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="61dce-144">Varsayılan olarak oluşturulan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="61dce-144">Replace the code generated by default with the following code:</span></span>

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

-   <span data-ttu-id="61dce-145">Projeye bir **Kategori** sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="61dce-146">Varsayılan olarak oluşturulan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="61dce-146">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="61dce-147">Varlıkları tanımlamaya ek olarak, **DbContext** öğesinden türeten bir sınıf tanımlamanız ve **dbset @ no__t-2TEntity @ no__t-3** özelliklerini kullanıma sunabilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="61dce-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="61dce-148">**Dbset** özellikleri bağlamın modele hangi türleri dahil etmek istediğinizi bilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="61dce-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="61dce-149">**DbContext** ve **Dbset** türleri EntityFramework derlemesinde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="61dce-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="61dce-150">DbContext türetilmiş türünün bir örneği, çalışma zamanı sırasında varlık nesnelerini yönetir, bu da nesneleri bir veritabanındaki verilerle doldurmayı, değişiklik izlemeyi ve kalıcı verileri veritabanına getirmeyi içerir.</span><span class="sxs-lookup"><span data-stu-id="61dce-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="61dce-151">Projeye yeni bir **Productcontext** sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="61dce-152">Varsayılan olarak oluşturulan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="61dce-152">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="61dce-153">Projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="61dce-154">Seçenek 2: Database First kullanarak model tanımlama</span><span class="sxs-lookup"><span data-stu-id="61dce-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="61dce-155">Bu bölümde, EF Designer kullanarak bir veritabanından modelinize ters mühendislik uygulamak için Database First nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="61dce-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="61dce-156">Önceki bölümü tamamladıysanız (**Seçenek 1: Code First)**  kullanarak bir model tanımlayın, ardından bu bölümü atlayıp doğrudan **yavaş yükleme** bölümüne gidin.</span><span class="sxs-lookup"><span data-stu-id="61dce-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="61dce-157">Var olan bir veritabanı oluştur</span><span class="sxs-lookup"><span data-stu-id="61dce-157">Create an Existing Database</span></span>

<span data-ttu-id="61dce-158">Genellikle, var olan bir veritabanını hedeflerken zaten oluşturulur, ancak bu izlenecek yol için, erişmek üzere bir veritabanı oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="61dce-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="61dce-159">Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösteren bir sürümdür:</span><span class="sxs-lookup"><span data-stu-id="61dce-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="61dce-160">Visual Studio 2010 kullanıyorsanız, bir SQL Express veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="61dce-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="61dce-161">Visual Studio 2012 kullanıyorsanız, [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="61dce-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="61dce-162">Şimdi veritabanını oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="61dce-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="61dce-163">**@No__t-1 Sunucu Gezgini görüntüle**</span><span class="sxs-lookup"><span data-stu-id="61dce-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="61dce-164">Veri bağlantıları ' na sağ tıklayın **-&gt; bağlantı ekle...**</span><span class="sxs-lookup"><span data-stu-id="61dce-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="61dce-165">Sunucu Gezgini bir veritabanına bağlı değilseniz, veri kaynağı olarak Microsoft SQL Server seçmeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="61dce-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Veri kaynağını Değiştir](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="61dce-167">Hangisini yüklediğinize bağlı olarak LocalDB 'ye veya SQL Express 'e bağlanın ve veritabanı adı olarak **ürünleri** girin</span><span class="sxs-lookup"><span data-stu-id="61dce-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Bağlantı LocalDB Ekle](~/ef6/media/addconnectionlocaldb.png)

    ![Bağlantı Express ekleme](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="61dce-170">**Tamam** ' ı seçtiğinizde, yeni bir veritabanı oluşturmak isteyip istemediğiniz sorulur, **Evet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="61dce-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Veritabanı oluştur](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="61dce-172">Yeni veritabanı artık Sunucu Gezgini görüntülenir, sağ tıklayıp **Yeni sorgu** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="61dce-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="61dce-173">Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **Yürüt** ' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="61dce-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="61dce-174">Tersine mühendislik modeli</span><span class="sxs-lookup"><span data-stu-id="61dce-174">Reverse Engineer Model</span></span>

<span data-ttu-id="61dce-175">Modelimizi oluşturmak için Visual Studio 'nun bir parçası olarak dahil edilen Entity Framework Designer kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="61dce-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="61dce-176">**Proje-&gt; yeni öğe Ekle...**</span><span class="sxs-lookup"><span data-stu-id="61dce-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="61dce-177">Sol menüden **verileri** seçin ve ardından **ADO.net varlık veri modeli**</span><span class="sxs-lookup"><span data-stu-id="61dce-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="61dce-178">Ad olarak **ProductModel** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="61dce-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="61dce-179">Bu, **varlık veri modeli Sihirbazı 'nı** başlatır</span><span class="sxs-lookup"><span data-stu-id="61dce-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="61dce-180">**Veritabanından oluştur** ' u seçin ve **İleri** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61dce-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="61dce-182">İlk bölümde oluşturduğunuz veritabanına bağlantıyı seçin, bağlantı dizesinin adı olarak **Productcontext** girin ve **İleri** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61dce-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Bağlantınızı seçin](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="61dce-184">Tüm tabloları içeri aktarmak için ' tablolar ' seçeneğinin yanındaki onay kutusuna tıklayın ve ' son ' a tıklayın</span><span class="sxs-lookup"><span data-stu-id="61dce-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Nesnelerinizi seçin](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="61dce-186">Tersine mühendislik işlemi tamamlandıktan sonra, yeni model projenize eklenir ve Entity Framework Designer görüntülemeniz için açılır.</span><span class="sxs-lookup"><span data-stu-id="61dce-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="61dce-187">Veritabanına yönelik bağlantı ayrıntılarına sahip bir App. config dosyası da projenize eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="61dce-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="61dce-188">Visual Studio 2010 ' de ek adımlar</span><span class="sxs-lookup"><span data-stu-id="61dce-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="61dce-189">Visual Studio 2010 ' de çalışıyorsanız, EF6 kod oluşturmayı kullanmak için EF tasarımcısını güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="61dce-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="61dce-190">EF Designer 'daki modelinizin boş bir noktasına sağ tıklayıp **kod oluşturma öğesi Ekle...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="61dce-191">Sol menüden **çevrimiçi şablonlar** ' ı seçin ve **DbContext** ' i arayın</span><span class="sxs-lookup"><span data-stu-id="61dce-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="61dce-192">**C @ no__t-1 Için EF 6. x DbContext Generator ' ı seçin,** ad olarak **productsmodel** girin ve Ekle ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61dce-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="61dce-193">Veri bağlama için kod üretimi güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="61dce-193">Updating code generation for data binding</span></span>

<span data-ttu-id="61dce-194">EF, modelden T4 şablonları kullanarak kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="61dce-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="61dce-195">Visual Studio ile birlikte gelen veya Visual Studio galerisinden indirilen şablonlar genel amaçlı kullanım amaçlıdır.</span><span class="sxs-lookup"><span data-stu-id="61dce-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="61dce-196">Bu, bu şablonlardan oluşturulan varlıkların basit ICollection @ no__t-0T @ no__t-1 özelliklerine sahip olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="61dce-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="61dce-197">Ancak, veri bağlama yaparken ISource 'u uygulayan koleksiyon özelliklerinin olması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="61dce-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="61dce-198">Bu nedenle, yukarıdaki ObservableListSource sınıfını oluşturuyoruz ve artık bu sınıftan kullanımı sağlamak için şablonları değiştirebiliyoruz.</span><span class="sxs-lookup"><span data-stu-id="61dce-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="61dce-199">**Çözüm Gezgini** açın ve **ProductModel. edmx** dosyasını bulun</span><span class="sxs-lookup"><span data-stu-id="61dce-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="61dce-200">ProductModel. edmx dosyasının altında iç içe olacak **ProductModel.tt** dosyasını bulun</span><span class="sxs-lookup"><span data-stu-id="61dce-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Ürün modeli şablonu](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="61dce-202">Visual Studio Düzenleyicisi 'nde açmak için ProductModel.tt dosyasına çift tıklayın</span><span class="sxs-lookup"><span data-stu-id="61dce-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="61dce-203">"**ICollection**" sözcüğünün iki yinelemesini "**ObservableListSource**" ile bulur ve değiştirin.</span><span class="sxs-lookup"><span data-stu-id="61dce-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="61dce-204">Bunlar yaklaşık olarak 296 ve 484 satırlarında bulunur.</span><span class="sxs-lookup"><span data-stu-id="61dce-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="61dce-205">İlk "**HashSet**" oluşumunu "**ObservableListSource**" ile bulur ve değiştirin.</span><span class="sxs-lookup"><span data-stu-id="61dce-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="61dce-206">Bu oluşum yaklaşık satır 50 ' de bulunur.</span><span class="sxs-lookup"><span data-stu-id="61dce-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="61dce-207">Kodda daha sonra bulunan ikinci diyez kümesi **oluşumunu değiştirmeyin.**</span><span class="sxs-lookup"><span data-stu-id="61dce-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="61dce-208">ProductModel.tt dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="61dce-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="61dce-209">Bu, varlıkların kodunun yeniden üretilmesine neden olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="61dce-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="61dce-210">Kod otomatik olarak yeniden oluşturmaz, ProductModel.tt 'e sağ tıklayın ve "özel araç Çalıştır" seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="61dce-211">Artık Category.cs dosyasını (ProductModel.tt altında iç içe) açarsanız, Products koleksiyonunun **ObservableListSource @ no__t-1Product @ no__t-2**türünde olduğunu görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="61dce-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="61dce-212">Projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="61dce-213">Geç yükleme</span><span class="sxs-lookup"><span data-stu-id="61dce-213">Lazy Loading</span></span>

<span data-ttu-id="61dce-214">**Product** sınıfında **category** sınıfı ve **category** özelliğindeki **Products** özelliği gezinti özellikleridir.</span><span class="sxs-lookup"><span data-stu-id="61dce-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="61dce-215">Entity Framework, gezinti özellikleri iki varlık türü arasında bir ilişkiye gitmek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="61dce-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="61dce-216">EF, gezinti özelliğine ilk kez eriştiğinizde ilgili varlıkları veritabanından otomatik olarak yükleme seçeneği sunar.</span><span class="sxs-lookup"><span data-stu-id="61dce-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="61dce-217">Bu yükleme türü (yavaş yükleme olarak adlandırılır) ile, her gezinti özelliğine ilk kez eriştiğinizde, içerik bağlamda değilse veritabanına karşı ayrı bir sorgu yürütülecektir.</span><span class="sxs-lookup"><span data-stu-id="61dce-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="61dce-218">POCO varlık türleri kullanılırken, EF, çalışma zamanı sırasında türetilmiş proxy türlerinin örneklerini oluşturarak ve sonra yükleme kancasını eklemek için sınıflarınızda sanal özellikleri geçersiz kılarak geç yüklemeye erişir.</span><span class="sxs-lookup"><span data-stu-id="61dce-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="61dce-219">İlgili nesnelerin geç yüklemesini almak için, gezinti özelliği alıcıları ' ı **Public** ve **Virtual** (Visual Basic) olarak bildirmeniz ve sınıfınız **mühürlenmemelidir** **(Visual Basic** içinde**NotOverridable** ).</span><span class="sxs-lookup"><span data-stu-id="61dce-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="61dce-220">Database First gezinti özelliklerinin kullanılması, yavaş yüklemeyi etkinleştirmek için otomatik olarak sanal hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="61dce-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="61dce-221">Code First bölümünde, aynı nedenden dolayı gezinti özelliklerini sanal hale getirmeyi seçtik</span><span class="sxs-lookup"><span data-stu-id="61dce-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="61dce-222">Nesneyi denetimlere bağlama</span><span class="sxs-lookup"><span data-stu-id="61dce-222">Bind Object to Controls</span></span>

<span data-ttu-id="61dce-223">Bu WinForms uygulaması için modelde tanımlanan sınıfları veri kaynağı olarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="61dce-224">Ana menüden **Proje-&gt; yeni veri kaynağı Ekle...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="61dce-225">(Visual Studio 2010 ' de, veri @no__t seçmeniz gerekir- **1 yeni veri kaynağı Ekle...** )</span><span class="sxs-lookup"><span data-stu-id="61dce-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="61dce-226">Veri kaynağı türü seçin penceresinde, **nesne** ' yi seçin ve **İleri** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61dce-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="61dce-227">Veri nesnelerini seçin iletişim kutusunda, **Winformyüzthefsample** 'ın iki kez katlamayı kaldırın ve **Kategori** ' yi seçin. Bu, ürün veri kaynağını seçmek zorunda kalmaz.</span><span class="sxs-lookup"><span data-stu-id="61dce-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![Veri Kaynağı](~/ef6/media/datasource.png)

-   <span data-ttu-id="61dce-229">Son ' a tıklayın **.**</span><span class="sxs-lookup"><span data-stu-id="61dce-229">Click **Finish.**</span></span>
    <span data-ttu-id="61dce-230">Veri kaynakları penceresi görüntülenmiyorsa, **diğer Windows-&gt; veri kaynaklarını görüntüle-&gt;** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="61dce-230">If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources**</span></span>
-   <span data-ttu-id="61dce-231">Veri kaynakları penceresi otomatik olarak gizlenmemesi için Sabitle simgesine basın.</span><span class="sxs-lookup"><span data-stu-id="61dce-231">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="61dce-232">Pencere zaten görünür durumdaysa Yenile düğmesine vurması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="61dce-232">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Veri kaynağı 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="61dce-234">Çözüm Gezgini, tasarımcıda ana formu açmak için **Form1.cs** dosyasına çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61dce-234">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="61dce-235">**Kategori** veri kaynağını seçin ve form üzerine sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-235">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="61dce-236">Varsayılan olarak, tasarımcıya yeni bir DataGridView (**Categorydatagridview**) ve gezinti araç çubuğu denetimleri eklenir.</span><span class="sxs-lookup"><span data-stu-id="61dce-236">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="61dce-237">Bu denetimler, ayrıca oluşturulan BindingSource (**Categorybindingsource**) ve bağlama Gezgini (**categorybindingnavigator**) bileşenlerine bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="61dce-237">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="61dce-238">**Categorydatagridview**üzerinde sütunları düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-238">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="61dce-239">**CategoryID** sütununu salt okunurdur olarak ayarlamak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="61dce-239">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="61dce-240">Verileri kaydettikten sonra **CategoryID** özelliğinin değeri veritabanı tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="61dce-240">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="61dce-241">DataGridView denetimine sağ tıklayıp sütunları Düzenle... seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-241">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="61dce-242">CategoryID sütununu seçin ve ReadOnly değerini true olarak ayarlayın</span><span class="sxs-lookup"><span data-stu-id="61dce-242">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="61dce-243">Tamam 'a basın</span><span class="sxs-lookup"><span data-stu-id="61dce-243">Press OK</span></span>
-   <span data-ttu-id="61dce-244">Kategori veri kaynağı altında products ' ı seçin ve forma sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-244">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="61dce-245">ProductDataGridView ve productBindingSource forma eklenir.</span><span class="sxs-lookup"><span data-stu-id="61dce-245">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="61dce-246">ProductDataGridView üzerinde sütunları düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-246">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="61dce-247">CategoryID ve category sütunlarını gizlemek ve ProductID 'yi salt okunurdur olarak ayarlamak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="61dce-247">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="61dce-248">Verileri kaydettikten sonra ProductID özelliğinin değeri veritabanı tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="61dce-248">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="61dce-249">DataGridView denetimine sağ tıklayıp **Sütunları Düzenle...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-249">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="61dce-250">**ProductID** sütununu seçin ve **ReadOnly** **değerini true**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="61dce-250">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="61dce-251">**CategoryID** sütununu seçin ve **Kaldır** düğmesine basın.</span><span class="sxs-lookup"><span data-stu-id="61dce-251">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="61dce-252">**Kategori** sütunuyla aynısını yapın.</span><span class="sxs-lookup"><span data-stu-id="61dce-252">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="61dce-253">Tuşuna **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="61dce-253">Press **OK**.</span></span>

    <span data-ttu-id="61dce-254">Şimdiye kadar, tasarımcı 'daki BindingSource bileşenleriyle DataGridView denetimlerimizi ilişkilendirdik.</span><span class="sxs-lookup"><span data-stu-id="61dce-254">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="61dce-255">Sonraki bölümde, categoryBindingSource. DataSource öğesini DbContext tarafından izlenmekte olan varlıkların koleksiyonuna ayarlamak için arkasındaki koda kod ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="61dce-255">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="61dce-256">Kategori altındaki ürünleri sürüklediğimiz ve bıraktığı zaman, WinForms, productsBindingSource. DataSource özelliğini Products BindingSource ve productsBindingSource. DataMember özelliğine ayarlamamız gerekir.</span><span class="sxs-lookup"><span data-stu-id="61dce-256">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="61dce-257">Bu bağlama nedeniyle, yalnızca şu anda seçili olan kategoriye ait ürünler productDataGridView içinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="61dce-257">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="61dce-258">Sağ fare düğmesine tıklayıp **etkin**' i seçerek gezinti araç çubuğundaki **Kaydet** düğmesini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="61dce-258">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Form 1 Tasarımcısı](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="61dce-260">Düğmeye çift tıklayarak Kaydet düğmesine ait olay işleyicisini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="61dce-260">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="61dce-261">Bu işlem olay işleyicisini ekler ve sizi form için arka plan koduna getirir.</span><span class="sxs-lookup"><span data-stu-id="61dce-261">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="61dce-262">**Categorybindinggezgintorsaveıtem @ no__t-1Click** olay işleyicisi kodu bir sonraki bölüme eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="61dce-262">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="61dce-263">Veri etkileşimini Işleyen kodu ekleyin</span><span class="sxs-lookup"><span data-stu-id="61dce-263">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="61dce-264">Artık veri erişimi gerçekleştirmek için ProductContext kullanmak üzere kodu ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="61dce-264">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="61dce-265">Ana form penceresi için kodu aşağıda gösterildiği gibi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="61dce-265">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="61dce-266">Kod, ProductContext 'in uzun süre çalışan bir örneğini bildirir.</span><span class="sxs-lookup"><span data-stu-id="61dce-266">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="61dce-267">Verileri sorgulamak ve veritabanına kaydetmek için ProductContext nesnesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="61dce-267">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="61dce-268">ProductContext örneğindeki Dispose () yöntemi, geçersiz kılınan OnClosing yönteminden çağırılır.</span><span class="sxs-lookup"><span data-stu-id="61dce-268">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="61dce-269">Kod açıklamaları kodun ne yaptığını hakkında ayrıntılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="61dce-269">The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="61dce-270">Windows Forms uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="61dce-270">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="61dce-271">Uygulamayı derleyin ve çalıştırın ve işlevleri test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="61dce-271">Compile and run the application and you can test out the functionality.</span></span>

    ![Kaydetmeden önce form 1](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="61dce-273">Mağaza oluşturulduktan sonra oluşturulan anahtarların ekranda gösterilmesi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="61dce-273">After saving the store generated keys are shown on the screen.</span></span>

    ![Kaydettikten sonra 1 formu](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="61dce-275">Code First kullandıysanız, sizin için bir **Winformyüzthefsample. ProductContext** veritabanının oluşturulduğunu da görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="61dce-275">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![Sunucu Nesne Gezgini](~/ef6/media/serverobjexplorer.png)
