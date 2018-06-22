---
title: İş - EF çekirdek nasıl sorgular
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054258"
---
# <a name="how-queries-work"></a><span data-ttu-id="37941-102">Sorguları nasıl çalışır</span><span class="sxs-lookup"><span data-stu-id="37941-102">How Queries Work</span></span>

<span data-ttu-id="37941-103">Entity Framework Çekirdek dil tümleştirmek sorgu (LINQ) veritabanından verileri sorgulamak kullanır.</span><span class="sxs-lookup"><span data-stu-id="37941-103">Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="37941-104">LINQ, C# (veya .NET dilinizi tercih) türetilen bağlamını ve varlık sınıfları esas alarak kesin türü belirtilmiş sorguları yazmaya kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="37941-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="37941-105">Bir sorgu ömrü</span><span class="sxs-lookup"><span data-stu-id="37941-105">The life of a query</span></span>

<span data-ttu-id="37941-106">Her sorgu geçtiği işlemi üst düzey bir genel bakış verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="37941-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="37941-107">LINQ Sorgu sağlayıcısı tarafından işlenmek üzere hazır bir temsilini oluşturmak için Entity Framework Çekirdek tarafından işlenir</span><span class="sxs-lookup"><span data-stu-id="37941-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="37941-108">Sonuç önbelleğe alınan bu işlem, sorgu her çalıştırılışında yapılması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="37941-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="37941-109">Sonuç veritabanı sağlayıcıya geçirilir</span><span class="sxs-lookup"><span data-stu-id="37941-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="37941-110">Veritabanı sağlayıcısı sorgu hangi kısımlarının veritabanında değerlendirilebilir tanımlar</span><span class="sxs-lookup"><span data-stu-id="37941-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="37941-111">Bu sorgunun bölümlerini veritabanı belirli sorgu dili (örneğin bir ilişkisel veritabanı için SQL) için çevrilir</span><span class="sxs-lookup"><span data-stu-id="37941-111">These parts of the query are translated to database specific query language (e.g. SQL for a relational database)</span></span>
   3. <span data-ttu-id="37941-112">Bir veya daha fazla sorgular ve veritabanı döndürülen sonuç gönderilir (sonucu olan değerleri veritabanından varlık örnekleri)</span><span class="sxs-lookup"><span data-stu-id="37941-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="37941-113">Her öğe için sonuç kümesinde</span><span class="sxs-lookup"><span data-stu-id="37941-113">For each item in the result set</span></span>
   1. <span data-ttu-id="37941-114">Bu izleme sorgu ise, veri bağlamı örneği için değişiklik İzleyicisi'ni zaten bir varlığı temsil edip etmediğini EF denetler</span><span class="sxs-lookup"><span data-stu-id="37941-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="37941-115">Bu durumda, var olan varlık döndürülür</span><span class="sxs-lookup"><span data-stu-id="37941-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="37941-116">Aksi durumda, yeni bir varlık oluşturulur, değişiklik izleme kurulması ve yeni bir varlık döndürdü</span><span class="sxs-lookup"><span data-stu-id="37941-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="37941-117">Hayır-izleme sorgu varsa EF verileri zaten bu sorgu için sonuç bir varlığı temsil edip etmediğini denetler</span><span class="sxs-lookup"><span data-stu-id="37941-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="37941-118">Bu nedenle, varolan varlık döndürülürse <sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="37941-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="37941-119">Aksi halde yeni bir varlık oluşturulur ve döndürdü</span><span class="sxs-lookup"><span data-stu-id="37941-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="37941-120"><sup>(1) </sup> Zaten döndürülen varlıkları izlemek için zayıf başvurular hiçbir izleme sorguları kullanın.</span><span class="sxs-lookup"><span data-stu-id="37941-120"><sup>(1)</sup> No tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="37941-121">Aynı kimliğe sahip bir önceki sonuç kapsamının dışına gider ve atık toplama devam, yeni bir varlık örneği elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="37941-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="37941-122">Sorguları ne zaman çalıştırılır</span><span class="sxs-lookup"><span data-stu-id="37941-122">When queries are executed</span></span>

<span data-ttu-id="37941-123">LINQ işleçleri çağırdığınızda, sorgu bir bellek içi temsili yalnızca oluşturmakta olduğunuz.</span><span class="sxs-lookup"><span data-stu-id="37941-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="37941-124">Sorgu veritabanına sonuçları tüketilen yalnızca gönderilir.</span><span class="sxs-lookup"><span data-stu-id="37941-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="37941-125">Veritabanına gönderilen sorgu sonucunda en yaygın işlemleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="37941-125">The most common operations that result in the query being sent to the database are:</span></span>
* <span data-ttu-id="37941-126">Sonuçlarda yineleme bir `for` döngü</span><span class="sxs-lookup"><span data-stu-id="37941-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="37941-127">Bir işleç gibi kullanılarak `ToList`, `ToArray`, `Single`, `Count`</span><span class="sxs-lookup"><span data-stu-id="37941-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="37941-128">Veri bağlama bir kullanıcı Arabirimi için bir sorgu sonuçları</span><span class="sxs-lookup"><span data-stu-id="37941-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="37941-129">**Kullanıcı girişi her zaman doğrulayın:** sırada EF SQL ekleme saldırılarına karşı koruma sağlar, tüm genel doğrulama giriş yapın.</span><span class="sxs-lookup"><span data-stu-id="37941-129">**Always validate user input:** While EF does provide protection from SQL injection attacks, it does not do any general validation of input.</span></span> <span data-ttu-id="37941-130">LINQ sorgularını, atanmış varlık özellikleri, vb. için kullanılan API'leri, geçirilen değerleri güvenilmeyen kaynak sonra uygulama gereksinimlerinizi başına uygun doğrulama alınması, bu nedenle gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="37941-130">Therefore if values being passed to APIs, used in LINQ queries, assigned to entity properties, etc., come from an untrusted source then appropriate validation, per your application requirements, should be performed.</span></span> <span data-ttu-id="37941-131">Bu, dinamik sorgular oluşturmak için kullanılan herhangi bir kullanıcı giriş içerir.</span><span class="sxs-lookup"><span data-stu-id="37941-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="37941-132">Yalnızca hedeflenen ifadeleri emin olmanız gerekir ifadeleri oluşturmak için oluşturulan kullanıcı girişi kabul ediyorsunuz bile LINQ, kullanırken.</span><span class="sxs-lookup"><span data-stu-id="37941-132">Even when using LINQ, if you are accepting user input to build expressions you need to make sure than only intended expressions can be constructed.</span></span>
