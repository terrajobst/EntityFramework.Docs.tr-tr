---
title: Dizinleri - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993223"
---
# <a name="indexes"></a><span data-ttu-id="5fdc5-102">Dizinleri</span><span class="sxs-lookup"><span data-stu-id="5fdc5-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="5fdc5-103">Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5fdc5-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="5fdc5-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="5fdc5-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="5fdc5-105">İlişkisel bir veritabanındaki bir dizin aynı dizin Entity Framework'ün temel kavram eşlenir.</span><span class="sxs-lookup"><span data-stu-id="5fdc5-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="5fdc5-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="5fdc5-106">Conventions</span></span>

<span data-ttu-id="5fdc5-107">Kural gereği, dizinleri adlandırılır `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="5fdc5-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="5fdc5-108">Bileşik dizinler için `<property name>` özellik adlarının bir alt çizgi ayrılmış listesi olur.</span><span class="sxs-lookup"><span data-stu-id="5fdc5-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5fdc5-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="5fdc5-109">Data Annotations</span></span>

<span data-ttu-id="5fdc5-110">Veri ek açıklamalarını kullanma dizinleri yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="5fdc5-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5fdc5-111">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="5fdc5-111">Fluent API</span></span>

<span data-ttu-id="5fdc5-112">Fluent API'si, bir dizinin adını yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fdc5-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="5fdc5-113">Ayrıca, bir filtre belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5fdc5-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="5fdc5-114">Bir 'Is NOT NULL' ve SQL Server sağlayıcıyı EF eklediğinde benzersiz bir dizinin parçası olan tüm null yapılabilir sütunlar için filtreleyin.</span><span class="sxs-lookup"><span data-stu-id="5fdc5-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="5fdc5-115">Bu kuralı belirtebilirsiniz geçersiz kılmak için bir `null` değeri.</span><span class="sxs-lookup"><span data-stu-id="5fdc5-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
