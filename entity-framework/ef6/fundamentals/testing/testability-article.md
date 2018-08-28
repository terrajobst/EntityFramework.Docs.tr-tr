---
title: Test Edilebilirlik ve Entity Framework 4.0
author: divega
ms.date: 2016-10-23
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 17a9f09022531a81042979464de05fbbd2570759
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995235"
---
# <a name="testability-and-entity-framework-40"></a>Test Edilebilirlik ve Entity Framework 4.0
Scott Allen

Yayımlanma: Mayıs 2010

## <a name="introduction"></a>Giriş

Bu teknik incelemede, açıklamakta ve ADO.NET Entity Framework 4.0 ve Visual Studio 2010 test edilebilir kodunun nasıl yazılacağını göstermektedir. Bu yazıda, test Odaklı Tasarım (TDD) ya da davranışı Odaklı Tasarım (BDD) gibi belirli bir test yöntemi odaklanmak denemez. Bunun yerine bu yazıda, ADO.NET varlık çerçevesi kullanır ancak kolay yalıtmak ve otomatik bir şekilde test kalır kodunun nasıl yazılacağını üzerinde odaklanır. Veri erişimi senaryoları test kolaylaştırmak ve framework kullanılırken bu desenleri uygulamak nasıl sık karşılaşılan tasarım desenleri inceleyeceğiz. Ayrıca bu özellikleri test edilebilir kod içinde nasıl çalışabileceğini görmek için framework'ün belirli özellikleri inceleyeceğiz.

## <a name="what-is-testable-code"></a>Test edilebilir kod nedir?

Otomatik birim testleriyle bir yazılım parçasıdır doğrulama yeteneği arzu birçok avantaj sunar. Herkesin iyi testler yazılım kusurlarını bir uygulama ve uygulamanın kalite - birim testleri sahip olmanın ancak şu ana kadar yalnızca hataları bulmaya ötesinde gider artış sayısını azaltacak bilir.

Zamandan tasarruf edin ve oluşturdukları yazılım denetiminde kalmak bir geliştirme ekibi iyi birim test paketi sağlar. Takım değişiklik yapabilmeniz için var olan kod, yeniden düzenleme, tasarlanması ve yeni gereksinimleri karşılaması ve tüm bunları yaparken test paketi bilmek bir uygulamaya yeni bir bileşen eklemek için yeniden yapılandırmaya yazılım uygulamanın davranışı doğrulayabilirsiniz. Birim testleri değişimi kolaylaştırın ve yazılım bakımı karmaşıklığı arttıkça korumak için bir hızlı geri bildirim döngüsü bir parçasıdır.

Birim testi, ancak bir fiyat ile birlikte gelir. Takım, birim testleri oluşturmak ve sürdürmek için zaman ayırmanız gerekir. Bu testleri oluşturmak için gereken çaba miktarı doğrudan ilişkili **Test Edilebilirlik** temel alınan yazılım. Test etmek için yazılımın ne kadar kolay olduğunu? Test Edilebilirlik aklınızda ile yazılım tasarlamanın takım geçerlilik testleri beklemediğiniz test edilebilir yazılımla birlikte çalışan ekip daha hızlı oluşturun.

ADO.NET Entity Framework 4.0 (EF4) Microsoft Test Edilebilirlik düşünülerek tasarlanan. Bu, geliştiricilerin framework kodunun kendisi karşı birim testleri yazma gelmez. Bunun yerine, EF4 için Test Edilebilirlik hedefleri üzerinde framework derlemeleri test edilebilir kod oluşturmak kolaylaştırır. Biz belirli örneklere göz atacağız önce test edilebilir kod kalitelerini anlamak için faydalı vardır.

### <a name="the-qualities-of-testable-code"></a>Test edilebilir kod kalitelerini

Test etmek kolayca kod her zaman en az iki nitelikler sergiler. İlk test edilebilir kod kolay **gözlemleyin**. Bazı girişler kümesini göz önünde bulundurulduğunda, kodun çıktısı gözlemlemek kolay olmalıdır. Yöntem doğrudan bir hesaplamanın sonucu döndürdüğünden Örneğin, aşağıdaki yöntemi test kolaydır.

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

Bir yöntem test yöntemi, bir ağ yuva, bir veritabanı tablosu veya dosyası aşağıdaki kod gibi hesaplanan değer yazıyorsa zordur. Değerini almak için ek çalışma gerçekleştirmek test vardır.

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

İkincisi, test edilebilir kod kolay **yalıtmak**. Bir hatalı test edilebilir kod örneği olarak aşağıdaki sözde kod kullanalım.

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

Gözlemlemek kolay bir yöntemdir: biz bir sigorta ilke geçirin ve dönüş değeri, beklenen sonuç eşleşen doğrulayın. Ancak, test yöntemi için size bir veritabanı doğru şema ile yüklü olması ve bir e-posta göndermek yöntemin çalışır durumda SMTP sunucusunu yapılandırmanız gerekir.

Birim testi yalnızca yöntemi içindeki hesaplama mantığı olun ister, ancak test e-posta sunucusu çevrimdışı olduğundan veya veritabanı sunucusu taşıdığınız için başarısız olabilir. Bu hatalar her ikisi de doğrulamak için test istediği davranışı ilgili değildir. Davranış izole etmek zordur.

Genellikle, test edilebilir kod yazmak için çaba yazılım geliştiriciler, bir görev ayrımı nettir yazmadan kodda korumak çaba harcar. Yukarıdaki yöntemi, iş hesaplamaları odaklanın ve veritabanı ve e-posta uygulama ayrıntıları diğer bileşenler için temsilci. Robert c Martin bu tek sorumluluk ilkesini çağırır. Bir nesne bir ilke değeri hesaplama gibi tek, dar sorumluluk kapsüllemesi gerekir. Diğer tüm veritabanını ve bildirim iş başka bir nesnenin sorumluluğunda olması gerekir. Bu şekilde yazılmış kodu tek bir görev üzerinde odaklanmıştır yalıtılması daha kolay olur.

. NET'te tek sorumluluk ilkesini izleyin ve ayırma sağlamak için ihtiyacımız soyutlama sahibiz. Arabirim tanımları kullanın ve yerine somut bir türde arabirimi soyutlama kullanmak için kodu zorla. Bu belgede daha sonra nasıl yukarıda verilen hatalı örneği çalışabilirsiniz gibi bir yöntem, arabirimleri görüyoruz *Ara* gibi bunlar veritabanına Bahsediyor. Test zaman, ancak biz veritabanına konuşuruz değil ancak bunun yerine verileri bellek içinde tutar işlevsiz bir uygulama yerine kullanabilirsiniz. İşlevsiz bu uygulama, veri erişim kodu veya veritabanı yapılandırması ilgisiz sorunlar koddan yalıtmak.

Yalıtım için başka avantajları vardır. Son yöntemi iş hesaplamaya yalnızca yürütmek için birkaç milisaniye sürer, ancak test çeşitli sunucular için birkaç saniye koda atlama konuşmalar ve ağ geçici olarak çalışabilir. Küçük değişikliklere hızlı kolaylaştırmak için birim testleri çalıştırmanız gerekir. Birim testleri, ayrıca tekrarlanabilir ve teste ilgisi olmayan bir bileşen bir sorun olduğu için başarısız değil. Yazma inceleyin ve yalıtmak için kolayca kod geliştiricilerin kod için testleri yazmak daha kolay bir zaman gerekeceği anlamına gelir, yürütülecek testleri için daha az süre beklemesini ve daha da önemlisi, var olmayan hataları izleme daha az zaman harcama.

