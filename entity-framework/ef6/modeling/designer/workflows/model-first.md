---
title: EF6 öncelikle - model
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
caps.latest.revision: 3
ms.openlocfilehash: e7876776ed0dee764d5ced97b863a3580e02e20b
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912678"
---
# <a name="model-first"></a><span data-ttu-id="81ee8-102">İlk model</span><span class="sxs-lookup"><span data-stu-id="81ee8-102">Model First</span></span>
<span data-ttu-id="81ee8-103">Bu video ve adım adım izlenecek yol, Entity Framework kullanarak Model First geliştirmeye giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="81ee8-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="81ee8-104">Model, Entity Framework Designer kullanarak yeni bir model oluşturmak ve ardından bir veritabanı şema modeli oluşturmak öncelikle sağlar.</span><span class="sxs-lookup"><span data-stu-id="81ee8-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="81ee8-105">Model bir EDMX dosyası (.edmx uzantısı) depolanır ve görüntülenebilir ve Entity Framework Tasarımcısı'nda düzenlenebilir.</span><span class="sxs-lookup"><span data-stu-id="81ee8-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="81ee8-106">Uygulamanıza etkileşim sınıfları EDMX dosyasından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="81ee8-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="81ee8-107">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="81ee8-107">Watch the video</span></span>
<span data-ttu-id="81ee8-108">Bu video ve adım adım izlenecek yol, Entity Framework kullanarak Model First geliştirmeye giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="81ee8-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="81ee8-109">Model, Entity Framework Designer kullanarak yeni bir model oluşturmak ve ardından bir veritabanı şema modeli oluşturmak öncelikle sağlar.</span><span class="sxs-lookup"><span data-stu-id="81ee8-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="81ee8-110">Model bir EDMX dosyası (.edmx uzantısı) depolanır ve görüntülenebilir ve Entity Framework Tasarımcısı'nda düzenlenebilir.</span><span class="sxs-lookup"><span data-stu-id="81ee8-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="81ee8-111">Uygulamanıza etkileşim sınıfları EDMX dosyasından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="81ee8-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="81ee8-112">**Tarafından sunulan**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="81ee8-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="81ee8-113">**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="81ee8-113">**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="81ee8-114">Ön koşullar</span><span class="sxs-lookup"><span data-stu-id="81ee8-114">Pre-Requisites</span></span>

<span data-ttu-id="81ee8-115">Bu izlenecek yolu tamamlamak için Visual Studio 2012 yüklü veya Visual Studio 2010 olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="81ee8-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="81ee8-116">Visual Studio 2010 kullanıyorsanız, aynı zamanda sahip gerekecektir [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) yüklü.</span><span class="sxs-lookup"><span data-stu-id="81ee8-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="81ee8-117">1. Uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="81ee8-117">1. Create the Application</span></span>

<span data-ttu-id="81ee8-118">Örneği basit tutmak için veri erişimi gerçekleştirdiği ilk modeli kullanan bir temel bir konsol uygulamasının oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="81ee8-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="81ee8-119">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="81ee8-119">Open Visual Studio</span></span>
-   <span data-ttu-id="81ee8-120">**Dosya -&gt; yeni -&gt; proje...**</span><span class="sxs-lookup"><span data-stu-id="81ee8-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="81ee8-121">Seçin **Windows** sol menüden ve **konsol uygulaması**</span><span class="sxs-lookup"><span data-stu-id="81ee8-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="81ee8-122">Girin **ModelFirstSample** adı</span><span class="sxs-lookup"><span data-stu-id="81ee8-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="81ee8-123">Seçin **Tamam**</span><span class="sxs-lookup"><span data-stu-id="81ee8-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="81ee8-124">2. Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="81ee8-124">2. Create Model</span></span>

<span data-ttu-id="81ee8-125">Modelimiz oluşturmak için Entity Framework Visual Studio'nun bir parçası olarak dahil olan Designer'ın, kullanan oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="81ee8-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="81ee8-126">**Takım projesi -&gt; Yeni Öğe Ekle...**</span><span class="sxs-lookup"><span data-stu-id="81ee8-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="81ee8-127">Seçin **veri** sol menüden ardından **ADO.NET varlık veri modeli**</span><span class="sxs-lookup"><span data-stu-id="81ee8-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="81ee8-128">Girin **BloggingModel** tıklayın ve adı olarak **Tamam**, bu varlık veri modeli Sihirbazı başlatır</span><span class="sxs-lookup"><span data-stu-id="81ee8-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="81ee8-129">Seçin **boş Model** tıklatıp **son**</span><span class="sxs-lookup"><span data-stu-id="81ee8-129">Select **Empty Model** and click **Finish**</span></span>

    ![CreateEmptyModel](~/ef6/media/createemptymodel.png)

