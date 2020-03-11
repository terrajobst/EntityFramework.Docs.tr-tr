---
title: Tasarımcı varlık bölünmesi-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418464"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="12059-102">Tasarımcı varlığı bölme</span><span class="sxs-lookup"><span data-stu-id="12059-102">Designer Entity Splitting</span></span>
<span data-ttu-id="12059-103">Bu izlenecek yol, bir modeli Entity Framework Designer (EF Designer) ile değiştirerek bir varlık türünün iki tabloya nasıl eşleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="12059-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="12059-104">Tablolar ortak bir anahtar paylaşıyorsa, bir varlığı birden çok tabloya eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="12059-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="12059-105">Bir varlık türünü iki tabloya eşlemek için uygulanan kavramlar, bir varlık türünü ikiden fazla tabloya eşlemek için kolayca genişletilir.</span><span class="sxs-lookup"><span data-stu-id="12059-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="12059-106">Aşağıdaki görüntüde, EF Designer ile çalışırken kullanılan ana pencereler gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="12059-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Tasarımcısı](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="12059-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="12059-108">Prerequisites</span></span>

<span data-ttu-id="12059-109">Visual Studio 2012 veya Visual Studio 2010, Ultimate, Premium, Professional veya Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="12059-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="12059-110">Veritabanını oluşturma</span><span class="sxs-lookup"><span data-stu-id="12059-110">Create the Database</span></span>

<span data-ttu-id="12059-111">Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösteren bir sürümdür:</span><span class="sxs-lookup"><span data-stu-id="12059-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="12059-112">Visual Studio 2012 kullanıyorsanız, LocalDB veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="12059-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="12059-113">Visual Studio 2010 kullanıyorsanız, bir SQL Express veritabanı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="12059-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="12059-114">İlk olarak, tek bir varlıkta birleştirme yapacağımız iki tablo içeren bir veritabanı oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="12059-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="12059-115">Visual Studio’yu açın</span><span class="sxs-lookup"><span data-stu-id="12059-115">Open Visual Studio</span></span>
-   <span data-ttu-id="12059-116">**&gt; Sunucu Gezgini görüntüle**</span><span class="sxs-lookup"><span data-stu-id="12059-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="12059-117">Veri bağlantıları ' na sağ tıklayın **&gt; bağlantı ekle...**</span><span class="sxs-lookup"><span data-stu-id="12059-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="12059-118">Sunucu Gezgini bir veritabanına bağlı değilseniz, veri kaynağı olarak **Microsoft SQL Server** seçmeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="12059-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="12059-119">Hangi hangisinin yüklü olduğuna bağlı olarak, LocalDB veya SQL Express 'e bağlanın</span><span class="sxs-lookup"><span data-stu-id="12059-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="12059-120">Veritabanı adı olarak **Entitybölünmesi** girin</span><span class="sxs-lookup"><span data-stu-id="12059-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="12059-121">**Tamam** ' ı seçtiğinizde, yeni bir veritabanı oluşturmak isteyip istemediğiniz sorulur, **Evet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="12059-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="12059-122">Yeni veritabanı artık Sunucu Gezgini görüntülenir</span><span class="sxs-lookup"><span data-stu-id="12059-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="12059-123">Visual Studio 2012 kullanıyorsanız</span><span class="sxs-lookup"><span data-stu-id="12059-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="12059-124">Sunucu Gezgini veritabanında veritabanına sağ tıklayın ve **Yeni sorgu** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="12059-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="12059-125">Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **Yürüt** ' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="12059-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="12059-126">Visual Studio 2010 kullanıyorsanız</span><span class="sxs-lookup"><span data-stu-id="12059-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="12059-127">**Veri&gt; Transact SQL Düzenleyicisi-&gt; yeni sorgu bağlantısı ' nı seçin...**</span><span class="sxs-lookup"><span data-stu-id="12059-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="12059-128">Sunucu adı olarak **.\\SQLExpress** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="12059-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="12059-129">Sorgu düzenleyicisinin en üstündeki açılan listeden **Entitybölünmesi** veritabanını seçin</span><span class="sxs-lookup"><span data-stu-id="12059-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="12059-130">Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **SQL 'ı Yürüt** ' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="12059-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

## <a name="create-the-project"></a><span data-ttu-id="12059-131">Proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="12059-131">Create the Project</span></span>

-   <span data-ttu-id="12059-132">**Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="12059-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="12059-133">Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol uygulaması** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="12059-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="12059-134">Projenin adı olarak **Mapentitytotablessample** girin ve **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="12059-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="12059-135">İlk bölümde oluşturulan SQL sorgusunu kaydetmek isteyip istemediğiniz sorulduğunda **Hayır** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="12059-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="12059-136">Veritabanını temel alan bir model oluşturma</span><span class="sxs-lookup"><span data-stu-id="12059-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="12059-137">Çözüm Gezgini ' de proje adına sağ tıklayın, **Ekle**' nin üzerine gelin ve ardından **Yeni öğe**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="12059-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="12059-138">Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="12059-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="12059-139">Dosya adı için **Mapentitytotablesmodel. edmx** girin ve ardından **Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="12059-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="12059-140">Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından İleri ' ye tıklayın **.**</span><span class="sxs-lookup"><span data-stu-id="12059-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="12059-141">Açılan listeden **Entitybölünmesi** bağlantısını seçin ve **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="12059-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="12059-142">Veritabanı nesnelerinizi seçin iletişim kutusunda, **tablolar** düğümünün yanındaki kutuyu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="12059-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="12059-143">Bu, tüm tabloları **Entitybölünmesi** veritabanından modele ekler.</span><span class="sxs-lookup"><span data-stu-id="12059-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="12059-144"> **Son**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="12059-144">Click **Finish**.</span></span>

<span data-ttu-id="12059-145">Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="12059-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="12059-146">Bir varlığı Iki tabloya eşleme</span><span class="sxs-lookup"><span data-stu-id="12059-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="12059-147">Bu adımda **Person ve Person** **Info** tablolarından verileri birleştirmek için **kişi** varlık türünü güncelleştireceğiz.</span><span class="sxs-lookup"><span data-stu-id="12059-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="12059-148"> **PersonInfo **varlığının **e-posta** ve **Telefon** özelliklerini seçin ve **CTRL + X** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="12059-148">Select the **Email** and **Phone** properties of the **PersonInfo **entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="12059-149"> **Kişi **varlığını seçin ve **CTRL + V** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="12059-149">Select the **Person **entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="12059-150">Tasarım yüzeyinde, **PersonInfo** varlığını seçin ve klavyede **Sil** düğmesine basın.</span><span class="sxs-lookup"><span data-stu-id="12059-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="12059-151">Modelden Person (kişi) **' ı kaldırmak isteyip istemediğiniz sorulduğunda,** bu **bilgileri** **kişi** varlığıyla eşlemek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="12059-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![Tabloları sil](~/ef6/media/deletetables.png)

<span data-ttu-id="12059-153">Sonraki adımlarda **eşleme ayrıntıları** penceresi gerekir.</span><span class="sxs-lookup"><span data-stu-id="12059-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="12059-154">Bu pencereyi göremiyorsanız, tasarım yüzeyine sağ tıklayıp **eşleme ayrıntıları**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="12059-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="12059-155"> ** varlık** türünü seçin ve **eşleme ayrıntıları** penceresinde **&lt;tablo Ekle veya &gt;görüntüle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="12059-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="12059-156">Açılan listeden **PersonInfo ** seçin.</span><span class="sxs-lookup"><span data-stu-id="12059-156">Select **PersonInfo ** from the drop-down list.</span></span>
    <span data-ttu-id="12059-157"> **Eşleme ayrıntıları** penceresi varsayılan sütun eşlemeleriyle güncelleştirilir, bunlar senaryolarımız için uygundur.</span><span class="sxs-lookup"><span data-stu-id="12059-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="12059-158"> **Kişi** varlık türü artık **kişi** ve **PersonInfo** tablolarıyla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="12059-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Eşleme 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="12059-160">Modeli kullanma</span><span class="sxs-lookup"><span data-stu-id="12059-160">Use the Model</span></span>

-   <span data-ttu-id="12059-161">Aşağıdaki kodu Main yöntemine yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="12059-161">Paste the following code in the Main method.</span></span>

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

-   <span data-ttu-id="12059-162">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="12059-162">Compile and run the application.</span></span>

<span data-ttu-id="12059-163">Aşağıdaki T-SQL deyimleri, bu uygulamayı çalıştırmanın bir sonucu olarak veritabanına karşı yürütüldü.</span><span class="sxs-lookup"><span data-stu-id="12059-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="12059-164">Aşağıdaki iki **Insert** deyimi, bağlamını yürütmenin sonucu olarak yürütüldü. SaveChanges ().</span><span class="sxs-lookup"><span data-stu-id="12059-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="12059-165">Bunlar, **kişi** varlığındaki verileri alır ve **kişi ile Person** **Info** tabloları arasında böler.</span><span class="sxs-lookup"><span data-stu-id="12059-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![1 ekle](~/ef6/media/insert1.png)

    ![2 Ekle](~/ef6/media/insert2.png)
-   <span data-ttu-id="12059-168">Veritabanındaki kişilerin numaralandırıldığı bir sonuç olarak aşağıdaki **seçim** yürütüldü.</span><span class="sxs-lookup"><span data-stu-id="12059-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="12059-169">**Kişi ve Person** **Info** tablosundaki verileri birleştirir.</span><span class="sxs-lookup"><span data-stu-id="12059-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Şunu seçin:](~/ef6/media/select.png)
