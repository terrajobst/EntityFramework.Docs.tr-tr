---
title: Model tabanlı kurallar-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: c873e9a216bd9bd1934f2149ae6af602072f3608
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656163"
---
# <a name="model-based-conventions"></a>Model tabanlı kurallar
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.  

Model tabanlı kurallar, kural tabanlı model yapılandırması Gelişmiş yöntemidir. Çoğu senaryoda, [DbModelBuilder 'Daki özel Code First kuralı API 'si](~/ef6/modeling/code-first/conventions/custom.md) kullanılmalıdır. Model tabanlı kurallar kullanılmadan önce, kurallar için DbModelBuilder API 'sinin anlaşılmasına önerilir.  

Model tabanlı kurallar, standart kurallar aracılığıyla yapılandırılamayan özellikleri ve tabloları etkileyen kuralların oluşturulmasına izin verir. Bunların örnekleri, hiyerarşi modelleri ve bağımsız Ilişkilendirme sütunları için tablo 'da ayrıştırıcı sütunlarıdır.  

## <a name="creating-a-convention"></a>Kural oluşturma   

Model tabanlı bir kural oluşturmanın ilk adımı, işlem hattının modele uygulanması gereken durumlarda tercih edilir. İki tür model kuralı vardır, kavramsal (C-Space) ve mağaza (S-Space). Bir C-Space kuralı, uygulamanın oluşturduğu modele uygulanmış olmasına karşın, veritabanını temsil eden modelin sürümüne uygulanan bir S-Space kuralı ve otomatik olarak oluşturulan sütunların nasıl adlandırıldığı gibi öğeleri denetler.  

Model kuralı, ıceptualmodelconvention veya ıtoremodelconvention öğesinden genişleyen bir sınıftır.  Bu arabirimler her ikisi de, kuralın uygulandığı veri türünü filtrelemek için kullanılan MetadataItem türünde olabilecek genel bir türü kabul eder.  

## <a name="adding-a-convention"></a>Kural ekleme   

Model kuralları normal kural sınıflarıyla aynı şekilde eklenir. **Onmodeloluþturma** yönteminde, kuralı bir modelin kuralları listesine ekleyin.  

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

Ayrıca, kural. AddBefore\<\> veya kuralları. Addadfter\<\> yöntemler kullanılarak başka bir kurala göre de bir kural eklenebilir. Entity Framework geçerli olan kurallar hakkında daha fazla bilgi için Notlar bölümüne bakın.  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>Örnek: ayrıştırıcı model kuralı  

EF tarafından oluşturulan sütunların yeniden adlandırılması, diğer kurallar API 'Leri ile yapaamıyoruz bir örnektir.  Bu durum model kurallarının kullanılması tek seçenektir.  

Oluşturulan sütunları yapılandırmak için model tabanlı bir kural kullanmanın bir örneği, Ayrıştırıcı sütunlarının adlandırılma şeklini özelleştirirsiniz.  Aşağıda, "ayrıştırıcı" adlı modeldeki her sütunu "EntityType" olarak yeniden adlandıran basit model temelli bir kural örneği verilmiştir.  Bu, geliştiricinin basitçe "ayrıştırıcı" olarak adlandırılan sütunları içerir. "Ayrıştırıcı" sütunu oluşturulmuş bir sütun olduğundan, bu sütun S-Space içinde çalıştırılmalıdır.  

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

## <a name="example-general-ia-renaming-convention"></a>Örnek: genel IA yeniden adlandırma kuralı   

Model tabanlı kuralların işlem içindeki başka bir karmaşık örneği, bağımsız Ilişkilerin (IAS) adlandırılma şeklini yapılandırmaktır.  Bu durum, API 'nin EF tarafından oluşturulduğundan ve DbModelBuilder API 'sinin erişebileceği modelde mevcut olmadığından model kurallarının geçerli olduğu bir durumdur.  

EF bir IA oluşturduğunda, EntityType_KeyName adlı bir sütun oluşturur. Örneğin, MüşteriNo adlı bir anahtar sütunu olan müşteri adlı bir ilişki için, Customer_CustomerId adlı bir sütun oluşturur. Aşağıdaki kural, '\_' karakterini, IA için oluşturulan sütun adından kaldırır.  

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
        for (int i = 0; i < properties.Count; ++i)
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

## <a name="extending-existing-conventions"></a>Mevcut kuralları genişletme   

Modelinize Entity Framework zaten geçerli olan kurallardan birine benzer bir kural yazmanız gerekiyorsa, sıfırdan yeniden yazmak zorunda kalmamak için her zaman bu kuralı genişletebilirsiniz.  Buna bir örnek, var olan kimlik eşleştirme kuralını özel bir ile değiştirmek için bir örnektir.   Anahtar kuralını geçersiz kılma avantajı, geçersiz kılınan yöntemin yalnızca önceden algılanan ve açıkça yapılandırılmış bir anahtar olmadığında çağrılacaktır. Entity Framework tarafından kullanılan kuralların listesi şurada bulunabilir: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  

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

Daha sonra var olan anahtar kuralına göre yeni kuralımızı eklememiz gerekiyor. CustomKeyDiscoveryConvention eklendikten sonra ıdkeydiscoveryconvention ' ı kaldırabiliriz.  Mevcut ıdkeydiscoveryconvention ' ı kaldırmadıysanız, ilk olarak çalıştırıldığı için bu kural kimlik bulma kuralına göre önceliklidir, ancak hiçbir "anahtar" özelliği bulunamadığı takdirde "kimlik" kuralı çalışacaktır.  Bu davranış, her bir kural, modeli önceki kural tarafından güncelleştirilmiş (bağımsız olarak ve tamamen birlikte çalışmak yerine) bir önceki kurala göre gördüğü için, örneğin önceki bir kural bir sütun adını Özel kuralınızın ilgisini çeken (adın ne zaman ilgilenilmediğine göre), bu sütun için geçerlidir.  

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

Şu anda Entity Framework tarafından uygulanan kuralların listesi, MSDN belgelerinde şu adreste bulunabilir: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  Bu liste doğrudan kaynak kodumuza çekilir.  Entity Framework 6 ' nın kaynak kodu [GitHub](https://github.com/aspnet/entityframework6/) ' da kullanılabilir ve Entity Framework tarafından kullanılan kuralların birçoğu özel model tabanlı kurallar için iyi başlangıç noktalarıdır.  
