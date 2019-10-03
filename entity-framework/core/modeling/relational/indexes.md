---
title: Dizinler (Ilişkisel veritabanı)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 7bb74d0bfa6090b597eb988a46f00494e25f233e
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813640"
---
# <a name="indexes-relational-database"></a><span data-ttu-id="c2cbd-102">Dizinler (Ilişkisel veritabanı)</span><span class="sxs-lookup"><span data-stu-id="c2cbd-102">Indexes (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="c2cbd-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="c2cbd-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c2cbd-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="c2cbd-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c2cbd-105">İlişkisel veritabanındaki bir dizin, Entity Framework çekirdekdeki bir dizinle aynı kavram ile eşlenir.</span><span class="sxs-lookup"><span data-stu-id="c2cbd-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="c2cbd-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="c2cbd-106">Conventions</span></span>

<span data-ttu-id="c2cbd-107">Kurala göre, dizinler adlandırılır `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="c2cbd-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="c2cbd-108">Bileşik dizinler `<property name>` için özellik adlarının alt çizgiyle ayrılmış bir listesi haline gelir.</span><span class="sxs-lookup"><span data-stu-id="c2cbd-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c2cbd-109">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="c2cbd-109">Data Annotations</span></span>

<span data-ttu-id="c2cbd-110">Dizinler, veri açıklamaları kullanılarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="c2cbd-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c2cbd-111">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="c2cbd-111">Fluent API</span></span>

<span data-ttu-id="c2cbd-112">Bir dizinin adını yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c2cbd-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="c2cbd-113">Ayrıca, bir filtre de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c2cbd-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="c2cbd-114">SQL Server sağlayıcısı EF kullanıldığında, benzersiz bir dizinin parçası olan tüm null yapılabilen sütunlar için bir ' NOT NULL ' filtresi ekler.</span><span class="sxs-lookup"><span data-stu-id="c2cbd-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="c2cbd-115">Bu kuralı geçersiz kılmak için bir `null` değer sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c2cbd-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a><span data-ttu-id="c2cbd-116">SQL Server dizinlerine sütun Ekle</span><span class="sxs-lookup"><span data-stu-id="c2cbd-116">Include Columns in SQL Server Indexes</span></span>

<span data-ttu-id="c2cbd-117">Sorgudaki tüm sütunlar dizine anahtar veya anahtar olmayan sütunlar olarak dahil edildiğinde sorgu performansını önemli ölçüde artırmak için, [dahil edilen sütunlara sahip dizinleri](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c2cbd-117">You can configure [indexes with included columns](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) to significantly improve query performance when all columns in the query are included in the index as key or non-key columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ForSqlServerHasIndex.cs?name=Model)]
