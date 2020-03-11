---
title: İlişkiler-EF Tasarımcısı-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418247"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="16ee8-102">İlişkiler-EF Tasarımcısı</span><span class="sxs-lookup"><span data-stu-id="16ee8-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="16ee8-103">Bu sayfada, AŞV tasarımcısını kullanarak modelinizde ilişkiler ayarlama hakkında bilgi sağlanır.</span><span class="sxs-lookup"><span data-stu-id="16ee8-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="16ee8-104">EF 'teki ilişkiler ve ilişkileri kullanarak verilere erişme ve verileri işleme hakkında genel bilgi için bkz. [ilişkiler & gezinti özellikleri](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="16ee8-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="16ee8-105">İlişkilendirmeler bir modeldeki varlık türleri arasındaki ilişkileri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="16ee8-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="16ee8-106">Bu konu başlığı, Entity Framework Designer (EF Designer) ile ilişkilerin nasıl eşleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="16ee8-107">Aşağıdaki görüntüde, EF Designer ile çalışırken kullanılan ana pencereler gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Tasarımcısı](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="16ee8-109">Kavramsal model oluşturduğunuzda, eşlenmemiş varlıklar ve ilişkilendirmeler hakkında uyarılar Hata Listesi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="16ee8-110">Bu uyarıları yoksayabilirsiniz çünkü veritabanını modelden oluşturmayı seçtiğinizde hatalar kaybolur.</span><span class="sxs-lookup"><span data-stu-id="16ee8-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="16ee8-111">İlişkilendirmelere genel bakış</span><span class="sxs-lookup"><span data-stu-id="16ee8-111">Associations Overview</span></span>

