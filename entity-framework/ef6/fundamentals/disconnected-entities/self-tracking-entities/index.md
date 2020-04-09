---
title: Kendi kendini takip eden varlıklar - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5e60f5be-7bbb-4bf8-835e-0ac808d6c84a
ms.openlocfilehash: 3bb9759d89fbd0c10b911625aa7d0afd7747de14
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419525"
---
# <a name="self-tracking-entities"></a>Kendi kendini izleyen varlıklar

> [!IMPORTANT]
> Artık kendi kendine izleme varlıkları şablonu kullanmanızı önermiyoruz. Yalnızca varolan uygulamaları desteklemek için kullanılabilir olmaya devam edecektir. Uygulamanız, varlıkların bağlantısı kesilmiş grafiklerle çalışmayı gerektiriyorsa, topluluk tarafından daha aktif olarak geliştirilen Self-Tracking-Tntities'a benzer bir teknoloji olan [İzlenebilir Varlıklar](https://trackableentities.github.io/)veya düşük düzeyli değişiklik izleme API'lerini kullanarak özel kod yazma gibi diğer alternatifleri göz önünde bulundurun.

Varlık Çerçevesi tabanlı bir uygulamada, nesnelerinizdeki değişiklikleri izlemekiçin bir bağlam sorumludur. Daha sonra veritabanındaki değişiklikleri sürdürmek için SaveChanges yöntemini kullanırsınız. N-Tier uygulamalarıyla çalışırken, varlık nesneleri genellikle bağlamdan kesilir ve değişiklikleri nasıl izleyeceğinize ve bu değişiklikleri bağlama geri bildirmeye karar vermeniz gerekir. Kendi Kendini İzleme Varlıkları (STEs) herhangi bir katmandaki değişiklikleri izlemenize ve sonra bu değişiklikleri kaydedilecek bir bağlamda yeniden oynatmanıza yardımcı olabilir.  

STEs'i yalnızca bağlam nesne grafiğinde yapılan değişikliklerin yapıldığı bir katmanda kullanılamıyorsa kullanın. Bağlam varsa, bağlam değişiklikleri izleme yi ilgilenecek, çünkü STS'leri kullanmaya gerek yoktur.  

Bu şablon öğesi iki .tt (metin şablonu) dosyası oluşturur:  

- **Model adı\>.tt dosyası, kendi kendini izleyen varlıklar tarafından kullanılan değişiklik izleme mantığını ve kendi kendine izleme varlıkları üzerinde durum ayarlamasını sağlayan uzantı yöntemlerini içeren varlık türlerini ve yardımcı sınıfını oluşturur. \<**  
- ** \<Model adı.\> Context.tt** dosyası, **ObjectContext** ve **ObjectSet** sınıfları için **Uygulamalı Değişiklikler** yöntemlerini içeren türetilmiş bir bağlam ve uzantı sınıfı oluşturur. Bu yöntemler, veritabanındaki değişiklikleri kaydetmek için gerçekleştirilmesi gereken işlem kümesini ortaya çıkarmak için kendi kendini izleyen varlıkların grafiğinde bulunan değişiklik izleme bilgilerini inceler.  

## <a name="get-started"></a>Başlarken  

Başlamak [için, Kendi Kendine İzleme Varlıklar Walkthrough](walkthrough.md) sayfasını ziyaret edin.  

## <a name="functional-considerations-when-working-with-self-tracking-entities"></a>Kendi Kendini Takip Eden Varlıklarla Çalışırken İşlevsel Hususlar  
> [!IMPORTANT]
> Artık kendi kendine izleme varlıkları şablonu kullanmanızı önermiyoruz. Yalnızca varolan uygulamaları desteklemek için kullanılabilir olmaya devam edecektir. Uygulamanız, varlıkların bağlantısı kesilmiş grafiklerle çalışmayı gerektiriyorsa, topluluk tarafından daha aktif olarak geliştirilen Self-Tracking-Tntities'a benzer bir teknoloji olan [İzlenebilir Varlıklar](https://trackableentities.github.io/)veya düşük düzeyli değişiklik izleme API'lerini kullanarak özel kod yazma gibi diğer alternatifleri göz önünde bulundurun.

Kendi kendini izleyen varlıklarla çalışırken aşağıdakileri göz önünde bulundurun:  

