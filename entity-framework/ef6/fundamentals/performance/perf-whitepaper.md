---
title: EF4 EF5 ve EF6 için performans konuları
author: divega
ms.date: 2016-10-23
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: f71a13ec06ad46259b3f33216367723b53314a5c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996754"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a>EF 6 4 ve 5 için performans konuları
David Obando, Eric Dettinger ve diğerleri

Yayım Tarihi: Nisan 2012

Son güncelleştirme: Mayıs 2014

------------------------------------------------------------------------

## <a name="1-introduction"></a>1. Giriş

Nesne İlişkisel eşleme çerçeveleri, nesne yönelimli bir uygulamada veri erişimi için bir soyutlamayı sağlamak için kullanışlı bir yoludur. .NET uygulamaları için Microsoft'un O/RM Entity Framework önerilir. Herhangi bir soyutlama ile yine de önemli performans hale gelebilir.

Bu teknik incelemede, geliştiriciler performansını etkileyebilir Entity Framework iç algoritmaları hakkında fikir verir ve araştırma için ipuçları sağlamak için Entity Framework kullanan uygulamalar geliştirirken performansla ilgili önemli noktalar göstermek için yazılmıştır ve Entity Framework kullanan uygulamalarında performans iyileştirme. Kullanılabilir birkaç performans üzerinde iyi konuların zaten web üzerinde ve size mümkün olduğunda bu kaynakları şuraya işaret eden çalıştınız.

Performans karmaşık bir konudur. Bu teknik incelemede bir kaynak olarak yardımcı olmak amacıyla performans yaptığınız hazırlanmıştır ilgili kararları Entity Framework kullanan uygulamalarınız için. Performans göstermek için bazı test ölçüm ekledik ancak bu ölçümlerin mutlak uygulamanızda göreceğiniz performans göstergelerini olarak sağlanmasıyla.

Pratik amaçlar için bu belgede, Entity Framework 4, .NET 4.0 ve Entity Framework 5 altında çalıştırılır ve 6 .NET 4.5 altında çalışan varsayar. Entity Framework 5 için yapılan performans geliştirmelerin çoğunu, .NET 4.5 ile birlikte gelen temel bileşenleri içinde yer alır.

Entity Framework 6 bir bant sürüm olduğundan ve .NET ile birlikte gelen Entity Framework bileşenleri, bağlı değildir. Entity Framework 6 hem .NET 4.0 hem de .NET 4.5 üzerinde çalışır ve bir büyük performans avantajı adresinden .NET 4.0 yükseltme henüz ancak kendi son Entity Framework BITS isteyen kişiler sunabilir. Bu belge, Entity Framework 6 bahsetmeleri, bu makalenin yazıldığı sırada kullanılabilir en son sürüme başvuran: 6.1.0 sürümü.

## <a name="2-cold-vs-warm-query-execution"></a>2. Soğuk vs. Orta Gecikmeli sorgu yürütme

Herhangi bir sorgu, belirli bir model karşı yapılan ilk kez Entity Framework birçok iş yüklemek ve model doğrulamak için arka planda gerçekleştirir. Biz bu ilk sorgu için "soğuk" sorgu olarak sık bakın.  Daha önceden yüklenmiş bir modeli sorguları "sıcak" sorgu olarak bilinir ve çok daha hızlıdır.

Şimdi Entity Framework kullanarak bir sorgu yürütülürken zaman nerede harcandığını bir üst düzey görünümü yararlanın ve şeyler Entity Framework 6'da burada geliştirdiğinizi bakın.

**İlk sorgu yürütme – soğuk sorgu**

| Kod kullanıcı yazma                                                                                     | Eylem                    | EF4 Performans etkisi                                                                                                                                                                                                                                                                                                                                                                                                        | EF5 Performans etkisi                                                                                                                                                                                                                                                                                                                                                                                                                                                    | EF6 Performans etkisi                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Bağlam oluşturma          | Orta                                                                                                                                                                                                                                                                                                                                                                                                                        | Orta                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Sorgu ifadesi oluşturma | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                           | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | LINQ Sorgu yürütme      | -Meta verileri yükleniyor: önbelleğe alınan ancak yüksek <br/> -Nesil görüntüleme: büyük olasılıkla çok yüksek ancak önbelleğe alınmış <br/> -Değerlendirme parametresi: Orta <br/> -Sorgu çevirisi: Orta <br/> -Materializer oluşturma: önbelleğe alınan ancak orta <br/> -Veritabanı sorgu yürütme: büyük olasılıkla yüksek <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Nesne gerçekleştirme: Orta <br/> -Arama identity: Orta | -Meta verileri yükleniyor: önbelleğe alınan ancak yüksek <br/> -Nesil görüntüleme: büyük olasılıkla çok yüksek ancak önbelleğe alınmış <br/> -Değerlendirme parametresi: düşük <br/> -Sorgu çevirisi: önbelleğe alınan ancak orta <br/> -Materializer oluşturma: önbelleğe alınan ancak orta <br/> -Veritabanı sorgu yürütme: büyük olasılıkla yüksek (bazı durumlarda sorguların daha iyi) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Nesne gerçekleştirme: Orta <br/> -Arama identity: Orta | -Meta verileri yükleniyor: önbelleğe alınan ancak yüksek <br/> -Nesil görüntüleme: önbelleğe alınan ancak orta <br/> -Değerlendirme parametresi: düşük <br/> -Sorgu çevirisi: önbelleğe alınan ancak orta <br/> -Materializer oluşturma: önbelleğe alınan ancak orta <br/> -Veritabanı sorgu yürütme: büyük olasılıkla yüksek (bazı durumlarda sorguların daha iyi) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Nesne gerçekleştirme: Orta (EF5 daha daha hızlı) <br/> -Arama identity: Orta |
| `}`                                                                                                  | Connection.Close          | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                           | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


**İkinci sorgu yürütmenin – Orta Gecikmeli sorgu**

| Kod kullanıcı yazma                                                                                     | Eylem                    | EF4 Performans etkisi                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | EF5 Performans etkisi                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | EF6 Performans etkisi                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Bağlam oluşturma          | Orta                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Orta                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Sorgu ifadesi oluşturma | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | LINQ Sorgu yürütme      | -Metadata ~~Yükleniyor~~ arama: ~~önbelleğe alınmış ancak yüksek~~ düşük <br/> -Görüntüleme ~~nesil~~ arama: ~~potansiyel olarak çok yüksek ancak önbelleğe alınmış~~ düşük <br/> -Değerlendirme parametresi: Orta <br/> -Sorgu ~~çeviri~~ arama: Orta <br/> -Materializer ~~nesil~~ arama: ~~önbelleğe alınmış ancak orta~~ düşük <br/> -Veritabanı sorgu yürütme: büyük olasılıkla yüksek <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Nesne gerçekleştirme: Orta <br/> -Arama identity: Orta | -Metadata ~~Yükleniyor~~ arama: ~~önbelleğe alınmış ancak yüksek~~ düşük <br/> -Görüntüleme ~~nesil~~ arama: ~~potansiyel olarak çok yüksek ancak önbelleğe alınmış~~ düşük <br/> -Değerlendirme parametresi: düşük <br/> -Sorgu ~~çeviri~~ arama: ~~önbelleğe alınmış ancak orta~~ düşük <br/> -Materializer ~~nesil~~ arama: ~~önbelleğe alınmış ancak orta~~ düşük <br/> -Veritabanı sorgu yürütme: büyük olasılıkla yüksek (bazı durumlarda sorguların daha iyi) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Nesne gerçekleştirme: Orta <br/> -Arama identity: Orta | -Metadata ~~Yükleniyor~~ arama: ~~önbelleğe alınmış ancak yüksek~~ düşük <br/> -Görüntüleme ~~nesil~~ arama: ~~önbelleğe alınmış ancak orta~~ düşük <br/> -Değerlendirme parametresi: düşük <br/> -Sorgu ~~çeviri~~ arama: ~~önbelleğe alınmış ancak orta~~ düşük <br/> -Materializer ~~nesil~~ arama: ~~önbelleğe alınmış ancak orta~~ düşük <br/> -Veritabanı sorgu yürütme: büyük olasılıkla yüksek (bazı durumlarda sorguların daha iyi) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Nesne gerçekleştirme: Orta (EF5 daha daha hızlı) <br/> -Arama identity: Orta |
| `}`                                                                                                  | Connection.Close          | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


Soğuk ve orta Gecikmeli sorgular performans maliyetini azaltmak için birkaç yolu vardır ve biz şu aşağıdaki bölümdeki göz atacağız. Sorunlar performans görünümü oluşturma sırasında deneyimli çözmenize yardımcı önceden üretilmiş görünümleri kullanarak soğuk sorgularda yükleme modeli maliyetini azaltmayı özellikle inceleyeceğiz. Orta Gecikmeli sorgular için biz sorgu planını önbelleğe alma, izleme sorgu yok ve farklı bir sorgu yürütme seçenekleri ele alacağız.

### <a name="21-what-is-view-generation"></a>2.1 görünüm oluşturma nedir?

Nesil olduğundan hangi görünümü anlamak için önce "Eşlemesi Görünüm" nedir anlıyoruz gerekir. Eşleme, her varlık kümesi ve ilişkilendirme belirtilen dönüşümleri yürütülebilir temsillerini görünümleridir. Dahili olarak, bu eşleme görünümler CQTs (kurallı sorgu ağaçları) şeklini alır. Eşleme görünümleri iki tür vardır:

-   Sorgu görünümleri: Bunlar veritabanı şema kavramsal modeline gitmek gerekli dönüşümü temsil eder.
-   Görünümleri güncelleştirme: Bunlar veritabanı şemasına kavramsal modelden gitmek gerekli dönüşümü temsil eder.

Kavramsal model veritabanı şemasını çeşitli şekillerde farklılık gösterebilir göz önünde bulundurun. Örneğin, tek bir tablo, iki farklı varlık türü için verileri depolamak için kullanılabilir. Devralma ve önemsiz olmayan eşlemeleri eşleme görünümleri karmaşıklığı içinde bir rol oynar.

Eşleme belirtimine göre bu görünümler bilgi işlem görünüm oluşturma dediğimiz işlemidir. Görünüm oluşturma ya da dinamik olarak bir modeli yüklendiğinde veya derleme zamanında "önceden üretilmiş görünümleri"; kullanarak gerçekleştirilebilir İkinci bir C varlık SQL deyimlerini biçiminde serileştirilme şeklini\# veya VB dosyası.

Görünüm oluşturulduğunda, aynı zamanda doğrulanır. Performans açısından, varlıklar arasındaki bağlantıları mantıklı ve desteklenen tüm işlemler için doğru kardinalite sahip sağlayan gerçekten doğrulanması görünümlerinin görünüm oluşturma maliyetini büyük çoğunluğu oluşur.

Bir varlık kümesi üzerindeki sorgu yürütüldüğünde, sorgu karşılık gelen sorgu görünümü ile birleştirilir ve bu bileşim sonucunu yedekleme deposu anlamak için sorgu gösterimini oluşturmak için plan derleyicide çalıştırılır. SQL Server için bu derleme sonucunu bir T-SQL SELECT deyimi olacaktır. Bir güncelleştirme bir varlık kümesi üzerinde gerçekleştirilir, ilk kez güncelleştirme görünüm hedef veritabanı için DML deyimleri dönüştürmek için benzer bir süreç aracılığıyla çalıştırılır.

### <a name="22-factors-that-affect-view-generation-performance"></a>2.2 görünüm oluşturma performansı etkileyen faktörler

Performans görünümü oluşturma adım yalnızca modelinizi boyutu aynı zamanda modelin nasıl birbirine bağlı olduğu bağlıdır. İki varlık devralma zincirini veya ilişkilendirme aracılığıyla birbirine bağlandıysa bağlandığı söylenir. Benzer şekilde iki tablo yabancı anahtar birbirine bağlandıysa, bağlı. Bağlı varlıkları ve tablolar, şemalar sayısı arttıkça, görünüm oluşturma arttıkça maliyet.

Oluşturmak ve görünümleri doğrulamak için kullanırız algoritması en kötü durumda, üstel, ancak bazı iyileştirmeler bu geliştirmek için kullanırız. Performansı olumsuz şekilde etkileyebilir görünen büyük faktörler şunlardır:

-   Varlıkların sayısı ve bu varlıklar arasında ilişkilendirmeler miktarı söz konusu model boyutu.
-   Model karmaşıklığı, özellikle çok sayıda türleri içeren devralma.
-   Bağımsız ilişkilendirmeleri, yabancı anahtar ilişkilerini yerine kullanma.

Küçük, basit modelleri için maliyet önceden üretilmiş görünümleri kullanarak rahatsız değil için küçük olabilir. Model boyutu ve karmaşıklığı arttıkça görünümü oluşturma ve doğrulama maliyetini azaltmak için kullanılabilecek birkaç seçenek vardır.

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a>2.3 kullanarak Pre-Generated modeli azaltmak için görünümleri yükleme süresi

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools"></a>2.3.1 Entity Framework güç araçları kullanarak önceden üretilmiş görünümleri

Ayrıca, model sınıfı dosyasını sağ tıklatın ve "Görünümleri Oluştur" seçmek için Entity Framework menüsünü kullanarak EDMX ve Code First modelleri, görünümleri oluşturmak için Entity Framework güç araçları kullanarak göz önünde bulundurun. Entity Framework güç araçları yalnızca DbContext türetilmiş bağlamları üzerinde çalışır ve adresinde bulunabilir \< http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d>.

Entity Framework 6 üzerinde önceden üretilmiş görünümleri kullanma hakkında daha fazla bilgi için ziyaret [Pre-Generated eşleme görünümleri](~/ef6/fundamentals/performance/pre-generated-views.md).

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a>2.3.2 nasıl EDMGen tarafından oluşturulan bir model ile önceden üretilmiş görünümleri kullanma

EDMGen Entity Framework 4 ve 5, ancak değil, Entity Framework 6 ile çalışır ve .NET ile birlikte gelen bir yardımcı programdır. EDMGen komut satırından bir model dosyası, nesne katmanı ve görünümleri oluşturmanıza olanak sağlar. Çıktıları birini VB veya C istediğiniz dilde görünümleri dosyasında olacaktır\#. Her varlık kümesinin varlık SQL kod parçacıkları içeren bir kod dosyası budur. Önceden üretilmiş görünümleri etkinleştirmek için yalnızca dosyasını projenize ekleyin.

Model için şema dosyalarını el ile yaptığınız düzenlemeler, görünümleri dosyanın yeniden oluşturmanız gerekir. EDMGen ile çalıştırarak bunu yapabilirsiniz **/mode:ViewGeneration** bayrağı.

