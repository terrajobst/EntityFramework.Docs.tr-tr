---
title: Desteklenen .NET uygulamaları - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 02e9450cb0ead1701da9f58c51bef3031a3be4ed
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678682"
---
# <a name="net-implementations-supported-by-ef-core"></a>EF çekirdek tarafından desteklenen .NET uygulamaları

.NET kodu yazabilir ve bu amaç doğrultusunda hala çalışıyoruz her yerde kullanılabilir olması için EF çekirdek istiyoruz. Otomatikleştirilmiş test ederek .NET Core ve .NET Framework EF çekirdek'in desteğini ele sırada ve başarılı bir şekilde kullanılmasını bilinen birçok uygulama, Mono, Xamarin ve UWP bazı sorunlar vardır.

Aşağıdaki tabloda her .NET uygulama yönelik yönergeler sağlanmaktadır:

| .NET uygulaması                                                                                                  | Durum                                                             | EF çekirdek 1.x gereksinimleri                                                                                | EF çekirdek 2.x gereksinimleri <sup>(1)</sup>                                                                 |
|:---------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|
| **.NET core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [konsol](../get-started/netcore/index.md), vb..) | Tam olarak desteklenen ve önerilen                                    | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/)                                                | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)                                                |
| **.NET framework** (WinForms, WPF, ASP.NET, [konsol](../get-started/full-dotnet/index.md), vb..)                    | Tam olarak desteklenen ve önerilen. Ayrıca kullanılabilir EF6 <sup>(2)</sup> | .NET Framework 4.5.1                                                                                    | .NET Framework 4.6.1                                                                                    |
| **Mono & Xamarin**                                                                                                   | Devam eden <sup>(3)</sup>                                         | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7                               | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5                        |
| [**Evrensel Windows platformu**](../get-started/uwp/index.md)                                                        | EF çekirdek önerilen 2.0.1 <sup>(4)</sup>                           | [.NET core UWP 5.x paketi](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) | [.NET core UWP 6.x paketi](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) |

<sup>(1) </sup> EF çekirdek 2.0 hedefler ve bu nedenle destekleyen .NET uygulamalarında gerektirir [.NET standart 2.0](https://docs.microsoft.com/dotnet/standard/net-standard).

<sup>(2) </sup> Bkz [karşılaştırmak EF çekirdek & EF6](../../efcore-and-ef6/index.md) doğru teknolojiyi seçin.

<sup>(3) </sup> Sorunlar ve bazı uygulamaların düzgün çalışmasını EF çekirdek 2.0 kullanılarak geliştirilen engelleyebilir xamarin'le bilinen sınırlamalar vardır. [Etkin sorunlar] listesini kontrol edin ([ ](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) geçici çözümler için.

<sup>(4) </sup> EF çekirdek ve .NET UWP önceki sürümleri ile .NET yerel araç zinciri derlenmiş uygulamalarla özellikle çok sayıda uyumluluk sorunları vardı. Yeni .NET UWP sürümü .NET standart 2.0 için destek ekler ve .NET yerel çoğu önceden bildirilen uyumluluk sorunları giderir 2.0 içerir. EF çekirdek 2.0.1 daha kapsamlı UWP ile test edilmiştir ancak test otomatik.

Beklendiği gibi çalışmıyor herhangi bir birleşimini için Microsoft'un yeni sorunlar oluşturmanızı öneririz. [EF çekirdek sorun İzleyicisi](https://github.com/aspnet/entityframeworkcore/issues/new). İçin Xamarin özgü sorunları için sorun İzleyicisi'ni kullanın. [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) veya [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).
