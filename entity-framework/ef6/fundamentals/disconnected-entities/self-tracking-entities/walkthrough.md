---
title: Kendi kendine Izlenen varlıklar Izlenecek yol-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 9bd644461f50a7eff1006cb8866ca9a3b08b6b8d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419535"
---
# <a name="self-tracking-entities-walkthrough"></a><span data-ttu-id="f4dba-102">Kendi kendine Izlenen varlıkları gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="f4dba-102">Self-Tracking Entities Walkthrough</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f4dba-103">Artık kendi kendine izleme varlıkları şablonunu kullanmanızı önermiyoruz.</span><span class="sxs-lookup"><span data-stu-id="f4dba-103">We no longer recommend using the self-tracking-entities template.</span></span> <span data-ttu-id="f4dba-104">Yalnızca var olan uygulamaları desteklemek için kullanılabilir olmaya devam edecektir.</span><span class="sxs-lookup"><span data-stu-id="f4dba-104">It will only continue to be available to support existing applications.</span></span> <span data-ttu-id="f4dba-105">Uygulamanız, bağlantılı olmayan grafik grafiklerle çalışmayı gerektiriyorsa, bu, topluluk tarafından daha etkin bir şekilde geliştirilmiş olan ve alt düzey değişiklik izleme API 'Leri kullanılarak özel kod yazma gibi, kendini Izlemeye benzer bir teknoloji olan, [izleyicileri oluşturan varlıklar](https://trackableentities.github.io/)gibi diğer alternatifleri göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="f4dba-105">If your application requires working with disconnected graphs of entities, consider other alternatives such as [Trackable Entities](https://trackableentities.github.io/), which is a technology similar to Self-Tracking-Entities that is more actively developed by the community, or writing custom code using the low-level change tracking APIs.</span></span>

<span data-ttu-id="f4dba-106">Bu izlenecek yol, bir Windows Communication Foundation (WCF) hizmetinin bir varlık grafiği döndüren bir işlemi kullanıma sunduğunu gösteren senaryoyu gösterir.</span><span class="sxs-lookup"><span data-stu-id="f4dba-106">This walkthrough demonstrates the scenario in which a Windows Communication Foundation (WCF) service exposes an operation that returns an entity graph.</span></span> <span data-ttu-id="f4dba-107">Daha sonra, bir istemci uygulaması bu grafiği yönetir ve Entity Framework kullanarak bir veritabanına güncelleştirmeleri doğrulayan ve kaydeden bir hizmet işlemine yapılan değişiklikleri gönderir.</span><span class="sxs-lookup"><span data-stu-id="f4dba-107">Next, a client application manipulates that graph and submits the modifications to a service operation that validates and saves the updates to a database using Entity Framework.</span></span>

<span data-ttu-id="f4dba-108">Bu yönergeyi tamamlamadan önce, [kendi kendine Izleme varlıkları](index.md) sayfasını okuduğunuzdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="f4dba-108">Before completing this walkthrough make sure you read the [Self-Tracking Entities](index.md) page.</span></span>

<span data-ttu-id="f4dba-109">Bu izlenecek yol aşağıdaki eylemleri tamamlar:</span><span class="sxs-lookup"><span data-stu-id="f4dba-109">This walkthrough completes the following actions:</span></span>

-   <span data-ttu-id="f4dba-110">Erişmek için bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4dba-110">Creates a database to access.</span></span>
-   <span data-ttu-id="f4dba-111">Modeli içeren bir sınıf kitaplığı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4dba-111">Creates a class library that contains the model.</span></span>
-   <span data-ttu-id="f4dba-112">Kendi kendini Izleyen varlık Oluşturucu şablonunu değiştirir.</span><span class="sxs-lookup"><span data-stu-id="f4dba-112">Swaps to the Self-Tracking Entity Generator template.</span></span>
-   <span data-ttu-id="f4dba-113">Varlık sınıflarını ayrı bir projeye kaydırır.</span><span class="sxs-lookup"><span data-stu-id="f4dba-113">Moves the entity classes to a separate project.</span></span>
-   <span data-ttu-id="f4dba-114">Varlıkları sorgulama ve kaydetme işlemlerini kullanıma sunan bir WCF hizmeti oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4dba-114">Creates a WCF service that exposes operations to query and save entities.</span></span>
-   <span data-ttu-id="f4dba-115">Hizmeti kullanan istemci uygulamaları (konsol ve WPF) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4dba-115">Creates client applications (Console and WPF) that consume the service.</span></span>

