---
title: Araçlar & Uzantılar-EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e70011b42818e4df1ec5b9b88d7adb9d36bb26f1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654795"
---
# <a name="ef-core-tools--extensions"></a>EF Core araçları & uzantıları

Bu araçlar ve uzantılar, Entity Framework Core 2,0 ve üzeri için ek işlevler sağlar.

> [!IMPORTANT]  
> Uzantılar çeşitli kaynaklardan oluşturulmuştur ve Entity Framework Core projenin bir parçası olarak korunmaz. Üçüncü taraf uzantısını değerlendirirken, gereksinimlerinizi karşıladığından emin olmak için kalite, lisanslama, uyumluluk, destek vb. değerlendirdiğinizden emin olun.

## <a name="tools"></a>Araçlar

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro, Entity Framework ve Entity Framework Core desteğiyle bir varlık modelleme çözümüdür. Bu, varlık modelinizi kolayca tanımlamanızı ve veritabanını ilk veya modeli kullanarak veritabanınıza eşlemenizi sağlar, böylece sorguları hemen yazmaya başlayabilirsiniz.

[Web sitesi](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Varlık geliştiriciyi kaldırma

Varlık geliştiricisi, ADO.NET Entity Framework, Nhazırda beklet, LinqConnect, Telerik veri erişimi ve LINQ to SQL için güçlü bir ORM tasarlayıcıdır. EF Core modellerinin görsel olarak tasarlanmasını, ilk veya veritabanı ilk yaklaşımı ve C# ya da kod üretimini Visual Basic destekler.

[Web sitesi](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core güç araçları

EF Core güç araçları, basit bir kullanıcı arabiriminde çeşitli EF Core tasarım zamanı görevleri sunan bir Visual Studio 2017 uzantısıdır. Bu, var olan veritabanlarından DbContext ve varlık sınıflarının tersine mühendislik ve [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), veritabanı geçişleri yönetimi ve model görselleştirmeleri içerir.

[GitHub wiki](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Entity Framework görsel düzenleyici

Entity Framework Visual Düzenleyicisi, EF 6 ve EF Core sınıflarının görsel tasarımı için bir ORM Tasarımcısı ekleyen bir Visual Studio uzantısıdır. Kod, T4 şablonları kullanılarak oluşturulur, bu nedenle, herhangi bir gereksinimlerinize uyacak şekilde özelleştirilebilir. Devralma, tek yönlü ve çift yönlü ilişkilendirmeler, numaralandırmalar ve sınıflarınızın renk Kodlayabilme ve tasarımınızın potansiyel olarak büyük bir kısmını açıklamak için metin blokları ekleme imkanını destekler.

['Nde](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>Katfactory

CatFactory, bir SQL Server veritabanından DbContext sınıflarının, varlıkların, eşleme yapılandırmalarının ve depo sınıflarının oluşturulmasını otomatik hale getirebilen .NET Core için bir yapı iskelesi altyapısıdır.

[GitHub deposu](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>LoreSoft 'ın Entity Framework Core Oluşturucu

Entity Framework Core Generator (EFG), `dotnet ef dbcontext scaffold`gibi var olan bir veritabanından EF Core modeller oluşturabilen, ancak bölge değişikliği aracılığıyla veya eşleme dosyalarını ayrıştırarak güvenli kod yeniden [oluşturmayı](https://efg.loresoft.com/en/latest/regeneration/) da destekleyen bir .NET Core CLI aracıdır. Bu araç, görünüm modellerinin, doğrulamanın ve nesne Eşleyici kodunun oluşturulmasını destekler.

[Eğitim](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[belgeleri](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Uzantılar

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft. EntityFrameworkCore. oto geçmişi

EF Core tarafından gerçekleştirilen veri değişikliklerinin bir geçmiş tablosuna otomatik olarak kaydedilmesini sağlayan bir eklenti kitaplığı.

[GitHub deposu](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft. EntityFrameworkCore. Dynamiclınq

EF Core ile zaman uyumsuz destek içeren System. LINQ. Dynamic .NET Core/.NET Standard bağlantı noktasıdır.
System. LINQ. Dynamic, kod yerine dize ifadelerinden dinamik olarak LINQ sorgularının nasıl oluşturulacağını gösteren bir Microsoft örneği olarak kaynaklı.

[GitHub deposu](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache. Core

Aynı sorguların sonraki yürütmelerinin veritabanına erişmeyi ve verileri doğrudan önbellekten almasını önlemek için, EF Core sorgularının sonuçlarının ikinci düzey bir önbellekte depolanmasını sağlayan bir uzantı.

[GitHub deposu](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore. PrimaryKey

Bu kitaplık, birincil anahtar (bileşik anahtarlar dahil) değerlerinin bir sözlük olarak herhangi bir varlıktan alınmasına izin verir.

[GitHub deposu](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore. TypedOriginalValues

Bu kitaplık, varlık özelliklerinin orijinal değerlerine kesin olarak belirlenmiş erişimi sağlar.

[GitHub deposu](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco (Oluşturucu konsolu), .NET Core üzerinde çalışan ve kod oluşturma için enterpolasyonlu dizeler kullanan C# bir konsol projesine dayalı basit bir kod Oluşturucu. Geco, plurmaya, sinleştirme ve düzenlenebilir şablonlara yönelik desteğe sahip EF Core için bir ters model Oluşturucu içerir. Ayrıca, bir çekirdek veri betik Oluşturucu, bir komut dosyası Çalıştırıcısı ve bir veritabanı temizleyicisi de sağlar.

[GitHub deposu](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit. Microsoft. EntityFrameworkCore

LinqKit. Microsoft. EntityFrameworkCore, LINQKit kitaplığının EF Core uyumlu bir sürümüdür. LINQKit, LINQ to SQL ve Power Users Entity Framework için ücretsiz bir uzantılar kümesidir. Koşul ifadelerinin dinamik olarak oluşturulmasına ve alt sorgularda ifade değişkenlerinin kullanılmasına benzer gelişmiş işlevlere izin vermez.  

[GitHub deposu](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>Newınlınq. EntityFrameworkCore

Neınlinq, işlevleri yeniden kullanmayı, sorguları yeniden yazmayı ve çevrilebilir koşullara ve seçicileri kullanarak dinamik sorgular oluşturmayı etkinleştirmek için Entity Framework gibi LINQ sağlayıcılarını genişletir.

[GitHub deposu](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft. EntityFrameworkCore. UnitOfWork

Microsoft. EntityFrameworkCore 'un depoyu, iş biçimlerini ve dağıtılmış işlem desteklenen birden çok veritabanını desteklemesi için bir eklentisi.

[GitHub deposu](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore. BulkExtensions

Toplu işlemler için EF Core uzantıları (INSERT, Update, Delete).

[GitHub deposu](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Brıcelam. EntityFrameworkCore. Pluralizer

EF Core için tasarım zamanı plurun ekler.

[GitHub deposu](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a>Pomelofons/Pomelo. EntityFrameworkCore. Extensions. ToSql

Basit senaryolarda belirli bir LINQ sorgusu için oluşturulacak EF Core SQL ifadesini alan basit bir genişletme yöntemi. EF Core, tek bir LINQ sorgusu için birden fazla SQL deyimi ve parametre değerlerine bağlı olarak farklı SQL deyimlerini oluşturabileceği için ToSql yöntemi basit senaryolarla sınırlıdır.

[GitHub deposu](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbandı. EntityFrameworkCore. ındexattribute

EF Core (model oluşturma için uzantılı) için [Index] özniteliğinin ' den önceki değer.

[GitHub deposu](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore. ınmemoryyardımcıları

Bellek Içi veritabanı sağlayıcısı EF Core etrafında bir sarmalayıcı sağlar. İlişkisel bir sağlayıcı gibi çalışır hale getirir.

[GitHub deposu](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore. TemporalSupport

EF Core için zamana bağlı destek bir uygulama.

[GitHub deposu](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore. önbelleklenebilir

EF Core için yüksek performanslı ikinci düzey sorgu önbelleği.

[GitHub deposu](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Entity Framework Plus

DbContext dosyanızı, şunlar gibi özelliklerle genişletir: filtre, denetim, önbelleğe alma, sorgu gelecekteki, toplu silme, toplu güncelleştirme ve daha fazlasını Içerir.

[Web sitesi](https://entityframework-plus.net/)
[GitHub deposu](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Entity Framework uzantıları

DbContext uygulamanızı yüksek performanslı toplu işlemlerle genişletir: BulkSaveChanges, Bulkınsert, BulkUpdate, BulkDelete, BulkMerge ve daha fazlası.

[Web sitesi](https://entityframework-extensions.net/)

### <a name="reconciler"></a>Bağdaştıriler

İlgili varlıkları ekleyerek, güncelleştirerek ve kaldırarak depodaki bir varlık grafiğini verilen bir tek güncelleştirme.

[GitHub deposu](https://github.com/jtheisen/reconciler)
