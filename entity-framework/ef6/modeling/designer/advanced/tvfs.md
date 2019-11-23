---
title: Tablo değerli Işlevler (TVFs)-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 35684196dcd7b708a8feeb1eca3096e8d4e555ec
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182531"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="131ef-102">Tablo değerli Işlevler (TVFs)</span><span class="sxs-lookup"><span data-stu-id="131ef-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="131ef-103">**Yalnızca EF5** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 5 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="131ef-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="131ef-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="131ef-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="131ef-105">Video ve adım adım izlenecek yol, Entity Framework Designer kullanarak tablo değerli işlevlerin (TVFs) nasıl eşlendiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="131ef-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="131ef-106">Ayrıca, bir LINQ sorgusundan bir TVF 'nin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="131ef-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="131ef-107">TVFs Şu anda yalnızca Database First iş akışında desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="131ef-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="131ef-108">TVF desteği Entity Framework sürüm 5 ' te tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="131ef-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="131ef-109">Tablo değerli işlevler, numaralandırmalar ve uzamsal türler gibi yeni özellikleri kullanmak için .NET Framework 4,5 ' i hedeflemelidir.</span><span class="sxs-lookup"><span data-stu-id="131ef-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="131ef-110">Visual Studio 2012, .NET 4,5 'i varsayılan olarak hedefler.</span><span class="sxs-lookup"><span data-stu-id="131ef-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="131ef-111">TVFs, tek bir anahtar farklılığı olan saklı yordamlara çok benzer: bir TVF 'nin sonucu birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="131ef-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="131ef-112">Bu, bir TVF 'nin sonuçlarının bir LINQ sorgusunda kullanılabileceği anlamına gelir, çünkü saklı yordamın sonuçları.</span><span class="sxs-lookup"><span data-stu-id="131ef-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="131ef-113">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="131ef-113">Watch the video</span></span>

<span data-ttu-id="131ef-114">**Sunduğu**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="131ef-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="131ef-115">[Wmv](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="131ef-115">[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="131ef-116">Önkoşulların önkoşulları</span><span class="sxs-lookup"><span data-stu-id="131ef-116">Pre-Requisites</span></span>

<span data-ttu-id="131ef-117">Bu yönergeyi tamamlamak için şunları yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="131ef-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="131ef-118">[Okul veritabanını](~/ef6/resources/school-database.md)yükler.</span><span class="sxs-lookup"><span data-stu-id="131ef-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="131ef-119">Visual Studio 'nun yeni bir sürümüne sahip</span><span class="sxs-lookup"><span data-stu-id="131ef-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="131ef-120">Projeyi ayarlama</span><span class="sxs-lookup"><span data-stu-id="131ef-120">Set up the Project</span></span>

1.  <span data-ttu-id="131ef-121">Visual Studio 'Yu aç</span><span class="sxs-lookup"><span data-stu-id="131ef-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="131ef-122">**Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="131ef-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="131ef-123">Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin</span><span class="sxs-lookup"><span data-stu-id="131ef-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="131ef-124">Projenin adı olarak **TVF** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="131ef-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="131ef-125">Veritabanına bir TVF ekleyin</span><span class="sxs-lookup"><span data-stu-id="131ef-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="131ef-126">**Görünüm-&gt; SQL Server Nesne Gezgini** seçin</span><span class="sxs-lookup"><span data-stu-id="131ef-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="131ef-127">LocalDB sunucu listesinde değilse: **SQL Server** sağ tıklayın ve **Ekle SQL Server** ' yi seçin ve LocalDB sunucusuna bağlanmak Için varsayılan **Windows kimlik doğrulamasını** kullanın</span><span class="sxs-lookup"><span data-stu-id="131ef-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="131ef-128">LocalDB düğümünü Genişlet</span><span class="sxs-lookup"><span data-stu-id="131ef-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="131ef-129">Veritabanları düğümü altında, okul veritabanı düğümüne sağ tıklayın ve yeni sorgu ' yı seçin. **..**</span><span class="sxs-lookup"><span data-stu-id="131ef-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="131ef-130">T-SQL düzenleyicisinde, aşağıdaki TVF tanımını yapıştırın</span><span class="sxs-lookup"><span data-stu-id="131ef-130">In T-SQL Editor, paste the following TVF definition</span></span>

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   <span data-ttu-id="131ef-131">T-SQL düzenleyicisinde sağ fare düğmesine tıklayın ve **Yürüt** ' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="131ef-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="131ef-132">Getstudentgradesforkurs işlevi okul veritabanına eklendi</span><span class="sxs-lookup"><span data-stu-id="131ef-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="131ef-133">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="131ef-133">Create a Model</span></span>

