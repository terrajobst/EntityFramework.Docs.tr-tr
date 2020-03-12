---
title: Desteklenen .NET uygulamaları-EF Core
author: bricelam
ms.date: 03/03/2020
uid: core/platforms/index
ms.openlocfilehash: 693d4cae85eddf86d01e17084415147c52a008c7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417324"
---
# <a name="net-implementations-supported-by-ef-core"></a><span data-ttu-id="07e86-102">EF Core tarafından desteklenen .NET uygulamaları</span><span class="sxs-lookup"><span data-stu-id="07e86-102">.NET implementations supported by EF Core</span></span>

<span data-ttu-id="07e86-103">EF Core tüm modern .NET uygulamalarındaki geliştiriciler için kullanılabilir olmasını istiyoruz ve bu hedefle çalışmaya devam ediyoruz.</span><span class="sxs-lookup"><span data-stu-id="07e86-103">We want EF Core to be available to developers on all modern .NET implementations, and we're still working towards that goal.</span></span> <span data-ttu-id="07e86-104">EF Core .NET Core üzerinde destek, otomatik test ve başarıyla kullanılması bilinen birçok uygulama tarafından ele alınmıştır, mono, Xamarin ve UWP bazı sorunlar yaşıyor.</span><span class="sxs-lookup"><span data-stu-id="07e86-104">While EF Core's support on .NET Core is covered by automated testing and many applications known to be using it successfully, Mono, Xamarin and UWP have some issues.</span></span>

## <a name="overview"></a><span data-ttu-id="07e86-105">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="07e86-105">Overview</span></span>

<span data-ttu-id="07e86-106">Aşağıdaki tabloda her .NET uygulamasına yönelik rehberlik sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="07e86-106">The following table provides guidance for each .NET implementation:</span></span>

| <span data-ttu-id="07e86-107">EF Core</span><span class="sxs-lookup"><span data-stu-id="07e86-107">EF Core</span></span>                       | <span data-ttu-id="07e86-108">2,1 ve 3,1</span><span class="sxs-lookup"><span data-stu-id="07e86-108">2.1 and 3.1</span></span> |
|:------------------------------|:------------|
| <span data-ttu-id="07e86-109">.NET Standard</span><span class="sxs-lookup"><span data-stu-id="07e86-109">.NET Standard</span></span>                 | <span data-ttu-id="07e86-110">2,0</span><span class="sxs-lookup"><span data-stu-id="07e86-110">2.0</span></span>         |
| <span data-ttu-id="07e86-111">.NET Core</span><span class="sxs-lookup"><span data-stu-id="07e86-111">.NET Core</span></span>                     | <span data-ttu-id="07e86-112">2,0</span><span class="sxs-lookup"><span data-stu-id="07e86-112">2.0</span></span>         |
| <span data-ttu-id="07e86-113">.NET Framework<sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="07e86-113">.NET Framework<sup>(1)</sup></span></span>  | <span data-ttu-id="07e86-114">4.7.2</span><span class="sxs-lookup"><span data-stu-id="07e86-114">4.7.2</span></span>       |
| <span data-ttu-id="07e86-115">Mono</span><span class="sxs-lookup"><span data-stu-id="07e86-115">Mono</span></span>                          | <span data-ttu-id="07e86-116">5,4</span><span class="sxs-lookup"><span data-stu-id="07e86-116">5.4</span></span>         |
| <span data-ttu-id="07e86-117">Xamarin. iOS<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="07e86-117">Xamarin.iOS<sup>(2)</sup></span></span>     | <span data-ttu-id="07e86-118">10,14</span><span class="sxs-lookup"><span data-stu-id="07e86-118">10.14</span></span>       |
| <span data-ttu-id="07e86-119">Xamarin. Android<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="07e86-119">Xamarin.Android<sup>(2)</sup></span></span> | <span data-ttu-id="07e86-120">8.0</span><span class="sxs-lookup"><span data-stu-id="07e86-120">8.0</span></span>         |
| <span data-ttu-id="07e86-121">UWP<sup>(3)</sup></span><span class="sxs-lookup"><span data-stu-id="07e86-121">UWP<sup>(3)</sup></span></span>             | <span data-ttu-id="07e86-122">10.0.16299</span><span class="sxs-lookup"><span data-stu-id="07e86-122">10.0.16299</span></span>  |
| <span data-ttu-id="07e86-123">Unity<sup>(4)</sup></span><span class="sxs-lookup"><span data-stu-id="07e86-123">Unity<sup>(4)</sup></span></span>           | <span data-ttu-id="07e86-124">2018,1</span><span class="sxs-lookup"><span data-stu-id="07e86-124">2018.1</span></span>      |

