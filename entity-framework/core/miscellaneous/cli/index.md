---
title: Komut satırı başvurusu - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: d43b01fc61bb1c9b678e12e41c27d7efe9a59fa5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490369"
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
