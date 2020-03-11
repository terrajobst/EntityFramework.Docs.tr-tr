---
title: Code First kuralları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: 4d03a32db5d84eb37c22617a95005b272172a65d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419292"
---
# <a name="code-first-conventions"></a><span data-ttu-id="66ba6-102">Code First kuralları</span><span class="sxs-lookup"><span data-stu-id="66ba6-102">Code First Conventions</span></span>
<span data-ttu-id="66ba6-103">Code First, veya Visual Basic .NET sınıfları kullanarak C# bir modeli açıklamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="66ba6-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="66ba6-104">Modelin temel şekli, kuralları kullanılarak algılanır.</span><span class="sxs-lookup"><span data-stu-id="66ba6-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="66ba6-105">Kurallar, Code First çalışırken sınıf tanımlarına göre kavramsal bir modeli otomatik olarak yapılandırmak için kullanılan kuralların kümeleridir.</span><span class="sxs-lookup"><span data-stu-id="66ba6-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="66ba6-106">Kurallar System. Data. Entity. ModelConfiguration. kurallara ilişkin ad alanında tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="66ba6-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="66ba6-107">Modelinize veri açıklamalarını veya Fluent API kullanarak daha fazla yapılandırma yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66ba6-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="66ba6-108">Öncelik, Fluent API ve ardından veri ek açıklamaları ve kuralları aracılığıyla yapılandırmaya verilir.</span><span class="sxs-lookup"><span data-stu-id="66ba6-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="66ba6-109">Daha fazla bilgi için bkz. [veri ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md), [akıcı, API ılışkılerı](~/ef6/modeling/code-first/fluent/relationships.md), [akıcı apı türleri & ÖZELLIKLERI](~/ef6/modeling/code-first/fluent/types-and-properties.md) ve [vb.NET ile akıcı API](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="66ba6-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="66ba6-110">[API belgelerinde](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx)Code First kuralların ayrıntılı bir listesi bulunur.</span><span class="sxs-lookup"><span data-stu-id="66ba6-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="66ba6-111">Bu konu, Code First tarafından kullanılan kurallara genel bir bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="66ba6-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="66ba6-112">Tür keşfi</span><span class="sxs-lookup"><span data-stu-id="66ba6-112">Type Discovery</span></span>  

<span data-ttu-id="66ba6-113">Code First geliştirme kullanılırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan .NET Framework sınıfları yazarak başlarsınız.</span><span class="sxs-lookup"><span data-stu-id="66ba6-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="66ba6-114">Sınıfları tanımlamaya ek olarak, **DbContext** 'in modele hangi türleri dahil etmek istediğinizi öğrenmenizi de gerekir.</span><span class="sxs-lookup"><span data-stu-id="66ba6-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="66ba6-115">Bunu yapmak için, **DbContext** 'ten türetilen bir bağlam sınıfı tanımlar ve modelin bir parçası olmasını istediğiniz türler Için **dbset** özelliklerini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="66ba6-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="66ba6-116">Code First, başvurulan türler farklı bir derlemede tanımlansa bile, bu türleri içerir ve başvurulan türleri de alır.</span><span class="sxs-lookup"><span data-stu-id="66ba6-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="66ba6-117">Türleriniz bir devralma hiyerarşisine katılırsanız, temel sınıf için bir **Dbset** özelliği tanımlanması yeterlidir ve türetilmiş türler, temel sınıfla aynı derlemede olmaları durumunda otomatik olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="66ba6-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="66ba6-118">Aşağıdaki örnekte, **Ssınlentities** sınıfında (**Departmanlar**) tanımlanmış tek bir **dbset** özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="66ba6-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="66ba6-119">Code First başvurulan herhangi bir türü bulup çekmek için bu özelliği kullanır.</span><span class="sxs-lookup"><span data-stu-id="66ba6-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

<span data-ttu-id="66ba6-120">Bir türü modelden dışlamak isterseniz **Noteşlenmiş** özniteliği veya **dbmodelbuilder. Ignore** Fluent API kullanın.</span><span class="sxs-lookup"><span data-stu-id="66ba6-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="66ba6-121">Birincil anahtar kuralı</span><span class="sxs-lookup"><span data-stu-id="66ba6-121">Primary Key Convention</span></span>  

<span data-ttu-id="66ba6-122">Bir sınıftaki özellik "ID" (büyük/küçük harfe duyarlı değil) veya sınıf adı "ID" tarafından adlandırılmışsa, bir özelliğin birincil anahtar olduğunu Code First.</span><span class="sxs-lookup"><span data-stu-id="66ba6-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="66ba6-123">Birincil anahtar özelliğinin türü sayısal veya GUID ise, kimlik sütunu olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="66ba6-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="66ba6-124">İlişki kuralı</span><span class="sxs-lookup"><span data-stu-id="66ba6-124">Relationship Convention</span></span>  

