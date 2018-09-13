---
title: Özel kod öncelikli kurallar - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489850"
---
# <a name="custom-code-first-conventions"></a><span data-ttu-id="4d9ee-102">Özel kod öncelikli kurallar</span><span class="sxs-lookup"><span data-stu-id="4d9ee-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="4d9ee-103">**EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="4d9ee-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="4d9ee-105">Code First kullanarak modelinizi sınıflardan kuralları kümesi kullanılarak hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="4d9ee-106">Varsayılan [kod öncelikli kurallar](~/ef6/modeling/code-first/conventions/built-in.md) şeyler gibi özellik olur bir varlığın birincil anahtarı, bir varlık eşler için tablo ve hangi kesinlik ve ölçek ondalık bir sütun varsayılan olarak sahip adını belirleyin.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="4d9ee-107">Bazen bu varsayılan kuralları modeliniz için ideal değildir ve bunların etrafına veri ek açıklamaları ya da Fluent API'sini kullanarak birçok tek tek varlıklarla yapılandırarak çalışmak zorunda.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="4d9ee-108">Özel kod öncelikli kurallar modeliniz için yapılandırma Varsayılanları sağlayan kendi kuralları tanımlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="4d9ee-109">Bu kılavuzda, biz özel kuralları ve bunların her biri oluşturma farklı türlerini keşfedin.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="4d9ee-110">Model tabanlı kuralları</span><span class="sxs-lookup"><span data-stu-id="4d9ee-110">Model-Based Conventions</span></span>

<span data-ttu-id="4d9ee-111">Bu sayfa DbModelBuilder API'si için özel kurallar kapsar.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="4d9ee-112">Bu API, çoğu özel kuralları yazmak için yeterli olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="4d9ee-113">Ancak, de mevcuttur özelliği, model tabanlı kuralları - oluşturulduktan sonra son modelin yönlendirme kurallarını yazmak için - Gelişmiş senaryolar işlemek için.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="4d9ee-114">Daha fazla bilgi için [Model tabanlı kuralları](~/ef6/modeling/code-first/conventions/model.md).</span><span class="sxs-lookup"><span data-stu-id="4d9ee-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="4d9ee-115">Modelimiz</span><span class="sxs-lookup"><span data-stu-id="4d9ee-115">Our Model</span></span>

<span data-ttu-id="4d9ee-116">Size sunduğumuz kuralları ile kullanabileceğiniz basit bir model tanımlayarak başlayalım.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="4d9ee-117">Aşağıdaki sınıflar, projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-117">Add the following classes to your project.</span></span>

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

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="4d9ee-118">Özel kuralları ile tanışın</span><span class="sxs-lookup"><span data-stu-id="4d9ee-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="4d9ee-119">Varlık türü için birincil anahtar olmasını anahtar adlı herhangi bir özelliği yapılandıran bir kuralı yazalım.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="4d9ee-120">Kuralları bağlamında OnModelCreating geçersiz kılarak erişilebilir model oluşturucu üzerinde etkindir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="4d9ee-121">ProductContext sınıfı aşağıdaki gibi güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-121">Update the ProductContext class as follows:</span></span>

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

<span data-ttu-id="4d9ee-122">Artık, herhangi bir özelliği modelimizi anahtar adlı olacak kendi parçası ne olursa olsun varlığın birincil anahtarı yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="4d9ee-123">Biz de bizim kuralları daha belirli yapılandırma kullanacağız özelliği türüne göre filtreleme yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="4d9ee-124">Bu, varlığın anahtarı, ancak bir tamsayı ise yalnızca birincil anahtarının adlı tüm özelliklerini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="4d9ee-125">Iskey yöntemi ilgi çekici bir özelliğidir, olmasıdır eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="4d9ee-126">Bu, birden çok özellikleri Iskey çağırın ve tüm bileşik anahtarın bir parçası olacak anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="4d9ee-127">Bunun için bir uyarı, bir anahtar için birden çok özellik belirttiğinizde bu özellikler için bir sipariş da belirtmelisiniz ' dir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="4d9ee-128">Bu yöntem gibi aşağıda HasColumnOrder çağırarak yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="4d9ee-129">Bu kod türlerine int anahtar sütunu ve dize adı sütunu oluşan bileşik anahtara sahip modelimizi yapılandıracaksınız.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="4d9ee-130">Biz modeli Tasarımcısı'nda görüntülediğinizde şöyle görünebilir:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-130">If we view the model in the designer it would look like this:</span></span>

