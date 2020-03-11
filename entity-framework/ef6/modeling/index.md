---
title: Model oluşturma-EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 4890228E-CEA1-4595-B8AD-CA81253F8767
ms.openlocfilehash: bd9843a93121f53518a307c9d2d43b68ae03369c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419755"
---
# <a name="creating-a-model"></a>Model Oluşturma

EF modeli, uygulama sınıflarının ve özelliklerinin veritabanı tablolarına ve sütunlarına nasıl eşlenme hakkındaki ayrıntıları depolar. EF modeli oluşturmanın iki ana yöntemi vardır:

- **Code First kullanımı**: geliştirici, modeli belirtmek için kod yazar. EF, çalışma zamanında model ve eşlemeleri varlık sınıflarına ve geliştirici tarafından belirtilen ek model yapılandırmasına göre oluşturur.

- **EF tasarımcısını kullanarak**: GELIŞTIRICI, EF tasarımcısını kullanarak modeli belirtmek için kutular ve çizgiler çizer. Elde edilen model, EDMX uzantılı bir dosyada XML olarak depolanır. Uygulamanın etki alanı nesneleri, genellikle kavramsal modelden otomatik olarak oluşturulur.

## <a name="ef-workflows"></a>EF iş akışları

Bu yaklaşımlardan her ikisi de mevcut bir veritabanını hedeflemek veya yeni bir veritabanı oluşturmak için kullanılabilir ve bu da 4 farklı iş akışı elde edilir.
Sizin için en uygun olanı öğrenin:  

|                                           | Yalnızca kod yazmak istiyorum...                                                                                                                   | Tasarımcı kullanmak istiyorum...                                                                                                                        |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| **Yeni bir veritabanı oluşturdum**          | [Modelinizi kodda tanımlamak ve sonra bir veritabanı oluşturmak için **Code First** kullanın.](~/ef6/modeling/code-first/workflows/new-database.md)           | [Kutuları ve çizgileri kullanarak modelinizi tanımlamak ve sonra bir veritabanı oluşturmak için **model First** kullanın.](~/ef6/modeling/designer/workflows/model-first.md)   |
| **Var olan bir veritabanına erişebilmem gerekiyor** | [Mevcut bir veritabanıyla eşleşen kod tabanlı bir model oluşturmak için **Code First** kullanın.](~/ef6/modeling/code-first/workflows/existing-database.md) | [Mevcut bir veritabanıyla eşleşen kutular ve satırlar modeli oluşturmak için **Database First** kullanın.](~/ef6/modeling/designer/workflows/database-first.md) |

### <a name="watch-the-video-what-ef-workflow-should-i-use"></a>Videoyu izleyin: hangi EF iş akışını kullanmalıyım?

Bu kısa videoda, farklar ve sizin için uygun olanı bulma açıklanmaktadır.

**Sunulma ölçütü**: [Rowa Miller](https://romiller.com/)

![](../media/whichworkflow-thumb.png) [wmv](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.wmv) | [MP4](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_mp4video_ChoseYourWorkflow.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.zip) iş akışı parmak izi

Videoyu izlerken, EF tasarımcısını veya Code First kullanmak istiyorsanız, her ikisini de öğrenin!

## <a name="a-look-under-the-hood"></a>Ele bir görünüm

Code First veya EF Designer ' ı kullanıp kullanmadığına bakılmaksızın bir EF modelinin her zaman birkaç bileşeni vardır:

- Uygulamanın etki alanı nesneleri veya varlık türleri. Bu, genellikle nesne katmanı olarak adlandırılır

- [Varlık veri modeli](~/ef6/resources/glossary.md#entity-data-model)kullanarak açıklanan, etki alanına özgü varlık türlerinden ve ilişkilerinden oluşan kavramsal bir model. Bu katmana genellikle _kavramsal_olarak "C" harfiyle başvurulur.

- Veritabanında tanımlanan tabloları, sütunları ve ilişkileri temsil eden bir depolama modeli. Bu katman genellikle _depolama_için daha sonraki "S" ile adlandırılır.  

- Kavramsal model ve veritabanı şeması arasındaki eşleme. Bu eşleme, genellikle "C-S" eşlemesi olarak adlandırılır.

EF 'in eşleme altyapısı, veritabanındaki tablolara yönelik olarak oluşturma, okuma, güncelleştirme ve silme gibi varlıklara karşı işlemleri dönüştürmek için "C-S" eşlemesini kullanır.

Kavramsal model ve uygulamanın nesneleri arasındaki eşleme, genellikle "O-C" eşlemesi olarak adlandırılır. "C-S" eşlemesi ile karşılaştırıldığında, "O-C" eşlemesi örtük ve bire bir ' dir: kavramsal modelde tanımlanan varlıklar, Özellikler ve ilişkiler, .NET nesnelerinin şekillerini ve türlerini eşleştirmek için gereklidir. EF4 ve ötesinde nesneler katmanı, EF üzerinde hiçbir bağımlılığı olmayan özelliklere sahip basit nesnelerden oluşabilir. Bunlar genellikle düz eski CLR nesneleri (POCO) olarak adlandırılır ve türlerin ve özelliklerin eşlemesi ad eşleştirme kuralları üzerinde taban olarak gerçekleştirilir. Daha önce, EF 3,5 ' de, nesne katmanı için, EntityObject sınıfından türetmek zorunda olan ve "O-C" eşlemesini uygulamak için EF özniteliklerini yürütmek zorunda olan varlıklar gibi belirli kısıtlamalar vardır.
