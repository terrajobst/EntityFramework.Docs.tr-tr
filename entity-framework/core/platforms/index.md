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
# <a name="net-implementations-supported-by-ef-core"></a>.EF Core tarafından desteklenen NET uygulamaları

EF Core'un tüm modern .NET uygulamalarında geliştiricilerin kullanımına sunulmasını istiyoruz ve hala bu hedef doğrultusunda çalışıyoruz. EF Core'un .NET Core'daki desteği otomatik test kapsamında olmakla birlikte, başarılı bir şekilde kullandığı bilinen birçok uygulamanın mono, Xamarin ve UWP'nin bazı sorunları vardır.

## <a name="overview"></a>Genel Bakış

Aşağıdaki tablo her .NET uygulaması için kılavuz sağlar:

| EF Core                       | 2.1 ve 3.1 |
|:------------------------------|:------------|
| .NET Standard                 | 2,0         |
| .NET Core                     | 2,0         |
| .NET Çerçeve<sup>(1)</sup>  | 4.7.2       |
| Mono                          | 5,4         |
| Xamarin.iOS<sup>(2)</sup>     | 10.14       |
| Xamarin.Android<sup>(2)</sup> | 8.0         |
| UWP<sup>(3)</sup>             | 10.0.16299  |
| Birlik<sup>(4)</sup>           | 2018.1      |

<sup>(1)</sup> Aşağıdaki [.NET Framework](#net-framework) bölümüne bakınız.

<sup>(2)</sup> Xamarin ile ef core kullanılarak geliştirilen bazı uygulamaların doğru çalışmasını engelleyebilecek sorunlar ve bilinen sınırlamalar vardır. Geçici geçici çözüm için [etkin sorunların](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) listesini denetleyin.

<sup>(3)</sup> EF Core 2.0.1 ve daha yeni önerilir. [.NET Core UWP 6.x paketini](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/)yükleyin. Bu makalenin [Evrensel Windows Platformu](#universal-windows-platform) bölümüne bakın.

<sup>(4)</sup> Birlik ile ilgili sorunlar ve bilinen sınırlamalar vardır. [Etkin sorunların](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity)listesini kontrol edin.

## <a name="net-framework"></a>.NET Framework

.NET Framework'u hedefleyen uygulamaların .NET Standart kitaplıklarıyla çalışmak için değişikliklere ihtiyacı olabilir:

Proje dosyasını edin ve aşağıdaki girişin ilk özellik grubunda göründüğünden emin olun:

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

Test projeleri için aşağıdaki girişin de bulunduğundan emin olun:

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

Visual Studio'nun eski bir sürümünü kullanmak istiyorsanız, .NET Standard 2.0 kitaplıklarıyla çalışmak [üzere NuGet istemcisini sürüm 3.6.0'a yükselttiniz.](https://www.nuget.org/downloads)

Ayrıca NuGet package.config'den mümkünse PackageReference'a geçiş yapmanızı öneririz. Proje dosyanıza aşağıdaki özelliği ekleyin:

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a>Evrensel Windows Platformu

EF Core ve .NET UWP'nin önceki sürümlerinde, özellikle .NET Native araç zinciri yle derlenen uygulamalarda çok sayıda uyumluluk sorunu vardı. Yeni .NET UWP sürümü .NET Standard 2.0 için destek ekler ve daha önce bildirilen uyumluluk sorunlarının çoğunu gideren .NET Native 2.0 içerir. EF Core 2.0.1 UWP ile daha ayrıntılı olarak test edilmiştir ancak test otomatik değildir.

UWP'de EF Core kullanırken:

* Sorgu performansını en iyi duruma getirmek için LINQ sorgularında anonim türlerden kaçının. Bir UWP uygulamasını uygulama mağazasına dağıtmak için .NET Native ile derlenecek bir uygulama gerekir. Anonim türleri olan sorgular .NET Native'de daha kötü performansa sahiptir.

* Performansı `SaveChanges()` en iyi duruma getirmek için [ChangeTrackingStrategy.ChangingAndChangedNotifications'i](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) kullanın ve varlık türlerinizde [INotifyPropertyChanged , INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx)ve [INotifyCollectionChanged'i](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) uygulayın. [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx)

## <a name="report-issues"></a>Sorun bildirme

Beklendiği gibi çalışmayan herhangi bir kombinasyon [için, EF Core sorun izleyicisi](https://github.com/aspnet/entityframeworkcore/issues/new)üzerinde yeni sorunlar oluşturmayı öneririz. Xamarin'e özgü sorunlar için [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) veya [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new)için sorun izleyicisini kullanın.
