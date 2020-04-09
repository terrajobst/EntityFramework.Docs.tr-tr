---
title: Araçlar & Uzantıları - EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e3806f7161fecfe66450d3e08f97caf3d2c84cf3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634233"
---
# <a name="ef-core-tools--extensions"></a>EF Temel Araçlar & Uzantıları

Bu araçlar ve uzantılar, Entity Framework Core 2.1 ve sonraki etkinlikler için ek işlevler sağlar.

> [!IMPORTANT]  
> Uzantılar çeşitli kaynaklar tarafından oluşturulur ve Entity Framework Core projesinin bir parçası olarak sürdürülemez. Bir üçüncü taraf uzantısı nı düşünürken, gereksinimlerinizi karşıladığından emin olmak için kalitesini, lisansını, uyumluluğunu, desteğini vb. değerlendirdiğinizden emin olun. Özellikle, EF Core'un eski bir sürümü için oluşturulmuş bir uzantının en son sürümlerle çalışmadan önce güncelleştirilmesi gerekebilir.

## <a name="tools"></a>Araçlar

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro, Entity Framework ve Entity Framework Core desteğine sahip bir varlık modelleme çözümüdür. Önce veritabanını veya modeli kullanarak varlık modelinizi kolayca tanımlamanızı ve veritabanınızla eşlenizi belirlemenizi sağlar, böylece sorguları hemen yazmaya başlarsınız. EF Çekirdek için: 2.

