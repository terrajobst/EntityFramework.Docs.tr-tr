---
title: Özel Code First kuralları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419229"
---
# <a name="custom-code-first-conventions"></a><span data-ttu-id="a0888-102">Özel Code First kuralları</span><span class="sxs-lookup"><span data-stu-id="a0888-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="a0888-103">**Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="a0888-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="a0888-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="a0888-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="a0888-105">Code First kullanılırken, modelinizle bir dizi kural kullanarak sınıfınızdan hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="a0888-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="a0888-106">Varsayılan [Code First kuralları](~/ef6/modeling/code-first/conventions/built-in.md) , hangi özelliğin bir varlığın birincil anahtarı, bir varlığın eşlendiği tablo adı ve bir ondalık sütunu için varsayılan olarak hangi duyarlık ve ölçekleme olacağını belirleyen şeyleri belirlemektir.</span><span class="sxs-lookup"><span data-stu-id="a0888-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="a0888-107">Bazen bu varsayılan kurallar modelinize uygun değildir ve veri açıklamalarını veya akıcı API 'YI kullanarak çok sayıda varlığı yapılandırarak onları geçici olarak çözebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0888-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="a0888-108">Özel Code First kuralları modelinize yönelik yapılandırma Varsayılanları sağlayan kendi kurallarınızı tanımlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="a0888-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="a0888-109">Bu kılavuzda, farklı özel kural türlerini ve bunların her birini oluşturmayı inceleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="a0888-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="a0888-110">Model tabanlı kurallar</span><span class="sxs-lookup"><span data-stu-id="a0888-110">Model-Based Conventions</span></span>

<span data-ttu-id="a0888-111">Bu sayfa, özel kurallar için DbModelBuilder API 'sini içerir.</span><span class="sxs-lookup"><span data-stu-id="a0888-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="a0888-112">Bu API, çoğu özel kuralı yazmak için yeterli olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a0888-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="a0888-113">Bununla birlikte, gelişmiş senaryoları işlemek için, oluşturulduktan sonra son modeli düzenleyen model tabanlı kuralları yazma özelliği de vardır.</span><span class="sxs-lookup"><span data-stu-id="a0888-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="a0888-114">Daha fazla bilgi için bkz. [model tabanlı kurallar](~/ef6/modeling/code-first/conventions/model.md).</span><span class="sxs-lookup"><span data-stu-id="a0888-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="a0888-115">Modelimiz</span><span class="sxs-lookup"><span data-stu-id="a0888-115">Our Model</span></span>

<span data-ttu-id="a0888-116">Kılavuzlarımızla birlikte kullanabilmemiz için basit bir model tanımlayarak başlayalım.</span><span class="sxs-lookup"><span data-stu-id="a0888-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="a0888-117">Aşağıdaki sınıfları projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a0888-117">Add the following classes to your project.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="a0888-118">Özel kurallara giriş</span><span class="sxs-lookup"><span data-stu-id="a0888-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="a0888-119">Anahtar adlı herhangi bir özelliği, varlık türü için birincil anahtar olacak şekilde yapılandıran bir kural yazalım.</span><span class="sxs-lookup"><span data-stu-id="a0888-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="a0888-120">Kurallar, bağlamda Onmodeloluþturma geçersiz kılınarak erişilebilen model Oluşturucu üzerinde etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a0888-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="a0888-121">ProductContext sınıfını aşağıdaki gibi güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="a0888-121">Update the ProductContext class as follows:</span></span>

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

