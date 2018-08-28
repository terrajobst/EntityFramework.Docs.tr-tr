---
title: Varlıkları izlenecek yol - EF6 Self izleme
author: divega
ms.date: 2016-10-23
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 64ca9ae42df1a1c740131e254b8f80f67b2f9f97
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995427"
---
# <a name="self-tracking-entities-walkthrough"></a><span data-ttu-id="1c44a-102">Kendi kendine izleme varlıkları gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="1c44a-102">Self-Tracking Entities Walkthrough</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1c44a-103">Artık, self tracking varlıkları şablon kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="1c44a-103">We no longer recommend using the self-tracking-entities template.</span></span> <span data-ttu-id="1c44a-104">Bu yalnızca var olan uygulamaları desteklemek kullanılabilir olmaya devam edecek.</span><span class="sxs-lookup"><span data-stu-id="1c44a-104">It will only continue to be available to support existing applications.</span></span> <span data-ttu-id="1c44a-105">Uygulamanızın çalışma bağlantısı kesilmiş varlıklar grafikleriyle gerektiriyorsa, başka alternatifler gibi düşünün [izlenebilir varlıkları](http://trackableentities.github.io/), Self-Tracking-daha etkin bir şekilde tarafından geliştirilen varlıklara benzer bir teknoloji olan Topluluk veya alt düzey değişiklik API'leri izleme kullanarak özel kod yazma.</span><span class="sxs-lookup"><span data-stu-id="1c44a-105">If your application requires working with disconnected graphs of entities, consider other alternatives such as [Trackable Entities](http://trackableentities.github.io/), which is a technology similar to Self-Tracking-Entities that is more actively developed by the community, or writing custom code using the low-level change tracking APIs.</span></span>

<span data-ttu-id="1c44a-106">Bu izlenecek yol, bir Windows Communication Foundation (WCF) hizmeti bir varlık grafikte döndüren bir işlem kullanıma sunan bir senaryo gösterir.</span><span class="sxs-lookup"><span data-stu-id="1c44a-106">This walkthrough demonstrates the scenario in which a Windows Communication Foundation (WCF) service exposes an operation that returns an entity graph.</span></span> <span data-ttu-id="1c44a-107">Ardından, bir istemci uygulaması, bu grafik yönetir ve doğrular ve Entity Framework kullanarak bir veritabanına güncelleştirmeleri kaydeden bir hizmet işlemi için yapılan değişiklikleri gönderir.</span><span class="sxs-lookup"><span data-stu-id="1c44a-107">Next, a client application manipulates that graph and submits the modifications to a service operation that validates and saves the updates to a database using Entity Framework.</span></span>

<span data-ttu-id="1c44a-108">Bu izlenecek yolda tamamlamadan önce okuduğunuzdan emin olun [Self-Tracking varlıkları](index.md) sayfası.</span><span class="sxs-lookup"><span data-stu-id="1c44a-108">Before completing this walkthrough make sure you read the [Self-Tracking Entities](index.md) page.</span></span>

<span data-ttu-id="1c44a-109">Bu kılavuz, aşağıdaki eylemleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="1c44a-109">This walkthrough completes the following actions:</span></span>

-   <span data-ttu-id="1c44a-110">Erişmek için bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1c44a-110">Creates a database to access.</span></span>
-   <span data-ttu-id="1c44a-111">Modeli içeren bir sınıf kitaplığı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1c44a-111">Creates a class library that contains the model.</span></span>
-   <span data-ttu-id="1c44a-112">Takasları Self-Tracking varlık Oluşturucu şablon.</span><span class="sxs-lookup"><span data-stu-id="1c44a-112">Swaps to the Self-Tracking Entity Generator template.</span></span>
-   <span data-ttu-id="1c44a-113">Varlık sınıfları için ayrı bir proje taşır.</span><span class="sxs-lookup"><span data-stu-id="1c44a-113">Moves the entity classes to a separate project.</span></span>
-   <span data-ttu-id="1c44a-114">Sorgulamak ve varlıkları kaydetmek için operations sunan bir WCF Hizmeti oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1c44a-114">Creates a WCF service that exposes operations to query and save entities.</span></span>
-   <span data-ttu-id="1c44a-115">İstemci hizmeti kullanmak uygulamaları (konsolu ve WPF) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1c44a-115">Creates client applications (Console and WPF) that consume the service.</span></span>

