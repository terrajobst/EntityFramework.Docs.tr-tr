---
title: İlişkiler, gezinti özellikleri ve yabancı anahtarlar - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: c1d48f18a7dd25a6a48537f0de5379f861bf447a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998007"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a><span data-ttu-id="425a9-102">İlişkiler, gezinti özellikleri ve yabancı anahtarlar</span><span class="sxs-lookup"><span data-stu-id="425a9-102">Relationships, navigation properties and foreign keys</span></span>
<span data-ttu-id="425a9-103">Bu konu, Entity Framework varlıklar arasındaki ilişkilerin nasıl yönettiğine bir genel bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="425a9-103">This topic gives an overview of how Entity Framework manages relationships between entities.</span></span> <span data-ttu-id="425a9-104">Ayrıca ilişkileri eşleyin ve düzenleme hakkında rehberlik sağlar.</span><span class="sxs-lookup"><span data-stu-id="425a9-104">It also gives some guidance on how to map and manipulate relationships.</span></span>

## <a name="relationships-in-ef"></a><span data-ttu-id="425a9-105">EF ilişkileri</span><span class="sxs-lookup"><span data-stu-id="425a9-105">Relationships in EF</span></span>

<span data-ttu-id="425a9-106">İlişkisel veritabanları, tablolar arasında ilişki (ilişkilendirmeleri olarak da bilinir), yabancı anahtarlar tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="425a9-106">In relational databases, relationships (also called associations) between tables are defined through foreign keys.</span></span> <span data-ttu-id="425a9-107">Yabancı anahtar (FK) bir sütun veya kurmak ve iki tablodaki veriler arasında bir bağlantı zorlamak için kullanılan sütunlar bileşimidir.</span><span class="sxs-lookup"><span data-stu-id="425a9-107">A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables.</span></span> <span data-ttu-id="425a9-108">Genellikle üç türde ilişki vardır: bire bir, bire çok ve çok-çok.</span><span class="sxs-lookup"><span data-stu-id="425a9-108">There are generally three types of relationships: one-to-one, one-to-many, and many-to-many.</span></span> <span data-ttu-id="425a9-109">Bir-çok ilişki, yabancı anahtar birçok ilişki sonunu temsil eden tabloda tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="425a9-109">In a one-to-many relationship, the foreign key is defined on the table that represents the many end of the relationship.</span></span> <span data-ttu-id="425a9-110">Çoktan çoğa ilişki ile ilgili iki tablodan yabancı anahtarlar birincil anahtarı oluşur (bir birleşim veya birleşim tablo olarak adlandırılır) üçüncü bir tablo tanımlama ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="425a9-110">The many-to-many relationship involves defining a third table (called a junction or join table), whose primary key is composed of the foreign keys from both related tables.</span></span> <span data-ttu-id="425a9-111">Bire bir ilişki, yabancı anahtar olarak ayrıca birincil anahtar görevi görür ve ayrı yabancı anahtar sütunu yok ya da tablo için yoktur.</span><span class="sxs-lookup"><span data-stu-id="425a9-111">In a one-to-one relationship, the primary key acts additionally as a foreign key and there is no separate foreign key column for either table.</span></span>

<span data-ttu-id="425a9-112">Aşağıdaki görüntüde katılan iki tablo-çok ilişkisini gösterir.</span><span class="sxs-lookup"><span data-stu-id="425a9-112">The following image shows two tables that participate in one-to-many relationship.</span></span> <span data-ttu-id="425a9-113">**Kurs** tablodur bağımlı tablo içerdiği için **DepartmentID** bağlantı sütunu **departmanı** tablo.</span><span class="sxs-lookup"><span data-stu-id="425a9-113">The **Course** table is the dependent table because it contains the **DepartmentID** column that links it to the **Department** table.</span></span>

![Veritabanı2](~/ef6/media/database2.png)

