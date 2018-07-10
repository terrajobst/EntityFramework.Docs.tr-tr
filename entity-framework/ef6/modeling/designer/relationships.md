---
title: İlişkiler - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
caps.latest.revision: 3
ms.openlocfilehash: f924945b19dd6d73847ff3ec52c0b5a286c591bb
ms.sourcegitcommit: 9ae4473425c5e76337c9d032b0e5dbfedf1fcf57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914467"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="d9c72-102">İlişkiler - EF Designer</span><span class="sxs-lookup"><span data-stu-id="d9c72-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="d9c72-103">Bu sayfa, ilişkileri EF Designer kullanarak modelinizdeki ayarlama hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="d9c72-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="d9c72-104">EF ve erişmek ve ilişkileri kullanarak verileri işlemek nasıl ilişkiler hakkında genel bilgi için bkz. [ilişkileri ve gezinti özellikleri](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="d9c72-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="d9c72-105">Bir modeldeki varlık türleri arasındaki ilişkileri ilişkileri tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="d9c72-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="d9c72-106">Bu konuda, Entity Framework Designer (EF Designer) ilişkilerini eşleme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="d9c72-107">EF Designer ile çalışırken, kullanılan ana windows aşağıdaki resimde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="d9c72-109">Kavramsal model oluşturduğunuzda, hata Listesi'nde eşlenmemiş varlıkları ve ilişkileri hakkında uyarılar görünebilir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="d9c72-110">Modelden veritabanını oluşturmak seçtikten sonra hatalar kaybolur çünkü bu uyarıları gözardı edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9c72-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="d9c72-111">İlişkilendirmeleri genel bakış</span><span class="sxs-lookup"><span data-stu-id="d9c72-111">Associations Overview</span></span>

<span data-ttu-id="d9c72-112">EF Designer kullanarak modelinizi tasarlarken, bir .edmx dosyası modelinizi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d9c72-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="d9c72-113">.Edmx dosyası içinde bir **ilişkilendirme** öğe iki varlık türleri arasındaki bir ilişkiyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d9c72-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="d9c72-114">İlişkilendirmesine katılan varlık türleri ve varlık türleri çeşitliliği bilinen ilişkinin her iki ucunda olası sayısını belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="d9c72-115">Bir ilişkilendirme end'ün çoğulluğunun bir değer bir (1) sıfır veya bir (0..1) ya da birden çok olabilir (\*).</span><span class="sxs-lookup"><span data-stu-id="d9c72-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="d9c72-116">Bu bilgiler, iki alt belirtilen **son** öğeleri.</span><span class="sxs-lookup"><span data-stu-id="d9c72-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="d9c72-117">Ürün (varlıklarınızı yabancı anahtarları kullanıma sunmak isterseniz) çalışma zamanında, varlık türü örneklerinin bir ilişkilendirmenin bir ucunda Gezinti özellikleri veya yabancı anahtarlar erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="d9c72-118">Kullanıma sunulan yabancı anahtarlar ile varlıklar arasında ilişki ile yönetilen bir **Referentialconstraint'teki** öğesi (alt öğesi **ilişkilendirme** öğesi).</span><span class="sxs-lookup"><span data-stu-id="d9c72-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="d9c72-119">Yabancı anahtar ilişkileri için her zaman içinde varlıklarınızı kullanıma önerilir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="d9c72-120">Çoktan çoğa içinde (\*:\*) varlıkları için yabancı anahtarlar ekleyemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9c72-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="d9c72-121">İçinde bir \*:\* ilişki, ilişki bilgilerini, bağımsız bir nesne ile yönetilir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="d9c72-122">CSDL öğeleri hakkında bilgi için (**Referentialconstraint'teki**, **ilişkilendirme**, vb.) görmek [CSDL belirtimi](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="d9c72-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="d9c72-123">Oluşturma ve ilişkileri silme</span><span class="sxs-lookup"><span data-stu-id="d9c72-123">Create and Delete Associations</span></span>

<span data-ttu-id="d9c72-124">EF Designer güncelleştirmeleri ile ilişkilendirme .edmx dosyasını modeli içeriğini oluşturma.</span><span class="sxs-lookup"><span data-stu-id="d9c72-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="d9c72-125">Bir ilişkilendirme oluşturduktan sonra eşlemeleri (Bu konunun ilerleyen bölümlerinde açıklanmıştır) ilişkisi oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="d9c72-126">Bu bölümde, modeldeki arasında bir ilişki oluşturmak istediğiniz varlıkları zaten eklenmiş varsayılır.</span><span class="sxs-lookup"><span data-stu-id="d9c72-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="d9c72-127">Bir ilişkilendirme oluşturmak için</span><span class="sxs-lookup"><span data-stu-id="d9c72-127">To create an association</span></span>

