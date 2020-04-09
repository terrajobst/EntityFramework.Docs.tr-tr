---
title: EF6'dan EF Core'a taşıma - EDMX Tabanlı Model Taşıma - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416925"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>EF6 EDMX Tabanlı Modeli EF Core'a Taşıma

EF Core, modeller için EDMX dosya biçimini desteklemez. Bu modelleri taşımanın en iyi seçeneği, uygulamanız için veritabanından yeni bir kod tabanlı model oluşturmaktır.

## <a name="install-ef-core-nuget-packages"></a>EF Core NuGet paketlerini yükleyin

NuGet `Microsoft.EntityFrameworkCore.Tools` paketini yükleyin.

## <a name="regenerate-the-model"></a>Modeli yeniden oluşturma

Artık varolan veritabanınızı temel alan bir model oluşturmak için ters mühendislik işlevini kullanabilirsiniz.

Paket Yöneticisi Konsolu'nda (Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu) aşağıdaki komutu çalıştırın. Tabloların bir alt kümesini iskelek için komut seçenekleri için [Paket Yöneticisi Konsolu 'na (Visual Studio)](../../core/miscellaneous/cli/powershell.md) bakın.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Örneğin, SQL Server LocalDB örneğiniz üzerindeki Blog veritabanından bir modeli iskeleoluşturma komutu burada dır.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>EF6 modelini kaldırma

Şimdi EF6 modelini uygulamanızdan kaldırırsınız.

EF Core ve EF6 aynı uygulamada yan yana kullanılabildiği için EF6 NuGet paketini (EntityFramework) yüklü bırakmak iyidir. Ancak, UYGULAMANIZIN herhangi bir alanında EF6'yı kullanmak istemiyorsanız, paketi kaldırmanız dikkat gerektiren kod parçaları üzerinde derleme hataları na yardımcı olacaktır.

## <a name="update-your-code"></a>Kodunuzu güncelleştirin

Bu noktada, EF6 ve EF Core arasındaki davranış değişikliklerinin sizi etkileyip etkilemeyeceğini görmek için derleme hatalarını ele alma ve kodu gözden geçirme meselesidir.

## <a name="test-the-port"></a>Bağlantı noktasını test edin

Uygulamanızın derlenmiş olması, uygulamanın ef core'a başarıyla iletilmiş olduğu anlamına gelmez. Davranış değişikliklerinin hiçbirinin uygulamanızı olumsuz etkilemediğinden emin olmak için uygulamanızın tüm alanlarını test etmeniz gerekir.
