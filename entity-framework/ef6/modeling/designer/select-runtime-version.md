---
title: EF Designer modelleri için Entity Framework çalışma zamanı sürümünü seçme-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418149"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>EF Designer modelleri için Entity Framework çalışma zamanı sürümü seçme
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.

EF6 ile başlayarak, bir model oluştururken hedeflemek istediğiniz çalışma zamanının sürümünü seçmenizi sağlamak için EF tasarımcısına aşağıdaki ekran eklenmiştir. Entity Framework en son sürümü projede zaten yüklü olmadığında ekran görüntülenir. En son sürüm zaten yüklüyse, varsayılan olarak yalnızca kullanılacaktır.

![Ekranınızdaki](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>EF6. x hedefleme

EF6 çalışma zamanını projenize eklemek için ' sürümünüzü seçin ' ekranından EF6 seçeneğini belirleyebilirsiniz. EF6 ekledikten sonra bu ekranı geçerli projede görmekten vazgeçmeniz gerekir.

Zaten EF 'in daha eski bir sürümü yüklüyse, EF6 devre dışı bırakılır (çünkü çalışma zamanının birden çok sürümünü aynı projeden hedefleyemiyorum). EF6 seçeneği burada etkinleştirilmemişse, projenizi EF6 sürümüne yükseltmek için aşağıdaki adımları izleyin:

1.  Çözüm Gezgini ' de projenize sağ tıklayın ve **NuGet Paketlerini Yönet..** . seçeneğini belirleyin.
2.  **Güncelleştirme** seçin
3.  **EntityFramework 'ü** seçin (bunu istediğiniz sürüme güncelleştirdiğinizden emin olun)
4.  **Güncelleştir** 'e tıklayın

 

## <a name="targeting-ef5x"></a>EF5. x hedefleme

EF5 çalışma zamanını projenize eklemek için ' sürümünüzü seçin ' ekranından EF5 seçeneğini belirleyebilirsiniz. EF5 ekledikten sonra ekran EF6 seçeneği devre dışı olarak görünmeye devam eder.

Çalışma zamanının zaten yüklü olduğu bir EF4. x sürümüne sahipseniz, EF5 yerine ekranda listelenen EF sürümünü görürsünüz. Bu durumda, aşağıdaki adımları kullanarak EF5 sürümüne yükseltebilirsiniz:

1.  **Araçlar-&gt; kitaplığı Paket Yöneticisi-&gt; Paket Yöneticisi konsolu** ' nu seçin
2.  **Install-Package EntityFramework-Version 5.0.0** Çalıştır

 

## <a name="targeting-ef4x"></a>EF4. x hedefleme

Aşağıdaki adımları kullanarak EF4. x çalışma zamanını projenize yükleyebilirsiniz:

1.  **Araçlar-&gt; kitaplığı Paket Yöneticisi-&gt; Paket Yöneticisi konsolu** ' nu seçin
2.  **Install-Package EntityFramework-Version 4.3.0** 'ı Çalıştır
