---
title: EF6 - Model oluşturma
author: divega
ms.date: 2018-07-05
ms.assetid: 4890228E-CEA1-4595-B8AD-CA81253F8767
ms.openlocfilehash: c4455da306f4dd1defa0e273123e4e5e2949e5d7
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250796"
---
# <a name="creating-a-model"></a>Model Oluşturma

EF modeli, veritabanı tabloları ve sütunları için uygulama sınıfları ve özellikleri nasıl eşleştiği hakkında ayrıntılı bilgi depolar. EF modeli oluşturmak için iki ana yolu vardır:

- **Code First kullanarak**: Geliştirici modelini belirtmek için bir kod yazar. EF modelleri ve eşlemelerin varlık sınıflarını temel çalışma zamanı ve geliştirici tarafından sağlanan ek modeli yapılandırması oluşturur.

- **EF Designer kullanarak**: Geliştirici kutuları ve EF Designer kullanarak model belirtmek için bir satır çizer. Sonuç olarak oluşan model EDMX uzantılı bir dosyaya XML olarak depolanır. Uygulama etki alanı nesnelerini genellikle kavramsal modelden otomatik olarak oluşturulur.

## <a name="ef-workflows"></a>EF iş akışları

Bu yaklaşımların her ikisi de mevcut bir veritabanını hedefleyen veya 4 farklı akışlarında kaynaklanan yeni bir veritabanı oluşturmak için kullanılabilir.
İlgili bir sizin için en uygun olduğunu öğrenin:  

|                                           | Yalnızca kod yazmak istiyorum...                                                                                                                   | Bir tasarımcı kullanmak istediğiniz...                                                                                                                        |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| **Yeni veritabanı oluşturuluyor**          | [Kullanım **Code First** modelinizi kodun içinde tanımlamak ve ardından bir veritabanı oluşturun.](~/ef6/modeling/code-first/workflows/new-database.md)           | [Kullanım **modeli ilk** kutuları ve satırları kullanarak modelinizi tanımlayın ve ardından bir veritabanı oluşturun.](~/ef6/modeling/designer/workflows/model-first.md)   |
| **Mevcut bir veritabanına erişmek istiyorum** | [Kullanım **Code First** mevcut bir veritabanına eşleyen bir göre kod modeli oluşturun.](~/ef6/modeling/code-first/workflows/existing-database.md) | [Kullanım **veritabanı ilk** mevcut bir veritabanına eşler kutuları ve satırları modeli oluşturun.](~/ef6/modeling/designer/workflows/database-first.md) |

### <a name="watch-the-video-what-ef-workflow-should-i-use"></a>Videoyu izleyin: hangi EF iş akışı kullanmalıyım?

Bu kısa video farklar ve sizin için doğru olanı bulmak nasıl açıklar.

**Tarafından sunulan**: [Rowan Miller](http://romiller.com/)

![Hangi iş akışı Thumb](../media/whichworkflow-thumb.png) [WMV](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.wmv) | [MP4](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_mp4video_ChoseYourWorkflow.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.zip)

Sonra hala EF Designer veya Code First kullanmak istediğinize karar rahat hissine kapılmayın videoyu varsa, her ikisi de öğrenin!

## <a name="a-look-under-the-hood"></a>Başlık altında bir görünüm

Code First veya EF Designer kullanmadığınıza bakılmaksızın, EF modeli her zaman birçok bileşenden oluşur:

- Uygulama etki alanı nesnelerini veya varlık kendilerini yazar. Bu genellikle nesne katmanı olarak adlandırılır

- Etki alanına özgü varlık türleri ve ilişkileri kullanarak açıklanan oluşan kavramsal bir modeli [varlık veri modeli](~/ef6/resources/glossary.md#entity-data-model). Bu katman, genellikle "C" harfi için adlandırılır _kavramsal_.

- Tabloları, sütunları ve ilişkileri veritabanında tanımlanan temsil eden bir depolama model. Bu katman, genellikle daha sonra "S", için adlandırılır _depolama_.  

- Kavramsal model ve veritabanı şeması arasındaki eşlemeyi. Bu eşleme, genellikle "C-S" eşlemesi olarak da adlandırılır.

"C-S" varlıklar üzerinde işlemler dönüştürme - gibi oluşturma, okuma, güncelleştirme ve silme - eşdeğer işlemler veritabanındaki tablolar karşı eşleme EF'ın eşleme altyapısından yararlanır.

Kavramsal model ve uygulamanın nesneler arasındaki eşleme, genellikle "O-C" eşlemesi olarak da adlandırılır. "C-S" eşleme karşılaştırıldığında, "O-C" eşleme örtük ve bire bir: varlıklar, özellikler ve ilişkiler kavramsal modelde tanımlı şekiller ve .NET nesne türleri ile eşleşecek biçimde gereklidir. EF4 ve sonrasında, nesneleri katmanı Basit Nesne EF bağımlılıkları olmadan özellikleriyle oluşturulmuş olabilir. Bunlar genellikle düz eski CLR nesnelerini (POCO) olarak adlandırılır ve eşleme türleri ve özellikleri gerçekleştirilir temel kuralları eşleşen adı. Daha önce EF 3. 5'EntityObject türetin gerek kalmadan ve "O-C" eşleme uygulamak için EF öznitelikleri taşıma zorunluluğunu ortadan kaldırarak varlık gibi nesne katmanı için belirli kısıtlamalar vardı.