<span data-ttu-id="1c44a-116">Veritabanı ilk Bu izlenecek yolda kullanacağız ancak aynı teknikleri eşit ilk modeli için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="1c44a-116">We'll use Database First in this walkthrough but the same techniques apply equally to Model First.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="1c44a-117">Ön koşullar</span><span class="sxs-lookup"><span data-stu-id="1c44a-117">Pre-Requisites</span></span>

<span data-ttu-id="1c44a-118">Bu izlenecek yolu tamamlamak için Visual Studio'nun yeni bir sürümü gerekir.</span><span class="sxs-lookup"><span data-stu-id="1c44a-118">To complete this walkthrough you will need a recent version of Visual Studio.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="1c44a-119">Bir veritabanı oluşturun</span><span class="sxs-lookup"><span data-stu-id="1c44a-119">Create a Database</span></span>

<span data-ttu-id="1c44a-120">Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösterir:</span><span class="sxs-lookup"><span data-stu-id="1c44a-120">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="1c44a-121">Visual Studio 2012 kullanıyorsanız, bir LocalDB veritabanına oluşturmayı.</span><span class="sxs-lookup"><span data-stu-id="1c44a-121">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="1c44a-122">Visual Studio 2010 kullanıyorsanız, SQL Express veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="1c44a-122">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="1c44a-123">Yeni bir ubuntu ve veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1c44a-123">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="1c44a-124">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="1c44a-124">Open Visual Studio</span></span>
-   <span data-ttu-id="1c44a-125">**Görünüm -&gt; Sunucu Gezgini**</span><span class="sxs-lookup"><span data-stu-id="1c44a-125">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="1c44a-126">Sağ tıklayın **veri bağlantıları -&gt; bağlantı ekle...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-126">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="1c44a-127">Sunucu gezgininden veritabanına bağlamadıysanız seçmeniz gerekir önce **Microsoft SQL Server** veri kaynağı</span><span class="sxs-lookup"><span data-stu-id="1c44a-127">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="1c44a-128">LocalDB veya hangisinin bağlı olarak yüklediğiniz SQL Express için Bağlan</span><span class="sxs-lookup"><span data-stu-id="1c44a-128">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="1c44a-129">Girin **STESample** veritabanı adı</span><span class="sxs-lookup"><span data-stu-id="1c44a-129">Enter **STESample** as the database name</span></span>
-   <span data-ttu-id="1c44a-130">Seçin **Tamam** ve bir yeni bir veritabanı oluşturmak istiyorsanız istenir **Evet**</span><span class="sxs-lookup"><span data-stu-id="1c44a-130">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="1c44a-131">Yeni veritabanı şimdi sunucu Gezgini'nde görünür.</span><span class="sxs-lookup"><span data-stu-id="1c44a-131">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="1c44a-132">Visual Studio 2012 kullanıyorsanız</span><span class="sxs-lookup"><span data-stu-id="1c44a-132">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="1c44a-133">Sunucu Gezgini veritabanı üzerinde sağ tıklayıp **yeni sorgu**</span><span class="sxs-lookup"><span data-stu-id="1c44a-133">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="1c44a-134">Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **Yürüt**</span><span class="sxs-lookup"><span data-stu-id="1c44a-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="1c44a-135">Visual Studio 2010 kullanıyorsanız</span><span class="sxs-lookup"><span data-stu-id="1c44a-135">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="1c44a-136">Seçin **veri -&gt; Transact - SQL Düzenleyicisi&gt; yeni bağlantı...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-136">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="1c44a-137">Girin **.\\ SQLEXPRESS** tıklayın ve sunucu adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1c44a-137">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="1c44a-138">Seçin **STESample** veritabanı açılır menüden aşağı sorgu Düzenleyicisi'ni üstünde</span><span class="sxs-lookup"><span data-stu-id="1c44a-138">Select the **STESample** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="1c44a-139">Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **SQL Yürüt**</span><span class="sxs-lookup"><span data-stu-id="1c44a-139">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a><span data-ttu-id="1c44a-140">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="1c44a-140">Create the Model</span></span>