<span data-ttu-id="a0888-122">Şimdi, modelinizdeki anahtar adlı bir özellik, bölümünün parçası olan her bir varlığın birincil anahtarı olarak yapılandırılacaktır.</span><span class="sxs-lookup"><span data-stu-id="a0888-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="a0888-123">Ayrıca, yapılandırdığımız Özellik türü üzerine filtreleyerek kurallarımızı daha belirgin hale yapabiliriz:</span><span class="sxs-lookup"><span data-stu-id="a0888-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="a0888-124">Bu, anahtar adlı tüm özellikleri varlığının birincil anahtarı olacak şekilde yapılandırır, ancak yalnızca bir tamsayıdır.</span><span class="sxs-lookup"><span data-stu-id="a0888-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="a0888-125">IsKey yönteminin ilginç bir özelliği eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="a0888-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="a0888-126">Yani, IsKey öğesini birden çok özelliklerde çağırdığınızda ve hepsi bir bileşik anahtarın parçası haline gelirse.</span><span class="sxs-lookup"><span data-stu-id="a0888-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="a0888-127">Bunun için bir desteklenmediği uyarısıyla, bir anahtar için birden çok özellik belirttiğinizde, bu özellikler için de bir sıra belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a0888-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="a0888-128">Bunu aşağıdaki gibi Hasccolumnorder metodunu çağırarak yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a0888-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="a0888-129">Bu kod, modelimizin türlerini, int anahtar sütununu ve dize adı sütununu içeren bileşik bir anahtara sahip olacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a0888-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="a0888-130">Modeli tasarımcıda görüntüleyebilmemiz şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="a0888-130">If we view the model in the designer it would look like this:</span></span>

![bileşik anahtar](~/ef6/media/compositekey.png)

<span data-ttu-id="a0888-132">Özellik kurallarına başka bir örnek, modelinmdeki tüm DateTime özelliklerini, DateTime yerine SQL Server datetime2 türüyle eşlenecek şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="a0888-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="a0888-133">Bunu aşağıdakiler ile elde edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a0888-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="a0888-134">Kural sınıfları</span><span class="sxs-lookup"><span data-stu-id="a0888-134">Convention Classes</span></span>

<span data-ttu-id="a0888-135">Kuralları tanımlamanın bir diğer yolu, kurallarınızı kapsüllemek için bir kural sınıfı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="a0888-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="a0888-136">Bir kural sınıfı kullanırken, System. Data. Entity. ModelConfiguration. Convention ad alanındaki kural sınıfından devralan bir tür oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="a0888-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="a0888-137">Daha önce aşağıdakileri yaparak, datetime2 kuralına sahip bir kural sınıfı oluşturuyoruz:</span><span class="sxs-lookup"><span data-stu-id="a0888-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

<span data-ttu-id="a0888-138">Bu kuralı kullanmak için EF 'in bu kuralı kullanmasını söylemek için, bu yönergeyi Onmodeloluþturma 'daki kurallara göre eklersiniz. Bu, izlenecek yol aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="a0888-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="a0888-139">Gördüğünüz gibi, kural koleksiyonu için kuralımız bir örnek ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="a0888-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="a0888-140">Kurallardan devralma, takımlar veya projeler arasında kuralları gruplandırmanın ve paylaşmanın kolay bir yolunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="a0888-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="a0888-141">Örneğin, tüm kuruluşların projelerinin kullandığı ortak bir kural kümesine sahip bir sınıf kitaplığına sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0888-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="a0888-142">Özel Öznitelikler</span><span class="sxs-lookup"><span data-stu-id="a0888-142">Custom Attributes</span></span>

