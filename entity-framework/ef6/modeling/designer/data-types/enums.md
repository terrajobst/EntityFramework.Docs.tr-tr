---
title: Enum desteği-EF Tasarımcısı-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 92a763b84a04d3ce7ec0853ef2a4852356cf7997
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182516"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="2c4a2-102">Enum desteği-EF Tasarımcısı</span><span class="sxs-lookup"><span data-stu-id="2c4a2-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="2c4a2-103">**Yalnızca EF5** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 5 ' te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="2c4a2-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="2c4a2-105">Bu video ve adım adım izlenecek yol, Entity Framework Designer numaralandırma türlerini nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="2c4a2-106">Ayrıca, bir LINQ sorgusunda numaralandırmaları nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="2c4a2-107">Bu izlenecek yol, yeni bir veritabanı oluşturmak için Model First kullanır, ancak aynı zamanda, var olan bir veritabanına eşlemek için [Database First](~/ef6/modeling/designer/workflows/database-first.md) iş AKıŞıYLA birlikte EF Designer da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="2c4a2-108">Enum desteği Entity Framework 5 ' te tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="2c4a2-109">Numaralandırmalar, uzamsal veri türleri ve tablo değerli işlevler gibi yeni özellikleri kullanmak için, .NET Framework 4,5 ' i hedeflemelidir.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="2c4a2-110">Visual Studio 2012, .NET 4,5 'i varsayılan olarak hedefler.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="2c4a2-111">Entity Framework, bir numaralandırma aşağıdaki temel türlere sahip olabilir: **byte**, **Int16**, **Int32**, **Int64** veya **SByte**.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="2c4a2-112">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="2c4a2-112">Watch the Video</span></span>
<span data-ttu-id="2c4a2-113">Bu videoda, Entity Framework Designer ile numaralandırma türlerinin nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="2c4a2-114">Ayrıca, bir LINQ sorgusunda numaralandırmaları nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="2c4a2-115">**Sunduğu**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="2c4a2-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="2c4a2-116">**Video**: [wmv](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="2c4a2-116">**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="2c4a2-117">Önkoşulların önkoşulları</span><span class="sxs-lookup"><span data-stu-id="2c4a2-117">Pre-Requisites</span></span>

<span data-ttu-id="2c4a2-118">Bu yönergeyi tamamlamak için Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümünün yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="2c4a2-119">Projeyi ayarlama</span><span class="sxs-lookup"><span data-stu-id="2c4a2-119">Set up the Project</span></span>

1.  <span data-ttu-id="2c4a2-120">Visual Studio 2012'yi açın</span><span class="sxs-lookup"><span data-stu-id="2c4a2-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="2c4a2-121">**Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="2c4a2-122">Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin</span><span class="sxs-lookup"><span data-stu-id="2c4a2-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="2c4a2-123">Projenin adı olarak **Trmefdesigner** girin ve **Tamam 'a** tıklayın</span><span class="sxs-lookup"><span data-stu-id="2c4a2-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="2c4a2-124">EF tasarımcısını kullanarak yeni model oluşturma</span><span class="sxs-lookup"><span data-stu-id="2c4a2-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="2c4a2-125">Çözüm Gezgini ' de proje adına sağ tıklayın, **Ekle**' nin üzerine gelin ve ardından **Yeni öğe** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="2c4a2-126">Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** seçin</span><span class="sxs-lookup"><span data-stu-id="2c4a2-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="2c4a2-127">Dosya adı için **Enumtestmodel. edmx** girin ve ardından **Ekle** ' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="2c4a2-128">Varlık Veri Modeli Sihirbazı sayfasında, model Içeriklerini Seç iletişim kutusunda **boş model** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="2c4a2-129">**Son** ' a tıklayın</span><span class="sxs-lookup"><span data-stu-id="2c4a2-129">Click **Finish**</span></span>