<span data-ttu-id="1c44a-141">Öncelikle, bir proje modeli yerleştirmek için ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="1c44a-141">First up, we need a project to put the model in.</span></span>

-   <span data-ttu-id="1c44a-142">**Dosya -&gt; yeni -&gt; proje...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-142">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="1c44a-143">Seçin **Visual C\#**  sol bölmeden ardından **sınıf kitaplığı**</span><span class="sxs-lookup"><span data-stu-id="1c44a-143">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="1c44a-144">Girin **STESample** tıklayın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1c44a-144">Enter **STESample** as the name and click **OK**</span></span>

<span data-ttu-id="1c44a-145">Basit bir model bizim veritabanına erişmek için EF Tasarımcısı'nda şimdi oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="1c44a-145">Now we'll create a simple model in the EF Designer to access our database:</span></span>

-   <span data-ttu-id="1c44a-146">**Takım projesi -&gt; Yeni Öğe Ekle...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-146">**Project -&gt; Add New Item...**</span></span>
-   <span data-ttu-id="1c44a-147">Seçin **veri** sol bölmeden ardından **ADO.NET varlık veri modeli**</span><span class="sxs-lookup"><span data-stu-id="1c44a-147">Select **Data** from the left pane and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="1c44a-148">Girin **BloggingModel** tıklayın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1c44a-148">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="1c44a-149">Seçin **veritabanından Oluştur** tıklatıp **İleri**</span><span class="sxs-lookup"><span data-stu-id="1c44a-149">Select **Generate from database** and click **Next**</span></span>
-   <span data-ttu-id="1c44a-150">Önceki bölümde oluşturduğunuz veritabanı için bağlantı bilgilerini girin</span><span class="sxs-lookup"><span data-stu-id="1c44a-150">Enter the connection information for the database that you created in the previous section</span></span>
-   <span data-ttu-id="1c44a-151">Girin **BloggingContext** tıklayın ve bağlantı dizesi adı olarak **İleri**</span><span class="sxs-lookup"><span data-stu-id="1c44a-151">Enter **BloggingContext** as the name for the connection string and click **Next**</span></span>
-   <span data-ttu-id="1c44a-152">Yanındaki kutuyu işaretleyin **tabloları** tıklatıp **son**</span><span class="sxs-lookup"><span data-stu-id="1c44a-152">Check the box next to **Tables** and click **Finish**</span></span>

## <a name="swap-to-ste-code-generation"></a><span data-ttu-id="1c44a-153">Ste'leri birleştirme kod oluşturma için takas etme</span><span class="sxs-lookup"><span data-stu-id="1c44a-153">Swap to STE Code Generation</span></span>

<span data-ttu-id="1c44a-154">Artık varsayılan kod oluşturma ve değiştirme Self-Tracking varlıklara devre dışı bırakmak ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="1c44a-154">Now we need to disable the default code generation and swap to Self-Tracking Entities.</span></span>

### <a name="if-you-are-using-visual-studio-2012"></a><span data-ttu-id="1c44a-155">Visual Studio 2012 kullanıyorsanız</span><span class="sxs-lookup"><span data-stu-id="1c44a-155">If you are using Visual Studio 2012</span></span>

-   <span data-ttu-id="1c44a-156">Genişletin **BloggingModel.edmx** içinde **Çözüm Gezgini** ve silme **BloggingModel.tt** ve **BloggingModel.Context.tt** 
     *Bu durum, varsayılan kod oluşturma devre dışı bırakır*</span><span class="sxs-lookup"><span data-stu-id="1c44a-156">Expand **BloggingModel.edmx** in **Solution Explorer** and delete the **BloggingModel.tt** and **BloggingModel.Context.tt**
