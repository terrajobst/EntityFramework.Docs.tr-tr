---
title: Desteklenen .NET uygulamaları-EF Core
author: rowanmiller
ms.date: 08/30/2017
uid: core/platforms/index
ms.openlocfilehash: 6450884ea8f1b7bfd12d6b0c722b150b2574c5c3
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781202"
---
# <a name="net-implementations-supported-by-ef-core"></a>EF Core tarafından desteklenen .NET uygulamaları

EF Core tüm modern .NET uygulamalarındaki geliştiriciler için kullanılabilir olmasını istiyoruz ve bu hedefle çalışmaya devam ediyoruz. EF Core .NET Core üzerinde destek, otomatik test ve başarıyla kullanılması bilinen birçok uygulama tarafından ele alınmıştır, mono, Xamarin ve UWP bazı sorunlar yaşıyor.

## <a name="overview"></a>Genel bakış

Aşağıdaki tabloda her .NET uygulamasına yönelik rehberlik sunulmaktadır:

| EF Core                       | 2.1        | 3.0             | 3.1        |
|:------------------------------|:-----------|:----------------|:-----------|
| .NET Standard                 | 2,0        | 2.1             | 2,0        |
| .NET Core                     | 2,0        | 3.0             | 2,0        |
| .NET Framework<sup>(1)</sup>  | 4.7.2      | (desteklenmiyor) | 4.7.2      |
| Mono                          | 5,4        | 6.4             | 5,4        |
| Xamarin. iOS<sup>(2)</sup>     | 10,14      | 12,16           | 10,14      |
| Xamarin. Android<sup>(2)</sup> | 8.0        | 10.0            | 8.0        |
| UWP<sup>(3)</sup>             | 10.0.16299 | TBD             | 10.0.16299 |
| Unity<sup>(4)</sup>           | 2018,1     | TBD             | 2018,1     |

<sup>(1)</sup> aşağıdaki [.NET Framework](#net-framework) bölümüne bakın.

<sup>(2)</sup> Xamarin ile ilgili sorunlar ve bilinen sınırlamalar vardır ve EF Core kullanılarak geliştirilen bazı uygulamaların doğru şekilde çalışmasını engelleyebilir. Geçici çözümler için [etkin sorunlar](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) listesini denetleyin.

<sup>(3)</sup> EF Core 2.0.1 ve daha yeni önerilir. [.NET Core UWP 6. x paketini](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/)yükler. Bu makalenin [Evrensel Windows platformu](#universal-windows-platform) bölümüne bakın.

<sup>(4)</sup> Unity ile ilgili sorunlar ve bilinen sınırlamalar vardır. [Etkin sorunlar](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity)listesini denetleyin.

## <a name="net-framework"></a>.NET Framework

.NET Framework hedef uygulamaların .NET Standard kitaplıklarıyla çalışması için değişiklikler gerekebilir:

Proje dosyasını düzenleyin ve aşağıdaki girdinin ilk özellik grubunda göründüğünden emin olun:

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

Test projeleri için aşağıdaki girişin mevcut olduğundan da emin olun:

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

Visual Studio 'nun eski bir sürümünü kullanmak istiyorsanız, NuGet istemcisini .NET Standard 2,0 kitaplıklarıyla çalışmak üzere [3.6.0 sürümüne yükseltdiğinizden](https://www.nuget.org/downloads) emin olun.

Ayrıca, NuGet paketleri. config ' den, mümkünse PackageReference 'a geçiş yapmanızı öneririz. Aşağıdaki özelliği proje dosyanıza ekleyin:

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a>Evrensel Windows Platformu

EF Core ve .NET UWP 'nin önceki sürümlerinde, özellikle .NET Native araç zinciri ile derlenen uygulamalarla çok sayıda uyumluluk sorunu vardı. Yeni .NET UWP sürümü, .NET Standard 2,0 için destek ekler ve daha önce bildirilen uyumluluk sorunlarının çoğunu düzelten .NET Native 2,0 içerir. EF Core 2.0.1 UWP ile daha kapsamlı bir şekilde sınanmıştır ancak test otomatik değildir.

UWP üzerinde EF Core kullanırken:

* Sorgu performansını iyileştirmek için LINQ sorgularında anonim türlerden kaçının. Bir UWP uygulamasını App Store 'a dağıtmak için bir uygulamanın .NET Native ile derlenmesi gerekir. Anonim türlere sahip sorguların .NET Native performansı kötüleştiğini.

* `SaveChanges()` performansını iyileştirmek için, [Changetrackingstrateji. ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) kullanın ve varlık Türlerinize [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx)ve [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) uygulayın.

## <a name="report-issues"></a>Sorunları raporla

Beklendiği gibi çalışmayan hiçbir birleşim için [EF Core sorunu izleyicide](https://github.com/aspnet/entityframeworkcore/issues/new)yeni sorunlar oluşturulmasını öneririz. Xamarin 'e özgü sorunlarda, [Xamarin. Android](https://github.com/xamarin/xamarin-android/issues/new) veya [Xamarin. iOS](https://github.com/xamarin/xamarin-macios/issues/new)için sorun izleyicisini kullanın.
