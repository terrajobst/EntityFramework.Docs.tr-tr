---
title: EF6 ve EF Core - bunları aynı uygulamada kullanma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: 6f95c02f4f24746605794832b1e26744fc554580
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995715"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>EF Core ve EF6 aynı uygulamada kullanma

EF Core ve EF6 aynı .NET Framework uygulamasına veya Kitaplığı hem NuGet paketlerini yükleyerek kullanmak mümkündür.

Bazı türleri EF Core ve EF6 aynı ada sahip ve yalnızca ad alanını aynı kod dosyasında hem EF Core ve EF6 kullanarak karmaşıklaştırır göre farklılık gösterir. Belirsizlik ad alanı diğer ad yönergeleri kullanarak kolayca kaldırabilirsiniz. Örneğin:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Birden çok EF modeli olan bir var olan bir uygulama bağlantı noktası oluşturma, seçmeli olarak bazı EF core'a taşıma ve EF6 diğerleri için kullanmaya devam etmek seçebilirsiniz.