<span data-ttu-id="16ee8-112">Ortamınızı EF Designer kullanarak tasarladığınızda, bir. edmx dosyası modelinizi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="16ee8-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="16ee8-113">. Edmx dosyasında, bir **ilişkilendirme** öğesi iki varlık türü arasında bir ilişki tanımlar.</span><span class="sxs-lookup"><span data-stu-id="16ee8-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="16ee8-114">Bir ilişki, ilişkiye dahil olan varlık türlerini ve ilişkinin her ucunda çoğulluk olarak bilinen varlık türlerinin olası sayısını belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="16ee8-115">Bir ilişki ucunun çoğulluğu bir (1), sıfır veya bir (0.. 1) veya çok (\*) bir değere sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="16ee8-116">Bu bilgiler iki alt **End** öğesinde belirtilir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="16ee8-117">Çalışma zamanında, bir ilişkilendirmenin bir sonundaki varlık türü örneklerine, gezinti özellikleri veya yabancı anahtarlar aracılığıyla erişilebilir (varlıklarınızda yabancı anahtarlar açığa çıkarmak istiyorsanız).</span><span class="sxs-lookup"><span data-stu-id="16ee8-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="16ee8-118">Yabancı anahtarlar kullanıma sunulduğunda, varlıklar arasındaki ilişki bir **ReferentialConstraint** öğesi ( **Association** öğesinin bir alt öğesi) ile yönetilir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="16ee8-119">Varlıklarınızda ilişkiler için her zaman yabancı anahtarlar oluşturmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="16ee8-120">Çoka çok (\*:\*), varlıklara yabancı anahtarlar ekleyemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="16ee8-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="16ee8-121">\*:\* ilişkisinde, ilişki bilgileri bağımsız bir nesne ile yönetilir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="16ee8-122">CSDL öğeleri (**ReferentialConstraint**, **ilişkilendirme**vb.) hakkında daha fazla bilgi için bkz. [csdl belirtimi](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="16ee8-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="16ee8-123">Ilişki oluşturma ve silme</span><span class="sxs-lookup"><span data-stu-id="16ee8-123">Create and Delete Associations</span></span>

<span data-ttu-id="16ee8-124">EF Designer ile bir ilişki oluşturmak. edmx dosyasının model içeriğini günceller.</span><span class="sxs-lookup"><span data-stu-id="16ee8-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="16ee8-125">İlişki oluşturduktan sonra ilişkilendirme için eşlemeler oluşturmanız gerekir (Bu konunun ilerleyen kısımlarında açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="16ee8-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="16ee8-126">Bu bölümde, modelinize arasında bir ilişki oluşturmak istediğiniz varlıkları zaten eklemiş olduğunuz varsayılır.</span><span class="sxs-lookup"><span data-stu-id="16ee8-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="16ee8-127">Bir ilişki oluşturmak için</span><span class="sxs-lookup"><span data-stu-id="16ee8-127">To create an association</span></span>

1.  <span data-ttu-id="16ee8-128">Tasarım yüzeyinde boş bir alana sağ tıklayın, **Yeni Ekle**' nin üzerine gelin ve **ilişkilendirme seç...** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="16ee8-129">İlişkilendirme **Ekle** iletişim kutusunda ilişkilendirmenin ayarlarını girin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![Ilişki Ekle](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="16ee8-131"> **Gezinti özelliğini **temizleyerek ve varlık onay kutularına **&lt;varlık türü adı&gt; yabancı anahtar özellikleri **ekleyerek, ilişkilendirmenin sonundaki varlıklara gezinti özellikleri veya yabancı anahtar özellikleri eklememe seçeneğini belirleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16ee8-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the **Navigation Property **and **Add foreign key properties to the &lt;entity type name&gt; Entity **checkboxes.</span></span> <span data-ttu-id="16ee8-132">Yalnızca bir gezinti özelliği eklerseniz, ilişki yalnızca bir yönde Traversable olacaktır.</span><span class="sxs-lookup"><span data-stu-id="16ee8-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="16ee8-133">Hiçbir gezinti özelliği yoksa, ilişkinin sonunda varlıklara erişebilmek için yabancı anahtar özellikleri eklemeyi seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="16ee8-134"> **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="16ee8-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="16ee8-135">Bir ilişkilendirmeyi silmek için</span><span class="sxs-lookup"><span data-stu-id="16ee8-135">To delete an association</span></span>

<span data-ttu-id="16ee8-136">Bir ilişkilendirmeyi silmek için aşağıdakilerden birini yapın:</span><span class="sxs-lookup"><span data-stu-id="16ee8-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="16ee8-137">EF Designer yüzeyinde ilişkiye sağ tıklayın ve **Sil**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="16ee8-138">Veya</span><span class="sxs-lookup"><span data-stu-id="16ee8-138">OR -</span></span>

-   <span data-ttu-id="16ee8-139">Bir veya daha fazla ilişki seçin ve DELETE tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="16ee8-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="16ee8-140">Varlıklarınızda yabancı anahtar özellikleri ekleyin (başvurusal kısıtlamalar)</span><span class="sxs-lookup"><span data-stu-id="16ee8-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="16ee8-141">Varlıklarınızda ilişkiler için her zaman yabancı anahtarlar oluşturmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="16ee8-142">Entity Framework bir özelliğin bir ilişki için yabancı anahtar görevi göreceğini belirlemek için bir başvuru kısıtlaması kullanır.</span><span class="sxs-lookup"><span data-stu-id="16ee8-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="16ee8-143">Bir ilişki oluştururken ***&lt;varlık türü adı&gt; varlık onay kutusuna yabancı anahtar özellikleri ekleme*** ' yi denetlediyseniz, bu başvuru kısıtlaması sizin için eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="16ee8-144">Bir başvuru kısıtlaması eklemek veya düzenlemek için EF tasarımcısını kullandığınızda, EF Designer,. edmx dosyasının CSDL içeriğindeki bir **ReferentialConstraint** öğesi ekler veya değiştirir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="16ee8-145">Düzenlemek istediğiniz ilişkiye çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="16ee8-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="16ee8-146"> **Başvurusal kısıtlama** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="16ee8-147"> **Asıl** açılan listesinden, başvuru kısıtlamasındaki asıl varlığı seçin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="16ee8-148">Varlığın anahtar özellikleri iletişim kutusundaki **asıl anahtar** listesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="16ee8-149"> **Bağımlı** aşağı açılan listesinden, başvuru kısıtlamasındaki bağımlı varlığı seçin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="16ee8-150">Bağımlı anahtarı olan her bir asıl anahtar için, **bağımlı anahtar** sütunundaki açılan listelerden karşılık gelen bir bağımlı anahtar seçin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![Ref kısıtlaması](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="16ee8-152"> **Tamam**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="16ee8-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="16ee8-153">Ilişki eşlemeleri oluşturma ve düzenleme</span><span class="sxs-lookup"><span data-stu-id="16ee8-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="16ee8-154">Bir ilişkilendirmenin, EF Designer 'ın **eşleme ayrıntıları** penceresinde veritabanına nasıl eşlendiğini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16ee8-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="16ee8-155">Yalnızca bir başvuru kısıtlaması belirtilmemiş ilişkilerin ayrıntılarını eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16ee8-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="16ee8-156">Bir başvuru kısıtlaması belirtilmişse, varlığa bir yabancı anahtar özelliği dahil edilir ve yabancı anahtarın hangi sütuna eşlendiğini denetlemek için varlık için eşleme ayrıntılarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16ee8-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="16ee8-157">İlişki eşlemesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="16ee8-157">Create an association mapping</span></span>

-   <span data-ttu-id="16ee8-158">Tasarım yüzeyinde bir ilişkiye sağ tıklayın ve **Tablo eşleme**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="16ee8-159">Bu, ilişkilendirme eşlemesini **eşleme ayrıntıları** penceresinde görüntüler.</span><span class="sxs-lookup"><span data-stu-id="16ee8-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="16ee8-160"> **Tablo veya Görünüm Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="16ee8-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="16ee8-161">Depolama modelindeki tüm tabloları içeren bir açılır liste görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="16ee8-162">İlişkilendirmenin eşolacağı tabloyu seçin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="16ee8-163"> **Eşleme ayrıntıları** penceresi, ilişkinin her iki ucunu ve her **uçta**varlık türü için anahtar özelliklerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="16ee8-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="16ee8-164">Her anahtar özelliği için, **sütun** alanına tıklayın ve özelliğin eşolacağı sütunu seçin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![Eşleme ayrıntıları 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="16ee8-166">İlişki eşlemesini düzenleme</span><span class="sxs-lookup"><span data-stu-id="16ee8-166">Edit an association mapping</span></span>

-   <span data-ttu-id="16ee8-167">Tasarım yüzeyinde bir ilişkiye sağ tıklayın ve **Tablo eşleme**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="16ee8-168">Bu, ilişkilendirme eşlemesini **eşleme ayrıntıları** penceresinde görüntüler.</span><span class="sxs-lookup"><span data-stu-id="16ee8-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="16ee8-169">\ \**Tablo adı&gt;&lt;Için haritalar\ \**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="16ee8-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="16ee8-170">Depolama modelindeki tüm tabloları içeren bir açılır liste görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="16ee8-171">İlişkilendirmenin eşolacağı tabloyu seçin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="16ee8-172"> **Eşleme ayrıntıları** penceresi, ilişkinin her iki ucunu ve her uçta varlık türü için anahtar özelliklerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="16ee8-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="16ee8-173">Her anahtar özelliği için, **sütun** alanına tıklayın ve özelliğin eşolacağı sütunu seçin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="16ee8-174">Gezinti özelliklerini Düzenle ve Sil</span><span class="sxs-lookup"><span data-stu-id="16ee8-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="16ee8-175">Gezinti özellikleri, bir modeldeki ilişkilendirmenin sonundaki varlıkları bulmak için kullanılan kısayol özellikleridir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="16ee8-176">İki varlık türü arasında bir ilişki oluşturduğunuzda, gezinti özellikleri oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="16ee8-177">Gezinti özelliklerini düzenlemek için</span><span class="sxs-lookup"><span data-stu-id="16ee8-177">To edit navigation properties</span></span>

-   <span data-ttu-id="16ee8-178">EF Designer yüzeyinde bir gezinti özelliği seçin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="16ee8-179">Gezinti özelliği hakkında bilgi, Visual Studio **özellikleri** penceresinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="16ee8-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="16ee8-180"> **Özellikler** penceresindeki özellik ayarlarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="16ee8-181">Gezinti özelliklerini silmek için</span><span class="sxs-lookup"><span data-stu-id="16ee8-181">To delete navigation properties</span></span>

-   <span data-ttu-id="16ee8-182">Kavramsal modeldeki varlık türlerinde yabancı anahtarlar gösterilmeyerek, bir gezinti özelliğinin silinmesi karşılık gelen ilişkilendirmeyi yalnızca bir yönde Traversable veya Traversable değil.</span><span class="sxs-lookup"><span data-stu-id="16ee8-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="16ee8-183">EF Designer yüzeyinde bir gezinti özelliğine sağ tıklayın ve **Sil**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="16ee8-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
