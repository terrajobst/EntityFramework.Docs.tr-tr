---
title: Araçlar & Uzantılar-EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 99f59153a452a2f4aad5811110ebc5b5da7717ef
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888049"
---
# <a name="ef-core-tools--extensions"></a>EF Core araçları & uzantıları

Bu araçlar ve uzantılar, Entity Framework Core 2,1 ve üzeri için ek işlevler sağlar.

> [!IMPORTANT]  
> Uzantılar çeşitli kaynaklardan oluşturulmuştur ve Entity Framework Core projenin bir parçası olarak korunmaz. Üçüncü taraf uzantısını değerlendirirken, gereksinimlerinizi karşıladığından emin olmak için kalite, lisanslama, uyumluluk, destek vb. değerlendirdiğinizden emin olun. Özellikle, EF Core eski bir sürümü için oluşturulmuş bir uzantının en son sürümlerle çalışmadan önce güncelleştirilmesi gerekebilir.

## <a name="tools"></a>Araçları

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro, Entity Framework ve Entity Framework Core desteğiyle bir varlık modelleme çözümüdür. Bu, varlık modelinizi kolayca tanımlamanızı ve veritabanını ilk veya modeli kullanarak veritabanınıza eşlemenizi sağlar, böylece sorguları hemen yazmaya başlayabilirsiniz. EF Core için: 2.

