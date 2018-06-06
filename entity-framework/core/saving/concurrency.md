---
title: Eşzamanlılık çakışmalarını - EF çekirdek işleme
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 2d8909585201a45eb020537847800f125b3b0120
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754392"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="a329a-102">Eşzamanlılık çakışmalarını işleme</span><span class="sxs-lookup"><span data-stu-id="a329a-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="a329a-103">Bu sayfayı eşzamanlılık EF çekirdek nasıl çalıştığını ve uygulamanızda eşzamanlılık çakışmaları nasıl ele alınacağını belgeler.</span><span class="sxs-lookup"><span data-stu-id="a329a-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="a329a-104">Bkz: [eşzamanlılık belirteçleri](xref:core/modeling/concurrency) eşzamanlılık belirteçleri modelinizde yapılandırma hakkında ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="a329a-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="a329a-105">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) github'da.</span><span class="sxs-lookup"><span data-stu-id="a329a-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="a329a-106">_Veritabanı eşzamanlılık_ , çok sayıda işlem veya kullanıcıların erişmek veya aynı anda aynı verileri bir veritabanındaki değiştirmek durumlarda başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="a329a-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="a329a-107">_Eşzamanlılık denetimi_ eşzamanlı değişiklikleri bulunması veri tutarlılığını sağlamak için kullanılan belirli mekanizmaları ifade eder.</span><span class="sxs-lookup"><span data-stu-id="a329a-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="a329a-108">EF çekirdek uygulayan _iyimser eşzamanlılık denetimini_, birden çok işlem sağlayacaktır veya kullanıcılar bağımsız olarak eşitleme yükü olmadan değişiklikler yapmak anlamına veya kilitleme.</span><span class="sxs-lookup"><span data-stu-id="a329a-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="a329a-109">İdeal durumda, bu değişiklikler birbirleri ile müdahale etmez ve bu nedenle başarılı mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a329a-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="a329a-110">En kötü Durum senaryosu iki veya daha fazla işlem çakışan değişiklik dener ve bunlardan yalnızca birini başarılı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a329a-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="a329a-111">Eşzamanlılık denetimi EF çekirdek nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="a329a-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="a329a-112">Eşzamanlılık belirteçleri iyimser eşzamanlılık denetimini uygulamak için kullanılan olarak yapılandırılan özellikler: her bir güncelleştirme veya silme işlemi gerçekleştirildi sırasında `SaveChanges`, Veritabanı eşzamanlılık belirteci değeri karşı özgün karşılaştırılır değer EF çekirdek tarafından okunur.</span><span class="sxs-lookup"><span data-stu-id="a329a-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="a329a-113">Değerler eşleşiyorsa işlemi tamamlayabilir.</span><span class="sxs-lookup"><span data-stu-id="a329a-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="a329a-114">Değerler eşleşmiyorsa EF çekirdek başka bir kullanıcı çakışan bir işlem gerçekleştirdi ve geçerli işlemi iptal varsayar.</span><span class="sxs-lookup"><span data-stu-id="a329a-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="a329a-115">Başka bir kullanıcı geçerli işlem ile çakışan bir işlem gerçekleştirdiğinde durum olarak bilinen _eşzamanlılık çakışması_.</span><span class="sxs-lookup"><span data-stu-id="a329a-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="a329a-116">Veritabanı sağlayıcıları eşzamanlılık belirteci değerleri karşılaştırma uygulamak için sorumlu.</span><span class="sxs-lookup"><span data-stu-id="a329a-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="a329a-117">İlişkisel veritabanları EF çekirdek eşzamanlılık belirteci değeri için bir denetimi içeren `WHERE` yan tümcesi herhangi `UPDATE` veya `DELETE` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="a329a-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="a329a-118">EF çekirdek deyimleri yürüttükten sonra etkilenen satır sayısını okur.</span><span class="sxs-lookup"><span data-stu-id="a329a-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="a329a-119">Hiçbir satır etkilenen bir eşzamanlılık çakışması algılandı ve EF çekirdek oluşturur `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="a329a-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="a329a-120">Örneğin, biz yapılandırmak isteyebilirsiniz `LastName` üzerinde `Person` bir eşzamanlılık belirteci olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a329a-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="a329a-121">Kişi üzerinde herhangi bir güncelleştirme işlemi eşzamanlılık iade içerecektir sonra `WHERE` yan tümcesi:</span><span class="sxs-lookup"><span data-stu-id="a329a-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="a329a-122">Eşzamanlılık çakışmalarını çözme</span><span class="sxs-lookup"><span data-stu-id="a329a-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="a329a-123">Bir kullanıcı için bazı değişiklikleri kaydetmek çalışırsa önceki örnekle devam etmeden bir `Person`, ancak başka bir kullanıcı zaten değiştirdi `LastName`, sonra da bir özel durum.</span><span class="sxs-lookup"><span data-stu-id="a329a-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="a329a-124">Bu noktada, uygulama yalnızca kullanıcı güncelleştirme çakışan değişiklikler nedeniyle başarısız oldu ve bildirin geçin.</span><span class="sxs-lookup"><span data-stu-id="a329a-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="a329a-125">Ancak bu kaydı hala aynı gerçek kişi temsil emin olun ve işlemi yeniden denemek için kullanıcıdan istenebilir.</span><span class="sxs-lookup"><span data-stu-id="a329a-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="a329a-126">Bu işlem, örneğidir _bir eşzamanlılık çakışması çözümleme_.</span><span class="sxs-lookup"><span data-stu-id="a329a-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="a329a-127">Bir eşzamanlılık çakışması çözümleme içerir geçerli bekleyen değişiklikleri birleştirme `DbContext` veritabanındaki değerlerle.</span><span class="sxs-lookup"><span data-stu-id="a329a-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="a329a-128">Hangi değerlerin birleştirilir uygulamanın göre değişir ve kullanıcı girişi tarafından yönlendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="a329a-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="a329a-129">**Değerleri bir eşzamanlılık çakışması çözümlenmesine yardımcı olmak kullanılabilir üç kümesi vardır:**</span><span class="sxs-lookup"><span data-stu-id="a329a-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="a329a-130">**Geçerli değerler** uygulama veritabanına yazmaya çalışıyordu değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="a329a-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="a329a-131">**Özgün değerler** tüm düzenlemeleri yapılmadan önce ilk olarak veritabanından alınan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="a329a-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="a329a-132">**Veritabanı değerleri** şu anda veritabanında depolanan değerler.</span><span class="sxs-lookup"><span data-stu-id="a329a-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="a329a-133">Bir eşzamanlılık çakışması işlemek için genel yaklaşım şöyledir:</span><span class="sxs-lookup"><span data-stu-id="a329a-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="a329a-134">Catch `DbUpdateConcurrencyException` sırasında `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="a329a-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="a329a-135">Kullanım `DbUpdateConcurrencyException.Entries` etkilenen varlıklar için yeni bir değişiklik kümesini hazırlamak için.</span><span class="sxs-lookup"><span data-stu-id="a329a-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="a329a-136">Veritabanındaki geçerli değerleri yansıtacak şekilde eşzamanlılık belirteci özgün değerlerini yenileyin.</span><span class="sxs-lookup"><span data-stu-id="a329a-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="a329a-137">Hiçbir çakışmalar kadar işlemi yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="a329a-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="a329a-138">Aşağıdaki örnekte, `Person.FirstName` ve `Person.LastName` Kurulum eşzamanlılık belirteçleri değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="a329a-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="a329a-139">Var olan bir `// TODO:` kaydedilecek değer seçmek için uygulama belirli mantığını dahil olduğu konumda açıklama.</span><span class="sxs-lookup"><span data-stu-id="a329a-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
