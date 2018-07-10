---
title: Tasarımcı TPH devralma - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
caps.latest.revision: 3
ms.openlocfilehash: 0a017d3b97808cede3134119940b2e5839d0f282
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912720"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="2c558-102">Tasarımcı TPH devralma</span><span class="sxs-lookup"><span data-stu-id="2c558-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="2c558-103">Bu adım adım kılavuzda, Entity Framework Designer (EF Designer) ile kavramsal modeldeki tablo başına hiyerarşi (TPH) devralma uygulamak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2c558-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="2c558-104">TPH devralma, varlık türleri bir devralma hiyerarşisindeki tüm verileri korumak için bir veritabanı tablosu kullanır.</span><span class="sxs-lookup"><span data-stu-id="2c558-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="2c558-105">Bu izlenecek yolda biz kişi tablo üç varlık türü için eşler: kişi (temel türü), (türetilen kişiden) Öğrenci ve Eğitmen (türetilen kişiden).</span><span class="sxs-lookup"><span data-stu-id="2c558-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="2c558-106">Biz kavramsal model veritabanı (veritabanı ilk) oluşturacak ve ardından EF Designer kullanarak TPH devralma uygulamak için modeli alter.</span><span class="sxs-lookup"><span data-stu-id="2c558-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="2c558-107">İlk modeli kullanarak bir TPH devralma eşlemek mümkündür, ancak karmaşık olan kendi veritabanı oluşturma iş akışı yazma etmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2c558-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="2c558-108">Bu iş akışını ardından atadığınız **veritabanı oluşturma iş akışı** EF Designer özelliği.</span><span class="sxs-lookup"><span data-stu-id="2c558-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="2c558-109">Daha kolay bir alternatif, Code First kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="2c558-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="2c558-110">Diğer devralma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="2c558-110">Other Inheritance Options</span></span>

<span data-ttu-id="2c558-111">Tablo başına tür (TPT), veritabanı ayrı tablolarda Devralmada rol oynayan varlıkları eşlenen devralma başka bir türdür.</span><span class="sxs-lookup"><span data-stu-id="2c558-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span>  <span data-ttu-id="2c558-112">Tablo başına tür devralma EF Designer ile eşlemek hakkında daha fazla bilgi için bkz: [EF Designer TPT devralma](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="2c558-112">For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="2c558-113">Tablo başına somut tür devralma (TPC) ve karma devralma modelleri Entity Framework çalışma zamanı tarafından desteklenir ancak EF tasarımcı tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="2c558-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="2c558-114">TPC veya karma devralma kullanmak istiyorsanız, iki seçeneğiniz vardır: Code First kullanın veya el ile EDMX dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="2c558-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="2c558-115">EDMX ile çalışmayı tercih ederseniz eşleme Ayrıntıları penceresi "Güvenli Mod'da" yerleştirilir ve eşlemelerini değiştirmek için tasarımcı kullanmanız mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="2c558-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c558-116">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="2c558-116">Prerequisites</span></span>

