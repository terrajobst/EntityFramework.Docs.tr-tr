---
title: Tasarımcı TPT devralma-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418212"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="935a2-102">Tasarımcı TPT devralma</span><span class="sxs-lookup"><span data-stu-id="935a2-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="935a2-103">Bu adım adım izlenecek yol, Entity Framework Designer (EF Designer) kullanarak modelinizde tablo başına (TPT) devralmayı nasıl uygulayacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="935a2-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="935a2-104">Tablo başına tür devralma, devralma hiyerarşisindeki her bir türün devralınmamış özellikleri ve anahtar özellikleri için verileri korumak üzere veritabanında ayrı bir tablo kullanır.</span><span class="sxs-lookup"><span data-stu-id="935a2-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="935a2-105">Bu kılavuzda, **kursu** (temel tür), **onlinekursu** (kursa türetiliyor) ve **Onsitekursu** ( **kursa**türetilir) varlıklarıyla aynı adlara sahip tablolara eşliyoruz.</span><span class="sxs-lookup"><span data-stu-id="935a2-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="935a2-106">Veritabanından bir model oluşturacak ve ardından modeli TPT devralmayı uygulayacak şekilde değiştirecek.</span><span class="sxs-lookup"><span data-stu-id="935a2-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="935a2-107">Ayrıca Model First başlatabilir ve sonra veritabanını modelden oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="935a2-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="935a2-108">EF Designer, varsayılan olarak TPT stratejisini kullanır ve böylece modeldeki Devralınanlar ayrı tablolarla eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="935a2-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="935a2-109">Diğer devralma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="935a2-109">Other Inheritance Options</span></span>

<span data-ttu-id="935a2-110">Hiyerarşi başına tablo (TPH), bir devralma hiyerarşisindeki tüm varlık türlerinin verilerini korumak için bir veritabanı tablosunun kullanıldığı başka bir devralma türüdür.</span><span class="sxs-lookup"><span data-stu-id="935a2-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span><span data-ttu-id="935a2-111">  Entity Desisgner hiyerarşi başına devralmayı, ile eşleme hakkında bilgi için bkz. [EF Designer TPH devralma](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="935a2-111">  For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="935a2-112">Somut tür devralma (TPC) ve karışık devralma modellerinin tablo başına Entity Framework çalışma zamanı tarafından desteklendiğini ancak EF Designer tarafından desteklenmediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="935a2-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="935a2-113">TPC veya karışık devralma kullanmak istiyorsanız iki seçeneğiniz vardır: Code First kullanın veya EDMX dosyasını el ile düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="935a2-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="935a2-114">EDMX dosyası ile çalışmayı seçerseniz, eşleme ayrıntıları penceresi "güvenli mod" a yerleştirilir ve bu eşlemeleri değiştirmek için tasarımcıyı kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="935a2-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="935a2-115">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="935a2-115">Prerequisites</span></span>

