---
title: Uzamsal - EF Designer - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: 04ba6e8d4a59816ca31e831b8e466cb1152a3d1b
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490221"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="64344-102">Uzamsal - EF Designer</span><span class="sxs-lookup"><span data-stu-id="64344-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="64344-103">**EF5 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 5'te kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="64344-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="64344-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="64344-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="64344-105">Video ve adım adım kılavuzu uzamsal türler Entity Framework Designer ile eşleştirme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="64344-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="64344-106">Ayrıca, LINQ sorgusu iki konum arasında bir uzaklık bulmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="64344-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="64344-107">Bu izlenecek yol modeli ilk yeni bir veritabanı oluşturmak için kullanır, ancak EF Designer ile de kullanılabilir [veritabanı ilk](~/ef6/modeling/designer/workflows/database-first.md) mevcut bir veritabanına eşlemek için iş akışı.</span><span class="sxs-lookup"><span data-stu-id="64344-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="64344-108">Uzamsal türü desteği, Entity Framework 5'te tanıtıldı.</span><span class="sxs-lookup"><span data-stu-id="64344-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="64344-109">Not uzamsal türü, sabit listeleri ve tablo değerli işlevler gibi yeni özellikleri kullanmak için .NET Framework 4.5 hedeflemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="64344-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="64344-110">Visual Studio 2012, varsayılan olarak .NET 4.5 hedefler.</span><span class="sxs-lookup"><span data-stu-id="64344-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="64344-111">Uzamsal veri türleri kullanmayı da uzamsal desteğine sahip bir varlık çerçevesi sağlayıcısına kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="64344-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="64344-112">Bkz: [uzamsal türler için sağlayıcı desteği](~/ef6/fundamentals/providers/spatial-support.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="64344-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="64344-113">İki ana uzamsal veri türü vardır: coğrafya ve geometri.</span><span class="sxs-lookup"><span data-stu-id="64344-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="64344-114">Coğrafya veri türü ellipsoidal verilerini depolayan (örneğin, GPS'i enlem ve boylam koordinatlarını).</span><span class="sxs-lookup"><span data-stu-id="64344-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="64344-115">Geometri veri türü (düz) Euclidean koordinat sistemini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="64344-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="64344-116">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="64344-116">Watch the video</span></span>
<span data-ttu-id="64344-117">Bu videoda, Entity Framework Designer uzamsal türleriyle eşleme işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="64344-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="64344-118">Ayrıca, LINQ sorgusu iki konum arasında bir uzaklık bulmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="64344-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="64344-119">**Tarafından sunulan**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="64344-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="64344-120">**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="64344-120">**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="64344-121">Ön koşullar</span><span class="sxs-lookup"><span data-stu-id="64344-121">Pre-Requisites</span></span>

<span data-ttu-id="64344-122">Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümü, bu izlenecek yolu tamamlamak için yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="64344-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="64344-123">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="64344-123">Set up the Project</span></span>

1.  <span data-ttu-id="64344-124">Visual Studio 2012'yi açın</span><span class="sxs-lookup"><span data-stu-id="64344-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="64344-125">Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**</span><span class="sxs-lookup"><span data-stu-id="64344-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="64344-126">Sol bölmede **Visual C\#** ve ardından **konsol** şablonu</span><span class="sxs-lookup"><span data-stu-id="64344-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="64344-127">Girin **SpatialEFDesigner** tıklayın ve proje adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="64344-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="64344-128">Yeni EF Designer kullanarak Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="64344-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="64344-129">Çözüm Gezgini'nde proje adına sağ tıklayın, fareyle **Ekle**ve ardından **yeni öğe**</span><span class="sxs-lookup"><span data-stu-id="64344-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="64344-130">Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde</span><span class="sxs-lookup"><span data-stu-id="64344-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="64344-131">Girin **UniversityModel.edmx** dosya adı ve ardından **Ekle**</span><span class="sxs-lookup"><span data-stu-id="64344-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="64344-132">Varlık veri modeli Sihirbazı sayfasında **boş Model** Choose Model Contents iletişim kutusunda</span><span class="sxs-lookup"><span data-stu-id="64344-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="64344-133">Tıklayın **son**</span><span class="sxs-lookup"><span data-stu-id="64344-133">Click **Finish**</span></span>

<span data-ttu-id="64344-134">Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="64344-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="64344-135">Sihirbaz, aşağıdaki eylemleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="64344-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="64344-136">Kavramsal model ve depolama modeli bunları arasındaki eşlemeyi tanımlar EnumTestModel.edmx dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="64344-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="64344-137">Oluşturulan meta veri dosyaları bütünleştirilmiş koda gömülü için Yapıt meta veri işleme özelliğini .edmx dosyasını çıkış bütünleştirilmiş kodu Ekle ayarlar.</span><span class="sxs-lookup"><span data-stu-id="64344-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="64344-138">Aşağıdaki derlemelere başvuru ekler: EntityFramework System.ComponentModel.DataAnnotations ve System.Data.Entity.</span><span class="sxs-lookup"><span data-stu-id="64344-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="64344-139">UniversityModel.tt ve UniversityModel.Context.tt dosyaları oluşturur ve bunları altında .edmx dosyasını ekler.</span><span class="sxs-lookup"><span data-stu-id="64344-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="64344-140">Bu T4 şablonu dosyaları .edmx modeli'ndeki varlıkları eşleyen POCO türleri ve türetilen DbContext türünü tanımlayan kodu oluştur</span><span class="sxs-lookup"><span data-stu-id="64344-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="64344-141">Yeni bir varlık türü Ekle</span><span class="sxs-lookup"><span data-stu-id="64344-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="64344-142">Select tasarım yüzeyinde boş bir alana sağ tıklayın **Ekle -&gt; varlık**, yeni varlık iletişim kutusu görünür.</span><span class="sxs-lookup"><span data-stu-id="64344-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="64344-143">Belirtin **University** türü için ad ve belirtin **UniversityID** için anahtar özellik adı, türü olarak bırakın **Int32**</span><span class="sxs-lookup"><span data-stu-id="64344-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="64344-144">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="64344-144">Click **OK**</span></span>
4.  <span data-ttu-id="64344-145">Varlık sağ tıklayıp **Ekle - yeni&gt; skaler özelliği**</span><span class="sxs-lookup"><span data-stu-id="64344-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="64344-146">Yeni özelliğe Yeniden Adlandır **adı**</span><span class="sxs-lookup"><span data-stu-id="64344-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="64344-147">Başka bir skaler bir özellik ekleyin ve yeniden adlandırın **konumu** Özellikler penceresini açın ve yeni özelliğin türünü değiştirme **coğrafi konum**</span><span class="sxs-lookup"><span data-stu-id="64344-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="64344-148">Modeli kaydedin ve projeyi derleyin</span><span class="sxs-lookup"><span data-stu-id="64344-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="64344-149">Oluşturma sırasında hata Listesi'nde eşlenmemiş varlıkları ve ilişkileri hakkında uyarılar görünebilir.</span><span class="sxs-lookup"><span data-stu-id="64344-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="64344-150">Veritabanı Modeli'nden seçtikten sonra hatalar kaybolur çünkü bu uyarıları gözardı edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="64344-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="64344-151">Modelden veritabanı oluştur</span><span class="sxs-lookup"><span data-stu-id="64344-151">Generate Database from Model</span></span>

<span data-ttu-id="64344-152">Şimdi biz modelini temel alan bir veritabanı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="64344-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="64344-153">Varlık Tasarımcısı seçin ve yüzey üzerinde boş bir alanı sağ **Model veritabanından oluştur**</span><span class="sxs-lookup"><span data-stu-id="64344-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="64344-154">Veritabanı Oluştur Sihirbazı'nın, veri bağlantısı iletişim kutusu Seç'e tıklayın görüntülenen **yeni bağlantı** belirt düğmesine **(localdb)\\ifadesini mssqllocaldb** vesunucuadıiçin **University** tıklayın ve veritabanı için **Tamam**</span><span class="sxs-lookup"><span data-stu-id="64344-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="64344-155">Yeni bir veritabanı oluşturmak isteyip istemediğinizi soran bir iletişim kutusu açılır, tıklayın **Evet**.</span><span class="sxs-lookup"><span data-stu-id="64344-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="64344-156">Tıklayın **sonraki** ve oluşturulan DDL bir veritabanı oluşturmak için bir tanım DDL içermeyen Ayarları iletişim kutusu Not ve Özet görüntülenir Veritabanı Oluşturma Sihirbazı'nı veri tanımlama dili (DDL) oluşturur. bir sabit listesi türü eşleyen tablo</span><span class="sxs-lookup"><span data-stu-id="64344-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="64344-157">Tıklayın **son** Son'a DDL betik yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="64344-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="64344-158">Veritabanı Oluşturma Sihirbazı'nı aşağıdakileri yapar: açılır **UniversityModel.edmx.sql** T-SQL Düzenleyicisi oluşturur deposu şeması ve eşleme bölümlerini EDMX bağlantı dizesi bilgilerini ekler App.config dosyasına dosya</span><span class="sxs-lookup"><span data-stu-id="64344-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="64344-159">T-SQL Düzenleyicisi'nde sağ fare düğmesine tıklayıp **yürütme** Connect sunucusu iletişim kutusu görüntülenirse, 2. adımdaki bağlantı bilgilerini girin ve tıklayın **Bağlan**</span><span class="sxs-lookup"><span data-stu-id="64344-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="64344-160">Oluşturulan şema görüntülemek için SQL Server Nesne Gezgini içinde veritabanı adını sağ tıklatın ve seçin **Yenile**</span><span class="sxs-lookup"><span data-stu-id="64344-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="64344-161">Kalıcı hale getirmek ve veri alma</span><span class="sxs-lookup"><span data-stu-id="64344-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="64344-162">Main yöntemi tanımlandığı Program.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="64344-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="64344-163">Ana işlevine aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="64344-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="64344-164">Kod bağlamı için iki yeni University nesneleri ekler.</span><span class="sxs-lookup"><span data-stu-id="64344-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="64344-165">Uzamsal özellikler DbGeography.FromText yöntemi kullanılarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="64344-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="64344-166">WellKnownText yöntemine geçirilen olarak temsil edilen Coğrafya noktası.</span><span class="sxs-lookup"><span data-stu-id="64344-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="64344-167">Kodu daha sonra verileri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="64344-167">The code then saves the data.</span></span> <span data-ttu-id="64344-168">Konumunda belirtilen konuma yakın olduğu University nesnesi döndüren oluşturulur ve yürütülen ardından LINQ Sorgu.</span><span class="sxs-lookup"><span data-stu-id="64344-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
    {
        Name = "School of Fine Art",
        Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
    });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                                orderby u.Location.Distance(myLocation)
                                select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

<span data-ttu-id="64344-169">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="64344-169">Compile and run the application.</span></span> <span data-ttu-id="64344-170">Program şu çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="64344-170">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="64344-171">Veritabanındaki verileri görüntülemek için SQL Server Nesne Gezgini içinde veritabanı adını sağ tıklatın ve seçin **Yenile**.</span><span class="sxs-lookup"><span data-stu-id="64344-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="64344-172">Tablo ve seçim sağ fare düğmesini tıklatıp **görünüm verilerini**.</span><span class="sxs-lookup"><span data-stu-id="64344-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="64344-173">Özet</span><span class="sxs-lookup"><span data-stu-id="64344-173">Summary</span></span>

<span data-ttu-id="64344-174">Bu izlenecek yolda Entity Framework Designer kullanarak uzamsal türler eşlemeyle ilgili bilgi ve kod içinde uzamsal türler kullanma inceledik.</span><span class="sxs-lookup"><span data-stu-id="64344-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
