---
title: Model tabanlı kuralları - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: 80b722730b4ca6c9d00a8611b6c9027e8bc9fe61
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283713"
---
# <a name="model-based-conventions"></a>Model tabanlı kuralları
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.  

Model tabanlı, Gelişmiş bir yöntem tabanlı model yapılandırmasının kurallardır. Çoğu senaryo için [DbModelBuilder özel kod ilk kuralı API](~/ef6/modeling/code-first/conventions/custom.md) kullanılmalıdır. Dayalı modeli kuralları kullanmadan önce bir anlayış DbModelBuilder API kuralları için önerilir.  

Model tabanlı kuralları özellikleri ve standart kuralları ile yapılandırılabilir olmayan tablolar etkileyen kurallarının oluşturmasına olanak sağlar. Bu tablo başına hiyerarşi modelleri ayrıştırıcı sütunlarında ve bağımsız ilişkilendirme sütunları örnekleridir.  

## <a name="creating-a-convention"></a>Bir kuralı oluşturma   

İşlem hattı, modele uygulanacak kuralı gerektiğinde dayalı modeli kuralı oluşturmanın ilk adımı seçmektir. Model kuralları, kavramsal (C-Space) ve Store (S-alan) iki tür vardır. Bir C alan kuralı uygulama oluşturan veritabanı temsil eden model sürümüne uygulanan S alan kuralı ise ve nasıl otomatik olarak oluşturulan sütunları gibi denetimleri şeyler adlı modele uygulanır.  

Bir model kuralı IConceptualModelConvention veya IStoreModelConvention genişleten bir sınıftır.  Bu arabirimlerin her ikisi de olabilir genel bir tür kabul kuralı geçerli veri türünü filtrelemek için kullanılan MetadataItem yazın.  

## <a name="adding-a-convention"></a>Bir kural ekleme   

Model kuralları normal kuralları sınıflar olarak aynı şekilde eklenir. İçinde **OnModelCreating** yöntemi, bir model için kuralları listesi kuralı ekleyin.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

public class BlogContext : DbContext  
{  
    public DbSet<Post> Posts { get; set; }  
    public DbSet<Comment> Comments { get; set; }  

    protected override void OnModelCreating(DbModelBuilder modelBuilder)  
    {  
        modelBuilder.Conventions.Add<MyModelBasedConvention>();  
    }  
}
```  

Bir kuralı Conventions.AddBefore kullanarak başka bir kural ile ilgili olarak da eklenebilir\< \> veya Conventions.AddAfter\< \> yöntemleri. Entity Framework uygulanan kuralları hakkında daha fazla bilgi için Notlar bölümüne bakın.  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>Örnek: Ayrıştırıcı Model kuralı  

EF tarafından oluşturulan sütunları yeniden adlandırma, diğer kuralları ile API'leri yapamayacağı bir şeyin bir örnektir.  Bu bir durumdur modeli kurallarını kullanarak tek seçeneğiniz olduğu.  

Oluşturulan sütunları yapılandırmak için bir model tabanlı kuralı kullanma örneği, ayrıştırıcı sütunları adlı biçimi özelleştirildiğinde.  Her sütunda "Ayrıştırıcı" adlı model "EntityType için" yeniden adlandırır bir dayalı basit model kuralı örneği aşağıda verilmiştir.  Bu, geliştirici yalnızca "Ayrıştırıcı" adlı sütun içerir. "Ayrıştırıcı" sütunu oluşturulan bir sütun olduğundan bu S alanda çalışan gerekir.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

class DiscriminatorRenamingConvention : IStoreModelConvention<EdmProperty>  
{  
    public void Apply(EdmProperty property, DbModel model)  
    {            
        if (property.Name == "Discriminator")  
        {  
            property.Name = "EntityType";  
        }  
    }  
}
```  

## <a name="example-general-ia-renaming-convention"></a>Örnek: Genel IA kuralı yeniden adlandırma   

Başka bir daha karmaşık eylem dayalı modeli kuralları bağımsız ilişkilendirmeleri (IAS) adlandırdığınız şekilde yapılandırmak için örneğidir.  Bu Model kuralları burada IAS oluşturulur çünkü EF tarafından uygulanabilir ve DbModelBuilder API erişimi olan modelde olmayan bir durumdur.  

EF bir IA oluşturduğunda EntityType_KeyName adlı bir sütun oluşturur. Örneğin, müşteri adlı bir anahtar sütunu ile bir ilişki Customer_CustomerId adlı bir sütun oluşturmak CustomerID adlı. Aşağıdaki kural şeritler '\_' karakteri dışında IA. için oluşturulan sütun adı  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

// Provides a convention for fixing the independent association (IA) foreign key column names.  
public class ForeignKeyNamingConvention : IStoreModelConvention<AssociationType>
{