<span data-ttu-id="f4dba-116">Bu izlenecek yolda Database First kullanacağız, ancak aynı teknikler Model First için de aynı şekilde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="f4dba-116">We'll use Database First in this walkthrough but the same techniques apply equally to Model First.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="f4dba-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="f4dba-117">Pre-Requisites</span></span>

<span data-ttu-id="f4dba-118">Bu izlenecek yolu tamamlamak için, Visual Studio 'nun yeni bir sürümüne ihtiyacınız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="f4dba-118">To complete this walkthrough you will need a recent version of Visual Studio.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="f4dba-119">Veritabanı Oluşturma</span><span class="sxs-lookup"><span data-stu-id="f4dba-119">Create a Database</span></span>

<span data-ttu-id="f4dba-120">Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösteren bir sürümdür:</span><span class="sxs-lookup"><span data-stu-id="f4dba-120">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="f4dba-121">Visual Studio 2012 kullanıyorsanız, LocalDB veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="f4dba-121">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="f4dba-122">Visual Studio 2010 kullanıyorsanız, bir SQL Express veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="f4dba-122">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="f4dba-123">Şimdi veritabanını oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="f4dba-123">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="f4dba-124">Visual Studio’yu açın</span><span class="sxs-lookup"><span data-stu-id="f4dba-124">Open Visual Studio</span></span>
-   <span data-ttu-id="f4dba-125">**&gt; Sunucu Gezgini görüntüle**</span><span class="sxs-lookup"><span data-stu-id="f4dba-125">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="f4dba-126">Veri bağlantıları ' na sağ tıklayın **&gt; bağlantı ekle...**</span><span class="sxs-lookup"><span data-stu-id="f4dba-126">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="f4dba-127">Sunucu Gezgini bir veritabanına bağlı değilseniz, veri kaynağı olarak **Microsoft SQL Server** seçmeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="f4dba-127">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="f4dba-128">Hangi hangisinin yüklü olduğuna bağlı olarak, LocalDB veya SQL Express 'e bağlanın</span><span class="sxs-lookup"><span data-stu-id="f4dba-128">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="f4dba-129">Veritabanı adı olarak **Stesample** girin</span><span class="sxs-lookup"><span data-stu-id="f4dba-129">Enter **STESample** as the database name</span></span>
-   <span data-ttu-id="f4dba-130">**Tamam** ' ı seçtiğinizde, yeni bir veritabanı oluşturmak isteyip istemediğiniz sorulur, **Evet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="f4dba-130">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="f4dba-131">Yeni veritabanı artık Sunucu Gezgini görüntülenir</span><span class="sxs-lookup"><span data-stu-id="f4dba-131">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="f4dba-132">Visual Studio 2012 kullanıyorsanız</span><span class="sxs-lookup"><span data-stu-id="f4dba-132">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="f4dba-133">Sunucu Gezgini veritabanında veritabanına sağ tıklayın ve **Yeni sorgu** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="f4dba-133">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="f4dba-134">Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **Yürüt** ' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="f4dba-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="f4dba-135">Visual Studio 2010 kullanıyorsanız</span><span class="sxs-lookup"><span data-stu-id="f4dba-135">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="f4dba-136">**Veri&gt; Transact SQL Düzenleyicisi-&gt; yeni sorgu bağlantısı ' nı seçin...**</span><span class="sxs-lookup"><span data-stu-id="f4dba-136">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="f4dba-137">Sunucu adı olarak **.\\SQLExpress** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="f4dba-137">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="f4dba-138">Sorgu Düzenleyicisi 'nin en üstündeki açılan listeden **Stesample** veritabanını seçin</span><span class="sxs-lookup"><span data-stu-id="f4dba-138">Select the **STESample** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="f4dba-139">Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **SQL 'ı Yürüt** ' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="f4dba-139">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

