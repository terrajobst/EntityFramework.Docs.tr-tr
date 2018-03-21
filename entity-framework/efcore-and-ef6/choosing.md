---
title: "EF çekirdek ve sizin için uygun olan EF6-"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: f0a632902384a65ea3cddf752ad262c7a2e89e2e
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2018
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="dd6b0-102">EF çekirdek ve EF6: hangisinin sizin için uygun</span><span class="sxs-lookup"><span data-stu-id="dd6b0-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="dd6b0-103">Aşağıdaki bilgiler Entity Framework Çekirdek ve Entity Framework 6 arasında seçmenize yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="dd6b0-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="dd6b0-104">Yeni uygulamalar için yönergeler</span><span class="sxs-lookup"><span data-stu-id="dd6b0-104">Guidance for new applications</span></span>

<span data-ttu-id="dd6b0-105">EF çekirdek yeni uygulamalar için uygulamanın EF çekirdek henüz uygulanmadı herhangi bir özellik gerektirmez ve tüm özelliklerini EF çekirdek yararlanmak isterseniz kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="dd6b0-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="dd6b0-106">EF6 .NET Framework 4.0 (veya sonraki bir sürümünü) gerektirir ve yalnızca (yani, .NET Core üzerinde çalışmaz ve diğer işletim sistemlerinde desteklenmez) Windows desteklenir, ancak hala uzun bu kısıtlamalar kabul edilebilir olduğundan yeni uygulamalar için uygun bir seçim değil ve bir uygulama için EF6 bulunmayan yeni EF çekirdek özellikler gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="dd6b0-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (i.e. it does not run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="dd6b0-107">Gözden geçirme [özellik karşılaştırması](features.md) EF çekirdek uygulamanız için uygun bir seçim sunulup sunulmadığını görmek için.</span><span class="sxs-lookup"><span data-stu-id="dd6b0-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="dd6b0-108">Mevcut EF6 uygulamalar için kılavuz</span><span class="sxs-lookup"><span data-stu-id="dd6b0-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="dd6b0-109">EF çekirdek temel değişiklikleri nedeniyle değişiklik yapmak için ilgi çekici bir nedeniniz yoksa EF6 uygulamaya EF çekirdek taşınmaya çalışılırken önermiyoruz.</span><span class="sxs-lookup"><span data-stu-id="dd6b0-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="dd6b0-110">Taşımak istiyorsanız EF yeni özellikler ve ardından emin olun kullandığınız yapmak için çekirdek kendi sınırlamaları kullanan başlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="dd6b0-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="dd6b0-111">Gözden geçirme [özellik karşılaştırması](features.md) EF çekirdek uygulamanız için uygun bir seçim sunulup sunulmadığını görmek için.</span><span class="sxs-lookup"><span data-stu-id="dd6b0-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="dd6b0-112">**Taşımak EF6 EF çekirdek için bir yükseltme yerine bir bağlantı noktası olarak görüntülemeniz gerekir.**</span><span class="sxs-lookup"><span data-stu-id="dd6b0-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="dd6b0-113">Bkz: [EF çekirdek EF6 bağlantı noktası oluşturma](porting/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="dd6b0-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
