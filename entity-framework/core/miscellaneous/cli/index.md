---
title: "Komut satırı başvurusu - EF çekirdek"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: db25ed55e3724ee71743e563f39a6e4b16c17589
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="367df-102">Entity Framework Çekirdek araçları</span><span class="sxs-lookup"><span data-stu-id="367df-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="367df-103">Entity Framework Çekirdek Araçlar EF çekirdek uygulamaları geliştirme sırasında yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="367df-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="367df-104">Öncelikle bir veritabanı şeması mühendislik ters tarafından DbContext ve varlık türleri iskele ve geçişler yönetmek için kullanılırlar.</span><span class="sxs-lookup"><span data-stu-id="367df-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="367df-105">[EF çekirdek Paket Yöneticisi Konsolu (PMC) araçları] [ 1] Visual Studio içinde üst düzey bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="367df-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="367df-106">Bunları çalıştırma NuGet kullanarak [Paket Yöneticisi Konsolu][2].</span><span class="sxs-lookup"><span data-stu-id="367df-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="367df-107">Bu araçlar, .NET Framework ve .NET Core projeleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="367df-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="367df-108">[EF çekirdek .NET komut satırı araçları] [ 3] bir uzantısı olan [.NET Core komut satırı arabirimi (CLI) araçları] [ 4] olan platformlar arası ve Visual Studio dışında çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="367df-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="367df-109">Bu araçlar .NET Core SDK projesi gerektirir (biriyle `Sdk="Microsoft.NET.Sdk"` veya proje dosyasında benzer).</span><span class="sxs-lookup"><span data-stu-id="367df-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="367df-110">Her iki araçları aynı işlevselliği kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="367df-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="367df-111">Visual Studio'da geliştiriyorsanız daha tümleşik bir deneyim sağlamak beri PMC araçları kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="367df-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="367df-112">çerçeveler</span><span class="sxs-lookup"><span data-stu-id="367df-112">Frameworks</span></span>
----------
<span data-ttu-id="367df-113">Proje .NET Framework veya .NET Core hedefleme destek araçları.</span><span class="sxs-lookup"><span data-stu-id="367df-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="367df-114">Ardından, bir sınıf kitaplığı kullanmak istiyorsanız, bir .NET Core veya .NET Framework sınıf kitaplığı mümkünse kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="367df-114">If you want to use a class library, then consider using a .NET Core or .NET Framework class library if possible.</span></span> <span data-ttu-id="367df-115">Bu en az sorunları .NET araçları ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="367df-115">This will result in the least issues with .NET tooling.</span></span> <span data-ttu-id="367df-116">Sonra .NET standart bir sınıf kitaplığı kullanmak istiyorsanız bunun yerine, içine sınıf kitaplığı yükleyebilir conrete hedef platformu araçları sahip olması başlangıç projesi hedefleyen .NET Framework veya .NET Core kullanmanız gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="367df-116">If instead you wish to use a .NET Standard class library, then you will need to use a startup project that targets .NET Framework or .NET Core so that the tooling has a conrete target platform into which it can load your class library.</span></span> <span data-ttu-id="367df-117">Bu başlangıç projesi kukla proje olabilir ile gerçek kodu yok--bunu yalnızca bir hedef için araç sağlamak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="367df-117">This startup project can be a dummy project with no real code--it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="367df-118">Projenize başka bir framework (örneğin, Evrensel Windows veya Xamarin) hedefliyorsa, ayrı bir .NET standart sınıf kitaplığı oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="367df-118">If your project targets another framework (for example, Universal Windows or Xamarin), then you will need to create a separate .NET Standard class library.</span></span> <span data-ttu-id="367df-119">Bu durumda, aynı zamanda araçları tarafından kullanılan bir başlangıç projesi oluşturmak için yukarıdaki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="367df-119">In this case, follow the guidance above to also create a startup project that can be used by the tooling.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="367df-120">Başlangıç ve hedef projeleri</span><span class="sxs-lookup"><span data-stu-id="367df-120">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="367df-121">Bir komut çağırma her iki proje dahil olan: hedef projeyi ve başlangıç projesi.</span><span class="sxs-lookup"><span data-stu-id="367df-121">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="367df-122">Hedef dosyalar eklendiği (veya kaldırılan bazı durumlarda) projesidir.</span><span class="sxs-lookup"><span data-stu-id="367df-122">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="367df-123">Başlangıç projesi projenizin kodu çalıştırırken araçları tarafından Öykünülen adrestir.</span><span class="sxs-lookup"><span data-stu-id="367df-123">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="367df-124">Hedef projeyi ve başlangıç projesi aynı olabilir.</span><span class="sxs-lookup"><span data-stu-id="367df-124">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
