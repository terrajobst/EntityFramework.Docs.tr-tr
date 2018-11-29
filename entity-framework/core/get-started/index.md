---
title: Alma başlatıldı - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: b846d63f2c285a43d60eecfb2be3d460a5d31924
ms.sourcegitcommit: 064b09431f05848830e145a6cd65cad58881557c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52552600"
---
# <a name="getting-started-with-entity-framework-core"></a><span data-ttu-id="d967e-102">Entity Framework Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="d967e-102">Getting Started with Entity Framework Core</span></span>

## <a name="installing-ef-coreinstallindexmd"></a>[<span data-ttu-id="d967e-103">EF Core’u Yükleme</span><span class="sxs-lookup"><span data-stu-id="d967e-103">Installing EF Core</span></span>](install/index.md)

<span data-ttu-id="d967e-104">Farklı platformlar ve popüler Ide'leri, uygulamanın EF Core eklemek gerekli adımları bir özeti.</span><span class="sxs-lookup"><span data-stu-id="d967e-104">A summary of the steps necessary to add EF Core to your application in different platforms and popular IDEs.</span></span>

## <a name="step-by-step-tutorials"></a><span data-ttu-id="d967e-105">Adım adım öğreticiler</span><span class="sxs-lookup"><span data-stu-id="d967e-105">Step-by-step Tutorials</span></span>

<span data-ttu-id="d967e-106">Bu Tanıtım öğreticilerine önceki Entity Framework Core olanağıyla veya herhangi bir IDE'ye gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d967e-106">These introductory tutorials require no previous knowledge of Entity Framework Core or any particular IDE.</span></span> <span data-ttu-id="d967e-107">Bunlar, sorgular ve verileri bir veritabanından kaydeden basit bir uygulama oluşturmak adım adım sürer.</span><span class="sxs-lookup"><span data-stu-id="d967e-107">They will take you step-by-step through the creation of a simple application that queries and saves data from a database.</span></span> <span data-ttu-id="d967e-108">Çeşitli işletim sistemlerini ve uygulama türleri üzerinde çalışmaya başlamanızı sağlayacak öğreticiler sağladık.</span><span class="sxs-lookup"><span data-stu-id="d967e-108">We have provided tutorials to get you started on various operating systems and application types.</span></span>

<span data-ttu-id="d967e-109">Entity Framework Core varolan bir veritabanını temel alan bir model oluşturmak veya size, modelinize dayalı bir veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d967e-109">Entity Framework Core can create a model based on an existing database, or create a database for you based on your model.</span></span> <span data-ttu-id="d967e-110">Bu yaklaşımların her ikisini de gösteren öğreticiler vardır.</span><span class="sxs-lookup"><span data-stu-id="d967e-110">There are tutorials that demonstrate both of these approaches.</span></span>

* <span data-ttu-id="d967e-111">.NET core konsol uygulamaları</span><span class="sxs-lookup"><span data-stu-id="d967e-111">.NET Core Console Apps</span></span>
  * [<span data-ttu-id="d967e-112">Yeni Veritabanı</span><span class="sxs-lookup"><span data-stu-id="d967e-112">New Database</span></span>](netcore/new-db-sqlite.md)
* <span data-ttu-id="d967e-113">ASP.NET Core uygulamaları</span><span class="sxs-lookup"><span data-stu-id="d967e-113">ASP.NET Core Apps</span></span>
  * [<span data-ttu-id="d967e-114">Yeni Veritabanı</span><span class="sxs-lookup"><span data-stu-id="d967e-114">New Database</span></span>](aspnetcore/new-db.md)
  * [<span data-ttu-id="d967e-115">Mevcut Veritabanı</span><span class="sxs-lookup"><span data-stu-id="d967e-115">Existing Database</span></span>](aspnetcore/existing-db.md)
  * [<span data-ttu-id="d967e-116">EF Core ve Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d967e-116">EF Core and Razor Pages</span></span>](/aspnet/core/data/ef-rp/intro)
* <span data-ttu-id="d967e-117">Evrensel Windows Platformu (UWP) uygulamaları</span><span class="sxs-lookup"><span data-stu-id="d967e-117">Universal Windows Platform (UWP) Apps</span></span>
  * [<span data-ttu-id="d967e-118">Yeni Veritabanı</span><span class="sxs-lookup"><span data-stu-id="d967e-118">New Database</span></span>](uwp/getting-started.md)
* <span data-ttu-id="d967e-119">.NET framework uygulamaları</span><span class="sxs-lookup"><span data-stu-id="d967e-119">.NET Framework Apps</span></span>
  * [<span data-ttu-id="d967e-120">Yeni Veritabanı</span><span class="sxs-lookup"><span data-stu-id="d967e-120">New Database</span></span>](full-dotnet/new-db.md)
  * [<span data-ttu-id="d967e-121">Mevcut Veritabanı</span><span class="sxs-lookup"><span data-stu-id="d967e-121">Existing Database</span></span>](full-dotnet/existing-db.md)

> [!NOTE]  
> <span data-ttu-id="d967e-122">Bu öğreticileri ve eşlik eden örnekleri EF Core 2.1 kullanacak şekilde güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="d967e-122">These tutorials and the accompanying samples have been updated to use EF Core 2.1.</span></span> <span data-ttu-id="d967e-123">Ancak, çoğu durumda en az değişiklik yönergeleri ile önceki sürümleri kullanan uygulamalar oluşturma olanağı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d967e-123">However, in the majority of cases it should be possible to create applications that use previous releases, with minimal modification to the instructions.</span></span> 
