---
title: API - EF Core oluşturma ve bırakma
author: bricelam
ms.author: bricelam
ms.date: 11/10/2017
ms.openlocfilehash: 336f6fd655603a2474a58dfef377e121d9b04c3a
ms.sourcegitcommit: a088421ecac4f5dc5213208170490181ae2f5f0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285645"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="af312-102">API oluşturma ve bırakma</span><span class="sxs-lookup"><span data-stu-id="af312-102">Create and Drop APIs</span></span>

<span data-ttu-id="af312-103">Basit bir alternatif EnsureCreated ve EnsureDeleted yöntemleri sağlaması [geçişler](migrations/index.md) veritabanı şemasını yönetme.</span><span class="sxs-lookup"><span data-stu-id="af312-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="af312-104">Veriler geçici ve şema değiştiğinde bırakılan bu senaryolarda yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="af312-104">This is useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="af312-105">Örneğin prototip testleri veya yerel sırasında.</span><span class="sxs-lookup"><span data-stu-id="af312-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="af312-106">Bazı sağlayıcıları (özellikle, ilişkisel olmayan olanlar) geçişleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="af312-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="af312-107">Bunlar için EnsureCreated genellikle veritabanı şemasına başlatmak için en kolay yoludur.</span><span class="sxs-lookup"><span data-stu-id="af312-107">For these, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="af312-108">EnsureCreated ve geçişleri birlikte düzgün çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="af312-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="af312-109">Geçişleri kullanıyorsanız EnsureCreated şema başlatmak için kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="af312-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="af312-110">Geçişleri EnsureCreated geçiş, sorunsuz bir deneyim değil.</span><span class="sxs-lookup"><span data-stu-id="af312-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="af312-111">Bunu başarmanın simpelest veritabanını bırakın ve geçişleri kullanarak yeniden oluşturmak için yoludur.</span><span class="sxs-lookup"><span data-stu-id="af312-111">The simpelest way to achieve this is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="af312-112">Geçişleri gelecekte kullanarak öngörüyorsanız, EnsureCreated kullanmak yerine Migrations ile hemen başlatmak idealdir.</span><span class="sxs-lookup"><span data-stu-id="af312-112">If you anticipate using Migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="af312-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="af312-113">EnsureDeleted</span></span>

<span data-ttu-id="af312-114">Varsa EnsureDeleted yöntemi veritabanı kaldıracağız.</span><span class="sxs-lookup"><span data-stu-id="af312-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="af312-115">Uygun izinleriniz yoksa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="af312-115">If you don't have the appropiate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="af312-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="af312-116">EnsureCreated</span></span>

<span data-ttu-id="af312-117">Mevcut değil ve veritabanı şeması başlatmak EnsureCreated veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="af312-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="af312-118">Herhangi bir tablo olması durumunda (başka bir DbContext sınıfı için tablolar dahil), şema başlatılması olmaz.</span><span class="sxs-lookup"><span data-stu-id="af312-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="af312-119">Bu yöntemlerin zaman uyumsuz sürümleri de mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="af312-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="af312-120">SQL betiği</span><span class="sxs-lookup"><span data-stu-id="af312-120">SQL Script</span></span>

<span data-ttu-id="af312-121">EnsureCreated tarafından kullanılan SQL almak için GenerateCreateScript yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="af312-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="af312-122">Birden çok DbContext sınıfı</span><span class="sxs-lookup"><span data-stu-id="af312-122">Multiple DbContext classes</span></span>

<span data-ttu-id="af312-123">Tablo veritabanında mevcut olduğunda EnsureCreated yalnızca çalışır.</span><span class="sxs-lookup"><span data-stu-id="af312-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="af312-124">Gerekirse, şema başlatılması gerekip gerekmediğini görmek için kendi denetiminizi yazabilir ve temel alınan IRelationalDatabaseCreator hizmet Şeması'nı başlatmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="af312-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
