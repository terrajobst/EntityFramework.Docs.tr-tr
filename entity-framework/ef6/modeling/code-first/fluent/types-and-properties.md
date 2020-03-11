---
title: Akıcı API-özellikleri ve türleri yapılandırma ve eşleme-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 7371cc99142ccf8fc6bea237d7d58d1e67fcecec
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419068"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>Akıcı API-özellikleri ve türleri yapılandırma ve eşleme
Code First Entity Framework ile çalışırken, varsayılan davranış, POCO sınıflarınızı, EF 'e bakılan bir dizi kuralı kullanarak tablo olarak eşlemenize yardımcı olur. Ancak, bazen bu kuralları takip edemez veya bu kuralları izlemek istemiyor, varlıkları kuralların izlediklerinde başka bir şeyle eşleştirmek zorunda değilsiniz.  

Bir kural dışında bir şey kullanmak için EF 'i yapılandırabileceğiniz iki temel yol vardır, yani [ek açıklamalar](~/ef6/modeling/code-first/data-annotations.md) veya EFS Fluent API. Ek açıklamalar yalnızca Fluent API işlevselliğinin bir alt kümesini kapsar, bu nedenle ek açıklamalar kullanılarak elde edilemeyecek olan eşleme senaryoları vardır. Bu makale, özellikleri yapılandırmak için Fluent API nasıl kullanacağınızı göstermek için tasarlanmıştır.  

İlk Fluent API kod, türetilmiş [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx)'Teki [onmodeloluþturma](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) yöntemi geçersiz kılınarak en yaygın olarak erişilir. Aşağıdaki örnekler, akıcı API ile çeşitli görevlerin nasıl yapılacağını göstermek ve kodu dışarı kopyalayıp modelinize uyacak şekilde özelleştirmeyi sağlamak üzere tasarlanmıştır. Bu durumda, bu makalenin sonunda verilmiştir. Bu durumda, bu makalenin sonunda sağlanır.  

## <a name="model-wide-settings"></a>Model genelindeki ayarlar  

### <a name="default-schema-ef6-onwards"></a>Varsayılan şema (EF6 onsürümleri)  

EF6 ile başlayarak, tüm tablolar, saklı yordamlar vb. için kullanılacak veritabanı şemasını belirtmek üzere DbModelBuilder üzerinde HasDefaultSchema yöntemini kullanabilirsiniz. Bu varsayılan ayar, için açıkça farklı bir şema yapılandırdığınız tüm nesneler için geçersiz kılınır.  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a>Özel kurallar (EF6 ve sonraki sürümler)  

EF6 ile başlayarak, Code First dahil olanlar için kendi kurallarınızı oluşturabilirsiniz. Daha ayrıntılı bilgi için bkz. [özel Code First kuralları](~/ef6/modeling/code-first/conventions/custom.md).  

## <a name="property-mapping"></a>Özellik Eşleme  

[Özellik](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) yöntemi, bir varlığa veya karmaşık türe ait her bir özelliğin özniteliklerini yapılandırmak için kullanılır. Özellik yöntemi, belirli bir özellik için bir yapılandırma nesnesi elde etmek için kullanılır. Yapılandırma nesnesindeki seçenekler, yapılandırılmakta olan türe özgüdür; Isunıcode yalnızca dize özelliklerinde kullanılabilir.  

### <a name="configuring-a-primary-key"></a>Birincil anahtar yapılandırma  

Birincil anahtarlar için Entity Framework kuralı:  

1. Sınıfınız, adı "ID" veya "ID" olan bir özellik tanımlar  
2. ya da "ID" veya "ID" gelen bir sınıf adı  

Açıkça bir özelliği birincil anahtar olarak ayarlamak için HasKey yöntemini kullanabilirsiniz. Aşağıdaki örnekte, OfficeAssignment türündeki Komutctorıd birincil anahtarını yapılandırmak için HasKey yöntemi kullanılır.  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>Bileşik birincil anahtar yapılandırma  

Aşağıdaki örnek, DepartmentID ve ad özelliklerini departman türünün bileşik birincil anahtarı olacak şekilde yapılandırır.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>Sayısal birincil anahtarlar için kimlik devre dışı bırakma  

Aşağıdaki örnek, değerin veritabanı tarafından üretilmeyeceğini göstermek için DepartmentID özelliğini System. ComponentModel. Dataaçıklamalarda. DatabaseGeneratedOption. None olarak ayarlar.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>Bir özellikte maksimum uzunluğu belirtme  

Aşağıdaki örnekte, Name özelliği 50 karakterden uzun olmamalıdır. Değeri 50 karakterden daha uzun yaparsanız, [Dbentityvalidationexception](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) özel durumu alırsınız. Code First bu modelden bir veritabanı oluşturursa, Ad sütununun uzunluk üst sınırını 50 karakter olacak şekilde ayarlar.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>Gerekli olacak özelliği yapılandırma  

