---
title: EF Core 1.1'deki yenilikler - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417519"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="8f7f2-102">EF Core 1.1'deki yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="8f7f2-102">New features in EF Core 1.1</span></span>

## <a name="modeling"></a><span data-ttu-id="8f7f2-103">Modelleme</span><span class="sxs-lookup"><span data-stu-id="8f7f2-103">Modeling</span></span>

### <a name="field-mapping"></a><span data-ttu-id="8f7f2-104">Alan eşlemesi</span><span class="sxs-lookup"><span data-stu-id="8f7f2-104">Field mapping</span></span>

<span data-ttu-id="8f7f2-105">Bir özellik için bir destek alanını yapılandırmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="8f7f2-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="8f7f2-106">Bu, salt okunur özellikler veya özellik yerine Get/Set yöntemlerine sahip veriler için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="8f7f2-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="8f7f2-107">SQL Server'da Bellek En İyi Duruma Getirilmiş Tablolara Eşleme</span><span class="sxs-lookup"><span data-stu-id="8f7f2-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>

<span data-ttu-id="8f7f2-108">Bir varlığın eşlenen tablosunun bellek için en iyi duruma getirilmiş olduğunu belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8f7f2-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="8f7f2-109">Modelinize dayalı bir veritabanı oluşturmak ve korumak için EF `Database.EnsureCreated()`Core'u kullanırken (geçişlerle veya geçişlerle), bu varlıklar için bellek için en iyi duruma getirilmiş bir tablo oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8f7f2-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="8f7f2-110">Değişiklik izleme</span><span class="sxs-lookup"><span data-stu-id="8f7f2-110">Change tracking</span></span>

### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="8f7f2-111">EF6'dan ek değişiklik izleme API'leri</span><span class="sxs-lookup"><span data-stu-id="8f7f2-111">Additional change tracking APIs from EF6</span></span>

<span data-ttu-id="8f7f2-112">, `Reload`, `GetModifiedProperties` `GetDatabaseValues` vb. gibi.</span><span class="sxs-lookup"><span data-stu-id="8f7f2-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="8f7f2-113">Sorgu</span><span class="sxs-lookup"><span data-stu-id="8f7f2-113">Query</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="8f7f2-114">Açık Yükleme</span><span class="sxs-lookup"><span data-stu-id="8f7f2-114">Explicit Loading</span></span>

<span data-ttu-id="8f7f2-115">Daha önce veritabanından yüklenen bir varlık üzerinde bir gezinti özelliğinin popülasyonunu tetiklemenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="8f7f2-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>

### <a name="dbsetfind"></a><span data-ttu-id="8f7f2-116">DbSet.Bul</span><span class="sxs-lookup"><span data-stu-id="8f7f2-116">DbSet.Find</span></span>

<span data-ttu-id="8f7f2-117">Bir varlığı birincil anahtar değerine göre getirmenin kolay bir yolunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="8f7f2-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="8f7f2-118">Diğer</span><span class="sxs-lookup"><span data-stu-id="8f7f2-118">Other</span></span>

### <a name="connection-resiliency"></a><span data-ttu-id="8f7f2-119">Bağlantı dayanıklılığı</span><span class="sxs-lookup"><span data-stu-id="8f7f2-119">Connection resiliency</span></span>

<span data-ttu-id="8f7f2-120">Başarısız veritabanı komutlarını otomatik olarak yeniden dener.</span><span class="sxs-lookup"><span data-stu-id="8f7f2-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="8f7f2-121">Bu, geçici hataların yaygın olduğu SQL Azure'a bağlantı verirken özellikle kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="8f7f2-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>

### <a name="simplified-service-replacement"></a><span data-ttu-id="8f7f2-122">Basitleştirilmiş hizmet değiştirme</span><span class="sxs-lookup"><span data-stu-id="8f7f2-122">Simplified service replacement</span></span>

<span data-ttu-id="8f7f2-123">EF'nin kullandığı dahili hizmetlerin değiştirilmesini kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="8f7f2-123">Makes it easier to replace internal services that EF uses.</span></span>
