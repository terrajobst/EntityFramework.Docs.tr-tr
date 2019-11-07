---
title: Basamaklı silme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: 51c8b6f4517a3f87821ed1e4e2d60549e06ed39d
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656066"
---
# <a name="cascade-delete"></a><span data-ttu-id="8a344-102">Basamaklı Silme</span><span class="sxs-lookup"><span data-stu-id="8a344-102">Cascade Delete</span></span>

<span data-ttu-id="8a344-103">Art arda silme, bir satırı silmenin ilgili satırların silinmesini otomatik olarak tetiklemesine izin veren bir özelliği anlatmak için Veritabanı terminolojisinde yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8a344-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="8a344-104">EF Core Delete davranışları tarafından ele alınan yakından ilgili bir kavram, bir üst öğe ile ilişkisi olduğu zaman bir alt varlığın otomatik olarak silinmesinden sonra bu, yaygın olarak "artık silme" olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="8a344-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="8a344-105">EF Core birkaç farklı silme davranışı uygular ve tek tek ilişkilerin silme davranışlarının yapılandırılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="8a344-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="8a344-106">EF Core Ayrıca, [ilişkinin gerekliğine](../modeling/relationships.md#required-and-optional-relationships)göre her ilişki için yararlı varsayılan silme davranışlarını otomatik olarak yapılandıran kuralları uygular.</span><span class="sxs-lookup"><span data-stu-id="8a344-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="8a344-107">Davranışları Sil</span><span class="sxs-lookup"><span data-stu-id="8a344-107">Delete behaviors</span></span>

<span data-ttu-id="8a344-108">Silme davranışları *DeleteBehavior* Numaralandırıcı türünde tanımlanır ve bir sorumlu/üst varlığın silinmesini veya bağımlı/alt varlıklara olan ilişkinin ne kadar olduğunu denetlemek Için *OnDelete* Fluent API geçirilebilir bağımlı/alt varlıklar üzerinde yan etkisi vardır.</span><span class="sxs-lookup"><span data-stu-id="8a344-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="8a344-109">Bir asıl/üst varlık silindiğinde veya alt öğeyle olan ilişki bırakıldığında, EF 'in gerçekleştirebileceği üç eylem vardır:</span><span class="sxs-lookup"><span data-stu-id="8a344-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>

* <span data-ttu-id="8a344-110">Alt/Bağımlı öğe silinebilir</span><span class="sxs-lookup"><span data-stu-id="8a344-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="8a344-111">Çocuğun yabancı anahtar değerleri null olarak ayarlanabilir</span><span class="sxs-lookup"><span data-stu-id="8a344-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="8a344-112">Alt öğe değişmeden kalır</span><span class="sxs-lookup"><span data-stu-id="8a344-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="8a344-113">EF Core modelinde yapılandırılan silme davranışı yalnızca asıl varlık EF Core kullanılarak silindiğinde ve bağımlı varlıkların belleğe yüklenmesi durumunda (izlenen bağımlılar için) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8a344-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (that is, for tracked dependents).</span></span> <span data-ttu-id="8a344-114">Bağlam tarafından izlenmeyen verilerin uygulanmış olduğundan emin olmak için, karşılık gelen bir Cascade davranışının veritabanında kurulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a344-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="8a344-115">Veritabanını oluşturmak için EF Core kullanırsanız, bu basamaklı davranış sizin için kurulum olacaktır.</span><span class="sxs-lookup"><span data-stu-id="8a344-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="8a344-116">Yukarıdaki ikinci eylem için yabancı anahtar null yapılabilir değilse, yabancı anahtar değerini null olarak ayarlama geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="8a344-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="8a344-117">(Null yapılamayan yabancı anahtar, gerekli bir ilişkiye eşdeğerdir.) Bu durumlarda EF Core, SaveChanges çağrılmadan önce yabancı anahtar özelliğinin null olarak işaretlendiğinden, değişikliğin veritabanına kalıcı hale uğradığından bir özel durum oluşturulduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="8a344-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="8a344-118">Bu, veritabanından bir kısıtlama ihlali almaya benzer.</span><span class="sxs-lookup"><span data-stu-id="8a344-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="8a344-119">Aşağıdaki tablolarda listelendiği gibi dört silme davranışı vardır.</span><span class="sxs-lookup"><span data-stu-id="8a344-119">There are four delete behaviors, as listed in the tables below.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="8a344-120">İsteğe bağlı ilişkiler</span><span class="sxs-lookup"><span data-stu-id="8a344-120">Optional relationships</span></span>

<span data-ttu-id="8a344-121">İsteğe bağlı ilişkiler için (boş değer atanabilir yabancı anahtar), aşağıdaki etkilere neden olan bir boş yabancı anahtar değeri _kaydetmek mümkündür:_</span><span class="sxs-lookup"><span data-stu-id="8a344-121">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="8a344-122">Davranış adı</span><span class="sxs-lookup"><span data-stu-id="8a344-122">Behavior Name</span></span>               | <span data-ttu-id="8a344-123">Bellekte bağımlı/alt öğe üzerindeki etki</span><span class="sxs-lookup"><span data-stu-id="8a344-123">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="8a344-124">Veritabanında bağımlı/alt öğe üzerindeki etki</span><span class="sxs-lookup"><span data-stu-id="8a344-124">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="8a344-125">**Seçilemez**</span><span class="sxs-lookup"><span data-stu-id="8a344-125">**Cascade**</span></span>                 | <span data-ttu-id="8a344-126">Varlıklar silindi</span><span class="sxs-lookup"><span data-stu-id="8a344-126">Entities are deleted</span></span>                   | <span data-ttu-id="8a344-127">Varlıklar silindi</span><span class="sxs-lookup"><span data-stu-id="8a344-127">Entities are deleted</span></span>                   |
| <span data-ttu-id="8a344-128">**Clientsetnull** (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="8a344-128">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="8a344-129">Yabancı anahtar özellikleri null olarak ayarlandı</span><span class="sxs-lookup"><span data-stu-id="8a344-129">Foreign key properties are set to null</span></span> | <span data-ttu-id="8a344-130">Yok.</span><span class="sxs-lookup"><span data-stu-id="8a344-130">None</span></span>                                   |
| <span data-ttu-id="8a344-131">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="8a344-131">**SetNull**</span></span>                 | <span data-ttu-id="8a344-132">Yabancı anahtar özellikleri null olarak ayarlandı</span><span class="sxs-lookup"><span data-stu-id="8a344-132">Foreign key properties are set to null</span></span> | <span data-ttu-id="8a344-133">Yabancı anahtar özellikleri null olarak ayarlandı</span><span class="sxs-lookup"><span data-stu-id="8a344-133">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="8a344-134">**Girmesini**</span><span class="sxs-lookup"><span data-stu-id="8a344-134">**Restrict**</span></span>                | <span data-ttu-id="8a344-135">Yok.</span><span class="sxs-lookup"><span data-stu-id="8a344-135">None</span></span>                                   | <span data-ttu-id="8a344-136">Yok.</span><span class="sxs-lookup"><span data-stu-id="8a344-136">None</span></span>                                   |

### <a name="required-relationships"></a><span data-ttu-id="8a344-137">Gerekli ilişkiler</span><span class="sxs-lookup"><span data-stu-id="8a344-137">Required relationships</span></span>

<span data-ttu-id="8a344-138">Gerekli ilişkiler (null yapılamayan yabancı anahtar) için, bir boş yabancı anahtar değeri kaydetmek mümkün _değildir_ , bu durum aşağıdaki etkilere neden olur:</span><span class="sxs-lookup"><span data-stu-id="8a344-138">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="8a344-139">Davranış adı</span><span class="sxs-lookup"><span data-stu-id="8a344-139">Behavior Name</span></span>         | <span data-ttu-id="8a344-140">Bellekte bağımlı/alt öğe üzerindeki etki</span><span class="sxs-lookup"><span data-stu-id="8a344-140">Effect on dependent/child in memory</span></span> | <span data-ttu-id="8a344-141">Veritabanında bağımlı/alt öğe üzerindeki etki</span><span class="sxs-lookup"><span data-stu-id="8a344-141">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="8a344-142">**Basamakla** (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="8a344-142">**Cascade** (Default)</span></span> | <span data-ttu-id="8a344-143">Varlıklar silindi</span><span class="sxs-lookup"><span data-stu-id="8a344-143">Entities are deleted</span></span>                | <span data-ttu-id="8a344-144">Varlıklar silindi</span><span class="sxs-lookup"><span data-stu-id="8a344-144">Entities are deleted</span></span>                  |
| <span data-ttu-id="8a344-145">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="8a344-145">**ClientSetNull**</span></span>     | <span data-ttu-id="8a344-146">SaveChanges atar</span><span class="sxs-lookup"><span data-stu-id="8a344-146">SaveChanges throws</span></span>                  | <span data-ttu-id="8a344-147">Yok.</span><span class="sxs-lookup"><span data-stu-id="8a344-147">None</span></span>                                  |
| <span data-ttu-id="8a344-148">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="8a344-148">**SetNull**</span></span>           | <span data-ttu-id="8a344-149">SaveChanges atar</span><span class="sxs-lookup"><span data-stu-id="8a344-149">SaveChanges throws</span></span>                  | <span data-ttu-id="8a344-150">SaveChanges atar</span><span class="sxs-lookup"><span data-stu-id="8a344-150">SaveChanges throws</span></span>                    |
| <span data-ttu-id="8a344-151">**Girmesini**</span><span class="sxs-lookup"><span data-stu-id="8a344-151">**Restrict**</span></span>          | <span data-ttu-id="8a344-152">Yok.</span><span class="sxs-lookup"><span data-stu-id="8a344-152">None</span></span>                                | <span data-ttu-id="8a344-153">Yok.</span><span class="sxs-lookup"><span data-stu-id="8a344-153">None</span></span>                                  |

<span data-ttu-id="8a344-154">Yukarıdaki tablolarda, *none* bir kısıtlama ihlaline yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="8a344-154">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="8a344-155">Örneğin, bir asıl/alt varlık silinirse ancak bağımlı/alt öğenin yabancı anahtarını değiştirmek için herhangi bir eylem yapılmaz, veritabanı büyük olasılıkla yabancı bir kısıtlama ihlali nedeniyle SaveChanges üzerinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8a344-155">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="8a344-156">Yüksek düzeyde:</span><span class="sxs-lookup"><span data-stu-id="8a344-156">At a high level:</span></span>

* <span data-ttu-id="8a344-157">Üst öğesi olmadan var olmayan varlıklarınız varsa ve alt öğeleri otomatik olarak silmek için EF 'i istiyorsanız, *Cascade*kullanın.</span><span class="sxs-lookup"><span data-stu-id="8a344-157">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="8a344-158">Üst öğe olmadan mevcut olmayan varlıklar genellikle, *Cascade* varsayılan değer olan gerekli ilişkilerden birini kullanır.</span><span class="sxs-lookup"><span data-stu-id="8a344-158">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="8a344-159">Bir üst öğeye sahip olabilecek veya olmayan varlıklarınız varsa ve yabancı anahtarı sizin yerinize dışarıda bırakmak istiyorsanız, bu durumda *Clientsetnull* kullanın</span><span class="sxs-lookup"><span data-stu-id="8a344-159">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="8a344-160">Bir üst öğesi olmadan var olabilen varlıklar genellikle isteğe bağlı ilişkilerden ( *Clientsetnull* değeri için varsayılan değer) kullanır.</span><span class="sxs-lookup"><span data-stu-id="8a344-160">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="8a344-161">Ayrıca, alt varlık yüklü olmasa bile veritabanının null değerleri alt yabancı anahtarlara yaymasını istiyorsanız, *SetNull*' ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="8a344-161">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="8a344-162">Bununla birlikte, veritabanının bunu desteklemesi gerektiğini ve bu şekilde veritabanının yapılandırılmasını unutmayın. Bu, uygulamada genellikle bu seçeneği pratik hale getirir.</span><span class="sxs-lookup"><span data-stu-id="8a344-162">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="8a344-163">Bu, *SetNull* varsayılan değer değildir.</span><span class="sxs-lookup"><span data-stu-id="8a344-163">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="8a344-164">EF Core bir varlığı otomatik olarak silmek veya yabancı anahtarı otomatik olarak boş bırakmak istemiyorsanız *kısıtla*' yı kullanın.</span><span class="sxs-lookup"><span data-stu-id="8a344-164">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="8a344-165">Bunun için, kodunuzun alt varlıkları ve yabancı anahtar değerlerini el ile eşitlenmiş halde tutmasını, aksi takdirde kısıtlama özel durumları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8a344-165">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="8a344-166">EF Core ' de, EF6 aksine, geçişli efektler hemen gerçekleşmez, ancak bunun yerine SaveChanges çağrılır.</span><span class="sxs-lookup"><span data-stu-id="8a344-166">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="8a344-167">**EF Core 2,0 değişiklikleri:** Önceki sürümlerde *kısıtlama* , izlenen bağımlı varlıklarda isteğe bağlı yabancı anahtar özelliklerinin null olarak ayarlanmasına ve isteğe bağlı ilişkiler için varsayılan silme davranışına neden olur.</span><span class="sxs-lookup"><span data-stu-id="8a344-167">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="8a344-168">EF Core 2,0 ' de, bu davranışı temsil eden ve isteğe bağlı ilişkiler için varsayılan değer haline getirilen *Clientsetnull* eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="8a344-168">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="8a344-169">*Kısıtlama* davranışı, bağımlı varlıklar üzerinde hiçbir bir yan etkiye hiçbir şekilde ayarlanmıştı.</span><span class="sxs-lookup"><span data-stu-id="8a344-169">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="8a344-170">Varlık silme örnekleri</span><span class="sxs-lookup"><span data-stu-id="8a344-170">Entity deletion examples</span></span>

<span data-ttu-id="8a344-171">Aşağıdaki kod, indirilebilen ve çalıştırılabilen bir [Örneğin](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="8a344-171">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="8a344-172">Örnek, bir üst varlık silindiğinde hem isteğe bağlı hem de gerekli ilişkilerin her silme davranışı için ne olacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="8a344-172">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="8a344-173">Ne olduğunu anlamak için her çeşitlemeyi inceleyelim.</span><span class="sxs-lookup"><span data-stu-id="8a344-173">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="8a344-174">DeleteBehavior. Cascade, gerekli veya isteğe bağlı ilişki</span><span class="sxs-lookup"><span data-stu-id="8a344-174">DeleteBehavior.Cascade with required or optional relationship</span></span>

```console
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="8a344-175">Blog silindi olarak işaretlendi</span><span class="sxs-lookup"><span data-stu-id="8a344-175">Blog is marked as Deleted</span></span>
* <span data-ttu-id="8a344-176">SaveChanges, SaveChanges 'e kadar basamaklar olmadığı için başlangıçta değiştirilmeden kalır</span><span class="sxs-lookup"><span data-stu-id="8a344-176">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="8a344-177">SaveChanges hem bağımlılar/alt öğe (gönderiler) hem de asıl/üst öğe (blog) için silme gönderir</span><span class="sxs-lookup"><span data-stu-id="8a344-177">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="8a344-178">Kaydettikten sonra, veritabanından silindiklerinden beri tüm varlıklar ayrılır</span><span class="sxs-lookup"><span data-stu-id="8a344-178">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="8a344-179">Gerekli ilişki ile DeleteBehavior. ClientSetNull veya DeleteBehavior. SetNull</span><span class="sxs-lookup"><span data-stu-id="8a344-179">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="8a344-180">Blog silindi olarak işaretlendi</span><span class="sxs-lookup"><span data-stu-id="8a344-180">Blog is marked as Deleted</span></span>
* <span data-ttu-id="8a344-181">SaveChanges, SaveChanges 'e kadar basamaklar olmadığı için başlangıçta değiştirilmeden kalır</span><span class="sxs-lookup"><span data-stu-id="8a344-181">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="8a344-182">SaveChanges Post FK 'ı null olarak ayarlamaya çalışır, ancak FK null değer atanabilir olmadığından bu başarısız olur</span><span class="sxs-lookup"><span data-stu-id="8a344-182">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="8a344-183">İsteğe bağlı ilişki ile DeleteBehavior. ClientSetNull veya DeleteBehavior. SetNull</span><span class="sxs-lookup"><span data-stu-id="8a344-183">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="8a344-184">Blog silindi olarak işaretlendi</span><span class="sxs-lookup"><span data-stu-id="8a344-184">Blog is marked as Deleted</span></span>
* <span data-ttu-id="8a344-185">SaveChanges, SaveChanges 'e kadar basamaklar olmadığı için başlangıçta değiştirilmeden kalır</span><span class="sxs-lookup"><span data-stu-id="8a344-185">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="8a344-186">SaveChanges denemesi, asıl/üst öğeyi (blog) silmeden önce hem bağımlıların hem de alt öğelerin (gönderilerin) FK değerini null olarak ayarlar</span><span class="sxs-lookup"><span data-stu-id="8a344-186">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="8a344-187">Kaydettikten sonra asıl/üst öğe (blog) silinir, ancak bağımlılar/alt öğeler (gönderiler) hala izlenir</span><span class="sxs-lookup"><span data-stu-id="8a344-187">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="8a344-188">İzlenen bağımlılar/alt öğeler (gönderiler) artık null FK değerlerine sahiptir ve silinen asıl/üst öğeye (blog) başvuruları kaldırılmıştır</span><span class="sxs-lookup"><span data-stu-id="8a344-188">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="8a344-189">DeleteBehavior. gerekli veya isteğe bağlı ilişki ile kısıtla</span><span class="sxs-lookup"><span data-stu-id="8a344-189">DeleteBehavior.Restrict with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="8a344-190">Blog silindi olarak işaretlendi</span><span class="sxs-lookup"><span data-stu-id="8a344-190">Blog is marked as Deleted</span></span>
* <span data-ttu-id="8a344-191">SaveChanges, SaveChanges 'e kadar basamaklar olmadığı için başlangıçta değiştirilmeden kalır</span><span class="sxs-lookup"><span data-stu-id="8a344-191">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="8a344-192">*Restrict* , EF 'in FK otomatik olarak null olarak ayarlanmayacağını söylediğinden, null olmayan ve SaveChanges, kaydetmeden önce</span><span class="sxs-lookup"><span data-stu-id="8a344-192">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="8a344-193">Artık örnekleri Sil örnekleri</span><span class="sxs-lookup"><span data-stu-id="8a344-193">Delete orphans examples</span></span>

<span data-ttu-id="8a344-194">Aşağıdaki kod, indirilebilen ve çalıştırılabilen bir [Örneğin](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="8a344-194">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="8a344-195">Örnek, bir üst/birincil ve alt öğeleri/bağımlılığının arasındaki ilişki olmadığında, hem isteğe bağlı hem de gerekli ilişkiler için her silme davranışının ne olacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="8a344-195">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="8a344-196">Bu örnekte, birincil/üst öğe (blog) üzerindeki koleksiyon gezintisi özelliğinden bağımlılar/alt öğeler (postalar) kaldırılarak ilişki ortadan kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="8a344-196">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="8a344-197">Ancak, bağımlı/alt ile asıl/üst öğeye yapılan başvurunun null olarak dışına çıkar olması durumunda davranış aynıdır.</span><span class="sxs-lookup"><span data-stu-id="8a344-197">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="8a344-198">Ne olduğunu anlamak için her çeşitlemeyi inceleyelim.</span><span class="sxs-lookup"><span data-stu-id="8a344-198">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="8a344-199">DeleteBehavior. Cascade, gerekli veya isteğe bağlı ilişki</span><span class="sxs-lookup"><span data-stu-id="8a344-199">DeleteBehavior.Cascade with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="8a344-200">Gönderi, FK 'in null olarak işaretlendiğinden ilişki kesilmesine neden olduğu için değiştirilmiş olarak işaretlenir</span><span class="sxs-lookup"><span data-stu-id="8a344-200">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="8a344-201">FK null yapılabilir değilse, gerçek değer null olarak işaretlenmiş olsa bile değişmeyecektir</span><span class="sxs-lookup"><span data-stu-id="8a344-201">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="8a344-202">SaveChanges, bağımlılar/çocuklar için silme gönderir (gönderiler)</span><span class="sxs-lookup"><span data-stu-id="8a344-202">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="8a344-203">Kaydettikten sonra, bağımlılıklar/alt öğeler (postalar) veritabanından silindiği için ayrılır</span><span class="sxs-lookup"><span data-stu-id="8a344-203">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="8a344-204">Gerekli ilişki ile DeleteBehavior. ClientSetNull veya DeleteBehavior. SetNull</span><span class="sxs-lookup"><span data-stu-id="8a344-204">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="8a344-205">Gönderi, FK 'in null olarak işaretlendiğinden ilişki kesilmesine neden olduğu için değiştirilmiş olarak işaretlenir</span><span class="sxs-lookup"><span data-stu-id="8a344-205">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="8a344-206">FK null yapılabilir değilse, gerçek değer null olarak işaretlenmiş olsa bile değişmeyecektir</span><span class="sxs-lookup"><span data-stu-id="8a344-206">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="8a344-207">SaveChanges Post FK 'ı null olarak ayarlamaya çalışır, ancak FK null değer atanabilir olmadığından bu başarısız olur</span><span class="sxs-lookup"><span data-stu-id="8a344-207">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="8a344-208">İsteğe bağlı ilişki ile DeleteBehavior. ClientSetNull veya DeleteBehavior. SetNull</span><span class="sxs-lookup"><span data-stu-id="8a344-208">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="8a344-209">Gönderi, FK 'in null olarak işaretlendiğinden ilişki kesilmesine neden olduğu için değiştirilmiş olarak işaretlenir</span><span class="sxs-lookup"><span data-stu-id="8a344-209">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="8a344-210">FK null yapılabilir değilse, gerçek değer null olarak işaretlenmiş olsa bile değişmeyecektir</span><span class="sxs-lookup"><span data-stu-id="8a344-210">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="8a344-211">SaveChanges hem bağımlılığının hem de alt öğelerin (gönderilerin) FK değerini null olarak ayarlar</span><span class="sxs-lookup"><span data-stu-id="8a344-211">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="8a344-212">Kaydedildikten sonra, bağımlılar/alt öğeler (gönderimler) artık null FK değerlerine sahiptir ve silinen asıl/üst öğeye (blog) başvuruları kaldırılmıştır</span><span class="sxs-lookup"><span data-stu-id="8a344-212">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="8a344-213">DeleteBehavior. gerekli veya isteğe bağlı ilişki ile kısıtla</span><span class="sxs-lookup"><span data-stu-id="8a344-213">DeleteBehavior.Restrict with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="8a344-214">Gönderi, FK 'in null olarak işaretlendiğinden ilişki kesilmesine neden olduğu için değiştirilmiş olarak işaretlenir</span><span class="sxs-lookup"><span data-stu-id="8a344-214">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="8a344-215">FK null yapılabilir değilse, gerçek değer null olarak işaretlenmiş olsa bile değişmeyecektir</span><span class="sxs-lookup"><span data-stu-id="8a344-215">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="8a344-216">*Restrict* , EF 'in FK otomatik olarak null olarak ayarlanmayacağını söylediğinden, null olmayan ve SaveChanges, kaydetmeden önce</span><span class="sxs-lookup"><span data-stu-id="8a344-216">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="8a344-217">İzlenmeyen varlıklara basamaklandırın</span><span class="sxs-lookup"><span data-stu-id="8a344-217">Cascading to untracked entities</span></span>

<span data-ttu-id="8a344-218">*SaveChanges*öğesini çağırdığınızda, Cascade silme kuralları bağlam tarafından izlenmekte olan tüm varlıklara uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8a344-218">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="8a344-219">Bu durum yukarıda gösterilen tüm örneklerde, SQL 'in hem asıl/üst öğeyi (blog) hem de tüm bağımlıları/alt öğeleri (gönderiler) silmek için oluşturulmasının durumdur:</span><span class="sxs-lookup"><span data-stu-id="8a344-219">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="8a344-220">Yalnızca asıl öğe yüklüyse (örneğin, bir blog için bir `Include(b => b.Posts)` yoksa, gönderiler dahil etmeden bir sorgu yapıldığında, SaveChanges yalnızca SQL 'i temel/üst öğeyi silmek için oluşturur:</span><span class="sxs-lookup"><span data-stu-id="8a344-220">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="8a344-221">Bağımlılar/alt öğeler (gönderimler) yalnızca, veritabanında karşılık gelen bir Cascade davranışı yapılandırıldıysa silinir.</span><span class="sxs-lookup"><span data-stu-id="8a344-221">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="8a344-222">Veritabanını oluşturmak için EF kullanırsanız, bu basamaklı davranış sizin için kurulum olacaktır.</span><span class="sxs-lookup"><span data-stu-id="8a344-222">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
