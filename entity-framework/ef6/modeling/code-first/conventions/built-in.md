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
# <a name="code-first-conventions"></a>Code First kuralları
Code First, veya Visual Basic .NET sınıfları kullanarak C# bir modeli açıklamanıza olanak sağlar. Modelin temel şekli, kuralları kullanılarak algılanır. Kurallar, Code First çalışırken sınıf tanımlarına göre kavramsal bir modeli otomatik olarak yapılandırmak için kullanılan kuralların kümeleridir. Kurallar System. Data. Entity. ModelConfiguration. kurallara ilişkin ad alanında tanımlanır.  

Modelinize veri açıklamalarını veya Fluent API kullanarak daha fazla yapılandırma yapabilirsiniz. Öncelik, Fluent API ve ardından veri ek açıklamaları ve kuralları aracılığıyla yapılandırmaya verilir. Daha fazla bilgi için bkz. [veri ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md), [akıcı, API ılışkılerı](~/ef6/modeling/code-first/fluent/relationships.md), [akıcı apı türleri & ÖZELLIKLERI](~/ef6/modeling/code-first/fluent/types-and-properties.md) ve [vb.NET ile akıcı API](~/ef6/modeling/code-first/fluent/vb.md).  

[API belgelerinde](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx)Code First kuralların ayrıntılı bir listesi bulunur. Bu konu, Code First tarafından kullanılan kurallara genel bir bakış sağlar.  

## <a name="type-discovery"></a>Tür keşfi  

Code First geliştirme kullanılırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan .NET Framework sınıfları yazarak başlarsınız. Sınıfları tanımlamaya ek olarak, **DbContext** 'in modele hangi türleri dahil etmek istediğinizi öğrenmenizi de gerekir. Bunu yapmak için, **DbContext** 'ten türetilen bir bağlam sınıfı tanımlar ve modelin bir parçası olmasını istediğiniz türler Için **dbset** özelliklerini kullanıma sunar. Code First, başvurulan türler farklı bir derlemede tanımlansa bile, bu türleri içerir ve başvurulan türleri de alır.  

Türleriniz bir devralma hiyerarşisine katılırsanız, temel sınıf için bir **Dbset** özelliği tanımlanması yeterlidir ve türetilmiş türler, temel sınıfla aynı derlemede olmaları durumunda otomatik olarak dahil edilir.  

Aşağıdaki örnekte, **Ssınlentities** sınıfında (**Departmanlar**) tanımlanmış tek bir **dbset** özelliği vardır. Code First başvurulan herhangi bir türü bulup çekmek için bu özelliği kullanır.  

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

Bir türü modelden dışlamak isterseniz **Noteşlenmiş** özniteliği veya **dbmodelbuilder. Ignore** Fluent API kullanın.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>Birincil anahtar kuralı  

Bir sınıftaki özellik "ID" (büyük/küçük harfe duyarlı değil) veya sınıf adı "ID" tarafından adlandırılmışsa, bir özelliğin birincil anahtar olduğunu Code First. Birincil anahtar özelliğinin türü sayısal veya GUID ise, kimlik sütunu olarak yapılandırılır.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>İlişki kuralı  

Entity Framework, gezinti özellikleri iki varlık türü arasında bir ilişkiye gitmek için bir yol sağlar. Her nesnenin katıldığı her ilişki için bir gezinti özelliği olabilir. Gezinti özellikleri, bir başvuru nesnesi (çoğulluk bir veya sıfır ya da-bir) ya da bir koleksiyon (çokluk çok ise) döndüren her iki yönde ilişkilerde gezinmeniz ve bunları yönetmenize olanak tanır. Code First, türleriniz üzerinde tanımlanan gezinti özelliklerine göre ilişkilerle ilişkilerler.  