<span data-ttu-id="2c4a2-130">Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="2c4a2-131">Sihirbaz aşağıdaki eylemleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="2c4a2-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="2c4a2-132">Kavramsal modeli, depolama modelini ve aralarındaki eşlemeyi tanımlayan EnumTestModel. edmx dosyasını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="2c4a2-133">Oluşturulan meta veri dosyalarının derlemeye katıştırılması için. edmx dosyasının meta veri yapıt Işleme özelliğini çıktı derlemesine katıştırmak üzere ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="2c4a2-134">Aşağıdaki derlemelere bir başvuru ekler: EntityFramework, System. ComponentModel. Dataaçıklamalarda ve System. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="2c4a2-135">EnumTestModel.tt ve EnumTestModel.Context.tt dosyalarını oluşturur ve. edmx dosyasının altına ekler.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="2c4a2-136">Bu T4 şablon dosyaları,. edmx modelindeki varlıklarla eşlenen DbContext türetilmiş türünü ve POCO türlerini tanımlayan kodu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="2c4a2-137">Yeni varlık türü Ekle</span><span class="sxs-lookup"><span data-stu-id="2c4a2-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="2c4a2-138">Tasarım yüzeyinde boş bir alana sağ tıklayın, **Ekle-&gt; varlık**' ı seçin, yeni varlık iletişim kutusu görüntülenir</span><span class="sxs-lookup"><span data-stu-id="2c4a2-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="2c4a2-139">Tür adı için **departmanı** belirtin ve anahtar özellik adı için **DepartmentID** belirtin, türü **Int32** olarak bırakın</span><span class="sxs-lookup"><span data-stu-id="2c4a2-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="2c4a2-140">**Tamam**’a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-140">Click **OK**</span></span>
4.  <span data-ttu-id="2c4a2-141">Varlığa sağ tıklayın ve **New-&gt; skaler özelliği Ekle** ' yi seçin</span><span class="sxs-lookup"><span data-stu-id="2c4a2-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="2c4a2-142">Yeni özelliği **ad** olarak yeniden adlandır</span><span class="sxs-lookup"><span data-stu-id="2c4a2-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="2c4a2-143">Yeni özelliğin türünü **Int32** olarak değiştirin (varsayılan olarak, yeni özellik dize türündedir) ve türü değiştirmek için Özellikler penceresi açın ve Type özelliğini **Int32** olarak değiştirin</span><span class="sxs-lookup"><span data-stu-id="2c4a2-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="2c4a2-144">Başka bir skaler özellik ekleyin ve **bütçeye**yeniden adlandırın, türü **Decimal** olarak değiştirin</span><span class="sxs-lookup"><span data-stu-id="2c4a2-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="2c4a2-145">Sabit listesi türü Ekle</span><span class="sxs-lookup"><span data-stu-id="2c4a2-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="2c4a2-146">Entity Framework Designer, ad özelliğine sağ tıklayın, **sabit listesine Dönüştür** ' ü seçin</span><span class="sxs-lookup"><span data-stu-id="2c4a2-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![Sabit listesine Dönüştür](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="2c4a2-148">Numaralandırma **Ekle** iletişim kutusunda, enum türü adı için **DepartmentNames** yazın, temel alınan türü **Int32**olarak değiştirin ve ardından aşağıdaki üyeleri türüne ekleyin: İngilizce, matematik ve ekonomiku</span><span class="sxs-lookup"><span data-stu-id="2c4a2-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![Sabit listesi türü Ekle](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="2c4a2-150">**Tamam** 'a basın</span><span class="sxs-lookup"><span data-stu-id="2c4a2-150">Press **OK**</span></span>
4.  <span data-ttu-id="2c4a2-151">Modeli kaydetme ve projeyi derleme</span><span class="sxs-lookup"><span data-stu-id="2c4a2-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="2c4a2-152">Oluşturduğunuzda, eşlenmemiş varlıklar ve ilişkilendirmeler hakkında uyarılar Hata Listesi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="2c4a2-153">Bu uyarıları yoksayabilirsiniz çünkü veritabanını modelden oluşturmayı seçmemiz gerekir, ancak hatalar kaybolur.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="2c4a2-154">Özellikler penceresi görüyorsanız, ad özelliğinin türünün **DepartmentNames** olarak değiştirildiğini ve yeni eklenen sabit listesi türünün tür listesine eklendiğini fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="2c4a2-155">Model tarayıcı penceresine geçiş yaparsanız, türün de enum Types düğümüne eklendiğini görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![Model tarayıcı](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="2c4a2-157">Ayrıca, sağ fare düğmesine tıklayıp **sabit listesi türü Ekle**' yi seçerek bu pencereden yeni enum türleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="2c4a2-158">Tür oluşturulduktan sonra, tür listesinde görünür ve bir özellikle ilişkilendirebileceksiniz</span><span class="sxs-lookup"><span data-stu-id="2c4a2-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="2c4a2-159">Modelden veritabanı oluştur</span><span class="sxs-lookup"><span data-stu-id="2c4a2-159">Generate Database from Model</span></span>

<span data-ttu-id="2c4a2-160">Artık modeli temel alan bir veritabanı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="2c4a2-161">Entity Desisgner yüzeyinde boş bir alana sağ tıklayın ve **modelden veritabanı oluştur** ' u seçin.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="2c4a2-162">Veritabanı oluşturma Sihirbazı ' nın veri bağlantısını seçin Iletişim kutusu görüntülenir **Yeni bağlantı** düğmesine tıklayın (LocalDB) sunucu adı ve veritabanı Için **enumtest** için **Mssqllocaldb\\** ve ardından **Tamam** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="2c4a2-163">Yeni bir veritabanı oluşturmak isteyip istemediğinizi soran bir iletişim kutusu açılır ve **Evet**' e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="2c4a2-164">**İleri** ' ye tıklayın ve veritabanı oluşturma Sihirbazı bir veritabanı oluşturmak için veri tanımlama DILI (ddl) oluşturur oluşturulan DDL, Özet ve ayarlar iletişim kutusu ' nda, DDL 'nin numaralandırma türüyle eşleşen bir tablo tanımı içermediğini belirten bir açıklama içermez.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="2c4a2-165">Son **' a** tıklayarak DDL betiğini yürütmez.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="2c4a2-166">Veritabanı oluşturma Sihirbazı şunları yapar: **Enumtest. edmx** dosyasını açar. T-SQL DÜZENLEYICISINDE, edmx dosyasının mağaza şeması ve eşleme bölümleri, bağlantı dizesi bilgilerini App. config dosyasına ekler</span><span class="sxs-lookup"><span data-stu-id="2c4a2-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="2c4a2-167">T-SQL Düzenleyicisi ' nde sağ fare düğmesine tıklayın ve sunucuya Bağlan iletişim kutusunu **Çalıştır** ' ı seçin. Adım 2 ' den bağlantı bilgilerini girin ve **Bağlan** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="2c4a2-168">Oluşturulan şemayı görüntülemek için SQL Server Nesne Gezgini veritabanı adına sağ tıklayın ve **Yenile** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="2c4a2-169">Kalıcı ve veri alma</span><span class="sxs-lookup"><span data-stu-id="2c4a2-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="2c4a2-170">Main yönteminin tanımlandığı Program.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="2c4a2-171">Aşağıdaki kodu Main işlevine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-171">Add the following code into the Main function.</span></span> <span data-ttu-id="2c4a2-172">Kod, içeriğe yeni bir departman nesnesi ekler.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="2c4a2-173">Daha sonra verileri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-173">It then saves the data.</span></span> <span data-ttu-id="2c4a2-174">Kod ayrıca, adın DepartmentNames. Ingilizce olduğu bir departmanı döndüren bir LINQ sorgusu yürütür.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="2c4a2-175">Uygulamayı derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-175">Compile and run the application.</span></span> <span data-ttu-id="2c4a2-176">Program aşağıdaki çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="2c4a2-176">The program produces the following output:</span></span>

```console
DepartmentID: 1 Name: English
```

<span data-ttu-id="2c4a2-177">Veritabanındaki verileri görüntülemek için SQL Server Nesne Gezgini veritabanı adına sağ tıklayın ve **Yenile**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="2c4a2-178">Ardından, tablodaki sağ fare düğmesine tıklayın ve **verileri görüntüle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="2c4a2-179">Özet</span><span class="sxs-lookup"><span data-stu-id="2c4a2-179">Summary</span></span>

<span data-ttu-id="2c4a2-180">Bu kılavuzda, Entity Framework Designer kullanarak numaralandırma türlerini nasıl eşleyeceğinizi ve kod içinde Numaralandırmaların nasıl kullanılacağını inceledik.</span><span class="sxs-lookup"><span data-stu-id="2c4a2-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
