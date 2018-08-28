---
title: Eşzamanlılık çakışmalarını - EF Core işleme
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: e050b17bfa31a4785161c700bc0355e83162b405
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993118"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="a19da-102">Eşzamanlılık çakışmalarını işleme</span><span class="sxs-lookup"><span data-stu-id="a19da-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="a19da-103">Bu sayfa, eşzamanlılık EF Core nasıl çalıştığını ve uygulamanızdaki eşzamanlılık çakışmalarını nasıl ele alınacağını belgeler.</span><span class="sxs-lookup"><span data-stu-id="a19da-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="a19da-104">Bkz: [eşzamanlılık belirteçleri](xref:core/modeling/concurrency) modelinizde eşzamanlılık belirteçleri yapılandırma hakkında ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="a19da-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="a19da-105">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="a19da-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="a19da-106">_Veritabanı eşzamanlılık_ , birden fazla işlem veya kullanıcıların erişim veya aynı verileri bir veritabanında aynı anda değiştirme durumları ifade eder.</span><span class="sxs-lookup"><span data-stu-id="a19da-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="a19da-107">_Eşzamanlılık denetimi_ eşzamanlı değişiklikleri oradayken veri tutarlılığını sağlamak için kullanılan belirli mekanizmaları ifade eder.</span><span class="sxs-lookup"><span data-stu-id="a19da-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="a19da-108">EF Core uygulayan _iyimser eşzamanlılık denetimi_, birden çok işlem sağlar veya kullanıcıların yaptığı değişiklikleri eşitleme yükü olmadan bağımsız olarak anlamı veya kilitlenmesi.</span><span class="sxs-lookup"><span data-stu-id="a19da-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="a19da-109">İdeal durumda, bu değişiklikler birbiriyle müdahale etmez ve bu nedenle başarılı olması mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a19da-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="a19da-110">En kötü durum senaryosunda çakışan değişiklikler yapmak iki veya daha fazla işlem dener ve bunlardan yalnızca biri başarılı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a19da-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="a19da-111">EF Core eşzamanlılık denetimi nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="a19da-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="a19da-112">Eşzamanlılık belirteçleri iyimser eşzamanlılık denetimi uygulamak için kullanılan yapılandırılmış özellikleri: her bir update veya delete işlemi gerçekleştirildi sırasında `SaveChanges`, veritabanında eşzamanlılık belirteci değeri özgün karşı karşılaştırılır. EF Core tarafından okunur değeri.</span><span class="sxs-lookup"><span data-stu-id="a19da-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="a19da-113">Değerleri eşleşirse, işlemi tamamlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a19da-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="a19da-114">Değerler eşleşmiyorsa, EF Core başka bir kullanıcı çakışan bir işlem gerçekleştirdi ve geçerli işlemi iptal varsayar.</span><span class="sxs-lookup"><span data-stu-id="a19da-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="a19da-115">Başka bir kullanıcı geçerli işlem ile çakışan bir işlem gerçekleştirildiğinde durumu olarak da bilinen _eşzamanlılık çakışması_.</span><span class="sxs-lookup"><span data-stu-id="a19da-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="a19da-116">Veritabanı sağlayıcıları eşzamanlılık belirteci değerleri karşılaştırma uygulamak için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="a19da-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="a19da-117">İlişkisel veritabanları, eşzamanlılık belirteci değeri için bir onay EF Core içeren `WHERE` herhangi yan tümcesi `UPDATE` veya `DELETE` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="a19da-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="a19da-118">EF Core deyimler yürütüldükten sonra etkilenen satır sayısını okur.</span><span class="sxs-lookup"><span data-stu-id="a19da-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="a19da-119">Hiçbir satır etkilenen bir eşzamanlılık çakışması algılandı ve EF Core oluşturur `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="a19da-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="a19da-120">Örneğin, biz yapılandırmak isteyebilirsiniz `LastName` üzerinde `Person` eşzamanlı bir simge olması.</span><span class="sxs-lookup"><span data-stu-id="a19da-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="a19da-121">Kişi üzerinde herhangi bir güncelleştirme işlemi eşzamanlılık onay işareti içerecektir sonra `WHERE` yan tümcesi:</span><span class="sxs-lookup"><span data-stu-id="a19da-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="a19da-122">Eşzamanlılık çakışmalarını çözümleme</span><span class="sxs-lookup"><span data-stu-id="a19da-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="a19da-123">Bazı değişiklikleri kaydetmek bir kullanıcı çalışırsa, önceki örneğe devam etmeden bir `Person`, ancak başka bir kullanıcı zaten değiştirdi `LastName`, sonra da bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a19da-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="a19da-124">Bu noktada, uygulamayı yalnızca güncelleştirme çakışan değişiklikler nedeniyle başarısız olduğunu kullanıcıya bildirmek ve geçin.</span><span class="sxs-lookup"><span data-stu-id="a19da-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="a19da-125">Ancak, bu kaydı hala aynı gerçek kişi temsil emin olun ve işlemi yeniden denemek için kullanıcıdan istenebilir.</span><span class="sxs-lookup"><span data-stu-id="a19da-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="a19da-126">Bu işlem, örneğidir _bir eşzamanlılık çakışması çözümleme_.</span><span class="sxs-lookup"><span data-stu-id="a19da-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="a19da-127">Bir eşzamanlılık çakışması çözümleme içerir geçerli bekleyen değişiklikleri birleştirme `DbContext` veritabanı değerleri.</span><span class="sxs-lookup"><span data-stu-id="a19da-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="a19da-128">Hangi değerlerin birleştirilir, uygulamaya göre değişir ve kullanıcı girişi tarafından yönlendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="a19da-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="a19da-129">**Değer bir eşzamanlılık çakışması çözmenize yardımcı olması kullanılabilir üç kümeleri vardır:**</span><span class="sxs-lookup"><span data-stu-id="a19da-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="a19da-130">**Geçerli değerler** uygulama veritabanına yazmaya denedi değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="a19da-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="a19da-131">**Özgün değerlerine** tüm düzenlemeleri yapılmadan önce ilk olarak veritabanından alınan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="a19da-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="a19da-132">**Veritabanı değerleri** şu anda veritabanında depolanan değer.</span><span class="sxs-lookup"><span data-stu-id="a19da-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="a19da-133">Eşzamanlılık çakışmalarını işleme için genel yaklaşım şöyledir:</span><span class="sxs-lookup"><span data-stu-id="a19da-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="a19da-134">Catch `DbUpdateConcurrencyException` sırasında `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="a19da-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="a19da-135">Kullanım `DbUpdateConcurrencyException.Entries` etkilenen varlıklar için yeni bir değişiklik kümesini hazırlamak için.</span><span class="sxs-lookup"><span data-stu-id="a19da-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="a19da-136">Veritabanı geçerli değerleri yansıtacak şekilde eşzamanlılık belirteci öğesinin özgün değerleri yenileyin.</span><span class="sxs-lookup"><span data-stu-id="a19da-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="a19da-137">Herhangi bir çakışma ortaya kadar işlemi yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="a19da-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="a19da-138">Aşağıdaki örnekte, `Person.FirstName` ve `Person.LastName` Kurulum eşzamanlılık belirteçleri değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="a19da-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="a19da-139">Var olan bir `// TODO:` yorum kaydedilecek değer seçmesini belirli mantıksal uygulama dahil olduğu yerde.</span><span class="sxs-lookup"><span data-stu-id="a19da-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
