---
title: Yenilikler-EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149116"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="8fd75-102">EF6 'deki yenilikler</span><span class="sxs-lookup"><span data-stu-id="8fd75-102">What's New in EF6</span></span>

<span data-ttu-id="8fd75-103">En son özellikleri ve en yüksek kararlılığı elde etmenizi sağlamak için en son yayınlanan Entity Framework sürümünü kullanmanızı kesinlikle öneririz.</span><span class="sxs-lookup"><span data-stu-id="8fd75-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="8fd75-104">Bununla birlikte, önceki bir sürümü kullanmanız gerektiğini veya en son yayın öncesi sürümde yeni geliştirmeler yapmak isteyebileceğiniz hakkında fark ediyoruz.</span><span class="sxs-lookup"><span data-stu-id="8fd75-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="8fd75-105">EF 'in belirli sürümlerini yüklemek için bkz. [Get Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="8fd75-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="8fd75-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="8fd75-106">EF 6.3.0</span></span>

<span data-ttu-id="8fd75-107">EF 6.3.0 Runtime, Eylül 2019 ' de NuGet 'e yayımlandı.</span><span class="sxs-lookup"><span data-stu-id="8fd75-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="8fd75-108">Bu sürümün ana amacı, EF 6 kullanan mevcut uygulamaların .NET Core 3,0 ' e geçirilmesini kolaylaştırmıştı.</span><span class="sxs-lookup"><span data-stu-id="8fd75-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="8fd75-109">Topluluk ayrıca çeşitli hata düzeltmeleri ve geliştirmeleri de katkıda bulunur.</span><span class="sxs-lookup"><span data-stu-id="8fd75-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="8fd75-110">Ayrıntılar için her 6.3.0 [kilometre taşında](https://github.com/aspnet/EntityFramework6/milestones?state=closed) kapatılan sorunlara bakın.</span><span class="sxs-lookup"><span data-stu-id="8fd75-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="8fd75-111">Burada, daha fazla bilgi yer verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="8fd75-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="8fd75-112">.NET Core 3,0 desteği</span><span class="sxs-lookup"><span data-stu-id="8fd75-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="8fd75-113">EntityFramework paketi artık .NET Framework 4. x ' e ek olarak .NET Standard 2,1 ' i hedefliyor</span><span class="sxs-lookup"><span data-stu-id="8fd75-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x</span></span>
  - <span data-ttu-id="8fd75-114">Geçişler komutları, işlem dışında yürütülecek ve SDK stili projelerle çalışacak şekilde yeniden yazıldı</span><span class="sxs-lookup"><span data-stu-id="8fd75-114">The migrations commands have been rewritten to execute out of process and work with SDK-style projects</span></span>
- <span data-ttu-id="8fd75-115">SQL Server HierarchyId desteği</span><span class="sxs-lookup"><span data-stu-id="8fd75-115">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="8fd75-116">Roslyn ve NuGet PackageReference ile iyileştirilmiş uyumluluk</span><span class="sxs-lookup"><span data-stu-id="8fd75-116">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="8fd75-117">Derlemelerden geçişleri etkinleştirmek, eklemek, betik oluşturmak ve uygulamak için EF6. exe eklendi.</span><span class="sxs-lookup"><span data-stu-id="8fd75-117">Added ef6.exe for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="8fd75-118">Bu, migrate. exe ' nin yerini alır</span><span class="sxs-lookup"><span data-stu-id="8fd75-118">This replaces migrate.exe</span></span>

## <a name="past-releases"></a><span data-ttu-id="8fd75-119">Geçmiş yayınlar</span><span class="sxs-lookup"><span data-stu-id="8fd75-119">Past Releases</span></span>

<span data-ttu-id="8fd75-120">[Eski yayınlar](past-releases.md) sayfası, tüm önceki EF sürümlerinin ve her yayında tanıtılan ana özelliklerin bir arşivini içerir.</span><span class="sxs-lookup"><span data-stu-id="8fd75-120">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
