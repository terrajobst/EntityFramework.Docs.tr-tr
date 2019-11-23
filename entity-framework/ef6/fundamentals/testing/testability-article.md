---
title: Test edilebilirlik ve Entity Framework 4,0-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 28ec5446ce9faf98fb8fff141832236d70b29daf
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181585"
---
# <a name="testability-and-entity-framework-40"></a>Test edilebilirlik ve Entity Framework 4,0
Scott Allen

Yayımlandı: Mayıs 2010

## <a name="introduction"></a>Giriş

Bu Teknik İnceleme, ADO.NET Entity Framework 4,0 ve Visual Studio 2010 ile nasıl bir test edilebilir kod yazılacağını açıklar. Bu kağıt, test odaklı tasarım (TDD) veya davranış odaklı tasarım (BDD) gibi belirli bir test metodolojisine odaklanmayı denemez. Bunun yerine, bu kağıt, otomatik bir biçimde yalıtmak ve test etmek için, ADO.NET Entity Framework kullanan kodu yazma konusunda odaklanacaktır. Veri erişim senaryolarında sınamayı kolaylaştıran ortak tasarım düzenlerine bakacağız ve Framework kullanırken bu desenleri nasıl uygulayacağınızı öğreneceksiniz. Ayrıca, bu özelliklerin test edilebilir kodda nasıl çalıştığını görmek için Framework 'ün belirli özelliklerine bakacağız.

## <a name="what-is-testable-code"></a>Testable kod nedir?

Otomatikleştirilmiş birim testlerini kullanarak bir yazılım parçasını doğrulama özelliği, birçok tercih eden avantaj sunar. Herkes iyi testlerin bir uygulamadaki yazılım kusurlarının sayısını azalttığını ve uygulamanın kalitesini artırdığını bilir, ancak yerinde birim testlerine sahip olmak, yalnızca hata bulmaktan daha fazla ilerler.

İyi bir birim test paketi, geliştirme ekibinin zaman kaydetmesine ve oluşturdukları yazılımın denetiminde kalmasına izin verir. Bir ekip, yeni gereksinimleri karşılamak için mevcut kodda değişiklik yapabilir, yeniden düzenleme, yeniden düzenleme ve yazılımı yeniden yapılandırabilir ve test paketinin uygulamanın davranışını doğrulayabilirken bir uygulamaya yeni bileşenler ekleyebilirler. Birim testleri, değişikliği kolaylaştırmak ve yazılım bakım artışı arttıkça yazılımın bakım artışına karşı korumak için hızlı geri bildirim döngüsünün bir parçasıdır.

Ancak birim testi bir fiyatla birlikte gelir. Bir ekibin birim testlerini oluşturmak ve sürdürmek için zaman yatırmaları gerekebilir. Bu testleri oluşturmak için gereken çaba miktarı, temel yazılımın **Test edilebilirlik** ile doğrudan ilgilidir. Yazılım ne kadar kolay test edilecek? Göz önünde bulundurularak yazılım tasarlayan bir ekip, takımdan çalışmayan yazılımlarla çalışan ekipten daha hızlı etkili sınamalar oluşturacaktır.

Microsoft, ADO.NET Entity Framework 4,0 (EF4) 'i test edilebilirlik ile tasarlamıştır. Bu, geliştiricilerin çerçeve kodu için birim testleri yazmaları anlamına gelmez. Bunun yerine, EF4 için test edici amaçları, Framework üzerinde yapı oluşturan test edilebilir kod oluşturmayı kolaylaştırır. Belirli örneklere bakmadan önce, test edilebilir kodun kalitelerini anlamak oldukça önemlidir.

### <a name="the-qualities-of-testable-code"></a>Testable kodunun nitelikleri

Test için kolay olan kod her zaman en az iki nitelikler sergiler. İlk olarak, test edilebilir kodun **gözlemek**kolaydır. Bazı girişler kümesi verildiğinde kodun çıkışını gözlemlemek kolay olmalıdır. Örneğin, yöntemi, bir hesaplamanın sonucunu doğrudan döndürdüğünden, aşağıdaki yöntemi kolayca test edin.

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

Yöntem, hesaplanan değeri bir ağ yuvasına, bir veritabanı tablosuna veya aşağıdaki kodla benzer bir dosyaya yazdığında bir yöntemi test etmek zordur. Test, değeri almak için ek iş gerçekleştirmelidir.

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

İkinci olarak, test edilebilir kodun kolayca **yalıtılacağı**. Aşağıdaki sözde kodu yanlış bir test kodu örneği olarak kullanalım.

``` csharp
    public int ComputePolicyValue(InsurancePolicy policy) {
        using (var connection = new SqlConnection("dbConnection"))
        using (var command = new SqlCommand(query, connection)) {

            // business calculations omitted ...               

            if (totalValue > notificationThreshold) {
                var message = new MailMessage();
                message.Subject = "Warning!";
                var client = new SmtpClient();
                client.Send(message);
            }
        }
        return totalValue;
    }
```

Yöntemi, daha kolay bir şekilde bir sigorta ilkesi geçirebilir ve dönüş değerinin beklenen bir sonuçla eşleşip eşleşmediğini doğrulayabilirsiniz. Ancak, yöntemi test etmek için doğru şemaya sahip bir veritabanının yüklü olması ve yöntemin bir e-posta göndermeyi denediği durumlarda SMTP sunucusunu yapılandırmanız gerekir.

Birim testi yalnızca yöntemi içindeki hesaplama mantığını doğrulamak istiyor, ancak e-posta sunucusu çevrimdışı olduğu veya veritabanı sunucusu taşındığı için test başarısız olabilir. Bu hatalardan her ikisi de testin doğrulamak istediği davranışta ilgisiz değildir. Davranışı yalıtmak zordur.

Test edilebilir kod yazmak için gereken yazılım geliştiricileri, genellikle yazdıkları koddaki kaygıları ayırmayı sürdürmek için çaba harcar. Yukarıdaki yöntem iş hesaplamalarına odaklanmalı ve diğer bileşenlere veritabanı ve e-posta uygulama ayrıntılarını devredebilir. Robert C. MARTIN bu tek sorumluluk Ilkesini çağırır. Bir nesne, bir ilkenin değerini hesaplamak gibi tek ve dar bir sorumluluğu kapsüllemelidir. Diğer tüm veritabanı ve bildirim çalışmaları, başka bir nesnenin sorumluluğu olmalıdır. Bu biçimde yazılan kodun tek bir göreve odaklandığı için yalıtmak daha kolaydır.

.NET ' te, tek sorumluluk Ilkesini izlemek ve yalıtıma ulaşmak için gereken soyutlamalar var. Arabirim tanımlarını kullanabilir ve kodu somut bir tür yerine arabirim soyutlamasını kullanmaya zorlayabilirsiniz. Bu yazının ilerleyen kısımlarında, yukarıdaki hatalı *örnek gibi bir* yöntemin veritabanıyla iletişim kurabiledikleri gibi arabirimler ile nasıl çalışabilecekleri hakkında bilgi edineceksiniz. Bununla birlikte, test sırasında veritabanıyla iletişim kurmayacak, ancak bellekte verileri tutan bir kukla uygulamayı yerine getirebilirsiniz. Bu kukla uygulama, veri erişim kodundaki veya veritabanı yapılandırmasındaki ilişkisiz sorunlardan kodu yalıtır.

Yalıtımın ek avantajları vardır. Son yöntemdeki iş hesaplamasının yürütülmesi yalnızca birkaç milisaniye sürer, ancak test kendisi ağ etrafında kod attığından ve çeşitli sunuculara ulaşacak kadar birkaç saniye çalışabilir. Küçük değişiklikleri kolaylaştırmak için birim testlerin hızlı çalışması gerekir. Test ile ilişkili olmayan bir bileşen bir sorun içerdiğinden, birim testleri de yinelenebilir olmalıdır ve başarısız olmaz. Gözlemek ve yalıtmak için çok daha kolay kod yazmak, geliştiricilerin kod için testleri yazma daha kolay zaman harcayacağı, testlerin yürütülmesi için beklediği daha az zaman harcaması ve daha önemlisi, mevcut olmayan hataları daha az zaman harcamaya daha fazla bilgi sahibi olacağı anlamına gelir.

Test almanın avantajlarından faydalanmanız ve test edilen kodun getirdiği kaliteleri anlamanız önerilir. EF4 ile birlikte çalışarak, verileri bir veritabanına kaydetmek ve daha kolay bir şekilde yalıtmak için bir kod yazmak istiyoruz, ancak ilk olarak veri erişimi için test edilebilir tasarımları tartışmak üzere odaklanıyoruz.

## <a name="design-patterns-for-data-persistence"></a>Veri kalıcılığı için tasarım desenleri

