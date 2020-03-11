---
title: Tasarımcı sorgusu saklı yordamları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 2e0092b526278597e8477d47eeb642598647bb91
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418394"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="836be-102">Tasarımcı sorgusu saklı yordamları</span><span class="sxs-lookup"><span data-stu-id="836be-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="836be-103">Bu adım adım izlenecek yol, saklı yordamları bir modele aktarmak için Entity Framework Designer (EF Designer) kullanmayı ve ardından sonuçları almak için içeri aktarılan saklı yordamları çağırmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="836be-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="836be-104">Code First, saklı yordamlara veya işlevlere eşlemeyi desteklemediğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="836be-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="836be-105">Ancak, System. Data. Entity. DbSet. SqlQuery yöntemini kullanarak saklı yordamları veya işlevleri çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="836be-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="836be-106">Örnek:</span><span class="sxs-lookup"><span data-stu-id="836be-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="836be-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="836be-107">Prerequisites</span></span>

<span data-ttu-id="836be-108">Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:</span><span class="sxs-lookup"><span data-stu-id="836be-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="836be-109">Visual Studio 'nun son sürümü.</span><span class="sxs-lookup"><span data-stu-id="836be-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="836be-110">[Okul örnek veritabanı](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="836be-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="836be-111">Projeyi ayarlama</span><span class="sxs-lookup"><span data-stu-id="836be-111">Set up the Project</span></span>

-   <span data-ttu-id="836be-112">Visual Studio 2012'yi açın.</span><span class="sxs-lookup"><span data-stu-id="836be-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="836be-113">**Dosya&gt; yeni&gt; proje** ' yi seçin</span><span class="sxs-lookup"><span data-stu-id="836be-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="836be-114">Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="836be-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="836be-115">Ad olarak **Efwithsprocssample** girin.</span><span class="sxs-lookup"><span data-stu-id="836be-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="836be-116"> **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="836be-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="836be-117">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="836be-117">Create a Model</span></span>

-   <span data-ttu-id="836be-118">Çözüm Gezgini projeye sağ tıklayın ve **Ekle-&gt; yeni öğe**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="836be-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="836be-119">Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="836be-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="836be-120">Dosya adı için **Efwithsprocsmodel. edmx** yazın ve ardından **Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="836be-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="836be-121">Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="836be-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="836be-122"> **Yeni bağlantı**' ya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="836be-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="836be-123">Bağlantı özellikleri iletişim kutusunda sunucu adını (örneğin, **(LocalDB)\\mssqllocaldb**) girin, kimlik doğrulama yöntemini seçin, veritabanı adı için **okul** yazın ve ardından **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="836be-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="836be-124">Veri bağlantınızı seçin iletişim kutusu, veritabanı bağlantı ayarınız ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="836be-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="836be-125">Veritabanı nesnelerinizi seçin iletişim kutusunda tablolar onay kutusunu işaretleyerek tüm **tabloları seçin** .</span><span class="sxs-lookup"><span data-stu-id="836be-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="836be-126">Ayrıca, **saklı yordamlar ve işlevler** düğümü altında aşağıdaki saklı yordamları seçin: **Getstudentnotlar** ve **GetDepartmentName**.</span><span class="sxs-lookup"><span data-stu-id="836be-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![İçeri Aktarma](~/ef6/media/import.jpg)

    <span data-ttu-id="836be-128">*Visual Studio 2012 ile başlayarak EF Designer, saklı yordamların Toplu içe aktarımını destekler. **Seçilen saklı yordamları ve Işlevleri Içeri aktar varlık modeline** varsayılan olarak işaretlidir.*</span><span class="sxs-lookup"><span data-stu-id="836be-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="836be-129"> **Son**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="836be-129">Click **Finish**.</span></span>

<span data-ttu-id="836be-130">Varsayılan olarak, içeri aktarılan her saklı yordamın veya işlevin birden fazla sütunu döndüren sonuç şekli otomatik olarak yeni bir karmaşık tür olur.</span><span class="sxs-lookup"><span data-stu-id="836be-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="836be-131">Bu örnekte, **Getstudentnotlar** Işlevinin sonuçlarını **Studentgrad** varlığına ve **GetDepartmentName** sonuçlarını **none** (**hiçbiri** varsayılan değer) olarak eşlemek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="836be-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="836be-132">Bir işlev içeri aktarma işleminin bir varlık türü döndürmesi için, karşılık gelen saklı yordam tarafından döndürülen sütunlar döndürülen varlık türünün skaler özellikleriyle tam olarak eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="836be-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="836be-133">Bir işlev içeri aktarması Ayrıca basit türler, karmaşık türler veya hiçbir değer için Koleksiyonlar döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="836be-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="836be-134">Tasarım yüzeyine sağ tıklayıp **model tarayıcısı**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="836be-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="836be-135">**Model tarayıcısı**' nda **işlev içeri aktarmalar**' ı seçin ve ardından **getstudentnotlar** işlevine çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="836be-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="836be-136">Işlev Içeri aktarmayı Düzenle iletişim kutusunda **varlıklar** seçin ve **Studentgrad**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="836be-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="836be-137">*İşlev içeri **aktarmaları** iletişim kutusunun üst kısmındaki **işlev içe aktarma birleştirilebilir** onay kutusu, birleştirilebilir işlevlere eşlemenizi sağlar. Bu kutuyu işaretleyin, **saklı yordam/Işlev adı** açılır listesinde yalnızca birleştirilebilir Işlevler (tablo değerli işlevler) görüntülenir. Bu kutuyu denetlemeyin, listede yalnızca birleştirilemeyen işlevler gösterilir.*</span><span class="sxs-lookup"><span data-stu-id="836be-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="836be-138">Modeli kullanma</span><span class="sxs-lookup"><span data-stu-id="836be-138">Use the Model</span></span>

<span data-ttu-id="836be-139">**Main** yönteminin tanımlandığı **program.cs** dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="836be-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="836be-140">Aşağıdaki kodu Main işlevine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="836be-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="836be-141">Kod, iki saklı yordamı çağırır: **Getstudentnotlar** (belirtilen *studentitıd*Için **studentnotlar** ' ı döndürür) ve **GetDepartmentName** (çıkış parametresindeki departmanın adını döndürür).</span><span class="sxs-lookup"><span data-stu-id="836be-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

<span data-ttu-id="836be-142">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="836be-142">Compile and run the application.</span></span> <span data-ttu-id="836be-143">Program aşağıdaki çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="836be-143">The program produces the following output:</span></span>

```console
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="836be-144">Çıktı Parametreleri</span><span class="sxs-lookup"><span data-stu-id="836be-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="836be-145">Çıkış parametreleri kullanılıyorsa, sonuçlar tamamen okunana kadar bu değerler kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="836be-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="836be-146">Bu, DbDataReader 'ın temel davranışının nedeni, daha fazla ayrıntı için [DataReader kullanarak veri alma](https://go.microsoft.com/fwlink/?LinkID=398589) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="836be-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