Umarım test avantajları için teşekkür ederiz ve test edilebilir kod sergiler kalitelerini anlayın. Biz hakkında EF4 gözlemlenebilir ve yalıtmak kolay kalan çalışırken verileri bir veritabanına kaydetmek için çalışan bir kodun nasıl yazılacağını ele almak için ancak ilk biz veri erişimi için test edilebilir tasarımlar tartışmak için sunduğumuz odağını daraltmak.

## <a name="design-patterns-for-data-persistence"></a>Veri kalıcılığı için Tasarım desenleri

Daha önce sunulan hatalı örneklerin her ikisi de çok fazla sorumlulukları vardı. İlk hatalı örnek bir hesaplama gerçekleştirmek zorundaydınız *ve* bir dosyaya yazma. Hatalı ikinci örnek, bir veritabanından verileri okuyamadı gerekiyordu *ve* iş hesaplamayı *ve* e-posta gönderin. Konuları ayrı ve diğer bileşenler için sorumluluk daha küçük yöntemleri tasarlayarak test edilebilir kod yazmaya yönelik harika strides yapacaksınız. Hedef, küçük ve odaklı soyutlama eylemlerden bir araya getirerek işlevselliği oluşturmaktır.

Küçük veri kalıcılığı gelir ve arıyoruz için odaklanmış soyutlama kadar sık olduğunda, tasarım desenleri belgelenmiştir. Kurumsal uygulama mimarisi desenleri Martin Fowler'ın kitap yazdırma bu düzenleri tanımlamak için ilk iş oluştu. Nasıl bu ADO.NET varlık çerçevesi uygular ve bu modellerle çalıştığını göstereceğiz önce aşağıdaki bölümlerde bu desenleri kısa bir açıklama sağlarız.

### <a name="the-repository-pattern"></a>Depo düzeni

Fowler bir depo "etki alanı ve veri arasında eşleme katmanları etki alanı nesnelerini erişmek için bir koleksiyon benzeri arabirimi kullanarak aracılık" diyor. Hedef depo düzeninin aðaçlarýn veri erişim kodu ayırmanızı ve önceki yalıtım gördüğümüz gibi test edilebilirliğe yönelik gerekli bir nitelik.

Yalıtım için depo nesneleri bir koleksiyon benzeri arabirimi kullanarak nasıl gösterir anahtardır. Depo hiçbir fikriniz nasıl sahip kullanmak için yazma mantıksal depo, rica nesnelerini gerçekleştirmek. Depo bir veritabanına konuşmak veya yalnızca bir bellek içi koleksiyonundan nesneleri döndürebilir. Kodunuzu bilmesi gereken şey depo koleksiyonu devam ettirirler görünüyor, ve alma, ekleme ve nesneleri koleksiyonundan silme.

Mevcut .NET uygulamalarında somut bir depo genellikle aşağıdaki gibi bir genel arabirimi devralır:

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Bir uygulama için EF4 sağlıyoruz, ancak temel konseptini aynı kalır, arabirim tanımı için birkaç değişiklik oluşturacağız. Kod, bir koşul değerlendirmeye göre varlıklar koleksiyonu almak için birincil anahtar değerine göre varlık almak için bu arabirimi uygulayan somut bir depo kullanın veya yalnızca tüm kullanılabilir varlıkları alma. Ayrıca kod ekleyebilir ve depo arabirimi aracılığıyla varlıkları kaldırın.

IRepository, çalışan nesneleri göz önünde bulundurulduğunda, kod aşağıdaki işlemleri gerçekleştirebilirsiniz.

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

Kod bir arabirim (çalışan IRepository) kullandığından, kod ile arabirimin farklı uygulamaları sağlayabilir. Bir uygulama, Microsoft SQL Server veritabanına EF4 ve kalıcı nesneler tarafından desteklenen bir uygulama olabilir. Farklı bir uygulama (bir test sırasında kullanın), bir bellek içi biri listeyi çalışan nesneler tarafından desteklenen. Arabirimi, kodda ayırma sağlamak için yardımcı olur.

IRepository fark&lt;T&gt; arabirimi kaydetme işlemi koymuyor. Biz varolan nesneleri nasıl güncelleştiririm? Kaydetme işlemi içeren IRepository tanımlarında gelebilir ve bu depolar, uygulamalarını hemen veritabanına bir nesneyi kalıcı hale getirmek gerekir. Ancak, birçok uygulamada ayrı ayrı nesneleri kalıcı hale getirmek istiyoruz yok. Bunun yerine, nesneleri hayata, belki de bu nesnelere farklı depolarından iş etkinliği bir parçası olarak değiştirin ve tek, atomik bir işlem kapsamında tüm nesneleri kalıcı hale getirmek istiyoruz. Neyse ki, bu türden bir davranış izin vermek için bir desen yoktur.

### <a name="the-unit-of-work-pattern"></a>Çalışma deseni birimi

Fowler iş birimi "nesnelerin bir listesini bir iş işlem tarafından etkilenen ve değişiklikleri dışında yazma ve eşzamanlılık sorunlarının çözümü koordinatları olarak tutacaktır" diyor. Bir depodan hayata ve iş birimini değişiklikleri kaydetmek için biz bahsedin, biz nesnelere yaptığınız tüm değişiklikler kalıcı nesnelere değişiklikleri izlemek için iş birimini sorumluluğundadır. Ayrıca yeni nesneler için tüm depolar ekledik ve bir veritabanı ve Yönet silme işlemi aynı zamanda nesneleri eklemek yapılacak iş birimi sorumluluğundadır.

Ardından ADO.NET veri kümeleri ile hiç olmadığı kadar herhangi bir iş yaptıysanız, zaten çalışma deseni birimle hakkında bilgi sahibi olacaksınız. ADO.NET veri kümeleri, bizim güncelleştirme, silme ve ekleme DataRow nesnelerin izlemek için konuklarımızın ve (bir TableAdapter yardımıyla) veritabanına yaptığımız değişiklikleri mutabık kılma. Ancak, temel alınan veritabanı bağlantısı kesilmiş bir alt kümesi veri kümesi nesnelerini model. Çalışma deseni birimidir, iş nesneleri ve veri erişim kodu yalıtılmış ve veritabanının farkında olan etki alanı nesnelerini çalışır ancak aynı davranışı sergiler.

.NET kodu iş birimi modellemek için bir Özet aşağıdakine benzeyebilir:

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

Tek bir iş birimi emin oluruz iş birimini depo başvurularından göstererek nesnenin tüm varlıkların gerçekleştirilmiş bir iş işlemi sırasında izleme olanağı vardır. Commit yöntemi gerçek bir iş birimi için burada tüm veritabanı bellek içi değişikliklerle mutabık kılınacak sihrin uygulamasıdır. 

Kod, IUnitOfWork başvuru göz önünde bulundurulduğunda, bir veya daha fazla depolarından alınan iş nesneler değişiklik ve atomik yürütme işlemini kullanarak tüm değişiklikleri kaydedin.

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a>Yavaş yük düzeni

Fowler adı yavaş yük "tüm verileri içermeyen bir nesne gerektiği halde nasıl bilir" tanımlamak için kullanır. Saydam yavaş yükleniyor ne zaman sağlamak için önemli bir özelliği olan test edilebilir iş kod yazma ve ilişkisel bir veritabanı ile çalışmaya. Örneğin, aşağıdaki kodu düşünün.

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