<span data-ttu-id="425a9-115">Varlık Çerçevesi'nde varlığın diğer varlıklarla ilişki veya ilişki ile ilgili olabilir.</span><span class="sxs-lookup"><span data-stu-id="425a9-115">In Entity Framework, an entity can be related to other entities through an association or relationship.</span></span> <span data-ttu-id="425a9-116">Her ilişki varlık türü ve tür (bir, sıfır veya bir veya birçok) Bu ilişki iki varlıktaki için Çokluk tanımlayan iki ucu içerir.</span><span class="sxs-lookup"><span data-stu-id="425a9-116">Each relationship contains two ends that describe the entity type and the multiplicity of the type (one, zero-or-one, or many) for the two entities in that relationship.</span></span> <span data-ttu-id="425a9-117">İlişki hangi son ilişkisinde bir asıl rolüdür tanımlayan bir başvuru kısıtlamasını tarafından yönetilebilir ve bağımlı rol olduğu.</span><span class="sxs-lookup"><span data-stu-id="425a9-117">The relationship may be governed by a referential constraint, which describes which end in the relationship is a principal role and which is a dependent role.</span></span>

<span data-ttu-id="425a9-118">Gezinti özellikleri iki varlık türleri arasındaki ilişkiyi gitmek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="425a9-118">Navigation properties provide a way to navigate an association between two entity types.</span></span> <span data-ttu-id="425a9-119">Her nesne içinde katılan her ilişki için bir gezinti özelliği olabilir.</span><span class="sxs-lookup"><span data-stu-id="425a9-119">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="425a9-120">Gezinti özellikleri gidin ve bir başvuru nesnesi döndürerek her iki yönde ilişkileri yönetmenize olanak sağlar (çeşitlilik tek ise ya da sıfır veya bir) veya (çeşitlilik birçok ise) bir koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="425a9-120">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="425a9-121">Ayrıca tek yönlü gezinme her ikisini de değil ve ilişkisine katılıyor türleri yalnızca birinde gezinme özelliği bu durumda tanımlamak seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="425a9-121">You may also choose to have one-way navigation, in which case you define the navigation property on only one of the types that participates in the relationship and not on both.</span></span>

<span data-ttu-id="425a9-122">Bir veritabanındaki yabancı anahtarlar eşleyen modelinde özellikler eklemek için önerilir.</span><span class="sxs-lookup"><span data-stu-id="425a9-122">It is recommended to include properties in the model that map to foreign keys in the database.</span></span> <span data-ttu-id="425a9-123">Dahil edilen yabancı anahtar özellikleri ile oluşturabilir veya bağımlı bir nesne üzerinde yabancı anahtar değerini değiştirerek bir ilişki.</span><span class="sxs-lookup"><span data-stu-id="425a9-123">With foreign key properties included, you can create or change a relationship by modifying the foreign key value on a dependent object.</span></span> <span data-ttu-id="425a9-124">Bu tür bir ilişki, yabancı anahtar ilişkilendirmesi adı verilir.</span><span class="sxs-lookup"><span data-stu-id="425a9-124">This kind of association is called a foreign key association.</span></span> <span data-ttu-id="425a9-125">Yabancı anahtarlar kullanarak bağlantısı kesilmiş varlıklar ile çalışırken daha da önemlidir.</span><span class="sxs-lookup"><span data-stu-id="425a9-125">Using foreign keys is even more essential when working with disconnected entities.</span></span> <span data-ttu-id="425a9-126">Not, söz konusu olduğunda 1-1 veya 1-0 ile çalışma... 1 ilişki ayrı yabancı anahtar sütunu yok, birincil anahtar özelliği yabancı anahtar olarak davranır ve modeldeki her zaman dahildir.</span><span class="sxs-lookup"><span data-stu-id="425a9-126">Note, that when working with 1-to-1 or 1-to-0..1 relationships, there is no separate foreign key column, the primary key property acts as the foreign key and is always included in the model.</span></span>

<span data-ttu-id="425a9-127">Yabancı anahtar sütunları, modelde yer almaz, ilişki bilgilerini bağımsız bir nesne yönetilir.</span><span class="sxs-lookup"><span data-stu-id="425a9-127">When foreign key columns are not included in the model, the association information is managed as an independent object.</span></span> <span data-ttu-id="425a9-128">Yabancı anahtar özellikleri yerine nesne başvuruları arasında ilişkileri izlenir.</span><span class="sxs-lookup"><span data-stu-id="425a9-128">Relationships are tracked through object references instead of foreign key properties.</span></span> <span data-ttu-id="425a9-129">Bu tür bir ilişkilendirme olarak adlandırılan bir *bağımsız ilişkilendirme*.</span><span class="sxs-lookup"><span data-stu-id="425a9-129">This type of association is called an *independent association*.</span></span> <span data-ttu-id="425a9-130">Değiştirmek için en yaygın yolu bir *bağımsız ilişkilendirme* ilişkilendirmesine katılan her varlık için oluşturulan gezinti özelliklerini değiştirmek için.</span><span class="sxs-lookup"><span data-stu-id="425a9-130">The most common way to modify an *independent association* is to modify the navigation properties that are generated for each entity that participates in the association.</span></span>