<span data-ttu-id="66ba6-125">Entity Framework, gezinti özellikleri iki varlık türü arasında bir ilişkiye gitmek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="66ba6-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="66ba6-126">Her nesnenin katıldığı her ilişki için bir gezinti özelliği olabilir.</span><span class="sxs-lookup"><span data-stu-id="66ba6-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="66ba6-127">Gezinti özellikleri, bir başvuru nesnesi (çoğulluk bir veya sıfır ya da-bir) ya da bir koleksiyon (çokluk çok ise) döndüren her iki yönde ilişkilerde gezinmeniz ve bunları yönetmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="66ba6-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="66ba6-128">Code First, türleriniz üzerinde tanımlanan gezinti özelliklerine göre ilişkilerle ilişkilerler.</span><span class="sxs-lookup"><span data-stu-id="66ba6-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="66ba6-129">Gezinti özelliklerine ek olarak, bağımlı nesneleri temsil eden türlere yabancı anahtar özellikleri dahil etmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="66ba6-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="66ba6-130">Asıl birincil anahtar özelliği ile aynı veri türüne ve aşağıdaki biçimlerden birini izleyen bir ada sahip herhangi bir özellik, ilişki için bir yabancı anahtarı temsil eder: '\<gezinti özelliği adı\>\<asıl birincil anahtar özellik adı\>', '\<birincil anahtar özellik adı\>' veya ' \<asıl birincil anahtar özellik adı\>'.\<\></span><span class="sxs-lookup"><span data-stu-id="66ba6-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="66ba6-131">Birden fazla eşleşme bulunursa, öncelik yukarıda belirtilen sırada verilir.</span><span class="sxs-lookup"><span data-stu-id="66ba6-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="66ba6-132">Yabancı anahtar algılama, büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="66ba6-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="66ba6-133">Yabancı anahtar özelliği algılandığında, Code First yabancı anahtarın null değer alabilirliğine göre ilişkinin çokluğunu alır.</span><span class="sxs-lookup"><span data-stu-id="66ba6-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="66ba6-134">Özellik null yapılabilir ise, ilişki isteğe bağlı olarak kaydedilir; Aksi takdirde, ilişki gerektiği şekilde kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="66ba6-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="66ba6-135">Bağımlı varlıktaki bir yabancı anahtar null yapılabilir değilse, Code First ilişkide basamaklı silme ayarlar.</span><span class="sxs-lookup"><span data-stu-id="66ba6-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="66ba6-136">Bağımlı varlıktaki bir yabancı anahtar null yapılabilir ise, Code First ilişkide basamaklı silme ayarı yapmaz ve asıl öğe silindiğinde yabancı anahtar null olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="66ba6-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="66ba6-137">Kural tarafından algılanan çoğulluk ve basamaklı silme davranışı, Fluent API kullanılarak geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="66ba6-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="66ba6-138">Aşağıdaki örnekte, bölüm ve kurs sınıfları arasındaki ilişkiyi tanımlamak için gezinti özellikleri ve yabancı anahtar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="66ba6-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> <span data-ttu-id="66ba6-139">Aynı türler arasında birden çok ilişki varsa (örneğin, **kişi** sınıfının **Reviedilimlerin** **ve** **Authoredbooks** gezinti özelliklerini içerdiğini ve **kitap** sınıfı **Yazar** ve **Gözden geçiren** gezinti özelliklerini içeriyorsa), veri açıklamalarını veya Fluent API kullanarak ilişkileri el ile yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="66ba6-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="66ba6-140">Daha fazla bilgi için bkz. [veri ek açıklamaları-ilişkiler](~/ef6/modeling/code-first/data-annotations.md) ve [floent API-Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="66ba6-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="66ba6-141">Karmaşık türler kuralı</span><span class="sxs-lookup"><span data-stu-id="66ba6-141">Complex Types Convention</span></span>  

<span data-ttu-id="66ba6-142">Code First birincil anahtarın çıkarsanmayacak bir sınıf tanımını bulduğunda ve veri ek açıklamaları veya Fluent API ile hiçbir birincil anahtar kaydedilmemişse, tür otomatik olarak karmaşık bir tür olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="66ba6-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="66ba6-143">Karmaşık tür algılama ayrıca türün varlık türlerine başvuran özelliklere sahip olmaması ve başka bir türdeki bir koleksiyon özelliğinden başvurulmaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="66ba6-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="66ba6-144">Aşağıdaki sınıf tanımları verildiğinde Code First, birincil anahtarı olmadığından **ayrıntıların** karmaşık bir tür olduğunu çıkarırdı.</span><span class="sxs-lookup"><span data-stu-id="66ba6-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

``` csharp
public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}
```  

## <a name="connection-string-convention"></a><span data-ttu-id="66ba6-145">Bağlantı dizesi kuralı</span><span class="sxs-lookup"><span data-stu-id="66ba6-145">Connection String Convention</span></span>  

<span data-ttu-id="66ba6-146">DbContext 'in kullanım bağlantısını bulması için kullandığı kurallar hakkında bilgi edinmek için bkz. [Bağlantılar ve modeller](~/ef6/fundamentals/configuring/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="66ba6-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="66ba6-147">Kuralları kaldırma</span><span class="sxs-lookup"><span data-stu-id="66ba6-147">Removing Conventions</span></span>  

<span data-ttu-id="66ba6-148">System. Data. Entity. ModelConfiguration. kurallara ilişkin ad alanında tanımlanan kurallardan herhangi birini kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66ba6-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="66ba6-149">Aşağıdaki örnek **Pluralizingtablenameconvention**'ı kaldırır.</span><span class="sxs-lookup"><span data-stu-id="66ba6-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a><span data-ttu-id="66ba6-150">Özel Kurallar</span><span class="sxs-lookup"><span data-stu-id="66ba6-150">Custom Conventions</span></span>  

<span data-ttu-id="66ba6-151">Özel kurallar EF6 ve sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="66ba6-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="66ba6-152">Daha fazla bilgi için bkz. [özel Code First kuralları](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="66ba6-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
