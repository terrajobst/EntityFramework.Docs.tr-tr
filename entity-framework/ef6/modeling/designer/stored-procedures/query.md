---
title: Saklı yordamları - EF6 Sorgu Tasarımcısı
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 04478ea1c8cd43a7ba4ee788e464992af3de7f64
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283907"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="a9aa0-102">Sorgu Tasarımcısı saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="a9aa0-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="a9aa0-103">Bu adım adım bir modele saklı yordamlar içeri aktarmak için Entity Framework Designer (EF Designer) kullanın ve ardından sonuçları almak için içeri aktarılan saklı yordamları çağırmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="a9aa0-104">Code First saklı yordamları ve işlevleri için eşleme desteklemediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="a9aa0-105">Ancak, System.Data.Entity.DbSet.SqlQuery yöntemi kullanarak saklı yordamları ve işlevleri çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="a9aa0-106">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a9aa0-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="a9aa0-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="a9aa0-107">Prerequisites</span></span>

<span data-ttu-id="a9aa0-108">Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:</span><span class="sxs-lookup"><span data-stu-id="a9aa0-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="a9aa0-109">Visual Studio'nun en son sürümü.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="a9aa0-110">[School örnek veritabanını](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="a9aa0-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="a9aa0-111">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="a9aa0-111">Set up the Project</span></span>

-   <span data-ttu-id="a9aa0-112">Visual Studio 2012'yi açın.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="a9aa0-113">Seçin **File -&gt; yeni -&gt; proje**</span><span class="sxs-lookup"><span data-stu-id="a9aa0-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="a9aa0-114">Sol bölmede **Visual C\#** ve ardından **konsol** şablonu.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="a9aa0-115">Girin **EFwithSProcsSample** adı.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="a9aa0-116">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="a9aa0-117">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="a9aa0-117">Create a Model</span></span>

-   <span data-ttu-id="a9aa0-118">Çözüm Gezgini'nde projeye sağ tıklayıp **Ekle -&gt; yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="a9aa0-119">Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="a9aa0-120">Girin **EFwithSProcsModel.edmx** dosya adı ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="a9aa0-121">Choose Model Contents iletişim kutusunda **veritabanından Oluştur**ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="a9aa0-122">Tıklayın **yeni bağlantı**.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="a9aa0-123">Bağlantı Özellikleri iletişim kutusuna sunucu adını girin (örneğin, **(localdb)\\ifadesini mssqllocaldb**) seçin kimlik doğrulama yöntemi, tür **Okul** veritabanı adı ve ardından tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="a9aa0-124">Veri bağlantınızı seçin iletişim kutusunda, veritabanı bağlantı ayarı ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="a9aa0-125">Veritabanı nesnelerinizi seçin iletişim kutusunda, denetleyin **tabloları** tüm tabloları seçmek için onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="a9aa0-126">Ayrıca, altında aşağıdaki saklı yordamları seçin **saklı yordamları ve işlevleri** düğüm: **GetStudentGrades** ve **GetDepartmentName**.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![{1&gt;İçeri Aktar&lt;1}](~/ef6/media/import.jpg)

    <span data-ttu-id="a9aa0-128">*EF Designer Visual Studio 2012'den itibaren saklı yordamlar toplu olarak içeri aktarma destekler. **Alma theentity modeline saklı yordamları ve işlevleri seçili** varsayılan olarak işaretlidir.*</span><span class="sxs-lookup"><span data-stu-id="a9aa0-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="a9aa0-129">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-129">Click **Finish**.</span></span>

<span data-ttu-id="a9aa0-130">Varsayılan olarak, her bir içeri aktarılan saklı yordam ya da birden fazla sütun döndüren işlev sonucu şeklini otomatik olarak yeni bir karmaşık türü olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="a9aa0-131">Bu örnekte biz sonuçlarını eşlemek istediğiniz **GetStudentGrades** işlevi **StudentGrade** varlık ve sonuçları **GetDepartmentName** için**hiçbiri** (**hiçbiri** varsayılan değerdir).</span><span class="sxs-lookup"><span data-stu-id="a9aa0-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="a9aa0-132">Bir işlev alma bir varlık türü döndürmek, karşılık gelen bir saklı yordam tarafından döndürülen sütunlar skaler özellikler döndürülen varlık türünün tam olarak eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="a9aa0-133">Bir işlev içeri aktarma, ayrıca koleksiyonları basit türler, karmaşık türler veya herhangi bir değer döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="a9aa0-134">Tasarım yüzeyi ve select sağ **Model tarayıcı**.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="a9aa0-135">İçinde **Model tarayıcı**seçin **işlevi içeri aktarmalar**ve çift tıklatarak **GetStudentGrades** işlevi.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="a9aa0-136">İşlev içeri aktarma Düzenle iletişim kutusunda **varlıkları** ve **StudentGrade**.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="a9aa0-137">***İşlevi alma birleştirilebilir** en üstündeki onay kutusu **işlevi içeri aktarmalar** iletişim için birleştirilemeyen işlevler eşlemenize izin. Bu kutuyu işaretlerseniz, yalnızca birleştirilemeyen işlevler (tablo değerli işlevler) görünür **saklı yordam / işlev adı** aşağı açılan listesi. Bu kutuyu işaretleyin değil, yalnızca olmayan işlevler listesinde gösterilir.*</span><span class="sxs-lookup"><span data-stu-id="a9aa0-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="a9aa0-138">Kullanım modeli</span><span class="sxs-lookup"><span data-stu-id="a9aa0-138">Use the Model</span></span>

<span data-ttu-id="a9aa0-139">Açık **Program.cs** dosya nerede **ana** yöntemi tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="a9aa0-140">Ana işlevine aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="a9aa0-141">Kod, iki saklı yordam çağırır: **GetStudentGrades** (döndürür **StudentGrades** için belirtilen *StudentId*) ve **GetDepartmentName** (departman adı çıkış parametresinde döndürür).</span><span class="sxs-lookup"><span data-stu-id="a9aa0-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

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

<span data-ttu-id="a9aa0-142">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-142">Compile and run the application.</span></span> <span data-ttu-id="a9aa0-143">Program şu çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="a9aa0-143">The program produces the following output:</span></span>

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="a9aa0-144">Çıktı Parametreleri</span><span class="sxs-lookup"><span data-stu-id="a9aa0-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="a9aa0-145">Çıktı parametreleri kullandıysanız değerleri sonuçları tamamen okunana kadar kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="a9aa0-146">Bu dbdatareader öğesine dönüştürülemedi arka plandaki davranışı nedeniyle, bkz: [alma verileri kullanarak bir DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="a9aa0-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
