---
title: Tasarlayıcı tablosu bölme - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
caps.latest.revision: 3
ms.openlocfilehash: 6ecf9a77f7c6a743e3902de65bb75349c6ef799f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912081"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="d2310-102">Bölme tasarlayıcı tablosu</span><span class="sxs-lookup"><span data-stu-id="d2310-102">Designer Table Splitting</span></span>
<span data-ttu-id="d2310-103">Bu izlenecek yol, birden çok varlık türleri tek bir tabloya bir model Entity Framework Designer (EF Designer) ile değiştirerek eşlemeyle ilgili bilgi gösterir.</span><span class="sxs-lookup"><span data-stu-id="d2310-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="d2310-104">Bölme tablosunu kullanmak isteyebileceğiniz nedenlerinden biri nesnelerinizi yüklemek için yükleme yavaş kullanırken bazı özellikler yüklenmesini geciktirme.</span><span class="sxs-lookup"><span data-stu-id="d2310-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span> <span data-ttu-id="d2310-105">Ayrı bir varlık çok büyük miktarda veri içeren ve yalnızca gerekli olduğunda, yükleme özelliklerini ayırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d2310-105">You can separate the properties that might contain very large amount of data into a seperate entity and only load it when required.</span></span>

<span data-ttu-id="d2310-106">EF Designer ile çalışırken, kullanılan ana windows aşağıdaki resimde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d2310-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="d2310-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="d2310-108">Prerequisites</span></span>

