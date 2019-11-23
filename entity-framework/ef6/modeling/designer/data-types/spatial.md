---
title: Uzamsal-EF Tasarımcısı-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: a9c54fbc14dd02ce5d4d91449a0d5f9e72f7f0f7
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182511"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="42b8b-102">Uzamsal-EF Tasarımcısı</span><span class="sxs-lookup"><span data-stu-id="42b8b-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="42b8b-103">**Yalnızca EF5** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 5 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="42b8b-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="42b8b-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="42b8b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="42b8b-105">Video ve adım adım izlenecek yol, Entity Framework Designer uzamsal türlerin nasıl eşleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="42b8b-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="42b8b-106">Ayrıca, iki konum arasındaki mesafeyi bulmak için bir LINQ sorgusunun nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="42b8b-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="42b8b-107">Bu izlenecek yol, yeni bir veritabanı oluşturmak için Model First kullanır, ancak aynı zamanda, var olan bir veritabanına eşlemek için [Database First](~/ef6/modeling/designer/workflows/database-first.md) iş AKıŞıYLA birlikte EF Designer da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="42b8b-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="42b8b-108">Uzamsal tür desteği Entity Framework 5 ' te tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="42b8b-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="42b8b-109">Uzamsal tür, numaralandırmalar ve tablo değerli işlevler gibi yeni özellikleri kullanmak için, .NET Framework 4,5 ' i hedeflemelidir.</span><span class="sxs-lookup"><span data-stu-id="42b8b-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="42b8b-110">Visual Studio 2012, .NET 4,5 'i varsayılan olarak hedefler.</span><span class="sxs-lookup"><span data-stu-id="42b8b-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="42b8b-111">Uzamsal veri türlerini kullanmak için, uzamsal destek içeren bir Entity Framework sağlayıcısı da kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="42b8b-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="42b8b-112">Daha fazla bilgi için bkz. [uzamsal türler için sağlayıcı desteği](~/ef6/fundamentals/providers/spatial-support.md) .</span><span class="sxs-lookup"><span data-stu-id="42b8b-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="42b8b-113">İki ana uzamsal veri türü vardır: Coğrafya ve geometri.</span><span class="sxs-lookup"><span data-stu-id="42b8b-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="42b8b-114">Coğrafya veri türü ellipsoidal verilerini depolar (örneğin, GPS Enlem ve boylam koordinatları).</span><span class="sxs-lookup"><span data-stu-id="42b8b-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="42b8b-115">Geometri veri türü, Euclidean (düz) koordinat sistemini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="42b8b-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="42b8b-116">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="42b8b-116">Watch the video</span></span>
<span data-ttu-id="42b8b-117">Bu videoda, Entity Framework Designer uzamsal türlerin nasıl eşleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="42b8b-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="42b8b-118">Ayrıca, iki konum arasındaki mesafeyi bulmak için bir LINQ sorgusunun nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="42b8b-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="42b8b-119">**Sunduğu**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="42b8b-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="42b8b-120">**Video**: [wmv](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="42b8b-120">**Video**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="42b8b-121">Önkoşulların önkoşulları</span><span class="sxs-lookup"><span data-stu-id="42b8b-121">Pre-Requisites</span></span>

<span data-ttu-id="42b8b-122">Bu yönergeyi tamamlamak için Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümünün yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="42b8b-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="42b8b-123">Projeyi ayarlama</span><span class="sxs-lookup"><span data-stu-id="42b8b-123">Set up the Project</span></span>

1.  <span data-ttu-id="42b8b-124">Visual Studio 2012'yi açın</span><span class="sxs-lookup"><span data-stu-id="42b8b-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="42b8b-125">**Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="42b8b-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="42b8b-126">Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin</span><span class="sxs-lookup"><span data-stu-id="42b8b-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="42b8b-127">Projenin adı olarak **Spatialefdesigner** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="42b8b-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="42b8b-128">EF tasarımcısını kullanarak yeni model oluşturma</span><span class="sxs-lookup"><span data-stu-id="42b8b-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="42b8b-129">Çözüm Gezgini ' de proje adına sağ tıklayın, **Ekle**' nin üzerine gelin ve ardından **Yeni öğe** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="42b8b-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="42b8b-130">Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** seçin</span><span class="sxs-lookup"><span data-stu-id="42b8b-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="42b8b-131">Dosya adı için **Üniversıtymodel. edmx** girin ve ardından **Ekle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="42b8b-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="42b8b-132">Varlık Veri Modeli Sihirbazı sayfasında, model Içeriklerini Seç iletişim kutusunda **boş model** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="42b8b-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="42b8b-133">**Son** ' a tıklayın</span><span class="sxs-lookup"><span data-stu-id="42b8b-133">Click **Finish**</span></span>

