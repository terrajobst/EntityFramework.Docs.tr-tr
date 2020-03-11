---
title: Tasarımcı tablosu bölme-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f5e7532e6c0b473d8ce77cbd11e3e673b0af6cbe
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418170"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="44874-102">Tasarımcı tablosu bölme</span><span class="sxs-lookup"><span data-stu-id="44874-102">Designer Table Splitting</span></span>
<span data-ttu-id="44874-103">Bu izlenecek yol, bir modeli Entity Framework Designer (EF Designer) ile değiştirerek birden çok varlık türünün tek bir tabloya nasıl eşlendiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="44874-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="44874-104">Tablo bölmeyi kullanmak isteyebileceğiniz bir nedenden dolayı, nesnelerinizi yüklemek için yavaş yükleme kullandığınızda bazı özelliklerin yüklenmesi ertelenebilir.</span><span class="sxs-lookup"><span data-stu-id="44874-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span><span data-ttu-id="44874-105"> Büyük miktarda veri içerebilen özellikleri ayrı bir varlıkta ayırabilir ve yalnızca gerektiğinde yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="44874-105"> You can separate the properties that might contain very large amount of data into a separate entity and only load it when required.</span></span>

<span data-ttu-id="44874-106">Aşağıdaki görüntüde, EF Designer ile çalışırken kullanılan ana pencereler gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="44874-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Tasarımcısı](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="44874-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="44874-108">Prerequisites</span></span>

