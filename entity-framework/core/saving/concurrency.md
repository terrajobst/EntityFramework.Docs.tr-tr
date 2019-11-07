---
title: Eşzamanlılık çakışmalarını işleme-EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: b72fa472698e76e18f155cf96b738b0e193eee0f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654624"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="b63dc-102">Eşzamanlılık Çakışmalarını İşleme</span><span class="sxs-lookup"><span data-stu-id="b63dc-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="b63dc-103">Bu sayfa, eşzamanlılık EF Core ' de nasıl çalıştığını ve uygulamanızda eşzamanlılık çakışmalarının nasıl işleneceğini belgeler.</span><span class="sxs-lookup"><span data-stu-id="b63dc-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="b63dc-104">Modelinizdeki eşzamanlılık belirteçlerini yapılandırma hakkında ayrıntılı bilgi için bkz. [eşzamanlılık belirteçleri](xref:core/modeling/concurrency) .</span><span class="sxs-lookup"><span data-stu-id="b63dc-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="b63dc-105">Bu makalenin [örneğini](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) GitHub ' da görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b63dc-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="b63dc-106">_Veritabanı eşzamanlılık_ , birden fazla işlem veya kullanıcının aynı anda bir veritabanındaki verileri erişen veya değiştiren durumlara başvurur.</span><span class="sxs-lookup"><span data-stu-id="b63dc-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="b63dc-107">_Eşzamanlılık denetimi_ , eşzamanlı değişiklikler olması halinde veri tutarlılığı sağlamak için kullanılan belirli mekanizmaların anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="b63dc-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="b63dc-108">EF Core, birden çok işlemin veya kullanıcının değişiklik yapmasına veya kilitlenmeden bağımsız olarak değişiklik yapmasına olanak tanıyan _iyimser eşzamanlılık denetimini_uygular.</span><span class="sxs-lookup"><span data-stu-id="b63dc-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="b63dc-109">İdeal durumda, bu değişiklikler birbirini engellemez ve bu nedenle başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="b63dc-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="b63dc-110">En kötü durum senaryosunda, iki veya daha fazla işlem çakışan değişiklikler yapmaya çalışacaktır ve bunlardan yalnızca biri başarılı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b63dc-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="b63dc-111">Eşzamanlılık denetimi nasıl EF Core?</span><span class="sxs-lookup"><span data-stu-id="b63dc-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="b63dc-112">Eşzamanlılık belirteçleri olarak yapılandırılan Özellikler iyimser eşzamanlılık denetimi uygulamak için kullanılır: `SaveChanges`sırasında her bir güncelleştirme veya silme işlemi gerçekleştirildiğinde, veritabanındaki eşzamanlılık belirtecinin değeri, okunan özgün değere göre karşılaştırılır EF Core.</span><span class="sxs-lookup"><span data-stu-id="b63dc-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="b63dc-113">Değerler eşleşiyorsa, işlem tamamlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b63dc-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="b63dc-114">Değerler eşleşmezse EF Core başka bir kullanıcının çakışan bir işlem gerçekleştirdiğinizi ve geçerli işlemi iptal eder.</span><span class="sxs-lookup"><span data-stu-id="b63dc-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="b63dc-115">Başka bir Kullanıcı, geçerli işlemle çakışan bir işlem gerçekleştirdiğinde _eşzamanlılık çakışması_olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="b63dc-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="b63dc-116">Veritabanı sağlayıcıları eşzamanlılık belirteci değerlerinin karşılaştırmasını uygulamaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="b63dc-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="b63dc-117">İlişkisel veritabanlarında EF Core, herhangi bir `UPDATE` veya `DELETE` deyimlerinin `WHERE` yan tümcesindeki eşzamanlılık belirtecinin değeri için bir denetim içerir.</span><span class="sxs-lookup"><span data-stu-id="b63dc-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="b63dc-118">Deyimlerini yürüttükten sonra, EF Core etkilenen satır sayısını okur.</span><span class="sxs-lookup"><span data-stu-id="b63dc-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="b63dc-119">Hiçbir satır etkilenmiyorsa, bir eşzamanlılık çakışması algılanır ve EF Core `DbUpdateConcurrencyException`oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b63dc-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="b63dc-120">Örneğin, `Person` `LastName` bir eşzamanlılık belirteci olacak şekilde yapılandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b63dc-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="b63dc-121">Ardından, kişi üzerindeki tüm güncelleştirme işlemleri `WHERE` yan tümcesine eşzamanlılık denetimini içerir:</span><span class="sxs-lookup"><span data-stu-id="b63dc-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="b63dc-122">Eşzamanlılık çakışmalarını çözme</span><span class="sxs-lookup"><span data-stu-id="b63dc-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="b63dc-123">Önceki örnekle devam etmek, bir Kullanıcı `Person`bazı değişiklikleri kaydetmeye çalışırsa, ancak başka bir Kullanıcı `LastName`zaten değiştirdiyseniz, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b63dc-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="b63dc-124">Bu noktada, uygulama, değişiklikleri çakışan değişiklikler nedeniyle başarılı bir şekilde kullanıcıya bildirebilir ve üzerinde geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="b63dc-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="b63dc-125">Ancak kullanıcıdan bu kaydın aynı gerçek kişiyi temsil ettiğini ve işlemi yeniden denemesini istemek istenebilir.</span><span class="sxs-lookup"><span data-stu-id="b63dc-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="b63dc-126">Bu işlem, _bir eşzamanlılık çakışmasını çözmeye_yönelik bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="b63dc-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="b63dc-127">Eşzamanlılık çakışmasını çözmek, geçerli `DbContext` bekleyen değişikliklerin veritabanındaki değerlerle birleştirilmesini içerir.</span><span class="sxs-lookup"><span data-stu-id="b63dc-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="b63dc-128">Birleştirilecek değerler uygulamaya göre değişir ve Kullanıcı girişiyle yönlendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="b63dc-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="b63dc-129">**Eşzamanlılık çakışmasını çözmeye yardımcı olmak için üç değer kümesi mevcuttur:**</span><span class="sxs-lookup"><span data-stu-id="b63dc-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

