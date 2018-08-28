---
title: Uzamsal - EF6 öncelikle - kod
author: divega
ms.date: 2016-10-23
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: d15b407203a7dd7ddf92d0759af00f3ad5e41f57
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995218"
---
# <a name="spatial---code-first"></a><span data-ttu-id="0fbac-102">Uzamsal - Code First</span><span class="sxs-lookup"><span data-stu-id="0fbac-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="0fbac-103">**EF5 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 5'te kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="0fbac-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="0fbac-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="0fbac-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="0fbac-105">Video ve adım adım izlenecek yol, Entity Framework Code First uzamsal türleriyle eşleme işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0fbac-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="0fbac-106">Ayrıca, LINQ sorgusu iki konum arasında bir uzaklık bulmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="0fbac-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="0fbac-107">Bu izlenecek yolda Code First yeni bir veritabanı oluşturmak için kullanır, ancak ayrıca [var olan bir veritabanına ilk kod](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="0fbac-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="0fbac-108">Uzamsal türü desteği, Entity Framework 5'te tanıtıldı.</span><span class="sxs-lookup"><span data-stu-id="0fbac-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="0fbac-109">Not uzamsal türü, sabit listeleri ve tablo değerli işlevler gibi yeni özellikleri kullanmak için .NET Framework 4.5 hedeflemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="0fbac-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="0fbac-110">Visual Studio 2012, varsayılan olarak .NET 4.5 hedefler.</span><span class="sxs-lookup"><span data-stu-id="0fbac-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="0fbac-111">Uzamsal veri türleri kullanmayı da uzamsal desteğine sahip bir varlık çerçevesi sağlayıcısına kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="0fbac-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="0fbac-112">Bkz: [uzamsal türler için sağlayıcı desteği](~/ef6/fundamentals/providers/spatial-support.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="0fbac-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="0fbac-113">İki ana uzamsal veri türü vardır: coğrafya ve geometri.</span><span class="sxs-lookup"><span data-stu-id="0fbac-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="0fbac-114">Coğrafya veri türü ellipsoidal verilerini depolayan (örneğin, GPS'i enlem ve boylam koordinatlarını).</span><span class="sxs-lookup"><span data-stu-id="0fbac-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="0fbac-115">Geometri veri türü (düz) Euclidean koordinat sistemini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="0fbac-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="0fbac-116">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="0fbac-116">Watch the video</span></span>
<span data-ttu-id="0fbac-117">Bu videoda, Entity Framework Code First uzamsal türleriyle eşleme işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0fbac-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="0fbac-118">Ayrıca, LINQ sorgusu iki konum arasında bir uzaklık bulmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="0fbac-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="0fbac-119">**Tarafından sunulan**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="0fbac-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="0fbac-120">**Video**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="0fbac-120">**Video**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="0fbac-121">Ön koşullar</span><span class="sxs-lookup"><span data-stu-id="0fbac-121">Pre-Requisites</span></span>

<span data-ttu-id="0fbac-122">Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümü, bu izlenecek yolu tamamlamak için yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0fbac-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="0fbac-123">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="0fbac-123">Set up the Project</span></span>

1.  <span data-ttu-id="0fbac-124">Visual Studio 2012'yi açın</span><span class="sxs-lookup"><span data-stu-id="0fbac-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="0fbac-125">Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**</span><span class="sxs-lookup"><span data-stu-id="0fbac-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="0fbac-126">Sol bölmede **Visual C\#** ve ardından **konsol** şablonu</span><span class="sxs-lookup"><span data-stu-id="0fbac-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="0fbac-127">Girin **SpatialCodeFirst** tıklayın ve proje adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="0fbac-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="0fbac-128">Code First kullanarak yeni bir modeli tanımlamak</span><span class="sxs-lookup"><span data-stu-id="0fbac-128">Define a New Model using Code First</span></span>

<span data-ttu-id="0fbac-129">Code First geliştirme kullanırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan bir .NET Framework sınıf yazarak başlayın.</span><span class="sxs-lookup"><span data-stu-id="0fbac-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="0fbac-130">Aşağıdaki kod, University sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="0fbac-130">The code below defines the University class.</span></span>

<span data-ttu-id="0fbac-131">University DbGeography tür konumu özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="0fbac-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="0fbac-132">DbGeography türünü kullanmak üzere System.Data.Entity derlemeye bir başvuruda bulunun ve ayrıca using deyimi System.Data.Spatial eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="0fbac-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="0fbac-133">Program.cs dosyasını açın ve aşağıdakini yapıştırın dosyasının en üstüne using deyimlerini:</span><span class="sxs-lookup"><span data-stu-id="0fbac-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="0fbac-134">Program.cs dosyasına aşağıdaki University sınıf tanımını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0fbac-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="0fbac-135">Tanımlama DbContext türetilmiş türü</span><span class="sxs-lookup"><span data-stu-id="0fbac-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="0fbac-136">Varlıklar tanımlayan yanı sıra olan DB kullanıma sunar ve DbContext türeyen bir sınıfı tanımlamanız gereken&lt;TEntity&gt; özellikleri.</span><span class="sxs-lookup"><span data-stu-id="0fbac-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="0fbac-137">Olan DB&lt;TEntity&gt; özellikleri modele dahil etmek istediğiniz hangi türlerin bilmeniz bağlam olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="0fbac-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="0fbac-138">Nesneleri bir veritabanındaki verilerle doldurma içeren çalışma zamanı sırasında varlık nesnesi DbContext türetilmiş türün bir örneğini yönetir, izleme ve kalıcı veri veritabanına değiştirin.</span><span class="sxs-lookup"><span data-stu-id="0fbac-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="0fbac-139">DbContext ve olan DB türleri EntityFramework derlemede tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="0fbac-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="0fbac-140">EntityFramework NuGet paketini kullanarak bu DLL'ye bir başvuru ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="0fbac-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="0fbac-141">Çözüm Gezgini'nde proje adına sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0fbac-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="0fbac-142">Seçin **NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="0fbac-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="0fbac-143">NuGet paketlerini Yönet iletişim kutusunda, seçmek **çevrimiçi** sekmesini **EntityFramework** paket.</span><span class="sxs-lookup"><span data-stu-id="0fbac-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="0fbac-144">Tıklayın **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="0fbac-144">Click **Install**</span></span>

