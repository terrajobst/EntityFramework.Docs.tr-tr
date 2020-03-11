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
# <a name="fluent-api---relationships"></a>Akıcı API-Ilişkiler
> [!NOTE]
> Bu sayfa, Fluent API kullanarak Code First modelinizdeki ilişkileri ayarlama hakkında bilgi sağlar. EF 'teki ilişkiler ve ilişkileri kullanarak verilere erişme ve verileri işleme hakkında genel bilgi için bkz. [ilişkiler & gezinti özellikleri](~/ef6/fundamentals/relationships.md).  

Code First ile çalışırken, etki alanı CLR sınıflarınızı tanımlayarak modelinizi tanımlarsınız. Varsayılan olarak, Entity Framework sınıflarınızı veritabanı şemasına eşlemek için Code First kurallarını kullanır. Code First adlandırma kurallarını kullanıyorsanız, çoğu durumda, sınıflarınızda tanımladığınız yabancı anahtarlara ve gezinti özelliklerine göre tablolarınız arasında ilişkiler ayarlamak için Code First güvenebilirsiniz. Sınıflarınızı tanımlarken kuralları izlemeyin veya kuralların çalışma biçimini değiştirmek istiyorsanız, sınıflarınızı yapılandırmak için Fluent API veya veri açıklamalarını kullanarak Code First tablolarınız arasındaki ilişkileri eşleyebilir.  

## <a name="introduction"></a>Giriş  

Fluent API bir ilişki yapılandırırken, EntityTypeConfiguration örneğiyle başlar ve ardından HasRequired, HasOptional veya HasMany yöntemini kullanarak bu varlığın katıldığı ilişki türünü belirtebilirsiniz. HasRequired ve HasOptional yöntemleri başvuru gezintisi özelliğini temsil eden bir lambda ifadesi alır. HasMany yöntemi, koleksiyon gezintisi özelliğini temsil eden bir lambda ifadesi alır. Daha sonra WithRequired, WithOptional ve WithMany yöntemlerini kullanarak bir ters gezinti özelliği yapılandırabilirsiniz. Bu yöntemlerin, bağımsız değişken kullanmayan ve tek yönlü gezintilerle kardinalite belirtmek için kullanılabilecek aşırı yüklemeleri vardır.  

Ardından, HasForeignKey metodunu kullanarak yabancı anahtar özelliklerini yapılandırabilirsiniz. Bu yöntem, yabancı anahtar olarak kullanılacak özelliği temsil eden bir lambda ifadesi alır.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>Gerekli-Isteğe bağlı bir Ilişkiyi yapılandırma (bire-sıfır veya-bir)  

Aşağıdaki örnek, bire sıfır veya-bir ilişkiyi yapılandırır. OfficeAssignment, birincil anahtar ve yabancı anahtar olan Komutctorıd özelliğine sahiptir, çünkü özelliğin adı, birincil anahtarı yapılandırmak için HasKey yönteminin kullanıldığı kuralı takip etmez.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>Her Iki ucunun de gerekli olduğu bir Ilişkiyi yapılandırma (bire bir)  

Çoğu durumda Entity Framework, hangi türün bağımlı olduğunu ve bir ilişkide sorumlu olduğunu belirtebilir. Ancak, ilişkinin her iki ucu de gerektiğinde veya her iki taraf da isteğe bağlı olduğunda Entity Framework bağımlı ve sorumluyu tanımlayamıyor. İlişkinin her iki ucu de gerektiğinde, HasRequired yönteminden sonra WithRequiredPrincipal veya WithRequiredDependent kullanın. İlişkinin her iki ucu isteğe bağlı olduğunda, HasOptional yönteminden sonra WithOptionalPrincipal veya WithOptionalDependent kullanın.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>Çoktan çoğa Ilişki yapılandırma  

Aşağıdaki kod kurs ve eğitmen türleri arasında çoktan çoğa bir ilişki yapılandırır. Aşağıdaki örnekte, bir JOIN tablosu oluşturmak için varsayılan Code First kuralları kullanılır. Sonuç olarak, kurs Seeğitmeni tablosu Course_CourseID ve Instructor_InstructorID sütunları ile oluşturulur.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

Yol tablosu adını ve tablodaki sütunların adlarını belirtmek istiyorsanız, Map metodunu kullanarak ek yapılandırma yapmanız gerekir. Aşağıdaki kod, CourseID ve Komutctorıd sütunları ile Courseeğitmen tablosunu oluşturur.  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a>Tek bir gezinti özelliği ile Ilişki yapılandırma  

Tek yönlü (tek yönlü olarak da adlandırılır) ilişkisi, bir gezinti özelliğinin yalnızca ilişkinin yalnızca birinde, her ikisi üzerinde değil, ne zaman sona ereceğini bir şekilde tanımlandığını de alır Kurala göre Code First her zaman tek yönlü ilişkiyi bire çok olarak yorumlar. Örneğin, yalnızca eğitmen türünde bir gezinti özelliği olan eğitmen ve OfficeAssignment arasında bire bir ilişki istiyorsanız, bu ilişkiyi yapılandırmak için Fluent API kullanmanız gerekir.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>Basamaklı silme etkinleştiriliyor  

Willcascade Deondelete yöntemini kullanarak bir ilişkide basamaklı silme yapılandırabilirsiniz. Bağımlı varlıktaki bir yabancı anahtar null yapılabilir değilse, Code First ilişkide basamaklı silme ayarlar. Bağımlı varlıktaki bir yabancı anahtar null yapılabilir ise, Code First ilişkide basamaklı silme ayarı yapmaz ve asıl öğe silindiğinde yabancı anahtar null olarak ayarlanır.  

Bu basamaklı silme kurallarını kullanarak kaldırabilirsiniz:  

modelBuilder. kurallarınızı. Remove\<Onetomanybasamakdedeleteconvention\>()  
modelBuilder. kuralları. Remove\<Manytomanybasamakdedeleteconvention\>()  

Aşağıdaki kod, ilişkiyi gerekli olacak şekilde yapılandırır ve ardından Cascade silmeyi devre dışı bırakır.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>Bileşik yabancı anahtar yapılandırma  

Departman türündeki birincil anahtar DepartmentID ve ad özelliklerinden oluşur, bölüm için birincil anahtarı ve kurs türlerindeki yabancı anahtarı aşağıdaki gibi yapılandırın:  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Modelde tanımlanmayan bir yabancı anahtarı yeniden adlandırma  

CLR türünde bir yabancı anahtar tanımlamadıysanız, ancak veritabanında olması gereken adı belirtmek istiyorsanız aşağıdakileri yapın:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>Code First kuralına uymayan bir yabancı anahtar adı yapılandırma  

Kurs sınıfındaki yabancı anahtar özelliği DepartmentID yerine SomeDepartmentID olarak adlandırıldıysa, SomeDepartmentID 'nin yabancı anahtar olmasını istediğinizi belirtmek için aşağıdakileri yapmanız gerekir:  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>Örneklerde kullanılan model  

Bu sayfadaki örnekler için aşağıdaki Code First modeli kullanılır.  

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
