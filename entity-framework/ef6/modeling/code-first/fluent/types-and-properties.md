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
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="d4ae8-102">Akıcı API-özellikleri ve türleri yapılandırma ve eşleme</span><span class="sxs-lookup"><span data-stu-id="d4ae8-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="d4ae8-103">Code First Entity Framework ile çalışırken, varsayılan davranış, POCO sınıflarınızı, EF 'e bakılan bir dizi kuralı kullanarak tablo olarak eşlemenize yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="d4ae8-104">Ancak, bazen bu kuralları takip edemez veya bu kuralları izlemek istemiyor, varlıkları kuralların izlediklerinde başka bir şeyle eşleştirmek zorunda değilsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="d4ae8-105">Bir kural dışında bir şey kullanmak için EF 'i yapılandırabileceğiniz iki temel yol vardır, yani [ek açıklamalar](~/ef6/modeling/code-first/data-annotations.md) veya EFS Fluent API.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="d4ae8-106">Ek açıklamalar yalnızca Fluent API işlevselliğinin bir alt kümesini kapsar, bu nedenle ek açıklamalar kullanılarak elde edilemeyecek olan eşleme senaryoları vardır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="d4ae8-107">Bu makale, özellikleri yapılandırmak için Fluent API nasıl kullanacağınızı göstermek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="d4ae8-108">İlk Fluent API kod, türetilmiş [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx)'Teki [onmodeloluþturma](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) yöntemi geçersiz kılınarak en yaygın olarak erişilir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="d4ae8-109">Aşağıdaki örnekler, akıcı API ile çeşitli görevlerin nasıl yapılacağını göstermek ve kodu dışarı kopyalayıp modelinize uyacak şekilde özelleştirmeyi sağlamak üzere tasarlanmıştır. Bu durumda, bu makalenin sonunda verilmiştir. Bu durumda, bu makalenin sonunda sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="d4ae8-110">Model genelindeki ayarlar</span><span class="sxs-lookup"><span data-stu-id="d4ae8-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="d4ae8-111">Varsayılan şema (EF6 onsürümleri)</span><span class="sxs-lookup"><span data-stu-id="d4ae8-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="d4ae8-112">EF6 ile başlayarak, tüm tablolar, saklı yordamlar vb. için kullanılacak veritabanı şemasını belirtmek üzere DbModelBuilder üzerinde HasDefaultSchema yöntemini kullanabilirsiniz. Bu varsayılan ayar, için açıkça farklı bir şema yapılandırdığınız tüm nesneler için geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="d4ae8-113">Özel kurallar (EF6 ve sonraki sürümler)</span><span class="sxs-lookup"><span data-stu-id="d4ae8-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="d4ae8-114">EF6 ile başlayarak, Code First dahil olanlar için kendi kurallarınızı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="d4ae8-115">Daha ayrıntılı bilgi için bkz. [özel Code First kuralları](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="d4ae8-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="d4ae8-116">Özellik Eşleme</span><span class="sxs-lookup"><span data-stu-id="d4ae8-116">Property Mapping</span></span>  

<span data-ttu-id="d4ae8-117">[Özellik](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) yöntemi, bir varlığa veya karmaşık türe ait her bir özelliğin özniteliklerini yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="d4ae8-118">Özellik yöntemi, belirli bir özellik için bir yapılandırma nesnesi elde etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="d4ae8-119">Yapılandırma nesnesindeki seçenekler, yapılandırılmakta olan türe özgüdür; Isunıcode yalnızca dize özelliklerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="d4ae8-120">Birincil anahtar yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d4ae8-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="d4ae8-121">Birincil anahtarlar için Entity Framework kuralı:</span><span class="sxs-lookup"><span data-stu-id="d4ae8-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="d4ae8-122">Sınıfınız, adı "ID" veya "ID" olan bir özellik tanımlar</span><span class="sxs-lookup"><span data-stu-id="d4ae8-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="d4ae8-123">ya da "ID" veya "ID" gelen bir sınıf adı</span><span class="sxs-lookup"><span data-stu-id="d4ae8-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="d4ae8-124">Açıkça bir özelliği birincil anahtar olarak ayarlamak için HasKey yöntemini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="d4ae8-125">Aşağıdaki örnekte, OfficeAssignment türündeki Komutctorıd birincil anahtarını yapılandırmak için HasKey yöntemi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="d4ae8-126">Bileşik birincil anahtar yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d4ae8-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="d4ae8-127">Aşağıdaki örnek, DepartmentID ve ad özelliklerini departman türünün bileşik birincil anahtarı olacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="d4ae8-128">Sayısal birincil anahtarlar için kimlik devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="d4ae8-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="d4ae8-129">Aşağıdaki örnek, değerin veritabanı tarafından üretilmeyeceğini göstermek için DepartmentID özelliğini System. ComponentModel. Dataaçıklamalarda. DatabaseGeneratedOption. None olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="d4ae8-130">Bir özellikte maksimum uzunluğu belirtme</span><span class="sxs-lookup"><span data-stu-id="d4ae8-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="d4ae8-131">Aşağıdaki örnekte, Name özelliği 50 karakterden uzun olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="d4ae8-132">Değeri 50 karakterden daha uzun yaparsanız, [Dbentityvalidationexception](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) özel durumu alırsınız.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="d4ae8-133">Code First bu modelden bir veritabanı oluşturursa, Ad sütununun uzunluk üst sınırını 50 karakter olacak şekilde ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="d4ae8-134">Gerekli olacak özelliği yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d4ae8-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="d4ae8-135">Aşağıdaki örnekte, Name özelliği gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="d4ae8-136">Bu adı belirtmezseniz, bir DbEntityValidationException özel durumu alırsınız.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="d4ae8-137">Code First bu modelden bir veritabanı oluşturursa, bu özelliği depolamak için kullanılan sütun genellikle null atanamaz olur.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="d4ae8-138">Bazı durumlarda, özelliği gerekli olsa bile veritabanındaki sütunun null olmaması mümkün olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="d4ae8-139">Örneğin, birden çok tür için bir TPH devralma stratejisi verisi kullanıldığında tek bir tabloda depolanır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="d4ae8-140">Türetilmiş bir tür gerekli bir özellik içeriyorsa, hiyerarşideki tüm türlerin bu özelliği içermesi olmadığından, sütun null yapılamayan yapılamaz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="d4ae8-141">Bir veya daha fazla özelliklerde dizin yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d4ae8-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="d4ae8-142">**EF 6.1 yalnızca** sonraki sürümler-dizin özniteliği Entity Framework 6,1 ' de tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="d4ae8-143">Önceki bir sürümü kullanıyorsanız, bu bölümdeki bilgiler uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="d4ae8-144">Dizin oluşturma, akıcı API tarafından yerel olarak desteklenmez, ancak akıcı API aracılığıyla **ındexattribute** desteğini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="d4ae8-145">Dizin öznitelikleri, modele daha sonra işlem hattında daha sonra veritabanında bulunan bir model ek açıklaması eklenerek işlenir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="d4ae8-146">Bu ek açıklamaları, akıcı API 'YI kullanarak el ile ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="d4ae8-147">Bunu yapmanın en kolay yolu, yeni dizinin tüm ayarlarını içeren **ındexattribute** öğesinin bir örneğini oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="d4ae8-148">Daha sonra **ındexattribute** ayarlarını EF modelinde depolanabilecek bir model ek açıklamasına DÖNÜŞTÜRECEK bir EF özel türü olan **ındexannotation** öğesinin bir örneğini oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="d4ae8-149">Bunlar daha sonra, ek açıklamanın ad **dizinini** belirterek, akıcı API 'de **Hasccolumnannotation** yöntemine geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="d4ae8-150">**Indexattribute**'da kullanılabilen ayarların tüm listesi Için [Code First veri ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md)' nın *Dizin* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="d4ae8-151">Bu, dizin adını özelleştirmeyi, benzersiz dizinler oluşturmayı ve çok sütunlu dizinler oluşturmayı içerir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="d4ae8-152">Bir **ındexattribute** dizisini **ındexannotation**oluşturucusuna geçirerek, tek bir özellikte birden çok dizin ek açıklaması belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="d4ae8-153">CLR özelliğinin veritabanındaki bir sütunla Eşlememe belirtme</span><span class="sxs-lookup"><span data-stu-id="d4ae8-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="d4ae8-154">Aşağıdaki örnek, bir CLR türündeki bir özelliğin veritabanındaki bir sütunla eşlenmiyor olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="d4ae8-155">CLR özelliğini veritabanındaki belirli bir sütunla eşleme</span><span class="sxs-lookup"><span data-stu-id="d4ae8-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="d4ae8-156">Aşağıdaki örnek, ad CLR özelliğini DepartmentName veritabanı sütunuyla eşler.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="d4ae8-157">Modelde tanımlanmayan bir yabancı anahtarı yeniden adlandırma</span><span class="sxs-lookup"><span data-stu-id="d4ae8-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="d4ae8-158">Bir CLR türünde yabancı anahtar tanımlamadıysanız, ancak veritabanında olması gereken adı belirtmek istiyorsanız aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="d4ae8-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="d4ae8-159">Dize özelliğinin Unicode Içeriğini destekleyip desteklemediğini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d4ae8-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="d4ae8-160">Varsayılan dizeler Unicode 'Dur (SQL Server içinde nvarchar).</span><span class="sxs-lookup"><span data-stu-id="d4ae8-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="d4ae8-161">Bir dizenin varchar türünde olması gerektiğini belirtmek için ısunıcode yöntemini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="d4ae8-162">Veritabanı sütununun veri türünü yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d4ae8-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="d4ae8-163">[Hasccolumntype](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) yöntemi, aynı temel türün farklı temsillerine eşlemeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="d4ae8-164">Bu yöntemin kullanılması, çalışma zamanında verilerin herhangi bir dönüşümünü gerçekleştirmenize izin vermez.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="d4ae8-165">Iunıcode 'un, veritabanı belirsiz olduğu için sütun varchar 'a ayarlamanın tercih edilen yolu olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="d4ae8-166">Karmaşık bir tür üzerinde özellikleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d4ae8-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="d4ae8-167">Karmaşık bir tür üzerinde skaler özellikleri yapılandırmanın iki yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="d4ae8-168">ComplexTypeConfiguration üzerindeki özelliği çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="d4ae8-169">Ayrıca, karmaşık bir türün bir özelliğine erişmek için nokta gösterimini de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="d4ae8-170">Iyimser eşzamanlılık belirteci olarak kullanılacak bir özelliği yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d4ae8-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="d4ae8-171">Bir varlıktaki bir özelliğin eşzamanlılık belirtecini temsil ettiğini belirtmek için ConcurrencyCheck özniteliğini ya da IsConcurrencyToken yöntemini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="d4ae8-172">Ayrıca, özelliğini veritabanında bir satır sürümü olacak şekilde yapılandırmak için ırowversıon yöntemini de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="d4ae8-173">Özelliği bir satır sürümü olacak şekilde ayarlamak, onu iyimser eşzamanlılık belirteci olacak şekilde otomatik olarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="d4ae8-174">Tür eşleme</span><span class="sxs-lookup"><span data-stu-id="d4ae8-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="d4ae8-175">Bir sınıfın karmaşık bir tür olduğunu belirtme</span><span class="sxs-lookup"><span data-stu-id="d4ae8-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="d4ae8-176">Kurala göre, belirtilen birincil anahtarı olmayan bir tür karmaşık bir tür olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="d4ae8-177">Code First karmaşık bir türü algılamamasının bazı senaryolar vardır (örneğin, KIMLIK olarak adlandırılan bir özellik varsa, ancak birincil anahtar olması anlamına gelir).</span><span class="sxs-lookup"><span data-stu-id="d4ae8-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="d4ae8-178">Böyle durumlarda, türün karmaşık bir tür olduğunu açıkça belirtmek için Fluent API kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="d4ae8-179">CLR varlık türünün veritabanındaki bir tabloya Eşlenmeme belirtme</span><span class="sxs-lookup"><span data-stu-id="d4ae8-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="d4ae8-180">Aşağıdaki örnek, bir CLR türünün veritabanındaki bir tabloya eşlenmeden nasıl dışlanacağını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="d4ae8-181">Bir varlık türünü veritabanındaki belirli bir tabloyla eşleme</span><span class="sxs-lookup"><span data-stu-id="d4ae8-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="d4ae8-182">Departmanın tüm özellikleri t_ departmanı adlı tablodaki sütunlara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="d4ae8-183">Şema adını şöyle da belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d4ae8-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="d4ae8-184">Hiyerarşi başına tablo (TPH) devralmayı eşleme</span><span class="sxs-lookup"><span data-stu-id="d4ae8-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="d4ae8-185">TPH eşleme senaryosunda, bir devralma hiyerarşisindeki tüm türler tek bir tabloyla eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="d4ae8-186">Bir Ayrıştırıcı sütunu, her satırın türünü tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="d4ae8-187">Code First ile modelinizi oluştururken, TPH devralma hiyerarşisine katılan türler için varsayılan stratejidir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="d4ae8-188">Varsayılan olarak, ayrıştırıcı sütunu tabloya "ayrıştırıcı" adı ile eklenir ve hiyerarşideki her türün CLR türü adı ayrıştırıcı değerleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="d4ae8-189">Varsayılan davranışı Fluent API kullanarak değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="d4ae8-190">Tablo tür başına (TPT) devralmayı eşleme</span><span class="sxs-lookup"><span data-stu-id="d4ae8-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="d4ae8-191">TPT eşleme senaryosunda, tüm türler ayrı tablolara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="d4ae8-192">Yalnızca bir temel türe veya türetilmiş türe ait özellikler, bu türle eşleşen bir tabloda depolanır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="d4ae8-193">Türetilmiş türlerle eşlenen tablolar, türetilmiş tabloya temel tabloyla birleştiren bir yabancı anahtar de depolar.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="d4ae8-194">Tablo başına somut sınıf (TPC) devralmayı eşleme</span><span class="sxs-lookup"><span data-stu-id="d4ae8-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="d4ae8-195">TPC eşleme senaryosunda, hiyerarşideki Özet olmayan tüm türler ayrı tablolara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="d4ae8-196">Türetilmiş sınıflarla eşlenen tablolarda, veritabanındaki temel sınıfla eşlenen tabloyla ilişki yoktur.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="d4ae8-197">Devralınan özellikler dahil olmak üzere bir sınıfın tüm özellikleri karşılık gelen tablonun sütunlarına eşlenir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="d4ae8-198">Türetilmiş her türü yapılandırmak için Mapınheritedproperties metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="d4ae8-199">Mapınheritedproperties, temel sınıftan devralınan tüm özellikleri, türetilmiş sınıf için tablodaki yeni sütunlara yeniden eşler.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="d4ae8-200">TPC devralma hiyerarşisine katılan tablolar birincil anahtar paylaşmadığından, veritabanı tarafından oluşturulan ve aynı kimlik kaynağını içeren bir değer varsa, alt sınıflara eşlenen tablolara eklenirken yinelenen varlık anahtarları olacaktır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="d4ae8-201">Bu sorunu çözmek için, her tablo için farklı bir başlangıç çekirdek değeri belirtebilir veya birincil anahtar özelliğindeki kimlik devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="d4ae8-202">Kimlik, Code First çalışırken tamsayı anahtar özellikleri için varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="d4ae8-203">Bir varlık türünün özelliklerini veritabanındaki birden çok tabloya eşleme (varlık bölme)</span><span class="sxs-lookup"><span data-stu-id="d4ae8-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="d4ae8-204">Varlık bölünmesi bir varlık türünün özelliklerinin birden çok tabloya yayılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="d4ae8-205">Aşağıdaki örnekte, departman varlığı iki tabloya ayrılır: Department ve DepartmentDetails.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="d4ae8-206">Varlık bölünmesi, bir özellik alt kümesini belirli bir tabloya eşlemek için Map yöntemine birden çok çağrı kullanır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="d4ae8-207">Birden çok varlık türünü veritabanındaki bir tabloyla eşleme (tablo bölme)</span><span class="sxs-lookup"><span data-stu-id="d4ae8-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="d4ae8-208">Aşağıdaki örnek, birincil anahtarı tek bir tabloyla paylaşan iki varlık türünü eşler.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="d4ae8-209">Bir varlık türünü, saklı yordamları ekleme/güncelleştirme/silme (EF6 ve sonraki sürümler) ile eşleme</span><span class="sxs-lookup"><span data-stu-id="d4ae8-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="d4ae8-210">EF6 ' den itibaren, güncelleştirme Ekle ve Sil için saklı yordamları kullanmak üzere bir varlığı eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="d4ae8-211">Daha fazla ayrıntı için bkz. [Code First, saklı yordamları ekleme/güncelleştirme/silme](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span><span class="sxs-lookup"><span data-stu-id="d4ae8-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="d4ae8-212">Örneklerde kullanılan model</span><span class="sxs-lookup"><span data-stu-id="d4ae8-212">Model Used in Samples</span></span>  

<span data-ttu-id="d4ae8-213">Bu sayfadaki örnekler için aşağıdaki Code First modeli kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d4ae8-213">The following Code First model is used for the samples on this page.</span></span>  

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
