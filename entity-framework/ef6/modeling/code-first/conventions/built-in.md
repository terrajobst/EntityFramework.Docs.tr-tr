---
title: Kod öncelikli kurallar - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: c5fa580879a4b53fed34d94b737988875f38c62c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995532"
---
# <a name="code-first-conventions"></a>Kod öncelikli kurallar
Kod, C# veya Visual Basic .NET sınıflarını kullanarak bir modeli tanımlamak öncelikle sağlar. Modelin temel şekil kurallarını kullanarak algılandı. Code First ile çalışırken, sınıf tanımlarını temel alarak bir kavramsal model otomatik olarak yapılandırmak için kullanılan kural kümesi kurallardır. Kuralları System.Data.Entity.ModelConfiguration.Conventions ad alanında tanımlanır.  

Veri ek açıklamaları ya da fluent API'sini kullanarak, modelinizi daha da yapılandırabilirsiniz. Veri ek açıklamaları ardından ve kuralları fluent API'si üzerinden yapılandırma için öncelik verilir. Daha fazla bilgi için [veri ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md), [Fluent API'si - ilişkileri](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API'si - türleri ve özellikleri](~/ef6/modeling/code-first/fluent/types-and-properties.md) ve [Fluent API'si ile VB.NET](~/ef6/modeling/code-first/fluent/vb.md).  

Ayrıntılı kod öncelikli kurallar listesi kullanılabilir [API belgeleri](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx). Bu konu, Code First tarafından kullanılan kuralları için genel bir bakış sağlar.  

## <a name="type-discovery"></a>Bulma türü  

Code First geliştirme kullanırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan bir .NET Framework sınıf yazarak başlayın. Sınıf tanımlamanın yanı sıra, ayrıca izin gerekir **DbContext** modele dahil etmek istediğiniz hangi türlerin bildirin. Bunu yapmak için türetilen bir bağlamı sınıfı tanımlayın **DbContext** ve ortaya çıkaran **olan DB** modelinin bir parçası olmasını istediğiniz türleri için özellikleri. Kod öncelikle bu türler dahil edilir ve başvurulan türleri farklı bir derlemede tanımlanan bile tüm başvurulan türlerinde çeker.  

Türlerinizin bir devralma hiyerarşisinde katılırsanız, tanımlamak için yeterli olan bir **olan DB** temel sınıf olarak aynı derlemede olmaları durumunda özelliği temel sınıf ve türetilen türler için otomatik olarak dahil edilecektir.  

Aşağıdaki örnekte, yalnızca bir tane olduğunu **olan DB** tanımlanan özellik **SchoolEntities** sınıfı (**Departmanlar**). Kod, bulmak ve herhangi başvurulan türleri çekmek için önce bu özelliği kullanır.  

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

Modelden bir türü dışlamak istiyorsanız, kullanın **NotMapped** özniteliği veya **DbModelBuilder.Ignore** fluent API'si.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>Birincil anahtar kuralı  

Kod önce bir özellik "ID" (büyük/küçük harfe duyarlı değil) adlı bir özellik bir sınıf veya sınıf adı "ID" tarafından izlenen bir birincil anahtar olduğunu algılar. Birincil anahtar özelliği türü sayısal veya GUID olacaktır bir kimlik sütunu yapılandırılmış.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>İlişki kuralı  

Varlık Çerçevesi'nde, gezinti özellikleri iki varlık türleri arasındaki bir ilişki gitmek için bir yol sağlar. Her nesne içinde katılan her ilişki için bir gezinti özelliği olabilir. Gezinti özellikleri gidin ve bir başvuru nesnesi döndürerek her iki yönde ilişkileri yönetmenize olanak sağlar (çeşitlilik tek ise ya da sıfır veya bir) veya (çeşitlilik birçok ise) bir koleksiyon. Kod ilk, türlerinde tanımlanan Gezinti özelliklerine göre ilişkileri algılar.  

Gezinme özelliklerinin yanı sıra, yabancı anahtar özelliklerini bağımlı nesneleri temsil eden türleri dahil öneririz. Yabancı anahtar ilişkisi için asıl birincil anahtar özelliği olarak aynı veri türüne sahip ve aşağıdaki biçimlerden birini izleyen bir ad ile herhangi bir özelliği temsil eder: '\<gezinme özelliği adı\>\<asıl birincil anahtar özelliği adı\>','\<asıl sınıf adı\>\<birincil anahtar özelliği adı\>', veya '\<asıl birincil anahtar özelliği adı\>'. Birden fazla eşleşme bulunursa, yukarıda listelenen sırayla öncelik verilir. Yabancı anahtar algılama büyük/küçük harfe duyarlı değildir. Bir yabancı anahtar özellik algılandığında, Code First, ilişkinin çoğulluk değerinin null atanabilirliği yabancı anahtarı üzerinde göre çıkarır. Özellik boş değer atanabilir ise ilişki isteğe bağlı olarak kaydedilir; Aksi takdirde ilişki kayıtlı gerektiğinde.  

Bağımlı varlıkta bir yabancı anahtar null değilse, ardından Code First art arda silme ilişkisine ayarlar. Bağımlı varlıkta bir yabancı anahtar null yapılabilir, Code First ayarlı değil art arda silme ilişkisine ve asıl silindiğinde yabancı anahtarı ayarlama null. Tarafından algılanan davranışı çeşitlilik ve art arda silme kuralını fluent API'sini kullanarak kılınabilir.  

Aşağıdaki örnekte, gezinti özellikleri ve yabancı anahtar bölümü ve kursu sınıfları arasındaki ilişkiyi tanımlamak için kullanılır.  

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
> Aynı türleri arasında birden çok ilişki varsa (örneğin, tanımladığınız varsayalım **kişi** ve **kitap** sınıfları, burada **kişi** sınıfı içerir **ReviewedBooks** ve **AuthoredBooks** Gezinti özellikleri ve **kitap** sınıfı içeren **Yazar** ve  **Gözden Geçiren** Gezinti özellikleri) veri ek açıklamaları ya da fluent API'sini kullanarak ilişkileri el ile yapılandırmanız gerekir. Daha fazla bilgi için [veri ek açıklamaları - ilişkileri](~/ef6/modeling/code-first/data-annotations.md) ve [Fluent API'si - ilişkileri](~/ef6/modeling/code-first/fluent/relationships.md).  