[Web sitesi](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Varlık geliştiriciyi kaldırma

Varlık geliştiricisi, ADO.NET Entity Framework, Nhazırda beklet, LinqConnect, Telerik veri erişimi ve LINQ to SQL için güçlü bir ORM tasarlayıcıdır. EF Core modellerinin görsel olarak tasarlanmasını, ilk veya veritabanı ilk yaklaşımı ve C# ya da kod üretimini Visual Basic destekler. EF Core için: 2.

[Web sitesi](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a>Entity Framework için Nhizte ORM

Entity Framework için türü kesin belirlenmiş, Genişletilebilir sınıflar oluşturan bir ORM. Oluşturulan kod Entity Framework Core. Fark yoktur. Bu, EF veya Custom ORM için bir değiştirme değildir. Ekibin karmaşık veritabanı şemalarını yönetmesine olanak tanıyan bir görsel modelleme katmanıdır. Git gibi SCM yazılımıyla birlikte çalışarak, en az çakışmalarıyla modelinize çoklu Kullanıcı erişimi sağlar. Yükleyici, model değişikliklerini izler ve yükseltme betikleri oluşturur. EF Core için: 3.

[GitHub sitesi](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a>EF Core güç araçları

EF Core güç araçları, basit bir kullanıcı arabiriminde çeşitli EF Core tasarım zamanı görevleri sunan bir Visual Studio uzantısıdır. Bu, var olan veritabanlarından DbContext ve varlık sınıflarının tersine mühendislik ve [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), veritabanı geçişleri yönetimi ve model görselleştirmeleri içerir. EF Core için: 2, 3.

[GitHub wiki](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Entity Framework görsel düzenleyici

Entity Framework Visual Düzenleyicisi, EF 6 ve EF Core sınıflarının görsel tasarımı için bir ORM Tasarımcısı ekleyen bir Visual Studio uzantısıdır. Kod, T4 şablonları kullanılarak oluşturulur, bu nedenle, herhangi bir gereksinimlerinize uyacak şekilde özelleştirilebilir. Devralma, tek yönlü ve çift yönlü ilişkilendirmeler, numaralandırmalar ve sınıflarınızın renk Kodlayabilme ve tasarımınızın potansiyel olarak büyük bir kısmını açıklamak için metin blokları ekleme imkanını destekler. EF Core için: 2.

['Nde](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>Katfactory

CatFactory, bir SQL Server veritabanından DbContext sınıflarının, varlıkların, eşleme yapılandırmalarının ve depo sınıflarının oluşturulmasını otomatik hale getirebilen .NET Core için bir yapı iskelesi altyapısıdır. EF Core için: 2.

[GitHub deposu](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>LoreSoft 'ın Entity Framework Core Oluşturucu

Entity Framework Core Generator (EFG), `dotnet ef dbcontext scaffold`gibi var olan bir veritabanından EF Core modeller oluşturabilen, ancak bölge değişikliği aracılığıyla veya eşleme dosyalarını ayrıştırarak güvenli kod yeniden [oluşturmayı](https://efg.loresoft.com/en/latest/regeneration/) da destekleyen bir .NET Core CLI aracıdır. Bu araç, görünüm modellerinin, doğrulamanın ve nesne Eşleyici kodunun oluşturulmasını destekler. EF Core için: 2.

[Eğitim](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[belgeleri](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Uzantıları

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft. EntityFrameworkCore. oto geçmişi

EF Core tarafından gerçekleştirilen veri değişikliklerinin bir geçmiş tablosuna otomatik olarak kaydedilmesini sağlayan bir eklenti kitaplığı. EF Core için: 2.

[GitHub deposu](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache. Core

Aynı sorguların sonraki yürütmelerinin veritabanına erişmeyi ve verileri doğrudan önbellekten almasını önlemek için, EF Core sorgularının sonuçlarının ikinci düzey bir önbellekte depolanmasını sağlayan bir uzantı. EF Core için: 2.

[GitHub deposu](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a>Geco

Geco (Oluşturucu konsolu), .NET Core üzerinde çalışan ve kod oluşturma için enterpolasyonlu dizeler kullanan C# bir konsol projesine dayalı basit bir kod Oluşturucu. Geco, plurmaya, sinleştirme ve düzenlenebilir şablonlara yönelik desteğe sahip EF Core için bir ters model Oluşturucu içerir. Ayrıca, bir çekirdek veri betik Oluşturucu, bir komut dosyası Çalıştırıcısı ve bir veritabanı temizleyicisi de sağlar. EF Core için: 2.

[GitHub deposu](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a>EntityFrameworkCore. Scafkatlama. Handleçubuklar 

Handleçubuklar şablonlarıyla Entity Framework Core toolzincirini kullanarak var olan bir veritabanından ters mühendislik uygulanmış sınıfların özelleştirilmesine izin verir. EF Core için: 2, 3.

[GitHub deposu](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a>Newınlınq. EntityFrameworkCore 

Neınlinq, işlevleri yeniden kullanmayı, sorguları yeniden yazmayı ve çevrilebilir koşullara ve seçicileri kullanarak dinamik sorgular oluşturmayı etkinleştirmek için Entity Framework gibi LINQ sağlayıcılarını genişletir. EF Core için: 2, 3.

[GitHub deposu](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft. EntityFrameworkCore. UnitOfWork

Microsoft. EntityFrameworkCore 'un depoyu, iş biçimlerini ve dağıtılmış işlem desteklenen birden çok veritabanını desteklemesi için bir eklentisi. EF Core için: 2.

[GitHub deposu](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore. BulkExtensions

Toplu işlemler için EF Core uzantıları (INSERT, Update, Delete). EF Core için: 2, 3.

[GitHub deposu](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Brıcelam. EntityFrameworkCore. Pluralizer

Tasarım zamanı plurlaması ekler. EF Core için: 2.

[GitHub deposu](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbandı. EntityFrameworkCore. ındexattribute

[Index] özniteliğinin (model oluşturma uzantısı ile) bir önceki örneği. EF Core için: 2, 3.

[GitHub deposu](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore. ınmemoryyardımcıları

Bellek Içi veritabanı sağlayıcısı EF Core etrafında bir sarmalayıcı sağlar. İlişkisel bir sağlayıcı gibi çalışır hale getirir. EF Core için: 2.

[GitHub deposu](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore. TemporalSupport

Zamana bağlı destek uygulama. EF Core için: 2.

[GitHub deposu](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a>EfCoreTemporalTable

Sunulan uzantı yöntemlerini kullanarak, sık kullanılan veritabanınızda kolayca zamana bağlı sorgular gerçekleştirin: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`. EF Core için: 3.

[GitHub deposu](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a>EFCore. TimeTraveler

Zaten tanımlamış olduğunuz EF Core kodu, varlıkları ve eşleştirmeleri kullanarak, isteğe bağlı [SQL Server geçmişle](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) karşı tam özellikli Entity Framework Core sorgulara izin verin.  Kodunuzu `using (TemporalQuery.AsOf(targetDateTime)) {...}`içine sarmalayarak zaman içinde hareket edin. EF Core için: 3.

[GitHub deposu](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a>EntityFrameworkCore. TemporalTables

SQL Server kullanan geliştiricilerin kolayca zamana bağlı tabloları kullanmasına izin veren Entity Framework Core için uzantı kitaplığı. EF Core için: 2.

[GitHub deposu](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore. önbelleklenebilir

Yüksek performanslı ikinci düzey bir sorgu önbelleği. EF Core için: 2.

[GitHub deposu](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Entity Framework Plus

DbContext dosyanızı, şunlar gibi özelliklerle genişletir: filtre, denetim, önbelleğe alma, sorgu gelecekteki, toplu silme, toplu güncelleştirme ve daha fazlasını Içerir. EF Core için: 2, 3.

[Web sitesi](https://entityframework-plus.net/)
[GitHub deposu](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Entity Framework uzantıları

DbContext uygulamanızı yüksek performanslı toplu işlemlerle genişletir: BulkSaveChanges, Bulkınsert, BulkUpdate, BulkDelete, BulkMerge ve daha fazlası. EF Core için: 2, 3.

[Web sitesi](https://entityframework-extensions.net/)

### <a name="expressionify"></a>Expressionbelirt

Uzantı yöntemlerini LINQ Lambdalar içinde çağırma desteği ekleyin. EF Core için: 3,1

[GitHub deposu](https://github.com/ClaveConsulting/Expressionify)
