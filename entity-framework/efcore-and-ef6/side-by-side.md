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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="1ec31-102">Aynı uygulamada EF Core ve EF6 kullanma</span><span class="sxs-lookup"><span data-stu-id="1ec31-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="1ec31-103">Her iki NuGet paketini de yükleyerek aynı uygulama veya kitaplıkta EF Core ve EF6 kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="1ec31-103">It is possible to use EF Core and EF6 in the same application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="1ec31-104">Bazı türler EF Core ve EF6 ' de aynı adlara sahiptir ve yalnızca ad alanına göre farklılık gösterir. Bu, hem EF Core hem de EF6 aynı kod dosyasında kullanmayı karmaşıklaşmayabilir.</span><span class="sxs-lookup"><span data-stu-id="1ec31-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="1ec31-105">Belirsizlik ad alanı diğer ad yönergeleri kullanılarak kolayca kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ec31-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="1ec31-106">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ec31-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="1ec31-107">Birden çok EF modeli olan mevcut bir uygulamayı taşıma işlemi yapıyorsanız, bunlardan bazılarının EF Core için seçmeli bağlantı noktası seçebilirsiniz ve diğerleri için EF6 kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ec31-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
