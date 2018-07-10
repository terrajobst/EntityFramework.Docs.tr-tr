---
title: Kod öncelikli kurallar - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
caps.latest.revision: 4
ms.openlocfilehash: b124a034ba900780cc4d7e1354408cd3995e874e
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912063"
---
# <a name="code-first-conventions"></a><span data-ttu-id="34ec1-102">Kod öncelikli kurallar</span><span class="sxs-lookup"><span data-stu-id="34ec1-102">Code First Conventions</span></span>
<span data-ttu-id="34ec1-103">Kod, C# veya Visual Basic .NET sınıflarını kullanarak bir modeli tanımlamak öncelikle sağlar.</span><span class="sxs-lookup"><span data-stu-id="34ec1-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="34ec1-104">Modelin temel şekil kurallarını kullanarak algılandı.</span><span class="sxs-lookup"><span data-stu-id="34ec1-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="34ec1-105">Code First ile çalışırken, sınıf tanımlarını temel alarak bir kavramsal model otomatik olarak yapılandırmak için kullanılan kural kümesi kurallardır.</span><span class="sxs-lookup"><span data-stu-id="34ec1-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="34ec1-106">Kuralları System.Data.Entity.ModelConfiguration.Conventions ad alanında tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="34ec1-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="34ec1-107">Veri ek açıklamaları ya da fluent API'sini kullanarak, modelinizi daha da yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="34ec1-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="34ec1-108">Veri ek açıklamaları ardından ve kuralları fluent API'si üzerinden yapılandırma için öncelik verilir.</span><span class="sxs-lookup"><span data-stu-id="34ec1-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="34ec1-109">Daha fazla bilgi için [veri ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md), [Fluent API'si - ilişkileri](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API'si - türleri ve özellikleri](~/ef6/modeling/code-first/fluent/types-and-properties.md) ve [Fluent API'si ile VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="34ec1-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="34ec1-110">Ayrıntılı kod öncelikli kurallar listesi kullanılabilir [API belgeleri](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="34ec1-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="34ec1-111">Bu konu, Code First tarafından kullanılan kuralları için genel bir bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="34ec1-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="34ec1-112">Bulma türü</span><span class="sxs-lookup"><span data-stu-id="34ec1-112">Type Discovery</span></span>  

<span data-ttu-id="34ec1-113">Code First geliştirme kullanırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan bir .NET Framework sınıf yazarak başlayın.</span><span class="sxs-lookup"><span data-stu-id="34ec1-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="34ec1-114">Sınıf tanımlamanın yanı sıra, ayrıca izin gerekir **DbContext** modele dahil etmek istediğiniz hangi türlerin bildirin.</span><span class="sxs-lookup"><span data-stu-id="34ec1-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="34ec1-115">Bunu yapmak için türetilen bir bağlamı sınıfı tanımlayın **DbContext** ve ortaya çıkaran **olan DB** modelinin bir parçası olmasını istediğiniz türleri için özellikleri.</span><span class="sxs-lookup"><span data-stu-id="34ec1-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="34ec1-116">Kod öncelikle bu türler dahil edilir ve başvurulan türleri farklı bir derlemede tanımlanan bile tüm başvurulan türlerinde çeker.</span><span class="sxs-lookup"><span data-stu-id="34ec1-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="34ec1-117">Türlerinizin bir devralma hiyerarşisinde katılırsanız, tanımlamak için yeterli olan bir **olan DB** temel sınıf olarak aynı derlemede olmaları durumunda özelliği temel sınıf ve türetilen türler için otomatik olarak dahil edilecektir.</span><span class="sxs-lookup"><span data-stu-id="34ec1-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="34ec1-118">Aşağıdaki örnekte, yalnızca bir tane olduğunu **olan DB** tanımlanan özellik **SchoolEntities** sınıfı (**Departmanlar**).</span><span class="sxs-lookup"><span data-stu-id="34ec1-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="34ec1-119">Kod, bulmak ve herhangi başvurulan türleri çekmek için önce bu özelliği kullanır.</span><span class="sxs-lookup"><span data-stu-id="34ec1-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

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

<span data-ttu-id="34ec1-120">Modelden bir türü dışlamak istiyorsanız, kullanın **NotMapped** özniteliği veya **DbModelBuilder.Ignore** fluent API'si.</span><span class="sxs-lookup"><span data-stu-id="34ec1-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="34ec1-121">Birincil anahtar kuralı</span><span class="sxs-lookup"><span data-stu-id="34ec1-121">Primary Key Convention</span></span>  

<span data-ttu-id="34ec1-122">Kod önce bir özellik "ID" (büyük/küçük harfe duyarlı değil) adlı bir özellik bir sınıf veya sınıf adı "ID" tarafından izlenen bir birincil anahtar olduğunu algılar.</span><span class="sxs-lookup"><span data-stu-id="34ec1-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="34ec1-123">Birincil anahtar özelliği türü sayısal veya GUID olacaktır bir kimlik sütunu yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="34ec1-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="34ec1-124">İlişki kuralı</span><span class="sxs-lookup"><span data-stu-id="34ec1-124">Relationship Convention</span></span>  

