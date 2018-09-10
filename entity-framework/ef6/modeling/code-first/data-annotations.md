---
title: İlk veri ek açıklamaları - EF6 kod
author: divega
ms.date: 2016-10-23
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 57e2b988f81d9c82e10a07a5cd4f3a1decfd838a
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251212"
---
# <a name="code-first-data-annotations"></a>Kod ilk veri ek açıklamaları
> [!NOTE]
> **EF4.1 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 4.1 içinde kullanıma sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bu bilgilerin geçerli değildir.

Bu sayfadaki içeriğin başlangıçta Julie Lerman tarafından yazılmış bir makaledeki uyarlanmış (\<http://thedatafarm.com>).

Entity Framework Code First EF sorgulama, gerçekleştirilecek dayanan modeli temsil etmek için kendi etki alanı sınıfları kullanmanıza olanak tanır değişiklik izleme ve işlevleri güncelleştiriliyor. Kod 'kuralı yapılandırmanız üzerinde.' olarak adlandırılan bir programlama modeli ilk yararlanır. Kod ilk sınıflarınızı Entity Framework kurallarını izleyin ve bu durumda, öğrenmek, işi gerçekleştirmek için otomatik olarak çalışacak varsayar. Ancak, sınıflarınızı bu kuralları uygulamazsanız sınıflarınızı EF önkoşul bilgilerini sağlamak için yapılandırmaları ekleme olanağına sahip olursunuz.

Kod öncelikle bu yapılandırmalar, sınıfa eklemek için iki yol sunar. Bir basit öznitelikleri DataAnnotations adlı kullanıyor ve ikinci Code First's Fluent yapılandırmaları kesin, kodda açıklamak için bir yol sağlayan API kullanıyor.

Bu makalede, en sık gerekli yapılandırmaları vurgulama – sınıflarınızı yapılandırmak için (System.ComponentModel.DataAnnotations ad alanında) DataAnnotations üzerinden odaklanır. DataAnnotations ayrıca .NET uygulamaları, bu uygulamalar, istemci tarafı doğrulama için aynı ek açıklamalar yararlanmasını sağlayan bir ASP.NET MVC gibi bir dizi tarafından anlaşılabilir.


## <a name="the-model"></a>Model

Kod ilk DataAnnotations sınıfları basit çiftiyle kazandırabileceğinizi göstereceğiz: Blog ve gönderi.

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

Olduğu gibi Blog ve gönderi sınıfları rahatça kod ilk kuralını izler ve EF uyumluluk etkinleştirmek için hiçbir tweaks gerektirir. Ancak, sınıfları ve bunların eşleneceğine veritabanı hakkında daha fazla bilgi için EF sağlamak için ek açıklamaları kullanabilirsiniz.

 

## <a name="key"></a>Anahtar

Entity Framework varlık izleme için kullanılan bir anahtar değere sahip her varlık kullanır. Bir Code First, örtük anahtar özellikleri kuraldır; Kod, önce "Id" veya sınıf adı ve "Id" gibi "BlogId" adlı bir özellik arar. Bu özellik için bir birincil anahtar sütunu veritabanında eşler.

Blog ve gönderi sınıfları bu kural izleyin. Bunlar ne oldu? Peki Blog kullanılan adı *PrimaryTrackingKey* bunun yerine veya hatta *foo*? Kod öncelikle bu kuralıyla eşleşen bir özellik bulamazsa bir anahtarı özelliği olması gerekir, Entity Framework'ün gereksinimi nedeniyle bir özel durum oluşturur. Anahtar ek açıklama EntityKey kullanılacak hangi özelliğinin olduğunu belirtmek için kullanabilirsiniz.

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

Eğer ilk kod kullanarak veritabanı oluşturma özelliği, Blog tablonun varsayılan olarak kimlik olarak da tanımlanır PrimaryTrackingKey adlı birincil anahtar sütunu gerekir.

![Blog tabloda birincil anahtar ile](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a>Bileşik anahtarlar

Entity Framework, bileşik anahtarlar - birden fazla özelliği içeren birincil anahtarları destekler. Örneğin, birincil anahtarı PassportNumber ve IssuingCountry bir birleşimi olan bir Passport sınıfı olabilir.

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

Yukarıdaki sınıfı EF modelinizde kullanılmaya çalışılıyor sonuçlanır bir `InvalidOperationException`:

*Bileşik birincil anahtar türü için 'Passport' sıralama belirlenemiyor. ColumnAttribute veya HasKey yöntemi bileşik birincil anahtarlar için sipariş belirtmek için kullanın.*

Bileşik anahtarlar kullanmak için Entity Framework anahtar özellikleri için bir sipariş tanımlamanızı gerektirir. Bir sıra belirtmek için sütunu ek açıklama kullanarak bunu yapabilirsiniz.

>[!NOTE]
> (Dizin temelinde güncellememek yerine) sıra değeri herhangi bir değer kullanılabilmesi için görelidir. Örneğin, 100 ve 200 1 ve 2 yerine kabul edilebilir olacaktır.

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

Bileşik yabancı anahtarları varlık varsa, karşılık gelen birincil anahtar özellikleri için kullanılan aynı sütun belirtmeniz gerekir.

Yalnızca göreli yabancı anahtar özellikleri içinde sıralamasını aynı, atanan değerleri tam olması gerekir **sipariş** eşleşmesi gerekmez. Örneğin, aşağıdaki sınıfında, 3 ve 4 1 ve 2 yerine kullanılabilir.

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

## <a name="required"></a>Gerekli

Gerekli ek açıklama EF belirli bir özellik gereklidir bildirir.

Başlık özelliği için gerekli ekleme özelliği veri içerdiğinden emin olmak için EF (ve MVC) zorlar.

``` csharp
    [Required]
    public string Title { get; set; }
```

Hiçbir ek olmadan uygulamada başladığınız kodun veya değişiklikler, bir MVC uygulaması dinamik olarak bile özellik ve ek açıklama adları kullanarak bir ileti oluşturma, istemci tarafı doğrulama gerçekleştirir.

![Oluşturma sayfasıdır başlıklı gerekli hata](~/ef6/media/jj591583-figure02.png)

Gerekli öznitelik eşlenen özelliği null olamaz yapmanızı tarafından oluşturulan veritabanı da etkiler. Başlık alanı "için null olmayan" olarak değiştiğine dikkat edin.

>[!NOTE]
> Bazı durumlarda veritabanını özelliği gerekli olsa bile null yapılamaz sütunda mümkün olmayabilir. Örneğin, ne zaman TPH devralma stratejisi veri için birden fazla türü kullanılarak tek bir tabloda depolanır. Gerekli bir özellik türetilmiş bir tür içeriyorsa, bu özellik hiyerarşideki tüm türleri olduğundan sütun atanamayan yapılamaz.

 

![Bloglar tablo](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a>MaxLength ve MinLength

Yalnızca gerekli olduğu gibi MaxLength ve MinLength öznitelikleri ek özellik doğrulamaları belirtmenizi sağlar.

BloggerName uzunluğu gereksinimleri aşağıda verilmiştir. Örnek ayrıca nasıl birleştirileceğini öznitelikleri gösterir.

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

MaxLength ek açıklaması özelliğinin uzunluğu 10'a ayarlayarak veritabanı etkiler.

![En büyük uzunluk BloggerName sütunu gösteren blogları tablo](~/ef6/media/jj591583-figure04.png)

MVC istemci-tarafı ek açıklama ve EF 4.1 sunucu tarafı ek açıklama hem de dikkate bir hata iletisi dinamik olarak yeniden oluşturma, bu doğrulama: "alanı BloggerName '10' en fazla uzunluğu bir dize veya dizi türü olmalıdır." Bu iletiyi biraz daha uzun. Birçok ek açıklamaları ErrorMessage özniteliğiyle bir hata iletisi belirtmenizi sağlar.

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Ayrıca, gerekli ek açıklamada ErrorMessage belirtebilirsiniz.

![Özel hata iletisiyle sayfası oluşturma](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a>NotMapped

Kod ilk kuralı, veritabanında bir desteklenen veri türü her bir özellik temsil edildiğini belirler. Ancak bu her zaman uygulamalarınızı durumda değildir. Örneğin başlık ve BloggerName alanlara göre bir kod oluşturur Blog sınıftaki bir özelliği olabilir. Bu özellik, dinamik olarak oluşturulabilir ve depolanması gerekmez. Bu BlogCode özelliği gibi NotMapped ek açıklama ile veritabanına eşlemeyin herhangi bir özelliği işaretleyebilirsiniz.

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

 

## <a name="complextype"></a>ComplexType

Etki alanı varlıklarınızı sınıf kümesi açıklamak ve eksiksiz bir varlık tanımlamak için bu sınıflardan sonra katman durumdur. Örneğin, modelinize BlogDetails adlı bir sınıf ekleyebilir.

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Anahtar özelliği herhangi bir türde BlogDetails yok dikkat edin. Etki alanı Odaklı Tasarım içinde BlogDetails bir değer nesnesi olarak adlandırılır. Varlık çerçevesi karmaşık türler değer nesnelere başvurur.  Karmaşık türler, kendi izlenemez.

Ancak, izleniyor Blog nesnesinin bir parçası BlogDetails Blog sınıf özelliği olarak. Bu ilk tanımak kod için sırada BlogDetails sınıfı bir ComplexType işaretlemeniz gerekir.

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Artık bu blog BlogDetails temsil etmek için Blog sınıftaki bir özelliği ekleyebilirsiniz.

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

Veritabanında Blog tablo adını BlogDetail özelliğinde yer alan özellikler dahil olmak üzere blog özelliklerin tümünü içerir. Varsayılan olarak, her biri ile BlogDetail karmaşık türün adı gelmelidir.

![Karmaşık tür tabloyla blogu](~/ef6/media/jj591583-figure06.png)

Başka bir ilgi çekici Not Notes özelliği NULL olmayan bir DateTime sınıfında olarak tanımlandı ancak ilgili veritabanı alanını boş değer atanabilir olmasıdır. Veritabanı şeması etkilemek istiyorsanız, gerekli ek açıklama kullanmanız gerekir.

 

## <a name="concurrencycheck"></a>ConcurrencyCheck

ConcurrencyCheck ek açıklama, veritabanında bir kullanıcı, düzenler veya bir varlığı silen denetimi eşzamanlılık için kullanılacak bir veya daha fazla özellik bayrağını olanak tanır. EF Designer ile aşinaysanız, sabit bir özelliğin ConcurrencyMode ayarı ile hizalar.

BloggerName özelliğini ekleyerek ConcurrencyCheck nasıl çalıştığını görelim.

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

SaveChanges çağrıldığında ConcurrencyCheck üzerindeki ek açıklama BloggerName alanı nedeniyle güncelleştirme bu özellik, özgün değer kullanılır. Komut doğru satır yalnızca anahtar değeri aynı zamanda BloggerName özgün değeri filtreleyerek bulmaya çalışır.  Komutu bir PrimaryTrackingKey sahip olan satırı güncelleştirin görebileceğiniz veritabanına gönderilen güncelleştirme komut kritik bölümleri, 1 ve "Bu blog veritabanından alınırken, özgün değer Julie", bir BloggerName şunlardır.

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

Birisi bu blog blogger adı sırada değişti, bu güncelleştirme başarısız olur ve işlemek için gereken bir DbUpdateConcurrencyException elde edersiniz.

 

## <a name="timestamp"></a>Zaman damgası

Eşzamanlılık denetimi için rowversion veya zaman damgası alanı kullanımı daha yaygındır. Ancak bayt dizisi özellik türü olduğu sürece ConcurrencyCheck ek açıklama kullanmak yerine daha belirli zaman damgası ek açıklama kullanabilirsiniz. Kod ilk zaman damgası özellikleri aynı ConcurrencyCheck özellikleri olarak değerlendirir, ancak aynı zamanda kod oluşturan veritabanı alanını atanamaz olduğundan emin olmanızı sağlar. Bu gibi durumlarda, bir zaman damgası özelliği yalnızca belirli bir sınıf içinde olabilir.

Aşağıdaki özellikler, Blog sınıfına ekleme:

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

ilk veritabanı tablosu, bir NULL olmayan bir zaman damgası sütunu oluşturma kod sonuçlanır.

![Zaman damgası sütunu tabloyla blogları](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a>Tablo ve sütun

Code First veritabanı oluşturma izni vermiş olursunuz, tabloları ve sütunları sanal makinesi adını değiştirmek isteyebilirsiniz. Code First ile varolan bir veritabanını kullanabilirsiniz. Ancak, her zaman sınıfları ve özellikleri, etki alanınızdaki adları tabloları ve sütunları veritabanınızdaki adlarının eşleştiğini durumda değil.

My sınıfı Blog olarak adlandırılır ve bu blogları adlı bir tabloya eşler ilk kural olarak, kod varsayar. Bu durumda değilse tablo özniteliğiyle tablonun adını belirtebilirsiniz. Burada Örneğin, ek açıklama tablo adını InternalBlogs olduğunu belirler.

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

Sütun ek açıklama eşlenen sütun özniteliklerini belirtilirken bir daha fazla yatkın olduğu. Bir ad, veri türü veya hatta bir sütun tabloda göründüğü sırayı koşabilirsiniz. Sütun özniteliği örneği aşağıda verilmiştir.

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

Sütunun TypeName öznitelik veri türü DataAnnotation ile karıştırmayın. Veri türü, kullanıcı Arabiriminde kullanılan ek açıklamanın olup ve Code First tarafından göz ardı edilir.

Yeniden oluşturulduğunda sonra tablosu aşağıdadır. Tablo adı için InternalBlogs değişti ve karmaşık tür tanımı sütundan BlogDescription sunulmuştur. Ek açıklamada ad belirtilmediğinden, kod sütun adı karmaşık tür adı ile başlangıç kuralı ilk kullanmaz.

![Bloglar tablo ve sütun olarak yeniden adlandırıldı](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a>DatabaseGenerated

Hesaplanan özellikler olanağı önemli veritabanı özellikleri var. Code First sınıflarınızı eşleniyorsa, hesaplanan sütunlar içeren tablolar, varlık Çerçevesi'bu sütunları güncelleştirmeye çalışmak istemediğiniz. Ancak, veritabanından eklenmiş veya güncelleştirilmiş verileri sonra bu değerleri döndürülecek EF istiyorsunuz. Sınıfınızda, hesaplanan enum yanı sıra özelliklere işaretleyemedi DatabaseGenerated ek açıklama kullanabilirsiniz. Diğer numaralandırmalar yok ve kimlik.

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

Kod ilk veritabanı oluşturulurken bayt veya zaman damgası sütuna oluşturulan veritabanı kullanabilir, aksi durumda yalnızca bu kod önce hesaplanan sütun için formülün belirlemek mümkün olmayacağından, mevcut veritabanlarının işaret ettiğinde kullanmanız gerekir.

Varsayılan olarak, okuma, tamsayı olan bir anahtar özellik veritabanındaki bir kimlik anahtarı olur. DatabaseGenerated DatabaseGenerationOption.Identity için ayarı ile aynı olacaktır. Bir kimlik anahtarı olmasını istemiyorsanız DatabaseGenerationOption.None için değeri ayarlayabilirsiniz.

 

## <a name="index"></a>Dizin

> [!NOTE]
> **EF6.1 ve sonraki sürümler yalnızca** -Entity Framework 6.1 içinde dizin özniteliği tanıtılmıştır. Önceki bir sürümü kullanıyorsanız, bu bölümdeki bilgiler, geçerli değildir.

Bir veya daha fazla sütuna kullanarak bir dizin oluşturabilirsiniz **IndexAttribute**. Öznitelik için bir veya daha fazla özellik ekleyerek EF veritabanında oluşturduğunda, veritabanında ilgili dizini oluşturmak neden veya karşılık gelen iskelesini **CreateIndex** Code First Migrations'ı kullanıyorsanız çağırır.

Örneğin, aşağıdaki kod üzerinde oluşturulan bir dizin sonuçlanır **derecelendirme** sütununun **gönderileri** veritabanındaki tablo.

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

Varsayılan olarak, dizin adlandırılacağını **IX\_&lt;özellik adı&gt;**  (IX\_yukarıdaki örnekte derecelendirme). Dizini için bir ad olsa da belirtebilirsiniz. Aşağıdaki örnek dizinin adlandırılmalıdır belirtir **PostRatingIndex**.

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

Varsayılan olarak, dizinleri benzersiz olmayan, ancak kullanabileceğiniz **IsUnique** adlı bir dizin benzersiz olması gerektiğini belirtmek için parametre. Aşağıdaki örnekte benzersiz bir dizin üzerinde tanıtır. bir **kullanıcı**ait oturum açma adı.

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

### <a name="multiple-column-indexes"></a>Birden çok sütun dizinleri

Birden fazla sütuna yayılmış dizinler için belirli bir tabloda birden fazla dizin ek açıklamalar aynı adı kullanarak belirtilir. Çok sütunlu dizinleri oluşturduğunuzda, sipariş sütunlar için dizin belirtmeniz gerekir. Örneğin, aşağıdaki kod üzerinde çok sütunlu bir dizin oluşturur **derecelendirme** ve **BlogId** adlı **IX\_BlogAndRating**. **BlogId** dizini ilk sütunda ve **derecelendirme** saniyedir.

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a>İlişki öznitelikleri: InverseProperty ve ForeignKey

> [!NOTE]
> Bu sayfa, Code First modelinizde veri ek açıklamalarını kullanma ilişkileri ayarlama hakkında bilgi sağlar. EF ve erişmek ve ilişkileri kullanarak verileri işlemek nasıl ilişkiler hakkında genel bilgi için bkz. [ilişkileri ve gezinti özellikleri](~/ef6/fundamentals/relationships.md). *

Kod ilk kuralı modelinizdeki en yaygın ilişki ölçeklendirilmesini, ancak Yardım gereken yere bazı durumlar vardır.

Anahtar özelliği ilişkisini gönderi ile ilgili bir sorun oluşturulan Blog sınıfında adının değiştirilmesi. 

Veritabanı oluşturma, kod ilk Post sınıfı BlogId özelliğinde görür ve, bir sınıf adına ve "Id" olarak Blog sınıfı için yabancı anahtar değeriyle aynı olduğunu kural olarak tanır. Ancak blog sınıfında BlogId özellik yok. Bu çözüm iletide bir gezinti özelliği oluşturun ve ilk iki sınıf arasında bir ilişki oluşturma işlemini anlama kod yardımcı olmak için yabancı DataAnnotation kullanmaktır — Post.BlogId özelliğini kullanarak — kısıtlamalarını belirleme yanı sıra Veritabanı.

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

Veritabanı kısıtlamasındaki InternalBlogs.PrimaryTrackingKey Posts.BlogId arasında bir ilişki gösterilmektedir. 

![InternalBlogs.PrimaryTrackingKey Posts.BlogId arasındaki ilişki](~/ef6/media/jj591583-figure09.png)

Sınıflar arasında birden çok ilişkilerine sahip InverseProperty kullanılır.

Post sınıfta olan bir blog gönderisi yazıp izlemek isteyebilirsiniz düzenleyen kişiyi yanı sıra. Post sınıfı için iki yeni gezinti özellikleri aşağıda verilmiştir.

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

Bu özellik tarafından başvurulan kişi sınıfı eklemek gerekir. Kişi sınıfın tüm kişi ve tüm bu kişi tarafından güncelleştirilmiş gönderilerinin tarafından yazılan gönderiler için bir posta dön Gezinti özellikleri vardır.

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

Kod ilk iki sınıf kendi özelliklerinde eşleştirilecek mümkün değil. Gönderiler için veritabanı tablosu oluşturan kişi için bir yabancı anahtar olmalıdır, biri UpdatedBy kişi ancak kod ilk oluşturacaktır dört yabancı anahtar özelliklerini: kişi\_kimliği, kişi\_ıd1, oluşturan\_kimliği ve UpdatedBy\_kimliği.

![Tablo fazladan yabancı anahtarlar ile gönderir.](~/ef6/media/jj591583-figure10.png)

Bu sorunları düzeltmek için InverseProperty ek açıklama özellikleri hizalaması belirtmek için kullanabilirsiniz.

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

Bizzat PostsWritten özelliği bu Post türe başvurur bildiğinden Post.CreatedBy ilişkisi oluşturun. Benzer şekilde, PostsUpdated Post.UpdatedBy için bağlanacaksınız. Ve kod önce ek yabancı anahtarlar oluşturmaz.

![Ek yabancı anahtarlar tablosuz gönderir](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a>Özet

DataAnnotations yalnızca, istemci ve sunucu tarafı doğrulama kodu ilk sınıflarınızı açıklayan sağlar, ancak geliştirmek ve hatta kendi kurallarına dayalı sınıflarınızı hakkında kod ilk hale getirecek varsayımların düzeltmek de sağlar. İle DataAnnotations, yalnızca veritabanı şeması oluşturma sürücü değil, ancak kod ilk sınıflarınızı önceden varolan bir veritabanına eşleyebilirsiniz.

Bunlar çok esnek olmakla birlikte, yalnızca en yaygın olarak kod ilk sınıflarınızı üzerinde yaptığınız değişikliklerin gerekli DataAnnotations sağlayan aklınızda bulundurun. Sınıflarınızı istisnai durumlara bazıları için yapılandırmak için diğer yapılandırma mekanizması, Code First's Fluent API'si görünmelidir.
