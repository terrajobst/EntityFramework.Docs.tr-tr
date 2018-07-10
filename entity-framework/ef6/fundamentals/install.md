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
ms.openlocfilehash: 52bc05bd25da919052ee58bbcc19d57e12ebc9d3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912654"
---
# <a name="get-entity-framework"></a><span data-ttu-id="33f6c-102">Entity Framework Al</span><span class="sxs-lookup"><span data-stu-id="33f6c-102">Get Entity Framework</span></span>
<span data-ttu-id="33f6c-103">Entity Framework EF araçlarını Visual Studio ve EF çalışma zamanı için oluşur.</span><span class="sxs-lookup"><span data-stu-id="33f6c-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="33f6c-104">Visual Studio için EF araçları</span><span class="sxs-lookup"><span data-stu-id="33f6c-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="33f6c-105">Visual Studio için Entity Framework Tools EF Designer ve EF modeli Sihirbazı'nı içerir ve ilk veritabanı için gerekli olan ve ilk iş akışlarını modellemek.</span><span class="sxs-lookup"><span data-stu-id="33f6c-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="33f6c-106">EF aracı, tüm yeni Visual Studio sürümlerinde yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="33f6c-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="33f6c-107">Özel bir yükleme öğesi emin olmak için ihtiyacınız olacak Visual Studio'nun gerçekleştirirseniz "Entity Framework 6 Tools" içeren bir iş yükünü seçme veya ayrı ayrı bir bileşen seçerek seçilir.</span><span class="sxs-lookup"><span data-stu-id="33f6c-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="33f6c-108">Bazı Visual Studio sürümlerini güncelleştirilmiş EF Araçlar bir indirme olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="33f6c-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="33f6c-109">Bkz: [Visual Studio sürümlerini](~/ef6/what-is-new/visual-studio.md) EF Araçlar kullanılabilir en son sürümünü Visual Studio sürümünüz için alma hakkında yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="33f6c-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="33f6c-110">EF çalışma zamanı</span><span class="sxs-lookup"><span data-stu-id="33f6c-110">EF Runtime</span></span>

<span data-ttu-id="33f6c-111">Entity Framework'ün en son sürümü olarak kullanılabilir [EntityFramework NuGet paketi](http://nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="33f6c-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](http://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="33f6c-112">NuGet Paket Yöneticisi ile ilgili bilgi sahibi değilseniz okumanızı öneririz [NuGet genel bakış](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="33f6c-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="33f6c-113">EF NuGet paketi yükleniyor</span><span class="sxs-lookup"><span data-stu-id="33f6c-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="33f6c-114">Sağ tıklayarak EntityFramework paketini yükleyebilirsiniz **başvuruları** projenizin klasörü seçerek **NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="33f6c-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![ManageNuGetPackages](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="33f6c-116">Paket Yöneticisi konsolundan yükleme</span><span class="sxs-lookup"><span data-stu-id="33f6c-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="33f6c-117">Alternatif olarak, aşağıdaki komutu çalıştırarak EntityFramework yükleyebilirsiniz [Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="33f6c-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="33f6c-118">EF belirli bir sürümünü yükleme</span><span class="sxs-lookup"><span data-stu-id="33f6c-118">Installing a specific version of EF</span></span>

<span data-ttu-id="33f6c-119">Ve sonraki sürümlerde EF 4.1 EF çalışma zamanının yeni sürümü olarak yayımlanmıştır [EntityFramework NuGet paketi](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="33f6c-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Pacakge](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="33f6c-120">.NET Framework tabanlı bir proje için bu sürümlerinin herhangi birinin eklenebilir Visual Studio'nun aşağıdaki komutu çalıştırarak [Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span><span class="sxs-lookup"><span data-stu-id="33f6c-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="33f6c-121">Unutmayın `<number>` belirli bir sürüm temsil yüklemek için EF örneğin 6.2.0 EF 6.2 numaralı sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="33f6c-121">Note that `<number>` represents the specific version of EF to install, e.g. 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="33f6c-122">EF çalışma zamanları 4.1 önce .NET Framework parçası olan ve ayrı olarak yüklenemez.</span><span class="sxs-lookup"><span data-stu-id="33f6c-122">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="33f6c-123">En son Önizleme yükleme</span><span class="sxs-lookup"><span data-stu-id="33f6c-123">Installing the Latest Preview</span></span>

<span data-ttu-id="33f6c-124">Yukarıdaki yöntemleri en son verecektir Entity Framework'ün yayın'tam olarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="33f6c-124">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="33f6c-125">Genellikle, deneyin ve bize geri bildirim sağlamak için isteriz kullanılabilir Entity Framework'ün yayın öncesi sürümler vardır.</span><span class="sxs-lookup"><span data-stu-id="33f6c-125">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="33f6c-126">EntityFramework seçebileceğiniz en son Önizlemesi'ni yüklemek için **öncesini** NuGet paketlerini Yönet penceresinde.</span><span class="sxs-lookup"><span data-stu-id="33f6c-126">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="33f6c-127">Hiçbir yayın öncesi sürümler kullanılabilir olduğunda otomatik olarak en son erişmenizi sağlayacak tam olarak desteklenen Entity Framework sürümü.</span><span class="sxs-lookup"><span data-stu-id="33f6c-127">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![IncludePreRelease](~/ef6/media/includeprerelease.png)

<span data-ttu-id="33f6c-129">Alternatif olarak, aşağıdaki komutu çalıştırabilirsiniz [Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="33f6c-129">Alternatively, you can run the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```