<span data-ttu-id="a0888-143">Kuralların diğer harika kullanımı, bir modeli yapılandırırken yeni özniteliklerin kullanılmasını olanaklı hale kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="a0888-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="a0888-144">Bunu göstermek için, dize özelliklerini Unicode olmayan olarak işaretlemek üzere kullandığımız bir öznitelik oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="a0888-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="a0888-145">Şimdi bu özniteliği modelinize uygulamak için bir kural oluşturalım:</span><span class="sxs-lookup"><span data-stu-id="a0888-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="a0888-146">Bu kural ile Unicode olmayan özniteliğini dize özelliklerinden herhangi birine ekleyebiliriz. Bu, veritabanındaki sütunun nvarchar yerine varchar olarak depolanacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="a0888-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="a0888-147">Bu kural hakkında daha fazla bilgi için, Unicode olmayan özniteliğini dize özelliği dışındaki herhangi bir yere yerleştirirseniz bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a0888-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="a0888-148">Bu, bir dize dışında herhangi bir türde ısunıcode 'u yapılandıramadığı için bunu yapar.</span><span class="sxs-lookup"><span data-stu-id="a0888-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="a0888-149">Bu durumda, bir dize olmayan herhangi bir şeyi filtreleyerek, daha sonra, kuralınızın daha özel olmasını sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0888-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="a0888-150">Yukarıdaki kural özel öznitelikler tanımlamak için çalışırken, özellikle öznitelik sınıfından özellikleri kullanmak istediğinizde, daha kolay olabilecek başka bir API vardır.</span><span class="sxs-lookup"><span data-stu-id="a0888-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="a0888-151">Bu örnekte, öznitemizi güncelleyeceğiz ve bunu bir ıunıcode özniteliğiyle değiştiririz, bu nedenle şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="a0888-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

<span data-ttu-id="a0888-152">Bunu yaptıktan sonra, bir özelliğin Unicode olması gerekip gerekmediğini bildirmek için öznitemize bir bool ayarlayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="a0888-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="a0888-153">Bunu şu şekilde yapılandırma sınıfının ClrProperty öğesine erişmekte olduğumuz kurala göre yapabiliriz:</span><span class="sxs-lookup"><span data-stu-id="a0888-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="a0888-154">Bu çok kolaydır, ancak kuralları API 'nin HAVING metodunu kullanarak elde etmenin daha kısa bir yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="a0888-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="a0888-155">HAVING yöntemi Func&lt;PropertyInfo, T&gt; türünde bir parametreye sahiptir ve bu, WHERE yöntemiyle aynı şekilde PropertyInfo kabul eder, ancak bir nesne döndürmesi beklenir.</span><span class="sxs-lookup"><span data-stu-id="a0888-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="a0888-156">Döndürülen nesne null ise, özelliği yapılandırılmaz, bu, özellikleri burada olduğu gibi filtreleyebilirsiniz, ancak döndürülen nesneyi de yakalayıp yapılandırma yöntemine iletmektir çünkü farklı olur.</span><span class="sxs-lookup"><span data-stu-id="a0888-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="a0888-157">Bu, aşağıdaki gibi çalışmaktadır:</span><span class="sxs-lookup"><span data-stu-id="a0888-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="a0888-158">Özel öznitelikler HAVING metodunu kullanmanın tek nedeni olmadığından, türlerinizi veya özellikleri yapılandırırken filtrelemediğiniz bir şey hakkında neden olmanız gerektiği her yerde yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="a0888-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="a0888-159">Türleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a0888-159">Configuring Types</span></span>

