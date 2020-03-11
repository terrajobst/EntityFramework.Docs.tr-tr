---
title: İlişkiler, gezinti özellikleri ve yabancı anahtarlar-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: 892e872e3cb11ea95084cf6d9ab43c8d500bc0de
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419355"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a><span data-ttu-id="800bd-102">İlişkiler, gezinti özellikleri ve yabancı anahtarlar</span><span class="sxs-lookup"><span data-stu-id="800bd-102">Relationships, navigation properties, and foreign keys</span></span>

<span data-ttu-id="800bd-103">Bu makale, varlıklar arasındaki ilişkileri nasıl yönettiğini Entity Framework bir genel bakış sunar.</span><span class="sxs-lookup"><span data-stu-id="800bd-103">This article gives an overview of how Entity Framework manages relationships between entities.</span></span> <span data-ttu-id="800bd-104">Ayrıca, ilişkilerin eşlenme ve işleme hakkında bazı yönergeler de sağlar.</span><span class="sxs-lookup"><span data-stu-id="800bd-104">It also gives some guidance on how to map and manipulate relationships.</span></span>

## <a name="relationships-in-ef"></a><span data-ttu-id="800bd-105">EF 'teki ilişkiler</span><span class="sxs-lookup"><span data-stu-id="800bd-105">Relationships in EF</span></span>

<span data-ttu-id="800bd-106">İlişkisel veritabanlarında, tablolar arasındaki ilişkiler (ilişkilendirmeler olarak da bilinir) yabancı anahtarlar aracılığıyla tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="800bd-106">In relational databases, relationships (also called associations) between tables are defined through foreign keys.</span></span> <span data-ttu-id="800bd-107">Yabancı anahtar (FK), iki tablodaki veriler arasında bağlantı kurmak ve zorlamak için kullanılan bir sütun veya sütun birleşimidir.</span><span class="sxs-lookup"><span data-stu-id="800bd-107">A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables.</span></span> <span data-ttu-id="800bd-108">Genellikle üç tür ilişki vardır: bire bir, bire çok ve çoktan çoğa.</span><span class="sxs-lookup"><span data-stu-id="800bd-108">There are generally three types of relationships: one-to-one, one-to-many, and many-to-many.</span></span> <span data-ttu-id="800bd-109">Bire çok ilişkisinde, yabancı anahtar ilişkinin birçok sonunu temsil eden tabloda tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="800bd-109">In a one-to-many relationship, the foreign key is defined on the table that represents the many end of the relationship.</span></span> <span data-ttu-id="800bd-110">Çoktan çoğa ilişki, birincil anahtarı ilgili tablolardaki yabancı anahtarlardan oluşan bir üçüncü tablo (kavşak veya birleşim tablosu olarak adlandırılır) tanımlamayı içerir.</span><span class="sxs-lookup"><span data-stu-id="800bd-110">The many-to-many relationship involves defining a third table (called a junction or join table), whose primary key is composed of the foreign keys from both related tables.</span></span> <span data-ttu-id="800bd-111">Bire bir ilişkide, birincil anahtar ek olarak yabancı anahtar olarak davranır ve her iki tablo için ayrı bir yabancı anahtar sütunu yoktur.</span><span class="sxs-lookup"><span data-stu-id="800bd-111">In a one-to-one relationship, the primary key acts additionally as a foreign key and there is no separate foreign key column for either table.</span></span>

<span data-ttu-id="800bd-112">Aşağıdaki görüntüde bire çok ilişkisine katılan iki tablo gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="800bd-112">The following image shows two tables that participate in one-to-many relationship.</span></span> <span data-ttu-id="800bd-113">**Kurs** tablosu, **bölüm** tablosuna bağlayan **DepartmentID** sütununu içerdiğinden bağımlı tablodur.</span><span class="sxs-lookup"><span data-stu-id="800bd-113">The **Course** table is the dependent table because it contains the **DepartmentID** column that links it to the **Department** table.</span></span>

![Departman ve kurs tabloları](~/ef6/media/database2.png)

