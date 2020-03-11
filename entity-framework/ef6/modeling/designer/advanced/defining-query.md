---
title: Sorgu-EF Designer-EF6 tanımlama
author: divega
ms.date: 10/23/2016
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: b1589dc12ccb50754c2e950932a2d82bc4869f6b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418800"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="25a81-102">Query-EF tasarımcısını tanımlama</span><span class="sxs-lookup"><span data-stu-id="25a81-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="25a81-103">Bu izlenecek yol, EF Designer kullanarak bir modele bir tanımlama sorgusunun ve ilgili varlık türünün nasıl ekleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="25a81-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="25a81-104">Bir veritabanı görünümü tarafından sağlananlara benzer işlevsellik sağlamak için genellikle tanımlama sorgusu kullanılır, ancak görünüm, veritabanında değil modelde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="25a81-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="25a81-105">Tanımlama sorgusu, bir. edmx dosyasının **Definingquery** öğesinde BELIRTILEN bir SQL ifadesini çalıştırmanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="25a81-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="25a81-106">Daha fazla bilgi için, [SSDL belirtiminde](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) **definingquery** bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="25a81-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="25a81-107">Sorgu tanımlama kullanılırken, modelinizde bir varlık türü de tanımlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="25a81-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="25a81-108">Varlık türü, tanımlama sorgusunun açığa çıkarılan verileri yüzey için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="25a81-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="25a81-109">Bu varlık türünün karşılaştığı verilerin salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="25a81-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="25a81-110">Parametreli sorgular sorgu tanımlayarak yürütülemez.</span><span class="sxs-lookup"><span data-stu-id="25a81-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="25a81-111">Ancak veriler, saklı yordamlara veri sunan varlık türünün INSERT, Update ve delete işlevleri eşlenerek güncellenebilir.</span><span class="sxs-lookup"><span data-stu-id="25a81-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="25a81-112">Daha fazla bilgi için bkz. [Saklı yordamlarla ekleme, güncelleştirme ve silme](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="25a81-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="25a81-113">Bu konu başlığı altında, aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="25a81-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="25a81-114">Tanımlama sorgusu ekleme</span><span class="sxs-lookup"><span data-stu-id="25a81-114">Add a Defining Query</span></span>
-   <span data-ttu-id="25a81-115">Modele bir varlık türü ekleyin</span><span class="sxs-lookup"><span data-stu-id="25a81-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="25a81-116">Tanımlama sorgusunu varlık türü ile eşleyin</span><span class="sxs-lookup"><span data-stu-id="25a81-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25a81-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="25a81-117">Prerequisites</span></span>

