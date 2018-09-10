---
title: Karmaşık türler - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: 2a516bd14131fd035a4d005e0fdf140f7ff4d65f
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250835"
---
# <a name="complex-types---ef-designer"></a><span data-ttu-id="8a742-102">Karmaşık türler - EF Designer</span><span class="sxs-lookup"><span data-stu-id="8a742-102">Complex Types - EF Designer</span></span>
<span data-ttu-id="8a742-103">Bu konuda, karmaşık türün özelliklerini içeren varlıklar için sorgulama ve karmaşık türler Entity Framework Designer (EF Designer) ile eşlemek nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="8a742-103">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="8a742-104">EF Designer ile çalışırken, kullanılan ana windows aşağıdaki resimde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8a742-104">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="8a742-106">Kavramsal model oluşturduğunuzda, hata Listesi'nde eşlenmemiş varlıkları ve ilişkileri hakkında uyarılar görünebilir.</span><span class="sxs-lookup"><span data-stu-id="8a742-106">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="8a742-107">Modelden veritabanını oluşturmak seçtikten sonra hatalar kaybolur çünkü bu uyarıları gözardı edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8a742-107">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="8a742-108">Karmaşık bir tür nedir</span><span class="sxs-lookup"><span data-stu-id="8a742-108">What is a Complex Type</span></span>

<span data-ttu-id="8a742-109">Skaler olmayan özelliklerinin skaler özellikler varlıkları düzenlenmesine olanak sağlayan varlık türler karmaşık türleridir.</span><span class="sxs-lookup"><span data-stu-id="8a742-109">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="8a742-110">Varlıklar gibi karmaşık türler, skaler özellikler veya diğer karmaşık tür özellikleri oluşur.</span><span class="sxs-lookup"><span data-stu-id="8a742-110">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="8a742-111">Karmaşık türleri temsil eden nesneleri ile çalışırken, aşağıdakilere dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="8a742-111">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="8a742-112">Karmaşık türler, anahtarları yoksa ve bu nedenle bağımsız olarak var olamaz.</span><span class="sxs-lookup"><span data-stu-id="8a742-112">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="8a742-113">Karmaşık türler yalnızca varlık türleri veya diğer karmaşık türler özellikleri olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="8a742-113">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="8a742-114">Karmaşık türler ilişkilendirmeler katılamaz ve gezinti özellikleri içeremez.</span><span class="sxs-lookup"><span data-stu-id="8a742-114">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="8a742-115">Karmaşık tür özellikleri olamaz **null**.</span><span class="sxs-lookup"><span data-stu-id="8a742-115">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="8a742-116">Bir \*\* InvalidOperationException \*\* gerçekleşir, **DbContext.SaveChanges** çağrılır ve karmaşık bir null Nesne karşılaşıldı.</span><span class="sxs-lookup"><span data-stu-id="8a742-116">An \*\*InvalidOperationException \*\*occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="8a742-117">Skaler özellikleri karmaşık nesneler olabilir **null**.</span><span class="sxs-lookup"><span data-stu-id="8a742-117">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="8a742-118">Karmaşık türler, diğer karmaşık türlerden devralamaz.</span><span class="sxs-lookup"><span data-stu-id="8a742-118">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="8a742-119">Karmaşık tür olarak tanımlamalıdır bir **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="8a742-119">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="8a742-120">EF üyeleri bir karmaşık tür nesne üzerindeki değişiklikleri algılar, **DbContext.DetectChanges** çağrılır.</span><span class="sxs-lookup"><span data-stu-id="8a742-120">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="8a742-121">Varlık çerçevesi çağıran **DetectChanges** otomatik olarak, aşağıdaki üyeleri çağrılır: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span><span class="sxs-lookup"><span data-stu-id="8a742-121">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="8a742-122">Bir varlığın özelliklerini yeni karmaşık tür yeniden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="8a742-122">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="8a742-123">Bir varlık, kavramsal modelde zaten varsa, bazı özellikleri bir karmaşık tür özellikte yeniden isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8a742-123">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="8a742-124">Tasarımcı yüzeyinde birini seçin veya daha fazla özellik (Gezinti özellikleri hariç), bir varlığın sonra sağ tıklayıp **yeniden düzenleme -&gt; taşımak için yeni karmaşık tür**.</span><span class="sxs-lookup"><span data-stu-id="8a742-124">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![Yeniden Düzenleme (Refactor)](~/ef6/media/refactor.png)