![bileşik anahtar](~/ef6/media/compositekey.png)

<span data-ttu-id="4d9ee-132">Başka bir özellik kuralları my modeldeki datetime yerine SQL Server datetime2 türüne eşlemek için tüm DateTime özelliklerini yapılandırmak için örneğidir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="4d9ee-133">Bunu aşağıdaki elde:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="4d9ee-134">Kuralı sınıfları</span><span class="sxs-lookup"><span data-stu-id="4d9ee-134">Convention Classes</span></span>

<span data-ttu-id="4d9ee-135">Kuralları tanımlamanın bir başka yolu kuralınızın yalıtılacak kuralı sınıfı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="4d9ee-136">Bir kuralı sınıf kullanırken System.Data.Entity.ModelConfiguration.Conventions ad alanındaki kuralı sınıfından devralan bir tür oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="4d9ee-137">Aşağıdakileri yaparak size daha önce gösterilen datetime2 kuralıyla bir kuralı sınıf oluşturabiliriz:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

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

<span data-ttu-id="4d9ee-138">Bu kural, adım adım kılavuzla birlikte takip ediyorsanız, şuna benzeyecektir OnModelCreating kuralları koleksiyona eklediğiniz kullanılacak EF bildirmek için:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="4d9ee-139">Gördüğünüz gibi biz bizim kuralı örneği kuralları koleksiyona ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="4d9ee-140">Kuralı devralan gruplandırma ve takımlar veya projeleri arasında kuralları paylaşımı kullanışlı bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="4d9ee-141">Örneğin, bir sınıf kitaplığı ile kuruluşunuzun tüm projeleri, kullanım kuralları ortak bir dizi olabilir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="4d9ee-142">Özel Öznitelikler</span><span class="sxs-lookup"><span data-stu-id="4d9ee-142">Custom Attributes</span></span>

<span data-ttu-id="4d9ee-143">Kuralları başka bir harika kullanımı modeli yapılandırırken kullanılacak yeni bir öznitelik etkinleştirmektir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="4d9ee-144">Bunu açıklamak üzere; dize özellikleri Unicode olmayan işaretlemek için kullanabileceğiniz bir öznitelik oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="4d9ee-145">Şimdi, bu öznitelik için modelimizi uygulamak için bir kural oluşturalım:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="4d9ee-146">Bu kural ile herhangi bir veritabanı sütunu anlamına gelir bizim dize özellikleri Unicode olmayan tüm karşılaştırmalar öznitelik nvarchar yerine varchar olarak depolanan ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="4d9ee-147">Bir özel durum oluşturur sonra Unicode olmayan tüm karşılaştırmalar özniteliği bir dize özelliği dışında herhangi bir şey üzerinde koyarsanız bu kural hakkında dikkat edilecek bir şey olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="4d9ee-148">Bir dize dışında herhangi bir türü IsUnicode yapılandıramıyorsunuz için bunu yapar.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="4d9ee-149">Bu durumda, böylece bir dize olmayan şeyi filtreleri sonra kuralınızın daha belirli yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="4d9ee-150">Özel öznitelikler tanımlamak için yukarıdaki kuralı çalışırken kullanmak özellikle öznitelik sınıfından özellikleri kullanmak istediğinizde çok daha kolay olabilir başka bir API yoktur.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="4d9ee-151">Bu örnekte bizim özniteliği güncelleştirme ve şuna benzer şekilde IsUnicode özniteliğin değiştirme kullanacağız:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

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

<span data-ttu-id="4d9ee-152">Biz bunu aldıktan sonra bool bizim özniteliği olup olmadığını bir özelliği Unicode olmalıdır kuralı bildirmek için ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="4d9ee-153">Biz zaten böyle yapılandırma sınıfın ClrProperty erişerek sahibiz kuralı içinde yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="4d9ee-154">Bu oldukça kolaydır, ancak sahip kullanarak bunu elde etmenin daha birleştiren bir yolu yoktur API kurallarının yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="4d9ee-155">Sahip yönteminin bir parametresi vardır, Func yazın&lt;PropertyInfo, T&gt; kabul eden PropertyInfo Where aynı yöntemi, bir nesneyi döndürmek için bekleniyordu ancak.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="4d9ee-156">Özellik yapılandırılmadı sonra döndürülen nesne null ise, yani özellikleriyle, nerede olduğu gibi kullanıma filtre uygulayabilirsiniz, ancak ayrıca döndürülen nesne yakalamak ve yapılandırma yönteme geçirin, farklıdır.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="4d9ee-157">Bu, aşağıdaki gibi çalışır:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="4d9ee-158">Özel özniteliklere sahip kullanmak için tek nedeni olmayan yöntemi, bu yararlıdır, türler veya özellikler yapılandırırken filtre uyguladığınız bir şey hakkında neden gereken herhangi bir yerde.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="4d9ee-159">Yapılandırma türü</span><span class="sxs-lookup"><span data-stu-id="4d9ee-159">Configuring Types</span></span>

