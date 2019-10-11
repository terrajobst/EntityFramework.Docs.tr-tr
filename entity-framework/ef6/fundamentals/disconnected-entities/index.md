---
title: Bağlantısı kesilen varlıklarla çalışma-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 12138003-a373-4817-b1b7-724130202f5f
ms.openlocfilehash: f1ce44e7b00ec4c60a81ed850ce5c9d866495e1b
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181762"
---
# <a name="working-with-disconnected-entities"></a>Bağlantısı kesilmiş varlıklarla çalışma
Entity Framework tabanlı bir uygulamada, izlenen varlıklara uygulanan değişikliklerin saptanmasından bağlam sınıfı sorumludur. SaveChanges yöntemini çağırmak, bağlamı tarafından izlenen değişiklikleri veritabanına devam ettirir. N katmanlı uygulamalarla çalışırken, varlık nesneleri genellikle bağlamla bağlantısı kesildiğinde değiştirilir ve değişikliklerin nasıl izleneceğini ve bu değişiklikleri içeriğe geri rapor etme kararı vermeniz gerekir. Bu konuda, bağlantısı kesilen varlıklarla Entity Framework kullanılırken kullanılabilen farklı seçenekler açıklanmaktadır.   

## <a name="web-service-frameworks"></a>Web hizmeti çerçeveleri

Web hizmetleri teknolojileri, genellikle, bağlantısı kesilen nesneler üzerinde değişiklikleri kalıcı hale getirmek için kullanılabilen desenleri destekler. Örneğin, ASP.NET Web API 'SI, bir veritabanındaki bir nesneye yapılan değişiklikleri sürdürmek için EF çağrıları içerebilen kod denetleyici eylemleri sağlar. Aslında, Visual Studio 'da Web API 'SI araçları, Entity Framework 6 modelinizdeki bir Web API denetleyicisinin dağlamayı kolaylaştırır. Daha fazla bilgi için bkz. [Entity Framework 6 Ile Web API 'si kullanma](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/).   

Tarihsel olarak, [WCF veri Hizmetleri](https://docs.microsoft.com/dotnet/framework/data/wcf/create-a-data-service-using-an-adonet-ef-data-wcf) ve [rıa Hizmetleri](https://docs.microsoft.com/previous-versions/dotnet/wcf-ria/ee707344(v=vs.91))gibi Entity Framework tümleştirme Için sunulan başka birçok Web hizmeti teknolojisi de mevcuttur.

## <a name="low-level-ef-apis"></a>Düşük düzey EF API 'Leri

Var olan n katmanlı bir çözümü kullanmak istemiyorsanız veya bir Web API hizmetlerindeki denetleyici eyleminde ne olduğunu özelleştirmek istiyorsanız Entity Framework, bağlantısı kesilen bir katmanda yapılan değişiklikleri uygulamanıza izin veren API 'Ler sağlar. Daha fazla bilgi için bkz. [ekleme, iliştirme ve varlık durumu](~/ef6/saving/change-tracking/entity-state.md).  

## <a name="self-tracking-entities"></a>Kendi kendine Izleme varlıkları  

EF bağlamıyla bağlantısı kesilirken varlıkların rastgele grafiklerde yapılan değişiklikleri izlemek, sabit bir sorundur. Bu uygulamayı çözme denemesinden biri kendi kendini Izleyen varlıkların kod oluşturma şablonudur. Bu şablon, bağlı olmayan bir katmanda, varlıkların kendisinde durum olarak yapılan değişiklikleri izlemek için mantığı içeren varlık sınıfları oluşturur. Bu değişiklikleri bir bağlamda uygulamak için bir genişletme yöntemleri kümesi de oluşturulur.

Bu şablon, EF Designer kullanılarak oluşturulan modellerle birlikte kullanılabilir, ancak Code First modelleriyle kullanılamaz. Daha fazla bilgi için bkz. [kendi kendine Izleme varlıkları](self-tracking-entities/index.md).  

> [!IMPORTANT]
> Artık kendi kendine izleme varlıkları şablonunu kullanmanızı önermiyoruz. Yalnızca var olan uygulamaları desteklemek için kullanılabilir olmaya devam edecektir. Uygulamanız, bağlantısı kesilen varlıkların, topluluk tarafından daha etkin bir şekilde geliştirilen veya yazma gibi, kendi kendini Izlemeye benzer bir teknoloji olan, [izleyicileri](https://trackableentities.github.io/)olan diğer alternatifleri göz önünde bulundurun. alt düzey değişiklik izleme API 'Lerini kullanan özel kod.
