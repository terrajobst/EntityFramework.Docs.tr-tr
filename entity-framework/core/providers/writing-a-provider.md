---
title: Veritabanı sağlayıcısı yazma-EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d52a8581772cc5405e94966fa7ebdff4128c252
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654773"
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="e2bf5-102">Veritabanı Sağlayıcısı Yazma</span><span class="sxs-lookup"><span data-stu-id="e2bf5-102">Writing a Database Provider</span></span>

<span data-ttu-id="e2bf5-103">Entity Framework Core veritabanı sağlayıcısı yazma hakkında daha fazla bilgi için, bkz. [Arthur Vicranlar](https://github.com/ajcvickers)tarafından [bir EF Core sağlayıcısı yazmak istiyor](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) .</span><span class="sxs-lookup"><span data-stu-id="e2bf5-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

> [!NOTE]
> <span data-ttu-id="e2bf5-104">Bu gönderimler EF Core 1,1 ' den beri güncelleştirilmemiş ve bu süre [681 sorunu](https://github.com/aspnet/EntityFramework.Docs/issues/681) bu belgelerde yapılan güncelleştirmeleri izlemediğinden önemli değişiklikler yapıldı.</span><span class="sxs-lookup"><span data-stu-id="e2bf5-104">These posts have not been updated since EF Core 1.1 and there have been significant changes since that time [Issue 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) is tracking updates to this documentation.</span></span>

<span data-ttu-id="e2bf5-105">EF Core kod temeli açık kaynaktır ve başvuru olarak kullanılabilecek çeşitli veritabanı sağlayıcıları içerir.</span><span class="sxs-lookup"><span data-stu-id="e2bf5-105">The EF Core codebase is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="e2bf5-106">Kaynak kodunu <https://github.com/aspnet/EntityFrameworkCore>' de bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e2bf5-106">You can find the source code at <https://github.com/aspnet/EntityFrameworkCore>.</span></span> <span data-ttu-id="e2bf5-107">Ayrıca, [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)ve [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact)gibi yaygın olarak kullanılan üçüncü taraf sağlayıcılarının koduna bakmak da yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="e2bf5-107">It may also be helpful to look at the code for commonly used third-party providers, such as [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), and [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span> <span data-ttu-id="e2bf5-108">Özellikle, bu projeler, NuGet üzerinde yayımladığımız işlev testlerini genişletmek ve çalıştırmak için kurulumlardır.</span><span class="sxs-lookup"><span data-stu-id="e2bf5-108">In particular, these projects are setup to extend from and run functional tests that we publish on NuGet.</span></span> <span data-ttu-id="e2bf5-109">Bu tür bir kurulum kesinlikle önerilir.</span><span class="sxs-lookup"><span data-stu-id="e2bf5-109">This kind of setup is strongly recommended.</span></span>

## <a name="keeping-up-to-date-with-provider-changes"></a><span data-ttu-id="e2bf5-110">Sağlayıcı değişiklikleriyle güncel tutma</span><span class="sxs-lookup"><span data-stu-id="e2bf5-110">Keeping up-to-date with provider changes</span></span>

<span data-ttu-id="e2bf5-111">2,1 sürümünden sonra çalışmaya başladıktan sonra, sağlayıcı kodunda ilgili değişikliklere ihtiyacı olabilecek bir [değişiklikler günlüğü](provider-log.md) oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="e2bf5-111">Starting with work after the 2.1 release, we have created a [log of changes](provider-log.md) that may need corresponding changes in provider code.</span></span> <span data-ttu-id="e2bf5-112">Bu, mevcut bir sağlayıcıyı EF Core yeni bir sürümüyle çalışacak şekilde güncelleştirirken yardımcı olmaya yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="e2bf5-112">This is intended to help when updating an existing provider to work with a new version of EF Core.</span></span>

<span data-ttu-id="e2bf5-113">2,1 ' dan önce, GitHub sorunlarımızda [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) ve [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etiketleri ve benzer bir amaç için çekme isteklerimize kullandık.</span><span class="sxs-lookup"><span data-stu-id="e2bf5-113">Prior to 2.1, we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our GitHub issues and pull requests for a similar purpose.</span></span> <span data-ttu-id="e2bf5-114">Bu etiketleri sorunlarını, belirli bir sürümdeki iş öğelerinin, sağlayıcılarda hangi iş öğelerinin yapılmasını gerektirdiğini belirten bir belirtiye vermek için de kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="e2bf5-114">We will continiue to use these lables on issues to give an indication which work items in a given release may also require work to be done in providers.</span></span> <span data-ttu-id="e2bf5-115">Bir `providers-beware` etiketi genellikle bir iş öğesi uygulamasının sağlayıcıları bozabileceğinden, bir `providers-fyi` etiketinin genellikle sağlayıcıların kesilmeyeceği, ancak örneğin yeni işlevselliği etkinleştirmek için de kodun değiştirilmesi gerekebilecek anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e2bf5-115">A `providers-beware` label typically means that the implementation of an work item may break providers, while a `providers-fyi` label typically means that providers will not be broken, but code may need to be changed anyway, for example, to enable new functionality.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="e2bf5-116">Üçüncü taraf sağlayıcıların önerilen adlandırma</span><span class="sxs-lookup"><span data-stu-id="e2bf5-116">Suggested naming of third party providers</span></span>

<span data-ttu-id="e2bf5-117">NuGet paketleri için aşağıdaki adlandırmanın kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="e2bf5-117">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="e2bf5-118">Bu, EF Core ekibi tarafından teslim edilen paketlerin adlarıyla tutarlıdır.</span><span class="sxs-lookup"><span data-stu-id="e2bf5-118">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="e2bf5-119">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e2bf5-119">For example:</span></span>

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