<span data-ttu-id="a0888-160">Şimdiye kadar tüm kurallarımızda özellikler için olduğundan, modelinizdeki türleri yapılandırmak için kurallar API 'sinin başka bir alanı vardır.</span><span class="sxs-lookup"><span data-stu-id="a0888-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="a0888-161">Deneyim şimdiye kadar görtiğimiz kurallara benzer, ancak yapılandırma içindeki Seçenekler özellik düzeyi yerine varlıkta olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a0888-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="a0888-162">Tür düzeyi kuralları için gerçekten yararlı olabilecek olan işlemlerden biri, bir veya daha fazla farklı adlandırma kuralına sahip olan mevcut bir şemayla eşlemek için tablo adlandırma kuralını değiştiriyor.</span><span class="sxs-lookup"><span data-stu-id="a0888-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="a0888-163">Bunu yapmak için ilk olarak modelinizdeki bir türün TypeInfo kabul edebilecek ve bu tür için tablo adının ne olması gerektiğini döndüren bir yönteme ihtiyaç duyarsınız:</span><span class="sxs-lookup"><span data-stu-id="a0888-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="a0888-164">Bu yöntem bir tür alır ve CamelCase yerine alt çizgi ile küçük harf kullanan bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="a0888-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="a0888-165">Modelimizde bu, ProductCategory sınıfının ProductCategories yerine ürün\_kategorisi adlı bir tabloyla eşlenecek anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="a0888-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="a0888-166">Bu yönteme ulaştıktan sonra, bunu şöyle çağırabiliriz:</span><span class="sxs-lookup"><span data-stu-id="a0888-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="a0888-167">Bu kural, modelimizin her türünü, GetTableName yönteminden döndürülen tablo adıyla eşlenecek şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a0888-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="a0888-168">Bu kural, akıcı API kullanılarak modeldeki her varlık için ToTable yöntemini çağırmaya eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="a0888-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="a0888-169">Bunun için bir şey, ToTable EF 'i çağırdığınızda tablo adlarını belirlerken normalde yapacağından emin olmak için, tam tablo adı olarak sağladığınız dizeyi doğru bir şekilde alır.</span><span class="sxs-lookup"><span data-stu-id="a0888-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="a0888-170">Kuralımız tablo adının ürün\_kategorileri yerine ürün\_kategorisi olması neden olur.</span><span class="sxs-lookup"><span data-stu-id="a0888-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="a0888-171">Çoğullaştırma hizmeti kendimize ' e bir çağrı yaparak kurallarımızda bunu çözebiliriz.</span><span class="sxs-lookup"><span data-stu-id="a0888-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="a0888-172">Aşağıdaki kodda, EF 'in kullandığı ve tablo adınızla plmış olan çoğullaştırma hizmetini almak için EF6 ' de eklenen [bağımlılık çözümleme](~/ef6/fundamentals/configuring/dependency-resolution.md) özelliğini kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="a0888-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> <span data-ttu-id="a0888-173">GetService 'in genel sürümü System. Data. Entity. Infrastructure. DependencyResolution ad alanındaki bir genişletme yöntemidir, bunu kullanabilmeniz için bağlamına bir using ifadesini eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a0888-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="a0888-174">ToTable ve devralma</span><span class="sxs-lookup"><span data-stu-id="a0888-174">ToTable and Inheritance</span></span>

<span data-ttu-id="a0888-175">ToTable 'ın diğer önemli bir yönü, bir türü belirli bir tabloyla açıkça eşleştirdiğinizde, EF 'in kullanacağı eşleme stratejisini değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0888-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="a0888-176">Bir devralma hiyerarşisindeki her tür için ToTable çağrısı yaparsanız, tür adını yukarıda yaptığımız gibi tablonun adı olarak geçirerek, varsayılan hiyerarşi başına (TPH) eşleme stratejisini tür başına tablo (TPT) olarak değiştirirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0888-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="a0888-177">Bunu tanımlamanın en iyi yolu somut bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="a0888-177">The best way to describe this is whith a concrete example:</span></span>

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

<span data-ttu-id="a0888-178">Varsayılan olarak, hem çalışan hem de yönetici veritabanında aynı tablo (çalışanlar) ile eşlenir.</span><span class="sxs-lookup"><span data-stu-id="a0888-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="a0888-179">Tabloda, her bir satırda ne tür bir örnek depolandığını söyleyen bir Ayrıştırıcı sütunu olan çalışanlar ve yöneticiler bulunur.</span><span class="sxs-lookup"><span data-stu-id="a0888-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="a0888-180">Bu, hiyerarşi için tek bir tablo olduğundan, bu TPH eşlemedir.</span><span class="sxs-lookup"><span data-stu-id="a0888-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="a0888-181">Ancak, her iki ClassID üzerinde de ToTable ' ı çağırırsanız her bir türün kendi tablosu olduğu için, her tür kendi tablosuyla eşlenir (TPT olarak da bilinir).</span><span class="sxs-lookup"><span data-stu-id="a0888-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="a0888-182">Yukarıdaki kod, aşağıdakine benzer bir tablo yapısına eşlenir:</span><span class="sxs-lookup"><span data-stu-id="a0888-182">The code above will map to a table structure that looks like the following:</span></span>

