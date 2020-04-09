---
title: Desteklenen .NET uygulamaları - EF Core
author: bricelam
ms.date: 03/03/2020
uid: core/platforms/index
ms.openlocfilehash: 693d4cae85eddf86d01e17084415147c52a008c7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417324"
---
# <a name="net-implementations-supported-by-ef-core"></a><span data-ttu-id="1e417-102">.EF Core tarafından desteklenen NET uygulamaları</span><span class="sxs-lookup"><span data-stu-id="1e417-102">.NET implementations supported by EF Core</span></span>

<span data-ttu-id="1e417-103">EF Core'un tüm modern .NET uygulamalarında geliştiricilerin kullanımına sunulmasını istiyoruz ve hala bu hedef doğrultusunda çalışıyoruz.</span><span class="sxs-lookup"><span data-stu-id="1e417-103">We want EF Core to be available to developers on all modern .NET implementations, and we're still working towards that goal.</span></span> <span data-ttu-id="1e417-104">EF Core'un .NET Core'daki desteği otomatik test kapsamında olmakla birlikte, başarılı bir şekilde kullandığı bilinen birçok uygulamanın mono, Xamarin ve UWP'nin bazı sorunları vardır.</span><span class="sxs-lookup"><span data-stu-id="1e417-104">While EF Core's support on .NET Core is covered by automated testing and many applications known to be using it successfully, Mono, Xamarin and UWP have some issues.</span></span>

## <a name="overview"></a><span data-ttu-id="1e417-105">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="1e417-105">Overview</span></span>

<span data-ttu-id="1e417-106">Aşağıdaki tablo her .NET uygulaması için kılavuz sağlar:</span><span class="sxs-lookup"><span data-stu-id="1e417-106">The following table provides guidance for each .NET implementation:</span></span>

| <span data-ttu-id="1e417-107">EF Core</span><span class="sxs-lookup"><span data-stu-id="1e417-107">EF Core</span></span>                       | <span data-ttu-id="1e417-108">2.1 ve 3.1</span><span class="sxs-lookup"><span data-stu-id="1e417-108">2.1 and 3.1</span></span> |
|:------------------------------|:------------|
| <span data-ttu-id="1e417-109">.NET Standard</span><span class="sxs-lookup"><span data-stu-id="1e417-109">.NET Standard</span></span>                 | <span data-ttu-id="1e417-110">2,0</span><span class="sxs-lookup"><span data-stu-id="1e417-110">2.0</span></span>         |
| <span data-ttu-id="1e417-111">.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e417-111">.NET Core</span></span>                     | <span data-ttu-id="1e417-112">2,0</span><span class="sxs-lookup"><span data-stu-id="1e417-112">2.0</span></span>         |
| <span data-ttu-id="1e417-113">.NET Çerçeve<sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="1e417-113">.NET Framework<sup>(1)</sup></span></span>  | <span data-ttu-id="1e417-114">4.7.2</span><span class="sxs-lookup"><span data-stu-id="1e417-114">4.7.2</span></span>       |
| <span data-ttu-id="1e417-115">Mono</span><span class="sxs-lookup"><span data-stu-id="1e417-115">Mono</span></span>                          | <span data-ttu-id="1e417-116">5,4</span><span class="sxs-lookup"><span data-stu-id="1e417-116">5.4</span></span>         |
| <span data-ttu-id="1e417-117">Xamarin.iOS<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="1e417-117">Xamarin.iOS<sup>(2)</sup></span></span>     | <span data-ttu-id="1e417-118">10.14</span><span class="sxs-lookup"><span data-stu-id="1e417-118">10.14</span></span>       |
| <span data-ttu-id="1e417-119">Xamarin.Android<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="1e417-119">Xamarin.Android<sup>(2)</sup></span></span> | <span data-ttu-id="1e417-120">8.0</span><span class="sxs-lookup"><span data-stu-id="1e417-120">8.0</span></span>         |
| <span data-ttu-id="1e417-121">UWP<sup>(3)</sup></span><span class="sxs-lookup"><span data-stu-id="1e417-121">UWP<sup>(3)</sup></span></span>             | <span data-ttu-id="1e417-122">10.0.16299</span><span class="sxs-lookup"><span data-stu-id="1e417-122">10.0.16299</span></span>  |
| <span data-ttu-id="1e417-123">Birlik<sup>(4)</sup></span><span class="sxs-lookup"><span data-stu-id="1e417-123">Unity<sup>(4)</sup></span></span>           | <span data-ttu-id="1e417-124">2018.1</span><span class="sxs-lookup"><span data-stu-id="1e417-124">2018.1</span></span>      |

