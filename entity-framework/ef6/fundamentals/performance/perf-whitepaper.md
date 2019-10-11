---
title: EF4, EF5 ve EF6-EF6 için performans konuları
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: 07eb605f0d39f0c1bcfe781540525180f0dd0b22
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181655"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a>EF 4, 5 ve 6 için performans konuları
David OBANDO, Eric Dettu ve diğerleri

Yayımladığı 2012 Nisan

Son güncelleme tarihi: Mayıs 2014

------------------------------------------------------------------------

## <a name="1-introduction"></a>1. Giriş

Nesne Ilişkisel eşleme çerçeveleri, nesne odaklı bir uygulamada veri erişimi için bir soyutlama sağlamanın kullanışlı bir yoludur. .NET uygulamaları için Microsoft 'un önerilen O/RM Entity Framework. Ancak, herhangi bir soyutlama ile performans sorun oluşturabilir.

Bu Teknik İnceleme, Entity Framework kullanarak uygulamalar geliştirirken, geliştiricilere performansı etkileyebilecek Entity Framework Dahili algoritmalar hakkında fikir vermek ve araştırma için ipuçları sağlamak üzere yazılmıştır. Entity Framework kullanan uygulamalarında performansı artırma. Web 'de zaten performans üzerinde bulunan çok sayıda iyi konu vardır ve mümkün olduğunda bu kaynaklara işaret eden bir daha da çalıştık.

Performans, karmaşık bir konudur. Bu Teknik İnceleme, Entity Framework kullanan uygulamalarınız için performans ile ilgili kararlar almanıza yardımcı olacak bir kaynak olarak hazırlanmıştır. Performansı göstermek için bazı test ölçümleri ekledik, ancak bu ölçümler uygulamanızda göreceğiniz performansın mutlak göstergeleri olarak tasarlanmamıştır.

Pratik amaçlarla, bu belge Entity Framework 4 ' ün .NET 4,0 altında çalıştırıldığını varsayar ve Entity Framework 5 ve 6 .NET 4,5 altında çalıştırılır. Entity Framework 5 ' te yapılan performans geliştirmelerinden birçoğu, .NET 4,5 ile birlikte gelen çekirdek bileşenler içinde bulunur.

Entity Framework 6 bant dışı bir sürümdür ve .NET ile birlikte gelen Entity Framework bileşenlerine bağlı değildir. Entity Framework 6 hem .NET 4,0 hem de .NET 4,5 üzerinde çalışır ve .NET 4,0 'den yükseltilmeyen ancak uygulamalarınızda en son Entity Framework bitleri istediğiniz büyük bir performans avantajı sunabilir. Bu belgede Entity Framework 6 ' dan bahsetme, bu yazma sırasında bulunan en son sürüme başvurur: sürüm 6.1.0.

## <a name="2-cold-vs-warm-query-execution"></a>2. Soğuk ile Sıcak sorgu yürütme

Bazı sorgular belirli bir modele göre ilk kez yapıldığında Entity Framework, arka planda modeli yüklemek ve doğrulamak için çok sayıda iş yapar. Bu ilk sorguya "soğuk" sorgusu olarak sıklıkla başvurduk.  Önceden yüklenmiş bir modele yönelik daha fazla sorgu "normal" sorgular olarak bilinir ve çok daha hızlıdır.

Entity Framework kullanarak bir sorgu yürütürken nerede harcandığına ilişkin üst düzey bir görünüm atalım ve Entity Framework 6 ' da nesnelerin nerede geliştiğini görün.

**İlk sorgu yürütme – soğuk sorgu**