## <a name="create-the-model"></a><span data-ttu-id="f4dba-140">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="f4dba-140">Create the Model</span></span>

<span data-ttu-id="f4dba-141">İlk olarak, modeli içine koyabileceğiniz bir proje gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="f4dba-141">First up, we need a project to put the model in.</span></span>

-   <span data-ttu-id="f4dba-142">**Dosya-&gt; yeni&gt; projesi...**</span><span class="sxs-lookup"><span data-stu-id="f4dba-142">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="f4dba-143">Sol bölmeden ve sonra **sınıf kitaplığı** 'Ndan **Visual C\#** seçin</span><span class="sxs-lookup"><span data-stu-id="f4dba-143">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="f4dba-144">Ad olarak **Stesample** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="f4dba-144">Enter **STESample** as the name and click **OK**</span></span>

<span data-ttu-id="f4dba-145">Şimdi, veritabanımıza erişmek için EF tasarımcısında basit bir model oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="f4dba-145">Now we'll create a simple model in the EF Designer to access our database:</span></span>

-   <span data-ttu-id="f4dba-146">**Proje-&gt; yeni öğe Ekle...**</span><span class="sxs-lookup"><span data-stu-id="f4dba-146">**Project -&gt; Add New Item...**</span></span>
-   <span data-ttu-id="f4dba-147">Sol bölmedeki **verileri** seçin ve ardından **ADO.net varlık veri modeli**</span><span class="sxs-lookup"><span data-stu-id="f4dba-147">Select **Data** from the left pane and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="f4dba-148">Ad olarak **BloggingModel** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="f4dba-148">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="f4dba-149">**Veritabanından oluştur** ' u seçin ve **İleri** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f4dba-149">Select **Generate from database** and click **Next**</span></span>
-   <span data-ttu-id="f4dba-150">Önceki bölümde oluşturduğunuz veritabanı için bağlantı bilgilerini girin</span><span class="sxs-lookup"><span data-stu-id="f4dba-150">Enter the connection information for the database that you created in the previous section</span></span>
-   <span data-ttu-id="f4dba-151">Bağlantı dizesinin adı olarak **BloggingContext** girin ve **İleri** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f4dba-151">Enter **BloggingContext** as the name for the connection string and click **Next**</span></span>
-   <span data-ttu-id="f4dba-152">**Tablolar** ' ın yanındaki kutuyu Işaretleyin ve **son** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f4dba-152">Check the box next to **Tables** and click **Finish**</span></span>

## <a name="swap-to-ste-code-generation"></a><span data-ttu-id="f4dba-153">STE kod oluşturmaya değiştirme</span><span class="sxs-lookup"><span data-stu-id="f4dba-153">Swap to STE Code Generation</span></span>

<span data-ttu-id="f4dba-154">Şimdi varsayılan kod oluşturmayı devre dışı bırakmaktan ve kendini Izlemeye yönelik olarak takas etmemiz gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="f4dba-154">Now we need to disable the default code generation and swap to Self-Tracking Entities.</span></span>

### <a name="if-you-are-using-visual-studio-2012"></a><span data-ttu-id="f4dba-155">Visual Studio 2012 kullanıyorsanız</span><span class="sxs-lookup"><span data-stu-id="f4dba-155">If you are using Visual Studio 2012</span></span>

-   <span data-ttu-id="f4dba-156">**Çözüm Gezgini** 'de **BloggingModel. edmx** ' i genişletin ve **BloggingModel.tt** ve **BloggingModel.Context.tt**
    silin. *Bu, varsayılan kod üretimini devre dışı bırakır*</span><span class="sxs-lookup"><span data-stu-id="f4dba-156">Expand **BloggingModel.edmx** in **Solution Explorer** and delete the **BloggingModel.tt** and **BloggingModel.Context.tt**