<span data-ttu-id="42b8b-134">Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="42b8b-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="42b8b-135">Sihirbaz aşağıdaki eylemleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="42b8b-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="42b8b-136">Kavramsal modeli, depolama modelini ve aralarındaki eşlemeyi tanımlayan EnumTestModel. edmx dosyasını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="42b8b-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="42b8b-137">Oluşturulan meta veri dosyalarının derlemeye katıştırılması için. edmx dosyasının meta veri yapıt Işleme özelliğini çıktı derlemesine katıştırmak üzere ayarlar.</span><span class="sxs-lookup"><span data-stu-id="42b8b-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="42b8b-138">Aşağıdaki derlemelere bir başvuru ekler: EntityFramework, System. ComponentModel. Dataaçıklamalarda ve System. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="42b8b-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="42b8b-139">UniversityModel.tt ve UniversityModel.Context.tt dosyalarını oluşturur ve. edmx dosyasının altına ekler.</span><span class="sxs-lookup"><span data-stu-id="42b8b-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="42b8b-140">Bu T4 şablon dosyaları,. edmx modelindeki varlıklarla eşlenen DbContext türetilmiş türünü ve POCO türlerini tanımlayan kodu oluşturur</span><span class="sxs-lookup"><span data-stu-id="42b8b-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="42b8b-141">Yeni varlık türü Ekle</span><span class="sxs-lookup"><span data-stu-id="42b8b-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="42b8b-142">Tasarım yüzeyinde boş bir alana sağ tıklayın, **Ekle-&gt; varlık**' ı seçin, yeni varlık iletişim kutusu görüntülenir</span><span class="sxs-lookup"><span data-stu-id="42b8b-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="42b8b-143">Tür adı olarak **University** belirtin ve anahtar özellik adı Için **üniverkimliği** belirtin, türü **Int32** olarak bırakın</span><span class="sxs-lookup"><span data-stu-id="42b8b-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="42b8b-144">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="42b8b-144">Click **OK**</span></span>
4.  <span data-ttu-id="42b8b-145">Varlığa sağ tıklayın ve **New-&gt; skaler özelliği Ekle** ' yi seçin</span><span class="sxs-lookup"><span data-stu-id="42b8b-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="42b8b-146">Yeni özelliği **ad** olarak yeniden adlandır</span><span class="sxs-lookup"><span data-stu-id="42b8b-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="42b8b-147">Başka bir skaler özellik ekleyin ve **konuma** yeniden adlandırın Özellikler penceresi açın ve yeni özelliğin türünü **Coğrafya** olarak değiştirin</span><span class="sxs-lookup"><span data-stu-id="42b8b-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="42b8b-148">Modeli kaydetme ve projeyi derleme</span><span class="sxs-lookup"><span data-stu-id="42b8b-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="42b8b-149">Oluşturduğunuzda, eşlenmemiş varlıklar ve ilişkilendirmeler hakkında uyarılar Hata Listesi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="42b8b-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="42b8b-150">Bu uyarıları yoksayabilirsiniz çünkü veritabanını modelden oluşturmayı seçmemiz gerekir, ancak hatalar kaybolur.</span><span class="sxs-lookup"><span data-stu-id="42b8b-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="42b8b-151">Modelden veritabanı oluştur</span><span class="sxs-lookup"><span data-stu-id="42b8b-151">Generate Database from Model</span></span>