*This will disable the default code generation*</span></span>
-   <span data-ttu-id="1c44a-157">EF Designer seçin ve yüzey üzerinde boş bir alana sağ **kod oluşturma öğesi Ekle...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-157">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="1c44a-158">Seçin **çevrimiçi** arayın ve sol bölmedeki **Pıştırarak Oluşturucusu**</span><span class="sxs-lookup"><span data-stu-id="1c44a-158">Select **Online** from the left pane and search for **STE Generator**</span></span>
-   <span data-ttu-id="1c44a-159">Seçin **Pıştırarak Oluşturucu c\#**  şablon girin **STETemplate** tıklayın ve adı olarak **Ekle**</span><span class="sxs-lookup"><span data-stu-id="1c44a-159">Select the **STE Generator for C\#** template, enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="1c44a-160">**STETemplate.tt** ve **STETemplate.Context.tt** dosyaları BloggingModel.edmx dosya altında iç içe geçmiş eklendi</span><span class="sxs-lookup"><span data-stu-id="1c44a-160">The **STETemplate.tt** and **STETemplate.Context.tt** files are added nested under the BloggingModel.edmx file</span></span>

### <a name="if-you-are-using-visual-studio-2010"></a><span data-ttu-id="1c44a-161">Visual Studio 2010 kullanıyorsanız</span><span class="sxs-lookup"><span data-stu-id="1c44a-161">If you are using Visual Studio 2010</span></span>

-   <span data-ttu-id="1c44a-162">EF Designer seçin ve yüzey üzerinde boş bir alana sağ **kod oluşturma öğesi Ekle...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-162">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="1c44a-163">Seçin **kod** sol bölmeden ardından **ADO.NET Self-Tracking varlık Oluşturucu**</span><span class="sxs-lookup"><span data-stu-id="1c44a-163">Select **Code** from the left pane and then **ADO.NET Self-Tracking Entity Generator**</span></span>
-   <span data-ttu-id="1c44a-164">Girin **STETemplate** tıklayın ve adı olarak **Ekle**</span><span class="sxs-lookup"><span data-stu-id="1c44a-164">Enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="1c44a-165">**STETemplate.tt** ve **STETemplate.Context.tt** dosyalarını doğrudan projenize eklendi</span><span class="sxs-lookup"><span data-stu-id="1c44a-165">The **STETemplate.tt** and **STETemplate.Context.tt** files are added directly to your project</span></span>

## <a name="move-entity-types-into-separate-project"></a><span data-ttu-id="1c44a-166">Varlık türleri ayrı projeye Taşı</span><span class="sxs-lookup"><span data-stu-id="1c44a-166">Move Entity Types into Separate Project</span></span>

<span data-ttu-id="1c44a-167">Self-Tracking varlıkları kullanmak için istemci uygulamamız bizim modelden üretilen varlık sınıflarının erişimi gerekir.</span><span class="sxs-lookup"><span data-stu-id="1c44a-167">To use Self-Tracking Entities our client application needs access to the entity classes generated from our model.</span></span> <span data-ttu-id="1c44a-168">İstemci uygulamasına modelin tamamını göstermek istemediğiniz için varlık sınıflarını ayrı bir projeye taşımak için ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="1c44a-168">Because we don't want to expose the whole model to the client application we're going to move the entity classes into a separate project.</span></span>

<span data-ttu-id="1c44a-169">İlk adım, varolan projede, varlık sınıfları oluşturma önlemektir:</span><span class="sxs-lookup"><span data-stu-id="1c44a-169">The first step is to stop generating entity classes in the existing project:</span></span>

