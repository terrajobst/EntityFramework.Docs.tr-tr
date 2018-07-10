---
title: Entity Framework Sözlüğü - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
caps.latest.revision: 3
ms.openlocfilehash: 8a06cfb2dbe79364c3f5cc21ecb32fd60d239e8a
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914108"
---
# <a name="entity-framework-glossary"></a>Entity Framework Sözlüğü
## <a name="code-first"></a>İlk kod
Kod kullanarak bir Entity Framework modelini oluşturma. Model hedefleyebilir ve var olan bir veritabanını veya yeni bir veritabanı.

## <a name="context"></a>Bağlam
Sorgu ve veri kaydetmenize olanak sağlayan, veritabanı ile bir oturumu temsil eden sınıf. Bir bağlam DbContext veya ObjectContext sınıfından türetilir.

## <a name="convention-code-first"></a>Kuralı (ilk kodu)
Sınıflarınızı modelden şeklini çıkarsamak için Entity Framework kullanan bir kural.

## <a name="database-first"></a>İlk veritabanı
EF Designer kullanarak bir Entity Framework modeli oluşturma, varolan bir veritabanını hedefler.

## <a name="eager-loading"></a>İstekli yükleme
Burada bir varlık türü için sorgu ayrıca ilgili varlıkları sorgunun bir parçası yükler ilgili veri yükleme deseni.

## <a name="ef-designer"></a>EF Designer
Bir görsel tasarımcı Visual Studio'da, kutuları ve satırları kullanarak bir Entity Framework modelini oluşturmanıza olanak sağlar.

## <a name="entity"></a>Varlık
Bir sınıf veya müşteriler, ürünler ve siparişler gibi uygulama verilerini temsil eden nesne.

## <a name="entity-data-model"></a>Varlık Veri Modeli
Varlıklar ve aralarındaki ilişkiler açıklanmıştır modeli. EF kullanan EDM kavramsal modelinde açıklamak için geliştirici programları. EDM Dr tarafından sunulan bir varlık ilişkisi modeli oluşturur. Peter Chen. EDM başlangıçta Microsoft gelen Geliştirici ve sunucu teknolojileri paketi arasında ortak veri modeli olma birincil amacı ile geliştirilmiştir. EDM de OData protokolünün bir parçası kullanılır.

## <a name="explicit-loading"></a>Açık yükleme
Bir API çağrısı yaparak ilgili nesneler burada yüklenen ilgili veri yükleme deseni.

## <a name="fluent-api"></a>Fluent API'si
Code First modelini yapılandırmak için kullanılan bir API.

## <a name="foreign-key-association"></a>Yabancı anahtar ilişkilendirmesi
Yabancı anahtar temsil eden bir özellik (yani ürünün CategoryID özelliği içerir) bağımlı varlık sınıfında burada dahil varlıklar arasında ilişkilendirme.

## <a name="identifying-relationship"></a>İlişki tanımlama
Asıl varlığın birincil anahtarı bağımlı varlığın birincil anahtarının bir parçası olduğu bir ilişki. Bu tür ilişkiyi bağımlı varlık asıl varlık var olamaz.

## <a name="independent-association"></a>Bağımsız ilişkilendirme
Varlıklar arasında ilişkilendirme burada yabancı anahtar (yani bir ürün sınıfı kategori ancak hiç CategoryID özelliği arasında bir ilişki içeriyor) bağımlı varlık sınıfına temsil eden bir özellik yok. Entity Framework, bağımsız bir nesne, bu ilişkiyi izlemek için kullanır.

## <a name="lazy-loading"></a>Yavaş yükleniyor
Gezinti özelliğine erişinceye burada ilgili nesneleri otomatik olarak yüklenir, ilgili veri yükleme deseni.

## <a name="model-first"></a>İlk model
EF Designer kullanarak bir Entity Framework modeli oluşturma, ardından yeni bir veritabanı oluşturmak için kullanılır.

## <a name="navigation-property"></a>Gezinme özelliği
Başka bir varlığa başvuran bir varlığın bir özelliği (yani ürün kategorisi gezinti özelliği içerir ve kategorisi ürünleri gezinme özelliğini içerir).

## <a name="poco"></a>POCO
Düz eski CLR nesnesi kısaltması. Basit kullanıcı sınıf herhangi bir çerçeveyi ile bağımlılık yok. EF, bağlamında bir EntityObject türemiyor bir varlık sınıfı her arabirimlerini uygular veya EF tanımlı herhangi bir öznitelik taşır. Kalıcılık çerçevesinden bağımsız çalışabildiğinden böyle bir varlık sınıfları ayrıca "Kalıcılık ignorant" olduğu söylenir.  

## <a name="relationship-inverse"></a>İlişki ters
Örneğin, ürün bir ilişki karşı sonu. Kategori ve kategorisi. Ürün.

## <a name="self-tracking-entity"></a>Kendi kendine varlık izleme
N katmanlı geliştirmeye yardımcı olan kod oluşturma şablondan oluşturulmuş bir varlık.

## <a name="table-per-concrete-type-tpc"></a>Tablo başına somut türünü (TPC)
Veritabanındaki tabloda ayırmak için bir yöntem devralma hiyerarşisinde girildiği soyut olmayan her eşleme eşlenir.

## <a name="table-per-hierarchy-tph"></a>Tablo-başına-hiyerarşi (TPH)
Hiyerarşideki tüm türleri aynı veritabanındaki tabloda burada eşlendiğine devralma eşleme yöntemidir. Sütunları her satırda ne tür tanımlamak için kullanılan bir ayrıştırıcı ilişkilidir.

## <a name="table-per-type-tpt"></a>Tablo başına tür (TPT)
Burada hiyerarşideki tüm türlerin genel özelliklerini veritabanında aynı tabloya eşlenir, ancak her türü için benzersiz özellikler için ayrı bir tabloda eşlenen devralma eşleme yöntemidir.

## <a name="type-discovery"></a>Bulma türü
Entity Framework modelinin bir parçası olması gereken türleri tanımlama işlemi.
