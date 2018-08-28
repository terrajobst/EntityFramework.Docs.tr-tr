---
title: EF Core 1.1 - EF Core yenilikleri
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 9f8f2d46f967c7d8ec4f8ea410e51531dfe3ca7b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995441"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="819f7-102">EF Core 1.1 yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="819f7-102">New features in EF Core 1.1</span></span>

## <a name="modelling"></a><span data-ttu-id="819f7-103">Modelleme</span><span class="sxs-lookup"><span data-stu-id="819f7-103">Modelling</span></span>
### <a name="field-mapping"></a><span data-ttu-id="819f7-104">Alan eşleme</span><span class="sxs-lookup"><span data-stu-id="819f7-104">Field mapping</span></span>
<span data-ttu-id="819f7-105">Bir özellik için destek alanı yapılandırmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="819f7-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="819f7-106">Bu salt okunur özellikler veya Get/Set yöntemleri yerine bir özellik olan veriler için kullanışlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="819f7-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="819f7-107">SQL Server bellek için iyileştirilmiş tablolara eşleme</span><span class="sxs-lookup"><span data-stu-id="819f7-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>
<span data-ttu-id="819f7-108">Bir varlığa eşlenmiş tablosu bellek için iyileştirilmiş olduğunu belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="819f7-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="819f7-109">Ne zaman EF Core oluşturmak ve bir veritabanını korumak için tabanlı kullanarak modelinize göre (geçişleri ile veya `Database.EnsureCreated()`), bu varlıklar için bellek için iyileştirilmiş bir tablo oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="819f7-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="819f7-110">Change tracking</span><span class="sxs-lookup"><span data-stu-id="819f7-110">Change tracking</span></span>
### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="819f7-111">Ek değişiklik EF6'dan API'leri izleme</span><span class="sxs-lookup"><span data-stu-id="819f7-111">Additional change tracking APIs from EF6</span></span>
<span data-ttu-id="819f7-112">Gibi `Reload`, `GetModifiedProperties`, `GetDatabaseValues` vs.</span><span class="sxs-lookup"><span data-stu-id="819f7-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="819f7-113">Sorgu</span><span class="sxs-lookup"><span data-stu-id="819f7-113">Query</span></span>
### <a name="explicit-loading"></a><span data-ttu-id="819f7-114">Açık yükleme</span><span class="sxs-lookup"><span data-stu-id="819f7-114">Explicit Loading</span></span>
<span data-ttu-id="819f7-115">Bir gezinme özelliği veritabanından daha önce yüklenen bir varlıkta popülasyonunu tetiklemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="819f7-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>
### <a name="dbsetfind"></a><span data-ttu-id="819f7-116">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="819f7-116">DbSet.Find</span></span>
<span data-ttu-id="819f7-117">Birincil anahtar değerini alan bir varlık getirmek için kolay bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="819f7-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="819f7-118">Diğer</span><span class="sxs-lookup"><span data-stu-id="819f7-118">Other</span></span>
### <a name="connection-resiliency"></a><span data-ttu-id="819f7-119">Bağlantı dayanıklılığı</span><span class="sxs-lookup"><span data-stu-id="819f7-119">Connection resiliency</span></span>
<span data-ttu-id="819f7-120">Veritabanı komutları otomatik olarak yeniden deneme başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="819f7-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="819f7-121">Bu özellikle yararlı olduğunda SQL Azure, geçici hataları nerede genel bağlantı.</span><span class="sxs-lookup"><span data-stu-id="819f7-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>
### <a name="simplified-service-replacement"></a><span data-ttu-id="819f7-122">Basitleştirilmiş hizmet değiştirme</span><span class="sxs-lookup"><span data-stu-id="819f7-122">Simplified service replacement</span></span>
<span data-ttu-id="819f7-123">EF kullanan iç Hizmetleri değiştirilecek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="819f7-123">Makes it easier to replace internal services that EF uses.</span></span>
