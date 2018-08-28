---
title: Fluent API'si - ilişkileri - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: e13a1370f46362e0f2ca3743ec5b063ce6f87cbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994768"
---
# <a name="fluent-api---relationships"></a>Fluent API'si - ilişkileri
> [!NOTE]
> Bu sayfa fluent API'sini kullanarak, Code First modeli ilişkileri ayarlama hakkında bilgi sağlar. EF ve erişmek ve ilişkileri kullanarak verileri işlemek nasıl ilişkiler hakkında genel bilgi için bkz. [ilişkileri ve gezinti özellikleri](~/ef6/fundamentals/relationships.md).  

Code First ile çalışırken, etki alanı CLR sınıflarını tanımlayarak modelinizi tanımlayın. Varsayılan olarak Entity Framework veritabanı şemasına sınıflarınızı eşlemek için kod öncelikli kurallar kullanır. Code First adlandırma kurallarını kullanırsanız, çoğu durumda, ilk temel sınıflarında tanımlayan Gezinti özellikleri ve yabancı anahtarlar, tablolar arasında ilişki kurmak için kod üzerinde güvenebilirsiniz. Sınıfları tanımlarken kurallara uymayan veya kuralları biçimini değiştirmek istiyorsanız iş, Code First, tablolar arasındaki ilişkileri eşleyebilmeniz sınıflarınızı yapılandırmak için veri ek açıklamaları ya da fluent API'sini kullanabilirsiniz.  

## <a name="introduction"></a>Giriş  

Fluent API'si ile bir ilişki yapılandırırken EntityTypeConfiguration örneğiyle başlatın ve sonra bu varlığın katıldığı ilişki türünü belirtmek için HasRequired, HasOptional veya çok yöntemini kullanın. HasRequired ve HasOptional yöntemleri, bir başvuru gezinme özelliğini temsil eden bir lambda ifadesi alır. Çok yöntem bir koleksiyon gezinme özelliği temsil eden bir lambda ifadesi alır. Ters gezinti özelliği WithRequired WithOptional ve WithMany yöntemlerini kullanarak daha sonra yapılandırabilirsiniz. Bu yöntemleri ile tek yönlü gezintiler kardinalite belirtmek için kullanılabilir ve bağımsız değişkenler almayan aşırı yüklemeleri vardır.  

Ardından, yabancı anahtar özelliklerini HasForeignKey yöntemi kullanarak yapılandırabilirsiniz. Bu yöntem, yabancı anahtar olarak kullanılacak özelliği temsil eden bir lambda ifadesi alır.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>Gereklidir-için-isteğe bağlı bir ilişki (-bire-sıfır-veya-bir) yapılandırma  

Aşağıdaki örnek, bir sıfır-veya-bire bir ilişki yapılandırır. Özelliğin adı HasKey yöntemi birincil anahtarı yapılandırmak için kullanılan kuralı izlemiyor çünkü OfficeAssignment birincil anahtar ve yabancı anahtar Instructorıd özelliği vardır.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>Burada ikisinde (bire) gerekli bir ilişki yapılandırma  

Çoğu durumda, hangi tür bağımlı ve asıl ilişkisinde olduğu Entity Framework çıkarabilir. Bağımlı ve asıl ancak zaman hem ilişkinin uçlarından gerekli ya da her iki tarafında da isteğe bağlı Entity Framework tanımlanamıyor. Her iki ucunda da ilişki gerekli olduğunda WithRequiredPrincipal veya WithRequiredDependent sonra HasRequired yöntemi kullanın. Her iki ucunda da ilişki isteğe bağlı olduğunuzda WithOptionalPrincipal veya WithOptionalDependent sonra HasOptional yöntemi kullanın.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>Çoktan çoğa ilişki yapılandırma  

Aşağıdaki kod kurs ve Eğitmenler türü arasında bir çoktan çoğa ilişki yapılandırır. Aşağıdaki örnekte, varsayılan kod öncelikli kurallar, bir birleştirme tablo oluşturmak için kullanılır. Sonuç olarak CourseInstructor tablo Course_CourseID ve Instructor_InstructorID sütunlarla oluşturulur.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

Eşleme yöntemini kullanarak ek yapılandırma gerçekleştirmeniz gereken tabloda birleştirme tablo adını ve sütunlarının adlarını belirtmek istiyorsanız. Aşağıdaki kod CourseID ve Instructorıd sütunlar içeren CourseInstructor tablo oluşturur.  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a>Bir gezinti özelliği ile bir ilişki yapılandırma  

Tek yönlü (tek yönlü da denir) ilişkidir bir gezinti özelliği yalnızca bir ilişkinin uçlarından hem de her ikisi de olarak tanımlandığında. Kural gereği, Code First her zaman tek yönlü bir ilişki tek-çok olarak yorumlar. Örneğin, eğitmen ve bir gezinti özelliği yalnızca Eğitmen türüne sahip olduğu, OfficeAssignment, arasında bire bir ilişki istiyorsanız, bu ilişkiyi yapılandırmak için fluent API'sini kullanmanız gerekir.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>Art arda silme işlevini etkinleştirme  

Art arda silme WillCascadeOnDelete yöntemi kullanarak, bir ilişkiye yapılandırabilirsiniz. Bağımlı varlıkta bir yabancı anahtar null değilse, ardından Code First art arda silme ilişkisine ayarlar. Bağımlı varlıkta bir yabancı anahtar null yapılabilir, Code First ayarlı değil art arda silme ilişkisine ve asıl silindiğinde yabancı anahtarı ayarlama null.  

Bu art arda silme kuralları kullanarak kaldırabilirsiniz:  

modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)  
modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)  

Aşağıdaki kod, gerekli bir ilişki yapılandırır ve art arda silme işlevini devre dışı bırakır.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>Bileşik bir yabancı anahtar yapılandırma  

Birincil anahtar bölüm türüne DepartmentID ve ad özelliklerini oluşuyorsa, birincil anahtar bölüm ve yabancı anahtarı Kurs türleri gibi yapılandırabileceğinizi gösteren:  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Modelde tanımlı olmayan bir yabancı anahtar yeniden adlandırma  

Değil CLR türüne bir yabancı anahtar tanımlayın, ancak veritabanında olması gereken hangi adını belirtmek istediğiniz seçerseniz, aşağıdakileri yapın:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>Kod ilk kuralı izlemez bir yabancı anahtar adı yapılandırma  

Yabancı anahtar özelliği kurs sınıfındaki SomeDepartmentID DepartmentID yerine çağrılırsa SomeDepartmentID yabancı anahtarı olmasını istediğinizi belirtmek için aşağıdakileri yapmanız gerekir:  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>Örneklerde kullanılan modeli  

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
