---
title: Model tabanlı kurallar-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: c873e9a216bd9bd1934f2149ae6af602072f3608
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419173"
---
# <a name="model-based-conventions"></a><span data-ttu-id="7bcde-102">Model tabanlı kurallar</span><span class="sxs-lookup"><span data-stu-id="7bcde-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="7bcde-103">**Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="7bcde-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="7bcde-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="7bcde-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="7bcde-105">Model tabanlı kurallar, kural tabanlı model yapılandırması Gelişmiş yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="7bcde-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="7bcde-106">Çoğu senaryoda, [DbModelBuilder 'Daki özel Code First kuralı API 'si](~/ef6/modeling/code-first/conventions/custom.md) kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="7bcde-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="7bcde-107">Model tabanlı kurallar kullanılmadan önce, kurallar için DbModelBuilder API 'sinin anlaşılmasına önerilir.</span><span class="sxs-lookup"><span data-stu-id="7bcde-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="7bcde-108">Model tabanlı kurallar, standart kurallar aracılığıyla yapılandırılamayan özellikleri ve tabloları etkileyen kuralların oluşturulmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="7bcde-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="7bcde-109">Bunların örnekleri, hiyerarşi modelleri ve bağımsız Ilişkilendirme sütunları için tablo 'da ayrıştırıcı sütunlarıdır.</span><span class="sxs-lookup"><span data-stu-id="7bcde-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="7bcde-110">Kural oluşturma</span><span class="sxs-lookup"><span data-stu-id="7bcde-110">Creating a Convention</span></span>   

<span data-ttu-id="7bcde-111">Model tabanlı bir kural oluşturmanın ilk adımı, işlem hattının modele uygulanması gereken durumlarda tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="7bcde-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="7bcde-112">İki tür model kuralı vardır, kavramsal (C-Space) ve mağaza (S-Space).</span><span class="sxs-lookup"><span data-stu-id="7bcde-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="7bcde-113">Bir C-Space kuralı, uygulamanın oluşturduğu modele uygulanmış olmasına karşın, veritabanını temsil eden modelin sürümüne uygulanan bir S-Space kuralı ve otomatik olarak oluşturulan sütunların nasıl adlandırıldığı gibi öğeleri denetler.</span><span class="sxs-lookup"><span data-stu-id="7bcde-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="7bcde-114">Model kuralı, ıceptualmodelconvention veya ıtoremodelconvention öğesinden genişleyen bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="7bcde-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="7bcde-115">Bu arabirimler her ikisi de, kuralın uygulandığı veri türünü filtrelemek için kullanılan MetadataItem türünde olabilecek genel bir türü kabul eder.</span><span class="sxs-lookup"><span data-stu-id="7bcde-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="7bcde-116">Kural ekleme</span><span class="sxs-lookup"><span data-stu-id="7bcde-116">Adding a Convention</span></span>   

<span data-ttu-id="7bcde-117">Model kuralları normal kural sınıflarıyla aynı şekilde eklenir.</span><span class="sxs-lookup"><span data-stu-id="7bcde-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="7bcde-118">**Onmodeloluþturma** yönteminde, kuralı bir modelin kuralları listesine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7bcde-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

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

