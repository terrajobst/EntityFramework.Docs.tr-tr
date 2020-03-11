---
title: EF Core 1,1 ' deki yenilikler-EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417519"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="786dd-102">EF Core 1,1 ' deki yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="786dd-102">New features in EF Core 1.1</span></span>

## <a name="modeling"></a><span data-ttu-id="786dd-103">Modelleme</span><span class="sxs-lookup"><span data-stu-id="786dd-103">Modeling</span></span>

### <a name="field-mapping"></a><span data-ttu-id="786dd-104">Alan eşleme</span><span class="sxs-lookup"><span data-stu-id="786dd-104">Field mapping</span></span>

<span data-ttu-id="786dd-105">Bir özellik için bir destek alanı yapılandırmanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="786dd-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="786dd-106">Bu, salt okuma özellikleri veya bir özellik yerine get/set yöntemlerine sahip veriler için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="786dd-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="786dd-107">SQL Server bellek için Iyileştirilmiş tablolarla eşleme</span><span class="sxs-lookup"><span data-stu-id="786dd-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>

<span data-ttu-id="786dd-108">Bir varlığın eşlendiği tablonun bellek için iyileştirilmiş olduğunu belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="786dd-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="786dd-109">Modelinize dayalı bir veritabanı oluşturmak ve korumak için EF Core kullanılırken (geçişlerle veya `Database.EnsureCreated()`), bu varlıklar için bellek için iyileştirilmiş bir tablo oluşturulacaktır.</span><span class="sxs-lookup"><span data-stu-id="786dd-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="786dd-110">Değişiklik izleme</span><span class="sxs-lookup"><span data-stu-id="786dd-110">Change tracking</span></span>

### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="786dd-111">EF6 'ten ek değişiklik izleme API 'Leri</span><span class="sxs-lookup"><span data-stu-id="786dd-111">Additional change tracking APIs from EF6</span></span>

<span data-ttu-id="786dd-112">`Reload`, `GetModifiedProperties`, `GetDatabaseValues` vb.</span><span class="sxs-lookup"><span data-stu-id="786dd-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="786dd-113">Sorgu</span><span class="sxs-lookup"><span data-stu-id="786dd-113">Query</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="786dd-114">Açık yükleme</span><span class="sxs-lookup"><span data-stu-id="786dd-114">Explicit Loading</span></span>

<span data-ttu-id="786dd-115">Daha önce veritabanından yüklenmiş bir varlık üzerinde bir gezinti özelliği popülasyonu tetiklemeniz sağlar.</span><span class="sxs-lookup"><span data-stu-id="786dd-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>

### <a name="dbsetfind"></a><span data-ttu-id="786dd-116">DbSet. Find</span><span class="sxs-lookup"><span data-stu-id="786dd-116">DbSet.Find</span></span>

<span data-ttu-id="786dd-117">, Birincil anahtar değerine göre bir varlığı getirmek için kolay bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="786dd-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="786dd-118">Diğer</span><span class="sxs-lookup"><span data-stu-id="786dd-118">Other</span></span>

### <a name="connection-resiliency"></a><span data-ttu-id="786dd-119">Bağlantı dayanıklılığı</span><span class="sxs-lookup"><span data-stu-id="786dd-119">Connection resiliency</span></span>

<span data-ttu-id="786dd-120">Başarısız veritabanı komutlarını otomatik olarak yeniden dener.</span><span class="sxs-lookup"><span data-stu-id="786dd-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="786dd-121">Bu, özellikle, geçici hataların yaygın olduğu SQL Azure bağlantı olduğunda faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="786dd-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>

### <a name="simplified-service-replacement"></a><span data-ttu-id="786dd-122">Basitleştirilmiş hizmet değişimi</span><span class="sxs-lookup"><span data-stu-id="786dd-122">Simplified service replacement</span></span>

<span data-ttu-id="786dd-123">EF 'in kullandığı iç Hizmetleri değiştirmeyi kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="786dd-123">Makes it easier to replace internal services that EF uses.</span></span>
