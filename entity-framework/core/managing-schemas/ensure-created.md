---
title: API 'Leri oluşturma ve bırakma-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
uid: core/managing-schemas/ensure-created
ms.openlocfilehash: 32ac6cd043df73cd041780ec4c8805675adc5ab1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416872"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="acd6c-102">API Oluşturma ve Bırakma</span><span class="sxs-lookup"><span data-stu-id="acd6c-102">Create and Drop APIs</span></span>

<span data-ttu-id="acd6c-103">Ensuyeniden oluşturma ve EnsureDeleted yöntemleri, veritabanı şemasını yönetmeye yönelik [geçişlere](migrations/index.md) hafif bir alternatif sağlar.</span><span class="sxs-lookup"><span data-stu-id="acd6c-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="acd6c-104">Bu yöntemler, verilerin geçici olduğu ve şema değiştiğinde bırakılan senaryolarda faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="acd6c-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="acd6c-105">Örneğin, prototipleme sırasında, testlerde veya yerel önbellekler için.</span><span class="sxs-lookup"><span data-stu-id="acd6c-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="acd6c-106">Bazı sağlayıcılar (özellikle ilişkisel olmayan) geçişleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="acd6c-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="acd6c-107">Bu sağlayıcılar için, veritabanı şemasını başlatmanın en kolay yolu genellikle yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="acd6c-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="acd6c-108">Yeniden oluşturulup geçişler birlikte iyi çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="acd6c-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="acd6c-109">Geçişleri kullanıyorsanız şemayı başlatmak için yeniden oluşturulması kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="acd6c-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="acd6c-110">Geçişlere geçiş aşamasından geçiş, sorunsuz bir deneyim değildir.</span><span class="sxs-lookup"><span data-stu-id="acd6c-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="acd6c-111">Bunu yapmanın en kolay yolu, veritabanını bırakıp geçişleri kullanarak yeniden oluşturmayı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="acd6c-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="acd6c-112">Daha sonra geçişlerde geçiş yapmayı düşünüyorsanız, en iyi şekilde, yeniden oluşturulması yerine geçişle başlamak yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="acd6c-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="acd6c-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="acd6c-113">EnsureDeleted</span></span>

<span data-ttu-id="acd6c-114">EnsureDeleted yöntemi, varsa veritabanını de bırakacak.</span><span class="sxs-lookup"><span data-stu-id="acd6c-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="acd6c-115">Uygun izinleriniz yoksa bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="acd6c-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="acd6c-116">Yeniden oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="acd6c-116">EnsureCreated</span></span>

<span data-ttu-id="acd6c-117">Yeniden oluşturma, mevcut değilse veritabanını oluşturur ve veritabanı şemasını başlatabilir.</span><span class="sxs-lookup"><span data-stu-id="acd6c-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="acd6c-118">Herhangi bir tablo varsa (başka bir DbContext sınıfı için tablolar dahil), şema başlatılmaz.</span><span class="sxs-lookup"><span data-stu-id="acd6c-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="acd6c-119">Bu yöntemlerin zaman uyumsuz sürümleri de mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="acd6c-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="acd6c-120">SQL betiği</span><span class="sxs-lookup"><span data-stu-id="acd6c-120">SQL Script</span></span>

<span data-ttu-id="acd6c-121">Yeniden oluştururken kullanılan SQL 'i almak için GenerateCreateScript yöntemini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="acd6c-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="acd6c-122">Birden çok DbContext sınıfı</span><span class="sxs-lookup"><span data-stu-id="acd6c-122">Multiple DbContext classes</span></span>

<span data-ttu-id="acd6c-123">Ensuyeniden oluşturulması yalnızca veritabanında hiçbir tablo yoksa işe yarar.</span><span class="sxs-lookup"><span data-stu-id="acd6c-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="acd6c-124">Gerekirse, şemanın başlatılmış olması gerekip gerekmediğini görmek için kendi kontrol etmeniz yazabilir ve şemayı başlatmak için temeldeki ırelationaldatabasecreator hizmetini kullanın.</span><span class="sxs-lookup"><span data-stu-id="acd6c-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
