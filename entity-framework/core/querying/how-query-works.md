---
title: Sorgular nasıl çalışır-EF Core
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: bc085755f39b1288f092a8b2df892c1bf82a89f1
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72186269"
---
# <a name="how-queries-work"></a><span data-ttu-id="0de01-102">Sorgular nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="0de01-102">How Queries Work</span></span>

<span data-ttu-id="0de01-103">Entity Framework Core, veritabanındaki verileri sorgulamak için dil ile tümleşik sorgu (LINQ) kullanır.</span><span class="sxs-lookup"><span data-stu-id="0de01-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="0de01-104">LINQ, türetilmiş bağlama ve C# varlık sınıflarınıza göre kesin tür belirtilmiş sorgular yazmak için (veya .net dilinizi tercih ettiğiniz) kullanmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="0de01-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="0de01-105">Bir sorgunun ömrü</span><span class="sxs-lookup"><span data-stu-id="0de01-105">The life of a query</span></span>

<span data-ttu-id="0de01-106">Aşağıda her bir sorgunun gittiği işleme ilişkin üst düzey bir genel bakış verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="0de01-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="0de01-107">LINQ sorgusu, veritabanı sağlayıcısı tarafından işlenmek üzere hazırlanmaya yönelik bir temsili oluşturmak için Entity Framework Core tarafından işlenir</span><span class="sxs-lookup"><span data-stu-id="0de01-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="0de01-108">Sonuç, bu işlemin sorgu her yürütüldüğünde yapılması gerekmemesi için önbelleğe alınır</span><span class="sxs-lookup"><span data-stu-id="0de01-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="0de01-109">Sonuç veritabanı sağlayıcısına geçirilir</span><span class="sxs-lookup"><span data-stu-id="0de01-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="0de01-110">Veritabanı sağlayıcısı, sorgunun hangi bölümlerinin veritabanında değerlendirilebileceğini belirler</span><span class="sxs-lookup"><span data-stu-id="0de01-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="0de01-111">Sorgunun bu bölümleri veritabanına özgü sorgu diline çevrilir (örneğin, bir ilişkisel veritabanı için SQL)</span><span class="sxs-lookup"><span data-stu-id="0de01-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="0de01-112">Veritabanına bir veya daha fazla sorgu gönderiliyor ve döndürülen sonuç kümesi (sonuçlar, varlık örneklerinden değil, veritabanındaki değerlerdir)</span><span class="sxs-lookup"><span data-stu-id="0de01-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="0de01-113">Sonuç kümesindeki her öğe için</span><span class="sxs-lookup"><span data-stu-id="0de01-113">For each item in the result set</span></span>
   1. <span data-ttu-id="0de01-114">Bu bir izleme sorgiyiyiyise EF, verilerin bağlam örneği için değişiklik izleyicide zaten bir varlığı temsil ettiğini denetler</span><span class="sxs-lookup"><span data-stu-id="0de01-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="0de01-115">Öyleyse, var olan varlık döndürülür</span><span class="sxs-lookup"><span data-stu-id="0de01-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="0de01-116">Aksi takdirde, yeni bir varlık oluşturulur, değişiklik izleme kurulum olur ve yeni varlık döndürülür</span><span class="sxs-lookup"><span data-stu-id="0de01-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="0de01-117">Bu bir izleme sorgusu değilse EF, verilerin bu sorgu için sonuç kümesinde zaten olan bir varlığı temsil ettiğini denetler</span><span class="sxs-lookup"><span data-stu-id="0de01-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="0de01-118">Varsa, var olan varlık döndürülür <sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="0de01-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="0de01-119">Aksi takdirde, yeni bir varlık oluşturulur ve döndürülür</span><span class="sxs-lookup"><span data-stu-id="0de01-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="0de01-120"><sup>(1)</sup> izleme sorgusu, önceden Döndürülmüş varlıkların izlenmesini sağlamak için zayıf başvurular kullanır.</span><span class="sxs-lookup"><span data-stu-id="0de01-120"><sup>(1)</sup> No tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="0de01-121">Aynı kimliğe sahip önceki bir sonuç kapsam dışına gittiğinde ve çöp toplama işlemi çalıştırıyorsa, yeni bir varlık örneği alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0de01-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="0de01-122">Sorgular yürütüldüğünde</span><span class="sxs-lookup"><span data-stu-id="0de01-122">When queries are executed</span></span>

<span data-ttu-id="0de01-123">LINQ işleçlerini çağırdığınızda sorgunun bellek içi gösterimini oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="0de01-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="0de01-124">Sorgu yalnızca sonuçlar tüketiliyorsa veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="0de01-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="0de01-125">Veritabanına gönderilen sorgunun sonucu olan en yaygın işlemler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="0de01-125">The most common operations that result in the query being sent to the database are:</span></span>
* <span data-ttu-id="0de01-126">Sonuçları bir `for` döngüsünde yineleme</span><span class="sxs-lookup"><span data-stu-id="0de01-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="0de01-127">@No__t-0, `ToArray`, `Single`, `Count` gibi bir işleç kullanarak</span><span class="sxs-lookup"><span data-stu-id="0de01-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="0de01-128">Sorgunun sonuçlarını Kullanıcı arabirimine bağlama</span><span class="sxs-lookup"><span data-stu-id="0de01-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="0de01-129">**Kullanıcı girişini her zaman doğrula:** EF Core, sorgularda parametreleri ve kaçış sabit değerlerini kullanarak SQL ekleme saldırılarına karşı koruma sağlar, girişleri doğrulamaz.</span><span class="sxs-lookup"><span data-stu-id="0de01-129">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="0de01-130">Uygulamanın gereksinimlerine göre uygun doğrulama, güvenilmeyen kaynaklardan gelen değerler LINQ sorgularında kullanılmadan, varlık özelliklerine atanmadan veya diğer EF Core API 'Lerine geçirilmeden önce gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="0de01-130">Appropriate validation, per the application's requirements, should be performed before values from untrusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="0de01-131">Bu, sorguları dinamik olarak oluşturmak için kullanılan herhangi bir kullanıcı girişini içerir.</span><span class="sxs-lookup"><span data-stu-id="0de01-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="0de01-132">LINQ kullanırken bile, derleme ifadelerine Kullanıcı girişini kabul ediyorsanız, yalnızca amaçlanan ifadelerin oluşturulabilse emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="0de01-132">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