Aşağıdaki örnekte, Name özelliği gereklidir. Bu adı belirtmezseniz, bir DbEntityValidationException özel durumu alırsınız. Code First bu modelden bir veritabanı oluşturursa, bu özelliği depolamak için kullanılan sütun genellikle null atanamaz olur.  

> [!NOTE]
> Bazı durumlarda, özelliği gerekli olsa bile veritabanındaki sütunun null olmaması mümkün olmayabilir. Örneğin, birden çok tür için bir TPH devralma stratejisi verisi kullanıldığında tek bir tabloda depolanır. Türetilmiş bir tür gerekli bir özellik içeriyorsa, hiyerarşideki tüm türlerin bu özelliği içermesi olmadığından, sütun null yapılamayan yapılamaz.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>Bir veya daha fazla özelliklerde dizin yapılandırma  

> [!NOTE]
> **EF 6.1 yalnızca** sonraki sürümler-dizin özniteliği Entity Framework 6,1 ' de tanıtılmıştı. Önceki bir sürümü kullanıyorsanız, bu bölümdeki bilgiler uygulanmaz.  

Dizin oluşturma, akıcı API tarafından yerel olarak desteklenmez, ancak akıcı API aracılığıyla **ındexattribute** desteğini kullanabilirsiniz. Dizin öznitelikleri, modele daha sonra işlem hattında daha sonra veritabanında bulunan bir model ek açıklaması eklenerek işlenir. Bu ek açıklamaları, akıcı API 'YI kullanarak el ile ekleyebilirsiniz.  

Bunu yapmanın en kolay yolu, yeni dizinin tüm ayarlarını içeren **ındexattribute** öğesinin bir örneğini oluşturmaktır. Daha sonra **ındexattribute** ayarlarını EF modelinde depolanabilecek bir model ek açıklamasına DÖNÜŞTÜRECEK bir EF özel türü olan **ındexannotation** öğesinin bir örneğini oluşturabilirsiniz. Bunlar daha sonra, ek açıklamanın ad **dizinini** belirterek, akıcı API 'de **Hasccolumnannotation** yöntemine geçirilebilir.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

**Indexattribute**'da kullanılabilen ayarların tüm listesi Için [Code First veri ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md)' nın *Dizin* bölümüne bakın. Bu, dizin adını özelleştirmeyi, benzersiz dizinler oluşturmayı ve çok sütunlu dizinler oluşturmayı içerir.  

Bir **ındexattribute** dizisini **ındexannotation**oluşturucusuna geçirerek, tek bir özellikte birden çok dizin ek açıklaması belirtebilirsiniz.  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a>CLR özelliğinin veritabanındaki bir sütunla Eşlememe belirtme  

Aşağıdaki örnek, bir CLR türündeki bir özelliğin veritabanındaki bir sütunla eşlenmiyor olduğunu gösterir.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>CLR özelliğini veritabanındaki belirli bir sütunla eşleme  

Aşağıdaki örnek, ad CLR özelliğini DepartmentName veritabanı sütunuyla eşler.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Modelde tanımlanmayan bir yabancı anahtarı yeniden adlandırma  

Bir CLR türünde yabancı anahtar tanımlamadıysanız, ancak veritabanında olması gereken adı belirtmek istiyorsanız aşağıdakileri yapın:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>Dize özelliğinin Unicode Içeriğini destekleyip desteklemediğini yapılandırma  

Varsayılan dizeler Unicode 'Dur (SQL Server içinde nvarchar). Bir dizenin varchar türünde olması gerektiğini belirtmek için ısunıcode yöntemini kullanabilirsiniz.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>Veritabanı sütununun veri türünü yapılandırma  

[Hasccolumntype](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) yöntemi, aynı temel türün farklı temsillerine eşlemeyi sağlar. Bu yöntemin kullanılması, çalışma zamanında verilerin herhangi bir dönüşümünü gerçekleştirmenize izin vermez. Iunıcode 'un, veritabanı belirsiz olduğu için sütun varchar 'a ayarlamanın tercih edilen yolu olduğunu unutmayın.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>Karmaşık bir tür üzerinde özellikleri yapılandırma  

Karmaşık bir tür üzerinde skaler özellikleri yapılandırmanın iki yolu vardır.  

ComplexTypeConfiguration üzerindeki özelliği çağırabilirsiniz.  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

Ayrıca, karmaşık bir türün bir özelliğine erişmek için nokta gösterimini de kullanabilirsiniz.  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>Iyimser eşzamanlılık belirteci olarak kullanılacak bir özelliği yapılandırma  

Bir varlıktaki bir özelliğin eşzamanlılık belirtecini temsil ettiğini belirtmek için ConcurrencyCheck özniteliğini ya da IsConcurrencyToken yöntemini kullanabilirsiniz.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

