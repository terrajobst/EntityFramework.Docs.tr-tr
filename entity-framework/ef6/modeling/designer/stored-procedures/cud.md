---
title: Tasarımcı CUD saklı yordamları - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 36c9b97b77fec30136cba1d850a0259c689e69ae
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250926"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="5fa39-102">Tasarımcı CUD saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="5fa39-102">Designer CUD Stored Procedures</span></span>
<span data-ttu-id="5fa39-103">Bu adım adım kılavuzda oluşturma eşlemeyle ilgili bilgi göster\\ekleme, güncelleştirme ve silme (CUD) işlemleri saklı yordamlara Entity Framework Designer (EF Designer) kullanarak bir varlık türü.</span><span class="sxs-lookup"><span data-stu-id="5fa39-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span>  <span data-ttu-id="5fa39-104">Varsayılan olarak Entity Framework CUD işlemleri için SQL deyimleri otomatik olarak oluşturur, ancak saklı yordamlar bu işlemleri için eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fa39-104">By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="5fa39-105">Code First saklı yordamları ve işlevleri için eşleme desteklemediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="5fa39-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="5fa39-106">Ancak, System.Data.Entity.DbSet.SqlQuery yöntemi kullanarak saklı yordamları ve işlevleri çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="5fa39-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="5fa39-107">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5fa39-107">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="5fa39-108">Saklı yordamları CUD işlemleri için eşleme durumlarda dikkat edilmesi gerekenler</span><span class="sxs-lookup"><span data-stu-id="5fa39-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="5fa39-109">CUD işlemleri için saklı yordamlar eşlerken aşağıdaki maddeler geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="5fa39-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span> 

-   <span data-ttu-id="5fa39-110">Saklı yordama CUD işlemleri birini eşliyorsanız, bunların tümünün eşleyin.</span><span class="sxs-lookup"><span data-stu-id="5fa39-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="5fa39-111">Üç eşlemeyin eşlenmemiş işlemleri yürütüldü ve bir başarısız olur **UpdateException** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5fa39-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
-   <span data-ttu-id="5fa39-112">Saklı yordamın her parametre varlık özelliklerine eşlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5fa39-112">You must map every parameter of the stored procedure to entity properties.</span></span>
-   <span data-ttu-id="5fa39-113">Sunucu eklenen satır için birincil anahtar değeri oluşturursa, bu değer varlığın anahtar özelliğine eşlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5fa39-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="5fa39-114">Aşağıdaki örnekte **InsertPerson** saklı yordam, yeni oluşturulan birincil anahtarı saklı yordamın sonuç kümesinin bir parçası döndürür.</span><span class="sxs-lookup"><span data-stu-id="5fa39-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="5fa39-115">Varlık anahtarı için birincil anahtarı eşlenir (**Personıd**) kullanarak **&lt;sonuç bağlaması Ekle&gt;** EF Designer özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="5fa39-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
-   <span data-ttu-id="5fa39-116">Saklı yordam, kavramsal modeldeki varlıklar ile eşleştirilmiş 1:1 çağrılarıdır.</span><span class="sxs-lookup"><span data-stu-id="5fa39-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="5fa39-117">Örneğin, kavramsal model ve ardından harita Devralma Hiyerarşisi uygularsanız CUD yordamları için depolanan **üst** (Temel) ve **alt** kaydetme(türetilmiş)varlıklar**Alt** değişiklikleri yalnızca çağıracaktır **alt**saklı yordamları, değil tetikleyecek **üst**yordamları çağrıları depolanan.</span><span class="sxs-lookup"><span data-stu-id="5fa39-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5fa39-118">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="5fa39-118">Prerequisites</span></span>

