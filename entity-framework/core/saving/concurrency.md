---
title: Eşzamanlılık Çakışmalarını Ele Alma - EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: a1d1a5a11d482f9104691aa3c072dbd1c548e9f1
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417592"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="93b47-102">Eşzamanlılık Çakışmalarını İşleme</span><span class="sxs-lookup"><span data-stu-id="93b47-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="93b47-103">Bu sayfa, eşzamanlıbirimin EF Core'da nasıl çalıştığını ve uygulamanızdaki eşzamanlılık çakışmaları nasıl işleyeceğini belgeletir.</span><span class="sxs-lookup"><span data-stu-id="93b47-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="93b47-104">Modelinizde eşzamanlılık belirteçleri yapılandırma hakkında ayrıntılar için [Eşzamanlılık Belirteçleri'ne](xref:core/modeling/concurrency) bakın.</span><span class="sxs-lookup"><span data-stu-id="93b47-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="93b47-105">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) GitHub'da görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93b47-105">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="93b47-106">_Veritabanı eşzamanlılığı,_ birden çok işlem veya kullanıcının aynı anda bir veritabanındaki aynı verilere erişteği veya değiştirdiği durumları ifade eder.</span><span class="sxs-lookup"><span data-stu-id="93b47-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="93b47-107">_Eşzamanlı denetim,_ eşzamanlı değişikliklerin varlığında veri tutarlılığı sağlamak için kullanılan belirli mekanizmaları ifade eder.</span><span class="sxs-lookup"><span data-stu-id="93b47-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="93b47-108">EF _Core, eşitleme_veya kilitleme yükü olmadan birden çok sürecin veya kullanıcıların bağımsız olarak değişiklik yapmalarına izin verdiği anlamına gelen iyimser eşzamanlılık denetimini uygular.</span><span class="sxs-lookup"><span data-stu-id="93b47-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="93b47-109">İdeal durumda, bu değişiklikler birbirine müdahale etmeyecek ve bu nedenle başarılı olmak mümkün olacak.</span><span class="sxs-lookup"><span data-stu-id="93b47-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="93b47-110">En kötü durum senaryosunda, iki veya daha fazla işlem çakışan değişiklikler yapmaya çalışır ve bunlardan yalnızca biri başarılı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="93b47-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="93b47-111">EŞZAMANLıL denetim EF Core'da nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="93b47-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="93b47-112">Eşzamanlılık belirteçleri olarak yapılandırılan özellikler iyimser eşzamanlılık denetimini uygulamak için `SaveChanges`kullanılır: bir güncelleştirme veya silme işlemi sırasında gerçekleştirildiğinde, veritabanındaki eşzamanlılık belirteci değeri EF Core tarafından okunan özgün değerle karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="93b47-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="93b47-113">Değerler eşleşirse, işlem tamamlanabilir.</span><span class="sxs-lookup"><span data-stu-id="93b47-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="93b47-114">Değerler eşleşmiyorsa, EF Core başka bir kullanıcının çakışan bir işlem gerçekleştirdiğini varsayar ve geçerli hareketi iptal eder.</span><span class="sxs-lookup"><span data-stu-id="93b47-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="93b47-115">Başka bir kullanıcının geçerli işlemle çakışan bir işlem gerçekleştirdiği durum _eşzamanlılık çakışması_olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="93b47-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="93b47-116">Veritabanı sağlayıcıları eşzamanlılık belirteç değerlerinin karşılaştırmasını uygulamaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="93b47-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="93b47-117">İlişkisel veritabanlarında EF Core, herhangi `WHERE` `UPDATE` bir veya `DELETE` deyimin yan tümcesindeki eşzamanlılık belirteci değerini denetler.</span><span class="sxs-lookup"><span data-stu-id="93b47-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="93b47-118">İfadeleri çalıştırdıktan sonra, EF Core etkilenen satır sayısını okur.</span><span class="sxs-lookup"><span data-stu-id="93b47-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="93b47-119">Satır lar etkilenmezse, eşzamanlılık çakışması algılanır `DbUpdateConcurrencyException`ve EF Core atar.</span><span class="sxs-lookup"><span data-stu-id="93b47-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="93b47-120">Örneğin, eşzamanlılık belirteci `LastName` `Person` olarak yapılandırmak isteyebiliriz.</span><span class="sxs-lookup"><span data-stu-id="93b47-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="93b47-121">Daha sonra Person üzerindeki herhangi bir güncelleştirme `WHERE` işlemi, maddede eşzamanlılık denetimini içerir:</span><span class="sxs-lookup"><span data-stu-id="93b47-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="93b47-122">Eşzamanlılık çakışmalarını çözme</span><span class="sxs-lookup"><span data-stu-id="93b47-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="93b47-123">Önceki örnekle devam edersek, bir kullanıcı bazı değişiklikleri `Person`kaydetmeye çalışırsa, ancak `LastName`başka bir kullanıcı zaten , bir özel durum atılır.</span><span class="sxs-lookup"><span data-stu-id="93b47-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="93b47-124">Bu noktada, uygulama yalnızca çakışan değişiklikler nedeniyle güncelleştirmenin başarılı olmadığını kullanıcıya bildirebilir ve devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="93b47-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="93b47-125">Ancak, kullanıcıdan bu kaydın hala aynı gerçek kişiyi temsil ettiğinden emin olmasını ve işlemi yeniden denemesini isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="93b47-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="93b47-126">Bu işlem, _eşzamanlılık çakışması çözme_bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="93b47-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="93b47-127">Eşzamanlılık çakışması çözümlenmesi, bekleyen değişiklikleri `DbContext` geçerliden veritabanındaki değerlerle birleştirmeyi içerir.</span><span class="sxs-lookup"><span data-stu-id="93b47-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="93b47-128">Hangi değerlerin birleştirilmesi uygulamaya göre değişir ve kullanıcı girişi tarafından yönlendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="93b47-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="93b47-129">**Eşzamanlılık çakışmasını çözmeye yardımcı olmak için kullanılabilir üç değer kümesi vardır:**</span><span class="sxs-lookup"><span data-stu-id="93b47-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