<span data-ttu-id="07e86-125"><sup>(1)</sup> aşağıdaki [.NET Framework](#net-framework) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="07e86-125"><sup>(1)</sup> See the [.NET Framework](#net-framework) section below.</span></span>

<span data-ttu-id="07e86-126"><sup>(2)</sup> Xamarin ile ilgili sorunlar ve bilinen sınırlamalar vardır ve EF Core kullanılarak geliştirilen bazı uygulamaların doğru şekilde çalışmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="07e86-126"><sup>(2)</sup> There are issues and known limitations with Xamarin which may prevent some applications developed using EF Core from working correctly.</span></span> <span data-ttu-id="07e86-127">Geçici çözümler için [etkin sorunlar](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) listesini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="07e86-127">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) for workarounds.</span></span>

<span data-ttu-id="07e86-128"><sup>(3)</sup> EF Core 2.0.1 ve daha yeni önerilir.</span><span class="sxs-lookup"><span data-stu-id="07e86-128"><sup>(3)</sup> EF Core 2.0.1 and newer recommended.</span></span> <span data-ttu-id="07e86-129">[.NET Core UWP 6. x paketini](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/)yükler.</span><span class="sxs-lookup"><span data-stu-id="07e86-129">Install the [.NET Core UWP 6.x package](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span></span> <span data-ttu-id="07e86-130">Bu makalenin [Evrensel Windows platformu](#universal-windows-platform) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="07e86-130">See the [Universal Windows Platform](#universal-windows-platform) section of this article.</span></span>

<span data-ttu-id="07e86-131"><sup>(4)</sup> Unity ile ilgili sorunlar ve bilinen sınırlamalar vardır.</span><span class="sxs-lookup"><span data-stu-id="07e86-131"><sup>(4)</sup> There are issues and known limitations with Unity.</span></span> <span data-ttu-id="07e86-132">[Etkin sorunlar](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity)listesini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="07e86-132">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span></span>

## <a name="net-framework"></a><span data-ttu-id="07e86-133">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="07e86-133">.NET Framework</span></span>

<span data-ttu-id="07e86-134">.NET Framework hedef uygulamaların .NET Standard kitaplıklarıyla çalışması için değişiklikler gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="07e86-134">Applications that target .NET Framework may need changes to work with .NET Standard libraries:</span></span>

<span data-ttu-id="07e86-135">Proje dosyasını düzenleyin ve aşağıdaki girdinin ilk özellik grubunda göründüğünden emin olun:</span><span class="sxs-lookup"><span data-stu-id="07e86-135">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

<span data-ttu-id="07e86-136">Test projeleri için aşağıdaki girişin mevcut olduğundan da emin olun:</span><span class="sxs-lookup"><span data-stu-id="07e86-136">For test projects, also make sure the following entry is present:</span></span>

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

<span data-ttu-id="07e86-137">Visual Studio 'nun eski bir sürümünü kullanmak istiyorsanız, NuGet istemcisini .NET Standard 2,0 kitaplıklarıyla çalışmak üzere [3.6.0 sürümüne yükseltdiğinizden](https://www.nuget.org/downloads) emin olun.</span><span class="sxs-lookup"><span data-stu-id="07e86-137">If you want to use an older version of Visual Studio, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

<span data-ttu-id="07e86-138">Ayrıca, NuGet paketleri. config ' den, mümkünse PackageReference 'a geçiş yapmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="07e86-138">We also recommend migrating from NuGet packages.config to PackageReference if possible.</span></span> <span data-ttu-id="07e86-139">Aşağıdaki özelliği proje dosyanıza ekleyin:</span><span class="sxs-lookup"><span data-stu-id="07e86-139">Add the following property to your project file:</span></span>

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a><span data-ttu-id="07e86-140">Evrensel Windows Platformu</span><span class="sxs-lookup"><span data-stu-id="07e86-140">Universal Windows Platform</span></span>

<span data-ttu-id="07e86-141">EF Core ve .NET UWP 'nin önceki sürümlerinde, özellikle .NET Native araç zinciri ile derlenen uygulamalarla çok sayıda uyumluluk sorunu vardı.</span><span class="sxs-lookup"><span data-stu-id="07e86-141">Earlier versions of EF Core and .NET UWP had numerous compatibility issues, especially with applications compiled with the .NET Native toolchain.</span></span> <span data-ttu-id="07e86-142">Yeni .NET UWP sürümü, .NET Standard 2,0 için destek ekler ve daha önce bildirilen uyumluluk sorunlarının çoğunu düzelten .NET Native 2,0 içerir.</span><span class="sxs-lookup"><span data-stu-id="07e86-142">The new .NET UWP version adds support for .NET Standard 2.0 and contains .NET Native 2.0, which fixes most of the compatibility issues previously reported.</span></span> <span data-ttu-id="07e86-143">EF Core 2.0.1 UWP ile daha kapsamlı bir şekilde sınanmıştır ancak test otomatik değildir.</span><span class="sxs-lookup"><span data-stu-id="07e86-143">EF Core 2.0.1 has been tested more thoroughly with UWP but testing is not automated.</span></span>

<span data-ttu-id="07e86-144">UWP üzerinde EF Core kullanırken:</span><span class="sxs-lookup"><span data-stu-id="07e86-144">When using EF Core on UWP:</span></span>

* <span data-ttu-id="07e86-145">Sorgu performansını iyileştirmek için LINQ sorgularında anonim türlerden kaçının.</span><span class="sxs-lookup"><span data-stu-id="07e86-145">To optimize query performance, avoid anonymous types in LINQ queries.</span></span> <span data-ttu-id="07e86-146">Bir UWP uygulamasını App Store 'a dağıtmak için bir uygulamanın .NET Native ile derlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="07e86-146">Deploying a UWP application to the app store requires an application to be compiled with .NET Native.</span></span> <span data-ttu-id="07e86-147">Anonim türlere sahip sorguların .NET Native performansı kötüleştiğini.</span><span class="sxs-lookup"><span data-stu-id="07e86-147">Queries with anonymous types have worse performance on .NET Native.</span></span>

* <span data-ttu-id="07e86-148">`SaveChanges()` performansını iyileştirmek için, [Changetrackingstrateji. ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) kullanın ve varlık Türlerinize [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx)ve [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) uygulayın.</span><span class="sxs-lookup"><span data-stu-id="07e86-148">To optimize `SaveChanges()` performance, use [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) and implement [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx), and [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types.</span></span>

## <a name="report-issues"></a><span data-ttu-id="07e86-149">Sorunları raporla</span><span class="sxs-lookup"><span data-stu-id="07e86-149">Report issues</span></span>

<span data-ttu-id="07e86-150">Beklendiği gibi çalışmayan hiçbir birleşim için [EF Core sorunu izleyicide](https://github.com/aspnet/entityframeworkcore/issues/new)yeni sorunlar oluşturulmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="07e86-150">For any combination that doesn�t work as expected, we encourage creating new issues on the [EF Core issue tracker](https://github.com/aspnet/entityframeworkcore/issues/new).</span></span> <span data-ttu-id="07e86-151">Xamarin 'e özgü sorunlarda, [Xamarin. Android](https://github.com/xamarin/xamarin-android/issues/new) veya [Xamarin. iOS](https://github.com/xamarin/xamarin-macios/issues/new)için sorun izleyicisini kullanın.</span><span class="sxs-lookup"><span data-stu-id="07e86-151">For Xamarin-specific issues use the issue tracker for [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) or [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span></span>