Daha önce sunulan kötü örneklerden her ikisi de çok fazla sorumluluklara sahipti. İlk hatalı örnek bir hesaplama gerçekleştirmesi *ve* bir dosyaya yazılması gerekiyordu. İkinci hatalı örnek *bir veritabanından veri* okuyup bir iş hesaplaması *ve* e-posta gönderecek. Diğer bileşenlere sorunları ve sorumluluğun yetkisini ayıran daha küçük Yöntemler tasarlayarak, test edilebilir kod yazmak için harika bir yöntem elde edersiniz. Amaç, küçük ve odaklanmış soyutlamalar arasından eylemler oluşturarak işlevselliği dermaktır.

Veri kalıcılığına geldiğinde, bakdığımız küçük ve odaklanmış soyutlamalar, tasarım desenleri olarak belgelendikleri yaygın olarak verilmiştir. Marwler 'in kurumsal uygulama mimarisine ait kitap desenleri, bu desenleri yazdırma sırasında açıklamaya yönelik ilk çalışmamıştı. Bu ADO.NET Entity Framework nasıl uyguladığımızda ve bu desenlerle nasıl çalıştığını göstermemiz için aşağıdaki bölümlerde bu desenlerin kısa bir açıklamasını sunuyoruz.

### <a name="the-repository-pattern"></a>Depo deseninin

Fowler, "etki alanı nesnelerine erişmek için koleksiyon benzeri bir arabirim kullanarak etki alanı ve veri eşleme katmanları arasında bir depo" belirtir. Depo deseninin hedefi, verileri veri erişiminin dakikalarından yalıtmak ve daha önceki yalıtımın, test edilebilirlik için gerekli bir nitelik.

Yalıtım için anahtar, deponun koleksiyon benzeri bir arabirim kullanarak nesneleri nasıl kullanıma sunduğunu gösterir. Depoyu kullanmak için yazdığınız mantık, deponun isteğiniz nesneleri nasıl sunacağı konusunda fikir sahibi değildir. Depo bir veritabanıyla konuşabilir veya yalnızca bellek içi bir koleksiyondaki nesneleri döndürebilir. Tüm kodunuzun bilmeniz gerekir, deponun koleksiyonu sürdürmek için görünmesi ve koleksiyondan nesne alabilir, ekleyebilir ve silebilirsiniz.

Mevcut .NET uygulamalarında somut bir depo genellikle aşağıdaki gibi genel bir arabirimden devralınır:

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

EF4 için bir uygulama sağladığımızda arabirim tanımında birkaç değişiklik yapacağız, ancak temel kavram aynı kalır. Kod, bir koşulun değerlendirilmesine göre bir varlık koleksiyonu almak için bu arabirimi uygulayan somut bir depoyu kullanabilir, bir koşul değerlendirmesini temel alan varlıkların koleksiyonunu alabilir veya yalnızca tüm kullanılabilir varlıkları alırlar. Kod ayrıca depo arabirimi aracılığıyla varlık ekleyebilir ve kaldırabilir.

Çalışan nesnelerinin bir ırepository 'si verildiğinde, kod aşağıdaki işlemleri gerçekleştirebilir.

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

Kod bir arabirim (bir çalışan ırepository) kullandığından, kodu farklı arabirim uygulamalarıyla sağlayabiliriz. Bir uygulama, EF4 ve kalıcı nesneleri Microsoft SQL Server bir veritabanında desteklenen bir uygulama olabilir. Farklı bir uygulama (test sırasında kullandığımız bir adet), çalışan nesnelerinin bellek içi bir listesi tarafından yönetilebilir. Arabirim, kodda yalıtım elde etmenize yardımcı olur.

Irepository&lt;T&gt; arabiriminin bir kaydetme işlemi sergilemediğine dikkat edin. Mevcut nesneleri nasıl güncelleştiririz? Kaydet işlemini içeren ırepository tanımlarında gelebilir ve bu depoların uygulamalarının bir nesneyi veritabanına hemen kalıcı hale getirmeniz gerekecektir. Ancak birçok uygulamada nesneleri ayrı ayrı kalıcı hale getirmek istemedik. Bunun yerine, farklı depolardan belki de nesneleri hayata getirmek istiyoruz, bu nesneleri iş etkinliklerinin bir parçası olarak değiştirebilir ve sonra tüm nesneleri tek bir atomik işlemin parçası olarak kalıcı hale getiririz. Neyse ki, bu tür davranışa izin veren bir model vardır.

### <a name="the-unit-of-work-pattern"></a>Çalışma birimi deseninin

Fowler, bir iş birimini "bir iş işleminden etkilenen nesnelerin listesini koruuyor ve yazma işlemlerini ve eşzamanlılık sorunlarının çözümlendiğini" belirtir. Bu, bir depodan hayata getirdiğimiz nesnelerdeki değişiklikleri izlemek ve iş birimine değişiklikleri kaydetmeye söylediğimiz zaman nesneler üzerinde yaptığımız değişiklikleri sürdürmek için iş biriminin sorumluluğundadır. Ayrıca, tüm depolara eklediğimiz yeni nesneleri almak ve nesneleri bir veritabanına eklemek ve ayrıca silmeyi de zorunlu hale gelmesi için iş biriminin sorumluluğundadır.

ADO.NET veri kümeleri ile herhangi bir çalışmayı yapmadıysanız, iş deseninin birimini zaten öğrenirsiniz. ADO.NET veri kümeleri, güncelleştirmelerimizi, Silinenleri ve DataRow nesneleri ekleme olanımızı takip etmiş ve (TableAdapter 'ın yardımıyla) bir veritabanında yapılan tüm değişiklikleri mutabık hale getirmenize olanak içeriyordu. Ancak, veri kümesi nesneleri temeldeki veritabanının bağlantısı kesik bir alt kümesini modelleyebilir. İş deseninin birimi aynı davranışı sergiler, ancak veri erişim kodundan yalıtılmış ve veritabanının farkında olmadan iş nesneleri ve etki alanı nesneleriyle çalışır.

.NET kodundaki iş birimini modelleyebilir bir soyutlama aşağıdakine benzeyebilir:

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

İş biriminden depo başvurularını açığa çıkarmaktan, tek bir iş nesnesinin bir iş işlemi sırasında gerçekleştirilmiş tüm varlıkları izleyebilmesini sağlayabiliriz. Gerçek bir çalışma birimi için COMMIT yönteminin uygulanması, tüm Magic 'in, bellek içi değişiklikleri veritabanıyla bağdaştırma gerçekleştiği yerdir. 

Bir IUnitOfWork başvurusu verildiğinde, kod, bir veya daha fazla depodan alınan iş nesnelerinde değişiklik yapabilir ve atomik kaydetme işlemini kullanarak tüm değişiklikleri kaydedebilir.

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a>Geç yük kalıbı

Fowler, "ihtiyacınız olan tüm verileri içermeyen, ancak nasıl alınacağını bilen" bir nesneyi belirtmek için Lazy Load adını kullanır. Saydam yavaş yükleme, test kodu yazarken ve ilişkisel veritabanıyla çalışırken önemli bir özelliktir. Örnek olarak, aşağıdaki kodu göz önünde bulundurun.

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

Zaman kartı koleksiyonu nasıl doldurulur? Olası iki yanıt vardır. Bir yanıt, çalışan deposunun bir çalışanı getirmesi istendiğinde, çalışanın ilişkili zaman kartı bilgileriyle birlikte hem çalışanı hem de alacak bir sorgu yayınlar. İlişkisel veritabanlarında, bu genellikle JOIN yan tümcesi içeren bir sorgu gerektirir ve uygulama gereksiniminden daha fazla bilgi alınmasına neden olabilir. Uygulamanın zaman çizelgeleri özelliğine dokunması gerekmediği ne olursa?

İkinci bir yanıt, "isteğe bağlı" zaman kartı özelliğini yüklemek için kullanılır. Kod, zaman kartı bilgilerini almak için özel API 'Leri çağırmadığından, bu yavaş yükleme örtük ve iş mantığına saydamdır. Kod, gerektiğinde kart bilgilerinin mevcut olduğunu varsayar. Genellikle Yöntem etkinleştirmeleri için çalışma zamanı yakalanmasını içeren, geç yükleme ile ilgili bazı Magic vardır. Kesintiye uğratan kod, iş mantığını ücretsiz olarak bırakarak, veritabanıyla iletişim sağlanmasından ve zaman kartı bilgilerinin alınmasında sorumludur. Bu yavaş yük Magic, iş kodunun kendisini veri alma işlemlerinden yalıtmasına ve daha kararlı bir koda neden olmasına olanak sağlar.

Yavaş yükün dezavantajı, bir uygulamanın kodun ek bir sorgu *yürütebilmesi için* zaman kartı bilgisine ihtiyaç duyacaktır. Bu pek çok uygulama için bir sorun değildir; ancak, performans duyarlı uygulamalar veya uygulamalar için bir dizi çalışan nesnesinden döngü gerçekleştirin ve döngünün her yinelemesi sırasında zaman kartlarını almak üzere bir sorgu yürütüyordur (genellikle N + 1 olarak ifade edilir) sorgu sorunu), yavaş yükleme bir sürükle. Bu senaryolarda bir uygulama, zaman kartı bilgilerini mümkün olduğunca en verimli şekilde yüklemek isteyebilir.