-   <span data-ttu-id="1c44a-170">Sağ **STETemplate.tt** içinde **Çözüm Gezgini** seçip **özellikleri**</span><span class="sxs-lookup"><span data-stu-id="1c44a-170">Right-click on **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="1c44a-171">İçinde **özellikleri** penceresi Temizle **TextTemplatingFileGenerator** gelen **CustomTool** özelliği</span><span class="sxs-lookup"><span data-stu-id="1c44a-171">In the **Properties** window clear **TextTemplatingFileGenerator** from the **CustomTool** property</span></span>
-   <span data-ttu-id="1c44a-172">Genişletin **STETemplate.tt** içinde **Çözüm Gezgini** ve bunun altında iç içe geçmiş tüm dosyaları silin</span><span class="sxs-lookup"><span data-stu-id="1c44a-172">Expand **STETemplate.tt** in **Solution Explorer** and delete all files nested under it</span></span>

<span data-ttu-id="1c44a-173">Ardından, yeni bir proje ekleyin ve varlık sınıfları oluşturmak için kullanacağız</span><span class="sxs-lookup"><span data-stu-id="1c44a-173">Next, we are going to add a new project and generate the entity classes in it</span></span>

-   <span data-ttu-id="1c44a-174">**Dosya -&gt; Ekle -&gt; proje...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-174">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="1c44a-175">Seçin **Visual C\#**  sol bölmeden ardından **sınıf kitaplığı**</span><span class="sxs-lookup"><span data-stu-id="1c44a-175">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="1c44a-176">Girin **STESample.Entities** tıklayın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1c44a-176">Enter **STESample.Entities** as the name and click **OK**</span></span>
-   <span data-ttu-id="1c44a-177">**Takım projesi -&gt; varolan öğeyi Ekle...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-177">**Project -&gt; Add Existing Item...**</span></span>
-   <span data-ttu-id="1c44a-178">Gidin **STESample** proje klasörü</span><span class="sxs-lookup"><span data-stu-id="1c44a-178">Navigate to the **STESample** project folder</span></span>
-   <span data-ttu-id="1c44a-179">Görüntülemek için seçin **tüm dosyalar (\*.\*)**</span><span class="sxs-lookup"><span data-stu-id="1c44a-179">Select to view **All Files (\*.\*)**</span></span>
-   <span data-ttu-id="1c44a-180">Seçin **STETemplate.tt** dosyası</span><span class="sxs-lookup"><span data-stu-id="1c44a-180">Select the **STETemplate.tt** file</span></span>
-   <span data-ttu-id="1c44a-181">Yanındaki aşağı açılan oka tıklayın **Ekle** düğmesini tıklatın ve seçin **bağlantı olarak Ekle**</span><span class="sxs-lookup"><span data-stu-id="1c44a-181">Click on the drop down arrow next to the **Add** button and select **Add As Link**</span></span>

    ![AddLinkedTemplate](~/ef6/media/addlinkedtemplate.png)

<span data-ttu-id="1c44a-183">Bunu varlık sınıflarının ad bağlamı olarak oluşturulmasını emin olmak için dağıtacağız.</span><span class="sxs-lookup"><span data-stu-id="1c44a-183">We're also going to make sure the entity classes get generated in the same namespace as the context.</span></span> <span data-ttu-id="1c44a-184">Bu, yalnızca using deyimlerini eklemek için uygulama genelinde ihtiyacımız sayısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="1c44a-184">This just reduces the number of using statements we need to add throughout our application.</span></span>

-   <span data-ttu-id="1c44a-185">Bağlantılı üzerinde sağ **STETemplate.tt** içinde **Çözüm Gezgini** seçip **özellikleri**</span><span class="sxs-lookup"><span data-stu-id="1c44a-185">Right-click on the linked **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="1c44a-186">İçinde **özellikleri** penceresi kümesi **özel aracı Namespace** için **STESample**</span><span class="sxs-lookup"><span data-stu-id="1c44a-186">In the **Properties** window set **Custom Tool Namespace** to **STESample**</span></span>

