---
title: Değer Karşılaştırıcılar-EF Core
description: EF Core özellik değerlerini nasıl karşılaştırdığı denetlemek için değer Karşılaştırıcılar kullanma
author: ajcvickers
ms.date: 03/20/2020
uid: core/modeling/value-comparers
ms.openlocfilehash: 9dfed7b7ef8163f4f5c94a0c81c510807c53c13d
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80148273"
---
# <a name="value-comparers"></a><span data-ttu-id="c6e2a-103">Değer Karşılaştırıcılar</span><span class="sxs-lookup"><span data-stu-id="c6e2a-103">Value Comparers</span></span>

> [!NOTE]  
> <span data-ttu-id="c6e2a-104">Bu özellik EF Core 3,0 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-104">This feature is new in EF Core 3.0.</span></span>

> [!TIP]  
> <span data-ttu-id="c6e2a-105">Bu belgedeki kod, GitHub 'da bir [runbir örnek](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/)olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-105">The code in this document can be found on GitHub as a [runnable sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/).</span></span>

## <a name="background"></a><span data-ttu-id="c6e2a-106">Arka plan</span><span class="sxs-lookup"><span data-stu-id="c6e2a-106">Background</span></span>

<span data-ttu-id="c6e2a-107">EF Core, şu durumlarda özellik değerlerini karşılaştırması gerekir:</span><span class="sxs-lookup"><span data-stu-id="c6e2a-107">EF Core needs to compare property values when:</span></span>

* <span data-ttu-id="c6e2a-108">[Güncelleştirme değişikliklerinin algılanmasının](xref:core/saving/basic) parçası olarak bir özelliğin değiştirilip değiştirilmediğini belirleme</span><span class="sxs-lookup"><span data-stu-id="c6e2a-108">Determining whether a property has been changed as part of [detecting changes for updates](xref:core/saving/basic)</span></span>
* <span data-ttu-id="c6e2a-109">İlişkiler çözümlenirken iki anahtar değerin aynı olup olmadığını belirleme</span><span class="sxs-lookup"><span data-stu-id="c6e2a-109">Determining whether two key values are the same when resolving relationships</span></span> 

<span data-ttu-id="c6e2a-110">Bu, int, bool, DateTime, vb. gibi yaygın temel türler için otomatik olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-110">This is handled automatically for common primitive types such as int, bool, DateTime, etc.</span></span>

<span data-ttu-id="c6e2a-111">Daha karmaşık türler için, karşılaştırmanın nasıl yapılacağı gibi seçimler yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-111">For more complex types, choices need to be made as to how to do the comparison.</span></span>
<span data-ttu-id="c6e2a-112">Örneğin, bir bayt dizisi karşılaştırılabilir:</span><span class="sxs-lookup"><span data-stu-id="c6e2a-112">For example, a byte array could be compared:</span></span>

* <span data-ttu-id="c6e2a-113">Başvuruya göre yalnızca yeni bir bayt dizisi kullanılıyorsa farklılık tespit edilir</span><span class="sxs-lookup"><span data-stu-id="c6e2a-113">By reference, such that a difference is only detected if a new byte array is used</span></span>
* <span data-ttu-id="c6e2a-114">Derin karşılaştırmaya göre, dizideki baytları algılaması algılandı</span><span class="sxs-lookup"><span data-stu-id="c6e2a-114">By deep comparison, such that mutation of the bytes in the array is detected</span></span>

<span data-ttu-id="c6e2a-115">Varsayılan olarak EF Core, anahtar olmayan bayt dizileri için bu yaklaşımların ilkini kullanır.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-115">By default, EF Core uses the first of these approaches for non-key byte arrays.</span></span>
<span data-ttu-id="c6e2a-116">Diğer bir deyişle, yalnızca başvurular karşılaştırılır ve bir değişiklik yalnızca mevcut bir bayt dizisi yenisiyle değiştirildiğinde algılanır.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-116">That is, only references are compared and a change is detected only when an existing byte array is replaced with a new one.</span></span>
<span data-ttu-id="c6e2a-117">Bu, SaveChanges 'ı yürütürken birçok büyük bayt dizilerinin derin karşılaştırmayı önleyen kolay bir karardır.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-117">This is a pragmatic decision that avoids deep comparison of many large byte arrays when executing SaveChanges.</span></span>
<span data-ttu-id="c6e2a-118">Ancak, farklı bir görüntüye sahip bir resim olan bir görüntü iyi bir şekilde işlenir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-118">But the common scenario of replacing, say, an image with a different image is handled in a performant way.</span></span>

