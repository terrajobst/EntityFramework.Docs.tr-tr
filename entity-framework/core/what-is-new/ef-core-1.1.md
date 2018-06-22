---
title: EF çekirdek 1.1 - EF çekirdek yenilikler nelerdir?
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 74c1033cab2704bdbb9fa4d3ce111df1f1c29418
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054288"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="29543-102">EF çekirdek 1.1 yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="29543-102">New features in EF Core 1.1</span></span>

## <a name="modelling"></a><span data-ttu-id="29543-103">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="29543-103">Modelling</span></span>
### <a name="field-mapping"></a><span data-ttu-id="29543-104">Alan eşleme</span><span class="sxs-lookup"><span data-stu-id="29543-104">Field mapping</span></span>
<span data-ttu-id="29543-105">Bir özellik için bir yedekleme alanını yapılandırmanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="29543-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="29543-106">Bu salt okunur özellikler veya bir özellik yerine Get/Set yöntemleri sahip veriler için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="29543-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="29543-107">Bellek için iyileştirilmiş tablolar SQL Server ile eşleme</span><span class="sxs-lookup"><span data-stu-id="29543-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>
<span data-ttu-id="29543-108">Bir varlık eşlenmiş tablosu bellek için iyileştirilmiş olduğunu belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="29543-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="29543-109">Ne zaman EF çekirdek bir veritabanını oluşturmak ve korumak için tabanlı kullanarak modelinize göre (geçişler biriyle veya `Database.EnsureCreated()`), bir bellek için iyileştirilmiş tablo bu varlıklar için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="29543-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="29543-110">Change tracking</span><span class="sxs-lookup"><span data-stu-id="29543-110">Change tracking</span></span>
### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="29543-111">Ek değişiklik EF6 API izleme</span><span class="sxs-lookup"><span data-stu-id="29543-111">Additional change tracking APIs from EF6</span></span>
<span data-ttu-id="29543-112">Gibi `Reload`, `GetModifiedProperties`, `GetDatabaseValues` vs.</span><span class="sxs-lookup"><span data-stu-id="29543-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="29543-113">Sorgu</span><span class="sxs-lookup"><span data-stu-id="29543-113">Query</span></span>
### <a name="explicit-loading"></a><span data-ttu-id="29543-114">Açık yükleniyor</span><span class="sxs-lookup"><span data-stu-id="29543-114">Explicit Loading</span></span>
<span data-ttu-id="29543-115">Veritabanından daha önce yüklenen bir varlık bir gezinti özelliği popülasyonunu tetiklemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="29543-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>
### <a name="dbsetfind"></a><span data-ttu-id="29543-116">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="29543-116">DbSet.Find</span></span>
<span data-ttu-id="29543-117">Birincil anahtar değerine göre varlık almak için kolay bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="29543-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="29543-118">Diğer</span><span class="sxs-lookup"><span data-stu-id="29543-118">Other</span></span>
### <a name="connection-resiliency"></a><span data-ttu-id="29543-119">Bağlantı dayanıklılığı</span><span class="sxs-lookup"><span data-stu-id="29543-119">Connection resiliency</span></span>
<span data-ttu-id="29543-120">Veritabanı komutları otomatik olarak yeniden denemeler başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="29543-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="29543-121">Bu özellikle yararlı olduğunda SQL Azure, geçici hataları nerede ortak bağlantı.</span><span class="sxs-lookup"><span data-stu-id="29543-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>
### <a name="simplified-service-replacement"></a><span data-ttu-id="29543-122">Basitleştirilmiş hizmet değiştirme</span><span class="sxs-lookup"><span data-stu-id="29543-122">Simplified service replacement</span></span>
<span data-ttu-id="29543-123">EF kullanan iç Hizmetleri değiştirmek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="29543-123">Makes it easier to replace internal services that EF uses.</span></span>
