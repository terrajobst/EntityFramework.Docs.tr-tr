---
title: Eski sürümler - EF6 olan Entity Framework'ün
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1060bb99-765f-4f32-aaeb-d6635d3dbd3e
caps.latest.revision: 4
ms.openlocfilehash: 64cf28b858ad364243447a529f26475d449cf73e
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913453"
---
# <a name="past-releases-of-entity-framework"></a>Eski sürümler Entity Framework'ün

Entity Framework'ün ilk sürümü, 2008, .NET Framework 3.5 SP1 ve Visual Studio 2008 SP1'in bir parçası olarak yayımlanmıştır.

Sevk olarak EF4.1 sürüm ile başlayarak [EntityFramework NuGet paketini](https://www.nuget.org/packages/EntityFramework/) -en popüler paketleri NuGet.org üzerinde şu anda biri.

Sürümleri arasında 4.1 ve 5.0, EntityFramework NuGet paketini, .NET Framework bir parçası olarak gönderildiğini EF kitaplıkları genişletilmiş.   

EF sürüm 6 ile başlayarak, açık kaynaklı proje hale geldi ve ayrıca bant formdan .NET Framework tamamen taşınır.
Bu uygulamaya EntityFramework sürüm 6 NuGet paketini ekleme, .NET Framework bir parçası olarak gönderilen EF BITS bağlı olmayan EF kitaplığı tam bir kopyasını alıyorsanız anlamına gelir.
Bu biraz geliştirme hızımızı ve yeni özellik teslimini hızlandırın yardımcı olmuştur.

Haziran 2016'da, EF Core 1.0 yayımladık. EF Core, yeni bir kod temeli üzerinde temel alır ve bir EF daha basit ve Genişletilebilir sürüm olarak tasarlanmıştır.
Şu anda EF Core odaklandığı Microsoft Entity Framework takım için geliştirme aşamasındadır.
Başka bir deyişle, EF6 için planlanan yeni önemli bir özellik vardır. EF6 ancak yine de açık kaynak bir proje ve desteklenen bir Microsoft ürünü korunur.

Her sürümde sunulan yeni özelliklerle ilgili bilgilerle ters kronolojik sırada önceki sürümlerin bir listesi aşağıdadır.

## <a name="ef-613"></a>EF 6.1.3
6.1.3 EF çalışma zamanı için NuGet'ın Ekim 2015'te bırakıldığını.
Bu sürüm yalnızca düzeltmeleri yüksek öncelikli hataları ve gerilemeleri 6.1.2 üzerinde bildirilen içerir yayın.
Düzeltmeleri içerir:

- Sorgu: Gerileme EF 6.1.2: sunulan OUTER APPLY ve 1:1 ilişki ve "izin" yan tümcesi için daha karmaşık sorgular
- Devralınan sınıf temel sınıf özelliği gizleme TPT sorun
- 'Git' sözcüğün metinde içerdiğinde DbMigration.Sql başarısız olur.
- Uyumluluk bayrağı UnionAll ve INTERSECT düzleştirme desteği için oluşturma
- Birden çok içeren sorgu 6.1.2 (içinde 6.1.1 çalışma) çalışmıyor
- EF 6.1.1 için 6.1.2 ' yükselttikten sonra ", bir hata SQL diziminizde sahip" özel durumu

## <a name="ef-612"></a>EF 6.1.2
6.1.2 EF çalışma zamanı için NuGet aralık 2014'te bırakıldığını.
Bu çoğunlukla hata düzeltmeleri hakkında sürümüdür. Biz de birkaç önemli değişiklik topluluk üyelerinden kabul:
- **Sorgu önbelleği parametreleri app/web.configuration dosyasından yapılandırılabilir**
    ``` xml
    <entityFramework>
      <queryCache size='1000' cleaningIntervalInSeconds='-1'/>
    </entityFramework>
    ```
- **DbMigration SqlFile ve SqlResource yöntemlerde** SQL çalıştırmanıza olanak tanır betiği dosya olarak depolanan veya katıştırılmış kaynak.

## <a name="ef-611"></a>EF 6.1.1
EF 6.1.1 çalışma zamanı için NuGet Haziran 2014'te bırakıldığını.
Bu sürüm, birden çok kişi karşılaştığı sorunlar için düzeltmeler içerir. Diğerlerinin yanı sıra:
- Tasarımcı: Ondalık Duyarlığı EF6 Tasarımcısı'nda ile EF5 edmx açılırken hata
- Varsayılan örneği algılama mantığı LocalDB ile SQL Server 2014 çalışmaz

## <a name="ef-610"></a>EF 6.1.0
6.1.0 EF çalışma zamanı için NuGet Mart 2014'te bırakıldığını.
Bu küçük güncelleştirme çok sayıda yeni özellik içerir:

- **Birleştirme Araçları** yeni EF modeli oluşturmak için tutarlı bir yol sağlar. Bu özellik [ADO.NET varlık veri modeli Sihirbazı'nı oluşturma Code First modelleri destekleyecek şekilde genişletir](~/ef6/modeling/code-first/workflows/existing-database.md), varolan bir veritabanından tersine mühendislik dahil. Bu özellikler daha önce Beta kalite EF Power Tools içinde mevcuttu.
- **[İşlem işleme hatalarının işleme](~/ef6/fundamentals/connection-resiliency/commit-failures.md) ** kullanan yeni çıkan yeteneğini işlem işlem kesmeye CommitFailureHandler sağlar. CommitFailureHandler bağlantı hatalardan otomatik kurtarma, bir işlem Sistemi'ne artırabileceksiniz sağlar.
- **[IndexAttribute](~/ef6/modeling/code-first/data-annotations.md) ** yerleştirerek belirtilmesi için dizinler sağlayan bir `[Index]` Code First modelinizde bir özellik (veya özellikleri) özniteliği. Kod önce sonra karşılık gelen bir dizin veritabanında oluşturursunuz.
- **Genel eşleme API** EF sahip nasıl özellikler ve türler sütun ve veritabanındaki tablolar eşlendiğine şirket bilgilerine erişim sağlar. Sürümleri bu API iç içindeydi.
- **[Kesiciler App/Web.config dosyasıyla yapılandırma yeteneği](~/ef6/fundamentals/configuring/config-file.md) ** dinleyiciler, uygulamanın yeniden derlemeye gerek kalmadan eklenecek sağlar.
- **System.Data.Entity.Infrastructure.Interception.DatabaseLogger**günlük dosyasında tüm veritabanı işlemleri kolaylaştıran yeni bir dinleyiciyi olduğu. Önceki özelliğiyle birlikte kolayca böylece [dağıtılmış bir uygulama için veritabanı işlem günlüğünü geçiş](~/ef6/fundamentals/configuring/config-file.md), yeniden derlemenize gerek kalmadan.
- **Geçişleri modeli değişiklik algılama** iskele kurulmuş geçişleri değişiklik algılama işleminin performansı da geliştirilmiştir daha doğru; olacak şekilde iyileştirilmiştir.
- **Performans iyileştirmeleri** başlatma sırasında azaltılmış veritabanı işlemleri dahil olmak üzere en iyi duruma getirme LINQ sorgularında null eşitlik karşılaştırması için daha hızlı oluşturma (modeli oluşturma) daha fazla senaryolarda ve görüntüleme daha verimli materialization birden fazla ilişki ile izlenen varlık.

## <a name="ef-602"></a>EF 6.0.2
6.0.2 EF çalışma zamanı için NuGet aralık 2013'te bırakıldığını.
Tanıtılan sorunları düzeltmek için bu düzeltme eki sürümü sınırlıdır EF6 sürümde (EF5 beri performans/davranışını depoda gerilemeyi).

## <a name="ef-601"></a>EF 6.0.1
6.0.1 EF çalışma zamanı bırakıldığını için NuGet, Ekim 2013 EF 6.0.0'dan, aynı anda ile ikinci birkaç ay önce kilitli Visual Studio sürümünde katıştırılan olduğundan.
Tanıtılan sorunları düzeltmek için bu düzeltme eki sürümü sınırlıdır EF6 sürümde (EF5 beri performans/davranışını depoda gerilemeyi).
EF modellerine yönelik Isınma sırasında bazı performans sorunları gidermek için en önemli değişiklikler bulunuyordu.
Bu, Isınma performans EF6 ve bu sorunları odak alanı EF6 içinde yapılan gelen performans artışı bazıları negating gibi önemli.

## <a name="ef-60"></a>EF 6.0
6.0.0'dan EF çalışma zamanı için NuGet'ın Ekim 2013'te bırakıldığını.
Bu, tam bir EF çalışma zamanı dahil ilk sürümdür [EntityFramework NuGet paketini](https://www.nuget.org/packages/EntityFramework/) hangi bağımlı değildir EF bitleri, .NET Framework'ün bir parçasıdır.
NuGet paketi için çalışma zamanı kalan bölümlerini taşıma, varolan kod için yeni değişiklik sayısı gereklidir.
Bölümüne [Entity Framework 6'ya yükseltme](upgrading-to-ef6.md) yükseltmek amacıyla gereken el ile yapılacak adımlar hakkında daha fazla bilgi.

Bu sürüm, çok sayıda yeni özellikler içerir.
Aşağıdaki özellikler, Code First veya EF Designer ile oluşturulan modeller için çalışır:

- **[Zaman uyumsuz sorgu ve tasarruf](~/ef6/fundamentals/async.md) ** .NET 4. 5 ' tanıtılan görev tabanlı zaman uyumsuz desenleri için destek ekler.
- **[Bağlantı dayanıklılığı](~/ef6/fundamentals/connection-resiliency/retry-logic.md) ** geçici bağlantı hatalardan otomatik kurtarma sağlar.
- **[Kod tabanlı yapılandırma](~/ef6/fundamentals/configuring/code-based.md) ** kodda bir yapılandırma dosyasında – geleneksel olarak gerçekleştirilen yapılandırma – gerçekleştirme seçeneği sunar.
- **[Bağımlılık çözümlemesi](~/ef6/fundamentals/configuring/dependency-resolution.md) ** destek sunuyor hizmet bulucu için desen ve bazı özel uygulamaları ile değiştirilebilir işlevsellik parçalarını kullanıma factored.
- **[Durdurma/SQL günlüğü](~/ef6/fundamentals/logging-and-interception.md) ** durdurma EF işlemleri için üstte yerleşik basit SQL günlüğü alt düzey yapı taşlarını sağlar.
- **Test Edilebilirlik geliştirmeleri** DbContext ve olan DB için test double oluşturmayı kolaylaştırmak, [sahte bir framework kullanarak](~/ef6/fundamentals/testing/mocking.md) veya [kendi test double](~/ef6/fundamentals/testing/writing-test-doubles.md).
- **[DbContext artık zaten açık bir DbConnection ile oluşturulabilir](~/ef6/fundamentals/connection-management.md) ** nerede olacağını yararlı bağlantı bağlamı oluştururken açık olup senaryoları etkinleştirir (bileşenler arasındaki bir bağlantıyı paylaşmak gibi burada bağlantının durumunu garanti).
- **[İşlem desteği geliştirildi](~/ef6/saving/transactions.md) ** framework yanı sıra bir işlem içinde Framework oluşturmanın geliştirilmiş yolu dış bir işlem için destek sağlar.
- **Numaralandırmalar, uzamsal ve daha iyi performans .NET 4.0** - taşıyarak duyuyoruz artık .NET üzerinde EF5 gelen numaralandırma desteği, uzamsal veri türleri ve performans iyileştirmeleri sunuyor EF NuGet paketi .NET Framework'e bulunması için kullanılan çekirdek bileşenleri 4.0.
- **Gelişmiş LINQ sorgularında Enumerable.Contains performansını**.
- **Geliştirilmiş Isınma süresini (görünümü oluşturma)**, özellikle büyük modeller için.
- **Takılabilir Çoğullaştırma &amp; Singularization hizmet**.
- **Equals veya GetHashCode özel uygulamalar** varlığı temel sınıfları artık desteklenmektedir.
- **DbSet.AddRange/RemoveRange** eklemek veya birden çok varlık bir kümeden kaldırmak için en iyi duruma getirilmiş bir yol sağlar.
- **DbChangeTracker.HasChanges** veritabanına kaydedilmesi için bekleyen tüm değişiklikleri olup olmadığını görmek için kolay ve etkili bir yol sağlar.
- **SqlCeFunctions** bir SQL Compact SqlFunctions için eşdeğer sağlar.

Aşağıdaki özellikler Code First yalnızca geçerlidir:

- **[Özel kod öncelikli kurallar](~/ef6/modeling/code-first/conventions/custom.md) ** izin yinelenen yapılandırmasını önlemek için kendi kuralları yazma. Daha karmaşık kurallar yazma olanak tanımak için basit bir API basit kuralları ve bunun yanı sıra bazı daha karmaşık bir yapı taşlarını sunuyoruz.
- **[İlk eşleme Insert/Update/Delete saklı yordamlar için kod](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md) ** artık desteklenmektedir.
- **[Idempotent geçiş betikleri](~/ef6/modeling/code-first/migrations/index.md) ** bir veritabanının herhangi bir sürümünü en son sürüme yükseltebilirsiniz bir SQL komut dosyası oluşturmayı sağlar.
- **[Yapılandırılabilir geçişleri geçmiş tablosu](~/ef6/modeling/code-first/migrations/history-customization.md) ** geçişler geçmiş tablosu tanımını özelleştirmenizi sağlar. Bu, özellikle düzgün çalışması geçişler geçmiş tablosu için belirtilecek uygun veri türleri vb. gerektiren veritabanı sağlayıcıları için kullanışlıdır.
- **Veritabanı başına birden fazla bağlamı** modelinin bir Code First Migrations'ı kullanırken, veritabanı başına veya Code First otomatik olarak veritabanı için oluşturduğunuz önceki sınırlamayı kaldırır.
- **[DbModelBuilder.HasDefaultSchema](~/ef6/modeling/code-first/fluent/types-and-properties.md) ** bir yeni kodu ilk varsayılan veritabanı şeması tek bir yerde yapılandırılması için Code First modeli için izin veren API. Daha önce Code First varsayılan şema için sabit kodlanmış &quot;dbo&quot; ve ait olduğu tablo şemasını yapılandırmak için tek yolu ToTable API aracılığıyla.
- **DbModelBuilder.Configurations.AddFromAssembly yöntemi** kod ilk Fluent API'si ile yapılandırma sınıfları kullanırken, bir derlemede tanımlanan tüm yapılandırma sınıfları kolayca eklemenize olanak sağlar.
- **[Özel geçiş işlemleri](http://romiller.com/2013/02/27/ef6-writing-your-own-code-first-migration-operations/) ** kod tabanlı işlemlerinde kullanılmak üzere ek işlemler eklemek etkinleştirilmiş.
- **Varsayılan işlem yalıtım düzeyi için read_commıtted_snapshot değiştirildiğinde** daha fazla ölçeklenebilirlik ve daha az kilitlenmeler için izin verme Code First kullanarak oluşturulan veritabanları için.
- **Varlık ve karmaşık türler artık olabilir nestedinside sınıfları**. |

## <a name="ef-50"></a>EF 5.0
5.0.0 EF çalışma zamanı için NuGet Ağustos 2012'de bırakıldığını.
Bu sürüm, numaralandırma desteği, tablo değerli işlevler, uzamsal veri türleri ve çeşitli performans iyileştirmeleri dahil olmak üzere bazı yeni özellikler sunar.

Entity Framework Designer Visual Studio 2012'de de birden çok diyagramları, her model için destek sunuyor saklı yordamlar tasarım yüzeyi ve toplu içeri aktarma şekillerinin renklendirme.

Özellikle EF 5 sürüm için bir araya içerik listesi aşağıda verilmiştir.

-   [EF 5 yayın sonrası](http://blogs.msdn.com/b/adonet/archive/2012/08/15/ef5-released.aspx)
-   EF5 yeni özellikler
    -   [Sabit listesi kod ilk desteği](~/ef6/modeling/code-first/data-types/enums.md)
    -   [EF Designer enum desteği](~/ef6/modeling/designer/data-types/enums.md)
    -   [Uzamsal veri türleri kodda önce](~/ef6/modeling/code-first/data-types/spatial.md)
    -   [EF Designer uzamsal veri türleri](~/ef6/modeling/designer/data-types/spatial.md)
    -   [Uzamsal türler için sağlayıcı desteği](~/ef6/fundamentals/providers/spatial-support.md)
    -   [Tablo değerli işlevler](~/ef6/modeling/designer/advanced/tvfs.md)
    -   [Model başına birden çok diyagramı](~/ef6/modeling/designer/multiple-diagrams.md)
-   Modelinizi ayarlama
    -   [Model Oluşturma](~/ef6/modeling/index.md)
    -   [Bağlantılar ve modeller](~/ef6/fundamentals/configuring/connection-strings.md)
    -   [Performans Konuları](~/ef6/fundamentals/performance/perf-whitepaper.md)
    -   [Microsoft SQL Azure ile çalışma](~/ef6/fundamentals/connection-resiliency/retry-logic.md)
    -   [Yapılandırma dosyası ayarları](~/ef6/fundamentals/configuring/config-file.md)
    -   [Sözlüğü](~/ef6/resources/glossary.md)
    -   İlk kod
        -   [Yeni bir veritabanına (gözden geçirme ve video) ilk kod](~/ef6/modeling/code-first/workflows/new-database.md)
        -   [Mevcut bir veritabanına (gözden geçirme ve video) ilk kod](~/ef6/modeling/code-first/workflows/existing-database.md)
        -   [Kurallar](~/ef6/modeling/code-first/conventions/built-in.md)
        -   [Veri ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md)
        -   [Özellikleri & türlerini yapılandırma/eşleme Fluent API'si-](~/ef6/modeling/code-first/fluent/types-and-properties.md)
        -   [Fluent API'si - ilişkileri yapılandırma](~/ef6/modeling/code-first/fluent/relationships.md)
        -   [Fluent API'si ile VB.NET](~/ef6/modeling/code-first/fluent/vb.md)
        -   [Code First geçişleri](~/ef6/modeling/code-first/migrations/index.md)
        -   [Otomatik Code First geçişleri](~/ef6/modeling/code-first/migrations/automatic.md)
        -   [Migrate.exe](~/ef6/modeling/code-first/migrations/migrate-exe.md)
        -   [DbSets tanımlama](~/ef6/modeling/code-first/dbsets.md)
    -   EF Designer
        -   [Model ilk (gözden geçirme ve video)](~/ef6/modeling/designer/workflows/model-first.md)
        -   [İlk (gözden geçirme ve video) veritabanı](~/ef6/modeling/designer/workflows/database-first.md)
        -   [Karmaşık türler](~/ef6/modeling/designer/data-types/complex-types.md)
        -   [İlişkilendirmeleri/ilişkileri](~/ef6/modeling/designer/relationships.md)
        -   [TPT devralma deseni](~/ef6/modeling/designer/inheritance/tpt.md)
        -   [TPH devralma deseni](~/ef6/modeling/designer/inheritance/tph.md)
        -   [Saklı yordamlarla sorgu](~/ef6/modeling/designer/stored-procedures/query.md)
        -   [Birden çok sonuç kümesi saklı yordamlar](~/ef6/modeling/designer/advanced/multiple-result-sets.md)
        -   [Ekle, Güncelleştir ve saklı yordamlarla Sil](~/ef6/modeling/designer/stored-procedures/cud.md)
        -   [Bir varlık için birden fazla tabloyu (varlık bölme) eşleme](~/ef6/modeling/designer/entity-splitting.md)
        -   [Birden fazla varlık için bir tablonun (tablo bölme) eşleme](~/ef6/modeling/designer/table-splitting.md)
        -   [Sorguları tanımlama](~/ef6/modeling/designer/advanced/defining-query.md)
        -   [Kod oluşturma şablonları](~/ef6/modeling/designer/codegen/index.md)
        -   [Objectcontext'e döndürülüyor](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)
-   Modelinizi kullanma
    -   [DbContext ile çalışma](~/ef6/fundamentals/working-with-dbcontext.md)
    -   [Varlıkları sorgulama bulma](~/ef6/querying/index.md)
    -   [İlişkilerle çalışma](~/ef6/fundamentals/relationships.md)
    -   [İlgili varlıklar yükleniyor](~/ef6/querying/related-data.md)
    -   [Yerel verilerle çalışma](~/ef6/querying/local-data.md)
    -   [N katmanlı uygulamalar](~/ef6/fundamentals/disconnected-entities/index.md)
    -   [Ham SQL Sorguları](~/ef6/querying/raw-sql.md)
    -   [İyimser eşzamanlılık desenlerinin](~/ef6/saving/concurrency.md)
    -   [Proxy ile çalışma](~/ef6/fundamentals/proxies.md)
    -   [Değişiklikleri otomatik algıla](~/ef6/saving/change-tracking/auto-detect-changes.md)
    -   [Hayır-izleme sorguları](~/ef6/querying/no-tracking.md)
    -   [Load Yöntemi](~/ef6/querying/load-method.md)
    -   [Ekle/ekleme ve varlık durumları](~/ef6/saving/change-tracking/entity-state.md)
    -   [Özellik değerleri ile çalışma](~/ef6/saving/change-tracking/property-values.md)
    -   [Veri bağlama WPF ile (Windows Presentation Foundation)](~/ef6/fundamentals/databinding/wpf.md)
    -   [Veri bağlama ile WinForms (Windows Forms)](~/ef6/fundamentals/databinding/winforms.md)

## <a name="ef-431"></a>EF 4.3.1
4.3.1 EF çalışma zamanı bırakıldığını için NuGet EF 4.3.0 kısa süre sonra Şubat 2012.
Bu düzeltme eki sürümü dahil bazı EF 4.3 yayın yönelik hata düzeltmelerinin ve EF 4.3, Visual Studio 2012 ile kullanan müşteriler için daha iyi LocalDB desteği sunmuştur.

Özellikle 4.3.1 EF yayın için bir araya içerik listesini burada, çoğu EF 4.1 için sağlanan içeriği hala geçerlidir EF 4.3 için de.

-   [EF 4.3.1 sürüm Blog Gönderisi](http://blogs.msdn.com/b/adonet/archive/2012/02/29/ef4-3-1-and-ef5-beta-1-available-on-nuget.aspx)

## <a name="ef-43"></a>EF 4.3
4.3.0 EF çalışma zamanı için NuGet'ın Şubat 2012'de bırakıldığını.
Bu sürüm kodu, Code First modeli geliştikçe artımlı olarak değiştirilmesi ilk tarafından oluşturulmuş bir veritabanı sağlayan yeni bir Code First Migrations özelliği eklenmiştir.

Özellikle EF 4.3 yayını için bir araya içerik listesini İşte, çoğu EF 4.1 için sağlanan içeriği hala geçerlidir EF 4.3 için de:
-   [EF 4.3 yayın sonrası](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-released.aspx)
-   [EF 4.3 kod tabanlı geçişler gözden geçirme](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-code-based-migrations-walkthrough.aspx)
-   [EF 4.3 otomatik geçişleri gözden geçirme](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-automatic-migrations-walkthrough.aspx)

## <a name="ef-42"></a>EF 4.2
4.2.0 EF çalışma zamanı için NuGet'ın Kasım 2011'in bırakıldığını.
Bu sürüm, 4.1.1 EF için hata düzeltmeleri içerir. yayın.
Bu sürüm yalnızca içerdiğinden, hata düzeltmeleri, olduğu EF 4.1.2 düzeltme eki sürümü ancak biz 4.2 için düzeltme eki sürüm numaraları 4.1.x kullandık serbest bırakır ve benimseme tarihi uzağa taşımak bize izin verecek şekilde taşımak için kabul [anlam Versionsing](https://semver.org) semantic Versioning standart.

Özellikle EF 4.2 sürümü için bir araya içerik listesini burada, EF 4.1 için sağlanan içerik yine de EF 4.2 için de geçerlidir.

-   [EF 4.2 yayın sonrası](http://blogs.msdn.com/b/adonet/archive/2011/11/01/ef-4-2-released.aspx)
-   [Kod ilk gözden geçirme](http://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-code-first-walkthrough.aspx)
-   [Model & veritabanı ilk Kılavuzu](http://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-model-amp-database-first-walkthrough.aspx)

## <a name="ef-411"></a>EF 4.1.1
4.1.10715 EF çalışma zamanı için NuGet Temmuz 2011'in bırakıldığını.
Hata düzeltmelerinin yanı sıra bazı bileşenler bir Code First modeli ile çalışmak için tasarım zamanı için kolaylaştırmak için bu düzeltme eki sürümü kullanıma sunuldu.
Bu bileşenler, Code First Migrations (EF 4.3 dahil) ve EF güç araçları tarafından kullanılır.

Fark edeceksiniz garip sürüm 4.1.10715 paket sayısı.
Benimsemeye karar vermeden tarihi düzeltme eki sürümleri kullanılacak kullandık [Semantic Versioning](https://semver.org).
Bu sürümü EF 4.1 düzeltme eki 1 (yani, 4.1.1) düşünebilirsiniz.

İşte biz put birlikte 4.1.1 için içerik listesini sürüm:

-   [EF 4.1.1 yayın sonrası](http://blogs.msdn.com/b/adonet/archive/2011/07/25/ef-4-1-update-1-released.aspx)

## <a name="ef-41"></a>EF 4.1
4.1.10331 EF çalışma zamanı, Nisan 2011'in NuGet üzerinde yayımlanacak ilk oluştu.
Bu sürüm, Basitleştirilmiş DbContext API ve kod ilk iş akışınızı dahil.

Garip sürüm numarasını görürsünüz form veya 4.1.10331 4.1 gerçekten verilmiş olması. Ayrıca bir 4.1.10311 yoktur 4.1.0-rc olmalıydı sürümü ('rc', 'Sürüm Adayı' anlamına gelir).
Benimsemeye karar vermeden tarihi düzeltme eki sürümleri kullanılacak kullandık [Semantic Versioning](https://semver.org).

4.1 sürüm için bir araya içerik listesi aşağıda verilmiştir. Çoğu, Entity Framework daha sonraki sürümleri için hala geçerlidir:

-   [EF 4.1 yayın sonrası](http://blogs.msdn.com/b/adonet/archive/2011/04/11/ef-4-1-released.aspx)
-   [Kod ilk gözden geçirme](http://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-code-first-walkthrough.aspx)
-   [Model & veritabanı ilk Kılavuzu](http://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-model-amp-database-first-walkthrough.aspx)
-   [SQL Azure Federasyonlar ve Entity Framework](http://blogs.msdn.com/b/adonet/archive/2012/01/10/sql-azure-federations-and-the-entity-framework.aspx)

## <a name="ef-40"></a>EF 4.0
Bu sürümde, .NET Framework 4 ve Visual Studio 2010, Nisan 2010'da eklenmiştir.
POCO desteği, yabancı anahtar eşleme, yavaş yükleniyor, Test Edilebilirlik geliştirmeleri, özelleştirilebilir kod üretme ve Model ilk iş akışınızı önemli yeni özellikler bu sürümde dahil.

Entity Framework'ün ikinci sürüm olmasına rağmen EF ile birlikte .NET Framework sürümü ile hizalamak için 4 olarak adlandırılmıştı.
Bu sürümde sonra Entity Framework NuGet üzerinde kullanılabilir hale getirme çalışmaya ve biz artık .NET Framework sürümüne bağlı bu yana semantic versioning benimsenen.

.NET Framework'ün sonraki bazı sürümleri dahil EF BITS yapılan önemli değişiklikler gönderildi unutmayın.
Aslında, birçok yeni özelliği EF 5.0 geliştirmeler üzerinde bu bit olarak uygulanan.
Bununla birlikte, sürüm oluşturma hikaye için EF basitleşir yapmak için tüm yeni sürümlere oluşur halde EF 4.0 çalışma zamanı .NET Framework'ün bir parçası olan EF BITS başvurmak devam ediyoruz [EntityFramework NuGet paketi](https://www.nuget.org/packages/EntityFramework/).         

## <a name="ef-35"></a>EF 3.5
.NET 3.5 Service Pack 1 ve Visual Studio 2008 SP1, Ağustos 2008'de sunulan, Entity Framework'ün ilk sürümünden eklenmiştir.
Bu sürüm, veritabanı ilk iş akışı kullanarak temel O/RM destek sağladı.
