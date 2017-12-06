---
title: "Desteklenen .NET uygulamaları - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 6b6ea9ca833810169d0d850f3bea8776b761c007
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="net-implementations-supported-by-ef-core"></a>EF çekirdek tarafından desteklenen .NET uygulamaları

.NET kodu yazabilir ve bu amaç doğrultusunda hala çalışıyoruz her yerde kullanılabilir olması için EF çekirdek istiyoruz. Aşağıdaki tabloda nerede EF çekirdek etkinleştirmek için istiyoruz her .NET uygulama yönelik yönergeler sağlanmaktadır.

EF çekirdek 2.0 hedefleri [.NET standart 2.0](https://docs.microsoft.com/dotnet/standard/net-standard) ve bu nedenle onu destekleyen .NET uygulamalarında gerektirir.

| .NET uygulaması | Durum | 1.x gerektirir | 2.x gerektirir
|-|-|-|-
| **.NET core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [konsol](../get-started/netcore/index.md), vb..) | **Tam olarak desteklenir ve önerilen:** otomatik test tarafından Covered ve başarılı bir şekilde kullanılmasını bilinen birçok uygulama. | [.NET core SDK 1.x](https://www.microsoft.com/net/core/) | [.NET core SDK 2.x](https://www.microsoft.com/net/core/)
| **.NET framework** (WinForms, WPF, ASP.NET, [konsol](../get-started/full-dotnet/index.md), vb..) | **Tam olarak desteklenir ve önerilen:** otomatik test tarafından Covered ve başarılı bir şekilde kullanılmasını bilinen birçok uygulama. EF 6 Bu platform da kullanılabilir (bkz [karşılaştırmak EF çekirdek & EF6](../../efcore-and-ef6/index.md) doğru teknolojiyi seçmek için). | .NET Framework 4.5.1 | .NET framework 4.6.1
| **Mono & Xamarin** | **Devam ediyor – sorunlarla karşılaşılabilir:** bazı test EF çekirdek ekip ve müşteriler tarafından gerçekleştirilen. Erken Benimseyenler, bazı başarı bildirilen ancak [sorunları karşılaştı](https://github.com/aspnet/entityframework/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) ve diğerleri büyük olasılıkla sınama olarak bitişik devam eder. Özellikle, bazı uygulamaların düzgün çalışmasını EF çekirdek 2.0 kullanılarak geliştirilen engelleyebilir Xamarin.iOS sınırlamaları vardır. | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7 | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5
| [**Evrensel Windows platformu**](../get-started/uwp/index.md) | **Devam ediyor – sorunlarla karşılaşılabilir:** bazı test EF çekirdek ekip ve müşteriler tarafından gerçekleştirilen. [Sorunları](https://github.com/aspnet/entityframework/issues?utf8=%E2%9C%93&q=is%3Aopen%20is%3Aissue%20label%3Aarea-uwp%20) genellikle yayın derlemesi sırasında kullanılır ve (, .NET yerel kullanmıyorsanız, veya yalnızca deneme, pek çok sorunu giderir istiyorsanız Windows Mağazası'na dağıtmak için bir gerekliliktir .NET yerel araç zinciri ile derlendiğinde bildirdi etkilemez). | [En son .NET UWP 5 paketi](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/5.4.1) | [En son .NET UWP 6 paketi](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) <sup>(1)</sup>

<sup>(1) </sup> .NET UWP bu sürümü .NET standart 2.0 için destek ekler ve .NET yerel uyumluluk çoğunu giderir 2.0 içeren sorunları önceden bildirdi ancak test ortaya [birkaç sorunları kalan](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A2.0.1+label%3Aarea-uwp) EF çekirdek Yaklaşan düzeltme eki sürümde adresine planladığınıza 2.0.

Birleşiminden her zaman beklendiği gibi çalışmıyor yeni sorunları oluşturma olan öneririz [EF çekirdek sorun İzleyicisi](https://github.com/aspnet/entityframeworkcore/issues/new), ve Xamarin için ilgili sorunlar, [Xamarin sorun İzleyicisi](https://bugzilla.xamarin.com/newbug).
