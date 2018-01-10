---
title: "Basamaklı silme - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: a9481fe851cc264ab3eaecad052c2e683ae57a44
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2017
---
# <a name="cascade-delete"></a>Art arda silme

Art arda silme ilişkili satırları silme işlemini otomatik olarak tetikleyecek bir satırın silme işlemine izin veren bir özellik açıklamak için Veritabanı terminolojisinde yaygın olarak kullanılır. Ayrıca EF çekirdek delete davranışları tarafından kapsanan bir yakından ilgili ilişki üst olduğunda alt varlık otomatik olarak silinmesini zarar görmesi--genellikle "artık silme" olarak bilinen bu i kavramdır.

EF çekirdek birkaç farklı silme davranışı uygular ve tek tek ilişkileri Sil davranışlarını yapılandırmasını sağlar. EF çekirdek de otomatik olarak yararlı varsayılan silme davranışlar [requiredness ilişkinin üzerinde](.. /Modeling/relationships.MD#Required-and-Optional-relationships) tabanlı her ilişki için yapılandırma kuralları uygular.

## <a name="delete-behaviors"></a>Davranışları Sil
Silme davranışları tanımlanmış *DeleteBehavior* Numaralandırıcı yazın ve için geçirilen *OnDelete* denetlemek için fluent API olup olmadığını silinmesi asıl/üst varlık veya, severing bağımlı/alt varlıkları ilişkisi bağımlı/alt varlıklarını bir yan etkisi olması gerekir.

EF asıl/üst varlık silinmiş veya alt ilişkisi zarar görmesi üç eylemleri şunlardır:
* Alt/bağımlı silinebilir
* Çocuğunuzun yabancı anahtar değerleri ayarlanabilir null
* Alt değişmeden kalır

> [!NOTE]  
> EF çekirdek modelinde yapılandırılmış silme davranışı, yalnızca asıl varlık EF çekirdek kullanarak silinir ve bağımlı varlıkları (yani için izlenen bağımlıları) bellekte yüklenen uygulanır. Karşılık gelen bir cascade davranış bağlamdan izlenmiyor veri emin olmak için veritabanını kurulumunda uygulanan gerekli eyleme sahip olması gerekir. Veritabanını oluşturmak için EF çekirdek kullanırsanız, bu cascade davranış Kurulumu olacaktır.

Bir yabancı anahtar değeri null olarak ayarlama özelliği Yukarıdaki ikinci eylemi için yabancı anahtar null değilse, geçerli değil. (Null bir yabancı anahtar için gerekli bir ilişki eşdeğerdir.) Bu durumlarda, değişikliği veritabanına kalıcı olamayacağından aynı zamanda bir özel durum SaveChanges çağrılıncaya kadar yabancı anahtar özelliği null işaretlenmiş olduğundan EF çekirdek izler. Bu, bir kısıtlama ihlali veritabanından alma için benzer.

Dört silme davranışı, aşağıdaki tablolarda listelenen gibi vardır. İsteğe bağlı ilişkiler (boş değer atanabilir yabancı anahtar), _olan_ olası, aşağıdaki efektler sonuçları bir null yabancı anahtar değeri kaydetmek:

| Davranış adı | Bağımlı/alt bellekte etkisi | Bağımlı/alt veritabanı üzerinde etkisi
|-|-|-
| **CASCADE** | Varlıkları silinir | Varlıkları silinir
| **ClientSetNull** (varsayılan) | Yabancı anahtar özellikleri null | Yok
| **SetNull** | Yabancı anahtar özellikleri null | Yabancı anahtar özellikleri null
| **Kısıtlama** | Yok | Yok

Gerekli ilişkileri (null yabancı anahtar) olduğu _değil_ olası, aşağıdaki efektler sonuçları bir null yabancı anahtar değeri kaydetmek:

| Davranış adı | Bağımlı/alt bellekte etkisi | Bağımlı/alt veritabanı üzerinde etkisi
|-|-|-
| **CASCADE** (varsayılan) | Varlıkları silinir | Varlıkları silinir
| **ClientSetNull** | SaveChanges oluşturur | Yok
| **SetNull** | SaveChanges oluşturur | SaveChanges oluşturur
| **Kısıtlama** | Yok | Yok

Yukarıdaki tablolarda *hiçbiri* bir kısıtlama ihlali neden olabilir. Örneğin, bir asıl/alt varlık silinir, ancak bağımlı/alt yabancı anahtarı değiştirmek için hiçbir işlem yapılmadı, sonra veritabanını olasılıkla SaveChanges üzerinde bir yabancı kısıtlaması ihlali nedeniyle durum oluşturur.

Yüksek düzeyde:
* Bir üst var olamaz varlıklar varsa ve alt öğelerini otomatik olarak silmek için halletmeniz için EF istediğiniz sonra kullanın *Cascade*.
  * Bir üst genellikle olun olmadan var olamaz varlıklar için gerekli ilişkilerini, kullandığınız *Cascade* varsayılandır.
* Bir üst öğeye sahip değil veya varlıklar varsa ve yabancı anahtar sizin için nulling halletmeniz için EF istediğiniz sonra kullanın *ClientSetNull*
  * Bir üst genellikle olun olmadan bulunabilir varlıklar için isteğe bağlı ilişkilerini, kullandığınız *ClientSetNull* varsayılandır.
  * Alt yabancı anahtarlar için null değerler bile yaymak de denemek için veritabanı isterseniz, alt varlık yüklenmedi, ardından *SetNull*. Ancak, veritabanı bu desteklemesi gerekir ve bu gibi veritabanı yapılandırma diğer kısıtlamaları neden olabilir, uygulamada genellikle bu seçenek pratik hale getirir unutmayın. Nedeni budur *SetNull* varsayılan olarak etkin değildir.
* EF çekirdeğini herhangi bir varlık otomatik olarak silin veya otomatik olarak yabancı anahtar null sonra kullanmak istemiyorsanız, *sınırla*. Bu kodunuzu alt varlıkları ve yabancı anahtar değerleri eşitleme el ile tutmanızı gerektirdiğini unutmayın, aksi takdirde kısıtlaması özel durumlar oluşturulacaktır.

> [!NOTE]
> EF çekirdek, EF6, aksine, geçişli etkileri hemen, ancak bunun yerine yalnızca SaveChanges zaman çağrıldığını durum değil.

> [!NOTE]  
> **EF çekirdek 2.0 değişiklikleri:** önceki sürümlerde *sınırla* isteğe bağlı yabancı anahtar özelliklerini ayarlamak için izlenen bağımlı varlıklarında neden olur null ve varsayılan davranışı isteğe bağlı ilişkileri silin oluştu. EF çekirdek 2. 0'da, *ClientSetNull* bu davranışı göstermek için sunulmuştur ve isteğe bağlı ilişkiler için varsayılan hale geldi. Davranışını *sınırla* hiçbir zaman bağımlı varlıklarını herhangi yan etkileri için ayarlanmış.

## <a name="entity-deletion-examples"></a>Varlık silme örnekleri

Aşağıdaki kod parçası olan bir [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) bir çalışma indirilebilir. Örnek bir üst varlık silindiğinde her silme davranışı isteğe bağlıdır ve gerekli ilişkiler için ne olacağını gösterir.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Şimdi neler olduğunu anlamak için her değişim yol.

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

* Blog silinmiş işaretlenmiş
* Basamaklar SaveChanges kadar durum değil bu yana gönderileri Unchanged başlangıçta kalır.
* Silme işlemi için hem bağımlıları/alt öğe (postaları) ve ardından asıl/üst (blog) SaveChanges gönderir
* Şimdi veritabanından silinmiş olduğundan kaydettikten sonra tüm varlıklar ayrılmış

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

* Blog silinmiş işaretlenmiş
* Basamaklar SaveChanges kadar durum değil bu yana gönderileri Unchanged başlangıçta kalır.
* SaveChanges FK post null değerine ayarlamaya çalışır, ancak FK boş değer atanabilir olmadığından bu başarısız

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

* Blog silinmiş işaretlenmiş
* Basamaklar SaveChanges kadar durum değil bu yana gönderileri Unchanged başlangıçta kalır.
* SaveChanges denemeleri ayarlar her iki bağımlıları/alt (postaları) FK null asıl/üst (blog) silmeden önce
* Kaydetme sonra asıl/üst (blog) silinir, ancak bağımlıları/alt (postaları) hala izlenir
* İzlenen bağımlıları/alt (postaları) şimdi null FK değerlere sahip ve bunların silinen asıl/üst (blog) referansı kaldırılmıştır

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

* Blog silinmiş işaretlenmiş
* Basamaklar SaveChanges kadar durum değil bu yana gönderileri Unchanged başlangıçta kalır.
* Bu yana *sınırla* otomatik olarak FK null olarak ayarlamak için EF söyler null olmayan kalır ve SaveChanges kaydetmeden oluşturur

## <a name="delete-orphans-examples"></a>Artık örnekler Sil

Aşağıdaki kod parçası olan bir [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) bir çalışma indirilebilir. Örnek bir üst/asıl ve kendi alt öğelerini/dependents arasındaki ilişkiyi zarar görmesi halinde her silme davranışı isteğe bağlıdır ve gerekli ilişkiler için ne olacağını gösterir. Bu örnekte, koleksiyon gezinti özelliği asıl/üst (blog) bağımlıları/alt (postaları) kaldırarak ilişki zarar görmesi. Bağımlı/alt başvurusundan asıl/üst için bunun yerine çıkışı nulled ancak davranışı aynı olur.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Şimdi neler olduğunu anlamak için her değişim yol.

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

* İlişki severing boş olarak işaretlenecek FK neden olduğundan gönderileri değiştirilen işaretlenmiş
  * FK null değilse, boş olarak işaretlenmiş olsa bile sonra gerçek değer değişmez
* Silme işlemi için bağımlılıklar/alt öğe (postaları) SaveChanges gönderir
* Şimdi veritabanından silinmiş olduğundan kaydettikten sonra bağımlılıklar/alt (postaları) ayrılmış

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

* İlişki severing boş olarak işaretlenecek FK neden olduğundan gönderileri değiştirilen işaretlenmiş
  * FK null değilse, boş olarak işaretlenmiş olsa bile sonra gerçek değer değişmez
* SaveChanges FK post null değerine ayarlamaya çalışır, ancak FK boş değer atanabilir olmadığından bu başarısız

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

* İlişki severing boş olarak işaretlenecek FK neden olduğundan gönderileri değiştirilen işaretlenmiş
  * FK null değilse, boş olarak işaretlenmiş olsa bile sonra gerçek değer değişmez
* Her iki bağımlıları/alt (postaları) FK SaveChanges null olarak ayarlar
* Kaydetme sonra bağımlıları/alt (postaları) şimdi null FK değerlere sahip ve bunların silinen asıl/üst (blog) referansı kaldırılmıştır

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

* İlişki severing boş olarak işaretlenecek FK neden olduğundan gönderileri değiştirilen işaretlenmiş
  * FK null değilse, boş olarak işaretlenmiş olsa bile sonra gerçek değer değişmez
* Bu yana *sınırla* otomatik olarak FK null olarak ayarlamak için EF söyler null olmayan kalır ve SaveChanges kaydetmeden oluşturur

## <a name="cascading-to-untracked-entities"></a>İzlenmeyen varlıklara geçişli

Çağırdığınızda *SaveChanges*, bağlam tarafından izlenen herhangi bir varlık cascade delete kuralları uygulanır. Bu tüm durumdur yukarıda gösterilen örnek olduğu SQL asıl/üst (blog) ve tüm bağımlıları/alt (postaları) silmek için oluşturulan neden:

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Asıl yüklenen--yalnızca örneğin, ne zaman bir sorgu için yapılan bir blog bir `Include(b => b.Posts)` gönderileri--de içerecek şekilde sonra SaveChanges yalnızca asıl/üst silmek için SQL oluşturur:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Veritabanında yapılandırılan karşılık gelen bir cascade davranışına varsa bağımlıları/alt (postaları) yalnızca silinir. Veritabanını oluşturmak için EF kullanırsanız, bu cascade davranış Kurulumu olacaktır.
