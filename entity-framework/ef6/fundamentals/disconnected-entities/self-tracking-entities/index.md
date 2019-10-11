---
title: Kendi kendine izleme varlıkları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5e60f5be-7bbb-4bf8-835e-0ac808d6c84a
ms.openlocfilehash: 3bb9759d89fbd0c10b911625aa7d0afd7747de14
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181724"
---
# <a name="self-tracking-entities"></a>Kendi kendine izleme varlıkları

> [!IMPORTANT]
> Artık kendi kendine izleme varlıkları şablonunu kullanmanızı önermiyoruz. Yalnızca var olan uygulamaları desteklemek için kullanılabilir olmaya devam edecektir. Uygulamanız, bağlantısı kesilen varlıkların, topluluk tarafından daha etkin bir şekilde geliştirilen veya yazma gibi, kendi kendini Izlemeye benzer bir teknoloji olan, [izleyicileri](https://trackableentities.github.io/)olan diğer alternatifleri göz önünde bulundurun. alt düzey değişiklik izleme API 'Lerini kullanan özel kod.

Entity Framework tabanlı bir uygulamada, nesnelerdeki değişiklikleri izlemeden bir bağlam sorumludur. Sonra değişiklikleri veritabanında kalıcı hale getirmek için SaveChanges yöntemini kullanırsınız. N katmanlı uygulamalarla çalışırken, varlık nesnelerinin genellikle bağlamla bağlantısı kesilir ve değişikliklerin nasıl izleneceğini ve bu değişiklikleri içeriğe geri rapor etmeniz gerektiğine karar vermeniz gerekir. Kendi kendini izleyen varlıklar (Steler), herhangi bir katmandaki değişiklikleri izlemenize ve sonra bu değişiklikleri kaydedilecek bir bağlamda yeniden yürütmeye yardımcı olabilir.  

Yalnızca bağlam, nesne grafiğinde yapılan değişikliklerin yapıldığı bir katmanda kullanılabilir değilse, STEs 'yi kullanın. Bağlam kullanılabiliyorsa, bağlam değişiklikleri izlemek için gereken bir işlem olması gerekmez.  

Bu şablon öğesi iki. tt (metin şablonu) dosyası oluşturur:  

- **@No__t-1model Name\>.tt** dosyası, kendi kendine izleme varlıkları ve kendi kendini takip eden varlıklarda ayar durumuna izin veren genişletme yöntemleri tarafından kullanılan değişiklik izleme mantığını içeren bir yardımcı sınıfı ve varlık türlerini üretir.  
- **@No__t-1model adı @ no__t-2. Context.tt** dosyası türetilmiş bir bağlam ve **ObjectContext** ve **ObjectSet** sınıfları için **ApplyChanges** yöntemlerini içeren bir uzantı sınıfı oluşturur. Bu yöntemler, değişiklikleri veritabanına kaydetmek için gerçekleştirilmesi gereken işlemler kümesini çıkarması için kendi kendine izleme varlıklarının grafiğinde bulunan değişiklik izleme bilgilerini inceler.  

## <a name="get-started"></a>Başlarken  

Başlamak için, [kendi kendine Izleme varlıkları Izlenecek yol](walkthrough.md) sayfasını ziyaret edin.  

## <a name="functional-considerations-when-working-with-self-tracking-entities"></a>Kendi kendine Izleme varlıklarıyla çalışırken işlevsel konular  
> [!IMPORTANT]
> Artık kendi kendine izleme varlıkları şablonunu kullanmanızı önermiyoruz. Yalnızca var olan uygulamaları desteklemek için kullanılabilir olmaya devam edecektir. Uygulamanız, bağlantısı kesilen varlıkların, topluluk tarafından daha etkin bir şekilde geliştirilen veya yazma gibi, kendi kendini Izlemeye benzer bir teknoloji olan, [izleyicileri](https://trackableentities.github.io/)olan diğer alternatifleri göz önünde bulundurun. alt düzey değişiklik izleme API 'Lerini kullanan özel kod.

Kendi kendine izleme varlıklarıyla çalışırken aşağıdakileri göz önünde bulundurun:  

- İstemci projenizin varlık türlerini içeren derlemeye bir başvurusu olduğundan emin olun. İstemci projesine yalnızca hizmet başvurusunu eklerseniz, istemci projesi gerçek kendi kendini izleyen varlık türlerini değil, WCF proxy türlerini kullanır. Bu, istemci üzerindeki varlıkların izlenmesini yöneten otomatik bildirim özelliklerini alacağınız anlamına gelir. Özellikle varlık türlerini eklemek istemiyorsanız, hizmete geri gönderilecek değişiklikler için istemcideki değişiklik izleme bilgilerini el ile ayarlamanız gerekir.  
- Hizmet işlemine yapılan çağrılar durum bilgisiz olmalıdır ve yeni bir nesne bağlamı örneği oluşturur. Ayrıca, bir **using** bloğunda nesne bağlamı oluşturmanızı öneririz.  
- İstemcide değiştirilmiş olan grafiği hizmete gönderdiğinizde ve sonra istemcide aynı grafikle çalışmaya devam etmeyi planlıyorsanız, grafiği el ile yinelemek ve değişikliği sıfırlamak için her nesne üzerinde **AcceptChanges** yöntemini çağırmanız gerekir ýndan.  

    > Grafiğinizde nesneler veritabanı tarafından oluşturulan değerlerle (örneğin, kimlik veya eşzamanlılık değerleri) özellikler içeriyorsa Entity Framework, bu özelliklerin değerleri, **SaveChanges** yöntemi olduktan sonra veritabanı tarafından oluşturulan değerlerle değiştirilir çağırılır. Kaydedilmiş nesneleri ya da nesneler için oluşturulan özellik değerlerinin bir listesini istemciye geri döndürmek için hizmet işleminizi uygulayabilirsiniz. Daha sonra istemci, nesne örnekleri veya nesne özelliği değerlerini hizmet işleminden döndürülen nesneler veya özellik değerleriyle değiştirmek zorunda olur.  
- Birden çok hizmet isteğinin grafiklerini birleştirme, sonuçta elde edilen grafikte yinelenen anahtar değerleriyle nesneleri ortaya çıkarabilir. Entity Framework **ApplyChanges** metodunu çağırdığınızda yinelenen anahtarlarla nesneleri kaldırmaz, ancak bunun yerine bir özel durum oluşturur. Yinelenen anahtar değerleriyle grafiklerin oluşmasını önlemek için, aşağıdaki blogda açıklanan desenlerden birini izleyin: [Kendi kendine Izleme varlıkları: ApplyChanges ve yinelenen varlıklar @ no__t-0.  
- Yabancı anahtar özelliğini ayarlayarak nesneler arasındaki ilişkiyi değiştirdiğinizde, başvuru gezintisi özelliği null olarak ayarlanır ve istemcideki uygun asıl varlıkla eşitlenmez. Grafik nesne bağlamına eklendikten sonra (örneğin, **ApplyChanges** yöntemini çağırdıktan sonra), yabancı anahtar özellikleri ve gezinti özellikleri eşitlenir.  

    > Yabancı anahtar ilişkisinde basamaklı silme belirttiyseniz, başvuru gezintisi özelliğinin uygun asıl nesneyle eşitlenmiş olmadığı bir sorun olabilir. Sorumluyu silerseniz, silme bağımlı nesnelere yayılmaz. Art arda silme belirtilirse, yabancı anahtar özelliğini ayarlamak yerine ilişkileri değiştirmek için gezinti özelliklerini kullanın.  
- Kendi kendini izleyen varlıklar, yavaş yükleme gerçekleştirmek için etkin değildir.  
- İkili serileştirme ve ASP.NET durum yönetimi nesnelerine serileştirme, kendi kendine izleme varlıkları tarafından desteklenmez. Ancak, ikili serileştirme desteğini eklemek için şablonu özelleştirebilirsiniz. Daha fazla bilgi için, bkz. [Ikili serileştirme ve kendi kendine Izleme varlıkları Ile ViewState kullanma](https://go.microsoft.com/fwlink/?LinkId=199208).  

## <a name="security-considerations"></a>Güvenlik Değerlendirmeleri  

Kendi kendine izleme varlıklarıyla çalışırken aşağıdaki güvenlik konuları dikkate alınmalıdır:  

- Hizmet, güvenilir olmayan bir istemciden veya güvenilmeyen bir kanaldan veri alma veya güncelleştirme isteklerine güvenmemelidir. İstemcinin kimliği doğrulanmalıdır: güvenli bir kanal veya ileti zarfı kullanılmalıdır. Verilen senaryoya ilişkin beklenen ve meşru değişikliklere uyduğundan emin olmak için istemcilerin verileri güncelleştirme veya alma isteklerinin doğrulanması gerekir.  
- Gizli bilgileri varlık anahtarları (örneğin, sosyal güvenlik numaraları) olarak kullanmaktan kaçının. Bu, kendi kendini takip eden varlık grafiklerde hassas bilgileri tam güvenilir olmayan bir istemciye yanlışlıkla serileştirmek olasılığını azaltır. Bağımsız İlişkilendirmelerde, serileştirilmekte olan varlıkla ilişkili bir varlığın özgün anahtarı da istemciye gönderilebilir.  
- Gizli veriler içeren özel durum iletilerini istemci katmanına yaymamak için sunucu katmanındaki **ApplyChanges** ve **SaveChanges** çağrılarına özel durum işleme kodunda sarmalanması gerekir.  
