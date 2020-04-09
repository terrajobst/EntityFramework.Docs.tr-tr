---
title: EF Core - EF Core kullanarak bileşenleri test edin
description: EF Core kullanan uygulamaları test etmek için farklı yaklaşımlar
author: ajcvickers
ms.date: 03/23/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: b1ab37ebb0a3aae09d5d5b225f746cf83dfba170
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634245"
---
# <a name="testing-code-that-uses-ef-core"></a>EF Core kullanan test kodu

Veritabanına erişen sınama kodu şunları gerektirir:
* Üretimde kullanılan aynı veritabanı sistemine karşı sorguları ve güncelleştirmeleri çalıştırma.
* Sorguları ve güncelleştirmeleri yönetmek daha kolay olan diğer bazı veritabanı sistemine karşı çalıştırmak.
* Bir veritabanını kullanmaktan kaçınmak için test çiftlerini veya başka bir mekanizmayı kullanma.

Bu belge, bu seçeneklerin her birinde yer alan dengeleri özetler ve EF Core'un her yaklaşımda nasıl kullanılabileceğini gösterir.  

## <a name="all-database-providers-are-not-equal"></a>Tüm veritabanı sağlayıcıları eşit değildir

EF Core'un temel veritabanı sisteminin her yönünü özetlemek için tasarlanmadığını anlamak çok önemlidir.
Bunun yerine, EF Core herhangi bir veritabanı sistemi ile kullanılabilecek desenler ve kavramlar ortak bir kümesidir.
EF Core veritabanı sağlayıcıları daha sonra veritabanına özgü davranış ve işlevleri bu ortak çerçeve üzerinde katman.
Bu, her veritabanı sisteminin, uygun olduğu durumlarda diğer veritabanı sistemleriyle ortak lığı korurken en iyi yaptığı şeyi yapmasına olanak tanır. 

Temelde, bu veritabanı sağlayıcısı nı değiştirmenin EF Core davranışını değiştireceği ve uygulamanın davranıştaki tüm farklılıkları açıkça hesaba ketmediği sürece doğru çalışması beklenmediği anlamına gelir.
Bu söyleniyor, birçok durumda ilişkisel veritabanları arasında ortak yüksek derecede olduğu için bu iş çalışacaktır.
Bu iyi ve kötü.
Veritabanları arasında taşıma nispeten kolay olabilir, çünkü iyi.
Uygulama yeni veritabanı sistemine karşı tam olarak sınanmazsa yanlış bir güvenlik hissi verebilir çünkü kötü.  

## <a name="approach-1-production-database-system"></a>Yaklaşım 1: Üretim veritabanı sistemi

Önceki bölümde açıklandığı gibi, üretimde ne çalışır sınama emin olmak için tek yolu aynı veritabanı sistemini kullanmaktır.
Örneğin, dağıtılan uygulama SQL Azure kullanıyorsa, sql azure'a karşı da sınama yapılmalıdır.

Ancak, kod üzerinde etkin olarak çalışırken her geliştiricinin SQL Azure'a karşı testler çalıştırması hem yavaş hem de pahalı olacaktır.
Bu, bu yaklaşımlar boyunca ilgili ana ticareti göstermektedir: test verimliliğini artırmak için üretim veritabanı sisteminden sapmak ne zaman uygundur?

Neyse ki, bu durumda cevap oldukça kolaydır: geliştirici testi için yerel veya şirket içi SQL Server kullanın.
SQL Azure ve SQL Server son derece benzer, bu nedenle SQL Server karşı test genellikle makul bir ticaret kapalıdır.
Bununla ilgili olarak, üretime geçmeden önce SQL Azure'a karşı testler çalıştırmak hala akıllıca olacaktır.
 
### <a name="localdb"></a>Localdb 

Tüm büyük veritabanı sistemleri yerel sınama için "Developer Edition" bir çeşit var.
SQL Server da [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15)adlı bir özelliği vardır.
LocalDb'ın birincil avantajı, veritabanı örneğini isteğe bağlı olarak döndürmesidir.
Bu, testler çalıştırmıyorsanız bile makinenizde çalışan bir veritabanı hizmetiolmasını önler.

