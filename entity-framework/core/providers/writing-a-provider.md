---
title: "Veritabanı sağlayıcısı - EF çekirdek yazma"
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d6d3748a9097b3b8eeee2a8a516c53f3b2afa58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="b6b29-102">Veritabanı sağlayıcısı yazma</span><span class="sxs-lookup"><span data-stu-id="b6b29-102">Writing a Database Provider</span></span>

<span data-ttu-id="b6b29-103">Bir Entity Framework Çekirdek veritabanı sağlayıcısı yazma hakkında daha fazla bilgi için bkz: [EF çekirdek sağlayıcısı yazmak istediğiniz şekilde](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) tarafından [Arthur Vickers](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="b6b29-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

<span data-ttu-id="b6b29-104">EF çekirdek kod tabanını açık kaynaklıdır ve başvuru olarak kullanılan birkaç veritabanı sağlayıcıları içerir.</span><span class="sxs-lookup"><span data-stu-id="b6b29-104">The EF Core code base is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="b6b29-105">Kaynak kodu https://github.com/aspnet/EntityFramework bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b6b29-105">You can find the source code at https://github.com/aspnet/EntityFramework.</span></span>

## <a name="the-providers-beware-label"></a><span data-ttu-id="b6b29-106">Etiket sağlayıcıları-kaybolacağını unutmayın</span><span class="sxs-lookup"><span data-stu-id="b6b29-106">The providers-beware label</span></span>

<span data-ttu-id="b6b29-107">Bir sağlayıcı üzerinde çalışmaya sonra izlemesi [ `providers-beware` ](https://github.com/aspnet/EntityFramework/labels/providers-beware) bizim GitHub sorunlar ve çekme istekleri etiketi.</span><span class="sxs-lookup"><span data-stu-id="b6b29-107">Once you begin work on a provider, watch for the [`providers-beware`](https://github.com/aspnet/EntityFramework/labels/providers-beware) label on our GitHub issues and pull requests.</span></span> <span data-ttu-id="b6b29-108">Bu etiket sağlayıcısı yazıcılarının etkileyebilecek değişiklikleri belirlemek için kullanırız.</span><span class="sxs-lookup"><span data-stu-id="b6b29-108">We use this label to identify changes that may impact provider writers.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="b6b29-109">Üçüncü taraf sağlayıcılar adlandırma önerilen</span><span class="sxs-lookup"><span data-stu-id="b6b29-109">Suggested naming of third party providers</span></span>

<span data-ttu-id="b6b29-110">NuGet paketleri için aşağıdaki adlandırma kullanarak öneririz.</span><span class="sxs-lookup"><span data-stu-id="b6b29-110">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="b6b29-111">Bu, EF çekirdek ekibi tarafından teslim edilen paket adları ile tutarlıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b29-111">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="b6b29-112">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b6b29-112">For example:</span></span>
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
