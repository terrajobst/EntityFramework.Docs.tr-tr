---
title: Model tabanlı kuralları - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: fb79164f71cb3afff705a83f5078a13d043abca8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490941"
---
# <a name="model-based-conventions"></a><span data-ttu-id="42067-102">Model tabanlı kuralları</span><span class="sxs-lookup"><span data-stu-id="42067-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="42067-103">**EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="42067-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="42067-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="42067-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="42067-105">Model tabanlı, Gelişmiş bir yöntem tabanlı model yapılandırmasının kurallardır.</span><span class="sxs-lookup"><span data-stu-id="42067-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="42067-106">Çoğu senaryo için [DbModelBuilder özel kod ilk kuralı API](~/ef6/modeling/code-first/conventions/custom.md) kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="42067-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="42067-107">Dayalı modeli kuralları kullanmadan önce bir anlayış DbModelBuilder API kuralları için önerilir.</span><span class="sxs-lookup"><span data-stu-id="42067-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="42067-108">Model tabanlı kuralları özellikleri ve standart kuralları ile yapılandırılabilir olmayan tablolar etkileyen kurallarının oluşturmasına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="42067-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="42067-109">Bu tablo başına hiyerarşi modelleri ayrıştırıcı sütunlarında ve bağımsız ilişkilendirme sütunları örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="42067-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="42067-110">Bir kuralı oluşturma</span><span class="sxs-lookup"><span data-stu-id="42067-110">Creating a Convention</span></span>   

<span data-ttu-id="42067-111">İşlem hattı, modele uygulanacak kuralı gerektiğinde dayalı modeli kuralı oluşturmanın ilk adımı seçmektir.</span><span class="sxs-lookup"><span data-stu-id="42067-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="42067-112">Model kuralları, kavramsal (C-Space) ve Store (S-alan) iki tür vardır.</span><span class="sxs-lookup"><span data-stu-id="42067-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="42067-113">Bir C alan kuralı uygulama oluşturan veritabanı temsil eden model sürümüne uygulanan S alan kuralı ise ve nasıl otomatik olarak oluşturulan sütunları gibi denetimleri şeyler adlı modele uygulanır.</span><span class="sxs-lookup"><span data-stu-id="42067-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="42067-114">Bir model kuralı IConceptualModelConvention veya IStoreModelConvention genişleten bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="42067-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="42067-115">Bu arabirimlerin her ikisi de olabilir genel bir tür kabul kuralı geçerli veri türünü filtrelemek için kullanılan MetadataItem yazın.</span><span class="sxs-lookup"><span data-stu-id="42067-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="42067-116">Bir kural ekleme</span><span class="sxs-lookup"><span data-stu-id="42067-116">Adding a Convention</span></span>   

<span data-ttu-id="42067-117">Model kuralları normal kuralları sınıflar olarak aynı şekilde eklenir.</span><span class="sxs-lookup"><span data-stu-id="42067-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="42067-118">İçinde **OnModelCreating** yöntemi, bir model için kuralları listesi kuralı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="42067-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

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