<span data-ttu-id="800bd-115">Entity Framework, bir varlık bir ilişki veya ilişki aracılığıyla diğer varlıklarla ilişkili olabilir.</span><span class="sxs-lookup"><span data-stu-id="800bd-115">In Entity Framework, an entity can be related to other entities through an association or relationship.</span></span> <span data-ttu-id="800bd-116">Her ilişki, söz konusu ilişkideki iki varlık için varlık türünü ve türün çokluğunu (bir, sıfır-veya-bir ya da çok) tanımlayan iki ucu içerir.</span><span class="sxs-lookup"><span data-stu-id="800bd-116">Each relationship contains two ends that describe the entity type and the multiplicity of the type (one, zero-or-one, or many) for the two entities in that relationship.</span></span> <span data-ttu-id="800bd-117">İlişki, ilişkinin bir asıl rol olduğunu ve bağımlı bir rol olduğunu açıklayan bir başvuru kısıtlaması tarafından yönetilebilir.</span><span class="sxs-lookup"><span data-stu-id="800bd-117">The relationship may be governed by a referential constraint, which describes which end in the relationship is a principal role and which is a dependent role.</span></span>

<span data-ttu-id="800bd-118">Gezinti özellikleri iki varlık türü arasındaki bir ilişkilendirmeyi gezinmek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="800bd-118">Navigation properties provide a way to navigate an association between two entity types.</span></span> <span data-ttu-id="800bd-119">Her nesnenin katıldığı her ilişki için bir gezinti özelliği olabilir.</span><span class="sxs-lookup"><span data-stu-id="800bd-119">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="800bd-120">Gezinti özellikleri, bir başvuru nesnesi (çoğulluk bir veya sıfır ya da-bir) ya da bir koleksiyon (çokluk çok ise) döndüren her iki yönde ilişkilerde gezinmeniz ve bunları yönetmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="800bd-120">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="800bd-121">Tek yönlü bir gezinmenin de tercih edebilirsiniz. Bu durumda, gezinti özelliğini yalnızca ilişkiye katılan ve her ikisi üzerinde değil, yalnızca birinde tanımladığınız türlerden birinde tanımlarsınız.</span><span class="sxs-lookup"><span data-stu-id="800bd-121">You may also choose to have one-way navigation, in which case you define the navigation property on only one of the types that participates in the relationship and not on both.</span></span>

<span data-ttu-id="800bd-122">Modeldeki yabancı anahtarlarla eşlenen özellikleri eklemek önerilir.</span><span class="sxs-lookup"><span data-stu-id="800bd-122">It is recommended to include properties in the model that map to foreign keys in the database.</span></span> <span data-ttu-id="800bd-123">Yabancı anahtar özellikleri dahil olmak üzere, bağımlı bir nesne üzerindeki yabancı anahtar değerini değiştirerek bir ilişki oluşturabilir veya değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="800bd-123">With foreign key properties included, you can create or change a relationship by modifying the foreign key value on a dependent object.</span></span> <span data-ttu-id="800bd-124">Bu tür bir ilişkiye yabancı anahtar ilişkilendirmesi denir.</span><span class="sxs-lookup"><span data-stu-id="800bd-124">This kind of association is called a foreign key association.</span></span> <span data-ttu-id="800bd-125">Bağlantısı kesilmiş varlıklarla çalışırken yabancı anahtarların kullanılması daha da önemlidir.</span><span class="sxs-lookup"><span data-stu-id="800bd-125">Using foreign keys is even more essential when working with disconnected entities.</span></span> <span data-ttu-id="800bd-126">1 ile 1 arasında veya 1 ile 0 arasında çalışırken. 1 ilişkiler, ayrı bir yabancı anahtar sütunu yoktur, birincil anahtar özelliği yabancı anahtar olarak davranır ve her zaman modele dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="800bd-126">Note, that when working with 1-to-1 or 1-to-0..1 relationships, there is no separate foreign key column, the primary key property acts as the foreign key and is always included in the model.</span></span>

