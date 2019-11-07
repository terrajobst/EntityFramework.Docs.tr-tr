---
title: Entity Framework sözlüğü-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: df0da4a68b3d2c882d9673417ee5fe335eccae2b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656152"
---
# <a name="entity-framework-glossary"></a>Entity Framework sözlüğü
## <a name="code-first"></a>Code First
Kodu kullanarak Entity Framework modeli oluşturma. Model, var olan bir veritabanını veya yeni bir veritabanını hedefleyebilir.

## <a name="context"></a>Bağlam
Veritabanı ile bir oturumu temsil eden ve verileri sorgulamanızı ve kaydetmenizi sağlayan bir sınıf. Bağlam DbContext veya ObjectContext sınıfından türetilir.

## <a name="convention-code-first"></a>Kural (Code First)
Sınıflarınızda modelinizin şeklini çıkarsmak için Entity Framework tarafından kullanılan bir kural.

## <a name="database-first"></a>Database First
Var olan bir veritabanını hedefleyen EF tasarımcısını kullanarak Entity Framework modeli oluşturma.

## <a name="eager-loading"></a>Ekip yükleme
Bir varlık türü için bir sorgunun aynı zamanda ilgili varlıkları sorgunun bir parçası olarak yüklediği ilgili verileri yükleme kalıbı.

## <a name="ef-designer"></a>EF Tasarımcısı
Kutuları ve çizgileri kullanarak bir Entity Framework modeli oluşturmanıza olanak tanıyan Visual Studio 'da görsel tasarımcı.

## <a name="entity"></a>Varlık
Müşteriler, ürünler ve siparişler gibi uygulama verilerini temsil eden bir sınıf veya nesne.

## <a name="entity-data-model"></a>Varlık Veri Modeli
Varlıkları ve aralarındaki ilişkileri açıklayan bir model. EF, geliştirici programlarının yanındaki kavramsal modeli anlatmak için EDM kullanır. EDM derlemeleri Dr. Peter Chen tarafından tanıtılan varlık Ilişki modelinde. EDM, ilk olarak Microsoft 'un bir geliştirici ve sunucu teknolojileri paketi genelinde ortak veri modeli olma konusunda birincil hedefle geliştirilmiştir. EDM, OData protokolünün bir parçası olarak da kullanılır.

## <a name="explicit-loading"></a>Açık yükleme
Bir API çağırarak ilgili nesnelerin yüklendiği ilgili verileri yükleme kalıbı.

## <a name="fluent-api"></a>Akıcı API
Code First modeli yapılandırmak için kullanılabilen bir API.

## <a name="foreign-key-association"></a>Yabancı anahtar ilişkilendirmesi
Bağımlı varlığın sınıfına, yabancı anahtarı temsil eden bir özelliğin dahil olduğu varlıklar arasındaki ilişki. Örneğin, ürün bir CategoryID özelliği içerir.

## <a name="identifying-relationship"></a>İlişki tanımlama
Asıl varlığın birincil anahtarının bağımlı varlığın birincil anahtarının bir parçası olduğu bir ilişki. Bu tür bir ilişkide, bağımlı varlık asıl varlık olmadan bulunamaz.

## <a name="independent-association"></a>Bağımsız ilişkilendirme
Bağımlı varlık sınıfındaki yabancı anahtarı temsil eden özellik olmayan varlıklar arasındaki ilişki. Örneğin, bir ürün sınıfı kategori ile ilişki içerir ancak CategoryID özelliği yoktur. Entity Framework, ilişkinin durumunu iki ilişkilendirme sonunda varlıkların durumundan bağımsız olarak izler.

## <a name="lazy-loading"></a>Geç yükleme
Bir gezinti özelliğine erişildiğinde ilgili nesnelerin otomatik olarak yüklendiği ilgili verileri yükleme kalıbı.

## <a name="model-first"></a>Model First
Yeni bir veritabanı oluşturmak için kullanılan EF tasarımcısını kullanarak Entity Framework modeli oluşturma.

## <a name="navigation-property"></a>Gezinti özelliği
Başka bir varlığa başvuran bir varlığın özelliği. Örneğin, ürün bir kategori gezinti özelliği içerir ve kategori bir ürünler gezinti özelliği içerir.

## <a name="poco"></a>POCO
Düz eski CLR nesnesi kısaltması. Hiçbir çerçeveye bağımlılığı olmayan basit bir Kullanıcı sınıfı. EF bağlamında, EntityObject sınıfından türemeyen bir varlık sınıfı, herhangi bir arabirim uygular veya EF içinde tanımlanan öznitelikleri taşır. Kalıcılık çerçevesinden ayrılan bu tür varlık sınıfları, "Kalıcılık Ignorant" olarak da kabul edilir.  

## <a name="relationship-inverse"></a>İlişki ters
Bir ilişkinin ters sonu (örneğin, ürün). Kategori ve kategori. Ürünüyle.

## <a name="self-tracking-entity"></a>Kendini izleyen varlık
N katmanlı geliştirmeye yardımcı olan kod oluşturma şablonundan oluşturulmuş bir varlık.

## <a name="table-per-concrete-type-tpc"></a>Tablo başına somut tür (TPC)
Hiyerarşideki her özet olmayan türün veritabanındaki ayrı tabloyla eşlendiği devralmayı eşleme yöntemi.

## <a name="table-per-hierarchy-tph"></a>Hiyerarşi başına tablo (TPH)
Hiyerarşideki tüm türlerin veritabanındaki aynı tabloyla eşlendiği devralmayı eşleme yöntemi. Her bir satırın ilişkilendirildiği türü belirlemek için bir Ayrıştırıcı sütunu kullanılır.

## <a name="table-per-type-tpt"></a>Tür başına tablo (TPT)
Hiyerarşideki tüm türlerin ortak özelliklerinin veritabanındaki aynı tabloyla eşlendiği devralmayı eşleme yöntemi, ancak her bir türe özgü özellikler ayrı bir tabloya eşlenir.

## <a name="type-discovery"></a>Tür keşfi
Bir Entity Framework modelinin parçası olması gereken türleri tanımlama işlemi.