*This will disable the default code generation*</span></span>
-   <span data-ttu-id="f4dba-157">EF Designer yüzeyinde boş bir alana sağ tıklayın ve **kod oluşturma öğesi Ekle...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="f4dba-157">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="f4dba-158">Sol bölmeden **çevrimiçi** ' i seçin ve **Ste Generator** araması yapın</span><span class="sxs-lookup"><span data-stu-id="f4dba-158">Select **Online** from the left pane and search for **STE Generator**</span></span>
-   <span data-ttu-id="f4dba-159">**C\#şablonu Için Ste Generator** ' yı seçin, ad olarak **Stetemplate** girin ve **Ekle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f4dba-159">Select the **STE Generator for C\#** template, enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="f4dba-160">**STETemplate.tt** ve **STETemplate.Context.tt** dosyaları, BloggingModel. edmx dosyasının altına iç içe eklenir</span><span class="sxs-lookup"><span data-stu-id="f4dba-160">The **STETemplate.tt** and **STETemplate.Context.tt** files are added nested under the BloggingModel.edmx file</span></span>

### <a name="if-you-are-using-visual-studio-2010"></a><span data-ttu-id="f4dba-161">Visual Studio 2010 kullanıyorsanız</span><span class="sxs-lookup"><span data-stu-id="f4dba-161">If you are using Visual Studio 2010</span></span>

-   <span data-ttu-id="f4dba-162">EF Designer yüzeyinde boş bir alana sağ tıklayın ve **kod oluşturma öğesi Ekle...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="f4dba-162">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="f4dba-163">Sol bölmedeki **kodu** seçin ve ardından **ADO.net kendi kendine izleme varlık Oluşturucu**</span><span class="sxs-lookup"><span data-stu-id="f4dba-163">Select **Code** from the left pane and then **ADO.NET Self-Tracking Entity Generator**</span></span>
-   <span data-ttu-id="f4dba-164">Ad olarak **Stetemplate** girin ve **Ekle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f4dba-164">Enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="f4dba-165">**STETemplate.tt** ve **STETemplate.Context.tt** dosyaları projenize doğrudan eklenir</span><span class="sxs-lookup"><span data-stu-id="f4dba-165">The **STETemplate.tt** and **STETemplate.Context.tt** files are added directly to your project</span></span>

## <a name="move-entity-types-into-separate-project"></a><span data-ttu-id="f4dba-166">Varlık türlerini ayrı projeye taşı</span><span class="sxs-lookup"><span data-stu-id="f4dba-166">Move Entity Types into Separate Project</span></span>

<span data-ttu-id="f4dba-167">Kendi kendini Izleyen varlıkları kullanmak için, istemci uygulamamız, modelinizde oluşturulan varlık sınıflarına erişmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="f4dba-167">To use Self-Tracking Entities our client application needs access to the entity classes generated from our model.</span></span> <span data-ttu-id="f4dba-168">Tüm modeli istemci uygulamasına göstermek istemediğimiz için, varlık sınıflarını ayrı bir projeye taşıyacağız.</span><span class="sxs-lookup"><span data-stu-id="f4dba-168">Because we don't want to expose the whole model to the client application we're going to move the entity classes into a separate project.</span></span>

<span data-ttu-id="f4dba-169">İlk adım, mevcut projede varlık sınıfları oluşturmayı durdurmaktır:</span><span class="sxs-lookup"><span data-stu-id="f4dba-169">The first step is to stop generating entity classes in the existing project:</span></span>

-   <span data-ttu-id="f4dba-170">**Çözüm Gezgini** 'de **STETemplate.tt** öğesine sağ tıklayın ve **Özellikler** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="f4dba-170">Right-click on **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="f4dba-171">**Özellikler** penceresinde **CustomTool** özelliğinden **TextTemplatingFileGenerator** seçimini kaldırın</span><span class="sxs-lookup"><span data-stu-id="f4dba-171">In the **Properties** window clear **TextTemplatingFileGenerator** from the **CustomTool** property</span></span>
-   <span data-ttu-id="f4dba-172">**Çözüm Gezgini** 'de **STETemplate.tt** öğesini genişletin ve içinde iç içe yerleştirilmiş tüm dosyaları silin</span><span class="sxs-lookup"><span data-stu-id="f4dba-172">Expand **STETemplate.tt** in **Solution Explorer** and delete all files nested under it</span></span>

