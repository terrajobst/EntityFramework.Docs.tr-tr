---
title: Numaralandırma desteği - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: a94a497e8c5b3213dd7eb4215de90164d437507d
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250641"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="28ce4-102">Numaralandırma desteği - EF Designer</span><span class="sxs-lookup"><span data-stu-id="28ce4-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="28ce4-103">**EF5 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 5'te kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="28ce4-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="28ce4-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="28ce4-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="28ce4-105">Bu video ve adım adım izlenecek yol, Entity Framework Designer ile numaralandırma türleri kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="28ce4-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="28ce4-106">Ayrıca, bir LINQ Sorgu numaralandırmalar kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="28ce4-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="28ce4-107">Bu izlenecek yol modeli ilk yeni bir veritabanı oluşturmak için kullanır, ancak EF Designer ile de kullanılabilir [veritabanı ilk](~/ef6/modeling/designer/workflows/database-first.md) mevcut bir veritabanına eşlemek için iş akışı.</span><span class="sxs-lookup"><span data-stu-id="28ce4-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="28ce4-108">Numaralandırma desteği, Entity Framework 5'te tanıtıldı.</span><span class="sxs-lookup"><span data-stu-id="28ce4-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="28ce4-109">Numaralandırmalar, uzamsal veri türleri ve tablo değerli işlevler gibi yeni özellikleri kullanmak için .NET Framework 4.5 hedeflemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="28ce4-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="28ce4-110">Visual Studio 2012, varsayılan olarak .NET 4.5 hedefler.</span><span class="sxs-lookup"><span data-stu-id="28ce4-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="28ce4-111">Varlık Çerçevesi'nde bir numaralandırma aşağıdaki temel alınan türler sahip olabilir: **bayt**, **Int16**, **Int32**, **Int64** , veya **SByte**.</span><span class="sxs-lookup"><span data-stu-id="28ce4-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="28ce4-112">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="28ce4-112">Watch the Video</span></span>
<span data-ttu-id="28ce4-113">Bu videoda, Entity Framework Designer ile numaralandırma türleri kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="28ce4-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="28ce4-114">Ayrıca, bir LINQ Sorgu numaralandırmalar kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="28ce4-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="28ce4-115">**Tarafından sunulan**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="28ce4-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="28ce4-116">**Video**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="28ce4-116">**Video**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="28ce4-117">Ön koşullar</span><span class="sxs-lookup"><span data-stu-id="28ce4-117">Pre-Requisites</span></span>

<span data-ttu-id="28ce4-118">Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümü, bu izlenecek yolu tamamlamak için yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="28ce4-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="28ce4-119">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="28ce4-119">Set up the Project</span></span>

1.  <span data-ttu-id="28ce4-120">Visual Studio 2012'yi açın</span><span class="sxs-lookup"><span data-stu-id="28ce4-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="28ce4-121">Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**</span><span class="sxs-lookup"><span data-stu-id="28ce4-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="28ce4-122">Sol bölmede **Visual C\#** ve ardından **konsol** şablonu</span><span class="sxs-lookup"><span data-stu-id="28ce4-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="28ce4-123">Girin **EnumEFDesigner** tıklayın ve proje adı olarak **Tamam**</span><span class="sxs-lookup"><span data-stu-id="28ce4-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="28ce4-124">Yeni EF Designer kullanarak Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="28ce4-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="28ce4-125">Çözüm Gezgini'nde proje adına sağ tıklayın, fareyle **Ekle**ve ardından **yeni öğe**</span><span class="sxs-lookup"><span data-stu-id="28ce4-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="28ce4-126">Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde</span><span class="sxs-lookup"><span data-stu-id="28ce4-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="28ce4-127">Girin **EnumTestModel.edmx** dosya adı ve ardından **Ekle**</span><span class="sxs-lookup"><span data-stu-id="28ce4-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="28ce4-128">Varlık veri modeli Sihirbazı sayfasında **boş Model** Choose Model Contents iletişim kutusunda</span><span class="sxs-lookup"><span data-stu-id="28ce4-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="28ce4-129">Tıklayın **son**</span><span class="sxs-lookup"><span data-stu-id="28ce4-129">Click **Finish**</span></span>