Zaman çizelgeleri koleksiyon nasıl doldurulur? İki olası yanıtların vardır. Çalışan havuzu bir çalışan getirilecek sorulduğunda çalışan birlikte çalışan ilgili zaman çizelgesi bilgileri almak için bir sorgu sorunları bir cevaptır. Bu genellikle bir sorgu ile bir JOIN yan tümcesi gerektiriyor ve uygulamanın daha fazla bilgi alınırken sonuçlanabilir ilişkisel veritabanları gerekir. Ne uygulamanın hiçbir zaman zaman çizelgeleri özelliği touch gerekiyor?

İkinci bir yanıt zaman çizelgeleri özelliği "isteğe bağlı" yüklenmesidir. Kod zaman çizelgesi bilgileri almak için özel API'ler çağırma kullanılamaz çünkü örtük ve iş mantığı için saydam bu yavaş yükleniyor. Kod, zaman çizelgesi bilgileri gerektiğinde mevcut olduğunu varsayar. Genel yöntem çağrılarını çalışma zamanı ele geçirilmesini gerektirir yavaş yükleme ile ilgili bazı Sihirli yoktur. Araya giren kodu veritabanına Bahsediyor ve iş mantığı iş mantığı olmasını boş bırakarak zaman çizelgesi bilgilerini almak için sorumludur. Bu geç yük Sihirli veri alma işlemleri ve daha fazla test edilebilir kod sonuçlarında kendisini yalıtmak için iş kodu sağlar.

Bir uygulama olan yavaş yük dezavantajı *mu* kodu bir ek sorgu yürütmek zaman çizelgesi bilgileri gerekir. Bu birçok uygulama için ancak performans duyarlı uygulamalar veya (genellikle N + 1 adlandırılan bir sorun her döngü sırasında zaman çizelgesi alınacak çalışan nesneler bir dizi döngü ve sorguyu yürüten uygulamalar için önemli değil Sorgu sorunu) olan bir sürükleme yavaş yükleniyor. Bu senaryolarda bir uygulama zaman çizelgesi bilgileri olası en verimli şekilde eagerly yüklemek isteyebilirsiniz.

Neyse ki, nasıl iki örtük geç yükleri EF4 destekler ve sonraki bölüme taşıyın ve bu desenleri uygulamak verimli eager yükler göreceğiz.

## <a name="implementing-patterns-with-the-entity-framework"></a>Entity Framework ile uygulama düzenleri

Güzel bir haberimiz var tüm son bölümde açıkladığımız tasarım desenleri ile EF4 uygulamak basit olmasıdır. Göstermek için basit bir ASP.NET MVC uygulaması düzenleyin ve çalışanlar ve bunların ilişkili zaman çizelgesi bilgileri görüntülemek için kullanılacak kullanacağız. Aşağıdaki "düz eski CLR nesneler" kullanarak başlayacağız (POCOs). 

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

Bu sınıf tanımları, biraz farklı yaklaşımlara ve EF4 özellikleri inceleyeceğiz, ancak bu sınıfların Kalıcılık ignorant (PI) mümkün olduğunca tutmak hedefi olduğu şekilde değiştirir. PI nesne bilmez *nasıl*, hatta *varsa*, tuttuğu durumu bulunduğu bir veritabanı içinde. PI ve POCOs test edilebilir yazılım ile bir aradadır. POCO yaklaşımı kullanarak nesneleri, daha az kısıtlanmış, daha esnek ve bunlar olmadan mevcut bir veritabanını çalışır çünkü test etmek daha kolay.

Yerinde POCOs ile Visual Studio'da bir varlık veri modeli (EDM) oluşturabilir (bkz. Şekil 1). EDM bizim varlıklar için kod oluşturmak için kullanacağız değil. Bunun yerine, biz lovingly el ile oluşturabilir varlıkları kullanmak istiyoruz. Yalnızca EDM bizim veritabanı şeması oluşturmak ve EF4 veritabanına nesneleri eşleştirmek için gereken meta verilerini sağlamak için kullanacağız.

![eftest_01](~/ef6/media/eftest-01.jpg)

**Şekil 1**

Not: önce EDM modeli geliştirmek istiyorsanız, temiz, EDM POCO kod oluşturmak mümkündür. Veri programlama ekibi tarafından sağlanan bir Visual Studio 2010 uzantısı ile bunu yapabilirsiniz. Uzantı indirmek için Visual Studio'da Araçlar menüsünden Uzantı Yöneticisi'ni başlatın ve "POCO" (bkz: Şekil 2)'için çevrimiçi Galerisine arayın. EF için kullanılabilen çeşitli POCO şablonlar vardır. Şablon kullanma hakkında daha fazla bilgi için bkz. " [izlenecek yol: POCO varlık çerçevesi için şablon](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".

![eftest_02](~/ef6/media/eftest-02.png)

**Şekil 2**

Başlangıç noktası bu POCO biz test edilebilir kod için iki farklı yaklaşım inceleyeceksiniz. İlk yaklaşım soyutlama depoları ve iş birimleri uygulamak için Entity Framework API'sinden yararlanır çünkü EF yaklaşım çağırmalıyım. İkinci yaklaşımda kendi özel deponuzu soyutlama oluşturur ve sonra olumlu ve olumsuz her yaklaşımın bakın. EF yaklaşım inceleyerek başlayacağız.  

### <a name="an-ef-centric-implementation"></a>EF merkezli uygulaması

ASP.NET MVC projesinde aşağıdaki denetleyici eylem göz önünde bulundurun. Eylemi bir çalışan nesnesini alır ve çalışan ayrıntılı bir görünümünü görüntülemek için bir sonuç döndürür.

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

Kod, test edilebilir nedir? Eylemin davranışı doğrulama ihtiyacımız en az iki testleri vardır. İlk olarak, eylemin doğru görünümü – bir kolayca test döndürür doğrulamak isteriz. Biz de eylem doğru çalışan alır ve veritabanını sorgulamak için kod çalıştırmadan bunu istiyoruz doğrulamak için bir test yazmak istersiniz. Test edilen kod izole etmek istediğimiz unutmayın. Test veri erişim kodu veya veritabanı yapılandırması bir hata nedeniyle başarısız değil yalıtım sağlayacaktır. Test başarısız olursa, biz denetleyici mantığında ve bazı alt düzey sistemi bileşeni değil, bir hataya sahip öğrenmiş olacaksınız.

Ayırma sağlamak için biz size sunulan arabirimler gibi bazı soyutlama önceki depoları ve iş birimleri için gerekir. Depo düzeni, etki alanı nesnelerini ve veri eşleme katmanı arasında aktarmak için tasarlanmıştır unutmayın. Bu senaryoda EF4 *olduğu* eşleme verileri katman ve zaten IObjectSet adlı bir depo benzeri soyutlama sağlar&lt;T&gt; (Başlangıç System.Data.Objects ad alanı). Arabirim tanımı aşağıdaki gibi görünür.

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

IObjectSet&lt;T&gt; nesnelerinin bir koleksiyonunu benzer çünkü bir depo gereksinimlerini karşılayan (IEnumerable aracılığıyla&lt;T&gt;) ve ekleme ve nesneleri sanal koleksiyondan kaldırmak için yöntemler sağlar. Attach ve Detach yöntemleri ek özellikler EF4 API'sinin kullanıma sunar. IObjectSet kullanılacak&lt;T&gt; depoları birbirine bağlamak için bir birim iş soyutlama ihtiyacımız depoları için arabirim.

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

