---
title: -Yapılandırma ve özellikler ve türler eşleme - Fluent API'si EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 031376d2fc4778e6f0fa2434ab7ccfd45d436c4a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490208"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>Fluent API'si - özellikler ve türler eşleme ve yapılandırma
Entity Framework Code First ile çalışırken tablolarına desteklenmiş EF kuralları kümesi kullanarak POCO sınıflarınızı eşlemek için varsayılan davranış. Bazı durumlarda, ancak olamaz veya bu kuralları izleyin ve hangi kuralları dikte dışında bir şey varlıkları eşlemeniz istemediğiniz.  

Yapılandırma kuralları dışında bir şey yani kullanılacak EF başlıca iki yolu vardır [ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md) veya EFs fluent API'si. Ek açıklamalar kullanarak elde edilemeyecek eşleme senaryoları şekilde ek açıklamalar yalnızca fluent API'si işlevlerinin bir alt kümesini kapsar. Bu makalede, fluent API'si özellikleri yapılandırmak için nasıl kullanılacağını göstermek için tasarlanmıştır.  

Kod ilk fluent API'si geçersiz kılarak en sık erişilen [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) yöntemi türetilmiş [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx). Aşağıdaki örnekler fluent API'si ile çeşitli görevleri gerçekleştirmek ve kodu kopyalayın ve sahip olarak kullanılabilmesi için modeli görmek istiyorsanız, modelinizi uyacak şekilde özelleştirmenize izin vermek nasıl göstermek için tasarlanmıştır-sonra bu makalenin sonunda sağlanır.  

## <a name="model-wide-settings"></a>Model genelindeki ayarları  

### <a name="default-schema-ef6-onwards"></a>Varsayılan şema (EF6 sonrası)  

EF6 ile başlangıç HasDefaultSchema yöntemi, tüm tabloları, saklı yordamlar, vb. için kullanılacak veritabanı şemasını belirtmek için DbModelBuilder üzerinde kullanabilirsiniz. Bu varsayılan ayar için farklı bir şeması açıkça yapılandırdığınız herhangi bir nesne için geçersiz kılınır.  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a>Özel kuralları (EF6 sonrası)  

Code First bulunan desteklemek için kendi kurallarına oluşturabilirsiniz EF6 ile'başlatılıyor. Daha fazla ayrıntı için [özel kod öncelikli kurallar](~/ef6/modeling/code-first/conventions/custom.md).  

## <a name="property-mapping"></a>Özellik eşleme  

[Özelliği](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) yöntemi, bir varlığı veya karmaşık türün ait her bir özellik için öznitelikleri yapılandırmak için kullanılır. Özellik yöntemi, belirli bir özellik için bir yapılandırma nesnesi elde etmek için kullanılır. Yapılandırma nesnesini seçenekleri yapılandırılmakta türüne özeldir; Örneğin, yalnızca dize özellikleri IsUnicode kullanılabilir.  

### <a name="configuring-a-primary-key"></a>Bir birincil anahtar yapılandırma  

Birincil anahtarlar için Entity Framework kuralıdır:  

1. Sınıfınıza "ID" veya "Id" adı olan bir özelliği tanımlar.  
2. veya bir sınıf adının ardından "ID" veya "Id"  

Bir birincil anahtar olmasını özellik açıkça ayarlamak için HasKey yöntemi kullanabilirsiniz. Aşağıdaki örnekte, HasKey yöntemi Instructorıd birincil anahtarın OfficeAssignment türüne göre yapılandırmak için kullanılır.  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>Bileşik bir birincil anahtar yapılandırma  

Aşağıdaki örnek, bileşik bir birincil anahtar bölüm türü olmasını DepartmentID ve ad özelliklerini yapılandırır.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>Kimliğin oturumunu için sayısal birincil anahtarları değiştirme  

Aşağıdaki örnek değeri veritabanı tarafından oluşturulmaz belirtmek için System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None DepartmentID özelliğini ayarlar.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>En fazla uzunluğu bir özellikte belirtme  

Aşağıdaki örnekte, Name özelliği 50 karakterden uzun olmalıdır. Değer 50 karakterden uzun yaparsanız erişmenizi sağlayacak bir [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) özel durum. Code First bir veritabanı bu modelden oluşturması halinde, ad sütununun uzunluğu en fazla 50 karakter olarak ayarlanır.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>Özelliği gerekli olacak şekilde yapılandırma  

Aşağıdaki örnekte, Name özelliği gereklidir. Adı belirtmezseniz, DbEntityValidationException özel durum alırsınız. Bu modelden Code First bir veritabanı oluşturur, ardından bu özellik depolamak için kullanılan sütun genellikle atanamayan olacaktır.  

