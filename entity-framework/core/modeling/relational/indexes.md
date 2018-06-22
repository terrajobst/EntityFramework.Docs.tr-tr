---
title: Dizinler - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29679005"
---
# <a name="indexes"></a><span data-ttu-id="a7025-102">Dizinler</span><span class="sxs-lookup"><span data-stu-id="a7025-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="a7025-103">Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a7025-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="a7025-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="a7025-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="a7025-105">İlişkisel bir veritabanındaki bir dizin aynı kavram çekirdek Entity Framework'ün bir dizin olarak eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="a7025-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="a7025-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="a7025-106">Conventions</span></span>

<span data-ttu-id="a7025-107">Kurala göre dizinleri adlı `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="a7025-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="a7025-108">Bileşik dizinler için `<property name>` özellik adları alt çizgi ayrılmış listesini olur.</span><span class="sxs-lookup"><span data-stu-id="a7025-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a7025-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="a7025-109">Data Annotations</span></span>

<span data-ttu-id="a7025-110">Dizinleri veri ek açıklamaları kullanılarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="a7025-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a7025-111">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="a7025-111">Fluent API</span></span>

<span data-ttu-id="a7025-112">Bir dizinin adını yapılandırmak için Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7025-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="a7025-113">Ayrıca, bir filtre belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7025-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="a7025-114">SQL Server sağlayıcısı EF kullanarak eklediğinde bir 'Is NOT NULL' için benzersiz bir dizinin parçası olan tüm boş değer atanabilir sütunları filtreleyin.</span><span class="sxs-lookup"><span data-stu-id="a7025-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="a7025-115">Bu kural sağladığınız geçersiz kılmak için bir `null` değeri.</span><span class="sxs-lookup"><span data-stu-id="a7025-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