<span data-ttu-id="34ec1-125">Varlık Çerçevesi'nde, gezinti özellikleri iki varlık türleri arasındaki bir ilişki gitmek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="34ec1-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="34ec1-126">Her nesne içinde katılan her ilişki için bir gezinti özelliği olabilir.</span><span class="sxs-lookup"><span data-stu-id="34ec1-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="34ec1-127">Gezinti özellikleri gidin ve bir başvuru nesnesi döndürerek her iki yönde ilişkileri yönetmenize olanak sağlar (çeşitlilik tek ise ya da sıfır veya bir) veya (çeşitlilik birçok ise) bir koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="34ec1-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="34ec1-128">Kod ilk, türlerinde tanımlanan Gezinti özelliklerine göre ilişkileri algılar.</span><span class="sxs-lookup"><span data-stu-id="34ec1-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="34ec1-129">Gezinme özelliklerinin yanı sıra, yabancı anahtar özelliklerini bağımlı nesneleri temsil eden türleri dahil öneririz.</span><span class="sxs-lookup"><span data-stu-id="34ec1-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="34ec1-130">Yabancı anahtar ilişkisi için asıl birincil anahtar özelliği olarak aynı veri türüne sahip ve aşağıdaki biçimlerden birini izleyen bir ad ile herhangi bir özelliği temsil eder: '\<gezinme özelliği adı\>\<asıl birincil anahtar özelliği adı\>','\<asıl sınıf adı\>\<birincil anahtar özelliği adı\>', veya '\<asıl birincil anahtar özelliği adı\>'.</span><span class="sxs-lookup"><span data-stu-id="34ec1-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="34ec1-131">Birden fazla eşleşme bulunursa, yukarıda listelenen sırayla öncelik verilir.</span><span class="sxs-lookup"><span data-stu-id="34ec1-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="34ec1-132">Yabancı anahtar algılama büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="34ec1-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="34ec1-133">Bir yabancı anahtar özellik algılandığında, Code First, ilişkinin çoğulluk değerinin null atanabilirliği yabancı anahtarı üzerinde göre çıkarır.</span><span class="sxs-lookup"><span data-stu-id="34ec1-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="34ec1-134">Özellik boş değer atanabilir ise ilişki isteğe bağlı olarak kaydedilir; Aksi takdirde ilişki kayıtlı gerektiğinde.</span><span class="sxs-lookup"><span data-stu-id="34ec1-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="34ec1-135">Bağımlı varlıkta bir yabancı anahtar null değilse, ardından Code First art arda silme ilişkisine ayarlar.</span><span class="sxs-lookup"><span data-stu-id="34ec1-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="34ec1-136">Bağımlı varlıkta bir yabancı anahtar null yapılabilir, Code First ayarlı değil art arda silme ilişkisine ve asıl silindiğinde yabancı anahtarı ayarlama null.</span><span class="sxs-lookup"><span data-stu-id="34ec1-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="34ec1-137">Tarafından algılanan davranışı çeşitlilik ve art arda silme kuralını fluent API'sini kullanarak kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="34ec1-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="34ec1-138">Aşağıdaki örnekte, gezinti özellikleri ve yabancı anahtar bölümü ve kursu sınıfları arasındaki ilişkiyi tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="34ec1-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

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
> <span data-ttu-id="34ec1-139">Aynı türleri arasında birden çok ilişki varsa (örneğin, tanımladığınız varsayalım **kişi** ve **kitap** sınıfları, burada **kişi** sınıfı içerir **ReviewedBooks** ve **AuthoredBooks** Gezinti özellikleri ve **kitap** sınıfı içeren **Yazar** ve  **Gözden Geçiren** Gezinti özellikleri) veri ek açıklamaları ya da fluent API'sini kullanarak ilişkileri el ile yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="34ec1-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="34ec1-140">Daha fazla bilgi için [veri ek açıklamaları - ilişkileri](~/ef6/modeling/code-first/data-annotations.md) ve [Fluent API'si - ilişkileri](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="34ec1-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="34ec1-141">Karmaşık türler kuralı</span><span class="sxs-lookup"><span data-stu-id="34ec1-141">Complex Types Convention</span></span>  

<span data-ttu-id="34ec1-142">Code First burada bir birincil anahtar çıkarsanamıyor ve birincil anahtar veri ek açıklamaları veya fluent API'si ile kayıtlı bir sınıf tanımını bulur, türü bir karmaşık tür olarak otomatik olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="34ec1-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="34ec1-143">Karmaşık tür algılama ayrıca türü başvuru varlık türleri ve başka bir tür üzerindeki bir koleksiyona özelliğinden başvurulmuyor özellikleri yok gerektirir.</span><span class="sxs-lookup"><span data-stu-id="34ec1-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="34ec1-144">Aşağıdaki sınıf tanımları verilen Code First, tanım Çıkarsama **ayrıntıları** , birincil anahtar içerdiğinden karmaşık bir türdür.</span><span class="sxs-lookup"><span data-stu-id="34ec1-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

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

## <a name="connection-string-convention"></a><span data-ttu-id="34ec1-145">Bağlantı dizesi kuralı</span><span class="sxs-lookup"><span data-stu-id="34ec1-145">Connection String Convention</span></span>  

<span data-ttu-id="34ec1-146">DbContext bakın kullanılacak bağlantı bulmak için kullandığı kuralları hakkında bilgi edinmek için [bağlantıları ve modelleri](~/ef6/fundamentals/configuring/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="34ec1-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="34ec1-147">Kuralları kaldırılıyor</span><span class="sxs-lookup"><span data-stu-id="34ec1-147">Removing Conventions</span></span>  

<span data-ttu-id="34ec1-148">System.Data.Entity.ModelConfiguration.Conventions ad alanında tanımlanan kuralları kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="34ec1-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="34ec1-149">Aşağıdaki örnek kaldırır **PluralizingTableNameConvention**.</span><span class="sxs-lookup"><span data-stu-id="34ec1-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

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

## <a name="custom-conventions"></a><span data-ttu-id="34ec1-150">Özel kuralları</span><span class="sxs-lookup"><span data-stu-id="34ec1-150">Custom Conventions</span></span>  

<span data-ttu-id="34ec1-151">Özel kuralları içinde EF6 ve sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="34ec1-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="34ec1-152">Daha fazla bilgi için [özel kod öncelikli kurallar](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="34ec1-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
