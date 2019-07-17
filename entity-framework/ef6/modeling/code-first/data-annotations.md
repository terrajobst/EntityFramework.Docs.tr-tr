---
title: İlk veri ek açıklamaları - EF6 kod
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: fcd01aef7303573001460b352f8099b2cc6e224a
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286475"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="46ea2-102">Kod ilk veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="46ea2-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="46ea2-103">**EF4.1 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 4.1 içinde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="46ea2-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="46ea2-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bu bilgilerin geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-104">If you are using an earlier version, some or all of this information does not apply.</span></span>

<span data-ttu-id="46ea2-105">Bu sayfadaki içeriğin başlangıçta Julie Lerman tarafından yazılmış bir makaledeki uyarlanmış (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="46ea2-105">The content on this page is adapted from an article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="46ea2-106">Entity Framework Code First EF sorgulama, gerçekleştirilecek dayanan modeli temsil etmek için kendi etki alanı sınıfları kullanmanıza olanak tanır değişiklik izleme ve işlevleri güncelleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="46ea2-106">Entity Framework Code First allows you to use your own domain classes to represent the model that EF relies on to perform querying, change tracking, and updating functions.</span></span> <span data-ttu-id="46ea2-107">Kod 'kuralı yapılandırmanız üzerinde.' olarak adlandırılan bir programlama modeli ilk yararlanır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-107">Code First leverages a programming pattern referred to as 'convention over configuration.'</span></span> <span data-ttu-id="46ea2-108">Kod ilk sınıflarınızı Entity Framework kurallarını izleyin ve bu durumda, öğrenmek, işi gerçekleştirmek için otomatik olarak çalışacak varsayar.</span><span class="sxs-lookup"><span data-stu-id="46ea2-108">Code First will assume that your classes follow the conventions of Entity Framework, and in that case, will automatically work out how to perform it's job.</span></span> <span data-ttu-id="46ea2-109">Ancak, sınıflarınızı bu kuralları uygulamazsanız sınıflarınızı EF önkoşul bilgilerini sağlamak için yapılandırmaları ekleme olanağına sahip olursunuz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-109">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the requisite information.</span></span>

<span data-ttu-id="46ea2-110">Kod öncelikle bu yapılandırmalar, sınıfa eklemek için iki yol sunar.</span><span class="sxs-lookup"><span data-stu-id="46ea2-110">Code First gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="46ea2-111">Bir basit öznitelikleri DataAnnotations adlı kullanıyor ve ikinci Code First's Fluent yapılandırmaları kesin, kodda açıklamak için bir yol sağlayan API kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="46ea2-111">One is using simple attributes called DataAnnotations, and the second is using Code First’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="46ea2-112">Bu makalede, en sık gerekli yapılandırmaları vurgulama – sınıflarınızı yapılandırmak için (System.ComponentModel.DataAnnotations ad alanında) DataAnnotations üzerinden odaklanır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-112">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="46ea2-113">DataAnnotations ayrıca .NET uygulamaları, bu uygulamalar, istemci tarafı doğrulama için aynı ek açıklamalar yararlanmasını sağlayan bir ASP.NET MVC gibi bir dizi tarafından anlaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-113">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="46ea2-114">Model</span><span class="sxs-lookup"><span data-stu-id="46ea2-114">The model</span></span>

<span data-ttu-id="46ea2-115">Ben sınıfları için basit bir çift kod ilk DataAnnotations kazandırabileceğinizi göstereceğiz: Blog ve gönderi.</span><span class="sxs-lookup"><span data-stu-id="46ea2-115">I’ll demonstrate Code First DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

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

<span data-ttu-id="46ea2-116">Olduğu gibi Blog ve gönderi sınıfları rahatça kod ilk kuralını izler ve EF uyumluluk etkinleştirmek için hiçbir tweaks gerektirir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-116">As they are, the Blog and Post classes conveniently follow code first convention and require no tweaks to enable EF compatability.</span></span> <span data-ttu-id="46ea2-117">Ancak, sınıfları ve bunların eşleneceğine veritabanı hakkında daha fazla bilgi için EF sağlamak için ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-117">However, you can also use the annotations to provide more information to EF about the classes and the database to which they map.</span></span>

 

## <a name="key"></a><span data-ttu-id="46ea2-118">Anahtar</span><span class="sxs-lookup"><span data-stu-id="46ea2-118">Key</span></span>

<span data-ttu-id="46ea2-119">Entity Framework varlık izleme için kullanılan bir anahtar değere sahip her varlık kullanır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-119">Entity Framework relies on every entity having a key value that is used for entity tracking.</span></span> <span data-ttu-id="46ea2-120">Bir Code First, örtük anahtar özellikleri kuraldır; Kod, önce "Id" veya sınıf adı ve "Id" gibi "BlogId" adlı bir özellik arar.</span><span class="sxs-lookup"><span data-stu-id="46ea2-120">One convention of Code First is implicit key properties; Code First will look for a property named “Id”, or a combination of class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="46ea2-121">Bu özellik için bir birincil anahtar sütunu veritabanında eşler.</span><span class="sxs-lookup"><span data-stu-id="46ea2-121">This property will map to a primary key column in the database.</span></span>

<span data-ttu-id="46ea2-122">Blog ve gönderi sınıfları bu kural izleyin.</span><span class="sxs-lookup"><span data-stu-id="46ea2-122">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="46ea2-123">Bunlar ne oldu?</span><span class="sxs-lookup"><span data-stu-id="46ea2-123">What if they didn’t?</span></span> <span data-ttu-id="46ea2-124">Peki Blog kullanılan adı *PrimaryTrackingKey* bunun yerine veya hatta *foo*?</span><span class="sxs-lookup"><span data-stu-id="46ea2-124">What if Blog used the name *PrimaryTrackingKey* instead, or even *foo*?</span></span> <span data-ttu-id="46ea2-125">Kod öncelikle bu kuralıyla eşleşen bir özellik bulamazsa bir anahtarı özelliği olması gerekir, Entity Framework'ün gereksinimi nedeniyle bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="46ea2-125">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="46ea2-126">Anahtar ek açıklama EntityKey kullanılacak hangi özelliğinin olduğunu belirtmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-126">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

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

<span data-ttu-id="46ea2-127">Eğer ilk kod kullanarak veritabanı oluşturma özelliği, Blog tablonun varsayılan olarak kimlik olarak da tanımlanır PrimaryTrackingKey adlı birincil anahtar sütunu gerekir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-127">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey, which is also defined as Identity by default.</span></span>

![Blog tabloda birincil anahtar ile](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="46ea2-129">Bileşik anahtarlar</span><span class="sxs-lookup"><span data-stu-id="46ea2-129">Composite keys</span></span>

<span data-ttu-id="46ea2-130">Entity Framework, bileşik anahtarlar - birden fazla özelliği içeren birincil anahtarları destekler.</span><span class="sxs-lookup"><span data-stu-id="46ea2-130">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="46ea2-131">Örneğin, birincil anahtarı PassportNumber ve IssuingCountry bir birleşimi olan bir Passport sınıfı olabilir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-131">For example, you could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

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

<span data-ttu-id="46ea2-132">Yukarıdaki sınıfı EF modelinizde kullanılmaya çalışılıyor sonuçlanır bir `InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="46ea2-132">Attempting to use the above class in your EF model would result in an `InvalidOperationException`:</span></span>

<span data-ttu-id="46ea2-133">*Bileşik birincil anahtar türü için 'Passport' sıralama belirlenemiyor. ColumnAttribute veya HasKey yöntemi bileşik birincil anahtarlar için sipariş belirtmek için kullanın.*</span><span class="sxs-lookup"><span data-stu-id="46ea2-133">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="46ea2-134">Bileşik anahtarlar kullanmak için Entity Framework anahtar özellikleri için bir sipariş tanımlamanızı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-134">In order to use composite keys, Entity Framework requires you to define an order for the key properties.</span></span> <span data-ttu-id="46ea2-135">Bir sıra belirtmek için sütunu ek açıklama kullanarak bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-135">You can do this by using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="46ea2-136">(Dizin temelinde güncellememek yerine) sıra değeri herhangi bir değer kullanılabilmesi için görelidir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-136">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="46ea2-137">Örneğin, 100 ve 200 1 ve 2 yerine kabul edilebilir olacaktır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-137">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

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

<span data-ttu-id="46ea2-138">Bileşik yabancı anahtarları varlık varsa, karşılık gelen birincil anahtar özellikleri için kullanılan aynı sütun belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-138">If you have entities with composite foreign keys, then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="46ea2-139">Yalnızca göreli yabancı anahtar özellikleri içinde sıralamasını aynı, atanan değerleri tam olması gerekir **sipariş** eşleşmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="46ea2-139">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="46ea2-140">Örneğin, aşağıdaki sınıfında, 3 ve 4 1 ve 2 yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-140">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

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

## <a name="required"></a><span data-ttu-id="46ea2-141">Gerekli</span><span class="sxs-lookup"><span data-stu-id="46ea2-141">Required</span></span>

<span data-ttu-id="46ea2-142">Gerekli ek açıklama EF belirli bir özellik gereklidir bildirir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-142">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="46ea2-143">Başlık özelliği için gerekli ekleme özelliği veri içerdiğinden emin olmak için EF (ve MVC) zorlar.</span><span class="sxs-lookup"><span data-stu-id="46ea2-143">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="46ea2-144">Hiçbir ek kod veya uygulama biçimlendirme değişikliklerle, bir MVC uygulaması dinamik olarak bile özellik ve ek açıklama adları kullanarak bir ileti oluşturma, istemci tarafı doğrulama gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-144">With no additional code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Oluşturma sayfasıdır başlıklı gerekli hata](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="46ea2-146">Gerekli öznitelik eşlenen özelliği null olamaz yapmanızı tarafından oluşturulan veritabanı da etkiler.</span><span class="sxs-lookup"><span data-stu-id="46ea2-146">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="46ea2-147">Başlık alanı "için null olmayan" olarak değiştiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="46ea2-147">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="46ea2-148">Bazı durumlarda veritabanını özelliği gerekli olsa bile null yapılamaz sütunda mümkün olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-148">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="46ea2-149">Örneğin, ne zaman TPH devralma stratejisi veri için birden fazla türü kullanılarak tek bir tabloda depolanır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-149">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="46ea2-150">Gerekli bir özellik türetilmiş bir tür içeriyorsa, bu özellik hiyerarşideki tüm türleri olduğundan sütun atanamayan yapılamaz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-150">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![Bloglar tablo](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="46ea2-152">MaxLength ve MinLength</span><span class="sxs-lookup"><span data-stu-id="46ea2-152">MaxLength and MinLength</span></span>

<span data-ttu-id="46ea2-153">Yalnızca gerekli olduğu gibi MaxLength ve MinLength öznitelikleri ek özellik doğrulamaları belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="46ea2-153">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="46ea2-154">BloggerName uzunluğu gereksinimleri aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-154">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="46ea2-155">Örnek ayrıca nasıl birleştirileceğini öznitelikleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-155">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="46ea2-156">MaxLength ek açıklaması özelliğinin uzunluğu 10'a ayarlayarak veritabanı etkiler.</span><span class="sxs-lookup"><span data-stu-id="46ea2-156">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![En büyük uzunluk BloggerName sütunu gösteren blogları tablo](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="46ea2-158">MVC istemci-tarafı ek açıklama ve EF 4.1 sunucu tarafı ek açıklama hem de bir hata iletisi dinamik olarak yeniden oluşturma, bu doğrulama yerine getirir: "Alan BloggerName '10' en fazla uzunluğu bir dize veya dizi türü olmalıdır." Bu iletiyi biraz daha uzun.</span><span class="sxs-lookup"><span data-stu-id="46ea2-158">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="46ea2-159">Birçok ek açıklamaları ErrorMessage özniteliğiyle bir hata iletisi belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="46ea2-159">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="46ea2-160">Ayrıca, gerekli ek açıklamada ErrorMessage belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-160">You can also specify ErrorMessage in the Required annotation.</span></span>

![Özel hata iletisiyle sayfası oluşturma](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="46ea2-162">NotMapped</span><span class="sxs-lookup"><span data-stu-id="46ea2-162">NotMapped</span></span>

<span data-ttu-id="46ea2-163">Kod ilk kuralı, veritabanında bir desteklenen veri türü her bir özellik temsil edildiğini belirler.</span><span class="sxs-lookup"><span data-stu-id="46ea2-163">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="46ea2-164">Ancak bu her zaman uygulamalarınızı durumda değildir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-164">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="46ea2-165">Örneğin başlık ve BloggerName alanlara göre bir kod oluşturur Blog sınıftaki bir özelliği olabilir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-165">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="46ea2-166">Bu özellik, dinamik olarak oluşturulabilir ve depolanması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="46ea2-166">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="46ea2-167">Bu BlogCode özelliği gibi NotMapped ek açıklama ile veritabanına eşlemeyin herhangi bir özelliği işaretleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-167">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

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

 

## <a name="complextype"></a><span data-ttu-id="46ea2-168">ComplexType</span><span class="sxs-lookup"><span data-stu-id="46ea2-168">ComplexType</span></span>

<span data-ttu-id="46ea2-169">Etki alanı varlıklarınızı sınıf kümesi açıklamak ve eksiksiz bir varlık tanımlamak için bu sınıflardan sonra katman durumdur.</span><span class="sxs-lookup"><span data-stu-id="46ea2-169">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="46ea2-170">Örneğin, modelinize BlogDetails adlı bir sınıf ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-170">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="46ea2-171">Anahtar özelliği herhangi bir türde BlogDetails yok dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="46ea2-171">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="46ea2-172">Etki alanı Odaklı Tasarım içinde BlogDetails bir değer nesnesi olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-172">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="46ea2-173">Varlık çerçevesi karmaşık türler değer nesnelere başvurur.</span><span class="sxs-lookup"><span data-stu-id="46ea2-173">Entity Framework refers to value objects as complex types.</span></span><span data-ttu-id="46ea2-174">  Karmaşık türler, kendi izlenemez.</span><span class="sxs-lookup"><span data-stu-id="46ea2-174">  Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="46ea2-175">Ancak, izleniyor Blog nesnesinin bir parçası BlogDetails Blog sınıf özelliği olarak.</span><span class="sxs-lookup"><span data-stu-id="46ea2-175">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="46ea2-176">Bu ilk tanımak kod için sırada BlogDetails sınıfı bir ComplexType işaretlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-176">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="46ea2-177">Artık bu blog BlogDetails temsil etmek için Blog sınıftaki bir özelliği ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-177">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="46ea2-178">Veritabanında Blog tablo adını BlogDetail özelliğinde yer alan özellikler dahil olmak üzere blog özelliklerin tümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-178">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="46ea2-179">Varsayılan olarak, her biri ile BlogDetail karmaşık türün adı gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-179">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![Karmaşık tür tabloyla blogu](~/ef6/media/jj591583-figure06.png)


## <a name="concurrencycheck"></a><span data-ttu-id="46ea2-181">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="46ea2-181">ConcurrencyCheck</span></span>

<span data-ttu-id="46ea2-182">ConcurrencyCheck ek açıklama, veritabanında bir kullanıcı, düzenler veya bir varlığı silen denetimi eşzamanlılık için kullanılacak bir veya daha fazla özellik bayrağını olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-182">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="46ea2-183">EF Designer ile aşinaysanız, sabit bir özelliğin ConcurrencyMode ayarı ile hizalar.</span><span class="sxs-lookup"><span data-stu-id="46ea2-183">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="46ea2-184">BloggerName özelliğini ekleyerek ConcurrencyCheck nasıl çalıştığını görelim.</span><span class="sxs-lookup"><span data-stu-id="46ea2-184">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="46ea2-185">SaveChanges çağrıldığında ConcurrencyCheck üzerindeki ek açıklama BloggerName alanı nedeniyle güncelleştirme bu özellik, özgün değer kullanılır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-185">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="46ea2-186">Komut doğru satır yalnızca anahtar değeri aynı zamanda BloggerName özgün değeri filtreleyerek bulmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-186">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span><span data-ttu-id="46ea2-187">  Komutu bir PrimaryTrackingKey sahip olan satırı güncelleştirin görebileceğiniz veritabanına gönderilen güncelleştirme komut kritik bölümleri, 1 ve "Bu blog veritabanından alınırken, özgün değer Julie", bir BloggerName şunlardır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-187">  Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="46ea2-188">Birisi bu blog blogger adı sırada değişti, bu güncelleştirme başarısız olur ve işlemek için gereken bir DbUpdateConcurrencyException elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-188">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="46ea2-189">Zaman damgası</span><span class="sxs-lookup"><span data-stu-id="46ea2-189">TimeStamp</span></span>

<span data-ttu-id="46ea2-190">Eşzamanlılık denetimi için rowversion veya zaman damgası alanı kullanımı daha yaygındır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-190">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="46ea2-191">Ancak bayt dizisi özellik türü olduğu sürece ConcurrencyCheck ek açıklama kullanmak yerine daha belirli zaman damgası ek açıklama kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-191">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="46ea2-192">Kod ilk zaman damgası özellikleri aynı ConcurrencyCheck özellikleri olarak değerlendirir, ancak aynı zamanda kod oluşturan veritabanı alanını atanamaz olduğundan emin olmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="46ea2-192">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="46ea2-193">Bu gibi durumlarda, bir zaman damgası özelliği yalnızca belirli bir sınıf içinde olabilir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-193">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="46ea2-194">Aşağıdaki özellikler, Blog sınıfına ekleme:</span><span class="sxs-lookup"><span data-stu-id="46ea2-194">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="46ea2-195">ilk veritabanı tablosu, bir NULL olmayan bir zaman damgası sütunu oluşturma kod sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-195">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![Zaman damgası sütunu tabloyla blogları](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="46ea2-197">Tablo ve sütun</span><span class="sxs-lookup"><span data-stu-id="46ea2-197">Table and Column</span></span>

<span data-ttu-id="46ea2-198">Code First veritabanı oluşturma izni vermiş olursunuz, tabloları ve sütunları sanal makinesi adını değiştirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-198">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="46ea2-199">Code First ile varolan bir veritabanını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-199">You can also use Code First with an existing database.</span></span> <span data-ttu-id="46ea2-200">Ancak, her zaman sınıfları ve özellikleri, etki alanınızdaki adları tabloları ve sütunları veritabanınızdaki adlarının eşleştiğini durumda değil.</span><span class="sxs-lookup"><span data-stu-id="46ea2-200">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="46ea2-201">My sınıfı Blog olarak adlandırılır ve bu blogları adlı bir tabloya eşler ilk kural olarak, kod varsayar.</span><span class="sxs-lookup"><span data-stu-id="46ea2-201">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="46ea2-202">Bu durumda değilse tablo özniteliğiyle tablonun adını belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-202">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="46ea2-203">Burada Örneğin, ek açıklama tablo adını InternalBlogs olduğunu belirler.</span><span class="sxs-lookup"><span data-stu-id="46ea2-203">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="46ea2-204">Sütun ek açıklama eşlenen sütun özniteliklerini belirtilirken bir daha fazla yatkın olduğu.</span><span class="sxs-lookup"><span data-stu-id="46ea2-204">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="46ea2-205">Bir ad, veri türü veya hatta bir sütun tabloda göründüğü sırayı koşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-205">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="46ea2-206">Sütun özniteliği örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-206">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="46ea2-207">Sütunun TypeName öznitelik veri türü DataAnnotation ile karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="46ea2-207">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="46ea2-208">Veri türü, kullanıcı Arabiriminde kullanılan ek açıklamanın olup ve Code First tarafından göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-208">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="46ea2-209">Yeniden oluşturulduğunda sonra tablosu aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-209">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="46ea2-210">Tablo adı için InternalBlogs değişti ve karmaşık tür tanımı sütundan BlogDescription sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="46ea2-210">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="46ea2-211">Ek açıklamada ad belirtilmediğinden, kod sütun adı karmaşık tür adı ile başlangıç kuralı ilk kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-211">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![Bloglar tablo ve sütun olarak yeniden adlandırıldı](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="46ea2-213">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="46ea2-213">DatabaseGenerated</span></span>

<span data-ttu-id="46ea2-214">Hesaplanan özellikler olanağı önemli veritabanı özellikleri var.</span><span class="sxs-lookup"><span data-stu-id="46ea2-214">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="46ea2-215">Code First sınıflarınızı eşleniyorsa, hesaplanan sütunlar içeren tablolar, varlık Çerçevesi'bu sütunları güncelleştirmeye çalışmak istemediğiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-215">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="46ea2-216">Ancak, veritabanından eklenmiş veya güncelleştirilmiş verileri sonra bu değerleri döndürülecek EF istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-216">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="46ea2-217">Sınıfınızda, hesaplanan enum yanı sıra özelliklere işaretleyemedi DatabaseGenerated ek açıklama kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-217">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="46ea2-218">Diğer numaralandırmalar yok ve kimlik.</span><span class="sxs-lookup"><span data-stu-id="46ea2-218">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="46ea2-219">Kod ilk veritabanı oluşturulurken bayt veya zaman damgası sütuna oluşturulan veritabanı kullanabilir, aksi durumda yalnızca bu kod önce hesaplanan sütun için formülün belirlemek mümkün olmayacağından, mevcut veritabanlarının işaret ettiğinde kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-219">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="46ea2-220">Varsayılan olarak, okuma, tamsayı olan bir anahtar özellik veritabanındaki bir kimlik anahtarı olur.</span><span class="sxs-lookup"><span data-stu-id="46ea2-220">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="46ea2-221">DatabaseGenerated DatabaseGeneratedOption.Identity için ayarı ile aynı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-221">That would be the same as setting DatabaseGenerated to DatabaseGeneratedOption.Identity.</span></span> <span data-ttu-id="46ea2-222">Bir kimlik anahtarı olmasını istemiyorsanız DatabaseGeneratedOption.None için değeri ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-222">If you do not want it to be an identity key, you can set the value to DatabaseGeneratedOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="46ea2-223">Dizin</span><span class="sxs-lookup"><span data-stu-id="46ea2-223">Index</span></span>

> [!NOTE]
> <span data-ttu-id="46ea2-224">**EF6.1 ve sonraki sürümler yalnızca** -Entity Framework 6.1 içinde dizin özniteliği tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-224">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="46ea2-225">Önceki bir sürümü kullanıyorsanız, bu bölümdeki bilgiler, geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-225">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="46ea2-226">Bir veya daha fazla sütuna kullanarak bir dizin oluşturabilirsiniz **IndexAttribute**.</span><span class="sxs-lookup"><span data-stu-id="46ea2-226">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="46ea2-227">Öznitelik için bir veya daha fazla özellik ekleyerek EF veritabanında oluşturduğunda, veritabanında ilgili dizini oluşturmak neden veya karşılık gelen iskelesini **CreateIndex** Code First Migrations'ı kullanıyorsanız çağırır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-227">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="46ea2-228">Örneğin, aşağıdaki kod üzerinde oluşturulan bir dizin sonuçlanır **derecelendirme** sütununun **gönderileri** veritabanındaki tablo.</span><span class="sxs-lookup"><span data-stu-id="46ea2-228">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

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

<span data-ttu-id="46ea2-229">Varsayılan olarak, dizin adlandırılacağını **IX\_&lt;özellik adı&gt;**  (IX\_yukarıdaki örnekte derecelendirme).</span><span class="sxs-lookup"><span data-stu-id="46ea2-229">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="46ea2-230">Dizini için bir ad olsa da belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-230">You can also specify a name for the index though.</span></span> <span data-ttu-id="46ea2-231">Aşağıdaki örnek dizinin adlandırılmalıdır belirtir **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="46ea2-231">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="46ea2-232">Varsayılan olarak, dizinleri benzersiz olmayan, ancak kullanabileceğiniz **IsUnique** adlı bir dizin benzersiz olması gerektiğini belirtmek için parametre.</span><span class="sxs-lookup"><span data-stu-id="46ea2-232">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="46ea2-233">Aşağıdaki örnekte benzersiz bir dizin üzerinde tanıtır. bir **kullanıcı**ait oturum açma adı.</span><span class="sxs-lookup"><span data-stu-id="46ea2-233">The following example introduces a unique index on a **User**'s login name.</span></span>

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

### <a name="multiple-column-indexes"></a><span data-ttu-id="46ea2-234">Birden çok sütun dizinleri</span><span class="sxs-lookup"><span data-stu-id="46ea2-234">Multiple-Column Indexes</span></span>

<span data-ttu-id="46ea2-235">Birden fazla sütuna yayılmış dizinler için belirli bir tabloda birden fazla dizin ek açıklamalar aynı adı kullanarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-235">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="46ea2-236">Çok sütunlu dizinleri oluşturduğunuzda, sipariş sütunlar için dizin belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-236">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="46ea2-237">Örneğin, aşağıdaki kod üzerinde çok sütunlu bir dizin oluşturur **derecelendirme** ve **BlogId** adlı **IX\_BlogIdAndRating**.</span><span class="sxs-lookup"><span data-stu-id="46ea2-237">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogIdAndRating**.</span></span> <span data-ttu-id="46ea2-238">**BlogId** dizini ilk sütunda ve **derecelendirme** saniyedir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-238">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="46ea2-239">İlişki öznitelikleri: InverseProperty ve ForeignKey</span><span class="sxs-lookup"><span data-stu-id="46ea2-239">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="46ea2-240">Bu sayfa, Code First modelinizde veri ek açıklamalarını kullanma ilişkileri ayarlama hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="46ea2-240">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="46ea2-241">EF ve erişmek ve ilişkileri kullanarak verileri işlemek nasıl ilişkiler hakkında genel bilgi için bkz. [ilişkileri ve gezinti özellikleri](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="46ea2-241">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="46ea2-242">Kod ilk kuralı modelinizdeki en yaygın ilişki ölçeklendirilmesini, ancak Yardım gereken yere bazı durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-242">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="46ea2-243">Anahtar özelliği ilişkisini gönderi ile ilgili bir sorun oluşturulan Blog sınıfında adının değiştirilmesi.</span><span class="sxs-lookup"><span data-stu-id="46ea2-243">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="46ea2-244">Veritabanı oluşturma, kod ilk Post sınıfı BlogId özelliğinde görür ve, bir sınıf adına ve "Id" olarak Blog sınıfı için yabancı anahtar değeriyle aynı olduğunu kural olarak tanır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-244">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="46ea2-245">Ancak blog sınıfında BlogId özellik yok.</span><span class="sxs-lookup"><span data-stu-id="46ea2-245">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="46ea2-246">Bu çözüm iletide bir gezinti özelliği oluşturun ve ilk iki sınıf arasında bir ilişki oluşturma işlemini anlama kod yardımcı olmak için yabancı DataAnnotation kullanmaktır — Post.BlogId özelliğini kullanarak — kısıtlamalarını belirleme yanı sıra Veritabanı.</span><span class="sxs-lookup"><span data-stu-id="46ea2-246">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

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

<span data-ttu-id="46ea2-247">Veritabanı kısıtlamasındaki InternalBlogs.PrimaryTrackingKey Posts.BlogId arasında bir ilişki gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-247">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![InternalBlogs.PrimaryTrackingKey Posts.BlogId arasındaki ilişki](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="46ea2-249">Sınıflar arasında birden çok ilişkilerine sahip InverseProperty kullanılır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-249">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="46ea2-250">Post sınıfta olan bir blog gönderisi yazıp izlemek isteyebilirsiniz düzenleyen kişiyi yanı sıra.</span><span class="sxs-lookup"><span data-stu-id="46ea2-250">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="46ea2-251">Post sınıfı için iki yeni gezinti özellikleri aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-251">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="46ea2-252">Bu özellik tarafından başvurulan kişi sınıfı eklemek gerekir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-252">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="46ea2-253">Kişi sınıfın tüm kişi ve tüm bu kişi tarafından güncelleştirilmiş gönderilerinin tarafından yazılan gönderiler için bir posta dön Gezinti özellikleri vardır.</span><span class="sxs-lookup"><span data-stu-id="46ea2-253">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="46ea2-254">Kod ilk iki sınıf kendi özelliklerinde eşleştirilecek mümkün değil.</span><span class="sxs-lookup"><span data-stu-id="46ea2-254">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="46ea2-255">Gönderiler için veritabanı tablosu oluşturan kişi, diğeri UpdatedBy kişinin bir yabancı anahtar olması gerekir, ancak kod ilk dört yabancı anahtar özelliklerini oluşturur: Kişi\_kimliği, kişi\_ıd1, oluşturan\_kimliği ve UpdatedBy\_kimliği.</span><span class="sxs-lookup"><span data-stu-id="46ea2-255">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![Tablo fazladan yabancı anahtarlar ile gönderir.](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="46ea2-257">Bu sorunları düzeltmek için InverseProperty ek açıklama özellikleri hizalaması belirtmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-257">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="46ea2-258">Bizzat PostsWritten özelliği bu Post türe başvurur bildiğinden Post.CreatedBy ilişkisi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="46ea2-258">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="46ea2-259">Benzer şekilde, PostsUpdated Post.UpdatedBy için bağlanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="46ea2-259">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="46ea2-260">Ve kod önce ek yabancı anahtarlar oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-260">And code first will not create the extra foreign keys.</span></span>

![Ek yabancı anahtarlar tablosuz gönderir](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="46ea2-262">Özet</span><span class="sxs-lookup"><span data-stu-id="46ea2-262">Summary</span></span>

<span data-ttu-id="46ea2-263">DataAnnotations yalnızca, istemci ve sunucu tarafı doğrulama kodu ilk sınıflarınızı açıklayan sağlar, ancak geliştirmek ve hatta kendi kurallarına dayalı sınıflarınızı hakkında kod ilk hale getirecek varsayımların düzeltmek de sağlar.</span><span class="sxs-lookup"><span data-stu-id="46ea2-263">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="46ea2-264">İle DataAnnotations, yalnızca veritabanı şeması oluşturma sürücü değil, ancak kod ilk sınıflarınızı önceden varolan bir veritabanına eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46ea2-264">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="46ea2-265">Bunlar çok esnek olmakla birlikte, yalnızca en yaygın olarak kod ilk sınıflarınızı üzerinde yaptığınız değişikliklerin gerekli DataAnnotations sağlayan aklınızda bulundurun.</span><span class="sxs-lookup"><span data-stu-id="46ea2-265">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="46ea2-266">Sınıflarınızı istisnai durumlara bazıları için yapılandırmak için diğer yapılandırma mekanizması, Code First's Fluent API'si görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="46ea2-266">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
