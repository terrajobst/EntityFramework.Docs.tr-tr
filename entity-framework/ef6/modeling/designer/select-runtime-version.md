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
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="e5dfc-102">EF Designer modelleri için Entity Framework çalışma zamanı sürümü seçme</span><span class="sxs-lookup"><span data-stu-id="e5dfc-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="e5dfc-103">**Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="e5dfc-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="e5dfc-105">EF6 ile başlayarak, bir model oluştururken hedeflemek istediğiniz çalışma zamanının sürümünü seçmenizi sağlamak için EF tasarımcısına aşağıdaki ekran eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="e5dfc-106">Entity Framework en son sürümü projede zaten yüklü olmadığında ekran görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="e5dfc-107">En son sürüm zaten yüklüyse, varsayılan olarak yalnızca kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-107">If the latest version is already installed it will just be used by default.</span></span>

![Ekranınızdaki](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="e5dfc-109">EF6. x hedefleme</span><span class="sxs-lookup"><span data-stu-id="e5dfc-109">Targeting EF6.x</span></span>

<span data-ttu-id="e5dfc-110">EF6 çalışma zamanını projenize eklemek için ' sürümünüzü seçin ' ekranından EF6 seçeneğini belirleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="e5dfc-111">EF6 ekledikten sonra bu ekranı geçerli projede görmekten vazgeçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="e5dfc-112">Zaten EF 'in daha eski bir sürümü yüklüyse, EF6 devre dışı bırakılır (çünkü çalışma zamanının birden çok sürümünü aynı projeden hedefleyemiyorum).</span><span class="sxs-lookup"><span data-stu-id="e5dfc-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="e5dfc-113">EF6 seçeneği burada etkinleştirilmemişse, projenizi EF6 sürümüne yükseltmek için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="e5dfc-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="e5dfc-114">Çözüm Gezgini ' de projenize sağ tıklayın ve **NuGet Paketlerini Yönet..** . seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="e5dfc-115">**Güncelleştirme** seçin</span><span class="sxs-lookup"><span data-stu-id="e5dfc-115">Select **Updates**</span></span>
3.  <span data-ttu-id="e5dfc-116">**EntityFramework 'ü** seçin (bunu istediğiniz sürüme güncelleştirdiğinizden emin olun)</span><span class="sxs-lookup"><span data-stu-id="e5dfc-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="e5dfc-117">**Güncelleştir** 'e tıklayın</span><span class="sxs-lookup"><span data-stu-id="e5dfc-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="e5dfc-118">EF5. x hedefleme</span><span class="sxs-lookup"><span data-stu-id="e5dfc-118">Targeting EF5.x</span></span>

<span data-ttu-id="e5dfc-119">EF5 çalışma zamanını projenize eklemek için ' sürümünüzü seçin ' ekranından EF5 seçeneğini belirleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="e5dfc-120">EF5 ekledikten sonra ekran EF6 seçeneği devre dışı olarak görünmeye devam eder.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="e5dfc-121">Çalışma zamanının zaten yüklü olduğu bir EF4. x sürümüne sahipseniz, EF5 yerine ekranda listelenen EF sürümünü görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="e5dfc-122">Bu durumda, aşağıdaki adımları kullanarak EF5 sürümüne yükseltebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e5dfc-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="e5dfc-123">**Araçlar-&gt; kitaplığı Paket Yöneticisi-&gt; Paket Yöneticisi konsolu** ' nu seçin</span><span class="sxs-lookup"><span data-stu-id="e5dfc-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="e5dfc-124">**Install-Package EntityFramework-Version 5.0.0** Çalıştır</span><span class="sxs-lookup"><span data-stu-id="e5dfc-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="e5dfc-125">EF4. x hedefleme</span><span class="sxs-lookup"><span data-stu-id="e5dfc-125">Targeting EF4.x</span></span>

<span data-ttu-id="e5dfc-126">Aşağıdaki adımları kullanarak EF4. x çalışma zamanını projenize yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e5dfc-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="e5dfc-127">**Araçlar-&gt; kitaplığı Paket Yöneticisi-&gt; Paket Yöneticisi konsolu** ' nu seçin</span><span class="sxs-lookup"><span data-stu-id="e5dfc-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="e5dfc-128">**Install-Package EntityFramework-Version 4.3.0** 'ı Çalıştır</span><span class="sxs-lookup"><span data-stu-id="e5dfc-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