<span data-ttu-id="f4dba-173">Ardından, yeni bir proje ekleyeceğiz ve bu projede varlık sınıfları oluşturacağız</span><span class="sxs-lookup"><span data-stu-id="f4dba-173">Next, we are going to add a new project and generate the entity classes in it</span></span>

-   <span data-ttu-id="f4dba-174">**Dosya-&gt; Add-&gt; projesi...**</span><span class="sxs-lookup"><span data-stu-id="f4dba-174">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="f4dba-175">Sol bölmeden ve sonra **sınıf kitaplığı** 'Ndan **Visual C\#** seçin</span><span class="sxs-lookup"><span data-stu-id="f4dba-175">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="f4dba-176">Ad olarak **Stesample. Entities** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="f4dba-176">Enter **STESample.Entities** as the name and click **OK**</span></span>
-   <span data-ttu-id="f4dba-177">**Proje-&gt; var olan öğe Ekle...**</span><span class="sxs-lookup"><span data-stu-id="f4dba-177">**Project -&gt; Add Existing Item...**</span></span>
-   <span data-ttu-id="f4dba-178">**Stesample** proje klasörüne gitme</span><span class="sxs-lookup"><span data-stu-id="f4dba-178">Navigate to the **STESample** project folder</span></span>
-   <span data-ttu-id="f4dba-179">Tüm dosyaları görüntülemek için seçin **(\*.\*)**</span><span class="sxs-lookup"><span data-stu-id="f4dba-179">Select to view **All Files (\*.\*)**</span></span>
-   <span data-ttu-id="f4dba-180">**STETemplate.tt** dosyasını seçin</span><span class="sxs-lookup"><span data-stu-id="f4dba-180">Select the **STETemplate.tt** file</span></span>
-   <span data-ttu-id="f4dba-181">**Ekle** düğmesinin yanındaki aşağı açılan oka tıklayın ve **bağlantı olarak ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="f4dba-181">Click on the drop down arrow next to the **Add** button and select **Add As Link**</span></span>

    ![Bağlı Şablon Ekle](~/ef6/media/addlinkedtemplate.png)

<span data-ttu-id="f4dba-183">Ayrıca, varlık sınıflarının bağlamla aynı ad alanında oluşturulduğundan emin veriyoruz.</span><span class="sxs-lookup"><span data-stu-id="f4dba-183">We're also going to make sure the entity classes get generated in the same namespace as the context.</span></span> <span data-ttu-id="f4dba-184">Bu, uygulamamız genelinde eklememiz gereken using deyimlerinin sayısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="f4dba-184">This just reduces the number of using statements we need to add throughout our application.</span></span>

-   <span data-ttu-id="f4dba-185">**Çözüm Gezgini** bağlantılı **STETemplate.tt** sağ tıklayın ve **Özellikler** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="f4dba-185">Right-click on the linked **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="f4dba-186">**Özellikler** penceresinde, **Stesample** Için **özel araç ad alanını** ayarlayın</span><span class="sxs-lookup"><span data-stu-id="f4dba-186">In the **Properties** window set **Custom Tool Namespace** to **STESample**</span></span>

<span data-ttu-id="f4dba-187">STE şablonu tarafından oluşturulan kod, derlemek için **System. Runtime. Serialization** öğesine bir başvuruya sahip olacaktır.</span><span class="sxs-lookup"><span data-stu-id="f4dba-187">The code generated by the STE template will need a reference to **System.Runtime.Serialization** in order to compile.</span></span> <span data-ttu-id="f4dba-188">Bu kitaplık, serileştirilebilir varlık türlerinde kullanılan WCF **DataContract** ve **DataMember** öznitelikleri için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="f4dba-188">This library is needed for the WCF **DataContract** and **DataMember** attributes that are used on the serializable entity types.</span></span>