Bu arabirimin somut bir uygulama için SQL Server Bahsediyor ve EF4 ObjectContext sınıftan kullanarak oluşturmak kolaydır. Gerçek iş birimi olarak EF4 API ObjectContext sınıftır.

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

Bir IObjectSet getirme&lt;T&gt; yaşama ObjectContext nesnesinin CreateObjectSet yöntemini çağırma olarak oldukça kolaydır. Arka planda framework meta verileri kullanır. somut ObjectSet üretmek için EDM bizim sağladığımız&lt;T&gt;. Biz IObjectSet döndüren ile kullanacağız&lt;T&gt; istemci kodu, test edilebilirliğe korumak yardımcı olacağından arabirim.

Somut bu uygulama, üretim ortamında kullanışlıdır ancak nasıl bizim IUnitOfWork soyutlama test edilmesini kolaylaştırmak için kullanacağız üzerinde odaklanmak ihtiyacımız.

### <a name="the-test-doubles"></a>Test double

Denetleyici eylemini yalıtmak için gerçek birimini (bir ObjectContext tarafından desteklenen) iş ve iş (bellek içi işlemleri) bir test çift veya "sahte" birimi arasında geçiş yapma özelliğini gerekir. Bu tür bir geçiş gerçekleştirmek için genel iş birimi, ancak bunun yerine denetleyici Oluşturucu parametresi olarak iş birimini geçişinde örneği MVC denetleyicisi bulunmamasını yaklaşımdır.

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

Yukarıdaki kod, bağımlılık ekleme örneğidir. Biz, bağımlılık (iş birimi) oluşturur, ancak Denetleyicisi'nde bağımlılık ekleme denetleyicisi izin vermez. MVC projesinde bağımlılık ekleme otomatikleştirmek için bir denetim (IOC) kapsayıcı tersine çevirme ile birlikte bir özel denetleyici üretecini kullanımı yaygındır. Bu konular için bu makalenin kapsamı dışında olsa da, bu makalenin sonunda başvuruları aşağıdaki göre daha fazla bilgi edinebilirsiniz.

Sahte bir birim test etme için kullanabiliriz iş uygulamasının aşağıdaki gibi görünebilir.

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

Sahte iş birimi bir kabul edilen özelliği sunan dikkat edin. Bazen, test edilmesini kolaylaştırmak sahte bir sınıfa özellikler eklemek yararlıdır. Bu durumda kod bir iş birimi işlemeler, kabul edilen özelliğini kontrol ederek gözlemlemek kolaydır.

Ayrıca sahte IObjectSet gerekir&lt;T&gt; çalışan ve zaman çizelgesi nesneleri bellekte tutmak için. Biz genel türler kullanarak tek bir uygulama sağlar.

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

Çift bu test için bir temel alınan HashSet çalışmasının çoğunu Temsilciler&lt;T&gt; nesne. Not Bu IObjectSet&lt;T&gt; T bir sınıf (bir başvuru türü) zorlamayı genel bir kısıtlama gerektirir ve ayrıca bize Iqueryable uygulamak için zorlar&lt;T&gt;. Bir bellek içi koleksiyonu Iqueryable bilgisi görünür hale getirmek kolay&lt;T&gt; AsQueryable standart LINQ işlecini kullanarak.

### <a name="the-tests"></a>Testleri

Geleneksel birim testleri, tüm testleri tüm eylemler için tek bir MVC denetleyicisi tutmak için tek bir test sınıfı kullanır. Bu testler ya da herhangi bir türde birim testi, bellek içi kullanarak fakes yazabiliriz oluşturduk. Ancak bu makalede sorundan kaçınmak için tek parça test sınıfı yaklaşımı ve bunun yerine belirli bir işlevsellik odaklanmak için yaptığımız testleri gruplayın.  Örneğin, yeni çalışan "Oluştur" tek test sınıfının yeni bir çalışan oluşturmaktan sorumlu tek denetleyici eylemi doğrulamak için kullanacağız şekilde test etmek istiyoruz işlevleri olabilir.

Bazı ortak kurulum kodu için tüm bu ayrıntılı test sınıflarında ihtiyacımız yoktur. Örneğin, size her zaman, bellek içi depo ve iş sahte birimi oluşturmanız gerekir. Ayrıca çalışan denetleyici örneği eklenen iş sahte birimle ihtiyacımız var. Bir temel sınıfı kullanılarak biz test sınıflarında bu ortak kurulum kodu sahip olacaksınız.

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

"Nesne Annenizin ve taban sınıfta kullanıyoruz" test verilerini oluşturmak için bir ortak desendir. Bir nesne Annenizin test varlıkları arasında birden çok test armatürleri örneklemek için Fabrika yöntemleri içerir.

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

(Bkz. Şekil 3) test armatürleri sayısı için EmployeeControllerTestBase temel sınıf olarak kullanabiliriz. Her test düzeni, belirli bir denetleyici eylemi test eder. Örneğin, bir test düzeni, oluşturma eylemini (bir çalışan oluşturmak için görüntülemek için) bir HTTP GET isteği sırasında kullanılan testlere olmadığını odaklanın ve farklı bir düzen, bir HTTP POST isteğinde kullanılan Oluştur eylemi odaklanacaktır (tarafından gönderilen bilgi almak için Kullanıcı) bir çalışan oluşturmak için kullanılır. Her bir türetilmiş sınıf, yalnızca belirli bağlamında ve sonuçlar belirli test bağlamını doğrulamak için gereken bir onayları sağlamak için gereken kurulum sorumludur.

![eftest_03](~/ef6/media/eftest-03.png)

**Şekil 3**

Burada sunulan adlandırma kuralı ve test stili test edilebilir kod için gerekli değildir-yalnızca bir yaklaşımdır. Şekil 4, test Çalıştırıcısı eklentisi Visual Studio 2010 için Jet Brains Resharper çalıştıran testleri gösterir.

![eftest_04](~/ef6/media/eftest-04.png)

**Şekil 4**

Her denetleyici eylemi için birim testleri, paylaşılan Kurulum kodu işlemek için temel sınıf ile küçük ve yazmak kolay. Testler hızlıca (biz bellek içi işlemleri yapıldığından) yürütülür ve ilgisiz altyapı veya çevre kaygıları nedeniyle (biz birim testten ayırdığınız nedeniyle) başarısız olmamalıdır.

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

Bu sınamalarda, temel sınıf Kurulum işin çoğunu yapar. Temel sınıf oluşturucusu bellek içi depoya, sahte bir iş birimi ve EmployeeController sınıfının bir örneğini oluşturur unutmayın. Test sınıfı bu taban sınıftan türetilen ve oluşturma yöntemi test özellikleri odaklanır. Bu durumda özellikleri yordamı sınama biriminde görürsünüz "düzenlemek, davranan ve onaylama işlemi" adımlarla boil:

-   Gelen veri benzetimini yapmak için bir Yeniçalışan nesnesi oluşturun.
-   EmployeeController oluşturma eylemini çağırın ve Yeniçalışan içinde geçirin.
-   Oluşturma eylemini (havuzda çalışan görünür) beklenen sonuçları üretir doğrulayın.

Ne oluşturduğumuz EmployeeController eylemlerden herhangi birini test olanak sağlıyor. Biz testler için çalışan denetleyici dizin eylemi yazdığınızda, biz testlerimiz için aynı temel kurulum'kurmak için test temel sınıftan devralabilir. Temel sınıf yeniden bellek içi depo ve iş birimini sahte EmployeeController örneği oluşturur. Dizin eylem için testler dizin eylemi çağırmaya odaklanmak yalnızca gerekli ve eylem modeli nitelikleri test döndürür.

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

