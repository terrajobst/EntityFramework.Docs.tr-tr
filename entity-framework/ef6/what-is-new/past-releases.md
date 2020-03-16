---
title: Entity Framework son sürümleri-EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 1060bb99-765f-4f32-aaeb-d6635d3dbd3e
uid: ef6/what-is-new/past-releases
ms.openlocfilehash: b7181334cd125c5cbf296d5b3674c0b5f087f438
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2020
ms.locfileid: "79402175"
---
# <a name="past-releases-of-entity-framework"></a>Entity Framework geçmiş sürümleri

Entity Framework ilk sürümü, .NET Framework 3,5 SP1 ve Visual Studio 2008 SP1 'in bir parçası olarak 2008 ' de yayımlanmıştır.

EF 4.1 sürümünden başlayarak, [EntityFramework NuGet paketi](https://www.nuget.org/packages/EntityFramework/) olarak (NuGet.org ' deki en popüler paketlerden biri) birlikte gönderilir.

EntityFramework NuGet paketi, 4,1 ve 5,0 sürümleri arasında .NET Framework bir parçası olarak gönderilen EF kitaplıklarını genişletti.

Sürüm 6 ' dan başlayarak, EF açık kaynaklı bir proje haline geldi ve ayrıca .NET Framework tamamen bant dışına taşındı.
Yani, bir uygulamaya EntityFramework sürüm 6 NuGet paketi eklediğinizde, .NET Framework bir parçası olarak gönderilen EF bitlerini temel alan EF kitaplığının tamamen bir kopyasını alıyorsunuz.
Bu, yeni özelliklerin geliştirilmesi ve tesliminin ilerlemesini hızlandırmaya yardımcı oldu.

Haziran 2016 ' de EF Core 1,0 ' de yayımlandık. EF Core, yeni bir kod tabanına dayalıdır ve EF 'in daha basit ve genişletilebilir bir sürümü olarak tasarlanmıştır.
Şu anda EF Core, Microsoft 'un Entity Framework ekibi için geliştirmenin ana odadır.
Bu, EF6 için planlanmış yeni bir önemli özellik olmadığı anlamına gelir. Ancak EF6, hala açık kaynaklı bir proje ve desteklenen bir Microsoft ürünü olarak sürdürülür.

Aşağıda, her yayında sunulan yeni özelliklerle ilgili bilgiler içeren geçmiş sürümlerin, geriye doğru kronolojik sırada listesi verilmiştir.

## <a name="ef-tools-update-in-visual-studio-2017-157"></a>Visual Studio 'da EF araçları güncelleştirmesi 2017 15,7
Mayıs 2018 ' de, Visual Studio 2017 15,7 kapsamında EF araçlarının güncelleştirilmiş bir sürümünü yayımladık.
Bu, bazı yaygın sorun noktaları için iyileştirmeler içerir:

- Birkaç kullanıcı arabirimi erişilebilirlik hatası düzeltmeleri
- Mevcut veritabanlarından model oluştururken SQL Server performans gerileme için geçici çözüm [#4](https://github.com/aspnet/entityframework6/issues/4)
- SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185) daha büyük modellerin modellerini güncelleştirme desteği

EF araçları 'nın bu yeni sürümündeki diğer bir geliştirme, yeni bir projede model oluştururken EF 6,2 çalışma zamanını yüklemektedir. Visual Studio 'nun eski sürümlerinde, NuGet paketinin karşılık gelen sürümünü yükleyerek EF 6,2 çalışma zamanının (EF 'in geçmiş sürümleri) kullanılması mümkündür.

## <a name="ef-620"></a>EF 6.2.0
EF 6,2 çalışma zamanı, Ekim 2017 ' de NuGet 'e yayımlandı.
Açık kaynak katkıda bulunanlar topluluğumuza yönelik harika bir bölümde Teşekkür ederiz, EF 6,2 birçok [hata](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) düzeltmesi ve [ürün geliştirmesi](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20)içerir.

EF 6,2 çalışma zamanını etkileyen en önemli değişikliklerin kısa bir listesi aşağıdadır:

- Kalıcı bir önbellekten tamamlanan kod ilk modellerini yükleyerek başlangıç süresini düşürün [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- Dizinleri tanımlamak için akıcı API [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241) gıbı çeviren LINQ sorguları yazmayı etkinleştirmek Için dbfunctions. like ()
- Migrate. exe artık-Script seçeneğini destekliyor [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- EF6, artık SQL Server bir sıra tarafından oluşturulan anahtar değerleriyle çalışabilir [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- SQL Azure yürütme stratejisi için geçici hataların listesini güncelleştirin [#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Hata: sorguları veya SQL komutlarını yeniden deneme "SqlParameter zaten başka bir SqlParameterCollection tarafından bulunuyor" [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Hata: DbQuery. ToString (), hata ayıklayıcıda sık sık zaman aşımına uğruyor [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="ef-613"></a>EF 6.1.3
EF 6.1.3 Runtime, Ekim 2015 ' de NuGet 'e yayımlandı.
Bu sürüm yalnızca, 6.1.2 sürümünde bildirilen yüksek öncelikli kusurların ve gerilemelerin düzeltmelerini içerir.
Düzeltmeler şunlardır:

- Sorgu: EF 'de gerileme 6.1.2: dış uygulama, 1:1 ilişkiler ve "Let" yan tümcesi için tanıtılan ve daha karmaşık sorgular
- Devralınan sınıftaki temel sınıf özelliğini gizleme ile TPT sorunu
- DbMigration. SQL, metinde ' Go ' sözcüğü dahil edildiğinde başarısız olur
- UnionAll ve Intersect düzleştirme desteği için uyumluluk bayrağı oluştur
- Birden çok Içerme içeren sorgu 6.1.2 içinde çalışmaz (6.1.1 ile çalışma)
- EF 6.1.1 'ten 6.1.2 'e yükselttikten sonra "SQL sözdiziminde bir hata var" özel durumu

## <a name="ef-612"></a>EF 6.1.2
EF 6.1.2 Runtime, Aralık 2014 ' de NuGet 'e yayımlandı.
Bu sürüm çoğunlukla hata düzeltmeleri hakkında. Ayrıca, Topluluk üyelerinden birkaç önemli değişiklik de kabul ediyoruz:
- **Sorgu önbelleği parametreleri App/Web. Configuration dosyasından yapılandırılabilir**
    ``` xml
    <entityFramework>
      <queryCache size='1000' cleaningIntervalInSeconds='-1'/>
    </entityFramework>
    ```
- **DbMigration 'Daki SqlFile ve sqlresource yöntemleri** , dosya veya katıştırılmış kaynak olarak depolanan bir SQL komut dosyasını çalıştırmanıza olanak sağlar.

## <a name="ef-611"></a>EF 6.1.1
EF 6.1.1 Runtime, Haziran 2014 ' de NuGet 'e yayımlandı.
Bu sürüm, bir dizi kişinin karşılaştığı sorunlara yönelik düzeltmeler içerir. Diğerleri arasında:
- Tasarımcı: EF6 tasarımcısında Decimal Precision EF5 edmx açılırken hata oluştu
- LocalDB için varsayılan örnek algılama mantığı SQL Server 2014 ile çalışmıyor

## <a name="ef-610"></a>EF 6.1.0
EF 6.1.0 Runtime, Mart 2014 ' de NuGet 'e yayımlandı.
Bu küçük güncelleştirme, önemli sayıda yeni özellik içerir:

- **Araç birleştirme** , yenı bir EF modeli oluşturmak için tutarlı bir yol sağlar. Bu özellik, var olan bir veritabanından ters mühendislik dahil olmak üzere [Code First modelleri oluşturmayı desteklemek için ADO.NET varlık veri modeli Sihirbazı 'nı genişletir](~/ef6/modeling/code-first/workflows/existing-database.md). Bu özellikler daha önce, EF güç araçlarında Beta kalitede kullanıma sunulmuştur.
- **[İşlem işleme hatalarının işlenmesi](~/ef6/fundamentals/connection-resiliency/commit-failures.md)** , işlem işlemlerini ele almak için yeni tanıtılan özelliği kullanan CommitFailureHandler 'ı sağlar. CommitFailureHandler, işlem yürüten bağlantı hatalarından otomatik kurtarmaya izin verir.
- **[Indexattribute](~/ef6/modeling/code-first/data-annotations.md)** , Code First modelinizdeki bir özelliğe (veya özelliklere) `[Index]` özniteliği yerleştirilerek dizinlerin belirtilmesini sağlar. Code First, veritabanında karşılık gelen bir dizin oluşturur.
- **Ortak eşleme API 'si** , Özellikler ve türlerin veritabanındaki sütunlara ve tablolarla nasıl eşlendiğine ilişkin EF bilgisine erişim sağlar. Geçmişte bu API, iç sürümiydi.
- Uygulama **[/Web. config dosyası aracılığıyla yakalayıcılar yapılandırma özelliği](~/ef6/fundamentals/configuring/config-file.md)** , uygulama yeniden derlenmeden ekleme yapılmasına izin verir.
- **System. Data. Entity. Infrastructure. yakalayıcısı. Databasegünlükçü**, tüm veritabanı işlemlerini bir dosyaya günlüğe kaydetmek kolay hale getiren yeni bir dinleyici. Önceki özellikle birlikte, bu, yeniden derlemenize gerek kalmadan [dağıtılan bir uygulama için veritabanı işlemlerinin günlüğe kaydedilmesini](~/ef6/fundamentals/configuring/config-file.md)kolayca değiştirmenize olanak sağlar.
- **Geçiş modeli değişikliği algılaması** geliştirilmiştir, böylece yapı iskelesi geçişleri daha doğru olur; değişiklik algılama sürecinin performansı de geliştirilmiştir.
- Başlatma sırasında azaltılmış veritabanı işlemleri, LINQ sorgularında null eşitlik karşılaştırması için iyileştirmeler, daha hızlı görünüm oluşturma (model oluşturma) ve birden çok ilişki içeren izlenen varlıkların daha verimli bir şekilde hazırlanması gibi **performans iyileştirmeleri** .

## <a name="ef-602"></a>EF 6.0.2
EF 6.0.2 Runtime, Aralık 2013 ' de NuGet 'e yayımlandı.
Bu düzeltme eki sürümü, EF6 sürümünde tanıtılan sorunları düzeltmek için sınırlıdır (EF5 'den itibaren performans/davranış gerilemeleri).

## <a name="ef-601"></a>EF 6.0.1
*, Visual Studio 'nun birkaç aydan daha önce kilitlenen bir sürümüne katıştırıldığından, EF 6.0.1 Runtime, Ekim 2013 ' de aynı anda, ve için EF 6.0.0 ile birlikte NuGet 'e yayınlanmıştır.
Bu düzeltme eki sürümü, EF6 sürümünde tanıtılan sorunları düzeltmek için sınırlıdır (EF5 'den itibaren performans/davranış gerilemeleri).
En önemli değişiklikler, EF modelleri için ısınma sırasında bazı performans sorunlarını çözmemizi sağlar.
Bu, ısınma süresi EF6 ' de odaklanılmış olan bir alandır ve bu sorunlar EF6 ' de yapılan diğer performans kazançlarını de tahmin edildiği için önemlidir.

## <a name="ef-60"></a>EF 6,0
EF 6.0.0 Runtime, Ekim 2013 ' de NuGet 'e yayımlandı.
Bu, .NET Framework bir parçası olan EF bits 'e bağlı olmayan [EntityFramework NuGet paketine](https://www.nuget.org/packages/EntityFramework/) , tamamlanmış bir EF çalışma zamanının dahil olduğu ilk sürümdür.
Çalışma zamanının kalan bölümlerinin NuGet paketine taşınması, var olan kod için bir dizi Son değişiklik gerektirdi.
Yükseltmek için gereken el ile adımlar hakkında daha fazla bilgi için [Entity Framework 6](upgrading-to-ef6.md) ' ya yükseltme hakkında bölümüne bakın.

Bu sürüm çok sayıda yeni özellik içerir.
Aşağıdaki özellikler Code First veya EF Designer ile oluşturulan modeller için çalışır:

- **[Zaman uyumsuz sorgu ve kaydetme](~/ef6/fundamentals/async.md)** , .NET 4,5 ' de tanıtılan görev tabanlı zaman uyumsuz desenler için destek ekler.
- **[Bağlantı dayanıklılığı](~/ef6/fundamentals/connection-resiliency/retry-logic.md)** , geçici bağlantı hatalarından otomatik kurtarmayı mümkün.
- **[Kod tabanlı yapılandırma](~/ef6/fundamentals/configuring/code-based.md)** , bir yapılandırma dosyasında geleneksel olarak gerçekleştirilen yapılandırmayı gerçekleştirme seçeneği sağlar: kodda.
- **[Bağımlılık çözümlemesi](~/ef6/fundamentals/configuring/dependency-resolution.md)** , hizmet bulucu deseninin desteğini tanıtır ve özel uygulamalarla değiştirilebilmesi için bazı işlevsellik parçalarını değerlendirdik.
- **[Ele geçirme/SQL günlüğü](~/ef6/fundamentals/logging-and-interception.md)** , en üstte yerleşik olarak bulunan basit SQL günlüğü ile EF işlemlerinin ele geçirilmesini sağlayan alt düzey yapı taşları sağlar.
- Test edilebilir **iyileştirmeler** , [bir sahte işlem çerçevesi kullanırken](~/ef6/fundamentals/testing/mocking.md) veya [kendi test Double 'larınızı yazarken](~/ef6/fundamentals/testing/writing-test-doubles.md)DbContext ve dbset için test Double değerleri oluşturmayı kolaylaştırır.
- **[DbContext artık zaten açık olan bir DbConnection ile oluşturulabilir](~/ef6/fundamentals/connection-management.md)** . Bu, bağlantı bağlamı oluşturulurken (bağlantının durumunu garanti edebildiğiniz bileşenler arasında bir bağlantı paylaşımı gibi) açık olduğunda yararlı olabilecek senaryolara izin verebilir.
- **[Iyileştirilmiş Işlem desteği](~/ef6/saving/transactions.md)** , çerçeveye dışarıdan bir işlem için destek sağlar ve Framework içinde işlem oluşturmanın gelişmiş yollarını sağlar.
- **.Net 4,0 ' de numaralandırmalar, uzamsal ve daha Iyi performans** -.NET Framework olması için kullanılan çekırdek bileşenleri EF NuGet paketine taşıyarak, artık .NET 4,0 üzerinde EF5 adresinden enum desteği, uzamsal veri türleri ve performans iyileştirmeleri sunabilebiliyoruz.
- **Sıralanabilir. LINQ sorgularında gelişmiş performans performansı**.
- Özellikle büyük modeller için **geliştirilmiş ısınma süresi (görünüm oluşturma)** .
- **Takılabilir &amp;, eklenebilir bir hizmet**.
- Varlık sınıflarında **eşittir veya GetHashCode özel uygulamaları** artık desteklenmektedir.
- **Dbset. AddRange/RemoveRange** bir kümeden birden çok varlık eklemek veya kaldırmak için iyileştirilmiş bir yol sağlar.
- **Dbchangetracker. HasChanges** , veritabanına kaydedilecek bekleyen değişiklikler olup olmadığını görmek için kolay ve etkili bir yol sağlar.
- **Sqlcefunctions** , SqlFunctions ÖĞESINE bir SQL Compact eşdeğeri sağlar.

Aşağıdaki özellikler yalnızca Code First için geçerlidir:

- **[Özel Code First kuralları](~/ef6/modeling/code-first/conventions/custom.md)** , yinelenen yapılandırmadan kaçınmaya yardımcı olmak için kendi kurallarınızı yazmaya izin verir. Daha karmaşık kurallar yazmanıza imkan tanımak için basit kurallara ve daha karmaşık yapı taşlarına yönelik basit bir API sunuyoruz.
- **[Saklı yordamları ekleme/güncelleştirme/silme Ile eşleme Code First](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)** artık desteklenmektedir.
- **[Idempotent geçişleri betikleri](~/ef6/modeling/code-first/migrations/index.md)** , herhangi bir sürümdeki bir veritabanını en son sürüme yükseltebilmeniz IÇIN bir SQL betiği oluşturmanıza olanak sağlar.
- **[Yapılandırılabilir geçişler geçmişi tablosu](~/ef6/modeling/code-first/migrations/history-customization.md)** , geçişleri geçmiş tablosunun tanımını özelleştirmenizi sağlar. Bu özellikle, geçişler geçmiş tablosunun doğru şekilde çalışması için uygun veri türleri vb. gerektiren veritabanı sağlayıcıları için yararlıdır.
- **Veritabanı başına birden çok bağlam** , geçişleri kullanırken veya Code First veritabanını otomatik olarak oluştururken veritabanı başına bir Code First modelinin önceki sınırlamasını kaldırır.
- **[Dbmodelbuilder. HasDefaultSchema](~/ef6/modeling/code-first/fluent/types-and-properties.md)** , bir Code First modelinin varsayılan veritabanı şemasının tek bir yerde yapılandırılmasına izin veren yeni BIR Code First API 'sidir. Daha önce Code First varsayılan şeması, &quot;dbo&quot; ve bir tablonun ait olduğu şemayı ToTable API 'SI aracılığıyla yapılandırmanın tek yolunu yapılandırmak için sabit kodlandı.
- **Dbmodelbuilder. Configurations. AddFromAssembly yöntemi** , Code FIRST akıcı API ile yapılandırma sınıfları kullandığınızda bir derlemede tanımlanan tüm yapılandırma sınıflarını kolayca eklemenize olanak tanır.
- **[Özel geçiş işlemleri](https://romiller.com/2013/02/27/ef6-writing-your-own-code-first-migration-operations/)** , kod tabanlı geçişlerde kullanılacak ek işlemler eklemenizi sağlar.
- **Varsayılan işlem yalıtım düzeyi** , Code First kullanılarak oluşturulan veritabanları için READ_COMMITTED_SNAPSHOT olarak değiştirilir ve daha fazla ölçeklenebilirlik ve daha az kilitlenme sağlar.
- **Varlık ve karmaşık türler artık nestedinsıde sınıfları olabilir**.

## <a name="ef-50"></a>EF 5,0
EF 5.0.0 Runtime, Ağustos 2012 ' de NuGet 'e yayımlandı.
Bu sürüm, sabit listesi desteği, tablo değerli işlevler, uzamsal veri türleri ve çeşitli performans iyileştirmeleri dahil bazı yeni özellikler sunar.

Visual Studio 2012 ' de Entity Framework Designer, model başına birden çok diyagram, Tasarım yüzeyinde şekillerin renklendirmesi ve saklı yordamların Toplu içe aktarılması için destek sunar.

EF 5 sürümü için özel olarak birlikte koyduğumuz içeriğin bir listesi aşağıda verilmiştir:

-   [EF 5 yayın gönderisi](https://blogs.msdn.com/b/adonet/archive/2012/08/15/ef5-released.aspx)
-   EF5 'deki yeni özellikler
    -   [Code First enum desteği](~/ef6/modeling/code-first/data-types/enums.md)
    -   [EF tasarımcısında enum desteği](~/ef6/modeling/designer/data-types/enums.md)
    -   [Code First uzamsal veri türleri](~/ef6/modeling/code-first/data-types/spatial.md)
    -   [EF tasarımcısında uzamsal veri türleri](~/ef6/modeling/designer/data-types/spatial.md)
    -   [Uzamsal türler için sağlayıcı desteği](~/ef6/fundamentals/providers/spatial-support.md)
    -   [Tablo Değerli İşlevler](~/ef6/modeling/designer/advanced/tvfs.md)
    -   [Model başına birden çok diyagram](~/ef6/modeling/designer/multiple-diagrams.md)
-   Modelinizi ayarlama
    -   [Model Oluşturma](~/ef6/modeling/index.md)
    -   [Bağlantılar ve modeller](~/ef6/fundamentals/configuring/connection-strings.md)
    -   [Performans Konuları](~/ef6/fundamentals/performance/perf-whitepaper.md)
    -   [Microsoft SQL Azure ile çalışma](~/ef6/fundamentals/connection-resiliency/retry-logic.md)
    -   [Yapılandırma dosyası ayarları](~/ef6/fundamentals/configuring/config-file.md)
    -   [Sözlük](~/ef6/resources/glossary.md)
    -   Code First
        -   [Yeni bir veritabanına Code First (izlenecek yol ve video)](~/ef6/modeling/code-first/workflows/new-database.md)
        -   [Var olan bir veritabanına Code First (izlenecek yol ve video)](~/ef6/modeling/code-first/workflows/existing-database.md)
        -   [Kurallar](~/ef6/modeling/code-first/conventions/built-in.md)
        -   [Veri Açıklamaları](~/ef6/modeling/code-first/data-annotations.md)
        -   [Akıcı API-özellikleri yapılandırma/eşleme & türleri](~/ef6/modeling/code-first/fluent/types-and-properties.md)
        -   [Akıcı API-Ilişkileri yapılandırma](~/ef6/modeling/code-first/fluent/relationships.md)
        -   [VB.NET ile akıcı API](~/ef6/modeling/code-first/fluent/vb.md)
        -   [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md)
        -   [Otomatik Code First Migrations](~/ef6/modeling/code-first/migrations/automatic.md)
        -   [Migrate. exe](~/ef6/modeling/code-first/migrations/migrate-exe.md)
        -   [DbSets 'leri tanımlama](~/ef6/modeling/code-first/dbsets.md)
    -   EF Tasarımcısı
        -   [Model First (izlenecek yol ve video)](~/ef6/modeling/designer/workflows/model-first.md)
        -   [Database First (izlenecek yol ve video)](~/ef6/modeling/designer/workflows/database-first.md)
        -   [Karmaşık Türler](~/ef6/modeling/designer/data-types/complex-types.md)
        -   [İlişkilendirmeler/Ilişkiler](~/ef6/modeling/designer/relationships.md)
        -   [TPT devralma düzeni](~/ef6/modeling/designer/inheritance/tpt.md)
        -   [TPH devralma stili](~/ef6/modeling/designer/inheritance/tph.md)
        -   [Saklı yordamlar içeren sorgu](~/ef6/modeling/designer/stored-procedures/query.md)
        -   [Birden çok sonuç kümesiyle saklı yordamlar](~/ef6/modeling/designer/advanced/multiple-result-sets.md)
        -   [Saklı yordamlarla INSERT, Update & Delete](~/ef6/modeling/designer/stored-procedures/cud.md)
        -   [Varlığı birden çok tabloya eşleme (varlık bölme)](~/ef6/modeling/designer/entity-splitting.md)
        -   [Birden çok varlığı tek bir tabloyla eşleme (tablo bölme)](~/ef6/modeling/designer/table-splitting.md)
        -   [Sorguları tanımlama](~/ef6/modeling/designer/advanced/defining-query.md)
        -   [Kod oluşturma şablonları](~/ef6/modeling/designer/codegen/index.md)
        -   [ObjectContext 'e geri döndürülüyor](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)
-   Modelinizi kullanma
    -   [DbContext ile Çalışma](~/ef6/fundamentals/working-with-dbcontext.md)
    -   [Varlıkları sorgulama/bulma](~/ef6/querying/index.md)
    -   [Ilişkilerle çalışma](~/ef6/fundamentals/relationships.md)
    -   [Ilgili varlıkları yükleme](~/ef6/querying/related-data.md)
    -   [Yerel verilerle çalışma](~/ef6/querying/local-data.md)
    -   [N katmanlı uygulamalar](~/ef6/fundamentals/disconnected-entities/index.md)
    -   [Ham SQL Sorguları](~/ef6/querying/raw-sql.md)
    -   [İyimser eşzamanlılık desenleri](~/ef6/saving/concurrency.md)
    -   [Proxy ile çalışma](~/ef6/fundamentals/proxies.md)
    -   [Değişiklikleri otomatik algıla](~/ef6/saving/change-tracking/auto-detect-changes.md)
    -   [Izleme sorguları yok](~/ef6/querying/no-tracking.md)
    -   [Load Yöntemi](~/ef6/querying/load-method.md)
    -   [Ekle/Ekle ve varlık durumları](~/ef6/saving/change-tracking/entity-state.md)
    -   [Özellik değerleriyle çalışma](~/ef6/saving/change-tracking/property-values.md)
    -   [WPF ile veri bağlama (Windows Presentation Foundation)](~/ef6/fundamentals/databinding/wpf.md)
    -   [WinForms ile veri bağlama (Windows Forms)](~/ef6/fundamentals/databinding/winforms.md)

## <a name="ef-431"></a>EF 4.3.1
EF 4.3.1 Runtime, AŞV 4.3.0 sonrasında 2012 Şubat ayında NuGet 'e yayımlandı.
Bu düzeltme eki sürümü, EF 4,3 sürümüne bazı hata düzeltmeleri içeriyordu ve Visual Studio 2012 ile EF 4,3 kullanan müşteriler için daha iyi LocalDB desteği getirmiştir.

EF 4.3.1 Release 'e özellikle birlikte koyduğumuz içeriğin bir listesi aşağıda EF 4,1 için belirtilen içeriğin çoğu da EF 4,3 ' de geçerlidir:

-   [EF 4.3.1 Release blog gönderisi](https://blogs.msdn.com/b/adonet/archive/2012/02/29/ef4-3-1-and-ef5-beta-1-available-on-nuget.aspx)

## <a name="ef-43"></a>EF 4,3
EF 4.3.0 Runtime, Şubat 2012 ' de NuGet 'e yayımlandı.
Bu sürüm, Code First modeliniz geliştikçe Code First tarafından oluşturulan bir veritabanının artımlı olarak değiştirilmesine izin veren yeni Code First Migrations özelliğini içerir.

EF 4,3 sürümü için bir araya koyduğumuz içeriğin bir listesi aşağıda, EF 4,1 için belirtilen içeriğin çoğu de EF 4,3 için geçerlidir:
-   [EF 4,3 yayın gönderisi](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-released.aspx)
-   [EF 4,3 kod tabanlı geçişleri gözden geçirme](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-code-based-migrations-walkthrough.aspx)
-   [EF 4,3 otomatik geçişleri Izlenecek yol](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-automatic-migrations-walkthrough.aspx)

## <a name="ef-42"></a>EF 4,2
EF 4.2.0 Runtime, Kasım 2011 ' de NuGet 'e yayımlandı.
Bu sürüm, EF 4.1.1 sürümüne hata düzeltmeleri içerir.
Bu yayında yalnızca hata düzeltmeleri bulunduğundan, EF 4.1.2 Patch sürümü olabilir, ancak 4.1. x yayınlarında kullandığımız Tarih tabanlı düzeltme eki sürüm numaralarını benimsememizi ve anlamsal sürüm oluşturma için [semantik Versionsing](https://semver.org) standardını benimsememizi sağlamak üzere 4,2 ' a geçmek zorunda olduğumuz.

Yalnızca EF 4,2 sürümü için birlikte koyduğumuz içerik listesi, EF 4,1 için sunulan içerik yine de EF 4,2 için geçerlidir:

-   [EF 4,2 yayın gönderisi](https://blogs.msdn.com/b/adonet/archive/2011/11/01/ef-4-2-released.aspx)
-   [Code First Izlenecek yol](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-code-first-walkthrough.aspx)
-   [Model & Database First Izlenecek yol](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-model-amp-database-first-walkthrough.aspx)

## <a name="ef-411"></a>EF 4.1.1
EF 4.1.10715 Runtime, 2011 Temmuz ayında NuGet 'e yayımlandı.
Hata düzeltmelerine ek olarak, bu düzeltme eki sürümü, tasarım zamanı araçlarının Code First modeliyle çalışmasını kolaylaştırmak için bazı bileşenler getirmiştir.
Bu bileşenler Code First Migrations (EF 4,3 ' de bulunur) ve EF güç araçları tarafından kullanılır.

Paketin alışılmadık sürüm numarası 4.1.10715 olduğunu fark edeceksiniz.
[Anlamsal sürüm oluşturmayı](https://semver.org)benimsemeye karar vermeden önce tarih tabanlı düzeltme eki sürümlerini kullanmak için kullandık.
Bu sürümü EF 4,1 Patch 1 (veya EF 4.1.1) olarak düşünebilirsiniz.

4\.1.1 sürümü için birlikte koyduğumuz içeriğin bir listesi aşağıda verilmiştir:

-   [EF 4.1.1 yayın gönderisi](https://blogs.msdn.com/b/adonet/archive/2011/07/25/ef-4-1-update-1-released.aspx)

## <a name="ef-41"></a>EF 4,1
EF 4.1.10331 Runtime, NuGet 'de 2011 Nisan 'da yayımlanacak ilk ilkiydi.
Bu sürüm Basitleştirilmiş DbContext API 'sini ve Code First iş akışını içerir.

Gerçekten 4,1 olması gereken, 4.1.10331, alışılmadık bir sürüm numarası olduğunu fark edeceksiniz. Ayrıca, 4.1.0-RC olması gereken bir 4.1.10311 sürümü vardır (' RC ' ' Release Candidate ' için temsil edilir).
[Anlamsal sürüm oluşturmayı](https://semver.org)benimsemeye karar vermeden önce tarih tabanlı düzeltme eki sürümlerini kullanmak için kullandık.

4,1 sürümü için birlikte koyduğumuz içeriğin bir listesi aşağıda verilmiştir. Bunun büyük bölümü Entity Framework sonraki sürümleri için de geçerlidir:

-   [EF 4,1 yayın gönderisi](https://blogs.msdn.com/b/adonet/archive/2011/04/11/ef-4-1-released.aspx)
-   [Code First Izlenecek yol](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-code-first-walkthrough.aspx)
-   [Model & Database First Izlenecek yol](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-model-amp-database-first-walkthrough.aspx)
-   [SQL Azure Federations ve Entity Framework](https://blogs.msdn.com/b/adonet/archive/2012/01/10/sql-azure-federations-and-the-entity-framework.aspx)

## <a name="ef-40"></a>EF 4,0
Bu sürüm .NET Framework 4 ' te ve Visual Studio 2010 ' a Nisan 2010 ' de eklenmiştir.
Bu sürümdeki önemli yeni özellikler POCO desteği, yabancı anahtar eşleme, yavaş yükleme, test edilebilir geliştirmeler, özelleştirilebilir kod üretimi ve Model First iş akışı dahil edilmiştir.

Entity Framework ikinci sürümü olsa da, birlikte gelen .NET Framework sürümü ile hizalamak için EF 4 olarak adlandırılmıştı.
Bu sürümden sonra, artık .NET Framework sürümüne bağlı olmadığından, NuGet ve benimsenen sürümde Entity Framework oluşturmaya başladık.

Bazı sonraki .NET Framework sürümlerinin, dahil olan EF bitleriyle ilgili önemli güncelleştirmelerle birlikte gönderildiğini unutmayın.
Aslında, EF 5,0 'nin yeni özelliklerinin çoğu bu bitler için geliştirmeler olarak uygulanmıştır.
Öte yandan, EF için sürüm oluşturma hikayesini almak için, .NET Framework bir parçası olan EF bitlerini, daha yeni sürümlerin [EntityFramework NuGet paketinden](https://www.nuget.org/packages/EntityFramework/)oluşması halınde, EF 4,0 çalışma zamanı olarak adlandırmaya devam ediyoruz.

## <a name="ef-35"></a>EF 3,5
Entity Framework ilk sürümü .NET 3,5 Service Pack 1 ve Visual Studio 2008 SP1 'e eklenmiştir ve bu sürüm, 2008 Ağustos ayında yayımlanmıştır.
Bu sürüm, Database First iş akışını kullanarak temel O/RM desteğini sağladı.
