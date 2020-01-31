---
title: EF6 ve EF Core-bunları aynı uygulamada kullanma
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: bcf0a26535c4ec880a9ac25478c987fb683f6d26
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888141"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Aynı uygulamada EF Core ve EF6 kullanma

Her iki NuGet paketini de yükleyerek aynı uygulama veya kitaplıkta EF Core ve EF6 kullanmak mümkündür.

Bazı türler EF Core ve EF6 ' de aynı adlara sahiptir ve yalnızca ad alanına göre farklılık gösterir. Bu, hem EF Core hem de EF6 aynı kod dosyasında kullanmayı karmaşıklaşmayabilir. Belirsizlik ad alanı diğer ad yönergeleri kullanılarak kolayca kaldırılabilir. Örneğin:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Birden çok EF modeli olan mevcut bir uygulamayı taşıma işlemi yapıyorsanız, bunlardan bazılarının EF Core için seçmeli bağlantı noktası seçebilirsiniz ve diğerleri için EF6 kullanmaya devam edebilirsiniz.
