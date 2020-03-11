---
title: Code First Data ek açıklamaları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 9fac2a90c46d78ff5fd632800cc0050276467773
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419187"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="a53fc-102">Code First veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="a53fc-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="a53fc-103">**EF 4.1 yalnızca** sonraki sürümler-bu sayfada açıklanan özellikler, API 'ler vb. Entity Framework 4,1 ' de tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="a53fc-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="a53fc-104">Önceki bir sürümü kullanıyorsanız, bu bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-104">If you are using an earlier version, some or all of this information does not apply.</span></span>

<span data-ttu-id="a53fc-105">Bu sayfadaki içerik, başlangıçta Julie Lerman (\<http://thedatafarm.com>)tarafından yazılmış bir makaleden uyarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-105">The content on this page is adapted from an article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="a53fc-106">Entity Framework Code First, sorgu, değişiklik izleme ve işlevleri güncelleştirme işlemleri gerçekleştirmek için EF 'in kullandığı modeli göstermek üzere kendi etki alanı sınıflarınızı kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="a53fc-106">Entity Framework Code First allows you to use your own domain classes to represent the model that EF relies on to perform querying, change tracking, and updating functions.</span></span> <span data-ttu-id="a53fc-107">Code First ' yapılandırma üzerinden kural ' olarak adlandırılan bir programlama deseninin üzerinden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-107">Code First leverages a programming pattern referred to as 'convention over configuration.'</span></span> <span data-ttu-id="a53fc-108">Code First, sınıflarınızın Entity Framework kurallarını izlediğinden emin olur ve bu durumda, işini nasıl gerçekleştireceğiniz otomatik olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-108">Code First will assume that your classes follow the conventions of Entity Framework, and in that case, will automatically work out how to perform its job.</span></span> <span data-ttu-id="a53fc-109">Ancak, sınıflarınız bu kurallara uymadıysanız, gerekli bilgileri içeren EF sağlamak için sınıflarınıza yapılandırma ekleme olanağınız vardır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-109">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the requisite information.</span></span>

<span data-ttu-id="a53fc-110">Code First, bu yapılandırmaların sınıflarınızı eklenmesi için iki yol sunar.</span><span class="sxs-lookup"><span data-stu-id="a53fc-110">Code First gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="a53fc-111">Bunlardan biri, Datanot 'ler adlı basit öznitelikleri, ikincisi ise imperatively yapılandırma bilgilerini (kod içinde) açıklamanıza olanak sağlayan Code First akıcı API 'yi kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-111">One is using simple attributes called DataAnnotations, and the second is using Code First’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="a53fc-112">Bu makale, sınıflarınızı yapılandırmak için gerekli olan en sık kullanılan yapılandırmaların vurgulanması için veri açıklamalarını (System. ComponentModel. Dataaçıklamalarda ad alanında) kullanmaya odaklanacaktır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-112">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="a53fc-113">Dataaçıklamalarda Ayrıca, ASP.NET MVC gibi çeşitli .NET uygulamaları (Bu uygulamaların istemci tarafı doğrulamaları için aynı ek açıklamaların kullanmasına izin veren) tarafından da anlaşılmıştır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-113">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="a53fc-114">Model</span><span class="sxs-lookup"><span data-stu-id="a53fc-114">The model</span></span>

<span data-ttu-id="a53fc-115">Basit bir sınıf çiftiyle Code First veri açıklamalarını göstereceğim: blog ve gönderi.</span><span class="sxs-lookup"><span data-stu-id="a53fc-115">I’ll demonstrate Code First DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }

    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public DateTime DateCreated { get; set; }
        public string Content { get; set; }
        public int BlogId { get; set; }
        public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="a53fc-116">Bu şekilde, blog ve gönderi sınıfları kod ilk kuralını kullanır ve EF uyumluluk sağlamak için bir tdalgalı KS gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="a53fc-116">As they are, the Blog and Post classes conveniently follow code first convention and require no tweaks to enable EF compatability.</span></span> <span data-ttu-id="a53fc-117">Ancak, açıklamaları ve bunların eşlendikleri veritabanı hakkında daha fazla bilgi sağlamak için ek açıklamaları da kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-117">However, you can also use the annotations to provide more information to EF about the classes and the database to which they map.</span></span>

 

