---
title: Numaralandırma desteği - EF6 öncelikle - kod
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
caps.latest.revision: 3
ms.openlocfilehash: 698c84a9f0679c67a086574de2f253f214fb8d0b
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914111"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="1b686-102">Numaralandırma desteği - kod önce</span><span class="sxs-lookup"><span data-stu-id="1b686-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="1b686-103">**EF5 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 5'te kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1b686-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="1b686-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="1b686-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="1b686-105">Bu video ve adım adım izlenecek yol, numaralandırma türleri ile Entity Framework Code First işlemi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="1b686-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="1b686-106">Ayrıca, bir LINQ Sorgu numaralandırmalar kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="1b686-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="1b686-107">Bu izlenecek yolda Code First yeni bir veritabanı oluşturmak için kullanır, ancak ayrıca [mevcut bir veritabanına eşlemek için Code First](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="1b686-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="1b686-108">Numaralandırma desteği, Entity Framework 5'te tanıtıldı.</span><span class="sxs-lookup"><span data-stu-id="1b686-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="1b686-109">Numaralandırmalar, uzamsal veri türleri ve tablo değerli işlevler gibi yeni özellikleri kullanmak için .NET Framework 4.5 hedeflemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="1b686-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="1b686-110">Visual Studio 2012, varsayılan olarak .NET 4.5 hedefler.</span><span class="sxs-lookup"><span data-stu-id="1b686-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="1b686-111">Varlık Çerçevesi'nde bir numaralandırma aşağıdaki temel alınan türler sahip olabilir: **bayt**, **Int16**, **Int32**, **Int64** , veya **SByte**.</span><span class="sxs-lookup"><span data-stu-id="1b686-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="1b686-112">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="1b686-112">Watch the video</span></span>
<span data-ttu-id="1b686-113">Bu video, numaralandırma türleri ile Entity Framework Code First kullanma işlemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="1b686-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="1b686-114">Ayrıca, bir LINQ Sorgu numaralandırmalar kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="1b686-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="1b686-115">**Tarafından sunulan**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="1b686-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="1b686-116">**Video**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="1b686-116">**Video**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="1b686-117">Ön koşullar</span><span class="sxs-lookup"><span data-stu-id="1b686-117">Pre-Requisites</span></span>

<span data-ttu-id="1b686-118">Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümü, bu izlenecek yolu tamamlamak için yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1b686-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="1b686-119">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="1b686-119">Set up the Project</span></span>

1.  <span data-ttu-id="1b686-120">Visual Studio 2012'yi açın</span><span class="sxs-lookup"><span data-stu-id="1b686-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="1b686-121">Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**</span><span class="sxs-lookup"><span data-stu-id="1b686-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="1b686-122">Sol bölmede **Visual C\#** ve ardından **konsol** şablonu</span><span class="sxs-lookup"><span data-stu-id="1b686-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="1b686-123">Girin **EnumCodeFirst** tıklayın ve proje adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="1b686-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="1b686-124">Code First kullanarak yeni bir modeli tanımlamak</span><span class="sxs-lookup"><span data-stu-id="1b686-124">Define a New Model using Code First</span></span>

<span data-ttu-id="1b686-125">Code First geliştirme kullanırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan bir .NET Framework sınıf yazarak başlayın.</span><span class="sxs-lookup"><span data-stu-id="1b686-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="1b686-126">Aşağıdaki kod, departman sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="1b686-126">The code below defines the Department class.</span></span>

<span data-ttu-id="1b686-127">Kod ayrıca DepartmentNames sabit listesi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="1b686-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="1b686-128">Varsayılan olarak, sabit listesidir. **int** türü.</span><span class="sxs-lookup"><span data-stu-id="1b686-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="1b686-129">Departman sınıfı adı özelliği DepartmentNames türüdür.</span><span class="sxs-lookup"><span data-stu-id="1b686-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="1b686-130">Program.cs dosyasını açın ve aşağıdaki sınıf tanımları yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="1b686-130">Open the Program.cs file and paste the following class definitions.</span></span>

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="1b686-131">Tanımlama DbContext türetilmiş türü</span><span class="sxs-lookup"><span data-stu-id="1b686-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="1b686-132">Varlıklar tanımlayan yanı sıra olan DB kullanıma sunar ve DbContext türeyen bir sınıfı tanımlamanız gereken&lt;TEntity&gt; özellikleri.</span><span class="sxs-lookup"><span data-stu-id="1b686-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="1b686-133">Olan DB&lt;TEntity&gt; özellikleri modele dahil etmek istediğiniz hangi türlerin bilmeniz bağlam olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="1b686-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="1b686-134">Nesneleri bir veritabanındaki verilerle doldurma içeren çalışma zamanı sırasında varlık nesnesi DbContext türetilmiş türün bir örneğini yönetir, izleme ve kalıcı veri veritabanına değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1b686-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="1b686-135">DbContext ve olan DB türleri EntityFramework derlemede tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="1b686-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="1b686-136">EntityFramework NuGet paketini kullanarak bu DLL'ye bir başvuru ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="1b686-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="1b686-137">Çözüm Gezgini'nde proje adına sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1b686-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="1b686-138">Seçin **NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="1b686-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="1b686-139">NuGet paketlerini Yönet iletişim kutusunda, seçmek **çevrimiçi** sekmesini **EntityFramework** paket.</span><span class="sxs-lookup"><span data-stu-id="1b686-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="1b686-140">Tıklayın **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="1b686-140">Click **Install**</span></span>