Gezinti özelliklerine ek olarak, bağımlı nesneleri temsil eden türlere yabancı anahtar özellikleri dahil etmenizi öneririz. Asıl birincil anahtar özelliği ile aynı veri türüne ve aşağıdaki biçimlerden birini izleyen bir ada sahip herhangi bir özellik, ilişki için bir yabancı anahtarı temsil eder: '\<gezinti özelliği adı\>\<asıl birincil anahtar özellik adı\>', '\<birincil anahtar özellik adı\>' veya ' \<asıl birincil anahtar özellik adı\>'.\<\> Birden fazla eşleşme bulunursa, öncelik yukarıda belirtilen sırada verilir. Yabancı anahtar algılama, büyük/küçük harfe duyarlı değildir. Yabancı anahtar özelliği algılandığında, Code First yabancı anahtarın null değer alabilirliğine göre ilişkinin çokluğunu alır. Özellik null yapılabilir ise, ilişki isteğe bağlı olarak kaydedilir; Aksi takdirde, ilişki gerektiği şekilde kaydedilir.  

Bağımlı varlıktaki bir yabancı anahtar null yapılabilir değilse, Code First ilişkide basamaklı silme ayarlar. Bağımlı varlıktaki bir yabancı anahtar null yapılabilir ise, Code First ilişkide basamaklı silme ayarı yapmaz ve asıl öğe silindiğinde yabancı anahtar null olarak ayarlanır. Kural tarafından algılanan çoğulluk ve basamaklı silme davranışı, Fluent API kullanılarak geçersiz kılınabilir.  

Aşağıdaki örnekte, bölüm ve kurs sınıfları arasındaki ilişkiyi tanımlamak için gezinti özellikleri ve yabancı anahtar kullanılır.  

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
> Aynı türler arasında birden çok ilişki varsa (örneğin, **kişi** sınıfının **Reviedilimlerin** **ve** **Authoredbooks** gezinti özelliklerini içerdiğini ve **kitap** sınıfı **Yazar** ve **Gözden geçiren** gezinti özelliklerini içeriyorsa), veri açıklamalarını veya Fluent API kullanarak ilişkileri el ile yapılandırmanız gerekir. Daha fazla bilgi için bkz. [veri ek açıklamaları-ilişkiler](~/ef6/modeling/code-first/data-annotations.md) ve [floent API-Relationships](~/ef6/modeling/code-first/fluent/relationships.md).  

## <a name="complex-types-convention"></a>Karmaşık türler kuralı  

Code First birincil anahtarın çıkarsanmayacak bir sınıf tanımını bulduğunda ve veri ek açıklamaları veya Fluent API ile hiçbir birincil anahtar kaydedilmemişse, tür otomatik olarak karmaşık bir tür olarak kaydedilir. Karmaşık tür algılama ayrıca türün varlık türlerine başvuran özelliklere sahip olmaması ve başka bir türdeki bir koleksiyon özelliğinden başvurulmaması gerekir. Aşağıdaki sınıf tanımları verildiğinde Code First, birincil anahtarı olmadığından **ayrıntıların** karmaşık bir tür olduğunu çıkarırdı.  

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

## <a name="connection-string-convention"></a>Bağlantı dizesi kuralı  

DbContext 'in kullanım bağlantısını bulması için kullandığı kurallar hakkında bilgi edinmek için bkz. [Bağlantılar ve modeller](~/ef6/fundamentals/configuring/connection-strings.md).  

## <a name="removing-conventions"></a>Kuralları kaldırma  

System. Data. Entity. ModelConfiguration. kurallara ilişkin ad alanında tanımlanan kurallardan herhangi birini kaldırabilirsiniz. Aşağıdaki örnek **Pluralizingtablenameconvention**'ı kaldırır.  

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

## <a name="custom-conventions"></a>Özel Kurallar  

Özel kurallar EF6 ve sonraki sürümlerde desteklenir. Daha fazla bilgi için bkz. [özel Code First kuralları](~/ef6/modeling/code-first/conventions/custom.md).
