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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="ed638-102">EF Core ve EF6 aynı uygulamada kullanma</span><span class="sxs-lookup"><span data-stu-id="ed638-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="ed638-103">EF Core ve EF6 aynı .NET Framework uygulamasına veya Kitaplığı hem NuGet paketlerini yükleyerek kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="ed638-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="ed638-104">Bazı türleri EF Core ve EF6 aynı ada sahip ve yalnızca ad alanını aynı kod dosyasında hem EF Core ve EF6 kullanarak karmaşıklaştırır göre farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="ed638-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="ed638-105">Belirsizlik ad alanı diğer ad yönergeleri kullanarak kolayca kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed638-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="ed638-106">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ed638-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="ed638-107">Birden çok EF modeli olan bir var olan bir uygulama bağlantı noktası oluşturma, seçmeli olarak bazı EF core'a taşıma ve EF6 diğerleri için kullanmaya devam etmek seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed638-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
