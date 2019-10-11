---
title: Uzamsal-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: 018f480c1f0f1e74fc9f7a8950a6880e96f1facc
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182657"
---
# <a name="spatial---code-first"></a><span data-ttu-id="ecf61-102">Uzamsal-Code First</span><span class="sxs-lookup"><span data-stu-id="ecf61-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="ecf61-103">**Yalnızca EF5** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 5 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ecf61-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="ecf61-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="ecf61-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="ecf61-105">Video ve adım adım izlenecek yol, uzamsal türlerin Entity Framework Code First nasıl eşleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="ecf61-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="ecf61-106">Ayrıca, iki konum arasındaki mesafeyi bulmak için bir LINQ sorgusunun nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="ecf61-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="ecf61-107">Bu izlenecek yol, yeni bir veritabanı oluşturmak için Code First kullanır, ancak [Code First mevcut bir veritabanına](~/ef6/modeling/code-first/workflows/existing-database.md)de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ecf61-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="ecf61-108">Uzamsal tür desteği Entity Framework 5 ' te tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="ecf61-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="ecf61-109">Uzamsal tür, numaralandırmalar ve tablo değerli işlevler gibi yeni özellikleri kullanmak için, .NET Framework 4,5 ' i hedeflemelidir.</span><span class="sxs-lookup"><span data-stu-id="ecf61-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="ecf61-110">Visual Studio 2012, .NET 4,5 'i varsayılan olarak hedefler.</span><span class="sxs-lookup"><span data-stu-id="ecf61-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="ecf61-111">Uzamsal veri türlerini kullanmak için, uzamsal destek içeren bir Entity Framework sağlayıcısı da kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="ecf61-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="ecf61-112">Daha fazla bilgi için bkz. [uzamsal türler için sağlayıcı desteği](~/ef6/fundamentals/providers/spatial-support.md) .</span><span class="sxs-lookup"><span data-stu-id="ecf61-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="ecf61-113">İki ana uzamsal veri türü vardır: Coğrafya ve geometri.</span><span class="sxs-lookup"><span data-stu-id="ecf61-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="ecf61-114">Coğrafya veri türü ellipsoidal verilerini depolar (örneğin, GPS Enlem ve boylam koordinatları).</span><span class="sxs-lookup"><span data-stu-id="ecf61-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="ecf61-115">Geometri veri türü, Euclidean (düz) koordinat sistemini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ecf61-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="ecf61-116">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="ecf61-116">Watch the video</span></span>
<span data-ttu-id="ecf61-117">Bu videoda, Entity Framework Code First ile uzamsal türlerin nasıl eşleneceğini gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ecf61-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="ecf61-118">Ayrıca, iki konum arasındaki mesafeyi bulmak için bir LINQ sorgusunun nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="ecf61-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="ecf61-119">**Sunulma ölçütü**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="ecf61-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="ecf61-120">**Video**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="ecf61-120">**Video**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="ecf61-121">Önkoşulların önkoşulları</span><span class="sxs-lookup"><span data-stu-id="ecf61-121">Pre-Requisites</span></span>

<span data-ttu-id="ecf61-122">Bu yönergeyi tamamlamak için Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümünün yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ecf61-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="ecf61-123">Projeyi ayarlama</span><span class="sxs-lookup"><span data-stu-id="ecf61-123">Set up the Project</span></span>

1.  <span data-ttu-id="ecf61-124">Visual Studio 2012 'yi açın</span><span class="sxs-lookup"><span data-stu-id="ecf61-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="ecf61-125">**Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ecf61-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="ecf61-126">Sol bölmede, **Visual C @ no__t-1**' e tıklayın ve ardından **konsol** şablonunu seçin</span><span class="sxs-lookup"><span data-stu-id="ecf61-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="ecf61-127">Projenin adı olarak **SpatialCodeFirst** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="ecf61-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="ecf61-128">Code First kullanarak yeni bir model tanımlama</span><span class="sxs-lookup"><span data-stu-id="ecf61-128">Define a New Model using Code First</span></span>

<span data-ttu-id="ecf61-129">Code First geliştirme kullanılırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan .NET Framework sınıfları yazarak başlarsınız.</span><span class="sxs-lookup"><span data-stu-id="ecf61-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="ecf61-130">Aşağıdaki kod University sınıfını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="ecf61-130">The code below defines the University class.</span></span>

<span data-ttu-id="ecf61-131">Üniversitenin, Dbcoğrafya türünün Location özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="ecf61-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="ecf61-132">Dbcoğrafya türünü kullanmak için System. Data. Entity derlemesine bir başvuru eklemeniz ve ayrıca System. Data. uzamsal using ifadesini de eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="ecf61-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="ecf61-133">Program.cs dosyasını açın ve aşağıdaki using deyimlerini dosyanın en üstüne yapıştırın:</span><span class="sxs-lookup"><span data-stu-id="ecf61-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="ecf61-134">Aşağıdaki üniversite sınıfı tanımını Program.cs dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ecf61-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="ecf61-135">DbContext türetilmiş türünü tanımlayın</span><span class="sxs-lookup"><span data-stu-id="ecf61-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="ecf61-136">Varlıkları tanımlamaya ek olarak, DbContext öğesinden türeten bir sınıf tanımlamanız ve DbSet @ no__t-0TEntity @ no__t-1 özelliklerini kullanıma sunabilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="ecf61-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="ecf61-137">DbSet @ no__t-0TEntity @ no__t-1 özellikleri bağlamın modele hangi türleri dahil etmek istediğinizi bilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="ecf61-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="ecf61-138">DbContext türetilmiş türünün bir örneği, çalışma zamanı sırasında varlık nesnelerini yönetir, bu da nesneleri bir veritabanındaki verilerle doldurmayı, değişiklik izlemeyi ve kalıcı verileri veritabanına getirmeyi içerir.</span><span class="sxs-lookup"><span data-stu-id="ecf61-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="ecf61-139">DbContext ve DbSet türleri EntityFramework derlemesinde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="ecf61-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="ecf61-140">EntityFramework NuGet paketini kullanarak bu DLL 'ye bir başvuru ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="ecf61-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="ecf61-141">Çözüm Gezgini, proje adına sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ecf61-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="ecf61-142">**NuGet Paketlerini Yönet...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="ecf61-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="ecf61-143">NuGet Paketlerini Yönet iletişim kutusunda **çevrimiçi** sekmesini seçin ve **EntityFramework** paketini seçin.</span><span class="sxs-lookup"><span data-stu-id="ecf61-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="ecf61-144">**Install** 'a tıklayın</span><span class="sxs-lookup"><span data-stu-id="ecf61-144">Click **Install**</span></span>