<span data-ttu-id="c6e2a-119">Öte yandan, ikili anahtarları temsil etmek için bayt dizileri kullanıldığında başvuru eşitliği çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-119">On the other hand, reference equality would not work when byte arrays are used to represent binary keys.</span></span>
<span data-ttu-id="c6e2a-120">Bir FK özelliğinin, karşılaştırılması gereken bir PK özelliği ile _aynı örneğe_ ayarlanmış olması çok düşüktür.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-120">It's very unlikely that an FK property is set to the _same instance_ as a PK property to which it needs to be compared.</span></span>
<span data-ttu-id="c6e2a-121">Bu nedenle EF Core, anahtar olarak davranan bayt dizileri için derin karşılaştırmalar kullanır.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-121">Therefore, EF Core uses deep comparisons for byte arrays acting as keys.</span></span>
<span data-ttu-id="c6e2a-122">İkili anahtarlar genellikle kısaysa bu yana büyük bir performans isabetinin olması çok düşüktür.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-122">This is unlikely to have a big performance hit since binary keys are usually short.</span></span>

### <a name="snapshots"></a><span data-ttu-id="c6e2a-123">Anlık Görüntüler</span><span class="sxs-lookup"><span data-stu-id="c6e2a-123">Snapshots</span></span>

<span data-ttu-id="c6e2a-124">Kesilebilir türlerde derin karşılaştırmalar, EF Core Özellik değeri için derin bir "Snapshot" oluşturma olanağına ihtiyaç duymasıdır.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-124">Deep comparisons on mutable types means that EF Core needs the ability to create a deep "snapshot" of the property value.</span></span>
<span data-ttu-id="c6e2a-125">Bunun yerine başvuruyu kopyalamak, _aynı nesne_olduklarından hem geçerli değerin hem de anlık görüntünün değiştirilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-125">Just copying the reference instead would result in mutating both the current value and the snapshot, since they are _the same object_.</span></span>
<span data-ttu-id="c6e2a-126">Bu nedenle, değişebilir türlerde derin karşılaştırmalar kullanıldığında derin anlık görüntüyle de gereklidir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-126">Therefore, when deep comparisons are used on mutable types, deep snapshotting is also required.</span></span>

## <a name="properties-with-value-converters"></a><span data-ttu-id="c6e2a-127">Değer Dönüştürücülerine sahip özellikler</span><span class="sxs-lookup"><span data-stu-id="c6e2a-127">Properties with value converters</span></span>

<span data-ttu-id="c6e2a-128">Yukarıdaki durumda EF Core, bayt dizileri için yerel eşleme desteğine sahiptir ve bu nedenle uygun Varsayılanları otomatik olarak seçebilirler.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-128">In the case above, EF Core has native mapping support for byte arrays and so can automatically choose appropriate defaults.</span></span>
<span data-ttu-id="c6e2a-129">Ancak, özellik bir [değer Dönüştürücüsü](xref:core/modeling/value-conversions)aracılığıyla eşlenmişse EF Core her zaman kullanılacak uygun karşılaştırmayı belirleyemez.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-129">However, if the property is mapped through a [value converter](xref:core/modeling/value-conversions), then EF Core can't always determine the appropriate comparison to use.</span></span>
<span data-ttu-id="c6e2a-130">Bunun yerine EF Core her zaman özelliğin türü tarafından tanımlanan varsayılan eşitlik karşılaştırmasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-130">Instead, EF Core always uses the default equality comparison defined by the type of the property.</span></span>
<span data-ttu-id="c6e2a-131">Bu genellikle doğrudur, ancak daha karmaşık türler eşlerken geçersiz kılınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-131">This is often correct, but may need to be overridden when mapping more complex types.</span></span>

### <a name="simple-immutable-classes"></a><span data-ttu-id="c6e2a-132">Basit sabit sınıflar</span><span class="sxs-lookup"><span data-stu-id="c6e2a-132">Simple immutable classes</span></span>

<span data-ttu-id="c6e2a-133">Basit, sabit bir sınıfı eşlemek için bir değer Dönüştürücüsü kullanan bir özelliği düşünün.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-133">Consider a property the uses a value converter to map a simple, immutable class.</span></span>

[!code-csharp[SimpleImmutableClass](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=SimpleImmutableClass)]

