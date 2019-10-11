---
title: Model First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 1b37805beb3d33f0b6dad2577a8abb3ea8f7b1e4
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182437"
---
# <a name="model-first"></a><span data-ttu-id="3db80-102">Model First</span><span class="sxs-lookup"><span data-stu-id="3db80-102">Model First</span></span>
<span data-ttu-id="3db80-103">Bu video ve adım adım yönergeler, Entity Framework kullanarak Model First geliştirmeye yönelik bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="3db80-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="3db80-104">Model First, Entity Framework Designer kullanarak yeni bir model oluşturmanızı ve sonra modelden bir veritabanı şeması oluşturmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="3db80-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="3db80-105">Model bir EDMX dosyasında (. edmx uzantılı) depolanır ve Entity Framework Designer görüntülenebilir ve düzenlenebilir.</span><span class="sxs-lookup"><span data-stu-id="3db80-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="3db80-106">Uygulamanızda etkileşimde bulunan sınıflar, EDMX dosyasından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3db80-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="3db80-107">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="3db80-107">Watch the video</span></span>
<span data-ttu-id="3db80-108">Bu video ve adım adım yönergeler, Entity Framework kullanarak Model First geliştirmeye yönelik bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="3db80-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="3db80-109">Model First, Entity Framework Designer kullanarak yeni bir model oluşturmanızı ve sonra modelden bir veritabanı şeması oluşturmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="3db80-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="3db80-110">Model bir EDMX dosyasında (. edmx uzantılı) depolanır ve Entity Framework Designer görüntülenebilir ve düzenlenebilir.</span><span class="sxs-lookup"><span data-stu-id="3db80-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="3db80-111">Uygulamanızda etkileşimde bulunan sınıflar, EDMX dosyasından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3db80-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="3db80-112">**Sunulma ölçütü**: [ROWA Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="3db80-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="3db80-113">**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="3db80-113">**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="3db80-114">Önkoşulların önkoşulları</span><span class="sxs-lookup"><span data-stu-id="3db80-114">Pre-Requisites</span></span>

<span data-ttu-id="3db80-115">Bu izlenecek yolu tamamlamak için Visual Studio 2010 veya Visual Studio 2012 yüklü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3db80-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="3db80-116">Visual Studio 2010 kullanıyorsanız, [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) ' in yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3db80-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="3db80-117">1. Uygulamayı oluşturma</span><span class="sxs-lookup"><span data-stu-id="3db80-117">1. Create the Application</span></span>

<span data-ttu-id="3db80-118">Şeyleri basit tutmak için, veri erişimi gerçekleştirmek üzere Model First kullanan temel bir konsol uygulaması oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="3db80-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="3db80-119">Visual Studio 'Yu aç</span><span class="sxs-lookup"><span data-stu-id="3db80-119">Open Visual Studio</span></span>
-   <span data-ttu-id="3db80-120">**Dosya-&gt; yeni-&gt; proje...**</span><span class="sxs-lookup"><span data-stu-id="3db80-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="3db80-121">Sol taraftaki menüden ve **konsol uygulamasından** **Windows** ' u seçin</span><span class="sxs-lookup"><span data-stu-id="3db80-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="3db80-122">Ad olarak **Modelfirstsample** girin</span><span class="sxs-lookup"><span data-stu-id="3db80-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="3db80-123">**Tamam 'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="3db80-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="3db80-124">2. Model oluştur</span><span class="sxs-lookup"><span data-stu-id="3db80-124">2. Create Model</span></span>

<span data-ttu-id="3db80-125">Modelimizi oluşturmak için Visual Studio 'nun bir parçası olarak dahil edilen Entity Framework Designer kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="3db80-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="3db80-126">**Proje-&gt; yeni öğe Ekle...**</span><span class="sxs-lookup"><span data-stu-id="3db80-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="3db80-127">Sol menüden **verileri** seçin ve ardından **ADO.net varlık veri modeli**</span><span class="sxs-lookup"><span data-stu-id="3db80-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="3db80-128">Ad olarak **BloggingModel** girin ve **Tamam**' a tıkladıktan sonra varlık veri modeli Sihirbazı başlatılır</span><span class="sxs-lookup"><span data-stu-id="3db80-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="3db80-129">**Boş model** ' i seçin ve **son** ' a tıklayın</span><span class="sxs-lookup"><span data-stu-id="3db80-129">Select **Empty Model** and click **Finish**</span></span>

    ![Boş model oluştur](~/ef6/media/createemptymodel.png)

