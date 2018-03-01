---
title: "Veritabanı sağlayıcısı - EF çekirdek yazma"
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 4bddf5858ab2c6b2fd22571a20edb3f7c85e2853
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="e0fa8-102">Veritabanı sağlayıcısı yazma</span><span class="sxs-lookup"><span data-stu-id="e0fa8-102">Writing a Database Provider</span></span>

<span data-ttu-id="e0fa8-103">Bir Entity Framework Çekirdek veritabanı sağlayıcısı yazma hakkında daha fazla bilgi için bkz: [EF çekirdek sağlayıcısı yazmak istediğiniz şekilde](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) tarafından [Arthur Vickers](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="e0fa8-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

<span data-ttu-id="e0fa8-104">EF çekirdek kod tabanını açık kaynaklıdır ve başvuru olarak kullanılan birkaç veritabanı sağlayıcıları içerir.</span><span class="sxs-lookup"><span data-stu-id="e0fa8-104">The EF Core code base is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="e0fa8-105">Kaynak kodu https://github.com/aspnet/EntityFrameworkCore bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0fa8-105">You can find the source code at https://github.com/aspnet/EntityFrameworkCore.</span></span>

## <a name="the-providers-beware-label"></a><span data-ttu-id="e0fa8-106">Etiket sağlayıcıları-kaybolacağını unutmayın</span><span class="sxs-lookup"><span data-stu-id="e0fa8-106">The providers-beware label</span></span>

<span data-ttu-id="e0fa8-107">Bir sağlayıcı üzerinde çalışmaya sonra izlemesi [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) bizim GitHub sorunlar ve çekme istekleri etiketi.</span><span class="sxs-lookup"><span data-stu-id="e0fa8-107">Once you begin work on a provider, watch for the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) label on our GitHub issues and pull requests.</span></span> <span data-ttu-id="e0fa8-108">Bu etiket sağlayıcısı yazıcılarının etkileyebilecek değişiklikleri belirlemek için kullanırız.</span><span class="sxs-lookup"><span data-stu-id="e0fa8-108">We use this label to identify changes that may impact provider writers.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="e0fa8-109">Üçüncü taraf sağlayıcılar adlandırma önerilen</span><span class="sxs-lookup"><span data-stu-id="e0fa8-109">Suggested naming of third party providers</span></span>

<span data-ttu-id="e0fa8-110">NuGet paketleri için aşağıdaki adlandırma kullanarak öneririz.</span><span class="sxs-lookup"><span data-stu-id="e0fa8-110">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="e0fa8-111">Bu, EF çekirdek ekibi tarafından teslim edilen paket adları ile tutarlıdır.</span><span class="sxs-lookup"><span data-stu-id="e0fa8-111">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="e0fa8-112">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e0fa8-112">For example:</span></span>
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
