---
title: Bağlantısı kesilmiş varlıklar - EF6 ile çalışma
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 12138003-a373-4817-b1b7-724130202f5f
caps.latest.revision: 3
ms.openlocfilehash: 5419215a77b57ab3c92fb88a512510070ea23bd6
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913450"
---
# <a name="working-with-disconnected-entities"></a>Bağlantısı kesilmiş varlıklar ile çalışma
Entity Framework tabanlı bir uygulama, izlenen varlıklarına uygulanan değişiklikler algılanıyor için bir bağlam sınıfını sorumludur. SaveChanges yöntemini çağırarak veritabanı için bağlam tarafından izlenen değişiklikleri devam ettirir. N katmanlı uygulamalar ile çalışırken, varlık nesnesi genellikle bağlamdan bağlı değilken değiştirilen ve değişiklikleri ve rapor bağlamına bu değişiklikleri izlemek nasıl karar vermeniz gerekir. Bu konu ile Entity Framework kullanarak varlıkları kesildiğinde kullanılabilir farklı seçenekler açıklanır.   

## <a name="web-service-frameworks"></a>Web hizmeti çerçeveleri

Web hizmetleri teknolojileri genellikle ayrı ayrı bağlantısı kesilmiş nesnelerdeki değişiklikleri kalıcı hale getirmek için kullanılan desenleri destekler. Örneğin, ASP.NET Web API, bir nesne bir veritabanı üzerinde yapılan değişiklikleri kalıcı hale getirmek için EF çağrılar dahil edebilirsiniz kodunu denetleyici eylemleri için sağlar. Aslında, Visual Studio Araçları Web API'si, Entity Framework 6 modelinizi bir Web API denetleyicisi iskele oluşturmayı kolaylaştırır. Daha fazla bilgi için [Entity Framework 6 ile Web API kullanarak](https://docs.microsoft.com/en-us/aspnet/web-api/overview/data/using-web-api-with-entity-framework/).   

Tarihsel olarak, olmuştur Entity Framework ile tümleştirme gibi sunulan çeşitli diğer Web hizmetleri teknolojileri [WCF Veri Hizmetleri](https://docs.microsoft.com/dotnet/framework/data/wcf/create-a-data-service-using-an-adonet-ef-data-wcf) ve [RIA Services](https://docs.microsoft.com/en-us/previous-versions/dotnet/wcf-ria/ee707344(v=vs.91)).

## <a name="low-level-ef-apis"></a>Alt düzey EF API'leri

Entity Framework, var olan bir n katmanlı çözüm kullanmak istemiyorsanız veya bir denetleyici eylemi bir Web API hizmetlerindeki içinde özelleştirmek istiyorsanız, bağlantısı kesilmiş bir katmanına yapılan değişiklikleri uygulamak olanak tanıyan API'ler sağlar. Daha fazla bilgi için [Ekle, Ekle ve varlık durumu](~/ef6/saving/change-tracking/entity-state.md).  

## <a name="self-tracking-entities"></a>Kendi kendine izleme varlıkları  

EF bağlamdan bağlı değilken varlıkların rastgele grafiklerinde değişiklikleri izleme sabit bir sorundur. Çözüyor girişimleri Self-Tracking varlıkları kod kuşak şablonu biriydi. Bu şablon durumunda varlıklar olarak bağlantısı kesilmiş bir katmanına yapılan değişiklikleri izlemek için mantığı içeren varlık sınıflar oluşturur. Bir uzantı yöntemi kümesi için bir bağlam bu değişiklikleri uygulamak için de oluşturulur.

Bu şablon EF Tasarımcı kullanılarak oluşturulmuş modelleri ile kullanılabilir, ancak Code First modelleri ile kullanılamaz. Daha fazla bilgi için [Self-Tracking varlıkları](self-tracking-entities/index.md).  

> [!IMPORTANT]
> Artık, self tracking varlıkları şablon kullanmanızı öneririz. Bu yalnızca var olan uygulamaları desteklemek kullanılabilir olmaya devam edecek. Uygulamanızın çalışma bağlantısı kesilmiş varlıklar grafikleriyle gerektiriyorsa, başka alternatifler gibi düşünün [izlenebilir varlıkları](http://trackableentities.github.io/), Self-Tracking-daha etkin bir şekilde tarafından geliştirilen varlıklara benzer bir teknoloji olan Topluluk veya alt düzey değişiklik API'leri izleme kullanarak özel kod yazma.