<span data-ttu-id="1c44a-187">Ste'leri birleştirme şablon tarafından oluşturulan kodu başvuru gerekir **System.Runtime.Serialization** derlemek için.</span><span class="sxs-lookup"><span data-stu-id="1c44a-187">The code generated by the STE template will need a reference to **System.Runtime.Serialization** in order to compile.</span></span> <span data-ttu-id="1c44a-188">Bu kitaplık için WCF gerekli **DataContract** ve **DataMember** seri hale getirilebilir varlık türleri üzerinde kullanılan öznitelikler.</span><span class="sxs-lookup"><span data-stu-id="1c44a-188">This library is needed for the WCF **DataContract** and **DataMember** attributes that are used on the serializable entity types.</span></span>

-   <span data-ttu-id="1c44a-189">Sağ tıklayın **STESample.Entities** projesi **Çözüm Gezgini** seçip **Başvuru Ekle...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-189">Right click on the **STESample.Entities** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="1c44a-190">Visual Studio 2012'de - yanındaki kutuyu işaretleyin **System.Runtime.Serialization** tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1c44a-190">In Visual Studio 2012 - check the box next to **System.Runtime.Serialization** and click **OK**</span></span>
    -   <span data-ttu-id="1c44a-191">Visual Studio 2010'da - seçin **System.Runtime.Serialization** tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1c44a-191">In Visual Studio 2010 - select **System.Runtime.Serialization** and click **OK**</span></span>

<span data-ttu-id="1c44a-192">Son olarak, bizim bağlamda projeyle varlık türleri başvuru gerekir.</span><span class="sxs-lookup"><span data-stu-id="1c44a-192">Finally, the project with our context in it will need a reference to the entity types.</span></span>

-   <span data-ttu-id="1c44a-193">Sağ tıklayın **STESample** projesi **Çözüm Gezgini** seçip **Başvuru Ekle...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-193">Right click on the **STESample** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="1c44a-194">Visual Studio 2012'de - seçin **çözüm** yanındaki onay kutusunu sol bölmeden **STESample.Entities** tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1c44a-194">In Visual Studio 2012 - select **Solution** from the left pane, check the box next to **STESample.Entities** and click **OK**</span></span>
    -   <span data-ttu-id="1c44a-195">Visual Studio 2010'da - seçin **projeleri** sekmesinde **STESample.Entities** tıklatıp **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1c44a-195">In Visual Studio 2010 - select the **Projects** tab, select **STESample.Entities** and click **OK**</span></span>

>[!NOTE]
> <span data-ttu-id="1c44a-196">Varlık türleri ayrı bir projeye taşımak için başka bir seçenek, varsayılan konumundan bağlama yerine şablon dosyası taşımaktır.</span><span class="sxs-lookup"><span data-stu-id="1c44a-196">Another option for moving the entity types to a separate project is to move the template file, rather than linking it from its default location.</span></span> <span data-ttu-id="1c44a-197">Bunu yaparsanız, güncelleştirmeniz gerekecektir **InputFile** edmx dosyasının göreli yolunu sağlamak için şablonu değişken (Bu örnekte olacaktır **.. \\BloggingModel.edmx**).</span><span class="sxs-lookup"><span data-stu-id="1c44a-197">If you do this, you will need to update the **inputFile** variable in the template to provide the relative path to the edmx file (in this example that would be **..\\BloggingModel.edmx**).</span></span>

## <a name="create-a-wcf-service"></a><span data-ttu-id="1c44a-198">Bir WCF hizmeti oluşturma</span><span class="sxs-lookup"><span data-stu-id="1c44a-198">Create a WCF Service</span></span>

<span data-ttu-id="1c44a-199">Verilerimizi kullanıma sunmak için bir WCF hizmeti ekleme zamanı artık projesi oluşturarak başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="1c44a-199">Now it's time to add a WCF Service to expose our data, we'll start by creating the project.</span></span>