Neyse ki, bir sonraki bölüme geçiş yaptığımız ve bu desenleri uygulayan, EF4 'in hem örtük yavaş yükleri hem de verimli Eager yüklerini nasıl desteklediğini görüyoruz.

## <a name="implementing-patterns-with-the-entity-framework"></a>Entity Framework desenler uygulama

En iyi haberler, son bölümde açıklandığımız tüm Tasarım desenlerinin EF4 ile uygulanması için basittir. Bir basit ASP.NET MVC uygulaması kullanarak çalışanları ve ilgili zaman kartı bilgilerini düzenleyebilir ve görüntüleyebilirsiniz. Aşağıdaki "düz eski CLR nesneleri" (POCOs) kullanılarak başlayacağız. 

``` csharp
    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime HireDate { get; set; }
        public ICollection<TimeCard> TimeCards { get; set; }
    }

    public class TimeCard {
        public int Id { get; set; }
        public int Hours { get; set; }
        public DateTime EffectiveDate { get; set; }
    }
```

Bu sınıf tanımları, EF4 'in farklı yaklaşımlarını ve özelliklerini araştırtığımız kadar biraz değişecektir, ancak amaç bu sınıfları mümkün olduğunca Kalıcılık Ignorant (PI) olarak tutacaktır. Bir PI nesnesi, sahip olduğu durumun bir veritabanı içinde *nasıl*olduğunu *veya hatta olduğunu*bilmez. PI ve Pocos, test edilebilir yazılımlarla birlikte çalışmaya devam ediyor. Bir POCO yaklaşımı kullanan nesneler, bir veritabanı mevcut olmadan çalışabildiklerinden daha az kısıtlamalı, daha esnektir ve test edilebilir.

POCOs 'un yerinde, Visual Studio 'da bir Varlık Veri Modeli (EDM) oluşturabilir (bkz. Şekil 1). Varlıklarımıza yönelik kod oluşturmak için EDM 'yi kullanmıyoruz. Bunun yerine, sevdiğimiz varlıkları kullanmak istiyoruz. Veritabanı şemanızı oluşturmak için yalnızca EDM kullanacağız ve nesneleri veritabanına eşlemek için meta veri EF4 ihtiyaçlarını sunacağız.

![EF test_01](~/ef6/media/eftest-01.jpg)

**Şekil 1**

Note: önce EDM modelini geliştirmek isterseniz, EDM 'den temiz, POCO kodu oluşturmak mümkündür. Bunu, veri programlama ekibi tarafından sunulan bir Visual Studio 2010 uzantısıyla yapabilirsiniz. Uzantıyı indirmek için, Visual Studio 'daki Araçlar menüsünden Uzantı Yöneticisi ' ni başlatın ve "POCO" şablonlarının çevrimiçi galerisinde arama yapın (bkz. Şekil 2). EF için birkaç POCO şablonu mevcuttur. Şablonu kullanma hakkında daha fazla bilgi için, bkz. " [Izlenecek yol: POCO şablonu Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".

![EF test_02](~/ef6/media/eftest-02.png)

**Şekil 2**

Bu POCO başlangıç noktasından, test edilebilir kodda iki farklı yaklaşım keşfedecektir. İlk yaklaşım, iş ve depo birimleri uygulamak için Entity Framework API 'den soyutlamalar kullandığından EF yaklaşımını çağırdım. İkinci yaklaşımda kendi özel depo soyutlamalarını oluşturacağız ve ardından her yaklaşımın olumlu ve olumsuz yönlerini görürsünüz. EF yaklaşımını inceleyerek başlayacağız.  

### <a name="an-ef-centric-implementation"></a>EF merkezli bir uygulama

Bir ASP.NET MVC projesinden aşağıdaki denetleyici eylemini göz önünde bulundurun. Eylem bir çalışan nesnesi alır ve çalışanın ayrıntılı görünümünü görüntülemek için bir sonuç döndürür.

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

Kod test edilebilir mi? Eylemin davranışının doğrulanması için gereken en az iki test var. İlk olarak, eylemin doğru görünümü döndürdüğünü ve kolay bir test olduğunu doğrulamak istiyoruz. Ayrıca, eylemin doğru çalışanı aldığını doğrulamak için de bir test yazmak istiyoruz ve veritabanını sorgulamak için kodu yürütmeden bunu yapmak istiyoruz. Test altındaki kodu yalıtmak istediğinizi unutmayın. Yalıtım, veri erişim kodundaki veya veritabanı yapılandırmasındaki bir hata nedeniyle testin başarısız olmamasını güvence altına almayacaktır. Test başarısız olursa, denetleyici mantığındaki bir hata olduğunu ve bazı alt düzey sistem bileşenlerinden olmadığını biliyoruz.

Yalıtım sağlamak için, daha önce depolar ve iş birimleri için sunduğumuz arabirimler gibi bazı soyutlamalar olması gerekir. Depo deseninin, etki alanı nesneleri ve veri eşleme katmanı arasında aracılık için tasarlandığını unutmayın. Bu *SENARYODA EF4 veri* eşleme katmanıdır ve zaten ıobjectset&lt;t&gt; (System. Data. Objects ad alanından) adlı depo benzeri bir soyutlama sağlar. Arabirim tanımı aşağıdaki gibi görünür.

``` csharp
    public interface IObjectSet<TEntity> :
                     IQueryable<TEntity>,
                     IEnumerable<TEntity>,
                     IQueryable,
                     IEnumerable
                     where TEntity : class
    {
        void AddObject(TEntity entity);
        void Attach(TEntity entity);
        void DeleteObject(TEntity entity);
        void Detach(TEntity entity);
    }
```

IObjectSet&lt;T&gt;, bir depo gereksinimlerini karşılar çünkü bir nesne koleksiyonuna (IEnumerable&lt;T&gt;aracılığıyla) benzer ve sanal koleksiyona nesne ekleme ve kaldırma yöntemleri sağlar. Attach ve Detach yöntemleri, EF4 API 'sinin ek özelliklerini kullanıma sunar. Depolamakümesi&lt;T&gt; depolar için arabirim olarak kullanmak için, depoları birbirine bağlamak üzere bir iş soyutlama birimi gerekir.

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