## <a name="complex-types-convention"></a>Karmaşık türler kuralı  

Code First burada bir birincil anahtar çıkarsanamıyor ve birincil anahtar veri ek açıklamaları veya fluent API'si ile kayıtlı bir sınıf tanımını bulur, türü bir karmaşık tür olarak otomatik olarak kaydedilir. Karmaşık tür algılama ayrıca türü başvuru varlık türleri ve başka bir tür üzerindeki bir koleksiyona özelliğinden başvurulmuyor özellikleri yok gerektirir. Aşağıdaki sınıf tanımları verilen Code First, tanım Çıkarsama **ayrıntıları** , birincil anahtar içerdiğinden karmaşık bir türdür.  

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

DbContext bakın kullanılacak bağlantı bulmak için kullandığı kuralları hakkında bilgi edinmek için [bağlantıları ve modelleri](~/ef6/fundamentals/configuring/connection-strings.md).  

## <a name="removing-conventions"></a>Kuralları kaldırılıyor  

System.Data.Entity.ModelConfiguration.Conventions ad alanında tanımlanan kuralları kaldırabilirsiniz. Aşağıdaki örnek kaldırır **PluralizingTableNameConvention**.  

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

## <a name="custom-conventions"></a>Özel kuralları  

Özel kuralları içinde EF6 ve sonraki sürümlerde desteklenir. Daha fazla bilgi için [özel kod öncelikli kurallar](~/ef6/modeling/code-first/conventions/custom.md).