[!code-csharp[ConfigureImmutableClassProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=ConfigureImmutableClassProperty)]

<span data-ttu-id="c6e2a-134">Bu türün özelliklerine özel karşılaştırmalar veya anlık görüntü gerekmez, çünkü:</span><span class="sxs-lookup"><span data-stu-id="c6e2a-134">Properties of this type do not need special comparisons or snapshots because:</span></span>
* <span data-ttu-id="c6e2a-135">Farklı örneklerin doğru şekilde karşılaştırılabilmesi için eşitlik geçersiz kılındı</span><span class="sxs-lookup"><span data-stu-id="c6e2a-135">Equality is overridden so that different instances will compare correctly</span></span>
* <span data-ttu-id="c6e2a-136">Tür sabittir, bu nedenle bir anlık görüntü değeri değiştirici yoktur</span><span class="sxs-lookup"><span data-stu-id="c6e2a-136">The type is immutable, so there is no chance of mutating a snapshot value</span></span>

<span data-ttu-id="c6e2a-137">Bu durumda, EF Core varsayılan davranışı olduğu kadar iyidir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-137">So in this case the default behavior of EF Core is fine as it is.</span></span>

### <a name="simple-immutable-structs"></a><span data-ttu-id="c6e2a-138">Basit sabit yapılar</span><span class="sxs-lookup"><span data-stu-id="c6e2a-138">Simple immutable Structs</span></span>

<span data-ttu-id="c6e2a-139">Basit yapılar için eşleme de basittir ve özel Karşılaştırıcılar veya anlık görüntüyle gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-139">The mapping for simple structs is also simple and requires no special comparers or snapshotting.</span></span>

[!code-csharp[SimpleImmutableStruct](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=SimpleImmutableStruct)]

[!code-csharp[ConfigureImmutableStructProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=ConfigureImmutableStructProperty)]

<span data-ttu-id="c6e2a-140">EF Core, yapı özelliklerinin derlenmiş, üye tabanlı karşılaştırmaları oluşturmak için yerleşik desteğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-140">EF Core has built-in support for generating compiled, memberwise comparisons of struct properties.</span></span>
<span data-ttu-id="c6e2a-141">Bu, yapıların EF için eşitlik geçersiz kılınmasına gerek duymadığı anlamına gelir, ancak bunu [başka nedenlerle](/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type)yapmak yine de tercih edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-141">This means structs don't need to have equality overridden for EF, but you may still choose to do this for [other reasons](/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type).</span></span>
<span data-ttu-id="c6e2a-142">Ayrıca, yapılar sabit olduğundan ve her zaman üye tabanlı olarak kopyalandığı için özel anlık görüntüyle gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-142">Also, special snapshotting is not needed since structs immutable and are always memberwise copied anyway.</span></span>
<span data-ttu-id="c6e2a-143">(Aynı zamanda değişebilir yapılar için de geçerlidir, ancak [kesilebilir yapıların genel olarak kaçınılması gerekir](/dotnet/csharp/write-safe-efficient-code).)</span><span class="sxs-lookup"><span data-stu-id="c6e2a-143">(This is also true for mutable structs, but [mutable structs should in general be avoided](/dotnet/csharp/write-safe-efficient-code).)</span></span>

### <a name="mutable-classes"></a><span data-ttu-id="c6e2a-144">Kesilebilir sınıflar</span><span class="sxs-lookup"><span data-stu-id="c6e2a-144">Mutable classes</span></span>

<span data-ttu-id="c6e2a-145">Mümkün olduğunda değer dönüştürücülerle değişmez türler (sınıflar veya yapılar) kullanmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-145">It is recommended that you use immutable types (classes or structs) with value converters when possible.</span></span>
<span data-ttu-id="c6e2a-146">Bu genellikle daha etkilidir ve kesilebilir bir tür kullanmaktan daha temiz anlambilim içerir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-146">This is usually more efficient and has cleaner semantics than using a mutable type.</span></span>

<span data-ttu-id="c6e2a-147">Bununla birlikte, uygulamanın değiştiretiğimiz türlerin özelliklerinin kullanılması yaygındır.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-147">However, that being said, it is common to use properties of types that the application cannot change.</span></span>
<span data-ttu-id="c6e2a-148">Örneğin, bir sayı listesi içeren bir özelliği eşleme:</span><span class="sxs-lookup"><span data-stu-id="c6e2a-148">For example, mapping a property containing a list of numbers:</span></span> 

[!code-csharp[ListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ListProperty)]

