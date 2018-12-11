---
title: Araçlar ve uzantılar - EF Core
author: ErikEJ
ms.date: 07/03/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 67eae6cb943b974cc9cd581b8054836d2e37b1e9
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53182000"
---
# <a name="ef-core-tools--extensions"></a>EF Core Araçlar ve uzantılar

Araçlar ve uzantılar için Entity Framework Core ek işlevsellik sağlar.

> [!IMPORTANT]  
> Uzantıları çeşitli kaynaklardan tarafından oluşturulur ve Entity Framework Core projesinin bir parçası korunur değil. Bir üçüncü taraf uzantı dikkate alındığında, kalite, lisanslama, uyumluluk, destek vb. gereksinimlerinizi karşıladıklarından emin olmak için değerlendirilecek emin olun.

## <a name="tools"></a>Araçlar

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro çözüm Entity Framework ve Entity Framework Core desteğiyle birlikte modeling bir varlıktır. Kolayca varlık modelinizi tanımlayın ve veritabanınıza ilk kullanarak veritabanı eşleme sağlar veya sorgu hemen yazma başlangıç yapmanıza yardımcı olacak ilk olarak, model.

[Web sitesi](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart varlık Geliştirici

Varlık, ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access ve LINQ to SQL için güçlü bir ORM Tasarımcısı geliştiricidir. Model-First kullanabilirsiniz ve ORM modelinizi tasarlamak ve C# veya Visual Basic .NET kodunu oluşturmak için veritabanı ilk yaklaşır. ORM modelleri oluşturmaya yönelik yeni bir yaklaşım sunar, üretkenliği artırıyor ve veritabanı uygulamaların geliştirilmesini kolaylaştırır.

[Web sitesi](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core güç araçları

Visual Studio 2017 + uzantısı. Varolan bir veritabanına veya SQL Server veritabanı projesi tersine mühendislik DbContext ve POCO sınıfların görselleştirin ve çeşitli yollarla, DbContext inceleyin.

[GitHub wiki](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

### <a name="entity-framework-visual-editor"></a>Entity Framework Visual Düzenleyicisi

Bir görsel tasarım sınıfların, Entity Framework 6, Core 2.0 ve Core 2.1 için ORM Tasarımcısı ekleyen bir Visual Studio 2017 uzantısı. Kod, her türlü ihtiyacı karşılamak için T4 şablonları tamamen özelleştirilebilir şekilde kullanarak oluşturulur. Numaralandırmalar ve sınıflarınızı renk kodu ve metin blokları tasarımınızı potansiyel olarak karıştıran bölümlerini açıklamak için ekleme olanağı gibi devralma, tek yönlü ve çift yönlü ilişkilendirmeleri tümü, desteklenir.

[Market](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory bir .NET Core ve Entity Framework Core için yapı iskelesi altyapısıdır. Mevcut bir veritabanını SQL Server örneğinden'a ve ardından veritabanı için modeller gösteriminde ile dışarı aktarma olduğu kavramı CatFactory arkasında; iskele varlıklar, yapılandırmalar, depolar ve daha fazlası.

[GitHub deposu](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>LoreSoft'ın Entity Framework Core Oluşturucusu

Entity Framework Core Generator (efg), EF Core modelleri çok gibi varolan bir veritabanından oluşturabilen bir .NET Core CLI aracı `dotnet ef dbcontext scaffold`. Ayrıca güvenli kod destekler, ancak farklı olduğu [anahtarınızın yeniden oluşturulması](https://efg.loresoft.com/en/latest/regeneration/). Anahtarınızın yeniden oluşturulması, bölge değiştirme aracılığıyla ya da eşleme dosyaları ayrıştırma gerçekleştirilir. Araç ayrıca oluşturma görünüm modelleri, doğrulama ve nesne Eşleyici kodu destekler. Daha fazla bilgi için öğretici ve ürün belge bağlantılarına bakın.

[Öğretici](http://www.loresoft.com/Generate-ASP-NET-Web-API)
[belgeleri](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Uzantıları

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Bir eklenti için otomatik olarak veri kaydını desteklemek Microsoft.EntityFrameworkCore geçmişi değiştirir.

[GitHub deposu](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Zaman uyumsuz destek ekleyen Microsoft.EntityFrameworkCore için dinamik LINQ uzantıları

 [GitHub deposu](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a>EFCore.Practices

Test destekleyen bir API – N + 1 sorgular için taramak için küçük bir çerçeve dahil olmak üzere bazı iyi ya da en iyi uygulamaları yakalamak çalışır.

[GitHub deposu](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

İkinci düzey önbelleğe alma kitaplığı. İkinci düzey önbelleğe alma sorgu bir önbellektir. Böylece aynı EF komutları, verileri alır, önbellek yerine yeniden veritabanına karşı yürütme EF komutlarının sonuçlarını önbellekte depolanır.

[GitHub deposu](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a>Detached.EntityFramework

Yükler ve tüm ayrılmış varlık grafikleri (alt varlıklar ve liste varlığı) kaydeder. İlham [GraphDiff](https://github.com/refactorthis/GraphDiff/). Ayrıca bazı eklentiler için simplificate denetim ve sayfalandırma gibi bazı yinelenen görevleri ekleyin olur.

[GitHub deposu](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Birincil anahtarı (bileşik anahtarlar dahil) herhangi bir varlığa bir sözlük olarak alın.

[GitHub deposu](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a>EntityFrameworkCore.Rx

Sık erişimli gözlemlenenler Entity Framework varlık için reaktif uzantısı sarmalayıcıları.

[GitHub deposu](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a>EntityFrameworkCore.Triggers

Ekleme, güncelleştirme ve olayları Sil içeren varlıklarınız için Tetikleyiciler ekleyin. Her üç olaylar vardır: önce sonra ve başarısızlık durumunda.

[GitHub deposu](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Türü belirlenmiş OriginalValue, varlık özelliklerini erişin. Basit ve karmaşık özellikler desteklenir, gezinti ve koleksiyonların değildir.

[GitHub deposu](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco bir ters Model Oluşturucu Çoğullaştırma/Singularization ve C# 6.0 ilişkilendirilmiş dizelerle tabanlı ve.Net Core üzerinde çalışan düzenlenebilir şablonlar için destek sağlar. Ayrıca bir çekirdek betiği Oluşturucu SQL birleştirme betikleri ve bir betik Çalıştırıcısı ile sağlar.

[Github deposu](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore LINQ için uzantıları SQL ve EntityFrameworkCore ileri kullanıcılar için ücretsiz bir kümesidir. İle Include(...) ve IDbAsync desteği.

[GitHub deposu](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq.EntityFrameworkCore İşlevler, bunları null için güvenli ve yapı dinamik sorgular yapma sorguları, yeniden yazma yeniden yalnızca küçük bir alt kümesini .NET işlevleri destekleyen LINQ sağlayıcıları gibi Entity Framework kullanarak için faydalı uzantılar sağlar. çevrilebilir doğrulamaları ve Seçici kullanıyor.

[GitHub deposu](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Havuz, iş birimi desenleri ve birden çok veritabanı ile birlikte dağıtılmış işlem desteklenen desteklemek Microsoft.EntityFrameworkCore bir eklenti.

[GitHub deposu](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a>EntityFramework.LazyLoading

EF Core 1.1 yavaş yükleniyor

[GitHub deposu](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Toplu işlemleri (INSERT, Update, Delete) EntityFrameworkCore uzantıları.

[GitHub deposu](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

EF Core için tasarım zamanı çoğullaştırma ekler.

[GitHub deposu](https://github.com/bricelam/EFCore.Pluralizer)
