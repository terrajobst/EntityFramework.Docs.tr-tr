---
title: Entity Framework Tasarımcısı - EF6 ObjectContext geri dönülüyor
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
caps.latest.revision: 3
ms.openlocfilehash: 17fa6538cf5e92f59ab72376af96ad65c640a085
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912789"
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