1.  <span data-ttu-id="d9c72-128">Tasarım yüzeyinde boş bir alana sağ tıklayın, fareyle **yeni Ekle**seçip **ilişki...** .</span><span class="sxs-lookup"><span data-stu-id="d9c72-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="d9c72-129">İlişkilendirmeyi ayarlarını doldurun **ekleme ilişkilendirme** iletişim.</span><span class="sxs-lookup"><span data-stu-id="d9c72-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![AddAssociation](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="d9c72-131">Gezinti özellikleri veya yabancı anahtar özelliklerini ilişkilendirmenin bir ucunda varlıklara temizleyerek eklememeyi seçebilirsiniz ** gezinti özelliği ** ve ** yabancı anahtar özellikleri &lt;varlık türü adı&gt; varlık ** onay kutuları.</span><span class="sxs-lookup"><span data-stu-id="d9c72-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the **Navigation Property **and **Add foreign key properties to the &lt;entity type name&gt; Entity **checkboxes.</span></span> <span data-ttu-id="d9c72-132">Yalnızca bir gezinti özelliği eklerseniz, ilişki sadece tek yöndedir traversable olacaktır.</span><span class="sxs-lookup"><span data-stu-id="d9c72-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="d9c72-133">Gezinti özelliği eklerseniz, ilişkilendirmenin bir ucunda varlıklara erişmek için yabancı anahtar özelliklerini eklemek seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="d9c72-134">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="d9c72-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="d9c72-135">Bir ilişkiyi silmek için</span><span class="sxs-lookup"><span data-stu-id="d9c72-135">To delete an association</span></span>

<span data-ttu-id="d9c72-136">Bir ilişkilendirme aşağıdakilerden birini yapın silmek için:</span><span class="sxs-lookup"><span data-stu-id="d9c72-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="d9c72-137">EF Tasarımcı yüzeyi ve select ilişkilendirmenin sağ **Sil**.</span><span class="sxs-lookup"><span data-stu-id="d9c72-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="d9c72-138">OR-</span><span class="sxs-lookup"><span data-stu-id="d9c72-138">OR -</span></span>

-   <span data-ttu-id="d9c72-139">Bir veya daha fazla ilişkilendirmesi'ni seçin ve DELETE tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="d9c72-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="d9c72-140">Yabancı anahtar özelliklerini varlıklarınızı (başvuru kısıtlamalarını) içerir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="d9c72-141">Yabancı anahtar ilişkileri için her zaman içinde varlıklarınızı kullanıma önerilir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="d9c72-142">Entity Framework başvurusal Kısıt bir ilişki için yabancı anahtar olarak davranan bir özelliği tanımlamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="d9c72-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="d9c72-143">İşaretlediyseniz ***yabancı anahtar özellikleri &lt;varlık türü adı&gt; varlık*** onay kutusu ilişki oluştururken bu başvuru kısıtlamasını sizin için eklendi.</span><span class="sxs-lookup"><span data-stu-id="d9c72-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="d9c72-144">EF Designer eklemek veya bir başvuru kısıtlamasını düzenlemek için EF Designer'ı kullandığınızda, ekler veya değiştirir bir **Referentialconstraint'teki** .edmx dosyasını CSDL içeriğini öğesinde.</span><span class="sxs-lookup"><span data-stu-id="d9c72-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="d9c72-145">Düzenlemek istediğiniz ilişkilendirme çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d9c72-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="d9c72-146">**Başvuru kısıtlamasını** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="d9c72-147">Gelen **asıl** aşağı açılan listesinde, başvuru kısıtlamasındaki asıl varlığı seçin.</span><span class="sxs-lookup"><span data-stu-id="d9c72-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="d9c72-148">Varlığın anahtar özellikler eklenir **sorumlusu anahtarı** iletişim kutusunda listesi.</span><span class="sxs-lookup"><span data-stu-id="d9c72-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="d9c72-149">Gelen **bağımlı** aşağı açılan listesinde, başvuru kısıtlamasındaki bağımlı varlığı seçin.</span><span class="sxs-lookup"><span data-stu-id="d9c72-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="d9c72-150">Bağımlı bir anahtara sahip her asıl anahtarı için aşağı açılan listelerden karşılık gelen bir bağımlı anahtarı seçin. **bağımlı anahtarı** sütun.</span><span class="sxs-lookup"><span data-stu-id="d9c72-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![RefConstraint](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="d9c72-152">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="d9c72-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="d9c72-153">Oluşturma ve ilişkilendirme eşlemelerini düzenleme</span><span class="sxs-lookup"><span data-stu-id="d9c72-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="d9c72-154">Bir ilişkilendirme veritabanına eşlemelerini nasıl belirtebilirsiniz **eşleşme ayrıntıları** EF Designer'ın penceresi.</span><span class="sxs-lookup"><span data-stu-id="d9c72-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="d9c72-155">Yalnızca belirtilen başvurusal Kısıt olmayan ilişkilendirmeleri ayrıntılarını eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9c72-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="d9c72-156">Başvurusal Kısıt belirtilirse yabancı anahtar özellik varlıkta bulunan ve varlık yabancı anahtarı hangi sütunun eşlendiği denetlemek için eşleşme ayrıntıları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9c72-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="d9c72-157">Bir ilişkilendirme eşlemesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="d9c72-157">Create an association mapping</span></span>

-   <span data-ttu-id="d9c72-158">İlişkilendirme tasarım yüzeyi ve seçin, sağ **Tablo eşleme**.</span><span class="sxs-lookup"><span data-stu-id="d9c72-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="d9c72-159">Bu ilişkilendirme eşlemede görüntüler **eşleme ayrıntılarını** penceresi.</span><span class="sxs-lookup"><span data-stu-id="d9c72-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="d9c72-160">Tıklayın **bir tablo veya Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="d9c72-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="d9c72-161">Depolama modelinde tüm tabloları içeren bir açılır liste görünür.</span><span class="sxs-lookup"><span data-stu-id="d9c72-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="d9c72-162">İlişkilendirme eşler tabloyu seçin.</span><span class="sxs-lookup"><span data-stu-id="d9c72-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="d9c72-163">**Eşleşme ayrıntıları** pencere görüntüler her iki ucunda da ilişki ve varlık türü için anahtar özellikler her **son**.</span><span class="sxs-lookup"><span data-stu-id="d9c72-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="d9c72-164">Her bir anahtar özellik için tıklatın **sütun** alan ve özelliği eşlemek istediğiniz sütunu seçin.</span><span class="sxs-lookup"><span data-stu-id="d9c72-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![MappingDetails4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="d9c72-166">Bir ilişkilendirme eşlemeyi Düzenle</span><span class="sxs-lookup"><span data-stu-id="d9c72-166">Edit an association mapping</span></span>

-   <span data-ttu-id="d9c72-167">İlişkilendirme tasarım yüzeyi ve seçin, sağ **Tablo eşleme**.</span><span class="sxs-lookup"><span data-stu-id="d9c72-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="d9c72-168">Bu ilişkilendirme eşlemede görüntüler **eşleme ayrıntılarını** penceresi.</span><span class="sxs-lookup"><span data-stu-id="d9c72-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="d9c72-169">Tıklayın **eşlendiği &lt;tablo adı&gt;**.</span><span class="sxs-lookup"><span data-stu-id="d9c72-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="d9c72-170">Depolama modelinde tüm tabloları içeren bir açılır liste görünür.</span><span class="sxs-lookup"><span data-stu-id="d9c72-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="d9c72-171">İlişkilendirme eşler tabloyu seçin.</span><span class="sxs-lookup"><span data-stu-id="d9c72-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="d9c72-172">**Eşleşme ayrıntıları** penceresi, her iki ucunda da ilişki ve varlık türü için anahtar özellikler her sonunda görüntüler.</span><span class="sxs-lookup"><span data-stu-id="d9c72-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="d9c72-173">Her bir anahtar özellik için tıklatın **sütun** alan ve özelliği eşlemek istediğiniz sütunu seçin.</span><span class="sxs-lookup"><span data-stu-id="d9c72-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="d9c72-174">Düzenle ve Sil Gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="d9c72-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="d9c72-175">Gezinti özellikleri bir modelde ilişkilendirme ucunda varlıkları bulmak için kullanılan kısayol özelliklerdir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="d9c72-176">İki varlık türleri arasındaki ilişkiyi oluşturduğunuzda, gezinti özellikleri oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="d9c72-177">Gezinti özelliklerini düzenlemek için</span><span class="sxs-lookup"><span data-stu-id="d9c72-177">To edit navigation properties</span></span>

-   <span data-ttu-id="d9c72-178">EF Designer yüzeyinde bir gezinti özelliği seçin.</span><span class="sxs-lookup"><span data-stu-id="d9c72-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="d9c72-179">Gezinti özelliği hakkında bilgi, Visual Studio'da görüntülenir **özellikleri** penceresi.</span><span class="sxs-lookup"><span data-stu-id="d9c72-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="d9c72-180">Özellik ayarlarını değiştirme **özellikleri** penceresi.</span><span class="sxs-lookup"><span data-stu-id="d9c72-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="d9c72-181">Gezinti özellikleri silmek için</span><span class="sxs-lookup"><span data-stu-id="d9c72-181">To delete navigation properties</span></span>

-   <span data-ttu-id="d9c72-182">Yabancı anahtarlar kavramsal modelin varlık türlerini gösterilmediğinden, bir gezinti özelliği silme karşılık gelen ilişki traversable tek bir yönde veya değil traversable hiç kalmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="d9c72-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="d9c72-183">EF Tasarımcı yüzeyi ve select Gezinti özelliğindeki sağ **Sil**.</span><span class="sxs-lookup"><span data-stu-id="d9c72-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
