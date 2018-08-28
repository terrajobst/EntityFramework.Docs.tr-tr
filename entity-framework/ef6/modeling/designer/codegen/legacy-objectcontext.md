---
title: Entity Framework Tasarımcısı - EF6 ObjectContext geri dönülüyor
author: divega
ms.date: 2016-10-23
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
ms.openlocfilehash: b52bfc36c97e1a3c7cd2d3716feb1ae48c68a56e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997318"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Entity Framework Tasarımcısı'nda ObjectContext geri dönülüyor
EF Designer ile oluşturulan bir model Entity Framework'ün önceki sürümüyle ObjectContext türetilmiş bir bağlam ve EntityObject türetilen varlık sınıfları oluşturur.

EF4.1 ile başlayan DbContext ve POCO varlık sınıflarından türetme bir bağlamı oluşturan kod oluşturma şablonuna takas önerilir.

Visual Studio 2012'de varsayılan olarak EF Designer ile oluşturulan tüm yeni modeller için oluşturulan DbContext kodunu alın. Mevcut modellerde DbContext göre Kod Oluşturucu için takas etmek karar vermediğiniz sürece ObjectContext göre kod üretmek devam eder.

## <a name="reverting-back-to-objectcontext-code-generation"></a>ObjectContext kod oluşturma için geri döndürülüyor

### <a name="1-disable-dbcontext-code-generation"></a>1. DbContext kod oluşturmayı devre dışı bırak

Nesil DbContext ve POCO türetilen sınıfların, projede iki .tt dosyaları tarafından işlenir, Çözüm Gezgini'nde .edmx dosyasını genişletirseniz, bu dosyaları görürsünüz. Bu dosyaların her ikisini de projenizden silin.

![CodeGenFiles](~/ef6/media/codegenfiles.png)

VB.NET kullanıyorsanız seçmeniz gerekecek **tüm dosyaları göster** iç içe geçmiş dosyaları görmek için düğme.

![Showallfıles](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. Yeniden ObjectContext kod oluşturmayı etkinleştir

EF Tasarımcısı'nda model açık sağ tıklayın, boş bir bölüm tasarım yüzeyi ve select **özellikleri**.

Özellikler penceresinde değişiklik **kod oluşturma stratejisi** gelen **hiçbiri** için **varsayılan**.

![CodeGenStrategy](~/ef6/media/codegenstrategy.png)