- <span data-ttu-id="93b47-130">**Geçerli değerler,** uygulamanın veritabanına yazmaya çalıştığı değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="93b47-130">**Current values** are the values that the application was attempting to write to the database.</span></span>
- <span data-ttu-id="93b47-131">**Özgün değerler,** herhangi bir değiştirme yapılmadan önce ilk olarak veritabanından alınan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="93b47-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>
- <span data-ttu-id="93b47-132">**Veritabanı değerleri,** veritabanında depolanan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="93b47-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="93b47-133">Eşzamanlılık çakışmalarını işlemek için genel yaklaşım:</span><span class="sxs-lookup"><span data-stu-id="93b47-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="93b47-134">Sırasında `DbUpdateConcurrencyException` `SaveChanges`catch .</span><span class="sxs-lookup"><span data-stu-id="93b47-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="93b47-135">Etkilenen `DbUpdateConcurrencyException.Entries` varlıklar için yeni bir değişiklik kümesi hazırlamak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="93b47-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="93b47-136">Veritabanındaki geçerli değerleri yansıtacak şekilde eşzamanlılık belirteci orijinal değerlerini yenileyin.</span><span class="sxs-lookup"><span data-stu-id="93b47-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="93b47-137">Çakışma oluşmayana işlemi yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="93b47-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="93b47-138">Aşağıdaki örnekte `Person.FirstName` ve `Person.LastName` eşzamanlılık belirteçleri olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="93b47-138">In the following example, `Person.FirstName` and `Person.LastName` are set up as concurrency tokens.</span></span> <span data-ttu-id="93b47-139">Kaydedilecek `// TODO:` değeri seçmek için uygulamaya özgü mantık eklediğiniz konumda bir açıklama vardır.</span><span class="sxs-lookup"><span data-stu-id="93b47-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