<span data-ttu-id="81ee8-131">Entity Framework Designer boş bir model ile açılır.</span><span class="sxs-lookup"><span data-stu-id="81ee8-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="81ee8-132">Artık modele varlıklar, özellikleri ve ilişkileri ekleme başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="81ee8-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="81ee8-133">Tasarım yüzeyi ve select sağ **özellikleri**</span><span class="sxs-lookup"><span data-stu-id="81ee8-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="81ee8-134">Özellikler penceresinde değişiklik **varlık kapsayıcı adı** için **BloggingContext**
    *sizin için bağlam oluşturulacak türetilmiş bağlamı adıdır Sorgu ve veri kaydetmek bize izin vererek, veritabanı ile bir oturumu temsil eder*</span><span class="sxs-lookup"><span data-stu-id="81ee8-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="81ee8-135">Tasarım yüzeyi ve select sağ **Ekle - yeni&gt; varlık...**</span><span class="sxs-lookup"><span data-stu-id="81ee8-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="81ee8-136">Girin **Blog** varlık adı olarak ve **BlogId** tıklatın ve anahtar adını olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="81ee8-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![AddBlogEntity](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="81ee8-138">Yeni varlık tasarım yüzeyi ve select üzerinde sağ tıklayın **Ekle - yeni&gt; skaler özelliği**, girin **adı** özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="81ee8-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="81ee8-139">Eklemek için bu işlemi tekrarlamanız bir **Url** özelliği.</span><span class="sxs-lookup"><span data-stu-id="81ee8-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="81ee8-140">Sağ **Url** tasarım yüzeyi ve seçme özelliği **özellikleri**, Özellikler penceresinde değişiklik **Nullable** ayarını **True** 
     *Bu Blog, bir Url atamadan veritabanına kaydetme olanak tanır*</span><span class="sxs-lookup"><span data-stu-id="81ee8-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="81ee8-141">Yalnızca modellerin, teknikleri kullanarak bir **Post** varlıkla bir **PostId** anahtar özelliği</span><span class="sxs-lookup"><span data-stu-id="81ee8-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="81ee8-142">Ekleme **başlık** ve **içerik** skaler özelliklerine **Post** varlık</span><span class="sxs-lookup"><span data-stu-id="81ee8-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="81ee8-143">Birkaç varlık sahibiz, bunlar arasında bir ilişkilendirme (veya ilişki) ekleme zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="81ee8-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="81ee8-144">Tasarım yüzeyi ve select sağ **Ekle - yeni&gt; ilişki...**</span><span class="sxs-lookup"><span data-stu-id="81ee8-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="81ee8-145">İlişkinin bir ucu üzerine olun **Blog** çokluğunu ile **bir** ve diğer uç noktasına **Post** çokluğunu ile **birçok** 
     *Bu Blog birçok gönderileri ve bir gönderi bir Bloga ait olduğu anlamına gelir*</span><span class="sxs-lookup"><span data-stu-id="81ee8-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="81ee8-146">Olun **'Posta' varlığa yabancı anahtar özelliklerini eklemek** kutusunda denetlenir ve tıklayın **Tamam**</span><span class="sxs-lookup"><span data-stu-id="81ee8-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![AddAssociationMF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="81ee8-148">Artık size bir veritabanından oluşturmak ve veri okuma ve yazma için kullanan basit bir model var.</span><span class="sxs-lookup"><span data-stu-id="81ee8-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![ModelInitial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="81ee8-150">Visual Studio 2010'daki ek adımlar</span><span class="sxs-lookup"><span data-stu-id="81ee8-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="81ee8-151">Visual Studio 2010'da çalışıyorsanız Entity Framework'ün en son sürüme yükseltmek için izlemeniz gereken bazı ek adımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="81ee8-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="81ee8-152">Yükseltme önemlidir çünkü en son hata düzeltmeleri yanı sıra kullanmak çok daha kolaydır, geliştirilmiş bir API yüzeyi, erişmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="81ee8-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="81ee8-153">Öncelikle, Nuget'ten Entity Framework'ün en son sürümünü almak ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="81ee8-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="81ee8-154">**Proje –&gt; NuGet paketlerini Yönet... ** 
     *Yoksa **NuGet paketlerini Yönet... ** yüklemelisiniz seçeneği [en son NuGet sürümünü](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="81ee8-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="81ee8-155">Seçin **çevrimiçi** sekmesi</span><span class="sxs-lookup"><span data-stu-id="81ee8-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="81ee8-156">Seçin **EntityFramework** paket</span><span class="sxs-lookup"><span data-stu-id="81ee8-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="81ee8-157">Tıklayın **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="81ee8-157">Click **Install**</span></span>

<span data-ttu-id="81ee8-158">Ardından, Entity Framework'ün sonraki sürümlerinde sunulan DbContext API kullanır kod üretmek için modelimizi takas etmek ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="81ee8-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="81ee8-159">Modelinizin EF Tasarımcısı'nda boş bir nokta üzerinde sağ tıklayıp **kod oluşturma öğesi Ekle...**</span><span class="sxs-lookup"><span data-stu-id="81ee8-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="81ee8-160">Seçin **çevrimiçi şablonlar** arayın ve sol menüden **DbContext**</span><span class="sxs-lookup"><span data-stu-id="81ee8-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="81ee8-161">EF seçin **5.x DbContext Oluşturucu c\#**, girin **BloggingModel** tıklayın ve adı olarak **Ekle**</span><span class="sxs-lookup"><span data-stu-id="81ee8-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![DbContextTemplate](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="81ee8-163">3. Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="81ee8-163">3. Generating the Database</span></span>

<span data-ttu-id="81ee8-164">Modelimiz göz önünde bulundurulduğunda, Entity Framework modelini kullanarak veri depolama ve alma için bize izin veren bir veritabanı şeması hesaplayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="81ee8-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="81ee8-165">Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösterir:</span><span class="sxs-lookup"><span data-stu-id="81ee8-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="81ee8-166">Visual Studio 2010 kullanıyorsanız, SQL Express veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="81ee8-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="81ee8-167">Visual Studio 2012 kullanıyorsanız sonra, oluşturduğunuz bir [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="81ee8-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="81ee8-168">Yeni bir ubuntu ve veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="81ee8-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="81ee8-169">Tasarım yüzeyi ve select sağ **Model veritabanından oluştur...**</span><span class="sxs-lookup"><span data-stu-id="81ee8-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="81ee8-170">Tıklayın **yeni bağlantı...**</span><span class="sxs-lookup"><span data-stu-id="81ee8-170">Click **New Connection…**</span></span> <span data-ttu-id="81ee8-171">LocalDB veya Visual Studio sürümüne bağlı olarak kullandığınız SQL Express belirtin girin **ModelFirst.Blogging** veritabanı adı.</span><span class="sxs-lookup"><span data-stu-id="81ee8-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![LocalDBConnectionMF](~/ef6/media/localdbconnectionmf.png)

    ![SqlExpressConnectionMF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="81ee8-174">Seçin **Tamam** ve bir yeni bir veritabanı oluşturmak istiyorsanız istenir **Evet**</span><span class="sxs-lookup"><span data-stu-id="81ee8-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="81ee8-175">Seçin **sonraki** ve Entity Framework Designer veritabanı şeması oluşturmak için bir betik hesaplar</span><span class="sxs-lookup"><span data-stu-id="81ee8-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="81ee8-176">Betik görüntülendikten sonra tıklayın **son** ve komut dosyası projenize eklenir ve açılır</span><span class="sxs-lookup"><span data-stu-id="81ee8-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="81ee8-177">Sağ tıklatın ve betik **yürütme**, LocalDB veritabanına bağlanmak belirtip için istenir veya SQL Server Express, kullanmakta olduğunuz Visual Studio sürümüne bağlı olarak</span><span class="sxs-lookup"><span data-stu-id="81ee8-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="81ee8-178">4. Okuma ve yazma veri</span><span class="sxs-lookup"><span data-stu-id="81ee8-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="81ee8-179">Biz bir modeliniz olduğuna göre bazı verilere erişmek için kullanılacak zaman var.</span><span class="sxs-lookup"><span data-stu-id="81ee8-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="81ee8-180">Sınıfları kullanacağız EDMX dosyasını temel alan için kullanılacak erişim verilerini otomatik olarak oluşturuluyor.</span><span class="sxs-lookup"><span data-stu-id="81ee8-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="81ee8-181">*Visual Studio 2010 kullanıyorsanız bu ekran görüntüsü, Visual Studio 2012'den BloggingModel.tt olduğu ve BloggingModel.Context.tt dosyalarını doğrudan projenize altında yerine EDMX dosyasının altında iç içe geçmiş.*</span><span class="sxs-lookup"><span data-stu-id="81ee8-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![GeneratedClasses](~/ef6/media/generatedclasses.png)

<span data-ttu-id="81ee8-183">Main yöntemi program.cs'ye aşağıda gösterildiği gibi uygulayın.</span><span class="sxs-lookup"><span data-stu-id="81ee8-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="81ee8-184">Bu kod, bizim bağlam yeni bir örneğini oluşturur ve yeni bir Blog eklemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="81ee8-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="81ee8-185">Ardından veritabanından başlığa göre alfabetik olarak sıralanmış tüm blogları almak için LINQ sorgusu kullanır.</span><span class="sxs-lookup"><span data-stu-id="81ee8-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="81ee8-186">Şimdi uygulamayı çalıştırmak ve test etmek.</span><span class="sxs-lookup"><span data-stu-id="81ee8-186">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="81ee8-187">5. Model değişikliklerle ilgilenme</span><span class="sxs-lookup"><span data-stu-id="81ee8-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="81ee8-188">Artık size ayrıca veritabanı şemasını güncelleştirmek için ihtiyacımız olan bu değişiklikler yaptığınızda modelimiz için bazı değişiklikler yapmanız gerekebileceğini zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="81ee8-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="81ee8-189">Modelimiz için yeni bir kullanıcı varlığı ekleyerek başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="81ee8-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="81ee8-190">Yeni bir **kullanıcı** varlık adı ile **kullanıcıadı** anahtar adı olarak ve **dize** anahtar özellik türü olarak</span><span class="sxs-lookup"><span data-stu-id="81ee8-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![AddUserEntity](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="81ee8-192">Sağ **kullanıcıadı** tasarım yüzeyi ve seçme özelliği **özellikleri**, Özellikler penceresinde değişiklik **MaxLength** ayarını **50 ** 
     *Bu kullanıcı adı 50 karakterden depolanabilir veri sınırlar*</span><span class="sxs-lookup"><span data-stu-id="81ee8-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="81ee8-193">Ekleme bir **DisplayName** skaler özelliğe **kullanıcı** varlık</span><span class="sxs-lookup"><span data-stu-id="81ee8-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="81ee8-194">Şimdi güncelleştirilmiş bir model vardır ve müşterilerimize yeni kullanıcının varlık türü uyum sağlayacak şekilde veritabanını güncelleştirmek hazırız.</span><span class="sxs-lookup"><span data-stu-id="81ee8-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="81ee8-195">Tasarım yüzeyi ve select sağ **Model veritabanından oluştur...** , Entity Framework, güncelleştirilmiş model temelinde bir şema yeniden oluşturmak için bir betik hesaplar.</span><span class="sxs-lookup"><span data-stu-id="81ee8-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="81ee8-196">Tıklayın **son**</span><span class="sxs-lookup"><span data-stu-id="81ee8-196">Click **Finish**</span></span>
-   <span data-ttu-id="81ee8-197">Mevcut DDL betik ve model eşlemesi ve depolama bölümleri hakkında uyarılar alırsınız, tıklayın **Evet** hem bu uyarılar için</span><span class="sxs-lookup"><span data-stu-id="81ee8-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="81ee8-198">Veritabanını oluşturmak için güncelleştirilmiş SQL betiği açılır</span><span class="sxs-lookup"><span data-stu-id="81ee8-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="81ee8-199">*Oluşturulan kodun tüm varolan tablolardan bırakın ve şema sıfırdan yeniden. Bu, yerel geliştirme için çalışabilir ancak zaten dağıtılmış bir veritabanına değişiklikleri gönderme için uygulanabilir değil. Zaten dağıtılmış bir veritabanına değişiklikleri yayımlamak gerekiyorsa, betiği düzenleyin veya bir geçiş öncesinde bir betik hesaplamak için bir şema karşılaştırma aracını kullanmak gerekir.*</span><span class="sxs-lookup"><span data-stu-id="81ee8-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="81ee8-200">Sağ tıklatın ve betik **yürütme**, LocalDB veritabanına bağlanmak belirtip için istenir veya SQL Server Express, kullanmakta olduğunuz Visual Studio sürümüne bağlı olarak</span><span class="sxs-lookup"><span data-stu-id="81ee8-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="81ee8-201">Özet</span><span class="sxs-lookup"><span data-stu-id="81ee8-201">Summary</span></span>

<span data-ttu-id="81ee8-202">Model ilk geliştirme için incelemiştik bu kılavuzda, hangi EF Tasarımcısı'nda bir model oluşturmak ve ardından bir veritabanı o Modeli'nden olduk.</span><span class="sxs-lookup"><span data-stu-id="81ee8-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="81ee8-203">Ardından model okuma ve bazı verileri veritabanından yazma kullandık.</span><span class="sxs-lookup"><span data-stu-id="81ee8-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="81ee8-204">Son olarak, biz güncelleştirilmiş model ve veritabanı şeması modeli eşleşecek şekilde yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="81ee8-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