<span data-ttu-id="28ce4-130">Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="28ce4-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="28ce4-131">Sihirbaz, aşağıdaki eylemleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="28ce4-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="28ce4-132">Kavramsal model ve depolama modeli bunları arasındaki eşlemeyi tanımlar EnumTestModel.edmx dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="28ce4-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="28ce4-133">Oluşturulan meta veri dosyaları bütünleştirilmiş koda gömülü için Yapıt meta veri işleme özelliğini .edmx dosyasını çıkış bütünleştirilmiş kodu Ekle ayarlar.</span><span class="sxs-lookup"><span data-stu-id="28ce4-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="28ce4-134">Aşağıdaki derlemelere başvuru ekler: EntityFramework System.ComponentModel.DataAnnotations ve System.Data.Entity.</span><span class="sxs-lookup"><span data-stu-id="28ce4-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="28ce4-135">EnumTestModel.tt ve EnumTestModel.Context.tt dosyaları oluşturur ve bunları altında .edmx dosyasını ekler.</span><span class="sxs-lookup"><span data-stu-id="28ce4-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="28ce4-136">Bu T4 şablonu dosyaları .edmx modeli'ndeki varlıkları eşleyen POCO türleri ve türetilen DbContext türünü tanımlayan bir kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="28ce4-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="28ce4-137">Yeni bir varlık türü Ekle</span><span class="sxs-lookup"><span data-stu-id="28ce4-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="28ce4-138">Select tasarım yüzeyinde boş bir alana sağ tıklayın **Ekle -&gt; varlık**, yeni varlık iletişim kutusu görünür.</span><span class="sxs-lookup"><span data-stu-id="28ce4-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="28ce4-139">Belirtin **departmanı** türü için ad ve belirtin **DepartmentID** için anahtar özellik adı, türü olarak bırakın **Int32**</span><span class="sxs-lookup"><span data-stu-id="28ce4-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="28ce4-140">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="28ce4-140">Click **OK**</span></span>
4.  <span data-ttu-id="28ce4-141">Varlık sağ tıklayıp **Ekle - yeni&gt; skaler özelliği**</span><span class="sxs-lookup"><span data-stu-id="28ce4-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="28ce4-142">Yeni özelliğe Yeniden Adlandır **adı**</span><span class="sxs-lookup"><span data-stu-id="28ce4-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="28ce4-143">Yeni özelliğin türünü değiştirmek **Int32** (varsayılan olarak, yeni özellik dize türüdür) türünü değiştirmek için Özellikler penceresini açmak ve değiştirmek için Type özelliği **Int32**</span><span class="sxs-lookup"><span data-stu-id="28ce4-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="28ce4-144">Başka bir skaler bir özellik ekleyin ve yeniden adlandırın **bütçe**, türe çeviremezsiniz **ondalık**</span><span class="sxs-lookup"><span data-stu-id="28ce4-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="28ce4-145">Enum Türü Ekle</span><span class="sxs-lookup"><span data-stu-id="28ce4-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="28ce4-146">Entity Framework Tasarımcısı'nda ad özelliği sağ tıklayın, **enum için Dönüştür**</span><span class="sxs-lookup"><span data-stu-id="28ce4-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![Numaralandırmaya Dönüştür](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="28ce4-148">İçinde **sabit listesi Ekle** iletişim kutusuna **DepartmentNames** Enum tür adı için temel alınan türe çeviremezsiniz **Int32**, ve ardından türü aşağıdaki üyeleri Ekle: İngilizce, Matematik ve ekonomik</span><span class="sxs-lookup"><span data-stu-id="28ce4-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![Enum Türü Ekle](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="28ce4-150">Tuşuna **Tamam**</span><span class="sxs-lookup"><span data-stu-id="28ce4-150">Press **OK**</span></span>
4.  <span data-ttu-id="28ce4-151">Modeli kaydedin ve projeyi derleyin</span><span class="sxs-lookup"><span data-stu-id="28ce4-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="28ce4-152">Oluşturma sırasında hata Listesi'nde eşlenmemiş varlıkları ve ilişkileri hakkında uyarılar görünebilir.</span><span class="sxs-lookup"><span data-stu-id="28ce4-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="28ce4-153">Veritabanı Modeli'nden seçtikten sonra hatalar kaybolur çünkü bu uyarıları gözardı edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="28ce4-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="28ce4-154">Özellikler penceresinde bakarsanız, Name özelliğinin türü için değiştirildiğini fark edeceksiniz **DepartmentNames** ve yeni eklenen numaralandırma türüne türlerini listesine eklendi.</span><span class="sxs-lookup"><span data-stu-id="28ce4-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="28ce4-155">Model tarayıcı penceresine geçiş yapıyorsanız, türü, sabit listesi türleri düğüme de eklendiğini görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="28ce4-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![Model tarayıcısı](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="28ce4-157">Ayrıca yeni sabit listesi türleri bu pencereden sağ fare düğmesini tıklatıp seçerek ekleyebileceğiniz **Enum Türü Ekle**.</span><span class="sxs-lookup"><span data-stu-id="28ce4-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="28ce4-158">Türü oluşturulduktan sonra türleri listesinde görünür ve bir özellik ile ilişkilendirin olacaktır</span><span class="sxs-lookup"><span data-stu-id="28ce4-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="28ce4-159">Modelden veritabanı oluştur</span><span class="sxs-lookup"><span data-stu-id="28ce4-159">Generate Database from Model</span></span>

<span data-ttu-id="28ce4-160">Şimdi biz modelini temel alan bir veritabanı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="28ce4-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="28ce4-161">Varlık Tasarımcısı seçin ve yüzey üzerinde boş bir alanı sağ **Model veritabanından oluştur**</span><span class="sxs-lookup"><span data-stu-id="28ce4-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="28ce4-162">Veritabanı Oluştur Sihirbazı'nın, veri bağlantısı iletişim kutusu Seç'e tıklayın görüntülenen **yeni bağlantı** belirt düğmesine **(localdb)\\ifadesini mssqllocaldb** vesunucuadıiçin **EnumTest** tıklayın ve veritabanı için **Tamam**</span><span class="sxs-lookup"><span data-stu-id="28ce4-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="28ce4-163">Yeni bir veritabanı oluşturmak isteyip istemediğinizi soran bir iletişim kutusu açılır, tıklayın **Evet**.</span><span class="sxs-lookup"><span data-stu-id="28ce4-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="28ce4-164">Tıklayın **sonraki** ve oluşturulan DDL bir veritabanı oluşturmak için bir tanım DDL içermeyen Ayarları iletişim kutusu Not ve Özet görüntülenir Veritabanı Oluşturma Sihirbazı'nı veri tanımlama dili (DDL) oluşturur. bir sabit listesi türü eşleyen tablo</span><span class="sxs-lookup"><span data-stu-id="28ce4-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="28ce4-165">Tıklayın **son** Son'a DDL betik yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="28ce4-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="28ce4-166">Veritabanı Oluşturma Sihirbazı'nı aşağıdakileri yapar: açılır **EnumTest.edmx.sql** T-SQL Düzenleyicisi oluşturur deposu şeması ve eşleme bölümlerini EDMX bağlantı dizesi bilgilerini ekler App.config dosyasına dosya</span><span class="sxs-lookup"><span data-stu-id="28ce4-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="28ce4-167">T-SQL Düzenleyicisi'nde sağ fare düğmesine tıklayıp **yürütme** Connect sunucusu iletişim kutusu görüntülenirse, 2. adımdaki bağlantı bilgilerini girin ve tıklayın **Bağlan**</span><span class="sxs-lookup"><span data-stu-id="28ce4-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="28ce4-168">Oluşturulan şema görüntülemek için SQL Server Nesne Gezgini içinde veritabanı adını sağ tıklatın ve seçin **Yenile**</span><span class="sxs-lookup"><span data-stu-id="28ce4-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="28ce4-169">Kalıcı hale getirmek ve veri alma</span><span class="sxs-lookup"><span data-stu-id="28ce4-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="28ce4-170">Main yöntemi tanımlandığı Program.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="28ce4-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="28ce4-171">Ana işlevine aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="28ce4-171">Add the following code into the Main function.</span></span> <span data-ttu-id="28ce4-172">Kod bağlamı için yeni bir bölüm nesnesi ekler.</span><span class="sxs-lookup"><span data-stu-id="28ce4-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="28ce4-173">Ardından, verileri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="28ce4-173">It then saves the data.</span></span> <span data-ttu-id="28ce4-174">Kod ayrıca bir bölüm adı DepartmentNames.English olduğu döndüren LINQ sorgusu yürütür.</span><span class="sxs-lookup"><span data-stu-id="28ce4-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="28ce4-175">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="28ce4-175">Compile and run the application.</span></span> <span data-ttu-id="28ce4-176">Program şu çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="28ce4-176">The program produces the following output:</span></span>

```
DepartmentID: 1 Name: English
```

<span data-ttu-id="28ce4-177">Veritabanındaki verileri görüntülemek için SQL Server Nesne Gezgini içinde veritabanı adını sağ tıklatın ve seçin **Yenile**.</span><span class="sxs-lookup"><span data-stu-id="28ce4-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="28ce4-178">Tablo ve seçim sağ fare düğmesini tıklatıp **görünüm verilerini**.</span><span class="sxs-lookup"><span data-stu-id="28ce4-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="28ce4-179">Özet</span><span class="sxs-lookup"><span data-stu-id="28ce4-179">Summary</span></span>

<span data-ttu-id="28ce4-180">Bu izlenecek yolda Numaralandırma türleri Entity Framework Designer kullanarak eşleme ve numaralandırmalar kodda kullanma inceledik.</span><span class="sxs-lookup"><span data-stu-id="28ce4-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
