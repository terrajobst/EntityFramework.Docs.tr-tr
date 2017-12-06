---
title: "Komut satırı başvurusu - EF çekirdek"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
<a name="entity-framework-core-tools"></a>Entity Framework Çekirdek araçları
===========================
Entity Framework Çekirdek Araçlar EF çekirdek uygulamaları geliştirme sırasında yardımcı olur. Öncelikle bir veritabanı şeması mühendislik ters tarafından DbContext ve varlık türleri iskele ve geçişler yönetmek için kullanılırlar.

[EF çekirdek Paket Yöneticisi Konsolu (PMC) araçları] [ 1] Visual Studio içinde üst düzey bir deneyim sağlar. Bunları çalıştırma NuGet kullanarak [Paket Yöneticisi Konsolu][2]. Bu araçlar, .NET Framework ve .NET Core projeleri ile çalışır.

[EF çekirdek .NET komut satırı araçları] [ 3] bir uzantısı olan [.NET Core komut satırı arabirimi (CLI) araçları] [ 4] olan platformlar arası ve Visual Studio dışında çalıştırabilirsiniz. Bu araçlar .NET Core SDK projesi gerektirir (biriyle `Sdk="Microsoft.NET.Sdk"` veya proje dosyasında benzer).

Her iki araçları aynı işlevselliği kullanıma sunar. Visual Studio'da geliştiriyorsanız daha tümleşik bir deneyim sağlamak beri PMC araçları kullanmanızı öneririz.

<a name="frameworks"></a>çerçeveler
----------
Proje .NET Framework veya .NET Core hedefleme destek araçları.

Projenize başka bir framework (örneğin, Evrensel Windows veya Xamarin) hedefliyorsa, ayrı bir oluşturmanızı öneririz .NET standart proje ve desteklenen çerçeveleri arası hedefleme biri.

Çapraz hedef .NET Core için örneğin, projeyi sağ tıklatın ve **Düzenle \*.csproj**. Güncelleştirme `TargetFramework` şekilde özelliği. (Not, özellik adı çoğul olur.)

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

.NET standart bir sınıf kitaplığı kullanıyorsanız, çapraz başlangıç projeniz .NET Framework veya .NET Core hedefliyorsa hedefi gerekmez.

<a name="startup-and-target-projects"></a>Başlangıç ve hedef projeleri
---------------------------
Bir komut çağırma her iki proje dahil olan: hedef projeyi ve başlangıç projesi.

Hedef dosyalar eklendiği (veya kaldırılan bazı durumlarda) projesidir.

Başlangıç projesi projenizin kodu çalıştırırken araçları tarafından Öykünülen adrestir.

Hedef projeyi ve başlangıç projesi aynı olabilir.


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