-   <span data-ttu-id="f4dba-189">**Çözüm Gezgini** ' deki **Stesample. Entities** projesine sağ tıklayın ve **Başvuru Ekle...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="f4dba-189">Right click on the **STESample.Entities** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="f4dba-190">Visual Studio 2012- **System. Runtime. Serialization** seçeneğinin yanındaki kutuyu Işaretleyin ve **Tamam** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f4dba-190">In Visual Studio 2012 - check the box next to **System.Runtime.Serialization** and click **OK**</span></span>
    -   <span data-ttu-id="f4dba-191">Visual Studio 2010- **System. Runtime. Serialization** öğesini seçin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="f4dba-191">In Visual Studio 2010 - select **System.Runtime.Serialization** and click **OK**</span></span>

<span data-ttu-id="f4dba-192">Son olarak, içinde bağlamımız bir proje varlık türlerine bir başvuruya sahip olur.</span><span class="sxs-lookup"><span data-stu-id="f4dba-192">Finally, the project with our context in it will need a reference to the entity types.</span></span>

-   <span data-ttu-id="f4dba-193">**Çözüm Gezgini** ' deki **stesample** projesine sağ tıklayın ve **Başvuru Ekle...** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="f4dba-193">Right click on the **STESample** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="f4dba-194">Visual Studio 2012-sol bölmeden **çözüm** ' i seçin, **Stesample. varlıklar** ' ın yanındaki kutuyu işaretleyin ve **Tamam** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f4dba-194">In Visual Studio 2012 - select **Solution** from the left pane, check the box next to **STESample.Entities** and click **OK**</span></span>
    -   <span data-ttu-id="f4dba-195">Visual Studio 2010- **Projeler** sekmesini seçin, **Stesample. Entities** ' yi seçin ve **Tamam** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f4dba-195">In Visual Studio 2010 - select the **Projects** tab, select **STESample.Entities** and click **OK**</span></span>

>[!NOTE]
> <span data-ttu-id="f4dba-196">Varlık türlerini ayrı bir projeye taşımaya yönelik başka bir seçenek de şablon dosyasını varsayılan konumundan bağlamak yerine taşımaktır.</span><span class="sxs-lookup"><span data-stu-id="f4dba-196">Another option for moving the entity types to a separate project is to move the template file, rather than linking it from its default location.</span></span> <span data-ttu-id="f4dba-197">Bunu yaparsanız, edmx dosyasına (Bu örnekte **..\\BloggingModel. edmx**) göreli yolu sağlamak Için şablonda **InputFile** değişkenini güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f4dba-197">If you do this, you will need to update the **inputFile** variable in the template to provide the relative path to the edmx file (in this example that would be **..\\BloggingModel.edmx**).</span></span>

## <a name="create-a-wcf-service"></a><span data-ttu-id="f4dba-198">WCF hizmeti oluşturma</span><span class="sxs-lookup"><span data-stu-id="f4dba-198">Create a WCF Service</span></span>

<span data-ttu-id="f4dba-199">Şimdi verilerimizi açığa çıkarmak için bir WCF hizmeti eklemenin zamanı, projeyi oluşturarak başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="f4dba-199">Now it's time to add a WCF Service to expose our data, we'll start by creating the project.</span></span>

-   <span data-ttu-id="f4dba-200">**Dosya-&gt; Add-&gt; projesi...**</span><span class="sxs-lookup"><span data-stu-id="f4dba-200">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="f4dba-201">Sol bölmeden ve ardından **WCF hizmeti uygulamasından** **Visual C\#** seçin</span><span class="sxs-lookup"><span data-stu-id="f4dba-201">Select **Visual C\#** from the left pane and then **WCF Service Application**</span></span>
-   <span data-ttu-id="f4dba-202">Ad olarak **Stesample. Service** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="f4dba-202">Enter **STESample.Service** as the name and click **OK**</span></span>
-   <span data-ttu-id="f4dba-203">**System. Data. Entity** derlemesine bir başvuru ekleyin</span><span class="sxs-lookup"><span data-stu-id="f4dba-203">Add a reference to the **System.Data.Entity** assembly</span></span>
-   <span data-ttu-id="f4dba-204">**Stesample** ve **Stesample. Entities** projelerine bir başvuru ekleyin</span><span class="sxs-lookup"><span data-stu-id="f4dba-204">Add a reference to the **STESample** and **STESample.Entities** projects</span></span>