Bu arabirimin somut bir uygulanması SQL Server konuşacak ve EF4 adresinden ObjectContext sınıfını kullanarak oluşturmak kolaydır. ObjectContext sınıfı, EF4 API 'sindeki gerçek iş birimidir.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;
            _context = new ObjectContext(connectionString);
        }

        public IObjectSet<Employee> Employees {
            get { return _context.CreateObjectSet<Employee>(); }
        }

        public IObjectSet<TimeCard> TimeCards {
            get { return _context.CreateObjectSet<TimeCard>(); }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

Bir IObjectReference&lt;T&gt; hayata getirme, ObjectContext nesnesinin CreateObjectSet metodunu çağırmak kadar kolaydır. Arka planda çerçeve, somut bir ObjectSet&lt;T&gt;oluşturmak için EDM 'da sağladığımız meta verileri kullanır. İstemci kodunda test kararlılığını korumaya yardımcı olacağı için IObjectSet&lt;T&gt; arabirimini döndürmeyle başlayacağız.

Bu somut uygulama üretimde faydalıdır, ancak sınamayı kolaylaştırmak için IUnitOfWork soyutımızı nasıl kullanacağımız üzerine odaklanmamız gerekir.

### <a name="the-test-doubles"></a>Test Double değerleri

Denetleyici eylemini yalıtmak için gerçek iş birimi (bir ObjectContext tarafından desteklenir) ve bir test Double veya "sahte" iş birimi arasında geçiş yapma yeteneğinin olması gerekir (bellek içi işlemler gerçekleştiriliyor). Bu tür bir geçiş yapmak için yaygın yaklaşım, MVC denetleyicisinin bir iş birimi oluşturmasını ve bunun yerine çalışma birimini denetleyiciye bir oluşturucu parametresi olarak iletmektir.

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

Yukarıdaki kod, bağımlılık ekleme için bir örnektir. Denetleyicinin bağımlılığı (iş birimi) oluşturmasına izin vermez, ancak bağımlılığı denetleyiciye ekler. Bir MVC projesinde, bağımlılık ekleme işlemini otomatikleştirmek için bir Control (IOC) kapsayıcısının Inversion ile birlikte özel bir denetleyici fabrikası kullanılması yaygındır. Bu konular bu makalenin kapsamı dışındadır, ancak bu makalenin sonundaki başvuruları izleyerek daha fazla bilgi edinebilirsiniz.

Test için kullandığımız sahte bir iş birimi, aşağıdaki gibi görünebilir.

``` csharp
    public class InMemoryUnitOfWork : IUnitOfWork {
        public InMemoryUnitOfWork() {
            Committed = false;
        }
        public IObjectSet<Employee> Employees {
            get;
            set;
        }

        public IObjectSet<TimeCard> TimeCards {
            get;
            set;
        }

        public bool Committed { get; set; }
        public void Commit() {
            Committed = true;
        }
    }
```

Sahte iş biriminin bir iletişim olarak kabul edilen bir özelliği kullanıma sunduğunu fark edin. Bazen, testi kolaylaştıran sahte bir sınıfa özellikler eklemek yararlı olabilir. Bu durumda, işlenen özelliği denetleyerek kodun bir iş birimi işlediğini gözlemlemek kolaydır.

Ayrıca, bellekte çalışan ve zaman kartı nesneleri tutmak için sahte bir IObjectSet&lt;T&gt; gerekir. Genel türler kullanarak tek bir uygulama sağlayabiliriz.

``` csharp
    public class InMemoryObjectSet<T> : IObjectSet<T> where T : class
        public InMemoryObjectSet()
            : this(Enumerable.Empty<T>()) {
        }
        public InMemoryObjectSet(IEnumerable<T> entities) {
            _set = new HashSet<T>();
            foreach (var entity in entities) {
                _set.Add(entity);
            }
            _queryableSet = _set.AsQueryable();
        }
        public void AddObject(T entity) {
            _set.Add(entity);
        }
        public void Attach(T entity) {
            _set.Add(entity);
        }
        public void DeleteObject(T entity) {
            _set.Remove(entity);
        }
        public void Detach(T entity) {
            _set.Remove(entity);
        }
        public Type ElementType {
            get { return _queryableSet.ElementType; }
        }
        public Expression Expression {
            get { return _queryableSet.Expression; }
        }
        public IQueryProvider Provider {
            get { return _queryableSet.Provider; }
        }
        public IEnumerator<T> GetEnumerator() {
            return _set.GetEnumerator();
        }
        IEnumerator IEnumerable.GetEnumerator() {
            return GetEnumerator();
        }

        readonly HashSet<T> _set;
        readonly IQueryable<T> _queryableSet;
    }
```

Bu test, işinin çoğunu temel bir diyez kümesi&lt;T&gt; nesnesine devreder. IObjectSet&lt;T&gt; 'in bir sınıf (başvuru türü) olarak T uygulayan genel bir kısıtlama gerektirdiğini ve ayrıca IQueryable&lt;T&gt;uygulamamızı zorleyebileceğini unutmayın. Bir bellek içi toplamanın standart LINQ işleci olan bir IQueryable&lt;T&gt; olarak görünmesini kolay hale getirmek kolaydır.

### <a name="the-tests"></a>Testler

Geleneksel birim testleri tek bir MVC denetleyicisindeki tüm eylemlerin tüm testlerini tutacak tek bir test sınıfı kullanacaktır. Bu testleri veya herhangi bir birim testi türünü derlediğiniz bellek içi Fakes 'i kullanarak yazalım. Ancak, bu makalede tek parçalı test sınıfı yaklaşımının önüne geçeceğiz ve testlerimizi belirli bir işlevsellik parçasına odaklanmak üzere gruplandıracağız.  Örneğin, "yeni çalışan oluştur" test etmek istediğimiz işlevsellik olabilir, bu nedenle yeni bir çalışan oluşturmaktan sorumlu tek denetleyicideki bir eylemi doğrulamak için tek bir test sınıfı kullanacağız.

Tüm bu ayrıntılı test sınıfları için ihtiyacımız olan bazı yaygın kurulum kodları vardır. Örneğin, her zaman bellek içi depolarımızın ve sahte iş birimimizin oluşturulması gerekir. Ayrıca, bir çalışan denetleyicinin bir örneğine, sahte iş birimi eklenmiş bir örnek de ihtiyacımız var. Bu ortak kurulum kodunu, bir temel sınıf kullanarak test sınıfları genelinde paylaşacağız.

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .ToList();
            _repository = new InMemoryObjectSet<Employee>(_employeeData);
            _unitOfWork = new InMemoryUnitOfWork();
            _unitOfWork.Employees = _repository;
            _controller = new EmployeeController(_unitOfWork);
        }

        protected IList<Employee> _employeeData;
        protected EmployeeController _controller;
        protected InMemoryObjectSet<Employee> _repository;
        protected InMemoryUnitOfWork _unitOfWork;
    }
```

Temel sınıfta kullandığımız "nesne anne", test verileri oluşturmaya yönelik bir ortak modeldir. Bir nesne Anne, test varlıklarını birden çok test armatürleri genelinde kullanılmak üzere örneklemek için Fabrika yöntemleri içerir.

``` csharp
    public static class EmployeeObjectMother {
        public static IEnumerable<Employee> CreateEmployees() {
            yield return new Employee() {
                Id = 1, Name = "Scott", HireDate=new DateTime(2002, 1, 1)
            };
            yield return new Employee() {
                Id = 2, Name = "Poonam", HireDate=new DateTime(2001, 1, 1)
            };
            yield return new Employee() {
                Id = 3, Name = "Simon", HireDate=new DateTime(2008, 1, 1)
            };
        }
        // ... more fake data for different scenarios
    }
```

EmployeeControllerTestBase öğesini bir dizi test armakodu için temel sınıf olarak kullanabiliriz (bkz. Şekil 3). Her test armatürü, belirli bir denetleyici eylemini test eder. Örneğin, bir test armatürü bir HTTP GET isteği sırasında kullanılan oluşturma eyleminin test edilmesine odaklanacaktır (bir çalışan oluşturmak için görünümü görüntülemek için) ve farklı bir armatürü, bir HTTP POST isteğinde kullanılan oluşturma eylemine odaklanacaktır ( bir çalışan oluşturmak için Kullanıcı). Her türetilmiş sınıf, yalnızca belirli bağlamında gerekli olan kurulumdan sorumludur ve belirli test bağlamı için sonuçları doğrulamak üzere gereken onayları sağlamaktır.

![EF test_03](~/ef6/media/eftest-03.png)

**Şekil 3**

Burada sunulan adlandırma kuralı ve test stili, test edilebilir kodu için gerekli değildir; tek bir yaklaşımdır. Şekil 4 ' te, Visual Studio 2010 için Jet Brains ReSharper Test Çalıştırıcısı eklentisinde çalışan testler gösterilmektedir.

![EF test_04](~/ef6/media/eftest-04.png)

**Şekil 4**

Paylaşılan kurulum kodunu işlemek için bir temel sınıf ile, her bir denetleyici eylemi için birim testleri küçük ve yazma kolaydır. Testler hızla yürütülecektir (bellek içi işlemleri gerçekleştirdiğimiz için) ve ilişkisiz altyapı veya çevresel sorunlar nedeniyle başarısız olmamalıdır (test edilen birimi yalıtdığımız için).

``` csharp
    [TestClass]
    public class EmployeeControllerCreateActionPostTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldAddNewEmployeeToRepository() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_repository.Contains(_newEmployee));
        }
        [TestMethod]
        public void ShouldCommitUnitOfWork() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_unitOfWork.Committed);
        }
        // ... more tests

        Employee _newEmployee = new Employee() {
            Name = "NEW EMPLOYEE",
            HireDate = new System.DateTime(2010, 1, 1)
        };
    }
```

Bu testlerde, temel sınıf kurulum işinin çoğunu yapar. Temel sınıf oluşturucunun bellek içi depoyu, sahte bir iş birimini ve EmployeeController sınıfının bir örneğini oluşturduğunu unutmayın. Test sınıfı bu temel sınıftan türetilir ve Create metodunu test etme özelliklerine odaklanır. Bu durumda, Ayrıntılar herhangi bir birim testi yordamında göreceğiniz "düzenleme, işlem ve onaylama" adımlarına sahiptir:

-   Gelen verilerin benzetimini yapmak için bir Yeniçalışan nesnesi oluşturun.
-   EmployeeController 'ın oluşturma eylemini çağırın ve Yeniçalışan içinde geçiş yapın.
-   Oluşturma eyleminin beklenen sonuçları (çalışanın depoda göründüğünü) ürettiğini doğrulayın.

Oluşturduğumuz, EmployeeController eylemlerini test etmemizi sağlar. Örneğin, testler için aynı temel kurulumu oluşturmak üzere test temel sınıfından kalıtımla aldığımız çalışan denetleyicinin Dizin eylemi için testler yazdığımızda. Temel sınıf, bellek içi depoyu, sahte iş birimini ve EmployeeController örneğini oluşturacaktır. Dizin eyleminin testleri yalnızca dizin eylemini çağırmaya ve eylemin döndürdüğü modelin kalitelerini test etmeye odaklanmalıdır.

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count);
        }
        [TestMethod]
        public void ShouldOrderModelByHiredateAscending() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                         as IEnumerable<Employee>;
            Assert.IsTrue(model.SequenceEqual(
                           _employeeData.OrderBy(e => e.HireDate)));
        }
        // ...
    }
```

Bellek içi Fakes ile oluşturmakta olduğumuz testler, yazılımın *durumunun* test edilmesine yönelik olarak tasarlanmıştır. Örneğin, oluşturma eylemini sınarken, oluşturma eylemi yürütüldükten sonra deponun durumunu incelemek istiyoruz. depo yeni çalışanı mi tutar?

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

