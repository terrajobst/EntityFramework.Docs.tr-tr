---
title: "Eşzamanlılık belirteçleri - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a><span data-ttu-id="58dce-102">Eşzamanlılık belirteçleri</span><span class="sxs-lookup"><span data-stu-id="58dce-102">Concurrency Tokens</span></span>

<span data-ttu-id="58dce-103">Ardından bir özelliği bir eşzamanlılık belirteci olarak yapılandırılmışsa, EF başka bir kullanıcı o değerini veritabanında değişiklikler kayda kaydedilirken değiştirdi denetleyin.</span><span class="sxs-lookup"><span data-stu-id="58dce-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span> <span data-ttu-id="58dce-104">EF iyimser eşzamanlılık desen kullanır, yani değeri değiştirilmediği varsayar ve verileri kaydetmek, ancak değeri değiştirildi bulursa throw deneyin.</span><span class="sxs-lookup"><span data-stu-id="58dce-104">EF uses an optimistic concurrency pattern, meaning it will assume the value has not changed and try to save the data, but throw if it finds the value has been changed.</span></span>

<span data-ttu-id="58dce-105">Örneğin biz yapılandırmak isteyebilirsiniz `LastName` üzerinde `Person` bir eşzamanlılık belirteci olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="58dce-105">For example we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="58dce-106">Bazı yapılan değişiklikleri kaydetmek bir kullanıcı çalışırsa, yani bir `Person`, ancak başka bir kullanıcı değiştirdi `LastName` bir özel durum sonra.</span><span class="sxs-lookup"><span data-stu-id="58dce-106">This means that if one user tries to save some changes to a `Person`, but another user has changed the `LastName` then an exception will be thrown.</span></span> <span data-ttu-id="58dce-107">Böylece uygulamanız bu kaydı hala aynı gerçek kişi yaptıkları değişiklikleri kaydetmeden önce temsil eden emin olmak için kullanıcı isteyebilir bu tercih edilebilir.</span><span class="sxs-lookup"><span data-stu-id="58dce-107">This may be desirable so that your application can prompt the user to ensure this record still represents the same actual person before saving their changes.</span></span>

> [!NOTE]
> <span data-ttu-id="58dce-108">Bu sayfayı eşzamanlılık belirteçleri yapılandırma konusunda belgelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="58dce-108">This page documents how to configure concurrency tokens.</span></span> <span data-ttu-id="58dce-109">Bkz: [eşzamanlılık işleme](../saving/concurrency.md) uygulamanızda iyimser eşzamanlılık kullanma örnekleri için.</span><span class="sxs-lookup"><span data-stu-id="58dce-109">See [Handling Concurrency](../saving/concurrency.md) for examples of how to use optimistic concurrency in your application.</span></span>

## <a name="how-concurrency-tokens-work-in-ef"></a><span data-ttu-id="58dce-110">Eşzamanlılık belirteçleri EF içinde nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="58dce-110">How concurrency tokens work in EF</span></span>

<span data-ttu-id="58dce-111">Veri depoları, güncelleştirilmiş veya hala silinmiş herhangi bir kayıt bağlam veritabanından veri ilk yüklendiğinde atandı eşzamanlılık belirteci için aynı değere sahip denetleyerek eşzamanlılık belirteçleri zorunlu kılabilir.</span><span class="sxs-lookup"><span data-stu-id="58dce-111">Data stores can enforce concurrency tokens by checking that any record being updated or deleted still has the same value for the concurrency token that was assigned when the context originally loaded the data from the database.</span></span>

<span data-ttu-id="58dce-112">Örneğin, ilişkisel veritabanları eşzamanlılık belirteci dahil ederek bunun `WHERE` yan tümcesi herhangi `UPDATE` veya `DELETE` komutlar ve etkilenen satırların sayısı denetleniyor.</span><span class="sxs-lookup"><span data-stu-id="58dce-112">For example, relational databases achieve this by including the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` commands and checking the number of rows that were affected.</span></span> <span data-ttu-id="58dce-113">Eşzamanlılık belirteci hala eşleşmesi durumunda bir satır güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="58dce-113">If the concurrency token still matches then one row will be updated.</span></span> <span data-ttu-id="58dce-114">Değeri veritabanındaki değer değiştiyse, hiçbir satır güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="58dce-114">If the value in the database has changed, then no rows are updated.</span></span>

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a><span data-ttu-id="58dce-115">Kurallar</span><span class="sxs-lookup"><span data-stu-id="58dce-115">Conventions</span></span>

<span data-ttu-id="58dce-116">Kurala göre hiçbir zaman eşzamanlılık belirteçleri yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="58dce-116">By convention, properties are never configured as concurrency tokens.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="58dce-117">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="58dce-117">Data Annotations</span></span>

<span data-ttu-id="58dce-118">Bir özelliği bir eşzamanlılık belirteci olarak yapılandırmak için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="58dce-118">You can use the Data Annotations to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a><span data-ttu-id="58dce-119">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="58dce-119">Fluent API</span></span>

<span data-ttu-id="58dce-120">Bir özelliği bir eşzamanlılık belirteci olarak yapılandırmak için Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="58dce-120">You can use the Fluent API to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a><span data-ttu-id="58dce-121">Zaman damgası/satır sürümü</span><span class="sxs-lookup"><span data-stu-id="58dce-121">Timestamp/row version</span></span>

<span data-ttu-id="58dce-122">Bir zaman damgası, yeni bir değer satır eklenmesi veya güncelleştirilmesi her zaman veritabanı tarafından oluşturulduğu bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="58dce-122">A timestamp is a property where a new value is generated by the database every time a row is inserted or updated.</span></span> <span data-ttu-id="58dce-123">Özelliği, ayrıca bir eşzamanlılık belirteci olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="58dce-123">The property is also treated as a concurrency token.</span></span> <span data-ttu-id="58dce-124">Bu, başka birisi için verileri sorgulanan beri güncelleştirmeye çalıştığınız bir satır değiştirmişse bir özel durum alırsınız sağlar.</span><span class="sxs-lookup"><span data-stu-id="58dce-124">This ensures you will get an exception if anyone else has modified a row that you are trying to update since you queried for the data.</span></span>

<span data-ttu-id="58dce-125">Bu, nasıl sağlanır kullanılan veritabanı kadar sağlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="58dce-125">How this is achieved is up to the database provider being used.</span></span> <span data-ttu-id="58dce-126">SQL Server için zaman damgası genellikle kullanılan bir *byte []* olacaktır özelliği kurulumunu olarak bir *ROWVERSION* veritabanındaki sütun.</span><span class="sxs-lookup"><span data-stu-id="58dce-126">For SQL Server, timestamp is usually used on a *byte[]* property, which will be setup as a *ROWVERSION* column in the database.</span></span>

### <a name="conventions"></a><span data-ttu-id="58dce-127">Kurallar</span><span class="sxs-lookup"><span data-stu-id="58dce-127">Conventions</span></span>

<span data-ttu-id="58dce-128">Kurala göre hiçbir zaman zaman damgaları yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="58dce-128">By convention, properties are never configured as timestamps.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="58dce-129">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="58dce-129">Data Annotations</span></span>

<span data-ttu-id="58dce-130">Bir özelliği bir zaman damgası yapılandırmak için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="58dce-130">You can use Data Annotations to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a><span data-ttu-id="58dce-131">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="58dce-131">Fluent API</span></span>

<span data-ttu-id="58dce-132">Fluent API bir zaman damgası bir özelliğini yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="58dce-132">You can use the Fluent API to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