## <a name="key"></a><span data-ttu-id="a53fc-118">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a53fc-118">Key</span></span>

<span data-ttu-id="a53fc-119">Entity Framework, varlık izleme için kullanılan bir anahtar değeri olan her varlığa dayanır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-119">Entity Framework relies on every entity having a key value that is used for entity tracking.</span></span> <span data-ttu-id="a53fc-120">Code First bir kuralı örtük anahtar özelliklerdir; Code First, "ID" adlı bir özelliği veya "blogID" gibi sınıf adı ve "kimlik" birleşimini arayacaktır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-120">One convention of Code First is implicit key properties; Code First will look for a property named “Id”, or a combination of class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="a53fc-121">Bu özellik, veritabanındaki bir birincil anahtar sütunuyla eşleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-121">This property will map to a primary key column in the database.</span></span>

<span data-ttu-id="a53fc-122">Blog ve post sınıflarının her ikisi de bu kuralı izler.</span><span class="sxs-lookup"><span data-stu-id="a53fc-122">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="a53fc-123">Ne olursa?</span><span class="sxs-lookup"><span data-stu-id="a53fc-123">What if they didn’t?</span></span> <span data-ttu-id="a53fc-124">Blog bunun yerine *Primarytrackingkey* adını kullansa da, hatta *foo*?</span><span class="sxs-lookup"><span data-stu-id="a53fc-124">What if Blog used the name *PrimaryTrackingKey* instead, or even *foo*?</span></span> <span data-ttu-id="a53fc-125">Kod ilk olarak bu kurala uyan bir özellik bulamazsa, bir anahtar özelliği olması gereken Entity Framework gereksinimi nedeniyle bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a53fc-125">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="a53fc-126">EntityKey olarak kullanılacak özelliği belirlemek için anahtar ek açıklamasını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-126">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

<span data-ttu-id="a53fc-127">Kod birincisinin veritabanı oluşturma özelliğini kullanıyorsanız, blog tablosu, varsayılan olarak kimlik olarak da tanımlanan PrimaryTrackingKey adlı bir birincil anahtar sütununa sahip olur.</span><span class="sxs-lookup"><span data-stu-id="a53fc-127">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey, which is also defined as Identity by default.</span></span>

