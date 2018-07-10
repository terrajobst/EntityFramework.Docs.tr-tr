---
title: EF Designer modelleri - EF6 için Entity Framework çalışma zamanı sürümü seçme
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
caps.latest.revision: 3
ms.openlocfilehash: 75f7b4ed81528683801893c31de490ce15be6733
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914256"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="133d3-102">EF Designer modelleri için Entity Framework çalışma zamanı sürüm seçme</span><span class="sxs-lookup"><span data-stu-id="133d3-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="133d3-103">**EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="133d3-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="133d3-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="133d3-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="133d3-105">Aşağıdaki ekranda EF6'ile başlayan EF modeli oluşturulurken hedeflemek istediğiniz çalışma zamanı sürümü seçmenizi sağlayacak için tasarımcıya eklendi.</span><span class="sxs-lookup"><span data-stu-id="133d3-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="133d3-106">Projede Entity Framework'ün en son sürümü zaten yüklü değilken ekranı görünür.</span><span class="sxs-lookup"><span data-stu-id="133d3-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="133d3-107">En son sürümü zaten yüklüyse yalnızca varsayılan olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="133d3-107">If the latest version is already installed it will just be used by default.</span></span>

![Ekran](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="133d3-109">EF6.x hedefleme</span><span class="sxs-lookup"><span data-stu-id="133d3-109">Targeting EF6.x</span></span>

<span data-ttu-id="133d3-110">EF6 çalışma zamanı projenize eklemek için 'Your sürümü seçin' ekranından EF6'ı seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="133d3-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="133d3-111">EF6 ekledikten sonra bu ekran geçerli projedeki görmeye durdurulacak.</span><span class="sxs-lookup"><span data-stu-id="133d3-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="133d3-112">Bu yana (aynı proje çalışma zamanının birden çok sürümlerini hedefleyemezsiniz) yüklü EF daha eski bir sürümü zaten varsa EF6 devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="133d3-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="133d3-113">EF6 seçeneği burada etkin değilse, projeniz için EF6 yükseltmek için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="133d3-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="133d3-114">Çözüm Gezgini'nde projenize sağ tıklayıp **NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="133d3-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="133d3-115">Seçin **güncelleştirmeleri**</span><span class="sxs-lookup"><span data-stu-id="133d3-115">Select **Updates**</span></span>
3.  <span data-ttu-id="133d3-116">Seçin **EntityFramework** (giderek istediğiniz sürüme güncelleştirmek için emin olun)</span><span class="sxs-lookup"><span data-stu-id="133d3-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="133d3-117">Tıklayın **güncelleştirme**</span><span class="sxs-lookup"><span data-stu-id="133d3-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="133d3-118">EF5.x hedefleme</span><span class="sxs-lookup"><span data-stu-id="133d3-118">Targeting EF5.x</span></span>

<span data-ttu-id="133d3-119">EF5 çalışma zamanı projenize eklemek için 'Your sürümü seçin' ekranından EF5 seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="133d3-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="133d3-120">EF5 ekledikten sonra ekran devre dışı EF6 seçeneğiyle görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="133d3-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="133d3-121">Zaten yüklü olan çalışma zamanı bir EF4.x sürümü varsa, bu ekran EF5 yerine listelenen EF sürümünü görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="133d3-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="133d3-122">Bu durumda, yükseltme EF5 için aşağıdaki adımları kullanarak:</span><span class="sxs-lookup"><span data-stu-id="133d3-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="133d3-123">Seçin **Araçları -&gt; kitaplık Paket Yöneticisi -&gt; Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="133d3-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="133d3-124">Çalıştırma **Install-Package EntityFramework-sürüm 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="133d3-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="133d3-125">EF4.x hedefleme</span><span class="sxs-lookup"><span data-stu-id="133d3-125">Targeting EF4.x</span></span>

<span data-ttu-id="133d3-126">Aşağıdaki adımları kullanarak projenize EF4.x çalışma zamanı yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="133d3-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="133d3-127">Seçin **Araçları -&gt; kitaplık Paket Yöneticisi -&gt; Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="133d3-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="133d3-128">Çalıştırma **Install-Package EntityFramework-sürüm 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="133d3-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
