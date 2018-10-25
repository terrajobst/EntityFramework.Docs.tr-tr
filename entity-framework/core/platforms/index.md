---
title: Desteklenen .NET uygulamaları - EF Core
author: rowanmiller
ms.date: 08/30/2017
uid: core/platforms/index
ms.openlocfilehash: 8fc25f4a35794162c92fd292990c24e977d1bf1b
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022268"
---
# <a name="net-implementations-supported-by-ef-core"></a>EF Core tarafından desteklenen .NET uygulamaları

EF Core, .NET kodu yazabilirsiniz ve bu amaç doğrultusunda yine de çalışıyoruz her yerde kullanılabilir olmasını istiyoruz. EF Core'nın destek .NET Core ve .NET Framework, otomatikleştirilmiş test ederek ele alınmaktadır ancak ve başarılı bir şekilde kullanılmasını bilinen birçok uygulama, Mono, Xamarin ve UWP bazı sorunlar vardır.

## <a name="overview"></a>Genel Bakış

Aşağıdaki tabloda her bir .NET uygulaması için yönergeler sağlar:

| .NET uygulaması                                                                                                  | Durum                                                             | EF Core 1.x gereksinimleri                                                                                | EF Core 2.x gereksinimleri <sup>(1)</sup>                                                                 |
|:---------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|
| **.NET core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [konsol](../get-started/netcore/index.md)vb..) | Tam olarak desteklenen ve önerilen                                    | [.NET core SDK'sı 1.x](https://www.microsoft.com/net/core/)                                                | [.NET core SDK 2.x](https://www.microsoft.com/net/core/)                                                |
| **.NET framework** (WinForms, WPF, ASP.NET, [konsol](../get-started/full-dotnet/index.md)vb..)                    | Tam olarak desteklenen ve önerilen. Ayrıca kullanılabilir EF6 <sup>(2)</sup> | .NET Framework 4.5.1                                                                                    | .NET Framework 4.6.1                                                                                    |
| **Mono ve Xamarin**                                                                                                   | Devam eden <sup>(3)</sup>                                         | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7                               | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5                        |
| [**Evrensel Windows platformu**](../get-started/uwp/index.md)                                                        | EF Core önerilen 2.0.1 <sup>(4)</sup>                           | [.NET core UWP 5.x paketi](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) | [.NET core UWP 6.x paket](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) |

<sup>(1) </sup> EF Core 2.0 hedefler ve bu nedenle destekleyen .NET uygulamaları [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard).

<sup>(2) </sup> Bkz [karşılaştırma EF Core ve EF6](../../efcore-and-ef6/index.md) doğru teknolojiyi seçme.

<sup>(3) </sup> Sorunları ve bazı uygulamaların düzgün çalışmasını EF Core 2.0 kullanarak geliştirilen engelleyebilecek Xamarin ile bilinen sınırlamalar vardır. Listesini kontrol edin [etkin sorunlar](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) geçici çözümler için.

<sup>(4) </sup> Bkz [Evrensel Windows platformu](#universal-windows-platform) bu makalenin.

## <a name="universal-windows-platform"></a>Evrensel Windows Platformu

EF Core ve .NET UWP önceki sürümlerinde, özellikle .NET Native araç zinciri ile derlenmiş uygulamalar ile çeşitli uyumluluk sorunları vardı. Yeni bir .NET UWP sürümü, .NET Standard 2.0 için destek ekler ve .NET yerel, çoğu daha önce bildirilen uyumluluk sorunlarını giderir 2.0 içerir. EF Core 2.0.1 daha kapsamlı UWP ile test edilmiştir, ancak test otomatik.

EF Core, UWP üzerinde kullanıldığında:

* Sorgu performansını iyileştirmek için LINQ sorgularında anonim türler kaçının. Bir UWP uygulaması app Store'a dağıtma .NET Native ile derlenen bir uygulamanın gerektirir. Anonim türler sorgularla daha zayıf performans üzerinde .NET Native vardır.

* En iyi duruma getirme `SaveChanges()` performansı, kullanım [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) ve uygulama [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging ](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx), ve [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) , varlık türleri.

## <a name="report-issues"></a>Rapor sorunları

Beklendiği gibi çalışmıyor herhangi bir birleşimini için yeni sorunlar oluşturma konusunda olan öneriyoruz [EF Core sorun İzleyicisi](https://github.com/aspnet/entityframeworkcore/issues/new). İçin Xamarin özgü sorunlar için sorun İzleyicisi'ni kullanın. [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) veya [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).