<span data-ttu-id="800bd-127">Yabancı anahtar sütunları modele dahil edilmediğinde, ilişkilendirme bilgileri bağımsız bir nesne olarak yönetilir.</span><span class="sxs-lookup"><span data-stu-id="800bd-127">When foreign key columns are not included in the model, the association information is managed as an independent object.</span></span> <span data-ttu-id="800bd-128">İlişkiler, yabancı anahtar özellikleri yerine nesne başvuruları aracılığıyla izlenir.</span><span class="sxs-lookup"><span data-stu-id="800bd-128">Relationships are tracked through object references instead of foreign key properties.</span></span> <span data-ttu-id="800bd-129">Bu tür bir ilişkilendirme *bağımsız bir ilişkilendirme*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="800bd-129">This type of association is called an *independent association*.</span></span> <span data-ttu-id="800bd-130">*Bağımsız bir ilişkilendirmeyi* değiştirmek için en yaygın yol, ilişkilendirmede yer alan her varlık için oluşturulan gezinti özelliklerini değiştirmektir.</span><span class="sxs-lookup"><span data-stu-id="800bd-130">The most common way to modify an *independent association* is to modify the navigation properties that are generated for each entity that participates in the association.</span></span>

<span data-ttu-id="800bd-131">Modelinizde bir veya her iki tür ilişkilendirmeyi kullanmayı seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="800bd-131">You can choose to use one or both types of associations in your model.</span></span> <span data-ttu-id="800bd-132">Ancak, yalnızca yabancı anahtarlar içeren bir JOIN tablosu tarafından bağlanan saf çoktan çoğa ilişkiye sahipseniz, EF bu çok-çok ilişkisini yönetmek için bağımsız bir ilişki kullanır.</span><span class="sxs-lookup"><span data-stu-id="800bd-132">However, if you have a pure many-to-many relationship that is connected by a join table that contains only foreign keys, the EF will use an independent association to manage such many-to-many relationship.</span></span>   

<span data-ttu-id="800bd-133">Aşağıdaki görüntüde Entity Framework Designer ile oluşturulmuş kavramsal bir model gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="800bd-133">The following image shows a conceptual model that was created with the Entity Framework Designer.</span></span> <span data-ttu-id="800bd-134">Model bire çok ilişkisine katılan iki varlık içerir.</span><span class="sxs-lookup"><span data-stu-id="800bd-134">The model contains two entities that participate in one-to-many relationship.</span></span> <span data-ttu-id="800bd-135">Her iki varlık de gezinti özelliklerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="800bd-135">Both entities have navigation properties.</span></span> <span data-ttu-id="800bd-136">**Kurs** , bağımlı varlıktır ve **DepartmentID** yabancı anahtar özelliği tanımlanmış.</span><span class="sxs-lookup"><span data-stu-id="800bd-136">**Course** is the dependent entity and has the **DepartmentID** foreign key property defined.</span></span>

![Gezinti özelliklerine sahip departman ve kurs tabloları](~/ef6/media/relationshipefdesigner.png)

<span data-ttu-id="800bd-138">Aşağıdaki kod parçacığı, Code First ile oluşturulmuş modeli gösterir.</span><span class="sxs-lookup"><span data-stu-id="800bd-138">The following code snippet shows the same model that was created with Code First.</span></span>

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
     this.Courses = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a><span data-ttu-id="800bd-139">İlişkileri yapılandırma veya eşleme</span><span class="sxs-lookup"><span data-stu-id="800bd-139">Configuring or mapping relationships</span></span>

<span data-ttu-id="800bd-140">Bu sayfanın geri kalanında ilişkiler kullanılarak verilere erişme ve verileri işleme konuları ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="800bd-140">The rest of this page covers how to access and manipulate data using relationships.</span></span> <span data-ttu-id="800bd-141">Modelinizde ilişkiler ayarlama hakkında daha fazla bilgi için aşağıdaki sayfalara bakın.</span><span class="sxs-lookup"><span data-stu-id="800bd-141">For information on setting up relationships in your model, see the following pages.</span></span>

