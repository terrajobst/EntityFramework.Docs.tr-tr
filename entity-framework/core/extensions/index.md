---
title: Araçlar ve uzantılar - EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 414773284df7c208b9a2acf0536fda459bdf775b
ms.sourcegitcommit: 7bde8e6ad3c4565a4638646ce04bcf5e66f7b5fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54069236"
---
# <a name="ef-core-tools--extensions"></a>EF Core Araçlar ve uzantılar

Bu araçlar ve uzantılar için Entity Framework Core 2.0 ve daha sonra ek işlevsellik sağlar.

> [!IMPORTANT]  
> Uzantılar, çeşitli kaynaklardan tarafından oluşturulur ve Entity Framework Core projesinin bir parçası korunmaz. Bir üçüncü taraf uzantı dikkate alındığında, lisanslama, uyumluluk, destek, gereksinimlerinizi karşıladığından emin olmak için vb. kalitesini değerlendirmek emin olun.

## <a name="tools"></a>Araçlar

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro çözüm Entity Framework ve Entity Framework Core desteğiyle birlikte modeling bir varlıktır. Kolayca varlık modelinizi tanımlayın ve veritabanınıza ilk kullanarak veritabanı eşleme sağlar veya sorgu hemen yazma başlangıç yapmanıza yardımcı olacak ilk olarak, model.

[Web sitesi](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart varlık Geliştirici

Varlık, ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access ve LINQ to SQL için güçlü bir ORM Tasarımcısı geliştiricidir. Bu görsel olarak EF Core modelleri tasarlama destekler, ilk modelini kullanarak veya veritabanı ilk yaklaşır, ve C# veya Visual Basic kod oluşturma. 

[Web sitesi](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core güç araçları

EF Core Power Tools çeşitli EF Core tasarım zamanı görevleri basit bir kullanıcı arabirimi kullanıma sunan bir Visual Studio 2017 uzantısıdır. DbContext ve varlık sınıflarını mevcut veritabanlarının tersine mühendisliğini içerir ve [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), veritabanı geçişlerini ve model görselleştirmeler yönetimi.

[GitHub wiki](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Entity Framework Visual Düzenleyicisi

Entity Framework Visual Düzenleyicisi bir görsel tasarım EF 6 ve EF Core sınıfları için ORM Tasarımcısı ekleyen bir Visual Studio 2017 uzantısıdır. Kod, her türlü ihtiyacı karşılamak için özelleştirilebilir için T4 şablonlarını kullanarak oluşturulur. Bu, devralma, tek yönlü ve çift yönlü ilişkilendirmeleri, numaralandırmalar ve sınıflarınızı renk kodu ve metin blokları ekleme olanağı tasarımınızı potansiyel olarak karıştıran bölümlerini açıklamak için destekler.

[Market](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory DbContext sınıfları, varlıklar, eşleme yapılandırmalarını ve bir SQL Server veritabanı sınıflarından depo oluşturulmasını otomatik hale getirebilirsiniz .NET Core için yapı iskelesi altyapısıdır.

[GitHub deposu](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>LoreSoft'ın Entity Framework Core Oluşturucusu

Entity Framework Core Generator (efg), EF Core modelleri çok gibi varolan bir veritabanından oluşturabilen bir .NET Core CLI aracı `dotnet ef dbcontext scaffold`, ancak güvenli kod da destekler [anahtarınızın yeniden oluşturulması](https://efg.loresoft.com/en/latest/regeneration/) bölge değiştirme veya aracılığıyla ayrıştırma eşleme dosyaları. Bu araç, görünüm modelleri oluşturma ve doğrulama nesne Eşleyici kodu destekler. 

[Öğretici](http://www.loresoft.com/Generate-ASP-NET-Web-API)
[belgeleri](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Uzantıları

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Bir eklenti kitaplık, EF Core tarafından gerçekleştirilen bir geçmiş tablosuna veri değişikliklerini kaydı otomatik olarak etkinleştirir.

[GitHub deposu](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

.NET Core / .NET standart bağlantı noktası, EF Core ile zaman uyumsuz desteği içeren System.Linq.Dynamic.
System.Linq.Dynamic dinamik olarak gelen kod yerine dize ifadeleri LINQ sorgularının nasıl oluşturulacağını gösteren bir Microsoft örnek olarak oluşturulur.

[GitHub deposu](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Aynı sorgu sonraki yürütmeleri veritabanına erişirken önlemek ve doğrudan önbellekten veri almak, ikinci düzey önbelleğine, EF Core sorguların sonuçlarını depolamak sağlayan uzantısı.

[GitHub deposu](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Bu kitaplık (bileşik anahtarlar dahil) birincil anahtar değerlerini alınırken bir sözlük olarak herhangi bir varlık izin verir.

[GitHub deposu](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Bu kitaplık, varlık özelliklerini öğesinin özgün değerleri kesin türü belirtilmiş erişim sağlar. 

[GitHub deposu](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco (Oluşturucusu Konsolu) olan .NET Core üzerinde çalışır ve kullanan bir konsol projesi dayalı bir basit Kod Oluşturucu C# Ara değerli dizeler için kod oluşturma. Geco ters model Oluşturucu EF Core için çoğullaştırma singularization ve düzenlenebilir şablonlar için destek içerir. Ayrıca bir çekirdek veri betiği Oluşturucu, bir betik Çalıştırıcı ve bir veritabanı Temizleyicisi sağlar.

[GitHub deposu](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore bir LINQKit kitaplığı EF Core ile uyumlu sürümüdür. LINQKit LINQ için uzantıları SQL ve Entity Framework ileri kullanıcılar için ücretsiz bir kümesidir. Bu, dinamik koşul ifadeleri oluşturmaya ve sorgularda ifade değişkenleri kullanma gibi gelişmiş işlevleri sağlar.  

[GitHub deposu](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq yeniden kullanma işlevleri etkinleştirmek için Entity Framework gibi sorguları yeniden yazmak ve çevrilebilir doğrulamaları ve Seçici kullanarak dinamik sorgular oluşturma LINQ sağlayıcıları genişletir.

[GitHub deposu](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Havuz, iş birimi desenleri ve birden çok veritabanı ile birlikte dağıtılmış işlem desteklenen desteklemek Microsoft.EntityFrameworkCore bir eklenti.

[GitHub deposu](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

EF Core uzantıları toplu işlemleri (INSERT, Update, Delete).

[GitHub deposu](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

EF Core için tasarım zamanı çoğullaştırma ekler.

[GitHub deposu](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a>PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql

Basit senaryoda belirli bir LINQ Sorgu için SQL deyimi EF Core alır bir basit bir genişletme yöntemi oluşturur. EF Core tek bir LINQ Sorgu için birden fazla SQL deyimi ve parametre değerlerini bağlı olarak farklı SQL deyimleri oluşturabileceğinden ToSql yöntemi basit senaryolar için sınırlıdır.

[GitHub deposu](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

(Uzantılı modeli oluşturmak için) EF Core için revival [dizin] özniteliği.

[GitHub deposu](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

EF Core bellek içi veritabanı sağlayıcısı çevresinde bir sarmalayıcı sağlar. Daha fazla ilişkisel bir sağlayıcı gibi davranmasını sağlar.

[GitHub deposu](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

EF Core için zamana bağlı desteği uygulaması.

[GitHub deposu](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

EF Core için yüksek performanslı ikinci düzey sorgu önbelleği.

[GitHub deposu](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)
