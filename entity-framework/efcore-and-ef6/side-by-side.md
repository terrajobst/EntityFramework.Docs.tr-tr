---
title: EF6 ve EF Core - Aynı Uygulamada Kullanma
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: bcf0a26535c4ec880a9ac25478c987fb683f6d26
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419646"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>EF Core ve EF6'yı Aynı Uygulamada Kullanma

Her iki NuGet paketini de yükleyerek EF Core ve EF6'yı aynı uygulama da veya kütüphanede kullanmak mümkündür.

Bazı türler EF Core ve EF6'da aynı ada sahiptir ve yalnızca ad alanına göre farklılık gösterir ve bu da aynı kod dosyasında hem EF Core hem de EF6'yı kullanarak karmaşıkhale gelebilir. Belirsizlik, ad alanı diğer ad yönergeleri kullanılarak kolayca kaldırılabilir. Örneğin:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Birden çok EF modeli olan varolan bir uygulamayı taşımayı, bazılarını seçerek EF Core'a taşımayı seçebilir ve diğerleri için EF6 kullanmaya devam edebilirsiniz.