Daha fazla başvuru için bkz. [nasıl yapılır: sorgu performansını artırmak için Pre-Generate görünümleri](https://msdn.microsoft.com/library/bb896240.aspx).

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a>2.3.3 nasıl Pre-Generated görünümlerini içeren bir EDMX dosyası kullanma

EDMGen görünümleri bir EDMX dosyası oluşturmak için de kullanabilirsiniz - daha önce başvurulan MSDN konusu - Bunu yapmak için bir derleme öncesi olay eklemeyi açıklar ancak bu karmaşık bir işlemdir ve burada mümkün olmayan bazı durumlar vardır. Genellikle, model bir edmx dosyası olduğunda görünümleri oluşturmak için T4 şablonu kullanmak daha kolaydır.

Görünümü oluşturmak için T4 şablonu kullanmayı açıklayan bir gönderi ADO.NET ekibi blogu vardır ( \< http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>). Bu gönderinin indirilir ve projenize eklediğiniz bir şablonu içerir. Entity Framework'ün en son sürümleri ile çalışmak için garantili olmayan şekilde şablonu, Entity Framework'ü ilk sürümü için yazılmıştır. Ancak, daha güncel bir görünüm oluşturma şablon yelpazesine Entity Framework 4 ve 5from için Visual Studio Galerisi indirebilirsiniz:

-   VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d>
-   C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d>

Entity Framework 6 kullanıyorsanız, görünüm oluşturma T4 şablonlarını konumundaki Visual Studio Galerisi'nden alabileceğiniz \< http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.

#### <a name="234-how-to-use-pre-generated-views-with-a-code-first-model"></a>2.3.4 nasıl bir Code First modeli Pre-Generated görünümlerini kullanma

Ayrıca, önceden üretilmiş görünümleri Code First projesiyle kullanmak da mümkündür. Entity Framework güç araçları, kod ilk projeniz için bir görünüm dosyası oluşturmak için özelliğine sahiptir. Entity Framework güç araçları Visual Studio Galerisi bulabileceğiniz \< http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d/>.

### <a name="24-reducing-the-cost-of-view-generation"></a>2.4 azaltarak maliyet görünümü oluşturma

Önceden üretilmiş görünümleri kullanarak görünüm oluşturma maliyet modeli yüklenmesini (çalışma zamanı) derleme zamanı taşır. Bu çalışma zamanında başlatma performansını artırır, ancak, geliştirirken hala görünüm oluşturma sorunları yaşar. Görünüm oluşturma, hem derleme ve çalıştırma maliyetini azaltmaya yardımcı olabilecek birkaç ek püf noktaları vardır.

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a>2.4.1 görünüm oluşturma maliyetini azaltmak için yabancı anahtar ilişkilerini kullanma

Görünüm oluşturma işleminde harcanan süreyi önemli ölçüde ilişkilendirmeleri bağımsız ilişkileri modelden yabancı anahtar ilişkileri de geçiş burada geliştirilmiş çalışmalarını bir dizi gördük.

Bu geliştirme göstermek için biz Navision modeli iki sürümünü EDMGen kullanarak oluşturulur. *Not: seeappendix Cfor Navision modelin bir açıklaması.* Varlıklar ve aralarındaki ilişkiler, çok büyük miktarda nedeniyle bu alıştırma için ilginç Navision modelidir.

Bu çok büyük bir modelin bir sürümü yabancı anahtar ilişkilerini oluşturuldu ve diğer bağımsız ilişkilerini oluşturuldu. Biz sonra görünümlerin her model için ne kadar sürdüğünü zaman aşımına uğradı. Varlık Framework5 test sınıfı EntityViewGenerator GenerateViews() yöntemden Entity Framework 6 test sınıfı StorageMappingItemCollection GenerateViews() yöntemden kullanılabilir ancak görünümleri oluşturmak için kullanılır. Bu kod Entity Framework 6 kod temelinde oluşan alanlarını yeniden yapılandırma nedeniyle.

Entity Framework 5 kullanılarak, görünüm oluşturma yabancı anahtarlara sahip olan model için bir laboratuvar makinede 65 dakika sürdü. Cihazın bilinmeyen ne kadar bağımsız ilişkilendirmeleri kullandığınız modeline görünümleri oluşturmak için alacağı. Size sunduğumuz laboratuvarda aylık güncelleştirmeleri yüklemek için makine yeniden başlatılmış önce bir ay üzerinden çalışan test kalmadı.

Entity Framework 6 kullanarak, yabancı anahtarlara sahip olan model için Görünüm oluşturma aynı Laboratuvar makinede 28 saniye sürdü. Bağımsız ilişkilerini kullanan model için Görünüm oluşturma 58 saniye sürdü. Entity Framework 6 için kendi görünüm oluşturma kodunu yapılan iyileştirmeler, çok sayıda proje daha hızlı başlangıç süreleri elde etmek için önceden üretilmiş görünümleri gerekmez anlamına gelir.

Entity Framework 4 ve 5'teki görünümleri önceden oluşturma EDMGen veya Entity Framework güç araçları ile yapılabilir açıklama önemlidir. Entity Framework güç araçları aracılığıyla ya da açıklandığı gibi program aracılığıyla oluşturma için Entity Framework 6 görünümü yapılabilir [Pre-Generated eşleme görünümleri](~/ef6/fundamentals/performance/pre-generated-views.md).

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a>2.4.1.1 nasıl yerine bağımsız ilişkilendirmeleri yabancı anahtarlar kullanın

Visual Studio'da EDMGen veya varlık Tasarımcısı kullanarak, varsayılan olarak FKs alın ve yalnızca IAS FKs arasında geçiş yapmak için tek bir onay kutusunun veya komut satırı bayrağı alır.

Büyük bir Code First modeli varsa, bağımsız ilişkilerini kullanarak görünümü oluşturma hakkında aynı etkiye sahip. Bazı geliştiriciler kendi nesne modeli kirletmesini için bu düşünür ancak sınıflarda, bağımlı nesneler için yabancı anahtar özellikleri ekleyerek bu etkiyi önleyebilirsiniz. Bu konu hakkında daha fazla bilgi bulabilirsiniz \< http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.

| Kullanırken      | Bunu yapın                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Varlık Tasarımcısı | İki varlık arasında bir ilişki eklendikten sonra bir başvuru kısıtlaması olduğundan emin olun. Başvuru kısıtlamalarını yerine bağımsız ilişkilendirmeleri yabancı anahtarları kullanmak için Entity Framework söyleyin. Ek ayrıntılar için ziyaret edin \< http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>. |
| EDMGen          | EDMGen veritabanından dosyaları oluşturmak için kullanılırken, yabancı anahtarlar dikkate ve bu nedenle modele eklenir. EDMGen tarafından kullanıma sunulan farklı seçenekler hakkında daha fazla bilgi için ziyaret [ http://msdn.microsoft.com/library/bb387165.aspx ](https://msdn.microsoft.com/library/bb387165.aspx).                           |
| İlk kod      | "İlişkisi kuralı" bölümüne bakın [kod öncelikli kurallar](~/ef6/modeling/code-first/conventions/built-in.md) Code First kullanarak bağımlı nesneler üzerinde yabancı anahtar özelliklerini eklemek hakkında daha fazla bilgi için konu.                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a>2.4.2 modeliniz için ayrı bir derleme taşıma

Proje yeniden olduğunda bile modeli değişti değildi modelinizi doğrudan uygulamanızın projeye eklenir ve derleme öncesi olay veya T4 şablonu üzerinden görünümlerini oluşturmak, görünüm oluşturma ve doğrulama gerçekleşir. Model için ayrı bir derleme taşıyın ve uygulamanızın proje başvurusu, diğer uygulamanıza modeli içeren projeyi yeniden dağıtmaya gerek kalmadan değişiklik yapabilirsiniz.

*Not:* derlemeleri ayırmak için modelinizi taşırken istemci projesinin uygulama yapılandırma dosyasına modeli için bağlantı dizelerini kopyalamayı unutmayın.

#### <a name="243-disable-validation-of-an-edmx-based-model"></a>2.4.3 edmx tabanlı bir modeli doğrulaması devre dışı bırak

Model değiştirilmemiş olsa bile, derleme zamanında EDMX modelleri doğrulanır. Modelinizi zaten doğrulandı, "Doğrulama üzerinde derleme" özelliği false olarak ayarlayarak Özellikler penceresinde derleme zamanında doğrulama gösterilmemesini sağlayabilirsiniz. Eşleme veya model değiştirdiğinizde, geçici olarak yaptığınız değişiklikleri doğrulamak için doğrulama yeniden etkinleştirebilirsiniz.

Performans geliştirmeleri için Entity Framework Designer için Entity Framework 6 yapılan ve "doğrula"derlemede maliyeti çok daha kısa designer'ın önceki sürümlerinde olduğunu unutmayın.

## <a name="3-caching-in-the-entity-framework"></a>3 önbelleğe alma varlık Çerçevesi'nde

Entity Framework, aşağıdaki biçimlerden önbelleğe alma yerleşik sahiptir:

1.  Önbelleğe alma nesnesi – ObjectContext örneği oluşturulan ObjectStateManager izleme, bu örneği kullanılarak alınan nesneleri bellekte tutar. Birinci düzey önbellek olarak da bilinen budur.
2.  Sorgu planı Caching - sorguda birden çok kez çalıştırıldığında oluşturulan depo komutu yeniden kullanılıyor.
3.  Önbelleğe alma - bir model için meta veriler aynı modelin farklı bağlantıları arasında paylaşım meta verileri.

ADO.NET veri sağlayıcısının sarmalama sağlayıcısı, veritabanından alınan sonuçları için bir önbelleği ile Entity Framework genişletmek için de kullanılabilir olarak bilinen özel bir tür ilk günden EF sağlayan önbellekler yanı sıra da ikinci düzey önbelleğe alma olarak bilinir.

### <a name="31-object-caching"></a>3.1 önbelleğe alma nesnesi

Bir varlığın bir sorgunun sonuçları döndürdüğünde varsayılan olarak aynı anahtara sahip bir varlık, ObjectStateManager zaten yüklü değilse, yalnızca EF gerçekleştiren önce ObjectContext denetler. EF aynı anahtarlara sahip bir varlık zaten varsa, sorgu sonuçlarında içerecektir. EF veritabanında sorgu hala verecek olsa da, bu davranışı çok varlığın birden çok kez gerçekleştirilmesini maliyeti atlayabilirsiniz.

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a>3.1.1 varlıkları DbContext Bul kullanarak nesneyi önbellekten alma

Normal bir sorgu olan DB (EF 4.1 ilk kez dahil edilen API'ler) bulma yöntemi bir arama bellekte veritabanında sorgu dahi vermeden önce gerçekleştirir. İki farklı ObjectContext örneği ayrı bir nesne önbellekler sahip oldukları anlamına gelir, iki farklı ObjectStateManager örnekleri olduğunu unutmamak önemlidir.

Bul, bağlam tarafından izlenen bir varlığın bulmaya için birincil anahtar değeri kullanır. Varlık bağlamı değilse, bir sorgu yürütülen sonra veritabanına karşı değerlendirilir ve varlık bağlamı veya veritabanında bulunmazsa null döndürülür. Bul bağlamına eklenmiş ancak veritabanı henüz kaydedilmedi varlıklar döndürdüğüne dikkat edin.

Bul kullanırken gerçekleştirilecek bir performans artışı yoktur. Varsayılan olarak bu yöntem çağrılarına tamamlama veritabanı hala bekleyen değişikliklerini algılamak için nesne önbelleği doğrulanması tetikler. Bu işlem, nesne önbelleği veya nesne önbelleğe eklenen bir büyük nesne grafiği nesnelerin çok büyük bir sayı ise çok pahalı olabilir, ancak bunu ayrıca devre dışı bırakılabilir. Bazı durumlarda, büyüklük kertesinde otomatik devre dışı bıraktığınızda yöntemi algılamak Bul çağırma farkının üzerinden değişiklikleri algılayabileceğini dikkatle. Henüz veritabanından alınacak nesne sahip nesne aslında karşı önbelleğinde olduğunda ve ikinci bir büyüklük algılanır. 5000 varlıkların bir yük, milisaniye cinsinden, bizim microbenchmarks bazılarını kullanarak gerçekleştirilen ölçümleri ile örnek bir grafik aşağıda verilmiştir:

![Net45LogScale](~/ef6/media/net45logscale.png ".NET 4.5 - Logaritmik ölçek")

Devre dışı auto-detect değişikliklerle örnek bulabilirsiniz:

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

Find yöntemi kullanırken dikkate alınması gereken sahip aşağıdaki gibidir:

1.  Nesne önbellekte değilse Bul avantajlarını tasarruflarını, ancak bir sorgu anahtarı tarafından hala basit bir sözdizimidir.
2.  Değişiklikleri otomatik algıla durumunda etkin maliyetini bir bulma yöntemi bir büyüklük sırası veya nesne önbelleğinizi varlıklarda miktarı ve modelinizi karmaşıklığına bağlı olarak daha da artırabilirsiniz.

Ayrıca, yalnızca Bul etkilenebileceğini aradığınız varlık döndürür ve değil zaten nesne önbellekte olmaları durumunda bunu otomatik olarak ilişkili varlıkları yükleri yapar. İlişkili varlıklar almanız gerekiyorsa, anahtar ile istekli yükleme tarafından bir sorgu kullanabilirsiniz. Daha fazla bilgi için **8.1 vs yavaş yükleniyor. İstekli yükleme**.

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a>3.1.2 nesne önbelleği birçok varlığın olduğunda performans sorunları

Nesne önbelleği, Entity Framework'ün genel yanıt hızını artırmak için yardımcı olur. Ancak, nesne önbelleği çok büyük miktarda varlıklar ekleme gibi belirli işlemlerini etkileyebilir yüklü olduğunda, Kaldır, Bul, giriş, SaveChanges ve daha fazlası. Özellikle, DetectChanges çağrısı tetikleyen işlemler tarafından çok büyük nesne önbellekler olumsuz etkilenir. DetectChanges Nesne grafiğini doğrudan Nesne grafiğini boyutu tarafından belirlenir, performans edecek ve Nesne Durum Yöneticisi ile eşitler. DetectChanges hakkında daha fazla bilgi için bkz: [POCO varlık değişiklikleri izleme](https://msdn.microsoft.com/library/dd456848.aspx).

Entity Framework 6 kullanırken, geliştiricilerin AddRange işlemi ve RemoveRange üzerindeki bir koleksiyona yineler ve Ekle örnek başına bir kez çağırmak yerine doğrudan bir olan DB, çağırma olanağına sahip olursunuz. Aralık yöntemleri kullanmanın avantajı DetectChanges maliyetini yalnızca bir kez eklenen her varlık başına bir kez yerine varlıkların tamamını için ödenen emin olan.

### <a name="32-query-plan-caching"></a>3.2 sorgu planı'önbelleğe alma

İlk kez bir sorgu yürütülür, bu depolama komutu (örneğin, T-SQL Server karşı çalıştırdığınızda yürütülür, SQL) kavramsal sorgu küçültmesini iç planı derleyici geçer.  Sorgu planını önbelleğe alma etkinse, sonraki açışınızda sorgu deposu yürütülen komut yürütme planı derleyici atlama için sorgu planı önbellek doğrudan alınır.

Sorgu planını önbelleğe aynı AppDomain içinde ObjectContext örnekleri arasında paylaşılır. Sorgu planını önbelleğe alma yararlanmak için bir ObjectContext örneği tutun gerek yoktur.

#### <a name="321-some-notes-about-query-plan-caching"></a>3.2.1 bazı sorgu planı önbelleğe alma ile ilgili notlar

-   Tüm türleri sorgulamak için sorgu planı önbelleği şu şekilde paylaşılır: Varlık SQL, LINQ to Entities ve CompiledQuery nesneleri.
-   Varsayılan olarak, sorgu planını önbelleğe alma varlık SQL sorgularında ObjectQuery veya bir EntityCommand aracılığıyla yürütülen olmadığını etkin. Ayrıca varsayılan için LINQ to Entities sorgularında Entity Framework, .NET 4.5 ve Entity Framework 6 etkin
    -   Sorgu planını önbelleğe alma (şirket EntityCommand veya ObjectQuery) EnablePlanCaching özelliği false olarak ayarlayarak devre dışı bırakılabilir. Örneğin:
``` csharp
                    var query = from customer in context.Customer
                                where customer.CustomerId == id
                                select new
                                {
                                    customer.CustomerId,
                                    customer.Name
                                };
                    ObjectQuery oQuery = query as ObjectQuery;
                    oQuery.EnablePlanCaching = false;
```
-   Parametreli sorgular için parametrenin değerini değiştirerek önbelleğe alınmış sorgu hala ulaşırsınız. Ancak, bir parametrenin facets (örneğin, boyutu, duyarlık veya Ölçek) değiştirerek farklı bir önbellek girdisi ulaşırsınız.
-   Entity SQL kullanılırken, sorgu dizesi anahtarı bir parçasıdır. Sorgu hiç değiştirilmesi, sorguları işlevsel olarak eşdeğerdir olsa bile farklı bir önbellek girişlerinde neden olacak. Bu, büyük/küçük harf veya boşluk değişiklikleri içerir.
-   LINQ kullanırken, bir anahtarın parçası oluşturmak için sorguyu işlenir. LINQ ifadesi değiştirilmesi, bu nedenle farklı bir anahtar oluşturur.
-   Başka bir teknik kısıtlamalar geçerli olabilir; Autocompiled sorguları daha fazla ayrıntı için bkz.

#### <a name="322------cache-eviction-algorithm"></a>3.2.2 önbellek çıkarma algoritması

İç algoritmaya works etkinleştirebilir veya devre dışı bırakma sorgu planını önbelleğe alma, anlayabilir nasıl yardımcı olabileceğini anlama. Temizleme algoritması aşağıdaki gibidir:

1.  Biz, önbellek girdileri (800) ayarlama sayısını içeren sonra düzenli aralıklarla (bir kez dakikada) önbelleği temizleyen, Zamanlayıcı başlatın.
2.  Önbellek süpürmeleri sırasında bir LFRU (sık – son kullanılan az) üzerinde önbellekten girdiler kaldırılır temel. Bu algoritma hem isabet sayısı ve yaş hangi girişlerin çıkarıldı karar verirken dikkate alır.
3.  Her önbellek Süpürme sonunda, önbelleği yeniden 800 girişler içeriyor.

Tüm önbellek girişlerinin, çıkarmak için hangi girişlerin belirlerken eşit olarak kabul edilir. Bu depolama komutu bir CompiledQuery için çıkarma varlık SQL sorgusu için depolama komutu olarak aynı şansı anlamına gelir.

Önbellek çıkarma Zamanlayıcı önbellekte 800 varlık vardır, ancak önbellek yalnızca 60 saniye sonra bu Zamanlayıcı başlatıldığında gözden geçirilmiştir devreye girdi olmadığını unutmayın. Bu, çok büyük 60 saniye için önbelleğinizi büyüme anlamına gelir.

#### <a name="323-------test-metrics-demonstrating-query-plan-caching-performance"></a>3.2.3 sorgu planını önbelleğe alma performans gösteren ölçümleri test

Sorgu planını önbelleğe alma, uygulamanızın performansı etkisini göstermek için bir test ediyoruz Navision model Entity SQL sorguları bir dizi yürütüldüğü gerçekleştirdiğimiz. Ek Navision modeli ve yürütüldü sorguları türde bir açıklaması için bkz. Bu test ediyoruz önce sorguları listesi boyunca yineleme yapmak ve her bir kez (önbelleğe alma etkinse) bunları önbelleğine eklemek için çalıştırın. Bu adım untimed bağlıdır. Ardından, ana iş parçacığı gerçekleşmesi için üst düzey önbellek izin vermek tekrar 60 saniye için uyku; Son olarak, önbelleğe alınan sorgularını yürütmek için 2. bir liste zaman yineleme. Ayrıca, böylece doğru bir şekilde elde zamanları sorgu planı önbelleği tarafından verilen avantajı sorgular kümelerine yürütülmeden önce yaptığı SQL Server planı önbellek temizlenir.

##### <a name="3231-------test-results"></a>3.2.3.1 test sonuçları

| Test                                                                   | EF5 önbellek yok | Önbelleğe alınmış EF5 | EF6 önbellek yok | Önbelleğe alınmış EF6 |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| Tüm 18723 sorguları numaralandırma                                          | 124          | 125.4      | 124.3        | 125.3      |
| Gözden geçirme (yalnızca ilk 800 sorgular, karmaşıklığı bağımsız olarak) önleme  | 41.7         | 5.5        | 40.5         | 5,4        |
| Yalnızca AggregatingSubtotals sorgular (hangi Süpürme önler 178 toplam -) | 39.5         | 4,5        | 38,1         | 4.6        |

*Her zaman saniyelerle.*

Moral - yürütme birçok olduğunda (örneğin sorguları dinamik olarak oluşturulan), ayrı sorguların önbelleğe alma için yardımcı olmaz ve sonuçta elde edilen önbelleğini temizlemeye gerçekten kullanımından planı en önbelleğe alınan avantaj elde edecektir sorguları tutabilirsiniz.

AggregatingSubtotals sorgular ile test edilen sorguların en karmaşık kullanılmaktadır. Daha karmaşık olan sorguyu sorgu planını önbelleğe alma görürsünüz daha fazla avantaj beklendiği gibi.

Bir CompiledQuery gerçekten bir LINQ Sorgu önbelleğe kendi planına sahip olduğundan, eşdeğer varlık SQL sorgusu karşı bir CompiledQuery karşılaştırması benzer sonuçlar olmalıdır. Bir uygulamanın dinamik Entity SQL sorguları çok sayıda varsa, aslında, önbellek sorgularla doldurma da verimli bir şekilde "önbellekten Temizlenen olduğunda derlemeyi" CompiledQueries neden olur. Bu senaryoda CompiledQueries önceliğini belirlemek için dinamik sorgular üzerinde önbelleğe alma devre dışı bırakarak performans artırılabilir. Üstelik, parametreli sorgular yerine dinamik sorgular kullanmak için uygulamayı yeniden olacaktır.

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a>3.3 LINQ sorguları ile performansı CompiledQuery kullanma

Testlerimiz CompiledQuery kullanarak bir yararı % 7, autocompiled LINQ sorguları getirebilirsiniz olduğunu gösterir; Bu, daha az zaman Entity Framework yığından kod yürütülürken % 7 geçireceksiniz anlamına gelir; Uygulamanızı %7 daha hızlı olacak gelmez. Genel olarak bakıldığında, yazma ve EF 5.0 CompiledQuery nesneleri bakım maliyeti avantaj karşılaştırıldığında sorun değer olmayabilir. Mesafe farklı olduğundan, ek anında iletme projeniz gerektiriyorsa, bu nedenle bu seçeneği alıştırma. CompiledQueries yalnızca ObjectContext türetilmiş modellerle uyumlu ve DbContext türetilmiş modellerle uyumlu olduğunu unutmayın.

Oluşturma ve bir CompiledQuery çağırma hakkında daha fazla bilgi için bkz. [derlenmiş sorgular (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).

CompiledQuery, sahip oldukları composability ile statik örnekleri ve sorunları kullanma özelliği gereksinimi kullanırken yapmanız gereken iki önemli noktalar vardır. Bu iki noktalar ayrıntılı bir açıklamasını buraya izler.

#### <a name="331-------use-static-compiledquery-instances"></a>3.3.1 statik CompiledQuery örnekleri kullan

LINQ sorgusu derleme zaman alan bir işlem olduğundan, biz bunu yapmanın veritabanından veri getirme ihtiyacımız her zaman istemezsiniz. Bir kez derlemek ve birden çok kez çalıştırmak CompiledQuery örnekleri izin, ancak dikkatli olması ve aynı CompiledQuery örneği üzerinde yeniden derlemek yerine her zaman yeniden kullanmak için tedarik edin. Statik üyeleri CompiledQuery örneklerini depolamak için kullanımını gerekli hale gelir; Aksi takdirde, hiçbir avantajı görmezsiniz.

Örneğin, seçilen kategori ürünleri görüntüleme işlemek için aşağıdaki yöntem gövdesini sayfanız olduğunu varsayın:

``` csharp
    // Warning: this is the wrong way of using CompiledQuery
    using (NorthwindEntities context = new NorthwindEntities())
    {
        string selectedCategory = this.categoriesList.SelectedValue;

        var productsForCategory = CompiledQuery.Compile<NorthwindEntities, string, IQueryable<Product>>(
            (NorthwindEntities nwnd, string category) =>
                nwnd.Products.Where(p => p.Category.CategoryName == category)
        );

        this.productsGrid.DataSource = productsForCategory.Invoke(context, selectedCategory).ToList();
        this.productsGrid.DataBind();
    }

    this.productsGrid.Visible = true;
```

Bu durumda, yöntem her çağrıldığında, çalışma sırasında yeni bir CompiledQuery örneği oluşturacaksınız. Sorgu planı önbellekten depo komutu alarak performans avantajlarının görmenin yerine, yeni bir örneği oluşturulduğunda CompiledQuery planı derleyici geçer. Yöntem her çağrıldığında aslında, sorgu planı önbelleğinizi yeni CompiledQuery girdisi ile kirletmesini.

Bunun yerine, yöntem her çağrıldığında aynı derlenmiş sorgu çağırdığınız şekilde derlenmiş sorgu statik bir örneğini oluşturmak istiyorsunuz. Yollarından biri bunu nesne Bağlamınızı bir üyesi olarak CompiledQuery örneği ekleyerek, bu nedenle.  Öğeleri küçük temizleyici CompiledQuery bir yardımcı yöntem aracılığıyla erişerek daha sonra yapabilirsiniz:

``` csharp
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IEnumerable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
            );

        public IEnumerable<Product> GetProductsForCategory(string categoryName)
        {
            return productsForCategoryCQ.Invoke(this, categoryName).ToList();
        }
```

Bu yardımcı yöntem şu şekilde çağrılması:

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-------composing-over-a-compiledquery"></a>3.3.2 bir CompiledQuery oluşturma

Özelliği herhangi bir LINQ sorgu oluşturmak için son derece kullanışlıdır. Bunu yapmak için yalnızca bir yöntem sonra Iqueryable gibi çağırma *Skip()* veya *Count()*. Ancak, girebiliyorsunuz yapılması, yeni bir Iqueryable nesnesi döndürür. Yeni bir Iqueryable nesne oluşturmayı, neden olacak, böylece varken bir CompiledQuery oluşturma gelen teknik olarak durdurmak için hiçbir şey, planı derleyici ölçeklendirilebilirlikten yeniden gerektirir.

Bazı bileşenler oluşan Iqueryable kullanın hale getirecek gelişmiş işlevselliği etkinleştirmek için nesneleri. Örneğin, ASP. NET GridView veri-Iqueryable nesneye SelectMethod özelliği üzerinden bağlanmış olabilir. GridView veri modeli üzerinde sayfalama ve sıralama izin vermek için bu Iqueryable nesne üzerinde compose. Gördüğünüz gibi bir CompiledQuery için GridView kullanarak derlenmiş sorgu isabet değil ancak yeni bir autocompiled sorgu oluşturur.

Müşteri danışma ekibi bunu kendi "Olası performans sorunları ile derlenmiş LINQ Sorgu yeniden derler" blog gönderisinde açıklanmaktadır: <http://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/potential-performance-issues-with-compiled-linq-query-re-compiles.aspx>.

Bir sorgu için aşamalı filtreler ekleyerek bu burada çalışabilir tek bir yerde andır. Örneğin, çeşitli açılan listeleri için isteğe bağlı filtreler (örneğin, ülke ve OrdersCount) müşteriler sayfasıyla sahip olduğunu varsayalım. Bir CompiledQuery Iqueryable sonuçları üzerinde bu filtreler oluşturabilirsiniz ancak bunun yapılması, bu nedenle planı Derleyici bunu her zaman giderek yeni sorguda neden olur.

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployee();

        if (this.orderCountFilterList.SelectedItem.Value != defaultFilterText)
        {
            int orderCount = int.Parse(orderCountFilterList.SelectedValue);
            myCustomers = myCustomers.Where(c => c.Orders.Count > orderCount);
        }

        if (this.countryFilterList.SelectedItem.Value != defaultFilterText)
        {
            myCustomers = myCustomers.Where(c => c.Address.Country == countryFilterList.SelectedValue);
        }

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Bu yeniden derleme kaçınmak için mümkün filtreler dikkate almanız CompiledQuery yazabilirsiniz:

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

Olduğu gibi kullanıcı arabiriminde çağrıldığı:

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        int? countFilter = (this.orderCountFilterList.SelectedIndex == 0) ?
            (int?)null :
            int.Parse(this.orderCountFilterList.SelectedValue);

        string countryFilter = (this.countryFilterList.SelectedIndex == 0) ?
            null :
            this.countryFilterList.SelectedValue;

        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployeeWithFilters(
                countFilter, countryFilter);

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Oluşturulan depo komutu her zaman null denetimleri filtrelerle olacaktır, ancak bunlar veritabanı sunucusu için en iyi duruma getirmeyi oldukça basit olmalıdır bir tradeoff burada verilmiştir:

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a>3.4 meta verileri önbelleğe alma

Entity Framework meta verileri önbelleğe almayı da destekler. Bu temelde tür bilgileri ve tür veritabanı eşleme bilgileri aynı modelin farklı bağlantıları arasında önbelleğe alma. Meta veri önbelleği AppDomain benzersiz değil.

#### <a name="341-metadata-caching-algorithm"></a>3.4.1 meta verileri önbelleğe alma algoritması

1.  Bir model için meta veri bilgileri için her bir EntityConnection bir ItemCollection içinde depolanır.
    -   Yan Not olarak modelin farklı bölümleri için farklı ItemCollection nesnesi vardır. Örneğin, StoreItemCollections veritabanı modeli hakkında bilgi içerir. ObjectItemCollection veri modeli hakkında bilgi içerir. Edmıtemcollection kavramsal modelle ilgili bilgiler içerir.

2.  İki bağlantı aynı bağlantı dizesi kullanıyorsanız, bunlar aynı ItemCollection örneği paylaşır.
3.  Eşdeğer işleve sahiptir ancak metin içeriğini eklemek farklı bağlantı dizeleri farklı meta veri önbelleği neden olabilir. Biz bağlantı dizelerini, bu nedenle yalnızca belirteçleri sırasını değiştirmek paylaşılan meta verilerinde sonuçlanmalıdır simgeleştirin. Ancak, işlevsel olarak aynı görünen iki bağlantı dizesi aynı sonra simgeleştirme değerlendirilmeyebilir.
4.  ItemCollection kullanım için düzenli olarak denetlenir. Bir çalışma alanı yakın zamanda eriştiğini değil belirlenirse sonraki önbellek Süpürme üzerinde Temizleme için işaretlenir.
5.  Yalnızca bir EntityConnection oluşturma (bağlantı açılıncaya kadar içindeki öğe koleksiyonlarını başlatılmayacak rağmen) oluşturulması için bir meta veri önbelleği neden olur. Bu çalışma alanı bellek içi ermesine "kullanımda" değil, önbelleğe alma algoritmasını belirler.

Müşteri danışma ekibi, "kullanımdan kaldırma" büyük modellerin kullanırken önlemek için bir ItemCollection bir başvuru tutan açıklayan bir blog gönderisi yazmıştır: \< http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a>3.4.2 arasındaki ilişkiyi meta verileri önbelleğe alma ve sorgu planı önbelleğe alma

Sorgu planı önbellek örneği depolama türlerinin MetadataWorkspace'nın ItemCollection yaşar. Bu, önbelleğe alınan depolama komutları sorguları kullanarak belirli bir MetadataWorkspace örneği her bağlam için kullanılacak anlamına gelir. Ayrıca, biraz farklıdır ve belirteç oluşturma sonrasında eşleşmeyen iki bağlantı dizeleri varsa, önbellek örnekleri plan farklı sorgu olacağı anlamına gelir.

### <a name="35-results-caching"></a>3.5 sonuçları önbelleğe alma

Önbelleğe alma sonuçları (diğer adıyla "ikinci düzey önbelleğe alma") ile sorguların sonuçlarını bir yerel önbellek üzerinde tutun. Bir sorgu verme ilk sonuçları, sorgu deposu ile karşılaştırarak önce yerel olarak kullanılabilir olup olmadığını görürsünüz. Sonuçları önbelleğe alma, Entity Framework tarafından doğrudan desteklenmiyor olsa da bir sarmalama sağlayıcısını kullanarak ikinci bir düzey önbellek eklemek mümkündür. Bir örnek kaydırma, ikinci düzey önbellek Alachisoft'ın sağlayıcısıdır [Entity Framework ikinci düzey önbellek üzerinde NCache tabanlı](http://www.alachisoft.com/ncache/entity-framework.html).

Bu uygulama, ikinci düzey önbelleğe alma ve sorgu yürütme planı hesaplanan ya da birinci düzey önbellekten LINQ ifadesi değerlendirildikten sonra bir yerde (ve funcletized) eklenen bir işlevdir. Materialization ardışık düzen yine de daha sonra yürütür için ikinci düzey önbellek sonra yalnızca ham veritabanı sonuçlarını depolar.

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a>3.5.1 sonuçlar sarmalama sağlayıcısında önbelleğe alma için ek başvurular

-   Windows Server AppFabric önbelleğe almayı kullanmak üzere örnek sarmalama sağlayıcı güncelleştirme içeren bir "İkinci düzey önbelleğe alma, Entity Framework ve Windows Azure" MSDN makalesi Julie Lerman yazmıştır: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)
-   Entity Framework 5 ile çalışıyorsanız, önbelleğe alma sağlayıcısı için Entity Framework 5 ile çalışan gerçekleştirmeyi açıklayan bir gönderi ekibi blogu vardır: \< http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>. 2. düzey projenize önbelleğe almanın eklenmesi otomatikleştirmeye yardımcı olmak için T4 şablonu da içerir.

## <a name="4-autocompiled-queries"></a>4 Autocompiled sorguları

Entity Framework kullanarak bir veritabanında bir sorgu verildiğinde, bir dizi adım ile gerçekten sonuçları düzeniyle önce gitmelidir; Böyle bir adım sorgu derlemesi ' dir. Entity SQL sorguları otomatik olarak önbelleğe gibi ikinci veya üçüncü kez planı derleyici atlayın ve bunun yerine önbellekteki plan kullanın aynı sorgu yürütmek için iyi bir performans sağlamak için bilinirdi.

Entity Framework 5 otomatik LINQ için de Entities sorgularında önbelleğe kullanıma sunuldu. Bu, LINQ Entities sorgusunda önbelleğe alınabilir hale getirir Entity Framework hızlandırmak için bir CompiledQuery oluşturma önceki sürümlerinde yaygın bir uygulama performansınızı kaydedildi. Önbelleğe alma artık otomatik olarak bir CompiledQuery kullanmadan yapılır olduğundan, "autocompiled sorguları" Bu özellik diyoruz. Sorgu planını önbelleğe alma sorgu planı önbellek ve onun mekanizması hakkında daha fazla bilgi için bkz.

Entity Framework derlenmesi için bir sorgu gerektirdiğinde algılar ve sorgu çağrıldığında, önce derlenen olsanız bile bunu yapar. Derlenmesi için sorguyu neden olan genel koşullar şunlardır:

-   Sorgunuz için ilişkili MergeOption değiştiriliyor. Önbelleğe alınmış sorgu kullanılmayacak, bunun yerine planı derleyici yeniden çalıştırın ve yeni oluşturulan plan önbelleğe.
-   ContextOptions.UseCSharpNullComparisonBehavior değerinin değiştirilmesi. MergeOption değiştirme aynı etkiye sahip olursunuz.

Diğer koşullar, sorgu önbelleği kullanılmasını önleyebilir. Ortak örnekler verilmiştir:

-   IEnumerable kullanarak&lt;T&gt;. İçeren&lt;&gt;(T değeri).
-   Sabitler ile sorguları oluşturan işlevleri'ni kullanarak.
-   Eşlenen olmayan bir nesne özelliklerini kullanma.
-   Sorgunuzu yeniden derlenmesi gerektiren başka bir sorgu bağlama.

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a>4.1 IEnumerable kullanarak&lt;T&gt;. İçeren&lt;T&gt;(T değeri)

Entity Framework IEnumerable çağırma sorguları önbelleğe almaz&lt;T&gt;. İçeren&lt;T&gt;(T değeri) karşı değerleri koleksiyonun geçici olarak kabul edilir bu yana bir bellek içi koleksiyonu. Her zaman planı derleyici tarafından işlenmesi için aşağıdaki örnek sorguda, önbelleğe alınacak değil:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var query = context.MyEntities
                    .Where(entity => ids.Contains(entity.Id));

    var results = query.ToList();
    ...
}
```

Hangi içerir karşı IEnumerable boyutunu yürütülen ne kadar hızlı ya da nasıl yavaş sorgunuzu derlenir belirler unutmayın. Önemli ölçüde yukarıdaki örnekte gösterildiği gibi büyük koleksiyonlar kullanırken performans olumsuz etkilenebilir.

Entity Framework 6 içeren iyileştirmeleri IEnumerable şekilde&lt;T&gt;. İçeren&lt;T&gt;sorgu yürütüldüğünde (T değeri) çalışır. Oluşturulan SQL kodu oluşturmak için çok daha hızlı ve daha okunabilir ve çoğu durumda, ayrıca daha hızlı sunucuda yürütür.

### <a name="42-using-functions-that-produce-queries-with-constants"></a>4.2 sabitleri sorgularla üreten işlevlerini kullanma

Skip(), Take() Contains() ve DefautIfEmpty() LINQ işleçleri parametreleri içeren SQL sorguları üretmez, ancak bunun yerine bunları sabitleri geçirilen değerleri koyun. Bu nedenle, aksi takdirde sorgu kirletmesini yukarı aynı uç olabilecek sorguları EF yığını ve veritabanı sunucusu önbellek, planlayın ve aynı sabit bir sonraki sorgu yürütme kullanılmadığı sürece reutilized değil. Örneğin:

``` csharp
var id = 10;
...
using (var context = new MyContext())
{
    var query = context.MyEntities.Select(entity => entity.Id).Contains(id);

    var results = query.ToList();
    ...
}
```

Bu örnekte, bu sorgu kimliği sorgu için farklı bir değerle yürütülen her zaman yeni bir plan ile derlenir.

Belirli dikkat edin atlama ve disk belleği yaparken Al. EF6 çünkü EF bu yönteme geçirilen değişkenleri yakalamak ve bunları çevirmesine SQLparameters için önbelleğe alınmış sorgu planı etkili bir şekilde yeniden kullanılabilir hale getirir bir lambda aşırı bu yöntemleri vardır. Bu da aksi takdirde, her sorgu atlama ve alma için farklı bir sabit ile kendi sorgu planı önbellek girişi elde edebileceğiniz bu yana önbellek temiz tutmaya yardımcı olur.

Yetersiz ancak yalnızca bu sınıf sorgu exemplify yönelik aşağıdaki kodu düşünün:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Bir lambda ile Atla çağırma aynı bu kodu daha hızlı bir sürümünü içerir:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i \< count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Aynı sorgu planı kullanıldığından sorgu, CPU süresi kaydeder ve sorgu önbelleği kirletmesini önler her çalıştırıldığında, ikinci kod parçacığı %11 kadar daha hızlı çalışabilir. İçinde bir kapanış atla parametresi olduğu için Ayrıca, kodu de artık şuna benzeyebilir:

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a>4.3 eşlenen olmayan bir nesne özelliklerini kullanma

Ne zaman bir parametre sonra sorgu önbelleğe şekilde sorgu olmayan eşlenmiş nesne türünün özelliklerini kullanır. Örneğin:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();

    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myObject.MyProperty)
                select entity;

   var results = query.ToList();
    ...
}
```

Bu örnekte, sınıf NonMappedType varlık modeli parçası olmadığını varsayar. Bu sorgu, eşlenen olmayan bir tür değil ve bunun yerine sorgu parametresi olarak bir yerel değişken kullanmak kolayca değiştirilebilir:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();
    var myValue = myObject.MyProperty;
    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myValue)
                select entity;

    var results = query.ToList();
    ...
}
```

Bu durumda, sorgu önbelleğe mümkün olacaktır ve sorgu planı önbellekten Kurum için avantaj sağlayacaktır.

### <a name="44-linking-to-queries-that-require-recompiling"></a>4.4 yeniden derlemeden gerektiren sorguları bağlama

Derlenmesi gereken bir sorguya dayalı ikinci bir sorgu varsa yukarıdakilerle aynı örneği izleyerek, ayrıca tüm ikinci sorgunuzu derlenecek. Bu senaryoyu göstermek üzere bir örnek aşağıda verilmiştir:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var firstQuery = from entity in context.MyEntities
                        where ids.Contains(entity.Id)
                        select entity;

    var secondQuery = from entity in context.MyEntities
                        where firstQuery.Any(otherEntity => otherEntity.Id == entity.Id)
                        select entity;

    var results = secondQuery.ToList();
    ...
}
```

