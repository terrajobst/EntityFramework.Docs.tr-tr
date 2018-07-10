---
title: Tablo değerli işlev (Tvf) - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
caps.latest.revision: 3
ms.openlocfilehash: 7d652725a2655b691b03aa3f43103753fe72ede7
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914315"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="049b0-102">Tablo değerli işlev (Tvf)</span><span class="sxs-lookup"><span data-stu-id="049b0-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="049b0-103">**EF5 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 5'te kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="049b0-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="049b0-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="049b0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="049b0-105">Video ve adım adım izlenecek yol, Entity Framework Designer kullanarak tablo değerli işlev (Tvf) map işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="049b0-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="049b0-106">Ayrıca, bir LINQ Sorgu bir TVF çağırmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="049b0-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="049b0-107">Tvf şu anda yalnızca veritabanı ilk iş akışında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="049b0-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="049b0-108">TVF destek Entity Framework 5 sürümü kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="049b0-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="049b0-109">Tablo değerli işlevler, sabit listeleri ve .NET Framework 4.5 hedeflemelidir uzamsal türler gibi yeni özellikleri kullanmak için dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="049b0-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="049b0-110">Visual Studio 2012, varsayılan olarak .NET 4.5 hedefler.</span><span class="sxs-lookup"><span data-stu-id="049b0-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="049b0-111">Tvf da önemli bir fark saklı yordamlar için oldukça benzerdir: bir TVF sonucunu birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="049b0-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="049b0-112">Bir LINQ Sorgu sonuçlarını bir saklı yordam işleyemez bir TVF sonuçlardan kullanılabilir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="049b0-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="049b0-113">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="049b0-113">Watch the video</span></span>

<span data-ttu-id="049b0-114">**Tarafından sunulan**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="049b0-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="049b0-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="049b0-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="049b0-116">Ön koşullar</span><span class="sxs-lookup"><span data-stu-id="049b0-116">Pre-Requisites</span></span>

<span data-ttu-id="049b0-117">Bu izlenecek yolu tamamlamak için şunları yapmanız:</span><span class="sxs-lookup"><span data-stu-id="049b0-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="049b0-118">Yükleme [School veritabanını](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="049b0-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="049b0-119">Visual Studio'nun yeni bir sürümü yüklü</span><span class="sxs-lookup"><span data-stu-id="049b0-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="049b0-120">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="049b0-120">Set up the Project</span></span>

1.  <span data-ttu-id="049b0-121">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="049b0-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="049b0-122">Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**</span><span class="sxs-lookup"><span data-stu-id="049b0-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="049b0-123">Sol bölmede **Visual C\#** ve ardından **konsol** şablonu</span><span class="sxs-lookup"><span data-stu-id="049b0-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="049b0-124">Girin **TVF** tıklayın ve proje adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="049b0-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="049b0-125">Veritabanına bir TVF ekleme</span><span class="sxs-lookup"><span data-stu-id="049b0-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="049b0-126">Seçin **görünümü -&gt; SQL Server Nesne Gezgini**</span><span class="sxs-lookup"><span data-stu-id="049b0-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="049b0-127">LocalDB sunucuları listesinde değilse: sağ **SQL Server** seçip **SQL Server Ekle** varsayılan **Windows kimlik doğrulaması** LocalDB sunucusuna bağlanmak için</span><span class="sxs-lookup"><span data-stu-id="049b0-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="049b0-128">LocalDB düğümünü genişletin</span><span class="sxs-lookup"><span data-stu-id="049b0-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="049b0-129">Veritabanı düğümü altında School veritabanı düğümünü sağ tıklatın ve seçin **yeni sorgu...**</span><span class="sxs-lookup"><span data-stu-id="049b0-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="049b0-130">T-SQL Düzenleyicisi'nde aşağıdaki TVF tanımını yapıştırın</span><span class="sxs-lookup"><span data-stu-id="049b0-130">In T-SQL Editor, paste the following TVF definition</span></span>

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

-   <span data-ttu-id="049b0-131">T-SQL Düzenleyicisi sağ fare düğmesine tıklayıp **Yürüt**</span><span class="sxs-lookup"><span data-stu-id="049b0-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="049b0-132">School veritabanını GetStudentGradesForCourse işlevi eklenir</span><span class="sxs-lookup"><span data-stu-id="049b0-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="049b0-133">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="049b0-133">Create a Model</span></span>

