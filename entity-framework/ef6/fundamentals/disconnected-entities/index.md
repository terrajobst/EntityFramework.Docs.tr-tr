---
title: Bağlantısı kesilen varlıklarla çalışma - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 12138003-a373-4817-b1b7-724130202f5f
ms.openlocfilehash: f1ce44e7b00ec4c60a81ed850ce5c9d866495e1b
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419528"
---
# <a name="working-with-disconnected-entities"></a>Bağlantısı kesilen varlıklarla çalışma
Varlık Çerçevesi tabanlı bir uygulamada, izlenen varlıklara uygulanan değişiklikleri algılamaktan bir bağlam sınıfı sorumludur. SaveChanges yöntemini çağırmak, bağlam tarafından izlenen değişiklikleri veritabanına kadar devam eder. N katmanlı uygulamalarla çalışırken, varlık nesneleri genellikle bağlamdan bağlantısı kesilirken değiştirilir ve değişiklikleri nasıl izleyeceğinize ve bu değişiklikleri içeriğe nasıl bildireceğine karar vermeniz gerekir. Bu konu, bağlantısı kesilen varlıklarla Varlık Çerçevesi kullanırken kullanılabilen farklı seçenekleri tartışır.   

## <a name="web-service-frameworks"></a>Web hizmeti çerçeveleri

Web hizmetleri teknolojileri genellikle tek tek bağlantısı kesilen nesnelerdeki değişiklikleri sürdürmek için kullanılabilecek desenleri destekler. Örneğin, ASP.NET Web API, veritabanındaki bir nesnede yapılan değişiklikleri sürdürmek için EF'ye yapılan çağrıları da içerebilen denetleyici eylemlerini kodlayanıza olanak tanır. Aslında, Visual Studio'daki Web API aracılama, Entity Framework 6 modelinizden bir Web API denetleyicisi satın almanızı kolaylaştırır. Daha fazla bilgi için, [Entity Framework 6 ile Web API'yi kullanarak](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/)bakın.   

Tarihsel olarak, [WCF Veri Hizmetleri](https://docs.microsoft.com/dotnet/framework/data/wcf/create-a-data-service-using-an-adonet-ef-data-wcf) ve [RIA Hizmetleri](https://docs.microsoft.com/previous-versions/dotnet/wcf-ria/ee707344(v=vs.91))gibi Entity Framework ile entegrasyon sunan birkaç diğer Web hizmetleri teknolojileri olmuştur.

## <a name="low-level-ef-apis"></a>Düşük seviyeli EF API'leri

Varolan bir n katmanlı çözüm kullanmak istemiyorsanız veya bir Web API hizmetlerinde denetleyici eyleminde neler olduğunu özelleştirmek istiyorsanız, Entity Framework bağlantısı kesilmiş bir katmanda yapılan değişiklikleri uygulamanıza olanak tanıyan API'ler sağlar. Daha fazla bilgi için [bkz.](~/ef6/saving/change-tracking/entity-state.md)  

## <a name="self-tracking-entities"></a>Kendi Kendini Takip Eden Varlıklar  

EF bağlamından bağlantısı kesilirken varlıkların rasgele grafikleri üzerindeki değişiklikleri izlemek zor bir sorundur. Bunu çözme girişimlerinden biri de Kendi Kendine İzleme Varlıklar kod oluşturma şablonuydu. Bu şablon, bağlantısı kesilmiş bir katmanda yapılan değişiklikleri varlıkların kendilerinde durum olarak izlemek için mantık içeren varlık sınıfları oluşturur. Bu değişiklikleri bir içeriğe uygulamak için bir dizi uzantı yöntemi de oluşturulur.

Bu şablon, EF Designer kullanılarak oluşturulan modellerde kullanılabilir, ancak Code First modelleri ile kullanılamaz. Daha fazla bilgi için, [Kendi Kendini İzleme Varlıkları'na](self-tracking-entities/index.md)bakın.  

> [!IMPORTANT]
> Artık kendi kendine izleme varlıkları şablonu kullanmanızı önermiyoruz. Yalnızca varolan uygulamaları desteklemek için kullanılabilir olmaya devam edecektir. Uygulamanız, varlıkların bağlantısı kesilmiş grafiklerle çalışmayı gerektiriyorsa, topluluk tarafından daha aktif olarak geliştirilen Self-Tracking-Tntities'a benzer bir teknoloji olan [İzlenebilir Varlıklar](https://trackableentities.github.io/)veya düşük düzeyli değişiklik izleme API'lerini kullanarak özel kod yazma gibi diğer alternatifleri göz önünde bulundurun.
