---
title: Akıcı API-Ilişkiler-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: 05f282c02699f8bf3c71197ac5e01000f1855917
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419075"
---
# <a name="fluent-api---relationships"></a><span data-ttu-id="c0e7a-102">Akıcı API-Ilişkiler</span><span class="sxs-lookup"><span data-stu-id="c0e7a-102">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="c0e7a-103">Bu sayfa, Fluent API kullanarak Code First modelinizdeki ilişkileri ayarlama hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-103">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="c0e7a-104">EF 'teki ilişkiler ve ilişkileri kullanarak verilere erişme ve verileri işleme hakkında genel bilgi için bkz. [ilişkiler & gezinti özellikleri](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="c0e7a-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>  

<span data-ttu-id="c0e7a-105">Code First ile çalışırken, etki alanı CLR sınıflarınızı tanımlayarak modelinizi tanımlarsınız.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-105">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="c0e7a-106">Varsayılan olarak, Entity Framework sınıflarınızı veritabanı şemasına eşlemek için Code First kurallarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-106">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="c0e7a-107">Code First adlandırma kurallarını kullanıyorsanız, çoğu durumda, sınıflarınızda tanımladığınız yabancı anahtarlara ve gezinti özelliklerine göre tablolarınız arasında ilişkiler ayarlamak için Code First güvenebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-107">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="c0e7a-108">Sınıflarınızı tanımlarken kuralları izlemeyin veya kuralların çalışma biçimini değiştirmek istiyorsanız, sınıflarınızı yapılandırmak için Fluent API veya veri açıklamalarını kullanarak Code First tablolarınız arasındaki ilişkileri eşleyebilir.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-108">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="c0e7a-109">Giriş</span><span class="sxs-lookup"><span data-stu-id="c0e7a-109">Introduction</span></span>  

<span data-ttu-id="c0e7a-110">Fluent API bir ilişki yapılandırırken, EntityTypeConfiguration örneğiyle başlar ve ardından HasRequired, HasOptional veya HasMany yöntemini kullanarak bu varlığın katıldığı ilişki türünü belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-110">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="c0e7a-111">HasRequired ve HasOptional yöntemleri başvuru gezintisi özelliğini temsil eden bir lambda ifadesi alır.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-111">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="c0e7a-112">HasMany yöntemi, koleksiyon gezintisi özelliğini temsil eden bir lambda ifadesi alır.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-112">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="c0e7a-113">Daha sonra WithRequired, WithOptional ve WithMany yöntemlerini kullanarak bir ters gezinti özelliği yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-113">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="c0e7a-114">Bu yöntemlerin, bağımsız değişken kullanmayan ve tek yönlü gezintilerle kardinalite belirtmek için kullanılabilecek aşırı yüklemeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-114">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="c0e7a-115">Ardından, HasForeignKey metodunu kullanarak yabancı anahtar özelliklerini yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-115">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="c0e7a-116">Bu yöntem, yabancı anahtar olarak kullanılacak özelliği temsil eden bir lambda ifadesi alır.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-116">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="c0e7a-117">Gerekli-Isteğe bağlı bir Ilişkiyi yapılandırma (bire-sıfır veya-bir)</span><span class="sxs-lookup"><span data-stu-id="c0e7a-117">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="c0e7a-118">Aşağıdaki örnek, bire sıfır veya-bir ilişkiyi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-118">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="c0e7a-119">OfficeAssignment, birincil anahtar ve yabancı anahtar olan Komutctorıd özelliğine sahiptir, çünkü özelliğin adı, birincil anahtarı yapılandırmak için HasKey yönteminin kullanıldığı kuralı takip etmez.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-119">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="c0e7a-120">Her Iki ucunun de gerekli olduğu bir Ilişkiyi yapılandırma (bire bir)</span><span class="sxs-lookup"><span data-stu-id="c0e7a-120">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="c0e7a-121">Çoğu durumda Entity Framework, hangi türün bağımlı olduğunu ve bir ilişkide sorumlu olduğunu belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-121">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="c0e7a-122">Ancak, ilişkinin her iki ucu de gerektiğinde veya her iki taraf da isteğe bağlı olduğunda Entity Framework bağımlı ve sorumluyu tanımlayamıyor.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-122">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="c0e7a-123">İlişkinin her iki ucu de gerektiğinde, HasRequired yönteminden sonra WithRequiredPrincipal veya WithRequiredDependent kullanın.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-123">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="c0e7a-124">İlişkinin her iki ucu isteğe bağlı olduğunda, HasOptional yönteminden sonra WithOptionalPrincipal veya WithOptionalDependent kullanın.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-124">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="c0e7a-125">Çoktan çoğa Ilişki yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c0e7a-125">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="c0e7a-126">Aşağıdaki kod kurs ve eğitmen türleri arasında çoktan çoğa bir ilişki yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-126">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="c0e7a-127">Aşağıdaki örnekte, bir JOIN tablosu oluşturmak için varsayılan Code First kuralları kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-127">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="c0e7a-128">Sonuç olarak, kurs Seeğitmeni tablosu Course_CourseID ve Instructor_InstructorID sütunları ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-128">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="c0e7a-129">Yol tablosu adını ve tablodaki sütunların adlarını belirtmek istiyorsanız, Map metodunu kullanarak ek yapılandırma yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-129">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="c0e7a-130">Aşağıdaki kod, CourseID ve Komutctorıd sütunları ile Courseeğitmen tablosunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-130">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="c0e7a-131">Tek bir gezinti özelliği ile Ilişki yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c0e7a-131">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="c0e7a-132">Tek yönlü (tek yönlü olarak da adlandırılır) ilişkisi, bir gezinti özelliğinin yalnızca ilişkinin yalnızca birinde, her ikisi üzerinde değil, ne zaman sona ereceğini bir şekilde tanımlandığını de alır</span><span class="sxs-lookup"><span data-stu-id="c0e7a-132">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="c0e7a-133">Kurala göre Code First her zaman tek yönlü ilişkiyi bire çok olarak yorumlar.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-133">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="c0e7a-134">Örneğin, yalnızca eğitmen türünde bir gezinti özelliği olan eğitmen ve OfficeAssignment arasında bire bir ilişki istiyorsanız, bu ilişkiyi yapılandırmak için Fluent API kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-134">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="c0e7a-135">Basamaklı silme etkinleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="c0e7a-135">Enabling Cascade Delete</span></span>  