-   <span data-ttu-id="1c44a-200">**Dosya -&gt; Ekle -&gt; proje...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-200">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="1c44a-201">Seçin **Visual C\#**  sol bölmeden ardından **WCF hizmeti uygulaması**</span><span class="sxs-lookup"><span data-stu-id="1c44a-201">Select **Visual C\#** from the left pane and then **WCF Service Application**</span></span>
-   <span data-ttu-id="1c44a-202">Girin **STESample.Service** tıklayın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1c44a-202">Enter **STESample.Service** as the name and click **OK**</span></span>
-   <span data-ttu-id="1c44a-203">Bir başvuru ekleyin **System.Data.Entity** derleme</span><span class="sxs-lookup"><span data-stu-id="1c44a-203">Add a reference to the **System.Data.Entity** assembly</span></span>
-   <span data-ttu-id="1c44a-204">Bir başvuru ekleyin **STESample** ve **STESample.Entities** projeleri</span><span class="sxs-lookup"><span data-stu-id="1c44a-204">Add a reference to the **STESample** and **STESample.Entities** projects</span></span>

<span data-ttu-id="1c44a-205">Çalışma zamanında bulunur, böylece bu projede EF bağlantı dizesini kopyalayın oluşturmamız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1c44a-205">We need to copy the EF connection string to this project so that it is found at runtime.</span></span>

-   <span data-ttu-id="1c44a-206">Açık **App.Config** dosya için ** STESample ** proje ve kopyalama **connectionStrings** öğesi</span><span class="sxs-lookup"><span data-stu-id="1c44a-206">Open the **App.Config** file for the **STESample **project and copy the **connectionStrings** element</span></span>
-   <span data-ttu-id="1c44a-207">Yapıştırma **connectionStrings** öğesi alt öğesi olarak **yapılandırma** öğesinin **Web.Config** dosyası **STESample.Service** proje</span><span class="sxs-lookup"><span data-stu-id="1c44a-207">Paste the **connectionStrings** element as a child element of the **configuration** element of the **Web.Config** file in the **STESample.Service** project</span></span>

<span data-ttu-id="1c44a-208">Artık gerçek hizmeti uygulama zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="1c44a-208">Now it's time to implement the actual service.</span></span>

-   <span data-ttu-id="1c44a-209">Açık **Iservice1.cs** ve içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1c44a-209">Open **IService1.cs** and replace the contents with the following code</span></span>

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   <span data-ttu-id="1c44a-210">Açık **service1.svc'yi** ve içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1c44a-210">Open **Service1.svc** and replace the contents with the following code</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a><span data-ttu-id="1c44a-211">Konsol uygulamasından hizmetini kullanma</span><span class="sxs-lookup"><span data-stu-id="1c44a-211">Consume the Service from a Console Application</span></span>

<span data-ttu-id="1c44a-212">Hizmetimiz kullanan bir konsol uygulaması oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="1c44a-212">Let's create a console application that uses our service.</span></span>

-   <span data-ttu-id="1c44a-213">**Dosya -&gt; yeni -&gt; proje...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-213">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="1c44a-214">Seçin **Visual C\#**  sol bölmeden ardından **konsol uygulaması**</span><span class="sxs-lookup"><span data-stu-id="1c44a-214">Select **Visual C\#** from the left pane and then **Console Application**</span></span>
-   <span data-ttu-id="1c44a-215">Girin **STESample.ConsoleTest** tıklayın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1c44a-215">Enter **STESample.ConsoleTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="1c44a-216">Bir başvuru ekleyin **STESample.Entities** proje</span><span class="sxs-lookup"><span data-stu-id="1c44a-216">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="1c44a-217">WCF hizmetimiz bir hizmet başvurusu ihtiyacımız</span><span class="sxs-lookup"><span data-stu-id="1c44a-217">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="1c44a-218">Sağ **STESample.ConsoleTest** projesi **Çözüm Gezgini** seçip **hizmet Başvurusu Ekle...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-218">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="1c44a-219">Tıklayın **keşfedin**</span><span class="sxs-lookup"><span data-stu-id="1c44a-219">Click **Discover**</span></span>
-   <span data-ttu-id="1c44a-220">Girin **BloggingService** tıklayın ve ad alanı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1c44a-220">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="1c44a-221">Şimdi biz hizmeti kullanmak için bazı kod yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c44a-221">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="1c44a-222">Açık **Program.cs** ve içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1c44a-222">Open **Program.cs** and replace the contents with the following code.</span></span>

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

