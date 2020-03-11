---
title: Entity Framework-EF6 için örnek olay Incelemeleri
author: divega
ms.date: 10/23/2016
ms.assetid: cd5d3ae3-717d-4095-a2ef-0e8fd72b1a2f
ms.openlocfilehash: d7982a3f89ac1e57b48039d828f287adf6dc5068
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417083"
---
# <a name="microsoft-case-studies-for-entity-framework"></a>Entity Framework için Microsoft örnek olay Incelemeleri
Bu sayfadaki olay incelemeleri, Entity Framework çalışan birkaç gerçek dünya Üretim projesini vurgulayacaktır.
> [!NOTE]
> Bu örnek olay incelemelerine ilişkin ayrıntılı sürümler artık Microsoft Web sitesinde bulunmamaktadır. Bu nedenle bağlantılar kaldırılmıştır.

## <a name="epicor"></a>Epicor
Epicor, en fazla 150 ülkede yer alan şirketler için kurumsal kaynak planlama (ERP) çözümlerini geliştiren büyük bir küresel yazılım şirketidir (400 'den fazla geliştiriciyle).
Kendi flagnakliye ürünü, Epicveya 9, .NET Framework kullanarak hizmet yönelimli bir mimariye (SOA) dayanır.
Dil ile tümleşik sorgu (LINQ) desteği sağlamak ve ayrıca arka uç SQL sunucularındaki yükü azaltmak için çok sayıda müşteri isteği ile birlikte, takım Visual Studio 2010 ve 4,0 .NET Framework yükseltmeye karar verdi.
4,0 Entity Framework kullanarak, bu hedeflere ulaşabiliyor ve ayrıca geliştirme ve bakımın önemli ölçüde basitleşiyor.
Özellikle, Entity Framework zengin T4 desteği, kendilerine oluşturulan kodların tam denetimini ele geçirmesine ve önceden derlenmiş sorgular ve önbelleğe alma gibi performans tasarrufu özellikleriyle otomatik olarak derlenmesine izin verilir.

> "Bazı performans testlerini son zamanlarda mevcut kodla ekledik ve SQL Server %90 oranında istekleri azalttık.
Bunun nedeni, ADO.NET Entity Framework 4. " – Erik Johnson, Başkan Yardımcısı, ürün araştırması  

## <a name="veracity-solutions"></a>Coacity çözümleri
Uzun süreli uygulamalar üzerinde bakım ve genişletme olanağı sunan bir olay planlama yazılım sistemi elde etmekle, Visual Studio 2010, Silverlight 4 ' te oluşturulan güçlü ve kullanımı kolay zengin bir Internet uygulaması olarak yeniden yazmak için Visual Studio ' i kullandı.
.NET RıA hizmetlerini kullanarak, kod çoğaltmasının önden çıkarılan ve katmanlar arasında ortak doğrulama ve kimlik doğrulama mantığı için izin verilen Entity Framework en üstünde hızlı bir şekilde bir hizmet katmanı oluşturabilebiliyoruz.  

> "İlk tanıtıldığı sırada Entity Framework satıldık ve Entity Framework 4 ' ün daha da iyi olduğunu kanıtlamış.
Araç geliştirildi ve bu modeller arasındaki kavramsal modeli, depolama modelini ve eşlemeyi tanımlayan. edmx dosyalarını işlemek daha kolay... Entity Framework, bu veri erişim katmanının bir gün içinde çalışmasını sağlayabilir ve sonra da kullanıma sundum.
Entity Framework de veri erişim katmanımız. Herkesin neden kullanmadığına bilmiyorum. " – Ali McBride, üst düzey geliştirici

## <a name="nec-display-solutions-of-america"></a>Amerika NEC ekran çözümleri
NEC, reklamları ve ağ sahiplerini avantaj sağlamak ve kendi gelirlerini artırmak için bir çözümle dijital BT tabanlı reklam için pazara girmek istedi.
Bunu yapmak için, geleneksel bir ad kampanyasında gereken el ile işlemleri otomatikleştiren bir çift Web uygulaması başlattı.
Siteler, SQL Server 2008 ile konuşmak için veri erişim katmanındaki Entity Framework birlikte ASP.NET, Silverlight 3, AJAX ve WCF kullanılarak oluşturulmuştur.

> "SQL Server sayesinde, gerçek zamanlı bilgileri ve iş açısından kritik uygulamalarımızda bulunan bilgilerin her zaman kullanılabilir olmasını sağlamaya yardımcı olmak için gerekli olan aktarım hızını ve"-Mike Corcora, BT BT Müdürü

## <a name="darwin-dimensions"></a>Darwin boyutları
Çok sayıda Microsoft teknolojisinden yararlanarak, Darwin 'daki ekip yeni bir avaku oluşturmak için, tüketicilerin oyun, animasyon ve sosyal ağ sayfalarında kullanılmak üzere şaşırtıcı ve harika bir harika ve harika bir şekilde iletişim kurmak için kullanabileceği bir çevrimiçi Avatar portalı oluşturur.
Entity Framework verimlilik avantajları ve Windows Workflow Foundation (WF) ve Windows Server AppFabric (yüksek düzeyde ölçeklenebilir bir bellek içi uygulama önbelleği) gibi bileşenlere çekilerek, takım %35 daha az bir ürünün harika bir ürününü sunmaya geliştirme süresi.
Ekip üyelerinin birden çok ülkede bölünmesine rağmen, ekip haftalık yayınlar ile çevik bir geliştirme sürecini takip eden bir işlemdir.

 > "Teknolojinin sake için teknoloji oluşturmamalıdır. Bir başlangıç olarak, saat ve para tasarrufu sağlayan teknolojimizin faydalanma açısından önemlidir.
 .NET, hızlı ve uygun maliyetli geliştirme seçimiydi. " – Zachary Olsen, mimar  

## <a name="silverware"></a>Silverware
Küçük ve orta büyüklükte Restoran grupları için satış noktası (POS) çözümleri geliştirme konusunda 15 yıldan fazla deneyimle, Silverware adresindeki geliştirme ekibi, daha fazla bilgi almak için ürünleri daha fazla kurumsal düzey özelliklerle geliştirmeye yönelik olarak ayarlanmıştır Restoran zincirleri.
Microsoft 'un geliştirme araçlarının en son sürümünü kullanarak, yeni çözümü daha önce dört kat daha hızlı derleyebilir.
LINQ ve Entity Framework gibi temel yeni özellikler, veri depolama ve raporlama ihtiyaçları için Crystal Reports 'tan SQL Server 2008 ve SQL Server Reporting Services (SSRS) arasında geçiş yapılmasını kolaylaştırır.

> "Geçerli veri yönetimi SilverWare başarısının bir anahtarıdır ve SQL raporlamayı benimsemeye karar verdik." -Nicholas Romanidu, BT/yazılım mühendisliği müdürü