1.  <span data-ttu-id="049b0-134">Çözüm Gezgini'nde proje adına sağ tıklayın, fareyle **Ekle**ve ardından **yeni öğe**</span><span class="sxs-lookup"><span data-stu-id="049b0-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="049b0-135">Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** içinde **şablonları** bölmesi</span><span class="sxs-lookup"><span data-stu-id="049b0-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="049b0-136">Girin **TVFModel.edmx** dosya adı ve ardından **Ekle**</span><span class="sxs-lookup"><span data-stu-id="049b0-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="049b0-137">Choose Model Contents iletişim kutusunda **veritabanından Oluştur**ve ardından **İleri**</span><span class="sxs-lookup"><span data-stu-id="049b0-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="049b0-138">Tıklayın **yeni bağlantı** Enter **(localdb)\\ifadesini mssqllocaldb** Enter sunucu adı metin kutusunda **Okul** adına tıklayın veritabanı için **Tamam**</span><span class="sxs-lookup"><span data-stu-id="049b0-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="049b0-139">Choose, veritabanı nesneleri iletişim kutusunun altında **tabloları** düğümünü **kişi**, **StudentGrade**, ve **kurs** tabloları</span><span class="sxs-lookup"><span data-stu-id="049b0-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="049b0-140">Seçin **GetStudentGradesForCourse** işlevi bulunan altında **saklı yordamları ve işlevleri** düğümü Not, Visual Studio 2012 ile başlayarak, varlık Tasarımcısı sayesinde toplu içeri aktarma Saklı yordamları ve işlevleri</span><span class="sxs-lookup"><span data-stu-id="049b0-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="049b0-141">Tıklayın **son**</span><span class="sxs-lookup"><span data-stu-id="049b0-141">Click **Finish**</span></span>
9.  <span data-ttu-id="049b0-142">Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="049b0-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="049b0-143">Seçtiğiniz tüm nesneleri **veritabanı nesnelerinizi seçin** iletişim kutusu, modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="049b0-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="049b0-144">Varsayılan olarak, her bir içeri aktarılan saklı yordam veya işlev sonucu şeklini otomatik olarak yeni bir karmaşık tür varlık modelinizdeki olur.</span><span class="sxs-lookup"><span data-stu-id="049b0-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="049b0-145">Ancak biz StudentGrade varlığa GetStudentGradesForCourse işlevinin sonuçlarını eşlemek istediğiniz: tasarım yüzeyi ve select sağ **Model tarayıcı** modeli tarayıcıda, seçin **işlevi içeri aktarmalar**ve çift tıklatarak **GetStudentGradesForCourse** işlevi içinde düzenleme işlevi Al iletişim kutusunda seçim **varlıkları** ve **StudentGrade**</span><span class="sxs-lookup"><span data-stu-id="049b0-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="049b0-146">Kalıcı hale getirmek ve veri alma</span><span class="sxs-lookup"><span data-stu-id="049b0-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="049b0-147">Main yöntemi tanımlandığı dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="049b0-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="049b0-148">Ana işlevine aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="049b0-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="049b0-149">Aşağıdaki kod, bir tablo değerli işlev kullanan bir sorgu oluşturmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="049b0-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="049b0-150">Sorgu sonuçları 3.5 eşit veya daha büyük bir sınıf ile ilgili öğrencilere ve ilgili kurs başlık içeren bir anonim tür içine yansıtıyor.</span><span class="sxs-lookup"><span data-stu-id="049b0-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

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

<span data-ttu-id="049b0-151">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="049b0-151">Compile and run the application.</span></span> <span data-ttu-id="049b0-152">Program şu çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="049b0-152">The program produces the following output:</span></span>

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="049b0-153">Özet</span><span class="sxs-lookup"><span data-stu-id="049b0-153">Summary</span></span>

<span data-ttu-id="049b0-154">Bu izlenecek yolda Entity Framework Designer kullanarak tablo değerli işlev (Tvf) map nasıl incelemiştik.</span><span class="sxs-lookup"><span data-stu-id="049b0-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="049b0-155">Ayrıca, bir LINQ Sorgu bir TVF çağırmak nasıl gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="049b0-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