[Web sitesi](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer, ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access ve LINQ to SQL için güçlü bir ORM tasarımcısıdır. EF Core modellerinin görsel olarak tasarlanması, önce model veya veritabanı yaklaşımlarının kullanılması ve C# veya Visual Basic kod oluşturmayı destekler. EF Çekirdek için: 2.

[Web sitesi](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a>Varlık Çerçevesi için nHydrate ORM

Varlık Çerçevesi için güçlü bir şekilde yazılmış, genişletilebilir sınıflar oluşturan bir ORM. Oluşturulan kod Varlık Framework Core'dur. Aralarında herhangi bir fark yoktur. Bu, EF veya özel bir ORM'un yerini almayacak. Bir takımın karmaşık veritabanı şemalarını yönetmesine olanak tanıyan görsel, modelleme katmanıdır. Git gibi SCM yazılımlarında iyi çalışır ve en az çakışma ile modelinize çok kullanıcılı erişim sağlar. Yükleyici model değişikliklerini izler ve yükseltme komut dosyaları oluşturur. EF Çekirdek için: 3.

[Github sitesi](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a>EF Çekirdek Elektrikli El Aletleri

EF Core Power Tools, çeşitli EF Core tasarım zamanı görevlerini basit bir kullanıcı arabiriminde ortaya çıkaran bir Visual Studio uzantısıdır. Varolan veritabanlarından ve [SQL Server DACPACs'dan](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications)DbContext ve varlık sınıflarının ters mühendislik, veritabanı geçişlerinin yönetimi ve model görselleştirmelerini içerir. EF Çekirdek için: 2, 3.

[GitHub wiki](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Entity Framework Görsel Editör

Entity Framework Visual Editor, EF 6'nın görsel tasarımı için bir ORM tasarımcısı ve EF Core sınıfları ekleyen bir Visual Studio uzantısıdır. Kod, t4 şablonları kullanılarak oluşturulur, böylece her ihtiyaca uyacak şekilde özelleştirilebilir. Devralma, tek yönlü ve çift yönlü çağrışımları, sayısallaştırmaları ve sınıflarınızı renk kodlama ve tasarımınızın olası esrarengiz kısımlarını açıklamak için metin blokları ekleme yeteneğini destekler. EF Çekirdek için: 2.

[Mağaza](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory, .NET Core için, DbContext sınıflarının, varlıkların, eşleme yapılandırmalarının ve depo sınıflarının bir SQL Server veritabanından üretimini otomatikleştirebilen bir iskele motorudur. EF Çekirdek için: 2.

[GitHub deposu](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>LoreSoft'un Varlık Çerçeve Çekirdek Jeneratörü

Entity Framework Core Generator (efg), varolan bir veritabanından EF Core modelleri oluşturabilen bir .NET Core CLI aracıdır, `dotnet ef dbcontext scaffold`ancak bölge değiştirme veya haritalama dosyalarını ayrıştırarak güvenli kod [yenilenmesini](https://efg.loresoft.com/en/latest/regeneration/) de destekler. Bu araç, görünüm modelleri, doğrulama ve nesne mapper kodu oluşturmayı destekler. EF Çekirdek için: 2.

[Öğretici](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Dokümantasyon](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Uzantıları

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

EF Core tarafından gerçekleştirilen veri değişikliklerinin tarih tablosuna otomatik olarak kaydedilmesini sağlayan bir eklenti kitaplığı. EF Çekirdek için: 2.

[GitHub deposu](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Aynı sorguların sonraki yürütmelerinin veritabanına erişmesini önleyebilmesi ve verileri doğrudan önbellekten alabilmeleri için EF Core sorgularının sonuçlarını ikinci düzey bir önbelleğe depolamayı sağlayan uzantı. EF Çekirdek için: 2.

[GitHub deposu](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a>Geco

Geco (Jeneratör Konsolu) .NET Core üzerinde çalışan ve kod oluşturma için C# enterpolasyonlu dizeleri kullanan bir konsol projesine dayalı basit bir kod üretecidir. Geco, EF Core için çoğullaştırma, tekilleştirme ve değiştirilebilir şablonlar için desteklenen bir ters model jeneratörü içerir. Ayrıca bir tohum veri komut dosyası jeneratör, bir komut dosyası koşucusu ve bir veritabanı temizleyici sağlar. EF Çekirdek için: 2.

[GitHub deposu](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a>EntityFrameworkCore.Scaffolding.Handlebars 

Handlebars şablonları ile Entity Framework Core araç zincirini kullanarak varolan bir veritabanından yeniden tasarlanmış sınıfların özelleştirmesine olanak tanır. EF Çekirdek için: 2, 3.

[GitHub deposu](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore 

NeinLinq, işlevleri yeniden kullanma, sorguları yeniden yazma ve translatable yüklemleri ve seçiciler kullanarak dinamik sorgular oluşturmayı etkinleştirmek için Entity Framework gibi LINQ sağlayıcılarını genişletir. EF Çekirdek için: 2, 3.

[GitHub deposu](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Microsoft.EntityFrameworkCore için depo, iş desenleri birimi ve dağıtılmış işlem desteklenen birden çok veritabanını destekleyen bir eklenti. EF Çekirdek için: 2.

[GitHub deposu](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Toplu işlemler için EF Core uzantıları (Ekleme, Güncelleme, Silme). EF Çekirdek için: 2, 3.

[GitHub deposu](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Tasarım zamanı çoğullaştırma ekler. EF Çekirdek için: 2.

[GitHub deposu](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

[Index] özniteliğinin canlandırılması (model oluşturma uzantısı ile). EF Çekirdek için: 2, 3.

[GitHub deposu](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

EF Core In-Memory Veritabanı Sağlayıcısı etrafında bir sarmalayıcı sağlar. Daha çok ilişkisel bir sağlayıcı gibi davranması sağlar. EF Çekirdek için: 2.

[GitHub deposu](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Zamansal desteğin uygulanması. EF Çekirdek için: 2.

[GitHub deposu](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a>EfCoreTemporalTable

Tanıtılan uzantı yöntemlerini kullanarak favori veritabanınızda `AsTemporalAll()` `AsTemporalAsOf(date)`zamansal `AsTemporalBetween(startDate, endDate)` `AsTemporalContained(startDate, endDate)`sorguları kolayca gerçekleştirin: , , `AsTemporalFrom(startDate, endDate)`, . EF Çekirdek için: 3.

[GitHub deposu](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a>EFCore.TimeTraveler

Daha önce tanımlamış olduğunuz EF Core kodunu, varlıkları ve eşleşmelerini kullanarak [SQL Server Geçici Geçmişi'](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) ne karşı tam özellikli Entity Framework Core sorgularına izin verin.  Kodunuzu ' a `using (TemporalQuery.AsOf(targetDateTime)) {...}`sarmalayarak zamanda yolculuk EF Çekirdek için: 3.

[GitHub deposu](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a>EntityFrameworkCore.TemporalTables

SQL Server kullanan geliştiricilerin zamansal tabloları kolayca kullanmasına olanak tanıyan Entity Framework Core uzantılı kitaplığı. EF Çekirdek için: 2.

[GitHub deposu](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Önbelleğe

Yüksek performanslı ikinci düzey sorgu önbelleği. EF Çekirdek için: 2.

[GitHub deposu](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Varlık Çerçevesi Plus

DbContext'ınızı Şu gibi özelliklerle genişletir: Filtre Ekle, Denetim, Önbelleğe Alma, Sorgula, Gelecek Sorgula, Toplu İş Silme, Toplu Güncelleme ve daha fazlası. EF Çekirdek için: 2, 3.

[Web sitesi](https://entityframework-plus.net/)
[GitHub deposu](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Varlık Çerçeve Uzantıları

Yüksek performanslı toplu işlemlerle DbContext'ınızı genişletir: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge ve daha fazlası. EF Çekirdek için: 2, 3.

[Web sitesi](https://entityframework-extensions.net/)

### <a name="expressionify"></a>İfade

Linq lambdas arama uzatma yöntemleri için destek ekleyin. EF Çekirdek için: 3.1

[GitHub deposu](https://github.com/ClaveConsulting/Expressionify)

### <a name="xlinq"></a>XLinq

İlişkisel veritabanları için Dil Tümleşik Sorgu (LINQ) teknolojisi. Güçlü bir şekilde yazılan sorguları yazmak için C# kullanmanızı sağlar. EF Çekirdek için: 3.1

- Sorgu oluşturma için tam C# desteği: lambda içinde birden fazla ifade, değişkenler, fonksiyonlar, vb.
- SQL ile anlamsal boşluk yok. XLinq, xLinq, `SELECT` `FROM`xandiman, tip güvenliği ve yeniden düzenleme ile tanıdık sözdizimini birleştirerek SQL deyimlerini (, , `WHERE`) birinci sınıf C# yöntemleri olarak bildirir.

Sonuç olarak SQL, API'sini yerel olarak ortaya çıkaran "başka" sınıf kitaplığı olur, tam anlamıyla *"Dil Tümleşik SQL"* olur.

[Web sitesi](http://xlinq.live/)
