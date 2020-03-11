---
title: Sabit listesi desteği-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419110"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="0076f-102">Sabit listesi desteği-Code First</span><span class="sxs-lookup"><span data-stu-id="0076f-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="0076f-103">**Yalnızca EF5** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 5 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="0076f-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="0076f-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="0076f-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="0076f-105">Bu video ve adım adım yönergeler, Entity Framework Code First ile numaralandırma türlerini nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="0076f-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="0076f-106">Ayrıca, bir LINQ sorgusunda numaralandırmaları nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="0076f-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="0076f-107">Bu izlenecek yol, yeni bir veritabanı oluşturmak için Code First kullanacaktır, ancak aynı zamanda [Code First kullanarak var olan bir veritabanına eşleyebilirsiniz](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="0076f-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="0076f-108">Enum desteği Entity Framework 5 ' te tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="0076f-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="0076f-109">Numaralandırmalar, uzamsal veri türleri ve tablo değerli işlevler gibi yeni özellikleri kullanmak için, .NET Framework 4,5 ' i hedeflemelidir.</span><span class="sxs-lookup"><span data-stu-id="0076f-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="0076f-110">Visual Studio 2012, .NET 4,5 'i varsayılan olarak hedefler.</span><span class="sxs-lookup"><span data-stu-id="0076f-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="0076f-111">Entity Framework, bir numaralandırma aşağıdaki temel türlere sahip olabilir: **byte**, **Int16**, **Int32**, **Int64** veya **SByte**.</span><span class="sxs-lookup"><span data-stu-id="0076f-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="0076f-112">Videoyu izleme</span><span class="sxs-lookup"><span data-stu-id="0076f-112">Watch the video</span></span>
<span data-ttu-id="0076f-113">Bu videoda, Entity Framework Code First ile numaralandırma türlerinin nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0076f-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="0076f-114">Ayrıca, bir LINQ sorgusunda numaralandırmaları nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="0076f-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="0076f-115">**Sunduğu**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="0076f-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="0076f-116">**Video**: [wmv](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="0076f-116">**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="0076f-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="0076f-117">Pre-Requisites</span></span>

<span data-ttu-id="0076f-118">Bu yönergeyi tamamlamak için Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümünün yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0076f-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="0076f-119">Projeyi ayarlama</span><span class="sxs-lookup"><span data-stu-id="0076f-119">Set up the Project</span></span>

1.  <span data-ttu-id="0076f-120">Visual Studio 2012'yi açın</span><span class="sxs-lookup"><span data-stu-id="0076f-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="0076f-121">**Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0076f-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="0076f-122">Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin</span><span class="sxs-lookup"><span data-stu-id="0076f-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="0076f-123">Projenin adı olarak **Enumcodefırst** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="0076f-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="0076f-124">Code First kullanarak yeni bir model tanımlama</span><span class="sxs-lookup"><span data-stu-id="0076f-124">Define a New Model using Code First</span></span>

<span data-ttu-id="0076f-125">Code First geliştirme kullanılırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan .NET Framework sınıfları yazarak başlarsınız.</span><span class="sxs-lookup"><span data-stu-id="0076f-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="0076f-126">Aşağıdaki kod departman sınıfını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="0076f-126">The code below defines the Department class.</span></span>

<span data-ttu-id="0076f-127">Kod ayrıca DepartmentNames sabit listesini de tanımlar.</span><span class="sxs-lookup"><span data-stu-id="0076f-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="0076f-128">Varsayılan olarak, sabit listesi **int** türündedir.</span><span class="sxs-lookup"><span data-stu-id="0076f-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="0076f-129">Departman sınıfındaki Name özelliği DepartmentNames türüdür.</span><span class="sxs-lookup"><span data-stu-id="0076f-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="0076f-130">Program.cs dosyasını açın ve aşağıdaki sınıf tanımlarını yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="0076f-130">Open the Program.cs file and paste the following class definitions.</span></span>

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
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="0076f-131">DbContext türetilmiş türünü tanımlayın</span><span class="sxs-lookup"><span data-stu-id="0076f-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="0076f-132">Varlıkları tanımlamaya ek olarak, DbContext 'ten türeten bir sınıf tanımlamanız ve DbSet&lt;TEntity&gt; özelliklerini kullanıma sunabilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="0076f-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="0076f-133">DbSet&lt;TEntity&gt; özellikleri, bağlamın modele hangi türleri dahil etmek istediğinizi bilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="0076f-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="0076f-134">DbContext türetilmiş türünün bir örneği, çalışma zamanı sırasında varlık nesnelerini yönetir, bu da nesneleri bir veritabanındaki verilerle doldurmayı, değişiklik izlemeyi ve kalıcı verileri veritabanına getirmeyi içerir.</span><span class="sxs-lookup"><span data-stu-id="0076f-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="0076f-135">DbContext ve DbSet türleri EntityFramework derlemesinde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="0076f-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="0076f-136">EntityFramework NuGet paketini kullanarak bu DLL 'ye bir başvuru ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="0076f-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="0076f-137">Çözüm Gezgini, proje adına sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0076f-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="0076f-138">**NuGet Paketlerini Yönet...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="0076f-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="0076f-139">NuGet Paketlerini Yönet iletişim kutusunda **çevrimiçi** sekmesini seçin ve **EntityFramework** paketini seçin.</span><span class="sxs-lookup"><span data-stu-id="0076f-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="0076f-140">**Install** 'a tıklayın</span><span class="sxs-lookup"><span data-stu-id="0076f-140">Click **Install**</span></span>

<span data-ttu-id="0076f-141">EntityFramework derlemesine ek olarak System. ComponentModel. Dataaçıklamalarda ve System. Data. Entity Derlemeleriyle ilgili başvurular da eklenir.</span><span class="sxs-lookup"><span data-stu-id="0076f-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="0076f-142">Program.cs dosyasının en üstüne aşağıdaki using ifadesini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="0076f-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="0076f-143">Program.cs içinde bağlam tanımını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0076f-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="0076f-144">Kalıcı ve veri alma</span><span class="sxs-lookup"><span data-stu-id="0076f-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="0076f-145">Main yönteminin tanımlandığı Program.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="0076f-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="0076f-146">Aşağıdaki kodu Main işlevine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0076f-146">Add the following code into the Main function.</span></span> <span data-ttu-id="0076f-147">Kod, içeriğe yeni bir departman nesnesi ekler.</span><span class="sxs-lookup"><span data-stu-id="0076f-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="0076f-148">Daha sonra verileri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="0076f-148">It then saves the data.</span></span> <span data-ttu-id="0076f-149">Kod ayrıca, adın DepartmentNames. Ingilizce olduğu bir departmanı döndüren bir LINQ sorgusu yürütür.</span><span class="sxs-lookup"><span data-stu-id="0076f-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="0076f-150">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0076f-150">Compile and run the application.</span></span> <span data-ttu-id="0076f-151">Program aşağıdaki çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="0076f-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="0076f-152">Oluşturulan veritabanını görüntüleme</span><span class="sxs-lookup"><span data-stu-id="0076f-152">View the Generated Database</span></span>

<span data-ttu-id="0076f-153">Uygulamayı ilk kez çalıştırdığınızda, Entity Framework sizin için bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0076f-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="0076f-154">Visual Studio 2012 'nin yüklü olduğu için, veritabanı LocalDB örneğinde oluşturulacak.</span><span class="sxs-lookup"><span data-stu-id="0076f-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="0076f-155">Entity Framework, varsayılan olarak, türetilmiş bağlamın tam adından sonra veritabanını ( **Enumcodefırst. EnumTestContext**olan bu örnek için) adlandırır.</span><span class="sxs-lookup"><span data-stu-id="0076f-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="0076f-156">Mevcut veritabanının sonraki zamanları kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="0076f-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="0076f-157">Veritabanı oluşturulduktan sonra modelinizde herhangi bir değişiklik yaparsanız, veritabanı şemasını güncelleştirmek için Code First Migrations kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="0076f-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="0076f-158">Geçişleri kullanma örneği için bkz. [Code First yeni bir veritabanı](~/ef6/modeling/code-first/workflows/new-database.md) .</span><span class="sxs-lookup"><span data-stu-id="0076f-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="0076f-159">Veritabanını ve verileri görüntülemek için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="0076f-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="0076f-160">Visual Studio 2012 ana menüsünde **görünüm** -&gt; **SQL Server Nesne Gezgini**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="0076f-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="0076f-161">LocalDB sunucular listesinde yoksa, **SQL Server** sağ fare düğmesine tıklayın ve **Ekle SQL Server** ' yi seçin ve LocalDB örneğine bağlanmak Için varsayılan **Windows kimlik doğrulamasını** kullanın</span><span class="sxs-lookup"><span data-stu-id="0076f-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="0076f-162">LocalDB düğümünü Genişlet</span><span class="sxs-lookup"><span data-stu-id="0076f-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="0076f-163">Yeni veritabanını görmek ve **bölüm** tablosu notuna göz atarak, bu Code First sabit listesi türüyle eşleşen bir tablo oluşturmadığından, **veritabanları** klasörünün sarmalamayı kaldırın</span><span class="sxs-lookup"><span data-stu-id="0076f-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="0076f-164">Verileri görüntülemek için tabloya sağ tıklayıp **verileri görüntüle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="0076f-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="0076f-165">Özet</span><span class="sxs-lookup"><span data-stu-id="0076f-165">Summary</span></span>

<span data-ttu-id="0076f-166">Bu izlenecek yolda, Entity Framework Code First ile numaralandırma türlerini nasıl kullanacağınızı inceledik.</span><span class="sxs-lookup"><span data-stu-id="0076f-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
