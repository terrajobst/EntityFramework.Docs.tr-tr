---
title: Zaman uyumsuz sorgular-EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: 415c57df599f1cb1a255f01d1072890fedfd6d2b
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921688"
---
# <a name="asynchronous-queries"></a>Zaman Uyumsuz Sorgular

Zaman uyumsuz sorgular, sorgu veritabanında yürütüldüğü sırada bir iş parçacığının engellenmesini önler. Bu, bir kalın istemci uygulamasının Kullanıcı arabirimini dondurmaktan kaçınmak için yararlı olabilir. Zaman uyumsuz işlemler, bir Web uygulamasındaki aktarım hızını da artırabilir ve bu durumda iş parçacığı, veritabanı işlemi tamamlanırken diğer isteklere hizmet vermek için serbest bırakılabilirler. Daha fazla bilgi için bkz. [zaman uyumsuz C#programlama ](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core, aynı bağlam örneğinde çalıştırılan birden çok paralel işlemi desteklemez. Sonraki işleme başlamadan önce her zaman bir işlemin tamamlanmasını beklemeniz gerekir. Bu, genellikle her zaman uyumsuz işlemdeki `await` anahtar sözcüğü kullanılarak yapılır.

Entity Framework Core, bir sorgunun yürütülmesine ve sonuçlar döndürülmesine neden olan LINQ yöntemlerine alternatif olarak kullanılabilecek bir dizi zaman uyumsuz uzantı yöntemi sağlar. Örnekler şunlardır `ToListAsync()` `ToArrayAsync()` ,,vb`SingleAsync()`. `Where(...)` ,`OrderBy(...)`Vb. gibi LINQ işleçlerinin zaman uyumsuz sürümleri yoktur, çünkü bu yöntemler yalnızca LINQ ifade ağacını oluşturur ve sorgunun veritabanında yürütülmesine neden olmaz.

> [!IMPORTANT]  
> EF Core zaman uyumsuz uzantı yöntemleri `Microsoft.EntityFrameworkCore` ad alanında tanımlanmıştır. Yöntemlerin kullanılabilmesi için bu ad alanı içeri aktarılmalıdır.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#Sample)]