<span data-ttu-id="1e417-125"><sup>(1)</sup> Aşağıdaki [.NET Framework](#net-framework) bölümüne bakınız.</span><span class="sxs-lookup"><span data-stu-id="1e417-125"><sup>(1)</sup> See the [.NET Framework](#net-framework) section below.</span></span>

<span data-ttu-id="1e417-126"><sup>(2)</sup> Xamarin ile ef core kullanılarak geliştirilen bazı uygulamaların doğru çalışmasını engelleyebilecek sorunlar ve bilinen sınırlamalar vardır.</span><span class="sxs-lookup"><span data-stu-id="1e417-126"><sup>(2)</sup> There are issues and known limitations with Xamarin which may prevent some applications developed using EF Core from working correctly.</span></span> <span data-ttu-id="1e417-127">Geçici geçici çözüm için [etkin sorunların](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) listesini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="1e417-127">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) for workarounds.</span></span>

<span data-ttu-id="1e417-128"><sup>(3)</sup> EF Core 2.0.1 ve daha yeni önerilir.</span><span class="sxs-lookup"><span data-stu-id="1e417-128"><sup>(3)</sup> EF Core 2.0.1 and newer recommended.</span></span> <span data-ttu-id="1e417-129">[.NET Core UWP 6.x paketini](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/)yükleyin.</span><span class="sxs-lookup"><span data-stu-id="1e417-129">Install the [.NET Core UWP 6.x package](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span></span> <span data-ttu-id="1e417-130">Bu makalenin [Evrensel Windows Platformu](#universal-windows-platform) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="1e417-130">See the [Universal Windows Platform](#universal-windows-platform) section of this article.</span></span>

<span data-ttu-id="1e417-131"><sup>(4)</sup> Birlik ile ilgili sorunlar ve bilinen sınırlamalar vardır.</span><span class="sxs-lookup"><span data-stu-id="1e417-131"><sup>(4)</sup> There are issues and known limitations with Unity.</span></span> <span data-ttu-id="1e417-132">[Etkin sorunların](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity)listesini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="1e417-132">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span></span>

## <a name="net-framework"></a><span data-ttu-id="1e417-133">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="1e417-133">.NET Framework</span></span>

<span data-ttu-id="1e417-134">.NET Framework'u hedefleyen uygulamaların .NET Standart kitaplıklarıyla çalışmak için değişikliklere ihtiyacı olabilir:</span><span class="sxs-lookup"><span data-stu-id="1e417-134">Applications that target .NET Framework may need changes to work with .NET Standard libraries:</span></span>

<span data-ttu-id="1e417-135">Proje dosyasını edin ve aşağıdaki girişin ilk özellik grubunda göründüğünden emin olun:</span><span class="sxs-lookup"><span data-stu-id="1e417-135">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

<span data-ttu-id="1e417-136">Test projeleri için aşağıdaki girişin de bulunduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="1e417-136">For test projects, also make sure the following entry is present:</span></span>

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

<span data-ttu-id="1e417-137">Visual Studio'nun eski bir sürümünü kullanmak istiyorsanız, .NET Standard 2.0 kitaplıklarıyla çalışmak [üzere NuGet istemcisini sürüm 3.6.0'a yükselttiniz.](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="1e417-137">If you want to use an older version of Visual Studio, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

<span data-ttu-id="1e417-138">Ayrıca NuGet package.config'den mümkünse PackageReference'a geçiş yapmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="1e417-138">We also recommend migrating from NuGet packages.config to PackageReference if possible.</span></span> <span data-ttu-id="1e417-139">Proje dosyanıza aşağıdaki özelliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="1e417-139">Add the following property to your project file:</span></span>

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a><span data-ttu-id="1e417-140">Evrensel Windows Platformu</span><span class="sxs-lookup"><span data-stu-id="1e417-140">Universal Windows Platform</span></span>

<span data-ttu-id="1e417-141">EF Core ve .NET UWP'nin önceki sürümlerinde, özellikle .NET Native araç zinciri yle derlenen uygulamalarda çok sayıda uyumluluk sorunu vardı.</span><span class="sxs-lookup"><span data-stu-id="1e417-141">Earlier versions of EF Core and .NET UWP had numerous compatibility issues, especially with applications compiled with the .NET Native toolchain.</span></span> <span data-ttu-id="1e417-142">Yeni .NET UWP sürümü .NET Standard 2.0 için destek ekler ve daha önce bildirilen uyumluluk sorunlarının çoğunu gideren .NET Native 2.0 içerir.</span><span class="sxs-lookup"><span data-stu-id="1e417-142">The new .NET UWP version adds support for .NET Standard 2.0 and contains .NET Native 2.0, which fixes most of the compatibility issues previously reported.</span></span> <span data-ttu-id="1e417-143">EF Core 2.0.1 UWP ile daha ayrıntılı olarak test edilmiştir ancak test otomatik değildir.</span><span class="sxs-lookup"><span data-stu-id="1e417-143">EF Core 2.0.1 has been tested more thoroughly with UWP but testing is not automated.</span></span>

<span data-ttu-id="1e417-144">UWP'de EF Core kullanırken:</span><span class="sxs-lookup"><span data-stu-id="1e417-144">When using EF Core on UWP:</span></span>

* <span data-ttu-id="1e417-145">Sorgu performansını en iyi duruma getirmek için LINQ sorgularında anonim türlerden kaçının.</span><span class="sxs-lookup"><span data-stu-id="1e417-145">To optimize query performance, avoid anonymous types in LINQ queries.</span></span> <span data-ttu-id="1e417-146">Bir UWP uygulamasını uygulama mağazasına dağıtmak için .NET Native ile derlenecek bir uygulama gerekir.</span><span class="sxs-lookup"><span data-stu-id="1e417-146">Deploying a UWP application to the app store requires an application to be compiled with .NET Native.</span></span> <span data-ttu-id="1e417-147">Anonim türleri olan sorgular .NET Native'de daha kötü performansa sahiptir.</span><span class="sxs-lookup"><span data-stu-id="1e417-147">Queries with anonymous types have worse performance on .NET Native.</span></span>

* <span data-ttu-id="1e417-148">Performansı `SaveChanges()` en iyi duruma getirmek için [ChangeTrackingStrategy.ChangingAndChangedNotifications'i](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) kullanın ve varlık türlerinizde [INotifyPropertyChanged , INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx)ve [INotifyCollectionChanged'i](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) uygulayın. [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx)</span><span class="sxs-lookup"><span data-stu-id="1e417-148">To optimize `SaveChanges()` performance, use [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) and implement [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx), and [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types.</span></span>

## <a name="report-issues"></a><span data-ttu-id="1e417-149">Sorun bildirme</span><span class="sxs-lookup"><span data-stu-id="1e417-149">Report issues</span></span>

<span data-ttu-id="1e417-150">Beklendiği gibi çalışmayan herhangi bir kombinasyon [için, EF Core sorun izleyicisi](https://github.com/aspnet/entityframeworkcore/issues/new)üzerinde yeni sorunlar oluşturmayı öneririz.</span><span class="sxs-lookup"><span data-stu-id="1e417-150">For any combination that doesn�t work as expected, we encourage creating new issues on the [EF Core issue tracker](https://github.com/aspnet/entityframeworkcore/issues/new).</span></span> <span data-ttu-id="1e417-151">Xamarin'e özgü sorunlar için [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) veya [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new)için sorun izleyicisini kullanın.</span><span class="sxs-lookup"><span data-stu-id="1e417-151">For Xamarin-specific issues use the issue tracker for [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) or [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span></span>
