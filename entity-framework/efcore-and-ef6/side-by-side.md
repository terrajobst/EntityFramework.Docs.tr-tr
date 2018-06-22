---
title: EF6 ve EF çekirdek - aynı uygulamada kullanma
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: f6eb4bf7d99fbc61f8ffbd0dc7c6c17789395303
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054213"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>EF çekirdek ve EF6 aynı uygulamasında kullanma

EF çekirdek ve EF6 aynı .NET Framework uygulama ya da kitaplık hem NuGet paketlerini yükleyerek kullanmak da mümkündür. 

Bazı türleri EF çekirdek ve EF6 aynı ada sahip ve yalnızca ad, aynı kod dosyasında EF çekirdek ve EF6 kullanarak karmaşıklaştırır göre farklılık gösterir. Belirsizliği kolayca ad alanı diğer adı yönergeleri, örneğin kullanılarak kaldırılabilir:

``` csharp
using Microsoft.EntityFrameworkCore;
using EF6 = System.Data.Entity; // e.g. EF6.DbContext
```

Birden çok EF modelleri olan mevcut bir uygulama bağlantı noktası oluşturma, seçmeli olarak bazıları EF çekirdek için bağlantı noktası ve diğerleri için EF6 kullanmaya devam etmek seçebilirsiniz.