<span data-ttu-id="c6e2a-149">[`List<T>` sınıfı](/dotnet/api/system.collections.generic.list-1?view=netstandard-2.1):</span><span class="sxs-lookup"><span data-stu-id="c6e2a-149">The [`List<T>` class](/dotnet/api/system.collections.generic.list-1?view=netstandard-2.1):</span></span>
* <span data-ttu-id="c6e2a-150">Başvuru eşitliği vardır; aynı değerleri içeren iki liste farklı olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-150">Has reference equality; two lists containing the same values are treated as different.</span></span>
* <span data-ttu-id="c6e2a-151">Değişebilir; Listedeki değerler eklenebilir ve kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-151">Is mutable; values in the list can be added and removed.</span></span>

<span data-ttu-id="c6e2a-152">Liste özelliğindeki tipik bir değer dönüştürme listeyi JSON öğesine ve öğesinden dönüştürebilir:</span><span class="sxs-lookup"><span data-stu-id="c6e2a-152">A typical value conversion on a list property might convert the list to and from JSON:</span></span>

[!code-csharp[ConfigureListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListProperty)]

<span data-ttu-id="c6e2a-153">Bu daha sonra, bu dönüşümle doğru karşılaştırmaları kullanmak EF Core zorlamak için özelliği `ValueComparer<T>` ayarlamayı gerektirir:</span><span class="sxs-lookup"><span data-stu-id="c6e2a-153">This then requires setting a `ValueComparer<T>` on the property to force EF Core use correct comparisons with this conversion:</span></span>

[!code-csharp[ConfigureListPropertyComparer](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListPropertyComparer)]

> [!NOTE]  
> <span data-ttu-id="c6e2a-154">Bir değer karşılaştırıcısı ayarlamak için model Oluşturucu ("floent") API 'SI henüz uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-154">The model builder ("fluent") API to set a value comparer has not yet been implemented.</span></span>
> <span data-ttu-id="c6e2a-155">Bunun yerine, yukarıdaki kod, oluşturucunun ' Metadata ' olarak kullanıma sunulan alt düzey ımutableproperty üzerinde SetValueComparer ' i çağırır.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-155">Instead, the code above calls SetValueComparer on the lower-level IMutableProperty exposed by the builder as 'Metadata'.</span></span>

<span data-ttu-id="c6e2a-156">`ValueComparer<T>` Oluşturucusu üç ifadeyi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="c6e2a-156">The `ValueComparer<T>` constructor accepts three expressions:</span></span>
* <span data-ttu-id="c6e2a-157">Kalite denetimi için bir ifade</span><span class="sxs-lookup"><span data-stu-id="c6e2a-157">An expression for checking quality</span></span>
* <span data-ttu-id="c6e2a-158">Karma kodu oluşturmak için bir ifade</span><span class="sxs-lookup"><span data-stu-id="c6e2a-158">An expression for generating a hash code</span></span>
* <span data-ttu-id="c6e2a-159">Bir değerin anlık görüntüsünü almak için bir ifade</span><span class="sxs-lookup"><span data-stu-id="c6e2a-159">An expression to snapshot a value</span></span>  

<span data-ttu-id="c6e2a-160">Bu durumda, sayı sıralarının aynı olup olmadığını denetleyerek karşılaştırma yapılır.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-160">In this case the comparison is done by checking if the sequences of numbers are the same.</span></span>

<span data-ttu-id="c6e2a-161">Benzer şekilde, karma kodu aynı sırayla oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-161">Likewise, the hash code is built from this same sequence.</span></span>
<span data-ttu-id="c6e2a-162">(Bu, değişebilir değerler üzerinde bir karma kod olduğunu ve bu nedenle [sorunlara neden](https://ericlippert.com/2011/02/28/guidelines-and-rules-for-gethashcode/)olabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-162">(Note that this is a hash code over mutable values and hence can [cause problems](https://ericlippert.com/2011/02/28/guidelines-and-rules-for-gethashcode/).</span></span>
<span data-ttu-id="c6e2a-163">Mümkünse, bunun yerine sabit olun.)</span><span class="sxs-lookup"><span data-stu-id="c6e2a-163">Be immutable instead if you can.)</span></span>