-   <span data-ttu-id="800bd-142">Code First ilişkilerini yapılandırmak için, bkz. [veri ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md) ve [akıcı API – ilişkiler](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="800bd-142">To configure relationships in Code First, see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>
-   <span data-ttu-id="800bd-143">Entity Framework Designer kullanarak ilişkileri yapılandırmak için, bkz. [EF Designer Ile ilişkiler](~/ef6/modeling/designer/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="800bd-143">To configure relationships using the Entity Framework Designer, see [Relationships with the EF Designer](~/ef6/modeling/designer/relationships.md).</span></span>

## <a name="creating-and-modifying-relationships"></a><span data-ttu-id="800bd-144">İlişki oluşturma ve değiştirme</span><span class="sxs-lookup"><span data-stu-id="800bd-144">Creating and modifying relationships</span></span>

<span data-ttu-id="800bd-145">Bir *yabancı anahtar ilişkisinde*, ilişkiyi değiştirdiğinizde, bağımlı nesnenin durumu, `EntityState.Unchanged` durumu `EntityState.Modified`olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="800bd-145">In a *foreign key association*, when you change the relationship, the state of a dependent object with an `EntityState.Unchanged` state changes to `EntityState.Modified`.</span></span> <span data-ttu-id="800bd-146">Bağımsız bir ilişkide ilişki değiştirildiğinde bağımlı nesnenin durumu güncellemez.</span><span class="sxs-lookup"><span data-stu-id="800bd-146">In an independent relationship, changing the relationship does not update the state of the dependent object.</span></span>

<span data-ttu-id="800bd-147">Aşağıdaki örneklerde, ilişkili nesneleri ilişkilendirmek için yabancı anahtar özelliklerinin ve gezinti özelliklerinin nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="800bd-147">The following examples show how to use the foreign key properties and navigation properties to associate the related objects.</span></span> <span data-ttu-id="800bd-148">Yabancı anahtar ilişkilendirmeleriyle, ilişkileri değiştirmek, oluşturmak veya değiştirmek için her iki yöntemi de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="800bd-148">With foreign key associations, you can use either method to change, create, or modify relationships.</span></span> <span data-ttu-id="800bd-149">Bağımsız İlişkilendirmelerde, yabancı anahtar özelliğini kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="800bd-149">With independent associations, you cannot use the foreign key property.</span></span>

- <span data-ttu-id="800bd-150">Aşağıdaki örnekte olduğu gibi, yabancı anahtar özelliğine yeni bir değer atayarak.</span><span class="sxs-lookup"><span data-stu-id="800bd-150">By assigning a new value to a foreign key property, as in the following example.</span></span>  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- <span data-ttu-id="800bd-151">Aşağıdaki kod, yabancı anahtarı **null**olarak ayarlayarak bir ilişkiyi kaldırır.</span><span class="sxs-lookup"><span data-stu-id="800bd-151">The following code removes a relationship by setting the foreign key to **null**.</span></span> <span data-ttu-id="800bd-152">Yabancı anahtar özelliğinin null yapılabilir olması gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="800bd-152">Note, that the foreign key property must be nullable.</span></span>  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > <span data-ttu-id="800bd-153">Başvuru eklenen durumundaysa (Bu örnekte, kurs nesnesi), SaveChanges çağrılmadan önce başvuru gezintisi özelliği yeni bir nesnenin anahtar değerleriyle eşitlenmez.</span><span class="sxs-lookup"><span data-stu-id="800bd-153">If the reference is in the added state (in this example, the course object), the reference navigation property will not be synchronized with the key values of a new object until SaveChanges is called.</span></span> <span data-ttu-id="800bd-154">Nesne bağlamı, kaydedilmeden eklenen nesneler için kalıcı anahtarlar içermediğinden eşitleme gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="800bd-154">Synchronization does not occur because the object context does not contain permanent keys for added objects until they are saved.</span></span> <span data-ttu-id="800bd-155">Yeni nesneler ilişkisi hemen sonra tam olarak eşitlenmiş olması gerekir, aşağıdaki yöntemlerin birini kullanın.\*</span><span class="sxs-lookup"><span data-stu-id="800bd-155">If you must have new objects fully synchronized as soon as you set the relationship, use one of the following methods.\*</span></span>

- <span data-ttu-id="800bd-156">Bir gezinti özelliğine yeni bir nesne atayarak.</span><span class="sxs-lookup"><span data-stu-id="800bd-156">By assigning a new object to a navigation property.</span></span> <span data-ttu-id="800bd-157">Aşağıdaki kod, kurs ile `department`arasında bir ilişki oluşturur.</span><span class="sxs-lookup"><span data-stu-id="800bd-157">The following code creates a relationship between a course and a `department`.</span></span> <span data-ttu-id="800bd-158">Nesneler bağlama eklenirse, `course` `department.Courses` koleksiyonuna de eklenir ve `course` nesnesindeki karşılık gelen yabancı anahtar özelliği departmanın anahtar özellik değerine ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="800bd-158">If the objects are attached to the context, the `course` is also added to the `department.Courses` collection, and the corresponding foreign key property on the `course` object is set to the key property value of the department.</span></span>  
  ``` csharp
  course.Department = department;
  ```

- <span data-ttu-id="800bd-159">İlişkiyi silmek için, gezinti özelliğini `null`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="800bd-159">To delete the relationship, set the navigation property to `null`.</span></span> <span data-ttu-id="800bd-160">.NET 4,0 tabanlı Entity Framework çalışıyorsanız, null olarak ayarlamadan önce ilgili ucun yüklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="800bd-160">If you are working with Entity Framework that is based on .NET 4.0, then the related end needs to be loaded before you set it to null.</span></span> <span data-ttu-id="800bd-161">Örnek:</span><span class="sxs-lookup"><span data-stu-id="800bd-161">For example:</span></span>   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  <span data-ttu-id="800bd-162">.NET 4,5 ' i temel alan Entity Framework 5,0 ' den başlayarak, ilişkili bitişi yüklemeden ilişkiyi null olarak ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="800bd-162">Starting with Entity Framework 5.0, that is based on .NET 4.5, you can set the relationship to null without loading the related end.</span></span> <span data-ttu-id="800bd-163">Ayrıca, aşağıdaki yöntemi kullanarak geçerli değeri null olarak ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="800bd-163">You can also set the current value to null using the following method.</span></span>   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- <span data-ttu-id="800bd-164">Bir varlık koleksiyonundaki bir nesneyi silerek veya ekleyerek.</span><span class="sxs-lookup"><span data-stu-id="800bd-164">By deleting or adding an object in an entity collection.</span></span> <span data-ttu-id="800bd-165">Örneğin, `department.Courses` koleksiyonuna `Course` türünde bir nesne ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="800bd-165">For example, you can add an object of type `Course` to the `department.Courses` collection.</span></span> <span data-ttu-id="800bd-166">Bu işlem, belirli bir **Kurs** ve belirli bir `department`arasında bir ilişki oluşturur.</span><span class="sxs-lookup"><span data-stu-id="800bd-166">This operation creates a relationship between a particular **course** and a particular `department`.</span></span> <span data-ttu-id="800bd-167">Nesneler içeriğe eklenmişse, **Kurs** nesnesindeki departman başvurusu ve yabancı anahtar özelliği uygun `department`ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="800bd-167">If the objects are attached to the context, the department reference and the foreign key property on the **course** object will be set to the appropriate `department`.</span></span>  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- <span data-ttu-id="800bd-168">İki varlık nesnesi arasında belirtilen ilişkinin durumunu değiştirmek için `ChangeRelationshipState` yöntemini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="800bd-168">By using the `ChangeRelationshipState` method to change the state of the specified relationship between two entity objects.</span></span> <span data-ttu-id="800bd-169">Bu yöntem, N katmanlı uygulamalarla ve *bağımsız bir ilişkilendirmede* (bir yabancı anahtar ilişkisiyle birlikte kullanılamaz) çalışırken yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="800bd-169">This method is most commonly used when working with N-Tier applications and an *independent association* (it cannot be used with a foreign key association).</span></span> <span data-ttu-id="800bd-170">Ayrıca, bu yöntemi kullanmak için aşağıdaki örnekte gösterildiği gibi, `ObjectContext`için öğesini açmalısınız.</span><span class="sxs-lookup"><span data-stu-id="800bd-170">Also, to use this method you must drop down to `ObjectContext`, as shown in the example below.</span></span>  
<span data-ttu-id="800bd-171">Aşağıdaki örnekte, Eğitmenler ve kurslar arasında çoktan çoğa ilişki vardır.</span><span class="sxs-lookup"><span data-stu-id="800bd-171">In the following example, there is a many-to-many relationship between Instructors and Courses.</span></span> <span data-ttu-id="800bd-172">`ChangeRelationshipState` yöntemini çağırarak ve `EntityState.Added` parametresini geçirerek, `SchoolContext` iki nesne arasında bir ilişki eklendiğini bilmesini sağlar:</span><span class="sxs-lookup"><span data-stu-id="800bd-172">Calling the `ChangeRelationshipState` method and passing the `EntityState.Added` parameter, lets the `SchoolContext` know that a relationship has been added between the two objects:</span></span>
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  <span data-ttu-id="800bd-173">Bir ilişki güncelleştiriyorsanız (yalnızca eklemeyi değil), yenisini ekledikten sonra eski ilişkiyi silmeniz gerektiğini unutmayın:</span><span class="sxs-lookup"><span data-stu-id="800bd-173">Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:</span></span>

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a><span data-ttu-id="800bd-174">Yabancı anahtarlar ve gezinti özellikleri arasındaki değişiklikleri eşitleme</span><span class="sxs-lookup"><span data-stu-id="800bd-174">Synchronizing the changes between the foreign keys and navigation properties</span></span>

<span data-ttu-id="800bd-175">Yukarıda açıklanan yöntemlerden birini kullanarak bağlama eklenmiş nesnelerin ilişkisini değiştirdiğinizde Entity Framework yabancı anahtarları, başvuruları ve koleksiyonları eşitlenmiş halde tutmaları gerekir. Entity Framework, proxy 'leri olan POCO varlıklarının bu eşitlemesini (ilişki çözme olarak da bilinir) otomatik olarak yönetir.</span><span class="sxs-lookup"><span data-stu-id="800bd-175">When you change the relationship of the objects attached to the context by using one of the methods described above, Entity Framework needs to keep foreign keys, references, and collections in sync. Entity Framework automatically manages this synchronization (also known as relationship fix-up) for the POCO entities with proxies.</span></span> <span data-ttu-id="800bd-176">Daha fazla bilgi için bkz. [proxy Ile çalışma](~/ef6/fundamentals/proxies.md).</span><span class="sxs-lookup"><span data-stu-id="800bd-176">For more information, see [Working with Proxies](~/ef6/fundamentals/proxies.md).</span></span>

<span data-ttu-id="800bd-177">Proxy 'siz POCO varlıklarını kullanıyorsanız, bağlam içindeki ilişkili nesneleri eşitlemeniz için **DetectChanges** yönteminin çağrıldığından emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="800bd-177">If you are using POCO entities without proxies, you must make sure that the **DetectChanges** method is called to synchronize the related objects in the context.</span></span> <span data-ttu-id="800bd-178">Aşağıdaki API 'Lerin bir **DetectChanges** çağrısını otomatik olarak tetikleyeceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="800bd-178">Note, that the following APIs automatically trigger a **DetectChanges** call.</span></span>

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
-   <span data-ttu-id="800bd-179">`DbSet` bir LINQ sorgusu yürütme</span><span class="sxs-lookup"><span data-stu-id="800bd-179">Executing a LINQ query against a `DbSet`</span></span>

## <a name="loading-related-objects"></a><span data-ttu-id="800bd-180">İlgili nesneler yükleniyor</span><span class="sxs-lookup"><span data-stu-id="800bd-180">Loading related objects</span></span>

<span data-ttu-id="800bd-181">Entity Framework, tanımlı ilişki tarafından döndürülen varlıkla ilişkili varlıkları yüklemek için genellikle gezinti özelliklerini kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="800bd-181">In Entity Framework you commonly use navigation properties to load entities that are related to the returned entity by the defined association.</span></span> <span data-ttu-id="800bd-182">Daha fazla bilgi için bkz. [Ilgili nesneleri yükleme](~/ef6/querying/related-data.md).</span><span class="sxs-lookup"><span data-stu-id="800bd-182">For more information, see [Loading Related Objects](~/ef6/querying/related-data.md).</span></span>

> [!NOTE]
> <span data-ttu-id="800bd-183">Yabancı anahtar ilişkisinde, bağımlı bir nesnenin ilgili bir sonunu yüklediğinizde ilgili nesne, şu anda bellekte olan bağımlı anahtarın yabancı anahtar değerine göre yüklenir:</span><span class="sxs-lookup"><span data-stu-id="800bd-183">In a foreign key association, when you load a related end of a dependent object, the related object will be loaded based on the foreign key value of the dependent that is currently in memory:</span></span>

``` csharp
    // Get the course where currently DepartmentID = 2.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

<span data-ttu-id="800bd-184">Bağımsız bir ilişkide, bağımlı bir nesnenin ilgili ucu, şu anda veritabanında olan yabancı anahtar değerine göre sorgulanır.</span><span class="sxs-lookup"><span data-stu-id="800bd-184">In an independent association, the related end of a dependent object is queried based on the foreign key value that is currently in the database.</span></span> <span data-ttu-id="800bd-185">Ancak, ilişki değiştirildiyse ve bağımlı nesne üzerindeki başvuru özelliği, nesne bağlamına yüklenen farklı bir Principal nesnesine işaret ediyorsa, Entity Framework istemci üzerinde tanımlanan bir ilişki oluşturmayı dener.</span><span class="sxs-lookup"><span data-stu-id="800bd-185">However, if the relationship was modified, and the reference property on the dependent object points to a different principal object that is loaded in the object context, Entity Framework will try to create a relationship as it is defined on the client.</span></span>

## <a name="managing-concurrency"></a><span data-ttu-id="800bd-186">Eşzamanlılığı yönetme</span><span class="sxs-lookup"><span data-stu-id="800bd-186">Managing concurrency</span></span>

<span data-ttu-id="800bd-187">Hem yabancı anahtar hem de bağımsız İlişkilendirmelerde eşzamanlılık denetimleri, modelde tanımlanan varlık anahtarlarına ve diğer varlık özelliklerine göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="800bd-187">In both foreign key and independent associations, concurrency checks are based on the entity keys and other entity properties that are defined in the model.</span></span> <span data-ttu-id="800bd-188">Bir model oluşturmak için EF tasarımcısını kullanırken, özelliğinin eşzamanlılık için denetlenmesi gerektiğini belirtmek üzere `ConcurrencyMode` özniteliğini **fixed** olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="800bd-188">When using the EF Designer to create a model, set the `ConcurrencyMode` attribute to **fixed** to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="800bd-189">Bir modeli tanımlamak için Code First kullanırken, eşzamanlılık için denetlenmesini istediğiniz özelliklerde `ConcurrencyCheck` ek açıklamasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="800bd-189">When using Code First to define a model, use the `ConcurrencyCheck` annotation on properties that you want to be checked for concurrency.</span></span> <span data-ttu-id="800bd-190">Code First ile çalışırken, özelliğin eşzamanlılık için denetlenmesi gerektiğini belirtmek için `TimeStamp` ek açıklamasını de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="800bd-190">When working with Code First you can also use the `TimeStamp` annotation to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="800bd-191">Belirli bir sınıfta yalnızca bir zaman damgası özelliğine sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="800bd-191">You can have only one timestamp property in a given class.</span></span> <span data-ttu-id="800bd-192">Code First, bu özelliği veritabanında null olmayan bir alana eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="800bd-192">Code First maps this property to a non-nullable field in the database.</span></span>

<span data-ttu-id="800bd-193">Eşzamanlılık denetimi ve çözümüne katılan varlıklarla çalışırken her zaman yabancı anahtar ilişkilendirmesini kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="800bd-193">We recommend that you always use the foreign key association when working with entities that participate in concurrency checking and resolution.</span></span>

<span data-ttu-id="800bd-194">Daha fazla bilgi için bkz. [eşzamanlılık çakışmalarını işleme](~/ef6/saving/concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="800bd-194">For more information, see [Handling Concurrency Conflicts](~/ef6/saving/concurrency.md).</span></span>

## <a name="working-with-overlapping-keys"></a><span data-ttu-id="800bd-195">Çakışan anahtarlarla çalışma</span><span class="sxs-lookup"><span data-stu-id="800bd-195">Working with overlapping Keys</span></span>

<span data-ttu-id="800bd-196">Çakışan anahtarlar, anahtardaki bazı özelliklerin aynı zamanda varlıktaki başka bir anahtarın parçası olduğu bileşik anahtarlardır.</span><span class="sxs-lookup"><span data-stu-id="800bd-196">Overlapping keys are composite keys where some properties in the key are also part of another key in the entity.</span></span> <span data-ttu-id="800bd-197">Bağımsız bir ilişkide çakışan bir anahtarınız olamaz.</span><span class="sxs-lookup"><span data-stu-id="800bd-197">You cannot have an overlapping key in an independent association.</span></span> <span data-ttu-id="800bd-198">Çakışan anahtarlar içeren bir yabancı anahtar ilişkilendirmesini değiştirmek için, nesne başvurularını kullanmak yerine yabancı anahtar değerlerini değiştirmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="800bd-198">To change a foreign key association that includes overlapping keys, we recommend that you modify the foreign key values instead of using the object references.</span></span>
