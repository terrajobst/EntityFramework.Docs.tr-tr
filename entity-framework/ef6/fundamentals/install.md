---
title: Entity Framework - EF6 Al
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
caps.latest.revision: 4
ms.openlocfilehash: 400bf1428e6754a88dbc1264c346bb66282725a0
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949002"
---
# <a name="get-entity-framework"></a>Entity Framework Al
Entity Framework EF araçlarını Visual Studio ve EF çalışma zamanı için oluşur.

## <a name="ef-tools-for-visual-studio"></a>Visual Studio için EF araçları

Visual Studio için Entity Framework Tools EF Designer ve EF modeli Sihirbazı'nı içerir ve ilk veritabanı için gerekli olan ve ilk iş akışlarını modellemek. EF aracı, tüm yeni Visual Studio sürümlerinde yer almaktadır. Özel bir yükleme öğesi emin olmak için ihtiyacınız olacak Visual Studio'nun gerçekleştirirseniz "Entity Framework 6 Tools" içeren bir iş yükünü seçme veya ayrı ayrı bir bileşen seçerek seçilir.

Bazı Visual Studio sürümlerini güncelleştirilmiş EF Araçlar bir indirme olarak kullanılabilir. Bkz: [Visual Studio sürümlerini](~/ef6/what-is-new/visual-studio.md) EF Araçlar kullanılabilir en son sürümünü Visual Studio sürümünüz için alma hakkında yönergeler için.

## <a name="ef-runtime"></a>EF çalışma zamanı

Entity Framework'ün en son sürümü olarak kullanılabilir [EntityFramework NuGet paketi](http://nuget.org/packages/EntityFramework/). NuGet Paket Yöneticisi ile ilgili bilgi sahibi değilseniz okumanızı öneririz [NuGet genel bakış](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).

### <a name="installing-the-ef-nuget-package"></a>EF NuGet paketi yükleniyor

Sağ tıklayarak EntityFramework paketini yükleyebilirsiniz **başvuruları** projenizin klasörü seçerek **NuGet paketlerini Yönet...**

![ManageNuGetPackages](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Paket Yöneticisi konsolundan yükleme

Alternatif olarak, aşağıdaki komutu çalıştırarak EntityFramework yükleyebilirsiniz [Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>EF belirli bir sürümünü yükleme

Ve sonraki sürümlerde EF 4.1 EF çalışma zamanının yeni sürümü olarak yayımlanmıştır [EntityFramework NuGet paketi](https://www.nuget.org/packages/EntityFramework/). .NET Framework tabanlı bir proje için bu sürümlerinin herhangi birinin eklenebilir Visual Studio'nun aşağıdaki komutu çalıştırarak [Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):

``` powershell
Install-Package EntityFramework -Version <number>
```

Unutmayın `<number>` EF yüklemek için belirli bir sürümünü temsil eder. Örneğin, 6.2.0 EF 6.2 numaralı sürümüdür.   

EF çalışma zamanları 4.1 önce .NET Framework parçası olan ve ayrı olarak yüklenemez.

### <a name="installing-the-latest-preview"></a>En son Önizleme yükleme

Yukarıdaki yöntemleri en son verecektir Entity Framework'ün yayın'tam olarak desteklenir. Genellikle, deneyin ve bize geri bildirim sağlamak için isteriz kullanılabilir Entity Framework'ün yayın öncesi sürümler vardır.

EntityFramework seçebileceğiniz en son Önizlemesi'ni yüklemek için **öncesini** NuGet paketlerini Yönet penceresinde. Hiçbir yayın öncesi sürümler kullanılabilir olduğunda otomatik olarak en son erişmenizi sağlayacak tam olarak desteklenen Entity Framework sürümü.

![IncludePreRelease](~/ef6/media/includeprerelease.png)

Alternatif olarak, aşağıdaki komutu çalıştırabilirsiniz [Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework -Pre
```
