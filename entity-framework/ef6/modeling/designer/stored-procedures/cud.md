---
title: Tasarımcı CUD saklı yordamları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418324"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="b72ea-102">Tasarımcı CUD saklı yordamları</span><span class="sxs-lookup"><span data-stu-id="b72ea-102">Designer CUD Stored Procedures</span></span>

<span data-ttu-id="b72ea-103">Bu adım adım izlenecek yol, bir varlık türünün Create\\INSERT, Update ve Delete (CUD) işlemlerinin, Entity Framework Designer (EF Designer) kullanılarak saklı yordamlara nasıl eşleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="b72ea-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="b72ea-104"> Varsayılan olarak, Entity Framework CUD işlemleri için SQL deyimlerini otomatik olarak oluşturur, ancak saklı yordamları bu işlemlere de eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b72ea-104"> By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="b72ea-105">Code First, saklı yordamlara veya işlevlere eşlemeyi desteklemediğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="b72ea-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="b72ea-106">Ancak, System. Data. Entity. DbSet. SqlQuery yöntemini kullanarak saklı yordamları veya işlevleri çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b72ea-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="b72ea-107">Örnek:</span><span class="sxs-lookup"><span data-stu-id="b72ea-107">For example:</span></span>

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="b72ea-108">CUD Işlemlerini Saklı yordamlarla eşlerken dikkat edilecek noktalar</span><span class="sxs-lookup"><span data-stu-id="b72ea-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="b72ea-109">CUD işlemlerini Saklı yordamlarla eşlerken aşağıdaki noktalar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="b72ea-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span>

- <span data-ttu-id="b72ea-110">CUD işlemlerinden birini saklı yordamla eşleştirdiğinizde, tümünü eşleyin.</span><span class="sxs-lookup"><span data-stu-id="b72ea-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="b72ea-111">Üçü de eşleştirmez, yürütülürse eşlenmemiş işlemler başarısız olur ve bir **UpdateException** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b72ea-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
- <span data-ttu-id="b72ea-112">Saklı yordamın her parametresini varlık özelliklerine eşlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="b72ea-112">You must map every parameter of the stored procedure to entity properties.</span></span>
- <span data-ttu-id="b72ea-113">Sunucu, ekli satır için birincil anahtar değerini oluşturursa, bu değeri varlığın anahtar özelliğine geri eşlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="b72ea-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="b72ea-114">Aşağıdaki örnekte, **ınsertperson** saklı yordamı saklı yordamın sonuç kümesinin bir parçası olarak yeni oluşturulan birincil anahtarı döndürür.</span><span class="sxs-lookup"><span data-stu-id="b72ea-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="b72ea-115">Birincil anahtar, EF Designer 'ın **&lt;sonuç bağlamaları ekle&gt;**  özelliği kullanılarak varlık anahtarı (**PersonID**) ile eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b72ea-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
- <span data-ttu-id="b72ea-116">Saklı yordam çağrıları, kavramsal modeldeki varlıklarla 1:1 eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b72ea-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="b72ea-117">Örneğin, kavramsal modelinize bir devralma hiyerarşisi uygularsanız ve sonra **üst** (temel) ve **alt** (türetilmiş) varlıklar için CUD saklı yordamlarını eşlediğinizde, **alt** değişiklikleri kaydetmek yalnızca **alt**öğenin saklı yordamlarını çağıracaktır, **üst**öğenin saklı yordam çağrılarını tetiklemez.</span><span class="sxs-lookup"><span data-stu-id="b72ea-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b72ea-118">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b72ea-118">Prerequisites</span></span>