<span data-ttu-id="c6e2a-164">Anlık görüntü, liste ToList ile kopyalanarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-164">The snapshot is created by cloning the list with ToList.</span></span>
<span data-ttu-id="c6e2a-165">Bu, yalnızca listelerin değişikliğe girecek olması durumunda gereklidir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-165">Again, this is only needed if the lists are going to be mutated.</span></span>
<span data-ttu-id="c6e2a-166">Mümkünse, bunun yerine sabit olun.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-166">Be immutable instead if you can.</span></span> 

> [!NOTE]  
> <span data-ttu-id="c6e2a-167">Değer dönüştürücüler ve Karşılaştırıcılar basit temsilciler yerine ifadeler kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-167">Value converters and comparers are constructed using expressions rather than simple delegates.</span></span>
> <span data-ttu-id="c6e2a-168">Bunun nedeni, EF 'in bu ifadeleri daha sonra temsilci başına bir varlık içine derlenen çok daha karmaşık bir ifade ağacına eklemeleridir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-168">This is because EF inserts these expressions into a much more complex expression tree that is then compiled into an entity shaper delegate.</span></span>
> <span data-ttu-id="c6e2a-169">Kavramsal olarak, bu derleyici içine benzer.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-169">Conceptually, this is similar to compiler inlining.</span></span>
> <span data-ttu-id="c6e2a-170">Örneğin, basit bir dönüştürme, dönüştürme yapmak için başka bir yönteme yapılan bir çağrı yerine yalnızca atama içinde derlenmiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-170">For example, a simple conversion may just be a compiled in cast, rather than a call to another method to do the conversion.</span></span>    

### <a name="key-comparers"></a><span data-ttu-id="c6e2a-171">Anahtar Karşılaştırıcılar</span><span class="sxs-lookup"><span data-stu-id="c6e2a-171">Key comparers</span></span>

<span data-ttu-id="c6e2a-172">Arka plan bölümü, anahtar karşılaştırmaların neden özel anlambilim gerektirebileceğini ele alır.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-172">The background section covers why key comparisons may require special semantics.</span></span>
<span data-ttu-id="c6e2a-173">Birincil, sorumlu veya yabancı anahtar özelliği üzerinde ayarlama sırasında anahtarlar için uygun bir karşılaştırıcı oluşturmayı unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-173">Make sure to create a comparer that is appropriate for keys when setting it on a primary, principal, or foreign key property.</span></span>

<span data-ttu-id="c6e2a-174">Aynı özellikte farklı semantik olması gereken nadir durumlarda [Setkeyvaluecomparer](/dotnet/api/microsoft.entityframeworkcore.mutablepropertyextensions.setkeyvaluecomparer?view=efcore-3.1) kullanın.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-174">Use [SetKeyValueComparer](/dotnet/api/microsoft.entityframeworkcore.mutablepropertyextensions.setkeyvaluecomparer?view=efcore-3.1) in the rare cases where different semantics is required on the same property.</span></span>

> [!NOTE]  
> <span data-ttu-id="c6e2a-175">SetStructuralComparer EF Core 5,0 ' de kullanımdan kaldırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-175">SetStructuralComparer has been obsoleted in EF Core 5.0.</span></span>
> <span data-ttu-id="c6e2a-176">Bunun yerine SetKeyValueComparer kullanın.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-176">Use SetKeyValueComparer instead.</span></span>

### <a name="overriding-defaults"></a><span data-ttu-id="c6e2a-177">Varsayılanları geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="c6e2a-177">Overriding defaults</span></span>

<span data-ttu-id="c6e2a-178">Bazen EF Core tarafından kullanılan varsayılan karşılaştırma uygun olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-178">Sometimes the default comparison used by EF Core may not be appropriate.</span></span>
<span data-ttu-id="c6e2a-179">Örneğin, bayt dizileri, varsayılan olarak EF Core içinde algılanır.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-179">For example, mutation of byte arrays is not, by default, detected in EF Core.</span></span>
<span data-ttu-id="c6e2a-180">Bu, özellikte farklı bir karşılaştırıcı ayarlanarak geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="c6e2a-180">This can be overridden by setting a different comparer on the property:</span></span> 

[!code-csharp[OverrideComparer](../../../samples/core/Modeling/ValueConversions/OverridingByteArrayComparisons.cs?name=OverrideComparer)]

<span data-ttu-id="c6e2a-181">EF Core artık bayt dizilerini karşılaştırabilir ve bu nedenle bayt dizisi mutasyonları tespit edilir.</span><span class="sxs-lookup"><span data-stu-id="c6e2a-181">EF Core will now compare byte sequences and will therefore detect byte array mutations.</span></span>
