---
title: Sorgular nasıl çalışır-EF Core
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: ba0d68469530e6272ffbb51946d7856122a261c7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417703"
---
# <a name="how-queries-work"></a><span data-ttu-id="a8014-102">Sorgular nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="a8014-102">How Queries Work</span></span>

<span data-ttu-id="a8014-103">Entity Framework Core, veritabanındaki verileri sorgulamak için dil ile tümleşik sorgu (LINQ) kullanır.</span><span class="sxs-lookup"><span data-stu-id="a8014-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="a8014-104">LINQ, türetilmiş bağlama ve C# varlık sınıflarınıza göre kesin tür belirtilmiş sorgular yazmak için (veya .net dilinizi tercih ettiğiniz) kullanmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="a8014-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="a8014-105">Bir sorgunun ömrü</span><span class="sxs-lookup"><span data-stu-id="a8014-105">The life of a query</span></span>

<span data-ttu-id="a8014-106">Aşağıda her bir sorgunun gittiği işleme ilişkin üst düzey bir genel bakış verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a8014-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="a8014-107">LINQ sorgusu, veritabanı sağlayıcısı tarafından işlenmek üzere hazırlanmaya yönelik bir temsili oluşturmak için Entity Framework Core tarafından işlenir</span><span class="sxs-lookup"><span data-stu-id="a8014-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="a8014-108">Sonuç, bu işlemin sorgu her yürütüldüğünde yapılması gerekmemesi için önbelleğe alınır</span><span class="sxs-lookup"><span data-stu-id="a8014-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="a8014-109">Sonuç veritabanı sağlayıcısına geçirilir</span><span class="sxs-lookup"><span data-stu-id="a8014-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="a8014-110">Veritabanı sağlayıcısı, sorgunun hangi bölümlerinin veritabanında değerlendirilebileceğini belirler</span><span class="sxs-lookup"><span data-stu-id="a8014-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="a8014-111">Sorgunun bu bölümleri veritabanına özgü sorgu diline çevrilir (örneğin, bir ilişkisel veritabanı için SQL)</span><span class="sxs-lookup"><span data-stu-id="a8014-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="a8014-112">Veritabanına bir veya daha fazla sorgu gönderiliyor ve döndürülen sonuç kümesi (sonuçlar, varlık örneklerinden değil, veritabanındaki değerlerdir)</span><span class="sxs-lookup"><span data-stu-id="a8014-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="a8014-113">Sonuç kümesindeki her öğe için</span><span class="sxs-lookup"><span data-stu-id="a8014-113">For each item in the result set</span></span>
   1. <span data-ttu-id="a8014-114">Bu bir izleme sorgiyiyiyise EF, verilerin bağlam örneği için değişiklik izleyicide zaten bir varlığı temsil ettiğini denetler</span><span class="sxs-lookup"><span data-stu-id="a8014-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="a8014-115">Öyleyse, var olan varlık döndürülür</span><span class="sxs-lookup"><span data-stu-id="a8014-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="a8014-116">Aksi takdirde, yeni bir varlık oluşturulur, değişiklik izleme kurulum olur ve yeni varlık döndürülür</span><span class="sxs-lookup"><span data-stu-id="a8014-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="a8014-117">Bu bir izleme sorgusu değilse EF, verilerin bu sorgu için sonuç kümesinde zaten olan bir varlığı temsil ettiğini denetler</span><span class="sxs-lookup"><span data-stu-id="a8014-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="a8014-118">Varsa, var olan varlık döndürülür <sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="a8014-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="a8014-119">Aksi takdirde, yeni bir varlık oluşturulur ve döndürülür</span><span class="sxs-lookup"><span data-stu-id="a8014-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="a8014-120"><sup>(1)</sup> hiçbir izleme sorgusu, zaten döndürülen varlıkların izlenmesini sağlamak için zayıf başvurular kullanır.</span><span class="sxs-lookup"><span data-stu-id="a8014-120"><sup>(1)</sup> No-tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="a8014-121">Aynı kimliğe sahip önceki bir sonuç kapsam dışına gittiğinde ve çöp toplama işlemi çalıştırıyorsa, yeni bir varlık örneği alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a8014-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="a8014-122">Sorgular yürütüldüğünde</span><span class="sxs-lookup"><span data-stu-id="a8014-122">When queries are executed</span></span>

<span data-ttu-id="a8014-123">LINQ işleçlerini çağırdığınızda sorgunun bellek içi gösterimini oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="a8014-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="a8014-124">Sorgu yalnızca sonuçlar tüketiliyorsa veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="a8014-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="a8014-125">Veritabanına gönderilen sorgunun sonucu olan en yaygın işlemler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a8014-125">The most common operations that result in the query being sent to the database are:</span></span>

* <span data-ttu-id="a8014-126">Sonuçları bir `for` döngüsünde yineleme</span><span class="sxs-lookup"><span data-stu-id="a8014-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="a8014-127">`ToList`, `ToArray`, `Single`, `Count` gibi bir işleç kullanma</span><span class="sxs-lookup"><span data-stu-id="a8014-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="a8014-128">Sorgunun sonuçlarını Kullanıcı arabirimine bağlama</span><span class="sxs-lookup"><span data-stu-id="a8014-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="a8014-129">**Kullanıcı girişini her zaman doğrula:** EF Core, sorgularda parametreleri ve kaçış sabit değerlerini kullanarak SQL ekleme saldırılarına karşı koruma sağlar, girişleri doğrulamaz.</span><span class="sxs-lookup"><span data-stu-id="a8014-129">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="a8014-130">Uygulamanın gereksinimlerine göre uygun doğrulama, güvenilmeyen kaynaklardan gelen değerler LINQ sorgularında kullanılmadan, varlık özelliklerine atanmadan veya diğer EF Core API 'Lerine geçirilmeden önce gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="a8014-130">Appropriate validation, per the application's requirements, should be performed before values from untrusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="a8014-131">Bu, sorguları dinamik olarak oluşturmak için kullanılan herhangi bir kullanıcı girişini içerir.</span><span class="sxs-lookup"><span data-stu-id="a8014-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="a8014-132">LINQ kullanırken bile, derleme ifadelerine Kullanıcı girişini kabul ediyorsanız, yalnızca amaçlanan ifadelerin oluşturulabilse emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a8014-132">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