Daha sonra etkileşim tabanlı teste bakacağız. Etkileşim tabanlı test, test altındaki kodun nesnelerimiz üzerinde doğru yöntemleri çağırırmasını ve doğru parametreleri geçtiğini ister. Şimdilik, başka bir tasarım düzenine (yavaş yük) geçeceğiz.

## <a name="eager-loading-and-lazy-loading"></a>Eager yükleme ve geç yükleme

ASP.NET MVC web uygulamasında bir noktada, bir çalışanın bilgilerini göstermek ve çalışanın ilişkili zaman kartlarını eklemek isteyebilirler. Örneğin, çalışanın adını ve sistemdeki toplam zaman kartı sayısını gösteren bir zaman kartı Özet görüntüsü olabilir. Bu özelliği uygulamak için birkaç yaklaşım olabilir.

### <a name="projection"></a>Projeksiyon

Özet oluşturmaya yönelik kolay bir yaklaşım, görünümde görüntülenmesini istediğimiz bilgilere adanmış bir model oluşturmaktır. Bu senaryoda, model aşağıdaki gibi görünebilir.

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

EmployeeSummaryViewModel bir varlık değil, başka bir deyişle veritabanında kalıcı hale getirmek istediğimiz bir şey değildir. Bu sınıfı yalnızca, verileri kesin belirlenmiş bir şekilde görünüme karıştırmak için kullanacağız. Görünüm modeli bir veri aktarımı nesnesi (DTO) gibi bir davranış (yöntem yok) içerdiğinden, yalnızca özellikler içeriyor. Özellikler, taşınması gereken verileri tutacağız. LINQ 'ın standart projeksiyon işlecini (Select işleci) kullanarak bu görünüm modelini oluşturmak kolaydır.

``` csharp
    public ViewResult Summary(int id) {
        var model = _unitOfWork.Employees
                               .Where(e => e.Id == id)
                               .Select(e => new EmployeeSummaryViewModel
                                  {
                                    Name = e.Name,
                                    TotalTimeCards = e.TimeCards.Count()
                                  })
                               .Single();
        return View(model);
    }
```

Yukarıdaki koda yönelik iki önemli özellik vardır. İlk: daha kolay bir şekilde izlemek ve yalıtmak için kodun test edilmesi kolaydır. Select işleci, gerçek iş biriminde olduğu gibi bellek içi faflarımızla aynı zamanda çalışır.

``` csharp
    [TestClass]
    public class EmployeeControllerSummaryActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithCorrectEmployeeSummary() {
            var id = 1;
            var result = _controller.Summary(id);
            var model = result.ViewData.Model as EmployeeSummaryViewModel;
            Assert.IsTrue(model.TotalTimeCards == 3);
        }
        // ...
    }
```

İkinci önemli özelliği, kodun çalışanların ve zaman kartı bilgilerini bir araya getirmek için tek ve verimli bir sorgu oluşturmasına izin verir. Çalışan bilgilerini ve zaman kartı bilgilerini özel API 'Ler kullanmadan aynı nesneye yükledik. Kod, yalnızca, bellek içi veri kaynaklarına ve uzak veri kaynaklarına karşı çalışan standart LINQ işleçleri kullanmak için gereken bilgileri ifade ediyor. EF4, LINQ sorgusu ve C\# derleyicisi tarafından oluşturulan ifade ağaçlarını tek ve verimli bir T-SQL sorgusuna çevirebildi.

``` SQL
    SELECT
    [Limit1].[Id] AS [Id],
    [Limit1].[Name] AS [Name],
    [Limit1].[C1] AS [C1]
    FROM (SELECT TOP (2)
      [Project1].[Id] AS [Id],
      [Project1].[Name] AS [Name],
      [Project1].[C1] AS [C1]
      FROM (SELECT
        [Extent1].[Id] AS [Id],
        [Extent1].[Name] AS [Name],
        (SELECT COUNT(1) AS [A1]
         FROM [dbo].[TimeCards] AS [Extent2]
         WHERE [Extent1].[Id] =
               [Extent2].[EmployeeTimeCard_TimeCard_Id]) AS [C1]
              FROM [dbo].[Employees] AS [Extent1]
               WHERE [Extent1].[Id] = @p__linq__0
         )  AS [Project1]
    )  AS [Limit1]
```

Bir görünüm modeliyle veya DTO nesnesiyle çalışmak istemediğimiz zaman, ancak gerçek varlıklarla, başka zamanlar da vardır. Bir çalışana *ve* çalışanların zaman kartlarına ihtiyacım olduğunu öğrendiğimiz zaman, ilgili verileri engellemeyen ve verimli bir şekilde yükleyebiliriz.

### <a name="explicit-eager-loading"></a>Açık Eager yüklemesi

İlgili varlık bilgilerini daha fazla yüklemek istediğimiz için, iş mantığı için bazı mekanizmaları (veya bu senaryoda, denetleyici eylem mantığı), isteğini depoya ifade etmek istiyoruz. EF4 ObjectQuery&lt;T&gt; sınıfı, bir sorgu sırasında alınacak ilgili nesneleri belirtmek için bir Include yöntemi tanımlar. EF4 ObjectContext 'in, ObjectQuery&lt;T&gt;devralan somut ObjectSet&lt;T&gt; sınıfı aracılığıyla varlıkları açığa çıkardığı unutulmamalıdır.  Denetleyici eylemimizde ObjectSet&lt;T&gt; başvurularını kullandığımızda, her çalışana yönelik bir zaman kartı bilgileri yüklemesi belirtmek için aşağıdaki kodu yazabilirsiniz.

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

Ancak, kod test etmemiz yaptığımız için, gerçek iş sınıfının dışından ObjectSet&lt;T&gt; kullanıma sunmuyoruz. Bunun yerine, sahte kümesi&lt;T&gt; arabirimine güveniyoruz, ancak IObjectSet&lt;T&gt; bir Içerme yöntemi tanımlamaz. LINQ 'ın, kendi ekleme işleçlerimizi oluşturduğumuz.

``` csharp
    public static class QueryableExtensions {
        public static IQueryable<T> Include<T>
                (this IQueryable<T> sequence, string path) {
            var objectQuery = sequence as ObjectQuery<T>;
            if(objectQuery != null)
            {
                return objectQuery.Include(path);
            }
            return sequence;
        }
    }
```

Bu Include işlecinin, IObjectSet&lt;T&gt;yerine IQueryable&lt;T&gt; için bir genişletme yöntemi olarak tanımlandığını unutmayın. Bu, yöntemi IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;ve ObjectSet&lt;T&gt;dahil olmak üzere daha geniş bir dizi olası tür ile kullanma olanağı sunar. Temeldeki sıra, gerçek bir EF4 ObjectQuery&lt;T&gt;olmadığından, bir sorun yoktur ve Içerme işleci hiçbir şey değildir. Temeldeki sıra bir ObjectQuery&lt;T&gt; (ya da ObjectQuery&lt;T&gt;) ise, EF4 ek veriler için gereksinimimizi görür ve uygun SQL sorgusunu *formüllendiriyor* .

Bu yeni işleçle birlikte, depodan bir zaman kartı bilgileri yüklemesi isteğinde bulunabilir.

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Gerçek bir ObjectContext 'e karşı çalıştırıldığında, kod aşağıdaki tek sorguyu oluşturur. Sorgu, çalışan nesnelerini bir yolculuğa alacak ve zaman kartları özelliğini tamamen dolduracak şekilde veritabanından yeterli bilgi toplar.