Bellek içi fakes ile oluşturmakta testleri test doğru yerleştirilir *durumu* yazılım. Örneğin, oluşturma eylemini yürütüldükten sonra – depo durumunu incelemek için istediğimiz oluşturma eylemini test ederken depoya yeni çalışan içeriyor mu?

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

Temel etkileşim test sırasında daha sonra inceleyeceğiz. Etkileşim tabanlı test, test edilen kod uygun yöntemleri bizim nesneler üzerinde çağrılır ve doğru parametreleri geçirilen sorar. Şu an için başka bir tasarım deseni – yavaş yük kapağına geçeceğiz.

## <a name="eager-loading-and-lazy-loading"></a>İstekli yükleme ve yavaş yükleniyor

ASP.NET MVC Web belirli bir noktada bir çalışanın bilgilerini görüntülemek ve çalışanın eklemek için isteyebilir uygulama zaman çizelgesi ilişkili. Örneğin, biz sistemde çalışan adı ve zaman çizelgesi toplam sayısını gösteren zaman çizelgesi Özet görüntü olabilir. Bu özelliği uygulamak için uygulayabileceğiniz çeşitli yaklaşımlar vardır.

### <a name="projection"></a>Projeksiyon

Özet oluşturmak için bir kolayca görünümünde görüntülemek istediğiniz bilgileri için adanmış bir modeli oluşturmak için yaklaşımdır. Bu senaryoda modeli aşağıdaki gibi görünebilir.

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

EmployeeSummaryViewModel bir varlık değil-diğer bir deyişle, bir veritabanında kalıcı hale getirmek istiyoruz değil unutmayın. Yalnızca bu sınıf türü kesin belirlenmiş bir şekilde görünüm verileri karıştırması kullanmak için kullanacağız. Bir veri aktarımı nesnesi (DTO), hiçbir davranışları (hiçbir yöntemleri) – yalnızca özellikler içerdiği için Görünüm modeli gibidir. Özellikleri taşımak için ihtiyacımız olan veriler tutar. Bu görünüm modeli LINQ'ın standart projeksiyon işleci – seçme işleci kullanarak örneği oluşturmak daha kolaydır.

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

Yukarıdaki kodu için iki önemli özellikler mevcuttur. İlk – kodu inceleyin ve yalıtmak hala kolay olduğundan test etmek kolay bir işlemdir. Gerçek iş birimini karşı çalıştığı gibi seçme işleci ekleyebiliyorsa, bellek içi fakes karşı çalışır.

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

İkinci önemli kod EF4 birlikte çalışan ve zaman çizelgesi bilgileri birleştirmek için tek ve verimli bir sorgu oluşturmak nasıl imkan özelliğidir. Çalışan bilgileri ve zaman çizelgesi bilgileri aynı nesnede herhangi bir özel API'leri kullanmadan yüklendik. Kod, yalnızca uzak veri kaynaklarını yanı sıra bellek içi veri kaynaklarına karşı çalışan LINQ işleçlerini standart'ı kullanarak gerektiren bilgi ifade. C ve LINQ Sorgu tarafından oluşturulan ifade ağaçları çevrilecek EF4 bağlanabildi\# derleyici içine tek ve verimli bir T-SQL sorgusu.

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
         )  AS [Project1]
    )  AS [Limit1]
