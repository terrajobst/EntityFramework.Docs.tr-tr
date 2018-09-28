---
title: Nasıl iş - EF Core sorgular
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/overview
ms.openlocfilehash: 23d26f9c0ac17fc0df744f5339946947ea366911
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415737"
---
# <a name="how-queries-work"></a><span data-ttu-id="aa821-102">Sorguları nasıl çalışır</span><span class="sxs-lookup"><span data-stu-id="aa821-102">How Queries Work</span></span>

<span data-ttu-id="aa821-103">Entity Framework Core dil tümleşik sorgu (LINQ) veritabanından verileri sorgulamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="aa821-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="aa821-104">LINQ C# (veya tercih ettiğiniz .NET dilinizi) türetilen bağlamını ve varlık sınıflarında temel alan türü kesin belirlenmiş sorgular yazmak için kullanmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="aa821-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="aa821-105">Bir sorgu ömrü</span><span class="sxs-lookup"><span data-stu-id="aa821-105">The life of a query</span></span>

<span data-ttu-id="aa821-106">Her sorgu geçtiği işlemine yüksek düzeyli bir genel bakış aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="aa821-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="aa821-107">LINQ Sorgu sağlayıcısı tarafından işlenmek üzere hazır olan bir gösterim oluşturmak için Entity Framework Core tarafından işlenen</span><span class="sxs-lookup"><span data-stu-id="aa821-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="aa821-108">Sonuç önbelleğe alınan bu işleme, sorgunun her çalıştırılışında yapılması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="aa821-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="aa821-109">Sonuç veritabanı sağlayıcıya geçirilir.</span><span class="sxs-lookup"><span data-stu-id="aa821-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="aa821-110">Veritabanı sağlayıcısı veritabanında sorgu hangi parçalarının değerlendirilebilir tanımlar.</span><span class="sxs-lookup"><span data-stu-id="aa821-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="aa821-111">Bu sorgunun bölümlerini veritabanı belirli bir sorgu dili (örneğin, ilişkisel bir veritabanı için SQL) için bunların çevirisi</span><span class="sxs-lookup"><span data-stu-id="aa821-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="aa821-112">Bir veya daha fazla sorgu için veritabanı ve sonuç kümesini döndürülen gönderilir (sonucu olan varlık örneklerini veritabanından alınan değerlerin)</span><span class="sxs-lookup"><span data-stu-id="aa821-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="aa821-113">Her öğe için sonuç kümesinde</span><span class="sxs-lookup"><span data-stu-id="aa821-113">For each item in the result set</span></span>
   1. <span data-ttu-id="aa821-114">Bir izleme sorguya varsa EF veri değişikliği İzleyicisi bağlam örneği için zaten bir varlığı temsil edip etmediğini denetler</span><span class="sxs-lookup"><span data-stu-id="aa821-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="aa821-115">Bu durumda, var olan bir varlığa döndürülür</span><span class="sxs-lookup"><span data-stu-id="aa821-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="aa821-116">Aksi halde yeni bir varlık oluşturulur, değişiklik izleme kurulduğundan ve yeni bir varlık döndürdü</span><span class="sxs-lookup"><span data-stu-id="aa821-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="aa821-117">Hayır-izleme sorgu varsa EF veriyi bir varlık zaten bu sorgu için sonuç temsil edip etmediğini denetler</span><span class="sxs-lookup"><span data-stu-id="aa821-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="aa821-118">Bu nedenle, var olan bir varlığa döndürülürse <sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="aa821-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="aa821-119">Aksi halde yeni bir varlık oluşturulur ve döndürülen</span><span class="sxs-lookup"><span data-stu-id="aa821-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="aa821-120"><sup>(1) </sup> Zaten döndürülen varlıkları izlemek için zayıf başvurular hiçbir izleme sorguları kullanın.</span><span class="sxs-lookup"><span data-stu-id="aa821-120"><sup>(1)</sup> No tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="aa821-121">Aynı kimliğe sahip bir önceki sonucu kapsam dışına gider ve atık toplama devam, yeni bir varlık örneği alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="aa821-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="aa821-122">Sorguları ne zaman çalıştırılır</span><span class="sxs-lookup"><span data-stu-id="aa821-122">When queries are executed</span></span>

<span data-ttu-id="aa821-123">LINQ işleçleri çağırdığınızda, bir sorgunun bellek içi temsili yalnızca oluşturuyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="aa821-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="aa821-124">Sonuçları tüketilen olduğunda sorguyu yalnızca veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="aa821-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="aa821-125">Veritabanına gönderilen sorgu sonucunda en yaygın işlemler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="aa821-125">The most common operations that result in the query being sent to the database are:</span></span>
* <span data-ttu-id="aa821-126">Yineleme sonuçları bir `for` döngü</span><span class="sxs-lookup"><span data-stu-id="aa821-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="aa821-127">Bir işleç gibi kullanarak `ToList`, `ToArray`, `Single`, `Count`</span><span class="sxs-lookup"><span data-stu-id="aa821-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="aa821-128">Veri bağlama için bir kullanıcı Arabirimi bir sorgunun sonuçlarını</span><span class="sxs-lookup"><span data-stu-id="aa821-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="aa821-129">**Her zaman kullanıcı girişini doğrulama:** sırada EF Core parametreleri kullanarak SQL ekleme saldırılarına karşı korur ve sorgularda değişmez değerleri kaçış, girişleri doğrulamaz.</span><span class="sxs-lookup"><span data-stu-id="aa821-129">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="aa821-130">Güvenilir olmayan kaynaklardan gelen değerleri LINQ sorgularında kullanılan, varlık özelliklerine atanan veya geçirilen diğer EF Core API'leri için önce başına uygulama gereksinimlerine uygun doğrulama gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="aa821-130">Appropriate validation, per the application's requirements, should be performed before values from untrusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="aa821-131">Bu sorguları dinamik olarak oluşturmak için kullanılan herhangi bir kullanıcı girişi içerir.</span><span class="sxs-lookup"><span data-stu-id="aa821-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="aa821-132">İfadeleri oluşturmak için kullanıcı girişi kabul ediyorsanız, LINQ kullanırken bile, yalnızca hedeflenen ifadeleri oluşturulabilir emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="aa821-132">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