<span data-ttu-id="425a9-131">Modelinizde ilişkilendirmeleri birini veya ikisini türleri kullanmayı da tercih edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="425a9-131">You can choose to use one or both types of associations in your model.</span></span> <span data-ttu-id="425a9-132">Yalnızca yabancı anahtarları içeren bir birleştirme tablo bağlı saf bir çoktan çoğa ilişki varsa, ancak, EF bağımsız ilişkilendirme böyle çoktan çoğa ilişki yönetmek için kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="425a9-132">However, if you have a pure many-to-many relationship that is connected by a join table that contains only foreign keys, the EF will use an independent association to manage such many-to-many relationship.</span></span>   

<span data-ttu-id="425a9-133">Aşağıdaki görüntüde, Entity Framework Designer ile oluşturulmuş bir kavramsal model gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="425a9-133">The following image shows a conceptual model that was created with the Entity Framework Designer.</span></span> <span data-ttu-id="425a9-134">Model-çok ilişkide yer alan iki varlık içerir.</span><span class="sxs-lookup"><span data-stu-id="425a9-134">The model contains two entities that participate in one-to-many relationship.</span></span> <span data-ttu-id="425a9-135">Her iki varlık Gezinti özellikleri vardır.</span><span class="sxs-lookup"><span data-stu-id="425a9-135">Both entities have navigation properties.</span></span> <span data-ttu-id="425a9-136">**Kurs** bağımlı varlık ve **DepartmentID** tanımlı yabancı anahtar özelliği.</span><span class="sxs-lookup"><span data-stu-id="425a9-136">**Course** is the depend entity and has the **DepartmentID** foreign key property defined.</span></span>

![RelationshipEFDesigner](~/ef6/media/relationshipefdesigner.png)