Örnek geneldir ancak nasıl, firstQuery için bağlama secondQuery önbelleğe başlatılamamasına neden olduğunu gösterir. FirstQuery yeniden derlemeden gerektiren bir sorgu değil olsaydı, ardından secondQuery önbelleğe alınan.

## <a name="5-notracking-queries"></a>5 NoTracking sorguları

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a>5.1 Durum Yönetim yükünü azaltmak için değişiklik devre dışı bırakma

Salt okunur bir senaryoda ve yüklenen nesneler Objectstatemanager'da ek yükü ortadan kaldırmak istiyorsanız, "No izleme" sorgu iletebilirsiniz.  Değişiklik izleme sorgu düzeyinde devre dışı bırakılabilir.

Ancak, değişiklik, izleme devre dışı bırakarak etkin nesne önbelleği devre dışı açıyorsunuz olduğunu unutmayın. Bir varlık için sorguladığınızda, biz ObjectStateManager daha önce gerçekleştirilmiş sorgu sonuçları çekerek materialization atlayamazsınız. Tekrar tekrar aynı içerik üzerinde aynı varlıklar için sorgu oluşturuyorsanız, değişiklik izleme kaldırmadan yararlanabilecek bir performans gerçekten görebilirsiniz.

ObjectContext kullanarak sorgulanırken ObjectQuery ve ObjectSet örnekleri ayarlanır ve bunlar üzerinde oluşan sorguları ana sorgunun etkili MergeOption devralır sonra bir MergeOption hatırlanır. DbContext kullanırken, izleme üzerinde olan DB AsNoTracking() değiştiricisi çağırarak devre dışı bırakılabilir.

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a>5.1.1 değişiklik DbContext kullanırken bir sorgu için izlemeyi devre dışı bırakma