<span data-ttu-id="935a2-116">Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:</span><span class="sxs-lookup"><span data-stu-id="935a2-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="935a2-117">Visual Studio 'nun son sürümü.</span><span class="sxs-lookup"><span data-stu-id="935a2-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="935a2-118">[Okul örnek veritabanı](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="935a2-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="935a2-119">Projeyi ayarlama</span><span class="sxs-lookup"><span data-stu-id="935a2-119">Set up the Project</span></span>

-   <span data-ttu-id="935a2-120">Visual Studio 2012'yi açın.</span><span class="sxs-lookup"><span data-stu-id="935a2-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="935a2-121">**Dosya&gt; yeni&gt; proje** ' yi seçin</span><span class="sxs-lookup"><span data-stu-id="935a2-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="935a2-122">Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="935a2-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="935a2-123">Ad olarak **Tptdbfirstsample** girin.</span><span class="sxs-lookup"><span data-stu-id="935a2-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="935a2-124"> **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="935a2-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="935a2-125">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="935a2-125">Create a Model</span></span>

-   <span data-ttu-id="935a2-126">Çözüm Gezgini projeye sağ tıklayın ve **Ekle-&gt; yeni öğe**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="935a2-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="935a2-127">Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="935a2-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="935a2-128">Dosya adı için **Tptmodel. edmx** yazın ve ardından **Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="935a2-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="935a2-129">Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="935a2-129">In the Choose Model Contents dialog box, select** Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="935a2-130"> **Yeni bağlantı**' ya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="935a2-130">Click **New Connection**.</span></span>
    <span data-ttu-id="935a2-131">Bağlantı özellikleri iletişim kutusunda sunucu adını (örneğin, **(LocalDB)\\mssqllocaldb**) girin, kimlik doğrulama yöntemini seçin, veritabanı adı için **okul** yazın ve ardından **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="935a2-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="935a2-132">Veri bağlantınızı seçin iletişim kutusu, veritabanı bağlantı ayarınız ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="935a2-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="935a2-133">Veritabanı nesnelerinizi seçin iletişim kutusundaki tablolar düğümünün altında **departmanı**, **kursu, onlinekursu ve onsitekursu** tablolarını seçin.</span><span class="sxs-lookup"><span data-stu-id="935a2-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="935a2-134"> **Son**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="935a2-134">Click **Finish**.</span></span>

<span data-ttu-id="935a2-135">Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="935a2-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="935a2-136">Veritabanı nesnelerinizi seçin iletişim kutusunda seçtiğiniz tüm nesneler modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="935a2-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="935a2-137">Tablo türü devralma Uygula</span><span class="sxs-lookup"><span data-stu-id="935a2-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="935a2-138">Tasarım yüzeyinde **onlinekurs** varlık türüne sağ tıklayın ve **Özellikler**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="935a2-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="935a2-139">**Özellikler** penceresinde, temel tür özelliğini **Kurs**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="935a2-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="935a2-140">**Onsitekursu** varlık türüne sağ tıklayın ve **Özellikler**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="935a2-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="935a2-141">**Özellikler** penceresinde, temel tür özelliğini **Kurs**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="935a2-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="935a2-142">**Onlinekurs** ve **Kurs** varlık türleri arasındaki ilişkiye (satır) sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="935a2-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="935a2-143">**Modelden Sil**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="935a2-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="935a2-144">**Onsitekursu** ve **Kurs** varlık türleri arasındaki ilişkiye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="935a2-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="935a2-145">**Modelden Sil**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="935a2-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="935a2-146">Artık, bu sınıflar **Kurs** temel türünden **CourseID** 'Yi devraldığı için **Onlinekursu** ve **onsitekursu** ' ndan kurs **Seid** özelliğini silecağız.</span><span class="sxs-lookup"><span data-stu-id="935a2-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="935a2-147">**Onlinekurs** varlık türünün **Kurs Seid** özelliğine sağ tıklayın ve sonra **Modelden Sil**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="935a2-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="935a2-148">**Onsitekursu** varlık türünün **CourseID** özelliğine sağ tıklayın ve sonra **Modelden Sil** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="935a2-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="935a2-149">Tablo türü devralma artık uygulandı.</span><span class="sxs-lookup"><span data-stu-id="935a2-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="935a2-151">Modeli kullanma</span><span class="sxs-lookup"><span data-stu-id="935a2-151">Use the Model</span></span>

<span data-ttu-id="935a2-152">**Main** yönteminin tanımlandığı **program.cs** dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="935a2-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="935a2-153">Aşağıdaki kodu **Main** işlevine yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="935a2-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="935a2-154">Kod üç sorguyu yürütür.</span><span class="sxs-lookup"><span data-stu-id="935a2-154">The code executes three queries.</span></span> <span data-ttu-id="935a2-155">İlk sorgu belirtilen departmanla ilgili tüm **kursları** geri getirir.</span><span class="sxs-lookup"><span data-stu-id="935a2-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="935a2-156">İkinci sorgu, belirtilen departmanla ilgili **Onlinekurslar** döndürmek için **OfType** metodunu kullanır.</span><span class="sxs-lookup"><span data-stu-id="935a2-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="935a2-157">Üçüncü sorgu **Onsitekursu**döndürür.</span><span class="sxs-lookup"><span data-stu-id="935a2-157">The third query returns **OnsiteCourses**.</span></span>

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
