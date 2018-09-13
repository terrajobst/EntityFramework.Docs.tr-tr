---
title: Fluent API'si - ilişkileri - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: 05f282c02699f8bf3c71197ac5e01000f1855917
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490473"
---
# <a name="fluent-api---relationships"></a><span data-ttu-id="04122-102">Fluent API'si - ilişkileri</span><span class="sxs-lookup"><span data-stu-id="04122-102">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="04122-103">Bu sayfa fluent API'sini kullanarak, Code First modeli ilişkileri ayarlama hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="04122-103">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="04122-104">EF ve erişmek ve ilişkileri kullanarak verileri işlemek nasıl ilişkiler hakkında genel bilgi için bkz. [ilişkileri ve gezinti özellikleri](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="04122-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>  

<span data-ttu-id="04122-105">Code First ile çalışırken, etki alanı CLR sınıflarını tanımlayarak modelinizi tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="04122-105">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="04122-106">Varsayılan olarak Entity Framework veritabanı şemasına sınıflarınızı eşlemek için kod öncelikli kurallar kullanır.</span><span class="sxs-lookup"><span data-stu-id="04122-106">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="04122-107">Code First adlandırma kurallarını kullanırsanız, çoğu durumda, ilk temel sınıflarında tanımlayan Gezinti özellikleri ve yabancı anahtarlar, tablolar arasında ilişki kurmak için kod üzerinde güvenebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04122-107">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="04122-108">Sınıfları tanımlarken kurallara uymayan veya kuralları biçimini değiştirmek istiyorsanız iş, Code First, tablolar arasındaki ilişkileri eşleyebilmeniz sınıflarınızı yapılandırmak için veri ek açıklamaları ya da fluent API'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04122-108">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="04122-109">Giriş</span><span class="sxs-lookup"><span data-stu-id="04122-109">Introduction</span></span>  

<span data-ttu-id="04122-110">Fluent API'si ile bir ilişki yapılandırırken EntityTypeConfiguration örneğiyle başlatın ve sonra bu varlığın katıldığı ilişki türünü belirtmek için HasRequired, HasOptional veya çok yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="04122-110">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="04122-111">HasRequired ve HasOptional yöntemleri, bir başvuru gezinme özelliğini temsil eden bir lambda ifadesi alır.</span><span class="sxs-lookup"><span data-stu-id="04122-111">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="04122-112">Çok yöntem bir koleksiyon gezinme özelliği temsil eden bir lambda ifadesi alır.</span><span class="sxs-lookup"><span data-stu-id="04122-112">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="04122-113">Ters gezinti özelliği WithRequired WithOptional ve WithMany yöntemlerini kullanarak daha sonra yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04122-113">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="04122-114">Bu yöntemleri ile tek yönlü gezintiler kardinalite belirtmek için kullanılabilir ve bağımsız değişkenler almayan aşırı yüklemeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="04122-114">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="04122-115">Ardından, yabancı anahtar özelliklerini HasForeignKey yöntemi kullanarak yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04122-115">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="04122-116">Bu yöntem, yabancı anahtar olarak kullanılacak özelliği temsil eden bir lambda ifadesi alır.</span><span class="sxs-lookup"><span data-stu-id="04122-116">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="04122-117">Gereklidir-için-isteğe bağlı bir ilişki (-bire-sıfır-veya-bir) yapılandırma</span><span class="sxs-lookup"><span data-stu-id="04122-117">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="04122-118">Aşağıdaki örnek, bir sıfır-veya-bire bir ilişki yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="04122-118">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="04122-119">Özelliğin adı HasKey yöntemi birincil anahtarı yapılandırmak için kullanılan kuralı izlemiyor çünkü OfficeAssignment birincil anahtar ve yabancı anahtar Instructorıd özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="04122-119">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="04122-120">Burada ikisinde (bire) gerekli bir ilişki yapılandırma</span><span class="sxs-lookup"><span data-stu-id="04122-120">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="04122-121">Çoğu durumda, hangi tür bağımlı ve asıl ilişkisinde olduğu Entity Framework çıkarabilir.</span><span class="sxs-lookup"><span data-stu-id="04122-121">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="04122-122">Bağımlı ve asıl ancak zaman hem ilişkinin uçlarından gerekli ya da her iki tarafında da isteğe bağlı Entity Framework tanımlanamıyor.</span><span class="sxs-lookup"><span data-stu-id="04122-122">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="04122-123">Her iki ucunda da ilişki gerekli olduğunda WithRequiredPrincipal veya WithRequiredDependent sonra HasRequired yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="04122-123">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="04122-124">Her iki ucunda da ilişki isteğe bağlı olduğunuzda WithOptionalPrincipal veya WithOptionalDependent sonra HasOptional yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="04122-124">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="04122-125">Çoktan çoğa ilişki yapılandırma</span><span class="sxs-lookup"><span data-stu-id="04122-125">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="04122-126">Aşağıdaki kod kurs ve Eğitmenler türü arasında bir çoktan çoğa ilişki yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="04122-126">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="04122-127">Aşağıdaki örnekte, varsayılan kod öncelikli kurallar, bir birleştirme tablo oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="04122-127">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="04122-128">Sonuç olarak CourseInstructor tablo Course_CourseID ve Instructor_InstructorID sütunlarla oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="04122-128">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="04122-129">Eşleme yöntemini kullanarak ek yapılandırma gerçekleştirmeniz gereken tabloda birleştirme tablo adını ve sütunlarının adlarını belirtmek istiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="04122-129">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="04122-130">Aşağıdaki kod CourseID ve Instructorıd sütunlar içeren CourseInstructor tablo oluşturur.</span><span class="sxs-lookup"><span data-stu-id="04122-130">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
    .Map(m =>
    {
        m.ToTable("CourseInstructor");
        m.MapLeftKey("CourseID");
        m.MapRightKey("InstructorID");
    });