![TPT örneği](~/ef6/media/tptexample.jpg)

<span data-ttu-id="a0888-184">Bunu ortadan kaldırabilirsiniz ve varsayılan TPH eşlemesini birkaç yolla koruyabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a0888-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="a0888-185">Hiyerarşideki her tür için aynı tablo adına sahip ToTable öğesini çağırın.</span><span class="sxs-lookup"><span data-stu-id="a0888-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="a0888-186">Yalnızca hiyerarşinin temel sınıfında, örneğin Employee olacak şekilde ToTable ' ı çağırın.</span><span class="sxs-lookup"><span data-stu-id="a0888-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="a0888-187">Yürütme sırası</span><span class="sxs-lookup"><span data-stu-id="a0888-187">Execution Order</span></span>

<span data-ttu-id="a0888-188">Kurallar, akıcı API ile aynı olan son bir WINS biçiminde çalışır.</span><span class="sxs-lookup"><span data-stu-id="a0888-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="a0888-189">Bunun anlamı, aynı özellikte aynı seçeneği yapılandıran iki kural yazarsanız ve ardından WINS 'i yürütmek için son bir şeydir.</span><span class="sxs-lookup"><span data-stu-id="a0888-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="a0888-190">Örnek olarak, aşağıdaki kodda tüm dizelerin uzunluk üst sınırı 500 olarak ayarlanır ancak modeldeki ad adlı tüm özellikler en fazla 250 uzunluğunda olacak şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="a0888-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="a0888-191">En fazla uzunluğu 250 olarak ayarlayan kural, tüm dizeleri 500 ' ye ayarlayan bir kural olduğundan, modelimizin adı adlı tüm özellikler, açıklamalar gibi diğer dizeler 500 olacak şekilde bir 250 MaxLength 'e sahip olur.</span><span class="sxs-lookup"><span data-stu-id="a0888-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="a0888-192">Bu şekilde kuralların kullanılması, modelinizdeki türler veya özellikler için genel bir kural sağlayabilmeniz ve bunları farklı alt kümeler için overide.</span><span class="sxs-lookup"><span data-stu-id="a0888-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="a0888-193">Akıcı API ve veri ek açıklamaları, belirli durumlarda bir kuralı geçersiz kılmak için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a0888-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="a0888-194">Yukarıdaki örneğimizde, bir özelliğin maksimum uzunluğunu ayarlamak için akıcı API kullandığımızda, daha fazla sayıda akıcı API daha genel yapılandırma kuralını kazanacağından, bunu kurala göre veya sonra koyabiliriz.</span><span class="sxs-lookup"><span data-stu-id="a0888-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="a0888-195">Yerleşik kurallar</span><span class="sxs-lookup"><span data-stu-id="a0888-195">Built-in Conventions</span></span>

<span data-ttu-id="a0888-196">Özel kurallar varsayılan Code First kurallarından etkilendiğinden, başka bir kural öncesinde veya sonrasında çalıştırılacak kurallar eklemek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="a0888-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="a0888-197">Bunu yapmak için, türetilmiş DbContext 'teki kural koleksiyonunun AddBefore ve Addadfter yöntemlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0888-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="a0888-198">Aşağıdaki kod, daha önce oluşturduğumuz kural sınıfını ekleyerek yerleşik anahtar bulma kuralına göre çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="a0888-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="a0888-199">Bu, yerleşik kurallardan önce veya sonra çalıştırılması gereken kuralları eklerken en çok kullanılan bir deyişle, yerleşik kuralların bir listesi burada bulunabilir: [System. Data. Entity. ModelConfiguration. kurallara ad alanı](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="a0888-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="a0888-200">Ayrıca, modelinize uygulanmasını istemediğiniz kuralları da kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0888-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="a0888-201">Bir kuralı kaldırmak için Remove metodunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="a0888-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="a0888-202">PluralizingTableNameConvention öğesinin kaldırılmasına bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a0888-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
