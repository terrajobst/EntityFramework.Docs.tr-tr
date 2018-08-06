---
title: Varlıklar - EF6 Self izleme
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 5e60f5be-7bbb-4bf8-835e-0ac808d6c84a
caps.latest.revision: 3
ms.openlocfilehash: 2fb4e9f4d4008c57e90c49a011bebb320eb2bb58
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913531"
---
# <a name="self-tracking-entities"></a>Kendi kendine izleme varlıkları

> [!IMPORTANT]
> Artık, self tracking varlıkları şablon kullanmanızı öneririz. Bu yalnızca var olan uygulamaları desteklemek kullanılabilir olmaya devam edecek. Uygulamanızın çalışma bağlantısı kesilmiş varlıklar grafikleriyle gerektiriyorsa, başka alternatifler gibi düşünün [izlenebilir varlıkları](http://trackableentities.github.io/), Self-Tracking-daha etkin bir şekilde tarafından geliştirilen varlıklara benzer bir teknoloji olan Topluluk veya alt düzey değişiklik API'leri izleme kullanarak özel kod yazma.

Entity Framework tabanlı bir uygulama, nesnelerinizi değişiklikleri izlemek için bir bağlam sorumludur. Veritabanı değişiklikleri kalıcı hale getirmek için SaveChanges yöntemi kullanın. N katmanlı uygulamalar ile çalışırken, varlık nesneler bağlamdan genellikle kesilir ve değişiklikleri ve rapor bağlamına bu değişiklikleri izlemek nasıl karar vermeniz gerekir. Varlıkları (Ste'ler) Self izleme, tüm katman değişiklikleri izlemenize ve ardından bu değişikliklerin kaydedilmesi için bir bağlam içinde tekrarlama yardımcı olabilir.  

Yalnızca bağlam bir katmanda kullanılabilir değilse, burada Nesne grafiğini değişiklikler yapılır Ste'leri kullanın. İçerik varsa, bağlam ilgileniriz değişiklikleri izlemek çünkü Ste'leri kullanmaya gerek yoktur.  

Bu şablonu öğesi iki .tt (metin şablonu) dosyaları oluşturur:  

- **\<Model adı\>.tt** varlık türleri ve kendi kendine varlıkları ve durumunun ayarlanması izin veren genişletme yöntemleri izleyerek kullanılan değişiklik izleme mantığı içeren bir yardımcı sınıfı dosyası oluşturur kendi kendine izleme varlıklarda.  
- **\<Model adı\>. Context.tt** dosyası oluşturur, türetilmiş bir bağlam ve içeren bir uzantı sınıfı **ApplyChanges** yöntemleri **ObjectContext** ve **ObjectSet** sınıfları. Bu yöntemler, değişiklikleri veritabanına kaydetmek için gerçekleştirilmesi gereken işlemler kümesini çıkarsanacak varlıkları kendi izleme Graph'te yer alan değişiklik izleme bilgileri inceleyin.  

## <a name="get-started"></a>Başlarken  

Başlamak için ziyaret [Self-Tracking varlıkları izlenecek](walkthrough.md) sayfası.  

## <a name="considerations-when-working-with-self-tracking-entities"></a>Varlıkları kendi izleme ile çalışırken dikkat edilmesi gerekenler  
> [!IMPORTANT]
> Artık, self tracking varlıkları şablon kullanmanızı öneririz. Bu yalnızca var olan uygulamaları desteklemek kullanılabilir olmaya devam edecek. Uygulamanızın çalışma bağlantısı kesilmiş varlıklar grafikleriyle gerektiriyorsa, başka alternatifler gibi düşünün [izlenebilir varlıkları](http://trackableentities.github.io/), Self-Tracking-daha etkin bir şekilde tarafından geliştirilen varlıklara benzer bir teknoloji olan Topluluk veya alt düzey değişiklik API'leri izleme kullanarak özel kod yazma.

Aşağıdaki Self izleme varlıkları ile çalışırken göz önünde bulundurun:  

- İstemci projenize varlık türleri içeren derlemeye bir başvurusu olduğundan emin olun. Yalnızca hizmet başvurusunu istemci projesi eklerseniz, istemci projesinin WCF proxy türleri değil gerçek Self izleme varlık türleri kullanın. Başka bir deyişle, varlık izlemeyi istemcide Yönet otomatik bildirim özellikleri almazsınız. Kasıtlı olarak varlık türleri dahil etmek istemiyorsanız, el ile hizmete geri gönderilecek değişiklikleri için istemci üzerinde değişiklik izleme bilgilerini ayarlamanız gerekir.  
- Hizmet işlemi çağrıları, durum bilgisiz ve nesne bağlamı yeni bir örneğini oluşturun. Ayrıca nesne bağlamında oluşturmanız önerilir bir **kullanarak** blok.  
- Çağrı ve graph el ile yinelemek zorunda hizmeti istemcisi değiştirildiği graf gönderin ve istemcide aynı grafiğe çalışmaya devam etmek istiyorsanız, **AcceptChanges** her nesneye yöntemi değişiklik İzleyici sıfırlayın.  

    > Grafınızı nesneleri veritabanı tarafından oluşturulan değerleri (örneğin, kimlik veya eşzamanlılık) özellikleriyle içeriyorsa, Entity Framework bu özelliklerin değerlerini sonra veritabanı tarafından oluşturulan değerleri değiştirir **SaveChanges** yöntemi çağrılır. Kaydedilen nesneleri veya nesneler için oluşturulan özellik değerlerinin listesini istemciye geri dönmek için hizmet işlemi uygulayabilir. İstemci nesneleri veya özellik değerlerini hizmet işleminden döndürülen nesne örneklerini veya nesnesi özellik değerleri yerine gerekir.  
- Birden çok hizmet istekleri grafiklerinden birleştirme, sonuçta elde edilen grafiğin yinelenen anahtar değerleri ile nesneler neden olabilir. Entity Framework çağırdığınızda yinelenen anahtarlarla nesneleri kaldırmaz **ApplyChanges** yöntemi ancak bunun yerine bir özel durum oluşturur. Yinelenen anahtar değerlerine sahip grafikler aşağıdaki blog içinde açıklanan desenlerden birini izleyin olmamasına özen gösterin: [Self-Tracking varlıklar: ApplyChanges ve yinelenen varlıkları](http://go.microsoft.com/fwlink/?LinkID=205119&clcid=0x409).  
- Yabancı anahtar özelliğini ayarlayarak nesneleri arasındaki ilişki değiştirdiğinizde, başvuru gezinti özelliği ayarlanmış null ve istemci üzerindeki uygun asıl varlığa eşitlenmiş değil. Grafik nesne bağlamına eklendikten sonra (örneğin, çağırdıktan sonra **ApplyChanges** yöntemi), yabancı anahtar özellikler ve gezinti özellikleri eşitlenir.  

    > Yabancı anahtar ilişkisine art arda silme belirttiyseniz, uygun asıl nesneyle eşitlenmiş başvuru gezinme özelliğinin olmaması bir sorun olabilir. Asıl silerseniz, bağımlı nesneleri silme dağıtılmaz. Belirtilen art arda silme varsa, yabancı anahtar özelliği ayarı yerine ilişkilerini değiştirmek için Gezinti özelliklerini kullanın.  
- Kendi kendine izleme varlıkları yavaş yükleme gerçekleştirmek için etkin değil.  
- İkili serileştirme ve seri hale getirme için ASP.NET durumu yönetim nesneleri desteklenmiyor varlıkları kendi kendine izleyerek. Ancak, ikili serileştirme desteği eklemek için şablonunu özelleştirebilirsiniz. Daha fazla bilgi için [kullanarak ikili serileştirme ve ViewState Self-Tracking varlıklarla](http://go.microsoft.com/fwlink/?LinkId=199208).  

### <a name="security-considerations"></a>Güvenlik Değerlendirmeleri  

Aşağıdaki güvenlik konuları hesaba kendi izleme varlıkları ile çalışırken dikkat edilmelidir:  

- Hizmet istekleri almak veya güvenilir olmayan bir istemci veya güvenilir olmayan bir kanal aracılığıyla verileri güncelleştirmek için güven yok. Bir istemci kimlik doğrulaması yapılması gerekir: güvenli bir kanal ya da ileti zarfı kullanılmalıdır. Beklenen ve yasal değişiklikleri verilen senaryo için uygun olmak için güncelleştirme veya veri almak için istemcilerin istekleri doğrulanması gerekir.  
- Varlık anahtarları (örneğin, sosyal güvenlik numaraları) hassas bilgiler kullanmaktan kaçının. Bu, tam olarak güvenilmeyen bir istemci kendi izleme varlık grafiklere gizli bilgileri yanlışlıkla serileştirmek olasılığını azaltır. Bağımsız ilişkilerini serileştirilmekte olan ilgili bir varlığın özgün anahtar istemciye gönderilebilir.  
- İstemci katmanı, gizli verileri içeren bir özel durum iletileri yayılmasını önlemek için çağrılar **ApplyChanges** ve **SaveChanges** sunucu üzerinde özel durum işleme kodu katmanı alınmalı.  
