---
title: Tasarımcı varlık bölme - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: 214561f0a0381bced3ceae0b6acfcd45f5dd65c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995625"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="5271d-102">Tasarımcı varlık bölme</span><span class="sxs-lookup"><span data-stu-id="5271d-102">Designer Entity Splitting</span></span>
<span data-ttu-id="5271d-103">Bu izlenecek yol, bir model Entity Framework Designer (EF Designer) ile değiştirerek, iki tabloya bir varlık türü eşlemeyle ilgili bilgi gösterir.</span><span class="sxs-lookup"><span data-stu-id="5271d-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="5271d-104">Tablolar, bir ortak anahtar paylaştığınızda bir varlık için birden çok tablo eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5271d-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="5271d-105">İki tabloya bir varlık türü eşlemek için uygulanacak kavramları kolayca ikiden fazla tablolara eşleme bir varlık türü için genişletilir.</span><span class="sxs-lookup"><span data-stu-id="5271d-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="5271d-106">EF Designer ile çalışırken, kullanılan ana windows aşağıdaki resimde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="5271d-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="5271d-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="5271d-108">Prerequisites</span></span>

<span data-ttu-id="5271d-109">Visual Studio 2012 veya Visual Studio 2010, Ultimate, Premium, Professional veya Web Express sürümü.</span><span class="sxs-lookup"><span data-stu-id="5271d-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="5271d-110">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="5271d-110">Create the Database</span></span>

<span data-ttu-id="5271d-111">Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösterir:</span><span class="sxs-lookup"><span data-stu-id="5271d-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="5271d-112">Visual Studio 2012 kullanıyorsanız, bir LocalDB veritabanına oluşturmayı.</span><span class="sxs-lookup"><span data-stu-id="5271d-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="5271d-113">Visual Studio 2010 kullanıyorsanız, SQL Express veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="5271d-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="5271d-114">İlk iki tablolarla birleştirmek için tek bir varlığa kullanacağız bir veritabanı oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="5271d-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="5271d-115">Visual Studio'yu Aç</span><span class="sxs-lookup"><span data-stu-id="5271d-115">Open Visual Studio</span></span>
-   <span data-ttu-id="5271d-116">**Görünüm -&gt; Sunucu Gezgini**</span><span class="sxs-lookup"><span data-stu-id="5271d-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="5271d-117">Sağ tıklayın **veri bağlantıları -&gt; bağlantı ekle...**</span><span class="sxs-lookup"><span data-stu-id="5271d-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="5271d-118">Sunucu gezgininden veritabanına bağlamadıysanız seçmeniz gerekir önce **Microsoft SQL Server** veri kaynağı</span><span class="sxs-lookup"><span data-stu-id="5271d-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="5271d-119">LocalDB veya hangisinin bağlı olarak yüklediğiniz SQL Express için Bağlan</span><span class="sxs-lookup"><span data-stu-id="5271d-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="5271d-120">Girin **EntitySplitting** veritabanı adı</span><span class="sxs-lookup"><span data-stu-id="5271d-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="5271d-121">Seçin **Tamam** ve bir yeni bir veritabanı oluşturmak istiyorsanız istenir **Evet**</span><span class="sxs-lookup"><span data-stu-id="5271d-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="5271d-122">Yeni veritabanı şimdi sunucu Gezgini'nde görünür.</span><span class="sxs-lookup"><span data-stu-id="5271d-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="5271d-123">Visual Studio 2012 kullanıyorsanız</span><span class="sxs-lookup"><span data-stu-id="5271d-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="5271d-124">Sunucu Gezgini veritabanı üzerinde sağ tıklayıp **yeni sorgu**</span><span class="sxs-lookup"><span data-stu-id="5271d-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="5271d-125">Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **Yürüt**</span><span class="sxs-lookup"><span data-stu-id="5271d-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="5271d-126">Visual Studio 2010 kullanıyorsanız</span><span class="sxs-lookup"><span data-stu-id="5271d-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="5271d-127">Seçin **veri -&gt; Transact - SQL Düzenleyicisi&gt; yeni bağlantı...**</span><span class="sxs-lookup"><span data-stu-id="5271d-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="5271d-128">Girin **.\\ SQLEXPRESS** tıklayın ve sunucu adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="5271d-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="5271d-129">Seçin **EntitySplitting** veritabanı açılır menüden aşağı sorgu Düzenleyicisi'ni üstünde</span><span class="sxs-lookup"><span data-stu-id="5271d-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="5271d-130">Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **SQL Yürüt**</span><span class="sxs-lookup"><span data-stu-id="5271d-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a><span data-ttu-id="5271d-131">Proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="5271d-131">Create the Project</span></span>