- İstemci projenizin varlık türlerini içeren derlemeye bir başvurusu olduğundan emin olun. İstemci projeye yalnızca hizmet başvurusu eklerseniz, istemci proje gerçek kendi kendini izleyen varlık türlerini değil, WCF proxy türlerini kullanır. Bu, istemcideki varlıkların izlenmesini yöneten otomatik bildirim özelliklerini almeyeceğiniz anlamına gelir. Varlık türlerini kasıtlı olarak eklemek istemiyorsanız, değişikliklerin hizmete geri gönderilmesi için istemcideki değişiklik izleme bilgilerini el ile ayarlamanız gerekir.  
- Hizmet işlemine yapılan çağrılar devletsiz olmalı ve nesne bağlamının yeni bir örneğini oluşturmalı. Ayrıca, nesne bağlamı **kullanarak** bir blok oluşturmanızı öneririz.  
- İstemci üzerinde değiştirilen grafiği hizmete gönderdiğinizde ve ardından istemci üzerinde aynı grafikle çalışmaya devam etmeyi planladığında, değişiklik izleyicisini sıfırlamak için grafikte el ile yinele ve her nesnedeki **AcceptChanges** yöntemini aramanız gerekir.  

    > Grafiğinizdeki nesneler veritabanı tarafından oluşturulan değerlere (örneğin, kimlik veya eşzamanlılık değerleri) sahip özellikler içeriyorsa, Varlık Çerçevesi, **SaveChanges** yöntemi çağrıldıktan sonra bu özelliklerin değerlerini veritabanı tarafından oluşturulan değerlerle değiştirir. Kaydedilen nesneleri döndürmek için hizmet işleminizi veya istemciye geri dönen nesneler için oluşturulan özellik değerlerinin listesini uygulayabilirsiniz. İstemci daha sonra nesne örnekleri veya nesne özellik değerlerini hizmet işleminden döndürülen nesneler veya özellik değerleriyle değiştirmesi gerekir.  
- Birden çok hizmet isteğinden grafiklerin birleştirilmesi, ortaya çıkan grafikte yinelenen anahtar değerleri olan nesneleri tanıtabilir. Entity Framework, **ApplyChanges** yöntemini aradiğinizde yinelenen anahtarlarla nesneleri kaldırmaz, bunun yerine bir özel durum atar. Yinelenen anahtar değerlere sahip grafiklerin olmasını önlemek için aşağıdaki blogda açıklanan desenlerden birini izleyin: [Kendi Kendine İzleme Varlıkları: Değişiklikleri Uygula ve yinelenen varlıklar.](https://go.microsoft.com/fwlink/?LinkID=205119&clcid=0x409)  
- Yabancı anahtar özelliğini ayarlayarak nesneler arasındaki ilişkiyi değiştirdiğinizde, başvuru navigasyon özelliği null olarak ayarlanır ve istemcideki ilgili asıl varlıkla eşitlenmemiş olarak ayarlanır. Grafik nesne bağlamına iliştirildikten sonra (örneğin, **Değişiklikleri Uygula** yöntemini aradıktan sonra), yabancı anahtar özellikleri ve gezinti özellikleri eşitlenir.  

    > Yabancı anahtar ilişkisinde basamaklı silme yi belirttiyseniz, uygun ana nesneyle eşitlenmiş bir başvuru gezinti özelliğinin olmaması sorun olabilir. Anaparayı silerseniz, silme bağımlı nesnelere yayılmaz. Belirtilen basamaklı silmeleriniz varsa, yabancı anahtar özelliğini ayarlamak yerine ilişkileri değiştirmek için gezinti özelliklerini kullanın.  
- Kendi kendine izleme varlıklar tembel yükleme gerçekleştirmek için etkin değildir.  
- ASP.NET durum yönetimi nesneleri için ikili serileştirme ve serileştirme kendi kendini izleyen varlıklar tarafından desteklenmez. Ancak, ikili serileştirme desteği eklemek için şablonu özelleştirebilirsiniz. Daha fazla bilgi için, [Kendi Kendini İzleme Varlıkları ile İkili Serileştirme ve ViewState kullanma](https://go.microsoft.com/fwlink/?LinkId=199208)bakın.  

## <a name="security-considerations"></a>Güvenlikle İlgili Dikkat Edilmesi Gerekenler  

Kendi kendini izleyen varlıklarla çalışırken aşağıdaki güvenlik hususları göz önünde bulundurulmalıdır:  

- Bir hizmet, güvenilmeyen bir istemciden veya güvenilmeyen bir kanaldan veri alma veya güncelleştirme isteklerine güvenmemelidir. İstemcinin kimliği doğrulanmalıdır: güvenli bir kanal veya ileti zarfı kullanılmalıdır. İstemcilerin verileri güncelleştirme veya alma istekleri, verilen senaryo için beklenen ve meşru değişikliklere uyduklarından emin olmak için doğrulanmalıdır.  
- Hassas bilgileri varlık anahtarı (örneğin, sosyal güvenlik numaraları) olarak kullanmaktan kaçının. Bu, kendi kendine izleme varlık grafiklerinde bulunan ve tam olarak güvenilmeyen bir istemciye, farkında olmadan hassas bilgileri serileştirme olasılığını azaltır. Bağımsız ilişkilendirmelerle, serihale edilen varlıkla ilgili bir varlığın özgün anahtarı istemciye de gönderilebilir.  
- Önemli verileri istemci katmanına içeren özel durum iletilerinin yayılmasını önlemek için, sunucu katmanındaki **Değişiklikleri Uygula** ve **Kaydet** çağrıları özel durum işleme koduna sarılı olmalıdır.  