``` SQL
    SELECT
    [Project1].[Id] AS [Id],
    [Project1].[Name] AS [Name],
    [Project1].[HireDate] AS [HireDate],
    [Project1].[C1] AS [C1],
    [Project1].[Id1] AS [Id1],
    [Project1].[Hours] AS [Hours],
    [Project1].[EffectiveDate] AS [EffectiveDate],
    [Project1].[EmployeeTimeCard_TimeCard_Id] AS [EmployeeTimeCard_TimeCard_Id]
    FROM ( SELECT
         [Extent1].[Id] AS [Id],
         [Extent1].[Name] AS [Name],
         [Extent1].[HireDate] AS [HireDate],
         [Extent2].[Id] AS [Id1],
         [Extent2].[Hours] AS [Hours],
         [Extent2].[EffectiveDate] AS [EffectiveDate],
         [Extent2].[EmployeeTimeCard_TimeCard_Id] AS
                    [EmployeeTimeCard_TimeCard_Id],
         CASE WHEN ([Extent2].[Id] IS NULL) THEN CAST(NULL AS int)
         ELSE 1 END AS [C1]
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

Harika haberler, eylem yöntemi içindeki koddur tamamen test edilebilir olarak kalır. Ekleme işlecini desteklemek için Fakes 'e yönelik ek özellikler sağlamamız gerekmez. Kötü haberler, kalıcılık Ignorant 'i tutmak istediğimiz kodun içinde Include işlecini kullandık. Bu, test edilebilir kod oluştururken değerlendirmeniz gereken dengetür bir örnektir. Kalıcı kaygıların, performans hedeflerini karşılamak için depo soyutlaması dışında sızmasına izin vermek için gereken durumlar vardır.

Eager yükleme alternatifi geç yükleme ' dir. Yavaş yükleme, iş kodumuza ilişkili verilerin gereksinimini açıkça duyurmak için ihtiyaç *duymadığımız* anlamına gelir. Bunun yerine, uygulama içinde varlıklarımızı kullanıyoruz ve ek veriler gerekliyse Entity Framework verileri isteğe bağlı olarak yükler.

### <a name="lazy-loading"></a>Geç yükleme

Bir iş mantığı parçasının hangi verileri suneceğimizi bilmeyen bir senaryoya çok daha kolay bir şekilde düşünün. Mantığın bir çalışan nesnesine ihtiyacı olduğunu biliyoruz, ancak bu yollardan bazılarının çalışanla ilgili zaman kartı bilgileri gerektirmesi ve bazıları olmaması durumunda farklı yürütme yollarına dallanbiliriz. Bu gibi senaryolar, verilerin genel olarak gerekli bir şekilde göründüğünden, örtük yavaş yükleme için mükemmeldir.

Ertelenmiş yükleme olarak da bilinen yavaş yükleme, bazı gereksinimleri varlık nesnelerimize yerleştirir. Doğru Kalıcılık kalıcılığı olan POCOs, kalıcılık katmanından herhangi bir gereksinim sağlamaz, ancak gerçek Kalıcılık Ignorance 'in elde etmek neredeyse olanaksız hale geliyordu.  Bunun yerine, kalıcılık Ignorance ' i göreli derece ölçyoruz. Bir kalıcılık odaklı taban sınıftan devralması gerekiyorsa veya POCOs 'ta geç yükleme elde etmek için özel bir koleksiyon kullanmanız talihsiz. Neyse ki, EF4 daha az zorlayıcıdır bir çözüme sahiptir.

### <a name="virtually-undetectable"></a>Neredeyse algılanamaz

POCO nesneleri kullanılırken EF4, varlıklar için çalışma zamanı proxy 'leri dinamik olarak oluşturabilir. Bu proxy 'ler, gerçekleştirilmiş POCOs ' i daha fazla sarın ve her bir özellik al ve ayarla işlemini ek iş gerçekleştirecek şekilde kesintiye getirerek ek hizmetler sağlar. Bu tür bir hizmet, ardığımız yavaş yükleme özelliğidir. Başka bir hizmet, program bir varlığın özellik değerlerini değiştirdiğinde kaydedebilen etkili bir değişiklik izleme mekanizmasıdır. Değişiklik listesi, GÜNCELLEŞTIRME komutlarını kullanarak değiştirilen varlıkların kalıcı hale getirilmesi için SaveChanges yöntemi sırasında ObjectContext tarafından kullanılır.

Bununla birlikte, bu proxy 'lerin çalışması için bir varlık üzerinde özellik al ve ayarla işlemlerine bağlamak için bir yol gerekir ve proxy 'ler sanal üyeleri geçersiz kılarak bu amaca ulaşırlar. Bu nedenle, örtük yavaş yükleme ve etkili değişiklik izleme sağlamak istiyorsam, POCO sınıfı tanımlarımıza dönmemiz ve özellikleri sanal olarak işaretlemesi gerekir.

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

Yine de çalışan varlığının, genellikle Kalıcılık Ignorant olduğunu varsayalım. Tek gereksinim, sanal üyelerin kullanılması ve bu kodun test edilebilirlik etkilemez. Herhangi bir özel taban sınıftan türetmemiz gerekmez, hatta yavaş yüklemeye adanmış özel bir koleksiyon kullanın. Kodun gösterdiği gibi, ICollection&lt;T&gt; uygulayan tüm sınıflar ilgili varlıkları tutmak için kullanılabilir.

İş birimimizin içinde yapabilmemiz gereken bir küçük değişiklik de vardır. Doğrudan bir ObjectContext nesnesiyle çalışırken geç yükleme varsayılan olarak *kapalıdır* . Bu özelliği, ertelenmiş yüklemeyi etkinleştirmek için ContextOptions özelliğinde ayarlayabiliriz ve her yerde yavaş yüklemeyi etkinleştirmek istiyorsam gerçek iş birimimizin içinde ayarlayabiliriz.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
         public SqlUnitOfWork() {
             // ...
             _context = new ObjectContext(connectionString);
             _context.ContextOptions.LazyLoadingEnabled = true;
         }    
         // ...
     }
```

Örtük yavaş yükleme etkin olduğunda, uygulama kodu bir çalışanı ve çalışanın ilişkili zaman kartlarını kullanarak ek verileri yüklemek için gerekli olan işin kalan çalışmalarından haberdar olabilir.

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

Yavaş yükleme, uygulama kodunu yazmayı daha kolay hale getirir ve proxy Magic ile kod tamamen test edilebilir kalır. Çalışma birimi için bellek içi Fakes, bir test sırasında gerektiğinde ilgili verileri içeren sahte varlıkları önyükleyebilir.

Bu noktada, IObjectSet&lt;T&gt; kullanarak depo oluşturma konusunda ilgilenmeniz ve kalıcılık çerçevesinin tüm işaretlerini gizlemek için soyutlamaları göz atacağız.

## <a name="custom-repositories"></a>Özel depolar

Bu makaledeki iş birimi tasarım deseninin ilk olarak sunulduğunu, iş biriminin nasıl görünebileceğini görmek için bazı örnek kodlar sağladık. Bu özgün fikri, birlikte çalıştığımız çalışan ve çalışan zaman kartı senaryosunu kullanarak yeniden sunalım.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

Bu iş birimi ile son bölümde oluşturduğumuz iş birimi arasındaki birincil fark, bu iş biriminin EF4 çerçevesinden herhangi bir soyutlama kullanmadığında (bir IObjectSet&lt;T&gt;). IObjectSet&lt;T&gt; bir depo arabirimi olarak iyi çalışmaktadır, ancak açığa çıkardığı API, uygulama gereksinimlerimize uygun şekilde hizalanmayabilir. Bu yaklaşan yaklaşımda, özel bir ırepository&lt;T&gt; soyutlama kullanan depoları temsil edeceğiz.

Test odaklı tasarım, davranış odaklı tasarım ve etki alanı odaklı Yöntemler tasarımını izleyen birçok geliştirici, bazı nedenlerle ırepository&lt;T&gt; yaklaşımını tercih eder. İlk olarak, ırepository&lt;T&gt; arabirimi bir "bozulma önleme" katmanını temsil eder. Etki alanı odaklı tasarım defterindeki Eric Evans 'Lar tarafından açıklandığı gibi, bir kalıcılık API 'SI gibi, etki alanı kodunuzu altyapı API 'Lerinden uzakta tutar. İkincisi, geliştiriciler, bir uygulamanın tam ihtiyaçlarını karşılayan (testleri yazarken bulunur), depoya Yöntemler oluşturabilir. Örneğin, genellikle bir KIMLIK değeri kullanarak tek bir varlığı bulduğumuz için, depo arabirimine bir Findbyıd yöntemi ekleyebiliriz.  Irepository&lt;T&gt; tanımımız aşağıdaki gibi görünür.

``` csharp
    public interface IRepository<T>
                    where T : class, IEntity {
        IQueryable<T> FindAll();
        IQueryable<T> FindWhere(Expression<Func\<T, bool>> predicate);
        T FindById(int id);       
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Varlık koleksiyonlarını göstermek için IQueryable&lt;T&gt; arabirimini kullanmaya geri başlayacağız. IQueryable&lt;T&gt;, LINQ Expression ağaçlarının EF4 sağlayıcısına akmasını ve sağlayıcıya sorgunun bütünsel görünümünü vermesini sağlar. İkinci bir seçenek IEnumerable&lt;T&gt;döndürmek, bu da EF4 LINQ sağlayıcının yalnızca deponun içinde yerleşik olan ifadeleri göremeyeceği anlamına gelir. Deponun dışında yapılan herhangi bir gruplandırma, sıralama ve projeksiyon, veritabanına gönderilen SQL komutuna uygulanmaz ve bu da performansa zarar verebilir. Diğer taraftan, yalnızca IEnumerable&lt;T&gt; sonuçları döndüren bir depo, yeni bir SQL komutu ile sizi hiçbir şekilde hiçbir şekilde hiçbir şekilde sürmez. Her iki yaklaşım da çalışacaktır ve her iki yaklaşım da testable olarak kalır.

Genel türler ve EF4 ObjectContext API 'SI kullanılarak ırepository&lt;T&gt; arabiriminin tek bir uygulamasını sağlamak basittir.

``` csharp
    public class SqlRepository<T> : IRepository<T>
                                    where T : class, IEntity {
        public SqlRepository(ObjectContext context) {
            _objectSet = context.CreateObjectSet<T>();
        }
        public IQueryable<T> FindAll() {
            return _objectSet;
        }
        public IQueryable<T> FindWhere(
                               Expression<Func\<T, bool>> predicate) {
            return _objectSet.Where(predicate);
        }
        public T FindById(int id) {
            return _objectSet.Single(o => o.Id == id);
        }
        public void Add(T newEntity) {
            _objectSet.AddObject(newEntity);
        }
        public void Remove(T entity) {
            _objectSet.DeleteObject(entity);
        }
        protected ObjectSet<T> _objectSet;
    }