> [!NOTE]
> Bazı durumlarda veritabanını özelliği gerekli olsa bile null yapılamaz sütunda mümkün olmayabilir. Örneğin, ne zaman TPH devralma stratejisi veri için birden fazla türü kullanılarak tek bir tabloda depolanır. Gerekli bir özellik türetilmiş bir tür içeriyorsa, bu özellik hiyerarşideki tüm türleri olduğundan sütun atanamayan yapılamaz.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>Bir dizin üzerinde bir veya daha fazla özelliklerini yapılandırma  

> [!NOTE]
> **EF6.1 ve sonraki sürümler yalnızca** -Entity Framework 6.1 içinde dizin özniteliği tanıtılmıştır. Önceki bir sürümü kullanıyorsanız, bu bölümdeki bilgiler, geçerli değildir.  

Dizinler oluşturma Fluent API'si tarafından yerel olarak desteklenmiyor ancak yapabileceğiniz desteği kullanım **IndexAttribute** Fluent API'si üzerinden. Dizin öznitelikleri, bir model ek açıklama sonra işlem hattını veritabanında daha sonra bir dizinde açık bir modeli de dahil olmak üzere tarafından işlenir. Bu ek açıklamaları Fluent API'sini kullanarak el ile ekleyebilirsiniz.  

Bunu yapmanın en kolay yolu bir örneği oluşturmaktır **IndexAttribute** , yeni bir dizin için tüm ayarları içerir. Daha sonra bir örneğini oluşturabilirsiniz **IndexAnnotation** dönüştürecek bir EF belirli türü **IndexAttribute** EF model üzerinde depolanan bir model ek açıklama olarak ayarlar. Bunlar ardından geçirilebilir **HasColumnAnnotation** yöntemi Fluent adını belirterek API üzerinde **dizin** ek açıklama için.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

Kullanılabilir ayarların tam listesi için **IndexAttribute**, bkz: *dizin* bölümünü [kod ilk veri ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md). Bu, dizin adı özelleştirme, benzersiz dizinler oluşturma ve birden çok sütun dizinleri oluşturma içerir.  

Tek bir özellikte birden çok dizin ek açıklamaları dizisi geçirerek belirtebilirsiniz **IndexAttribute** oluşturucusuna **IndexAnnotation**.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation(
        "Index",  
        new IndexAnnotation(new[]
            {
                new IndexAttribute("Index1"),
                new IndexAttribute("Index2") { IsUnique = true }
            })));