<span data-ttu-id="25a81-118">Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:</span><span class="sxs-lookup"><span data-stu-id="25a81-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="25a81-119">Visual Studio 'nun son sürümü.</span><span class="sxs-lookup"><span data-stu-id="25a81-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="25a81-120">[Okul örnek veritabanı](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="25a81-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="25a81-121">Projeyi ayarlama</span><span class="sxs-lookup"><span data-stu-id="25a81-121">Set up the Project</span></span>

<span data-ttu-id="25a81-122">Bu izlenecek yol, Visual Studio 2012 veya daha yeni bir sürümünü kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="25a81-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="25a81-123">Visual Studio'yu açın.</span><span class="sxs-lookup"><span data-stu-id="25a81-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="25a81-124">**Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="25a81-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="25a81-125">Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol uygulaması** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="25a81-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="25a81-126">Projenin adı olarak **Definingquerysample** girin ve **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="25a81-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="25a81-127">Okul veritabanına dayalı bir model oluşturma</span><span class="sxs-lookup"><span data-stu-id="25a81-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="25a81-128">Çözüm Gezgini ' de proje adına sağ tıklayın, **Ekle**' nin üzerine gelin ve ardından **Yeni öğe**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="25a81-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="25a81-129">Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="25a81-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="25a81-130">Dosya adı için **Definingquerymodel. edmx** girin ve ardından **Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="25a81-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="25a81-131">Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="25a81-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="25a81-132">Yeni bağlantı ' ya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="25a81-132">Click New Connection.</span></span> <span data-ttu-id="25a81-133">Bağlantı özellikleri iletişim kutusunda sunucu adını (örneğin, **(LocalDB)\\mssqllocaldb**) girin, kimlik doğrulama yöntemini seçin, veritabanı adı için **okul** yazın ve ardından **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="25a81-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="25a81-134">Veri bağlantınızı seçin iletişim kutusu, veritabanı bağlantı ayarınız ile güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="25a81-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="25a81-135">Veritabanı nesnelerinizi seçin iletişim kutusunda **tablolar** düğümünü kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="25a81-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="25a81-136">Bu, tüm tabloları **okul** modeline ekler.</span><span class="sxs-lookup"><span data-stu-id="25a81-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="25a81-137"> **Son**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="25a81-137">Click **Finish**.</span></span>
-   <span data-ttu-id="25a81-138">Çözüm Gezgini, **Definingquerymodel. edmx** dosyasına sağ tıklayın ve **birlikte aç...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="25a81-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="25a81-139">**XML (metin) Düzenleyicisi**seçin.</span><span class="sxs-lookup"><span data-stu-id="25a81-139">Select **XML (Text) Editor**.</span></span>

    ![XML Düzenleyicisi](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="25a81-141">Aşağıdaki iletiyle istenirse **Evet** ' e tıklayın:</span><span class="sxs-lookup"><span data-stu-id="25a81-141">Click **Yes** if prompted with the following message:</span></span>

    ![Uyarı 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="25a81-143">Tanımlama sorgusu ekleme</span><span class="sxs-lookup"><span data-stu-id="25a81-143">Add a Defining Query</span></span>

<span data-ttu-id="25a81-144">Bu adımda,. edmx dosyasının SSDL bölümüne bir tanımlama sorgusu ve bir varlık türü eklemek için XML düzenleyicisini kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="25a81-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="25a81-145">. Edmx dosyasının SSDL bölümüne bir **EntitySet** öğesi ekleyin (satır 5 ile 13 arasında).</span><span class="sxs-lookup"><span data-stu-id="25a81-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="25a81-146">Aşağıdakileri belirtin:</span><span class="sxs-lookup"><span data-stu-id="25a81-146">Specify the following:</span></span>
    -   <span data-ttu-id="25a81-147"> **EntitySet** öğesinin yalnızca **ad** ve **EntityType** öznitelikleri belirtilir.</span><span class="sxs-lookup"><span data-stu-id="25a81-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="25a81-148">Varlık türünün tam adı **EntityType** özniteliğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="25a81-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="25a81-149">Yürütülecek SQL açıklaması **Definingquery** öğesinde belirtilmiştir.</span><span class="sxs-lookup"><span data-stu-id="25a81-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

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

-   <span data-ttu-id="25a81-150">**EntityType** öğesini. edmx öğesinin SSDL bölümüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="25a81-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="25a81-151">dosyası aşağıda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="25a81-151">file as shown below.</span></span> <span data-ttu-id="25a81-152">Şunlara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="25a81-152">Note the following:</span></span>
    -   <span data-ttu-id="25a81-153">**Ad** özniteliğinin değeri, yukarıdaki **EntitySet** öğesinde **EntityType** özniteliğinin değerine karşılık gelir, ancak varlık türünün tam adı **EntityType** özniteliğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="25a81-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="25a81-154">Özellik adları, **Definingquery** öğesinde (yukarıda) SQL ifadesinin döndürdüğü sütun adlarına karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="25a81-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="25a81-155">Bu örnekte, varlık anahtarı benzersiz bir anahtar değeri sağlamak için üç özelliklerden oluşur.</span><span class="sxs-lookup"><span data-stu-id="25a81-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

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
> <span data-ttu-id="25a81-156">Daha sonra **güncelleştirme modeli Sihirbazı** iletişim kutusunu çalıştırırsanız, sorgu tanımlama da dahil olmak üzere depolama modelinde yapılan tüm değişikliklerin üzerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="25a81-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="25a81-157">Modele bir varlık türü ekleyin</span><span class="sxs-lookup"><span data-stu-id="25a81-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="25a81-158">Bu adımda, EF tasarımcısını kullanarak varlık türünü kavramsal modele ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="25a81-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span> <span data-ttu-id="25a81-159"> Aşağıdakilere göz önünde edin:</span><span class="sxs-lookup"><span data-stu-id="25a81-159"> Note the following:</span></span>

-   <span data-ttu-id="25a81-160">Varlığın **adı** , yukarıdaki **EntitySet** öğesinde **EntityType** özniteliğinin değerine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="25a81-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="25a81-161">Özellik adları, yukarıdaki **Definingquery** öğesinde SQL ifadesinin döndürdüğü sütun adlarına karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="25a81-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="25a81-162">Bu örnekte, varlık anahtarı benzersiz bir anahtar değeri sağlamak için üç özelliklerden oluşur.</span><span class="sxs-lookup"><span data-stu-id="25a81-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="25a81-163">Modeli EF tasarımcısında açın.</span><span class="sxs-lookup"><span data-stu-id="25a81-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="25a81-164">DefiningQueryModel. edmx öğesine çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="25a81-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="25a81-165">Aşağıdaki iletiyi **Evet** olarak söyleyin:</span><span class="sxs-lookup"><span data-stu-id="25a81-165">Say **Yes** to the following message:</span></span>

    ![Uyarı 2](~/ef6/media/warning2.png)

 

<span data-ttu-id="25a81-167">Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="25a81-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="25a81-168">Tasarımcı yüzeyine sağ tıklayın ve yeni-&gt;varlık **Ekle** ' yi seçin **...**</span><span class="sxs-lookup"><span data-stu-id="25a81-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="25a81-169">**Anahtar özelliği**için varlık adı ve **CourseID** Için **gradereport** belirtin.</span><span class="sxs-lookup"><span data-stu-id="25a81-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="25a81-170">**Gradereport** varlığına sağ tıklayın ve yeni-&gt; **skalar Özellik** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="25a81-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="25a81-171">Özelliğin varsayılan adını **FirstName**olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="25a81-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="25a81-172">Başka bir skaler özellik ekleyin ve ad için **LastName** öğesini belirtin.</span><span class="sxs-lookup"><span data-stu-id="25a81-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="25a81-173">Başka bir skaler özellik ekleyin ve ad için bir **sınıf** belirtin.</span><span class="sxs-lookup"><span data-stu-id="25a81-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="25a81-174">**Özellikler** penceresinde, **sınıf** **türü** özelliğini **Decimal**olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="25a81-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="25a81-175">**FirstName** ve **LastName** özelliklerini seçin.</span><span class="sxs-lookup"><span data-stu-id="25a81-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="25a81-176">**Özellikler** penceresinde **EntityKey** özellik değerini **true**olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="25a81-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="25a81-177">Sonuç olarak, aşağıdaki öğeler. edmx dosyasının **csdl** bölümüne eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="25a81-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="25a81-178">Tanımlama sorgusunu varlık türü ile eşleyin</span><span class="sxs-lookup"><span data-stu-id="25a81-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="25a81-179">Bu adımda, kavramsal ve depolama varlık türlerini eşlemek için eşleme ayrıntıları penceresini kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="25a81-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="25a81-180">Tasarım yüzeyinde **Gradereport** varlığına sağ tıklayın ve **Tablo eşleme**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="25a81-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="25a81-181">**Eşleme ayrıntıları** penceresi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="25a81-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="25a81-182">**&lt;tablo Ekle veya görüntüle&gt;** açılan listesini ( **tablo**s altında bulunur **) seçin.**</span><span class="sxs-lookup"><span data-stu-id="25a81-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="25a81-183">Kavramsal ve depolama **Gradereport** varlık türü arasındaki varsayılan eşlemeler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="25a81-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="25a81-184">![eşleme Details3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="25a81-184">![Mapping Details3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="25a81-185">Sonuç olarak, **EntitySetMapping** öğesi. edmx dosyasının Mapping bölümüne eklenir.</span><span class="sxs-lookup"><span data-stu-id="25a81-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

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

-   <span data-ttu-id="25a81-186">Uygulamayı derleyin.</span><span class="sxs-lookup"><span data-stu-id="25a81-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="25a81-187">Kodunuzda tanımlama sorgusunu çağırma</span><span class="sxs-lookup"><span data-stu-id="25a81-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="25a81-188">Artık, **Gradereport** varlık türünü kullanarak tanımlama sorgusunu çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="25a81-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
