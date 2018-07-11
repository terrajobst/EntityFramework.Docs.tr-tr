---
title: EF6 ve EF Core - bunları aynı uygulamada kullanma
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: ead251c5454473738c2f2bfdac6557aa3e1c5591
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949084"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>EF Core ve EF6 aynı uygulamada kullanma

EF Core ve EF6 aynı .NET Framework uygulamasına veya Kitaplığı hem NuGet paketlerini yükleyerek kullanmak mümkündür.

Bazı türleri EF Core ve EF6 aynı ada sahip ve yalnızca ad alanını aynı kod dosyasında hem EF Core ve EF6 kullanarak karmaşıklaştırır göre farklılık gösterir. Belirsizlik ad alanı diğer ad yönergeleri kullanarak kolayca kaldırabilirsiniz. Örneğin:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Birden çok EF modeli olan bir var olan bir uygulama bağlantı noktası oluşturma, seçmeli olarak bazı EF core'a taşıma ve EF6 diğerleri için kullanmaya devam etmek seçebilirsiniz.
