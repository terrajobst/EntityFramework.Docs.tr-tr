---
title: "Komut satırı başvurusu - EF çekirdek"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="bd7cc-102">Entity Framework Çekirdek araçları</span><span class="sxs-lookup"><span data-stu-id="bd7cc-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="bd7cc-103">Entity Framework Çekirdek Araçlar EF çekirdek uygulamaları geliştirme sırasında yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="bd7cc-104">Öncelikle bir veritabanı şeması mühendislik ters tarafından DbContext ve varlık türleri iskele ve geçişler yönetmek için kullanılırlar.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="bd7cc-105">[EF çekirdek Paket Yöneticisi Konsolu (PMC) araçları] [ 1] Visual Studio içinde üst düzey bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="bd7cc-106">Bunları çalıştırma NuGet kullanarak [Paket Yöneticisi Konsolu][2].</span><span class="sxs-lookup"><span data-stu-id="bd7cc-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="bd7cc-107">Bu araçlar, .NET Framework ve .NET Core projeleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="bd7cc-108">[EF çekirdek .NET komut satırı araçları] [ 3] bir uzantısı olan [.NET Core komut satırı arabirimi (CLI) araçları] [ 4] olan platformlar arası ve Visual Studio dışında çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="bd7cc-109">Bu araçlar .NET Core SDK projesi gerektirir (biriyle `Sdk="Microsoft.NET.Sdk"` veya proje dosyasında benzer).</span><span class="sxs-lookup"><span data-stu-id="bd7cc-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="bd7cc-110">Her iki araçları aynı işlevselliği kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="bd7cc-111">Visual Studio'da geliştiriyorsanız daha tümleşik bir deneyim sağlamak beri PMC araçları kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="bd7cc-112">çerçeveler</span><span class="sxs-lookup"><span data-stu-id="bd7cc-112">Frameworks</span></span>
----------
<span data-ttu-id="bd7cc-113">Proje .NET Framework veya .NET Core hedefleme destek araçları.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="bd7cc-114">Projenize başka bir framework (örneğin, Evrensel Windows veya Xamarin) hedefliyorsa, ayrı bir oluşturmanızı öneririz .NET standart proje ve desteklenen çerçeveleri arası hedefleme biri.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-114">If your project targets another framework (for example, Universal Windows or Xamarin), we recommend creating a separate .NET Standard project and cross-targeting one of the supported frameworks.</span></span>

<span data-ttu-id="bd7cc-115">Çapraz hedef .NET Core için örneğin, projeyi sağ tıklatın ve **Düzenle \*.csproj**.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-115">To cross-target .NET Core, for example, right-click on the project and select **Edit \*.csproj**.</span></span> <span data-ttu-id="bd7cc-116">Güncelleştirme `TargetFramework` şekilde özelliği.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-116">Update the `TargetFramework` property as follows.</span></span> <span data-ttu-id="bd7cc-117">(Not, özellik adı çoğul olur.)</span><span class="sxs-lookup"><span data-stu-id="bd7cc-117">(Note, the property name becomes plural.)</span></span>

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

<span data-ttu-id="bd7cc-118">.NET standart bir sınıf kitaplığı kullanıyorsanız, çapraz başlangıç projeniz .NET Framework veya .NET Core hedefliyorsa hedefi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-118">If you're using a .NET Standard class library, you don't need to cross-target if your startup project targets .NET Framework or .NET Core.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="bd7cc-119">Başlangıç ve hedef projeleri</span><span class="sxs-lookup"><span data-stu-id="bd7cc-119">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="bd7cc-120">Bir komut çağırma her iki proje dahil olan: hedef projeyi ve başlangıç projesi.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-120">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="bd7cc-121">Hedef dosyalar eklendiği (veya kaldırılan bazı durumlarda) projesidir.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-121">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="bd7cc-122">Başlangıç projesi projenizin kodu çalıştırırken araçları tarafından Öykünülen adrestir.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-122">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="bd7cc-123">Hedef projeyi ve başlangıç projesi aynı olabilir.</span><span class="sxs-lookup"><span data-stu-id="bd7cc-123">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
