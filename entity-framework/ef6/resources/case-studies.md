---
title: Entity Framework - EF6 için örnek olay incelemeleri
author: divega
ms.date: 2016-10-23
ms.assetid: cd5d3ae3-717d-4095-a2ef-0e8fd72b1a2f
ms.openlocfilehash: 5dcd19338d549c7ae798fdb3fc7677d8569b483f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995767"
---
# <a name="microsoft-case-studies-for-entity-framework"></a>Entity Framework için Microsoft incelemeleri
Bu sayfadaki örnek olay incelemeleri, Entity Framework işe birkaç gerçek üretim projeleri vurgulayın.
> [!NOTE]
> Bu örnek olay incelemeleri ayrıntılı sürümleri artık Microsoft Web sitesindeki kullanılamaz. Bu nedenle bağlantıları kaldırıldı.

## <a name="epicor"></a>Epicor
Epicor 150'den fazla ülkede şirketler için kurumsal kaynak planlama (ERP) çözümleri geliştirir bir büyük Genel yazılım (ile 400'den geliştiriciler) şirketidir.
Epicor 9, kendi Gemisi ürünü bir Service-Oriented mimarisi (.NET Framework kullanarak SOA üzerinde) temel alır.
Takım, dil tümleşik sorgu (LINQ) ve ayrıca kendi arka uç SQL sunucuları üzerindeki yükü azaltmak isteyen desteği sağlamak için çok sayıda müşteri istekleri ile karşılaşılan, Visual Studio 2010 ve .NET Framework 4.0 yükseltme karar verdi.
Entity Framework 4.0 kullanarak, bunlar bu hedefleri gerçekleştirmeye ve geliştirme ve Bakım büyük ölçüde basitleştirir.
Özellikle, Entity Framework'ün zengin T4 desteği bunları kendi oluşturulan kod tam denetimini elinize alın ve önceden derlenmiş sorgular ve önbelleğe alma gibi performans koruma özellikleri otomatik olarak oluşturmak izin verilir.

> "Biz varolan kodu ile kısa bir süre önce bazı performans testleri yürütülür ve biz isteklerin yüzde 90'ının SQL Server'a azaltabilirsiniz.
ADO.NET Entity Framework 4 nedeniyle olmasıdır." – Erik Johnson, Başkan Yardımcısı, ürün araştırma  

## <a name="veracity-solutions"></a>Belki çözümleri
Belki çözümleri uzun süreli genişletin ve korumak zor giden bir olay planlama yazılım sistemi alınan, güçlü ve kullanımı kolay bir zengin Internet uygulaması Silverlight 4'te yerleşik olarak yeniden yazmak için Visual Studio 2010 kullanılır.
.NET RIA hizmetlerini kullanarak, bir hizmet katmanı, kod yinelemesi kurtuldu ve Katmanlar arası ortak doğrulama ve kimlik doğrulaması mantığı için izin verilen Entity Framework'ün üstünde hızlıca oluşturabiliyor.  

> "Biz Entity Framework'ü ilk kez sunulmuştur ve daha da iyi olması için Entity Framework 4 kanıtlamış satıldı.
Araç geliştirilmiştir ve kavramsal model, depolama modelinin ve bu modelleri arasındaki eşlemeyi tanımlar .edmx dosyaları yönetmek daha kolay... Entity Framework ile çalışan bir gün içinde veri erişim katmanı alabilirim — miyim ilerledikçe kullanıma derleyin.
Entity Framework bizim pratikte veri erişim katmanı sınıflandırabilirsiniz. Herkes bunu neden kullanmaz bilmiyorum." – ALi McBride, üst düzey Geliştirici

## <a name="nec-display-solutions-of-america"></a>NEC görünen çözümlerinizi Amerika
NEC, kendi gelirlerini artırın ve reklamcılar ve ağ sahipleri yararlanmak için bir çözüm ile dijital yer alarak reklam için pazarına girmeleri istiyordu.
Bunu yapabilmek için geleneksel ad kampanyada gerekli el ile gerçekleştirilen işlemleri otomatikleştirin web uygulamalarının bir çift başlattı.
Siteleri, ASP.NET, Silverlight 3, AJAX ve WCF, SQL Server 2008'e konuşmak için Entity Framework Veri erişim katmanında birlikte kullanılarak oluşturulmuştur.

> "SQL Server ile biz size alabilir ihtiyacımız reklamcılar ve ağlar ile gerçek zamanlı ve güvenilirliği kritik uygulamalarımızın bilgileri her zaman kullanılabilir olacağına dikkat sağlamaya yardımcı olmak için bilgi sunmak için aktarım hızı düşünmüştür"-Mike Corcoran Direktörü BT

## <a name="darwin-dimensions"></a>Darwin boyutları
Çeşitli Microsoft teknolojilerini kullanarak, ekip Darwin, Evolver - tüketiciler kullanmak için çarpıcı, gerçek avatarlar oyunlar, animasyonları ve sosyal ağ sayfaları oluşturmak için kullanabilirsiniz, bir çevrimiçi avatar portalı oluşturmak için ayarlanan.
Entity Framework ve Windows Workflow Foundation (WF) ve Windows Server AppFabric (yüksek oranda ölçeklenebilir bellek içi uygulama önbelleği) gibi bileşenleri çekiliyor üretkenlik avantajlarıyla birlikte takım % 35 daha az harika bir ürün teslim edebilirsiniz geliştirme zamanı.
Takım üyeleri sahip olmasına rağmen bir Çevik Geliştirme işlemi haftalık sürümleriyle aşağıdaki takım birden fazla ülkede arasında bölün.

 > "Bu sizi teknolojinin çok teknoloji oluşturmamayı çalışırız. Bir başlangıç size zaman ve para tasarrufu teknolojisini kullanır önemlidir.
 .NET için hızlı ve uygun maliyetli geliştirme seçim sağladı." – Zachary Olsen, Mimarı  

## <a name="silverware"></a>Silverware
Daha büyük çekmek için daha fazla kuruluş düzeyi özellikleri ile ürün geliştirmek için Silverware geliştirme ekibinin 15 yıldan fazla ile küçük ve orta ölçekli bir restoran grupları için satış noktası (POS) çözümleri geliştirme deneyiminin, belirlenmiş Restoran zincirleri.
Microsoft'un Geliştirme Araçları'nın en son sürümünü kullanarak, bunlar dört kat daha hızlı daha önce yeni çözümü oluşturmak kullanabilirsiniz.
LINQ ve SQL Server 2008 ve SQL Server Raporlama Hizmetleri (SSRS) için kendi veri depolama için Crystal raporlardan taşımak kolay ve raporlama ihtiyaçlarını Entity Framework gibi önemli yeni özellikler.

> "Etkili veri yönetimi SilverWare – başarısını anahtarıdır ve SQL Raporlama benimsemeye karar nedeni budur." -Nicholas Romanidis, Direktörü BT / yazılım Mühendisliği
