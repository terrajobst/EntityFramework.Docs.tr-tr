---
title: Tasarımcı TPT devralma - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 68980fa89446940b8b7f5f73c519d38e727a9039
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996354"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="7e7fd-102">Tasarımcı TPT devralma</span><span class="sxs-lookup"><span data-stu-id="7e7fd-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="7e7fd-103">Bu adım adım kılavuzda, Entity Framework Designer (EF Designer) kullanarak modelinize tablo başına tür (TPT) devralma uygulamak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="7e7fd-104">Tablo başına tür devralma devralınan özellikler ve devralma hiyerarşisinde her türü için anahtar özellikler için verileri korumak için veritabanında ayrı bir tabloda kullanır.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="7e7fd-105">Bu örnekte biz eşler **kurs** (temel türü), **OnlineCourse** (kursuna türetilir) ve **OnsiteCourse** (türetilen **Kurs**) aynı ada sahip tablolar için varlıklar.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="7e7fd-106">Biz bir model veritabanından oluşturacak ve ardından TPT devralma uygulamak için modeli alter.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="7e7fd-107">Ayrıca ilk Model başlatabilir ve sonra da modelden veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="7e7fd-108">EF Designer TPT stratejisi varsayılan olarak kullanır ve bu nedenle herhangi bir devralma modeli tabloları ayırmak için eşleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="7e7fd-109">Diğer devralma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="7e7fd-109">Other Inheritance Options</span></span>

<span data-ttu-id="7e7fd-110">Tablo-başına-hiyerarşi (TPH) devralma hangi bir veritabanında tablo varlık türleri bir devralma hiyerarşisindeki tüm verileri korumak için kullanılan başka bir türdür.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span>  <span data-ttu-id="7e7fd-111">Varlık Tasarımcısı tablo başına hiyerarşi kalıtım eşlemek hakkında daha fazla bilgi için bkz: [EF Designer TPH devralma](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="7e7fd-111">For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="7e7fd-112">Devralma (TPC) ve karma devralma modelleri Entity Framework çalışma zamanı tarafından desteklenir ancak EF tasarımcı tarafından desteklenmeyen türü,'somut başına tablo unutmayın.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="7e7fd-113">TPC veya karma devralma kullanmak istiyorsanız, iki seçeneğiniz vardır: Code First kullanın veya el ile EDMX dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="7e7fd-114">EDMX ile çalışmayı tercih ederseniz eşleme Ayrıntıları penceresi "Güvenli Mod'da" yerleştirilir ve eşlemelerini değiştirmek için tasarımcı kullanmanız mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e7fd-115">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="7e7fd-115">Prerequisites</span></span>

<span data-ttu-id="7e7fd-116">Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:</span><span class="sxs-lookup"><span data-stu-id="7e7fd-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="7e7fd-117">Visual Studio'nun en son sürümü.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="7e7fd-118">[School örnek veritabanını](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="7e7fd-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="7e7fd-119">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="7e7fd-119">Set up the Project</span></span>

-   <span data-ttu-id="7e7fd-120">Visual Studio 2012'yi açın.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="7e7fd-121">Seçin **File -&gt; yeni -&gt; proje**</span><span class="sxs-lookup"><span data-stu-id="7e7fd-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="7e7fd-122">Sol bölmede **Visual C\#** ve ardından **konsol** şablonu.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="7e7fd-123">Girin **TPTDBFirstSample** adı.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="7e7fd-124">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="7e7fd-125">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="7e7fd-125">Create a Model</span></span>

-   <span data-ttu-id="7e7fd-126">Çözüm Gezgini'nde projeye sağ tıklayıp seçin **Ekle -&gt; yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="7e7fd-127">Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="7e7fd-128">Girin **TPTModel.edmx** dosya adı ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="7e7fd-129">Choose Model Contents iletişim kutusunda seçim ** Oluştur veritabanı ** kutusuna ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-129">In the Choose Model Contents dialog box, select** Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="7e7fd-130">Tıklayın **yeni bağlantı**.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-130">Click **New Connection**.</span></span>
    <span data-ttu-id="7e7fd-131">Bağlantı Özellikleri iletişim kutusuna sunucu adını girin (örneğin, **(localdb)\\ifadesini mssqllocaldb**) seçin kimlik doğrulama yöntemi, tür **Okul** veritabanı adı ve ardından tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="7e7fd-132">Veri bağlantınızı seçin iletişim kutusunda, veritabanı bağlantı ayarı ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="7e7fd-133">Veritabanı nesnelerinizi seçin iletişim kutusunda, tablolar düğümünü seçin **departmanı**, **kurs OnlineCourse ve OnsiteCourse** tablolar.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="7e7fd-134">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-134">Click **Finish**.</span></span>

<span data-ttu-id="7e7fd-135">Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="7e7fd-136">Veritabanı nesnelerinizi seçin iletişim kutusunda seçilen tüm nesneler, modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="7e7fd-137">Tablo başına tür devralma uygulama</span><span class="sxs-lookup"><span data-stu-id="7e7fd-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="7e7fd-138">Tasarım yüzeyinde, sağ **OnlineCourse** varlık türü **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="7e7fd-139">İçinde **özellikleri** penceresinde ayarlanmış temel tür özelliği **kurs**.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="7e7fd-140">Sağ **OnsiteCourse** varlık türü **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="7e7fd-141">İçinde **özellikleri** penceresinde ayarlanmış temel tür özelliği **kurs**.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="7e7fd-142">Arasındaki ilişkiyi (satır) sağ **OnlineCourse** ve **kurs** varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="7e7fd-143">Seçin **modelden silmek**.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="7e7fd-144">Arasındaki ilişkiyi sağ **OnsiteCourse** ve **kurs** varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="7e7fd-145">Seçin **modelden silmek**.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="7e7fd-146">Şimdi biz silecek **CourseID** özelliğinden **OnlineCourse** ve **OnsiteCourse** bu sınıfların devraldığı **CourseID** gelen **kurs** temel türü.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="7e7fd-147">Sağ **CourseID** özelliği **OnlineCourse** varlık türü ve ardından **modelden silmek**.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="7e7fd-148">Sağ **CourseID** özelliği **OnsiteCourse** varlık türü ve ardından **modelden Sil**</span><span class="sxs-lookup"><span data-stu-id="7e7fd-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="7e7fd-149">Tablo başına tür devralma artık uygulanır.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="7e7fd-151">Kullanım modeli</span><span class="sxs-lookup"><span data-stu-id="7e7fd-151">Use the Model</span></span>

<span data-ttu-id="7e7fd-152">Açık **Program.cs** dosya nerede **ana** yöntemi tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="7e7fd-153">Aşağıdaki kodu yapıştırın **ana** işlevi.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="7e7fd-154">Kod üç sorguları yürütür.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-154">The code executes three queries.</span></span> <span data-ttu-id="7e7fd-155">İlk sorgu geri getirir tüm **kursları** belirtilen departmana ilgili.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="7e7fd-156">İkinci sorgu kullanan **OfType** döndürülecek yöntemi **OnlineCourses** belirtilen departmana ilgili.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="7e7fd-157">Üçüncü sorgunun döndürdüğü **OnsiteCourses**.</span><span class="sxs-lookup"><span data-stu-id="7e7fd-157">The third query returns **OnsiteCourses**.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
