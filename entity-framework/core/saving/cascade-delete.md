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
# <a name="cascade-delete"></a>Basamaklı Silme

Art arda silme, bir satırı silmenin ilgili satırların silinmesini otomatik olarak tetiklemesine izin veren bir özelliği anlatmak için Veritabanı terminolojisinde yaygın olarak kullanılır. EF Core Delete davranışları tarafından ele alınan yakından ilgili bir kavram, bir üst öğe ile ilişkisi olduğu zaman bir alt varlığın otomatik olarak silinmesinden sonra bu, yaygın olarak "artık silme" olarak bilinir.

EF Core birkaç farklı silme davranışı uygular ve tek tek ilişkilerin silme davranışlarının yapılandırılmasına izin verir. EF Core Ayrıca, [ilişkinin gerekliğine](../modeling/relationships.md#required-and-optional-relationships)göre her ilişki için yararlı varsayılan silme davranışlarını otomatik olarak yapılandıran kuralları uygular.

## <a name="delete-behaviors"></a>Davranışları Sil

Silme davranışları *DeleteBehavior* Numaralandırıcı türünde tanımlanır ve bir sorumlu/üst varlığın silinmesini veya bağımlı/alt varlıklara olan ilişkinin ne kadar olduğunu denetlemek Için *OnDelete* Fluent API geçirilebilir bağımlı/alt varlıklar üzerinde yan etkisi vardır.

Bir asıl/üst varlık silindiğinde veya alt öğeyle olan ilişki bırakıldığında, EF 'in gerçekleştirebileceği üç eylem vardır:

* Alt/Bağımlı öğe silinebilir
* Çocuğun yabancı anahtar değerleri null olarak ayarlanabilir
* Alt öğe değişmeden kalır

> [!NOTE]  
> EF Core modelinde yapılandırılan silme davranışı yalnızca asıl varlık EF Core kullanılarak silindiğinde ve bağımlı varlıkların belleğe yüklenmesi durumunda (izlenen bağımlılar için) uygulanır. Bağlam tarafından izlenmeyen verilerin uygulanmış olduğundan emin olmak için, karşılık gelen bir Cascade davranışının veritabanında kurulması gerekir. Veritabanını oluşturmak için EF Core kullanırsanız, bu basamaklı davranış sizin için kurulum olacaktır.

Yukarıdaki ikinci eylem için yabancı anahtar null yapılabilir değilse, yabancı anahtar değerini null olarak ayarlama geçerli değildir. (Null yapılamayan yabancı anahtar, gerekli bir ilişkiye eşdeğerdir.) Bu durumlarda EF Core, SaveChanges çağrılmadan önce yabancı anahtar özelliğinin null olarak işaretlendiğinden, değişikliğin veritabanına kalıcı hale uğradığından bir özel durum oluşturulduğu anlamına gelir. Bu, veritabanından bir kısıtlama ihlali almaya benzer.

Aşağıdaki tablolarda listelendiği gibi dört silme davranışı vardır.

### <a name="optional-relationships"></a>İsteğe bağlı ilişkiler

İsteğe bağlı ilişkiler için (boş değer atanabilir yabancı anahtar), aşağıdaki etkilere neden olan bir boş yabancı anahtar değeri _kaydetmek mümkündür:_

| Davranış adı               | Bellekte bağımlı/alt öğe üzerindeki etki    | Veritabanında bağımlı/alt öğe üzerindeki etki  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Seçilemez**                 | Varlıklar silindi                   | Varlıklar silindi                   |
| **Clientsetnull** (varsayılan) | Yabancı anahtar özellikleri null olarak ayarlandı | Yok.                                   |
| **SetNull**                 | Yabancı anahtar özellikleri null olarak ayarlandı | Yabancı anahtar özellikleri null olarak ayarlandı |
| **Girmesini**                | Yok.                                   | Yok.                                   |

### <a name="required-relationships"></a>Gerekli ilişkiler

Gerekli ilişkiler (null yapılamayan yabancı anahtar) için, bir boş yabancı anahtar değeri kaydetmek mümkün _değildir_ , bu durum aşağıdaki etkilere neden olur:

| Davranış adı         | Bellekte bağımlı/alt öğe üzerindeki etki | Veritabanında bağımlı/alt öğe üzerindeki etki |
|:----------------------|:------------------------------------|:--------------------------------------|
| **Basamakla** (varsayılan) | Varlıklar silindi                | Varlıklar silindi                  |
| **ClientSetNull**     | SaveChanges atar                  | Yok.                                  |
| **SetNull**           | SaveChanges atar                  | SaveChanges atar                    |
| **Girmesini**          | Yok.                                | Yok.                                  |

Yukarıdaki tablolarda, *none* bir kısıtlama ihlaline yol açabilir. Örneğin, bir asıl/alt varlık silinirse ancak bağımlı/alt öğenin yabancı anahtarını değiştirmek için herhangi bir eylem yapılmaz, veritabanı büyük olasılıkla yabancı bir kısıtlama ihlali nedeniyle SaveChanges üzerinde oluşturulur.

Yüksek düzeyde:

* Üst öğesi olmadan var olmayan varlıklarınız varsa ve alt öğeleri otomatik olarak silmek için EF 'i istiyorsanız, *Cascade*kullanın.
  * Üst öğe olmadan mevcut olmayan varlıklar genellikle, *Cascade* varsayılan değer olan gerekli ilişkilerden birini kullanır.
* Bir üst öğeye sahip olabilecek veya olmayan varlıklarınız varsa ve yabancı anahtarı sizin yerinize dışarıda bırakmak istiyorsanız, bu durumda *Clientsetnull* kullanın
  * Bir üst öğesi olmadan var olabilen varlıklar genellikle isteğe bağlı ilişkilerden ( *Clientsetnull* değeri için varsayılan değer) kullanır.
  * Ayrıca, alt varlık yüklü olmasa bile veritabanının null değerleri alt yabancı anahtarlara yaymasını istiyorsanız, *SetNull*' ı kullanın. Bununla birlikte, veritabanının bunu desteklemesi gerektiğini ve bu şekilde veritabanının yapılandırılmasını unutmayın. Bu, uygulamada genellikle bu seçeneği pratik hale getirir. Bu, *SetNull* varsayılan değer değildir.
* EF Core bir varlığı otomatik olarak silmek veya yabancı anahtarı otomatik olarak boş bırakmak istemiyorsanız *kısıtla*' yı kullanın. Bunun için, kodunuzun alt varlıkları ve yabancı anahtar değerlerini el ile eşitlenmiş halde tutmasını, aksi takdirde kısıtlama özel durumları oluşturulur.

> [!NOTE]
> EF Core ' de, EF6 aksine, geçişli efektler hemen gerçekleşmez, ancak bunun yerine SaveChanges çağrılır.

> [!NOTE]  
> **EF Core 2,0 değişiklikleri:** Önceki sürümlerde *kısıtlama* , izlenen bağımlı varlıklarda isteğe bağlı yabancı anahtar özelliklerinin null olarak ayarlanmasına ve isteğe bağlı ilişkiler için varsayılan silme davranışına neden olur. EF Core 2,0 ' de, bu davranışı temsil eden ve isteğe bağlı ilişkiler için varsayılan değer haline getirilen *Clientsetnull* eklenmiştir. *Kısıtlama* davranışı, bağımlı varlıklar üzerinde hiçbir bir yan etkiye hiçbir şekilde ayarlanmıştı.

## <a name="entity-deletion-examples"></a>Varlık silme örnekleri

Aşağıdaki kod, indirilebilen ve çalıştırılabilen bir [Örneğin](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) parçasıdır. Örnek, bir üst varlık silindiğinde hem isteğe bağlı hem de gerekli ilişkilerin her silme davranışı için ne olacağını gösterir.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Ne olduğunu anlamak için her çeşitlemeyi inceleyelim.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior. Cascade, gerekli veya isteğe bağlı ilişki

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

* Blog silindi olarak işaretlendi
* SaveChanges, SaveChanges 'e kadar basamaklar olmadığı için başlangıçta değiştirilmeden kalır
* SaveChanges hem bağımlılar/alt öğe (gönderiler) hem de asıl/üst öğe (blog) için silme gönderir
* Kaydettikten sonra, veritabanından silindiklerinden beri tüm varlıklar ayrılır

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>Gerekli ilişki ile DeleteBehavior. ClientSetNull veya DeleteBehavior. SetNull

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

* Blog silindi olarak işaretlendi
* SaveChanges, SaveChanges 'e kadar basamaklar olmadığı için başlangıçta değiştirilmeden kalır
* SaveChanges Post FK 'ı null olarak ayarlamaya çalışır, ancak FK null değer atanabilir olmadığından bu başarısız olur

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>İsteğe bağlı ilişki ile DeleteBehavior. ClientSetNull veya DeleteBehavior. SetNull

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

* Blog silindi olarak işaretlendi
* SaveChanges, SaveChanges 'e kadar basamaklar olmadığı için başlangıçta değiştirilmeden kalır
* SaveChanges denemesi, asıl/üst öğeyi (blog) silmeden önce hem bağımlıların hem de alt öğelerin (gönderilerin) FK değerini null olarak ayarlar
* Kaydettikten sonra asıl/üst öğe (blog) silinir, ancak bağımlılar/alt öğeler (gönderiler) hala izlenir
* İzlenen bağımlılar/alt öğeler (gönderiler) artık null FK değerlerine sahiptir ve silinen asıl/üst öğeye (blog) başvuruları kaldırılmıştır

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior. gerekli veya isteğe bağlı ilişki ile kısıtla

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

* Blog silindi olarak işaretlendi
* SaveChanges, SaveChanges 'e kadar basamaklar olmadığı için başlangıçta değiştirilmeden kalır
* *Restrict* , EF 'in FK otomatik olarak null olarak ayarlanmayacağını söylediğinden, null olmayan ve SaveChanges, kaydetmeden önce

## <a name="delete-orphans-examples"></a>Artık örnekleri Sil örnekleri

Aşağıdaki kod, indirilebilen ve çalıştırılabilen bir [Örneğin](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) parçasıdır. Örnek, bir üst/birincil ve alt öğeleri/bağımlılığının arasındaki ilişki olmadığında, hem isteğe bağlı hem de gerekli ilişkiler için her silme davranışının ne olacağını gösterir. Bu örnekte, birincil/üst öğe (blog) üzerindeki koleksiyon gezintisi özelliğinden bağımlılar/alt öğeler (postalar) kaldırılarak ilişki ortadan kaldırılır. Ancak, bağımlı/alt ile asıl/üst öğeye yapılan başvurunun null olarak dışına çıkar olması durumunda davranış aynıdır.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Ne olduğunu anlamak için her çeşitlemeyi inceleyelim.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior. Cascade, gerekli veya isteğe bağlı ilişki

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

* Gönderi, FK 'in null olarak işaretlendiğinden ilişki kesilmesine neden olduğu için değiştirilmiş olarak işaretlenir
  * FK null yapılabilir değilse, gerçek değer null olarak işaretlenmiş olsa bile değişmeyecektir
* SaveChanges, bağımlılar/çocuklar için silme gönderir (gönderiler)
* Kaydettikten sonra, bağımlılıklar/alt öğeler (postalar) veritabanından silindiği için ayrılır

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>Gerekli ilişki ile DeleteBehavior. ClientSetNull veya DeleteBehavior. SetNull

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

* Gönderi, FK 'in null olarak işaretlendiğinden ilişki kesilmesine neden olduğu için değiştirilmiş olarak işaretlenir
  * FK null yapılabilir değilse, gerçek değer null olarak işaretlenmiş olsa bile değişmeyecektir
* SaveChanges Post FK 'ı null olarak ayarlamaya çalışır, ancak FK null değer atanabilir olmadığından bu başarısız olur

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>İsteğe bağlı ilişki ile DeleteBehavior. ClientSetNull veya DeleteBehavior. SetNull

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

* Gönderi, FK 'in null olarak işaretlendiğinden ilişki kesilmesine neden olduğu için değiştirilmiş olarak işaretlenir
  * FK null yapılabilir değilse, gerçek değer null olarak işaretlenmiş olsa bile değişmeyecektir
* SaveChanges hem bağımlılığının hem de alt öğelerin (gönderilerin) FK değerini null olarak ayarlar
* Kaydedildikten sonra, bağımlılar/alt öğeler (gönderimler) artık null FK değerlerine sahiptir ve silinen asıl/üst öğeye (blog) başvuruları kaldırılmıştır

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior. gerekli veya isteğe bağlı ilişki ile kısıtla

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

* Gönderi, FK 'in null olarak işaretlendiğinden ilişki kesilmesine neden olduğu için değiştirilmiş olarak işaretlenir
  * FK null yapılabilir değilse, gerçek değer null olarak işaretlenmiş olsa bile değişmeyecektir
* *Restrict* , EF 'in FK otomatik olarak null olarak ayarlanmayacağını söylediğinden, null olmayan ve SaveChanges, kaydetmeden önce

## <a name="cascading-to-untracked-entities"></a>İzlenmeyen varlıklara basamaklandırın

*SaveChanges*öğesini çağırdığınızda, Cascade silme kuralları bağlam tarafından izlenmekte olan tüm varlıklara uygulanır. Bu durum yukarıda gösterilen tüm örneklerde, SQL 'in hem asıl/üst öğeyi (blog) hem de tüm bağımlıları/alt öğeleri (gönderiler) silmek için oluşturulmasının durumdur:

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Yalnızca asıl öğe yüklüyse (örneğin, bir blog için bir `Include(b => b.Posts)` yoksa, gönderiler dahil etmeden bir sorgu yapıldığında, SaveChanges yalnızca SQL 'i temel/üst öğeyi silmek için oluşturur:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Bağımlılar/alt öğeler (gönderimler) yalnızca, veritabanında karşılık gelen bir Cascade davranışı yapılandırıldıysa silinir. Veritabanını oluşturmak için EF kullanırsanız, bu basamaklı davranış sizin için kurulum olacaktır.