-   <span data-ttu-id="5271d-132">Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="5271d-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="5271d-133">Sol bölmede **Visual C\#** ve ardından **konsol uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="5271d-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="5271d-134">Girin **MapEntityToTablesSample** tıklayın ve proje adı olarak **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="5271d-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="5271d-135">Tıklayın **Hayır** ilk bölümde oluşturduğunuz SQL sorguyu kaydetmek isteyip istemediğiniz sorulduğunda.</span><span class="sxs-lookup"><span data-stu-id="5271d-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="5271d-136">Veritabanını temel alan bir Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="5271d-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="5271d-137">Çözüm Gezgini'nde proje adına sağ tıklayın, fareyle **Ekle**ve ardından **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="5271d-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="5271d-138">Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="5271d-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="5271d-139">Girin **MapEntityToTablesModel.edmx** dosya adı ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5271d-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="5271d-140">Choose Model Contents iletişim kutusunda **veritabanından Oluştur**ve ardından **sonraki.**</span><span class="sxs-lookup"><span data-stu-id="5271d-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="5271d-141">Seçin **EntitySplitting** açılan bağlantı ve **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="5271d-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="5271d-142">Veritabanı nesnelerinizi seçin iletişim kutusunda yanındaki kutuyu işaretleyin **tabloları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="5271d-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="5271d-143">Bu tablodan tüm ekler **EntitySplitting** veritabanı modeli.</span><span class="sxs-lookup"><span data-stu-id="5271d-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="5271d-144">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="5271d-144">Click **Finish**.</span></span>

<span data-ttu-id="5271d-145">Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="5271d-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="5271d-146">Bir varlık için iki tablo eşleme</span><span class="sxs-lookup"><span data-stu-id="5271d-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="5271d-147">Bu adımda güncelleştireceğiz **kişi** alınan verileri birleştirmek varlık türü **kişi** ve **PersonInfo** tablolar.</span><span class="sxs-lookup"><span data-stu-id="5271d-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="5271d-148">Seçin **e-posta** ve **telefon** özelliklerini ** PersonInfo ** varlık ve ENTER tuşuna **Ctrl + X** anahtarları.</span><span class="sxs-lookup"><span data-stu-id="5271d-148">Select the **Email** and **Phone** properties of the **PersonInfo **entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="5271d-149">Seçin ** kişi ** varlık ve ENTER tuşuna **Ctrl + V** anahtarları.</span><span class="sxs-lookup"><span data-stu-id="5271d-149">Select the **Person **entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="5271d-150">Tasarım yüzeyinde seçin **PersonInfo** varlık ve ENTER tuşuna **Sil** klavyedeki düğmesi.</span><span class="sxs-lookup"><span data-stu-id="5271d-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="5271d-151">Tıklayın **Hayır** kaldırmak isteyip istemediğiniz sorulduğunda **PersonInfo** tablo hakkında eşlemek üzere duyuyoruz modelden **kişi** varlık.</span><span class="sxs-lookup"><span data-stu-id="5271d-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![DeleteTables](~/ef6/media/deletetables.png)

<span data-ttu-id="5271d-153">Sonraki adımlar gerektiren **eşleşme ayrıntıları** penceresi.</span><span class="sxs-lookup"><span data-stu-id="5271d-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="5271d-154">Bu pencere göremiyorsanız, tasarım yüzeyi ve select sağ **eşleşme ayrıntıları**.</span><span class="sxs-lookup"><span data-stu-id="5271d-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="5271d-155">Seçin **kişi** varlık türü ve tıklatın **&lt;bir tablo veya Görünüm Ekle&gt;** içinde **eşleşme ayrıntıları** penceresi.</span><span class="sxs-lookup"><span data-stu-id="5271d-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="5271d-156">Seçin ** PersonInfo ** aşağı açılan listeden.</span><span class="sxs-lookup"><span data-stu-id="5271d-156">Select **PersonInfo ** from the drop-down list.</span></span>
    <span data-ttu-id="5271d-157">**Eşleşme ayrıntıları** penceresi, varsayılan sütun eşlemelerini ile güncelleştirilir, bunlar bizim senaryomuz için bir sakınca yoktur.</span><span class="sxs-lookup"><span data-stu-id="5271d-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="5271d-158">**Kişi** varlık türü için eşlenmiş artık **kişi** ve **PersonInfo** tablolar.</span><span class="sxs-lookup"><span data-stu-id="5271d-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Mapping2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="5271d-160">Kullanım modeli</span><span class="sxs-lookup"><span data-stu-id="5271d-160">Use the Model</span></span>

-   <span data-ttu-id="5271d-161">Main yöntemine aşağıdaki kodu yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="5271d-161">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   <span data-ttu-id="5271d-162">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5271d-162">Compile and run the application.</span></span>

<span data-ttu-id="5271d-163">Bu uygulamayı çalıştıran sonucunda veritabanında aşağıdaki T-SQL deyimlerini yürütüldü.</span><span class="sxs-lookup"><span data-stu-id="5271d-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="5271d-164">Aşağıdaki iki **Ekle** deyimleri yürütülen bağlam yürütmenin sonucu olarak. SaveChanges().</span><span class="sxs-lookup"><span data-stu-id="5271d-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="5271d-165">Verilerden aldıkları **kişi** varlık ve arasında bölmek **kişi** ve **PersonInfo** tablolar.</span><span class="sxs-lookup"><span data-stu-id="5271d-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![Insert1](~/ef6/media/insert1.png)

    ![Insert2](~/ef6/media/insert2.png)
-   <span data-ttu-id="5271d-168">Aşağıdaki **seçin** veritabanında kişiler numaralandırma sonucu olarak yürütülmesi.</span><span class="sxs-lookup"><span data-stu-id="5271d-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="5271d-169">Verileri birleştirir **kişi** ve **PersonInfo** tablo.</span><span class="sxs-lookup"><span data-stu-id="5271d-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Seçim](~/ef6/media/select.png)