<span data-ttu-id="42b8b-152">Artık modeli temel alan bir veritabanı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="42b8b-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="42b8b-153">Entity Desisgner yüzeyinde boş bir alana sağ tıklayın ve **modelden veritabanı oluştur** ' u seçin.</span><span class="sxs-lookup"><span data-stu-id="42b8b-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="42b8b-154">Veritabanı Oluştur sihirbazının veri bağlantısını seçin Iletişim kutusu görüntülenir **Yeni bağlantı** düğmesine tıklayın (LocalDB) veritabanı için sunucu adı ve **üniversite** için **Mssqllocaldb\\** ve **Tamam** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="42b8b-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="42b8b-155">Yeni bir veritabanı oluşturmak isteyip istemediğinizi soran bir iletişim kutusu açılır ve **Evet**' e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="42b8b-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="42b8b-156">**İleri** ' ye tıklayın ve veritabanı oluşturma Sihirbazı bir veritabanı oluşturmak için veri tanımlama DILI (ddl) oluşturur oluşturulan DDL, Özet ve ayarlar iletişim kutusu ' nda, DDL 'nin numaralandırma türüyle eşleşen bir tablo tanımı içermediğini belirten bir açıklama içermez.</span><span class="sxs-lookup"><span data-stu-id="42b8b-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="42b8b-157">Son **' a** tıklayarak DDL betiğini yürütmez.</span><span class="sxs-lookup"><span data-stu-id="42b8b-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="42b8b-158">Veritabanı oluşturma Sihirbazı şunları yapar: **Üniversıtymodel. edmx** dosyasını açar. T-SQL DÜZENLEYICISI, edmx dosyasının depo şemasını ve eşleme bölümlerini, bağlantı dizesi bilgilerini App. config dosyasına ekler</span><span class="sxs-lookup"><span data-stu-id="42b8b-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="42b8b-159">T-SQL Düzenleyicisi ' nde sağ fare düğmesine tıklayın ve sunucuya Bağlan iletişim kutusunu **Çalıştır** ' ı seçin. Adım 2 ' den bağlantı bilgilerini girin ve **Bağlan** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="42b8b-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="42b8b-160">Oluşturulan şemayı görüntülemek için SQL Server Nesne Gezgini veritabanı adına sağ tıklayın ve **Yenile** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="42b8b-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="42b8b-161">Kalıcı ve veri alma</span><span class="sxs-lookup"><span data-stu-id="42b8b-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="42b8b-162">Main yönteminin tanımlandığı Program.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="42b8b-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="42b8b-163">Aşağıdaki kodu Main işlevine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="42b8b-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="42b8b-164">Kod, içeriğe iki yeni üniversite nesnesi ekler.</span><span class="sxs-lookup"><span data-stu-id="42b8b-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="42b8b-165">Uzamsal Özellikler Dbcoğrafya. FromText yöntemi kullanılarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="42b8b-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="42b8b-166">Wellknown olarak temsil edilen Coğrafya noktası yöntemine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="42b8b-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="42b8b-167">Kod daha sonra verileri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="42b8b-167">The code then saves the data.</span></span> <span data-ttu-id="42b8b-168">Daha sonra, konumu belirtilen konuma en yakın olan bir üniversite nesnesi döndüren LINQ sorgusu oluşturulur ve yürütülür.</span><span class="sxs-lookup"><span data-stu-id="42b8b-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

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

<span data-ttu-id="42b8b-169">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="42b8b-169">Compile and run the application.</span></span> <span data-ttu-id="42b8b-170">Program aşağıdaki çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="42b8b-170">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="42b8b-171">Veritabanındaki verileri görüntülemek için SQL Server Nesne Gezgini veritabanı adına sağ tıklayın ve **Yenile**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="42b8b-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="42b8b-172">Ardından, tablodaki sağ fare düğmesine tıklayın ve **verileri görüntüle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="42b8b-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="42b8b-173">Özet</span><span class="sxs-lookup"><span data-stu-id="42b8b-173">Summary</span></span>

<span data-ttu-id="42b8b-174">Bu izlenecek yolda, Entity Framework Designer kullanarak uzamsal türlerin nasıl eşlendiğini ve koddaki uzamsal türlerin nasıl kullanılacağını inceledik.</span><span class="sxs-lookup"><span data-stu-id="42b8b-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
