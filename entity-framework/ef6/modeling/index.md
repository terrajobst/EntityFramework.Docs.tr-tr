---
title: Model Oluşturma - EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 4890228E-CEA1-4595-B8AD-CA81253F8767
ms.openlocfilehash: bd9843a93121f53518a307c9d2d43b68ae03369c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419755"
---
# <a name="creating-a-model"></a>Model Oluşturma

BIR EF modeli, uygulama sınıfları ve özelliklerinin veritabanı tabloları ve sütunları ile nasıl eşleştiği hakkındaki ayrıntıları depolar. Bir EF modeli oluşturmanın iki ana yolu vardır:

- **Önce Kodu Kullanma**: Geliştirici modeli belirtmek için kod yazar. EF, varlık sınıflarını ve geliştirici tarafından sağlanan ek model yapılandırmasını temel alarak modelleri ve eşlemeleri çalışma zamanında oluşturur.

- **EF Tasarımcısı'nı kullanma**: Geliştirici, EF Designer'ı kullanarak modeli belirtmek için kutular ve çizgiler çizer. Ortaya çıkan model EDMX uzantılı bir dosyada XML olarak depolanır. Uygulamanın etki alanı nesneleri genellikle kavramsal modelden otomatik olarak oluşturulur.

## <a name="ef-workflows"></a>EF iş akışları

Bu yaklaşımların her ikisi de varolan bir veritabanını hedeflemek veya yeni bir veritabanı oluşturmak için kullanılabilir ve bu da 4 farklı iş akışına neden olur.
Hangisinin sizin için en iyi olduğunu öğrenin:  

|                                           | Ben sadece kod yazmak istiyorum ...                                                                                                                   | Ben bir tasarımcı kullanmak istiyorum ...                                                                                                                        |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| **Yeni bir veritabanı oluşturuyorum**          | [Modelinizi kodda tanımlamak ve ardından bir veritabanı oluşturmak için **Önce Kod'u** kullanın.](~/ef6/modeling/code-first/workflows/new-database.md)           | [Modellerinizi kutuları ve satırları kullanarak tanımlamak ve ardından bir veritabanı oluşturmak için **Önce Model'i** kullanın.](~/ef6/modeling/designer/workflows/model-first.md)   |
| **Varolan bir veritabanına erişmem gerekiyor** | [Varolan bir veritabanıyla eşlenebilen kod tabanlı bir model oluşturmak için **Önce Kod'u** kullanın.](~/ef6/modeling/code-first/workflows/existing-database.md) | [Varolan bir veritabanıyla eşlem yapan bir kutu ve satır modeli oluşturmak için **Önce Veritabanı'nı** kullanın.](~/ef6/modeling/designer/workflows/database-first.md) |

### <a name="watch-the-video-what-ef-workflow-should-i-use"></a>Videoyu izleyin: Hangi EF iş akışını kullanmalıyım?

Bu kısa video farklılıkları açıklar ve nasıl sizin için doğru olanı bulmak için.

**Sunan**: [Rowan Miller](https://romiller.com/)

![Hangi İş](../media/whichworkflow-thumb.png) Akışı Başparmak [WMV](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.wmv) | [MP4](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_mp4video_ChoseYourWorkflow.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.zip)

Videoyu izledikten sonra, EF Designer veya Code First'i kullanmak isteyip istemediğinizi karar verirken kendinizi hala rahat hissetmiyorsanız, her ikisini de öğrenin!

## <a name="a-look-under-the-hood"></a>Kaputun altına bir bakış

Önce Kod'u veya EF Tasarımcısı'nı kullanıp kullanmadığınıza bakılmaksızın, bir EF modelinin her zaman birkaç bileşeni vardır:

- Uygulamanın etki alanı nesneleri veya varlık türleri kendilerini. Bu genellikle nesne katmanı olarak adlandırılır

- [Varlık Veri Modeli](~/ef6/resources/glossary.md#entity-data-model)kullanılarak açıklanan etki alanına özgü varlık türleri ve ilişkilerinden oluşan kavramsal bir model. Bu katman genellikle _"C"_ harfi ile, kavramsal için adlandırılır.

- Veritabanında tanımlandığı gibi tabloları, sütunları ve ilişkileri temsil eden bir depolama modeli. Bu katman genellikle _depolama_için, daha sonra "S" ile adlandırılır.  

- Kavramsal model ve veritabanı şeması arasında bir eşleme. Bu eşleme genellikle "C-S" eşleme olarak adlandırılır.

EF'nin haritalama motoru, veritabanındaki tablolara karşı eşdeğer işlemlere dönüştürmek için "C-S" eşlemeden yararlanır ( örneğin oluşturma, okuma, güncelleme ve silme gibi varlıklara karşı işlemleri dönüştürür.

Kavramsal model ve uygulamanın nesneleri arasındaki eşleme genellikle "O-C" eşleme olarak adlandırılır. "C-S" eşlemeile karşılaştırıldığında, "O-C" eşleme örtülüdür ve bire birdir: kavramsal modelde tanımlanan varlıklar, özellikler ve ilişkiler .NET nesnelerinin şekil ve türlerine uymak için gereklidir. EF4 ve ötesinden, nesne katmanı EF'ye herhangi bir bağımlılık olmaksızın özellikleri olan basit nesnelerden oluşabilir. Bunlar genellikle Düz-Eski CLR Nesneleri (POCO) olarak adlandırılır ve tür ve özelliklerin eşleme ad eşleştirme kuralları temel yapılır. Daha önce, EF 3.5'te nesne katmanı için entityobject sınıfından türemiş varlıklar ve "O-C" eşlemesi uygulamak için EF özniteliklerini taşımak zorunda kalmak gibi belirli kısıtlamalar vardı.
