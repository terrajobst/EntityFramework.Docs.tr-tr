---
title: EF Designer modelleri - EF6 için Entity Framework çalışma zamanı sürümü seçme
author: divega
ms.date: 2016-10-23
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 8864bb8166a7c16455d5c3bebe91e2ce8d142685
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998248"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>EF Designer modelleri için Entity Framework çalışma zamanı sürüm seçme
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.

Aşağıdaki ekranda EF6'ile başlayan EF modeli oluşturulurken hedeflemek istediğiniz çalışma zamanı sürümü seçmenizi sağlayacak için tasarımcıya eklendi. Projede Entity Framework'ün en son sürümü zaten yüklü değilken ekranı görünür. En son sürümü zaten yüklüyse yalnızca varsayılan olarak kullanılır.

![Ekran](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>EF6.x hedefleme

EF6 çalışma zamanı projenize eklemek için 'Your sürümü seçin' ekranından EF6'ı seçebilirsiniz. EF6 ekledikten sonra bu ekran geçerli projedeki görmeye durdurulacak.

Bu yana (aynı proje çalışma zamanının birden çok sürümlerini hedefleyemezsiniz) yüklü EF daha eski bir sürümü zaten varsa EF6 devre dışı bırakılır. EF6 seçeneği burada etkin değilse, projeniz için EF6 yükseltmek için aşağıdaki adımları izleyin:

1.  Çözüm Gezgini'nde projenize sağ tıklayıp **NuGet paketlerini Yönet...**
2.  Seçin **güncelleştirmeleri**
3.  Seçin **EntityFramework** (giderek istediğiniz sürüme güncelleştirmek için emin olun)
4.  Tıklayın **güncelleştirme**

 

## <a name="targeting-ef5x"></a>EF5.x hedefleme

EF5 çalışma zamanı projenize eklemek için 'Your sürümü seçin' ekranından EF5 seçebilirsiniz. EF5 ekledikten sonra ekran devre dışı EF6 seçeneğiyle görürsünüz.

Zaten yüklü olan çalışma zamanı bir EF4.x sürümü varsa, bu ekran EF5 yerine listelenen EF sürümünü görürsünüz. Bu durumda, yükseltme EF5 için aşağıdaki adımları kullanarak:

1.  Seçin **Araçları -&gt; kitaplık Paket Yöneticisi -&gt; Paket Yöneticisi Konsolu**
2.  Çalıştırma **Install-Package EntityFramework-sürüm 5.0.0**

 

## <a name="targeting-ef4x"></a>EF4.x hedefleme

Aşağıdaki adımları kullanarak projenize EF4.x çalışma zamanı yükleyebilirsiniz:

1.  Seçin **Araçları -&gt; kitaplık Paket Yöneticisi -&gt; Paket Yöneticisi Konsolu**
2.  Çalıştırma **Install-Package EntityFramework-sürüm 4.3.0**