Sorgudaki AsNoTracking() yönteme bir çağrı zinciri tarafından bir sorgu modunu NoTracking için geçiş yapabilirsiniz. ObjectQuery DbContext API olan DB ve DbQuery sınıfları için MergeOption değişebilir bir özellik yok.

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a>5.1.2 değişiklik izleme ObjectContext kullanarak sorgu düzeyinde devre dışı bırakma

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a>5.1.3 değişiklik izleme ObjectContext kullanarak tüm varlık için devre dışı bırakma

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52-test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a>5.2 NoTracking sorguların performans avantajı gösteren test ölçümleri

Bu sınamada Navision modelin NoTracking sorguları izleme karşılaştırarak ObjectStateManager doldurma karşılığında bakacağız. Ek Navision modeli ve yürütüldü sorguları türde bir açıklaması için bkz. Bu test sorguları listesi boyunca yineleme yapmak ve her birini bir kere yürütülen. Test, bir kez NoTracking sorgularla de iki çeşidi "AppendOnly" varsayılan birleştirme seçeneği ile karşılaştık. Biz, her 3 kez çalıştırıldı ve çalıştırmalar ortalama değerini alın. Testleri arasında SQL Server'da sorgu önbelleği temizlemek ve aşağıdaki komutları çalıştırarak tempdb Daralt:

1.  DBCC DROPCLEANBUFFERS
2.  DBCC FREEPROCCACHE
3.  DBCC SHRINKDATABASE (tempdb, 0)

Test sonuçları, ORTANCA 3 çalışır:

|                        | İZLEME – ÇALIŞMA KÜMESİ | İZLEME YOK-ZAMAN | ÇALIŞMA KÜMESİ YALNIZCA – EKLEME | YALNIZCA – ZAMAN EKLE |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| **Entity Framework 5** | 460361728                 | 1163536 ms         | 596545536                 | 1273042 ms         |
| **Entity Framework 6** | 647127040                 | 190228 ms          | 832798720                 | 195521 ms          |

Entity Framework 5, Entity Framework 6 daha bir daha küçük bellek Ayak izi çalıştırma sonunda sahip olur. Entity Framework 6 tarafından kullanılan ek bellek ek bellek yapılar ve yeni özellikleri ve daha iyi performans sağlayan kod sonucudur.

Aynı zamanda ObjectStateManager kullanırken aynı zamanda bellek Ayak izi NET bir fark yoktur. Entity Framework 5, Ayak izi biz veritabanından gerçekleştirilmiş tüm varlıkları izler, 30 oranında artırıldı. Entity Framework 6 yapıldığında, Ayak izi % 28'artırdık.

Zaman açısından, Entity Framework 6 Entity Framework 5 bu test büyük bir kenar boşluğu ile çok daha iyi. Entity Framework 6 yaklaşık %16 Entity Framework 5 tarafından tüketilen sürenin test tamamlandı. Ayrıca, Entity Framework 5 ObjectStateManager kullanıldığında tamamlamak için %9 daha fazla zaman alır. Buna karşılık, Entity Framework 6 %3 ObjectStateManager kullanırken daha fazla zaman kullanıyor.

## <a name="6-query-execution-options"></a>6 sorgu yürütme seçenekleri

Entity Framework, sorgu için çeşitli yollar sunar. Biz aşağıdaki seçenekleri göz atın, Artıları ve eksileri her karşılaştırın ve performans özelliklerini inceleyin:

-   LINQ to Entities.
-   İzleme yok LINQ to Entities'de.
-   Bir ObjectQuery üzerinden SQL varlık.
-   Varlık bir EntityCommand üzerinden SQL.
-   ExecuteStoreQuery.
-   SqlQuery.
-   CompiledQuery.

### <a name="61-------linq-to-entities-queries"></a>6.1 LINQ to Entities sorgularında

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

**Uzmanları**

-   CUD işlemleri için uygundur.
-   Tam olarak gerçekleştirilmiş nesneleri.
-   En basit söz dizimi ile yazmak programlama dilinde yerleşik.
-   İyi bir performans.

**Simgeler**

-   Bazı teknik kısıtlamalar gibi:
    -   OUTER JOIN varlık SQL deyimlerinde basit değerinden daha karmaşık sorgular için sorguların OUTER JOIN DefaultIfEmpty kullanarak desenlerini sonuçlanır.
    -   Hala kullanamazsınız genel desen eşleştirme ile benzer.

### <a name="62-------no-tracking-linq-to-entities-queries"></a>6.2 hiçbir ' % s'izleme LINQ to Entities sorgularında

Ne zaman bağlamı ObjectContext türetilir:

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

Ne zaman bağlamı DbContext türetilir:

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

**Uzmanları**

-   Normal LINQ sorguları içinde performansı İyileştirildi.
-   Tam olarak gerçekleştirilmiş nesneleri.
-   En basit söz dizimi ile yazmak programlama dilinde yerleşik.

**Simgeler**

-   CUD işlemleri için uygun değildir.
-   Bazı teknik kısıtlamalar gibi:
    -   OUTER JOIN varlık SQL deyimlerinde basit değerinden daha karmaşık sorgular için sorguların OUTER JOIN DefaultIfEmpty kullanarak desenlerini sonuçlanır.
    -   Hala kullanamazsınız genel desen eşleştirme ile benzer.

Proje skaler özellikleri sorguları NoTracking belirtilmemiş olsa bile izlenmez unutmayın. Örneğin:

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

Bu belirli bir sorgu NoTracking olan açıkça belirtmeyen ancak değil düzeniyle beri gerçekleştirilmiş sonuç nesnesi durum Yöneticisi ardından bilinen tür değil izlenir.

### <a name="63-------entity-sql-over-an-objectquery"></a>6.3 varlık ObjectQuery üzerinden SQL

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

**Uzmanları**

-   CUD işlemleri için uygundur.
-   Tam olarak gerçekleştirilmiş nesneleri.
-   Destekler planını önbelleğe alma sorgulayın.

**Simgeler**

-   Kullanıcı daha fazla hataya daha sorgu yapıları dilinde yerleşik olan metinsel sorgu dizelerini içerir.

### <a name="64-------entity-sql-over-an-entity-command"></a>6.4 varlık varlığın komut üzerinden SQL

``` csharp
EntityCommand cmd = eConn.CreateCommand();
cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
{
    while (reader.Read())
    {
        // manually 'materialize' the product
    }
}
```

**Uzmanları**

-   Plan .NET 4.0 (planını önbelleğe alma .NET 4.5 diğer tüm sorgu türleri tarafından desteklenir) önbelleğe almayı destekler sorgulayın.

**Simgeler**

-   Kullanıcı daha fazla hataya daha sorgu yapıları dilinde yerleşik olan metinsel sorgu dizelerini içerir.
-   CUD işlemleri için uygun değildir.
-   Sonuçları değil otomatik olarak gerçekleştirilmiş ve veri okuyucusundan okunması gerekir.

### <a name="65-------sqlquery-and-executestorequery"></a>6.5 SqlQuery ve ExecuteStoreQuery

Veritabanında SqlQuery:

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

Üzerinde olan DB SqlQuery:

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

ExecyteStoreQuery:

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

**Uzmanları**

-   Plan derleyici atlanır beri genellikle en hızlı performansı.
-   Tam olarak gerçekleştirilmiş nesneleri.
-   Olan DB kullanıldığında CUD işlemleri için uygundur.

**Simgeler**

-   Sorgu metinsel ve hataya açık alanlardır.
-   Sorgu deposu semantiği yerine kavramsal semantiği kullanarak belirli bir arka uca bağlıdır.
-   Devralma mevcut olduğunda, sorgu hale talep türü için eşleme koşulları için hesap gerekiyor.

### <a name="66-------compiledquery"></a>6.6 CompiledQuery

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

**Uzmanları**

-   En fazla % 7 performans iyileştirmesi normal LINQ sorguları sağlar.
-   Tam olarak gerçekleştirilmiş nesneleri.
-   CUD işlemleri için uygundur.

**Simgeler**

-   Karmaşıklık ve ek yükü programlama artırdık.
-   Performans iyileştirmesi üzerinde derlenmiş bir sorgu oluştururken kaybolur.
-   Bazı LINQ sorguları bir CompiledQuery - Örneğin, anonim tür projeksiyonları yazılamaz.

### <a name="67-------performance-comparison-of-different-query-options"></a>6.7 farklı bir sorgu seçenekleri performans karşılaştırması

Burada içerik oluşturma değil uğradı basit microbenchmarks test yerleştirilmiştir. Size bir dizi önbelleğe alınmamış varlık denetimli bir ortamda 5000 kez sorgulama ölçülür. Bu uyarı ile gerçekleştirilecek sayılardır: bir uygulama tarafından üretilen gerçek sayılar yansıtmaz, ancak bunun yerine bir performans farkı sorgulanırken farklı seçenekler karşılaştırıldığında yoktur ne kadar çok doğru ölçümü olur elma-için-yeni bir bağlam maliyetini hariç elma.

| EF  | Test                                 | Süre (ms) | Bellek   |
|:----|:-------------------------------------|:----------|:---------|
| EF5 | ObjectContext ESQL                   | 2414      | 38801408 |
| EF5 | ObjectContext LINQ sorgusu             | 2692      | 38277120 |
| EF5 | DbContext LINQ Sorgu izleme yok     | 2818      | 41840640 |
| EF5 | DbContext LINQ sorgusu                 | 2930      | 41771008 |
| EF5 | ObjectContext LINQ Sorgu izleme yok | 3013      | 38412288 |
|     |                                      |           |          |
| EF6 | ObjectContext ESQL                   | 2059      | 46039040 |
| EF6 | ObjectContext LINQ sorgusu             | 3074      | 45248512 |
| EF6 | DbContext LINQ Sorgu izleme yok     | 3125      | 47575040 |
| EF6 | DbContext LINQ sorgusu                 | 3420      | 47652864 |
| EF6 | ObjectContext LINQ Sorgu izleme yok | 3593      | 45260800 |

![EF5Micro5000Warm](~/ef6/media/ef5micro5000warm.png)

![EF6Micro5000Warm](~/ef6/media/ef6micro5000warm.png)

Microbenchmarks çok kodu küçük değişikliklere duyarlıdır. Bu durumda, Entity Framework 5 maliyetlerini ve Entity Framework 6 arasındaki farkı olan eklenmesi nedeniyle [durdurma](~/ef6/fundamentals/logging-and-interception.md) ve [işlem geliştirmeleri](~/ef6/saving/transactions.md). Bu microbenchmarks numaraları, ancak, yükseltilmiş bir işleme Entity Framework yapar, çok küçük bir parça halinde uygulanır. Orta Gecikmeli sorgular, gerçek hayat senaryolarında, Entity Framework 6 için Entity Framework 5'ten yükseltme yaparken performans regresyon açmamalıdır.

Farklı bir sorgu seçeneklerini gerçek performansını karşılaştırmak için burada farklı sorgu seçeneği kategori adı "İçecekler" olan tüm ürünleri seçmek için kullanırız 5 ayrı test çeşitlemeleri oluşturduk. Her yineleme, bağlam maliyetini ve tüm döndürülen varlıkları düzeniyle maliyetini içerir. 10 yinelemeden zaman aşımına 1000 yineleme toplamını almadan önce untimed çalıştırılır. Gösterilen sonuçları geçen her test 5 çalıştırmalardan ORTANCA Çalıştır ' dir. Daha fazla bilgi için test kodunu içeren bir ek B bakın.

| EF  | Test                                        | Süre (ms) | Bellek   |
|:----|:--------------------------------------------|:----------|:---------|
| EF5 | ObjectContext varlık komutu                | 621       | 39350272 |
| EF5 | DbContext veritabanında Sql sorgusu             | 825       | 37519360 |
| EF5 | ObjectContext Store sorgu                   | 878       | 39460864 |
| EF5 | ObjectContext LINQ Sorgu izleme yok        | 969       | 38293504 |
| EF5 | Nesne sorgusu kullanarak ObjectContext varlık Sql | 1089      | 38981632 |
| EF5 | ObjectContext derlenmiş sorgu                | 1099      | 38682624 |
| EF5 | ObjectContext LINQ sorgusu                    | 1152      | 38178816 |
| EF5 | DbContext LINQ Sorgu izleme yok            | 1208      | 41803776 |
| EF5 | DbContext olan DB Sql sorgusu                | 1414      | 37982208 |
| EF5 | DbContext LINQ sorgusu                        | 1574      | 41738240 |
|     |                                             |           |          |
| EF6 | ObjectContext varlık komutu                | 480       | 47247360 |
| EF6 | ObjectContext Store sorgu                   | 493       | 46739456 |
| EF6 | DbContext veritabanında Sql sorgusu             | 614       | 41607168 |
| EF6 | ObjectContext LINQ Sorgu izleme yok        | 684       | 46333952 |
| EF6 | Nesne sorgusu kullanarak ObjectContext varlık Sql | 767 girişini       | 48865280 |
| EF6 | ObjectContext derlenmiş sorgu                | 788       | 48467968 |
| EF6 | DbContext LINQ Sorgu izleme yok            | 878       | 47554560 |
| EF6 | ObjectContext LINQ sorgusu                    | 953       | 47632384 |
| EF6 | DbContext olan DB Sql sorgusu                | 1023      | 41992192 |
| EF6 | DbContext LINQ sorgusu                        | 1290      | 47529984 |


![EF5WarmQuery1000](~/ef6/media/ef5warmquery1000.png)