    public void Apply(AssociationType association, DbModel model)
    {
        // Identify ForeignKey properties (including IAs)  
        if (association.IsForeignKey)
        {
            // rename FK columns  
            var constraint = association.Constraint;
            if (DoPropertiesHaveDefaultNames(constraint.FromProperties, constraint.ToRole.Name, constraint.ToProperties))
            {
                NormalizeForeignKeyProperties(constraint.FromProperties);
            }
            if (DoPropertiesHaveDefaultNames(constraint.ToProperties, constraint.FromRole.Name, constraint.FromProperties))
            {
                NormalizeForeignKeyProperties(constraint.ToProperties);
            }
        }
    }

    private bool DoPropertiesHaveDefaultNames(ReadOnlyMetadataCollection<EdmProperty> properties, string roleName, ReadOnlyMetadataCollection<EdmProperty> otherEndProperties)
    {
        if (properties.Count != otherEndProperties.Count)
        {
            return false;
        }

        for (int i = 0; i < properties.Count; ++i)
        {
            if (!properties[i].Name.EndsWith("_" + otherEndProperties[i].Name))
            {
                return false;
            }
        }
        return true;
    }

    private void NormalizeForeignKeyProperties(ReadOnlyMetadataCollection<EdmProperty> properties)
    {
        for (int i = 0; i \< properties.Count; ++i)
        {
            int underscoreIndex = properties[i].Name.IndexOf('_');
            if (underscoreIndex > 0)
            {
                properties[i].Name = properties[i].Name.Remove(underscoreIndex, 1);
            }                 
        }
    }
}
```  

## <a name="extending-existing-conventions"></a>Var olan kuralları genişletme   

Entity Framework modelinize zaten uygulanan kuralları birini benzer bir kuralı yazmak gerekiyorsa, her zaman sıfırdan yeniden yazmak zorunda kalmamak için kuralı genişletebilirsiniz.  Buna örnek olarak mevcut kimliği ile özel bir kural eşleşen değiştirmektir.   Anahtar kuralı geçersiz kılma için ek bir avantaj, yalnızca zaten algılandığında veya açıkça yapılandırılmış anahtar ise metod çağrılmadığı ' dir. Kurallarının bir listesini Entity Framework tarafından kullanılan kullanılabilir buraya: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;
using System.Linq;  

// Convention to detect primary key properties.
// Recognized naming patterns in order of precedence are:
// 1. 'Key'
// 2. [type name]Key
// Primary key detection is case insensitive.
public class CustomKeyDiscoveryConvention : KeyDiscoveryConvention
{
    private const string Id = "Key";

    protected override IEnumerable<EdmProperty> MatchKeyProperty(
        EntityType entityType, IEnumerable<EdmProperty> primitiveProperties)
    {
        Debug.Assert(entityType != null);
        Debug.Assert(primitiveProperties != null);

        var matches = primitiveProperties
            .Where(p => Id.Equals(p.Name, StringComparison.OrdinalIgnoreCase));

        if (!matches.Any())
       {
            matches = primitiveProperties
                .Where(p => (entityType.Name + Id).Equals(p.Name, StringComparison.OrdinalIgnoreCase));
        }

        // If the number of matches is more than one, then multiple properties matched differing only by
        // case--for example, "Key" and "key".  
        if (matches.Count() > 1)
        {
            throw new InvalidOperationException("Multiple properties match the key convention");
        }

        return matches;
    }
}
```  

Ardından sunduğumuz yeni kural önce mevcut anahtar kuralı eklemek ihtiyacımız var. Biz CustomKeyDiscoveryConvention ekledikten sonra IdKeyDiscoveryConvention kaldırabiliriz.  Biz bu yana ilk kez, ancak "anahtarını" özellik bulunduğu çalışması çalıştırın bu kuralı hala öncelik kimliği bulma kuralı götürecek mevcut IdKeyDiscoveryConvention kaldırmadı "id" kuralı çalışacaktır.  Her kural, böylece Örneğin, önceki kuralı bir sütun adı, bir şey eşleşecek şekilde güncelleştirildi (yerine üzerinde bağımsız olarak çalışan ve tüm birlikte birleştirilmeye) önceki Kural gereği güncelleştirilmiş gibi model gördüğünden bu davranışı görüyoruz. ilgi alanlarına, özel kuralı (önce adı ilgi bulunmadığında) sonra bu sütun için geçerli olur.  

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new CustomKeyDiscoveryConvention());
        modelBuilder.Conventions.Remove<IdKeyDiscoveryConvention>();
    }
}
```  

## <a name="notes"></a>Notlar  

Entity Framework tarafından uygulanmış kuralları listesini burada MSDN belgelerinde bulunabilir: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  Bu liste, doğrudan bizim kaynak koddan çekilir.  Entity Framework 6 için kaynak kodu kullanılabilir [GitHub](https://github.com/aspnet/entityframework6/) ve Entity Framework tarafından kullanılan kurallara birçoğu iyi başlangıç noktaları için özel model tabanlı kuralları.  