<span data-ttu-id="8a742-126">Seçili özelliklere sahip yeni bir karmaşık türü eklenir **Model tarayıcı**.</span><span class="sxs-lookup"><span data-stu-id="8a742-126">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="8a742-127">Karmaşık tür bir varsayılan ad verilir.</span><span class="sxs-lookup"><span data-stu-id="8a742-127">The complex type is given a default name.</span></span>

<span data-ttu-id="8a742-128">Yeni oluşturulan türü karmaşık bir özellik seçili nesnenin özelliklerini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="8a742-128">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="8a742-129">Tüm özellik eşlemeleri korunur.</span><span class="sxs-lookup"><span data-stu-id="8a742-129">All property mappings are preserved.</span></span>

![2 yeniden düzenleyin](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="8a742-131">Yeni bir karmaşık tür oluşturma</span><span class="sxs-lookup"><span data-stu-id="8a742-131">Create a New Complex Type</span></span>

<span data-ttu-id="8a742-132">Var olan bir varlığa özelliklerini içermeyen yeni bir karmaşık türü de oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8a742-132">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="8a742-133">Sağ **karmaşık türler** klasör modeli tarayıcıda işaret **AddNew karmaşık türü...** .</span><span class="sxs-lookup"><span data-stu-id="8a742-133">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="8a742-134">Alternatif olarak, seçebileceğiniz **karmaşık türler** klasörü ve ENTER tuşuna **Ekle** klavyenizde anahtar.</span><span class="sxs-lookup"><span data-stu-id="8a742-134">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![Yeni karmaşık tür ekleyin](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="8a742-136">Yeni bir karmaşık türü bir varsayılan ada sahip klasör eklenir.</span><span class="sxs-lookup"><span data-stu-id="8a742-136">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="8a742-137">Bu gibi durumlarda, özellikleri artık türüne ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8a742-137">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="8a742-138">Karmaşık bir türü özellikleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="8a742-138">Add Properties to a Complex Type</span></span>

<span data-ttu-id="8a742-139">Karmaşık bir tür özelliklerinin skaler türler veya var olan bir karmaşık türler olabilir.</span><span class="sxs-lookup"><span data-stu-id="8a742-139">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="8a742-140">Ancak, karmaşık tür özellikleri döngüsel başvurulara sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="8a742-140">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="8a742-141">Örneğin, bir karmaşık tür **OnsiteCourseDetails** karmaşık türün bir özellik olamaz **OnsiteCourseDetails**.</span><span class="sxs-lookup"><span data-stu-id="8a742-141">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="8a742-142">Aşağıda listelenen şekilde karmaşık bir tür için bir özelliği ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8a742-142">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="8a742-143">Model tarayıcı karmaşık tür sağ tıklatın, **Ekle**, gelin **skaler özelliği** veya **karmaşık özellik**, ardından istenen özellik türünü seçin.</span><span class="sxs-lookup"><span data-stu-id="8a742-143">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="8a742-144">Alternatif olarak, bir karmaşık türü seçin ve sonra basın **Ekle** klavyenizde anahtar.</span><span class="sxs-lookup"><span data-stu-id="8a742-144">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![Karmaşık tür için özellikleri ekleyin](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="8a742-146">Yeni bir özellik varsayılan ada sahip karmaşık tür eklenir.</span><span class="sxs-lookup"><span data-stu-id="8a742-146">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="8a742-147">OR-</span><span class="sxs-lookup"><span data-stu-id="8a742-147">OR -</span></span>

-   <span data-ttu-id="8a742-148">Bir varlık özelliği sağ tıklayın **EF Designer** seçin ve yüzey **kopyalama**, karmaşık türü ardından sağ tıklayarak **Model tarayıcı** seçip  **Yapıştırma**.</span><span class="sxs-lookup"><span data-stu-id="8a742-148">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="8a742-149">Karmaşık bir tür yeniden adlandır</span><span class="sxs-lookup"><span data-stu-id="8a742-149">Rename a Complex Type</span></span>

<span data-ttu-id="8a742-150">Karmaşık bir tür yeniden adlandırdığınızda, tüm başvuruları türüne proje boyunca güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8a742-150">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="8a742-151">Yavaş bir karmaşık tür çift **Model tarayıcı**.</span><span class="sxs-lookup"><span data-stu-id="8a742-151">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="8a742-152">Ad seçilir ve buna düzenleme modu.</span><span class="sxs-lookup"><span data-stu-id="8a742-152">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="8a742-153">OR-</span><span class="sxs-lookup"><span data-stu-id="8a742-153">OR -</span></span>

-   <span data-ttu-id="8a742-154">Karmaşık tür sağ **Model tarayıcı** seçip **Yeniden Adlandır**.</span><span class="sxs-lookup"><span data-stu-id="8a742-154">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="8a742-155">OR-</span><span class="sxs-lookup"><span data-stu-id="8a742-155">OR -</span></span>

-   <span data-ttu-id="8a742-156">Model tarayıcıda bir karmaşık türü seçin ve F2 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="8a742-156">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="8a742-157">OR-</span><span class="sxs-lookup"><span data-stu-id="8a742-157">OR -</span></span>

-   <span data-ttu-id="8a742-158">Karmaşık tür sağ **Model tarayıcı** seçip **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="8a742-158">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="8a742-159">Adını düzenleyin **özellikleri** penceresi.</span><span class="sxs-lookup"><span data-stu-id="8a742-159">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="8a742-160">Var olan bir karmaşık türü bir varlığa eklemek ve tablo sütunları özellikleriyle eşleyin</span><span class="sxs-lookup"><span data-stu-id="8a742-160">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="8a742-161">Bir varlık sağ tıklatın, **yeni Ekle**seçip **karmaşık özellik**.</span><span class="sxs-lookup"><span data-stu-id="8a742-161">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="8a742-162">Varsayılan ada sahip bir karmaşık tür özelliği varlık eklenir.</span><span class="sxs-lookup"><span data-stu-id="8a742-162">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="8a742-163">Varsayılan türü (seçtiğiniz var olan bir karmaşık türlerinden) özelliğine atanır.</span><span class="sxs-lookup"><span data-stu-id="8a742-163">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="8a742-164">İstenen türde özelliğine atayın **özellikleri** penceresi.</span><span class="sxs-lookup"><span data-stu-id="8a742-164">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="8a742-165">Bir varlığa bir karmaşık tür özelliği ekledikten sonra özelliklerini tablo sütunları eşlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a742-165">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="8a742-166">Tasarım yüzeyinde veya bir varlık türüne sağ **Model tarayıcı** seçip **Tablo eşlemeleri**.</span><span class="sxs-lookup"><span data-stu-id="8a742-166">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="8a742-167">Tablo eşlemeleri görüntülenen **eşleşme ayrıntıları** penceresi.</span><span class="sxs-lookup"><span data-stu-id="8a742-167">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="8a742-168">Genişletin **eşlendiği &lt;tablo adı&gt;**  düğümü.</span><span class="sxs-lookup"><span data-stu-id="8a742-168">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="8a742-169">A **sütun eşlemelerini** düğümü görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8a742-169">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="8a742-170">Genişletin **sütun eşlemelerini** düğümü.</span><span class="sxs-lookup"><span data-stu-id="8a742-170">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="8a742-171">Tablodaki tüm sütunlar listesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8a742-171">A list of all the columns in the table appears.</span></span> <span data-ttu-id="8a742-172">Varsayılan özellikleri (varsa) hangi sütunları eşlemeniz altında listelenen **değeri/özellik** başlığı.</span><span class="sxs-lookup"><span data-stu-id="8a742-172">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="8a742-173">Eşlemek istediğiniz sütunu seçin ve ardından ilgili sağ tıklayarak **değeri/özellik** alan.</span><span class="sxs-lookup"><span data-stu-id="8a742-173">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="8a742-174">Tüm skaler özellikler aşağı açılan listesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8a742-174">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="8a742-175">Uygun özelliğini seçin.</span><span class="sxs-lookup"><span data-stu-id="8a742-175">Select the appropriate property.</span></span>

    ![Karmaşık tür eşlemesi](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="8a742-177">Her bir tablo sütunu için 6 ve 7. adımları tekrarlayın.</span><span class="sxs-lookup"><span data-stu-id="8a742-177">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="8a742-178">Sütun eşlemesini silmek için eşleme ve ardından istediğiniz sütunu seçin **değeri/özellik** alan.</span><span class="sxs-lookup"><span data-stu-id="8a742-178">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="8a742-179">Ardından, **Sil** aşağı açılan listeden.</span><span class="sxs-lookup"><span data-stu-id="8a742-179">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="8a742-180">Harita karmaşık bir tür için bir işlev içeri aktarma</span><span class="sxs-lookup"><span data-stu-id="8a742-180">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="8a742-181">İşlev içeri aktarmalar saklı yordamlara dayanır.</span><span class="sxs-lookup"><span data-stu-id="8a742-181">Function imports are based on stored procedures.</span></span> <span data-ttu-id="8a742-182">Karmaşık bir tür için bir işlev içeri aktarma eşlemek için karşılık gelen bir saklı yordam tarafından döndürülen sütun numarası karmaşık türü özelliklerini eşleşmelidir ve özellik türleri ile uyumlu depolama türleri olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a742-182">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="8a742-183">Karmaşık bir türü eşlemek istediğiniz içeri aktarılan bir işlevin çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="8a742-183">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![İşlev içeri aktarmalar](~/ef6/media/functionimports.png)

-   <span data-ttu-id="8a742-185">Yeni işlev içeri aktarma ayarlarını aşağıdaki gibi doldurun:</span><span class="sxs-lookup"><span data-stu-id="8a742-185">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="8a742-186">Bir işlev alma oluşturma saklı yordamı belirtin **saklı yordam adı** alan.</span><span class="sxs-lookup"><span data-stu-id="8a742-186">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="8a742-187">Depolama modelinde tüm saklı yordamları görüntüleyen bir açılır liste alandır.</span><span class="sxs-lookup"><span data-stu-id="8a742-187">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="8a742-188">İşlev alma adını **işlev adı alma** alan.</span><span class="sxs-lookup"><span data-stu-id="8a742-188">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="8a742-189">Seçin **karmaşık** dönüş olarak yazın ve ardından uygun türde aşağı açılan listeden seçim yaparak karmaşık belirli dönüş türü belirtin.</span><span class="sxs-lookup"><span data-stu-id="8a742-189">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![İşlev Düzenle](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="8a742-191">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="8a742-191">Click **OK**.</span></span>
    <span data-ttu-id="8a742-192">Kavramsal modelde işlev içeri aktarma girişi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8a742-192">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="8a742-193">Sütun eşleme işlev içeri aktarma için özelleştirme</span><span class="sxs-lookup"><span data-stu-id="8a742-193">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="8a742-194">Model tarayıcıdaki işlev içeri aktarma sağ tıklayıp **alma işlev eşleme Mapping**.</span><span class="sxs-lookup"><span data-stu-id="8a742-194">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="8a742-195">**Eşleme ayrıntılarını** penceresi görüntülenir ve işlev içeri aktarma için varsayılan eşlemeyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="8a742-195">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="8a742-196">Oklar sütun değerleri ve özellik değerlerini arasındaki eşlemeleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="8a742-196">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="8a742-197">Varsayılan olarak, sütun adlarını karmaşık türünün özellik adlarıyla aynı olduğu varsayılır.</span><span class="sxs-lookup"><span data-stu-id="8a742-197">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="8a742-198">Varsayılan sütun adlarını gri metin olarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8a742-198">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="8a742-199">Gerekirse, sütun adlarını işlevi alma işlemi karşılık gelen saklı yordam tarafından döndürülen sütun adlarını eşleştirmek için değiştirin.</span><span class="sxs-lookup"><span data-stu-id="8a742-199">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="8a742-200">Karmaşık bir tür Sil</span><span class="sxs-lookup"><span data-stu-id="8a742-200">Delete a Complex Type</span></span>

<span data-ttu-id="8a742-201">Karmaşık bir tür sildiğinizde, kavramsal modelden türü silinir ve eşlemeleri türünün tüm örnekleri için silinir.</span><span class="sxs-lookup"><span data-stu-id="8a742-201">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="8a742-202">Ancak, başvuru türü için güncelleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="8a742-202">However, references to the type are not updated.</span></span> <span data-ttu-id="8a742-203">Örneğin, bir karmaşık tür özelliği ComplexType1 türünde bir varlık varsa ve ComplexType1 silinir **Model tarayıcı**, ilgili varlık özelliği güncelleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="8a742-203">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="8a742-204">Silinen bir karmaşık tür başvuran bir varlık içerdiğinden model doğrulama değil.</span><span class="sxs-lookup"><span data-stu-id="8a742-204">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="8a742-205">Güncelleştirmesi veya varlık Tasarımcısı kullanarak silinen karmaşık tür başvuruları silin.</span><span class="sxs-lookup"><span data-stu-id="8a742-205">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="8a742-206">Model tarayıcı karmaşık tür sağ tıklayıp **Sil**.</span><span class="sxs-lookup"><span data-stu-id="8a742-206">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="8a742-207">OR-</span><span class="sxs-lookup"><span data-stu-id="8a742-207">OR -</span></span>

-   <span data-ttu-id="8a742-208">Model tarayıcıda bir karmaşık türü seçin ve klavyenizde Delete tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="8a742-208">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="8a742-209">Karmaşık türün özelliklerini içeren varlıklar için sorgu</span><span class="sxs-lookup"><span data-stu-id="8a742-209">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="8a742-210">Aşağıdaki kodu içeren bir karmaşık tür özelliği türü nesneleri varlık koleksiyonunu döndüren bir sorgu yürütme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8a742-210">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
