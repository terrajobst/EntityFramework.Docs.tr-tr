---
title: Sorgu - EF Designer - EF6 tanımlama
author: divega
ms.date: 10/23/2016
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: b1589dc12ccb50754c2e950932a2d82bc4869f6b
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489485"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="926f1-102">Sorgu - EF Designer tanımlama</span><span class="sxs-lookup"><span data-stu-id="926f1-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="926f1-103">Bu kılavuzda bir tanımlama ekleneceği gösterilmektedir. sorgu ve karşılık gelen bir varlığın EF Designer kullanarak bir modele yazın.</span><span class="sxs-lookup"><span data-stu-id="926f1-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="926f1-104">Tanımlayan bir sorgu için bir veritabanı görünümü tarafından sağlanan benzer işlevselliği sağlamak için yaygın olarak kullanılır, ancak görünüm modeli, veritabanı tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="926f1-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="926f1-105">Tanımlayan bir sorgu içinde belirtilen bir SQL deyimi yürütme sağlar **DefiningQuery** bir .edmx dosyası öğesidir.</span><span class="sxs-lookup"><span data-stu-id="926f1-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="926f1-106">Daha fazla bilgi için **DefiningQuery** içinde [SSDL belirtimi](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="926f1-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="926f1-107">Tanımlama sorguları kullanırken, ayrıca bir varlık türü modelinizde tanımlamak vardır.</span><span class="sxs-lookup"><span data-stu-id="926f1-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="926f1-108">Varlık türü tanımlayan sorgu tarafından kullanıma sunulan veri yüzey için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="926f1-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="926f1-109">Bu varlık türü ortaya veri salt okunur olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="926f1-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="926f1-110">Parametreli sorgular sorguları tanımlama olarak yürütülemiyor.</span><span class="sxs-lookup"><span data-stu-id="926f1-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="926f1-111">Ancak, veri, INSERT, update ve delete işlevleri varlık türünün bu yüzeyleri veriler için saklı yordamlar eşleyerek güncelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="926f1-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="926f1-112">Daha fazla bilgi için [INSERT, Update ve Delete saklı yordamlarla](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="926f1-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="926f1-113">Bu konu aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="926f1-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="926f1-114">Tanımlayan bir sorgu Ekle</span><span class="sxs-lookup"><span data-stu-id="926f1-114">Add a Defining Query</span></span>
-   <span data-ttu-id="926f1-115">Modele bir varlık türü Ekle</span><span class="sxs-lookup"><span data-stu-id="926f1-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="926f1-116">Varlık türü tanımlayan sorgu eşleme</span><span class="sxs-lookup"><span data-stu-id="926f1-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="926f1-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="926f1-117">Prerequisites</span></span>

