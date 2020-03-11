---
title: Entity Framework Al-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 2bdec6a9be228fbe934d0f46aa1bfafdfb2c971c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419470"
---
# <a name="get-entity-framework"></a>Entity Framework Alma
Entity Framework, Visual Studio ve EF çalışma zamanına yönelik EF araçlarından oluşur.

## <a name="ef-tools-for-visual-studio"></a>Visual Studio için EF araçları

Visual Studio için Entity Framework Tools EF Designer ve EF model Sihirbazı 'nı içerir ve ilk olarak veritabanı ve model ilk iş akışları için gereklidir. EF araçları, Visual Studio 'nun tüm son sürümlerine dahildir. Visual Studio 'nun özel bir yüklemesini gerçekleştirirseniz, "Entity Framework 6 araçları" öğesinin, kendisini içeren bir iş yükü seçerek veya tek bir bileşen olarak seçerek seçili olduğundan emin olmanız gerekir.

Visual Studio 'nun bazı eski sürümlerinde, güncelleştirilmiş EF araçları indirme olarak sunulmaktadır. Visual Studio sürümünüz için sunulan en son EF araçları sürümünü nasıl alacağınız hakkında yönergeler için bkz. [Visual Studio sürümleri](~/ef6/what-is-new/visual-studio.md) .

## <a name="ef-runtime"></a>EF çalışma zamanı

Entity Framework en son sürümü [EntityFramework NuGet paketi](https://nuget.org/packages/EntityFramework/)olarak kullanılabilir. NuGet Paket Yöneticisi 'Ni tanımıyorsanız, [NuGet genel bakışını](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow)okumanızı öneririz.

### <a name="installing-the-ef-nuget-package"></a>EF NuGet paketini yükleme

Projenizin **Başvurular** klasörüne sağ tıklayıp NuGet Paketlerini Yönet ' i seçerek EntityFramework paketini yükleyebilirsiniz **...**

![NuGet Paketlerini yönetme](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Paket Yöneticisi konsolundan yükleme

Alternatif olarak, aşağıdaki komutu [Paket Yöneticisi konsolunda](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)çalıştırarak EntityFramework 'ü yükleyebilirsiniz.

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>Belirli bir EF sürümü yükleme

EF 4,1 ve sonraki sürümlerinde, EF çalışma zamanının yeni sürümleri [EntityFramework NuGet paketi](https://www.nuget.org/packages/EntityFramework/)olarak yayımlanmıştır. Bu sürümlerden herhangi biri, Visual Studio 'nun [Paket Yöneticisi konsolunda](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)aşağıdaki komutu çalıştırılarak .NET Framework tabanlı bir projeye eklenebilir:

``` powershell
Install-Package EntityFramework -Version <number>
```

`<number>` yüklenecek EF 'in belirli sürümünü temsil ettiğini unutmayın. Örneğin, 6.2.0, EF 6,2 için sayı sürümüdür.   

4,1 ' dan önceki EF çalışma zamanları .NET Framework bir parçası ve ayrı olarak yüklenemez.

### <a name="installing-the-latest-preview"></a>En son önizlemeyi yükleme

Yukarıdaki yöntemler Entity Framework için en son tam olarak desteklenen sürümü verecektir. Denemenizi sevdiğimiz ve bize geri bildirimde bulunmak için sevdiğiniz Entity Framework ön sürüm sürümlerinin çoğu vardır.

EntityFramework 'ün en son önizlemesini yüklemek için, NuGet Paketlerini Yönet penceresinde **ön sürümü dahil et** ' i seçebilirsiniz. Herhangi bir ön sürüm sürümü yoksa, Entity Framework en son tamamen desteklenen sürümünü otomatik olarak alırsınız.

![Ön sürümü dahil et](~/ef6/media/includeprerelease.png)

Alternatif olarak, [Paket Yöneticisi konsolunda](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)aşağıdaki komutu çalıştırabilirsiniz.

``` powershell
Install-Package EntityFramework -Pre
```