<span data-ttu-id="0fbac-145">EntityFramework derleme yanı sıra System.ComponentModel.DataAnnotations derlemesine bir başvuru da eklendiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="0fbac-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="0fbac-146">Program.cs dosyasının en üstüne aşağıdakileri ekleyin using deyimi:</span><span class="sxs-lookup"><span data-stu-id="0fbac-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="0fbac-147">Program.cs içinde içerik tanımı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0fbac-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="0fbac-148">Kalıcı hale getirmek ve veri alma</span><span class="sxs-lookup"><span data-stu-id="0fbac-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="0fbac-149">Main yöntemi tanımlandığı Program.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="0fbac-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="0fbac-150">Ana işlevine aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0fbac-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="0fbac-151">Kod bağlamı için iki yeni University nesneleri ekler.</span><span class="sxs-lookup"><span data-stu-id="0fbac-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="0fbac-152">Uzamsal özellikler DbGeography.FromText yöntemi kullanılarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="0fbac-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="0fbac-153">WellKnownText yöntemine geçirilen olarak temsil edilen Coğrafya noktası.</span><span class="sxs-lookup"><span data-stu-id="0fbac-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="0fbac-154">Kodu daha sonra verileri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="0fbac-154">The code then saves the data.</span></span> <span data-ttu-id="0fbac-155">Konumunda belirtilen konuma yakın olduğu University nesnesi döndüren oluşturulur ve yürütülen ardından LINQ Sorgu.</span><span class="sxs-lookup"><span data-stu-id="0fbac-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
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

<span data-ttu-id="0fbac-156">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0fbac-156">Compile and run the application.</span></span> <span data-ttu-id="0fbac-157">Program şu çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="0fbac-157">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="0fbac-158">Oluşturulan veritabanı görünümü</span><span class="sxs-lookup"><span data-stu-id="0fbac-158">View the Generated Database</span></span>

<span data-ttu-id="0fbac-159">Uygulamayı ilk kez çalıştırdığınızda, Entity Framework sizin için bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0fbac-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="0fbac-160">Biz Visual Studio 2012 yüklü olduğundan LocalDB örneğinde bir veritabanı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0fbac-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="0fbac-161">Varsayılan olarak Entity Framework veritabanı türetilmiş bağlamı tam olarak nitelenmiş adını sonra adları (Bu örnekte, **SpatialCodeFirst.UniversityContext**).</span><span class="sxs-lookup"><span data-stu-id="0fbac-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="0fbac-162">Varolan bir veritabanını kullanılacak sonraki saatleri.</span><span class="sxs-lookup"><span data-stu-id="0fbac-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="0fbac-163">Veritabanı oluşturulduktan sonra modelinize herhangi bir değişiklik yaparsanız, Code First Migrations veritabanı şemasını güncelleştirmek için kullanmanızı unutmayın.</span><span class="sxs-lookup"><span data-stu-id="0fbac-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="0fbac-164">Bkz: [yeni veritabanına Code First](~/ef6/modeling/code-first/workflows/new-database.md) geçişleri kullanma örneği için.</span><span class="sxs-lookup"><span data-stu-id="0fbac-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="0fbac-165">Veritabanını ve verileri görüntülemek için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="0fbac-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="0fbac-166">Visual Studio 2012 ana menüde **görünümü**  - &gt; **SQL Server Nesne Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="0fbac-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="0fbac-167">LocalDB sunucuları listesinde değilse, sağ fare düğmesine tıklayın **SQL Server** seçip **SQL Server Ekle** varsayılan **Windows kimlik doğrulaması** bağlanmak için LocalDB örneğini</span><span class="sxs-lookup"><span data-stu-id="0fbac-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="0fbac-168">LocalDB düğümünü genişletin</span><span class="sxs-lookup"><span data-stu-id="0fbac-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="0fbac-169">Unfold **veritabanları** göz atın ve yeni veritabanı görmek için klasör **üniversiteler** tablo</span><span class="sxs-lookup"><span data-stu-id="0fbac-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="0fbac-170">Verileri görüntülemek, tablonun sağ tıklayın ve seçmek için **verileri görüntüleme**</span><span class="sxs-lookup"><span data-stu-id="0fbac-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="0fbac-171">Özet</span><span class="sxs-lookup"><span data-stu-id="0fbac-171">Summary</span></span>

<span data-ttu-id="0fbac-172">Bu izlenecek yolda uzamsal türler ile Entity Framework Code First kullanma inceledik.</span><span class="sxs-lookup"><span data-stu-id="0fbac-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 
