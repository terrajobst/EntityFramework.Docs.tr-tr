---
title: EF6 ve EF Core-bunları aynı uygulamada kullanma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: 8bf9f51c0e5c4b1b3adf4a6a9a894689dc13d2d9
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149289"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Aynı uygulamada EF Core ve EF6 kullanma

Her iki NuGet paketini de yükleyerek aynı uygulama veya kitaplıkta EF Core ve EF6 kullanmak mümkündür.

Bazı türler EF Core ve EF6 ' de aynı adlara sahiptir ve yalnızca ad alanına göre farklılık gösterir. Bu, hem EF Core hem de EF6 aynı kod dosyasında kullanmayı karmaşıklaşmayabilir. Belirsizlik ad alanı diğer ad yönergeleri kullanılarak kolayca kaldırılabilir. Örneğin:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Birden çok EF modeli olan mevcut bir uygulamayı taşıma işlemi yapıyorsanız, bunlardan bazılarının EF Core için seçmeli bağlantı noktası seçebilirsiniz ve diğerleri için EF6 kullanmaya devam edebilirsiniz.