```  

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a>Bir CLR özelliği veritabanında bir sütun değil eşlemeye belirtme  

Aşağıdaki örnek, bir CLR türünün bir özelliği, bir veritabanı sütununa eşlenmemiş belirtmek gösterilmektedir.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>Belirli bir sütuna veritabanında bir CLR özellik eşlemesi  

Aşağıdaki örnek adı CLR özelliği DepartmentName veritabanı sütununa eşler.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Modelde tanımlı olmayan bir yabancı anahtar yeniden adlandırma  

Değil bir CLR türüne bir yabancı anahtar tanımlayın, ancak veritabanında olması gereken hangi adını belirtmek istediğiniz seçerseniz, aşağıdakileri yapın:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>Bir dize özelliğini Unicode içeriği destekleyip desteklemediğini yapılandırma  

Varsayılan olarak, Unicode (SQL Server'da nvarchar) dizelerdir. IsUnicode yöntemi, bir dize varchar türü olması belirtmek için kullanabilirsiniz.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>Veritabanı sütununun veri türü yapılandırılıyor  

[HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) yöntemi aynı temel türden farklı temsilleri eşleme sağlar. Bu yöntemi kullanarak, çalışma zamanında verilerin herhangi bir dönüştürme gerçekleştirmek izin vermez. Veritabanı belirsiz olduğundan IsUnicode varchar, tercih edilen şekilde ayarı sütun olduğuna dikkat edin.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>Karmaşık bir türde özelliklerini yapılandırma  

Karmaşık bir türde skaler özellikler yapılandırmanın iki yolu vardır.  

Üzerinde ComplexTypeConfiguration özelliğini çağırabilirsiniz.  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

Nokta gösterimi, karmaşık bir türün bir özelliğe erişmek için de kullanabilirsiniz.  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>Bir iyimser eşzamanlılık belirteci olarak kullanılacak bir özelliğin yapılandırma  

Bir varlıktaki bir özelliği bir eşzamanlılık belirteci temsil ettiğini belirlemek için ConcurrencyCheck öznitelikte veya IsConcurrencyToken yöntemi kullanabilirsiniz.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

IsRowVersion yöntemi, özelliği veritabanında bir satır sürümü olacak şekilde yapılandırmak için de kullanabilirsiniz. İyimser eşzamanlılık belirteci olması için bir satır sürümü otomatik olarak yapılandırır özelliğini ayarlama.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>Tür eşlemesi  

### <a name="specifying-that-a-class-is-a-complex-type"></a>Bir sınıf bir karmaşık tür olduğunu belirleme  

Kural gereği, belirtilen birincil anahtarı olmayan bir türü bir karmaşık tür olarak kabul edilir. Burada Code First bir karmaşık türü (örneğin, kimliği adlı bir özelliğe sahip, ancak bunun için bir birincil anahtar olmasını gelmez varsa) algılamaz bazı senaryolar vardır. Böyle durumlarda, bir türü karmaşık bir tür olduğunu açıkça belirtmek için fluent API'sini kullanırsınız.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>CLR varlık türü için veritabanında bir tablo değil eşlenecek belirtme  

Aşağıdaki örnek, veritabanındaki bir tabloda eşlenen bir CLR türü dışlanacak gösterilmektedir.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>Bir varlık türü veritabanında belirli bir tablo eşleme  

Bölüm tüm özellikleri, t_ Departman adlı bir tablodaki sütunları eşleştirilir.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

Bu gibi şema adı da belirtebilirsiniz:  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a>Tablo-başına-hiyerarşi (TPH) devralma eşleme  

TPH eşleme senaryosunda, tek bir tabloya bir devralma hiyerarşisindeki tüm türleri eşlenir. Bir Ayrıştırıcı sütunu, her satır türünü tanımlamak için kullanılır. Modelinizi Code First ile oluştururken TPH devralma hiyerarşisinde katılan türleri için varsayılan stratejisidir. Varsayılan olarak, "Ayrıştırıcı" adlı bir tablo ayrıştırıcı sütunu eklenir ve hiyerarşideki her bir türü CLR tür adını ayrıştırıcı değerleri kullanılır. Fluent API'sini kullanarak varsayılan davranışını değiştirebilirsiniz.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>Tablo başına tür (TPT) devralma eşleme  

TPT eşleme senaryosunda, tek tek tablolar için tüm türleri eşlenir. Yalnızca bir temel tür veya türetilmiş bir tür ait özellikler bu türüyle eşleyen bir tabloda depolanır. Türetilmiş türleri eşleyen tablo türetilmiş tablonun temel tablosu ile birleştiren bir yabancı anahtar da depolar.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>Tablo başına somut sınıf (TPC) devralma eşleme  

TPC eşleme senaryosunda, tek tek tablolar için hiyerarşideki tüm soyut olmayan türleri eşlenir. Türetilmiş sınıflara eşleme tabloları temel sınıf veritabanında eşleşen tablo hiçbir ilişkisi. Devralınan özellikler dahil olmak üzere, bir sınıfın tüm özellikler için karşılık gelen bir tablonun sütunlarını eşlenir.  

Her türetilmiş bir tür yapılandırma MapInheritedProperties yöntemi çağırın. MapInheritedProperties temel sınıftan türetilmiş bir sınıf için yeni sütun için devralınan tüm özellikleri yeniden eşlemesi.  

> [!NOTE]
> TPC devralma hiyerarşisinde katılan tablolara değil paylaştığından birincil anahtar var. Yinelenen varlık anahtarları oluşturulan veritabanı değerleri ile aynı Kimlik tohumu varsa, alt sınıflar için eşlenmiş tablolardaki eklerken olacağını unutmayın. Bu sorunu çözmek için her tablo için farklı başlangıç Çekirdek değer belirtin veya birincil anahtar özelliği kimliği devre dışı geçin. Kimlik, Code First ile çalışırken tamsayı anahtar özellikleri için varsayılan değer.  

``` csharp
modelBuilder.Entity<Course>()
    .Property(c => c.CourseID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);

modelBuilder.Entity<OnsiteCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnsiteCourse");
});

modelBuilder.Entity<OnlineCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnlineCourse");
});
```  

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a>' % S'veritabanı (bölme varlık) birden çok tabloya bir varlık türünün özellikleri eşleme  

Bölme varlık birden çok tabloda yayılma için bir varlık türünün özelliklerini sağlar. Aşağıdaki örnekte, iki tabloya departmanı varlık bölünür: bölüm ve DepartmentDetails. Varlık bölme, belirli bir tabloya bir özellik alt kümesi eşlemek için birden çok çağrı harita yöntemi kullanır.  

``` csharp
modelBuilder.Entity<Department>()
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Name });
        m.ToTable("Department");
    })
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Administrator, t.StartDate, t.Budget });
        m.ToTable("DepartmentDetails");
    });
```  

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>Birden çok varlık türleri (bölme tablosu) veritabanında bir tablo eşleme  

Aşağıdaki örnek, bir tablonun birincil anahtara paylaşan iki varlık türleri eşler.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Insert/Update/Delete saklı yordamlar (EF6 sonrası) için bir varlık türü eşleme  

EF6 ile başlangıç, saklı yordamlar için ekleme güncelleştirme ve silme varlığın eşleyebilirsiniz. Daha fazla bilgi için [kod ilk Insert/Update/Delete saklı yordamlar](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).  

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
