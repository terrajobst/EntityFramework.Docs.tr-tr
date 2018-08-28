---
title: Komut satırı başvurusu - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.openlocfilehash: 757d6562f5d3bcd4f026def02f208f5827786873
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997076"
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="691a9-102">Entity Framework Core araçları</span><span class="sxs-lookup"><span data-stu-id="691a9-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="691a9-103">Entity Framework Core araçları EF Core uygulamaları geliştirme sırasında yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="691a9-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="691a9-104">Öncelikle bir DbContext ve varlık türleri tarafından ters mühendislik bir veritabanı şeması iskelesini kurmak ve geçişleri yönetmek için kullanılırlar.</span><span class="sxs-lookup"><span data-stu-id="691a9-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="691a9-105">[EF Core Paket Yöneticisi Konsolu (PMC) araçları] [ 1] Visual Studio içinde üstün bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="691a9-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="691a9-106">Bunları kullanarak NuGet [Paket Yöneticisi Konsolu][2].</span><span class="sxs-lookup"><span data-stu-id="691a9-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="691a9-107">Bu araçlar, .NET Framework hem de .NET Core projeleriyle çalışır.</span><span class="sxs-lookup"><span data-stu-id="691a9-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="691a9-108">[EF Core .NET komut satırı araçları] [ 3] uzantısı olan [.NET Core komut satırı arabirimi (CLI) araçlarını] [ 4] olan platformlar arası ve Visual Studio'nun dışında çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="691a9-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="691a9-109">Bu araçları, .NET Core SDK projesindeki gerektirir (biriyle `Sdk="Microsoft.NET.Sdk"` veya proje dosyasında benzer).</span><span class="sxs-lookup"><span data-stu-id="691a9-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="691a9-110">Her iki araç aynı işlevselliği kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="691a9-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="691a9-111">Visual Studio'da geliştiriyorsanız, bunlar daha tümleşik bir deneyim sunmak olduğundan PMC Araçları'nı kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="691a9-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="691a9-112">Çerçeveler</span><span class="sxs-lookup"><span data-stu-id="691a9-112">Frameworks</span></span>
----------
<span data-ttu-id="691a9-113">Araçlar, .NET Framework veya .NET Core hedefleyen projeleri destekler.</span><span class="sxs-lookup"><span data-stu-id="691a9-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="691a9-114">Ardından, bir sınıf kitaplığı kullanmak istiyorsanız, bir .NET Core veya .NET Framework sınıf kitaplığı mümkünse kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="691a9-114">If you want to use a class library, then consider using a .NET Core or .NET Framework class library if possible.</span></span> <span data-ttu-id="691a9-115">Bu, .NET araçları ile en az sorunlarına neden olur.</span><span class="sxs-lookup"><span data-stu-id="691a9-115">This will result in the least issues with .NET tooling.</span></span> <span data-ttu-id="691a9-116">Ardından bunun yerine bir .NET Standard sınıf kitaplığı kullanmak istiyorsanız, içine, sınıf kitaplığı yükleyebilirsiniz conrete hedef platform araçlarına sahip olacak şekilde bir başlangıç projesi hedefleyen .NET Framework veya .NET Core kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="691a9-116">If instead you wish to use a .NET Standard class library, then you will need to use a startup project that targets .NET Framework or .NET Core so that the tooling has a conrete target platform into which it can load your class library.</span></span> <span data-ttu-id="691a9-117">Bu başlangıç projesi işlevsiz bir proje olabilir ile gerçek kod--bu yalnızca bir hedef için bir araç sağlamak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="691a9-117">This startup project can be a dummy project with no real code--it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="691a9-118">Projeniz başka bir framework (örneğin, Evrensel Windows veya Xamarin) hedefliyorsa, ayrı bir .NET Standard sınıf kitaplığı oluşturmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="691a9-118">If your project targets another framework (for example, Universal Windows or Xamarin), then you will need to create a separate .NET Standard class library.</span></span> <span data-ttu-id="691a9-119">Bu durumda, ayrıca araç tarafından kullanılabilmesi için bir başlangıç projesi oluşturmak için yukarıdaki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="691a9-119">In this case, follow the guidance above to also create a startup project that can be used by the tooling.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="691a9-120">Başlatma ve hedef projeleri</span><span class="sxs-lookup"><span data-stu-id="691a9-120">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="691a9-121">Bir komut çağırma her iki proje kullanılan vardır: hedef projeye ve başlangıç projesi.</span><span class="sxs-lookup"><span data-stu-id="691a9-121">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="691a9-122">Hedef dosyalar eklenir (veya bazı durumlarda kaldırıldı) projesidir.</span><span class="sxs-lookup"><span data-stu-id="691a9-122">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="691a9-123">Başlangıç projesi, proje kodunuzun yürütülürken araçları tarafından Öykünülen bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="691a9-123">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="691a9-124">Hedef proje hem başlangıç projesi aynı olabilir.</span><span class="sxs-lookup"><span data-stu-id="691a9-124">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