<span data-ttu-id="1c44a-223">Şimdi nasıl çalıştığını görmek için uygulamayı çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c44a-223">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="1c44a-224">Sağ **STESample.ConsoleTest** projesi **Çözüm Gezgini** seçip **hata ayıklama -&gt; yeni örnek Başlat**</span><span class="sxs-lookup"><span data-stu-id="1c44a-224">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>

<span data-ttu-id="1c44a-225">Uygulama yürütüldüğünde, aşağıdaki çıktıyı görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="1c44a-225">You'll see the following output when the application executes.</span></span>

```
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a><span data-ttu-id="1c44a-226">Bir WPF uygulamasından hizmetini kullanma</span><span class="sxs-lookup"><span data-stu-id="1c44a-226">Consume the Service from a WPF Application</span></span>

<span data-ttu-id="1c44a-227">Hizmetimiz kullanan bir WPF uygulaması oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="1c44a-227">Let's create a WPF application that uses our service.</span></span>

-   <span data-ttu-id="1c44a-228">**Dosya -&gt; yeni -&gt; proje...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-228">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="1c44a-229">Seçin **Visual C\#**  sol bölmeden ardından **WPF uygulaması**</span><span class="sxs-lookup"><span data-stu-id="1c44a-229">Select **Visual C\#** from the left pane and then **WPF Application**</span></span>
-   <span data-ttu-id="1c44a-230">Girin **STESample.WPFTest** tıklayın ve adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1c44a-230">Enter **STESample.WPFTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="1c44a-231">Bir başvuru ekleyin **STESample.Entities** proje</span><span class="sxs-lookup"><span data-stu-id="1c44a-231">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="1c44a-232">WCF hizmetimiz bir hizmet başvurusu ihtiyacımız</span><span class="sxs-lookup"><span data-stu-id="1c44a-232">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="1c44a-233">Sağ **STESample.WPFTest** projesi **Çözüm Gezgini** seçip **hizmet Başvurusu Ekle...**</span><span class="sxs-lookup"><span data-stu-id="1c44a-233">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="1c44a-234">Tıklayın **keşfedin**</span><span class="sxs-lookup"><span data-stu-id="1c44a-234">Click **Discover**</span></span>
-   <span data-ttu-id="1c44a-235">Girin **BloggingService** tıklayın ve ad alanı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1c44a-235">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="1c44a-236">Şimdi biz hizmeti kullanmak için bazı kod yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c44a-236">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="1c44a-237">Açık **MainWindow.xaml** ve içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1c44a-237">Open **MainWindow.xaml** and replace the contents with the following code.</span></span>

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   <span data-ttu-id="1c44a-238">Arka plan kod için MainWindow açın (**MainWindow.xaml.cs**) ve içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1c44a-238">Open the code behind for MainWindow (**MainWindow.xaml.cs**) and replace the contents with the following code</span></span>

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

<span data-ttu-id="1c44a-239">Şimdi nasıl çalıştığını görmek için uygulamayı çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1c44a-239">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="1c44a-240">Sağ **STESample.WPFTest** projesi **Çözüm Gezgini** seçip **hata ayıklama -&gt; yeni örnek Başlat**</span><span class="sxs-lookup"><span data-stu-id="1c44a-240">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>
-   <span data-ttu-id="1c44a-241">Ekran'ı kullanarak verileri işlemek ve yoluyla kullanarak hizmet kaydetmeden **Kaydet** düğmesi</span><span class="sxs-lookup"><span data-stu-id="1c44a-241">You can manipulate the data using the screen and save it via the service using the **Save** button</span></span>

![WPF](~/ef6/media/wpf.png)