<span data-ttu-id="425a9-138">Aşağıdaki kod parçacığı, Code First ile oluşturulan aynı modelin gösterir.</span><span class="sxs-lookup"><span data-stu-id="425a9-138">The following code snippet shows the same model that was created with Code First.</span></span>

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class Department
{
   public Department()
   {
     this.Course = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a><span data-ttu-id="425a9-139">Yapılandırma veya ilişkileri eşleme</span><span class="sxs-lookup"><span data-stu-id="425a9-139">Configuring or mapping relationships</span></span>

<span data-ttu-id="425a9-140">Bu sayfanın geri kalanını erişmek ve ilişkileri kullanarak verileri işlemek nasıl etkinleştireceğinizi de açıklar.</span><span class="sxs-lookup"><span data-stu-id="425a9-140">The rest of this page covers how to access and manipulate data using relationships.</span></span> <span data-ttu-id="425a9-141">Modelinizde ilişkileri ayarlama hakkında daha fazla bilgi için şu sayfalara bakın.</span><span class="sxs-lookup"><span data-stu-id="425a9-141">For information on setting up relationships in your model, see the following pages.</span></span>

-   <span data-ttu-id="425a9-142">İlişkiler Code First yapılandırmak için bkz [veri ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md) ve [Fluent API'si – ilişkileri](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="425a9-142">To configure relationships in Code First, see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>
-   <span data-ttu-id="425a9-143">Entity Framework Designer kullanarak ilişkilerini yapılandırmak için bkz [EF Designer ilişkilerle](~/ef6/modeling/designer/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="425a9-143">To configure relationships using the Entity Framework Designer, see [Relationships with the EF Designer](~/ef6/modeling/designer/relationships.md).</span></span>

## <a name="creating-and-modifying-relationships"></a><span data-ttu-id="425a9-144">Oluşturma ve ilişkileri değiştirme</span><span class="sxs-lookup"><span data-stu-id="425a9-144">Creating and modifying relationships</span></span>

<span data-ttu-id="425a9-145">İçinde bir *yabancı anahtar ilişkilendirmesi*, ilişki, durum değişikliklerini EntityState.Modified için bir EntityState.Unchanged ile bağımlı bir nesnenin durumu değiştiğinde.</span><span class="sxs-lookup"><span data-stu-id="425a9-145">In a *foreign key association*, when you change the relationship, the state of a dependent object with an EntityState.Unchanged state changes to EntityState.Modified.</span></span> <span data-ttu-id="425a9-146">Bağımsız bir ilişkide, ilişkinin değiştirme bağımlı nesne durumunu güncelleştirmez.</span><span class="sxs-lookup"><span data-stu-id="425a9-146">In an independent relationship, changing the relationship does not update the state of the dependent object.</span></span>

<span data-ttu-id="425a9-147">Aşağıdaki örnekler, yabancı anahtar özellikler ve gezinti özellikleri ilgili nesneleri ilişkilendirmek için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="425a9-147">The following examples show how to use the foreign key properties and navigation properties to associate the related objects.</span></span> <span data-ttu-id="425a9-148">Yabancı anahtar ilişkilerini değiştirme, oluşturmak veya ilişkileri değiştirmek için her iki yöntem kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="425a9-148">With foreign key associations, you can use either method to change, create, or modify relationships.</span></span> <span data-ttu-id="425a9-149">Bağımsız ilişkilerini, yabancı anahtar özelliği kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="425a9-149">With independent associations, you cannot use the foreign key property.</span></span>

-   <span data-ttu-id="425a9-150">Yeni bir değer aşağıdaki örnekte olduğu gibi bir yabancı anahtar özelliğine atayarak.</span><span class="sxs-lookup"><span data-stu-id="425a9-150">By assigning a new value to a foreign key property, as in the following example.</span></span>  
    ``` csharp
    course.DepartmentID = newCourse.DepartmentID;
    ```

-   <span data-ttu-id="425a9-151">Aşağıdaki kod bir ilişki, yabancı anahtarı ayarlayarak kaldırır **null**.</span><span class="sxs-lookup"><span data-stu-id="425a9-151">The following code removes a relationship by setting the foreign key to **null**.</span></span> <span data-ttu-id="425a9-152">Yabancı anahtar özelliği null olması gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="425a9-152">Note, that the foreign key property must be nullable.</span></span>  
    ``` csharp
    course.DepartmentID = null;
    ```  
    >[!NOTE]
    > <span data-ttu-id="425a9-153">Başvuru (Bu örnekte, kurs nesnesi) eklenmiş durumda ise, SaveChanges çağrılana kadar başvuru gezinti özelliği yeni bir nesnenin anahtar değerleriyle eşitlenmez.</span><span class="sxs-lookup"><span data-stu-id="425a9-153">If the reference is in the added state (in this example, the course object), the reference navigation property will not be synchronized with the key values of a new object until SaveChanges is called.</span></span> <span data-ttu-id="425a9-154">Nesne bağlamı kaydedilmeden kadar eklenen nesneler için kalıcı anahtarlar içermediğinden eşitleme gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="425a9-154">Synchronization does not occur because the object context does not contain permanent keys for added objects until they are saved.</span></span> <span data-ttu-id="425a9-155">Yeni nesneler ilişkisi hemen sonra tam olarak eşitlenmiş olması gerekir, aşağıdaki yöntemlerin birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="425a9-155">If you must have new objects fully synchronized as soon as you set the relationship, use one of the following methods.\*</span></span>

-   <span data-ttu-id="425a9-156">Yeni bir nesne bir gezinti özelliğine atayarak.</span><span class="sxs-lookup"><span data-stu-id="425a9-156">By assigning a new object to a navigation property.</span></span> <span data-ttu-id="425a9-157">Aşağıdaki kod bir kurs arasında bir ilişki oluşturur ve bir `department`.</span><span class="sxs-lookup"><span data-stu-id="425a9-157">The following code creates a relationship between a course and a `department`.</span></span> <span data-ttu-id="425a9-158">Nesneleri bağlamına ekliyse `course` de eklenir `department.Courses` koleksiyonu ve yabancı karşılık gelen anahtar özellik üzerinde `course` nesne departmanı anahtar özellik değerine ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="425a9-158">If the objects are attached to the context, the `course` is also added to the `department.Courses` collection, and the corresponding foreign key property on the `course` object is set to the key property value of the department.</span></span>  
    ``` csharp
    course.Department = department;
    ```

 -   <span data-ttu-id="425a9-159">İlişkiyi silmek için gezinme özelliğini ayarlamak `null`.</span><span class="sxs-lookup"><span data-stu-id="425a9-159">To delete the relationship, set the navigation property to `null`.</span></span> <span data-ttu-id="425a9-160">Entity Framework, .NET 4.0 tabanlı ile çalışıyorsanız, ilgili uç, null olarak ayarlamadan önce yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="425a9-160">If you are working with Entity Framework that is based on .NET 4.0, then the related end needs to be loaded before you set it to null.</span></span> <span data-ttu-id="425a9-161">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="425a9-161">For example:</span></span>  
    ``` chsarp
    context.Entry(course).Reference(c => c.Department).Load();  
    course.Department = null;
    ```  
    <span data-ttu-id="425a9-162">Entity Framework, .NET 4.5 üzerinde temel alınan 5.0 ile başlatma, ilişki null ilgili uç yüklemeden ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="425a9-162">Starting with Entity Framework 5.0, that is based on .NET 4.5, you can set the relationship to null without loading the related end.</span></span> <span data-ttu-id="425a9-163">Ayrıca, aşağıdaki yöntemi kullanarak null değeri ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="425a9-163">You can also set the current value to null using the following method.</span></span>  
    ``` csharp
    context.Entry(course).Reference(c => c.Department).CurrentValue = null;
    ```

-   <span data-ttu-id="425a9-164">Silme veya bir varlık koleksiyonu bir nesne ekleme.</span><span class="sxs-lookup"><span data-stu-id="425a9-164">By deleting or adding an object in an entity collection.</span></span> <span data-ttu-id="425a9-165">Örneğin, türü bir nesne ekleyebilirsiniz `Course` için `department.Courses` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="425a9-165">For example, you can add an object of type `Course` to the `department.Courses` collection.</span></span> <span data-ttu-id="425a9-166">Bu işlem, belirli bir arasında bir ilişki oluşturur **kurs** ve belirli bir `department`.</span><span class="sxs-lookup"><span data-stu-id="425a9-166">This operation creates a relationship between a particular **course** and a particular `department`.</span></span> <span data-ttu-id="425a9-167">Nesneler üzerinde eklenmiş içerik, departman başvuru ve yabancı anahtar özelliği varsa **kurs** nesne ayarlanacak uygun `department`.</span><span class="sxs-lookup"><span data-stu-id="425a9-167">If the objects are attached to the context, the department reference and the foreign key property on the **course** object will be set to the appropriate `department`.</span></span>  
    ``` csharp
    department.Courses.Add(newCourse);
    ```

- <span data-ttu-id="425a9-168">Kullanarak `ChangeRelationshipState` iki varlık nesnesi arasında belirtilen ilişki durumunu değiştirmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="425a9-168">By using the `ChangeRelationshipState` method to change the state of the specified relationship between two entity objects.</span></span> <span data-ttu-id="425a9-169">Bu yöntem N katmanlı uygulamalar ile çalışırken en yaygın olarak kullanılır ve bir *bağımsız ilişkilendirme* (yabancı anahtar ilişkilendirmesi ile kullanılamaz).</span><span class="sxs-lookup"><span data-stu-id="425a9-169">This method is most commonly used when working with N-Tier applications and an *independent association* (it cannot be used with a foreign key association).</span></span> <span data-ttu-id="425a9-170">Ayrıca, bu yöntemi kullanmak için düşürmeli aşağı `ObjectContext`aşağıdaki örnekte gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="425a9-170">Also, to use this method you must drop down to `ObjectContext`, as shown in the example below.</span></span>  
<span data-ttu-id="425a9-171">Aşağıdaki örnekte, eğitmenlerini ve dersleri arasında bir çoktan çoğa ilişki yoktur.</span><span class="sxs-lookup"><span data-stu-id="425a9-171">In the following example, there is a many-to-many relationship between Instructors and Courses.</span></span> <span data-ttu-id="425a9-172">Çağırma `ChangeRelationshipState` yöntemi ve geçirme `EntityState.Added` parametresi, sağlar `SchoolContext` iki nesne bir ilişki eklendiğini bildirin:</span><span class="sxs-lookup"><span data-stu-id="425a9-172">Calling the `ChangeRelationshipState` method and passing the `EntityState.Added` parameter, lets the `SchoolContext` know that a relationship has been added between the two objects:</span></span>

``` csharp

       ((IObjectContextAdapter)context).ObjectContext.
                 ObjectStateManager.
                  ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
```

    Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:

``` csharp
       ((IObjectContextAdapter)context).ObjectContext.
                  ObjectStateManager.
                  ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a><span data-ttu-id="425a9-173">Gezinti özellikleri ve yabancı anahtarlar arasındaki değişiklikleri eşitleme</span><span class="sxs-lookup"><span data-stu-id="425a9-173">Synchronizing the changes between the foreign keys and navigation properties</span></span>

<span data-ttu-id="425a9-174">Yukarıda anlatılan yöntemlerden birini kullanarak bağlamına iliştirilemez. nesneler arasındaki ilişkiyi değiştirdiğinizde, yabancı anahtarlar, başvuruları ve koleksiyonları eşitlemek Entity Framework gerekir. Entity Framework bu eşitleme (olarak da bilinen ilişki düzeltmesi yan yana) proxy'si ile POCO varlık için otomatik olarak yönetir.</span><span class="sxs-lookup"><span data-stu-id="425a9-174">When you change the relationship of the objects attached to the context by using one of the methods described above, Entity Framework needs to keep foreign keys, references, and collections in sync. Entity Framework automatically manages this synchronization (also known as relationship fix-up) for the POCO entities with proxies.</span></span> <span data-ttu-id="425a9-175">Daha fazla bilgi için [proxy ile çalışmayı](~/ef6/fundamentals/proxies.md).</span><span class="sxs-lookup"><span data-stu-id="425a9-175">For more information, see [Working with Proxies](~/ef6/fundamentals/proxies.md).</span></span>

<span data-ttu-id="425a9-176">POCO varlık olmayan proxy'si kullanıyorsanız, emin olmanız gerekir **DetectChanges** bağlamındaki ilişkili nesneleri eşitlenecek yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="425a9-176">If you are using POCO entities without proxies, you must make sure that the **DetectChanges** method is called to synchronize the related objects in the context.</span></span> <span data-ttu-id="425a9-177">Aşağıdaki API'ları otomatik olarak tetikleyen Not bir **DetectChanges** çağırın.</span><span class="sxs-lookup"><span data-stu-id="425a9-177">Note, that the following APIs automatically trigger a **DetectChanges** call.</span></span>

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   <span data-ttu-id="425a9-178">Yürütülen bir LINQ Sorgu karşı bir `DbSet`</span><span class="sxs-lookup"><span data-stu-id="425a9-178">Executing a LINQ query against a `DbSet`</span></span>

## <a name="loading-related-objects"></a><span data-ttu-id="425a9-179">Yükleme ile ilgili nesneler</span><span class="sxs-lookup"><span data-stu-id="425a9-179">Loading related objects</span></span>

<span data-ttu-id="425a9-180">En sık kullandığınız Entity Framework içinde tanımlanmış ilişki tarafından döndürülen varlık ilgili varlıkları yükleme için gezinme özelliklerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="425a9-180">In Entity Framework you use most commonly use the navigation properties to load entities that are related to the returned entity by the defined association.</span></span> <span data-ttu-id="425a9-181">Daha fazla bilgi için [ilgili nesneler Yükleniyor](~/ef6/querying/related-data.md).</span><span class="sxs-lookup"><span data-stu-id="425a9-181">For more information, see [Loading Related Objects](~/ef6/querying/related-data.md).</span></span>

> [!NOTE]
> <span data-ttu-id="425a9-182">Bağımlı bir nesne bir ilgili uç yüklediğinizde bir yabancı anahtar ilişkilendirmesine, ilgili nesneyi göre şu anda bellekte olmadığını bağımlı yabancı anahtar değeri olarak yüklenecektir:</span><span class="sxs-lookup"><span data-stu-id="425a9-182">In a foreign key association, when you load a related end of a dependent object, the related object will be loaded based on the foreign key value of the dependent that is currently in memory:</span></span>

``` csharp
    // Get the course where currently DepartmentID = 1.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

<span data-ttu-id="425a9-183">Bağımsız bir ilişkide, şu anda veritabanında yabancı anahtar değere göre bir bağımlı nesne ilgili sonuna sorgulanır.</span><span class="sxs-lookup"><span data-stu-id="425a9-183">In an independent association, the related end of a dependent object is queried based on the foreign key value that is currently in the database.</span></span> <span data-ttu-id="425a9-184">Ancak, ilişkisi değiştirildi ve bağımlı nesne noktalarında nesne bağlamında, Entity Framework yüklenen farklı bir asıl nesneye başvuru özelliği olarak bir ilişki oluşturmak deneyecek, istemcide tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="425a9-184">However, if the relationship was modified, and the reference property on the dependent object points to a different principal object that is loaded in the object context, Entity Framework will try to create a relationship as it is defined on the client.</span></span>

## <a name="managing-concurrency"></a><span data-ttu-id="425a9-185">Eşzamanlılığı yönetme</span><span class="sxs-lookup"><span data-stu-id="425a9-185">Managing concurrency</span></span>

<span data-ttu-id="425a9-186">Yabancı anahtar hem bağımsız ilişkilendirmeleri eşzamanlılık denetimlerinin varlık anahtarları ve modelde tanımlanan diğer varlık özellikleri temel alır.</span><span class="sxs-lookup"><span data-stu-id="425a9-186">In both foreign key and independent associations, concurrency checks are based on the entity keys and other entity properties that are defined in the model.</span></span> <span data-ttu-id="425a9-187">Bir model oluşturmak için EF Designer'ı kullanırken, ayarlayın `ConcurrencyMode` özniteliğini **sabit** özelliği için eşzamanlılık denetlenmesi gerektiğini belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="425a9-187">When using the EF Designer to create a model, set the `ConcurrencyMode` attribute to **fixed** to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="425a9-188">Bir modeli tanımlamak için Code First kullanarak kullanınl `ConcurrencyCheck` denetlenmesi için eşzamanlılık istediğiniz özellikleri ek açıklama.</span><span class="sxs-lookup"><span data-stu-id="425a9-188">When using Code First to define a model, use the `ConcurrencyCheck` annotation on properties that you want to be checked for concurrency.</span></span> <span data-ttu-id="425a9-189">Code First ile çalışırken de kullanabilirsiniz `TimeStamp` özelliği için eşzamanlılık denetlenmesi gerektiğini belirtmek için ek açıklama.</span><span class="sxs-lookup"><span data-stu-id="425a9-189">When working with Code First you can also use the `TimeStamp` annotation to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="425a9-190">Belirli bir sınıf içinde yalnızca bir zaman damgası özelliği olabilir.</span><span class="sxs-lookup"><span data-stu-id="425a9-190">You can have only one timestamp property in a given class.</span></span> <span data-ttu-id="425a9-191">Kod, bu özellik ilk veritabanı NULL olmayan bir alana eşlemeleri.</span><span class="sxs-lookup"><span data-stu-id="425a9-191">Code First maps this property to a non-nullable field in the database.</span></span>

<span data-ttu-id="425a9-192">Her zaman yabancı anahtar ilişkilendirmesi eşzamanlılık denetimi ve çözümleme katılacak varlıklar ile çalışırken kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="425a9-192">We recommend that you always use the foreign key association when working with entities that participate in concurrency checking and resolution.</span></span>

<span data-ttu-id="425a9-193">Daha fazla bilgi için [eşzamanlılık çakışmalarını işleme](~/ef6/saving/concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="425a9-193">For more information, see [Handling Concurrency Conflicts](~/ef6/saving/concurrency.md).</span></span>

## <a name="working-with-overlapping-keys"></a><span data-ttu-id="425a9-194">Anahtarları örtüşen ile çalışma</span><span class="sxs-lookup"><span data-stu-id="425a9-194">Working with overlapping Keys</span></span>

<span data-ttu-id="425a9-195">Bazı özellikler anahtarında da varlıktaki başka bir anahtarın parçası olduğu bileşik anahtarlar çakışan anahtarlar var.</span><span class="sxs-lookup"><span data-stu-id="425a9-195">Overlapping keys are composite keys where some properties in the key are also part of another key in the entity.</span></span> <span data-ttu-id="425a9-196">Çakışan bir anahtarı bağımsız ilişkilendirmesine sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="425a9-196">You cannot have an overlapping key in an independent association.</span></span> <span data-ttu-id="425a9-197">Anahtarları örtüşen içeren bir yabancı anahtar ilişkilendirmesi değiştirmek için nesne başvuruları kullanmak yerine, yabancı anahtar değerlerini değiştirmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="425a9-197">To change a foreign key association that includes overlapping keys, we recommend that you modify the foreign key values instead of using the object references.</span></span>
