---
title: Karmaşık türler-EF Tasarımcısı-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418653"
---
# <a name="complex-types---ef-designer"></a><span data-ttu-id="93979-102">Karmaşık türler-EF Tasarımcısı</span><span class="sxs-lookup"><span data-stu-id="93979-102">Complex Types - EF Designer</span></span>
<span data-ttu-id="93979-103">Bu konu, karmaşık türlerin Entity Framework Designer (EF Designer) ile nasıl eşleneceğini ve karmaşık türün özelliklerini içeren varlıkların nasıl sorgulanalınacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="93979-103">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="93979-104">Aşağıdaki görüntüde, EF Designer ile çalışırken kullanılan ana pencereler gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="93979-104">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Tasarımcısı](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="93979-106">Kavramsal model oluşturduğunuzda, eşlenmemiş varlıklar ve ilişkilendirmeler hakkında uyarılar Hata Listesi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="93979-106">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="93979-107">Bu uyarıları yoksayabilirsiniz çünkü veritabanını modelden oluşturmayı seçtiğinizde hatalar kaybolur.</span><span class="sxs-lookup"><span data-stu-id="93979-107">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="93979-108">Karmaşık tür nedir?</span><span class="sxs-lookup"><span data-stu-id="93979-108">What is a Complex Type</span></span>

<span data-ttu-id="93979-109">Karmaşık türler, skalar özelliklerin varlıklar içinde düzenlenmesine olanak sağlayan varlık türlerinin skalar olmayan özellikleridir.</span><span class="sxs-lookup"><span data-stu-id="93979-109">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="93979-110">Varlıklar gibi karmaşık türler de skalar özelliklerden veya diğer karmaşık tür özelliklerinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="93979-110">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="93979-111">Karmaşık türleri temsil eden nesnelerle çalışırken, aşağıdakilere dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="93979-111">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="93979-112">Karmaşık türlerde anahtarlar yoktur ve bu nedenle bağımsız olarak bulunamaz.</span><span class="sxs-lookup"><span data-stu-id="93979-112">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="93979-113">Karmaşık türler yalnızca varlık türlerinin veya diğer karmaşık türlerin özellikleri olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="93979-113">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="93979-114">Karmaşık türler ilişkilendirmelere katılamaz ve gezinti özellikleri içeremez.</span><span class="sxs-lookup"><span data-stu-id="93979-114">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="93979-115">Karmaşık tür özellikleri **null**olamaz.</span><span class="sxs-lookup"><span data-stu-id="93979-115">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="93979-116">**DbContext. SaveChanges** çağrıldığında ve null karmaşık bir nesneyle karşılaşıldığında **InvalidOperationException **oluşur.</span><span class="sxs-lookup"><span data-stu-id="93979-116">An **InvalidOperationException **occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="93979-117">Karmaşık nesnelerin skaler özellikleri **null**olabilir.</span><span class="sxs-lookup"><span data-stu-id="93979-117">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="93979-118">Karmaşık türler diğer karmaşık türlerden devralınabilir.</span><span class="sxs-lookup"><span data-stu-id="93979-118">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="93979-119">Karmaşık türü bir **sınıf**olarak tanımlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="93979-119">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="93979-120">EF, **DbContext. DetectChanges** çağrıldığında karmaşık tür nesnesindeki üyelerin değişikliklerini algılar.</span><span class="sxs-lookup"><span data-stu-id="93979-120">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="93979-121">Entity Framework, aşağıdaki Üyeler çağrıldığında **DetectChanges** öğesini otomatik olarak çağırır: **Dbset. Find**, **dbset. Local**, **Dbset. Remove**, **dbset. Add**, **dbset. Attach**, **DbContext. SaveChanges**, **DbContext. getvalidationerrors**, **DbContext. Entry**, **dbchangetracker. Entries**.</span><span class="sxs-lookup"><span data-stu-id="93979-121">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="93979-122">Varlığın özelliklerini yeni karmaşık türe yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="93979-122">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="93979-123">Kavramsal modelinizde zaten bir varlık varsa, bazı özelliklerden karmaşık tür özelliklerine yeniden düzenleme yapmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93979-123">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="93979-124">Tasarımcı yüzeyinde bir varlığın bir veya daha fazla özelliğini (gezinme özellikleri hariç) seçin, sonra sağ tıklayıp yeniden&gt; Düzenle ' yi seçerek **Yeni karmaşık türe taşıyın**.</span><span class="sxs-lookup"><span data-stu-id="93979-124">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![Yeniden Düzenleme (Refactor)](~/ef6/media/refactor.png)

<span data-ttu-id="93979-126">**Model tarayıcıya**seçili özelliklere sahip yeni bir karmaşık tür eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="93979-126">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="93979-127">Karmaşık türe varsayılan bir ad verilir.</span><span class="sxs-lookup"><span data-stu-id="93979-127">The complex type is given a default name.</span></span>

<span data-ttu-id="93979-128">Yeni oluşturulan türün karmaşık bir özelliği seçilen özelliklerin yerini alır.</span><span class="sxs-lookup"><span data-stu-id="93979-128">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="93979-129">Tüm özellik eşlemeleri korunur.</span><span class="sxs-lookup"><span data-stu-id="93979-129">All property mappings are preserved.</span></span>

![Yeniden Düzenle 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="93979-131">Yeni bir karmaşık tür oluşturun</span><span class="sxs-lookup"><span data-stu-id="93979-131">Create a New Complex Type</span></span>

<span data-ttu-id="93979-132">Ayrıca, var olan bir varlığın özelliklerini içermeyen yeni bir karmaşık tür oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93979-132">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="93979-133">Model tarayıcısında **karmaşık türler** klasörüne sağ tıklayın, **AddNew karmaşık türü..** . seçeneğine gelin.</span><span class="sxs-lookup"><span data-stu-id="93979-133">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="93979-134">Alternatif olarak, **karmaşık türler** klasörünü seçip klavyenizdeki **Insert** tuşuna basabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93979-134">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![Yeni karmaşık tür Ekle](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="93979-136">Klasöre varsayılan adla yeni bir karmaşık tür eklenir.</span><span class="sxs-lookup"><span data-stu-id="93979-136">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="93979-137">Artık türüne özellikler ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93979-137">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="93979-138">Karmaşık bir türe özellikler ekleme</span><span class="sxs-lookup"><span data-stu-id="93979-138">Add Properties to a Complex Type</span></span>

<span data-ttu-id="93979-139">Karmaşık bir türün özellikleri skaler türler veya var olan karmaşık türler olabilir.</span><span class="sxs-lookup"><span data-stu-id="93979-139">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="93979-140">Ancak, karmaşık tür özelliklerinin döngüsel başvuruları olamaz.</span><span class="sxs-lookup"><span data-stu-id="93979-140">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="93979-141">Örneğin, bir karmaşık tür **onsitecoursedetails** karmaşık türde **onsitecoursedetails**özelliğine sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="93979-141">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="93979-142">Bir özelliği, aşağıda listelenen yollarla karmaşık bir türe ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93979-142">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="93979-143">Model tarayıcısında bir karmaşık türe sağ tıklayın, **Ekle**' nin üzerine gelin, sonra da **skaler Özellik** veya **karmaşık özellik**' in üzerine gelin ve istediğiniz özellik türünü seçin.</span><span class="sxs-lookup"><span data-stu-id="93979-143">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="93979-144">Alternatif olarak, bir karmaşık tür seçebilir ve klavyenizdeki **ınsert** tuşuna basabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93979-144">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![Karmaşık türe özellikler ekleme](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="93979-146">Varsayılan bir ada sahip karmaşık türe yeni bir özellik eklenir.</span><span class="sxs-lookup"><span data-stu-id="93979-146">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="93979-147">Veya</span><span class="sxs-lookup"><span data-stu-id="93979-147">OR -</span></span>

-   <span data-ttu-id="93979-148">**EF Designer** yüzeyinde bir varlık özelliğine sağ tıklayın ve **Kopyala**' yı seçin, ardından **model tarayıcısında** karmaşık türe sağ tıklayıp **Yapıştır**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="93979-148">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="93979-149">Karmaşık bir türü yeniden adlandırma</span><span class="sxs-lookup"><span data-stu-id="93979-149">Rename a Complex Type</span></span>

<span data-ttu-id="93979-150">Karmaşık bir türü yeniden adlandırdığınızda, türe yapılan tüm başvurular proje genelinde güncellenir.</span><span class="sxs-lookup"><span data-stu-id="93979-150">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="93979-151">**Model tarayıcısında**bir karmaşık türe yavaş bir çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="93979-151">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="93979-152">Ad, düzenleme modunda seçilir ve görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93979-152">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="93979-153">Veya</span><span class="sxs-lookup"><span data-stu-id="93979-153">OR -</span></span>

-   <span data-ttu-id="93979-154">**Model tarayıcısında** bir karmaşık türe sağ tıklayıp **Yeniden Adlandır**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="93979-154">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="93979-155">Veya</span><span class="sxs-lookup"><span data-stu-id="93979-155">OR -</span></span>

-   <span data-ttu-id="93979-156">Model tarayıcısında bir karmaşık tür seçin ve F2 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="93979-156">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="93979-157">Veya</span><span class="sxs-lookup"><span data-stu-id="93979-157">OR -</span></span>

-   <span data-ttu-id="93979-158">**Model tarayıcısında** karmaşık bir türe sağ tıklayın ve **Özellikler**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="93979-158">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="93979-159">  **Özellikler** penceresinde adı düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="93979-159">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="93979-160">Bir varlığa var olan bir karmaşık türü ekleyin ve özelliklerini tablo sütunlarına eşleyin</span><span class="sxs-lookup"><span data-stu-id="93979-160">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="93979-161">Bir varlığa sağ tıklayın, **Yeni Ekle**' nin üzerine gelin ve **karmaşık özellik**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="93979-161">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="93979-162">Varlığa varsayılan adda bir karmaşık tür özelliği eklenir.</span><span class="sxs-lookup"><span data-stu-id="93979-162">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="93979-163">Varsayılan bir tür (varolan karmaşık türlerden seçilen) özelliğine atanır.</span><span class="sxs-lookup"><span data-stu-id="93979-163">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="93979-164">İstenen türü **özellikler** penceresindeki özelliğe atayın.</span><span class="sxs-lookup"><span data-stu-id="93979-164">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="93979-165">Bir varlığa karmaşık bir tür özelliği eklendikten sonra, özelliklerini tablo sütunlarına eşlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="93979-165">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="93979-166">Tasarım yüzeyinde veya **model tarayıcısı** bir varlık türüne sağ tıklayın ve **tablo eşlemeleri**' ni seçin.</span><span class="sxs-lookup"><span data-stu-id="93979-166">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="93979-167">Tablo eşlemeleri **eşleme ayrıntıları** penceresinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93979-167">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="93979-168"> **Haritaları &lt;tablo adı&gt;**  düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="93979-168">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="93979-169">Bir **sütun eşlemeleri** düğümü görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93979-169">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="93979-170"> **Sütun eşlemelerini** düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="93979-170">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="93979-171">Tablodaki tüm sütunların bir listesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93979-171">A list of all the columns in the table appears.</span></span> <span data-ttu-id="93979-172">Sütun haritasının, **değer/özellik** başlığı altında listelendiği varsayılan Özellikler (varsa).</span><span class="sxs-lookup"><span data-stu-id="93979-172">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="93979-173">Eşlemek istediğiniz sütunu seçin ve ardından karşılık gelen **değer/özellik** alanına sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="93979-173">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="93979-174">Tüm skaler özelliklerin açılan listesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93979-174">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="93979-175">Uygun özelliği seçin.</span><span class="sxs-lookup"><span data-stu-id="93979-175">Select the appropriate property.</span></span>

    ![Eşleme karmaşık türü](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="93979-177">Her tablo sütunu için 6 ve 7. adımları yineleyin.</span><span class="sxs-lookup"><span data-stu-id="93979-177">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="93979-178">Bir sütun eşlemesini silmek için, eşlemek istediğiniz sütunu seçin ve ardından **değer/özellik** alanına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="93979-178">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="93979-179">Sonra, açılan listeden **Sil** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="93979-179">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="93979-180">Bir Işlev Içeri aktarmayı karmaşık bir türe eşleyin</span><span class="sxs-lookup"><span data-stu-id="93979-180">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="93979-181">İşlev içeri aktarmaları, saklı yordamlara dayalıdır.</span><span class="sxs-lookup"><span data-stu-id="93979-181">Function imports are based on stored procedures.</span></span> <span data-ttu-id="93979-182">Bir işlev içeri aktarmayı karmaşık bir türe eşlemek için, karşılık gelen saklı yordam tarafından döndürülen sütunlar sayı olarak karmaşık türün özellikleriyle eşleşmelidir ve özellik türleriyle uyumlu depolama türlerine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="93979-182">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="93979-183">Karmaşık bir türe eşlemek istediğiniz içeri aktarılan işleve çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="93979-183">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![İşlev Içeri aktarmaları](~/ef6/media/functionimports.png)

-   <span data-ttu-id="93979-185">Yeni işlev içeri aktarma ayarlarını aşağıdaki gibi girin:</span><span class="sxs-lookup"><span data-stu-id="93979-185">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="93979-186"> **Saklı yordam adı** alanında bir işlev içeri aktarması oluşturduğunuz saklı yordamı belirtin.</span><span class="sxs-lookup"><span data-stu-id="93979-186">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="93979-187">Bu alan, depolama modelindeki tüm saklı yordamları görüntüleyen bir açılan liste listesidir.</span><span class="sxs-lookup"><span data-stu-id="93979-187">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="93979-188">İşlev **içeri aktarma adı** alanında işlev içeri aktarma adını belirtin.</span><span class="sxs-lookup"><span data-stu-id="93979-188">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="93979-189">Dönüş türü olarak **karmaşık** seçin ve ardından açılan listeden uygun türü seçerek belirli karmaşık dönüş türünü belirtin.</span><span class="sxs-lookup"><span data-stu-id="93979-189">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![Işlev Içeri aktarmayı Düzenle](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="93979-191"> **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="93979-191">Click **OK**.</span></span>
    <span data-ttu-id="93979-192">İşlev içeri aktarma girişi kavramsal modelde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="93979-192">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="93979-193">Işlev Içeri aktarması için sütun eşlemeyi özelleştirme</span><span class="sxs-lookup"><span data-stu-id="93979-193">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="93979-194">Model tarayıcısında içeri aktarma işlevine sağ tıklayın ve **Işlev Içeri aktarma eşlemesi**' ni seçin.</span><span class="sxs-lookup"><span data-stu-id="93979-194">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="93979-195"> **Eşleme ayrıntıları** penceresi görünür ve işlev içeri aktarma için varsayılan eşlemeyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="93979-195">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="93979-196">Oklar, sütun değerleri ve özellik değerleri arasındaki eşlemeleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="93979-196">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="93979-197">Varsayılan olarak, sütun adlarının karmaşık türün Özellik adlarıyla aynı olduğu varsayılır.</span><span class="sxs-lookup"><span data-stu-id="93979-197">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="93979-198">Varsayılan sütun adları gri metinde görünür.</span><span class="sxs-lookup"><span data-stu-id="93979-198">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="93979-199">Gerekirse, sütun adlarını, işlev içeri aktarmaya karşılık gelen saklı yordamın döndürdüğü sütun adlarıyla eşleşecek şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="93979-199">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="93979-200">Karmaşık bir türü silme</span><span class="sxs-lookup"><span data-stu-id="93979-200">Delete a Complex Type</span></span>

<span data-ttu-id="93979-201">Karmaşık bir türü sildiğinizde tür kavramsal modelden silinir ve türün tüm örnekleri için eşlemeler silinir.</span><span class="sxs-lookup"><span data-stu-id="93979-201">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="93979-202">Ancak, türe yapılan başvurular güncellenmez.</span><span class="sxs-lookup"><span data-stu-id="93979-202">However, references to the type are not updated.</span></span> <span data-ttu-id="93979-203">Örneğin, bir varlığın ComplexType1 türünde karmaşık bir tür özelliği varsa ve ComplexType1 **model tarayıcısında**silinirse, karşılık gelen varlık özelliği güncellenmez.</span><span class="sxs-lookup"><span data-stu-id="93979-203">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="93979-204">Silinen bir karmaşık türe başvuran bir varlık içerdiğinden model doğrulanmayacak.</span><span class="sxs-lookup"><span data-stu-id="93979-204">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="93979-205">Entity Desisgner kullanarak silinen karmaşık türlere başvuruları güncelleştirebilir veya silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93979-205">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="93979-206">Model tarayıcısında karmaşık bir türe sağ tıklayın ve **Sil**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="93979-206">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="93979-207">Veya</span><span class="sxs-lookup"><span data-stu-id="93979-207">OR -</span></span>

-   <span data-ttu-id="93979-208">Model tarayıcısında bir karmaşık tür seçin ve klavyenizdeki DELETE tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="93979-208">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="93979-209">Karmaşık türün özelliklerini Içeren varlıklar için sorgu</span><span class="sxs-lookup"><span data-stu-id="93979-209">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="93979-210">Aşağıdaki kod, karmaşık tür özelliği içeren varlık türü nesnelerinin bir koleksiyonunu döndüren bir sorgunun nasıl yürütüleceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="93979-210">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

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