<span data-ttu-id="3db80-131">Entity Framework Designer boş bir modelle açılır.</span><span class="sxs-lookup"><span data-stu-id="3db80-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="3db80-132">Artık modele varlıklar, Özellikler ve ilişkilendirmeler eklemeye başlayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="3db80-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="3db80-133">Tasarım yüzeyine sağ tıklayıp **Özellikler** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="3db80-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="3db80-134">Özellikler penceresi **varlık kapsayıcısı adını** **BloggingContext**
     olarak değiştirin-2*Bu, sizin için üretilecek olan türetilmiş bağlamın adıdır, bağlam veritabanı ile bir oturumu temsil eder ve bu da sorgulamanızı ve kaydetmemizi sağlar veri*</span><span class="sxs-lookup"><span data-stu-id="3db80-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="3db80-135">Tasarım yüzeyine sağ tıklayın ve **yeni &gt; varlık Ekle ' yi seçin...**</span><span class="sxs-lookup"><span data-stu-id="3db80-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="3db80-136">Varlık adı ve **blogID** olarak anahtar adı olarak blog girin ve **Tamam** ' **a** tıklayın.</span><span class="sxs-lookup"><span data-stu-id="3db80-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![Blog varlığı Ekle](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="3db80-138">Tasarım yüzeyinde yeni varlığa sağ tıklayın ve **Yeni-&gt; skaler Özellik Ekle**' yi seçin, özelliğin adı olarak **ad** girin.</span><span class="sxs-lookup"><span data-stu-id="3db80-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="3db80-139">**URL** özelliği eklemek için bu işlemi tekrarlayın.</span><span class="sxs-lookup"><span data-stu-id="3db80-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="3db80-140">Tasarım yüzeyinde **URL** özelliği ' ne sağ tıklayın ve **Özellikler**' i seçin Özellikler penceresi **null yapılabilir** ayarını **doğru**olarak değiştirin 
    \*Bu, bir web günlüğünü veritabanına atamadan veritabanına kaydetmemizi sağlar \*</span><span class="sxs-lookup"><span data-stu-id="3db80-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="3db80-141">Yeni öğrenmeniz gereken teknikleri kullanarak **postid** anahtar özelliğine sahip bir **Post** varlığı ekleyin</span><span class="sxs-lookup"><span data-stu-id="3db80-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="3db80-142">**Post** varlığına **başlık** ve **içerik** skaler özellikleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="3db80-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="3db80-143">Artık birkaç varlık olduğuna göre, aralarında bir ilişki (veya ilişki) ekleme zamanı.</span><span class="sxs-lookup"><span data-stu-id="3db80-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="3db80-144">Tasarım yüzeyine sağ tıklayıp **Yeni-&gt; Ilişkilendirme Ekle ' yi seçin...**</span><span class="sxs-lookup"><span data-stu-id="3db80-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="3db80-145">İlişki noktasının bir bitişini bir **tane** birçok @no__t çokluğa sahip **olacak şekilde** **bloga** ve diğer uç noktanın **çok sayıda**-4 *'* e</span><span class="sxs-lookup"><span data-stu-id="3db80-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
    *This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="3db80-146">**' Post ' varlığına yabancı anahtar özellikleri ekle** kutusunun işaretli olduğundan emin olun ve **Tamam** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="3db80-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![Ilişki ekleme MF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="3db80-148">Artık, bir veritabanı oluşturabileceğiniz ve veri okumak ve yazmak için kullanabileceğiniz basit bir modelimiz vardır.</span><span class="sxs-lookup"><span data-stu-id="3db80-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![Ilk model](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="3db80-150">Visual Studio 2010 ' de ek adımlar</span><span class="sxs-lookup"><span data-stu-id="3db80-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="3db80-151">Visual Studio 2010 ' de çalışıyorsanız, en son Entity Framework sürümüne yükseltmek için izlemeniz gereken bazı ek adımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="3db80-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="3db80-152">' Nin, kullanımı çok daha kolay olan gelişmiş bir API yüzeyine erişim sağladığından ve en son hata düzeltmelerinin yanı sıra yükseltme önemlidir.</span><span class="sxs-lookup"><span data-stu-id="3db80-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="3db80-153">İlk olarak, Entity Framework NuGet 'den en son sürümü alması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3db80-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="3db80-154">**Proje – &gt; NuGet Paketlerini Yönet...** 
    \* **NuGet Paketlerini Yönet...** seçeneğine sahipseniz [NuGet 'in en son sürümünü](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) yüklemelisiniz\*</span><span class="sxs-lookup"><span data-stu-id="3db80-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="3db80-155">**Çevrimiçi** sekmesini seçin</span><span class="sxs-lookup"><span data-stu-id="3db80-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="3db80-156">**EntityFramework** paketini seçin</span><span class="sxs-lookup"><span data-stu-id="3db80-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="3db80-157">**Install** 'a tıklayın</span><span class="sxs-lookup"><span data-stu-id="3db80-157">Click **Install**</span></span>

<span data-ttu-id="3db80-158">Bundan sonra, Entity Framework sonraki sürümlerinde tanıtılan DbContext API 'sini kullanan kodu oluşturmak için modelimizi değiştirmemiz gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="3db80-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="3db80-159">EF Designer 'daki modelinizin boş bir noktasına sağ tıklayıp **kod oluşturma öğesi Ekle...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="3db80-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="3db80-160">Sol menüden **çevrimiçi şablonlar** ' ı seçin ve **DbContext** ' i arayın</span><span class="sxs-lookup"><span data-stu-id="3db80-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="3db80-161">**C @ no__t-1 IÇIN EF 5. x DbContext Generator**' ı seçin, ad olarak **BloggingModel** girin ve **Ekle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="3db80-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![DbContext şablonu](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="3db80-163">3. Veritabanı oluşturuluyor</span><span class="sxs-lookup"><span data-stu-id="3db80-163">3. Generating the Database</span></span>

<span data-ttu-id="3db80-164">Modelimiz verildiğinde, Entity Framework modeli kullanarak veri depolamanıza ve almasına imkan tanıyan bir veritabanı şemasını hesaplayabilirler.</span><span class="sxs-lookup"><span data-stu-id="3db80-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="3db80-165">Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösteren bir sürümdür:</span><span class="sxs-lookup"><span data-stu-id="3db80-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="3db80-166">Visual Studio 2010 kullanıyorsanız, bir SQL Express veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="3db80-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="3db80-167">Visual Studio 2012 kullanıyorsanız, [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="3db80-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="3db80-168">Şimdi veritabanını oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="3db80-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="3db80-169">Tasarım yüzeyine sağ tıklayın ve **modelden veritabanı oluştur..** . seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="3db80-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="3db80-170">Yeni bağlantı ' ya tıklayın **...**</span><span class="sxs-lookup"><span data-stu-id="3db80-170">Click **New Connection…**</span></span> <span data-ttu-id="3db80-171">ve hangi Visual Studio sürümüne bağlı olarak, LocalDB ya da SQL Express 'i belirtin, veritabanı adı olarak **Modelfirst. blog** girin.</span><span class="sxs-lookup"><span data-stu-id="3db80-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![LocalDB bağlantısı MF](~/ef6/media/localdbconnectionmf.png)

    ![SQL Express bağlantı MF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="3db80-174">**Tamam** ' ı seçtiğinizde, yeni bir veritabanı oluşturmak isteyip istemediğiniz sorulur, **Evet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="3db80-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="3db80-175">**İleri ' yi** seçin ve Entity Framework Designer veritabanı şemasını oluşturmak için bir komut dosyası hesaplar</span><span class="sxs-lookup"><span data-stu-id="3db80-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="3db80-176">Betik görüntülenirken **son** ' a tıklayın ve betik projenize eklenir ve açılır</span><span class="sxs-lookup"><span data-stu-id="3db80-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="3db80-177">Komut dosyasına sağ tıklayın ve **Yürüt**' ü seçin, hangi Visual Studio sürümüne bağlı olarak ' ye bağlanacak veritabanını belirtmeniz, LocalDB veya SQL Server Express belirtmeniz istenecektir</span><span class="sxs-lookup"><span data-stu-id="3db80-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="3db80-178">4. Verileri okuma & yazma</span><span class="sxs-lookup"><span data-stu-id="3db80-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="3db80-179">Artık bir modelimiz olduğuna göre, bazı verilere erişmek için bunu kullanmanın zamanı.</span><span class="sxs-lookup"><span data-stu-id="3db80-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="3db80-180">Verilere erişmek için kullanacağınız sınıflar, EDMX dosyasına bağlı olarak sizin için otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3db80-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="3db80-181">*Bu ekran görüntüsü Visual Studio 2012 ' den, Visual Studio 2010 kullanıyorsanız BloggingModel.tt ve BloggingModel.Context.tt dosyaları, EDMX dosyasının altında iç içe değil doğrudan projenizin altında olacaktır.*</span><span class="sxs-lookup"><span data-stu-id="3db80-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Oluşturulan sınıflar](~/ef6/media/generatedclasses.png)

<span data-ttu-id="3db80-183">Ana yöntemi aşağıda gösterildiği gibi Program.cs ' de uygulayın.</span><span class="sxs-lookup"><span data-stu-id="3db80-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="3db80-184">Bu kod, bağlamımız yeni bir örnek oluşturur ve yeni bir blog eklemek için onu kullanır.</span><span class="sxs-lookup"><span data-stu-id="3db80-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="3db80-185">Daha sonra, bir LINQ sorgusu kullanarak, veritabanındaki tüm blogları başlık sırasına göre sıralanmış olarak alır.</span><span class="sxs-lookup"><span data-stu-id="3db80-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="3db80-186">Şimdi uygulamayı çalıştırabilir ve test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3db80-186">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="3db80-187">5. Model değişiklikleriyle ilgili</span><span class="sxs-lookup"><span data-stu-id="3db80-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="3db80-188">Artık modelinizde bazı değişiklikler yapma zamanı, bu değişiklikleri yaptığımız için de veritabanı şemasını güncelleştirmemiz gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="3db80-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="3db80-189">Modelinize yeni bir kullanıcı varlığı ekleyerek başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="3db80-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="3db80-190">Anahtarın Özellik türü olarak anahtar adı ve **dize** olarak **Kullanıcı** adı ile yeni bir **Kullanıcı** varlık adı ekleyin</span><span class="sxs-lookup"><span data-stu-id="3db80-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![Kullanıcı varlığı Ekle](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="3db80-192">Tasarım yüzeyinde **Kullanıcı adı** özelliğine sağ tıklayın ve **Özellikler**' i seçin Özellikler penceresi **MaxLength** ayarını **50**olarak değiştirin 
    *Bu, Kullanıcı adı 'nda depolanabilecek verileri 50 ' e kısıtlar karakterler*</span><span class="sxs-lookup"><span data-stu-id="3db80-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="3db80-193">**Kullanıcı** varlığına **DisplayName** skalar özelliği ekleyin</span><span class="sxs-lookup"><span data-stu-id="3db80-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="3db80-194">Şimdi güncelleştirilmiş bir modelimiz var ve veritabanını yeni Kullanıcı varlık türü ile uyumlu olacak şekilde güncelleştirmeye hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="3db80-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="3db80-195">Tasarım yüzeyine sağ tıklayın ve **modelden veritabanı oluştur...** ' u seçin, Entity Framework güncelleştirilmiş modele göre bir şemayı yeniden oluşturmak için bir komut dosyası hesaplar.</span><span class="sxs-lookup"><span data-stu-id="3db80-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="3db80-196">**Son** ' a tıklayın</span><span class="sxs-lookup"><span data-stu-id="3db80-196">Click **Finish**</span></span>
-   <span data-ttu-id="3db80-197">Var olan DDL betiğinin üzerine yazma ve modelin eşleme ve depolama bölümlerinin yazılmasına ilişkin uyarılar alabilirsiniz, her iki uyarı için de **Evet** ' e tıklayabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="3db80-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="3db80-198">Veritabanını oluşturmak için güncelleştirilmiş SQL betiği sizin için açıldı</span><span class="sxs-lookup"><span data-stu-id="3db80-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="3db80-199">@no__t-oluşturulan komut dosyası tüm mevcut tabloları bırakacak ve sonra şemayı sıfırdan yeniden oluşturacak. Bu, yerel geliştirme için çalışabilir, ancak daha önce dağıtılmış bir veritabanına yapılan değişiklikleri göndermek için uygun değildir. Zaten dağıtılmış bir veritabanında değişiklikler yayımlamanız gerekiyorsa, bir geçiş betiği hesaplamak için betiği düzenlemeniz veya bir şema karşılaştırma aracı kullanmanız gerekir. \*</span><span class="sxs-lookup"><span data-stu-id="3db80-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="3db80-200">Komut dosyasına sağ tıklayın ve **Yürüt**' ü seçin, hangi Visual Studio sürümüne bağlı olarak ' ye bağlanacak veritabanını belirtmeniz, LocalDB veya SQL Server Express belirtmeniz istenecektir</span><span class="sxs-lookup"><span data-stu-id="3db80-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="3db80-201">Özet</span><span class="sxs-lookup"><span data-stu-id="3db80-201">Summary</span></span>

<span data-ttu-id="3db80-202">Bu kılavuzda, EF tasarımcısında bir model oluşturmamız ve ardından bu modelden bir veritabanı oluşturması için Model First geliştirmeye baktık.</span><span class="sxs-lookup"><span data-stu-id="3db80-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="3db80-203">Daha sonra, veritabanından bazı verileri okumak ve yazmak için modeli kullandık.</span><span class="sxs-lookup"><span data-stu-id="3db80-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="3db80-204">Son olarak, modeli güncelleştirdik ve sonra modeliyle eşleşecek şekilde veritabanı şemasını yeniden oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="3db80-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
