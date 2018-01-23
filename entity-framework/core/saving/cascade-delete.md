---
title: "Basamaklı silme - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: e1cb194d7c7472af59eb44fe2a084fa16c40c186
ms.sourcegitcommit: 3b21a7fdeddc7b3c70d9b7777b72bef61f59216c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
---
# <a name="cascade-delete"></a><span data-ttu-id="cfa7f-102">Cascade Delete</span><span class="sxs-lookup"><span data-stu-id="cfa7f-102">Cascade Delete</span></span>

<span data-ttu-id="cfa7f-103">Art arda silme ilişkili satırları silme işlemini otomatik olarak tetikleyecek bir satırın silme işlemine izin veren bir özellik açıklamak için Veritabanı terminolojisinde yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="cfa7f-104">Ayrıca EF çekirdek delete davranışları tarafından kapsanan bir yakından ilgili ilişki üst olduğunda alt varlık otomatik olarak silinmesini zarar görmesi--genellikle "artık silme" olarak bilinen bu i kavramdır.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this i commonly known as "deleting orphans".</span></span>

<span data-ttu-id="cfa7f-105">EF çekirdek birkaç farklı silme davranışı uygular ve tek tek ilişkileri Sil davranışlarını yapılandırmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="cfa7f-106">EF çekirdek de otomatik olarak yararlı varsayılan silme davranışlar [requiredness ilişkinin üzerinde] tabanlı her ilişki için yapılandırma kuralları uygular (../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="cfa7f-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship] (../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="cfa7f-107">Davranışları Sil</span><span class="sxs-lookup"><span data-stu-id="cfa7f-107">Delete behaviors</span></span>
<span data-ttu-id="cfa7f-108">Silme davranışları tanımlanmış *DeleteBehavior* Numaralandırıcı yazın ve için geçirilen *OnDelete* denetlemek için fluent API olup olmadığını silinmesi asıl/üst varlık veya, severing bağımlı/alt varlıkları ilişkisi bağımlı/alt varlıklarını bir yan etkisi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="cfa7f-109">EF asıl/üst varlık silinmiş veya alt ilişkisi zarar görmesi üç eylemleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="cfa7f-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="cfa7f-110">Alt/bağımlı silinebilir</span><span class="sxs-lookup"><span data-stu-id="cfa7f-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="cfa7f-111">Çocuğunuzun yabancı anahtar değerleri ayarlanabilir null</span><span class="sxs-lookup"><span data-stu-id="cfa7f-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="cfa7f-112">Alt değişmeden kalır</span><span class="sxs-lookup"><span data-stu-id="cfa7f-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="cfa7f-113">EF çekirdek modelinde yapılandırılmış silme davranışı, yalnızca asıl varlık EF çekirdek kullanarak silinir ve bağımlı varlıkları (yani için izlenen bağımlıları) bellekte yüklenen uygulanır.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (i.e. for tracked dependents).</span></span> <span data-ttu-id="cfa7f-114">Karşılık gelen bir cascade davranış bağlamdan izlenmiyor veri emin olmak için veritabanını kurulumunda uygulanan gerekli eyleme sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="cfa7f-115">Veritabanını oluşturmak için EF çekirdek kullanırsanız, bu cascade davranış Kurulumu olacaktır.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="cfa7f-116">Bir yabancı anahtar değeri null olarak ayarlama özelliği Yukarıdaki ikinci eylemi için yabancı anahtar null değilse, geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="cfa7f-117">(Null bir yabancı anahtar için gerekli bir ilişki eşdeğerdir.) Bu durumlarda, değişikliği veritabanına kalıcı olamayacağından aynı zamanda bir özel durum SaveChanges çağrılıncaya kadar yabancı anahtar özelliği null işaretlenmiş olduğundan EF çekirdek izler.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="cfa7f-118">Bu, bir kısıtlama ihlali veritabanından alma için benzer.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="cfa7f-119">Dört silme davranışı, aşağıdaki tablolarda listelenen gibi vardır.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-119">There are four delete behaviors, as listed in the tables below.</span></span> <span data-ttu-id="cfa7f-120">İsteğe bağlı ilişkiler (boş değer atanabilir yabancı anahtar), _olan_ olası, aşağıdaki efektler sonuçları bir null yabancı anahtar değeri kaydetmek:</span><span class="sxs-lookup"><span data-stu-id="cfa7f-120">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="cfa7f-121">Davranış adı</span><span class="sxs-lookup"><span data-stu-id="cfa7f-121">Behavior Name</span></span> | <span data-ttu-id="cfa7f-122">Bağımlı/alt bellekte etkisi</span><span class="sxs-lookup"><span data-stu-id="cfa7f-122">Effect on dependent/child in memory</span></span> | <span data-ttu-id="cfa7f-123">Bağımlı/alt veritabanı üzerinde etkisi</span><span class="sxs-lookup"><span data-stu-id="cfa7f-123">Effect on dependent/child in database</span></span>
|-|-|-
| <span data-ttu-id="cfa7f-124">**CASCADE**</span><span class="sxs-lookup"><span data-stu-id="cfa7f-124">**Cascade**</span></span> | <span data-ttu-id="cfa7f-125">Varlıkları silinir</span><span class="sxs-lookup"><span data-stu-id="cfa7f-125">Entities are deleted</span></span> | <span data-ttu-id="cfa7f-126">Varlıkları silinir</span><span class="sxs-lookup"><span data-stu-id="cfa7f-126">Entities are deleted</span></span>
| <span data-ttu-id="cfa7f-127">**ClientSetNull** (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="cfa7f-127">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="cfa7f-128">Yabancı anahtar özellikleri null</span><span class="sxs-lookup"><span data-stu-id="cfa7f-128">Foreign key properties are set to null</span></span> | <span data-ttu-id="cfa7f-129">Yok.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-129">None</span></span>
| <span data-ttu-id="cfa7f-130">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="cfa7f-130">**SetNull**</span></span> | <span data-ttu-id="cfa7f-131">Yabancı anahtar özellikleri null</span><span class="sxs-lookup"><span data-stu-id="cfa7f-131">Foreign key properties are set to null</span></span> | <span data-ttu-id="cfa7f-132">Yabancı anahtar özellikleri null</span><span class="sxs-lookup"><span data-stu-id="cfa7f-132">Foreign key properties are set to null</span></span>
| <span data-ttu-id="cfa7f-133">**Kısıtlama**</span><span class="sxs-lookup"><span data-stu-id="cfa7f-133">**Restrict**</span></span> | <span data-ttu-id="cfa7f-134">Yok.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-134">None</span></span> | <span data-ttu-id="cfa7f-135">Yok.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-135">None</span></span>

<span data-ttu-id="cfa7f-136">Gerekli ilişkileri (null yabancı anahtar) olduğu _değil_ olası, aşağıdaki efektler sonuçları bir null yabancı anahtar değeri kaydetmek:</span><span class="sxs-lookup"><span data-stu-id="cfa7f-136">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="cfa7f-137">Davranış adı</span><span class="sxs-lookup"><span data-stu-id="cfa7f-137">Behavior Name</span></span> | <span data-ttu-id="cfa7f-138">Bağımlı/alt bellekte etkisi</span><span class="sxs-lookup"><span data-stu-id="cfa7f-138">Effect on dependent/child in memory</span></span> | <span data-ttu-id="cfa7f-139">Bağımlı/alt veritabanı üzerinde etkisi</span><span class="sxs-lookup"><span data-stu-id="cfa7f-139">Effect on dependent/child in database</span></span>
|-|-|-
| <span data-ttu-id="cfa7f-140">**CASCADE** (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="cfa7f-140">**Cascade** (Default)</span></span> | <span data-ttu-id="cfa7f-141">Varlıkları silinir</span><span class="sxs-lookup"><span data-stu-id="cfa7f-141">Entities are deleted</span></span> | <span data-ttu-id="cfa7f-142">Varlıkları silinir</span><span class="sxs-lookup"><span data-stu-id="cfa7f-142">Entities are deleted</span></span>
| <span data-ttu-id="cfa7f-143">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="cfa7f-143">**ClientSetNull**</span></span> | <span data-ttu-id="cfa7f-144">SaveChanges oluşturur</span><span class="sxs-lookup"><span data-stu-id="cfa7f-144">SaveChanges throws</span></span> | <span data-ttu-id="cfa7f-145">Yok.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-145">None</span></span>
| <span data-ttu-id="cfa7f-146">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="cfa7f-146">**SetNull**</span></span> | <span data-ttu-id="cfa7f-147">SaveChanges oluşturur</span><span class="sxs-lookup"><span data-stu-id="cfa7f-147">SaveChanges throws</span></span> | <span data-ttu-id="cfa7f-148">SaveChanges oluşturur</span><span class="sxs-lookup"><span data-stu-id="cfa7f-148">SaveChanges throws</span></span>
| <span data-ttu-id="cfa7f-149">**Kısıtlama**</span><span class="sxs-lookup"><span data-stu-id="cfa7f-149">**Restrict**</span></span> | <span data-ttu-id="cfa7f-150">Yok.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-150">None</span></span> | <span data-ttu-id="cfa7f-151">Yok.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-151">None</span></span>

<span data-ttu-id="cfa7f-152">Yukarıdaki tablolarda *hiçbiri* bir kısıtlama ihlali neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-152">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="cfa7f-153">Örneğin, bir asıl/alt varlık silinir, ancak bağımlı/alt yabancı anahtarı değiştirmek için hiçbir işlem yapılmadı, sonra veritabanını olasılıkla SaveChanges üzerinde bir yabancı kısıtlaması ihlali nedeniyle durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-153">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="cfa7f-154">Yüksek düzeyde:</span><span class="sxs-lookup"><span data-stu-id="cfa7f-154">At a high level:</span></span>
* <span data-ttu-id="cfa7f-155">Bir üst var olamaz varlıklar varsa ve alt öğelerini otomatik olarak silmek için halletmeniz için EF istediğiniz sonra kullanın *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-155">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="cfa7f-156">Bir üst genellikle olun olmadan var olamaz varlıklar için gerekli ilişkilerini, kullandığınız *Cascade* varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-156">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="cfa7f-157">Bir üst öğeye sahip değil veya varlıklar varsa ve yabancı anahtar sizin için nulling halletmeniz için EF istediğiniz sonra kullanın *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="cfa7f-157">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="cfa7f-158">Bir üst genellikle olun olmadan bulunabilir varlıklar için isteğe bağlı ilişkilerini, kullandığınız *ClientSetNull* varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-158">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="cfa7f-159">Alt yabancı anahtarlar için null değerler bile yaymak de denemek için veritabanı isterseniz, alt varlık yüklenmedi, ardından *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-159">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="cfa7f-160">Ancak, veritabanı bu desteklemesi gerekir ve bu gibi veritabanı yapılandırma diğer kısıtlamaları neden olabilir, uygulamada genellikle bu seçenek pratik hale getirir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-160">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="cfa7f-161">Nedeni budur *SetNull* varsayılan olarak etkin değildir.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-161">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="cfa7f-162">EF çekirdeğini herhangi bir varlık otomatik olarak silin veya otomatik olarak yabancı anahtar null sonra kullanmak istemiyorsanız, *sınırla*.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-162">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="cfa7f-163">Bu kodunuzu alt varlıkları ve yabancı anahtar değerleri eşitleme el ile tutmanızı gerektirdiğini unutmayın, aksi takdirde kısıtlaması özel durumlar oluşturulacaktır.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-163">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="cfa7f-164">EF çekirdek, EF6, aksine, geçişli etkileri hemen, ancak bunun yerine yalnızca SaveChanges zaman çağrıldığını durum değil.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-164">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="cfa7f-165">**EF çekirdek 2.0 değişiklikleri:** önceki sürümlerde *sınırla* isteğe bağlı yabancı anahtar özelliklerini ayarlamak için izlenen bağımlı varlıklarında neden olur null ve varsayılan davranışı isteğe bağlı ilişkileri silin oluştu.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-165">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="cfa7f-166">EF çekirdek 2. 0'da, *ClientSetNull* bu davranışı göstermek için sunulmuştur ve isteğe bağlı ilişkiler için varsayılan hale geldi.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-166">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="cfa7f-167">Davranışını *sınırla* hiçbir zaman bağımlı varlıklarını herhangi yan etkileri için ayarlanmış.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-167">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="cfa7f-168">Varlık silme örnekleri</span><span class="sxs-lookup"><span data-stu-id="cfa7f-168">Entity deletion examples</span></span>

<span data-ttu-id="cfa7f-169">Aşağıdaki kod parçası olan bir [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , indirilebilen ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-169">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="cfa7f-170">Örnek bir üst varlık silindiğinde her silme davranışı isteğe bağlıdır ve gerekli ilişkiler için ne olacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-170">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="cfa7f-171">Şimdi neler olduğunu anlamak için her değişim yol.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-171">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="cfa7f-172">Gerekli veya isteğe bağlı bir ilişki ile DeleteBehavior.Cascade</span><span class="sxs-lookup"><span data-stu-id="cfa7f-172">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="cfa7f-173">Blog silinmiş işaretlenmiş</span><span class="sxs-lookup"><span data-stu-id="cfa7f-173">Blog is marked as Deleted</span></span>
* <span data-ttu-id="cfa7f-174">Basamaklar SaveChanges kadar durum değil bu yana gönderileri Unchanged başlangıçta kalır.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-174">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="cfa7f-175">Silme işlemi için hem bağımlıları/alt öğe (postaları) ve ardından asıl/üst (blog) SaveChanges gönderir</span><span class="sxs-lookup"><span data-stu-id="cfa7f-175">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="cfa7f-176">Şimdi veritabanından silinmiş olduğundan kaydettikten sonra tüm varlıklar ayrılmış</span><span class="sxs-lookup"><span data-stu-id="cfa7f-176">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="cfa7f-177">DeleteBehavior.ClientSetNull ya da gerekli bir ilişki ile DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="cfa7f-177">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="cfa7f-178">Blog silinmiş işaretlenmiş</span><span class="sxs-lookup"><span data-stu-id="cfa7f-178">Blog is marked as Deleted</span></span>
* <span data-ttu-id="cfa7f-179">Basamaklar SaveChanges kadar durum değil bu yana gönderileri Unchanged başlangıçta kalır.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-179">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="cfa7f-180">SaveChanges FK post null değerine ayarlamaya çalışır, ancak FK boş değer atanabilir olmadığından bu başarısız</span><span class="sxs-lookup"><span data-stu-id="cfa7f-180">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="cfa7f-181">DeleteBehavior.ClientSetNull veya isteğe bağlı bir ilişki ile DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="cfa7f-181">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="cfa7f-182">Blog silinmiş işaretlenmiş</span><span class="sxs-lookup"><span data-stu-id="cfa7f-182">Blog is marked as Deleted</span></span>
* <span data-ttu-id="cfa7f-183">Basamaklar SaveChanges kadar durum değil bu yana gönderileri Unchanged başlangıçta kalır.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-183">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="cfa7f-184">SaveChanges denemeleri ayarlar her iki bağımlıları/alt (postaları) FK null asıl/üst (blog) silmeden önce</span><span class="sxs-lookup"><span data-stu-id="cfa7f-184">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="cfa7f-185">Kaydetme sonra asıl/üst (blog) silinir, ancak bağımlıları/alt (postaları) hala izlenir</span><span class="sxs-lookup"><span data-stu-id="cfa7f-185">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="cfa7f-186">İzlenen bağımlıları/alt (postaları) şimdi null FK değerlere sahip ve bunların silinen asıl/üst (blog) referansı kaldırılmıştır</span><span class="sxs-lookup"><span data-stu-id="cfa7f-186">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="cfa7f-187">Gerekli veya isteğe bağlı bir ilişki ile DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="cfa7f-187">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="cfa7f-188">Blog silinmiş işaretlenmiş</span><span class="sxs-lookup"><span data-stu-id="cfa7f-188">Blog is marked as Deleted</span></span>
* <span data-ttu-id="cfa7f-189">Basamaklar SaveChanges kadar durum değil bu yana gönderileri Unchanged başlangıçta kalır.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-189">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="cfa7f-190">Bu yana *sınırla* otomatik olarak FK null olarak ayarlamak için EF söyler null olmayan kalır ve SaveChanges kaydetmeden oluşturur</span><span class="sxs-lookup"><span data-stu-id="cfa7f-190">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="cfa7f-191">Artık örnekler Sil</span><span class="sxs-lookup"><span data-stu-id="cfa7f-191">Delete orphans examples</span></span>

<span data-ttu-id="cfa7f-192">Aşağıdaki kod parçası olan bir [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) bir çalışma indirilebilir.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-192">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="cfa7f-193">Örnek bir üst/asıl ve kendi alt öğelerini/dependents arasındaki ilişkiyi zarar görmesi halinde her silme davranışı isteğe bağlıdır ve gerekli ilişkiler için ne olacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-193">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="cfa7f-194">Bu örnekte, koleksiyon gezinti özelliği asıl/üst (blog) bağımlıları/alt (postaları) kaldırarak ilişki zarar görmesi.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-194">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="cfa7f-195">Bağımlı/alt başvurusundan asıl/üst için bunun yerine çıkışı nulled ancak davranışı aynı olur.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-195">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="cfa7f-196">Şimdi neler olduğunu anlamak için her değişim yol.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-196">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="cfa7f-197">Gerekli veya isteğe bağlı bir ilişki ile DeleteBehavior.Cascade</span><span class="sxs-lookup"><span data-stu-id="cfa7f-197">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="cfa7f-198">İlişki severing boş olarak işaretlenecek FK neden olduğundan gönderileri değiştirilen işaretlenmiş</span><span class="sxs-lookup"><span data-stu-id="cfa7f-198">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="cfa7f-199">FK null değilse, boş olarak işaretlenmiş olsa bile sonra gerçek değer değişmez</span><span class="sxs-lookup"><span data-stu-id="cfa7f-199">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="cfa7f-200">Silme işlemi için bağımlılıklar/alt öğe (postaları) SaveChanges gönderir</span><span class="sxs-lookup"><span data-stu-id="cfa7f-200">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="cfa7f-201">Şimdi veritabanından silinmiş olduğundan kaydettikten sonra bağımlılıklar/alt (postaları) ayrılmış</span><span class="sxs-lookup"><span data-stu-id="cfa7f-201">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="cfa7f-202">DeleteBehavior.ClientSetNull ya da gerekli bir ilişki ile DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="cfa7f-202">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="cfa7f-203">İlişki severing boş olarak işaretlenecek FK neden olduğundan gönderileri değiştirilen işaretlenmiş</span><span class="sxs-lookup"><span data-stu-id="cfa7f-203">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="cfa7f-204">FK null değilse, boş olarak işaretlenmiş olsa bile sonra gerçek değer değişmez</span><span class="sxs-lookup"><span data-stu-id="cfa7f-204">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="cfa7f-205">SaveChanges FK post null değerine ayarlamaya çalışır, ancak FK boş değer atanabilir olmadığından bu başarısız</span><span class="sxs-lookup"><span data-stu-id="cfa7f-205">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="cfa7f-206">DeleteBehavior.ClientSetNull veya isteğe bağlı bir ilişki ile DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="cfa7f-206">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="cfa7f-207">İlişki severing boş olarak işaretlenecek FK neden olduğundan gönderileri değiştirilen işaretlenmiş</span><span class="sxs-lookup"><span data-stu-id="cfa7f-207">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="cfa7f-208">FK null değilse, boş olarak işaretlenmiş olsa bile sonra gerçek değer değişmez</span><span class="sxs-lookup"><span data-stu-id="cfa7f-208">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="cfa7f-209">Her iki bağımlıları/alt (postaları) FK SaveChanges null olarak ayarlar</span><span class="sxs-lookup"><span data-stu-id="cfa7f-209">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="cfa7f-210">Kaydetme sonra bağımlıları/alt (postaları) şimdi null FK değerlere sahip ve bunların silinen asıl/üst (blog) referansı kaldırılmıştır</span><span class="sxs-lookup"><span data-stu-id="cfa7f-210">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="cfa7f-211">Gerekli veya isteğe bağlı bir ilişki ile DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="cfa7f-211">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="cfa7f-212">İlişki severing boş olarak işaretlenecek FK neden olduğundan gönderileri değiştirilen işaretlenmiş</span><span class="sxs-lookup"><span data-stu-id="cfa7f-212">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="cfa7f-213">FK null değilse, boş olarak işaretlenmiş olsa bile sonra gerçek değer değişmez</span><span class="sxs-lookup"><span data-stu-id="cfa7f-213">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="cfa7f-214">Bu yana *sınırla* otomatik olarak FK null olarak ayarlamak için EF söyler null olmayan kalır ve SaveChanges kaydetmeden oluşturur</span><span class="sxs-lookup"><span data-stu-id="cfa7f-214">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="cfa7f-215">İzlenmeyen varlıklara geçişli</span><span class="sxs-lookup"><span data-stu-id="cfa7f-215">Cascading to untracked entities</span></span>

<span data-ttu-id="cfa7f-216">Çağırdığınızda *SaveChanges*, bağlam tarafından izlenen herhangi bir varlık cascade delete kuralları uygulanır.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-216">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="cfa7f-217">Bu tüm durumdur yukarıda gösterilen örnek olduğu SQL asıl/üst (blog) ve tüm bağımlıları/alt (postaları) silmek için oluşturulan neden:</span><span class="sxs-lookup"><span data-stu-id="cfa7f-217">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="cfa7f-218">Asıl yüklenen--yalnızca örneğin, ne zaman bir sorgu için yapılan bir blog bir `Include(b => b.Posts)` gönderileri--de içerecek şekilde sonra SaveChanges yalnızca asıl/üst silmek için SQL oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cfa7f-218">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="cfa7f-219">Veritabanında yapılandırılan karşılık gelen bir cascade davranışına varsa bağımlıları/alt (postaları) yalnızca silinir.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-219">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="cfa7f-220">Veritabanını oluşturmak için EF kullanırsanız, bu cascade davranış Kurulumu olacaktır.</span><span class="sxs-lookup"><span data-stu-id="cfa7f-220">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
