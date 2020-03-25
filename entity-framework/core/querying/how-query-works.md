---
title: Sorgular nasıl çalışır-EF Core
author: ajcvickers
ms.date: 03/17/2020
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: e8a50efe31468ea8df211602636dd474550bc0ef
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136230"
---
# <a name="how-queries-work"></a><span data-ttu-id="3cfe7-102">Sorgular nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="3cfe7-102">How Queries Work</span></span>

<span data-ttu-id="3cfe7-103">Entity Framework Core, veritabanındaki verileri sorgulamak için dil ile tümleşik sorgu (LINQ) kullanır.</span><span class="sxs-lookup"><span data-stu-id="3cfe7-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="3cfe7-104">LINQ, türetilmiş bağlama ve C# varlık sınıflarınıza göre kesin tür belirtilmiş sorgular yazmak için (veya .net dilinizi tercih ettiğiniz) kullanmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="3cfe7-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="3cfe7-105">Bir sorgunun ömrü</span><span class="sxs-lookup"><span data-stu-id="3cfe7-105">The life of a query</span></span>

<span data-ttu-id="3cfe7-106">Aşağıda her bir sorgunun gittiği işleme ilişkin üst düzey bir genel bakış verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3cfe7-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="3cfe7-107">LINQ sorgusu, veritabanı sağlayıcısı tarafından işlenmek üzere hazırlanmaya yönelik bir temsili oluşturmak için Entity Framework Core tarafından işlenir</span><span class="sxs-lookup"><span data-stu-id="3cfe7-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="3cfe7-108">Sonuç, bu işlemin sorgu her yürütüldüğünde yapılması gerekmemesi için önbelleğe alınır</span><span class="sxs-lookup"><span data-stu-id="3cfe7-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="3cfe7-109">Sonuç veritabanı sağlayıcısına geçirilir</span><span class="sxs-lookup"><span data-stu-id="3cfe7-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="3cfe7-110">Veritabanı sağlayıcısı, sorgunun hangi bölümlerinin veritabanında değerlendirilebileceğini belirler</span><span class="sxs-lookup"><span data-stu-id="3cfe7-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="3cfe7-111">Sorgunun bu bölümleri veritabanına özgü sorgu diline çevrilir (örneğin, bir ilişkisel veritabanı için SQL)</span><span class="sxs-lookup"><span data-stu-id="3cfe7-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="3cfe7-112">Veritabanına bir sorgu gönderilir ve döndürülen sonuç kümesi (sonuçlar, varlık örneklerinden değil, veritabanındaki değerlerdir)</span><span class="sxs-lookup"><span data-stu-id="3cfe7-112">A query is sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="3cfe7-113">Sonuç kümesindeki her öğe için</span><span class="sxs-lookup"><span data-stu-id="3cfe7-113">For each item in the result set</span></span>
   1. <span data-ttu-id="3cfe7-114">Bu bir izleme sorgiyiyiyise EF, verilerin bağlam örneği için değişiklik izleyicide zaten bir varlığı temsil ettiğini denetler</span><span class="sxs-lookup"><span data-stu-id="3cfe7-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="3cfe7-115">Öyleyse, var olan varlık döndürülür</span><span class="sxs-lookup"><span data-stu-id="3cfe7-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="3cfe7-116">Aksi takdirde, yeni bir varlık oluşturulur, değişiklik izleme kurulum olur ve yeni varlık döndürülür</span><span class="sxs-lookup"><span data-stu-id="3cfe7-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="3cfe7-117">Bu bir izleme sorgusu değilse, her zaman yeni bir varlık oluşturulur ve döndürülür</span><span class="sxs-lookup"><span data-stu-id="3cfe7-117">If this is a no-tracking query, then a new entity is always created and returned</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="3cfe7-118">Sorgular yürütüldüğünde</span><span class="sxs-lookup"><span data-stu-id="3cfe7-118">When queries are executed</span></span>

<span data-ttu-id="3cfe7-119">LINQ işleçlerini çağırdığınızda sorgunun bellek içi gösterimini oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="3cfe7-119">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="3cfe7-120">Sorgu yalnızca sonuçlar tüketiliyorsa veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="3cfe7-120">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="3cfe7-121">Veritabanına gönderilen sorgunun sonucu olan en yaygın işlemler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="3cfe7-121">The most common operations that result in the query being sent to the database are:</span></span>

* <span data-ttu-id="3cfe7-122">Sonuçları bir `for` döngüsünde yineleme</span><span class="sxs-lookup"><span data-stu-id="3cfe7-122">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="3cfe7-123">`ToList`, `ToArray`, `Single`, `Count` veya eşdeğer zaman uyumsuz aşırı yüklemeler gibi bir işleç kullanma</span><span class="sxs-lookup"><span data-stu-id="3cfe7-123">Using an operator such as `ToList`, `ToArray`, `Single`, `Count` or the equivalent async overloads</span></span>

> [!WARNING]  
> <span data-ttu-id="3cfe7-124">**Kullanıcı girişini her zaman doğrula:** EF Core, sorgularda parametreleri ve kaçış sabit değerlerini kullanarak SQL ekleme saldırılarına karşı koruma sağlar, girişleri doğrulamaz.</span><span class="sxs-lookup"><span data-stu-id="3cfe7-124">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="3cfe7-125">Uygulamanın gereksinimlerine göre uygun doğrulama, güvenilir olmayan kaynaklardan gelen değerler LINQ sorgularında kullanılmadan, varlık özelliklerine atanmadan veya diğer EF Core API 'Lerine geçirilmeden önce gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="3cfe7-125">Appropriate validation, per the application's requirements, should be performed before values from un-trusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="3cfe7-126">Bu, sorguları dinamik olarak oluşturmak için kullanılan herhangi bir kullanıcı girişini içerir.</span><span class="sxs-lookup"><span data-stu-id="3cfe7-126">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="3cfe7-127">LINQ kullanırken bile, derleme ifadelerine Kullanıcı girişini kabul ediyorsanız, yalnızca amaçlanan ifadelerin oluşturulabilse emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3cfe7-127">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