<span data-ttu-id="4d9ee-160">Kadarki tüm müşterilerimizin kuralları özelliklerini silinmiş, ancak türleri modelinizde yapılandırmak için başka bir alan kurallarının API olduğu.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="4d9ee-161">Deneyimi şu ana kadar gördük kurallarına benzer, ancak varlık özelliği yerine şu iç yapılandırma seçenekleri düzeyinde olacaktır.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="4d9ee-162">Tür düzeyi kuralları için gerçekten kullanışlı olabilecek şeylerden biri tablo adlandırma kuralı, EF varsayılandan farklı bir var olan bir şema eşlemek için veya farklı bir adlandırma kuralı ile yeni bir veritabanı oluşturmak için değişiyor.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="4d9ee-163">Bunu yapmak için öncelikle TypeInfo modelimizi bir türü için kabul edebilir ve dönüş türü için tablo adı ne olmalıdır bir yöntem ihtiyacımız:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="4d9ee-164">Bu yöntem, bir türü alır ve alt çizgi CamelCase yerine küçük kullanan bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="4d9ee-165">Modelimiz bu ProductCategory sınıfı ürün adlı bir tabloya eşlenmesi anlamına gelir\_öğelerini tire yerine kategorisi.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="4d9ee-166">Bu yöntem sahibiz sonra size, böyle bir kural içinde çağırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="4d9ee-167">Bu kural her tür modelimiz bizim GetTableName yönteminden döndürülen tablo adını eşleştirmek için yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="4d9ee-168">Bu kural Fluent API'sini kullanarak modeldeki her bir varlık için ToTable metodunu eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="4d9ee-169">Bu hakkında dikkat edilecek bir çağırdığınızda ToTable EF herhangi bir tablo adları belirlerken normalde yaptığınız çoğullaştırma olmadan tam tablo adı olarak sağlayan dize süreceğini şeydir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="4d9ee-170">Tablo adı bizim kuralı ürünüdür. Bu yüzden\_kategori yerine ürün\_kategorileri.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="4d9ee-171">Bizim kuralına kendimize çoğullaştırma hizmetine bir çağrı yaparak çözebiliriz.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="4d9ee-172">Aşağıdaki kodda kullanacağız [bağımlılık çözümlemesi](~/ef6/fundamentals/configuring/dependency-resolution.md) EF kullandığınız çoğullaştırma hizmet alınacak EF6 içinde eklenen özellik ve pluralize bizim tablo adı.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

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
> <span data-ttu-id="4d9ee-173">Genel sürümünü GetService System.Data.Entity.Infrastructure.DependencyResolution ad alanındaki bir genişletme yöntemi, kullanarak bir eklemeniz gerekecektir Bağlamınızı kullanmak için deyimi.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="4d9ee-174">ToTable ve devralma</span><span class="sxs-lookup"><span data-stu-id="4d9ee-174">ToTable and Inheritance</span></span>

<span data-ttu-id="4d9ee-175">Bir tür açıkça belirli bir tabloda eşlerseniz EF kullanan eşleme stratejisi değiştirebilirsiniz sonra başka bir önemli yönüyle ToTable olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="4d9ee-176">Devralma hiyerarşisinde her tür için ToTable çağırırsanız, yukarıda yaptığımız gibi tür adı tablonun adı geçirerek daha sonra varsayılan tablo başına hiyerarşi (TPH) eşleme stratejisi tablo başına tür (TPT) değişir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="4d9ee-177">Bunu açıklamak için en iyi yolu whith somut bir örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-177">The best way to describe this is whith a concrete example:</span></span>

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

