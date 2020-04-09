---
title: Basamaklı Silme - EF Çekirdek
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: 6e92b869d691d0224abf1997d9eb7ea035489c5d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417616"
---
# <a name="cascade-delete"></a><span data-ttu-id="fede3-102">Basamaklı Silme</span><span class="sxs-lookup"><span data-stu-id="fede3-102">Cascade Delete</span></span>

<span data-ttu-id="fede3-103">Basamaklı silme genellikle veritabanı terminolojisinde, ilgili satırların silinmesini otomatik olarak tetiklemek için bir satırsilme sağlayan bir özelliği tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fede3-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="fede3-104">EF Core silme davranışları tarafından da kapsanan yakından ilişkili bir kavram, bir alt varlığın bir üst öğeyle ilişkisi kesildiğinde otomatik olarak silinmesidir - bu genellikle "yetimleri silme" olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="fede3-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="fede3-105">EF Core birkaç farklı silme davranışı uygular ve tek tek ilişkilerin silme davranışlarınıyapılandırmasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="fede3-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="fede3-106">EF Core ayrıca, [ilişkinin gerekliliğine](../modeling/relationships.md#required-and-optional-relationships)bağlı olarak her ilişki için yararlı varsayılan silme davranışlarını otomatik olarak yapılandıran kuralları da uygular.</span><span class="sxs-lookup"><span data-stu-id="fede3-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="fede3-107">Davranışları silme</span><span class="sxs-lookup"><span data-stu-id="fede3-107">Delete behaviors</span></span>

<span data-ttu-id="fede3-108">Silme davranışları *DeleteBehavior* sayısalator türünde tanımlanır ve bir asıl/üst varlığın silinmesini veya bağımlı/alt varlıklarla ilişkinin kesilmesinin bağımlı/alt varlıklar üzerinde yan etkisi olup olmadığını kontrol etmek için *OnDelete* akıcı API'sine geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="fede3-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="fede3-109">Bir asıl/üst varlık silindiğinde veya çocukla ilişki kesildiğinde EF'nin alabilecekleri üç eylem vardır:</span><span class="sxs-lookup"><span data-stu-id="fede3-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>

* <span data-ttu-id="fede3-110">Alt/bağımlı silinebilir</span><span class="sxs-lookup"><span data-stu-id="fede3-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="fede3-111">Çocuğun yabancı anahtar değerleri null ayarlanabilir</span><span class="sxs-lookup"><span data-stu-id="fede3-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="fede3-112">Çocuk değişmeden kalır</span><span class="sxs-lookup"><span data-stu-id="fede3-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="fede3-113">EF Core modelinde yapılandırılan silme davranışı yalnızca asıl varlık EF Core kullanılarak silindiğinde ve bağımlı varlıklar belleğe yüklendiğinde (diğer bir deyişle, izlenen bağımlılar için) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="fede3-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (that is, for tracked dependents).</span></span> <span data-ttu-id="fede3-114">Bağlam tarafından izlenmeden veri gerekli eylem uygulandığından emin olmak için ilgili basamaklı davranış veritabanında kurulumu gerekir.</span><span class="sxs-lookup"><span data-stu-id="fede3-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="fede3-115">Veritabanını oluşturmak için EF Core'u kullanırsanız, bu basamaklı davranış sizin için kurulum olacaktır.</span><span class="sxs-lookup"><span data-stu-id="fede3-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="fede3-116">Yukarıdaki ikinci eylem için, yabancı anahtar geçersiz değilse, geçersiz bir yabancı anahtar değeri ayarı geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="fede3-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="fede3-117">(Nullable olmayan yabancı anahtar gerekli bir ilişkiye eşdeğerdir.) Bu gibi durumlarda, EF Core, Yabancı anahtar özelliğinin SaveChanges çağrılana kadar null olarak işaretlendiğini ve değişiklik veritabanına kalıcı hale alınamadığı için bir özel durum atıldığını izler.</span><span class="sxs-lookup"><span data-stu-id="fede3-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="fede3-118">Bu, veritabanından bir kısıtlama ihlali almaya benzer.</span><span class="sxs-lookup"><span data-stu-id="fede3-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="fede3-119">Aşağıdaki tablolarda listelenen dört silme davranışı vardır.</span><span class="sxs-lookup"><span data-stu-id="fede3-119">There are four delete behaviors, as listed in the tables below.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="fede3-120">İsteğe bağlı ilişkiler</span><span class="sxs-lookup"><span data-stu-id="fede3-120">Optional relationships</span></span>

<span data-ttu-id="fede3-121">İsteğe bağlı ilişkiler (nullable _is_ yabancı anahtar) için aşağıdaki etkilere neden olan null yabancı anahtar değeri kaydetmek mümkündür:</span><span class="sxs-lookup"><span data-stu-id="fede3-121">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="fede3-122">Davranış Adı</span><span class="sxs-lookup"><span data-stu-id="fede3-122">Behavior Name</span></span>               | <span data-ttu-id="fede3-123">Bellekte bağımlı/alt üzerindeki etkisi</span><span class="sxs-lookup"><span data-stu-id="fede3-123">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="fede3-124">Veritabanındaki bağımlı/alt üzerindeki etkisi</span><span class="sxs-lookup"><span data-stu-id="fede3-124">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="fede3-125">**Cascade**</span><span class="sxs-lookup"><span data-stu-id="fede3-125">**Cascade**</span></span>                 | <span data-ttu-id="fede3-126">Varlıklar silinir</span><span class="sxs-lookup"><span data-stu-id="fede3-126">Entities are deleted</span></span>                   | <span data-ttu-id="fede3-127">Varlıklar silinir</span><span class="sxs-lookup"><span data-stu-id="fede3-127">Entities are deleted</span></span>                   |
| <span data-ttu-id="fede3-128">**ClientSetNull** (Varsayılan)</span><span class="sxs-lookup"><span data-stu-id="fede3-128">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="fede3-129">Yabancı anahtar özellikleri null ayarlanır</span><span class="sxs-lookup"><span data-stu-id="fede3-129">Foreign key properties are set to null</span></span> | <span data-ttu-id="fede3-130">None</span><span class="sxs-lookup"><span data-stu-id="fede3-130">None</span></span>                                   |
| <span data-ttu-id="fede3-131">**Setnull**</span><span class="sxs-lookup"><span data-stu-id="fede3-131">**SetNull**</span></span>                 | <span data-ttu-id="fede3-132">Yabancı anahtar özellikleri null ayarlanır</span><span class="sxs-lookup"><span data-stu-id="fede3-132">Foreign key properties are set to null</span></span> | <span data-ttu-id="fede3-133">Yabancı anahtar özellikleri null ayarlanır</span><span class="sxs-lookup"><span data-stu-id="fede3-133">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="fede3-134">**Kısıtlamak**</span><span class="sxs-lookup"><span data-stu-id="fede3-134">**Restrict**</span></span>                | <span data-ttu-id="fede3-135">None</span><span class="sxs-lookup"><span data-stu-id="fede3-135">None</span></span>                                   | <span data-ttu-id="fede3-136">None</span><span class="sxs-lookup"><span data-stu-id="fede3-136">None</span></span>                                   |

### <a name="required-relationships"></a><span data-ttu-id="fede3-137">Gerekli ilişkiler</span><span class="sxs-lookup"><span data-stu-id="fede3-137">Required relationships</span></span>

<span data-ttu-id="fede3-138">Gerekli ilişkiler (nullable yabancı anahtar) için aşağıdaki etkilere neden olan bir null yabancı anahtar değeri kaydetmek mümkün _değildir:_</span><span class="sxs-lookup"><span data-stu-id="fede3-138">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="fede3-139">Davranış Adı</span><span class="sxs-lookup"><span data-stu-id="fede3-139">Behavior Name</span></span>         | <span data-ttu-id="fede3-140">Bellekte bağımlı/alt üzerindeki etkisi</span><span class="sxs-lookup"><span data-stu-id="fede3-140">Effect on dependent/child in memory</span></span> | <span data-ttu-id="fede3-141">Veritabanındaki bağımlı/alt üzerindeki etkisi</span><span class="sxs-lookup"><span data-stu-id="fede3-141">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="fede3-142">**Basamaklı** (Varsayılan)</span><span class="sxs-lookup"><span data-stu-id="fede3-142">**Cascade** (Default)</span></span> | <span data-ttu-id="fede3-143">Varlıklar silinir</span><span class="sxs-lookup"><span data-stu-id="fede3-143">Entities are deleted</span></span>                | <span data-ttu-id="fede3-144">Varlıklar silinir</span><span class="sxs-lookup"><span data-stu-id="fede3-144">Entities are deleted</span></span>                  |
| <span data-ttu-id="fede3-145">**Müşteri Setnull**</span><span class="sxs-lookup"><span data-stu-id="fede3-145">**ClientSetNull**</span></span>     | <span data-ttu-id="fede3-146">SaveChanges atar</span><span class="sxs-lookup"><span data-stu-id="fede3-146">SaveChanges throws</span></span>                  | <span data-ttu-id="fede3-147">None</span><span class="sxs-lookup"><span data-stu-id="fede3-147">None</span></span>                                  |
| <span data-ttu-id="fede3-148">**Setnull**</span><span class="sxs-lookup"><span data-stu-id="fede3-148">**SetNull**</span></span>           | <span data-ttu-id="fede3-149">SaveChanges atar</span><span class="sxs-lookup"><span data-stu-id="fede3-149">SaveChanges throws</span></span>                  | <span data-ttu-id="fede3-150">SaveChanges atar</span><span class="sxs-lookup"><span data-stu-id="fede3-150">SaveChanges throws</span></span>                    |
| <span data-ttu-id="fede3-151">**Kısıtlamak**</span><span class="sxs-lookup"><span data-stu-id="fede3-151">**Restrict**</span></span>          | <span data-ttu-id="fede3-152">None</span><span class="sxs-lookup"><span data-stu-id="fede3-152">None</span></span>                                | <span data-ttu-id="fede3-153">None</span><span class="sxs-lookup"><span data-stu-id="fede3-153">None</span></span>                                  |

<span data-ttu-id="fede3-154">Yukarıdaki tablolarda, *Hiçbiri* bir kısıtlama ihlaline neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="fede3-154">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="fede3-155">Örneğin, bir asıl/alt varlık silinirse ancak bağımlı/alt çocuğun yabancı anahtarını değiştirmek için herhangi bir işlem yapılmazsa, veritabanı büyük olasılıkla yabancı kısıtlama ihlali nedeniyle SaveChanges'ı atar.</span><span class="sxs-lookup"><span data-stu-id="fede3-155">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="fede3-156">Yüksek düzeyde:</span><span class="sxs-lookup"><span data-stu-id="fede3-156">At a high level:</span></span>

* <span data-ttu-id="fede3-157">Ebeveyni olmadan var olamayan varlıklarınız varsa ve EF'nin çocukları otomatik olarak silmesine özen duymasını istiyorsanız, *Ardından Basamaklı'yı*kullanın.</span><span class="sxs-lookup"><span data-stu-id="fede3-157">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="fede3-158">Üst öğeolmadan var olamayan varlıklar genellikle *basamaklı* varsayılan olan gerekli ilişkilerden yararlar.</span><span class="sxs-lookup"><span data-stu-id="fede3-158">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="fede3-159">Bir ebeveyniniz olabilir veya olmayabilir varlıklar varsa ve EF sizin için yabancı anahtarı iptal dikkat çekmek istiyorsanız, o zaman *ClientSetNull* kullanın</span><span class="sxs-lookup"><span data-stu-id="fede3-159">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="fede3-160">Üst öğesi olmadan var olabilecek varlıklar genellikle *ClientSetNull'un* varsayılan olduğu isteğe bağlı ilişkilerden yararlar.</span><span class="sxs-lookup"><span data-stu-id="fede3-160">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="fede3-161">Veritabanının, alt varlık yüklenmese bile null değerlerini alt yabancı anahtarlara yaymayı da denemesini istiyorsanız, *SetNull'u*kullanın.</span><span class="sxs-lookup"><span data-stu-id="fede3-161">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="fede3-162">Ancak, veritabanının bunu desteklemesi gerektiğini ve veritabanını bu şekilde yapılandırılabilmenizin diğer kısıtlamalara neden olabileceğini ve bunun da uygulamada bu seçeneği genellikle pratik hale getirebileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="fede3-162">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="fede3-163">Bu nedenle *SetNull* varsayılan değildir.</span><span class="sxs-lookup"><span data-stu-id="fede3-163">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="fede3-164">EF Core'un bir varlığı otomatik olarak silmesini veya yabancı anahtarı otomatik olarak iptal etmesini istemiyorsanız, *Kısıtla'yı*kullanın.</span><span class="sxs-lookup"><span data-stu-id="fede3-164">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="fede3-165">Bunun için kodlarınızın alt varlıkları ve yabancı anahtar değerlerini el ile eşitleme de tutmalarını gerektirdiğini unutmayın aksi takdirde kısıtlama özel durumları atılır.</span><span class="sxs-lookup"><span data-stu-id="fede3-165">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="fede3-166">EF Core'da, EF6'dan farklı olarak basamaklı efektler hemen gerçekleşmez, bunun yerine yalnızca SaveChanges çağrıldığında gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="fede3-166">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="fede3-167">**EF Core 2.0'daki değişiklikler:** Önceki *sürümlerde, Kısıtla,* izlenen bağımlı varlıklardaki isteğe bağlı yabancı anahtar özelliklerinin null olarak ayarlanmasına neden olur ve isteğe bağlı ilişkiler için varsayılan silme davranışıdır.</span><span class="sxs-lookup"><span data-stu-id="fede3-167">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="fede3-168">EF Core 2.0'da, *ClientSetNull* bu davranışı temsil etmek üzere tanıtıldı ve isteğe bağlı ilişkiler için varsayılan oldu.</span><span class="sxs-lookup"><span data-stu-id="fede3-168">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="fede3-169">*Kısıtlama* davranışı bağımlı varlıklar üzerinde herhangi bir yan etkisi asla ayarlandı.</span><span class="sxs-lookup"><span data-stu-id="fede3-169">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="fede3-170">Varlık silme örnekleri</span><span class="sxs-lookup"><span data-stu-id="fede3-170">Entity deletion examples</span></span>

<span data-ttu-id="fede3-171">Aşağıdaki kod, karşıdan yüklenebilecek ve çalıştırılabilen bir [örneğin](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="fede3-171">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="fede3-172">Örnek, bir üst varlık silindiğinde hem isteğe bağlı hem de gerekli ilişkiler için her silme davranışı için neler olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="fede3-172">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="fede3-173">Neler olduğunu anlamak için her varyasyonu gözden geçirelim.</span><span class="sxs-lookup"><span data-stu-id="fede3-173">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="fede3-174">DeleteBehavior.Cascade gerekli veya isteğe bağlı ilişki ile</span><span class="sxs-lookup"><span data-stu-id="fede3-174">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="fede3-175">Blog Silinmiş olarak işaretlenir</span><span class="sxs-lookup"><span data-stu-id="fede3-175">Blog is marked as Deleted</span></span>
* <span data-ttu-id="fede3-176">Basamaklı kaydetmeler SaveChanges kadar gerçekleşmediğinden gönderiler başlangıçta değişmeden kalır</span><span class="sxs-lookup"><span data-stu-id="fede3-176">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="fede3-177">SaveChanges hem bağımlılar/çocuklar (gönderiler) hem de ana/üst (blog) için silme gönderir</span><span class="sxs-lookup"><span data-stu-id="fede3-177">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="fede3-178">Kaydedildikten sonra, veritabanından silindikleri için tüm varlıklar ayrılır</span><span class="sxs-lookup"><span data-stu-id="fede3-178">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="fede3-179">DeleteBehavior.ClientSetNull veya DeleteBehavior.SetNull gerekli ilişki ile</span><span class="sxs-lookup"><span data-stu-id="fede3-179">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="fede3-180">Blog Silinmiş olarak işaretlenir</span><span class="sxs-lookup"><span data-stu-id="fede3-180">Blog is marked as Deleted</span></span>
* <span data-ttu-id="fede3-181">Basamaklı kaydetmeler SaveChanges kadar gerçekleşmediğinden gönderiler başlangıçta değişmeden kalır</span><span class="sxs-lookup"><span data-stu-id="fede3-181">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="fede3-182">SaveChanges, FK gönderisini null'a ayarlamaya çalışır, ancak FK geçersiz olmadığı için bu başarısız olur</span><span class="sxs-lookup"><span data-stu-id="fede3-182">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="fede3-183">DeleteBehavior.ClientSetNull veya DeleteBehavior.SetNull isteğe bağlı ilişki ile</span><span class="sxs-lookup"><span data-stu-id="fede3-183">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="fede3-184">Blog Silinmiş olarak işaretlenir</span><span class="sxs-lookup"><span data-stu-id="fede3-184">Blog is marked as Deleted</span></span>
* <span data-ttu-id="fede3-185">Basamaklı kaydetmeler SaveChanges kadar gerçekleşmediğinden gönderiler başlangıçta değişmeden kalır</span><span class="sxs-lookup"><span data-stu-id="fede3-185">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="fede3-186">SaveChanges girişimleri, asıl/üst öğeyi (blog) silmeden önce her iki bağımlının/çocuğun (gönderilerin) FK'sını geçersiz kılacak şekilde ayarlar</span><span class="sxs-lookup"><span data-stu-id="fede3-186">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="fede3-187">Kaydedildikten sonra, asıl/üst (blog) silinir, ancak bağımlılar/çocuklar (gönderiler) hala izlenir</span><span class="sxs-lookup"><span data-stu-id="fede3-187">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="fede3-188">İzlenen bağımlılar/çocuklar (gönderiler) artık null FK değerlerine sahiptir ve silinen asıl/üst öğeye (blog) göndermeleri kaldırılmıştır</span><span class="sxs-lookup"><span data-stu-id="fede3-188">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="fede3-189">DeleteBehavior.Restrict gerekli veya isteğe bağlı ilişki ile</span><span class="sxs-lookup"><span data-stu-id="fede3-189">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="fede3-190">Blog Silinmiş olarak işaretlenir</span><span class="sxs-lookup"><span data-stu-id="fede3-190">Blog is marked as Deleted</span></span>
* <span data-ttu-id="fede3-191">Basamaklı kaydetmeler SaveChanges kadar gerçekleşmediğinden gönderiler başlangıçta değişmeden kalır</span><span class="sxs-lookup"><span data-stu-id="fede3-191">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="fede3-192">*Kısıtlama,* EF'ye FK'yı otomatik olarak null olarak ayarlamamasını söylediğinden, geçersiz kalır ve SaveChanges kaydetmeden atar</span><span class="sxs-lookup"><span data-stu-id="fede3-192">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="fede3-193">Yetim örneklerini silme</span><span class="sxs-lookup"><span data-stu-id="fede3-193">Delete orphans examples</span></span>

<span data-ttu-id="fede3-194">Aşağıdaki kod, karşıdan yüklenebilecek ve çalıştırılabilen bir [örneğin](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="fede3-194">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="fede3-195">Örnek, bir üst/asıl ve alt/alt öğesi arasındaki ilişki koptuğunda, hem isteğe bağlı hem de gerekli ilişkiler için her silme davranışı için neler olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="fede3-195">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="fede3-196">Bu örnekte, ilişki, asıl/üst öğedeki (blog) koleksiyon gezinti özelliğinden bağımlı/alt (gönderiler) kaldırılarak kesilir.</span><span class="sxs-lookup"><span data-stu-id="fede3-196">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="fede3-197">Ancak, bağımlı/alttan alt öğeye/üst öğeye yapılan başvuru iptal edilirse davranış aynıdır.</span><span class="sxs-lookup"><span data-stu-id="fede3-197">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="fede3-198">Neler olduğunu anlamak için her varyasyonu gözden geçirelim.</span><span class="sxs-lookup"><span data-stu-id="fede3-198">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="fede3-199">DeleteBehavior.Cascade gerekli veya isteğe bağlı ilişki ile</span><span class="sxs-lookup"><span data-stu-id="fede3-199">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="fede3-200">İlişkiyi kesme fk'nın null olarak işaretlenilmesine neden olduğundan gönderiler Değiştirilmiştir</span><span class="sxs-lookup"><span data-stu-id="fede3-200">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="fede3-201">FK nullable değilse, o zaman gerçek değeri null olarak işaretlenmiş olsa bile değişmez</span><span class="sxs-lookup"><span data-stu-id="fede3-201">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="fede3-202">SaveChanges bağımlılar/çocuklar (gönderiler) için silme gönderir</span><span class="sxs-lookup"><span data-stu-id="fede3-202">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="fede3-203">Tasarruf tan sonra, bağımlılar/çocuklar (gönderiler) artık veritabanından silindiğinden ayrılır</span><span class="sxs-lookup"><span data-stu-id="fede3-203">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="fede3-204">DeleteBehavior.ClientSetNull veya DeleteBehavior.SetNull gerekli ilişki ile</span><span class="sxs-lookup"><span data-stu-id="fede3-204">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="fede3-205">İlişkiyi kesme fk'nın null olarak işaretlenilmesine neden olduğundan gönderiler Değiştirilmiştir</span><span class="sxs-lookup"><span data-stu-id="fede3-205">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="fede3-206">FK nullable değilse, o zaman gerçek değeri null olarak işaretlenmiş olsa bile değişmez</span><span class="sxs-lookup"><span data-stu-id="fede3-206">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="fede3-207">SaveChanges, FK gönderisini null'a ayarlamaya çalışır, ancak FK geçersiz olmadığı için bu başarısız olur</span><span class="sxs-lookup"><span data-stu-id="fede3-207">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="fede3-208">DeleteBehavior.ClientSetNull veya DeleteBehavior.SetNull isteğe bağlı ilişki ile</span><span class="sxs-lookup"><span data-stu-id="fede3-208">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="fede3-209">İlişkiyi kesme fk'nın null olarak işaretlenilmesine neden olduğundan gönderiler Değiştirilmiştir</span><span class="sxs-lookup"><span data-stu-id="fede3-209">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="fede3-210">FK nullable değilse, o zaman gerçek değeri null olarak işaretlenmiş olsa bile değişmez</span><span class="sxs-lookup"><span data-stu-id="fede3-210">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="fede3-211">SaveChanges, her iki bağımlının/çocuğun (posta) FK'sını null ayarlar</span><span class="sxs-lookup"><span data-stu-id="fede3-211">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="fede3-212">Tasarruf tan sonra, bağımlılar/çocuklar (gönderiler) artık null FK değerlerine sahiptir ve silinen asıl/üst öğeye (blog) göndermeleri kaldırılmıştır</span><span class="sxs-lookup"><span data-stu-id="fede3-212">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="fede3-213">DeleteBehavior.Restrict gerekli veya isteğe bağlı ilişki ile</span><span class="sxs-lookup"><span data-stu-id="fede3-213">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="fede3-214">İlişkiyi kesme fk'nın null olarak işaretlenilmesine neden olduğundan gönderiler Değiştirilmiştir</span><span class="sxs-lookup"><span data-stu-id="fede3-214">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="fede3-215">FK nullable değilse, o zaman gerçek değeri null olarak işaretlenmiş olsa bile değişmez</span><span class="sxs-lookup"><span data-stu-id="fede3-215">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="fede3-216">*Kısıtlama,* EF'ye FK'yı otomatik olarak null olarak ayarlamamasını söylediğinden, geçersiz kalır ve SaveChanges kaydetmeden atar</span><span class="sxs-lookup"><span data-stu-id="fede3-216">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="fede3-217">İzlenmemiş varlıklara basamaklama</span><span class="sxs-lookup"><span data-stu-id="fede3-217">Cascading to untracked entities</span></span>

<span data-ttu-id="fede3-218">*SaveChanges'ı*aradiğinizde, basamaklı silme kuralları bağlam tarafından izlenen tüm varlıklara uygulanır.</span><span class="sxs-lookup"><span data-stu-id="fede3-218">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="fede3-219">Bu, yukarıda gösterilen tüm örneklerdeki durumdur, bu nedenle SQL hem asıl/üst (blog) hem de tüm bağımlıları/alt ları (gönderileri) silmek için oluşturulmuştur:</span><span class="sxs-lookup"><span data-stu-id="fede3-219">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="fede3-220">Yalnızca anapara yüklenirse (örneğin, gönderileri eklemeden `Include(b => b.Posts)` bir blog için sorgu yapılırsa- SaveChanges yalnızca asıl/üst öğeyi silmek için SQL oluşturur:</span><span class="sxs-lookup"><span data-stu-id="fede3-220">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="fede3-221">Bağımlılar/alt (gönderiler) yalnızca veritabanında ilgili basamaklı davranış yapılandırılırsa silinir.</span><span class="sxs-lookup"><span data-stu-id="fede3-221">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="fede3-222">Veritabanını oluşturmak için EF kullanırsanız, bu basamaklı davranış sizin için kurulum olacaktır.</span><span class="sxs-lookup"><span data-stu-id="fede3-222">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
