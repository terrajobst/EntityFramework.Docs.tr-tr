---
title: Tasarımcı TPH devralma-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418429"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="e4cb2-102">Tasarımcı TPH devralma</span><span class="sxs-lookup"><span data-stu-id="e4cb2-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="e4cb2-103">Bu adım adım izlenecek yol, Entity Framework Designer (EF Designer) ile kavramsal modelinizde hiyerarşi başına tablo (TPH) devralmanın nasıl uygulanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="e4cb2-104">TPH devralma bir devralma hiyerarşisindeki tüm varlık türlerinin verilerini korumak için bir veritabanı tablosu kullanır.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="e4cb2-105">Bu kılavuzda kişi tablosunu üç varlık türü ile eşliyoruz: kişi (temel tür), öğrenci (kişiden türetiliyor) ve eğitmen (kişiden türetilir).</span><span class="sxs-lookup"><span data-stu-id="e4cb2-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="e4cb2-106">Veritabanından bir kavramsal model oluşturacağız (Database First) ve ardından modeli, EF tasarımcısını kullanarak TPH devralmayı uygulayacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="e4cb2-107">Model First kullanarak bir TPH devrtabanına eşleme yapılabilir, ancak karmaşık olan kendi veritabanı oluşturma iş akışınızı yazmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="e4cb2-108">Daha sonra bu iş akışını EF tasarımcısında **veritabanı oluşturma Iş akışı** özelliğine atamalısınız.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="e4cb2-109">Code First kullanmak daha kolay bir alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="e4cb2-110">Diğer devralma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="e4cb2-110">Other Inheritance Options</span></span>