<span data-ttu-id="926f1-118">Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:</span><span class="sxs-lookup"><span data-stu-id="926f1-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="926f1-119">Visual Studio'nun en son sürümü.</span><span class="sxs-lookup"><span data-stu-id="926f1-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="926f1-120">[School örnek veritabanını](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="926f1-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="926f1-121">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="926f1-121">Set up the Project</span></span>

<span data-ttu-id="926f1-122">Bu izlenecek yol, Visual Studio 2012 veya daha yeni kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="926f1-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="926f1-123">Visual Studio'yu açın.</span><span class="sxs-lookup"><span data-stu-id="926f1-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="926f1-124">Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="926f1-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="926f1-125">Sol bölmede **Visual C\#** ve ardından **konsol uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="926f1-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="926f1-126">Girin **DefiningQuerySample** tıklayın ve proje adı olarak **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="926f1-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="926f1-127">School veritabanını temel alan bir Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="926f1-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="926f1-128">Çözüm Gezgini'nde proje adına sağ tıklayın, fareyle **Ekle**ve ardından **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="926f1-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="926f1-129">Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="926f1-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="926f1-130">Girin **DefiningQueryModel.edmx** dosya adı ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="926f1-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="926f1-131">Choose Model Contents iletişim kutusunda **veritabanından Oluştur**ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="926f1-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="926f1-132">Yeni bağlantı tıklayın.</span><span class="sxs-lookup"><span data-stu-id="926f1-132">Click New Connection.</span></span> <span data-ttu-id="926f1-133">Bağlantı Özellikleri iletişim kutusuna sunucu adını girin (örneğin, **(localdb)\\ifadesini mssqllocaldb**) seçin kimlik doğrulama yöntemi, tür **Okul** veritabanı adı ve ardından tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="926f1-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="926f1-134">Veri bağlantınızı seçin iletişim kutusunda, veritabanı bağlantı ayarı ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="926f1-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="926f1-135">Veritabanı nesnelerinizi seçin iletişim kutusunda, denetleyin **tabloları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="926f1-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="926f1-136">Bu, tüm tablolara ekler **Okul** modeli.</span><span class="sxs-lookup"><span data-stu-id="926f1-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="926f1-137">**Son**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="926f1-137">Click **Finish**.</span></span>
-   <span data-ttu-id="926f1-138">Çözüm Gezgini'nde sağ **DefiningQueryModel.edmx** seçin ve dosya **birlikte Aç...** .</span><span class="sxs-lookup"><span data-stu-id="926f1-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="926f1-139">Seçin **XML (metin) Düzenleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="926f1-139">Select **XML (Text) Editor**.</span></span>

    ![XML Düzenleyicisi](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="926f1-141">Tıklayın **Evet** şu ileti ile istenirse:</span><span class="sxs-lookup"><span data-stu-id="926f1-141">Click **Yes** if prompted with the following message:</span></span>

    ![Uyarı 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="926f1-143">Tanımlayan bir sorgu Ekle</span><span class="sxs-lookup"><span data-stu-id="926f1-143">Add a Defining Query</span></span>

<span data-ttu-id="926f1-144">Bir tanımlama eklemek için XML Düzenleyicisi'ni kullanacağız Bu adımda sorgulamak ve bir varlık .edmx dosyasını SSDL bölümünü yazın.</span><span class="sxs-lookup"><span data-stu-id="926f1-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="926f1-145">Ekleme bir **EntitySet** SSDL bölümüne .edmx dosyasını (Satır 5 13'ya kadar geçerli) öğesi.</span><span class="sxs-lookup"><span data-stu-id="926f1-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="926f1-146">Aşağıdakileri belirtin:</span><span class="sxs-lookup"><span data-stu-id="926f1-146">Specify the following:</span></span>
    -   <span data-ttu-id="926f1-147">Yalnızca **adı** ve **EntityType** özniteliklerini **EntitySet** öğe belirtilir.</span><span class="sxs-lookup"><span data-stu-id="926f1-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="926f1-148">Varlık türü tam adı kullanılıyor **EntityType** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="926f1-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="926f1-149">Çalıştırılacak SQL deyimi belirtilen **DefiningQuery** öğesi.</span><span class="sxs-lookup"><span data-stu-id="926f1-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   <span data-ttu-id="926f1-150">Ekleme **EntityType** .edmx SSDL bölümüne öğesi.</span><span class="sxs-lookup"><span data-stu-id="926f1-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="926f1-151">Aşağıdaki dosya gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="926f1-151">file as shown below.</span></span> <span data-ttu-id="926f1-152">Şunlara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="926f1-152">Note the following:</span></span>
    -   <span data-ttu-id="926f1-153">Değerini **adı** öznitelik değerine karşılık gelen **EntityType** özniteliğini **EntitySet** öğesi yukarıdaki rağmen tam adı varlık türü kullanılan **EntityType** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="926f1-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="926f1-154">Özellik adlarını SQL deyiminin döndürdüğü sütun adlarına karşılık gelen **DefiningQuery** öğesi (yukarıda).</span><span class="sxs-lookup"><span data-stu-id="926f1-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="926f1-155">Bu örnekte, varlık anahtarı benzersiz bir anahtar değeri sağlamak için üç özelliklerinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="926f1-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> <span data-ttu-id="926f1-156">Daha sonra çalıştırdığınızda **güncelleştirme modeli Sihirbazı** iletişim kutusunda, sorguları tanımlama dahil olmak üzere depolama modelinde yapılan değişiklikleri yazılır.</span><span class="sxs-lookup"><span data-stu-id="926f1-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="926f1-157">Modele bir varlık türü Ekle</span><span class="sxs-lookup"><span data-stu-id="926f1-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="926f1-158">Bu adımda EF Designer kullanarak kavramsal modele varlık türü ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="926f1-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span>  <span data-ttu-id="926f1-159">Şunlara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="926f1-159">Note the following:</span></span>

-   <span data-ttu-id="926f1-160">**Adı** varlığı değerine karşılık gelen **EntityType** özniteliğini **EntitySet** yukarıdaki öğesi.</span><span class="sxs-lookup"><span data-stu-id="926f1-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="926f1-161">Özellik adlarını SQL deyiminin döndürdüğü sütun adlarına karşılık gelen **DefiningQuery** yukarıdaki öğesi.</span><span class="sxs-lookup"><span data-stu-id="926f1-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="926f1-162">Bu örnekte, varlık anahtarı benzersiz bir anahtar değeri sağlamak için üç özelliklerinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="926f1-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="926f1-163">Model EF Designer ile açın.</span><span class="sxs-lookup"><span data-stu-id="926f1-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="926f1-164">DefiningQueryModel.edmx çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="926f1-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="926f1-165">Söyleyin **Evet** aşağıdaki iletisi:</span><span class="sxs-lookup"><span data-stu-id="926f1-165">Say **Yes** to the following message:</span></span>

    ![Uyarı 2](~/ef6/media/warning2.png)

 

<span data-ttu-id="926f1-167">Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="926f1-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="926f1-168">Tasarımcı yüzeyi ve select sağ **yeni Ekle**-&gt;**varlık...** .</span><span class="sxs-lookup"><span data-stu-id="926f1-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="926f1-169">Belirtin **GradeReport** varlık adı için ve **CourseID** için **anahtar özellik**.</span><span class="sxs-lookup"><span data-stu-id="926f1-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="926f1-170">Sağ **GradeReport** varlık ve select **yeni Ekle** - &gt; **skaler özelliği**.</span><span class="sxs-lookup"><span data-stu-id="926f1-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="926f1-171">Özellik için varsayılan adı değiştirmek **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="926f1-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="926f1-172">Başka bir skaler bir özellik ekleyin ve belirtin **LastName** adı.</span><span class="sxs-lookup"><span data-stu-id="926f1-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="926f1-173">Başka bir skaler bir özellik ekleyin ve belirtin **sınıf** adı.</span><span class="sxs-lookup"><span data-stu-id="926f1-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="926f1-174">İçinde **özellikleri** penceresinde değişiklik **sınıf**'s **türü** özelliğini **ondalık**.</span><span class="sxs-lookup"><span data-stu-id="926f1-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="926f1-175">Seçin **FirstName** ve **LastName** özellikleri.</span><span class="sxs-lookup"><span data-stu-id="926f1-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="926f1-176">İçinde **özellikleri** penceresinde değişiklik **EntityKey** özellik değerini **True**.</span><span class="sxs-lookup"><span data-stu-id="926f1-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="926f1-177">Sonuç olarak, aşağıdaki öğeleri eklenme **CSDL** .edmx dosyasını bölümü.</span><span class="sxs-lookup"><span data-stu-id="926f1-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="926f1-178">Varlık türü tanımlayan sorgu eşleme</span><span class="sxs-lookup"><span data-stu-id="926f1-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="926f1-179">Bu adımda, depolama varlık türleri ve eşleşme Ayrıntıları penceresi kavramsal eşlemek için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="926f1-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="926f1-180">Sağ **GradeReport** tasarım yüzeyi ve select varlıkta **Tablo eşleme**.</span><span class="sxs-lookup"><span data-stu-id="926f1-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="926f1-181">**Eşleşme ayrıntıları** penceresi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="926f1-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="926f1-182">Seçin **GradeReport** gelen **&lt;bir tablo veya Görünüm Ekle&gt;** açılır liste (altında bulunan **tablo**s).</span><span class="sxs-lookup"><span data-stu-id="926f1-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="926f1-183">Varsayılan kavramsal arasındaki eşlemeleri ve depolama **GradeReport** varlık türü görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="926f1-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="926f1-184">![Details3 eşleme](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="926f1-184">![Mapping Details3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="926f1-185">Sonuç olarak, **EntitySetMapping** öğesi .edmx dosyası için eşleme bölümüne eklenir.</span><span class="sxs-lookup"><span data-stu-id="926f1-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   <span data-ttu-id="926f1-186">Uygulamayı derleyin.</span><span class="sxs-lookup"><span data-stu-id="926f1-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="926f1-187">Kodunuzda tanımlayan sorgu çağırın</span><span class="sxs-lookup"><span data-stu-id="926f1-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="926f1-188">Kullanarak artık tanımlayan sorgu yürütebilirsiniz **GradeReport** varlık türü.</span><span class="sxs-lookup"><span data-stu-id="926f1-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
