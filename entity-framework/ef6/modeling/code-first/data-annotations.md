---
title: İlk veri ek açıklamaları - EF6 kod
author: divega
ms.date: 2016-10-23
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 0ab66afa3babafe657b3ddb32c02c3fba0ae310e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994592"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="6914f-102">Kod ilk veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="6914f-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="6914f-103">**EF4.1 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 4.1 içinde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6914f-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="6914f-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="6914f-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="6914f-105">Bu sayfadaki içeriğin gelen uyarlanmış ve Julie Lerman tarafından başlangıçta yazılı makalesi (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="6914f-105">The content on this page is adapted from and article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="6914f-106">Entity Framework Code First EF sorgulama, gerçekleştirilecek dayanan modeli temsil etmek için kendi etki alanı sınıfları kullanmanıza olanak tanır değişiklik izleme ve işlevleri güncelleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="6914f-106">Entity Framework Code First allows you to use your own domain classes to represent the model which EF relies on to perform querying, change tracking and updating functions.</span></span> <span data-ttu-id="6914f-107">Kod ilk yapılandırma kuralı başvurulan bir programlama modeli kullanır.</span><span class="sxs-lookup"><span data-stu-id="6914f-107">Code first leverages a programming pattern referred to as convention over configuration.</span></span> <span data-ttu-id="6914f-108">Bu, kodu anlamı sınıflarınızı EF kullanan kurallarına uyduğundan ilk varsayar.</span><span class="sxs-lookup"><span data-stu-id="6914f-108">What this means is that code first will assume that your classes follow the conventions that EF uses.</span></span> <span data-ttu-id="6914f-109">Bu durumda, EF işini yapması ayrıntıları çalışmaya mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="6914f-109">In that case, EF will be able to work out the details it needs to do its job.</span></span> <span data-ttu-id="6914f-110">Ancak, sınıflarınızı bu kuralları uygulamazsanız sınıflarınızı EF, gerekli bilgileri sağlamak için yapılandırmaları ekleme olanağına sahip olursunuz.</span><span class="sxs-lookup"><span data-stu-id="6914f-110">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the information it needs.</span></span>

<span data-ttu-id="6914f-111">Kod öncelikle bu yapılandırmalar, sınıfa eklemek için iki yol sunar.</span><span class="sxs-lookup"><span data-stu-id="6914f-111">Code first gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="6914f-112">Bir basit öznitelikleri DataAnnotations adlı kullanıyor ve diğeri kod ilk kullanmaktır yapılandırmaları kesin, kodda açıklamak için bir yol sağlayan Fluent API'si.</span><span class="sxs-lookup"><span data-stu-id="6914f-112">One is using simple attributes called DataAnnotations and the other is using code first’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="6914f-113">Bu makalede, en sık gerekli yapılandırmaları vurgulama – sınıflarınızı yapılandırmak için (System.ComponentModel.DataAnnotations ad alanında) DataAnnotations üzerinden odaklanır.</span><span class="sxs-lookup"><span data-stu-id="6914f-113">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="6914f-114">DataAnnotations ayrıca .NET uygulamaları, bu uygulamalar, istemci tarafı doğrulama için aynı ek açıklamalar yararlanmasını sağlayan bir ASP.NET MVC gibi bir dizi tarafından anlaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="6914f-114">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="6914f-115">Model</span><span class="sxs-lookup"><span data-stu-id="6914f-115">The model</span></span>

<span data-ttu-id="6914f-116">Kazandırabileceğinizi göstereceğiz ilk DataAnnotations sınıfları için basit bir çift kod: Blog ve gönderi.</span><span class="sxs-lookup"><span data-stu-id="6914f-116">I’ll demonstrate code first DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

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

<span data-ttu-id="6914f-117">Oldukları gibi Blog ve gönderi sınıfları kolayca kod ilk kuralı izleyin ve hiçbir tweaks EF çalışmanıza yardımcı olmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6914f-117">As they are, the Blog and Post classes conveniently follow code first convention and required no tweaks to help EF work with them.</span></span> <span data-ttu-id="6914f-118">Ancak daha fazla bilgi için EF sınıfları ve bunların eşleneceğine veritabanı hakkında sağlamaya ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-118">But you can also use the annotations to provide more information to EF about the classes and the database that they map to.</span></span>

 

## <a name="key"></a><span data-ttu-id="6914f-119">Anahtar</span><span class="sxs-lookup"><span data-stu-id="6914f-119">Key</span></span>

<span data-ttu-id="6914f-120">Entity Framework varlıkları izlemek için kullandığı bir anahtar değere sahip her varlık kullanır.</span><span class="sxs-lookup"><span data-stu-id="6914f-120">Entity Framework relies on every entity having a key value that it uses for tracking entities.</span></span> <span data-ttu-id="6914f-121">Bağımlı ilk kod kuralları nasıl kod ilk sınıfların her birini anahtar özelliği gelir biridir.</span><span class="sxs-lookup"><span data-stu-id="6914f-121">One of the conventions that code first depends on is how it implies which property is the key in each of the code first classes.</span></span> <span data-ttu-id="6914f-122">Sınıf adı ve "Id" "BlogId" gibi bir araya getiren bir ya da "Id" adlı bir özellik aramak için bu kuralıdır.</span><span class="sxs-lookup"><span data-stu-id="6914f-122">That convention is to look for a property named “Id” or one that combines the class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="6914f-123">Özellik için bir birincil anahtar sütunu veritabanında eşler.</span><span class="sxs-lookup"><span data-stu-id="6914f-123">The property will map to a primary key column in the database.</span></span>

<span data-ttu-id="6914f-124">Blog ve gönderi sınıfları bu kural izleyin.</span><span class="sxs-lookup"><span data-stu-id="6914f-124">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="6914f-125">Ancak ne oldu?</span><span class="sxs-lookup"><span data-stu-id="6914f-125">But what if they didn’t?</span></span> <span data-ttu-id="6914f-126">Peki Blog kullanılan adı *PrimaryTrackingKey* bunun yerine veya hatta *foo*?</span><span class="sxs-lookup"><span data-stu-id="6914f-126">What if Blog used the name *PrimaryTrackingKey* instead or even *foo*?</span></span> <span data-ttu-id="6914f-127">Kod öncelikle bu kuralıyla eşleşen bir özellik bulamazsa bir anahtarı özelliği olması gerekir, Entity Framework'ün gereksinimi nedeniyle bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6914f-127">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="6914f-128">Anahtar ek açıklama EntityKey kullanılacak hangi özelliğinin olduğunu belirtmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-128">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

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

<span data-ttu-id="6914f-129">Eğer ilk kod kullanarak veritabanı oluşturma özelliği, Blog tablonun varsayılan olarak kimlik olarak da tanımlanır PrimaryTrackingKey adlı birincil anahtar sütunu gerekir.</span><span class="sxs-lookup"><span data-stu-id="6914f-129">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey which is also defined as Identity by default.</span></span>

![jj591583_figure01](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="6914f-131">Bileşik anahtarlar</span><span class="sxs-lookup"><span data-stu-id="6914f-131">Composite keys</span></span>

<span data-ttu-id="6914f-132">Entity Framework, bileşik anahtarlar - birden fazla özelliği içeren birincil anahtarları destekler.</span><span class="sxs-lookup"><span data-stu-id="6914f-132">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="6914f-133">Örneğin, birincil anahtarı PassportNumber ve IssuingCountry bir birleşimi olan bir Passport sınıfı olabilir.</span><span class="sxs-lookup"><span data-stu-id="6914f-133">For example, your could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

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

<span data-ttu-id="6914f-134">Deneyin ve yukarıdaki sınıfı EF modelinizde olsaydı belirten bir InvalidOperationExceptions elde edebileceğiniz;</span><span class="sxs-lookup"><span data-stu-id="6914f-134">If you were to try and use the above class in your EF model you wuld get an InvalidOperationExceptions stating;</span></span>

<span data-ttu-id="6914f-135">*Bileşik birincil anahtar türü için 'Passport' sıralama belirlenemiyor. ColumnAttribute veya HasKey yöntemi bileşik birincil anahtarlar için sipariş belirtmek için kullanın.*</span><span class="sxs-lookup"><span data-stu-id="6914f-135">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="6914f-136">Bileşik anahtarlar sahip olduğunuzda, Entity Framework anahtar özellikler bir sipariş tanımlamanızı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6914f-136">When you have composite keys, Entity Framework requires you to define an order of the key properties.</span></span> <span data-ttu-id="6914f-137">Sütun ek açıklama bir sıra belirtmek için kullanarak bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-137">You can do this using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="6914f-138">(Dizin temelinde güncellememek yerine) sıra değeri herhangi bir değer kullanılabilmesi için görelidir.</span><span class="sxs-lookup"><span data-stu-id="6914f-138">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="6914f-139">Örneğin, 100 ve 200 1 ve 2 yerine kabul edilebilir olacaktır.</span><span class="sxs-lookup"><span data-stu-id="6914f-139">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

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

<span data-ttu-id="6914f-140">Bileşik yabancı anahtarları varlık varsa karşılık gelen birincil anahtar özellikleri için kullanılan aynı sütun belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6914f-140">If you have entities with composite foreign keys then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="6914f-141">Yalnızca göreli yabancı anahtar özellikleri içinde sıralamasını aynı, atanan değerleri tam olması gerekir **sipariş** eşleşmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="6914f-141">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="6914f-142">Örneğin, aşağıdaki sınıfında, 3 ve 4 1 ve 2 yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6914f-142">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

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

## <a name="required"></a><span data-ttu-id="6914f-143">Gerekli</span><span class="sxs-lookup"><span data-stu-id="6914f-143">Required</span></span>

<span data-ttu-id="6914f-144">Gerekli ek açıklama EF belirli bir özellik gereklidir bildirir.</span><span class="sxs-lookup"><span data-stu-id="6914f-144">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="6914f-145">Başlık özelliği için gerekli ekleme özelliği veri içerdiğinden emin olmak için EF (ve MVC) zorlar.</span><span class="sxs-lookup"><span data-stu-id="6914f-145">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="6914f-146">Hiçbir ek olmadan uygulamada başladığınız kodun veya değişiklikler, bir MVC uygulaması dinamik olarak bile özellik ve ek açıklama adları kullanarak bir ileti oluşturma, istemci tarafı doğrulama gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="6914f-146">With no additional no code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![jj591583_figure02](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="6914f-148">Gerekli öznitelik eşlenen özelliği null olamaz yapmanızı tarafından oluşturulan veritabanı da etkiler.</span><span class="sxs-lookup"><span data-stu-id="6914f-148">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="6914f-149">Başlık alanı "için null olmayan" olarak değiştiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6914f-149">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="6914f-150">Bazı durumlarda veritabanını özelliği gerekli olsa bile null yapılamaz sütunda mümkün olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="6914f-150">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="6914f-151">Örneğin, ne zaman TPH devralma stratejisi veri için birden fazla türü kullanılarak tek bir tabloda depolanır.</span><span class="sxs-lookup"><span data-stu-id="6914f-151">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="6914f-152">Gerekli bir özellik türetilmiş bir tür içeriyorsa, bu özellik hiyerarşideki tüm türleri olduğundan sütun atanamayan yapılamaz.</span><span class="sxs-lookup"><span data-stu-id="6914f-152">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![jj591583_figure03](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="6914f-154">MaxLength ve MinLength</span><span class="sxs-lookup"><span data-stu-id="6914f-154">MaxLength and MinLength</span></span>

<span data-ttu-id="6914f-155">Yalnızca gerekli olduğu gibi MaxLength ve MinLength öznitelikleri ek özellik doğrulamaları belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6914f-155">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="6914f-156">BloggerName uzunluğu gereksinimleri aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="6914f-156">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="6914f-157">Örnek ayrıca nasıl birleştirileceğini öznitelikleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="6914f-157">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="6914f-158">MaxLength ek açıklaması özelliğinin uzunluğu 10'a ayarlayarak veritabanı etkiler.</span><span class="sxs-lookup"><span data-stu-id="6914f-158">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![jj591583_figure04](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="6914f-160">MVC istemci-tarafı ek açıklama ve EF 4.1 sunucu tarafı ek açıklama hem de dikkate bir hata iletisi dinamik olarak yeniden oluşturma, bu doğrulama: "alanı BloggerName '10' en fazla uzunluğu bir dize veya dizi türü olmalıdır." Bu iletiyi biraz daha uzun.</span><span class="sxs-lookup"><span data-stu-id="6914f-160">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="6914f-161">Birçok ek açıklamaları ErrorMessage özniteliğiyle bir hata iletisi belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6914f-161">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="6914f-162">Ayrıca, gerekli ek açıklamada ErrorMessage belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-162">You can also specify ErrorMessage in the Required annotation.</span></span>

![jj591583_figure05](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="6914f-164">NotMapped</span><span class="sxs-lookup"><span data-stu-id="6914f-164">NotMapped</span></span>

<span data-ttu-id="6914f-165">Kod ilk kuralı, veritabanında bir desteklenen veri türü her bir özellik temsil edildiğini belirler.</span><span class="sxs-lookup"><span data-stu-id="6914f-165">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="6914f-166">Ancak bu her zaman uygulamalarınızı durumda değildir.</span><span class="sxs-lookup"><span data-stu-id="6914f-166">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="6914f-167">Örneğin başlık ve BloggerName alanlara göre bir kod oluşturur Blog sınıftaki bir özelliği olabilir.</span><span class="sxs-lookup"><span data-stu-id="6914f-167">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="6914f-168">Bu özellik, dinamik olarak oluşturulabilir ve depolanması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="6914f-168">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="6914f-169">Bu BlogCode özelliği gibi NotMapped ek açıklama ile veritabanına eşlemeyin herhangi bir özelliği işaretleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-169">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

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

 

## <a name="complextype"></a><span data-ttu-id="6914f-170">ComplexType</span><span class="sxs-lookup"><span data-stu-id="6914f-170">ComplexType</span></span>

<span data-ttu-id="6914f-171">Etki alanı varlıklarınızı sınıf kümesi açıklamak ve eksiksiz bir varlık tanımlamak için bu sınıflardan sonra katman durumdur.</span><span class="sxs-lookup"><span data-stu-id="6914f-171">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="6914f-172">Örneğin, modelinize BlogDetails adlı bir sınıf ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="6914f-172">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="6914f-173">Anahtar özelliği herhangi bir türde BlogDetails yok dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6914f-173">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="6914f-174">Etki alanı Odaklı Tasarım içinde BlogDetails bir değer nesnesi olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="6914f-174">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="6914f-175">Varlık çerçevesi karmaşık türler değer nesnelere başvurur.</span><span class="sxs-lookup"><span data-stu-id="6914f-175">Entity Framework refers to value objects as complex types.</span></span>  <span data-ttu-id="6914f-176">Karmaşık türler, kendi izlenemez.</span><span class="sxs-lookup"><span data-stu-id="6914f-176">Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="6914f-177">Ancak, izleniyor Blog nesnesinin bir parçası BlogDetails Blog sınıf özelliği olarak.</span><span class="sxs-lookup"><span data-stu-id="6914f-177">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="6914f-178">Bu ilk tanımak kod için sırada BlogDetails sınıfı bir ComplexType işaretlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6914f-178">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="6914f-179">Artık bu blog BlogDetails temsil etmek için Blog sınıftaki bir özelliği ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-179">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="6914f-180">Veritabanında Blog tablo adını BlogDetail özelliğinde yer alan özellikler dahil olmak üzere blog özelliklerin tümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="6914f-180">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="6914f-181">Varsayılan olarak, her biri ile BlogDetail karmaşık türün adı gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="6914f-181">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![jj591583_figure06](~/ef6/media/jj591583-figure06.png)

<span data-ttu-id="6914f-183">Başka bir ilgi çekici Not Notes özelliği NULL olmayan bir DateTime sınıfında olarak tanımlandı ancak ilgili veritabanı alanını boş değer atanabilir olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="6914f-183">Another interesting note is that although the DateCreated property was defined as a non-nullable DateTime in the class, the relevant database field is nullable.</span></span> <span data-ttu-id="6914f-184">Veritabanı şeması etkilemek istiyorsanız, gerekli ek açıklama kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6914f-184">You must use the Required annotation if you wish to affect the database schema.</span></span>

 

## <a name="concurrencycheck"></a><span data-ttu-id="6914f-185">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="6914f-185">ConcurrencyCheck</span></span>

<span data-ttu-id="6914f-186">ConcurrencyCheck ek açıklama, veritabanında bir kullanıcı, düzenler veya bir varlığı silen denetimi eşzamanlılık için kullanılacak bir veya daha fazla özellik bayrağını olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="6914f-186">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="6914f-187">EF Designer ile aşinaysanız, sabit bir özelliğin ConcurrencyMode ayarı ile hizalar.</span><span class="sxs-lookup"><span data-stu-id="6914f-187">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="6914f-188">BloggerName özelliğini ekleyerek ConcurrencyCheck nasıl çalıştığını görelim.</span><span class="sxs-lookup"><span data-stu-id="6914f-188">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="6914f-189">SaveChanges çağrıldığında ConcurrencyCheck üzerindeki ek açıklama BloggerName alanı nedeniyle güncelleştirme bu özellik, özgün değer kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6914f-189">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="6914f-190">Komut doğru satır yalnızca anahtar değeri aynı zamanda BloggerName özgün değeri filtreleyerek bulmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="6914f-190">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span>  <span data-ttu-id="6914f-191">Komutu bir PrimaryTrackingKey sahip olan satırı güncelleştirin görebileceğiniz veritabanına gönderilen güncelleştirme komut kritik bölümleri, 1 ve "Bu blog veritabanından alınırken, özgün değer Julie", bir BloggerName şunlardır.</span><span class="sxs-lookup"><span data-stu-id="6914f-191">Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="6914f-192">Birisi bu blog blogger adı sırada değişti, bu güncelleştirme başarısız olur ve işlemek için gereken bir DbUpdateConcurrencyException elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-192">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="6914f-193">Zaman damgası</span><span class="sxs-lookup"><span data-stu-id="6914f-193">TimeStamp</span></span>

<span data-ttu-id="6914f-194">Eşzamanlılık denetimi için rowversion veya zaman damgası alanı kullanımı daha yaygındır.</span><span class="sxs-lookup"><span data-stu-id="6914f-194">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="6914f-195">Ancak bayt dizisi özellik türü olduğu sürece ConcurrencyCheck ek açıklama kullanmak yerine daha belirli zaman damgası ek açıklama kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-195">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="6914f-196">Kod ilk zaman damgası özellikleri aynı ConcurrencyCheck özellikleri olarak değerlendirir, ancak aynı zamanda kod oluşturan veritabanı alanını atanamaz olduğundan emin olmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="6914f-196">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="6914f-197">Bu gibi durumlarda, bir zaman damgası özelliği yalnızca belirli bir sınıf içinde olabilir.</span><span class="sxs-lookup"><span data-stu-id="6914f-197">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="6914f-198">Aşağıdaki özellikler, Blog sınıfına ekleme:</span><span class="sxs-lookup"><span data-stu-id="6914f-198">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="6914f-199">ilk veritabanı tablosu, bir NULL olmayan bir zaman damgası sütunu oluşturma kod sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="6914f-199">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![jj591583_figure07](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="6914f-201">Tablo ve sütun</span><span class="sxs-lookup"><span data-stu-id="6914f-201">Table and Column</span></span>

<span data-ttu-id="6914f-202">Code First veritabanı oluşturma izni vermiş olursunuz, tabloları ve sütunları sanal makinesi adını değiştirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-202">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="6914f-203">Code First ile varolan bir veritabanını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-203">You can also use Code First with an existing database.</span></span> <span data-ttu-id="6914f-204">Ancak, her zaman sınıfları ve özellikleri, etki alanınızdaki adları tabloları ve sütunları veritabanınızdaki adlarının eşleştiğini durumda değil.</span><span class="sxs-lookup"><span data-stu-id="6914f-204">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="6914f-205">My sınıfı Blog olarak adlandırılır ve bu blogları adlı bir tabloya eşler ilk kural olarak, kod varsayar.</span><span class="sxs-lookup"><span data-stu-id="6914f-205">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="6914f-206">Bu durumda değilse tablo özniteliğiyle tablonun adını belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-206">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="6914f-207">Burada Örneğin, ek açıklama tablo adını InternalBlogs olduğunu belirler.</span><span class="sxs-lookup"><span data-stu-id="6914f-207">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="6914f-208">Sütun ek açıklama eşlenen sütun özniteliklerini belirtilirken bir daha fazla yatkın olduğu.</span><span class="sxs-lookup"><span data-stu-id="6914f-208">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="6914f-209">Bir ad, veri türü veya hatta bir sütun tabloda göründüğü sırayı koşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-209">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="6914f-210">Sütun özniteliği örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="6914f-210">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="6914f-211">Sütunun TypeName öznitelik veri türü DataAnnotation ile karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="6914f-211">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="6914f-212">Veri türü, kullanıcı Arabiriminde kullanılan ek açıklamanın olup ve Code First tarafından göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="6914f-212">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="6914f-213">Yeniden oluşturulduğunda sonra tablosu aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="6914f-213">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="6914f-214">Tablo adı için InternalBlogs değişti ve karmaşık tür tanımı sütundan BlogDescription sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6914f-214">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="6914f-215">Ek açıklamada ad belirtilmediğinden, kod sütun adı karmaşık tür adı ile başlangıç kuralı ilk kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="6914f-215">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![jj591583_figure08](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="6914f-217">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="6914f-217">DatabaseGenerated</span></span>

<span data-ttu-id="6914f-218">Hesaplanan özellikler olanağı önemli veritabanı özellikleri var.</span><span class="sxs-lookup"><span data-stu-id="6914f-218">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="6914f-219">Code First sınıflarınızı eşleniyorsa, hesaplanan sütunlar içeren tablolar, varlık Çerçevesi'bu sütunları güncelleştirmeye çalışmak istemediğiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-219">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="6914f-220">Ancak, veritabanından eklenmiş veya güncelleştirilmiş verileri sonra bu değerleri döndürülecek EF istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="6914f-220">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="6914f-221">Sınıfınızda, hesaplanan enum yanı sıra özelliklere işaretleyemedi DatabaseGenerated ek açıklama kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-221">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="6914f-222">Diğer numaralandırmalar yok ve kimlik.</span><span class="sxs-lookup"><span data-stu-id="6914f-222">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="6914f-223">Kod ilk veritabanı oluşturulurken bayt veya zaman damgası sütuna oluşturulan veritabanı kullanabilir, aksi durumda yalnızca bu kod önce hesaplanan sütun için formülün belirlemek mümkün olmayacağından, mevcut veritabanlarının işaret ettiğinde kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6914f-223">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="6914f-224">Varsayılan olarak, okuma, tamsayı olan bir anahtar özellik veritabanındaki bir kimlik anahtarı olur.</span><span class="sxs-lookup"><span data-stu-id="6914f-224">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="6914f-225">DatabaseGenerated DatabaseGenerationOption.Identity için ayarı ile aynı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="6914f-225">That would be the same as setting DatabaseGenerated to DatabaseGenerationOption.Identity.</span></span> <span data-ttu-id="6914f-226">Bir kimlik anahtarı olmasını istemiyorsanız DatabaseGenerationOption.None için değeri ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-226">If you do not want it to be an identity key, you can set the value to DatabaseGenerationOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="6914f-227">Dizin</span><span class="sxs-lookup"><span data-stu-id="6914f-227">Index</span></span>

> [!NOTE]
> <span data-ttu-id="6914f-228">**EF6.1 ve sonraki sürümler yalnızca** -Entity Framework 6.1 içinde dizin özniteliği tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="6914f-228">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="6914f-229">Önceki bir sürümü kullanıyorsanız, bu bölümdeki bilgiler, geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="6914f-229">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="6914f-230">Bir veya daha fazla sütuna kullanarak bir dizin oluşturabilirsiniz **IndexAttribute**.</span><span class="sxs-lookup"><span data-stu-id="6914f-230">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="6914f-231">Öznitelik için bir veya daha fazla özellik ekleyerek EF veritabanında oluşturduğunda, veritabanında ilgili dizini oluşturmak neden veya karşılık gelen iskelesini **CreateIndex** Code First Migrations'ı kullanıyorsanız çağırır.</span><span class="sxs-lookup"><span data-stu-id="6914f-231">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="6914f-232">Örneğin, aşağıdaki kod üzerinde oluşturulan bir dizin sonuçlanır **derecelendirme** sütununun **gönderileri** veritabanındaki tablo.</span><span class="sxs-lookup"><span data-stu-id="6914f-232">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

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

<span data-ttu-id="6914f-233">Varsayılan olarak, dizin adlandırılacağını **IX\_&lt;özellik adı&gt;**  (IX\_yukarıdaki örnekte derecelendirme).</span><span class="sxs-lookup"><span data-stu-id="6914f-233">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="6914f-234">Dizini için bir ad olsa da belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-234">You can also specify a name for the index though.</span></span> <span data-ttu-id="6914f-235">Aşağıdaki örnek dizinin adlandırılmalıdır belirtir **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="6914f-235">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="6914f-236">Varsayılan olarak, dizinleri benzersiz olmayan, ancak kullanabileceğiniz **IsUnique** adlı bir dizin benzersiz olması gerektiğini belirtmek için parametre.</span><span class="sxs-lookup"><span data-stu-id="6914f-236">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="6914f-237">Aşağıdaki örnekte benzersiz bir dizin üzerinde tanıtır. bir **kullanıcı**ait oturum açma adı.</span><span class="sxs-lookup"><span data-stu-id="6914f-237">The following example introduces a unique index on a **User**'s login name.</span></span>

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

### <a name="multiple-column-indexes"></a><span data-ttu-id="6914f-238">Birden çok sütun dizinleri</span><span class="sxs-lookup"><span data-stu-id="6914f-238">Multiple-Column Indexes</span></span>

<span data-ttu-id="6914f-239">Birden fazla sütuna yayılmış dizinler için belirli bir tabloda birden fazla dizin ek açıklamalar aynı adı kullanarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="6914f-239">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="6914f-240">Çok sütunlu dizinleri oluşturduğunuzda, sipariş sütunlar için dizin belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6914f-240">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="6914f-241">Örneğin, aşağıdaki kod üzerinde çok sütunlu bir dizin oluşturur **derecelendirme** ve **BlogId** adlı **IX\_BlogAndRating**.</span><span class="sxs-lookup"><span data-stu-id="6914f-241">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogAndRating**.</span></span> <span data-ttu-id="6914f-242">**BlogId** dizini ilk sütunda ve **derecelendirme** saniyedir.</span><span class="sxs-lookup"><span data-stu-id="6914f-242">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="6914f-243">İlişki öznitelikleri: InverseProperty ve ForeignKey</span><span class="sxs-lookup"><span data-stu-id="6914f-243">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="6914f-244">Bu sayfa, Code First modelinizde veri ek açıklamalarını kullanma ilişkileri ayarlama hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6914f-244">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="6914f-245">EF ve erişmek ve ilişkileri kullanarak verileri işlemek nasıl ilişkiler hakkında genel bilgi için bkz. [ilişkileri ve gezinti özellikleri](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="6914f-245">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="6914f-246">Kod ilk kuralı modelinizdeki en yaygın ilişki ölçeklendirilmesini, ancak Yardım gereken yere bazı durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="6914f-246">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="6914f-247">Anahtar özelliği ilişkisini gönderi ile ilgili bir sorun oluşturulan Blog sınıfında adının değiştirilmesi.</span><span class="sxs-lookup"><span data-stu-id="6914f-247">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="6914f-248">Veritabanı oluşturma, kod ilk Post sınıfı BlogId özelliğinde görür ve, bir sınıf adına ve "Id" olarak Blog sınıfı için yabancı anahtar değeriyle aynı olduğunu kural olarak tanır.</span><span class="sxs-lookup"><span data-stu-id="6914f-248">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="6914f-249">Ancak blog sınıfında BlogId özellik yok.</span><span class="sxs-lookup"><span data-stu-id="6914f-249">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="6914f-250">Bu çözüm iletide bir gezinti özelliği oluşturun ve ilk iki sınıf arasında bir ilişki oluşturma işlemini anlama kod yardımcı olmak için yabancı DataAnnotation kullanmaktır — Post.BlogId özelliğini kullanarak — kısıtlamalarını belirleme yanı sıra Veritabanı.</span><span class="sxs-lookup"><span data-stu-id="6914f-250">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

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

<span data-ttu-id="6914f-251">Veritabanı kısıtlamasındaki InternalBlogs.PrimaryTrackingKey Posts.BlogId arasında bir ilişki gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6914f-251">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![jj591583_figure09](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="6914f-253">Sınıflar arasında birden çok ilişkilerine sahip InverseProperty kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6914f-253">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="6914f-254">Post sınıfta olan bir blog gönderisi yazıp izlemek isteyebilirsiniz düzenleyen kişiyi yanı sıra.</span><span class="sxs-lookup"><span data-stu-id="6914f-254">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="6914f-255">Post sınıfı için iki yeni gezinti özellikleri aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="6914f-255">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="6914f-256">Bu özellik tarafından başvurulan kişi sınıfı eklemek gerekir.</span><span class="sxs-lookup"><span data-stu-id="6914f-256">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="6914f-257">Kişi sınıfın tüm kişi ve tüm bu kişi tarafından güncelleştirilmiş gönderilerinin tarafından yazılan gönderiler için bir posta dön Gezinti özellikleri vardır.</span><span class="sxs-lookup"><span data-stu-id="6914f-257">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="6914f-258">Kod ilk iki sınıf kendi özelliklerinde eşleştirilecek mümkün değil.</span><span class="sxs-lookup"><span data-stu-id="6914f-258">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="6914f-259">Gönderiler için veritabanı tablosu oluşturan kişi için bir yabancı anahtar olmalıdır, biri UpdatedBy kişi ancak kod ilk oluşturacaktır dört yabancı anahtar özelliklerini: kişi\_kimliği, kişi\_ıd1, oluşturan\_kimliği ve UpdatedBy\_kimliği.</span><span class="sxs-lookup"><span data-stu-id="6914f-259">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four will foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![jj591583_figure10](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="6914f-261">Bu sorunları düzeltmek için InverseProperty ek açıklama özellikleri hizalaması belirtmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-261">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="6914f-262">Bizzat PostsWritten özelliği bu Post türe başvurur bildiğinden Post.CreatedBy ilişkisi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6914f-262">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="6914f-263">Benzer şekilde, PostsUpdated Post.UpdatedBy için bağlanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="6914f-263">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="6914f-264">Ve kod önce ek yabancı anahtarlar oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="6914f-264">And code first will not create the extra foreign keys.</span></span>

![jj591583_figure11](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="6914f-266">Özet</span><span class="sxs-lookup"><span data-stu-id="6914f-266">Summary</span></span>

<span data-ttu-id="6914f-267">DataAnnotations yalnızca, istemci ve sunucu tarafı doğrulama kodu ilk sınıflarınızı açıklayan sağlar, ancak geliştirmek ve hatta kendi kurallarına dayalı sınıflarınızı hakkında kod ilk hale getirecek varsayımların düzeltmek de sağlar.</span><span class="sxs-lookup"><span data-stu-id="6914f-267">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="6914f-268">İle DataAnnotations, yalnızca veritabanı şeması oluşturma sürücü değil, ancak kod ilk sınıflarınızı önceden varolan bir veritabanına eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6914f-268">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="6914f-269">Bunlar çok esnek olmakla birlikte, yalnızca en yaygın olarak kod ilk sınıflarınızı üzerinde yaptığınız değişikliklerin gerekli DataAnnotations sağlayan aklınızda bulundurun.</span><span class="sxs-lookup"><span data-stu-id="6914f-269">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="6914f-270">Sınıflarınızı istisnai durumlara bazıları için yapılandırmak için diğer yapılandırma mekanizması, Code First's Fluent API'si görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="6914f-270">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
