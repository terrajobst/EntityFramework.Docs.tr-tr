---
title: -Yapılandırma ve özellikler ve türler eşleme - Fluent API'si EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
caps.latest.revision: 3
ms.openlocfilehash: ec8b484433d13899a88f44e37823dd1a4bed6530
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914309"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="91833-102">Fluent API'si - özellikler ve türler eşleme ve yapılandırma</span><span class="sxs-lookup"><span data-stu-id="91833-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="91833-103">Entity Framework Code First ile çalışırken tablolarına desteklenmiş EF kuralları kümesi kullanarak POCO sınıflarınızı eşlemek için varsayılan davranış.</span><span class="sxs-lookup"><span data-stu-id="91833-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="91833-104">Bazı durumlarda, ancak olamaz veya bu kuralları izleyin ve hangi kuralları dikte dışında bir şey varlıkları eşlemeniz istemediğiniz.</span><span class="sxs-lookup"><span data-stu-id="91833-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="91833-105">Yapılandırma kuralları dışında bir şey yani kullanılacak EF başlıca iki yolu vardır [ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md) veya EFs fluent API'si.</span><span class="sxs-lookup"><span data-stu-id="91833-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="91833-106">Ek açıklamalar kullanarak elde edilemeyecek eşleme senaryoları şekilde ek açıklamalar yalnızca fluent API'si işlevlerinin bir alt kümesini kapsar.</span><span class="sxs-lookup"><span data-stu-id="91833-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="91833-107">Bu makalede, fluent API'si özellikleri yapılandırmak için nasıl kullanılacağını göstermek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="91833-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="91833-108">Kod ilk fluent API'si geçersiz kılarak en sık erişilen [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) yöntemi türetilmiş [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="91833-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="91833-109">Aşağıdaki örnekler fluent API'si ile çeşitli görevleri gerçekleştirmek ve kodu kopyalayın ve sahip olarak kullanılabilmesi için modeli görmek istiyorsanız, modelinizi uyacak şekilde özelleştirmenize izin vermek nasıl göstermek için tasarlanmıştır-sonra bu makalenin sonunda sağlanır.</span><span class="sxs-lookup"><span data-stu-id="91833-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="91833-110">Model genelindeki ayarları</span><span class="sxs-lookup"><span data-stu-id="91833-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="91833-111">Varsayılan şema (EF6 sonrası)</span><span class="sxs-lookup"><span data-stu-id="91833-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="91833-112">EF6 ile başlangıç HasDefaultSchema yöntemi, tüm tabloları, saklı yordamlar, vb. için kullanılacak veritabanı şemasını belirtmek için DbModelBuilder üzerinde kullanabilirsiniz. Bu varsayılan ayar için farklı bir şeması açıkça yapılandırdığınız herhangi bir nesne için geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="91833-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="91833-113">Özel kuralları (EF6 sonrası)</span><span class="sxs-lookup"><span data-stu-id="91833-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="91833-114">Code First bulunan desteklemek için kendi kurallarına oluşturabilirsiniz EF6 ile'başlatılıyor.</span><span class="sxs-lookup"><span data-stu-id="91833-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="91833-115">Daha fazla ayrıntı için [özel kod öncelikli kurallar](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="91833-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="91833-116">Özellik eşleme</span><span class="sxs-lookup"><span data-stu-id="91833-116">Property Mapping</span></span>  

<span data-ttu-id="91833-117">[Özelliği](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) yöntemi, bir varlığı veya karmaşık türün ait her bir özellik için öznitelikleri yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91833-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="91833-118">Özellik yöntemi, belirli bir özellik için bir yapılandırma nesnesi elde etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91833-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="91833-119">Yapılandırma nesnesini seçenekleri yapılandırılmakta türüne özeldir; Örneğin, yalnızca dize özellikleri IsUnicode kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="91833-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="91833-120">Bir birincil anahtar yapılandırma</span><span class="sxs-lookup"><span data-stu-id="91833-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="91833-121">Birincil anahtarlar için Entity Framework kuralıdır:</span><span class="sxs-lookup"><span data-stu-id="91833-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="91833-122">Sınıfınıza "ID" veya "Id" adı olan bir özelliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="91833-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="91833-123">veya bir sınıf adının ardından "ID" veya "Id"</span><span class="sxs-lookup"><span data-stu-id="91833-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="91833-124">Bir birincil anahtar olmasını özellik açıkça ayarlamak için HasKey yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91833-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="91833-125">Aşağıdaki örnekte, HasKey yöntemi Instructorıd birincil anahtarın OfficeAssignment türüne göre yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91833-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="91833-126">Bileşik bir birincil anahtar yapılandırma</span><span class="sxs-lookup"><span data-stu-id="91833-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="91833-127">Aşağıdaki örnek, bileşik bir birincil anahtar bölüm türü olmasını DepartmentID ve ad özelliklerini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="91833-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="91833-128">Kimliğin oturumunu için sayısal birincil anahtarları değiştirme</span><span class="sxs-lookup"><span data-stu-id="91833-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="91833-129">Aşağıdaki örnek değeri veritabanı tarafından oluşturulmaz belirtmek için System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None DepartmentID özelliğini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="91833-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="91833-130">En fazla uzunluğu bir özellikte belirtme</span><span class="sxs-lookup"><span data-stu-id="91833-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="91833-131">Aşağıdaki örnekte, Name özelliği 50 karakterden uzun olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="91833-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="91833-132">Değer 50 karakterden uzun yaparsanız erişmenizi sağlayacak bir [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) özel durum.</span><span class="sxs-lookup"><span data-stu-id="91833-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="91833-133">Code First bir veritabanı bu modelden oluşturması halinde, ad sütununun uzunluğu en fazla 50 karakter olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="91833-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="91833-134">Özelliği gerekli olacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="91833-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="91833-135">Aşağıdaki örnekte, Name özelliği gereklidir.</span><span class="sxs-lookup"><span data-stu-id="91833-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="91833-136">Adı belirtmezseniz, DbEntityValidationException özel durum alırsınız.</span><span class="sxs-lookup"><span data-stu-id="91833-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="91833-137">Bu modelden Code First bir veritabanı oluşturur, ardından bu özellik depolamak için kullanılan sütun genellikle atanamayan olacaktır.</span><span class="sxs-lookup"><span data-stu-id="91833-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="91833-138">Bazı durumlarda veritabanını özelliği gerekli olsa bile null yapılamaz sütunda mümkün olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="91833-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="91833-139">Örneğin, ne zaman TPH devralma stratejisi veri için birden fazla türü kullanılarak tek bir tabloda depolanır.</span><span class="sxs-lookup"><span data-stu-id="91833-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="91833-140">Gerekli bir özellik türetilmiş bir tür içeriyorsa, bu özellik hiyerarşideki tüm türleri olduğundan sütun atanamayan yapılamaz.</span><span class="sxs-lookup"><span data-stu-id="91833-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="91833-141">Bir dizin üzerinde bir veya daha fazla özelliklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="91833-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="91833-142">**EF6.1 ve sonraki sürümler yalnızca** -Entity Framework 6.1 içinde dizin özniteliği tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="91833-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="91833-143">Önceki bir sürümü kullanıyorsanız, bu bölümdeki bilgiler, geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="91833-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="91833-144">Dizinler oluşturma Fluent API'si tarafından yerel olarak desteklenmiyor ancak yapabileceğiniz desteği kullanım **IndexAttribute** Fluent API'si üzerinden.</span><span class="sxs-lookup"><span data-stu-id="91833-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="91833-145">Dizin öznitelikleri, bir model ek açıklama sonra işlem hattını veritabanında daha sonra bir dizinde açık bir modeli de dahil olmak üzere tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="91833-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="91833-146">Bu ek açıklamaları Fluent API'sini kullanarak el ile ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91833-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="91833-147">Bunu yapmanın en kolay yolu bir örneği oluşturmaktır **IndexAttribute** , yeni bir dizin için tüm ayarları içerir.</span><span class="sxs-lookup"><span data-stu-id="91833-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="91833-148">Daha sonra bir örneğini oluşturabilirsiniz **IndexAnnotation** dönüştürecek bir EF belirli türü **IndexAttribute** EF model üzerinde depolanan bir model ek açıklama olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="91833-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="91833-149">Bunlar ardından geçirilebilir **HasColumnAnnotation** yöntemi Fluent adını belirterek API üzerinde **dizin** ek açıklama için.</span><span class="sxs-lookup"><span data-stu-id="91833-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="91833-150">Kullanılabilir ayarların tam listesi için **IndexAttribute**, bkz: *dizin* bölümünü [kod ilk veri ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md).</span><span class="sxs-lookup"><span data-stu-id="91833-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="91833-151">Bu, dizin adı özelleştirme, benzersiz dizinler oluşturma ve birden çok sütun dizinleri oluşturma içerir.</span><span class="sxs-lookup"><span data-stu-id="91833-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="91833-152">Tek bir özellikte birden çok dizin ek açıklamaları dizisi geçirerek belirtebilirsiniz **IndexAttribute** oluşturucusuna **IndexAnnotation**.</span><span class="sxs-lookup"><span data-stu-id="91833-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="91833-153">Bir CLR özelliği veritabanında bir sütun değil eşlemeye belirtme</span><span class="sxs-lookup"><span data-stu-id="91833-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="91833-154">Aşağıdaki örnek, bir CLR türünün bir özelliği, bir veritabanı sütununa eşlenmemiş belirtmek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="91833-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="91833-155">Belirli bir sütuna veritabanında bir CLR özellik eşlemesi</span><span class="sxs-lookup"><span data-stu-id="91833-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="91833-156">Aşağıdaki örnek adı CLR özelliği DepartmentName veritabanı sütununa eşler.</span><span class="sxs-lookup"><span data-stu-id="91833-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="91833-157">Modelde tanımlı olmayan bir yabancı anahtar yeniden adlandırma</span><span class="sxs-lookup"><span data-stu-id="91833-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="91833-158">Değil bir CLR türüne bir yabancı anahtar tanımlayın, ancak veritabanında olması gereken hangi adını belirtmek istediğiniz seçerseniz, aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="91833-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="91833-159">Bir dize özelliğini Unicode içeriği destekleyip desteklemediğini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="91833-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="91833-160">Varsayılan olarak, Unicode (SQL Server'da nvarchar) dizelerdir.</span><span class="sxs-lookup"><span data-stu-id="91833-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="91833-161">IsUnicode yöntemi, bir dize varchar türü olması belirtmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91833-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="91833-162">Veritabanı sütununun veri türü yapılandırılıyor</span><span class="sxs-lookup"><span data-stu-id="91833-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="91833-163">[HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) yöntemi aynı temel türden farklı temsilleri eşleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="91833-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="91833-164">Bu yöntemi kullanarak, çalışma zamanında verilerin herhangi bir dönüştürme gerçekleştirmek izin vermez.</span><span class="sxs-lookup"><span data-stu-id="91833-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="91833-165">Veritabanı belirsiz olduğundan IsUnicode varchar, tercih edilen şekilde ayarı sütun olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="91833-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="91833-166">Karmaşık bir türde özelliklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="91833-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="91833-167">Karmaşık bir türde skaler özellikler yapılandırmanın iki yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="91833-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="91833-168">Üzerinde ComplexTypeConfiguration özelliğini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91833-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="91833-169">Nokta gösterimi, karmaşık bir türün bir özelliğe erişmek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91833-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="91833-170">Bir iyimser eşzamanlılık belirteci olarak kullanılacak bir özelliğin yapılandırma</span><span class="sxs-lookup"><span data-stu-id="91833-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="91833-171">Bir varlıktaki bir özelliği bir eşzamanlılık belirteci temsil ettiğini belirlemek için ConcurrencyCheck öznitelikte veya IsConcurrencyToken yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91833-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="91833-172">IsRowVersion yöntemi, özelliği veritabanında bir satır sürümü olacak şekilde yapılandırmak için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91833-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="91833-173">İyimser eşzamanlılık belirteci olması için bir satır sürümü otomatik olarak yapılandırır özelliğini ayarlama.</span><span class="sxs-lookup"><span data-stu-id="91833-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="91833-174">Tür eşlemesi</span><span class="sxs-lookup"><span data-stu-id="91833-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="91833-175">Bir sınıf bir karmaşık tür olduğunu belirleme</span><span class="sxs-lookup"><span data-stu-id="91833-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="91833-176">Kural gereği, belirtilen birincil anahtarı olmayan bir türü bir karmaşık tür olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="91833-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="91833-177">Burada Code First bir karmaşık türü (örneğin, kimliği adlı bir özelliğe sahip, ancak bunun için bir birincil anahtar olmasını gelmez varsa) algılamaz bazı senaryolar vardır.</span><span class="sxs-lookup"><span data-stu-id="91833-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="91833-178">Böyle durumlarda, bir türü karmaşık bir tür olduğunu açıkça belirtmek için fluent API'sini kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="91833-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="91833-179">CLR varlık türü için veritabanında bir tablo değil eşlenecek belirtme</span><span class="sxs-lookup"><span data-stu-id="91833-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="91833-180">Aşağıdaki örnek, veritabanındaki bir tabloda eşlenen bir CLR türü dışlanacak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="91833-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="91833-181">Bir varlık türü veritabanında belirli bir tablo eşleme</span><span class="sxs-lookup"><span data-stu-id="91833-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="91833-182">Bölüm tüm özellikleri, t_ Departman adlı bir tablodaki sütunları eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="91833-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="91833-183">Bu gibi şema adı da belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="91833-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="91833-184">Tablo-başına-hiyerarşi (TPH) devralma eşleme</span><span class="sxs-lookup"><span data-stu-id="91833-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="91833-185">TPH eşleme senaryosunda, tek bir tabloya bir devralma hiyerarşisindeki tüm türleri eşlenir.</span><span class="sxs-lookup"><span data-stu-id="91833-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="91833-186">Bir Ayrıştırıcı sütunu, her satır türünü tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91833-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="91833-187">Modelinizi Code First ile oluştururken TPH devralma hiyerarşisinde katılan türleri için varsayılan stratejisidir.</span><span class="sxs-lookup"><span data-stu-id="91833-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="91833-188">Varsayılan olarak, "Ayrıştırıcı" adlı bir tablo ayrıştırıcı sütunu eklenir ve hiyerarşideki her bir türü CLR tür adını ayrıştırıcı değerleri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91833-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="91833-189">Fluent API'sini kullanarak varsayılan davranışını değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91833-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="91833-190">Tablo başına tür (TPT) devralma eşleme</span><span class="sxs-lookup"><span data-stu-id="91833-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="91833-191">TPT eşleme senaryosunda, tek tek tablolar için tüm türleri eşlenir.</span><span class="sxs-lookup"><span data-stu-id="91833-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="91833-192">Yalnızca bir temel tür veya türetilmiş bir tür ait özellikler bu türüyle eşleyen bir tabloda depolanır.</span><span class="sxs-lookup"><span data-stu-id="91833-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="91833-193">Türetilmiş türleri eşleyen tablo türetilmiş tablonun temel tablosu ile birleştiren bir yabancı anahtar da depolar.</span><span class="sxs-lookup"><span data-stu-id="91833-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="91833-194">Tablo başına somut sınıf (TPC) devralma eşleme</span><span class="sxs-lookup"><span data-stu-id="91833-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="91833-195">TPC eşleme senaryosunda, tek tek tablolar için hiyerarşideki tüm soyut olmayan türleri eşlenir.</span><span class="sxs-lookup"><span data-stu-id="91833-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="91833-196">Türetilmiş sınıflara eşleme tabloları temel sınıf veritabanında eşleşen tablo hiçbir ilişkisi.</span><span class="sxs-lookup"><span data-stu-id="91833-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="91833-197">Devralınan özellikler dahil olmak üzere, bir sınıfın tüm özellikler için karşılık gelen bir tablonun sütunlarını eşlenir.</span><span class="sxs-lookup"><span data-stu-id="91833-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="91833-198">Her türetilmiş bir tür yapılandırma MapInheritedProperties yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="91833-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="91833-199">MapInheritedProperties temel sınıftan türetilmiş bir sınıf için yeni sütun için devralınan tüm özellikleri yeniden eşlemesi.</span><span class="sxs-lookup"><span data-stu-id="91833-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="91833-200">TPC devralma hiyerarşisinde katılan tablolara değil paylaştığından birincil anahtar var. Yinelenen varlık anahtarları oluşturulan veritabanı değerleri ile aynı Kimlik tohumu varsa, alt sınıflar için eşlenmiş tablolardaki eklerken olacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="91833-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="91833-201">Bu sorunu çözmek için her tablo için farklı başlangıç Çekirdek değer belirtin veya birincil anahtar özelliği kimliği devre dışı geçin.</span><span class="sxs-lookup"><span data-stu-id="91833-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="91833-202">Kimlik, Code First ile çalışırken tamsayı anahtar özellikleri için varsayılan değer.</span><span class="sxs-lookup"><span data-stu-id="91833-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="91833-203">' % S'veritabanı (bölme varlık) birden çok tabloya bir varlık türünün özellikleri eşleme</span><span class="sxs-lookup"><span data-stu-id="91833-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="91833-204">Bölme varlık birden çok tabloda yayılma için bir varlık türünün özelliklerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="91833-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="91833-205">Aşağıdaki örnekte, iki tabloya departmanı varlık bölünür: bölüm ve DepartmentDetails.</span><span class="sxs-lookup"><span data-stu-id="91833-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="91833-206">Varlık bölme, belirli bir tabloya bir özellik alt kümesi eşlemek için birden çok çağrı harita yöntemi kullanır.</span><span class="sxs-lookup"><span data-stu-id="91833-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="91833-207">Birden çok varlık türleri (bölme tablosu) veritabanında bir tablo eşleme</span><span class="sxs-lookup"><span data-stu-id="91833-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="91833-208">Aşağıdaki örnek, bir tablonun birincil anahtara paylaşan iki varlık türleri eşler.</span><span class="sxs-lookup"><span data-stu-id="91833-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="91833-209">Insert/Update/Delete saklı yordamlar (EF6 sonrası) için bir varlık türü eşleme</span><span class="sxs-lookup"><span data-stu-id="91833-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="91833-210">EF6 ile başlangıç, saklı yordamlar için ekleme güncelleştirme ve silme varlığın eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91833-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="91833-211">Daha fazla bilgi için [kod ilk Insert/Update/Delete saklı yordamlar](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span><span class="sxs-lookup"><span data-stu-id="91833-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="91833-212">Örneklerde kullanılan modeli</span><span class="sxs-lookup"><span data-stu-id="91833-212">Model Used in Samples</span></span>  

<span data-ttu-id="91833-213">Bu sayfadaki örnekler için aşağıdaki Code First modeli kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91833-213">The following Code First model is used for the samples on this page.</span></span>  

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