Ayrıca, özelliğini veritabanında bir satır sürümü olacak şekilde yapılandırmak için ırowversıon yöntemini de kullanabilirsiniz. Özelliği bir satır sürümü olacak şekilde ayarlamak, onu iyimser eşzamanlılık belirteci olacak şekilde otomatik olarak yapılandırır.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>Tür eşleme  

### <a name="specifying-that-a-class-is-a-complex-type"></a>Bir sınıfın karmaşık bir tür olduğunu belirtme  

Kurala göre, belirtilen birincil anahtarı olmayan bir tür karmaşık bir tür olarak değerlendirilir. Code First karmaşık bir türü algılamamasının bazı senaryolar vardır (örneğin, KIMLIK olarak adlandırılan bir özellik varsa, ancak birincil anahtar olması anlamına gelir). Böyle durumlarda, türün karmaşık bir tür olduğunu açıkça belirtmek için Fluent API kullanırsınız.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>CLR varlık türünün veritabanındaki bir tabloya Eşlenmeme belirtme  

Aşağıdaki örnek, bir CLR türünün veritabanındaki bir tabloya eşlenmeden nasıl dışlanacağını göstermektedir.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>Bir varlık türünü veritabanındaki belirli bir tabloyla eşleme  

Departmanın tüm özellikleri t_ departmanı adlı tablodaki sütunlara eşlenir.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

Şema adını şöyle da belirtebilirsiniz:  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a>Hiyerarşi başına tablo (TPH) devralmayı eşleme  

TPH eşleme senaryosunda, bir devralma hiyerarşisindeki tüm türler tek bir tabloyla eşleştirilir. Bir Ayrıştırıcı sütunu, her satırın türünü tanımlamak için kullanılır. Code First ile modelinizi oluştururken, TPH devralma hiyerarşisine katılan türler için varsayılan stratejidir. Varsayılan olarak, ayrıştırıcı sütunu tabloya "ayrıştırıcı" adı ile eklenir ve hiyerarşideki her türün CLR türü adı ayrıştırıcı değerleri için kullanılır. Varsayılan davranışı Fluent API kullanarak değiştirebilirsiniz.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>Tablo tür başına (TPT) devralmayı eşleme  

TPT eşleme senaryosunda, tüm türler ayrı tablolara eşlenir. Yalnızca bir temel türe veya türetilmiş türe ait özellikler, bu türle eşleşen bir tabloda depolanır. Türetilmiş türlerle eşlenen tablolar, türetilmiş tabloya temel tabloyla birleştiren bir yabancı anahtar de depolar.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>Tablo başına somut sınıf (TPC) devralmayı eşleme  

TPC eşleme senaryosunda, hiyerarşideki Özet olmayan tüm türler ayrı tablolara eşlenir. Türetilmiş sınıflarla eşlenen tablolarda, veritabanındaki temel sınıfla eşlenen tabloyla ilişki yoktur. Devralınan özellikler dahil olmak üzere bir sınıfın tüm özellikleri karşılık gelen tablonun sütunlarına eşlenir.  

Türetilmiş her türü yapılandırmak için Mapınheritedproperties metodunu çağırın. Mapınheritedproperties, temel sınıftan devralınan tüm özellikleri, türetilmiş sınıf için tablodaki yeni sütunlara yeniden eşler.  

> [!NOTE]
> TPC devralma hiyerarşisine katılan tablolar birincil anahtar paylaşmadığından, veritabanı tarafından oluşturulan ve aynı kimlik kaynağını içeren bir değer varsa, alt sınıflara eşlenen tablolara eklenirken yinelenen varlık anahtarları olacaktır. Bu sorunu çözmek için, her tablo için farklı bir başlangıç çekirdek değeri belirtebilir veya birincil anahtar özelliğindeki kimlik devre dışı bırakabilirsiniz. Kimlik, Code First çalışırken tamsayı anahtar özellikleri için varsayılan değerdir.  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a>Bir varlık türünün özelliklerini veritabanındaki birden çok tabloya eşleme (varlık bölme)  

Varlık bölünmesi bir varlık türünün özelliklerinin birden çok tabloya yayılmasını sağlar. Aşağıdaki örnekte, departman varlığı iki tabloya ayrılır: Department ve DepartmentDetails. Varlık bölünmesi, bir özellik alt kümesini belirli bir tabloya eşlemek için Map yöntemine birden çok çağrı kullanır.  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>Birden çok varlık türünü veritabanındaki bir tabloyla eşleme (tablo bölme)  

Aşağıdaki örnek, birincil anahtarı tek bir tabloyla paylaşan iki varlık türünü eşler.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Bir varlık türünü, saklı yordamları ekleme/güncelleştirme/silme (EF6 ve sonraki sürümler) ile eşleme  

EF6 ' den itibaren, güncelleştirme Ekle ve Sil için saklı yordamları kullanmak üzere bir varlığı eşleyebilirsiniz. Daha fazla ayrıntı için bkz. [Code First, saklı yordamları ekleme/güncelleştirme/silme](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).  

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