<span data-ttu-id="5fa39-119">Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:</span><span class="sxs-lookup"><span data-stu-id="5fa39-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="5fa39-120">Visual Studio'nun en son sürümü.</span><span class="sxs-lookup"><span data-stu-id="5fa39-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="5fa39-121">[School örnek veritabanını](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="5fa39-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="5fa39-122">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="5fa39-122">Set up the Project</span></span>

-   <span data-ttu-id="5fa39-123">Visual Studio 2012'yi açın.</span><span class="sxs-lookup"><span data-stu-id="5fa39-123">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="5fa39-124">Seçin **File -&gt; yeni -&gt; proje**</span><span class="sxs-lookup"><span data-stu-id="5fa39-124">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="5fa39-125">Sol bölmede **Visual C\#** ve ardından **konsol** şablonu.</span><span class="sxs-lookup"><span data-stu-id="5fa39-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="5fa39-126">Girin **CUDSProcsSample** adı.</span><span class="sxs-lookup"><span data-stu-id="5fa39-126">Enter **CUDSProcsSample** as the name.</span></span>
-   <span data-ttu-id="5fa39-127">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="5fa39-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="5fa39-128">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="5fa39-128">Create a Model</span></span>

-   <span data-ttu-id="5fa39-129">Çözüm Gezgini'nde proje adına sağ tıklayın ve seçin **Ekle -&gt; yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="5fa39-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="5fa39-130">Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="5fa39-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="5fa39-131">Girin **CUDSProcs.edmx** dosya adı ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5fa39-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="5fa39-132">Choose Model Contents iletişim kutusunda **veritabanından Oluştur**ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="5fa39-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="5fa39-133">Tıklayın **yeni bağlantı**.</span><span class="sxs-lookup"><span data-stu-id="5fa39-133">Click **New Connection**.</span></span> <span data-ttu-id="5fa39-134">Bağlantı Özellikleri iletişim kutusuna sunucu adını girin (örneğin, **(localdb)\\ifadesini mssqllocaldb**) seçin kimlik doğrulama yöntemi, tür **Okul** veritabanı adı ve ardından tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="5fa39-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="5fa39-135">Veri bağlantınızı seçin iletişim kutusunda, veritabanı bağlantı ayarı ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5fa39-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="5fa39-136">Choose, veritabanı nesneleri iletişim kutusunun altında **tabloları** düğümünü **kişi** tablo.</span><span class="sxs-lookup"><span data-stu-id="5fa39-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
-   <span data-ttu-id="5fa39-137">Ayrıca, altında aşağıdaki saklı yordamları seçin **saklı yordamları ve işlevleri** düğüm: **DeletePerson**, **InsertPerson**, ve **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="5fa39-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span> 
-   <span data-ttu-id="5fa39-138">EF Designer Visual Studio 2012'den itibaren saklı yordamlar toplu olarak içeri aktarma destekler.</span><span class="sxs-lookup"><span data-stu-id="5fa39-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="5fa39-139">**Almak, varlık modele saklı yordamları ve işlevleri seçilen** varsayılan olarak işaretlidir.</span><span class="sxs-lookup"><span data-stu-id="5fa39-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="5fa39-140">Bu örnekte biz, ekleme, güncelleştirme ve silme varlık türleri saklı yordamları olduğundan, biz bunları almak istiyor musunuz ve bu onay kutusunun işaretini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="5fa39-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span> 

    ![S yakalar içeri aktarma](~/ef6/media/importsprocs.jpg)

-   <span data-ttu-id="5fa39-142">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="5fa39-142">Click **Finish**.</span></span>
    <span data-ttu-id="5fa39-143">Modelinizi düzenleme için bir tasarım yüzeyi sağlar, EF Designer görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="5fa39-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="5fa39-144">Kişi varlığı eşlemek için saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="5fa39-144">Map the Person Entity to Stored Procedures</span></span>

-   <span data-ttu-id="5fa39-145">Sağ **kişi** varlık türü **saklı yordam eşlemesi**.</span><span class="sxs-lookup"><span data-stu-id="5fa39-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
-   <span data-ttu-id="5fa39-146">Saklı yordam eşlemeleri görünür **eşleşme ayrıntıları** penceresi.</span><span class="sxs-lookup"><span data-stu-id="5fa39-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="5fa39-147">Tıklayın  **&lt;seçin işlevi Ekle&gt;**.</span><span class="sxs-lookup"><span data-stu-id="5fa39-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="5fa39-148">Alan, kavramsal modeldeki varlık türleri için eşlenen depolama modelinin saklı yordamları, aşağı açılan listesini duruma gelir.</span><span class="sxs-lookup"><span data-stu-id="5fa39-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="5fa39-149">Seçin **InsertPerson** aşağı açılan listeden.</span><span class="sxs-lookup"><span data-stu-id="5fa39-149">Select **InsertPerson** from the drop-down list.</span></span>
-   <span data-ttu-id="5fa39-150">Varsayılan eşlemeleri saklı yordam parametreleri ve varlık özellikleri arasında görünür.</span><span class="sxs-lookup"><span data-stu-id="5fa39-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="5fa39-151">Not oklar eşleme yönü belirtir: özellik değerleri, saklı yordam parametreleri için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="5fa39-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
-   <span data-ttu-id="5fa39-152">Tıklayın  **&lt;sonuç bağlaması Ekle&gt;**.</span><span class="sxs-lookup"><span data-stu-id="5fa39-152">Click **&lt;Add Result Binding&gt;**.</span></span>
-   <span data-ttu-id="5fa39-153">Tür **NewPersonID**, tarafından döndürülen parametrenin adı **InsertPerson** saklı yordamı.</span><span class="sxs-lookup"><span data-stu-id="5fa39-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="5fa39-154">Başında veya sonunda boşluk olmayan yazdığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="5fa39-154">Make sure not to type leading or trailing spaces.</span></span>
-   <span data-ttu-id="5fa39-155">Tuşuna **girin**.</span><span class="sxs-lookup"><span data-stu-id="5fa39-155">Press **Enter**.</span></span>
-   <span data-ttu-id="5fa39-156">Varsayılan olarak, **NewPersonID** varlık anahtara eşlenen **Personıd**.</span><span class="sxs-lookup"><span data-stu-id="5fa39-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="5fa39-157">Bir ok eşleme yönünü belirtir Not: sonuç sütunun değeri özelliği sağlandı.</span><span class="sxs-lookup"><span data-stu-id="5fa39-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![Eşleme ayrıntıları](~/ef6/media/mappingdetails.png)

-   <span data-ttu-id="5fa39-159">Tıklayın **&lt;güncelleştirme işlevini seçin&gt;** seçip **UpdatePerson** elde edilen aşağı açılan listeden.</span><span class="sxs-lookup"><span data-stu-id="5fa39-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="5fa39-160">Varsayılan eşlemeleri saklı yordam parametreleri ve varlık özellikleri arasında görünür.</span><span class="sxs-lookup"><span data-stu-id="5fa39-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
-   <span data-ttu-id="5fa39-161">Tıklayın **&lt;silme işlevi seçin&gt;** seçip **DeletePerson** elde edilen aşağı açılan listeden.</span><span class="sxs-lookup"><span data-stu-id="5fa39-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="5fa39-162">Varsayılan eşlemeleri saklı yordam parametreleri ve varlık özellikleri arasında görünür.</span><span class="sxs-lookup"><span data-stu-id="5fa39-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="5fa39-163">Ekleme, güncelleştirme ve silme işlemlerini **kişi** varlık türü artık saklı yordamlarla eşlenen.</span><span class="sxs-lookup"><span data-stu-id="5fa39-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="5fa39-164">Eşzamanlılık güncelleştirmeye veya silmeye saklı yordamlara sahip bir varlık denetleme etkinleştirmek istiyorsanız, aşağıdaki seçeneklerden birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="5fa39-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

-   <span data-ttu-id="5fa39-165">Kullanım bir **çıkış** sayısını döndürmek için parametre etkilenen satırlar saklı yordam ve onay **satırlardan etkilenen parametresi** parametre adının yanındaki onay kutusunu.</span><span class="sxs-lookup"><span data-stu-id="5fa39-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="5fa39-166">İşlem çağrıldığında, döndürülen değer sıfır ise bir [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5fa39-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
-   <span data-ttu-id="5fa39-167">Denetleme **kullanın, özgün değer** uyumluluğunu denetlemek için kullanmak istediğiniz bir özellik yanındaki onay kutusunu.</span><span class="sxs-lookup"><span data-stu-id="5fa39-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="5fa39-168">Bir güncelleştirme çalışırken, veritabanına yazma verileri yedeklediğinizde veritabanından ilk olarak okundu özelliğinin değeri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5fa39-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="5fa39-169">Değeri veritabanındaki değerle eşleşmiyorsa bir **OptimisticConcurrencyException** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5fa39-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="5fa39-170">Kullanım modeli</span><span class="sxs-lookup"><span data-stu-id="5fa39-170">Use the Model</span></span>

<span data-ttu-id="5fa39-171">Açık **Program.cs** dosya nerede **ana** yöntemi tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="5fa39-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="5fa39-172">Ana işlevine aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5fa39-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="5fa39-173">Yeni bir kod oluşturur **kişi** nesnesi daha sonra nesneyi güncelleştirir ve son olarak nesneyi siler.</span><span class="sxs-lookup"><span data-stu-id="5fa39-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>         

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

-   <span data-ttu-id="5fa39-174">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5fa39-174">Compile and run the application.</span></span> <span data-ttu-id="5fa39-175">Program şu çıktıyı üretir. \*</span><span class="sxs-lookup"><span data-stu-id="5fa39-175">The program produces the following output \*</span></span>
    >[!NOTE]
> <span data-ttu-id="5fa39-176">Sunucu tarafından otomatik olarak oluşturulan Personıd için büyük olasılıkla farklı sayı \* görürsünüz</span><span class="sxs-lookup"><span data-stu-id="5fa39-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="5fa39-177">Visual Studio Ultimate sürümü ile çalışıyorsanız, yürütülen SQL deyimleri görmek için IntelliTrace hata ayıklayıcısı ile kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fa39-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![IntelliTrace](~/ef6/media/intellitrace.png)