<span data-ttu-id="2c558-117">Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:</span><span class="sxs-lookup"><span data-stu-id="2c558-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="2c558-118">Visual Studio'nun en son sürümü.</span><span class="sxs-lookup"><span data-stu-id="2c558-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="2c558-119">[School örnek veritabanını](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="2c558-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="2c558-120">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="2c558-120">Set up the Project</span></span>

-   <span data-ttu-id="2c558-121">Visual Studio 2012'yi açın.</span><span class="sxs-lookup"><span data-stu-id="2c558-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="2c558-122">Seçin **File -&gt; yeni -&gt; proje**</span><span class="sxs-lookup"><span data-stu-id="2c558-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="2c558-123">Sol bölmede **Visual C\#** ve ardından **konsol** şablonu.</span><span class="sxs-lookup"><span data-stu-id="2c558-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="2c558-124">Girin **TPHDBFirstSample** adı.</span><span class="sxs-lookup"><span data-stu-id="2c558-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="2c558-125">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="2c558-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="2c558-126">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="2c558-126">Create a Model</span></span>

-   <span data-ttu-id="2c558-127">Çözüm Gezgini'nde proje adına sağ tıklayın ve seçin **Ekle -&gt; yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="2c558-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="2c558-128">Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="2c558-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="2c558-129">Girin **TPHModel.edmx** dosya adı ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="2c558-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="2c558-130">Choose Model Contents iletişim kutusunda **veritabanından Oluştur**ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="2c558-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="2c558-131">Tıklayın **yeni bağlantı**.</span><span class="sxs-lookup"><span data-stu-id="2c558-131">Click **New Connection**.</span></span>
    <span data-ttu-id="2c558-132">Bağlantı Özellikleri iletişim kutusuna sunucu adını girin (örneğin, **(localdb)\\ifadesini mssqllocaldb**) seçin kimlik doğrulama yöntemi, tür **Okul** veritabanı adı ve ardından tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="2c558-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="2c558-133">Veri bağlantınızı seçin iletişim kutusunda, veritabanı bağlantı ayarı ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2c558-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="2c558-134">Veritabanı nesnelerinizi seçin iletişim kutusunda, tablolar düğümünü seçin **kişi** tablo.</span><span class="sxs-lookup"><span data-stu-id="2c558-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="2c558-135">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c558-135">Click **Finish**.</span></span>

<span data-ttu-id="2c558-136">Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2c558-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="2c558-137">Veritabanı nesnelerinizi seçin iletişim kutusunda seçilen tüm nesneler, modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="2c558-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="2c558-138">Diğer bir deyişle nasıl **kişi** veritabanında tablo görünür.</span><span class="sxs-lookup"><span data-stu-id="2c558-138">That is how the **Person** table looks in the database.</span></span>

![PersonTable](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="2c558-140">Tablo başına hiyerarşi devralma uygulama</span><span class="sxs-lookup"><span data-stu-id="2c558-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="2c558-141">**Kişi** tablolu **ayrıştırıcı** sütunu iki değerden birine sahip olabilir: "Öğrenci" ve "Eğitmen".</span><span class="sxs-lookup"><span data-stu-id="2c558-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="2c558-142">Değerine bağlı olarak **kişi** tablo için eşleştirilir **Öğrenci** varlık veya **Eğitmen** varlık.</span><span class="sxs-lookup"><span data-stu-id="2c558-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="2c558-143">**Kişi** tablo de iki sütun vardır **İşeAlmaTarihi** ve **EnrollmentDate**, olması gereken **boş değer atanabilir** bir kişi olamayacağı için bir Öğrenci ve aynı zamanda bir eğitmen (en az değil, bu izlenecek yol).</span><span class="sxs-lookup"><span data-stu-id="2c558-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="2c558-144">Yeni varlık Ekle</span><span class="sxs-lookup"><span data-stu-id="2c558-144">Add new Entities</span></span>

-   <span data-ttu-id="2c558-145">Yeni varlık ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c558-145">Add a new entity.</span></span>
    <span data-ttu-id="2c558-146">Bunu yapmak için Entity Framework Designer'ın tasarım yüzeyinde boş bir alanı sağ tıklatın ve seçin **Add -&gt;varlık**.</span><span class="sxs-lookup"><span data-stu-id="2c558-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="2c558-147">Tür **Eğitmen** için **varlık adı** seçip **kişi** için aşağı açılan listeden **temel türü**.</span><span class="sxs-lookup"><span data-stu-id="2c558-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="2c558-148">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="2c558-148">Click **OK**.</span></span>
-   <span data-ttu-id="2c558-149">Başka bir yeni varlık ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c558-149">Add another new entity.</span></span> <span data-ttu-id="2c558-150">Tür **Öğrenci** için **varlık adı** seçip **kişi** için aşağı açılan listeden **temel türü**.</span><span class="sxs-lookup"><span data-stu-id="2c558-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="2c558-151">İki yeni varlık türleri tasarım yüzeyine eklendi.</span><span class="sxs-lookup"><span data-stu-id="2c558-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="2c558-152">Bir ok işaret yeni varlık türlerinden **kişi** varlık türü; gösterir **kişi** yeni varlık türleri için temel türdür.</span><span class="sxs-lookup"><span data-stu-id="2c558-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="2c558-153">Sağ **İşeAlmaTarihi** özelliği **kişi** varlık.</span><span class="sxs-lookup"><span data-stu-id="2c558-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="2c558-154">Seçin **Kes** (veya Ctrl-X anahtarını kullanın).</span><span class="sxs-lookup"><span data-stu-id="2c558-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="2c558-155">Sağ **Eğitmen** varlık ve select **Yapıştır** (veya Ctrl-V anahtarı kullanın).</span><span class="sxs-lookup"><span data-stu-id="2c558-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="2c558-156">Sağ **İşeAlmaTarihi** özelliğini tıklatın ve **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="2c558-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="2c558-157">İçinde **özellikleri** penceresinde **Nullable** özelliğini **false**.</span><span class="sxs-lookup"><span data-stu-id="2c558-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="2c558-158">Sağ **EnrollmentDate** özelliği **kişi** varlık.</span><span class="sxs-lookup"><span data-stu-id="2c558-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="2c558-159">Seçin **Kes** (veya Ctrl-X anahtarını kullanın).</span><span class="sxs-lookup"><span data-stu-id="2c558-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="2c558-160">Sağ **Öğrenci** varlık ve select **yapıştırın (veya anahtar kullan Ctrl-V).**</span><span class="sxs-lookup"><span data-stu-id="2c558-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="2c558-161">Seçin **EnrollmentDate** özelliği ve kümesi **Nullable** özelliğini **false**.</span><span class="sxs-lookup"><span data-stu-id="2c558-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="2c558-162">Seçin **kişi** varlık türü.</span><span class="sxs-lookup"><span data-stu-id="2c558-162">Select the **Person** entity type.</span></span> <span data-ttu-id="2c558-163">İçinde **özellikleri** penceresinde kendi **soyut** özelliğini **true**.</span><span class="sxs-lookup"><span data-stu-id="2c558-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="2c558-164">Silme **ayrıştırıcı** özelliğinden **kişi**.</span><span class="sxs-lookup"><span data-stu-id="2c558-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="2c558-165">Silinmesi nedeni aşağıdaki bölümde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2c558-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="2c558-166">Varlıkları Eşle</span><span class="sxs-lookup"><span data-stu-id="2c558-166">Map the entities</span></span>

-   <span data-ttu-id="2c558-167">Sağ **Eğitmen** seçip **Tablo eşleme.**</span><span class="sxs-lookup"><span data-stu-id="2c558-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="2c558-168">Eşleme Ayrıntıları penceresinde Eğitmen varlığı seçilidir.</span><span class="sxs-lookup"><span data-stu-id="2c558-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="2c558-169">Tıklayın **&lt;bir tablo veya Görünüm Ekle&gt;** içinde **eşleşme ayrıntıları** penceresi.</span><span class="sxs-lookup"><span data-stu-id="2c558-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="2c558-170">**&lt;Bir tablo veya Görünüm Ekle&gt;** alan görünümler seçilen varlığın hangi eşlenebilir için veya tablo açılır listesini olur.</span><span class="sxs-lookup"><span data-stu-id="2c558-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="2c558-171">Seçin **kişi** aşağı açılan listeden.</span><span class="sxs-lookup"><span data-stu-id="2c558-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="2c558-172">**Eşleşme ayrıntıları** penceresi varsayılan sütun eşlemelerini ve isteğe bağlı bir koşul eklemek için güncelleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c558-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="2c558-173">Tıklayarak  **&lt;koşul Ekle&gt;**.</span><span class="sxs-lookup"><span data-stu-id="2c558-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="2c558-174">**&lt;Koşul Ekle&gt;** alan koşulları ayarlanabilir sütun açılan listesini duruma gelir.</span><span class="sxs-lookup"><span data-stu-id="2c558-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="2c558-175">Seçin **ayrıştırıcı** aşağı açılan listeden.</span><span class="sxs-lookup"><span data-stu-id="2c558-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="2c558-176">İçinde **işleci** sütununun **eşleşme ayrıntıları** penceresinde açılan listeden =.</span><span class="sxs-lookup"><span data-stu-id="2c558-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="2c558-177">İçinde **değeri/özellik** sütununa, **Eğitmen**.</span><span class="sxs-lookup"><span data-stu-id="2c558-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="2c558-178">Nihai sonucu şu şekilde görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="2c558-178">The end result should look like this:</span></span>

    ![MappingDetails2](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="2c558-180">Bu adımı yineleyin **Öğrenci** varlık türü, ancak yapma eşittir koşulu **Öğrenci** değeri.</span><span class="sxs-lookup"><span data-stu-id="2c558-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="2c558-181">*İstedik kaldırmak için neden **ayrıştırıcı** özelliğidir, çünkü bir tablo sütunu birden çok kez eşlenemez. Bu nedenle de özellik eşlemesi kullanılamaz bu sütun koşullu eşlemek için kullanılacaktır. Bu kullanılabilir her ikisi için de bir koşul kullanıyorsa tek yolu bir **Is Null** veya **Is Not Null** karşılaştırma.*</span><span class="sxs-lookup"><span data-stu-id="2c558-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="2c558-182">Tablo başına hiyerarşi devralma artık uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2c558-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![FinalTPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="2c558-184">Kullanım modeli</span><span class="sxs-lookup"><span data-stu-id="2c558-184">Use the Model</span></span>

<span data-ttu-id="2c558-185">Açık **Program.cs** dosya nerede **ana** yöntemi tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="2c558-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="2c558-186">Aşağıdaki kodu yapıştırın **ana** işlevi.</span><span class="sxs-lookup"><span data-stu-id="2c558-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="2c558-187">Kod üç sorguları yürütür.</span><span class="sxs-lookup"><span data-stu-id="2c558-187">The code executes three queries.</span></span> <span data-ttu-id="2c558-188">İlk sorgu geri getirir tüm **kişi** nesneleri.</span><span class="sxs-lookup"><span data-stu-id="2c558-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="2c558-189">İkinci sorgu kullanan **OfType** döndürülecek yöntemi **Eğitmen** nesneleri.</span><span class="sxs-lookup"><span data-stu-id="2c558-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="2c558-190">Üçüncü sorgunun kullanan **OfType** döndürülecek yöntemi **Öğrenci** nesneleri.</span><span class="sxs-lookup"><span data-stu-id="2c558-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

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