```

Irepository&lt;T&gt; yaklaşımı, bir istemcinin bir varlığa ulaşmak için bir yöntem çağırması gerektiğinden sorgularımızda bazı ek denetimler elde etmenizi sağlar. Yöntemi içinde, uygulama kısıtlamalarını zorlamak için ek denetimler ve LINQ işleçleri sağlayabiliriz. Arabirimin genel tür parametresinde iki kısıtlama olduğunu fark edin. İlk kısıtlama, ObjectSet&lt;T&gt;için gerekli olan bir sınıftır ve ikinci kısıtlama varlıklarımızı uygulama için oluşturulan bir soyutlama olan IEntity 'ı uygulayacak şekilde zorlar. IEntity arabirimi, varlıkların okunabilir bir ID özelliğine sahip olmasını zorlar ve daha sonra bu özelliği Findbyıd yönteminde kullanabiliriz. IEntity aşağıdaki kodla tanımlanır.

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

Varlıklarımızın bu arabirimi uygulaması gerektiğinden, IEntity küçük bir kalıcılığı ihlal edilebilir olarak düşünülebilir. Kalıcılık Ignorance 'in, bir denge hakkında olduğunu ve birçok Findbyıd işlevinin, arabirim tarafından uygulanan kısıtlamayı aşacak şekilde olacağını unutmayın. Arabirimin test edilebilirlik üzerinde hiçbir etkisi yoktur.

Canlı bir ırepository 'nin örneklenmesi&lt;T&gt; bir EF4 ObjectContext gerektirir, bu nedenle somut bir iş uygulaması biriminin örnek oluşturmayı yönetmesi gerekir.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;

            _context = new ObjectContext(connectionString);
            _context.ContextOptions.LazyLoadingEnabled = true;
        }

        public IRepository<Employee> Employees {
            get {
                if (_employees == null) {
                    _employees = new SqlRepository<Employee>(_context);
                }
                return _employees;
            }
        }

        public IRepository<TimeCard> TimeCards {
            get {
                if (_timeCards == null) {
                    _timeCards = new SqlRepository<TimeCard>(_context);
                }
                return _timeCards;
            }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        SqlRepository<Employee> _employees = null;
        SqlRepository<TimeCard> _timeCards = null;
        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

### <a name="using-the-custom-repository"></a>Özel depoyu kullanma

Özel havuzumuzu kullanmak, IObjectSet&lt;T&gt;tabanlı depoyu kullanmaktan önemli ölçüde farklı değildir. LINQ işleçlerini doğrudan bir özelliğine uygulamak yerine, önce bir IQueryable&lt;T&gt; başvurusunu almak için bir deponun yöntemlerini çağırmanız gerekir.

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Daha önce uyguladığımız özel Içerme işlecinin değişiklik olmadan çalışabileceğini unutmayın. Deponun Findbyıd yöntemi, tek bir varlığı almaya çalışan eylemlerden yinelenen mantığı kaldırır.

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

İncelediğimiz iki yaklaşımın test edilebilirlik açısından önemli bir fark yoktur. HashSet&lt;çalışan&gt; tarafından desteklenen somut sınıflar oluşturarak (son bölümde yaptığımız gibi) ırepository&lt;T&gt; sahte uygulamalar sağlayabiliriz. Ancak bazı geliştiriciler, Fakes oluşturmak yerine, sahte nesneler ve sahte nesne çerçeveleri kullanmayı tercih eder. Uygulamamızı test etmek ve sonraki bölümde yer aldığı ve bu farklılıkları ve Fakes arasındaki farkları tartışmak için de moyaları kullanma bölümüne bakacağız.

### <a name="testing-with-mocks"></a>Moklarla test etme

Tek Marwler 'in bir "test Double" araması için farklı yaklaşımlar vardır. Bir test Double (bir film takip eden çift gibi), testler sırasında gerçek, üretim nesneleri için "Stand" olarak oluşturduğunuz bir nesnedir. Oluşturduğumuz bellek içi depolar, SQL Server ile iletişim kuran depolar için Double değerleri test ediyor. Kodu yalıtmak ve testleri hızlı bir şekilde çalıştırmak için birim testlerinde bu testlerin nasıl çift olarak kullanılacağını gördük.

Test, geliştirdiğimiz gerçek, çalışma uygulamalarına sahiptir. Arka planda her biri, bir dizi somut nesneleri depolar ve bir test sırasında depoyu işlememiz halinde bu koleksiyondan nesne ekler ve kaldırır. Bu şekilde test oluşturmak gibi bazı geliştiriciler, gerçek kod ve çalışma uygulamalarıyla birlikte bu şekilde iki katına çıkarır.  Bu test, *Fakes*'i çağırdığımız şeydir. Bunlarla çalışan uygulamalar vardır, ancak üretim kullanımı için yeterince gerçek değildir. Sahte depo aslında veritabanına yazmıyor. Sahte SMTP sunucusu aslında ağ üzerinden e-posta iletisi göndermez.

### <a name="mocks-versus-fakes"></a>Moları ve Fakes karşılaştırması

*Sahte*olarak bilinen başka bir test türü vardır. Fakes çalışma uygulamalarına sahip olsa da, moları hiçbir uygulamayla birlikte gelir. Bir sahte nesne çerçevesinin yardımıyla, bu sahte nesneleri çalışma zamanında oluşturur ve bunları test Double olarak kullanır. Bu bölümde, açık kaynak sahte işlem Framework moq ' ını kullanacağız. Bir çalışan deposu için dinamik olarak bir test Double oluşturmak üzere moq kullanmanın basit bir örneği aşağıda verilmiştir.

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

Bir ırepository&lt;çalışan&gt; uygulaması için moq soruyoruz ve dinamik olarak bir tane oluşturur. Irepository&lt;çalışan nesneye,&gt;, sahte&lt;T&gt; nesnesinin nesne özelliğine erişerek ulaşacağız. Denetleyicilerimize geçebilmemiz için bu iç nesne budur ve bu, bir test Double veya gerçek depo olduğunu bilmez. Nesneleri, gerçek bir uygulamayla bir nesne üzerinde çağırırız gibi, nesne üzerinde Yöntemler çağırabiliriz.

Add metodunu çağırdığımızda, sahte deponun ne yapacağına merak etmeniz gerekir. Sahte nesnenin arkasında hiçbir uygulama olmadığından, ekleme işlemi hiçbir şey yapmaz. Yazdığımız Fakes gibi sahnelerin arkasında somut bir koleksiyon yoktur, bu nedenle çalışan atılır. Findbyıd 'nin dönüş değeri ne? Bu durumda, sahte nesne yalnızca yapabileceği şeyi yapar; bu, varsayılan bir değer döndürür. Bir başvuru türü (bir çalışan) döndürtiğimiz için, dönüş değeri null bir değerdir.

Kneztal, daha az ses alabilir; Bununla birlikte, bu konuda daha fazla bilgi edindiğimiz iki ek özellik vardır. İlk olarak, moq çerçevesi, sahte nesne üzerinde yapılan tüm çağrıları kaydeder. Kodun ilerleyen kısımlarında, Add yöntemini veya herkes Findbyıd yöntemini çağırırsam moq 'a sorun. Daha sonra, bu "kara kutu" kayıt özelliğini testlerde nasıl kullanacağımız hakkında daha fazla bilgi edineceksiniz.

İkinci harika özellik, bir sahte nesneyi *beklentilerle*programlamak Için moq 'yi nasıl kullanabiliriz. Bir beklenti, sahte nesnesine verilen etkileşime nasıl yanıt verileceğini söyler. Örneğin, bir beklentisini bir beklenemizi programlarız ve birisi Findbyıd 'yi çağırdığında bir çalışan nesnesi döndürmesini söyleriz. Moq çerçevesi, bu beklentileri programlamak için bir kurulum API 'SI ve lambda ifadeleri kullanır.

``` csharp
    [TestMethod]
    public void MockSample() {
        Mock<IRepository<Employee>> mock =
            new Mock<IRepository<Employee>>();
        mock.Setup(m => m.FindById(5))
            .Returns(new Employee {Id = 5});
        IRepository<Employee> repository = mock.Object;
        var employee = repository.FindById(5);
        Assert.IsTrue(employee.Id == 5);
    }