<span data-ttu-id="e4cb2-111">Tablo/tür (TPT), veritabanındaki ayrı tabloların devralmaya katılan varlıklara eşlendiği başka bir devralma türüdür.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span> <span data-ttu-id="e4cb2-112"> EF Designer ile tablo türü devralmayı eşleme hakkında daha fazla bilgi için, bkz. [EF Designer TPT devralma](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="e4cb2-112"> For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="e4cb2-113">Tablo başına somut tür devralma (TPC) ve karışık devralma modelleri Entity Framework çalışma zamanı tarafından desteklenir, ancak EF Designer tarafından desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="e4cb2-114">TPC veya karışık devralma kullanmak istiyorsanız iki seçeneğiniz vardır: Code First kullanın veya EDMX dosyasını el ile düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="e4cb2-115">EDMX dosyası ile çalışmayı seçerseniz, eşleme ayrıntıları penceresi "güvenli mod" a yerleştirilir ve bu eşlemeleri değiştirmek için tasarımcıyı kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4cb2-116">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="e4cb2-116">Prerequisites</span></span>

<span data-ttu-id="e4cb2-117">Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:</span><span class="sxs-lookup"><span data-stu-id="e4cb2-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="e4cb2-118">Visual Studio 'nun son sürümü.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="e4cb2-119">[Okul örnek veritabanı](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="e4cb2-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="e4cb2-120">Projeyi ayarlama</span><span class="sxs-lookup"><span data-stu-id="e4cb2-120">Set up the Project</span></span>

-   <span data-ttu-id="e4cb2-121">Visual Studio 2012'yi açın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="e4cb2-122">**Dosya&gt; yeni&gt; proje** ' yi seçin</span><span class="sxs-lookup"><span data-stu-id="e4cb2-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="e4cb2-123">Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="e4cb2-124">Ad olarak **Tphdbfirstsample** girin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="e4cb2-125"> **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="e4cb2-126">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="e4cb2-126">Create a Model</span></span>

-   <span data-ttu-id="e4cb2-127">Çözüm Gezgini ' de proje adına sağ tıklayın ve **Ekle-&gt; yeni öğe**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="e4cb2-128">Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="e4cb2-129">Dosya adı için **Tphmodel. edmx** girin ve ardından **Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="e4cb2-130">Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="e4cb2-131"> **Yeni bağlantı**' ya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-131">Click **New Connection**.</span></span>
    <span data-ttu-id="e4cb2-132">Bağlantı özellikleri iletişim kutusunda sunucu adını (örneğin, **(LocalDB)\\mssqllocaldb**) girin, kimlik doğrulama yöntemini seçin, veritabanı adı için **okul** yazın ve ardından **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="e4cb2-133">Veri bağlantınızı seçin iletişim kutusu, veritabanı bağlantı ayarınız ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="e4cb2-134">Veritabanı nesnelerinizi seçin iletişim kutusundaki tablolar düğümünün altında **kişi** tablosunu seçin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="e4cb2-135"> **Son**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-135">Click **Finish**.</span></span>

<span data-ttu-id="e4cb2-136">Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="e4cb2-137">Veritabanı nesnelerinizi seçin iletişim kutusunda seçtiğiniz tüm nesneler modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="e4cb2-138">Bu, **kişi** tablosunun veritabanında nasıl göründüğünü bu şekilde yapar.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-138">That is how the **Person** table looks in the database.</span></span>

![Kişi tablosu](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="e4cb2-140">Tablo-hiyerarşi devralmayı Uygula</span><span class="sxs-lookup"><span data-stu-id="e4cb2-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="e4cb2-141"> **Kişi** tablosu, iki değerden birine sahip olabilen **ayrıştırıcı** sütununa sahiptir: "öğrenci" ve "eğitmen".</span><span class="sxs-lookup"><span data-stu-id="e4cb2-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="e4cb2-142">**Kişi** tablosunun, **öğrenci** varlığına veya **eğitmen** varlığına eşlendiği değere bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="e4cb2-143">Kişi **tablosunda Ayrıca** , bir kişi aynı anda bir öğrenci ve bir eğitmen (Bu kılavuzda değil) olabileceğinden **null değer atanabilir** olması gereken Iki sütun vardır: **HireDate**  ** ve kayıttarihi.**</span><span class="sxs-lookup"><span data-stu-id="e4cb2-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="e4cb2-144">Yeni varlık Ekle</span><span class="sxs-lookup"><span data-stu-id="e4cb2-144">Add new Entities</span></span>

-   <span data-ttu-id="e4cb2-145">Yeni bir varlık ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-145">Add a new entity.</span></span>
    <span data-ttu-id="e4cb2-146">Bunu yapmak için Entity Framework Designer Tasarım yüzeyindeki boş bir alana sağ tıklayın ve **Ekle-&gt;varlık**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="e4cb2-147"> **Varlık adı** için **eğitmen** yazın ve **temel tür**için açılan listeden **kişi** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="e4cb2-148"> **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-148">Click **OK**.</span></span>
-   <span data-ttu-id="e4cb2-149">Başka bir yeni varlık ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-149">Add another new entity.</span></span> <span data-ttu-id="e4cb2-150"> **Varlık adı** için **öğrenci** yazın ve **temel tür**için açılan listeden **kişi** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="e4cb2-151">Tasarım yüzeyine iki yeni varlık türü eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="e4cb2-152">Bir ok, yeni varlık türlerinden **kişiye** varlık türüne göre işaret eder; Bu, **kişinin** yeni varlık türleri için temel tür olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="e4cb2-153"> **Kişi** varlığının **hiredate** özelliğine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="e4cb2-154"> **Kes** ' i seçin (veya CTRL + X tuşunu kullanın).</span><span class="sxs-lookup"><span data-stu-id="e4cb2-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="e4cb2-155"> **Eğitmen** varlığına sağ tıklayıp **Yapıştır** ' ı seçin (veya CTRL-V anahtarını kullanın).</span><span class="sxs-lookup"><span data-stu-id="e4cb2-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="e4cb2-156"> **HireDate** özelliğine sağ tıklayın ve **Özellikler**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="e4cb2-157"> **Özellikler** penceresinde, **Nullable** özelliğini **false**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="e4cb2-158"> **Kişi** varlığının **kayıttarihi** özelliğine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="e4cb2-159"> **Kes** ' i seçin (veya CTRL + X tuşunu kullanın).</span><span class="sxs-lookup"><span data-stu-id="e4cb2-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="e4cb2-160"> **Öğrenci** varlığına sağ tıklayıp Yapıştır ' ı seçin **(veya CTRL-V anahtarını kullanın).**</span><span class="sxs-lookup"><span data-stu-id="e4cb2-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="e4cb2-161">Kayıttarihi \*\*\*\*  özelliğini seçin ve **Nullable** özelliğini **false**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="e4cb2-162">Varlık türü  **kişiyi** seçin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-162">Select the **Person** entity type.</span></span> <span data-ttu-id="e4cb2-163"> **Özellikler** penceresinde, **soyut** özelliğini **doğru**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="e4cb2-164">**Kişiden** **ayrıştırıcı** özelliğini silin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="e4cb2-165">Silinmesi gereken neden aşağıdaki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="e4cb2-166">Varlıkları eşleme</span><span class="sxs-lookup"><span data-stu-id="e4cb2-166">Map the entities</span></span>

-   <span data-ttu-id="e4cb2-167"> **Eğitmeni** sağ tıklatın ve **Tablo eşleme** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="e4cb2-168">Eğitmen varlığı, eşleme ayrıntıları penceresinde seçilir.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="e4cb2-169"> **Eşleme ayrıntıları** penceresinde\ \** tablo eklemek&lt;veya &gt;görüntüleyi\n\** ' e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="e4cb2-170"> **&lt;tablo veya görünüm&gt;**  alanı seçili varlığın eşleştiribileceği tablo veya görünümlerin açılan bir listesi haline gelir.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="e4cb2-171">Açılan listeden **kişi** seçin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="e4cb2-172"> **Eşleme ayrıntıları** penceresi, varsayılan sütun eşlemeleriyle ve koşul ekleme seçeneği ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="e4cb2-173"> **Koşul eklemek&lt;** ' ye tıklayın&gt;.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="e4cb2-174"> **&lt;koşul ekleme&gt;**  alan, koşulların ayarlanbileceği bir aşağı açılan sütun listesi haline gelir.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="e4cb2-175">Aşağı açılan listeden **ayrıştırıcı** seçin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="e4cb2-176"> **Eşleme ayrıntıları** penceresinin **işleç** sütununda, açılan listeden = seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="e4cb2-177"> **Değer/özellik** sütununda, **eğitmen**yazın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="e4cb2-178">Nihai sonuç şöyle görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="e4cb2-178">The end result should look like this:</span></span>

    ![Eşleme ayrıntıları](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="e4cb2-180"> **Öğrenci** varlık türü için bu adımları yineleyin, ancak koşulu **öğrenci** değerine eşit yapın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="e4cb2-181">***Ayrıştırıcı** özelliğini kaldırmak istediğimiz neden, bir tablo sütununu birden çok kez eşleyemezsiniz. Bu sütun koşullu eşleme için kullanılacaktır, bu nedenle de Özellik eşleme için kullanılamaz. Her ikisi için de kullanılabilecek tek yol, bir koşul **null** kullanıyorsa veya null karşılaştırma \*\*değildir*\* .\*</span><span class="sxs-lookup"><span data-stu-id="e4cb2-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="e4cb2-182">Hiyerarşi başına tablo devralma işlemi şimdi uygulandı.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![Son TPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="e4cb2-184">Modeli kullanma</span><span class="sxs-lookup"><span data-stu-id="e4cb2-184">Use the Model</span></span>

<span data-ttu-id="e4cb2-185">**Main** yönteminin tanımlandığı **program.cs** dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="e4cb2-186">Aşağıdaki kodu **Main** işlevine yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="e4cb2-187">Kod üç sorguyu yürütür.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-187">The code executes three queries.</span></span> <span data-ttu-id="e4cb2-188">İlk sorgu tüm **kişi** nesnelerini geri getirir.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="e4cb2-189">İkinci sorgu, **eğitmen** nesnelerini döndürmek için **OfType** metodunu kullanır.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="e4cb2-190">Üçüncü sorgu, **öğrenci** nesnelerini döndürmek için **OfType** metodunu kullanır.</span><span class="sxs-lookup"><span data-stu-id="e4cb2-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