<span data-ttu-id="4d9ee-178">Varsayılan olarak, veritabanındaki (çalışan) aynı tabloya hem çalışan hem de manager eşlenir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="4d9ee-179">Tablo çalışan ve yöneticilerin ne tür bir örnek, her satır için depolanan söyleyecektir bir ayrıştırıcı sütunu içerir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="4d9ee-180">Hiyerarşi için tek bir tablo olmadığından TPH eşleme budur.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="4d9ee-181">Her iki classe üzerinde ToTable çağırırsanız ancak ardından her tür bunun yerine kendi tablo olarak da bilinen her türü kendi tablosunda bulunduğundan TPT eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="4d9ee-182">Yukarıdaki kodu aşağıdakine benzer bir tablo yapısı için eşler:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-182">The code above will map to a table structure that looks like the following:</span></span>

![Tpt örneği](~/ef6/media/tptexample.jpg)

<span data-ttu-id="4d9ee-184">Bu durumu önlemek ve birkaç yolla varsayılan TPH eşleme Koru:</span><span class="sxs-lookup"><span data-stu-id="4d9ee-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="4d9ee-185">Hiyerarşideki her bir türü için aynı tablo adıyla ToTable çağırın.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="4d9ee-186">ToTable yalnızca üzerinde çalışan olacaktır Bizim örneğimizde hiyerarşinin temel sınıfı çağırın.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="4d9ee-187">Yürütme sırası</span><span class="sxs-lookup"><span data-stu-id="4d9ee-187">Execution Order</span></span>

<span data-ttu-id="4d9ee-188">Bir son WINS şekilde Fluent API'si ile aynı kuralları çalışır.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="4d9ee-189">Ne bu yapılandırma aynı seçeneği aynı özelliğin iki kuralları ve WINS yürütülecek sonuncu yazarsanız, anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="4d9ee-190">Örneğin, aşağıdaki kod tüm dizeleri en fazla uzunluğu 500'e ayarlanır ancak ardından 250 uzunluk üst sınırını olmasını modelinde adı olan tüm özellikleri yapılandırıyoruz.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="4d9ee-191">Uzunluk üst sınırı 250'ye ayarlamak için kuralı tüm dizeleri 500'e ayarlayan bir sonra olduğundan, bizim modelinde adı tüm özellikler 250 açıklamaları gibi herhangi diğer dizeler çalışırken, bir MaxLength gerekir, 500 olacaktır.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="4d9ee-192">Bu şekilde kurallarını kullanarak anlamına gelir, genel bir kural türleri veya modeli ve ardından geçersiz kılma özellikleri sağlayabilirsiniz bunları farklı alt kümeleri için.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="4d9ee-193">Fluent API'si ve veri ek açıklamaları belirli durumlarda bir yöntemi geçersiz kılmak için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="4d9ee-194">Biz Fluent API'si uzunluğu bir özelliği ayarlamak için kullanmışsınız daha belirli Fluent API'si daha genel yapılandırma kuralı kazanacak çünkü yukarıdaki ardından biz bunu önce veya sonra kural, yerleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="4d9ee-195">Yerleşik kuralları</span><span class="sxs-lookup"><span data-stu-id="4d9ee-195">Built-in Conventions</span></span>

<span data-ttu-id="4d9ee-196">Özel kurallar tarafından varsayılan kod öncelikli kurallar etkilenebilir, çünkü önce veya sonra başka bir kuralı çalıştırmak için kuralları eklemek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="4d9ee-197">Bunu yapmak için türetilmiş bir DbContext üzerinde kuralları koleksiyonu AddBefore ve AddAfter yöntemlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="4d9ee-198">Aşağıdaki kod, oluşturduğumuz daha önce oluşturulmuş önce çalıştıracağı kuralı sınıfı eklersiniz, anahtar bulma kuralı.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="4d9ee-199">Bu geçmeden önce veya sonra yerleşik kurallarını çalıştırmak için gereksinim kuralları eklerken en çok kullanımı olması için yerleşik kuralları listesini burada bulunabilir: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .</span><span class="sxs-lookup"><span data-stu-id="4d9ee-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="4d9ee-200">Ayrıca, modelinize uygulanan istemediğiniz kuralları kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="4d9ee-201">Bir kuralı kaldırmak için Remove yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="4d9ee-202">PluralizingTableNameConvention kaldırmanın bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4d9ee-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
