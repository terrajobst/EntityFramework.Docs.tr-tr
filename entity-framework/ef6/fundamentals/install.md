---
title: Entity Framework Al-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 2bdec6a9be228fbe934d0f46aa1bfafdfb2c971c
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181733"
---
# <a name="get-entity-framework"></a><span data-ttu-id="b20c8-102">Entity Framework Alma</span><span class="sxs-lookup"><span data-stu-id="b20c8-102">Get Entity Framework</span></span>
<span data-ttu-id="b20c8-103">Entity Framework, Visual Studio ve EF çalışma zamanına yönelik EF araçlarından oluşur.</span><span class="sxs-lookup"><span data-stu-id="b20c8-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="b20c8-104">Visual Studio için EF araçları</span><span class="sxs-lookup"><span data-stu-id="b20c8-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="b20c8-105">Visual Studio için Entity Framework Tools EF Designer ve EF model Sihirbazı 'nı içerir ve ilk olarak veritabanı ve model ilk iş akışları için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="b20c8-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="b20c8-106">EF araçları, Visual Studio 'nun tüm son sürümlerine dahildir.</span><span class="sxs-lookup"><span data-stu-id="b20c8-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="b20c8-107">Visual Studio 'nun özel bir yüklemesini gerçekleştirirseniz, "Entity Framework 6 araçları" öğesinin, kendisini içeren bir iş yükü seçerek veya tek bir bileşen olarak seçerek seçili olduğundan emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="b20c8-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="b20c8-108">Visual Studio 'nun bazı eski sürümlerinde, güncelleştirilmiş EF araçları indirme olarak sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b20c8-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="b20c8-109">Visual Studio sürümünüz için sunulan en son EF araçları sürümünü nasıl alacağınız hakkında yönergeler için bkz. [Visual Studio sürümleri](~/ef6/what-is-new/visual-studio.md) .</span><span class="sxs-lookup"><span data-stu-id="b20c8-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="b20c8-110">EF çalışma zamanı</span><span class="sxs-lookup"><span data-stu-id="b20c8-110">EF Runtime</span></span>

<span data-ttu-id="b20c8-111">Entity Framework en son sürümü [EntityFramework NuGet paketi](https://nuget.org/packages/EntityFramework/)olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b20c8-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="b20c8-112">NuGet Paket Yöneticisi 'Ni tanımıyorsanız, [NuGet genel bakışını](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow)okumanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="b20c8-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="b20c8-113">EF NuGet paketini yükleme</span><span class="sxs-lookup"><span data-stu-id="b20c8-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="b20c8-114">Projenizin **Başvurular** klasörüne sağ tıklayıp NuGet Paketlerini Yönet ' i seçerek EntityFramework paketini yükleyebilirsiniz **...**</span><span class="sxs-lookup"><span data-stu-id="b20c8-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![NuGet Paketlerini yönetme](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="b20c8-116">Paket Yöneticisi konsolundan yükleme</span><span class="sxs-lookup"><span data-stu-id="b20c8-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="b20c8-117">Alternatif olarak, aşağıdaki komutu [Paket Yöneticisi konsolunda](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)çalıştırarak EntityFramework 'ü yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b20c8-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="b20c8-118">Belirli bir EF sürümü yükleme</span><span class="sxs-lookup"><span data-stu-id="b20c8-118">Installing a specific version of EF</span></span>

<span data-ttu-id="b20c8-119">EF 4,1 ve sonraki sürümlerinde, EF çalışma zamanının yeni sürümleri [EntityFramework NuGet paketi](https://www.nuget.org/packages/EntityFramework/)olarak yayımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b20c8-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="b20c8-120">Bu sürümlerden herhangi biri, Visual Studio 'nun [Paket Yöneticisi konsolunda](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)aşağıdaki komutu çalıştırılarak .NET Framework tabanlı bir projeye eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="b20c8-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="b20c8-121">@No__t-0 ' ın yüklenecek EF 'in belirli sürümünü temsil ettiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="b20c8-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="b20c8-122">Örneğin, 6.2.0, EF 6,2 için sayı sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="b20c8-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="b20c8-123">4,1 ' dan önceki EF çalışma zamanları .NET Framework bir parçası ve ayrı olarak yüklenemez.</span><span class="sxs-lookup"><span data-stu-id="b20c8-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="b20c8-124">En son önizlemeyi yükleme</span><span class="sxs-lookup"><span data-stu-id="b20c8-124">Installing the Latest Preview</span></span>

<span data-ttu-id="b20c8-125">Yukarıdaki yöntemler Entity Framework için en son tam olarak desteklenen sürümü verecektir.</span><span class="sxs-lookup"><span data-stu-id="b20c8-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="b20c8-126">Denemenizi sevdiğimiz ve bize geri bildirimde bulunmak için sevdiğiniz Entity Framework ön sürüm sürümlerinin çoğu vardır.</span><span class="sxs-lookup"><span data-stu-id="b20c8-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="b20c8-127">EntityFramework 'ün en son önizlemesini yüklemek için, NuGet Paketlerini Yönet penceresinde **ön sürümü dahil et** ' i seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b20c8-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="b20c8-128">Herhangi bir ön sürüm sürümü yoksa, Entity Framework en son tamamen desteklenen sürümünü otomatik olarak alırsınız.</span><span class="sxs-lookup"><span data-stu-id="b20c8-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![Ön sürümü dahil et](~/ef6/media/includeprerelease.png)

<span data-ttu-id="b20c8-130">Alternatif olarak, [Paket Yöneticisi konsolunda](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)aşağıdaki komutu çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b20c8-130">Alternatively, you can run the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
