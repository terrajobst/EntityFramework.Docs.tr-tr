---
title: "EF çekirdek ve sizin için uygun olan EF6-"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="e3b97-102">EF çekirdek ve EF6: hangisinin sizin için uygun</span><span class="sxs-lookup"><span data-stu-id="e3b97-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="e3b97-103">Aşağıdaki bilgiler Entity Framework Çekirdek ve Entity Framework 6 arasında seçmenize yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="e3b97-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="e3b97-104">Yeni uygulamalar için yönergeler</span><span class="sxs-lookup"><span data-stu-id="e3b97-104">Guidance for new applications</span></span>

<span data-ttu-id="e3b97-105">EF çekirdek yeni bir üründür ve hala bazı önemli O/RM özellikleri eksik olduğundan, EF6 birçok uygulama için en uygun seçim olmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="e3b97-105">Because EF Core is a new product, and still lacks some critical O/RM features, EF6 will still be the most suitable choice for many applications.</span></span>

<span data-ttu-id="e3b97-106">**EF çekirdek için kullanmanızı öneririz uygulama türleri şunlardır:**</span><span class="sxs-lookup"><span data-stu-id="e3b97-106">**These are the types of applications we would recommend using EF Core for:**</span></span>

* <span data-ttu-id="e3b97-107">EF çekirdek henüz uygulanmadı özellikleri gerektirmeyen yeni uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="e3b97-107">New applications that do not require features that are not yet implemented in EF Core.</span></span> <span data-ttu-id="e3b97-108">Gözden geçirme [özellik karşılaştırması](features.md) EF çekirdek uygulamanız için uygun bir seçim sunulup sunulmadığını görmek için.</span><span class="sxs-lookup"><span data-stu-id="e3b97-108">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

* <span data-ttu-id="e3b97-109">.NET Core gibi Evrensel Windows Platformu (UWP) ve ASP.NET Core uygulamaları hedefleyen uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="e3b97-109">Applications that target .NET Core, such as Universal Windows Platform (UWP) and ASP.NET Core applications.</span></span> <span data-ttu-id="e3b97-110">.NET Framework (yani .NET Framework 4.5) gerektirdiği şekilde bu uygulamaları EF6 kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="e3b97-110">These applications can not use EF6 as it requires the .NET Framework (i.e. .NET Framework 4.5).</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="e3b97-111">Mevcut EF6 uygulamalar için kılavuz</span><span class="sxs-lookup"><span data-stu-id="e3b97-111">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="e3b97-112">EF çekirdek temel değişiklikleri nedeniyle değişiklik yapmak için ilgi çekici bir nedeniniz yoksa EF6 uygulamaya EF çekirdek taşınmaya çalışılırken önermiyoruz.</span><span class="sxs-lookup"><span data-stu-id="e3b97-112">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="e3b97-113">Taşımak istiyorsanız EF yeni özellikler ve ardından emin olun kullandığınız yapmak için çekirdek kendi sınırlamaları kullanan başlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="e3b97-113">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="e3b97-114">Gözden geçirme [özellik karşılaştırması](features.md) EF çekirdek uygulamanız için uygun bir seçim sunulup sunulmadığını görmek için.</span><span class="sxs-lookup"><span data-stu-id="e3b97-114">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="e3b97-115">**Taşımak EF6 EF çekirdek için bir yükseltme yerine bir bağlantı noktası olarak görüntülemeniz gerekir.**</span><span class="sxs-lookup"><span data-stu-id="e3b97-115">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="e3b97-116">Bkz: [EF çekirdek EF6 bağlantı noktası oluşturma](porting/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="e3b97-116">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