<span data-ttu-id="7bcde-119">Ayrıca, kural. AddBefore\<\> veya kuralları. Addadfter\<\> yöntemler kullanılarak başka bir kurala göre de bir kural eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="7bcde-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="7bcde-120">Entity Framework geçerli olan kurallar hakkında daha fazla bilgi için Notlar bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="7bcde-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="7bcde-121">Örnek: ayrıştırıcı model kuralı</span><span class="sxs-lookup"><span data-stu-id="7bcde-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="7bcde-122">EF tarafından oluşturulan sütunların yeniden adlandırılması, diğer kurallar API 'Leri ile yapaamıyoruz bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="7bcde-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="7bcde-123">Bu durum model kurallarının kullanılması tek seçenektir.</span><span class="sxs-lookup"><span data-stu-id="7bcde-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="7bcde-124">Oluşturulan sütunları yapılandırmak için model tabanlı bir kural kullanmanın bir örneği, Ayrıştırıcı sütunlarının adlandırılma şeklini özelleştirirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7bcde-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="7bcde-125">Aşağıda, "ayrıştırıcı" adlı modeldeki her sütunu "EntityType" olarak yeniden adlandıran basit model temelli bir kural örneği verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7bcde-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="7bcde-126">Bu, geliştiricinin basitçe "ayrıştırıcı" olarak adlandırılan sütunları içerir.</span><span class="sxs-lookup"><span data-stu-id="7bcde-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="7bcde-127">"Ayrıştırıcı" sütunu oluşturulmuş bir sütun olduğundan, bu sütun S-Space içinde çalıştırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="7bcde-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

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

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="7bcde-128">Örnek: genel IA yeniden adlandırma kuralı</span><span class="sxs-lookup"><span data-stu-id="7bcde-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="7bcde-129">Model tabanlı kuralların işlem içindeki başka bir karmaşık örneği, bağımsız Ilişkilerin (IAS) adlandırılma şeklini yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="7bcde-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="7bcde-130">Bu durum, API 'nin EF tarafından oluşturulduğundan ve DbModelBuilder API 'sinin erişebileceği modelde mevcut olmadığından model kurallarının geçerli olduğu bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="7bcde-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="7bcde-131">EF bir IA oluşturduğunda, EntityType_KeyName adlı bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7bcde-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="7bcde-132">Örneğin, MüşteriNo adlı bir anahtar sütunu olan müşteri adlı bir ilişki için, Customer_CustomerId adlı bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7bcde-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="7bcde-133">Aşağıdaki kural, '\_' karakterini, IA için oluşturulan sütun adından kaldırır.</span><span class="sxs-lookup"><span data-stu-id="7bcde-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

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

## <a name="extending-existing-conventions"></a><span data-ttu-id="7bcde-134">Mevcut kuralları genişletme</span><span class="sxs-lookup"><span data-stu-id="7bcde-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="7bcde-135">Modelinize Entity Framework zaten geçerli olan kurallardan birine benzer bir kural yazmanız gerekiyorsa, sıfırdan yeniden yazmak zorunda kalmamak için her zaman bu kuralı genişletebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7bcde-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="7bcde-136">Buna bir örnek, var olan kimlik eşleştirme kuralını özel bir ile değiştirmek için bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="7bcde-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="7bcde-137">Anahtar kuralını geçersiz kılma avantajı, geçersiz kılınan yöntemin yalnızca önceden algılanan ve açıkça yapılandırılmış bir anahtar olmadığında çağrılacaktır.</span><span class="sxs-lookup"><span data-stu-id="7bcde-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="7bcde-138">Entity Framework tarafından kullanılan kuralların listesi şurada bulunabilir: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="7bcde-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

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

<span data-ttu-id="7bcde-139">Daha sonra var olan anahtar kuralına göre yeni kuralımızı eklememiz gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="7bcde-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="7bcde-140">CustomKeyDiscoveryConvention eklendikten sonra ıdkeydiscoveryconvention ' ı kaldırabiliriz.</span><span class="sxs-lookup"><span data-stu-id="7bcde-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="7bcde-141">Mevcut ıdkeydiscoveryconvention ' ı kaldırmadıysanız, ilk olarak çalıştırıldığı için bu kural kimlik bulma kuralına göre önceliklidir, ancak hiçbir "anahtar" özelliği bulunamadığı takdirde "kimlik" kuralı çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="7bcde-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="7bcde-142">Bu davranış, her bir kural, modeli önceki kural tarafından güncelleştirilmiş (bağımsız olarak ve tamamen birlikte çalışmak yerine) bir önceki kurala göre gördüğü için, örneğin önceki bir kural bir sütun adını Özel kuralınızın ilgisini çeken (adın ne zaman ilgilenilmediğine göre), bu sütun için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="7bcde-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

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

## <a name="notes"></a><span data-ttu-id="7bcde-143">Notlar</span><span class="sxs-lookup"><span data-stu-id="7bcde-143">Notes</span></span>  

<span data-ttu-id="7bcde-144">Şu anda Entity Framework tarafından uygulanan kuralların listesi, MSDN belgelerinde şu adreste bulunabilir: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="7bcde-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="7bcde-145">Bu liste doğrudan kaynak kodumuza çekilir.</span><span class="sxs-lookup"><span data-stu-id="7bcde-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="7bcde-146">Entity Framework 6 ' nın kaynak kodu [GitHub](https://github.com/aspnet/entityframework6/) ' da kullanılabilir ve Entity Framework tarafından kullanılan kuralların birçoğu özel model tabanlı kurallar için iyi başlangıç noktalarıdır.</span><span class="sxs-lookup"><span data-stu-id="7bcde-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
