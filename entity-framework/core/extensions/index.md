---
title: Araçlar ve uzantılar - EF çekirdek
author: ErikEJ
ms.author: divega
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/extensions/index
ms.openlocfilehash: 6c8cb3e0d8552f274118e4020b7e2e8009af7e11
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
ms.locfileid: "29769445"
---
# <a name="ef-core-tools--extensions"></a>EF çekirdek Araçları & uzantıları

Araçlar ve uzantılar Entity Framework Çekirdek için ek işlevsellik sağlar.

> [!IMPORTANT]  
> Uzantıları çeşitli kaynakları tarafından oluşturulan ve Entity Framework Çekirdek projenin bir parçası olarak tutulan değil. Bir üçüncü taraf uzantı değerlendirirken, kalite, lisans, uyumluluk, destek vb. gereksinimlerinizi karşıladıklarından emin olmak için değerlendirilecek emin olun.

## <a name="tools"></a>Araçlar

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro Entity Framework ve Entity Framework Çekirdek desteği çözümüyle modelleme bir varlıktır. Kolayca varlık modeli tanımlamak ve veritabanı ilk kullanarak, veritabanı ile eşleniyor sağlar ya da ilk olarak, sorgu hemen yazma başlayabiliriz modelinizi.

[Web sitesi](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart varlık Geliştirici

Varlık Geliştirici ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik veri erişimi ve LINQ-SQL için güçlü bir ORM Tasarımcısı ' dir. Model ilk kullanabilirsiniz ve ORM modelinizi tasarlamak ve C# veya Visual Basic .NET kodu için oluşturmak için veritabanı ilk yaklaşıyor. ORM modelleri tasarlamak için yeni yaklaşımlar tanıtır, verimliliği artırır ve veritabanı uygulamaların geliştirilmesini kolaylaştırır.

[Web sitesi](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF çekirdek güç araçları

Visual Studio 2017+ extension. Varolan bir veritabanına veya SQL Server veritabanı projesi ters mühendislik DbContext ve POCO sınıflarının görselleştirin ve çeşitli yollarla, DbContext inceleyin.

[GitHub wiki](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

## <a name="extensions"></a>Uzantıları

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Bir eklenti için otomatik olarak veri kaydını desteklemek Microsoft.EntityFrameworkCore geçmişi değiştirir.

[GitHub depo](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Zaman uyumsuz desteği ekleyen Microsoft.EntityFrameworkCore için dinamik LINQ uzantıları

 [GitHub depo](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a>EFCore.Practices

Sınama destekleyen bir API içinde – N + 1 sorgular için taramak için küçük bir çerçeve dahil olmak üzere bazı iyi veya en iyi yöntemler yakalama girişimi.

[GitHub depo](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

İkinci düzey önbelleğe alma kitaplığı. İkinci düzey önbelleğe alma sorgusu bir önbelleğidir. EF komutları sonuçlarını önbelleğinde aynı EF komutlar kendi veri önbelleği yerine yeniden veritabanına karşı çalıştırma alacak depolanan.

[GitHub depo](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a>Detached.EntityFramework

Yükler ve tüm ayrılmış varlık grafikleri (kendi alt varlıkları ve listeleri olan varlık) kaydeder. Tarafından neden olacak [GraphDiff](https://github.com/refactorthis/GraphDiff/). Ayrıca bazı eklentiler için simplificate denetim ve sayfa numaralandırma gibi bazı yinelenen görevleri ekleyin olur.

[GitHub depo](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

(Bileşik anahtarlar dahil) birincil anahtar herhangi bir varlığa bir sözlük olarak alır.

[GitHub depo](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a>EntityFrameworkCore.Rx

Entity Framework varlıkların etkin gözlemlenenler için geriye dönük uzantısı sarmalayıcıları.

[GitHub depo](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a>EntityFrameworkCore.Triggers

Varlıklarınızı ile tetikleyicilere ekleme, güncelleştirme ve olayları Sil ekleyin. Her üç olay vardır: önce sonra ve başarısızlık durumunda.

[GitHub depo](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Yazılı varlık özelliklerinizi OriginalValue erişin. Basit ve karmaşık özellikler desteklenmez, gezinti/koleksiyonları değildir.

[GitHub depo](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco sağlar ters Model Oluşturucu desteği Çoğullaştırma/Singularization ve C# 6.0 ara değerli dizeler üzerinde bağlı ve .net çalıştıran düzenlenebilir şablonları ile çekirdek. Ayrıca bir çekirdek Kod Oluşturucu SQL birleştirme komut dosyaları ve komut dosyası Çalıştırıcısı sağlar.

[Github depo](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore LINQ uzantıları SQL ve EntityFrameworkCore güç kullanıcılar için boş bir kümesidir. Include(...) ve IDbAsync desteğiyle.

[GitHub depo](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

İşlevler, hatta bunları null güvenli ve yapı dinamik sorgular hale sorguları, yeniden yazma işlemi yeniden yalnızca küçük bir alt .NET işlevlerini destekleyen LINQ gibi sağlayıcıları Entity Framework kullanarak için faydalı uzantılar NeinLinq.EntityFrameworkCore sağlar çevrilebilir koşulları ve seçiciler kullanma.

[GitHub depo](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Havuz, iş desenleri birimidir ve birden çok veritabanı ile birlikte dağıtılmış işlem desteklenen desteklemek Microsoft.EntityFrameworkCore bir eklenti.

[GitHub depo](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a>EntityFramework.LazyLoading

Geç EF çekirdek 1.1 yükleme

[GitHub depo](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Toplu işlemler (INSERT, Update, Delete) EntityFrameworkCore uzantıları.

[GitHub depo](https://github.com/borisdj/EFCore.BulkExtensions)