![Birincil anahtarla blog tablosu](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="a53fc-129">Bileşik anahtarlar</span><span class="sxs-lookup"><span data-stu-id="a53fc-129">Composite keys</span></span>

<span data-ttu-id="a53fc-130">Entity Framework, birden fazla özellikten oluşan bileşik anahtarlar-birincil anahtarlar destekler.</span><span class="sxs-lookup"><span data-stu-id="a53fc-130">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="a53fc-131">Örneğin, birincil anahtarı PassportNumber ve ıssuingcountry birleşimi olan bir Passport sınıfına sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-131">For example, you could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="a53fc-132">EF modelinizde yukarıdaki sınıfı kullanmaya çalışmak `InvalidOperationException`neden olur:</span><span class="sxs-lookup"><span data-stu-id="a53fc-132">Attempting to use the above class in your EF model would result in an `InvalidOperationException`:</span></span>

<span data-ttu-id="a53fc-133">*' Passport ' türü için bileşik birincil anahtar sıralaması belirlenemiyor. Bileşik birincil anahtarlar için bir sipariş belirtmek üzere ColumnAttribute veya HasKey metodunu kullanın.*</span><span class="sxs-lookup"><span data-stu-id="a53fc-133">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="a53fc-134">Bileşik anahtarlar kullanabilmek için Entity Framework, anahtar özellikler için bir sıra tanımlamanızı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-134">In order to use composite keys, Entity Framework requires you to define an order for the key properties.</span></span> <span data-ttu-id="a53fc-135">Bunu, bir sipariş belirtmek için sütun ek açıklamasını kullanarak yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-135">You can do this by using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="a53fc-136">Düzen değeri göreli olur (dizin tabanlı değil), böylece herhangi bir değer kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-136">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="a53fc-137">Örneğin, 100 ve 200, 1 ve 2 ' nin yerine kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-137">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="a53fc-138">Birleşik yabancı anahtarlar içeren varlıklarınız varsa, ilgili birincil anahtar özellikleri için kullandığınız sütun sıralamasını belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-138">If you have entities with composite foreign keys, then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="a53fc-139">Yalnızca yabancı anahtar özellikleri içindeki göreli sıralama aynı olmalıdır, **sipariş** için atanan değerlerin tam olarak eşleşmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="a53fc-139">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="a53fc-140">Örneğin, aşağıdaki sınıfta, 1 ve 2 ' nin yerine 3 ve 4 kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-140">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a><span data-ttu-id="a53fc-141">Gerekli</span><span class="sxs-lookup"><span data-stu-id="a53fc-141">Required</span></span>

<span data-ttu-id="a53fc-142">Gerekli ek açıklama EF öğesine belirli bir özelliğin gerekli olduğunu söyler.</span><span class="sxs-lookup"><span data-stu-id="a53fc-142">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="a53fc-143">Title özelliğine gerekli ekleme özelliği, özelliğin veri içerdiğinden emin olmak için EF (ve MVC) özelliğini zorunlu kılar.</span><span class="sxs-lookup"><span data-stu-id="a53fc-143">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="a53fc-144">Uygulamada ek kod veya biçimlendirme değişikliği olmadan, bir MVC uygulaması, özelliği ve ek açıklama adlarını kullanarak dinamik olarak bir ileti oluşturarak istemci tarafı doğrulaması gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-144">With no additional code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Başlığa sahip sayfa oluştur gerekli hata](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="a53fc-146">Gerekli öznitelik, eşlenen özelliği null atanamaz hale getirerek oluşturulan veritabanını da etkiler.</span><span class="sxs-lookup"><span data-stu-id="a53fc-146">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="a53fc-147">Title alanının "Not null" olarak değiştirildiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a53fc-147">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="a53fc-148">Bazı durumlarda, özelliği gerekli olsa bile veritabanındaki sütunun null olmaması mümkün olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-148">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="a53fc-149">Örneğin, birden çok tür için bir TPH devralma stratejisi verisi kullanıldığında tek bir tabloda depolanır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-149">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="a53fc-150">Türetilmiş bir tür gerekli bir özellik içeriyorsa, hiyerarşideki tüm türlerin bu özelliği içermesi olmadığından, sütun null yapılamayan yapılamaz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-150">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![Bloglar tablosu](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="a53fc-152">MaxLength ve MinLength</span><span class="sxs-lookup"><span data-stu-id="a53fc-152">MaxLength and MinLength</span></span>

<span data-ttu-id="a53fc-153">MaxLength ve MinLength öznitelikleri, gerekli olduğu gibi ek özellik doğrulamaları belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-153">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="a53fc-154">Length gereksinimlerine sahip BloggerName.</span><span class="sxs-lookup"><span data-stu-id="a53fc-154">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="a53fc-155">Örnek ayrıca özniteliklerin nasıl birleştirileceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-155">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="a53fc-156">MaxLength ek açıklaması, özelliğin uzunluğunu 10 olarak ayarlayarak veritabanını etkiler.</span><span class="sxs-lookup"><span data-stu-id="a53fc-156">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![BloggerName sütununda maksimum uzunluğu gösteren Bloglar tablosu](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="a53fc-158">MVC istemci tarafı ek açıklaması ve EF 4,1 sunucu tarafı ek açıklaması, her ikisi de bu doğrulamaya uyar ve bir hata iletisini dinamik olarak oluşturuyor: "alan BloggerName, en fazla uzunluğu ' 10 ' olan bir dize veya dizi türü olmalıdır." Bu ileti çok uzun.</span><span class="sxs-lookup"><span data-stu-id="a53fc-158">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="a53fc-159">Birçok ek açıklama, ErrorMessage özniteliğiyle bir hata iletisi belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a53fc-159">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="a53fc-160">Gerekli ek açıklamada ErrorMessage de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-160">You can also specify ErrorMessage in the Required annotation.</span></span>

![Özel hata iletisiyle sayfa oluştur](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="a53fc-162">Noteşlendi</span><span class="sxs-lookup"><span data-stu-id="a53fc-162">NotMapped</span></span>

<span data-ttu-id="a53fc-163">Code First yöntemi, desteklenen bir veri türü olan her özelliğin veritabanında temsil edileceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="a53fc-163">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="a53fc-164">Ancak bu, uygulamalarınızda her zaman durum değildir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-164">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="a53fc-165">Örneğin, blog sınıfında başlık ve BloggerName alanlarını temel alan bir kod oluşturan bir özelliğe sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-165">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="a53fc-166">Bu özellik dinamik olarak oluşturulabilir ve depolanması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="a53fc-166">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="a53fc-167">Veritabanıyla eşlenmez olan herhangi bir özelliği, bu BlogCode özelliği gibi Noteşlenmiş ek açıklamayla işaretleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-167">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a><span data-ttu-id="a53fc-168">Türündedir</span><span class="sxs-lookup"><span data-stu-id="a53fc-168">ComplexType</span></span>

<span data-ttu-id="a53fc-169">Etki alanı varlıklarınızı bir sınıf kümesi genelinde anlatmak ve ardından bu sınıfları bir bütün varlığı tanımlayacak şekilde katmanlara eklemek çok seyrek değildir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-169">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="a53fc-170">Örneğin, modelinize BlogDetails adlı bir sınıf ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-170">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="a53fc-171">BlogDetails öğesinin herhangi bir anahtar özelliği türüne sahip olmadığına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a53fc-171">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="a53fc-172">Etki alanı odaklı tasarımda, BlogDetails değer nesnesi olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-172">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="a53fc-173">Entity Framework, değer nesnelerini karmaşık türler olarak ifade eder.</span><span class="sxs-lookup"><span data-stu-id="a53fc-173">Entity Framework refers to value objects as complex types.</span></span><span data-ttu-id="a53fc-174">  Karmaşık türler kendi üzerinde izlenemez.</span><span class="sxs-lookup"><span data-stu-id="a53fc-174">  Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="a53fc-175">Ancak blog sınıfındaki bir özellik olarak BlogDetails, bir blog nesnesinin bir parçası olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-175">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="a53fc-176">Kodun bu adı tanıması için, BlogDetails sınıfını bir ComplexType olarak işaretlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-176">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="a53fc-177">Artık blog sınıfına, bu blog için BlogDetails temsil eden bir özellik ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-177">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="a53fc-178">Veritabanında blog tablosu, BlogDetail özelliğinde yer alan özellikler de dahil olmak üzere blogun tüm özelliklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-178">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="a53fc-179">Varsayılan olarak, her biri daha önce karmaşık türün adı olan BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="a53fc-179">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![Karmaşık türdeki blog tablosu](~/ef6/media/jj591583-figure06.png)


## <a name="concurrencycheck"></a><span data-ttu-id="a53fc-181">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="a53fc-181">ConcurrencyCheck</span></span>

<span data-ttu-id="a53fc-182">ConcurrencyCheck ek açıklaması, bir Kullanıcı bir varlığı düzenlediğinde veya sildiğinde veritabanında eşzamanlılık denetimi için kullanılacak bir veya daha fazla özelliği işaretetmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-182">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="a53fc-183">EF Designer ile çalışıyorsanız bu, özelliğin ConcurrencyMode 'ı fixed olarak ayarlamaya göre hizalanır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-183">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="a53fc-184">BloggerName özelliğine ekleyerek ConcurrencyCheck 'in nasıl çalıştığını görelim.</span><span class="sxs-lookup"><span data-stu-id="a53fc-184">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="a53fc-185">SaveChanges çağrıldığında, BloggerName alanındaki ConcurrencyCheck ek açıklaması nedeniyle, bu özelliğin özgün değeri güncelleştirmede kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-185">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="a53fc-186">Komutu doğru satırı yalnızca anahtar değerde değil, BloggerName özgün değerine göre filtreleyerek bulmaya çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-186">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span><span data-ttu-id="a53fc-187">  GÜNCELLEŞTIRME komutunun veritabanına gönderilen kritik bölümleri aşağıda verilmiştir. komutun, bir PrimaryTrackingKey değeri 1 ve bir BloggerName "Julie" olan satırı veritabanından alındığı zaman özgün değeri olan "" olarak güncelleştiği, burada görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-187">  Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="a53fc-188">Bu, bu bloga ait blogu adını bu sırada değiştirdiyseniz, bu güncelleştirme başarısız olur ve işlemeniz gereken bir DbUpdateConcurrencyException alırsınız.</span><span class="sxs-lookup"><span data-stu-id="a53fc-188">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="a53fc-189">Ilişkin</span><span class="sxs-lookup"><span data-stu-id="a53fc-189">TimeStamp</span></span>

<span data-ttu-id="a53fc-190">Eşzamanlılık denetimi için rowversion veya timestamp alanlarını kullanmak daha yaygındır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-190">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="a53fc-191">Ancak, ConcurrencyCheck ek açıklamasını kullanmak yerine, özelliğin türü byte dizisi olduğu sürece daha belirli zaman damgası ek açıklamasını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-191">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="a53fc-192">Önce kod zaman damgası özelliklerini ConcurrencyCheck özellikleriyle aynı kabul eder, ancak kodun ilk oluşturduğu veritabanı alanının null değer atanamaz olduğundan da emin olur.</span><span class="sxs-lookup"><span data-stu-id="a53fc-192">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="a53fc-193">Belirli bir sınıfta yalnızca bir zaman damgası özelliğine sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-193">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="a53fc-194">Aşağıdaki özelliği blog sınıfına ekleme:</span><span class="sxs-lookup"><span data-stu-id="a53fc-194">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="a53fc-195">kod sonuçları, veritabanı tablosunda null yapılamayan bir zaman damgası sütunu oluşturuyor.</span><span class="sxs-lookup"><span data-stu-id="a53fc-195">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![Zaman damgası sütunuyla Bloglar tablosu](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="a53fc-197">Tablo ve sütun</span><span class="sxs-lookup"><span data-stu-id="a53fc-197">Table and Column</span></span>

<span data-ttu-id="a53fc-198">Veritabanı oluşturmaya Code First izin verirseniz, oluşturduğu tablo ve sütunların adını değiştirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-198">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="a53fc-199">Ayrıca, var olan bir veritabanıyla Code First de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-199">You can also use Code First with an existing database.</span></span> <span data-ttu-id="a53fc-200">Ancak, etki alanındaki sınıfların ve özelliklerin adlarının veritabanınızdaki tablo ve sütunların adlarıyla eşleşmesi her zaman değildir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-200">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="a53fc-201">Sınıfım, blog ve kurala göre adlandırılır, kod ise bu, blogların adlı bir tabloyla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-201">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="a53fc-202">Böyle bir durum söz konusu değilse tablo özniteliği ile tablonun adını belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-202">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="a53fc-203">Örneğin, ek açıklama tablo adının InternalBlogs olduğunu belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-203">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="a53fc-204">Sütun ek açıklaması, eşlenmiş bir sütunun özniteliklerini belirtmekte olan bir Apart ' dır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-204">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="a53fc-205">Bir ad, veri türü ya da tabloda bir sütunun göründüğü sıra gibi bir ad girebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-205">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="a53fc-206">Sütun özniteliğine bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-206">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="a53fc-207">Sütunun TypeName özniteliğini DataType DataAnnotation ile karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="a53fc-207">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="a53fc-208">Veri türü, Kullanıcı arabirimi için kullanılan bir ek açıklama ve Code First tarafından yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-208">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="a53fc-209">Oluşturulduktan sonra tablo aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-209">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="a53fc-210">Tablo adı InternalBlogs olarak değiştirilmiştir ve karmaşık türden Açıklama sütunu artık BlogDescription.</span><span class="sxs-lookup"><span data-stu-id="a53fc-210">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="a53fc-211">Ad ek açıklamada belirtildiğinden, önce kod sütun adını karmaşık türün adı ile başlatma kuralını kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-211">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![Bloglar tablosu ve sütunu yeniden adlandırıldı](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="a53fc-213">Veritabanı oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="a53fc-213">DatabaseGenerated</span></span>

<span data-ttu-id="a53fc-214">Önemli bir veritabanı özellikleri, hesaplanmış Özellikler kullanabilme özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-214">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="a53fc-215">Code First sınıflarınızı, hesaplanan sütunları içeren tablolarla eşleştirmeniz durumunda, bu sütunları güncelleştirmeyi denemek Entity Framework istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-215">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="a53fc-216">Ancak verileri ekledikten veya güncelleştirdikten sonra bu değerleri veritabanından döndürmesini isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-216">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="a53fc-217">Sınıfınızın bu özelliklerini hesaplanan Enum ile birlikte işaretlemek için DatabaseGenerated ek açıklamasını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-217">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="a53fc-218">Diğer numaralandırmalar None ve Identity 'Tur.</span><span class="sxs-lookup"><span data-stu-id="a53fc-218">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="a53fc-219">Kod ilk veritabanını oluştururken bayt veya zaman damgası sütunlarında oluşturulan veritabanını kullanabilirsiniz; Aksi takdirde, kod ilk olarak hesaplanan sütun formülünü belirleyemeyeceği için yalnızca var olan veritabanlarına işaret ederken bunu kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-219">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="a53fc-220">Yukarıdaki, varsayılan olarak bir tamsayı olan anahtar özelliği veritabanında bir kimlik anahtarı olacak şekilde daha fazla bilgi edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-220">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="a53fc-221">Bu, DatabaseGeneratedOption. Identity olarak oluşturulan databasesetting ile aynı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-221">That would be the same as setting DatabaseGenerated to DatabaseGeneratedOption.Identity.</span></span> <span data-ttu-id="a53fc-222">Kimlik anahtarı olmasını istemiyorsanız, bu değeri DatabaseGeneratedOption. None olarak ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-222">If you do not want it to be an identity key, you can set the value to DatabaseGeneratedOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="a53fc-223">Dizin oluşturma</span><span class="sxs-lookup"><span data-stu-id="a53fc-223">Index</span></span>

> [!NOTE]
> <span data-ttu-id="a53fc-224">**EF 6.1 yalnızca** sonraki sürümler-dizin özniteliği Entity Framework 6,1 ' de tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="a53fc-224">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="a53fc-225">Önceki bir sürümü kullanıyorsanız, bu bölümdeki bilgiler uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-225">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="a53fc-226">**Indexattribute**kullanarak bir veya daha fazla sütunda bir dizin oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-226">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="a53fc-227">Özniteliği bir veya daha fazla özelliğe eklemek, EF veritabanını oluşturduğunda karşılık gelen dizini veritabanında oluşturmasına veya Code First Migrations kullanıyorsanız karşılık gelen CreateIndex çağrılarını dolandırıcıya **dönüştürmesine** neden olur.</span><span class="sxs-lookup"><span data-stu-id="a53fc-227">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="a53fc-228">Örneğin, aşağıdaki kod veritabanındaki **postalar** tablosunun **Derecelendirme** sütununda oluşturulacak bir dizin oluşmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="a53fc-228">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

<span data-ttu-id="a53fc-229">Varsayılan olarak, Dizin **x\_&lt;Özellik adı&gt;** (Yukarıdaki örnekte x\_derecelendirmesi) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-229">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="a53fc-230">Ayrıca, dizin için de bir ad belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-230">You can also specify a name for the index though.</span></span> <span data-ttu-id="a53fc-231">Aşağıdaki örnek, dizininin **Postraytingındex**olarak adlandırılması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-231">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="a53fc-232">Varsayılan olarak, dizinler benzersiz değildir, ancak bir dizinin benzersiz olması gerektiğini belirtmek için **IsUnique** adlandırılmış parametresini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-232">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="a53fc-233">Aşağıdaki örnekte, **kullanıcının**oturum açma adında benzersiz bir dizin tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-233">The following example introduces a unique index on a **User**'s login name.</span></span>

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a><span data-ttu-id="a53fc-234">Birden çok sütunlu dizinler</span><span class="sxs-lookup"><span data-stu-id="a53fc-234">Multiple-Column Indexes</span></span>

<span data-ttu-id="a53fc-235">Birden çok sütuna yayılan dizinler, belirli bir tablo için birden fazla dizin ek açıklamasında aynı ad kullanılarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-235">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="a53fc-236">Çok sütunlu dizinler oluşturduğunuzda, dizindeki sütunlar için bir sıra belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-236">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="a53fc-237">Örneğin, aşağıdaki kod, **Derecelendirme** ve **blogID** **\_blogidandrating**adlı bir çok sütunlu dizin oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a53fc-237">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogIdAndRating**.</span></span> <span data-ttu-id="a53fc-238">**BlogID** dizindeki ilk sütundur ve **Derecelendirme ikincisdir** .</span><span class="sxs-lookup"><span data-stu-id="a53fc-238">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="a53fc-239">İlişki öznitelikleri: Evirseproperty ve ForeignKey</span><span class="sxs-lookup"><span data-stu-id="a53fc-239">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="a53fc-240">Bu sayfa, veri ek açıklamalarını kullanarak Code First modelinizde ilişkiler ayarlama hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a53fc-240">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="a53fc-241">EF 'teki ilişkiler ve ilişkileri kullanarak verilere erişme ve verileri işleme hakkında genel bilgi için bkz. [relationships & gezinti özellikleri](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="a53fc-241">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="a53fc-242">Kod ilk kuralı modelinizdeki en yaygın ilişkilerle ilgilenirken, ancak yardım gerektiren bazı durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-242">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="a53fc-243">Blog sınıfındaki anahtar özelliğinin adının değiştirilmesi, gönderiyle ilişkili bir sorun oluşturdu.</span><span class="sxs-lookup"><span data-stu-id="a53fc-243">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="a53fc-244">Veritabanı oluşturulurken, kod ilk olarak post sınıfında blogID özelliğini görür ve onu bir sınıf adı artı "kimlik" olarak, blog sınıfına yabancı anahtar olarak eşleşen kurala göre tanır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-244">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="a53fc-245">Ancak blog sınıfında blogID özelliği yok.</span><span class="sxs-lookup"><span data-stu-id="a53fc-245">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="a53fc-246">Bunun çözümü, göndermede bir gezinti özelliği oluşturmak ve yabancı veri ek açıklamasını kullanarak kodun ilk olarak iki sınıf arasında ilişki oluşturma (Post. blogID özelliğini kullanarak) ve içinde kısıtlamaları belirtme veritabanınızı.</span><span class="sxs-lookup"><span data-stu-id="a53fc-246">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="a53fc-247">Veritabanındaki kısıtlama, InternalBlogs. PrimaryTrackingKey ve gönderimleri. blogID arasındaki ilişkiyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-247">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![InternalBlogs. PrimaryTrackingKey ve gönderimleri. blogID arasındaki ilişki](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="a53fc-249">Ters Çevir özelliği, sınıflar arasında birden fazla ilişksahipseniz kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a53fc-249">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="a53fc-250">Post sınıfında, bir blog gönderisi yazanın yanı sıra kimin düzenlediğinden haberdar olmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-250">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="a53fc-251">Post sınıfının iki yeni gezinti özelliği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-251">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="a53fc-252">Ayrıca, bu özellikler tarafından başvurulan kişi sınıfına da eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-252">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="a53fc-253">Kişi sınıfının gönderiye geri dönmesi, biri kişi tarafından yazılan gönderilerin hepsi ve bu kişi tarafından güncellenen tüm gönderiler için bir tane.</span><span class="sxs-lookup"><span data-stu-id="a53fc-253">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="a53fc-254">Önce kod, kendi kendine iki sınıftaki özellikleri eşleşmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-254">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="a53fc-255">Gönderimler için veritabanı tablosunda, CreatedBy kişiye ait bir yabancı anahtar ve bir tane de UpdatedBy kişiye ait olmalıdır, ancak kod ilk olarak dört yabancı anahtar özelliği oluşturur: kişi\_kimliği, kişi\_ID1, CreatedBy\_ID ve UpdatedBy\_ID.</span><span class="sxs-lookup"><span data-stu-id="a53fc-255">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![Ek yabancı anahtarlarla nakleder tablosu](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="a53fc-257">Bu sorunları çözebilmeniz için, özelliklerin hizalamasını belirtmek üzere Evirseproperty ek açıklamasını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-257">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="a53fc-258">Bu kişiye ait Postsyazıldığı özelliği, bu, post türüne başvurduğunu bildiği için, bu, gönderi. CreatedBy ile ilişki oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a53fc-258">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="a53fc-259">Benzer şekilde, PostsUpdated, posta. UpdatedBy Ile gönderilir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-259">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="a53fc-260">Ve ilk olarak kod, ek yabancı anahtarlar oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-260">And code first will not create the extra foreign keys.</span></span>

![Ek yabancı anahtarlar olmadan gönderi tablosu](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="a53fc-262">Özet</span><span class="sxs-lookup"><span data-stu-id="a53fc-262">Summary</span></span>

<span data-ttu-id="a53fc-263">Veri açıklamaları yalnızca kodunuzun ilk sınıflarında istemci ve sunucu tarafı doğrulamasını açıklamanıza izin vermekle kalmaz, aynı zamanda kodun kuralları temel alınarak sınıflarınız hakkında daha fazla bilgi sahibi olacağı varsayımları geliştirmenize ve hatta düzeltmenizi de sağlar.</span><span class="sxs-lookup"><span data-stu-id="a53fc-263">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="a53fc-264">Dataaçıklamalarla yalnızca veritabanı şeması oluşturmayı kullanamazsınız, ancak kod ilk sınıflarınızı önceden var olan bir veritabanına eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a53fc-264">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="a53fc-265">Çok esnek olsa da, Datanot açıklamalarının yalnızca kod ilk sınıflarınızda yapabileceğiniz en sık gereken yapılandırma değişikliklerini sunabileceini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a53fc-265">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="a53fc-266">Sınıflarınızı bazı uç durumlardan bazılarına göre yapılandırmak için, farklı yapılandırma mekanizmasına, Code First akıcı API 'sine bakmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a53fc-266">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