<span data-ttu-id="1b686-141">EntityFramework derlemeye ek olarak System.ComponentModel.DataAnnotations ve System.Data.Entity derlemelere başvuruları de eklendiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1b686-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="1b686-142">Program.cs dosyasının en üstüne aşağıdakileri ekleyin using deyimi:</span><span class="sxs-lookup"><span data-stu-id="1b686-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="1b686-143">Program.cs içinde içerik tanımı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1b686-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="1b686-144">Kalıcı hale getirmek ve veri alma</span><span class="sxs-lookup"><span data-stu-id="1b686-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="1b686-145">Main yöntemi tanımlandığı Program.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="1b686-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="1b686-146">Ana işlevine aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1b686-146">Add the following code into the Main function.</span></span> <span data-ttu-id="1b686-147">Kod bağlamı için yeni bir bölüm nesnesi ekler.</span><span class="sxs-lookup"><span data-stu-id="1b686-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="1b686-148">Ardından, verileri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="1b686-148">It then saves the data.</span></span> <span data-ttu-id="1b686-149">Kod ayrıca bir bölüm adı DepartmentNames.English olduğu döndüren LINQ sorgusu yürütür.</span><span class="sxs-lookup"><span data-stu-id="1b686-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="1b686-150">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1b686-150">Compile and run the application.</span></span> <span data-ttu-id="1b686-151">Program şu çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="1b686-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="1b686-152">Oluşturulan veritabanı görünümü</span><span class="sxs-lookup"><span data-stu-id="1b686-152">View the Generated Database</span></span>

<span data-ttu-id="1b686-153">Uygulamayı ilk kez çalıştırdığınızda, Entity Framework sizin için bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1b686-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="1b686-154">Biz Visual Studio 2012 yüklü olduğundan LocalDB örneğinde bir veritabanı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1b686-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="1b686-155">Varsayılan olarak Entity Framework veritabanı türetilmiş bağlamı tam olarak nitelenmiş adını sonra adları (örneğin, bu **EnumCodeFirst.EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="1b686-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="1b686-156">Varolan bir veritabanını kullanılacak sonraki saatleri.</span><span class="sxs-lookup"><span data-stu-id="1b686-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="1b686-157">Veritabanı oluşturulduktan sonra modelinize herhangi bir değişiklik yaparsanız, Code First Migrations veritabanı şemasını güncelleştirmek için kullanmanızı unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1b686-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="1b686-158">Bkz: [yeni veritabanına Code First](~/ef6/modeling/code-first/workflows/new-database.md) geçişleri kullanma örneği için.</span><span class="sxs-lookup"><span data-stu-id="1b686-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="1b686-159">Veritabanını ve verileri görüntülemek için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="1b686-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="1b686-160">Visual Studio 2012 ana menüde **görünümü**  - &gt; **SQL Server Nesne Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="1b686-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="1b686-161">LocalDB sunucuları listesinde değilse, sağ fare düğmesine tıklayın **SQL Server** seçip **SQL Server Ekle** varsayılan **Windows kimlik doğrulaması** bağlanmak için LocalDB örneğini</span><span class="sxs-lookup"><span data-stu-id="1b686-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="1b686-162">LocalDB düğümünü genişletin</span><span class="sxs-lookup"><span data-stu-id="1b686-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="1b686-163">Unfold **veritabanları** göz atın ve yeni veritabanı görmek için klasör **departmanı** Code First sabit listesi türü eşleyen bir tablo oluşturmaz unutmayın, tablo</span><span class="sxs-lookup"><span data-stu-id="1b686-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="1b686-164">Verileri görüntülemek, tablonun sağ tıklayın ve seçmek için **verileri görüntüleme**</span><span class="sxs-lookup"><span data-stu-id="1b686-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="1b686-165">Özet</span><span class="sxs-lookup"><span data-stu-id="1b686-165">Summary</span></span>

<span data-ttu-id="1b686-166">Bu izlenecek yolda Numaralandırma türleri ile Entity Framework Code First kullanma inceledik.</span><span class="sxs-lookup"><span data-stu-id="1b686-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
