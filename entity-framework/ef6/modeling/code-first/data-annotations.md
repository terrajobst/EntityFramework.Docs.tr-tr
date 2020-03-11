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
# <a name="code-first-data-annotations"></a>Code First veri açıklamaları
> [!NOTE]
> **EF 4.1 yalnızca** sonraki sürümler-bu sayfada açıklanan özellikler, API 'ler vb. Entity Framework 4,1 ' de tanıtılmıştı. Önceki bir sürümü kullanıyorsanız, bu bilgilerin bazıları veya tümü uygulanmaz.

Bu sayfadaki içerik, başlangıçta Julie Lerman (\<http://thedatafarm.com>)tarafından yazılmış bir makaleden uyarlanmıştır.

Entity Framework Code First, sorgu, değişiklik izleme ve işlevleri güncelleştirme işlemleri gerçekleştirmek için EF 'in kullandığı modeli göstermek üzere kendi etki alanı sınıflarınızı kullanmanıza olanak sağlar. Code First ' yapılandırma üzerinden kural ' olarak adlandırılan bir programlama deseninin üzerinden yararlanır. Code First, sınıflarınızın Entity Framework kurallarını izlediğinden emin olur ve bu durumda, işini nasıl gerçekleştireceğiniz otomatik olarak çalışır. Ancak, sınıflarınız bu kurallara uymadıysanız, gerekli bilgileri içeren EF sağlamak için sınıflarınıza yapılandırma ekleme olanağınız vardır.

Code First, bu yapılandırmaların sınıflarınızı eklenmesi için iki yol sunar. Bunlardan biri, Datanot 'ler adlı basit öznitelikleri, ikincisi ise imperatively yapılandırma bilgilerini (kod içinde) açıklamanıza olanak sağlayan Code First akıcı API 'yi kullanmaktır.

Bu makale, sınıflarınızı yapılandırmak için gerekli olan en sık kullanılan yapılandırmaların vurgulanması için veri açıklamalarını (System. ComponentModel. Dataaçıklamalarda ad alanında) kullanmaya odaklanacaktır. Dataaçıklamalarda Ayrıca, ASP.NET MVC gibi çeşitli .NET uygulamaları (Bu uygulamaların istemci tarafı doğrulamaları için aynı ek açıklamaların kullanmasına izin veren) tarafından da anlaşılmıştır.


## <a name="the-model"></a>Model

Basit bir sınıf çiftiyle Code First veri açıklamalarını göstereceğim: blog ve gönderi.

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

Bu şekilde, blog ve gönderi sınıfları kod ilk kuralını kullanır ve EF uyumluluk sağlamak için bir tdalgalı KS gerektirmez. Ancak, açıklamaları ve bunların eşlendikleri veritabanı hakkında daha fazla bilgi sağlamak için ek açıklamaları da kullanabilirsiniz.

 

## <a name="key"></a>Anahtar

Entity Framework, varlık izleme için kullanılan bir anahtar değeri olan her varlığa dayanır. Code First bir kuralı örtük anahtar özelliklerdir; Code First, "ID" adlı bir özelliği veya "blogID" gibi sınıf adı ve "kimlik" birleşimini arayacaktır. Bu özellik, veritabanındaki bir birincil anahtar sütunuyla eşleşmeyecektir.

Blog ve post sınıflarının her ikisi de bu kuralı izler. Ne olursa? Blog bunun yerine *Primarytrackingkey* adını kullansa da, hatta *foo*? Kod ilk olarak bu kurala uyan bir özellik bulamazsa, bir anahtar özelliği olması gereken Entity Framework gereksinimi nedeniyle bir özel durum oluşturur. EntityKey olarak kullanılacak özelliği belirlemek için anahtar ek açıklamasını kullanabilirsiniz.

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

Kod birincisinin veritabanı oluşturma özelliğini kullanıyorsanız, blog tablosu, varsayılan olarak kimlik olarak da tanımlanan PrimaryTrackingKey adlı bir birincil anahtar sütununa sahip olur.

![Birincil anahtarla blog tablosu](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a>Bileşik anahtarlar

Entity Framework, birden fazla özellikten oluşan bileşik anahtarlar-birincil anahtarlar destekler. Örneğin, birincil anahtarı PassportNumber ve ıssuingcountry birleşimi olan bir Passport sınıfına sahip olabilirsiniz.

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

EF modelinizde yukarıdaki sınıfı kullanmaya çalışmak `InvalidOperationException`neden olur:

*' Passport ' türü için bileşik birincil anahtar sıralaması belirlenemiyor. Bileşik birincil anahtarlar için bir sipariş belirtmek üzere ColumnAttribute veya HasKey metodunu kullanın.*

Bileşik anahtarlar kullanabilmek için Entity Framework, anahtar özellikler için bir sıra tanımlamanızı gerektirir. Bunu, bir sipariş belirtmek için sütun ek açıklamasını kullanarak yapabilirsiniz.

>[!NOTE]
> Düzen değeri göreli olur (dizin tabanlı değil), böylece herhangi bir değer kullanılabilir. Örneğin, 100 ve 200, 1 ve 2 ' nin yerine kabul edilebilir.

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

Birleşik yabancı anahtarlar içeren varlıklarınız varsa, ilgili birincil anahtar özellikleri için kullandığınız sütun sıralamasını belirtmeniz gerekir.

Yalnızca yabancı anahtar özellikleri içindeki göreli sıralama aynı olmalıdır, **sipariş** için atanan değerlerin tam olarak eşleşmesi gerekmez. Örneğin, aşağıdaki sınıfta, 1 ve 2 ' nin yerine 3 ve 4 kullanılabilir.

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

Gerekli ek açıklama EF öğesine belirli bir özelliğin gerekli olduğunu söyler.

Title özelliğine gerekli ekleme özelliği, özelliğin veri içerdiğinden emin olmak için EF (ve MVC) özelliğini zorunlu kılar.

``` csharp
    [Required]
    public string Title { get; set; }
```

Uygulamada ek kod veya biçimlendirme değişikliği olmadan, bir MVC uygulaması, özelliği ve ek açıklama adlarını kullanarak dinamik olarak bir ileti oluşturarak istemci tarafı doğrulaması gerçekleştirir.

![Başlığa sahip sayfa oluştur gerekli hata](~/ef6/media/jj591583-figure02.png)

Gerekli öznitelik, eşlenen özelliği null atanamaz hale getirerek oluşturulan veritabanını da etkiler. Title alanının "Not null" olarak değiştirildiğine dikkat edin.

>[!NOTE]
> Bazı durumlarda, özelliği gerekli olsa bile veritabanındaki sütunun null olmaması mümkün olmayabilir. Örneğin, birden çok tür için bir TPH devralma stratejisi verisi kullanıldığında tek bir tabloda depolanır. Türetilmiş bir tür gerekli bir özellik içeriyorsa, hiyerarşideki tüm türlerin bu özelliği içermesi olmadığından, sütun null yapılamayan yapılamaz.

 

![Bloglar tablosu](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a>MaxLength ve MinLength

MaxLength ve MinLength öznitelikleri, gerekli olduğu gibi ek özellik doğrulamaları belirtmenize olanak tanır.

Length gereksinimlerine sahip BloggerName. Örnek ayrıca özniteliklerin nasıl birleştirileceğini gösterir.

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

MaxLength ek açıklaması, özelliğin uzunluğunu 10 olarak ayarlayarak veritabanını etkiler.

![BloggerName sütununda maksimum uzunluğu gösteren Bloglar tablosu](~/ef6/media/jj591583-figure04.png)

MVC istemci tarafı ek açıklaması ve EF 4,1 sunucu tarafı ek açıklaması, her ikisi de bu doğrulamaya uyar ve bir hata iletisini dinamik olarak oluşturuyor: "alan BloggerName, en fazla uzunluğu ' 10 ' olan bir dize veya dizi türü olmalıdır." Bu ileti çok uzun. Birçok ek açıklama, ErrorMessage özniteliğiyle bir hata iletisi belirtmenizi sağlar.

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Gerekli ek açıklamada ErrorMessage de belirtebilirsiniz.

![Özel hata iletisiyle sayfa oluştur](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a>Noteşlendi

Code First yöntemi, desteklenen bir veri türü olan her özelliğin veritabanında temsil edileceğini belirler. Ancak bu, uygulamalarınızda her zaman durum değildir. Örneğin, blog sınıfında başlık ve BloggerName alanlarını temel alan bir kod oluşturan bir özelliğe sahip olabilirsiniz. Bu özellik dinamik olarak oluşturulabilir ve depolanması gerekmez. Veritabanıyla eşlenmez olan herhangi bir özelliği, bu BlogCode özelliği gibi Noteşlenmiş ek açıklamayla işaretleyebilirsiniz.

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

 

## <a name="complextype"></a>Türündedir

Etki alanı varlıklarınızı bir sınıf kümesi genelinde anlatmak ve ardından bu sınıfları bir bütün varlığı tanımlayacak şekilde katmanlara eklemek çok seyrek değildir. Örneğin, modelinize BlogDetails adlı bir sınıf ekleyebilirsiniz.

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

BlogDetails öğesinin herhangi bir anahtar özelliği türüne sahip olmadığına dikkat edin. Etki alanı odaklı tasarımda, BlogDetails değer nesnesi olarak adlandırılır. Entity Framework, değer nesnelerini karmaşık türler olarak ifade eder.  Karmaşık türler kendi üzerinde izlenemez.

Ancak blog sınıfındaki bir özellik olarak BlogDetails, bir blog nesnesinin bir parçası olarak izlenir. Kodun bu adı tanıması için, BlogDetails sınıfını bir ComplexType olarak işaretlemeniz gerekir.

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Artık blog sınıfına, bu blog için BlogDetails temsil eden bir özellik ekleyebilirsiniz.

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

Veritabanında blog tablosu, BlogDetail özelliğinde yer alan özellikler de dahil olmak üzere blogun tüm özelliklerini içerir. Varsayılan olarak, her biri daha önce karmaşık türün adı olan BlogDetail.

![Karmaşık türdeki blog tablosu](~/ef6/media/jj591583-figure06.png)


## <a name="concurrencycheck"></a>ConcurrencyCheck

ConcurrencyCheck ek açıklaması, bir Kullanıcı bir varlığı düzenlediğinde veya sildiğinde veritabanında eşzamanlılık denetimi için kullanılacak bir veya daha fazla özelliği işaretetmenize olanak tanır. EF Designer ile çalışıyorsanız bu, özelliğin ConcurrencyMode 'ı fixed olarak ayarlamaya göre hizalanır.

BloggerName özelliğine ekleyerek ConcurrencyCheck 'in nasıl çalıştığını görelim.

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

SaveChanges çağrıldığında, BloggerName alanındaki ConcurrencyCheck ek açıklaması nedeniyle, bu özelliğin özgün değeri güncelleştirmede kullanılacaktır. Komutu doğru satırı yalnızca anahtar değerde değil, BloggerName özgün değerine göre filtreleyerek bulmaya çalışacaktır.  GÜNCELLEŞTIRME komutunun veritabanına gönderilen kritik bölümleri aşağıda verilmiştir. komutun, bir PrimaryTrackingKey değeri 1 ve bir BloggerName "Julie" olan satırı veritabanından alındığı zaman özgün değeri olan "" olarak güncelleştiği, burada görebilirsiniz.

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

Bu, bu bloga ait blogu adını bu sırada değiştirdiyseniz, bu güncelleştirme başarısız olur ve işlemeniz gereken bir DbUpdateConcurrencyException alırsınız.

 

## <a name="timestamp"></a>Ilişkin

Eşzamanlılık denetimi için rowversion veya timestamp alanlarını kullanmak daha yaygındır. Ancak, ConcurrencyCheck ek açıklamasını kullanmak yerine, özelliğin türü byte dizisi olduğu sürece daha belirli zaman damgası ek açıklamasını kullanabilirsiniz. Önce kod zaman damgası özelliklerini ConcurrencyCheck özellikleriyle aynı kabul eder, ancak kodun ilk oluşturduğu veritabanı alanının null değer atanamaz olduğundan da emin olur. Belirli bir sınıfta yalnızca bir zaman damgası özelliğine sahip olabilirsiniz.

Aşağıdaki özelliği blog sınıfına ekleme:

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

kod sonuçları, veritabanı tablosunda null yapılamayan bir zaman damgası sütunu oluşturuyor.

![Zaman damgası sütunuyla Bloglar tablosu](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a>Tablo ve sütun

Veritabanı oluşturmaya Code First izin verirseniz, oluşturduğu tablo ve sütunların adını değiştirmek isteyebilirsiniz. Ayrıca, var olan bir veritabanıyla Code First de kullanabilirsiniz. Ancak, etki alanındaki sınıfların ve özelliklerin adlarının veritabanınızdaki tablo ve sütunların adlarıyla eşleşmesi her zaman değildir.

Sınıfım, blog ve kurala göre adlandırılır, kod ise bu, blogların adlı bir tabloyla eşlenir. Böyle bir durum söz konusu değilse tablo özniteliği ile tablonun adını belirtebilirsiniz. Örneğin, ek açıklama tablo adının InternalBlogs olduğunu belirtmektir.

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

Sütun ek açıklaması, eşlenmiş bir sütunun özniteliklerini belirtmekte olan bir Apart ' dır. Bir ad, veri türü ya da tabloda bir sütunun göründüğü sıra gibi bir ad girebilirsiniz. Sütun özniteliğine bir örnek aşağıda verilmiştir.

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

Sütunun TypeName özniteliğini DataType DataAnnotation ile karıştırmayın. Veri türü, Kullanıcı arabirimi için kullanılan bir ek açıklama ve Code First tarafından yok sayılır.

Oluşturulduktan sonra tablo aşağıda verilmiştir. Tablo adı InternalBlogs olarak değiştirilmiştir ve karmaşık türden Açıklama sütunu artık BlogDescription. Ad ek açıklamada belirtildiğinden, önce kod sütun adını karmaşık türün adı ile başlatma kuralını kullanmaz.

![Bloglar tablosu ve sütunu yeniden adlandırıldı](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a>Veritabanı oluşturuldu

Önemli bir veritabanı özellikleri, hesaplanmış Özellikler kullanabilme özelliğidir. Code First sınıflarınızı, hesaplanan sütunları içeren tablolarla eşleştirmeniz durumunda, bu sütunları güncelleştirmeyi denemek Entity Framework istemezsiniz. Ancak verileri ekledikten veya güncelleştirdikten sonra bu değerleri veritabanından döndürmesini isteyebilirsiniz. Sınıfınızın bu özelliklerini hesaplanan Enum ile birlikte işaretlemek için DatabaseGenerated ek açıklamasını kullanabilirsiniz. Diğer numaralandırmalar None ve Identity 'Tur.

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

Kod ilk veritabanını oluştururken bayt veya zaman damgası sütunlarında oluşturulan veritabanını kullanabilirsiniz; Aksi takdirde, kod ilk olarak hesaplanan sütun formülünü belirleyemeyeceği için yalnızca var olan veritabanlarına işaret ederken bunu kullanmanız gerekir.

Yukarıdaki, varsayılan olarak bir tamsayı olan anahtar özelliği veritabanında bir kimlik anahtarı olacak şekilde daha fazla bilgi edinebilirsiniz. Bu, DatabaseGeneratedOption. Identity olarak oluşturulan databasesetting ile aynı olacaktır. Kimlik anahtarı olmasını istemiyorsanız, bu değeri DatabaseGeneratedOption. None olarak ayarlayabilirsiniz.

 

## <a name="index"></a>Dizin oluşturma

> [!NOTE]
> **EF 6.1 yalnızca** sonraki sürümler-dizin özniteliği Entity Framework 6,1 ' de tanıtılmıştı. Önceki bir sürümü kullanıyorsanız, bu bölümdeki bilgiler uygulanmaz.

**Indexattribute**kullanarak bir veya daha fazla sütunda bir dizin oluşturabilirsiniz. Özniteliği bir veya daha fazla özelliğe eklemek, EF veritabanını oluşturduğunda karşılık gelen dizini veritabanında oluşturmasına veya Code First Migrations kullanıyorsanız karşılık gelen CreateIndex çağrılarını dolandırıcıya **dönüştürmesine** neden olur.

Örneğin, aşağıdaki kod veritabanındaki **postalar** tablosunun **Derecelendirme** sütununda oluşturulacak bir dizin oluşmasına neden olur.

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

Varsayılan olarak, Dizin **x\_&lt;Özellik adı&gt;** (Yukarıdaki örnekte x\_derecelendirmesi) olarak adlandırılır. Ayrıca, dizin için de bir ad belirtebilirsiniz. Aşağıdaki örnek, dizininin **Postraytingındex**olarak adlandırılması gerektiğini belirtir.

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

Varsayılan olarak, dizinler benzersiz değildir, ancak bir dizinin benzersiz olması gerektiğini belirtmek için **IsUnique** adlandırılmış parametresini kullanabilirsiniz. Aşağıdaki örnekte, **kullanıcının**oturum açma adında benzersiz bir dizin tanıtılmıştır.

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

### <a name="multiple-column-indexes"></a>Birden çok sütunlu dizinler

Birden çok sütuna yayılan dizinler, belirli bir tablo için birden fazla dizin ek açıklamasında aynı ad kullanılarak belirtilir. Çok sütunlu dizinler oluşturduğunuzda, dizindeki sütunlar için bir sıra belirtmeniz gerekir. Örneğin, aşağıdaki kod, **Derecelendirme** ve **blogID** **\_blogidandrating**adlı bir çok sütunlu dizin oluşturur. **BlogID** dizindeki ilk sütundur ve **Derecelendirme ikincisdir** .

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a>İlişki öznitelikleri: Evirseproperty ve ForeignKey

> [!NOTE]
> Bu sayfa, veri ek açıklamalarını kullanarak Code First modelinizde ilişkiler ayarlama hakkında bilgi sağlar. EF 'teki ilişkiler ve ilişkileri kullanarak verilere erişme ve verileri işleme hakkında genel bilgi için bkz. [relationships & gezinti özellikleri](~/ef6/fundamentals/relationships.md). *

Kod ilk kuralı modelinizdeki en yaygın ilişkilerle ilgilenirken, ancak yardım gerektiren bazı durumlar vardır.

Blog sınıfındaki anahtar özelliğinin adının değiştirilmesi, gönderiyle ilişkili bir sorun oluşturdu. 

Veritabanı oluşturulurken, kod ilk olarak post sınıfında blogID özelliğini görür ve onu bir sınıf adı artı "kimlik" olarak, blog sınıfına yabancı anahtar olarak eşleşen kurala göre tanır. Ancak blog sınıfında blogID özelliği yok. Bunun çözümü, göndermede bir gezinti özelliği oluşturmak ve yabancı veri ek açıklamasını kullanarak kodun ilk olarak iki sınıf arasında ilişki oluşturma (Post. blogID özelliğini kullanarak) ve içinde kısıtlamaları belirtme veritabanınızı.

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

Veritabanındaki kısıtlama, InternalBlogs. PrimaryTrackingKey ve gönderimleri. blogID arasındaki ilişkiyi gösterir. 

![InternalBlogs. PrimaryTrackingKey ve gönderimleri. blogID arasındaki ilişki](~/ef6/media/jj591583-figure09.png)

Ters Çevir özelliği, sınıflar arasında birden fazla ilişksahipseniz kullanılır.

Post sınıfında, bir blog gönderisi yazanın yanı sıra kimin düzenlediğinden haberdar olmak isteyebilirsiniz. Post sınıfının iki yeni gezinti özelliği aşağıda verilmiştir.

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

Ayrıca, bu özellikler tarafından başvurulan kişi sınıfına da eklemeniz gerekir. Kişi sınıfının gönderiye geri dönmesi, biri kişi tarafından yazılan gönderilerin hepsi ve bu kişi tarafından güncellenen tüm gönderiler için bir tane.

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

Önce kod, kendi kendine iki sınıftaki özellikleri eşleşmeyebilir. Gönderimler için veritabanı tablosunda, CreatedBy kişiye ait bir yabancı anahtar ve bir tane de UpdatedBy kişiye ait olmalıdır, ancak kod ilk olarak dört yabancı anahtar özelliği oluşturur: kişi\_kimliği, kişi\_ID1, CreatedBy\_ID ve UpdatedBy\_ID.

![Ek yabancı anahtarlarla nakleder tablosu](~/ef6/media/jj591583-figure10.png)

Bu sorunları çözebilmeniz için, özelliklerin hizalamasını belirtmek üzere Evirseproperty ek açıklamasını kullanabilirsiniz.

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

Bu kişiye ait Postsyazıldığı özelliği, bu, post türüne başvurduğunu bildiği için, bu, gönderi. CreatedBy ile ilişki oluşturur. Benzer şekilde, PostsUpdated, posta. UpdatedBy Ile gönderilir. Ve ilk olarak kod, ek yabancı anahtarlar oluşturmaz.

![Ek yabancı anahtarlar olmadan gönderi tablosu](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a>Özet

Veri açıklamaları yalnızca kodunuzun ilk sınıflarında istemci ve sunucu tarafı doğrulamasını açıklamanıza izin vermekle kalmaz, aynı zamanda kodun kuralları temel alınarak sınıflarınız hakkında daha fazla bilgi sahibi olacağı varsayımları geliştirmenize ve hatta düzeltmenizi de sağlar. Dataaçıklamalarla yalnızca veritabanı şeması oluşturmayı kullanamazsınız, ancak kod ilk sınıflarınızı önceden var olan bir veritabanına eşleyebilirsiniz.

Çok esnek olsa da, Datanot açıklamalarının yalnızca kod ilk sınıflarınızda yapabileceğiniz en sık gereken yapılandırma değişikliklerini sunabileceini unutmayın. Sınıflarınızı bazı uç durumlardan bazılarına göre yapılandırmak için, farklı yapılandırma mekanizmasına, Code First akıcı API 'sine bakmanız gerekir.
