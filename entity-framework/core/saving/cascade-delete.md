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
# <a name="cascade-delete"></a>Basamaklı Silme

Basamaklı silme genellikle veritabanı terminolojisinde, ilgili satırların silinmesini otomatik olarak tetiklemek için bir satırsilme sağlayan bir özelliği tanımlamak için kullanılır. EF Core silme davranışları tarafından da kapsanan yakından ilişkili bir kavram, bir alt varlığın bir üst öğeyle ilişkisi kesildiğinde otomatik olarak silinmesidir - bu genellikle "yetimleri silme" olarak bilinir.

EF Core birkaç farklı silme davranışı uygular ve tek tek ilişkilerin silme davranışlarınıyapılandırmasına olanak tanır. EF Core ayrıca, [ilişkinin gerekliliğine](../modeling/relationships.md#required-and-optional-relationships)bağlı olarak her ilişki için yararlı varsayılan silme davranışlarını otomatik olarak yapılandıran kuralları da uygular.

## <a name="delete-behaviors"></a>Davranışları silme

Silme davranışları *DeleteBehavior* sayısalator türünde tanımlanır ve bir asıl/üst varlığın silinmesini veya bağımlı/alt varlıklarla ilişkinin kesilmesinin bağımlı/alt varlıklar üzerinde yan etkisi olup olmadığını kontrol etmek için *OnDelete* akıcı API'sine geçirilebilir.

Bir asıl/üst varlık silindiğinde veya çocukla ilişki kesildiğinde EF'nin alabilecekleri üç eylem vardır:

* Alt/bağımlı silinebilir
* Çocuğun yabancı anahtar değerleri null ayarlanabilir
* Çocuk değişmeden kalır

> [!NOTE]  
> EF Core modelinde yapılandırılan silme davranışı yalnızca asıl varlık EF Core kullanılarak silindiğinde ve bağımlı varlıklar belleğe yüklendiğinde (diğer bir deyişle, izlenen bağımlılar için) uygulanır. Bağlam tarafından izlenmeden veri gerekli eylem uygulandığından emin olmak için ilgili basamaklı davranış veritabanında kurulumu gerekir. Veritabanını oluşturmak için EF Core'u kullanırsanız, bu basamaklı davranış sizin için kurulum olacaktır.

Yukarıdaki ikinci eylem için, yabancı anahtar geçersiz değilse, geçersiz bir yabancı anahtar değeri ayarı geçerli değildir. (Nullable olmayan yabancı anahtar gerekli bir ilişkiye eşdeğerdir.) Bu gibi durumlarda, EF Core, Yabancı anahtar özelliğinin SaveChanges çağrılana kadar null olarak işaretlendiğini ve değişiklik veritabanına kalıcı hale alınamadığı için bir özel durum atıldığını izler. Bu, veritabanından bir kısıtlama ihlali almaya benzer.

Aşağıdaki tablolarda listelenen dört silme davranışı vardır.

### <a name="optional-relationships"></a>İsteğe bağlı ilişkiler

İsteğe bağlı ilişkiler (nullable _is_ yabancı anahtar) için aşağıdaki etkilere neden olan null yabancı anahtar değeri kaydetmek mümkündür:

| Davranış Adı               | Bellekte bağımlı/alt üzerindeki etkisi    | Veritabanındaki bağımlı/alt üzerindeki etkisi  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Cascade**                 | Varlıklar silinir                   | Varlıklar silinir                   |
| **ClientSetNull** (Varsayılan) | Yabancı anahtar özellikleri null ayarlanır | None                                   |
| **Setnull**                 | Yabancı anahtar özellikleri null ayarlanır | Yabancı anahtar özellikleri null ayarlanır |
| **Kısıtlamak**                | None                                   | None                                   |

### <a name="required-relationships"></a>Gerekli ilişkiler

Gerekli ilişkiler (nullable yabancı anahtar) için aşağıdaki etkilere neden olan bir null yabancı anahtar değeri kaydetmek mümkün _değildir:_

| Davranış Adı         | Bellekte bağımlı/alt üzerindeki etkisi | Veritabanındaki bağımlı/alt üzerindeki etkisi |
|:----------------------|:------------------------------------|:--------------------------------------|
| **Basamaklı** (Varsayılan) | Varlıklar silinir                | Varlıklar silinir                  |
| **Müşteri Setnull**     | SaveChanges atar                  | None                                  |
| **Setnull**           | SaveChanges atar                  | SaveChanges atar                    |
| **Kısıtlamak**          | None                                | None                                  |

Yukarıdaki tablolarda, *Hiçbiri* bir kısıtlama ihlaline neden olabilir. Örneğin, bir asıl/alt varlık silinirse ancak bağımlı/alt çocuğun yabancı anahtarını değiştirmek için herhangi bir işlem yapılmazsa, veritabanı büyük olasılıkla yabancı kısıtlama ihlali nedeniyle SaveChanges'ı atar.

Yüksek düzeyde:

* Ebeveyni olmadan var olamayan varlıklarınız varsa ve EF'nin çocukları otomatik olarak silmesine özen duymasını istiyorsanız, *Ardından Basamaklı'yı*kullanın.
  * Üst öğeolmadan var olamayan varlıklar genellikle *basamaklı* varsayılan olan gerekli ilişkilerden yararlar.
* Bir ebeveyniniz olabilir veya olmayabilir varlıklar varsa ve EF sizin için yabancı anahtarı iptal dikkat çekmek istiyorsanız, o zaman *ClientSetNull* kullanın
  * Üst öğesi olmadan var olabilecek varlıklar genellikle *ClientSetNull'un* varsayılan olduğu isteğe bağlı ilişkilerden yararlar.
  * Veritabanının, alt varlık yüklenmese bile null değerlerini alt yabancı anahtarlara yaymayı da denemesini istiyorsanız, *SetNull'u*kullanın. Ancak, veritabanının bunu desteklemesi gerektiğini ve veritabanını bu şekilde yapılandırılabilmenizin diğer kısıtlamalara neden olabileceğini ve bunun da uygulamada bu seçeneği genellikle pratik hale getirebileceğini unutmayın. Bu nedenle *SetNull* varsayılan değildir.
* EF Core'un bir varlığı otomatik olarak silmesini veya yabancı anahtarı otomatik olarak iptal etmesini istemiyorsanız, *Kısıtla'yı*kullanın. Bunun için kodlarınızın alt varlıkları ve yabancı anahtar değerlerini el ile eşitleme de tutmalarını gerektirdiğini unutmayın aksi takdirde kısıtlama özel durumları atılır.

> [!NOTE]
> EF Core'da, EF6'dan farklı olarak basamaklı efektler hemen gerçekleşmez, bunun yerine yalnızca SaveChanges çağrıldığında gerçekleşir.

> [!NOTE]  
> **EF Core 2.0'daki değişiklikler:** Önceki *sürümlerde, Kısıtla,* izlenen bağımlı varlıklardaki isteğe bağlı yabancı anahtar özelliklerinin null olarak ayarlanmasına neden olur ve isteğe bağlı ilişkiler için varsayılan silme davranışıdır. EF Core 2.0'da, *ClientSetNull* bu davranışı temsil etmek üzere tanıtıldı ve isteğe bağlı ilişkiler için varsayılan oldu. *Kısıtlama* davranışı bağımlı varlıklar üzerinde herhangi bir yan etkisi asla ayarlandı.

## <a name="entity-deletion-examples"></a>Varlık silme örnekleri

Aşağıdaki kod, karşıdan yüklenebilecek ve çalıştırılabilen bir [örneğin](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) parçasıdır. Örnek, bir üst varlık silindiğinde hem isteğe bağlı hem de gerekli ilişkiler için her silme davranışı için neler olduğunu gösterir.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Neler olduğunu anlamak için her varyasyonu gözden geçirelim.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade gerekli veya isteğe bağlı ilişki ile

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

* Blog Silinmiş olarak işaretlenir
* Basamaklı kaydetmeler SaveChanges kadar gerçekleşmediğinden gönderiler başlangıçta değişmeden kalır
* SaveChanges hem bağımlılar/çocuklar (gönderiler) hem de ana/üst (blog) için silme gönderir
* Kaydedildikten sonra, veritabanından silindikleri için tüm varlıklar ayrılır

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull veya DeleteBehavior.SetNull gerekli ilişki ile

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

* Blog Silinmiş olarak işaretlenir
* Basamaklı kaydetmeler SaveChanges kadar gerçekleşmediğinden gönderiler başlangıçta değişmeden kalır
* SaveChanges, FK gönderisini null'a ayarlamaya çalışır, ancak FK geçersiz olmadığı için bu başarısız olur

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull veya DeleteBehavior.SetNull isteğe bağlı ilişki ile

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

* Blog Silinmiş olarak işaretlenir
* Basamaklı kaydetmeler SaveChanges kadar gerçekleşmediğinden gönderiler başlangıçta değişmeden kalır
* SaveChanges girişimleri, asıl/üst öğeyi (blog) silmeden önce her iki bağımlının/çocuğun (gönderilerin) FK'sını geçersiz kılacak şekilde ayarlar
* Kaydedildikten sonra, asıl/üst (blog) silinir, ancak bağımlılar/çocuklar (gönderiler) hala izlenir
* İzlenen bağımlılar/çocuklar (gönderiler) artık null FK değerlerine sahiptir ve silinen asıl/üst öğeye (blog) göndermeleri kaldırılmıştır

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict gerekli veya isteğe bağlı ilişki ile

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

* Blog Silinmiş olarak işaretlenir
* Basamaklı kaydetmeler SaveChanges kadar gerçekleşmediğinden gönderiler başlangıçta değişmeden kalır
* *Kısıtlama,* EF'ye FK'yı otomatik olarak null olarak ayarlamamasını söylediğinden, geçersiz kalır ve SaveChanges kaydetmeden atar

## <a name="delete-orphans-examples"></a>Yetim örneklerini silme

Aşağıdaki kod, karşıdan yüklenebilecek ve çalıştırılabilen bir [örneğin](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) parçasıdır. Örnek, bir üst/asıl ve alt/alt öğesi arasındaki ilişki koptuğunda, hem isteğe bağlı hem de gerekli ilişkiler için her silme davranışı için neler olduğunu gösterir. Bu örnekte, ilişki, asıl/üst öğedeki (blog) koleksiyon gezinti özelliğinden bağımlı/alt (gönderiler) kaldırılarak kesilir. Ancak, bağımlı/alttan alt öğeye/üst öğeye yapılan başvuru iptal edilirse davranış aynıdır.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Neler olduğunu anlamak için her varyasyonu gözden geçirelim.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade gerekli veya isteğe bağlı ilişki ile

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

* İlişkiyi kesme fk'nın null olarak işaretlenilmesine neden olduğundan gönderiler Değiştirilmiştir
  * FK nullable değilse, o zaman gerçek değeri null olarak işaretlenmiş olsa bile değişmez
* SaveChanges bağımlılar/çocuklar (gönderiler) için silme gönderir
* Tasarruf tan sonra, bağımlılar/çocuklar (gönderiler) artık veritabanından silindiğinden ayrılır

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull veya DeleteBehavior.SetNull gerekli ilişki ile

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

* İlişkiyi kesme fk'nın null olarak işaretlenilmesine neden olduğundan gönderiler Değiştirilmiştir
  * FK nullable değilse, o zaman gerçek değeri null olarak işaretlenmiş olsa bile değişmez
* SaveChanges, FK gönderisini null'a ayarlamaya çalışır, ancak FK geçersiz olmadığı için bu başarısız olur

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull veya DeleteBehavior.SetNull isteğe bağlı ilişki ile

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

* İlişkiyi kesme fk'nın null olarak işaretlenilmesine neden olduğundan gönderiler Değiştirilmiştir
  * FK nullable değilse, o zaman gerçek değeri null olarak işaretlenmiş olsa bile değişmez
* SaveChanges, her iki bağımlının/çocuğun (posta) FK'sını null ayarlar
* Tasarruf tan sonra, bağımlılar/çocuklar (gönderiler) artık null FK değerlerine sahiptir ve silinen asıl/üst öğeye (blog) göndermeleri kaldırılmıştır

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict gerekli veya isteğe bağlı ilişki ile

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

* İlişkiyi kesme fk'nın null olarak işaretlenilmesine neden olduğundan gönderiler Değiştirilmiştir
  * FK nullable değilse, o zaman gerçek değeri null olarak işaretlenmiş olsa bile değişmez
* *Kısıtlama,* EF'ye FK'yı otomatik olarak null olarak ayarlamamasını söylediğinden, geçersiz kalır ve SaveChanges kaydetmeden atar

## <a name="cascading-to-untracked-entities"></a>İzlenmemiş varlıklara basamaklama

*SaveChanges'ı*aradiğinizde, basamaklı silme kuralları bağlam tarafından izlenen tüm varlıklara uygulanır. Bu, yukarıda gösterilen tüm örneklerdeki durumdur, bu nedenle SQL hem asıl/üst (blog) hem de tüm bağımlıları/alt ları (gönderileri) silmek için oluşturulmuştur:

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Yalnızca anapara yüklenirse (örneğin, gönderileri eklemeden `Include(b => b.Posts)` bir blog için sorgu yapılırsa- SaveChanges yalnızca asıl/üst öğeyi silmek için SQL oluşturur:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Bağımlılar/alt (gönderiler) yalnızca veritabanında ilgili basamaklı davranış yapılandırılırsa silinir. Veritabanını oluşturmak için EF kullanırsanız, bu basamaklı davranış sizin için kurulum olacaktır.
