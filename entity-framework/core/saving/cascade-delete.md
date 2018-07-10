---
title: Basamaklı silme - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: 2c50d94aafb3788761efc4225b6340a8e0da712d
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911566"
---
# <a name="cascade-delete"></a>Art arda silme

Art arda silme, Veritabanı terminolojisinde otomatik olarak ilişkili satırları silme işlemi tetiklemek için bir satır silinmesini sağlayan bir özellik tanımlamak için yaygın olarak kullanılır. Onun üst ilişkisi yazıyordunuz--ayrıca EF Core silme davranışları tarafından kapsanan yakından ilgili bir kavram alt varlık otomatik olarak silinmesini olduğunda bu genellikle "artık silinmesi" adı verilir.

EF Core, birkaç farklı silme davranışları uygular ve tek tek ilişkileri silme davranışlarını yapılandırmasını sağlar. EF Core de otomatik olarak yararlı varsayılan silme davranışları göre her ilişki için yapılandırma kuralları uygular [requiredness ilişkinin](../modeling/relationships.md#required-and-optional-relationships).

## <a name="delete-behaviors"></a>Davranışlar Sil
Silme davranışları tanımlanmış *DeleteBehavior* Numaralandırıcı yazın ve geçirilebilir *OnDelete* denetimine fluent API'si olup olmadığını sorumlusu/üst varlık veya, severing silinmesini İlişki bağımlı/alt varlıklar için bir yan etkisi bağımlı/alt varlıklar üzerinde olmalıdır.

EF sorumlusu/üst varlık silinmiş ya da alt ilişkinin yazıyordunuz üç eylemleri vardır:
* Alt/bağımlı silinebilir
* Çocuğun yabancı anahtar değerler olarak ayarlanabilir null
* Alt değişmeden kalır.

> [!NOTE]  
> EF Core modelinde yapılandırılmış silme davranışı, yalnızca bağımlı varlıkları bellekte (yani izlenen bağımlılıklar için) yüklenir ve EF Core kullanarak asıl varlık silinmiş uygulanır. Karşılık gelen bir cascade davranış bağlam tarafından izlenmiyor veri emin olmak için Veritabanı Kurulumu uygulanan gerekli eylem sahip olması gerekir. EF Core veritabanını oluşturmak için kullanıyorsanız, bu cascade davranışı için Kurulum olacaktır.

Bir yabancı anahtar değeri null olarak ayarlama özelliği Yukarıdaki ikinci eylem için yabancı anahtar null değilse geçerli değil. (Atanamaz yabancı anahtar gerekli bir ilişki eşdeğerdir.) Bu gibi durumlarda, değişiklik veritabanına kalıcı olamayacağından, aynı zamanda bir özel durum SaveChanges çağrılana kadar yabancı anahtar özelliği null işaretlenmiş olduğundan, EF Core izler. Bu, bir kısıtlama ihlali veritabanından alma için benzer.

Dört Sil davranışları, aşağıdaki tablolarda listelenen gibi vardır.

### <a name="optional-relationships"></a>İsteğe bağlı bir ilişki
İsteğe bağlı ilişkiler (boş değer atanabilir yabancı anahtar) için bunu _olduğu_ olası aşağıdaki efektler sonuçlanır bir yabancı anahtar null ve kaydetmek:

| Davranış adı               | Bağımlı/alt bellekte etkisi    | Bağımlı/alt veritabanı üzerindeki etkisi  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Basamakla**                 | Varlıkları silindi                   | Varlıkları silindi                   |
| **ClientSetNull** (varsayılan) | Yabancı anahtar özellikleri null | Yok.                                   |
| **SetNull**                 | Yabancı anahtar özellikleri null | Yabancı anahtar özellikleri null |
| **Kısıtlama**                | Yok.                                   | Yok.                                   |

### <a name="required-relationships"></a>Gerekli bir ilişki
Gerekli ilişkileri (atanamaz yabancı anahtar) olduğu _değil_ olası aşağıdaki efektler sonuçlanır bir yabancı anahtar null ve kaydetmek:

| Davranış adı         | Bağımlı/alt bellekte etkisi | Bağımlı/alt veritabanı üzerindeki etkisi |
|:----------------------|:------------------------------------|:--------------------------------------|
| **Art arda** (varsayılan) | Varlıkları silindi                | Varlıkları silindi                  |
| **ClientSetNull**     | SaveChanges oluşturur                  | Yok.                                  |
| **SetNull**           | SaveChanges oluşturur                  | SaveChanges oluşturur                    |
| **Kısıtlama**          | Yok.                                | Yok.                                  |

Yukarıdaki tablolarda *hiçbiri* kısıtlaması ihlali ile sonuçlanabilir. Örneğin, asıl/alt varlık silinmiş, ancak bir bağımlı/alt yabancı anahtarı değiştirmek için hiçbir işlem yapılmadı, ardından veritabanı büyük olasılıkla SaveChanges üzerinde bir yabancı kısıtlaması ihlali nedeniyle durum oluşturur.

Yüksek düzeyde:
* Bir üst sınıf olmadan var olamaz varlıkları varsa ve alt otomatik silerken halletmeniz için EF istediğiniz sonra kullanın *Cascade*.
  * Bir üst genellikle olun olmadan var olamaz varlıklar için gerekli ilişkilerini, kullandığınız *Cascade* varsayılandır.
* Bir üst öğeye sahip olmayabilir veya varlıkları varsa ve yabancı anahtarı sizin için nulling halletmeniz için EF istediğiniz sonra kullanın *ClientSetNull*
  * Bir üst genellikle olun olmadan bulunabilir varlıklar için isteğe bağlı ilişkilerini, kullandığınız *ClientSetNull* varsayılandır.
  * Veritabanı alt yabancı anahtarlar için null değerler bile yaymak ayrıca denemek isterseniz, alt varlık yüklenmedi, ardından *SetNull*. Ancak, bu veritabanını desteklemesi gerekir ve böyle veritabanını yapılandırma diğer kısıtlamaları neden olabilir, uygulamada genellikle bu seçenek pratik hale getirir unutmayın. Bu nedenle, *SetNull* varsayılan olarak etkin değildir.
* Şimdiye kadar otomatik olarak bir varlığı silmek veya otomatik olarak yabancı anahtar null ardından kullanarak EF Core istemiyorsanız *kısıtlama*. Bu kodunuzu alt varlıklar ve yabancı anahtar değerlerini el ile eşitleyin, gerektiğini unutmayın, aksi takdirde kısıtlaması özel durumları oluşturulacaktır.

> [!NOTE]
> EF6, aksine, EF Core, basamaklı etkileri hemen, ancak bunun yerine yalnızca SaveChanges zaman çağrıldığını durum değil.

> [!NOTE]  
> **EF Core 2.0 içindeki değişiklikleri:** önceki sürümlerde, *kısıtlama* isteğe bağlı bir yabancı anahtar özelliklerini ayarlamak için izlenen bağımlı varlıklarda neden boş ve varsayılan davranışı için isteğe bağlı ilişkileri silme idi. EF Core 2.0 *ClientSetNull* bu davranışı temsil etmek için sunulmuştur ve isteğe bağlı bir ilişki için varsayılan hale geldi. Davranışını *kısıtlama* hiçbir zaman bağımlı varlıkların herhangi yan etkileri olduğu ayarlanır.

## <a name="entity-deletion-examples"></a>Varlık silme örnekleri

Aşağıdaki kod parçası olan bir [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , indirilebilen ve çalıştırın. Örnek bir üst varlık silindiğinde her isteğe bağlıdır ve gerekli ilişkileri silme davranışı için ne olacağını gösterir.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Neler olduğunu anlamak için her değişim atalım.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>Gerekli veya isteğe bağlı bir ilişki ile DeleteBehavior.Cascade

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

* Blog silindi işaretlendi
* Basamaklar SaveChanges kadar gerçekleşecek değil beri gönderileri Unchanged başlangıçta kalır.
* Silme işlemi için hem bağımlılıklar/alt (posta) ve ardından asıl/üst (blog) SaveChanges gönderir
* Artık veritabanından silinmiş olduğundan kaydettikten sonra tüm varlıkları ayrılır

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull ya da gerekli bir ilişki ile DeleteBehavior.SetNull

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

* Blog silindi işaretlendi
* Basamaklar SaveChanges kadar gerçekleşecek değil beri gönderileri Unchanged başlangıçta kalır.
* FK post null olarak ayarlamak SaveChanges çalışır, ancak FK boş değer atanabilir olmadığından bu işlem başarısız

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull veya isteğe bağlı bir ilişki ile DeleteBehavior.SetNull

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

* Blog silindi işaretlendi
* Basamaklar SaveChanges kadar gerçekleşecek değil beri gönderileri Unchanged başlangıçta kalır.
* SaveChanges denemeleri ayarlar hem bağımlılıklar/alt (posta) FK null (blog) asıl/üst silmeden önce
* Kaydetme sonra asıl/üst (blog) silindiği ancak bağımlılıklar/alt (posta) hala izlenir
* İzlenen bağımlılıklar/alt (yazıları), artık null FK değerlere sahip ve Silinen sorumlusu/üst (blog) kendi başvurusu kaldırıldı

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>Gerekli veya isteğe bağlı bir ilişki ile DeleteBehavior.Restrict

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

* Blog silindi işaretlendi
* Basamaklar SaveChanges kadar gerçekleşecek değil beri gönderileri Unchanged başlangıçta kalır.
* Bu yana *kısıtlama* FK otomatik olarak null olarak ayarlamak için EF söyler, null olmayan kalır ve SaveChanges kaydetmeden oluşturur

## <a name="delete-orphans-examples"></a>Artık örnekler delete

Aşağıdaki kod parçası olan bir [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , indirilebilen ve çalıştırın. Örnek, bir üst/sorumlusu ve onun alt öğeleri/dependents arasındaki ilişkiyi yazıyordunuz her isteğe bağlıdır ve gerekli ilişkileri silme davranışı için neler gösterir. Bu örnekte, ilişki (blog) asıl/üst koleksiyon gezinme özelliği gelen bağımlılıklar/alt (posta) kaldırarak yazıyordunuz. Bağımlı/alt öğe başvuru sorumlusu/üst olarak bunun yerine çıkış nulled, ancak aynı davranıştır.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Neler olduğunu anlamak için her değişim atalım.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>Gerekli veya isteğe bağlı bir ilişki ile DeleteBehavior.Cascade

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

* İlişki severing FK null olarak işaretlenmesine neden olduğundan gönderileri değiştirilen işaretlenmiş
  * FK null olabilir değil, null olarak işaretlenmiş olsa bile ardından gerçek değer değişmez
* Silme işlemi için bağımlılıklar/alt (posta) SaveChanges gönderir
* Artık veritabanından silinmiş olduğundan kaydettikten sonra bağımlılıklar/alt (posta) ayrılmış

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull ya da gerekli bir ilişki ile DeleteBehavior.SetNull

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

* İlişki severing FK null olarak işaretlenmesine neden olduğundan gönderileri değiştirilen işaretlenmiş
  * FK null olabilir değil, null olarak işaretlenmiş olsa bile ardından gerçek değer değişmez
* FK post null olarak ayarlamak SaveChanges çalışır, ancak FK boş değer atanabilir olmadığından bu işlem başarısız

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull veya isteğe bağlı bir ilişki ile DeleteBehavior.SetNull

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

* İlişki severing FK null olarak işaretlenmesine neden olduğundan gönderileri değiştirilen işaretlenmiş
  * FK null olabilir değil, null olarak işaretlenmiş olsa bile ardından gerçek değer değişmez
* Her iki bağımlılıklar/alt (posta) FK SaveChanges null olarak ayarlar
* Kaydetme sonra bağımlılıklar/alt (yazıları), artık null FK değerlere sahip ve Silinen sorumlusu/üst (blog) kendi başvurusu kaldırıldı

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>Gerekli veya isteğe bağlı bir ilişki ile DeleteBehavior.Restrict

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

* İlişki severing FK null olarak işaretlenmesine neden olduğundan gönderileri değiştirilen işaretlenmiş
  * FK null olabilir değil, null olarak işaretlenmiş olsa bile ardından gerçek değer değişmez
* Bu yana *kısıtlama* FK otomatik olarak null olarak ayarlamak için EF söyler, null olmayan kalır ve SaveChanges kaydetmeden oluşturur

## <a name="cascading-to-untracked-entities"></a>İzlenmeyen varlıklara geçişli

Çağırdığınızda *SaveChanges*, bağlam tarafından izlenen herhangi bir varlık art arda silme kuralları uygulanır. Bu durum tüm, yukarıda gösterilen örnek olduğu SQL hem sorumlusu/üst (blog) ve tüm bağımlılıklar/alt öğeler (posta) silmek için oluşturulan neden:

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Asıl yüklenen--yalnızca örneğin, ne zaman bir sorgu yapılır bir blog bir `Include(b => b.Posts)` gönderileri--de içerecek şekilde sonra SaveChanges yalnızca asıl/üst silmek için SQL oluşturur:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Bağımlılıklar/alt (posta), yalnızca yapılandırılan karşılık gelen bir cascade davranışına veritabanı varsa, silinecek. EF veritabanını oluşturmak için kullanıyorsanız, bu cascade davranışı için Kurulum olacaktır.