<span data-ttu-id="d2310-109">Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:</span><span class="sxs-lookup"><span data-stu-id="d2310-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="d2310-110">Visual Studio'nun en son sürümü.</span><span class="sxs-lookup"><span data-stu-id="d2310-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="d2310-111">[School örnek veritabanını](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="d2310-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="d2310-112">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="d2310-112">Set up the Project</span></span>

<span data-ttu-id="d2310-113">Bu izlenecek yol, Visual Studio 2012 kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="d2310-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="d2310-114">Visual Studio 2012'yi açın.</span><span class="sxs-lookup"><span data-stu-id="d2310-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="d2310-115">Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="d2310-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="d2310-116">Sol bölmede, Visual C tıklayın\#ve ardından konsol uygulaması şablonu seçin.</span><span class="sxs-lookup"><span data-stu-id="d2310-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="d2310-117">Girin **TableSplittingSample** tıklayın ve proje adı olarak **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="d2310-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="d2310-118">School veritabanını temel alan bir Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="d2310-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="d2310-119">Çözüm Gezgini'nde proje adına sağ tıklayın, fareyle **Ekle**ve ardından **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="d2310-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="d2310-120">Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="d2310-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="d2310-121">Girin **TableSplittingModel.edmx** dosya adı ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="d2310-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="d2310-122">Choose Model Contents iletişim kutusunda **veritabanından Oluştur**ve ardından **sonraki.**</span><span class="sxs-lookup"><span data-stu-id="d2310-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="d2310-123">Yeni bağlantı tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d2310-123">Click New Connection.</span></span> <span data-ttu-id="d2310-124">Bağlantı Özellikleri iletişim kutusuna sunucu adını girin (örneğin, **(localdb)\\ifadesini mssqllocaldb**) seçin kimlik doğrulama yöntemi, tür **Okul** veritabanı adı ve ardından tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="d2310-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="d2310-125">Veri bağlantınızı seçin iletişim kutusunda, veritabanı bağlantı ayarı ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d2310-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="d2310-126">Veritabanı nesnelerinizi seçin iletişim kutusunda, düzleştirme **tabloları** düğüm ve onay **kişi** tablo.</span><span class="sxs-lookup"><span data-stu-id="d2310-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="d2310-127">Bu belirtilen tabloya ekler **Okul** modeli.</span><span class="sxs-lookup"><span data-stu-id="d2310-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="d2310-128">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d2310-128">Click **Finish**.</span></span>

<span data-ttu-id="d2310-129">Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d2310-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="d2310-130">Seçtiğiniz tüm nesneleri **veritabanı nesnelerinizi seçin** iletişim kutusu, modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="d2310-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="d2310-131">Tek bir tabloya iki varlıkları Eşle</span><span class="sxs-lookup"><span data-stu-id="d2310-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="d2310-132">Bu bölümde, bölecek **kişi** iki varlık varlığa ve ardından bunları tek bir tabloya eşleyin.</span><span class="sxs-lookup"><span data-stu-id="d2310-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="d2310-133">**Kişi** varlık büyük miktarda veriler içeren tüm özellikleri içermez; yalnızca örnek olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d2310-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="d2310-134">Tasarım yüzeyinde boş bir alana sağ tıklayın, fareyle **yeni Ekle**, tıklatıp **varlık**.</span><span class="sxs-lookup"><span data-stu-id="d2310-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="d2310-135">**Yeni varlık** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d2310-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="d2310-136">Tür **HireInfo** için **varlık adı** ve **Personıd** için **anahtar özellik** adı.</span><span class="sxs-lookup"><span data-stu-id="d2310-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="d2310-137">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="d2310-137">Click **OK**.</span></span>
-   <span data-ttu-id="d2310-138">Yeni bir varlık türü oluşturulur ve tasarım yüzeyinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d2310-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="d2310-139">Seçin **İşeAlmaTarihi** özelliği **kişi** varlık yazın ve ENTER tuşuna **Ctrl + X** anahtarları.</span><span class="sxs-lookup"><span data-stu-id="d2310-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="d2310-140">Seçin **HireInfo** varlık ve ENTER tuşuna **Ctrl + V** anahtarları.</span><span class="sxs-lookup"><span data-stu-id="d2310-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="d2310-141">Arasında ilişkilendirme oluşturma **kişi** ve **HireInfo**.</span><span class="sxs-lookup"><span data-stu-id="d2310-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="d2310-142">Bunu yapmak için tasarım yüzeyinde boş bir alana sağ tıklayın, işaret **yeni Ekle**, tıklatıp **ilişkilendirme**.</span><span class="sxs-lookup"><span data-stu-id="d2310-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="d2310-143">**Ekleme ilişkilendirme** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d2310-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="d2310-144">**PersonHireInfo** adı varsayılan olarak verilir.</span><span class="sxs-lookup"><span data-stu-id="d2310-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="d2310-145">Çokluk belirtin **1(One)** ilişkinin her iki ucunda.</span><span class="sxs-lookup"><span data-stu-id="d2310-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="d2310-146">Tuşuna **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="d2310-146">Press **OK**.</span></span>

<span data-ttu-id="d2310-147">Sonraki adım gerektirir **eşleşme ayrıntıları** penceresi.</span><span class="sxs-lookup"><span data-stu-id="d2310-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="d2310-148">Bu pencere göremiyorsanız, tasarım yüzeyi ve select sağ **eşleşme ayrıntıları**.</span><span class="sxs-lookup"><span data-stu-id="d2310-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="d2310-149">Seçin **HireInfo** varlık türü ve tıklatın **&lt;bir tablo veya Görünüm Ekle&gt;** içinde **eşleşme ayrıntıları** penceresi.</span><span class="sxs-lookup"><span data-stu-id="d2310-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="d2310-150">Seçin **kişi** gelen **&lt;bir tablo veya Görünüm Ekle&gt;** alan açılan listesi.</span><span class="sxs-lookup"><span data-stu-id="d2310-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="d2310-151">Listenin tabloları içeren veya seçilen varlığın hangi eşlenebilir için görüntüler.</span><span class="sxs-lookup"><span data-stu-id="d2310-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="d2310-152">Uygun özellikleri varsayılan olarak eşlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="d2310-152">The appropriate properties should be mapped by default.</span></span>

    ![Eşleme](~/ef6/media/mapping.png)

-   <span data-ttu-id="d2310-154">Seçin **PersonHireInfo** tasarım yüzeyinde ilişkilendirme.</span><span class="sxs-lookup"><span data-stu-id="d2310-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="d2310-155">Tasarım yüzeyi ve select ilişkilendirmenin sağ **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="d2310-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="d2310-156">İçinde **özellikleri** penceresinde **başvuru kısıtlamalarını** özelliği ve üç nokta düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d2310-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="d2310-157">Seçin **kişi** gelen **asıl** aşağı açılan listesi.</span><span class="sxs-lookup"><span data-stu-id="d2310-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="d2310-158">Tuşuna **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="d2310-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="d2310-159">Kullanım modeli</span><span class="sxs-lookup"><span data-stu-id="d2310-159">Use the Model</span></span>

-   <span data-ttu-id="d2310-160">Main yöntemine aşağıdaki kodu yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="d2310-160">Paste the following code in the Main method.</span></span>

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
-   <span data-ttu-id="d2310-161">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d2310-161">Compile and run the application.</span></span>

<span data-ttu-id="d2310-162">Aşağıdaki T-SQL deyimlerini karşı yürütüldü **Okul** sonucunda bu uygulamayı çalıştırmadan veritabanı.</span><span class="sxs-lookup"><span data-stu-id="d2310-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="d2310-163">Aşağıdaki **Ekle** bağlam yürütmenin sonucu olarak yürütülmesi. SaveChanges() ve birleştirir verilerinden **kişi** ve **HireInfo** varlıklar</span><span class="sxs-lookup"><span data-stu-id="d2310-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Ekleme](~/ef6/media/insert.png)

-   <span data-ttu-id="d2310-165">Aşağıdaki **seçin** bağlam yürütmenin sonucu olarak yürütülmesi. People.FirstOrDefault() ve sütunları eşlenen seçer **kişi**</span><span class="sxs-lookup"><span data-stu-id="d2310-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Select1](~/ef6/media/select1.png)

-   <span data-ttu-id="d2310-167">Aşağıdaki **seçin** gezinti özelliği existingPerson.Instructor erişme sonucu olarak çalıştırılan ve yalnızca eşlenen sütun seçer **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="d2310-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Select2](~/ef6/media/select2.png)