<span data-ttu-id="b72ea-119">Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:</span><span class="sxs-lookup"><span data-stu-id="b72ea-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="b72ea-120">Visual Studio 'nun son sürümü.</span><span class="sxs-lookup"><span data-stu-id="b72ea-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="b72ea-121">[Okul örnek veritabanı](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="b72ea-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="b72ea-122">Projeyi ayarlama</span><span class="sxs-lookup"><span data-stu-id="b72ea-122">Set up the Project</span></span>

- <span data-ttu-id="b72ea-123">Visual Studio 2012'yi açın.</span><span class="sxs-lookup"><span data-stu-id="b72ea-123">Open Visual Studio 2012.</span></span>
- <span data-ttu-id="b72ea-124">**Dosya&gt; yeni&gt; proje** ' yi seçin</span><span class="sxs-lookup"><span data-stu-id="b72ea-124">Select **File-&gt; New -&gt; Project**</span></span>
- <span data-ttu-id="b72ea-125">Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="b72ea-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
- <span data-ttu-id="b72ea-126">Ad olarak **Cudsprocssample** girin.</span><span class="sxs-lookup"><span data-stu-id="b72ea-126">Enter **CUDSProcsSample** as the name.</span></span>
- <span data-ttu-id="b72ea-127"> **Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="b72ea-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="b72ea-128">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="b72ea-128">Create a Model</span></span>

- <span data-ttu-id="b72ea-129">Çözüm Gezgini ' de proje adına sağ tıklayın ve **Ekle-&gt; yeni öğe**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b72ea-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
- <span data-ttu-id="b72ea-130">Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="b72ea-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
- <span data-ttu-id="b72ea-131">Dosya adı için **Cudsprocs. edmx** girin ve ardından **Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b72ea-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
- <span data-ttu-id="b72ea-132">Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b72ea-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
- <span data-ttu-id="b72ea-133"> **Yeni bağlantı**' ya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b72ea-133">Click **New Connection**.</span></span> <span data-ttu-id="b72ea-134">Bağlantı özellikleri iletişim kutusunda sunucu adını (örneğin, **(LocalDB)\\mssqllocaldb**) girin, kimlik doğrulama yöntemini seçin, veritabanı adı için **okul** yazın ve ardından **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b72ea-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="b72ea-135">Veri bağlantınızı seçin iletişim kutusu, veritabanı bağlantı ayarınız ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b72ea-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
- <span data-ttu-id="b72ea-136">Veritabanı nesnelerinizi seçin iletişim kutusunda, **tablolar** düğümü altında **kişi** tablosunu seçin.</span><span class="sxs-lookup"><span data-stu-id="b72ea-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
- <span data-ttu-id="b72ea-137">Ayrıca, **saklı yordamlar ve işlevler** düğümü altında aşağıdaki saklı yordamları seçin: **deleteperson**, **ınsertperson**ve **UpdatePerson**.</span><span class="sxs-lookup"><span data-stu-id="b72ea-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span>
- <span data-ttu-id="b72ea-138">Visual Studio 2012 ile başlayarak EF Designer, saklı yordamların Toplu içe aktarımını destekler.</span><span class="sxs-lookup"><span data-stu-id="b72ea-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="b72ea-139">**Seçilen saklı yordamları ve işlevleri varlık modeline aktar** varsayılan olarak denetlenir.</span><span class="sxs-lookup"><span data-stu-id="b72ea-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="b72ea-140">Bu örnekte varlık türlerini ekleyen, güncelleştiren ve silen yordamlar depoladık, bunları içeri aktarmak istemiyorum ve bu onay kutusunun işaretini silecek.</span><span class="sxs-lookup"><span data-stu-id="b72ea-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span>

    ![S profili içeri aktar](~/ef6/media/importsprocs.jpg)

- <span data-ttu-id="b72ea-142"> **Son**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b72ea-142">Click **Finish**.</span></span>
    <span data-ttu-id="b72ea-143">Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan EF Designer, görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b72ea-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="b72ea-144">Kişi varlığını saklı yordamlara eşleyin</span><span class="sxs-lookup"><span data-stu-id="b72ea-144">Map the Person Entity to Stored Procedures</span></span>

- <span data-ttu-id="b72ea-145">Varlık türü  **kişiye** sağ tıklayın ve **saklı yordam eşlemesi**' ni seçin.</span><span class="sxs-lookup"><span data-stu-id="b72ea-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
- <span data-ttu-id="b72ea-146">Saklı yordam eşlemeleri, **eşleme ayrıntıları** penceresinde görünür.</span><span class="sxs-lookup"><span data-stu-id="b72ea-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
- <span data-ttu-id="b72ea-147">\ \**&lt;Işlev ekle&gt;seçin\ \**' e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b72ea-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="b72ea-148">Bu alan, kavramsal modeldeki varlık türleriyle eşleştirilabilen depolama modelindeki saklı yordamların açılan bir listesi haline gelir.</span><span class="sxs-lookup"><span data-stu-id="b72ea-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="b72ea-149">Aşağı açılan listeden  **ınsertperson** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="b72ea-149">Select **InsertPerson** from the drop-down list.</span></span>
- <span data-ttu-id="b72ea-150">Saklı yordam parametreleri ve varlık özellikleri arasındaki varsayılan eşlemeler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b72ea-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="b72ea-151">Okların eşleme yönünü gösterdiğine unutmayın: özellik değerleri, saklı yordam parametrelerine sağlanır.</span><span class="sxs-lookup"><span data-stu-id="b72ea-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
- <span data-ttu-id="b72ea-152">\ \**&lt;sonuç bağlama&gt;Ekle\ \**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b72ea-152">Click **&lt;Add Result Binding&gt;**.</span></span>
- <span data-ttu-id="b72ea-153"> **Newpersonıd**, **ınsertperson** saklı yordamının döndürdüğü parametrenin adı yazın.</span><span class="sxs-lookup"><span data-stu-id="b72ea-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="b72ea-154">Baştaki veya sondaki boşlukları yazmayın emin olun.</span><span class="sxs-lookup"><span data-stu-id="b72ea-154">Make sure not to type leading or trailing spaces.</span></span>
- <span data-ttu-id="b72ea-155"> **ENTER**tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="b72ea-155">Press **Enter**.</span></span>
- <span data-ttu-id="b72ea-156">Varsayılan olarak **Newpersonıd** , **PersonID**varlık anahtarıyla eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b72ea-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="b72ea-157">Bir okun eşlemenin yönünü gösterir: sonuç sütununun değeri özelliği için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="b72ea-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![Eşleme ayrıntıları](~/ef6/media/mappingdetails.png)

- <span data-ttu-id="b72ea-159"> **&lt;işlev&gt;Güncelleştir** ' i tıklatın  sonra oluşturulan açılan listeden **UpdatePerson** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="b72ea-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="b72ea-160">Saklı yordam parametreleri ve varlık özellikleri arasındaki varsayılan eşlemeler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b72ea-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
- <span data-ttu-id="b72ea-161"> **&lt;fonksiyon&gt;Sil** ' i seçin ardından ortaya çıkan açılan listeden **deleteperson** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="b72ea-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="b72ea-162">Saklı yordam parametreleri ve varlık özellikleri arasındaki varsayılan eşlemeler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b72ea-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="b72ea-163"> **Kişi** varlık türünün ekleme, güncelleştirme ve silme işlemleri artık Saklı yordamlarla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="b72ea-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="b72ea-164">Saklı yordamlar içeren bir varlığı güncelleştirirken veya silerken eşzamanlılık denetimini etkinleştirmek istiyorsanız, aşağıdaki seçeneklerden birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="b72ea-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

- <span data-ttu-id="b72ea-165">Saklı yordamdaki etkilenen satırların sayısını döndürmek ve parametre adının yanındaki onay kutusunu **etkilenen** satırları denetlemek Için bir **output** parametresi kullanın.</span><span class="sxs-lookup"><span data-stu-id="b72ea-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="b72ea-166">İşlem çağrıldığında döndürülen değer sıfırsa, bir  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) atılır.</span><span class="sxs-lookup"><span data-stu-id="b72ea-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
- <span data-ttu-id="b72ea-167">Eşzamanlılık denetimi için kullanmak istediğiniz özelliğin yanındaki **özgün değeri kullan** onay kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="b72ea-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="b72ea-168">Bir güncelleştirme denendiğinde, veritabanında ilk olarak okunan özelliğin değeri veritabanına geri veri yazılırken kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="b72ea-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="b72ea-169">Değer veritabanındaki değerle eşleşmiyorsa, bir **OptimisticConcurrencyException** atılır.</span><span class="sxs-lookup"><span data-stu-id="b72ea-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="b72ea-170">Modeli kullanma</span><span class="sxs-lookup"><span data-stu-id="b72ea-170">Use the Model</span></span>

<span data-ttu-id="b72ea-171">**Main** yönteminin tanımlandığı **program.cs** dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="b72ea-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="b72ea-172">Aşağıdaki kodu Main işlevine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b72ea-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="b72ea-173">Kod yeni bir **kişi** nesnesi oluşturur, sonra nesneyi güncelleştirir ve son olarak nesneyi siler.</span><span class="sxs-lookup"><span data-stu-id="b72ea-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>

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

- <span data-ttu-id="b72ea-174">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b72ea-174">Compile and run the application.</span></span> <span data-ttu-id="b72ea-175">Program aşağıdaki çıktıyı üretir \*</span><span class="sxs-lookup"><span data-stu-id="b72ea-175">The program produces the following output \*</span></span>

> [!NOTE]
> <span data-ttu-id="b72ea-176">PersonID, sunucu tarafından otomatik olarak oluşturulur; bu nedenle büyük olasılıkla farklı bir sayı görürsünüz \*</span><span class="sxs-lookup"><span data-stu-id="b72ea-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="b72ea-177">Visual Studio 'nun Ultimate sürümüyle çalışıyorsanız, yürütülen SQL deyimlerini görmek için hata ayıklayıcı ile IntelliTrace kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b72ea-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![IntelliTrace](~/ef6/media/intellitrace.png)