![EF6WarmQuery1000](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> Eksiksiz olması için burada biz bir varlık SQL sorgusu üzerinde bir EntityCommand yürütme bir değişim ekledik. Sonuçları gibi sorgular için gerçekleştirilmiş değil, ancak karşılaştırma olmak zorunda değildir elma elma. Test karşılaştırması fairer yaparken denemek düzeniyle için bir Kapat yaklaşık içerir.

Bu uçtan uca durumda Entity Framework 6 Entity Framework 5 çeşitli parçaları çok açık DbContext başlatma ve hızlı MetadataCollection dahil yığını üzerinde yapılan performans iyileştirmeleri nedeniyle çok daha iyi&lt;T&gt; Arama.

## <a name="7-design-time-performance-considerations"></a>7 tasarım zamanı performans konuları

### <a name="71-------inheritance-strategies"></a>7.1 devralma stratejileri

Entity Framework kullanarak başka bir performans artışı, kullandığınız devralma stratejisidir. Entity Framework, devralma ve bunların bileşimleri 3 temel türlerini destekler:

-   Tablo başına hiyerarşi (her devralma haritalar hiyerarşideki belirli hangi tür belirtmek için bir ayrıştırıcı sütunu içeren bir tabloya ayarlandığı TPH) – satırda gösterilir.
-   Tablo başına tür (her türü kendi tablo veritabanına sahip olduğu TPT) –; alt tablolar yalnızca üst tablo içermiyor sütunları tanımlar.
-   Tablo başına sınıfı (her türü kendi tam tablo veritabanına sahip olduğu TPC) –; alt tablolar üst türlerinde tanımlanan dahil olmak üzere tüm bunların alanları tanımlayın.

Modelinizi TPT devralma kullanıyorsa, oluşturulan sorgular artık yürütme süresi Store'daki neden diğer devralma stratejileri ile oluşturulan olandan daha karmaşık olacaktır.  Genellikle TPT modeli üzerinde sorgular oluşturun ve elde edilen nesnelerini gerçekleştirmek için daha uzun sürer.

"TPT (tablo başına tür) devralma varlık Çerçevesi'nde kullanırken performans konuları" Bkz MSDN blog gönderisi: \< http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.

#### <a name="711-------avoiding-tpt-in-model-first-or-code-first-applications"></a>7.1.1 TPT modeli ilk ya da Code First uygulamaları engelleme

TPT şemaya sahip mevcut bir veritabanı üzerinde bir modeli oluşturduğunuzda, pek çok seçenek yok. Ancak, Model ilk ya da Code First kullanarak bir uygulama oluştururken, performans endişelerini TPT devralınmasını kaçınmanız gerekir.

Varlık Tasarımcısı Sihirbazı'nda kullandığınız Model ilk zaman modelinize için herhangi bir devralma TPT alırsınız. TPH devralma strateji ile ilk Model geçmek istiyorsanız, Visual Studio Gallery'den "varlık Tasarımcısı veritabanı oluşturma Power paketi" kullanıma kullanabilirsiniz ( \< http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).

Devralma ile bir modelin eşlemeyi yapılandırmak için Code First kullanarak EF TPH varsayılan olarak kullanır, bu nedenle devralma hiyerarşisindeki tüm varlıkları aynı tablonun eşleştirilecek. MSDN magazine'de "Kod ilk olarak varlığın Framework4.1" makale "Eşleme ile Fluent API'si" bölümüne bakın ( [ http://msdn.microsoft.com/magazine/hh126815.aspx ](https://msdn.microsoft.com/magazine/hh126815.aspx)) daha fazla ayrıntı için.

### <a name="72-------upgrading-from-ef4-to-improve-model-generation-time"></a>7.2 model oluşturma geliştirmek için EF4 yükseltme zamanı

Visual Studio 2010 SP1 yüklü olduğunda depolama katmanı (SSDL) oluşturan algoritma modelinin bir SQL Server'a özgü geliştirmeyi Entity Framework 5 ve 6 ve Entity Framework 4 için güncelleştirme olarak kullanılabilir. Bu çok büyük bir model oluşturma durumda olduğunda Navision model geliştirme aşağıdaki test sonuçlarını gösterir. Ek C ilgili daha fazla ayrıntı için bkz.

1005 varlık setleri ve ilişki Setleri 4227 modeli içerir.

| Yapılandırma                              | Dökümü harcanan süre                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Visual Studio 2010, Entity Framework 4     | SSDL oluşturma: 2 saat 27 dk <br/> Eşleme oluşturma: 1 saniye <br/> CSDL oluşturma: 1 saniye <br/> ObjectLayer oluşturma: 1 saniye <br/> Görünüm oluşturma: 2 saat 14 dakika |
| Visual Studio 2010 SP1, Entity Framework 4 | SSDL oluşturma: 1 saniye <br/> Eşleme oluşturma: 1 saniye <br/> CSDL oluşturma: 1 saniye <br/> ObjectLayer oluşturma: 1 saniye <br/> Görünüm oluşturma: 1 saat 53 dk   |
| Visual Studio 2013, Entity Framework 5     | SSDL oluşturma: 1 saniye <br/> Eşleme oluşturma: 1 saniye <br/> CSDL oluşturma: 1 saniye <br/> ObjectLayer oluşturma: 1 saniye <br/> Görünüm oluşturma: 65 dakika    |
| Visual Studio 2013, Entity Framework 6     | SSDL oluşturma: 1 saniye <br/> Eşleme oluşturma: 1 saniye <br/> CSDL oluşturma: 1 saniye <br/> ObjectLayer oluşturma: 1 saniye <br/> Görünüm oluşturma: 28 saniye.   |


Bu istemci geliştirme makinesi beklerken SSDL oluştururken, yükü neredeyse tamamen SQL Server üzerinde harcadığı sunucudan geri dönmeniz sonuçları için boşta çarpmaktadır. Dba'lar, özellikle bu geliştirme yönetilmesinde. Ayrıca, temelde tüm maliyet modeli oluşturma görünüm oluşturma işlemi artık gerçekleştirilir hatalarının ayıklanabileceğini belirtmekte yarar.

### <a name="73-------splitting-large-models-with-database-first-and-model-first"></a>7.3 veritabanı ile büyük modeller ilk bölme ve ilk Model

Model boyutu arttıkça, Tasarımcı yüzeyine anlaşılamayacak ve kullanmak daha zor hale gelir. Biz genellikle bir model Tasarımcısı etkili bir şekilde kullanmak için çok büyük olacak şekilde 300'den fazla varlıklarla göz önünde bulundurun. Büyük modellerin bölmek için çeşitli seçenekler şu blog gönderisinde açıklanmaktadır: \< http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.

Post Entity Framework'ün ilk sürümü için yazılmıştır, ancak adımlar hala geçerlidir.

### <a name="74-------performance-considerations-with-the-entity-data-source-control"></a>7.4 varlık veri kaynak denetimi ile performans konuları

Çok iş parçacıklı performans ve stres testleri durumlarda burada EntityDataSource denetimi kullanarak bir web uygulaması performansını önemli ölçüde deteriorates gördük. EntityDataSource art arda MetadataWorkspace.LoadFromAssembly varlıklar olarak kullanılacak türlerini bulmak için Web uygulaması tarafından başvurulan derlemeler üzerinde ProcessOrder temel nedeni.

Çözüm, kendi türetilmiş Objectcontext'e sınıfınızı tür adını EntityDataSource, ContextTypeName ayarlamaktır. Bu, varlık türleri, tüm başvurulan derlemelerin tarar mekanizması kapatır.

ContextTypeName alanını ayarlamak, işlevsel bir sorun olduğu yansıma yoluyla bir derlemeden bir tür yüklenemiyor, .NET 4.0 EntityDataSource bir ReflectionTypeLoadException oluşturur engeller. Bu sorun, .NET 4.5 içinde düzeltilmiştir.

### <a name="75-------poco-entities-and-change-tracking-proxies"></a>7.5 POCO varlık ve değişiklik izleme proxy'ler

Varlık çerçevesi veri sınıfları için herhangi bir değişiklik yapmadan özel veri sınıfları, veri modeli ile birlikte kullanmanıza olanak sağlar. Başka bir deyişle, "düz eski" CLR nesnelerine (POCO), veri modelinizle var olan etki alanı nesnelerini gibi kullanabilirsiniz. Bir veri modelinde tanımlanan varlıklara eşlenmesi bu POCO veri sınıfları (olarak da bilinen Kalıcılık ignorant nesneler), aynı sorgu çoğunu destekler, ekleme, güncelleştirme ve varlık veri modeli araçlarının ürettiği varlık türleri olarak davranışları silme.

Entity Framework, yavaş yükleniyor ve otomatik değişiklik izleme POCO varlıklarda gibi özellikleri etkinleştirmek istediğinizde, kullanılan POCO türlerinden türetilmiş proxy sınıfları da oluşturabilirsiniz. POCO sınıflarınızı proxy'ler kullanmanız Entity Framework, burada açıklandığı şekilde izin vermek için belirli gereksinimleri karşılamalıdır: [ http://msdn.microsoft.com/library/dd468057.aspx ](https://msdn.microsoft.com/library/dd468057.aspx).

Fırsat izleme proxy'leri varlıklarınızın özelliklerinden herhangi birini sahip değerine değiştirildi, Entity Framework, her zaman gerçek varlıklarınızın durumunu bilmesi için her zaman nesne durum Yöneticisi bildirir. Bu, ayarlayıcı yöntemlerinden birini özelliklerinizi gövdesine bildirim olaylar ekleme ve bu tür olayların işlenmesini nesne durum Yöneticisi sahip gerçekleştirilir. Proxy oluşturma varlık, genellikle görüntüler olmayan proxy'si POCO varlık eklenen Entity Framework tarafından oluşturulan olaylar nedeniyle oluşturmaya kıyasla daha pahalı unutmayın.

Değişiklik izleme proxy POCO varlık sahip olmadığında, değişiklikleri varlıklarınızı bir kopyasını bir önceki kaydedilen bir duruma karşı içeriğini karşılaştırarak bulunamadı. En son karşılaştırmanın atıldıktan sonra bunların hiçbiri değişmiş olsa bile bu ayrıntılı karşılaştırma uzun süren bir işlem birçok varlığın, bağlam içinde olduğunda ya da özellikler, çok büyük miktarda varlıklarınızı sahip olur.

Özet: değişiklik izleme proxy oluşturulurken isabet bir performans ödersiniz, ancak değişiklik izleme yardımcı olacak birçok özelliği ya da modelinizde birçok varlığın olduğunda varlıklarınızı sahip olduğunuzda değişiklik algılama sürecini hızlandırmak. Varlıkları miktarı çok fazla büyüdüğü değil özelliklerinin küçük bir sayı ile varlıklar için çok yararı, değişiklik izleme proxy'leri sahip olmayabilir.

## <a name="8-loading-related-entities"></a>İlgili varlıkları 8 yükleme

### <a name="81-lazy-loading-vs-eager-loading"></a>8.1 yavaş yükleme vs. İstekli yükleme

Entity Framework, hedef varlıkla ilgili varlıkları yükleme için çeşitli yollar sunar. Örneğin, ürünleri için sorgulama yapıldığında, ilgili Siparişler yüklenecek farklı yol vardır nesne durumu Manager'a. Performans açısından, ilgili varlıkları yüklenirken dikkate alınması gereken büyük soru yavaş yükleniyor veya istekli yükleme olacaktır.

İstekli yükleme kullanırken, ilgili varlıkları, hedef varlık kümesi ile birlikte yüklenir. Sorgunuzda INCLUDE deyimi, ilgili varlıkları almak için istediğiniz belirtmek için kullanın.

Yavaş yükleniyor kullanırken ilk sorgunuzu yalnızca hedef varlık kümesinde getirir. Ancak, bir gezinti özelliği eriştiğinizde, başka bir sorgu ilgili varlık yüklemek için depo verilir.

Bir varlık yüklendikten sonra yavaş yükleniyor veya istekli yükleme kullanıp kullanmadığınızı varlık için daha fazla sorgu, nesne durumu Yöneticisi'nden doğrudan yükleyin.

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a>8.2 yavaş yükleniyor ve istekli yükleme arasında seçim yapma

Önemli olan, böylece uygulamanız için doğru seçim yapabileceğiniz yavaş yükleniyor ve istekli yükleme arasındaki farkı anlamak olmasıdır. Bu, büyük bir yükü içerebilir, tek bir istek karşı veritabanında birden çok istek etmekten değerlendirmenize yardımcı olur. Uygulamanız bazı bölümlerinde istekli yükleme ve yavaş yükleniyor diğer bölümlerinde kullanmak uygun olabilir.

Başlık altında neler olduğunu ilişkin bir örnek olarak, Birleşik Krallık ve bunların sırası sayısını yaşayan müşterilerin sorgulamak istediğiniz varsayalım.

**İstekli yükleme kullanma**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

**Kullanılarak yavaş yükleniyor**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    context.ContextOptions.LazyLoadingEnabled = true;

    //Notice that the Include method call is missing in the query
    var ukCustomers = context.Customers.Where(c => c.Address.Country == "UK");

    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

İstekli yükleme kullanırken, tüm müşteriler döndüren tek bir sorgu sorunu ve tüm siparişleri. Depo komutu şu şekilde görünür:

``` SQL
SELECT
[Project1].[C1] AS [C1],
[Project1].[CustomerID] AS [CustomerID],
[Project1].[CompanyName] AS [CompanyName],
[Project1].[ContactName] AS [ContactName],
[Project1].[ContactTitle] AS [ContactTitle],
[Project1].[Address] AS [Address],
[Project1].[City] AS [City],
[Project1].[Region] AS [Region],
[Project1].[PostalCode] AS [PostalCode],
[Project1].[Country] AS [Country],
[Project1].[Phone] AS [Phone],
[Project1].[Fax] AS [Fax],
[Project1].[C2] AS [C2],
[Project1].[OrderID] AS [OrderID],
[Project1].[CustomerID1] AS [CustomerID1],
[Project1].[EmployeeID] AS [EmployeeID],
[Project1].[OrderDate] AS [OrderDate],
[Project1].[RequiredDate] AS [RequiredDate],
[Project1].[ShippedDate] AS [ShippedDate],
[Project1].[ShipVia] AS [ShipVia],
[Project1].[Freight] AS [Freight],
[Project1].[ShipName] AS [ShipName],
[Project1].[ShipAddress] AS [ShipAddress],
[Project1].[ShipCity] AS [ShipCity],
[Project1].[ShipRegion] AS [ShipRegion],
[Project1].[ShipPostalCode] AS [ShipPostalCode],
[Project1].[ShipCountry] AS [ShipCountry]
FROM ( SELECT
      [Extent1].[CustomerID] AS [CustomerID],
       [Extent1].[CompanyName] AS [CompanyName],
       [Extent1].[ContactName] AS [ContactName],
       [Extent1].[ContactTitle] AS [ContactTitle],
       [Extent1].[Address] AS [Address],
       [Extent1].[City] AS [City],
       [Extent1].[Region] AS [Region],
       [Extent1].[PostalCode] AS [PostalCode],
       [Extent1].[Country] AS [Country],
       [Extent1].[Phone] AS [Phone],
       [Extent1].[Fax] AS [Fax],
      1 AS [C1],
       [Extent2].[OrderID] AS [OrderID],
       [Extent2].[CustomerID] AS [CustomerID1],
       [Extent2].[EmployeeID] AS [EmployeeID],
       [Extent2].[OrderDate] AS [OrderDate],
       [Extent2].[RequiredDate] AS [RequiredDate],
       [Extent2].[ShippedDate] AS [ShippedDate],
       [Extent2].[ShipVia] AS [ShipVia],
       [Extent2].[Freight] AS [Freight],
       [Extent2].[ShipName] AS [ShipName],
       [Extent2].[ShipAddress] AS [ShipAddress],
       [Extent2].[ShipCity] AS [ShipCity],
       [Extent2].[ShipRegion] AS [ShipRegion],
       [Extent2].[ShipPostalCode] AS [ShipPostalCode],
       [Extent2].[ShipCountry] AS [ShipCountry],
      CASE WHEN ([Extent2].[OrderID] IS NULL) THEN CAST(NULL AS int) ELSE 1 END AS [C2]
      FROM  [dbo].[Customers] AS [Extent1]
      LEFT OUTER JOIN [dbo].[Orders] AS [Extent2] ON [Extent1].[CustomerID] = [Extent2].[CustomerID]
      WHERE N'UK' = [Extent1].[Country]
)  AS [Project1]
ORDER BY [Project1].[CustomerID] ASC, [Project1].[C2] ASC
```

Yavaş yükleniyor kullanırken, başlangıçta aşağıdaki sorguyu yürütün:

``` SQL
SELECT
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[CompanyName] AS [CompanyName],
[Extent1].[ContactName] AS [ContactName],
[Extent1].[ContactTitle] AS [ContactTitle],
[Extent1].[Address] AS [Address],
[Extent1].[City] AS [City],
[Extent1].[Region] AS [Region],
[Extent1].[PostalCode] AS [PostalCode],
[Extent1].[Country] AS [Country],
[Extent1].[Phone] AS [Phone],
[Extent1].[Fax] AS [Fax]
FROM [dbo].[Customers] AS [Extent1]
WHERE N'UK' = [Extent1].[Country]
```

Ve her bir müşterinin siparişleri gezinme özelliğini eriştiğinizde, depo aşağıdaki gibi başka bir sorgu verilir:

``` SQL
exec sp_executesql N'SELECT
[Extent1].[OrderID] AS [OrderID],
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[EmployeeID] AS [EmployeeID],
[Extent1].[OrderDate] AS [OrderDate],
[Extent1].[RequiredDate] AS [RequiredDate],
[Extent1].[ShippedDate] AS [ShippedDate],
[Extent1].[ShipVia] AS [ShipVia],
[Extent1].[Freight] AS [Freight],
[Extent1].[ShipName] AS [ShipName],
[Extent1].[ShipAddress] AS [ShipAddress],
[Extent1].[ShipCity] AS [ShipCity],
[Extent1].[ShipRegion] AS [ShipRegion],
[Extent1].[ShipPostalCode] AS [ShipPostalCode],
[Extent1].[ShipCountry] AS [ShipCountry]
FROM [dbo].[Orders] AS [Extent1]
WHERE [Extent1].[CustomerID] = @EntityKeyValue1',N'@EntityKeyValue1 nchar(5)',@EntityKeyValue1=N'AROUT'
```

Daha fazla bilgi için [ilgili nesneler Yükleniyor](https://msdn.microsoft.com/library/bb896272.aspx).

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a>8.2.1 yavaş yükleme karşılaştırması istekli yükleme kopya kağıdı

İstekli yükleme yavaş yükleniyor karşılaştırması seçmek için gönderilemez bir BT'ye yoktur. Her iki stratejileri de bilinçli bir karar yapabilmesi arasındaki farkları anlamak önce deneyin; Ayrıca, kodunuzu herhangi aşağıdaki senaryolar için uygun olmadığını göz önünde bulundurun:

| Senaryo                                                                    | Bizim önerisi                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Çok sayıda Gezinti özelliklerine getirilen varlıkların erişim gerekiyor mu? | **Hayır** -seçeneklerin muhtemelen kullanıyordur. Ancak, sorgunuzu getirme yükü performans avantajlarının daha az ihtiyacınız olacaktır istekli yükleme kullanarak karşılaşabileceğiniz çok büyük ise uyguladığından, nesnelerini gerçekleştirmek için ağ. <br/> <br/> **Evet** -çok sayıda Gezinti özellikleri, varlıkların erişmeniz gerekiyorsa, birden fazla deyimleri ile istekli yükleme sorgunuzda içerdiğini yaptığınız. Daha fazla varlık dahil, daha büyük yük sorgunuzu döndürür. Üç veya daha fazla varlık sorgunuzu dahil sonra yükleme Lazy için göz önünde bulundurun. |
| Tam olarak hangi verilerin çalışma zamanında gerekli olacağını biliyor musunuz?                   | **Hayır** -yavaş yükleniyor sizin için daha iyi olacaktır. Aksi takdirde, değil gerekir veri sorgulama yukarı sonlandırabiliriz. <br/> <br/> **Evet** - istekli yükleme büyük olasılıkla, en iyi sonucu; tüm ayarlar daha hızlı yükleme yardımcı olur. Sorgunuz çok büyük miktarda veri getirilirken gerektirir ve bu çok düşük olur, bunun yerine yüklenirken Lazy deneyin.                                                                                                                                                                                                                                                       |
| Kodunuzu veritabanınızı gölgeden uzak yürütüyor? (daha fazla ağ gecikmesi)  | **Hayır** - ağ gecikme süresi bir sorun olmadığında kullanılarak yavaş yükleniyor kodunuzu basitleştirin. Uygulamanızın topolojisini, verilen için veritabanı yakınlık yakalayana şekilde değiştirebileceğine unutmayın. <br/> <br/> **Evet** - ağ ne senaryonuz için daha iyi uyduğunu karar yalnızca bir sorun olduğunda. Genellikle daha az sayıda gidiş dönüş gerektirdiğinden istekli yükleme daha iyi olacaktır.                                                                                                                                                                                                      |


#### <a name="822-------performance-concerns-with-multiple-includes"></a>8.2.2 performans endişelerini ile birden çok içerir

Biz sunucu yanıt süresi sorunları ilgili performans sorular duyduğunuzda, sorunun sık birden çok içerik deyimleri sorgularla kaynağıdır. Bir sorguda ilgili varlıkları dahil olmak üzere güçlü olsa da, bu işlem arka planda neler olduğunu anlamak önemlidir.

İçindeki depolama komutu oluşturmak için iç planı derleyicimiz gitmek için bir sorgu ile birden çok içerik deyimleri oldukça uzun bir süre sürer. Bu süre çoğu, sonuçta elde edilen sorgu iyileştirmeye çalışıyor harcanır. Oluşturulan depolama komutu, eşleştirme bağlı olarak her dahil etme için Outer JOIN veya birleşim içerecektir. Bu gibi sorguların büyük bağlı grafikler, özellikle yedeklilik (örneğin INCLUDE birden fazla düzeyde geçirmek için kullanıldığında, yükte çok fazla olduğunda, bant genişliği sorunları acerbate olan tek bir yükte veritabanınızdan Getir ilişkileri bir-çok yönde).

Burada, sorgularınızı aşırı büyük yükleri ToTraceString kullanarak ve yükü boyutu görmek için SQL Server Management Studio içinde depolama komutu yürütülürken temel alınan bir TSQL sorgusu için erişerek iade ettiğiniz durumlarda kontrol edebilirsiniz. Azaltmak için yapabileceğiniz gibi durumlarda içerik deyimleri yalnızca sorgunuzda sayısını ihtiyacınız verilerinizi getirin. Veya örneğin alt sorgularda, daha küçük dizisine sorgunuzu bölüneceği mümkün olabilir:

**Sorgu kesmeden önce:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var customers = from c in context.Customers.Include(c => c.Orders)
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

**Sorgu sonu sonra:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var orders = from o in context.Orders
                 where o.Customer.LastName.StartsWith(lastNameParameter)
                 select o;

    orders.Load();

    var customers = from c in context.Customers
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

Yapıyoruz gibi bu yalnızca izlenen sorgulara çalışır bağlamı sahip kimlik çözümlemesi ve ilişki düzeltmesi otomatik olarak gerçekleştirmek için özelliği kullanımı.

Gecikmeli yükleme gibi karşılıklı avantaj ve dezavantajlarını daha fazla sorgular için daha küçük yüklerini sunulacaktır. Yalnızca ihtiyacınız olan verileri her bir varlıktaki açıkça seçmek için ayrı ayrı özellikler yansıtmaların de kullanabilirsiniz ancak, varlıklar bu durumda yüklenmemesi ve güncelleştirmeleri desteklenmiyor.

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a>8.2.3 geçici özelliklerinin yavaş yükleniyor

Entity Framework, şu anda skaler veya karmaşık özellikler yavaş yüklenmesini desteklemez. Ancak, bir BLOB gibi büyük bir nesne içeren bir tablo olduğu durumlarda, tablo bölme büyük özelliklerin ayrı bir varlığa ayırmak için kullanabilirsiniz. Örneğin, varbinary fotoğraf sütunu içeren bir ürün tablo olduğunu varsayalım. Sık sorgularınızı bu özellikte erişmeye ihtiyacınız yoksa, yalnızca, normalde varlığa bölümlerinde getirmek için bölme tablo kullanabilirsiniz. Ürün fotoğrafı temsil eden varlık yalnızca açıkça gerektiğinde yüklenecek.

Gil Fink'ın "Tablo bölme, Entity Framework" blog gönderisine tablo bölme etkinleştirme gösteren makaleden faydalanabilirsiniz: \< http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.

## <a name="9-other-considerations"></a>9 diğer konular

### <a name="91------server-garbage-collection"></a>9.1 sunucu çöp toplama

Bazı kullanıcılar, çöp toplayıcının düzgün şekilde yapılandırılmadığında, bunlar görmeyi paralellik sınırlar kaynak çekişmesini karşılaşabilirsiniz. EF birden çok iş parçacıklı bir senaryoda kullanılan veya herhangi bir uygulamada, bir sunucu tarafı sistemi benzer olduğunda, sunucu Çöp toplamayı etkinleştirmek emin olun. Bu, basit bir ayar, uygulama yapılandırma dosyasında aracılığıyla gerçekleştirilir:

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

Bu, iş parçacığı çekişmeyi azaltmak ve % 30 oranında e doygun CPU senaryolarda aktarım hızınızı artırın gerekir. Genel koşullarını nasıl yanı sıra sunucu çöp toplama (Bu, daha iyi kullanıcı Arabirimi ve istemci tarafı senaryolar için ayarlanmıştır) Klasik çöp toplama kullanarak uygulamanızın davranışını her zaman test etmeniz gerekir.

### <a name="92------autodetectchanges"></a>9.2 AutoDetectChanges

Daha önce belirtildiği gibi Entity Framework, nesne önbelleği birçok varlığın sahip olduğunda performans sorunlarını gösterebilir. Ekle, Kaldır, bulma, giriş ve SaveChanges, gibi bazı işlemleri, büyük miktarda CPU ne kadar büyük nesne önbelleği haline gelmiştir üzerinde tabanlı tüketebilir DetectChanges çağrıları tetikleyin. Bunun nedeni, nesne önbelleği ve Nesne Durum Yöneticisi'ni deneyin olarak kalır, böylece üretilen veri çeşit senaryo altında doğru olması garanti bir bağlam için gerçekleştirilen her işlem üzerinde mümkün olduğunca eşitlendiğini ' dir.

Entity Framework'ün otomatik değişiklik algılama ömrü, uygulamanız için etkin tutulacak genellikle iyi bir uygulamadır. Senaryonuz olumsuz yüksek CPU kullanımına göre etkilenmesini ve profillerinizi sabah DetectChanges çağrısı belirtmek, geçici olarak devre dışı AutoDetectChanges kodunuzun hassas bölümünde kapatma göz önünde bulundurun:

``` csharp
try
{
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    ...
}
finally
{
    context.Configuration.AutoDetectChangesEnabled = true;
}
```

AutoDetectChanges kapatmadan önce bu Entity Framework, gerçekleşirken varlıklar üzerinde değişiklikler hakkındaki belirli bilgileri izlemek için güncelleyebileceği kaybetmesine neden olabilir anlamak uygundur. Hatalı olarak işlenir, bu uygulama veri tutarsızlığına neden olabilir. AutoDetectChanges kapatarak daha fazla bilgi için okuma \< http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.

### <a name="93------context-per-request"></a>9.3 istek başına bağlamı

Entity Framework'ün bağlamları en iyi performans sağlamak için kısa süreli örnekleri deneyimi gibi kullanılmaya yöneliktir. Bağlamları bekleniyor kısa olması beklenir ve atılır ve bu nedenle basit ve meta verileri mümkün olduğunca reutilize uygulanmıştır. Web senaryolarında bunu aklınızda bulundurun ve tek bir isteğin süresinden daha fazla bilgi için bir bağlam yok önemlidir. Benzer şekilde, web olmayan senaryolarda bağlam göre varlık Çerçevesi'nde önbelleğe alma düzeylerini anlama atılmalıdır. Genel olarak bakıldığında, bir uygulama hem de iş parçacığı başına bağlamları ve statik içerikleri ömrü boyunca bir bağlam örneği kaçınmanız gerekir.

### <a name="94------database-null-semantics"></a>9.4 sürümünden veritabanı null semantikler

Varsayılan olarak Entity Framework C olan SQL kodu üretir\# null karşılaştırma semantiği. Aşağıdaki örnek sorgu göz önünde bulundurun:

``` csharp
            int? categoryId = 7;
            int? supplierId = 8;
            decimal? unitPrice = 0;
            short? unitsInStock = 100;
            short? unitsOnOrder = 20;
            short? reorderLevel = null;

            var q = from p incontext.Products
                    wherep.Category.CategoryName == "Beverages"
                          || (p.CategoryID == categoryId
                                || p.SupplierID == supplierId
                                || p.UnitPrice == unitPrice
                                || p.UnitsInStock == unitsInStock
                                || p.UnitsOnOrder == unitsOnOrder
                                || p.ReorderLevel == reorderLevel)
                    select p;

            var r = q.ToList();
```

Bu örnekte biz SupplierID ve UnitPrice gibi varlık boş değer atanabilir özellikte karşı boş değer atanabilir değişkenleri sayısı senedini karşılaştırdığınızı. Bu sorgu için oluşturulan SQL sütun değeri olarak aynı parametre değeri ise ya da hem parametrenin hem de sütun değerlerini null sorar. Bu veritabanı sunucusu, null değerlere işleme gizler ve tutarlı bir C sağlayacak\# deneyimi arasında farklı veritabanı satıcılarının null. Öte yandan, oluşturulan kod biraz karışık ve iyi olduğunda çalışmayabilir karşılaştırmalar miktarını where sorgu deyimi için çok sayıda büyür.

Bu durumu düzeltmek için bir veritabanı null semantikler kullanarak yoludur. Bu büyük olasılıkla farklı C davranabilir Not\# Entity Framework veritabanı altyapısı, boş değerler işleme kullanıma sunan basit SQL oluşturacak artık bu yana semantiği null. Veritabanı null semantikler etkinleştirilmiş başına bağlam bağlam içi yapılandırma karşı tek tek yapılandırma satır içeren olabilir:

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

Küçük ve orta ölçekli sorguları algılanabilir performans iyileştirmesi veritabanı null semantikler kullanılırken görüntülenmez, ancak fark sorguları ile çok sayıda olası null karşılaştırmalar daha belirgin hale gelir.

Yukarıdaki örnek sorguda bir performans farkı küçüktür %2 denetimli bir ortamda çalışan bir microbenchmark içinde oluştu.

### <a name="95------async"></a>9.5 zaman uyumsuz

.NET 4.5 veya sonraki sürümlerde çalışan zaman uyumsuz işlemler Entity Framework 6 sunulan desteği. Çoğunlukla, g/ç uygulamaları Çekişme ilgili en çok zaman uyumsuz sorgu kullanma avantajını yakalayabilirler ve kaydetme işlemleri. Uygulamanızın g/ç kms'den kaynaklanan çakışmayı saptanmamış, zaman uyumsuz kullanımını en iyi durumda zaman uyumlu olarak çalışacak ve sonuç aynı süre içinde zaman uyumlu bir çağrı olarak ya da en kötü durumda, yalnızca zaman uyumsuz bir görev için yürütme ertele ve ek tim Ekle tamamlandığında, senaryonuz e.

Zaman uyumsuz bir uygulamanızın performansını artıracak karar yardımcı olacak ne zaman uyumsuz programlama iş ziyaret bilgi [ http://msdn.microsoft.com/library/hh191443.aspx ](https://msdn.microsoft.com/library/hh191443.aspx). Entity Framework zaman uyumsuz işlemleri daha fazla bilgi için bkz. [zaman uyumsuz sorgu ve tasarruf](~/ef6/fundamentals/async.md
).

### <a name="96------ngen"></a>9.6 NGEN

Entity Framework 6, .NET framework'ün varsayılan yüklemede gelmez. Bu nedenle, Entity Framework derlemeleri Entity Framework kodunun herhangi bir MSIL derleme olarak aynı JIT'ing maliyetleri tabi olduğu anlamına gelir varsayılan NGEN 'D değildir. Bu, geliştirme ve ayrıca, uygulamanızın üretim ortamlarında soğuk başlangıç F5 deneyimi düşebilir. JIT'ing CPU ve bellek maliyetlerini azaltmak için Entity Framework uygun şekilde görüntüleri NGEN için tavsiye edilir. Entity Framework 6 NGEN ile başlangıç performansını artırmak nasıl hakkında daha fazla bilgi için bkz. [NGen ile başlangıç performansı artırma](~/ef6/fundamentals/performance/ngen.md).

### <a name="97------code-first-versus-edmx"></a>9.7 ilk EDMX karşı kodu

Entity Framework nedeniyle nesne yönelimli programlama ve bir bellek içi temsillerinin kavramsal model (nesneler), depolama şemanın (veritabanı) ve arasında bir eşleme tarafından ilişkisel veritabanları arasında empedans uyuşmazlığı sorunu hakkında iki. Bu meta veriler için kısa bir varlık veri modeli veya EDM çağrılır. Bu EDM Entity Framework görünümleri gidiş dönüş verileri veritabanına bellekte nesnelerin türetilir ve yedekleyin.

Entity Framework, resmi bir EDMX dosyası ile kullanıldığında, kavramsal model ve depolama şema eşleme belirtir, ardından EDM doğru olduğunu doğrulamak yalnızca model yükleme aşaması vardır (örneğin, hiçbir eşlemesi eksik olduğundan emin olun), ardından görünümlerin, görünümleri doğrulamak ve kullanıma hazır bu meta veriler bulunur. Yalnızca sonra can bir sorgu yürütülür veya yeni verileri veri deposuna kaydedilmiş olabilir.

Code First, temelinde, Gelişmiş bir varlık veri modeli Oluşturucu bir yaklaşımdır. Bir EDM sağlanan kod üretmek Entity Framework vardır; Bunu kuralları uygulamak ve modeli Fluent API'si aracılığıyla yapılandırma modeli katılan sınıflar çözümleme yapar. EDM oluşturulduktan sonra şekilde olduğu gibi projede yüklenmemiş bir EDMX dosyasının varlık çerçevesi temelde aynı şekilde davranır. Bu nedenle, Code First modeli oluşturma, bir EDMX bulunması karşılaştırıldığında Entity Framework için daha yavaş bir başlangıç zamanına çevirir fazladan karmaşıklık ekler. Maliyet tamamen boyutu ve oluşturulmakta olan modelin karmaşıklığını bağlıdır.

Code First karşı EDMX kullanmayı seçerken, Code First tarafından sunulan esnekliği modeli ilk kez oluşturma maliyetini artırır bilmek önemlidir. Uygulamanız bu ilk kez yük maliyetini dayanabilir verirse genellikle Code First gitmek için tercih edilen yol olacaktır.

## <a name="10-investigating-performance"></a>10 performans araştırma

### <a name="101-using-the-visual-studio-profiler"></a>10.1 Visual Studio Profiler kullanma

Entity Framework ile performans sorunları yaşıyorsanız, uygulamanızı kendi zaman harcadığı burada görmek için Visual Studio'da yerleşik olanlar gibi bir profil oluşturucu kullanabilirsiniz. Bu "Keşfetme - bölüm 1 ADO.NET Entity Framework performansının" blog gönderisinde pasta grafikler oluşturmak için kullandığımız aracıdır ( \< http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) Entity Framework, süre boyunca soğuk ve orta Gecikmeli sorgular nerede geçirdiği göster.

Profil Oluşturucu performans sorunu araştırmak için almaları bir gerçek örnek veri ve modelleme Müşteri danışma ekibi tarafından yazılan "profil oluşturma Entity Framework kullanarak Visual Studio 2010 Profiler" blog gönderisine gösterir.  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>. Bu gönderi, windows uygulaması için yazılmıştır. Bir web uygulamasının profilini çıkarmak gerekiyorsa Windows Performans kaydedici (WPR) ve Windows Performans Çözümleyicisi (WPA) araçları Visual Studio'dan çalışma daha iyi çalışabilir. Windows değerlendirme ve Dağıtım Seti ile dahil olan Windows Performans araç bir parçası olan WBT ve WPA ( [ http://www.microsoft.com/en-US/download/details.aspx?id=39982 ](https://www.microsoft.com/en-US/download/details.aspx?id=39982)).

### <a name="102-applicationdatabase-profiling"></a>10.2 uygulama/veritabanı profil oluşturma

Visual Studio'da yerleşik olarak bulunan profil oluşturucu gibi araçları uygulamanızın zaman harcadığı yerleri burada söyleyin.  Profil Oluşturucu başka türde kullanılabilir çalışan uygulamanızı, üretim veya üretim öncesi gereksinimlerine bağlı olarak dinamik analizini yapar ve yaygın görülen tehlikeleri ve veritabanı erişim ters desenler için arar.

Piyasadaki iki profil oluşturucular olan Entity Framework Profiler ( \< http://efprof.com>) ve ORMProfiler ( \< http://ormprofiler.com>).

Uygulamanız Code First kullanarak bir MVC uygulaması ise, StackExchange'nın MiniProfiler kullanabilirsiniz. Scott Hanselman blog bu araç açıklar: \< http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.

Uygulamanızın veritabanı etkinliğini, Julie Lerman'ın MSDN Magazine makalesini başlıklı bkz: profil oluşturma hakkında daha fazla bilgi için [profil oluşturma, veritabanı etkinliğini içindeki varlık çerçevesi](https://msdn.microsoft.com/magazine/gg490349.aspx).

### <a name="103-database-logger"></a>10.3 veritabanı Günlükçü

Entity Framework 6 kullanıyorsanız de yerleşik günlük kaydetme işlevini kullanarak göz önünde bulundurun. Bağlam veritabanı özelliği, etkinlik basit bir satır yapılandırma yoluyla oturum sağlanabilir:

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

Bu örnekte, veritabanı etkinliğini konsolda günlüğe kaydedilir, ancak herhangi bir eylemi çağırmak için günlük özelliği yapılandırılabilir&lt;dize&gt; temsilci.

Etkinleştirmek istiyorsanız veritabanı günlüğü olmadan yeniden derlemeden ve Entity Framework 6.1 kullanarak veya daha sonra uygulamanızın web.config veya app.config dosyasında bir dinleyiciyi ekleyerek bunu yapabilirsiniz.

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

Git yeniden derlemeye gerek kalmadan günlüğe kaydetme ekleme hakkında daha fazla bilgi için \< http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.

## <a name="11-appendix"></a>11 ek

### <a name="111-a-test-environment"></a>11.1 A. Test Ortamı

Bu ortam, ayrı bir makineye istemci uygulamadan alınan veritabanı ile 2 makine Kurulum kullanır. Ağ gecikme süresi düşük, ancak daha gerçekçi bir tek makineli ortamından, bu nedenle aynı rafa makinelerdir.

#### <a name="1111-------app-server"></a>11.1.1 uygulama sunucusu

##### <a name="11111------software-environment"></a>11.1.1.1 yazılım ortamı

-   Entity Framework 4 yazılım ortamı
    -   İşletim sistemi adı: Windows Server 2008 R2 Enterprise SP1.
    -   Visual Studio 2010 – Ultimate.
    -   Visual Studio 2010 SP1 (yalnızca bazı karşılaştırmalar için).
-   Entity Framework 5 ve 6 yazılım ortamı
    -   İşletim sistemi adı: Windows 8.1 Enterprise
    -   Visual Studio 2013 – Ultimate.

##### <a name="11112------hardware-environment"></a>11.1.1.2 donanım ortamı

-   Çift işlemci: Intel(R) Xeon(R) CPU L5520 W3530 2.27 GHz @ 2261 Mhz8 GHz, 4 çekirdek, 84 mantıksal işlemci.
-   2412 GB RamRAM.
-   4 bölüme bölme 136 GB SCSI250GB SATA 7200 rpm 3 GB/sn sürücüsü.

#### <a name="1112-------db-server"></a>11.1.2 DB sunucusu

##### <a name="11121------software-environment"></a>11.1.2.1 yazılım ortamı

-   İşletim sistemi adı: Windows Server 2008 R28.1 Enterprise SP1.
-   SQL Server 2008 R22012.

##### <a name="11122------hardware-environment"></a>11.1.2.2 donanım ortamı

-   Tek bir işlemcinin: Intel(R) Xeon(R) CPU L5520 2.27 GHz @ 2261 MhzES-1620 0 @ 3.60 GHz, 4 çekirdek, 8 mantıksal işlemci.
-   824 GB RamRAM.
-   4 bölüme bölme 465 GB ATA500GB SATA 7200 rpm 6 GB/sn sürücüsü.

### <a name="112------b-query-performance-comparison-tests"></a>11.2 sorgu b testleri karşılaştırma

Northwind modeli, bu testleri yürütmek için kullanıldı. Bu, Entity Framework designer kullanarak veritabanı oluşturuldu. Ardından, aşağıdaki kod, sorgu yürütme seçeneklerini performansını karşılaştırmak için kullanılan:

``` csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Common;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Data.Objects;
using System.Linq;

namespace QueryComparison
{
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
                );

        public IQueryable<Product> InvokeProductsForCategoryCQ(string categoryName)
        {
            return productsForCategoryCQ(this, categoryName);
        }
    }

    public class QueryTypePerfComparison
    {
        private static string entityConnectionStr = @"metadata=res://*/Northwind.csdl|res://*/Northwind.ssdl|res://*/Northwind.msl;provider=System.Data.SqlClient;provider connection string='data source=.;initial catalog=Northwind;integrated security=True;multipleactiveresultsets=True;App=EntityFramework'";

        public void LINQIncludingContextCreation()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTracking()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                context.Products.MergeOption = MergeOption.NoTracking;

                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void CompiledQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                var q = context.InvokeProductsForCategoryCQ("Beverages");
                q.ToList();
            }
        }

        public void ObjectQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
                products.ToList();
            }
        }

        public void EntityCommand()
        {
            using (EntityConnection eConn = new EntityConnection(entityConnectionStr))
            {
                eConn.Open();
                EntityCommand cmd = eConn.CreateCommand();
                cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

                using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
                {
                    List<Product> productsList = new List<Product>();
                    while (reader.Read())
                    {
                        DbDataRecord record = (DbDataRecord)reader.GetValue(0);

                        // 'materialize' the product by accessing each field and value. Because we are materializing products, we won't have any nested data readers or records.
                        int fieldCount = record.FieldCount;

                        // Treat all products as Product, even if they are the subtype DiscontinuedProduct.
                        Product product = new Product();  

                        product.ProductID = record.GetInt32(0);
                        product.ProductName = record.GetString(1);
                        product.SupplierID = record.GetInt32(2);
                        product.CategoryID = record.GetInt32(3);
                        product.QuantityPerUnit = record.GetString(4);
                        product.UnitPrice = record.GetDecimal(5);
                        product.UnitsInStock = record.GetInt16(6);
                        product.UnitsOnOrder = record.GetInt16(7);
                        product.ReorderLevel = record.GetInt16(8);
                        product.Discontinued = record.GetBoolean(9);

                        productsList.Add(product);
                    }
                }
            }
        }

        public void ExecuteStoreQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectResult<Product> beverages = context.ExecuteStoreQuery<Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Database.SqlQuery\<QueryComparison.DbC.Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbSet()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Products.SqlQuery(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void LINQIncludingContextCreationDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTrackingDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var q = context.Products.AsNoTracking().Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }
    }
}
```

### <a name="113-c-navision-model"></a>11,3 c Navision modeli

Microsoft Dynamics – NAV tanıtım için kullanılan büyük bir veritabanı Navision veritabanıdır Oluşturulan kavramsal model 1005 varlık setleri ve ilişki Setleri 4227 içerir. Testte kullanılan model "Düz" – hiçbir devralma için eklendi.

#### <a name="1131-queries-used-for-navision-tests"></a>11.3.1 Navision testler için kullanılan sorguları

Navision modeliyle kullanılan sorguları listesi Entity SQL sorguları 3 kategorilerini içerir:

##### <a name="11311-lookup"></a>11.3.1.1 arama

Basit arama sorgusuyla toplama

-   Sayısı: 16232
-   Örnek:

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312-singleaggregating"></a>11.3.1.2 SingleAggregating

Normal bir BI sorgu birden çok toplama işlemi, ancak hiçbir alt toplamlar (tek sorgu)

-   Sayısı: 2313
-   Örnek:

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

Burada MDF\_SessionLogin\_zaman\_Max() model olarak tanımlanmıştır:

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313-aggregatingsubtotals"></a>11.3.1.3 AggregatingSubtotals

Bir BI sorgu toplamalar ve alt toplamlar (aracılığıyla tüm birleşimi)

-   Sayısı: 178
-   Örnek:

``` xml
  <Query complexity="AggregatingSubtotals">
    <CommandText>
using NavisionFK;
function AmountConsumed(entities Collection([CRONUS_International_Ltd__Zone])) as
(
    Edm.Sum(select value N.Block_Movement FROM entities as E, E.CRONUS_International_Ltd__Bin as N)
)
function AmountConsumed(P1 Edm.Int32) as
(
    AmountConsumed(select value e from NavisionFKContext.CRONUS_International_Ltd__Zone as e where e.Zone_Ranking = P1)
)
----------------------------------------------------------------------------------------------------------------------
(
    select top(10) Zone_Ranking, Cross_Dock_Bin_Zone, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking, E.Cross_Dock_Bin_Zone
)
union all
(
    select top(10) Zone_Ranking, Cast(null as Edm.Byte) as P2, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking
)
union all
{
    Row(Cast(null as Edm.Int32) as P1, Cast(null as Edm.Byte) as P2, AmountConsumed(select value E
                                                                         from NavisionFKContext.CRONUS_International_Ltd__Zone as E
                                                                         where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed))
}</CommandText>
    <Parameters>
      <Parameter Name="MinAmountConsumed" DbType="Int32" Value="10000" />
    </Parameters>
  </Query>
```
