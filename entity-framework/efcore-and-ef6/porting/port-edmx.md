---
title: EF6 'den EF Core taşıma-EDMX tabanlı model-EF ile taşıma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182058"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>EF6 EDMX tabanlı bir modelin EF Core taşıma

EF Core modeller için EDMX dosya biçimini desteklemez. Bu modellerin bağlantı noktası için en iyi seçenek, uygulamanız için veritabanından yeni bir kod tabanlı model oluşturmak içindir.

## <a name="install-ef-core-nuget-packages"></a>EF Core NuGet paketlerini yükler

@No__t-0 NuGet paketini yükler.

## <a name="regenerate-the-model"></a>Modeli yeniden oluştur

Artık, mevcut veritabanınıza dayalı bir model oluşturmak için ters mühendislik işlevini kullanabilirsiniz.

Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın (Araçlar – > NuGet Paket Yöneticisi – > Paket Yöneticisi konsolu). Tabloların bir alt kümesini kankatlamak için bkz. komut seçenekleri için [Paket Yöneticisi Konsolu (Visual Studio)](../../core/miscellaneous/cli/powershell.md) .

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Örneğin, SQL Server LocalDB örneğinizdeki blog veritabanından bir modeli dolandırın bir komut aşağıda verilmiştir.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>EF6 modelini kaldır

Artık EF6 modelini uygulamanızdan kaldırırsınız.

EF Core ve EF6 'nin aynı uygulamada yan yana kullanılabilmesi için, EF6 NuGet paketini (EntityFramework) yüklü bırakmak çok iyidir. Ancak, uygulamanızın herhangi bir alanında EF6 kullanmayı hiç bilmiyorsanız, paketin kaldırılması, dikkat edilmesi gereken kod parçalarında derleme hataları sağlamaya yardımcı olur.

## <a name="update-your-code"></a>Kodunuzu güncelleştirme

Bu noktada derleme hatalarının adreslenmesi ve kodu gözden geçirmesinden bağımsız olarak, EF6 ve EF Core arasındaki davranış değişikliklerinin sizi etkileyebileceğini görürsünüz.

## <a name="test-the-port"></a>Bağlantı noktasını test etme

Uygulamanız derlendiğinden, EF Core başarılı bir şekilde bir şekilde bağlantı edildiği anlamına gelmez. Davranış değişikliklerinin hiçbirinin uygulamanızı olumsuz yönde etkilememesini sağlamak için uygulamanızın tüm bölümlerini test etmeniz gerekir.