<span data-ttu-id="f4dba-205">EF bağlantı dizesini, çalışma zamanında bulunduğu için bu projeye kopyalamamız gerekir.</span><span class="sxs-lookup"><span data-stu-id="f4dba-205">We need to copy the EF connection string to this project so that it is found at runtime.</span></span>

-   <span data-ttu-id="f4dba-206"> **Stesample **projesi için **app. config** dosyasını açın ve **connectionStrings** öğesini kopyalayın</span><span class="sxs-lookup"><span data-stu-id="f4dba-206">Open the **App.Config** file for the **STESample **project and copy the **connectionStrings** element</span></span>
-   <span data-ttu-id="f4dba-207">**ConnectionString** öğesini, **Stesample. Service** projesindeki **Web. config** dosyasının **yapılandırma** öğesinin bir alt öğesi olarak yapıştırın</span><span class="sxs-lookup"><span data-stu-id="f4dba-207">Paste the **connectionStrings** element as a child element of the **configuration** element of the **Web.Config** file in the **STESample.Service** project</span></span>

<span data-ttu-id="f4dba-208">Şimdi de gerçek hizmeti uygulama zamanı.</span><span class="sxs-lookup"><span data-stu-id="f4dba-208">Now it's time to implement the actual service.</span></span>

-   <span data-ttu-id="f4dba-209">**IService1.cs** açın ve içeriğini aşağıdaki kodla değiştirin</span><span class="sxs-lookup"><span data-stu-id="f4dba-209">Open **IService1.cs** and replace the contents with the following code</span></span>

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

-   <span data-ttu-id="f4dba-210">**Service1. svc** açın ve içerikleri aşağıdaki kodla değiştirin</span><span class="sxs-lookup"><span data-stu-id="f4dba-210">Open **Service1.svc** and replace the contents with the following code</span></span>

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

## <a name="consume-the-service-from-a-console-application"></a><span data-ttu-id="f4dba-211">Hizmeti bir konsol uygulamasından tüketme</span><span class="sxs-lookup"><span data-stu-id="f4dba-211">Consume the Service from a Console Application</span></span>

<span data-ttu-id="f4dba-212">Hizmetimizi kullanan bir konsol uygulaması oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="f4dba-212">Let's create a console application that uses our service.</span></span>

-   <span data-ttu-id="f4dba-213">**Dosya-&gt; yeni&gt; projesi...**</span><span class="sxs-lookup"><span data-stu-id="f4dba-213">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="f4dba-214">Sol bölmeden ve sonra **konsol uygulamasında** **Visual C\#** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="f4dba-214">Select **Visual C\#** from the left pane and then **Console Application**</span></span>
-   <span data-ttu-id="f4dba-215">Ad olarak **Stesample. ConsoleTest** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="f4dba-215">Enter **STESample.ConsoleTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="f4dba-216">**Stesample. Entities** projesine bir başvuru ekleyin</span><span class="sxs-lookup"><span data-stu-id="f4dba-216">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="f4dba-217">WCF hizmetinize bir hizmet başvurusu gerekiyor</span><span class="sxs-lookup"><span data-stu-id="f4dba-217">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="f4dba-218">**Çözüm Gezgini** ' deki **Stesample. consoletest** projesine sağ tıklayın ve **hizmet başvurusu Ekle...** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="f4dba-218">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="f4dba-219">**Keşfet** 'e tıklayın</span><span class="sxs-lookup"><span data-stu-id="f4dba-219">Click **Discover**</span></span>
-   <span data-ttu-id="f4dba-220">Ad alanı olarak **BloggingService** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="f4dba-220">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="f4dba-221">Artık hizmeti tüketmek için bazı kodlar yazabiliriz.</span><span class="sxs-lookup"><span data-stu-id="f4dba-221">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="f4dba-222">**Program.cs** açın ve içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f4dba-222">Open **Program.cs** and replace the contents with the following code.</span></span>

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