<span data-ttu-id="c0e7a-136">Willcascade Deondelete yöntemini kullanarak bir ilişkide basamaklı silme yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-136">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="c0e7a-137">Bağımlı varlıktaki bir yabancı anahtar null yapılabilir değilse, Code First ilişkide basamaklı silme ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-137">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="c0e7a-138">Bağımlı varlıktaki bir yabancı anahtar null yapılabilir ise, Code First ilişkide basamaklı silme ayarı yapmaz ve asıl öğe silindiğinde yabancı anahtar null olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-138">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="c0e7a-139">Bu basamaklı silme kurallarını kullanarak kaldırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c0e7a-139">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="c0e7a-140">modelBuilder. kurallarınızı. Remove\<Onetomanybasamakdedeleteconvention\>()</span><span class="sxs-lookup"><span data-stu-id="c0e7a-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="c0e7a-141">modelBuilder. kuralları. Remove\<Manytomanybasamakdedeleteconvention\>()</span><span class="sxs-lookup"><span data-stu-id="c0e7a-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="c0e7a-142">Aşağıdaki kod, ilişkiyi gerekli olacak şekilde yapılandırır ve ardından Cascade silmeyi devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-142">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="c0e7a-143">Bileşik yabancı anahtar yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c0e7a-143">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="c0e7a-144">Departman türündeki birincil anahtar DepartmentID ve ad özelliklerinden oluşur, bölüm için birincil anahtarı ve kurs türlerindeki yabancı anahtarı aşağıdaki gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="c0e7a-144">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="c0e7a-145">Modelde tanımlanmayan bir yabancı anahtarı yeniden adlandırma</span><span class="sxs-lookup"><span data-stu-id="c0e7a-145">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="c0e7a-146">CLR türünde bir yabancı anahtar tanımlamadıysanız, ancak veritabanında olması gereken adı belirtmek istiyorsanız aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="c0e7a-146">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="c0e7a-147">Code First kuralına uymayan bir yabancı anahtar adı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c0e7a-147">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="c0e7a-148">Kurs sınıfındaki yabancı anahtar özelliği DepartmentID yerine SomeDepartmentID olarak adlandırıldıysa, SomeDepartmentID 'nin yabancı anahtar olmasını istediğinizi belirtmek için aşağıdakileri yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="c0e7a-148">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="c0e7a-149">Örneklerde kullanılan model</span><span class="sxs-lookup"><span data-stu-id="c0e7a-149">Model Used in Samples</span></span>  

<span data-ttu-id="c0e7a-150">Bu sayfadaki örnekler için aşağıdaki Code First modeli kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c0e7a-150">The following Code First model is used for the samples on this page.</span></span>  

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
