---
title: Komut satırı başvurusu - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.openlocfilehash: 757d6562f5d3bcd4f026def02f208f5827786873
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997076"
---
<a name="entity-framework-core-tools"></a>Entity Framework Core araçları
===========================
Entity Framework Core araçları EF Core uygulamaları geliştirme sırasında yardımcı olur. Öncelikle bir DbContext ve varlık türleri tarafından ters mühendislik bir veritabanı şeması iskelesini kurmak ve geçişleri yönetmek için kullanılırlar.

[EF Core Paket Yöneticisi Konsolu (PMC) araçları] [ 1] Visual Studio içinde üstün bir deneyim sağlar. Bunları kullanarak NuGet [Paket Yöneticisi Konsolu][2]. Bu araçlar, .NET Framework hem de .NET Core projeleriyle çalışır.

[EF Core .NET komut satırı araçları] [ 3] uzantısı olan [.NET Core komut satırı arabirimi (CLI) araçlarını] [ 4] olan platformlar arası ve Visual Studio'nun dışında çalıştırabilirsiniz. Bu araçları, .NET Core SDK projesindeki gerektirir (biriyle `Sdk="Microsoft.NET.Sdk"` veya proje dosyasında benzer).

Her iki araç aynı işlevselliği kullanıma sunar. Visual Studio'da geliştiriyorsanız, bunlar daha tümleşik bir deneyim sunmak olduğundan PMC Araçları'nı kullanmanızı öneririz.

<a name="frameworks"></a>Çerçeveler
----------
Araçlar, .NET Framework veya .NET Core hedefleyen projeleri destekler.

Ardından, bir sınıf kitaplığı kullanmak istiyorsanız, bir .NET Core veya .NET Framework sınıf kitaplığı mümkünse kullanmayı düşünün. Bu, .NET araçları ile en az sorunlarına neden olur. Ardından bunun yerine bir .NET Standard sınıf kitaplığı kullanmak istiyorsanız, içine, sınıf kitaplığı yükleyebilirsiniz conrete hedef platform araçlarına sahip olacak şekilde bir başlangıç projesi hedefleyen .NET Framework veya .NET Core kullanmanız gerekir. Bu başlangıç projesi işlevsiz bir proje olabilir ile gerçek kod--bu yalnızca bir hedef için bir araç sağlamak için gereklidir.

Projeniz başka bir framework (örneğin, Evrensel Windows veya Xamarin) hedefliyorsa, ayrı bir .NET Standard sınıf kitaplığı oluşturmak gerekir. Bu durumda, ayrıca araç tarafından kullanılabilmesi için bir başlangıç projesi oluşturmak için yukarıdaki yönergeleri izleyin.

<a name="startup-and-target-projects"></a>Başlatma ve hedef projeleri
---------------------------
Bir komut çağırma her iki proje kullanılan vardır: hedef projeye ve başlangıç projesi.

Hedef dosyalar eklenir (veya bazı durumlarda kaldırıldı) projesidir.

Başlangıç projesi, proje kodunuzun yürütülürken araçları tarafından Öykünülen bir bileşendir.

Hedef proje hem başlangıç projesi aynı olabilir.


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