<span data-ttu-id="f4dba-223">Artık uygulamayı çalışır durumda görmek için çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f4dba-223">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="f4dba-224">**Çözüm Gezgini** ' deki **Stesample. consoletest** projesine sağ tıklayın ve **Hata Ayıkla-&gt; yeni örnek Başlat** ' ı seçin</span><span class="sxs-lookup"><span data-stu-id="f4dba-224">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>

<span data-ttu-id="f4dba-225">Uygulama yürütüldüğünde aşağıdaki çıktıyı görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="f4dba-225">You'll see the following output when the application executes.</span></span>

```console
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

## <a name="consume-the-service-from-a-wpf-application"></a><span data-ttu-id="f4dba-226">Bir WPF uygulamasından hizmeti tüketme</span><span class="sxs-lookup"><span data-stu-id="f4dba-226">Consume the Service from a WPF Application</span></span>

<span data-ttu-id="f4dba-227">Hizmetimizi kullanan bir WPF uygulaması oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="f4dba-227">Let's create a WPF application that uses our service.</span></span>

-   <span data-ttu-id="f4dba-228">**Dosya-&gt; yeni&gt; projesi...**</span><span class="sxs-lookup"><span data-stu-id="f4dba-228">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="f4dba-229">Sol bölmeden ve ardından **WPF uygulamasında** **Visual C\#** seçin</span><span class="sxs-lookup"><span data-stu-id="f4dba-229">Select **Visual C\#** from the left pane and then **WPF Application**</span></span>
-   <span data-ttu-id="f4dba-230">Ad olarak **Stesample. WPFTest** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="f4dba-230">Enter **STESample.WPFTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="f4dba-231">**Stesample. Entities** projesine bir başvuru ekleyin</span><span class="sxs-lookup"><span data-stu-id="f4dba-231">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="f4dba-232">WCF hizmetinize bir hizmet başvurusu gerekiyor</span><span class="sxs-lookup"><span data-stu-id="f4dba-232">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="f4dba-233">**Çözüm Gezgini** ' deki **Stesample. wpftest** projesine sağ tıklayın ve **hizmet başvurusu Ekle...** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="f4dba-233">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="f4dba-234">**Keşfet** 'e tıklayın</span><span class="sxs-lookup"><span data-stu-id="f4dba-234">Click **Discover**</span></span>
-   <span data-ttu-id="f4dba-235">Ad alanı olarak **BloggingService** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="f4dba-235">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="f4dba-236">Artık hizmeti tüketmek için bazı kodlar yazabiliriz.</span><span class="sxs-lookup"><span data-stu-id="f4dba-236">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="f4dba-237">**MainWindow. xaml** ' i açın ve içeriği aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f4dba-237">Open **MainWindow.xaml** and replace the contents with the following code.</span></span>

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

-   <span data-ttu-id="f4dba-238">MainWindow (**MainWindow.xaml.cs**) için arka plan kodunu açın ve içeriği aşağıdaki kodla değiştirin</span><span class="sxs-lookup"><span data-stu-id="f4dba-238">Open the code behind for MainWindow (**MainWindow.xaml.cs**) and replace the contents with the following code</span></span>

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

<span data-ttu-id="f4dba-239">Artık uygulamayı çalışır durumda görmek için çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f4dba-239">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="f4dba-240">**Çözüm Gezgini** ' deki **Stesample. wpftest** projesine sağ tıklayın ve **Hata Ayıkla-&gt; yeni örnek Başlat** ' ı seçin</span><span class="sxs-lookup"><span data-stu-id="f4dba-240">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>
-   <span data-ttu-id="f4dba-241">Ekranı kullanarak verileri işleyebilir ve **Kaydet** düğmesini kullanarak hizmet aracılığıyla kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f4dba-241">You can manipulate the data using the screen and save it via the service using the **Save** button</span></span>

![WPF ana penceresi](~/ef6/media/wpf.png)
