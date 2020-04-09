---
title: Sorgular Nasıl Çalışır - EF Core
author: ajcvickers
ms.date: 03/17/2020
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: e8a50efe31468ea8df211602636dd474550bc0ef
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136230"
---
# <a name="how-queries-work"></a><span data-ttu-id="55984-102">Sorgular Nasıl Çalışır?</span><span class="sxs-lookup"><span data-stu-id="55984-102">How Queries Work</span></span>

<span data-ttu-id="55984-103">Entity Framework Core, veritabanındaki verileri sorgulamak için Dil Tümleşik Sorgusu 'nu (LINQ) kullanır.</span><span class="sxs-lookup"><span data-stu-id="55984-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="55984-104">LINQ, türetilmiş bağlam ve varlık sınıflarınıza göre güçlü bir şekilde yazılan sorguları yazmak için C# (veya seçtiğiniz .NET dilini) kullanmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="55984-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="55984-105">Sorgunun ömrü</span><span class="sxs-lookup"><span data-stu-id="55984-105">The life of a query</span></span>

<span data-ttu-id="55984-106">Aşağıda, her sorgunun geçtiği işlemin üst düzey bir özeti veremiştir.</span><span class="sxs-lookup"><span data-stu-id="55984-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="55984-107">LINQ sorgusu, veritabanı sağlayıcısı tarafından işlenmeye hazır bir gösterim oluşturmak için Entity Framework Core tarafından işlenir</span><span class="sxs-lookup"><span data-stu-id="55984-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="55984-108">Sonuç önbelleğe alının, böylece sorgu her yürütüldolduğunda bu işlemin yapılması gerekmez</span><span class="sxs-lookup"><span data-stu-id="55984-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="55984-109">Sonuç veritabanı sağlayıcısına aktarılır</span><span class="sxs-lookup"><span data-stu-id="55984-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="55984-110">Veritabanı sağlayıcısı, sorgunun hangi bölümlerinin veritabanında değerlendirilebileceğini tanımlar</span><span class="sxs-lookup"><span data-stu-id="55984-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="55984-111">Sorgunun bu bölümleri veritabanına özgü sorgu diline (örneğin, ilişkisel bir veritabanı için SQL) çevrilir.</span><span class="sxs-lookup"><span data-stu-id="55984-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="55984-112">Bir sorgu veritabanına gönderilir ve sonuç kümesi döndürülür (sonuçlar varlık örnekleri değil veritabanından gelen değerlerdir)</span><span class="sxs-lookup"><span data-stu-id="55984-112">A query is sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="55984-113">Sonuç kümesindeki her öğe için</span><span class="sxs-lookup"><span data-stu-id="55984-113">For each item in the result set</span></span>
   1. <span data-ttu-id="55984-114">Bu bir izleme sorgusuysa, EF, verilerin bağlam örneği için değişiklik izleyicisinde zaten bulunan bir varlığı temsil edip edemediğini denetler</span><span class="sxs-lookup"><span data-stu-id="55984-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="55984-115">Bu nedenle, varolan varlık döndürülür</span><span class="sxs-lookup"><span data-stu-id="55984-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="55984-116">Değilse, yeni bir varlık oluşturulur, değişiklik izleme kurulumu ve yeni varlık döndürülür</span><span class="sxs-lookup"><span data-stu-id="55984-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="55984-117">Bu bir izleme sorgusu değilse, her zaman yeni bir varlık oluşturulur ve döndürülür</span><span class="sxs-lookup"><span data-stu-id="55984-117">If this is a no-tracking query, then a new entity is always created and returned</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="55984-118">Sorgular yürütüldüğünde</span><span class="sxs-lookup"><span data-stu-id="55984-118">When queries are executed</span></span>

<span data-ttu-id="55984-119">LINQ işleçlerini aradiğinizde, yalnızca sorgunun bellek içi bir temsilini oluşturuyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="55984-119">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="55984-120">Sorgu yalnızca sonuçlar tüketildiğinde veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="55984-120">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="55984-121">Sorgunun veritabanına gönderilmesiyle sonuçlanan en yaygın işlemler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="55984-121">The most common operations that result in the query being sent to the database are:</span></span>

* <span data-ttu-id="55984-122">Sonuçları bir `for` döngüde yinele</span><span class="sxs-lookup"><span data-stu-id="55984-122">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="55984-123">, `ToList`, `ToArray` `Single` `Count` , veya eşdeğer async aşırı yükleri gibi bir işleç kullanma</span><span class="sxs-lookup"><span data-stu-id="55984-123">Using an operator such as `ToList`, `ToArray`, `Single`, `Count` or the equivalent async overloads</span></span>

> [!WARNING]  
> <span data-ttu-id="55984-124">**Kullanıcı girişini her zaman doğrulayın:** EF Core parametreleri kullanarak ve sorgularda literals kaçarak SQL enjeksiyon saldırılarına karşı korur iken, girişleri doğrulamaz.</span><span class="sxs-lookup"><span data-stu-id="55984-124">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="55984-125">Uygun doğrulama, uygulamanın gereksinimlerine göre, INLAQ sorgularında güvenilmeyen kaynaklardan gelen değerler kullanılmadan, varlık özelliklerine atanmadan veya diğer EF Core API'lerine geçirilmeden önce gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="55984-125">Appropriate validation, per the application's requirements, should be performed before values from un-trusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="55984-126">Bu, sorguları dinamik olarak oluşturmak için kullanılan kullanıcı girişlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="55984-126">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="55984-127">LINQ kullanırken bile, ifadeleri oluşturmak için kullanıcı girişini kabul ediyorsanız, yalnızca amaçlanan ifadelerin oluşturulabileceğinden emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="55984-127">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