```

Bazen, biz bir görünüm modeli veya DTO nesne, ancak gerçek varlıklarla çalışmak istemiyor vardır. Ne zaman biliyoruz çalışan ihtiyacımız *ve* çalışanın zaman çizelgesi, biz eagerly örtük ve verimli bir şekilde ilgili veri yükleyebilir.

### <a name="explicit-eager-loading"></a>Açık istekli yükleme

İlgili varlık bilgileri ihtiyacımız bazı mekanizması iş mantığı için (veya bu senaryoda, denetleyici eylem mantıksal) eagerly yüklenmeye istediğinizde, arzu depoya ifade etmek için. EF4 ObjectQuery&lt;T&gt; sınıf sorgu sırasında almak için ilgili nesneleri belirlemek için bir dahil etme yöntemi tanımlar. EF4 ObjectContext varlıkları somut ObjectSet aracılığıyla kullanıma sunan unutmayın&lt;T&gt; ObjectQuery devralan sınıf&lt;T&gt;.  Biz ObjectSet kullanıyorsanız&lt;T&gt; başvuruları denetleyicisi eylemimiz biz aşağıdaki kodu yazabilirsiniz istekli bir zaman çizelgesi bilgileri, her çalışanın yüklemesini belirtin.

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

Bu yana kodumuz test edilebilir korumak çalışıyoruz ancak biz ObjectSet karşı savunmasız bırakıyorsunuz değil&lt;T&gt; gelen gerçek iş sınıfı ölçü dışında. Bunun yerine, biz üzerinde IObjectSet güvenin&lt;T&gt; arabirimi sahtesini oluşturmak kolaydır ancak IObjectSet&lt;T&gt; Ekle yöntemi tanımlamaz. LINQ güzelliği kendi Ekle işleci oluşturabiliriz ' dir.

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

Bu Ekle işleci Iqueryable için bir genişletme yöntemi tanımlanan fark&lt;T&gt; IObjectSet yerine&lt;T&gt;. Bu yöntemi bir yelpazede Iqueryable dahil olmak üzere, olası bir türü kullanma yeteneği sağlıyor&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;ve ObjectSet&lt;T&gt;. Temel alınan dizi orijinal bir EF4 ObjectQuery değil durumunda&lt;T&gt;, ardından yapılan hiçbir zarar yoktur ve bir İşlemsiz INCLUDE işlecidir. Alttaki sıra, *olduğu* ObjectQuery&lt;T&gt; (veya ObjectQuery türetilmiş&lt;T&gt;), EF4 bizim ek veri gereksinimini bakın ve uygun SQL düzenleme Sorgu.

Bu yeni işleciyle yerinde biz açıkça bir zaman çizelgesi bilgileri istekli yükünü depodan isteyebilir.

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Gerçek bir Objectcontext'e karşı çalıştırdığınızda, aşağıdaki tek sorgu kod üretir. Sorgu, veritabanından çalışan nesnelerini gerçekleştirmek ve tamamen kendi zaman çizelgeleri özelliğini doldurmak için bir seyahat yeterli bilgi toplar.

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
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

Eylem yöntemi içindeki kodu tam olarak test edilebilir kalır harika bir haberimiz var olur. Ekle işleci desteklemek fakes bizim için herhangi bir ek özellikler sağlamak gerekmez. Ekle işleci Kalıcılık ignorant tutmak istedik kod içinde kullanılacak vardı hatalı haber olur. Bu tür bir test edilebilir kod oluşturma sırasında değerlendirilecek ihtiyacınız olacak ödün prime bir örnektir. Kalıcılık kaygıları sızıntısı performans hedeflerinize ulaşmak için depo soyutlama dışında izin vermek için gerektiğinde zamanlar vardır.

İstekli yükleme için alternatiftir yavaş yükleniyor. Yaptığımız yavaş yükleniyor anlamına gelir *değil* açıkça ilişkili veri gereksinimini duyurmaktan iş kodumuz gerekir. Bunun yerine, Entity Framework kullandığımız bizim varlıkları uygulamada ve ek veri olup olmadığını gereken isteğe bağlı veri yükler.

### <a name="lazy-loading"></a>Yavaş yükleniyor

Biz ne veri parçasını iş mantığı gerekir burada bilmiyorum bir senaryoyu düşünün daha kolaydır. Mantıksal bir çalışan nesne olması gerekiyor, ancak biz burada bu yollar bazıları çalışana ait zaman çizelgesi bilgileri gerektirir ve bazı yapmak farklı yürütme yolları dal biliyor olabilirsiniz. Bu gibi senaryolarda veri Sihirli bir gerektiği şekilde göründüğünden örtük geç yüklemek için mükemmeldir.

Ertelenmiş olarak da bilinir, yavaş yükleniyor yükleniyor, bizim varlık nesneler üzerinde bazı gereksinimler yerleştirin. True Kalıcılık ignorance ile POCOs, gereksinimlere Kalıcılık katmandan yönde değil, ancak doğru Kalıcılık ignorance elde etmek çevrilmesi imkansızdır.  Bunun yerine Kalıcılık ignorance göreli derece cinsinden ölçer. Kalıcılık yönelik bir taban sınıftan devralın ya da özel bir koleksiyon POCOs yavaş yükleme elde etmek için kullanmak üzere ihtiyacımız talihsiz olurdu. Neyse ki, EF4 çalışılmasını daha az sezgisel bir çözümü vardır.

### <a name="virtually-undetectable"></a>Neredeyse algılanamayan

POCO nesneleri kullanırken EF4 varlıklar için çalışma zamanı proxy'si dinamik olarak oluşturur. Bu proxy'ler görünmez gerçekleştirilmiş POCOs kaydırma ve her bir özellik kesintiye tarafından ek hizmetler elde sağlar ve ayarlama işlemi ek çalışma gerçekleştirmek için. Böyle bir hizmet arıyoruz için yavaş yükleniyor özelliğidir. Verimli bir değişiklik izleme mekanizması program bir varlığın özellik değerleri değiştiğinde kaydedebilirsiniz başka bir hizmettir. Değişikliklerin listesini Objectcontext'in SaveChanges yöntemi sırasında değiştirilen güncelleştirme komutları kullanarak herhangi bir varlık kalıcı hale getirmek için kullanılır.

Çalışmak bu proxy'leri için ancak ihtiyaç duydukları get özelliği bağlama için bir yol ve ayarlama işlemleri varlık ve proxy'ler sanal üyelerini geçersiz kılarak bu amaca ulaşmak. Örtük geç yükleme ve değişiklik izleme etkin olmasını istiyoruz, bu nedenle, bizim POCO sınıf tanımları için geri dönün ve Özellikler sanal olarak işaretlemek ihtiyacımız var.

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

Biz yine de, çoğunlukla ignorant Kalıcılık çalışan varlıktır da söyleyebilirsiniz. Sanal üyeler kullanmak için tek gereksinim olmasıdır ve kodun test edilebilirliğini etkilemez. Özel bir temel sınıfından türetilir veya bile Gecikmeli yükleme için ayrılmış bir özel koleksiyon gerekmez. Kod kadarıyla ICollection uygulayan herhangi bir sınıfa&lt;T&gt; ilgili varlıkları tutmak kullanılabilir.

Bir alt değişiklik yapmak için iş birimi içinde ihtiyacımız yoktur. Gecikmeli yükleme zaman *kapalı* doğrudan bir Objectcontext'e nesne ile çalışırken, varsayılan olarak. Biz ContextOptions özelliği etkinleştirmek için Ertelenen yükleme ayarlayabilirsiniz bir özelliği yoktur ve her yerde yavaş yükleniyor sağlamak istiyorsanız Biz bu özellik, gerçek iş birimi içinde ayarlayabilirsiniz.

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

Etkinleştirilmiş örtük geç yükleme ile ek verileri yüklemek EF için gerekli çalışmanın hiç farkında olmadan kalan çalışırken zaman çizelgesi çalışanın ilişkili ve bir çalışan uygulama kodu kullanabilirsiniz.

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

Yavaş yükleniyor uygulama kodu yazmak daha kolay hale getirir ve kod ile proxy Sihirli tamamen test edilebilir kalır. Bellek içi fakes çalışma biriminin sadece sahte test sırasında gerektiğinde ilişkili veri varlıklarıyla önceden yükleyebilir.

Bu noktada biz IObjectSet kullanarak depo oluşturma uygulamamızla açacağım&lt;T&gt; Kalıcılık framework'ün tüm işaretleri gizlemek için soyutlama bakın.

## <a name="custom-repositories"></a>Özel depolar

Biz çalışma tasarım deseni birimi bu makalede gösterilen zaman biz bazı örnek kodlar iş birimini görünebileceği sağlanır. Şimdi yeniden çalışan ve biz ile çalışmalar çalışan zaman çizelgesi senaryosu kullanarak bu özgün bir fikir sunar.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

Bu iş birimi ve son bölümde oluşturduğumuz iş birimini arasındaki birincil fark nasıl bu iş birimi EF4 çerçeveden herhangi bir soyutlama kullanmaz olduğu (hiçbir IObjectSet yoktur&lt;T&gt;). IObjectSet&lt;T&gt; depo arabirimi ancak kullanıma sunduğu API'yi olarak çalışır değil mükemmel bir şekilde hizalayın bizim uygulama gereksinimlerine göre. Yaklaşan Bu yaklaşımda size özel IRepository kullanarak depoları temsil edecek&lt;T&gt; Özet.

Test Odaklı Tasarım, davranış Odaklı Tasarım ve etki alanı odaklı yöntemlerini tasarım izleyen birçok geliştiricinin tercih IRepository&lt;T&gt; çeşitli nedenlerle yaklaşım. İlk olarak, IRepository&lt;T&gt; arabirimi, bir "bozulma önleyici" Katman'ı temsil eder. Etki alanı Odaklı Tasarım nitelemiştir Eric Evans tarafından açıklandığı gibi etki alanı kodu bir Kalıcılık API gibi altyapı API'leri, uzakta bir bozulma önleyici katman tutar. İkincisi, geliştiriciler (testleri yazılırken bulunan gibi) bir uygulamanın tam gereksinimleri karşılayan depoya yöntemleri oluşturabilirsiniz. Örneğin, sık FindById yöntemi depo arabirimi ekleyebilmek için bir kimlik değeri kullanarak tek bir varlık bulmak ihtiyacımız.  Bizim IRepository&lt;T&gt; tanımı aşağıdaki gibi görünür.

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

Biz bırakma geri Iqueryable kullanmaya dikkat edin&lt;T&gt; arabirimi varlık koleksiyonlarını açığa çıkarır. Iqueryable&lt;T&gt; LINQ ifade ağaçları EF4 sağlayıcısına akış ve sağlayıcı sorguyu bütünsel bir görünümünü sağlar. IEnumerable döndürmek için ikinci bir seçenek olacaktır&lt;T&gt;, EF4 LINQ sağlayıcısı başka bir deyişle, yalnızca depo içinde yerleşik ifadeleri görürsünüz. Tüm gruplandırma, sıralama ve havuz dışında yapılan projeksiyon performansı olumsuz etkileyebilir veritabanına gönderilen SQL komutu içine oluşacaktır değil. Diğer yandan, yalnızca IEnumerable döndüren bir depo&lt;T&gt; sonuçları hiçbir zaman şaşırtan, yeni bir SQL komutu. Her iki yaklaşım çalışır ve her iki yaklaşım test edilebilir kalır.

IRepository tek bir uygulamasını sağlamak üzere basittir&lt;T&gt; arabirimi genel türler ve EF4 ObjectContext API kullanma.

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

IRepository&lt;T&gt; yaklaşım sağladığı bizim sorgular üzerinde bazı ek denetim bir varlığa almak için bir yöntemi çağırmak bir istemci olduğundan. Yöntem içinde size ek denetimler ve uygulama kısıtlamaları uygulamak için LINQ işleçleri sağlayabilir. Genel tür parametresi kısıtlamaları iki arabirim olduğuna dikkat edin. İlk sınırlamadır ObjectSet tarafından gerekli sınıf olumsuz taint&lt;T&gt;, bizim varlıkları IEntity – uygulama için oluşturulan bir soyutlama uygulamak için ikinci kısıtlamayı zorlar. Okunabilir bir kimlik özelliği için varlıklar IEntity arabirimi zorlar ve ardından bu özellik FindById yöntemin kullanabiliriz. Aşağıdaki kod ile IEntity tanımlanır.

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

Bu arabirim uygulamak için sunduğumuz varlıkları gerektiğinden IEntity Kalıcılık ignorance küçük ihlalini değerlendirilebilir. Kalıcılık ignorance ödünler olan ve birçok için FindById işlevselliğini arabirimi tarafından uygulanan kısıtlama gölgede unutmayın. Arabirim Test Edilebilirlik üzerinde bir etkisi yoktur.

Canlı IRepository örnekleme&lt;T&gt; somut bir birimi, iş uygulaması oluşturmada yönetmesi gereken şekilde bir EF4 Objectcontext'e gerektirir.

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

### <a name="using-the-custom-repository"></a>Özel depo kullanma

Özel depomuzda kullanarak IObjectSet üzerinde temel depoyu kullanarak önemli ölçüde farklı değil&lt;T&gt;. LINQ işleçleri doğrudan bir özelliğe uygulamak yerine, önce bir Iqueryable bilgisi almak için deponun yöntemlerini çağırmak gerekir&lt;T&gt; başvuru.

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Daha önce uygulanan özel Ekle işleci değişiklik çalışır dikkat edin. Deponun FindById yöntemi, tek bir varlık almaya çalışırken eylemlerden yinelenen mantık kaldırır.

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

Biz incelenirken iki yaklaşım de içinde önemli bir fark yoktur. Biz IRepository sahte uygulamaları sağlayabilir&lt;T&gt; HashSet tarafından desteklenen somut sınıflar oluşturma tarafından&lt;çalışan&gt; - son bölümde ne yaptığımız gibi. Ancak, bazı geliştiriciler, sahte nesneler kullanıp fakes derleme yerine nesne çerçeveleri sahte tercih eder. Kararlılığımızın test ve fakes mocks arasındaki farklar sonraki bölümde ele mocks kullanarak inceleyeceğiz.

### <a name="testing-with-mocks"></a>Mocks ile test etme

Bir "testi çift" hangi Martin Fowler çağrıları oluşturmak için farklı yaklaşım vardır. Bir test (bir film stunt gibi çift) çift yapı "gerçekten bekleme için" üretim nesneleri testleri sırasında nesne değildir. Oluşturduğumuz bellek içi depo için SQL Server konuşmak depoları için test double ' dir. Biz kod yalıtmak ve hızlı çalışan testler tutmak için birim testleri sırasında bu test double kullanmayı gördünüz.

Oluşturduğumuz test double gerçek ve çalışma uygulamaları vardır. Arka planda her biri bir somut nesnelerinin koleksiyonunu depolar ve bunlar ekleyin ve biz depo test sırasında değişiklik yaptıkça nesneleri bu koleksiyondan Kaldır. Bazı geliştiriciler, kendi test double bu şekilde – gerçek kod ve çalışma uygulamaları ile oluşturmak ister.  Bu test double dediğimiz olan *fakes*. Bunlar çalışan uygulamalara sahip, ancak bunlar üretimde kullanıma yönelik gerçek yeterli değildir. Sahte depo gerçekten veritabanına yazma değil. Sahte SMTP sunucusu, ağ üzerinden bir e-posta iletisi gerçekten göndermez.

### <a name="mocks-versus-fakes"></a>Mocks Fakes karşılaştırması

Başka türde bir çift olarak bilinen test yok. bir *sahte*. Fakes çalışma uygulamaları olsa da, mocks hiçbir bir uygulama ile birlikte gelir. Yardım sahte nesne framework'ün çalışma zamanında bu sahte nesneler oluşturmak ve test double kullanabilirsiniz. Bu bölümde framework Moq sahte işlem açık kaynak kullanacağız. Bir testi çift bir çalışan havuzu için dinamik olarak oluşturmak için Moq kullanmanın basit bir örnek aşağıda verilmiştir.

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

İçin bir IRepository Moq isteriz&lt;çalışan&gt; uygulama ve yapılar bir dinamik olarak. IRepository uygulayan nesne için aldığımız&lt;çalışan&gt; sahte nesne özelliğini erişerek&lt;T&gt; nesne. Bizim denetleyicilerine geçirerek bu iç nesne olan ve bu çift bir test gerçek depoyu olup olmadığını bilemezsiniz. Biz gerçek bir uygulama bir nesne üzerinde yöntemleri çağıracaktır gibi şu nesne üzerinde yöntemleri çağırabilirsiniz.

Biz Ekle yöntemi çağırdığınızda sahte depo yapacaksınız merak ediyor olmalıdır. Ekle, sahte nesnenin arkasında uygulaması olduğundan, hiçbir şey yapmaz. Çalışan göz ardı edilir şekilde yazdığımız, fakes ile vardı gibi arka planda somut hiçbir koleksiyon yok. FindById dönüş değeri hakkında neler diyeceksiniz? Bu durumda sahte nesnenin dönüş olan tek şey yapabilirsiniz, varsayılan bir değer yapar. Biz bir başvuru türü (çalışan) döndüren olduğundan, dönüş değeri boş bir değerdir.

Mocks işe yaramaz ses; Ancak, iki daha fazla hakkında konuştuk henüz mocks özellikleri vardır. İlk olarak, Moq framework sahte nesnesinde yapılan tüm çağrıları kaydeder. Daha sonra kodda herkes Ekle yöntemi çağırdığınız ya da herkes FindById yöntemi çağırdığınız biz Moq sorabilirsiniz. Bu "siyah kutu" kaydı özelliğini testlerinde daha sonra nasıl kullanabileceğimizi göreceğiz.

İkinci harika sahte bir nesneyle programlamak için Moq nasıl kullanabileceğimizi özelliğidir *beklentileri*. Bir Beklenti nasıl verilen etkileşimi yanıt sahte nesne bildirir. Örneğin, size sunduğumuz sahte bir Beklenti program ve birisi FindById çağırdığında çalışan nesneyi döndürmek için söyleyin. Moq framework bu beklentileri programlamak için Kurulum API ve lambda ifadeleri kullanır.

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

Bu örnekte bir depo dinamik olarak oluşturmak için Moq isteriz ve ardından bir Beklenti deposuyla yapacağız. Beklenti birisi 5 değerini geçirme FindById yöntemi çağırdığında bir kimliği değeri 5 olan yeni bir çalışan nesne döndürmek için sahte nesne bildirir. Bu test geçer ve sahte IRepository için tam bir uygulama oluşturmak gerekmedi&lt;T&gt;.

Şimdi daha önce yazdığımız testleri yeniden ziyaret ve bunları yerine fakes mocks kullanacak şekilde yeniden. Tıpkı daha önce bir temel sınıf ortak parçalarını tüm denetleyicinin testleri için ihtiyacımız olan altyapı kurmak için kullanacağız.

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

Kurulum kodu çoğunlukla aynı kalır. Fakes kullanmak yerine, Moq sahte nesneler oluşturmak için kullanacağız. Temel sınıf kodu çalışanlar özelliği çağırdığında sahte bir depo döndürmek sahte iş birimini için düzenler. Sahte Kurulum geri kalanını her belirli bir senaryoyu ayrılmış test armatürleri içinde gerçekleşir. Örneğin, test düzeni dizin eylem için eylem sahte deponun FindAll yöntemi çağırdığında, çalışanların bir listesini döndürmek için sahte depo Kurulum.

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

Beklentileri dışında testlerimiz için önce vardı testleri aşağıdakine benzer. Ancak, sahte bir çerçeve kaydı özelliği sayesinde biz farklı bir açıdan test yaklaşımını. Sonraki bölümde bu yeni perspektif inceleyeceğiz.

### <a name="state-versus-interaction-testing"></a>Durum etkileşimini test etme karşılaştırması

Yazılım sahte nesneler ile test etmek için kullanabileceğiniz farklı teknik vardır. Durumu test ne bu belgede şu ana kadar uyguladığımız olduğu tabanlı bir yaklaşımdır. Yazılım durumuyla ilgili test yapar onaylar durumuna bağlıdır. Son test denetleyicisinde bir eylem yöntemi çağrılır ve oluşturmanız gereken onaylama modelle ilgili yapılan. Test durumlarını diğer bazı örnekleri aşağıda verilmiştir:

-   Oluşturma yürütüldükten sonra yeni çalışan nesne deposu içerdiğini doğrulayın.
-   Dizin yürütüldükten sonra model tüm çalışanların bir listesini tutar doğrulayın.
-   Depoyu Sil yürütüldükten sonra belirli bir çalışan içermiyor doğrulayın.

Başka bir yaklaşım sahte nesneler göreceksiniz olduğunu doğrulamak için *etkileşimleri*. Sınama yapar onaylar nesnelerinin durumunu hakkında durumuna bağlı olsa da etkileşim nesnelerin nasıl etkileşimde bulunduğunu hakkında sınama yapar onaylar temel. Örneğin:

-   Oluşturma yürütüldüğünde denetleyiciyi deponun Ekle yöntemi çağıran doğrulayın.
-   Dizin yürütüldüğünde denetleyiciyi deponun FindAll yöntemi çağıran doğrulayın.
-   Denetleyiciyi çağıran düzenleme yürütüldüğünde, değişiklikleri kaydetmek için iş yürütme yönteminin birim doğrulayın.

Çünkü biz içinde koleksiyonlar duvarında açmak değildir ve sayıları doğrulanıyor etkileşim genellikle daha az test verileri gerektirir. Bir deponun FindById yöntemi doğru değerle - Ayrıntılar eylemi çağırır biliyorsanız, örneğin, ardından eylem büyük olasılıkla doğru davrandığını. Tüm test verileri ayarlamadan FindById döndürmek için Biz bu davranışı doğrulayabilirsiniz.

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

Yukarıdaki test düzeni gerekli tek temel sınıfı tarafından sağlanan Kurulumu kurulur. Biz denetleyici eylemini çağırdığınızda Moq sahte depoyla etkileşimleri kaydeder. Doğrulama API Moq kullanarak şu denetleyici doğru kimliği değeri ile FindById çağrılır, Moq kapatmamızı isteyebilirsiniz. Denetleyici yöntemin çağırmayan veya beklenmeyen bir parametreyi yöntemiyle çağrılır, doğrulama yöntemi bir özel durum oluşturur ve test başarısız olur.

Oluşturma eylemini işleme geçerli iş biriminde çağırır doğrulamak için başka bir örnek aşağıda verilmiştir.

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

Etkileşim test ile bir tehlike işlemciler üzerinde etkileşimleri belirtmek için ' dir. Her etkileşim doğrulamak kayıt ve sahte nesne ile her etkileşim test gelmez doğrulamak için sahte nesnesinin özelliğini denemelisiniz. Uygulama Ayrıntıları bazı etkileşimleri ve etkileşimler yalnızca doğrulamalıdır *gerekli* geçerli test karşılamak için.

Test ettiğiniz sistem ve kişisel mocks veya fakes arasında seçim büyük ölçüde bağlıdır (veya takım) tercihleri. Sahte nesneler test double uygulamak için gereken kod miktarını önemli ölçüde azaltabilir, ancak değil herkesin beklentilerini programlama ve etkileşimleri doğrulama deneyimli olduğu.

## <a name="conclusions"></a>Sonuçları

Bu belgede size ADO.NET varlık çerçevesi veri kalıcılığını kullanırken test edilebilir kod oluşturma için çeşitli yaklaşımlar gösterilen. Biz IObjectSet gibi yerleşik soyutlama yararlanabilir&lt;T&gt;, ya da kendi soyutlama IRepository gibi oluşturma&lt;T&gt;.  Her iki durumda da kalıcı olarak kalması bu soyutlama tüketicilerinin ignorant ve yüksek düzeyde sınanabilir ADO.NET Entity Framework 4.0 POCO desteği sağlar. Örtük geç iş ve uygulama hizmeti yüklenmesine izin verir gibi ek EF4 özellikleri ilişkisel bir veri deposu ayrıntılarını hakkında endişelenmeden çalışmak için kodu. Son olarak, sahte veya birim testleri içinde sahte oluştururuz soyutlama kolaydır ve bu yüksek oranda yalıtılmış, hızlı çalışması elde etmek için test çift ve güvenilir testler kullanabiliriz.

### <a name="additional-resources"></a>Ek Kaynaklar

-   Robert c Martin " [tek sorumluluk ilkesini](http://www.objectmentor.com/resources/articles/srp.pdf)"
-   Martin Fowler [desen Kataloğu](http://www.martinfowler.com/eaaCatalog/index.html) gelen *Kurumsal uygulama mimari desenleri*
-   Griffin Caprio " [bağımlılık ekleme](https://msdn.microsoft.com/magazine/cc163739.aspx)"
-   Veri programlama Blogundaki " [izlenecek yol: Test odaklı geliştirme Entity Framework 4.0 ile](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".
-   Veri programlama Blogundaki " [Entity Framework 4.0 ile kullanarak depo ve iş birimi düzenleri](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"
-   Dave Astels " [BDD giriş](http://blog.daveastels.com/files/BDD_Intro.pdf)"
-   Aaron Jensen " [makine belirtimleri ile tanışın](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"
-   Eric Lee " [MSTest ile BDD](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"
-   Eric Evans " [etki alanı Odaklı Tasarım](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"
-   Martin Fowler " [Mocks olmayan yer tutucular](http://martinfowler.com/articles/mocksArentStubs.html)"
-   Martin Fowler " [Test çift](http://martinfowler.com/bliki/TestDouble.html)"
-   Jeremy Miller, " [etkileşimini test etme ve durum](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"
-   [Moq](http://code.google.com/p/moq/)

### <a name="biography"></a>Biyografi

Scott Allen, pluralsight teknik ekibi üyesi ve poshbeauty.com sitesinin OdeToCode.com ' dir. Ticari yazılım geliştirme 15 yıl içinde Scott yüksek düzeyde ölçeklenebilir bir ASP.NET web uygulamaları 8-bit katıştırılmış cihazlara kadar her şey için çözümler üzerinde çalışmıştır. Scott OdeToCode, blog veya Twitter'da ulaşabilir [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode).
