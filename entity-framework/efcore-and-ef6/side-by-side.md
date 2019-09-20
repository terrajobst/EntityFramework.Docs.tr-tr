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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="44e20-102">Aynı uygulamada EF Core ve EF6 kullanma</span><span class="sxs-lookup"><span data-stu-id="44e20-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="44e20-103">Her iki NuGet paketini de yükleyerek aynı uygulama veya kitaplıkta EF Core ve EF6 kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="44e20-103">It is possible to use EF Core and EF6 in the same application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="44e20-104">Bazı türler EF Core ve EF6 ' de aynı adlara sahiptir ve yalnızca ad alanına göre farklılık gösterir. Bu, hem EF Core hem de EF6 aynı kod dosyasında kullanmayı karmaşıklaşmayabilir.</span><span class="sxs-lookup"><span data-stu-id="44e20-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="44e20-105">Belirsizlik ad alanı diğer ad yönergeleri kullanılarak kolayca kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="44e20-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="44e20-106">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="44e20-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="44e20-107">Birden çok EF modeli olan mevcut bir uygulamayı taşıma işlemi yapıyorsanız, bunlardan bazılarının EF Core için seçmeli bağlantı noktası seçebilirsiniz ve diğerleri için EF6 kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="44e20-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