<span data-ttu-id="42067-119">Bir kuralı Conventions.AddBefore kullanarak başka bir kural ile ilgili olarak da eklenebilir\< \> veya Conventions.AddAfter\< \> yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="42067-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="42067-120">Entity Framework uygulanan kuralları hakkında daha fazla bilgi için Notlar bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="42067-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="42067-121">Örnek: Ayrıştırıcı Model kuralı</span><span class="sxs-lookup"><span data-stu-id="42067-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="42067-122">EF tarafından oluşturulan sütunları yeniden adlandırma, diğer kuralları ile API'leri yapamayacağı bir şeyin bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="42067-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="42067-123">Bu bir durumdur modeli kurallarını kullanarak tek seçeneğiniz olduğu.</span><span class="sxs-lookup"><span data-stu-id="42067-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="42067-124">Oluşturulan sütunları yapılandırmak için bir model tabanlı kuralı kullanma örneği, ayrıştırıcı sütunları adlı biçimi özelleştirildiğinde.</span><span class="sxs-lookup"><span data-stu-id="42067-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="42067-125">Her sütunda "Ayrıştırıcı" adlı model "EntityType için" yeniden adlandırır bir dayalı basit model kuralı örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="42067-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="42067-126">Bu, geliştirici yalnızca "Ayrıştırıcı" adlı sütun içerir.</span><span class="sxs-lookup"><span data-stu-id="42067-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="42067-127">"Ayrıştırıcı" sütunu oluşturulan bir sütun olduğundan bu S alanda çalışan gerekir.</span><span class="sxs-lookup"><span data-stu-id="42067-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

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

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="42067-128">Örnek: Genel IA kuralı yeniden adlandırma</span><span class="sxs-lookup"><span data-stu-id="42067-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="42067-129">Başka bir daha karmaşık eylem dayalı modeli kuralları bağımsız ilişkilendirmeleri (IAS) adlandırdığınız şekilde yapılandırmak için örneğidir.</span><span class="sxs-lookup"><span data-stu-id="42067-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="42067-130">Bu Model kuralları burada IAS oluşturulur çünkü EF tarafından uygulanabilir ve DbModelBuilder API erişimi olan modelde olmayan bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="42067-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="42067-131">EF bir IA oluşturduğunda EntityType_KeyName adlı bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="42067-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="42067-132">Örneğin, müşteri adlı bir anahtar sütunu ile bir ilişki Customer_CustomerId adlı bir sütun oluşturmak CustomerID adlı.</span><span class="sxs-lookup"><span data-stu-id="42067-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="42067-133">Aşağıdaki kural şeritler '\_' karakteri dışında IA. için oluşturulan sütun adı</span><span class="sxs-lookup"><span data-stu-id="42067-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

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

## <a name="extending-existing-conventions"></a><span data-ttu-id="42067-134">Var olan kuralları genişletme</span><span class="sxs-lookup"><span data-stu-id="42067-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="42067-135">Entity Framework modelinize zaten uygulanan kuralları birini benzer bir kuralı yazmak gerekiyorsa, her zaman sıfırdan yeniden yazmak zorunda kalmamak için kuralı genişletebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="42067-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="42067-136">Buna örnek olarak mevcut kimliği ile özel bir kural eşleşen değiştirmektir.</span><span class="sxs-lookup"><span data-stu-id="42067-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="42067-137">Anahtar kuralı geçersiz kılma için ek bir avantaj, yalnızca zaten algılandığında veya açıkça yapılandırılmış anahtar ise metod çağrılmadığı ' dir.</span><span class="sxs-lookup"><span data-stu-id="42067-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="42067-138">Kurallarının bir listesini Entity Framework tarafından kullanılan kullanılabilir buraya: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="42067-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

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

<span data-ttu-id="42067-139">Ardından sunduğumuz yeni kural önce mevcut anahtar kuralı eklemek ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="42067-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="42067-140">Biz CustomKeyDiscoveryConvention ekledikten sonra IdKeyDiscoveryConvention kaldırabiliriz.</span><span class="sxs-lookup"><span data-stu-id="42067-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="42067-141">Biz bu yana ilk kez, ancak "anahtarını" özellik bulunduğu çalışması çalıştırın bu kuralı hala öncelik kimliği bulma kuralı götürecek mevcut IdKeyDiscoveryConvention kaldırmadı "id" kuralı çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="42067-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="42067-142">Her kural, böylece Örneğin, önceki kuralı bir sütun adı, bir şey eşleşecek şekilde güncelleştirildi (yerine üzerinde bağımsız olarak çalışan ve tüm birlikte birleştirilmeye) önceki Kural gereği güncelleştirilmiş gibi model gördüğünden bu davranışı görüyoruz. ilgi alanlarına, özel kuralı (önce adı ilgi bulunmadığında) sonra bu sütun için geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="42067-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

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

## <a name="notes"></a><span data-ttu-id="42067-143">Notlar</span><span class="sxs-lookup"><span data-stu-id="42067-143">Notes</span></span>  

<span data-ttu-id="42067-144">Entity Framework tarafından uygulanmış kuralları listesini burada MSDN belgelerinde bulunabilir: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="42067-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="42067-145">Bu liste, doğrudan bizim kaynak koddan çekilir.</span><span class="sxs-lookup"><span data-stu-id="42067-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="42067-146">Entity Framework 6 için kaynak kodu kullanılabilir [GitHub](https://github.com/aspnet/entityframework6/) ve Entity Framework tarafından kullanılan kurallara birçoğu iyi başlangıç noktaları için özel model tabanlı kuralları.</span><span class="sxs-lookup"><span data-stu-id="42067-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
