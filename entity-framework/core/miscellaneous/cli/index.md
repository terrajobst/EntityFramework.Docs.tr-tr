---
title: Komut satırı başvurusu - EF çekirdek
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: db25ed55e3724ee71743e563f39a6e4b16c17589
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
ms.locfileid: "29769429"
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

Ardından, bir sınıf kitaplığı kullanmak istiyorsanız, bir .NET Core veya .NET Framework sınıf kitaplığı mümkünse kullanmayı düşünün. Bu en az sorunları .NET araçları ile sonuçlanır. Sonra .NET standart bir sınıf kitaplığı kullanmak istiyorsanız bunun yerine, içine sınıf kitaplığı yükleyebilir conrete hedef platformu araçları sahip olması başlangıç projesi hedefleyen .NET Framework veya .NET Core kullanmanız gerekecektir. Bu başlangıç projesi kukla proje olabilir ile gerçek kodu yok--bunu yalnızca bir hedef için araç sağlamak için gereklidir.

Projenize başka bir framework (örneğin, Evrensel Windows veya Xamarin) hedefliyorsa, ayrı bir .NET standart sınıf kitaplığı oluşturmanız gerekir. Bu durumda, aynı zamanda araçları tarafından kullanılan bir başlangıç projesi oluşturmak için yukarıdaki yönergeleri izleyin.

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