- <span data-ttu-id="b63dc-130">**Geçerli değerler** , uygulamanın veritabanına yazmaya çalışan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="b63dc-130">**Current values** are the values that the application was attempting to write to the database.</span></span>
- <span data-ttu-id="b63dc-131">**Orijinal değerler** , hiçbir düzenleme yapılmadan önce veritabanından ilk olarak alınan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="b63dc-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>
- <span data-ttu-id="b63dc-132">Veritabanı **değerleri** , veritabanında Şu anda depolanan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="b63dc-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="b63dc-133">Eşzamanlılık çakışmalarını işlemeye yönelik genel yaklaşım şunlardır:</span><span class="sxs-lookup"><span data-stu-id="b63dc-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="b63dc-134">`SaveChanges`sırasında catch `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="b63dc-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="b63dc-135">Etkilenen varlıklar için yeni bir değişiklik kümesi hazırlamak üzere `DbUpdateConcurrencyException.Entries` kullanın.</span><span class="sxs-lookup"><span data-stu-id="b63dc-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="b63dc-136">Veritabanının geçerli değerlerini yansıtmak için eşzamanlılık belirtecinin orijinal değerlerini yenileyin.</span><span class="sxs-lookup"><span data-stu-id="b63dc-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="b63dc-137">Çakışma gerçekleşene kadar işlemi yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="b63dc-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="b63dc-138">Aşağıdaki örnekte `Person.FirstName` ve `Person.LastName` eşzamanlılık belirteçleri olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="b63dc-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="b63dc-139">Kaydedilecek değeri seçmek için uygulamaya özgü mantığı dahil ettiğiniz konumda bir `// TODO:` yorumu vardır.</span><span class="sxs-lookup"><span data-stu-id="b63dc-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