```  

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="04122-131">Bir gezinti özelliği ile bir ilişki yapılandırma</span><span class="sxs-lookup"><span data-stu-id="04122-131">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="04122-132">Tek yönlü (tek yönlü da denir) ilişkidir bir gezinti özelliği yalnızca bir ilişkinin uçlarından hem de her ikisi de olarak tanımlandığında.</span><span class="sxs-lookup"><span data-stu-id="04122-132">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="04122-133">Kural gereği, Code First her zaman tek yönlü bir ilişki tek-çok olarak yorumlar.</span><span class="sxs-lookup"><span data-stu-id="04122-133">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="04122-134">Örneğin, eğitmen ve bir gezinti özelliği yalnızca Eğitmen türüne sahip olduğu, OfficeAssignment, arasında bire bir ilişki istiyorsanız, bu ilişkiyi yapılandırmak için fluent API'sini kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="04122-134">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="04122-135">Art arda silme işlevini etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="04122-135">Enabling Cascade Delete</span></span>  

<span data-ttu-id="04122-136">Art arda silme WillCascadeOnDelete yöntemi kullanarak, bir ilişkiye yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04122-136">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="04122-137">Bağımlı varlıkta bir yabancı anahtar null değilse, ardından Code First art arda silme ilişkisine ayarlar.</span><span class="sxs-lookup"><span data-stu-id="04122-137">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="04122-138">Bağımlı varlıkta bir yabancı anahtar null yapılabilir, Code First ayarlı değil art arda silme ilişkisine ve asıl silindiğinde yabancı anahtarı ayarlama null.</span><span class="sxs-lookup"><span data-stu-id="04122-138">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="04122-139">Bu art arda silme kuralları kullanarak kaldırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="04122-139">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="04122-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)</span><span class="sxs-lookup"><span data-stu-id="04122-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="04122-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)</span><span class="sxs-lookup"><span data-stu-id="04122-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="04122-142">Aşağıdaki kod, gerekli bir ilişki yapılandırır ve art arda silme işlevini devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="04122-142">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="04122-143">Bileşik bir yabancı anahtar yapılandırma</span><span class="sxs-lookup"><span data-stu-id="04122-143">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="04122-144">Birincil anahtar bölüm türüne DepartmentID ve ad özelliklerini oluşuyorsa, birincil anahtar bölüm ve yabancı anahtarı Kurs türleri gibi yapılandırabileceğinizi gösteren:</span><span class="sxs-lookup"><span data-stu-id="04122-144">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

``` csharp
// Composite primary key
modelBuilder.Entity<Department>()
.HasKey(d => new { d.DepartmentID, d.Name });

// Composite foreign key
modelBuilder.Entity<Course>()  
    .HasRequired(c => c.Department)  
    .WithMany(d => d.Courses)
    .HasForeignKey(d => new { d.DepartmentID, d.DepartmentName });
```  

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="04122-145">Modelde tanımlı olmayan bir yabancı anahtar yeniden adlandırma</span><span class="sxs-lookup"><span data-stu-id="04122-145">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="04122-146">Değil CLR türüne bir yabancı anahtar tanımlayın, ancak veritabanında olması gereken hangi adını belirtmek istediğiniz seçerseniz, aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="04122-146">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="04122-147">Kod ilk kuralı izlemez bir yabancı anahtar adı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="04122-147">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="04122-148">Yabancı anahtar özelliği kurs sınıfındaki SomeDepartmentID DepartmentID yerine çağrılırsa SomeDepartmentID yabancı anahtarı olmasını istediğinizi belirtmek için aşağıdakileri yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="04122-148">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="04122-149">Örneklerde kullanılan modeli</span><span class="sxs-lookup"><span data-stu-id="04122-149">Model Used in Samples</span></span>  

<span data-ttu-id="04122-150">Bu sayfadaki örnekler için aşağıdaki Code First modeli kullanılır.</span><span class="sxs-lookup"><span data-stu-id="04122-150">The following Code First model is used for the samples on this page.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.ModelConfiguration.Conventions;
// add a reference to System.ComponentModel.DataAnnotations DLL
using System.ComponentModel.DataAnnotations;
using System.Collections.Generic;
using System;

public class SchoolEntities : DbContext
{
    public DbSet<Course> Courses { get; set; }
    public DbSet<Department> Departments { get; set; }
    public DbSet<Instructor> Instructors { get; set; }
    public DbSet<OfficeAssignment> OfficeAssignments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention then the generated tables will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}

public class Department
{
    public Department()
    {
        this.Courses = new HashSet<Course>();
    }
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }
    public decimal Budget { get; set; }
    public System.DateTime StartDate { get; set; }
    public int? Administrator { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; private set; }
}

public class Course
{
    public Course()
    {
        this.Instructors = new HashSet<Instructor>();
    }
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
    public virtual ICollection<Instructor> Instructors { get; private set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

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

public class Instructor
{
    public Instructor()
    {
        this.Courses = new List<Course>();
    }

    // Primary key
    public int InstructorID { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
    public System.DateTime HireDate { get; set; }

    // Navigation properties
    public virtual ICollection<Course> Courses { get; private set; }
}

public class OfficeAssignment
{
    // Specifying InstructorID as a primary
    [Key()]
    public Int32 InstructorID { get; set; }

    public string Location { get; set; }

    // When Entity Framework sees Timestamp attribute
    // it configures ConcurrencyCheck and DatabaseGeneratedPattern=Computed.
    [Timestamp]
    public Byte[] Timestamp { get; set; }

    // Navigation property
    public virtual Instructor Instructor { get; set; }
}
```  
