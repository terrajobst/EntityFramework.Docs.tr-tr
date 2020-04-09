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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="b33d2-102">EF Core ve EF6'yı Aynı Uygulamada Kullanma</span><span class="sxs-lookup"><span data-stu-id="b33d2-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="b33d2-103">Her iki NuGet paketini de yükleyerek EF Core ve EF6'yı aynı uygulama da veya kütüphanede kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="b33d2-103">It is possible to use EF Core and EF6 in the same application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="b33d2-104">Bazı türler EF Core ve EF6'da aynı ada sahiptir ve yalnızca ad alanına göre farklılık gösterir ve bu da aynı kod dosyasında hem EF Core hem de EF6'yı kullanarak karmaşıkhale gelebilir.</span><span class="sxs-lookup"><span data-stu-id="b33d2-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="b33d2-105">Belirsizlik, ad alanı diğer ad yönergeleri kullanılarak kolayca kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b33d2-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="b33d2-106">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b33d2-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="b33d2-107">Birden çok EF modeli olan varolan bir uygulamayı taşımayı, bazılarını seçerek EF Core'a taşımayı seçebilir ve diğerleri için EF6 kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b33d2-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