| Kod Kullanıcı yazmaları                                                                                     | Action                    | EF4 performans etkisi                                                                                                                                                                                                                                                                                                                                                                                                        | EF5 performans etkisi                                                                                                                                                                                                                                                                                                                                                                                                                                                    | EF6 performans etkisi                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Bağlam oluşturma          | Orta                                                                                                                                                                                                                                                                                                                                                                                                                        | Orta                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Sorgu ifadesi oluşturma | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                           | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | LINQ sorgu yürütme      | -Meta veri yükleme: Yüksek ancak önbelleğe alınmış <br/> -Üretimi görüntüle: Potansiyel olarak çok yüksek ancak önbelleğe alınmış <br/> -Parametre değerlendirmesi: Orta <br/> -Sorgu çevirisi: Orta <br/> -Materializer oluşturma: Orta, ancak önbelleğe alınmış <br/> -Veritabanı sorgu yürütme: Potansiyel olarak yüksek <br/> + Connection. Open <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Nesne materialization: Orta <br/> -Kimlik arama: Orta | -Meta veri yükleme: Yüksek ancak önbelleğe alınmış <br/> -Üretimi görüntüle: Potansiyel olarak çok yüksek ancak önbelleğe alınmış <br/> -Parametre değerlendirmesi: Düşük <br/> -Sorgu çevirisi: Orta, ancak önbelleğe alınmış <br/> -Materializer oluşturma: Orta, ancak önbelleğe alınmış <br/> -Veritabanı sorgu yürütme: Potansiyel olarak yüksek (bazı durumlarda daha Iyi sorgular) <br/> + Connection. Open <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Nesne materialization: Orta <br/> -Kimlik arama: Orta | -Meta veri yükleme: Yüksek ancak önbelleğe alınmış <br/> -Üretimi görüntüle: Orta, ancak önbelleğe alınmış <br/> -Parametre değerlendirmesi: Düşük <br/> -Sorgu çevirisi: Orta, ancak önbelleğe alınmış <br/> -Materializer oluşturma: Orta, ancak önbelleğe alınmış <br/> -Veritabanı sorgu yürütme: Potansiyel olarak yüksek (bazı durumlarda daha Iyi sorgular) <br/> + Connection. Open <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Nesne materialization: Orta (EF5 'den daha hızlı) <br/> -Kimlik arama: Orta |
| `}`                                                                                                  | Connection. Close          | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                           | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


**İkinci sorgu yürütme – sıcak sorgu**

| Kod Kullanıcı yazmaları                                                                                     | Action                    | EF4 performans etkisi                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | EF5 performans etkisi                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | EF6 performans etkisi                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Bağlam oluşturma          | Orta                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Orta                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Sorgu ifadesi oluşturma | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | LINQ sorgu yürütme      | -Meta veri ~~yükleme~~ araması: ~~Yüksek ancak önbelleğe alınmış~~ Zayıf <br/> - ~~Oluşturma~~ aramasını görüntüle: ~~Potansiyel olarak çok yüksek ancak önbelleğe alınmış~~ Zayıf <br/> -Parametre değerlendirmesi: Orta <br/> -Sorgu ~~çevirisi~~ arama: Orta <br/> -Materializer ~~oluşturma~~ araması: ~~Orta, ancak önbelleğe alınmış~~ Zayıf <br/> -Veritabanı sorgu yürütme: Potansiyel olarak yüksek <br/> + Connection. Open <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Nesne materialization: Orta <br/> -Kimlik arama: Orta | -Meta veri ~~yükleme~~ araması: ~~Yüksek ancak önbelleğe alınmış~~ Zayıf <br/> - ~~Oluşturma~~ aramasını görüntüle: ~~Potansiyel olarak çok yüksek ancak önbelleğe alınmış~~ Zayıf <br/> -Parametre değerlendirmesi: Düşük <br/> -Sorgu ~~çevirisi~~ arama: ~~Orta, ancak önbelleğe alınmış~~ Zayıf <br/> -Materializer ~~oluşturma~~ araması: ~~Orta, ancak önbelleğe alınmış~~ Zayıf <br/> -Veritabanı sorgu yürütme: Potansiyel olarak yüksek (bazı durumlarda daha Iyi sorgular) <br/> + Connection. Open <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Nesne materialization: Orta <br/> -Kimlik arama: Orta | -Meta veri ~~yükleme~~ araması: ~~Yüksek ancak önbelleğe alınmış~~ Zayıf <br/> - ~~Oluşturma~~ aramasını görüntüle: ~~Orta, ancak önbelleğe alınmış~~ Zayıf <br/> -Parametre değerlendirmesi: Düşük <br/> -Sorgu ~~çevirisi~~ arama: ~~Orta, ancak önbelleğe alınmış~~ Zayıf <br/> -Materializer ~~oluşturma~~ araması: ~~Orta, ancak önbelleğe alınmış~~ Zayıf <br/> -Veritabanı sorgu yürütme: Potansiyel olarak yüksek (bazı durumlarda daha Iyi sorgular) <br/> + Connection. Open <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Nesne materialization: Orta (EF5 'den daha hızlı) <br/> -Kimlik arama: Orta |
| `}`                                                                                                  | Connection. Close          | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Düşük                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


Hem soğuk hem de sıcak sorguların performans maliyetini düşürmenin birkaç yolu vardır ve aşağıdaki bölümde bunlara göz atalım. Özellikle, görünüm oluşturma sırasında karşılaşılan performans pakanları azaltmaya yardımcı olması gereken önceden oluşturulmuş görünümler kullanarak, soğuk sorgularda model yükleme maliyetini azaltmaya bakacağız. Isınma sorguları için sorgu planı önbelleğe alma, izleme sorgusu yok ve farklı sorgu yürütme seçenekleri ele alınacaktır.

### <a name="21-what-is-view-generation"></a>2,1 görünüm oluşturma nedir?

Hangi görünüm oluşturmanın olduğunu anlamak için öncelikle "eşleme görünümlerini" ne olduğunu anlamanız gerekir. Eşleme görünümleri, her bir varlık kümesi ve ilişkilendirmesi için eşlemede belirtilen dönüştürmelerin yürütülebilir temsilleridir. Dahili olarak, bu eşleme görünümleri CQTs (kurallı sorgu ağaçları) şeklini alır. İki tür eşleme görünümü vardır:

-   Sorgu görünümleri: Bu, veritabanı şemasından kavramsal modele gitmek için gereken dönüşümü temsil eder.
-   Güncelleştirme görünümleri: Bu, kavramsal modelden veritabanı şemasına gitmek için gereken dönüşümü temsil eder.

Kavramsal modelin, veritabanı şemasından çeşitli yollarla farklı olabileceğini aklınızda bulundurun. Örneğin, bir tek tablo, verileri iki farklı varlık türü için depolamak üzere kullanılabilir. Devralma ve önemsiz olmayan eşlemeler, eşleme görünümlerinin karmaşıklığından bir rol oynar.

Bu görünümleri eşlemenin belirtimine göre hesaplama işlemi, görünüm oluşturmayı çağırdığımız şeydir. Görünüm üretimi, bir model yüklendiğinde veya derleme zamanında "önceden oluşturulmuş görünümler" kullanılarak dinamik olarak yapılabilir; İkincisi, Entity SQL deyimleri biçiminde bir C @ no__t-0 veya VB dosyasına serileştirilir.

Görünümler oluşturulduğunda, bunlar da doğrulanmaz. Performans açısından, görünüm oluşturma maliyetinin büyük bir bölümü aslında, varlıklar arasındaki bağlantıların anlamlı hale gelmesini ve desteklenen tüm işlemler için doğru kardinalite olmasını sağlayan görünümlerin doğrulanmasından oluşur.

Bir varlık kümesi üzerinde bir sorgu yürütüldüğünde, sorgu ilgili sorgu görünümüyle birleştirilir ve bu birleşimin sonucu, yedekleme deposunun anlayabileceği sorgunun gösterimini oluşturmak için plan derleyicisi aracılığıyla çalıştırılır. SQL Server için, bu derlemenin nihai sonucu bir T-SQL SELECT deyimidir. Bir varlık kümesi üzerinde ilk kez güncelleştirme gerçekleştirildiğinde güncelleştirme görünümü, hedef veritabanı için DML deyimlerine dönüştürmek üzere benzer bir işlem aracılığıyla çalıştırılır.

### <a name="22-factors-that-affect-view-generation-performance"></a>Görünüm oluşturma performansını etkileyen 2,2 faktörleri

Görünüm oluşturma adımının performansı yalnızca modelinizin boyutuna ve ayrıca modelin nasıl bağlantılı olduğuna bağlı olarak farklılık gösterir. İki varlık bir devralma zinciri veya bir Ilişki aracılığıyla bağlandıysa, bağlandıkları söylenir. Benzer şekilde, iki tablo bir yabancı anahtar aracılığıyla bağlandıysa, bunlar bağlanır. Şemalarınızdaki bağlantılı varlıkların ve tabloların sayısı arttıkça, görünüm oluşturma maliyeti artar.

Görünümleri oluşturmak ve doğrulamak için kullandığımız algoritma en kötü durumda üstel olur, ancak bunu geliştirmek için bazı iyileştirmeler kullanıyoruz. Performansı olumsuz yönde etkileyen en büyük faktörler şunlardır:

-   Model boyutu, varlıkların sayısına ve bu varlıklar arasındaki ilişkilerin miktarına işaret eder.
-   Model karmaşıklığı, özellikle de çok sayıda tür içeren devralma.
-   Yabancı anahtar Ilişkilendirmeleri yerine bağımsız Ilişkilendirmeler kullanma.

Küçük ve basit modeller için maliyet, önceden üretilmiş görünümler kullanılarak oluşmayacak kadar küçük olabilir. Model boyutu ve karmaşıklığı artdıkça, görünüm oluşturma ve doğrulama maliyetini azaltmak için çeşitli seçenekler mevcuttur.

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a>2,3 model yükleme süresini azaltmak için önceden oluşturulmuş görünümleri kullanma

Entity Framework 6 ' da önceden oluşturulmuş görünümleri kullanma hakkında ayrıntılı bilgi için [önceden oluşturulmuş eşleme görünümlerini](~/ef6/fundamentals/performance/pre-generated-views.md) ziyaret edin

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools-community-edition"></a>Entity Framework Power Tools Community Edition 'ı kullanarak önceden üretilmiş Görünümler 2.3.1

Model sınıf dosyasına sağ tıklayıp "görünüm üret" i seçmek için Entity Framework menüsünü kullanarak EDMX ve Code First modellerinin görünümlerini oluşturmak için [Entity Framework 6 Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) 'ı kullanabilirsiniz. Entity Framework Power Tools Community Edition yalnızca DbContext ile türetilmiş bağlamlarda çalışır.

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a>2.3.2, EDMGen tarafından oluşturulan bir modelle önceden oluşturulmuş görünümleri nasıl kullanacağınızı

EDMGen, .NET ile birlikte gelen ve Entity Framework 4 ve 5 ile çalışan, ancak Entity Framework 6 ile birlikte çalışan bir yardımcı programdır. EDMGen, komut satırından bir model dosyası, nesne katmanı ve görünümler oluşturmanıza olanak sağlar. Çıktılardan biri, tercih ettiğiniz dilde bir görünüm dosyası, VB veya C @ no__t-0 olacaktır. Bu, her varlık kümesi için Entity SQL parçacıkları içeren bir kod dosyasıdır. Önceden oluşturulmuş görünümleri etkinleştirmek için, dosyayı projenize eklemeniz yeterlidir.

Model için şema dosyalarında el ile düzenleme yaparsanız, görünümler dosyasını yeniden oluşturmanız gerekecektir. Bunu, **/Mode: ViewGeneration** bayrağıyla EDMGen çalıştırarak yapabilirsiniz.

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a>2.3.3 bir EDMX dosyası ile önceden oluşturulmuş görünümleri kullanma

Ayrıca, bir EDMX dosyasının görünümlerini oluşturmak için EDMGen ' yı kullanabilirsiniz. daha önce başvurulan MSDN konusu bunu yapmak için oluşturma öncesi bir olay eklemeyi açıklar; ancak bu karmaşıktır ve mümkün olmayan bazı durumlar vardır. Modeliniz bir edmx dosyasında olduğunda görünümleri oluşturmak için bir T4 şablonu kullanmak genellikle daha kolay olur.

Görünümü oluşturmak için T4 şablonu kullanmayı açıklayan bir gönderi ADO.NET ekibi blogu vardır ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>). Bu gönderi, indirilip projenize eklenebilen bir şablon içerir. Şablon, Entity Framework ilk sürümü için yazıldığı için, en son Entity Framework sürümleriyle çalışmayı garanti edilmez. Ancak, Visual Studio galerisinden Entity Framework 4 ve 5 için daha güncel bir görünüm oluşturma şablonları yükleyebilirsiniz:

-   VB.NET: \< @ NO__T-1
-   C @ NO__T-0: \< @ NO__T-2

Entity Framework 6 kullanıyorsanız, görünüm oluşturma T4 şablonlarını konumundaki Visual Studio Galerisi'nden alabileceğiniz \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.

### <a name="24-reducing-the-cost-of-view-generation"></a>2,4 görüntü oluşturma maliyetini azaltma

Önceden oluşturulmuş görünümleri kullanmak, görünüm oluşturma maliyetini model yüklemeden (çalışma zamanı) tasarım zamanına taşıdır. Bu, çalışma zamanında başlangıç performansını artırırken, geliştirilirken görünüm oluşturma konusunda hala sorun yaşar. Hem derleme zamanında hem de çalışma zamanında görünüm oluşturma maliyetini azaltmaya yardımcı olabilecek çeşitli ek püf noktaları vardır.

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a>2.4.1, görünüm oluşturma maliyetini azaltmak için yabancı anahtar Ilişkilendirmelerini kullanma

Modeldeki ilişkilerin bağımsız ilişkilerden yabancı anahtar Ilişkilendirmelerine geçişi, görünüm oluşturma sırasında harcanan sürenin önemli ölçüde iyileştirilmesinden çok sayıda durum gördük.

Bu geliştirmeyi göstermek için, EDMGen kullanarak Navision modelinin iki sürümünü oluşturduk. *Note: Navision modelinin açıklaması için bkz. Ek C.* Navision modeli, Bu alıştırmada aralarındaki çok büyük miktarda varlık ve ilişki olması nedeniyle bu alıştırma için ilginç.

Bu çok büyük modelin bir sürümü yabancı anahtarlar Ilişkilendirmeleriyle oluşturulmuştur ve diğeri bağımsız Ilişkilendirmelerde oluşturulmuştur. Daha sonra her bir model için görünümleri oluşturma süresinin ne kadar sürdüğünü zaman aşımına uğradık. Entity Framework 5 testi, görünümleri oluşturmak için EntityViewGenerator sınıfından GenerateViews () yöntemini kullandı, Entity Framework 6 testi StorageMappingItemCollection sınıfından GenerateViews () yöntemini kullandı. Bu, Entity Framework 6 kod tabanında oluşan kod yeniden yapılandırma nedeniyle oluştu.

Entity Framework 5 ' ü kullanarak, yabancı anahtarlarla model oluşturma, bir laboratuvar makinesinde 65 dakika sürdü. Bağımsız İlişkilendirmelerin kullanıldığı modelin görünümlerini oluşturmak için ne kadar süre sürdüğünü bilinmiyor. Aylık güncelleştirmeleri yüklemek için makineyi laboratuvarımızda yeniden başlatılmadan önce, çalıştırılan testi bir ay boyunca çalıştırdık.

Entity Framework 6 ' yı kullanarak, yabancı anahtarlarla model oluşturma işlemi aynı laboratuvar makinesinde 28 saniye sürdü. Bağımsız Ilişkilerin kullanıldığı model için görünüm üretimi 58 saniye sürdü. Görünüm oluşturma kodu üzerinde 6 Entity Framework yapılan iyileştirmeler, birçok projenin daha hızlı başlangıç süreleri elde etmek için önceden oluşturulmuş görünümlere gerek duymayacağına bağlıdır.

Entity Framework 4 ve 5 ' teki önceden üretilen görünümlerin EDMGen veya Entity Framework güç araçlarıyla yapılabilirler. Entity Framework 6 görünüm üretimi için, [önceden oluşturulmuş eşleme görünümlerinde](~/ef6/fundamentals/performance/pre-generated-views.md)açıklandığı şekilde, Entity Framework güç araçları veya program aracılığıyla yapılabilir.

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a>2.4.1.1 bağımsız Ilişkilendirmeler yerine yabancı anahtarlar kullanma

EDMGen veya Visual Studio 'da Entity Desisgner kullanırken, varsayılan olarak, FKs 'ler ve IAS arasında geçiş yapmak için yalnızca tek bir onay kutusu ya da komut satırı bayrağı alır.

Büyük bir Code First modeliniz varsa, bağımsız Ilişkilerin kullanılması görünüm üretimi üzerinde aynı etkiye sahip olur. Bağımlı nesnelerinizin sınıflarına yabancı anahtar özellikleri ekleyerek bu etkiden kaçınabilirsiniz, ancak bazı geliştiriciler bunu nesne modellerini yoklamak üzere kabul eder. Bu konu hakkında daha fazla bilgi bulabilirsiniz \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.

| Kullanırken      | Bunu yapın                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Entity Desisgner | İki varlık arasında bir ilişki ekledikten sonra, bir başvuru kısıtlamasına sahip olduğunuzdan emin olun. Başvurusal kısıtlamalar Entity Framework bağımsız Ilişkilendirmeler yerine yabancı anahtarlar kullanmasını söyler. Ek ayrıntılar için ziyaret edin \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>. |
| EDMGen          | Veritabanından dosyaları oluşturmak için EDMGen kullanıldığında, yabancı anahtarlarınız dikkate alınır ve bu şekilde modele eklenir. EDMGen tarafından kullanıma sunulan farklı seçenekler hakkında daha fazla bilgi için ziyaret [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx).                           |
| Code First      | Code First kullanırken bağımlı nesnelere yabancı anahtar özellikleri ekleme hakkında bilgi için [Code First kuralları](~/ef6/modeling/code-first/conventions/built-in.md) konusunun "ilişki kuralı" bölümüne bakın.                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a>2.4.2 sections modelinizi ayrı bir derlemeye taşıma

Modeliniz doğrudan uygulamanızın projesine dahil edildiğinde ve oluşturma öncesi bir olay veya T4 şablonu aracılığıyla görünümler oluşturursanız, model değiştirilmese bile, oluşturma ve doğrulama proje yeniden oluşturulduğunda gerçekleşir. Modeli ayrı bir derlemeye taşır ve uygulamanızın projesinden başvurmanız durumunda, modeli içeren projeyi yeniden derlemeye gerek kalmadan uygulamanızda başka değişiklikler yapabilirsiniz.

*Note:*  modelinizi ayrı derlemelere taşırken, modelin bağlantı dizelerini istemci projesinin uygulama yapılandırma dosyasına kopyalamayı unutmayın.

#### <a name="243-disable-validation-of-an-edmx-based-model"></a>2.4.3 bir edmx tabanlı modelin doğrulanmasını devre dışı bırakma

Model değiştirilmemiş olsa bile, EDMX modelleri derleme zamanında onaylanır. Modeliniz zaten doğrulanmışsa, Özellikler penceresinde "derleme üzerinde Doğrula" özelliğini ayarlayarak derleme zamanında doğrulamayı gizleyebilirsiniz. Eşlemenizi veya modelinizi değiştirdiğinizde, değişikliklerinizi doğrulamak için geçici olarak doğrulamayı yeniden etkinleştirebilirsiniz.

Entity Framework 6 için Entity Framework Designer performans geliştirmeleri yapılmıştır ve "derleme üzerinde Doğrula" maliyeti, tasarımcının önceki sürümlerinden çok daha düşüktür.

## <a name="3-caching-in-the-entity-framework"></a>Entity Framework 3 önbelleğe alma

Entity Framework, aşağıdaki önbelleğe alma yerleşik biçimlerine sahiptir:

1.  Nesne önbelleğe alma – bir ObjectContext örneğine yerleşik olan ObjectStateManager, bu örneği kullanarak alınan nesnelerin belleğinde izler. Bu, birinci düzey önbellek olarak da bilinir.
2.  Sorgu planı önbelleğe alma-bir sorgu birden çok kez yürütüldüğünde oluşturulan depo komutunu yeniden kullanma.
3.  Meta veri önbelleğe alma-model için meta verileri aynı modele farklı bağlantılarda paylaşma.

EF 'in kullanıma sunduğu önbelleklerin yanı sıra, sarmalama sağlayıcısı olarak bilinen özel bir tür ADO.NET veri sağlayıcısı, veritabanından alınan sonuçlar için, ikinci düzey önbelleğe alma olarak da bilinen bir önbellekle Entity Framework genişletmek için de kullanılabilir.

### <a name="31-object-caching"></a>3,1 nesne önbelleğe alma

Varsayılan olarak, bir varlık bir sorgunun sonuçlarında döndürüldüğünde, yalnızca EF 'i çalıştırmadan önce, ObjectContext 'e aynı anahtara sahip bir varlığın zaten yüklenmiş olup olmadığını kontrol eder. Aynı anahtarlara sahip bir varlık zaten varsa, bunu sorgunun sonuçlarına dahil eder. EF, sorguyu veritabanına karşı almaya devam edebilse de, bu davranış, varlığı birden çok kez gerçekleştirme maliyetinin çoğunu atlayabilir.

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a>3.1.1 DbContext Find kullanarak nesne önbelleğinden varlık alma

Normal bir sorgunun aksine, DbSet 'teki bul Yöntemi (EF 4,1 ' de ilk kez dahil edilen API 'Ler), sorguyu veritabanına karşı vermeden önce bellekte bir arama yapar. İki farklı ObjectContext örneğinin iki farklı ObjectStateManager örneğine sahip olacağını, yani ayrı nesne önbellekleri olduğunu unutmayın.

Find, bağlam tarafından izlenen bir varlığı bulmayı denemek için birincil anahtar değerini kullanır. Varlık bağlamda değilse, bir sorgu yürütülür ve veritabanına göre değerlendirilir ve varlık bağlamda veya veritabanında bulunmazsa null değeri döndürülür. Bul 'un Ayrıca içeriğe eklenmiş ancak henüz veritabanına kaydedilmemiş olan varlıkları döndürdüğünü unutmayın.

Bul kullanılırken dikkate alınması gereken bir performans vardır. Bu yönteme varsayılan olarak yapılan çağrılar, hala veritabanına kaydedilmesi bekleyen değişiklikleri algılamak için nesne önbelleğinin doğrulanmasını tetikler. Nesne önbelleğinde çok fazla sayıda nesne varsa veya nesne önbelleğine eklenen büyük bir nesne grafiğinde bu işlem çok pahalı olabilir, ancak devre dışı bırakılabilir. Belirli durumlarda, değişiklikleri otomatik olarak algılamayı devre dışı bıraktığınızda Find metodunu çağırma konusunda farklılık gösterebilir. İkinci Büyüklük sırası, nesne gerçekten önbellekte olduğunda, nesnenin veritabanından alınması gerektiğinde algılanır. Aşağıda, 5000 varlıkların bir yüküne göre milisaniye olarak ifade edilen mikro kıyaslamalarımızın bazıları kullanılarak alınan ölçümlere örnek bir grafik verilmiştir:

![.Net 4,5 Logaritmik ölçek](~/ef6/media/net45logscale.png ".NET 4,5-Logaritmik ölçek")

Değişiklik otomatik algılama devre dışı olan bul örneği:

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

Find metodunu kullanırken göz önünde bulundurmanız gerekenler şunlardır:

1.  Nesne önbellekte değilse, bulma avantajları de daha basit olur, ancak sözdizimi anahtara göre sorgudan hala daha basittir.
2.  Değişiklikleri otomatik algıla etkinleştirilirse, bulma yönteminin maliyeti tek bir büyüklük sırasıyla artabilir veya modelinizin karmaşıklığına ve nesne önbelleğinizin varlık miktarına bağlı olarak daha da fazla olabilir.

Ayrıca, bul 'un yalnızca aradığınız varlığı döndürdüğünü ve zaten nesne önbelleğinde değilse ilişkili varlıklarını otomatik olarak yüklemediğini unutmayın. İlişkili varlıkları almanız gerekiyorsa, bir sorgu ile bir sorgu kullanarak bir sorgu kullanabilirsiniz. Daha fazla bilgi için bkz. **8,1 geç yükleme vs. @ No__t-0 yüklenirken Eager yükleniyor.

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a>Nesne önbelleğinde çok sayıda varlık olduğunda performans sorunları 3.1.2

Nesne önbelleği, Entity Framework genel yanıt hızını artırmaya yardımcı olur. Ancak, nesne önbelleğinde çok büyük miktarda varlık varsa, ekleme, kaldırma, bulma, giriş, SaveChanges ve daha fazlası gibi bazı işlemleri etkileyebilir. Özellikle, DetectChanges çağrısı tetikleyen işlemler, çok büyük nesne önbellekler tarafından olumsuz etkilenir. DetectChanges, nesne grafiğini nesne durumu yöneticisiyle eşitler ve bu, performansı doğrudan nesne grafiğinin boyutuyla belirlenir. DetectChanges hakkında daha fazla bilgi için bkz. [POCO varlıklarındaki değişiklikleri izleme](https://msdn.microsoft.com/library/dd456848.aspx).

Entity Framework 6 kullanırken, geliştiriciler bir koleksiyon üzerinde yineleme yapmak yerine doğrudan bir DbSet üzerinde AddRange ve RemoveRange ' ı çağırabilir ve örnek başına Add bir kez çağrı yapabilir. Aralık yöntemlerini kullanmanın avantajı, DetectChanges maliyetinin yalnızca bir kez eklenen her varlık için bir kez olmak üzere tüm varlık kümesi için bir kez ödeneceği bir avantajdır.

### <a name="32-query-plan-caching"></a>3,2 sorgu planını önbelleğe alma

Sorgu ilk kez yürütüldüğünde, kavramsal sorguyu mağaza komutuna (örneğin, SQL Server karşı çalıştırıldığında yürütülen T-SQL) dönüştürmek için iç plan derleyicisinden geçer.  Sorgu planı önbelleğe alma etkinse, sorgu bir sonraki yürütüldüğünde, plan derleyicisini atlayarak yürütme için doğrudan sorgu planı önbelleğinden mağaza komutu alınır.

Sorgu planı önbelleği aynı AppDomain içindeki ObjectContext örnekleri arasında paylaşılır. Sorgu planı önbelleklemesi avantajlarından yararlanmak için bir ObjectContext örneğine sahip olmanız gerekmez.

#### <a name="321-some-notes-about-query-plan-caching"></a>Sorgu planı önbelleğe alma hakkında bazı notları 3.2.1

-   Sorgu planı önbelleği tüm sorgu türleri için paylaşılır: Entity SQL, LINQ to Entities ve CompiledQuery nesneleri.
-   Varsayılan olarak, bir EntityCommand aracılığıyla veya bir ObjectQuery aracılığıyla yürütülüp yürütülmeksizin, sorgu planı önbelleğe alma, Entity SQL sorguları için etkinleştirilir. Ayrıca, .NET 4,5 ve Entity Framework 6 ' da Entity Framework LINQ to Entities sorguları için varsayılan olarak etkinleştirilir
    -   Sorgu planı önbelleğe alma, EnablePlanCaching özelliği (EntityCommand veya ObjectQuery üzerinde) false olarak ayarlanarak devre dışı bırakılabilir. Örneğin:
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
-   Parametreli sorgular için, parametrenin değerini değiştirmek, önbelleğe alınmış sorguya yine de devam edecektir. Ancak parametrenin modellerini (örneğin, boyut, duyarlık veya ölçek) değiştirmek önbellekte farklı bir girdiye ulaşacaktır.
-   Entity SQL kullanırken sorgu dizesi anahtarın bir parçasıdır. Sorgu işlevsel olarak eşdeğer olsa bile sorgunun her seferinde değiştirilmesi farklı önbellek girişlerine neden olur. Bu, büyük küçük harf veya boşluk değişiklikleri içerir.
-   LINQ kullanılırken, anahtarın bir parçasını oluşturmak için sorgu işlenir. LINQ ifadesinin değiştirilmesi, bu nedenle farklı bir anahtar oluşturur.
-   Diğer teknik sınırlamalar da uygulanabilir. daha fazla ayrıntı için bkz. oto derlenmiş sorgular.

#### <a name="322-cache-eviction-algorithm"></a>3.2.2 önbellek çıkarma algoritması

İç algoritmanın nasıl çalıştığını anlamak, sorgu planının önbelleğe alınmasını ne zaman etkinleştirebileceğinizi veya devre dışı bırakacağınızı belirlemenize yardımcı olur. Temizleme algoritması aşağıdaki gibidir:

1.  Önbellekte bir dizi girdi (800) varsa, düzenli aralıklarla (dakikada bir kez) bir süreölçeri, önbelleği başlatır.
2.  Önbellek uyumlu EPS 'ler sırasında, girdiler bir LFRU (en az sık kullanılan) temelinde önbellekten kaldırılır. Hangi girişlerin çıkarılcağına karar verirken bu algoritma hem isabet sayısını hem de yaşı hesaba girer.
3.  Her önbellek süpürme sonunda, önbellek 800 girdi içerir.

Hangi girişlerin çıkarılması belirlenirken tüm önbellek girişleri eşit olarak değerlendirilir. Bu, bir CompiledQuery için mağaza komutunun, bir Entity SQL sorgusunun depolama komutu olarak çıkarılması ihtimaline sahip olduğu anlamına gelir.

Önbellekte 800 varlık olduğunda önbellek çıkarma süreölçerinin çıkardığına, ancak önbelleğin yalnızca bu Zamanlayıcı başlatıldıktan sonra yalnızca 60 saniye olduğuna göz önünde kılandı. Yani, önbelleğinizin en fazla 60 saniyeye kadar büyüeceği anlamına gelir.

#### <a name="323-test-metrics-demonstrating-query-plan-caching-performance"></a>sorgu planını önbelleğe alma performansını gösteren 3.2.3 test ölçümleri

Sorgu planı önbelleğinin uygulamanızın performansına etkisini göstermek için, Navision modeline göre çok sayıda Entity SQL sorgusu yürütüyoruz bir test gerçekleştirdik. Navision modelinin açıklaması ve yürütülen sorgu türleri için ek başlığına bakın. Bu testte, ilk olarak sorgu listesi boyunca yineliyoruz ve önbelleğe eklemek için her birini bir kez yürütüyoruz (önbelleğe alma etkinse). Bu adım zaman aşımına uğradı. Daha sonra, önbelleğin sürme durumunda 60 saniye boyunca ana iş parçacığını uykuya geçiririz; son olarak, önbelleğe alınmış sorguların yürütülmesi için ikinci kez listede yineleme yaptık. Ayrıca, her sorgu kümesi, sorgu planı önbelleğinin verdiği avantajı doğru şekilde yansıttığından, her bir sorgu kümesi yürütülmeden önce SQL Server planı önbelleği temizlenir.

##### <a name="3231-test-results"></a>3.2.3.1 Test Sonuçları

| Test etme                                                                   | EF5 önbellek yok | EF5 önbelleğe alındı | EF6 önbellek yok | EF6 önbelleğe alındı |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| Tüm 18723 sorguları numaralandırılıyor                                          | 124          | 125,4      | 124,3        | 125,3      |
| Süpürme (karmaşıklıktan bağımsız olarak yalnızca ilk 800 sorgu)  | 41,7         | 5.5        | 40.5         | 5,4        |
| Yalnızca Aggregatingalt toplamları sorguları (178 toplam, süpürme önlenir) | 39,5         | 4,5        | 38,1         | 4.6        |

*Saniyeler içinde her zaman.*

Moral-çok sayıda farklı sorgu yürütürken (örneğin, dinamik olarak oluşturulan sorgular), önbelleğe alma işlemi yardım etmez ve önbelleğin ortaya çıkmasına yönelik olarak, gerçekten onu kullanarak plan önbelleğinin en iyi şekilde yararlanabileceği sorguları koruyabilir.

Aggregatingalt toplamları sorguları, test ettiğimiz sorguların en karmaşıktır. Beklenildiği gibi, sorgu daha karmaşık olduğunda sorgu planı önbelleğe alma işleminden daha fazla avantaj elde edersiniz.

Bir CompiledQuery, planı önbelleğe alınmış bir LINQ sorgusu olduğundan, bir CompiledQuery ile eşdeğer Entity SQL sorgusunun karşılaştırılması benzer sonuçlara sahip olmalıdır. Aslında, bir uygulamanın çok sayıda dinamik Entity SQL sorgusu varsa, önbelleğin sorgular ile doldurulması, ön bellekten Temizlendiklerinde "derlemeyi parçalara ayırma" işlemi de etkili olur. Bu senaryoda, dinamik sorgular üzerinde önbelleğe alma devre dışı bırakılarak, CompiledQueries önceliklendirilerek performans artırılabilir. Daha iyi kuşkusuz, dinamik sorgular yerine parametreli sorguları kullanmak için uygulamayı yeniden yazmak olacaktır.

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a>LINQ sorgularıyla performansı artırmak için CompiledQuery kullanma 3,3

Sınamalarımız, CompiledQuery kullanmanın, oto derlenen LINQ sorgularının üzerinde% 7 ' nin avantajlarından yararlanabileceğinizi gösteriyor; Bu, Entity Framework yığınından kod yürüten% 7 daha az zaman harcamanız anlamına gelir; Uygulamanızın% 7 daha hızlı olacağını ifade etmez. Genel olarak, f 5,0 ' de CompiledQuery nesnelerini yazma ve korumanın maliyeti avantajlarla karşılaştırıldığında sorun için değer olmayabilir. Gelinmeniz farklılık gösterebilir, bu nedenle projeniz fazladan gönderim gerektiriyorsa bu seçeneği kullanabilirsiniz. CompiledQueries 'in yalnızca ObjectContext ile türetilmiş modellerle uyumlu olduğunu ve DbContext ile türetilmiş modellerle uyumlu olduğunu unutmayın.

Bir CompiledQuery oluşturma ve çağırma hakkında daha fazla bilgi için bkz. [derlenmiş sorgular (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).

Bir CompiledQuery kullanırken yapmanız gereken iki önemli noktalar vardır. Bu, statik örnekler ve bunların birlikte bulunan sorunları kullanma gereksinimidir. Burada, bu iki önemli noktaların derinlemesine bir açıklaması verilmiştir.

#### <a name="331-use-static-compiledquery-instances"></a>3.3.1 statik CompiledQuery örnekleri kullan

LINQ sorgusunun derlenmesi zaman alan bir işlem olduğundan, veritabanından veri getirmeye gerek duyduğumuz her seferinde bunu yapmak istemedik. CompiledQuery örnekleri bir kez derlemenize ve birden çok kez çalıştırmanıza izin verir, ancak aynı CompiledQuery örneğini tekrar tekrar derlemek yerine yeniden kullanmak için dikkatli olmanız ve temin etmeniz gerekir. CompiledQuery örneklerini depolamak için statik üyelerin kullanımı gerekli hale gelir; Aksi takdirde hiçbir avantaj görmezsiniz.

Örneğin, sayfanızın seçili kategori için ürünleri görüntülemeyi işlemek üzere aşağıdaki yöntem gövdesine sahip olduğunu varsayalım:

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

Bu durumda, yöntemi her çağrıldığında anında, yeni bir CompiledQuery örneği oluşturacaksınız. Sorgu planı önbelleğinden mağaza komutunu alarak performans avantajlarını görmek yerine, her yeni örnek oluşturulduğunda CompiledQuery plan derleyicisini alır. Aslında, yöntem her çağrıldığında sorgu planı önbelleğinizi yeni bir CompiledQuery girişi ile yoklacaksınız.

Bunun yerine, derlenmiş sorgunun statik bir örneğini oluşturmak istersiniz; bu nedenle, yöntemi her çağrıldığında aynı derlenmiş sorguyu çağırılır. Bunu yapmanın bir yolu, CompiledQuery örneğini nesne bağlamınızın bir üyesi olarak eklemektir.  Daha sonra bir yardımcı yöntemi aracılığıyla CompiledQuery 'e erişerek bir şeyler daha az temizleyici yapabilirsiniz:

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

Bu yardımcı yöntem aşağıdaki şekilde çağrılabilir:

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-composing-over-a-compiledquery"></a>bir CompiledQuery üzerinde 3.3.2 oluşturma

Herhangi bir LINQ sorgusunu oluşturma özelliği son derece yararlı olur; Bunu yapmak için, bir yöntemi, *Skip ()* veya *Count ()* gibi IQueryable 'dan sonra çağırmanız yeterlidir. Ancak, bunun yapılması aslında yeni bir IQueryable nesnesi döndürür. Bir CompiledQuery üzerinden oluşturmanız için teknik olarak herhangi bir şey yapmayın, ancak bunu yapmak plan derleyicisinin yeniden geçirilmesini gerektiren yeni bir IQueryable nesnesinin oluşturulmasına neden olur.

Bazı bileşenler, gelişmiş işlevleri etkinleştirmek için oluşturulmuş IQueryable nesnelerini kullanır. Örneğin, ASP. NET ' in GridView, bir IQueryable nesnesine, SelectMethod özelliği aracılığıyla veri ile bağlantılı olabilir. GridView daha sonra veri modeli üzerinde sıralama ve sayfalama sağlamak için bu IQueryable nesnesini oluşturacak. Gördüğünüz gibi, GridView için bir CompiledQuery kullanmak derlenen sorguya ulaşmaz, ancak yeni bir oto derlenmiş sorgu oluşturur.

Bu, bir sorguya aşamalı filtreler eklenirken bir yer olabilir. Örneğin, isteğe bağlı filtreler (örneğin, ülke ve OrdersCount) için birkaç açılan liste içeren bir müşteriler sayfasına sahip olduğunuzu varsayalım. Bu filtreleri bir CompiledQuery 'nin IQueryable sonuçları üzerinden oluşturabilirsiniz, ancak bunu yaptığınızda Yeni sorgunun plan derleyicisine her yürüttüğünüzde, bu filtreler oluşur.

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

 Bu yeniden derlemeyi önlemek için, aşağıdaki olası filtreleri hesaba çekmek üzere CompiledQuery 'yi yeniden yazabilirsiniz:

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

Şu şekilde Kullanıcı arabiriminde çağrılacak:

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

 Burada oluşturulan depo komutu, her zaman null denetimleri olan filtrelere sahip olur, ancak bu, veritabanı sunucusunun en iyileştirmek için oldukça basittir:

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a>3,4 meta verileri önbelleğe alma

Entity Framework meta verileri önbelleğe almayı da destekler. Bu, temel olarak aynı modele farklı bağlantılarda tür bilgilerini ve tür-veritabanı eşleme bilgilerini önbelleğe alma işlemini sağlar. Meta veri önbelleği, AppDomain başına benzersizdir.

#### <a name="341-metadata-caching-algorithm"></a>3.4.1 Metadata önbelleğe alma algoritması

1.  Bir modelin meta veri bilgileri her EntityConnection için bir ItemCollection 'da depolanır.
    -   Yan bir notta, modelin farklı parçaları için farklı ItemCollection nesneleri vardır. Örneğin, Storeıtemcollections, veritabanı modeliyle ilgili bilgileri içerir; ObjectItemCollection, veri modeli hakkında bilgi içerir; EdmItemCollection, kavramsal model hakkında bilgi içerir.

2.  İki bağlantı aynı bağlantı dizesini kullanıyorsa, aynı ItemCollection örneğini paylaşacağız.
3.  İşlevsel eşdeğer ancak metin içeriğini eklemek farklı bağlantı dizeleri farklı meta veri önbelleklerine neden olabilir. Bağlantı dizelerini Simgeleştir, bu nedenle belirteçlerin sırasını değiştirmenin paylaşılan meta verilerle sonuçlanmaları gerekir. Ancak işlevsel olarak görünen iki bağlantı dizesi, simgeleştirme sonrasında özdeş olarak değerlendirilmeyebilir.
4.  ItemCollection düzenli olarak kullanım için denetlenir. Bir çalışma alanına son zamanlarda erişilmediğini tespit ediyorsanız, bir sonraki önbellek tarama işlemi üzerinde Temizleme için işaretlenir.
5.  Yalnızca bir EntityConnection oluşturmak, meta veri önbelleğinin oluşturulmasına neden olur (ancak, içindeki öğe koleksiyonları bağlantı açılıncaya kadar başlatılamaz). Bu çalışma alanı, önbelleğe alma algoritması "kullanımda" olmadığını belirlediğinde bellek içinde kalır.

Müşteri danışma ekibi, "kullanımdan kaldırma" büyük modellerin kullanırken önlemek için bir ItemCollection bir başvuru tutan açıklayan bir blog gönderisi yazmıştır: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a>Meta veri önbelleği ve sorgu planı önbelleğe alma arasındaki ilişkiyi 3.4.2

Sorgu planı önbellek örneği, MetadataWorkspace 'in depo türlerinin ItemCollection 'ı içinde bulunur. Bu, önbelleğe alınan depo komutlarının, belirli bir MetadataWorkspace kullanılarak oluşturulan herhangi bir bağlamda sorgu için kullanılacağı anlamına gelir. Ayrıca, biraz farklı olan iki bağlantı dizeniz varsa ve Simgeleştirici sonrasında eşleşmezse, farklı sorgu planı önbellek örneklerine sahip olursunuz.

### <a name="35-results-caching"></a>3,5 sonuçları önbelleğe alma

Sonuçları önbelleğe alma ("ikinci düzey önbelleğe alma" olarak da bilinir) sayesinde sorguların sonuçlarını yerel bir önbellekte saklayın. Bir sorgu verirken, öncelikle depoya karşı sorgu yapmadan önce sonuçların yerel olarak kullanılabilir olup olmadığını görürsünüz. Sonuçları önbelleğe alma, Entity Framework tarafından doğrudan desteklenirken, sarmalama sağlayıcısı kullanılarak ikinci düzey bir önbellek eklemek mümkündür. İkinci düzey önbellek içeren örnek bir sarmalama sağlayıcısı, [NCache 'e göre Alachisoft 'In Ikinci düzey önbelleği Entity Framework](https://www.alachisoft.com/ncache/entity-framework.html).

İkinci düzey önbelleğe almanın bu uygulama, LINQ ifadesi hesaplandıktan sonra (ve komik) ve sorgu yürütme planı hesaplandıktan veya ilk düzey önbellekten alındıktan sonra gerçekleşen, eklenen bir işlevsellikten oluşur. İkinci düzey önbellek daha sonra yalnızca ham veritabanı sonuçlarını depolar, bu nedenle gerçekleştirmesi işlem hattı daha sonra yine de yürütülür.

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a>sarmalama sağlayıcısıyla önbelleğe alma sonuçları için 3.5.1 ek başvuruları

-   Julie Lerman, örnek sarmalama sağlayıcısını Windows Server AppFabric Önbelleği kullanmak üzere güncelleştirmeyi içeren bir "Entity Framework ve Windows Azure 'da Ikinci düzey önbelleğe alma" MSDN makalesinde yazılmıştır: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)
-   Entity Framework 5 ile çalışıyorsanız, önbelleğe alma sağlayıcısı için Entity Framework 5 ile çalışan gerçekleştirmeyi açıklayan bir gönderi ekibi blogu vardır: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>. Ayrıca, projenize 2. düzey önbellek eklemeyi otomatik hale getirmeye yardımcı olan bir T4 şablonu da içerir.

## <a name="4-autocompiled-queries"></a>4 oto derlenmiş sorgular

Bir sorgu Entity Framework kullanarak bir veritabanına karşı verildiğinde, sonuçların gerçekten çıkarılmadan önce bir dizi adımdan ilerlemesi gerekir; Bu tür bir adım sorgu derlemesi olur. Entity SQL sorguları otomatik olarak önbelleğe alındığından iyi performansa sahip oldukları bilindiğinden, ikinci veya üçüncü kez aynı sorguyu yürüttüğünüzde plan derleyicisini atlayabilir ve bunun yerine önbelleğe alınmış planı kullanabilirsiniz.

Entity Framework 5, LINQ to Entities sorguları için otomatik önbelleğe alma özelliği de sunmuştur. Entity Framework geçmiş sürümlerinde, performansı hızlandırmak için bir CompiledQuery oluşturmak, bu, LINQ to Entities sorgusunun önbelleklenebilir olmasını sağlar. Önbelleğe alma işlemi bir CompiledQuery kullanılmadan otomatik olarak yapıldığından, bu özelliği "otomatik derlenmiş sorgular" olarak çağırırız. Sorgu planı önbelleği ve mekanizması hakkında daha fazla bilgi için bkz. sorgu planı önbelleğe alma.

Entity Framework, bir sorgunun yeniden derlenmesi gereken zaman algılar ve sorgu daha önce derlense bile çağrıldığında olur. Sorgunun yeniden derlenmesine neden olan genel koşullar şunlardır:

-   Sorgunuzla ilişkili MergeOption değiştiriliyor. Önbelleğe alınmış sorgu kullanılmayacak, bunun yerine plan derleyicisi yeniden çalışır ve yeni oluşturulan plan önbelleğe alınır.
-   ContextOptions. UseCSharpNullComparisonBehavior değeri değiştiriliyor. Birleştirme seçeneğini değiştirme ile aynı etkiyi alırsınız.

Diğer koşullar, sorgunuzun önbelleği kullanmasını engelleyebilir. Yaygın örnekler şunlardır:

-   IEnumerable @ no__t-0T @ no__t-1 kullanılıyor. @ No__t-2 @ no__t-3 (T değeri) içerir.
-   Sabitler ile sorgu üreten işlevleri kullanma.
-   Eşlenmeyen bir nesnenin özelliklerini kullanma.
-   Sorgunuzu yeniden derlenmesi gereken başka bir sorguya bağlama.

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a>4,1 IEnumerable @ no__t-0T @ no__t-1 kullanılıyor. Contains @ no__t-2T @ no__t-3 (T değeri)

Entity Framework IEnumerable @ no__t-0T @ no__t-1 ' i çağıran sorguları önbelleğe almaz. Koleksiyonun değerleri geçici olarak kabul edildiği için, bir bellek içi koleksiyonda @ no__t-2T @ no__t-3 (T değeri) içerir. Aşağıdaki örnek sorgu önbelleğe alınmayacak, bu nedenle plan derleyicisi tarafından her zaman işlenir:

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

Içerdiği IEnumerable 'ın, sorgunuzun ne kadar hızlı veya nasıl derlendiğini belirleyen bir boyut olduğunu unutmayın. Yukarıdaki örnekte gösterildiği gibi büyük koleksiyonlar kullanılırken performans önemli ölçüde düşebilir.

Entity Framework 6, IEnumerable @ no__t-0T @ no__t-1 şeklinde iyileştirmeler içerir. Sorgular yürütüldüğünde @ no__t-2T @ no__t-3 (T value) değerini içerir. Oluşturulan SQL kodu daha hızlıdır ve daha fazla okunabilir ve çoğu durumda sunucuda daha hızlı yürütülür.

### <a name="42-using-functions-that-produce-queries-with-constants"></a>sabitler ile sorgu üreten işlevleri kullanarak 4,2

Skip (), take (), Contains () ve Defautıempty () LINQ işleçleri, parametreleri olan SQL sorguları oluşturmaz, ancak bunun yerine, bunlara sabitler olarak geçirilen değerleri yerleştirir. Bu nedenle, başka bir şekilde özdeş olabilecek sorgular, her ikisi de EF Stack ve veritabanı sunucusunda sorgu planı önbelleğini yoklamaya ve aynı sabitler sonraki bir sorgu yürütmesinde kullanılmadıkça yeniden kullanılmamalıdır. Örneğin:

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

Bu örnekte, bu sorgu ID için farklı bir değerle yürütüldüğünde sorgu yeni bir plana derlenir.

Özellikle de disk belleği alırken atlama ve alma işlemleri için dikkat edin. EF6 içinde bu yöntemlerin, bu yöntemlere geçirilen değişkenleri yakalayıp SQLparameters 'a çevirebildiğinden, önbelleğe alınmış sorgu planını etkin bir şekilde yeniden kullanılabilir hale getiren bir lambda aşırı yüklemesi vardır. Bu Ayrıca, atlama ve alma için farklı bir sabite sahip her bir sorgu kendi sorgu planı önbellek girişini alacak şekilde önbelleğin temizleyiciyi tutmaya yardımcı olur.

Aşağıdaki kodu göz önünde bulundurun, ancak bu, yalnızca bu sorgu sınıfını muaf tutmak amacıyla geçerlidir:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Aynı kodun daha hızlı bir sürümü lambda ile Skip yöntemini çağırmayı içerir:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

İkinci kod parçacığı, sorgu her çalıştırıldığında aynı sorgu planı kullanıldığından, bu, CPU süresini kaydeden ve sorgu önbelleğinin kirletmesini önleyen bir kez daha hızlı çalışıyor olabilir. Ayrıca, atlanacak parametre bir kapanış içinde olduğundan, kod şu anda şöyle de görünebilir:

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a>4,3 eşlenmeyen bir nesnenin özelliklerini kullanma

Bir sorgu, eşlenmiş olmayan bir nesne türünün özelliklerini parametre olarak kullandığında sorgu önbelleğe alınmaz. Örneğin:

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

Bu örnekte, yönetilmeyen türdeki sınıfın varlık modelinin parçası olmadığını varsayın. Bu sorgu, eşlenmemiş bir tür kullanmadan kolayca değiştirilebilir ve bunun yerine sorgunun parametresi olarak yerel bir değişken kullanın:

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

Bu durumda, sorgu önbelleğe alınabilecektir ve sorgu planı önbelleğinden faydalanır.

### <a name="44-linking-to-queries-that-require-recompiling"></a>4,4 yeniden derleme gerektiren sorgulara bağlama

Yukarıdaki örnekteki aynı örneği izleyerek, yeniden derlenmesi gereken bir sorguyu temel alan ikinci bir sorgunuz varsa ikinci sorgunuzun tamamı de yeniden derlenir. Bu senaryoyu göstermek için bir örnek aşağıda verilmiştir:

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

Örnek geneldir, ancak firstQuery 'ye bağlanmanın, secondQuery 'nin önbelleğe alınmamasına neden olduğunu gösterir. FirstQuery yeniden derleme gerektiren bir sorgu olsaydı, secondQuery önbelleğe alınır.

## <a name="5-notracking-queries"></a>5 NoTracking sorgusu

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a>5,1 durum yönetimi yükünü azaltmak için değişiklik izlemeyi devre dışı bırakma

Yalnızca bir salt okuma senaryosuyla karşılaşdıysanız ve nesneleri ObjectStateManager 'a yükleme yükünden kaçınmak istiyorsanız, "Izleme yok" sorguları gönderebilirsiniz.  Değişiklik izleme, sorgu düzeyinde devre dışı bırakılabilir.

Değişiklik izlemeyi devre dışı bırakarak nesne önbelleğinin etkin bir şekilde kapatılmasını unutmayın. Bir varlık için sorgulama yaptığınızda, ObjectStateManager 'dan daha önce gerçekleştirilmiş sorgu sonuçlarını çekerek gerçekleştirmesi 'ı atlayamıyoruz. Aynı bağlamda aynı varlıkları tekrar tekrar sorguladıysanız değişiklik izlemeyi etkinleştirmenin bir performans avantajı görebilirsiniz.

ObjectContext kullanılarak sorgulama yaparken, ObjectQuery ve ObjectSet örnekleri, ayarlandıktan sonra bir MergeOption hatırlayacaktır ve üzerinde oluşturulan sorgular üst sorgunun etkin birleştirme seçeneğini miras alır. DbContext kullanılırken izleme, DbSet üzerinde AsNoTracking () değiştiricisi çağırarak devre dışı bırakılabilir.

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a>DbContext kullanılırken bir sorgu için değişiklik izlemeyi devre dışı bırakma 5.1.1

Sorgudaki AsNoTracking () yöntemine yapılan çağrıyı zincirleyerek bir sorgunun modunu NoTracking öğesine geçirebilirsiniz. ObjectQuery 'den farklı olarak, DbContext API 'sindeki DbSet ve DbQuery sınıfları MergeOption için kesilebilir bir özelliğe sahip değildir.

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a>ObjectContext kullanarak sorgu düzeyinde değişiklik izlemeyi devre dışı bırakma 5.1.2

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a>ObjectContext kullanarak tüm varlık kümesi için değişiklik izlemeyi devre dışı bırakma 5.1.3

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a>5,2, NoTracking sorgularının performans avantajını gösteren test ölçümleri

Bu testte, Izlemeyi Navision modeli için NoTracking sorgularıyla karşılaştırarak ObjectStateManager 'ı doldurma maliyetine göz atacağız. Navision modelinin açıklaması ve yürütülen sorgu türleri için ek başlığına bakın. Bu testte sorgular listesinden yineleme yapılır ve her birini bir kez yürütüyoruz. Bir testin iki çeşitlemeyi, NoTracking sorgularıyla ve bir kez "AppendOnly" varsayılan birleştirme seçeneğiyle çalıştırdık. Her bir çeşitleme 3 kez çalıştık ve çalıştırmaların ortalama değerini alır. Testler arasında SQL Server sorgu önbelleğini temizler ve aşağıdaki komutları çalıştırarak tempdb 'yi küçülttik:

1.  DBCC DROPCLEANARABELLEKLERI
2.  DBCC FREEPROCCACHE
3.  DBCC SHRINKDATABASE (tempdb, 0)

Test Sonuçları, 3 üzerinde ortanca çalışma:

|                        | IZLEME YOK – ÇALIŞMA KÜMESI | IZLEME YOK – ZAMAN | YALNIZCA APPEND – ÇALIŞMA KÜMESI | YALNIZCA APPEND – SAAT |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| **Entity Framework 5** | 460361728                 | 1163536 MS         | 596545536                 | 1273042 MS         |
| **Entity Framework 6** | 647127040                 | 190228 MS          | 832798720                 | 195521 MS          |

Entity Framework 5 ' ün sonunda Entity Framework 6 ' dan daha küçük bir bellek parmak izi olacaktır. Entity Framework 6 tarafından tüketilen ek bellek, yeni özellikleri ve daha iyi performans sağlayan ek bellek yapılarının ve kodların sonucudur.

Ayrıca, ObjectStateManager kullanılırken bellek parmak izde net bir farklılık vardır. Entity Framework 5, veritabanından gerçekleştirilmiş olan tüm varlıkların izini tutarken,% 30 ' a kadar olan ayak izini artırmıştır. Entity Framework 6 ' nın parmak izi% 28 ' i arttı.

Zaman içinde, Entity Framework 6 out, bu testte büyük bir kenar boşluğu ile Entity Framework 5 gerçekleştirir. Entity Framework 6, testi yaklaşık% 16 ' da Entity Framework 5 tarafından tüketilen zamanın% 16 ' da tamamladı. Ayrıca, Entity Framework 5, ObjectStateManager kullanılırken tamamlanacak% 9 daha fazla zaman alır. Buna karşılık, Entity Framework 6, ObjectStateManager kullanırken 3 ' ü daha fazla kez kullanıyor.

## <a name="6-query-execution-options"></a>6 sorgu yürütme seçenekleri

Entity Framework, sorgulamak için birkaç farklı yol sunar. Aşağıdaki seçeneklere göz atacağız, her birinin profesyonelleri ve dezavantajlarını karşılaştırıyoruz ve performans özelliklerini inceliyoruz:

-   LINQ to Entities.
-   Izleme LINQ to Entities.
-   Bir ObjectQuery üzerinde Entity SQL.
-   Bir EntityCommand üzerinden Entity SQL.
-   ExecuteStoreQuery.
-   SqlQuery.
-   CompiledQuery.

### <a name="61-linq-to-entities-queries"></a>6,1 LINQ to Entities sorguları

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

**Ları**

-   CUD işlemlerine uygun.
-   Tam gerçekleştirilmiş nesneler.
-   Programlama diline yerleşik sözdizimi ile yazmak en basit.
-   İyi performans.

**Larını**

-   Gibi belirli teknik kısıtlamalar:
    -   Dış BIRLEŞIM sorguları için Defaultıempty kullanan desenler Entity SQL ' deki basit dış BIRLEŞIM deyimlerinden daha karmaşık sorgularla sonuçlanır.
    -   Hala genel kalıp eşleme ile gıbı kullanamazsınız.

### <a name="62-no-tracking-linq-to-entities-queries"></a>6,2 Izleme LINQ to Entities sorgu yok

Bağlam ObjectContext 'e türetiliyor:

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

Bağlam DbContext türetiliyor:

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

**Ları**

-   Normal LINQ sorguları üzerinden geliştirilmiş performans.
-   Tam gerçekleştirilmiş nesneler.
-   Programlama diline yerleşik sözdizimi ile yazmak en basit.

**Larını**

-   CUD işlemleri için uygun değildir.
-   Gibi belirli teknik kısıtlamalar:
    -   Dış BIRLEŞIM sorguları için Defaultıempty kullanan desenler Entity SQL ' deki basit dış BIRLEŞIM deyimlerinden daha karmaşık sorgularla sonuçlanır.
    -   Hala genel kalıp eşleme ile gıbı kullanamazsınız.

NoTracking belirtilmemiş olsa bile, proje skaler özelliklerinin izlenmediğini unutmayın. Örneğin:

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

Bu belirli sorgu açıkça NoTracking olarak belirtilmiyor, ancak nesne durumu Yöneticisi tarafından bilinen bir tür olmadığından gerçekleştirilmiş sonuç izlenmiyor.

### <a name="63-entity-sql-over-an-objectquery"></a>ObjectQuery üzerinde 6,3 Entity SQL

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

**Ları**

-   CUD işlemlerine uygun.
-   Tam gerçekleştirilmiş nesneler.
-   Sorgu planı önbelleğe almayı destekler.

**Larını**

-   Dile yerleştirilmiş sorgu yapılarından daha fazla kullanıcı hatası ile daha fazla olan metinsel Sorgu dizelerini içerir.

### <a name="64-entity-sql-over-an-entity-command"></a>bir varlık komutu üzerinden 6,4 Entity SQL

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

**Ları**

-   .NET 4,0 ' de sorgu planı önbelleğe almayı destekler (plan önbelleği, .NET 4,5 ' deki diğer tüm sorgu türleri tarafından desteklenir).

**Larını**

-   Dile yerleştirilmiş sorgu yapılarından daha fazla kullanıcı hatası ile daha fazla olan metinsel Sorgu dizelerini içerir.
-   CUD işlemleri için uygun değildir.
-   Sonuçlar otomatik olarak gerçekleştirilmez ve veri okuyucudan okunmalıdır.

### <a name="65-sqlquery-and-executestorequery"></a>6,5 SqlQuery ve ExecuteStoreQuery

Veritabanında SqlQuery:

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

DbSet üzerinde SqlQuery:

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

**Ları**

-   Plan derleyicisi atlandığından genellikle en hızlı performans.
-   Tam gerçekleştirilmiş nesneler.
-   DbSet 'ten kullanıldığında CUD işlemlerine uygun.

**Larını**

-   Sorgu metinsel ve hataya açıktır.
-   Sorgu, kavramsal semantik yerine depo semantiğini kullanarak belirli bir arka uca bağlanır.
-   Devralma mevcut olduğunda, el ile oluşturulmuş sorgunun istenen tür için eşleme koşullarını hesaba eklemesi gerekir.

### <a name="66-compiledquery"></a>6,6 CompiledQuery

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

**Ları**

-   Normal LINQ sorguları üzerinde en fazla% 7 performans geliştirmesini sağlar.
-   Tam gerçekleştirilmiş nesneler.
-   CUD işlemlerine uygun.

**Larını**

-   Artan karmaşıklık ve programlama yükü.
-   Derlenmiş bir sorgunun üzerine oluştururken performans iyileştirmesi kaybolur.
-   Bazı LINQ sorguları bir CompiledQuery olarak yazılamaz; Örneğin, anonim türlerin tahminleri.

### <a name="67-performance-comparison-of-different-query-options"></a>6,7 farklı sorgu seçenekleri performans karşılaştırması

Bağlam oluşturma işlemi zaman aşımına uğramayan basit mikro kıyaslamalar. Denetlenen bir ortamda, önbelleğe alınmamış varlıkların bir kümesi için 5000 kez sorgu ölçüleceğini ölçüyoruz. Bu numaralar bir uyarı ile birlikte alınırlar: bir uygulama tarafından üretilen gerçek sayıları yansıtmaz, ancak bunun yerine farklı sorgulama seçenekleri karşılaştırıldığında performans farkının ne kadarının olacağını çok doğru bir ölçüdür Yeni bağlam oluşturma maliyeti hariç, elmalar ve elmalar.

| AŞV  | Test etme                                 | Zaman (MS) | Bellek   |
|:----|:-------------------------------------|:----------|:---------|
| EF5 | ObjectContext ESQL                   | 2414      | 38801408 |
| EF5 | ObjectContext LINQ sorgusu             | 2692      | 38277120 |
| EF5 | DbContext LINQ sorgusu Izleme yok     | 2818      | 41840640 |
| EF5 | DbContext LINQ sorgusu                 | 2930      | 41771008 |
| EF5 | ObjectContext LINQ sorgusu Izleme yok | 3013      | 38412288 |
|     |                                      |           |          |
| EF6 | ObjectContext ESQL                   | 2059      | 46039040 |
| EF6 | ObjectContext LINQ sorgusu             | 3074      | 45248512 |
| EF6 | DbContext LINQ sorgusu Izleme yok     | 3125      | 47575040 |
| EF6 | DbContext LINQ sorgusu                 | 3420      | 47652864 |
| EF6 | ObjectContext LINQ sorgusu Izleme yok | 3593      | 45260800 |

![EF5 mikro kıyaslamalar, 5000 sıcak yineleme](~/ef6/media/ef5micro5000warm.png)

![EF6 mikro kıyaslamalar, 5000 sıcak yineleme](~/ef6/media/ef6micro5000warm.png)

Mikro kıyaslamalar, koddaki küçük değişikliklere çok duyarlıdır. Bu durumda, Entity Framework 5 ve Entity Framework 6 ' nın maliyetleri arasındaki fark, [ele](~/ef6/fundamentals/logging-and-interception.md) alma ve [işlem geliştirmelerinden](~/ef6/saving/transactions.md)kaynaklanır. Bununla birlikte, bu mikro kıyaslamalar sayıları, Entity Framework ne yaptığını çok küçük parçalara ayırır. Entity Framework 5 ' ten Entity Framework 6 ' ya yükseltirken, normal sorguların gerçek zamanlı senaryolarında bir performans gerileme görmemelidir.

Farklı sorgu seçeneklerinin gerçek dünya performansını karşılaştırmak için, kategori adı "Içecek" olan tüm ürünleri seçmek üzere farklı bir sorgu seçeneği kullandığımız 5 ayrı test varyasyonunu oluşturduk. Her yineleme, bağlam oluşturma maliyetinin yanı sıra döndürülen tüm varlıkların nasıl bir ücret almakta olduğunu içerir. 10 yineleme, 1000 süreli yinelemelerin toplamı alınmadan önce zaman aşımına uğramadan çalıştırılır. Gösterilen sonuçlar, her testin 5 çalıştırmasından alınan ortanca çalışmadır. Daha fazla bilgi için bkz. Ek B, test kodunu içerir.

| AŞV  | Test etme                                        | Zaman (MS) | Bellek   |
|:----|:--------------------------------------------|:----------|:---------|
| EF5 | ObjectContext varlık komutu                | 621       | 39350272 |
| EF5 | Veritabanı üzerinde DbContext SQL sorgusu             | 825       | 37519360 |
| EF5 | ObjectContext mağaza sorgusu                   | 878       | 39460864 |
| EF5 | ObjectContext LINQ sorgusu Izleme yok        | 969       | 38293504 |
| EF5 | Nesne sorgusu kullanarak ObjectContext varlık SQL | 1089      | 38981632 |
| EF5 | ObjectContext derlenmiş sorgu                | 1099      | 38682624 |
| EF5 | ObjectContext LINQ sorgusu                    | 1152      | 38178816 |
| EF5 | DbContext LINQ sorgusu Izleme yok            | 1208      | 41803776 |
| EF5 | DbSet üzerinde DbContext SQL sorgusu                | 1414      | 37982208 |
| EF5 | DbContext LINQ sorgusu                        | 1574      | 41738240 |
|     |                                             |           |          |
| EF6 | ObjectContext varlık komutu                | 480       | 47247360 |
| EF6 | ObjectContext mağaza sorgusu                   | 493       | 46739456 |
| EF6 | Veritabanı üzerinde DbContext SQL sorgusu             | 614       | 41607168 |
| EF6 | ObjectContext LINQ sorgusu Izleme yok        | 684       | 46333952 |
| EF6 | Nesne sorgusu kullanarak ObjectContext varlık SQL | 767       | 48865280 |
| EF6 | ObjectContext derlenmiş sorgu                | 788       | 48467968 |
| EF6 | DbContext LINQ sorgusu Izleme yok            | 878       | 47554560 |
| EF6 | ObjectContext LINQ sorgusu                    | 953       | 47632384 |
| EF6 | DbSet üzerinde DbContext SQL sorgusu                | 1023      | 41992192 |
| EF6 | DbContext LINQ sorgusu                        | 1290      | 47529984 |


![EF5 sıcak sorgu 1000 yinelemeleri](~/ef6/media/ef5warmquery1000.png)

![EF6 sıcak sorgu 1000 yinelemeleri](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> Bu, bir EntityCommand üzerinde Entity SQL bir sorgu yürütüyoruz bir varyasyon ekledik. Ancak, bu tür sorgular için sonuçlar gerçekleştirilmediğinden, karşılaştırma, her zaman, her ne kadar elmalar olması gerekmez. Test, karşılaştırma fairer yapmayı denemek için bir kapanış tahmini içerir.

Bu uçtan uca bu durumda Entity Framework 6 out, çok daha hafif bir DbContext başlatması ve daha hızlı MetadataCollection @ no__t-0T @ no__t-1 aramaları dahil olmak üzere yığının çeşitli bölümlerinde yapılan performans geliştirmelerinden dolayı 5 Entity Framework.

## <a name="7-design-time-performance-considerations"></a>7 tasarım zamanı performans konuları

### <a name="71-inheritance-strategies"></a>7,1 devralma stratejileri

Entity Framework kullanırken başka bir performans değerlendirmesi kullandığınız devralma stratejisidir. Entity Framework, 3 temel devralma türünü ve bunların birleşimlerini destekler:

-   Hiyerarşi başına tablo (TPH) – her devralma kümesi, hiyerarşide hangi tür türün temsil edileceğini belirtmek için bir Ayrıştırıcı sütunu olan bir tabloyla eşlenir.
-   Tür başına tablo (TPT) – her türün veritabanında kendi tablosu vardır; alt tablolar yalnızca üst tablonun içermediği sütunları tanımlar.
-   Sınıf başına tablo (TPC) – her türün veritabanında kendine ait tam tablosu vardır; alt tablolar, üst türlerde tanımlananlar da dahil olmak üzere tüm alanlarını tanımlar.

Modeliniz TPT devralmayı kullanıyorsa, oluşturulan sorgular diğer devralma stratejileriyle oluşturulanlardan daha karmaşık olacaktır, bu da depodaki yürütme sürelerinin uzun süreleriyle sonuçlanabilir.  Genellikle bir TPT modeli üzerinde sorgu oluşturmak ve sonuçta elde edilen nesneleri faturalandırmak daha uzun sürer.

"TPT (tablo başına tür) devralma varlık Çerçevesi'nde kullanırken performans konuları" Bkz MSDN blog gönderisi: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.

#### <a name="711-avoiding-tpt-in-model-first-or-code-first-applications"></a>Model First veya Code First uygulamalarında TPT 7.1.1 önleme

Bir TPT şemasına sahip var olan bir veritabanı üzerinde bir model oluşturduğunuzda çok sayıda seçeneğiniz yoktur. Ancak Model First veya Code First kullanarak bir uygulama oluştururken performans sorunları için TPT devralmasından kaçının.

Entity Desisgner sihirbazında Model First kullandığınızda, modelinizde devralma için TPT alırsınız. TPH devralma strateji ile ilk Model geçmek istiyorsanız, Visual Studio Gallery'den "varlık Tasarımcısı veritabanı oluşturma Power paketi" kullanıma kullanabilirsiniz ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).

Bir modelin devralma ile eşlemesini yapılandırmak için Code First kullanırken EF, varsayılan olarak TPH kullanır, bu nedenle devralma hiyerarşisindeki tüm varlıklar aynı tabloyla eşleştirilir. MSDN magazine'de "Kod ilk olarak varlığın Framework4.1" makale "Eşleme ile Fluent API'si" bölümüne bakın ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) daha fazla ayrıntı için.

### <a name="72-upgrading-from-ef4-to-improve-model-generation-time"></a>7,2 model oluşturma süresini geliştirmek için EF4 'ten yükseltme

Modelin mağaza katmanını (SSDL) oluşturan algoritmaya yönelik SQL Server özgü bir geliştirme Entity Framework 5 ve 6 ' da ve Visual Studio 2010 SP1 yüklendiğinde Entity Framework 4 ' e bir güncelleştirme olarak sunulmaktadır. Aşağıdaki test sonuçları, çok büyük bir model oluştururken, bu örnekte Navision modelinde geliştirme gösterilmektedir. Daha fazla ayrıntı için bkz. Ek C.

Model, 1005 varlık kümesi ve 4227 ilişkilendirme kümesi içerir.

| Yapılandırma                              | Tüketilen sürenin dökümü                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Visual Studio 2010, Entity Framework 4     | SSDL oluşturma: 2 saat 27 dk <br/> Eşleme oluşturma: 1 saniye <br/> CSDL oluşturma: 1 saniye <br/> ObjectLayer oluşturma: 1 saniye <br/> Üretimi görüntüle: 2 h 14 dk |
| Visual Studio 2010 SP1, Entity Framework 4 | SSDL oluşturma: 1 saniye <br/> Eşleme oluşturma: 1 saniye <br/> CSDL oluşturma: 1 saniye <br/> ObjectLayer oluşturma: 1 saniye <br/> Üretimi görüntüle: 1 hr 53 dk   |
| Visual Studio 2013, Entity Framework 5     | SSDL oluşturma: 1 saniye <br/> Eşleme oluşturma: 1 saniye <br/> CSDL oluşturma: 1 saniye <br/> ObjectLayer oluşturma: 1 saniye <br/> Üretimi görüntüle: 65 dakika    |
| Visual Studio 2013, Entity Framework 6     | SSDL oluşturma: 1 saniye <br/> Eşleme oluşturma: 1 saniye <br/> CSDL oluşturma: 1 saniye <br/> ObjectLayer oluşturma: 1 saniye <br/> Üretimi görüntüle: 28 saniye.   |


SSDL 'ı oluştururken yükün neredeyse tamamen SQL Server, istemci geliştirme makinesi sonuçların sunucudan geri gelmesi için boşta beklediği sırada yükün neredeyse tamamen harcandığını belirten bir değer. DBAs özellikle bu geliştirmeyi bilmelidir. Ayrıca, tüm model oluşturma maliyetinin artık görünüm oluşturma bölümünde meydana gelir.

### <a name="73-splitting-large-models-with-database-first-and-model-first"></a>7,3 Database First ve Model First büyük modelleri bölme

Model boyutu arttıkça tasarımcı yüzeyi de karışık hale gelir ve kullanılması zordur. Genellikle, tasarımcı 'nın etkin bir şekilde kullanılması için 300 ' den fazla varlık içeren bir model düşünün. Büyük modellerin bölmek için çeşitli seçenekler şu blog gönderisinde açıklanmaktadır: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.

Gönderi Entity Framework ilk sürümü için yazıldı, ancak adımlar hala geçerlidir.

### <a name="74-performance-considerations-with-the-entity-data-source-control"></a>Varlık veri kaynağı denetimiyle ilgili 7,4 performans konuları

EntityDataSource denetimini kullanan bir Web uygulamasının performansının önemli ölçüde azalyacağı, çok iş parçacıklı performans ve stres testlerinde bu durumları gördük. Temeldeki neden, EntityDataSource 'un varlık olarak kullanılacak türleri bulması için Web uygulaması tarafından başvurulan derlemelerde MetadataWorkspace. LoadFromAssembly ' i tekrar tekrar çağırıyor olması olabilir.

Çözüm, EntityDataSource 'un ContextTypeName öğesini türetilmiş ObjectContext sınıfınızın tür adıyla ayarlamaya yönelik olur. Bu, varlık türleri için başvurulan tüm derlemeleri tarayan mekanizmayı kapatır.

ContextTypeName alanının ayarlanması ayrıca, .NET 4,0 ' deki EntityDataSource 'un yansıma aracılığıyla bir derlemeden bir tür yükleyebildiği bir ReflectionTypeLoadException oluşturduğu işlevsel bir sorunu önler. Bu sorun .NET 4,5 ' de düzeltilmiştir.

### <a name="75-poco-entities-and-change-tracking-proxies"></a>7,5 POCO varlıkları ve değişiklik izleme proxy 'leri

Entity Framework, veri sınıflarında herhangi bir değişiklik yapmadan veri modelinizle birlikte özel veri sınıflarını kullanmanıza olanak sağlar. Bu, mevcut etki alanı nesneleri gibi "düz eski" CLR nesnelerini (POCO) veri modelinizle kullanabileceğiniz anlamına gelir. Bu POCO veri sınıfları (Kalıcılık-Ignorant nesneleri olarak da bilinir), aynı sorgunun çoğunu destekler, Varlık Veri Modeli araçları tarafından oluşturulan varlık türleri olarak davranışları ekleyin, güncelleştirin ve silin.

Entity Framework, Poco varlıklarınızda elde edilen yavaş yükleme ve otomatik değişiklik izleme gibi özellikleri etkinleştirmek istediğinizde kullanılan, POCO türlerinizi türetilmiş proxy sınıfları da oluşturabilir. POCO sınıflarınızı proxy'ler kullanmanız Entity Framework, burada açıklandığı şekilde izin vermek için belirli gereksinimleri karşılamalıdır: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).

Varlık izleme proxy 'leri, varlıklarınızın özelliklerinden herhangi birinin değeri değiştiği her seferinde nesne durumu yöneticisini bilgilendirir, bu nedenle varlıklarınızın gerçek durumunu her zaman bilir Entity Framework. Bu işlem, özelliklerinizi ayarlarınızın ayarlayıcı yöntemlerinin gövdesine ekleyerek ve nesne durumu yöneticisinin bu gibi olayları işlemesini sağlamak için yapılır. Bir proxy varlık oluşturmanın genellikle Entity Framework tarafından oluşturulan eklenen olay kümesi nedeniyle proxy olmayan bir POCO varlığı oluşturmaktan daha pahalı olacağını unutmayın.

Bir POCO varlığının değişiklik izleme proxy 'si olmadığında, varlıklarınızın içerikleri önceki kaydedilmiş bir kopyanın bir kopyasına göre karşılaştırılırken değişiklikler bulunur. Bu derin karşılaştırma, ortamınızda çok fazla varlık olduğunda veya varlıklarınızda son karşılaştırma gerçekleşmesinden bu yana değiştirilmese bile çok büyük miktarda özelliğe sahip olduğunda uzun bir işlem olur.

Özet: değişiklik izleme proxy 'si oluştururken bir performans isabeti ödetireceksiniz, ancak değişiklik izleme, varlıklarınızda birçok özellik olduğunda veya modelinizde birçok varlık olduğunda değişiklik algılama sürecini hızlandırmanıza yardımcı olur. Varlık miktarının çok fazla büyümeyeceği az sayıda özelliği olan varlıklar için değişiklik izleme proxy 'lerinin olması çok avantajlı olabilir.

## <a name="8-loading-related-entities"></a>8 Ilgili varlıkları yükleme

### <a name="81-lazy-loading-vs-eager-loading"></a>8,1 yavaş yükleme ile Ekip yükleme

Entity Framework, hedef varlığınıza ilişkin varlıkları yüklemek için birkaç farklı yol sunar. Örneğin, ürünler için sorgulama yaptığınızda ilgili siparişlerin nesne durumu yöneticisine yüklenebilmenin farklı yolları vardır. Bir performans açısından, ilgili varlıkların yüklenmesi sırasında göz önünde bulundurmanız gereken en büyük soru, geç yükleme veya Eager yükleme kullanılıp kullanılmayacağını belirtir.

Eager yüklemesi kullanılırken, ilgili varlıklar hedef varlık kümesiyle birlikte yüklenir. Hangi ilgili varlıkların getirmek istediğinizi belirtmek için sorgunuzda bir Include ifadesini kullanın.

Geç yükleme kullanılırken, ilk sorgunuz yalnızca hedef varlık kümesine getirir. Ancak bir gezinti özelliğine her eriştiğinizde, ilgili varlığı yüklemek için mağazaya karşı başka bir sorgu verilir.

Bir varlık yüklendikten sonra, yavaş yükleme veya Eager yükleme kullandığınızda varlık için başka sorgular doğrudan nesne durumu yöneticisinden yüklenir.

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a>8,2 yavaş yükleme ve Eager yükleme arasında seçim yapma

Önemli olan şey, uygulamanız için doğru seçimi yapabilmeniz için yavaş yükleme ve Eager yükleme arasındaki farkı anlamanızdır. Bu, veritabanına yönelik birden çok istek ile büyük bir yük içerebilen tek bir istek arasındaki zorunluluğunu getirir değerlendirmenize yardımcı olur. Uygulamanızın bazı bölümlerinde ve başka bölümlere geç yüklemeye yönelik olarak yükleme kullanmak uygun olabilir.

Devlet kapsamında neler olduğuna ilişkin bir örnek olarak, UK 'de yaşayan müşterileri ve bunların sıra sayısını sorgulamak istediğinizi varsayalım.

**Eager yükleme kullanma**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

**Geç yükleme kullanma**

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

Eager yükleme kullanılırken, tüm müşterileri ve tüm siparişleri döndüren tek bir sorgu oluşturacaksınız. Mağaza komutu şöyle görünür:

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

Yavaş yükleme kullanırken başlangıçta aşağıdaki sorguyu verirsiniz:

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

Aynı zamanda bir müşterinin Orders gezinti özelliğine her eriştiğinizde, mağazaya karşı aşağıdakiler gibi başka bir sorgu da verilmiştir:

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

Daha fazla bilgi için [Ilgili nesneleri yükleme](https://msdn.microsoft.com/library/bb896272.aspx)bölümüne bakın.

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a>8.2.1 geç yükleme, bir ekip yükleme sayfası

Bu tür bir şey, bir veya daha fazla uygulamanın, yavaş yüklemeye karşı karşıya yüklemeyi tercih eden bir şeydir. İyi bilinçli bir karar vermek için, her iki strateji arasındaki farkları anlamak için deneyin; Ayrıca, kodunuzun aşağıdaki senaryolardan birine uygun olup olmadığını göz önünde bulundurun:

| Senaryo                                                                    | Önerimiz                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Getirilen varlıklardan birçok gezinti özelliklerine erişmeniz gerekiyor mu? | **Hayır** -her iki seçenek de büyük olasılıkla yapılır. Ancak, sorgunuzun yaptığı yük çok büyük değilse, nesnelerinizi bir araya getirmek için daha az ağ gidiş dönüşlerinin gerekli olduğu için, yükleme seçeneğini kullanarak performans avantajları yaşayabilirsiniz. <br/> <br/> **Evet** -varlıklardan çok sayıda gezinti özelliklerine erişmeniz gerekiyorsa, bunu sorgularınızda bulunan ve Eager yüklemesi ile birden çok Include deyimi kullanarak yapmanız gerekir. Ne kadar çok varlık varsa, sorgunuzun döndürdüğü yükün daha büyük olması gerekir. Sorgunuza üç veya daha fazla varlık eklediğinizde, geç yüklemeye geçmeyi düşünün. |
| Çalışma zamanında tam olarak hangi verilerin gerekli olacağını tanıyor musunuz?                   | **Hayır** -geç yükleme sizin için daha iyi olacaktır. Aksi takdirde, ihtiyaç duymayacak verilerin sorgulanmasını bitimeyebilirsiniz. <br/> <br/> **Evet** -Eager yüklemesi büyük olasılıkla en iyi sonuç; tüm kümeleri daha hızlı yüklemeye yardımcı olur. Sorgunuz çok büyük miktarda veri getirmeyi gerektiriyorsa ve bu çok yavaş hale gelirse bunun yerine yavaş yükleme yapmayı deneyin.                                                                                                                                                                                                                                                       |
| Kodunuz veritabanınızdan mi çalışıyor? (artan ağ gecikmesi)  | **Hayır** -ağ gecikmesi bir sorun değilse, yavaş yükleme kullanmak kodunuzu basitleştirebilir. Uygulamanızın topolojisinin değişeceğini unutmayın, bu nedenle verilen için veritabanı yakınlığını kaçırmayın. <br/> <br/> **Evet** -ağ bir sorun olduğunda, senaryonuza ne kadar uygun olduğuna karar verebilirsiniz. Genellikle, daha az gidiş dönüş gerektirdiğinden Eager yüklemesi daha iyi olacaktır.                                                                                                                                                                                                      |


#### <a name="822-performance-concerns-with-multiple-includes"></a>birden çok Içerme ile 8.2.2 performans sorunları

Sunucu yanıt süresi sorunlarını içeren performans sorularını duyduğumuz zaman, bu sorunun kaynağı birden çok Include deyimi ile sık kullanılan sorgulardır. Bir sorgudaki ilgili varlıkları da dahil ederken, kapakların altında neler olduğunu anlamak önemlidir.

İçinde birden fazla ekleme deyimi olan bir sorgu için, depo komutunu oluşturmak üzere iç plan derleyicimize gidebileceği görece uzun zaman alır. Bu sürenin büyük bölümü, elde edilen sorguyu iyileştirmeye çalışırken harcanacak. Oluşturulan depo komutu, eşlemelerinize bağlı olarak her bir ekleme için bir dış birleşim veya birleşim içerecektir. Bunun gibi sorgular, özellikle de yük içinde çok fazla artıklık olduğunda (örneğin, çapraz geçiş için çok sayıda artım olduğunda) çok sayıda bant genişliği sorununu Acer, tek bir yükte veritabanınızdaki büyük bağlantılı grafikler sağlar. bire çok yönünde ilişkilendirmeler).

Sorgu için ToTraceString kullanarak ve yük boyutunu görmek üzere SQL Server Management Studio ' de Store komutunu yürüterek, sorguların çok büyük yükleri döndüren durumları kontrol edebilirsiniz. Bu gibi durumlarda, yalnızca ihtiyaç duyduğunuz verileri getirmek için sorguunuzdaki ekleme deyimlerinin sayısını azaltmayı deneyebilirsiniz. Ya da sorgunuzu daha küçük bir alt sorgu dizisine bölebilir, örneğin:

**Sorguyu bozmadan önce:**

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

**Sorguyu kopardıktan sonra:**

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

Bu, içeriğin kimlik çözümlemesi ve ilişkilendirme düzeltmesini otomatik olarak gerçekleştirme yeteneği kullandığımızda yalnızca izlenen sorgularda çalışır.

Lazy yüklemesinde olduğu gibi, zorunluluğunu getirir daha küçük yük için daha fazla sorgu olacaktır. Ayrıca, her bir varlıktan yalnızca ihtiyaç duyduğunuz verileri açıkça seçmek için tek tek özelliklerin projeksiyonlarını da kullanabilirsiniz, ancak bu durumda varlıkları yüklemeyin ve güncelleştirmeler desteklenmez.

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a>özelliklerin geç yüklenmesini sağlamak için 8.2.3 geçici çözüm

Entity Framework şu anda skaler veya karmaşık özelliklerin geç yüklemesini desteklemez. Ancak, BLOB gibi büyük bir nesne içeren bir tablonuz olduğu durumlarda, büyük özellikleri ayrı bir varlığa ayırmak için tablo bölmeyi kullanabilirsiniz. Örneğin, değişken fotoğraf sütunu içeren bir ürün tablonuz olduğunu varsayalım. Sorgularda bu özelliğe sık sık erişmeniz gerekmiyorsa, yalnızca normal olarak ihtiyaç duyduğunuz varlığın parçalarını getirmek için tablo bölmeyi kullanabilirsiniz. Ürün fotoğrafı temsil eden varlık yalnızca açıkça ihtiyacınız olduğunda yüklenecektir.

Gil Fink'ın "Tablo bölme, Entity Framework" blog gönderisine tablo bölme etkinleştirme gösteren makaleden faydalanabilirsiniz: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.

## <a name="9-other-considerations"></a>9 diğer konular

### <a name="91-server-garbage-collection"></a>9,1 sunucu çöp toplama

Bazı kullanıcılar, Atık toplayıcısının düzgün yapılandırılmadığı durumlarda beklendikleri paralelliği sınırlayan kaynak çekişmesine karşılaşabilir. Çok iş parçacıklı bir senaryoda veya sunucu tarafı sistemine benzeyen herhangi bir uygulamada EF kullanıldığında, sunucu çöp toplamayı etkinleştirdiğinizden emin olun. Bu, uygulama yapılandırma dosyanızdaki basit bir ayar aracılığıyla yapılır:

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

Bu, iş parçacığı çekişmesini azaltmalı ve CPU doygun senaryolarda% 30 ' a varan aktarım hızını artırmalıdır. Genel koşullarda, her zaman uygulamanızın klasik çöp toplamayı (UI ve istemci tarafı senaryoları için daha iyi ayarlanmış) ve sunucu çöp toplama işlemini kullanarak nasıl davranacağını test etmelisiniz.

### <a name="92-autodetectchanges"></a>9,2 Oto DetectChanges

Daha önce belirtildiği gibi, nesne önbelleğinde çok sayıda varlık olduğunda Entity Framework performans sorunlarını gösterebilir. Add, Remove, Find, Entry ve SaveChanges gibi bazı işlemler, nesne önbelleğinin ne kadar büyük bir CPU tüketilebileceğini anlamak için, DetectChanges 'a çağrıları tetikler. Bunun nedeni, nesne önbelleğinin ve nesne durumu yöneticisinin, bir bağlam için gerçekleştirilen her işlem için mümkün olduğunca eşitlenmiş olarak kalmaya çalışacağı, üretilen verilerin çok sayıda senaryo kapsamında doğru olması garanti edilir.

Uygulamanızın tüm ömrü için Entity Framework otomatik değişiklik algılamayı etkin bırakmak genellikle iyi bir uygulamadır. Senaryonuz yüksek CPU kullanımından olumsuz bir şekilde etkileniyorsa ve profilleriniz, SSIS 'nin DetectChanges çağrısı olduğunu gösteriyorsa, kodunuzun hassas bölümünde, geçici olarak yeniden gözden geçirin:

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

Oto DetectChanges 'ı kapatmadan önce, bunun Entity Framework varlıklarda gerçekleşen değişikliklerle ilgili belirli bilgileri takip etme yeteneğini kaybetmesine neden olabileceğini anlamanız yararlı olur. Yanlış işlenirse, bu durum uygulamanızda veri tutarsızlığına neden olabilir. AutoDetectChanges kapatarak daha fazla bilgi için okuma \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.

### <a name="93-context-per-request"></a>istek başına 9,3 bağlam

Entity Framework bağlamlarının en iyi performans deneyimini sağlamak için kısa süreli örnekler olarak kullanılması amaçlanmıştır. Bağlamların kısa süreli ve atılacak olması beklenir. bu nedenle, mümkün olduğunca çok hafif ve yeniden kullanmaya yönelik olarak uygulanırlar. Web senaryolarında, tek bir isteğin süresinden daha uzun bir içeriğe sahip olmak için bunu göz önünde bulundurmanız önemlidir. Benzer şekilde, Web 'de olmayan senaryolarda bağlam, Entity Framework farklı önbelleğe alma seviyelerinin anlaşılmasına göre atılır. Genellikle, biri uygulamanın ömrü boyunca bağlam örneğine sahip olmanın yanı sıra iş parçacığı ve statik bağlamlara göre bağlamlar yapmaktan kaçınmalıdır.

### <a name="94-database-null-semantics"></a>9,4 veritabanı null semantiği

Varsayılan olarak Entity Framework, C @ no__t-0 null karşılaştırma semantiğinin bulunduğu SQL kodu oluşturur. Aşağıdaki örnek sorguyu göz önünde bulundurun:

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

Bu örnekte, bir dizi null yapılabilir değişkeni, varlığındaki null yapılabilir özelliklerle (SupplierID ve BirimFiyat gibi) karşılaştırıyoruz. Bu sorgu için oluşturulan SQL, parametre değerinin sütun değeriyle aynı olup olmadığını veya hem parametre hem de sütun değerlerinin null olduğunu sorar. Bu, veritabanı sunucusunun null değerleri işleme şeklini gizler ve farklı veritabanı satıcıları arasında tutarlı bir C @ no__t-0 boş deneyim sağlar. Diğer taraftan, oluşturulan kod bir bit evdir ve sorgunun where deyimindeki karşılaştırma miktarı büyük bir sayıya büyüdükçe iyi şekilde gerçekleştirilemeyebilir.

Bu durumla başa çıkmak için bir yol veritabanı null semantiğini kullanmaktır. Bu, veritabanı altyapısının null değerleri işleme biçimini sunan daha basit bir SQL üretecağından, bu durum büyük olasılıkla C @ no__t-0 null semanEntity Framework tiğinin farklı davranabileceğini unutmayın. Veritabanı null semantiği bağlam yapılandırmasına karşı tek bir yapılandırma satırıyla bağlam başına etkinleştirilebilir:

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

Küçük ve orta ölçekli sorgularda, veritabanı null semantiği kullanılırken bir algılanabilir performans geliştirmesi gösterilmez, ancak fark çok sayıda olası null karşılaştırmaları olan sorgularda daha belirgin olur.

Yukarıdaki örnek sorguda performans farkı, denetlenen bir ortamda çalışan bir mikro kıyaslama için% 2 ' den az idi.

### <a name="95-async"></a>9,5 zaman uyumsuz

Entity Framework 6, .NET 4,5 veya üzeri sürümlerde çalışırken zaman uyumsuz işlemler desteği getirmiştir. Çoğu bölümde, GÇ ile ilgili çekişmeye sahip uygulamalar, zaman uyumsuz sorgu ve kaydetme işlemlerini kullanmanın en iyi avantajını kullanacaktır. Uygulamanız GÇ çekişme ile karşılaşmamışsa, zaman uyumsuz kullanılması en iyi şekilde çalışır ve sonucu zaman uyumlu bir çağrı ile aynı süre içinde döndürür ya da en kötü durumda yürütmeyi zaman uyumsuz bir göreve erteleyin ve ek Tim ekleyin Senaryonuzun tamamlanmasını sağlar.

Zaman uyumsuz bir uygulamanızın performansını artıracak karar yardımcı olacak ne zaman uyumsuz programlama iş ziyaret bilgi [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx). Entity Framework zaman uyumsuz işlemlerin kullanımı hakkında daha fazla bilgi için bkz. [Async Query ve Save @ no__t-1.

### <a name="96-ngen"></a>9,6 NGEN

Entity Framework 6, .NET Framework 'ün varsayılan yüklemesinde gelmez. Bu nedenle, Entity Framework derlemeleri varsayılan olarak NGEN değildir ve bu da tüm Entity Framework kodlarının diğer MSIL bütünleştirilmiş kodları ile aynı maliyet maliyetlerine tabidir. Bu, uygulamanızı geliştirme sırasında ve ayrıca üretim ortamlarında uygulamanızın soğuk bir şekilde başlatılmasını sağlarken F5 deneyimini düşürebilir. CPU ve bellek maliyetlerini azaltmak için, uygun şekilde Entity Framework görüntülerini NGEN önerilir. Entity Framework 6 ' nın başlangıç performansını NGEN ile geliştirme hakkında daha fazla bilgi için bkz. [Ngen Ile başlangıç performansını artırma](~/ef6/fundamentals/performance/ngen.md).

### <a name="97-code-first-versus-edmx"></a>9,7 Code First ve EDMX

Kavramsal modelin bellek içi gösterimine (nesneler), depolama şemasına (veritabanı) ve arasında bir eşlemeye sahip olan nesne odaklı programlama ve ilişkisel veritabanları arasındaki empedance uyuşmazlığı sorunuyla ilgili nedenler Entity Framework. ikiye. Bu meta veriler Varlık Veri Modeli veya kısa için EDM olarak adlandırılır. Bu EDM Entity Framework, verileri bellekteki nesnelerden veritabanına ve geri dönüş için görünümleri türetecektir.

Entity Framework kavramsal modeli, depolama şemasını ve eşlemeyi belirten bir EDMX dosyası ile kullanıldığında, model yükleme aşamasının yalnızca EDM 'nin doğru olduğunu doğrulaması gerekir (örneğin, hiçbir eşleme olmadığından emin olun), ardından görünümleri oluşturun, ardından görünümleri doğrulayın ve bu meta verilerin kullanıma hazırolmasını sağlayabilirsiniz. Yalnızca bir sorgunun yürütülmesi veya veri deposuna yeni veri kaydedilmesi olabilir.

Code First yaklaşım, bu noktada, gelişmiş bir Varlık Veri Modeli Oluşturucu olan. Entity Framework, belirtilen koddan bir EDM üretecek. Bu, modele dahil olan sınıfları çözümleyerek, kuralları uygulayarak ve modeli akıcı API aracılığıyla yapılandırarak yapar. EDM oluşturulduktan sonra, Entity Framework temelde, projede bir EDMX dosyası bulunduğu şekilde davranır. Bu nedenle Code First modelin oluşturulması, bir EDMX 'e kıyasla Entity Framework için daha yavaş bir başlangıç zamanına çeviren ek karmaşıklık ekler. Maliyet, oluşturulmakta olan modelin boyutuna ve karmaşıklığına tamamen bağlıdır.

EDMX 'i Code First karşı kullanmayı seçerken, Code First tarafından tanıtılan esnekliğe, modelin ilk kez oluşturulmasına ilişkin maliyeti artırdığından emin olmak önemlidir. Uygulamanız bu ilk kez yükleme maliyetini tercih edebilir, genellikle Code First gitmek için tercih edilen yol olur.

## <a name="10-investigating-performance"></a>10 araştırma performansı

### <a name="101-using-the-visual-studio-profiler"></a>10,1 Visual Studio Profiler 'ı kullanma

Entity Framework performans sorunları yaşıyorsanız, uygulamanızın zamanını nerede harcadığını görmek için Visual Studio içinde yerleşik bir profil oluşturucu kullanabilirsiniz. Bu "Keşfetme - bölüm 1 ADO.NET Entity Framework performansının" blog gönderisinde pasta grafikler oluşturmak için kullandığımız aracıdır ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) Entity Framework, süre boyunca soğuk ve orta Gecikmeli sorgular nerede geçirdiği göster.

"Visual Studio 2010 Profiler kullanılarak profil oluşturma Entity Framework, veri ve modelleme müşteri danışmanlığı ekibi, bir performans sorununu araştırmak için profil oluşturucuyu nasıl kullandıklarından gerçek bir örnek gösterir.  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>. Bu gönderi bir Windows uygulaması için yazılmıştır. Bir Web uygulaması profili oluşturmanız gerekiyorsa, Windows performans Kaydedicisi (WPR) ve Windows Performans Çözümleyicisi (WPA) araçları Visual Studio 'dan çalışmaktan daha iyi çalışabilir. WPR ve WPA, Windows değerlendirme ve dağıtım seti 'Nde ( [http://www.microsoft.com/download/details.aspx?id=39982](https://www.microsoft.com/download/details.aspx?id=39982)) bulunan Windows performans araç seti 'nin bir parçasıdır.

### <a name="102-applicationdatabase-profiling"></a>10,2 uygulama/veritabanı profili oluşturma

Visual Studio 'da yerleşik olarak bulunan profil oluşturucu gibi araçlar, uygulamanızın ne zaman harcamış olduğunu söyler.  Çalışan uygulamanızın dinamik analizini, gereksinimlerinize bağlı olarak üretim veya ön üretim aşamasında gerçekleştiren ve veritabanı erişiminin genel sınırları ve çapraz düzenlerini belirten başka bir profil oluşturucu türü vardır.

Entity Framework profil Oluşturucu (\< @ no__t-1 ve ORMProfiler (\< @ no__t-3), ticari olarak kullanılabilir iki profil oluşturucular.

Uygulamanız Code First kullanan bir MVC uygulaması ise, StackExchange 'in mini Profiler 'ı kullanabilirsiniz. Scott Hanselman blog bu araç açıklar: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.

Uygulamanızın veritabanı etkinliğinin profilini oluşturma hakkında daha fazla bilgi için, [Entity Framework profil oluşturma veritabanı etkinliği](https://msdn.microsoft.com/magazine/gg490349.aspx)başlıklı Julie Lerman 'ın MSDN Magazine makalesine bakın.

### <a name="103-database-logger"></a>10,3 veritabanı günlükçüsü

Entity Framework 6 kullanıyorsanız, yerleşik günlüğe kaydetme işlevini de kullanmayı göz önünde bulundurun. Bağlam veritabanı özelliğine, etkinliklerini basit bir tek satır yapılandırması aracılığıyla günlüğe kaydetmek için de talimat verilir:

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

Bu örnekte, veritabanı etkinliği konsola kaydedilir, ancak Log özelliği herhangi bir @ no__t-0string @ no__t-1 temsilcisini çağırmak üzere yapılandırılabilir.

Veritabanı günlüğünü yeniden derlemeden etkinleştirmek istiyorsanız ve Entity Framework 6,1 veya sonraki bir sürümü kullanıyorsanız, uygulamanızın Web. config veya App. config dosyasına bir şifre ekleyerek bunu yapabilirsiniz.

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

Git yeniden derlemeye gerek kalmadan günlüğe kaydetme ekleme hakkında daha fazla bilgi için \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.

## <a name="11-appendix"></a>11 ek

### <a name="111-a-test-environment"></a>11,1 A. test ortamı

Bu ortam, istemci uygulamasından ayrı bir makine üzerinde veritabanıyla birlikte 2 makineli bir kurulum kullanır. Makineler aynı rafta olduğundan ağ gecikmesi görece düşüktür, ancak tek makineli bir ortamdan daha gerçekçi olur.

#### <a name="1111-app-server"></a>11.1.1 App Server

##### <a name="11111-software-environment"></a>11.1.1.1 yazılım ortamı

-   Entity Framework 4 yazılım ortamı
    -   İşletim sistemi adı: Windows Server 2008 R2 Enterprise SP1.
    -   Visual Studio 2010 – Ultimate.
    -   Visual Studio 2010 SP1 (yalnızca bazı karşılaştırmalar için).
-   Entity Framework 5 ve 6 yazılım ortamı
    -   İşletim sistemi adı: Windows 8.1 Enterprise
    -   Visual Studio 2013 – Ultimate.

##### <a name="11112-hardware-environment"></a>11.1.1.2 donanım ortamı

-   Çift Işlemci:     Intel (R) Xeon (R) CPU L5520 W3530 @ 2.27 GHz, 2261 Mhz8 GHz, 4 çekirdek, 84 mantıksal Işlemci.
-   2412 GB RamRAM.
-   136 GB SCSI250GB SATA 7200 RPM 3GB/s sürücü 4 bölüme bölünür.

#### <a name="1112-db-server"></a>11.1.2 DB sunucusu

##### <a name="11121-software-environment"></a>11.1.2.1 yazılım ortamı

-   İşletim sistemi adı: Windows Server 2008 R 28.1 Enterprise SP1.
-   SQL Server 2008 R22012.

##### <a name="11122-hardware-environment"></a>11.1.2.2 donanım ortamı

-   Tek Işlemci: Intel (R) Xeon (R) CPU L5520 @ 2.27 GHz, 2261 Mhleştirir-1620 0 @ 3.60 GHz, 4 çekirdek, 8 mantıksal Işlemci.
-   824 GB RamRAM.
-   465 GB ATA500GB SATA 7200 RPM 6GB/s sürücü 4 bölüme bölünür.

### <a name="112-b-query-performance-comparison-tests"></a>11,2 B. sorgu performansı karşılaştırma testleri

Bu testleri yürütmek için Northwind modeli kullanıldı. Entity Framework Tasarımcısı kullanılarak veritabanından oluşturulmuştur. Ardından, sorgu yürütme seçeneklerinin performansını karşılaştırmak için aşağıdaki kod kullanılmıştır:

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

### <a name="113-c-navision-model"></a>11,3 C. Navision modeli

Navision veritabanı, Microsoft Dynamics – NAV tanıtımı için kullanılan büyük bir veritabanıdır. Oluşturulan kavramsal model 1005 varlık kümesi ve 4227 ilişkilendirme kümesi içerir. Testte kullanılan model "Flat" olarak belirlenmiştir. Bu, kendisine devralma eklenmedi.

#### <a name="1131-queries-used-for-navision-tests"></a>Navision testleri için kullanılan 11.3.1 sorguları

Navision modeliyle kullanılan sorgular listesi, Entity SQL sorgularının 3 kategorisini içerir:

##### <a name="11311-lookup"></a>11.3.1.1 arama

Toplamaları olmayan basit bir arama sorgusu

-   Biriktirme 16232
-   Örnek:

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312singleaggregating"></a>11.3.1.2 Singletoplama

Birden çok toplama içeren normal bir bı sorgusu, ancak alt toplamlar (tek sorgu)

-   Biriktirme 2313
-   Örnek:

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

Burada MDF @ no__t-0SessionLogin @ no__t-1Time @ no__t-2Max () modelde şu şekilde tanımlanır:

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313aggregatingsubtotals"></a>11.3.1.3 Aggregatingalt toplamları

Toplamaları ve alt toplamları olan bir bı sorgusu (UNION ALL aracılığıyla)

-   Biriktirme 178
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
