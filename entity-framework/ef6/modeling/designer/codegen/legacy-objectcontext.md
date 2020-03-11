---
title: Entity Framework Designer-EF6 ' de ObjectContext 'e geri dönme
author: divega
ms.date: 10/23/2016
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
ms.openlocfilehash: 3e436f0d9cf94720be9c424b327816438d571ae8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418660"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Entity Framework Designer içinde ObjectContext 'e geri döndürülüyor
Entity Framework önceki sürümüyle EF Designer ile oluşturulan bir model, ObjectContext 'ten türetilmiş bir bağlam ve EntityObject öğesinden türetilen varlık sınıflarından oluşur.

EF 4.1 ile başlayarak, DbContext ve POCO varlık sınıflarından türeten bir içerik üreten bir kod oluşturma şablonuna değiştirmeyi öneririz.

Visual Studio 2012 ' de, EF Designer ile oluşturulan tüm yeni modeller için varsayılan olarak oluşturulan DbContext kodu alırsınız. DbContext tabanlı kod oluşturucuya takas etmeye karar vermediğiniz durumlar dışında mevcut modeller ObjectContext tabanlı kod oluşturmaya devam edecektir.

## <a name="reverting-back-to-objectcontext-code-generation"></a>ObjectContext kod oluşturmaya geri döndürülüyor

### <a name="1-disable-dbcontext-code-generation"></a>1. DbContext kodu oluşturmayı devre dışı bırak

Türetilmiş DbContext ve POCO sınıflarının oluşturulması projenizdeki iki. tt dosyası tarafından işlenirse, Çözüm Gezgini 'nde. edmx dosyasını genişletirseniz bu dosyalar görüntülenir. Bu dosyaların her ikisini de projenizden silin.

![Kod genel dosyaları](~/ef6/media/codegenfiles.png)

VB.NET kullanıyorsanız, iç içe geçmiş dosyaları görmek için **tüm dosyaları göster** düğmesini seçmeniz gerekir.

![Tüm dosyaları göster](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. ObjectContext kod üretimini yeniden etkinleştirin

Bu modeli, EF tasarımcısında açın, Tasarım yüzeyinde boş bir bölüme sağ tıklayıp **Özellikler**' i seçin.

Özellikler penceresi, **kod oluşturma stratejisini** **none** ' dan **varsayılana**değiştirin.

![Kod genel stratejisi](~/ef6/media/codegenstrategy.png)