```

Bu örnekte, moq 'ın bir depoyu dinamik olarak oluşturmasını ve sonra depoyu bir beklentisiyle programlıyoruz. Bu beklentiler, bir Kullanıcı Findbyıd metodunu 5 değerini geçirerek bir kimlik değeri 5 olan yeni bir çalışan nesnesi döndürmesini söyler. Bu test geçirilir ve sahte ırepository&lt;T&gt;için tam bir uygulama oluşturmamız gerekmiyor.

Daha önce yazdığımız testleri tekrar ziyaret edelim ve bunları Fakes yerine kları kullanacak şekilde yeniden ekleyeceğiz. Daha önce olduğu gibi, denetleyicinin tüm testleri için ihtiyacımız olan yaygın altyapı parçalarını ayarlamak için bir temel sınıf kullanacağız.

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .AsQueryable();
            _repository = new Mock<IRepository<Employee>>();
            _unitOfWork = new Mock<IUnitOfWork>();
            _unitOfWork.Setup(u => u.Employees)
                       .Returns(_repository.Object);
            _controller = new EmployeeController(_unitOfWork.Object);
        }

        protected IQueryable<Employee> _employeeData;
        protected Mock<IUnitOfWork> _unitOfWork;
        protected EmployeeController _controller;
        protected Mock<IRepository<Employee>> _repository;
    }
```

Kurulum kodu çoğunlukla aynı kalır. Fakes 'i kullanmak yerine, model nesneleri oluşturmak için moq kullanacağız. Temel sınıf, kod çalışanlar özelliğini çağırdığında bir sahte depo döndürecek şekilde, sahte iş birimi için düzenler. Sahte kurulumun geri kalanı, her bir senaryoya ayrılan test armatürleri içinde gerçekleşmeyecektir. Örneğin, Dizin eyleminin test armatürü, bir işlem, sahte deponun FindAll yöntemini çağırdığında çalışanların bir listesini döndürecek şekilde ayarlar.

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        public EmployeeControllerIndexActionTests() {
            _repository.Setup(r => r.FindAll())
                        .Returns(_employeeData);
        }
        // .. tests
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count());
        }
        // .. and more tests
    }
```

Beklentiler dışında, sınamalarımız, daha önce yaptığımız testlere benzer şekilde görünür. Bununla birlikte, bir sahte çerçevenin kayıt yeteneği sayesinde, farklı bir açıdan teste yaklaşımda olabilir. Sonraki bölümde bu yeni perspektife bakacağız.

### <a name="state-versus-interaction-testing"></a>Eyalet ve etkileşim testi

Sahte nesnelerle yazılım test etmek için kullanabileceğiniz farklı teknikler vardır. Bir yaklaşım, bu kağıda şimdiye kadar yaptığımız durum tabanlı test kullanmaktır. Durum tabanlı test, yazılımın durumu hakkında Onaylamalar yapar. Son testte, denetleyicide bir eylem yöntemi çağırdı ve oluşturması gereken model hakkında bir onay yaptı. Test durumunun diğer örnekleri aşağıda verilmiştir:

-   Oluşturma yürütüldükten sonra deponun yeni çalışan nesnesini içerdiğini doğrulayın.
-   Modelin Dizin yürütüldükten sonra tüm çalışanların bir listesini tuttuğundan emin olun.
-   Deponun, silme yürütüldükten sonra belirli bir çalışan içermediğini doğrulayın.

Diğer bir yaklaşım de, *etkileşimlerin*doğrulanmak üzere, sahte nesneler ile göreceğiniz bir yaklaşım. Durum tabanlı test, nesnelerin durumu hakkında onaylamaları yaptığında, etkileşim tabanlı test, nesnelerin nasıl etkileşime gireceğini onaylar. Örneğin:

-   Oluşturma yürütüldüğünde, denetleyicinin deponun Add metodunu çağırdığından emin olun.
-   Dizin yürütüldüğünde denetleyicinin deponun FindAll yöntemini çağırdığından emin olun.
-   Denetleyicinin, düzenleme yürütüldüğünde değişiklikleri kaydetmek için iş birimi yürütme yöntemini çağırdığından emin olun.

Etkileşim testi genellikle daha az test verisi gerektirir, çünkü koleksiyonlar içinde hiç bir göz aşmadığımyız ve sayıları doğrulıyoruz. Örneğin, Ayrıntılar eyleminin, doğru değere sahip bir deponun Findbyıd metodunu çağırdığından emin olduysa, eylem muhtemelen doğru şekilde davranmaktadır. Bu davranışı, Findbyıd 'den döndürülecek hiçbir test verisi ayarlamadan doğrulayabiliriz.

``` csharp
    [TestClass]
    public class EmployeeControllerDetailsActionTests
               : EmployeeControllerTestBase {
         // ...
        [TestMethod]
        public void ShouldInvokeRepositoryToFindEmployee() {
            var result = _controller.Details(_detailsId);
            _repository.Verify(r => r.FindById(_detailsId));
        }
        int _detailsId = 1;
    }
```

Yukarıdaki test armaöğesinde gereken tek kurulum, temel sınıf tarafından sunulan kurulumdır. Denetleyici eylemini çağırdığınızda MOQ, etkileşimleri sahte depoyla kaydeder. Moq doğrulama API 'sini kullanarak, denetleyici Findbyıd 'yi uygun KIMLIK değeri ile çağırırdı. Denetleyici yöntemi çağırmadı veya yöntemi beklenmeyen bir parametre değeriyle çağırırdı, Verify yöntemi bir özel durum oluşturur ve test başarısız olur.

İşte, geçerli iş birimi üzerinde yürütmeyi çağıran oluşturma eylemini doğrulamaya yönelik başka bir örnek.

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

Etkileşim testi olan bir olma tehlikesi, etkileşimlerin belirtilmesinden çok daha fazla. Sahte nesne ile her etkileşimi kaydetme ve doğrulama özelliği, testin her etkileşimi doğrulamaya çalışır. Bazı etkileşimler uygulama ayrıntılarıdır ve yalnızca geçerli testi karşılamak için *gerekli* olan etkileşimleri doğrulamanız gerekir.

Körler veya Fakes arasındaki seçim büyük ölçüde test ettiğiniz sisteme ve kişisel (veya ekip) tercihlerinize bağlıdır. Sahte nesneler, test Double değerlerini uygulamak için gereken kod miktarını büyük ölçüde azaltabilir, ancak herkes, beklentileri ve etkileşimleri doğrulamakta değildir.

## <a name="conclusions"></a>Sonuçlar

Bu yazıda, veri kalıcılığı için ADO.NET Entity Framework kullanırken, test edilebilir kod oluşturmak için çeşitli yaklaşımlar yaptık. IObjectSet&lt;T&gt;gibi yerleşik soyutlamalar kullanabilir veya ırepository&lt;T&gt;gibi kendi soyutlamalarını oluşturabilirsiniz.  Her iki durumda da, ADO.NET Entity Framework 4,0 ' deki POCO desteği, bu soyutlamalar tüketicilerinin kalıcı olarak Ignorant ve yüksek oranda bir şekilde kalmasına izin verir. Örtük yavaş yükleme gibi ek EF4 özellikleri, iş ve uygulama hizmeti kodunun, ilişkisel bir veri deposunun ayrıntıları konusunda endişelenmeden çalışmasına izin verir. Son olarak, oluşturduğumuz soyutlamalar birim testlerinin içinde anlamlı veya taklit edilebilir. bu test Double değerlerini kullanarak hızlı çalışan, yüksek oranda yalıtılmış ve güvenilir testler elde edebilirsiniz.

### <a name="additional-resources"></a>Ek Kaynaklar

-   Robert C. MARI, " [tek sorumluluk ilkesi](https://www.objectmentor.com/resources/articles/srp.pdf)"
-   Marwler, *Kurumsal uygulama mimarisi desenlerinden* [desenler kataloğu](https://www.martinfowler.com/eaaCatalog/index.html)
-   Griffin Caprio, " [bağımlılık ekleme](https://msdn.microsoft.com/magazine/cc163739.aspx)"
-   Veri programlama blogu, " [Izlenecek yol: Entity Framework 4,0 Ile test odaklı geliştirme](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".
-   Veri programlama blogu, " [Entity Framework 4,0 Ile depo ve Iş birimi desenleri kullanma](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"
-   Aaron Jensen, " [makine belirtimlerini tanıtma](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"
-   Eric eser, " [MSTest Ile BDD](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"
-   Eric Evans, " [etki alanı odaklı tasarım](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"
-   Marwler, "her bir yer [tutucular](https://martinfowler.com/articles/mocksArentStubs.html)yok"
-   Marwler, " [Test Double](https://martinfowler.com/bliki/TestDouble.html)"
-   [Moq dili](https://code.google.com/p/moq/)

### <a name="biography"></a>Biyografi

Scott Allen, Plurali ve OdeToCode.com 'in en altında bulunan teknik personelin bir üyesidir. 15 yıllık ticari yazılım geliştirme sürecinde Scott, 8 bit ekli cihazlardan her şeyin çözüm üzerinde, yüksek düzeyde ölçeklenebilir ASP.NET Web uygulamalarına kadar bir süredir çalıştık. Scott 'a OdeToCode konumundaki blogda veya [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode)adresinden Twitter üzerinden ulaşabilirsiniz.