LocalDb sorunları olmadan değil:
* [SQL Server Developer Edition'ın](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) yaptığı her şeyi desteklemez.
* Linux'ta kullanılamaz.
* Hizmet büküldükçe ilk test çalışmasında gecikmeye neden olabilir.

Şahsen, benim dev makine üzerinde çalışan bir veritabanı hizmeti olan bir sorun hiç bulamadım ve ben genellikle yerine Developer Edition kullanmanızı öneririz.
Ancak, bazı insanlar için uygun olabilir, özellikle daha az güçlü dev makineleri.  

## <a name="approach-2-sqlite"></a>Yaklaşım 2: SQLite

EF Core, SQL Server sağlayıcısını öncelikle yerel bir SQL Server örneğine karşı çalıştırarak sınar.
Bu testler, hızlı bir makinede birkaç dakika içinde on binlerce sorgu çalıştırın.
Bu, gerçek veritabanı sistemini kullanmanın bir performans çözümü olabileceğini göstermektedir.
Bazı hafif veritabanı kullanarak hızlı testleri çalıştırmak için tek yol olduğunu bir efsanedir.

Bununla söyleniyor, ya her ne sebeple olursa olsun, üretim veritabanı sisteminize yakın bir şeye karşı testler çalıştıramıyorsanız?
Bir sonraki en iyi seçenek benzer işlevsellik ile bir şey kullanmaktır.
Bu genellikle başka bir ilişkisel veritabanı anlamına gelir, hangi [SQLite](https://sqlite.org/index.html) bariz bir seçimdir.

SQLite iyi bir seçimdir çünkü:
* Uygulamanızla birlikte çalışır ve böylece düşük ek yükü vardır.
* Veritabanları için basit, otomatik olarak oluşturulan dosyaları kullanır ve bu nedenle veritabanı yönetimi gerektirmez.
* Dosya oluşturmayı bile önleyen bir bellek modu vardır.

Ancak, şunu unutmayın:
* SQLite kaçınılmazlık üretim veritabanı sistemi yaptığı her şeyi desteklemez.
* SQLite, bazı sorgular için üretim veritabanı sisteminizden farklı davranacaktır.

Yani bazı test için SQLite kullanıyorsanız, aynı zamanda gerçek veritabanı sistemine karşı test emin olun.

EF Core özel kılavuzu için [SQLite ile Test'e](xref:core/miscellaneous/testing/sqlite) bakın. 

## <a name="approach-3-the-ef-core-in-memory-database"></a>Yaklaşım 3: EF Core bellek veritabanı

EF Core, EF Core'un kendi iç testlerinde kullandığımız bir bellek içi veritabanıyla birlikte gelir.
Bu veritabanı genel **olarak EF Core kullanan uygulamaları test etmek için uygun değildir.** Daha ayrıntılı şekilde belirtmek gerekirse:
* İlişkisel bir veritabanı değildir
* Hareketleri desteklemiyor
* Performans için optimize edilmedi

EF Core dahili lerini test ederken bunların hiçbiri çok önemli değildir, çünkü veritabanının testle alakasız olduğu yerlerde kullanırız.
Öte yandan, bu şeyler EF Core kullanan bir uygulama test ederken çok önemli olma eğilimindedir.

## <a name="unit-testing"></a>Birim testi

Veritabanından bazı verileri kullanması gerekebilecek, ancak veritabanı etkileşimlerini doğal olarak sınamayan bir iş mantığı parçasını sınamayı düşünün.
Bir seçenek sahte veya sahte gibi bir [test çift](https://en.wikipedia.org/wiki/Test_double) kullanmaktır.

EF Core'un dahili testleri için test çiftleri kullanıyoruz.
Ancak, biz DbContext veya IQueryable alay asla.
Bunu yapmak zor, hantal ve kırılgandır.
**Bunu yapma.**

Bunun yerine, DbContext kullanan bir şeyi birim olarak sınarken bellek içi veritabanını kullanırız.
Bu durumda, sınama veritabanı davranışına bağlı olmadığından bellek içi veritabanını kullanmak uygundur.
Bunu gerçek veritabanı sorgularını veya güncelleştirmelerini sınamak için yapmayın.   

Birim sınama için bellek içi veritabanını kullanma konusunda EF Core'a özgü kılavuz için [bellek içi sağlayıcıyla sınama](xref:core/miscellaneous/testing/in-memory) konusuna bakın.
