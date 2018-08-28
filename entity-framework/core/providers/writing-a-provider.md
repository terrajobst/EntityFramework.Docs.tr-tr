---
title: EF Core - veritabanı sağlayıcısı yazma
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993672"
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="cbc75-102">Veritabanı sağlayıcısı yazma</span><span class="sxs-lookup"><span data-stu-id="cbc75-102">Writing a Database Provider</span></span>

<span data-ttu-id="cbc75-103">Bir Entity Framework Core veritabanı sağlayıcısı yazma hakkında daha fazla bilgi için bkz: [EF Core sağlayıcısı yazmak istediğiniz şekilde](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) tarafından [Arthur Vickers](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="cbc75-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

> [!NOTE]
> <span data-ttu-id="cbc75-104">Bu postaları EF Core 1.1 güncelleştirilmemiş ve o zamandan bu yana yapılan önemli değişiklikler olmuştur [sorunu 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) güncelleştirmeleri bu belge için izlemektir.</span><span class="sxs-lookup"><span data-stu-id="cbc75-104">These posts have not been updated since EF Core 1.1 and there have been significant changes since that time [Issue 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) is tracking updates to this documentation.</span></span>

<span data-ttu-id="cbc75-105">EF Core kod tabanının açık kaynaklıdır ve başvuru olarak kullanılan birkaç veritabanı sağlayıcısı içerir.</span><span class="sxs-lookup"><span data-stu-id="cbc75-105">The EF Core codebase is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="cbc75-106">Kaynak kodu bulabilirsiniz https://github.com/aspnet/EntityFrameworkCore.</span><span class="sxs-lookup"><span data-stu-id="cbc75-106">You can find the source code at https://github.com/aspnet/EntityFrameworkCore.</span></span> <span data-ttu-id="cbc75-107">Ayrıca yaygın olarak kullanılan üçüncü taraf sağlayıcılar için gibi koda göz atmak yararlı olabilir [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), ve [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="cbc75-107">It may also be helpful to look at the code for commonly used third-party providers, such as [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), and [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span> <span data-ttu-id="cbc75-108">Özellikle, bu projelerin gelen genişletmek ve NuGet üzerinde biz yayımlama işlevsel testleri çalıştırmak için kurulum değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="cbc75-108">In particular, these projects are setup to extend from and run functional tests that we publish on NuGet.</span></span> <span data-ttu-id="cbc75-109">Bu tür bir kurulum önemle tavsiye edilir.</span><span class="sxs-lookup"><span data-stu-id="cbc75-109">This kind of setup is strongly recommended.</span></span>

## <a name="keeping-up-to-date-with-provider-changes"></a><span data-ttu-id="cbc75-110">Sağlayıcı değişiklikleri takip etme</span><span class="sxs-lookup"><span data-stu-id="cbc75-110">Keeping up-to-date with provider changes</span></span>

<span data-ttu-id="cbc75-111">Oluşturduk 2.1 sürümünden sonra iş ile başlayarak, bir [değişiklik günlüğünü](provider-log.md) sağlayıcı kodu karşılık gelen değişiklikleri gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="cbc75-111">Starting with work after the 2.1 release, we have created a [log of changes](provider-log.md) that may need corresponding changes in provider code.</span></span> <span data-ttu-id="cbc75-112">Bu yeni bir sürümü EF Core ile çalışmak için mevcut bir sağlayıcı güncelleştirirken yardımcı olmayı hedefler.</span><span class="sxs-lookup"><span data-stu-id="cbc75-112">This is intended to help when updating an existing provider to work with a new version of EF Core.</span></span>

<span data-ttu-id="cbc75-113">2.1 önce kullandığımız [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) ve [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) bizim GitHub sorunlar ve çekme istekleri benzer bir amaç için etiketleri.</span><span class="sxs-lookup"><span data-stu-id="cbc75-113">Prior to 2.1, we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our GitHub issues and pull requests for a similar purpose.</span></span> <span data-ttu-id="cbc75-114">Bu etiketleri sorunları hangi iş öğelerinin belirli bir sürüm sağlayıcıları yapılacak işleri de gerektirebilir göstergesidir vermek için kullanılacak continiue yapacağız.</span><span class="sxs-lookup"><span data-stu-id="cbc75-114">We will continiue to use these lables on issues to give an indication which work items in a given release may also require work to be done in providers.</span></span> <span data-ttu-id="cbc75-115">A `providers-beware` etiket genellikle anlamına gelir uygulama bir iş öğesinin sağlayıcıları bozabilir ancak bir `providers-fyi` etiket genellikle anlamına gelir sağlayıcıları bozuk olmayacak, ancak yine de, örneğin değiştirilmesi, yeni işlevselliği etkinleştirmek için kod gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="cbc75-115">A `providers-beware` label typically means that the implementation of an work item may break providers, while a `providers-fyi` label typically means that providers will not be broken, but code may need to be changed anyway, for example, to enable new functionality.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="cbc75-116">Önerilen adlandırma üçüncü taraf sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="cbc75-116">Suggested naming of third party providers</span></span>

<span data-ttu-id="cbc75-117">NuGet paketleri için aşağıdaki adlandırma kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="cbc75-117">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="cbc75-118">Bu, EF Core ekibi tarafından teslim edilen paket adları ile tutarlıdır.</span><span class="sxs-lookup"><span data-stu-id="cbc75-118">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="cbc75-119">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cbc75-119">For example:</span></span>
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
