---
title: EF Core ve EF6 - sizin için uygun olan
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 17c81e0d6c384c06e45f0cf4f338d4ba402788e1
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949146"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="794df-102">EF Core ve EF6: hangisinin sizin için uygun</span><span class="sxs-lookup"><span data-stu-id="794df-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="794df-103">Aşağıdaki bilgiler, Entity Framework Core ve Entity Framework 6 arasında seçim yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="794df-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="794df-104">Yeni uygulamalar için yönergeler</span><span class="sxs-lookup"><span data-stu-id="794df-104">Guidance for new applications</span></span>

<span data-ttu-id="794df-105">EF Core için yeni uygulamalar ' ın tüm özelliklerini EF Core yararlanmak istiyorsanız ve uygulamanızı EF Core henüz uygulanmadı herhangi bir özellik gerektirmez kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="794df-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="794df-106">EF6 .NET Framework 4.0 (veya sonraki bir sürüm) gerektirir ve yalnızca Windows üzerinde desteklenir (diğer bir deyişle, EF6 şu anda .NET Core üzerinde çalışmaz ve diğer işletim sistemlerinde desteklenmez), ancak uzun bu kısıtlamaları olarak hala yeni uygulamalar için uygun bir seçim kabul edilebilir ve uygulamanın EF Core için EF6 kullanılamaz yeni özellikler gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="794df-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (that is, EF6 does not currently run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="794df-107">Gözden geçirme [özellik karşılaştırması](features.md) EF Core uygulamanız için uygun bir seçim olabilir, görmek için.</span><span class="sxs-lookup"><span data-stu-id="794df-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="794df-108">EF6 uygulamalarınız için yönergeler</span><span class="sxs-lookup"><span data-stu-id="794df-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="794df-109">EF Core temel yapılan değişiklikler nedeniyle değişiklik yapmak için yeterli bir neden olmadığı sürece EF6 uygulamanın EF core'a taşıma girişiminde önermiyoruz.</span><span class="sxs-lookup"><span data-stu-id="794df-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="794df-110">Taşımak istiyorsanız kullandığınız yeni özellikler sonra emin olmak için EF Core var. onun sınırlamaları dikkate başlamadan önce</span><span class="sxs-lookup"><span data-stu-id="794df-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="794df-111">Gözden geçirme [özellik karşılaştırması](features.md) EF Core uygulamanız için uygun bir seçim olabilir, görmek için.</span><span class="sxs-lookup"><span data-stu-id="794df-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="794df-112">**EF6'dan yükseltme işlemi yerine bir bağlantı noktası olarak hareket EF core'a görüntülemeniz gerekir.**</span><span class="sxs-lookup"><span data-stu-id="794df-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="794df-113">Bkz: [EF6'dan EF Core'a taşıma](porting/index.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="794df-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