<span data-ttu-id="ecf61-145">EntityFramework derlemesine ek olarak, System. ComponentModel. Dataek açıklama derlemesine bir başvuru de eklenir.</span><span class="sxs-lookup"><span data-stu-id="ecf61-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="ecf61-146">Program.cs dosyasının en üstüne aşağıdaki using ifadesini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ecf61-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="ecf61-147">Program.cs içinde bağlam tanımını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ecf61-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="ecf61-148">Kalıcı ve veri alma</span><span class="sxs-lookup"><span data-stu-id="ecf61-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="ecf61-149">Main yönteminin tanımlandığı Program.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="ecf61-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="ecf61-150">Aşağıdaki kodu Main işlevine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ecf61-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="ecf61-151">Kod, içeriğe iki yeni üniversite nesnesi ekler.</span><span class="sxs-lookup"><span data-stu-id="ecf61-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="ecf61-152">Uzamsal Özellikler Dbcoğrafya. FromText yöntemi kullanılarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="ecf61-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="ecf61-153">Wellknown olarak temsil edilen Coğrafya noktası yöntemine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="ecf61-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="ecf61-154">Kod daha sonra verileri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="ecf61-154">The code then saves the data.</span></span> <span data-ttu-id="ecf61-155">Daha sonra, konumu belirtilen konuma en yakın olan bir üniversite nesnesi döndüren LINQ sorgusu oluşturulur ve yürütülür.</span><span class="sxs-lookup"><span data-stu-id="ecf61-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

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

<span data-ttu-id="ecf61-156">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ecf61-156">Compile and run the application.</span></span> <span data-ttu-id="ecf61-157">Program aşağıdaki çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="ecf61-157">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="ecf61-158">Oluşturulan veritabanını görüntüleme</span><span class="sxs-lookup"><span data-stu-id="ecf61-158">View the Generated Database</span></span>

<span data-ttu-id="ecf61-159">Uygulamayı ilk kez çalıştırdığınızda, Entity Framework sizin için bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ecf61-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="ecf61-160">Visual Studio 2012 'nin yüklü olduğu için, veritabanı LocalDB örneğinde oluşturulacak.</span><span class="sxs-lookup"><span data-stu-id="ecf61-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="ecf61-161">Varsayılan olarak Entity Framework, türetilmiş bağlamın tam adı (Bu örnekte **SpatialCodeFirst. Üniversıtycontext**) sonra veritabanını adlandırır.</span><span class="sxs-lookup"><span data-stu-id="ecf61-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="ecf61-162">Mevcut veritabanının sonraki zamanları kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="ecf61-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="ecf61-163">Veritabanı oluşturulduktan sonra modelinizde herhangi bir değişiklik yaparsanız, veritabanı şemasını güncelleştirmek için Code First Migrations kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="ecf61-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="ecf61-164">Geçişleri kullanma örneği için bkz. [Code First yeni bir veritabanı](~/ef6/modeling/code-first/workflows/new-database.md) .</span><span class="sxs-lookup"><span data-stu-id="ecf61-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="ecf61-165">Veritabanını ve verileri görüntülemek için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="ecf61-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="ecf61-166">Visual Studio 2012 ana menüsünde **görüntüle** - @ no__t-2 **SQL Server Nesne Gezgini**öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="ecf61-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="ecf61-167">LocalDB sunucular listesinde yoksa, **SQL Server** sağ fare düğmesine tıklayın ve **Ekle SQL Server** ' yi seçin ve LocalDB örneğine bağlanmak Için varsayılan **Windows kimlik doğrulamasını** kullanın</span><span class="sxs-lookup"><span data-stu-id="ecf61-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="ecf61-168">LocalDB düğümünü Genişlet</span><span class="sxs-lookup"><span data-stu-id="ecf61-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="ecf61-169">Yeni veritabanını görmek ve **üniversiteler** tablosuna gitmek için **veritabanları** klasörünün sarmalamayı kaldırın</span><span class="sxs-lookup"><span data-stu-id="ecf61-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="ecf61-170">Verileri görüntülemek için tabloya sağ tıklayıp **verileri görüntüle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="ecf61-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="ecf61-171">Özet</span><span class="sxs-lookup"><span data-stu-id="ecf61-171">Summary</span></span>

<span data-ttu-id="ecf61-172">Bu kılavuzda, Entity Framework Code First ile uzamsal türleri nasıl kullanacağınızı inceledik.</span><span class="sxs-lookup"><span data-stu-id="ecf61-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 
