---
title: Basamaklı silme - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: 15b7e69676ef9aeb70121fcec404c34a17e5e2bb
ms.sourcegitcommit: 8d04a2ad98036f32ca70c77ce3040c5edb1cdf82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44384845"
---
# <a name="cascade-delete"></a><span data-ttu-id="917af-102">Art arda silme</span><span class="sxs-lookup"><span data-stu-id="917af-102">Cascade Delete</span></span>

<span data-ttu-id="917af-103">Art arda silme, Veritabanı terminolojisinde otomatik olarak ilişkili satırları silme işlemi tetiklemek için bir satır silinmesini sağlayan bir özellik tanımlamak için yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="917af-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="917af-104">Onun üst ilişkisi yazıyordunuz--ayrıca EF Core silme davranışları tarafından kapsanan yakından ilgili bir kavram alt varlık otomatik olarak silinmesini olduğunda bu genellikle "artık silinmesi" adı verilir.</span><span class="sxs-lookup"><span data-stu-id="917af-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="917af-105">EF Core, birkaç farklı silme davranışları uygular ve tek tek ilişkileri silme davranışlarını yapılandırmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="917af-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="917af-106">EF Core de otomatik olarak yararlı varsayılan silme davranışları göre her ilişki için yapılandırma kuralları uygular [requiredness ilişkinin](../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="917af-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="917af-107">Davranışlar Sil</span><span class="sxs-lookup"><span data-stu-id="917af-107">Delete behaviors</span></span>
<span data-ttu-id="917af-108">Silme davranışları tanımlanmış *DeleteBehavior* Numaralandırıcı yazın ve geçirilebilir *OnDelete* denetimine fluent API'si olup olmadığını sorumlusu/üst varlık veya, severing silinmesini İlişki bağımlı/alt varlıklar için bir yan etkisi bağımlı/alt varlıklar üzerinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="917af-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="917af-109">EF sorumlusu/üst varlık silinmiş ya da alt ilişkinin yazıyordunuz üç eylemleri vardır:</span><span class="sxs-lookup"><span data-stu-id="917af-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="917af-110">Alt/bağımlı silinebilir</span><span class="sxs-lookup"><span data-stu-id="917af-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="917af-111">Çocuğun yabancı anahtar değerler olarak ayarlanabilir null</span><span class="sxs-lookup"><span data-stu-id="917af-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="917af-112">Alt değişmeden kalır.</span><span class="sxs-lookup"><span data-stu-id="917af-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="917af-113">EF Core modelinde yapılandırılmış silme davranışı, yalnızca birincil varlık EF Core kullanarak silinir ve bağımlı varlıkları belleğe yüklenen uygulanır (yani, izlenen bağımlılıklar için).</span><span class="sxs-lookup"><span data-stu-id="917af-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (that is, for tracked dependents).</span></span> <span data-ttu-id="917af-114">Karşılık gelen bir cascade davranış bağlam tarafından izlenmiyor veri emin olmak için Veritabanı Kurulumu uygulanan gerekli eylem sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="917af-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="917af-115">EF Core veritabanını oluşturmak için kullanıyorsanız, bu cascade davranışı için Kurulum olacaktır.</span><span class="sxs-lookup"><span data-stu-id="917af-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="917af-116">Bir yabancı anahtar değeri null olarak ayarlama özelliği Yukarıdaki ikinci eylem için yabancı anahtar null değilse geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="917af-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="917af-117">(Atanamaz yabancı anahtar gerekli bir ilişki eşdeğerdir.) Bu gibi durumlarda, değişiklik veritabanına kalıcı olamayacağından, aynı zamanda bir özel durum SaveChanges çağrılana kadar yabancı anahtar özelliği null işaretlenmiş olduğundan, EF Core izler.</span><span class="sxs-lookup"><span data-stu-id="917af-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="917af-118">Bu, bir kısıtlama ihlali veritabanından alma için benzer.</span><span class="sxs-lookup"><span data-stu-id="917af-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="917af-119">Dört Sil davranışları, aşağıdaki tablolarda listelenen gibi vardır.</span><span class="sxs-lookup"><span data-stu-id="917af-119">There are four delete behaviors, as listed in the tables below.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="917af-120">İsteğe bağlı bir ilişki</span><span class="sxs-lookup"><span data-stu-id="917af-120">Optional relationships</span></span>
<span data-ttu-id="917af-121">İsteğe bağlı ilişkiler (boş değer atanabilir yabancı anahtar) için bunu _olduğu_ olası aşağıdaki efektler sonuçlanır bir yabancı anahtar null ve kaydetmek:</span><span class="sxs-lookup"><span data-stu-id="917af-121">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="917af-122">Davranış adı</span><span class="sxs-lookup"><span data-stu-id="917af-122">Behavior Name</span></span>               | <span data-ttu-id="917af-123">Bağımlı/alt bellekte etkisi</span><span class="sxs-lookup"><span data-stu-id="917af-123">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="917af-124">Bağımlı/alt veritabanı üzerindeki etkisi</span><span class="sxs-lookup"><span data-stu-id="917af-124">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="917af-125">**Basamakla**</span><span class="sxs-lookup"><span data-stu-id="917af-125">**Cascade**</span></span>                 | <span data-ttu-id="917af-126">Varlıkları silindi</span><span class="sxs-lookup"><span data-stu-id="917af-126">Entities are deleted</span></span>                   | <span data-ttu-id="917af-127">Varlıkları silindi</span><span class="sxs-lookup"><span data-stu-id="917af-127">Entities are deleted</span></span>                   |
| <span data-ttu-id="917af-128">**ClientSetNull** (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="917af-128">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="917af-129">Yabancı anahtar özellikleri null</span><span class="sxs-lookup"><span data-stu-id="917af-129">Foreign key properties are set to null</span></span> | <span data-ttu-id="917af-130">Yok.</span><span class="sxs-lookup"><span data-stu-id="917af-130">None</span></span>                                   |
| <span data-ttu-id="917af-131">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="917af-131">**SetNull**</span></span>                 | <span data-ttu-id="917af-132">Yabancı anahtar özellikleri null</span><span class="sxs-lookup"><span data-stu-id="917af-132">Foreign key properties are set to null</span></span> | <span data-ttu-id="917af-133">Yabancı anahtar özellikleri null</span><span class="sxs-lookup"><span data-stu-id="917af-133">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="917af-134">**kısıtlama**</span><span class="sxs-lookup"><span data-stu-id="917af-134">**Restrict**</span></span>                | <span data-ttu-id="917af-135">Yok.</span><span class="sxs-lookup"><span data-stu-id="917af-135">None</span></span>                                   | <span data-ttu-id="917af-136">Yok.</span><span class="sxs-lookup"><span data-stu-id="917af-136">None</span></span>                                   |

### <a name="required-relationships"></a><span data-ttu-id="917af-137">Gerekli bir ilişki</span><span class="sxs-lookup"><span data-stu-id="917af-137">Required relationships</span></span>
<span data-ttu-id="917af-138">Gerekli ilişkileri (atanamaz yabancı anahtar) olduğu _değil_ olası aşağıdaki efektler sonuçlanır bir yabancı anahtar null ve kaydetmek:</span><span class="sxs-lookup"><span data-stu-id="917af-138">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="917af-139">Davranış adı</span><span class="sxs-lookup"><span data-stu-id="917af-139">Behavior Name</span></span>         | <span data-ttu-id="917af-140">Bağımlı/alt bellekte etkisi</span><span class="sxs-lookup"><span data-stu-id="917af-140">Effect on dependent/child in memory</span></span> | <span data-ttu-id="917af-141">Bağımlı/alt veritabanı üzerindeki etkisi</span><span class="sxs-lookup"><span data-stu-id="917af-141">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="917af-142">**Art arda** (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="917af-142">**Cascade** (Default)</span></span> | <span data-ttu-id="917af-143">Varlıkları silindi</span><span class="sxs-lookup"><span data-stu-id="917af-143">Entities are deleted</span></span>                | <span data-ttu-id="917af-144">Varlıkları silindi</span><span class="sxs-lookup"><span data-stu-id="917af-144">Entities are deleted</span></span>                  |
| <span data-ttu-id="917af-145">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="917af-145">**ClientSetNull**</span></span>     | <span data-ttu-id="917af-146">SaveChanges oluşturur</span><span class="sxs-lookup"><span data-stu-id="917af-146">SaveChanges throws</span></span>                  | <span data-ttu-id="917af-147">Yok.</span><span class="sxs-lookup"><span data-stu-id="917af-147">None</span></span>                                  |
| <span data-ttu-id="917af-148">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="917af-148">**SetNull**</span></span>           | <span data-ttu-id="917af-149">SaveChanges oluşturur</span><span class="sxs-lookup"><span data-stu-id="917af-149">SaveChanges throws</span></span>                  | <span data-ttu-id="917af-150">SaveChanges oluşturur</span><span class="sxs-lookup"><span data-stu-id="917af-150">SaveChanges throws</span></span>                    |
| <span data-ttu-id="917af-151">**kısıtlama**</span><span class="sxs-lookup"><span data-stu-id="917af-151">**Restrict**</span></span>          | <span data-ttu-id="917af-152">Yok.</span><span class="sxs-lookup"><span data-stu-id="917af-152">None</span></span>                                | <span data-ttu-id="917af-153">Yok.</span><span class="sxs-lookup"><span data-stu-id="917af-153">None</span></span>                                  |

<span data-ttu-id="917af-154">Yukarıdaki tablolarda *hiçbiri* kısıtlaması ihlali ile sonuçlanabilir.</span><span class="sxs-lookup"><span data-stu-id="917af-154">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="917af-155">Örneğin, asıl/alt varlık silinmiş, ancak bir bağımlı/alt yabancı anahtarı değiştirmek için hiçbir işlem yapılmadı, ardından veritabanı büyük olasılıkla SaveChanges üzerinde bir yabancı kısıtlaması ihlali nedeniyle durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="917af-155">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="917af-156">Yüksek düzeyde:</span><span class="sxs-lookup"><span data-stu-id="917af-156">At a high level:</span></span>
* <span data-ttu-id="917af-157">Bir üst sınıf olmadan var olamaz varlıkları varsa ve alt otomatik silerken halletmeniz için EF istediğiniz sonra kullanın *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="917af-157">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="917af-158">Bir üst genellikle olun olmadan var olamaz varlıklar için gerekli ilişkilerini, kullandığınız *Cascade* varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="917af-158">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="917af-159">Bir üst öğeye sahip olmayabilir veya varlıkları varsa ve yabancı anahtarı sizin için nulling halletmeniz için EF istediğiniz sonra kullanın *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="917af-159">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="917af-160">Bir üst genellikle olun olmadan bulunabilir varlıklar için isteğe bağlı ilişkilerini, kullandığınız *ClientSetNull* varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="917af-160">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="917af-161">Veritabanı alt yabancı anahtarlar için null değerler bile yaymak ayrıca denemek isterseniz, alt varlık yüklenmedi, ardından *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="917af-161">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="917af-162">Ancak, bu veritabanını desteklemesi gerekir ve böyle veritabanını yapılandırma diğer kısıtlamaları neden olabilir, uygulamada genellikle bu seçenek pratik hale getirir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="917af-162">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="917af-163">Bu nedenle, *SetNull* varsayılan olarak etkin değildir.</span><span class="sxs-lookup"><span data-stu-id="917af-163">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="917af-164">Şimdiye kadar otomatik olarak bir varlığı silmek veya otomatik olarak yabancı anahtar null ardından kullanarak EF Core istemiyorsanız *kısıtlama*.</span><span class="sxs-lookup"><span data-stu-id="917af-164">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="917af-165">Bu kodunuzu alt varlıklar ve yabancı anahtar değerlerini el ile eşitleyin, gerektiğini unutmayın, aksi takdirde kısıtlaması özel durumları oluşturulacaktır.</span><span class="sxs-lookup"><span data-stu-id="917af-165">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="917af-166">EF6, aksine, EF Core, basamaklı etkileri hemen, ancak bunun yerine yalnızca SaveChanges zaman çağrıldığını durum değil.</span><span class="sxs-lookup"><span data-stu-id="917af-166">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="917af-167">**EF Core 2.0 içindeki değişiklikleri:** önceki sürümlerde, *kısıtlama* isteğe bağlı bir yabancı anahtar özelliklerini ayarlamak için izlenen bağımlı varlıklarda neden boş ve varsayılan davranışı için isteğe bağlı ilişkileri silme idi.</span><span class="sxs-lookup"><span data-stu-id="917af-167">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="917af-168">EF Core 2.0 *ClientSetNull* bu davranışı temsil etmek için sunulmuştur ve isteğe bağlı bir ilişki için varsayılan hale geldi.</span><span class="sxs-lookup"><span data-stu-id="917af-168">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="917af-169">Davranışını *kısıtlama* hiçbir zaman bağımlı varlıkların herhangi yan etkileri olduğu ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="917af-169">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="917af-170">Varlık silme örnekleri</span><span class="sxs-lookup"><span data-stu-id="917af-170">Entity deletion examples</span></span>

<span data-ttu-id="917af-171">Aşağıdaki kod parçası olan bir [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , indirilebilen ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="917af-171">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="917af-172">Örnek bir üst varlık silindiğinde her isteğe bağlıdır ve gerekli ilişkileri silme davranışı için ne olacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="917af-172">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="917af-173">Neler olduğunu anlamak için her değişim atalım.</span><span class="sxs-lookup"><span data-stu-id="917af-173">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="917af-174">Gerekli veya isteğe bağlı bir ilişki ile DeleteBehavior.Cascade</span><span class="sxs-lookup"><span data-stu-id="917af-174">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
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

* <span data-ttu-id="917af-175">Blog silindi işaretlendi</span><span class="sxs-lookup"><span data-stu-id="917af-175">Blog is marked as Deleted</span></span>
* <span data-ttu-id="917af-176">Basamaklar SaveChanges kadar gerçekleşecek değil beri gönderileri Unchanged başlangıçta kalır.</span><span class="sxs-lookup"><span data-stu-id="917af-176">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="917af-177">Silme işlemi için hem bağımlılıklar/alt (posta) ve ardından asıl/üst (blog) SaveChanges gönderir</span><span class="sxs-lookup"><span data-stu-id="917af-177">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="917af-178">Artık veritabanından silinmiş olduğundan kaydettikten sonra tüm varlıkları ayrılır</span><span class="sxs-lookup"><span data-stu-id="917af-178">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="917af-179">DeleteBehavior.ClientSetNull ya da gerekli bir ilişki ile DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="917af-179">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
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

* <span data-ttu-id="917af-180">Blog silindi işaretlendi</span><span class="sxs-lookup"><span data-stu-id="917af-180">Blog is marked as Deleted</span></span>
* <span data-ttu-id="917af-181">Basamaklar SaveChanges kadar gerçekleşecek değil beri gönderileri Unchanged başlangıçta kalır.</span><span class="sxs-lookup"><span data-stu-id="917af-181">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="917af-182">FK post null olarak ayarlamak SaveChanges çalışır, ancak FK boş değer atanabilir olmadığından bu işlem başarısız</span><span class="sxs-lookup"><span data-stu-id="917af-182">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="917af-183">DeleteBehavior.ClientSetNull veya isteğe bağlı bir ilişki ile DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="917af-183">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
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

* <span data-ttu-id="917af-184">Blog silindi işaretlendi</span><span class="sxs-lookup"><span data-stu-id="917af-184">Blog is marked as Deleted</span></span>
* <span data-ttu-id="917af-185">Basamaklar SaveChanges kadar gerçekleşecek değil beri gönderileri Unchanged başlangıçta kalır.</span><span class="sxs-lookup"><span data-stu-id="917af-185">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="917af-186">SaveChanges denemeleri ayarlar hem bağımlılıklar/alt (posta) FK null (blog) asıl/üst silmeden önce</span><span class="sxs-lookup"><span data-stu-id="917af-186">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="917af-187">Kaydetme sonra asıl/üst (blog) silindiği ancak bağımlılıklar/alt (posta) hala izlenir</span><span class="sxs-lookup"><span data-stu-id="917af-187">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="917af-188">İzlenen bağımlılıklar/alt (yazıları), artık null FK değerlere sahip ve Silinen sorumlusu/üst (blog) kendi başvurusu kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="917af-188">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="917af-189">Gerekli veya isteğe bağlı bir ilişki ile DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="917af-189">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
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

* <span data-ttu-id="917af-190">Blog silindi işaretlendi</span><span class="sxs-lookup"><span data-stu-id="917af-190">Blog is marked as Deleted</span></span>
* <span data-ttu-id="917af-191">Basamaklar SaveChanges kadar gerçekleşecek değil beri gönderileri Unchanged başlangıçta kalır.</span><span class="sxs-lookup"><span data-stu-id="917af-191">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="917af-192">Bu yana *kısıtlama* FK otomatik olarak null olarak ayarlamak için EF söyler, null olmayan kalır ve SaveChanges kaydetmeden oluşturur</span><span class="sxs-lookup"><span data-stu-id="917af-192">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="917af-193">Artık örnekler delete</span><span class="sxs-lookup"><span data-stu-id="917af-193">Delete orphans examples</span></span>

<span data-ttu-id="917af-194">Aşağıdaki kod parçası olan bir [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , indirilebilen ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="917af-194">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="917af-195">Örnek, bir üst/sorumlusu ve onun alt öğeleri/dependents arasındaki ilişkiyi yazıyordunuz her isteğe bağlıdır ve gerekli ilişkileri silme davranışı için neler gösterir.</span><span class="sxs-lookup"><span data-stu-id="917af-195">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="917af-196">Bu örnekte, ilişki (blog) asıl/üst koleksiyon gezinme özelliği gelen bağımlılıklar/alt (posta) kaldırarak yazıyordunuz.</span><span class="sxs-lookup"><span data-stu-id="917af-196">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="917af-197">Bağımlı/alt öğe başvuru sorumlusu/üst olarak bunun yerine çıkış nulled, ancak aynı davranıştır.</span><span class="sxs-lookup"><span data-stu-id="917af-197">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="917af-198">Neler olduğunu anlamak için her değişim atalım.</span><span class="sxs-lookup"><span data-stu-id="917af-198">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="917af-199">Gerekli veya isteğe bağlı bir ilişki ile DeleteBehavior.Cascade</span><span class="sxs-lookup"><span data-stu-id="917af-199">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
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

* <span data-ttu-id="917af-200">İlişki severing FK null olarak işaretlenmesine neden olduğundan gönderileri değiştirilen işaretlenmiş</span><span class="sxs-lookup"><span data-stu-id="917af-200">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="917af-201">FK null olabilir değil, null olarak işaretlenmiş olsa bile ardından gerçek değer değişmez</span><span class="sxs-lookup"><span data-stu-id="917af-201">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="917af-202">Silme işlemi için bağımlılıklar/alt (posta) SaveChanges gönderir</span><span class="sxs-lookup"><span data-stu-id="917af-202">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="917af-203">Artık veritabanından silinmiş olduğundan kaydettikten sonra bağımlılıklar/alt (posta) ayrılmış</span><span class="sxs-lookup"><span data-stu-id="917af-203">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="917af-204">DeleteBehavior.ClientSetNull ya da gerekli bir ilişki ile DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="917af-204">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
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

* <span data-ttu-id="917af-205">İlişki severing FK null olarak işaretlenmesine neden olduğundan gönderileri değiştirilen işaretlenmiş</span><span class="sxs-lookup"><span data-stu-id="917af-205">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="917af-206">FK null olabilir değil, null olarak işaretlenmiş olsa bile ardından gerçek değer değişmez</span><span class="sxs-lookup"><span data-stu-id="917af-206">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="917af-207">FK post null olarak ayarlamak SaveChanges çalışır, ancak FK boş değer atanabilir olmadığından bu işlem başarısız</span><span class="sxs-lookup"><span data-stu-id="917af-207">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="917af-208">DeleteBehavior.ClientSetNull veya isteğe bağlı bir ilişki ile DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="917af-208">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
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

* <span data-ttu-id="917af-209">İlişki severing FK null olarak işaretlenmesine neden olduğundan gönderileri değiştirilen işaretlenmiş</span><span class="sxs-lookup"><span data-stu-id="917af-209">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="917af-210">FK null olabilir değil, null olarak işaretlenmiş olsa bile ardından gerçek değer değişmez</span><span class="sxs-lookup"><span data-stu-id="917af-210">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="917af-211">Her iki bağımlılıklar/alt (posta) FK SaveChanges null olarak ayarlar</span><span class="sxs-lookup"><span data-stu-id="917af-211">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="917af-212">Kaydetme sonra bağımlılıklar/alt (yazıları), artık null FK değerlere sahip ve Silinen sorumlusu/üst (blog) kendi başvurusu kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="917af-212">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="917af-213">Gerekli veya isteğe bağlı bir ilişki ile DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="917af-213">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
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

* <span data-ttu-id="917af-214">İlişki severing FK null olarak işaretlenmesine neden olduğundan gönderileri değiştirilen işaretlenmiş</span><span class="sxs-lookup"><span data-stu-id="917af-214">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="917af-215">FK null olabilir değil, null olarak işaretlenmiş olsa bile ardından gerçek değer değişmez</span><span class="sxs-lookup"><span data-stu-id="917af-215">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="917af-216">Bu yana *kısıtlama* FK otomatik olarak null olarak ayarlamak için EF söyler, null olmayan kalır ve SaveChanges kaydetmeden oluşturur</span><span class="sxs-lookup"><span data-stu-id="917af-216">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="917af-217">İzlenmeyen varlıklara geçişli</span><span class="sxs-lookup"><span data-stu-id="917af-217">Cascading to untracked entities</span></span>

<span data-ttu-id="917af-218">Çağırdığınızda *SaveChanges*, bağlam tarafından izlenen herhangi bir varlık art arda silme kuralları uygulanır.</span><span class="sxs-lookup"><span data-stu-id="917af-218">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="917af-219">Bu durum tüm, yukarıda gösterilen örnek olduğu SQL hem sorumlusu/üst (blog) ve tüm bağımlılıklar/alt öğeler (posta) silmek için oluşturulan neden:</span><span class="sxs-lookup"><span data-stu-id="917af-219">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="917af-220">Asıl yüklenen--yalnızca örneğin, ne zaman bir sorgu yapılır bir blog bir `Include(b => b.Posts)` gönderileri--de içerecek şekilde sonra SaveChanges yalnızca asıl/üst silmek için SQL oluşturur:</span><span class="sxs-lookup"><span data-stu-id="917af-220">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="917af-221">Bağımlılıklar/alt (posta), yalnızca yapılandırılan karşılık gelen bir cascade davranışına veritabanı varsa, silinecek.</span><span class="sxs-lookup"><span data-stu-id="917af-221">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="917af-222">EF veritabanını oluşturmak için kullanıyorsanız, bu cascade davranışı için Kurulum olacaktır.</span><span class="sxs-lookup"><span data-stu-id="917af-222">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