<span data-ttu-id="44874-109">Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:</span><span class="sxs-lookup"><span data-stu-id="44874-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="44874-110">Visual Studio 'nun son sürümü.</span><span class="sxs-lookup"><span data-stu-id="44874-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="44874-111">[Okul örnek veritabanı](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="44874-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="44874-112">Projeyi ayarlama</span><span class="sxs-lookup"><span data-stu-id="44874-112">Set up the Project</span></span>

<span data-ttu-id="44874-113">Bu izlenecek yol, Visual Studio 2012 ' i kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="44874-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="44874-114">Visual Studio 2012'yi açın.</span><span class="sxs-lookup"><span data-stu-id="44874-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="44874-115">**Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="44874-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="44874-116">Sol bölmede, Visual C\#' ye tıklayın ve ardından konsol uygulaması şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="44874-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="44874-117">Projenin adı olarak **Tablespttingsample** girin ve **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="44874-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="44874-118">Okul veritabanına dayalı bir model oluşturma</span><span class="sxs-lookup"><span data-stu-id="44874-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="44874-119">Çözüm Gezgini ' de proje adına sağ tıklayın, **Ekle**' nin üzerine gelin ve ardından **Yeni öğe**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="44874-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="44874-120">Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="44874-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="44874-121">Dosya adı için **TableSplittingModel. edmx** girin ve ardından **Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="44874-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="44874-122">Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından İleri ' ye tıklayın **.**</span><span class="sxs-lookup"><span data-stu-id="44874-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="44874-123">Yeni bağlantı ' ya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="44874-123">Click New Connection.</span></span> <span data-ttu-id="44874-124">Bağlantı özellikleri iletişim kutusunda sunucu adını (örneğin, **(LocalDB)\\mssqllocaldb**) girin, kimlik doğrulama yöntemini seçin, veritabanı adı için **okul** yazın ve ardından **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="44874-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="44874-125">Veri bağlantınızı seçin iletişim kutusu, veritabanı bağlantı ayarınız ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="44874-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="44874-126">Veritabanı nesnelerinizi seçin iletişim kutusunda **tablo** düğümünü katlayın ve **kişi** tablosunu kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="44874-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="44874-127">Bu, belirtilen tabloyu **okul** modeline ekler.</span><span class="sxs-lookup"><span data-stu-id="44874-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="44874-128"> **Son**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="44874-128">Click **Finish**.</span></span>

<span data-ttu-id="44874-129">Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="44874-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="44874-130"> **Veritabanı nesnelerinizi seçin** iletişim kutusunda seçtiğiniz tüm nesneler modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="44874-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="44874-131">Iki varlığı tek bir tabloyla eşleyin</span><span class="sxs-lookup"><span data-stu-id="44874-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="44874-132">Bu bölümde, **kişi** varlığını iki varlığa bölecektir ve sonra bunları tek bir tabloyla eşleştirirsiniz.</span><span class="sxs-lookup"><span data-stu-id="44874-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="44874-133">**Kişi** varlığı, büyük miktarda veri içerebilen herhangi bir özellik içermez; yalnızca örnek olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="44874-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="44874-134">Tasarım yüzeyinde boş bir alana sağ tıklayın, **Yeni Ekle**' nin üzerine gelin ve **varlık**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="44874-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="44874-135"> **Yeni varlık** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="44874-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="44874-136">**Anahtar özellik** adı için **varlık adı** ve **PersonID** için **hireınfo** yazın.</span><span class="sxs-lookup"><span data-stu-id="44874-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="44874-137"> **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="44874-137">Click **OK**.</span></span>
-   <span data-ttu-id="44874-138">Tasarım yüzeyinde yeni bir varlık türü oluşturulur ve görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="44874-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="44874-139"> **Kişi** varlık türünün **hiredate** özelliğini seçin ve **CTRL + X** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="44874-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="44874-140">**Hireınfo** varlığını seçin ve **CTRL + V** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="44874-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="44874-141">**Kişi** ve **hireınfo**arasında bir ilişki oluşturun.</span><span class="sxs-lookup"><span data-stu-id="44874-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="44874-142">Bunu yapmak için tasarım yüzeyinde boş bir alana sağ tıklayın, **Yeni Ekle**' nin üzerine gelin ve **ilişkilendirme**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="44874-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="44874-143"> **Ilişki ekle** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="44874-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="44874-144">**Personhireınfo** adı varsayılan olarak verilir.</span><span class="sxs-lookup"><span data-stu-id="44874-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="44874-145">İlişkinin her iki ucunda çeşitlilik **1 (bir)** belirtin.</span><span class="sxs-lookup"><span data-stu-id="44874-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="44874-146">**Tamam**'a basın.</span><span class="sxs-lookup"><span data-stu-id="44874-146">Press **OK**.</span></span>

<span data-ttu-id="44874-147">Sonraki adım için **eşleme ayrıntıları** penceresi gerekir.</span><span class="sxs-lookup"><span data-stu-id="44874-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="44874-148">Bu pencereyi göremiyorsanız, tasarım yüzeyine sağ tıklayıp **eşleme ayrıntıları**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="44874-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="44874-149"> ** varlık** türünü seçin ve **eşleme ayrıntıları** penceresinde **&lt;tablo ekleme veya&gt; görüntüleme** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="44874-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="44874-150">**&lt;bir tablo eklemek veya &gt;** alan açılır listesinden **kişi** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="44874-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="44874-151">Liste, seçilen varlığın eşleştiribileceği tabloları veya görünümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="44874-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="44874-152">Uygun özellikler varsayılan olarak eşlenmelidir.</span><span class="sxs-lookup"><span data-stu-id="44874-152">The appropriate properties should be mapped by default.</span></span>

    ![Eşleme](~/ef6/media/mapping.png)

-   <span data-ttu-id="44874-154">Tasarım yüzeyinde **Personhireınfo** ilişkilendirmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="44874-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="44874-155">Tasarım yüzeyinde ilişkiye sağ tıklayın ve **Özellikler**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="44874-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="44874-156">**Özellikler** penceresinde **başvurusal kısıtlamalar** özelliğini seçin ve üç nokta düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="44874-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="44874-157">**Sorumlu** açılan listesinden **kişi** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="44874-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="44874-158">**Tamam**'a basın.</span><span class="sxs-lookup"><span data-stu-id="44874-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="44874-159">Modeli kullanma</span><span class="sxs-lookup"><span data-stu-id="44874-159">Use the Model</span></span>

-   <span data-ttu-id="44874-160">Aşağıdaki kodu Main yöntemine yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="44874-160">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   <span data-ttu-id="44874-161">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="44874-161">Compile and run the application.</span></span>

<span data-ttu-id="44874-162">Aşağıdaki T-SQL deyimleri, bu uygulamayı çalıştırmanın bir sonucu olarak **okul** veritabanına karşı yürütüldü.</span><span class="sxs-lookup"><span data-stu-id="44874-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="44874-163">Aşağıdaki **ekleme** bağlam yürütmenin sonucu olarak yürütüldü. SaveChanges () ve **kişi** ve **hireınfo** varlıklarındaki verileri birleştirir</span><span class="sxs-lookup"><span data-stu-id="44874-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Ekle](~/ef6/media/insert.png)

-   <span data-ttu-id="44874-165">Aşağıdaki **seçim** , bağlam yürütmenin sonucu olarak yürütüldü. Kişiler. FirstOrDefault () ve yalnızca **kişiyle** eşleştirilmiş sütunları seçer</span><span class="sxs-lookup"><span data-stu-id="44874-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![1 seçin](~/ef6/media/select1.png)

-   <span data-ttu-id="44874-167">Şu **Select** , Existingperson. eğitmeni gezinti özelliğine erişmenin ve yalnızca **hireınfo** ile eşleştirilmiş sütunları seçen bir sonuç olarak yürütüldü</span><span class="sxs-lookup"><span data-stu-id="44874-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![2 seçin](~/ef6/media/select2.png)