1.  <span data-ttu-id="131ef-134">Çözüm Gezgini ' de proje adına sağ tıklayın, **Ekle**' nin üzerine gelin ve ardından **Yeni öğe** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="131ef-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="131ef-135">Sol menüden **verileri** seçin ve ardından **şablonlar** bölmesinde **ADO.net varlık veri modeli** seçin</span><span class="sxs-lookup"><span data-stu-id="131ef-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="131ef-136">Dosya adı için **Tvfmodel. edmx** yazın ve ardından **Ekle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="131ef-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="131ef-137">Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından **İleri** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="131ef-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="131ef-138">Sunucu adı metin kutusuna **Yeni bağlantı** gir **(localdb)\\mssqllocaldb** ' ye tıklayın, veritabanı adı için **okul** girin, **Tamam** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="131ef-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="131ef-139">Veritabanı nesnelerinizi seçin iletişim kutusunda, **tablolar** düğümü altında, **kişiyi**, **studentgrad**' ı ve **Kurs** tabloları ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="131ef-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="131ef-140"> **Saklı yordamlar ve işlevler** , Visual Studio 2012 ' den başlayarak, Entity Desisgner, saklı yordamlarınızı ve işlevlerinizi Toplu içe aktarmanıza olanak sağlayan saklı yordamlar ve işlevler altında bulunan **Getstudentgradesforkurs** işlevini seçin</span><span class="sxs-lookup"><span data-stu-id="131ef-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="131ef-141"> **Son** ' a tıklayın</span><span class="sxs-lookup"><span data-stu-id="131ef-141">Click **Finish**</span></span>
9.  <span data-ttu-id="131ef-142">Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="131ef-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="131ef-143"> **Veritabanı nesnelerinizi seçin** iletişim kutusunda seçtiğiniz tüm nesneler modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="131ef-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="131ef-144">Varsayılan olarak, içeri aktarılan her saklı yordamın veya işlevin sonuç şekli, varlık modelinizde otomatik olarak yeni bir karmaşık tür olur.</span><span class="sxs-lookup"><span data-stu-id="131ef-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="131ef-145">Ancak Getstudentgradesforkurs işlevinin sonuçlarını Studentgrad varlığına eşlemek istiyoruz: tasarım yüzeyine sağ tıklayın ve model **tarayıcısı 'nda model tarayıcısı '** nı seçin, **işlev içeri aktarmalar**' ı seçin ve ardından **Getstudentgradesforkurs** Işlevine çift tıklayarak Işlev içeri aktarmayı Düzenle Iletişim kutusunda **varlık** seçin ve **studentgrad** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="131ef-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="131ef-146">Kalıcı ve veri alma</span><span class="sxs-lookup"><span data-stu-id="131ef-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="131ef-147">Main yönteminin tanımlandığı dosyayı açın.</span><span class="sxs-lookup"><span data-stu-id="131ef-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="131ef-148">Aşağıdaki kodu Main işlevine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="131ef-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="131ef-149">Aşağıdaki kod, tablo değerli bir Işlev kullanan bir sorgunun nasıl oluşturulacağını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="131ef-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="131ef-150">Sorgu sonuçları, ilgili kurs başlığını ve 3,5 değerine eşit veya daha büyük bir değere sahip olan ilgili öğrencileri içeren anonim bir tür olarak projeler.</span><span class="sxs-lookup"><span data-stu-id="131ef-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

<span data-ttu-id="131ef-151">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="131ef-151">Compile and run the application.</span></span> <span data-ttu-id="131ef-152">Program aşağıdaki çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="131ef-152">The program produces the following output:</span></span>

```console
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="131ef-153">Özet</span><span class="sxs-lookup"><span data-stu-id="131ef-153">Summary</span></span>

<span data-ttu-id="131ef-154">Bu izlenecek yolda, Entity Framework Designer kullanarak tablo değerli Işlevlerin (TVFs) nasıl eşlendiğini inceledik.</span><span class="sxs-lookup"><span data-stu-id="131ef-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="131ef-155">Ayrıca, bir LINQ sorgusundan bir TVF çağırma gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="131ef-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
