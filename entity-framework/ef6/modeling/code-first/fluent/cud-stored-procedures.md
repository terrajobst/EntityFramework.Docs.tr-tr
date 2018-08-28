---
title: İlk kod ekleme, güncelleştirme ve saklı yordamlar - EF6 silme
author: divega
ms.date: 2016-10-23
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: a0448fb44dabb2e03b2358aa7b4f69d92cffa15a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994579"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>İlk kod ekleme, güncelleştirme ve saklı yordamlar silme
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.  

Varsayılan olarak, Code First INSERT işlemi, güncelleştirme ve silme komutları doğrudan Tablo erişimini kullanarak tüm varlıklar yapılandıracaksınız. EF6 ', başlatma, Code First modeli modelinizdeki bazı veya tüm varlıklar için saklı yordamları kullanmak için yapılandırabilirsiniz.  

## <a name="basic-entity-mapping"></a>Temel varlık eşleme  

INSERT saklı yordamlar kullanmayı seçen, güncelleştirme ve Fluent API'sini kullanarak silin.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

Bunun yapılması beklenen şeklini veritabanında saklı yordamları oluşturmak için bazı kurallar kullanmak Code First neden olur.  

- Üç saklı yordamlar adlı  **\<type_name\>_ekle**,  **\<type_name\>_güncelleştirme** ve  **\<type_ adı\>_sil** (örneğin, Blog_Insert, Blog_Update ve Blog_Delete).  
- Parametre adları, özellik adlara karşılık gelir.  
  > [!NOTE]
  > Belirli bir özellik için bir sütunu yeniden adlandırmak için HasColumnName() veya sütun özniteliğini kullanırsanız, bu ad özelliği adı yerine parametreleri için kullanılır.  
- **INSERT saklı yordamı** oluşturulan depolama işaretlenmiş hariç her bir özellik için bir parametre gerekir (kimlik veya hesaplanan). Saklı yordamı bir sonuç deposu oluşturulan her bir özellik için bir sütun kümesi döndürmelidir.  
- **Update saklı yordamı** bir 'Computed' oluşturulan depolama deseniyle işaretlenenler dışında her bir özellik için bir parametreye sahip. Bazı eşzamanlılık belirteçleri ilk değeri bir parametre gerekli, bkz: *eşzamanlılık belirteçleri* ayrıntılarını bölümü altında. Saklı yordam her hesaplanan bir özellik için bir sütun kümesi bir sonuç döndürmesi gerekir.  
- **Delete saklı yordamı** varlık (veya varlık bir bileşik anahtarı varsa, birden çok parametre) anahtar değeri için bir parametre olmalıdır. Ayrıca, delete yordamı hedef tablodaki (varlık içinde bildirilen, karşılık gelen, yabancı anahtar özelliklerini olmayan ilişkiler) da bağımsız ilişkilendirme yabancı anahtarları için parametreleri olmalıdır. Bazı eşzamanlılık belirteçleri ilk değeri bir parametre gerekli, bkz: *eşzamanlılık belirteçleri* ayrıntılarını bölümü altında.  

Aşağıdaki sınıftan bir örnek olarak kullanma:  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

Varsayılan saklı yordamlar şu şekilde olur:  

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS BlogId
END
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId;
CREATE PROCEDURE [dbo].[Blog_Delete]  
  @BlogId int  
AS  
  DELETE FROM [dbo].[Blogs]
  WHERE BlogId = @BlogId
```  

### <a name="overriding-the-defaults"></a>Varsayılanları geçersiz kılma  

Kısmını veya tamamını varsayılan olarak yapılandırılmış geçersiz kılabilirsiniz.  

Bir veya daha fazla saklı yordamlar adını değiştirebilirsiniz. Bu örnekte, yalnızca update kayıtlı yordamı yeniden adlandırır.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

Bu örnekte, tüm üç saklı yordamlar yeniden adlandırır.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

Bu örneklerde çağrıları birlikte Zincirli olan, ancak lambda blok söz dizimi de kullanabilirsiniz.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    {  
      s.Update(u => u.HasName("modify_blog"));  
      s.Delete(d => d.HasName("delete_blog"));  
      s.Insert(i => i.HasName("insert_blog"));  
    });
```  

Bu örnekte parametre update kayıtlı yordamı BlogId özelliği için yeniden adlandırır.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

Bu, tüm chainable ve birleştirilebilir çağrılarıdır. Üç tüm saklı yordamları ve parametreleri yeniden adlandırır bir örnek aşağıda verilmiştir.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")  
                   .Parameter(b => b.BlogId, "blog_id")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url"))  
     .Delete(d => d.HasName("delete_blog")  
                   .Parameter(b => b.BlogId, "blog_id"))  
     .Insert(i => i.HasName("insert_blog")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url")));
```  

Oluşturulan veritabanı değerlerini içeren bir sonuç kümesi sütun adını da değiştirebilirsiniz.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures(s =>
    s.Insert(i => i.Result(b => b.BlogId, "generated_blog_identity")));
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS generated_blog_id
END
```  

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>Olmadan (bağımsız ilişkileri) sınıfında yabancı anahtar ilişkileri  

Bir yabancı anahtar özelliği, sınıf tanımında dahil edilirse, karşılık gelen parametre tıpkı diğer herhangi bir özelliği olarak adlandırılabilir. Bir yabancı anahtar özellik sınıfında olmadan bir ilişki mevcut olduğunda, varsayılan parametre adı.  **\<navigation_property_name\>_\<primary_key_name\>**.  

Örneğin, aşağıdaki sınıf tanımları eklemek ve iletileri güncelleştirmek için saklı yordamları beklenen Blog_BlogId parametresinde neden olur.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }

  public List<Post> Posts { get; set; }  
}  

public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public Blog Blog { get; set; }  
}
```  

### <a name="overriding-the-defaults"></a>Varsayılanları geçersiz kılma  

Birincil anahtar özelliği parametre yönteme yolunu sağlayarak sınıfında yer almayan yabancı anahtarlar parametrelerini değiştirebilirsiniz.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

Bağımlı varlık (yani bir gezinti özelliği yoksa hiçbir Post.Blog özelliği) ilişkilendirme yöntemi, ilişkinin diğer ucundaki belirleyin ve ardından her anahtar özellik karşılık gelen parametreleri yapılandırmak için kullanabilirsiniz.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>Eşzamanlılık belirteçleri  

Update ve delete saklı yordamları eşzamanlılık ile dağıtılacak da gerekebilir:  

- Varlık eşzamanlılık belirteçleri varsa, saklı yordam isteğe bağlı olarak güncelleştirilen/silinen satır (etkilenen satır) sayısını döndüren bir output parametresi olabilir. Bu tür RowsAffectedParameter yöntemi kullanılarak yapılandırılması gerekir.  
Varsayılan olarak EF kaç satır etkilendiğini belirlemek için dönüş değeri ExecuteNonQuery kullanır. Satırlardan etkilenen çıkış parametresi ExecuteNonQuery (EF'ın açısından) için yanlış olan dönüş değeri neden olacağından, sproc herhangi bir mantık gerçekleştirmek istiyorsanız, kullanışlı belirtme yürütme sonunda.  
- Her eşzamanlılık belirteci yok adlı bir parametre olacak  **\<property_name\>_Original** (örneğin, Timestamp_Original). Bu özelliğin – veritabanından sorgulandığında değer özgün değeri geçirilir.  
    - Zaman damgaları gibi– – veritabanı tarafından hesaplanan eşzamanlılık belirteçleri, yalnızca özgün bir değer parametresi sahip olur.  
    - Eşzamanlılık belirteçleri ayarlanan olmayan hesaplanan özellikler, güncelleştirme yordamı da yeni bir değer için bir parametre gerekir. Bu yeni değerleri için zaten ele adlandırma kurallarını kullanır. Böyle bir belirteç örneği bir Blog URL bir eşzamanlılık belirteci olarak kullanılmasına, kodunuzu (aksine, yalnızca bir veritabanı tarafından güncelleştirilen zaman damgası belirteç) tarafından yeni bir değer bu güncelleştirilebilir olduğundan yeni bir değer gereklidir.  

Bir örnek sınıf ve saklı yordam zaman damgası eşzamanlı bir simge ile güncelleştirin.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
  [Timestamp]
  public byte[] Timestamp { get; set; }
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Timestamp_Original rowversion  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Timestamp] = @Timestamp_Original
```  

İşte bir örnek sınıf ve saklı yordam hesaplanmayan eşzamanlılık belirteci ile güncelleştirin.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  [ConcurrencyCheck]
  public string Url { get; set; }  
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Url_Original nvarchar(max),
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Url] = @Url_Original
```  

### <a name="overriding-the-defaults"></a>Varsayılanları geçersiz kılma  

Satırlardan etkilenen parametresi isteğe bağlı olarak çıkarabilir.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

Hesaplanan Veritabanı eşzamanlılık belirteçleri için – yalnızca özgün değer geçirildiği – yalnızca mekanizması yeniden adlandırma standart parametresi parametresi özgün değeri için yeniden adlandırmak için de kullanabilirsiniz.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

– Nerede hem özgün hem yeni değeri geçirilir – hesaplanmayan eşzamanlılık belirteçleri için bir aşırı yüklemesini her parametre için bir ad sağlayın olanak tanıyan parametresini kullanabilirsiniz.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>Çoktan Çoğa İlişkiler  

Bu bölümde örnek olarak aşağıdaki sınıflar kullanacağız.  

``` csharp
public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public List<Tag> Tags { get; set; }  
}  

public class Tag  
{  
  public int TagId { get; set; }  
  public string TagName { get; set; }  

  public List<Post> Posts { get; set; }  
}
```  

Çok-çok ilişkileri saklı yordamlar aşağıdaki söz dizimi ile eşlenebilir.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

Sonra başka bir yapılandırma belirtilirse aşağıdaki saklı yordamı şekil varsayılan olarak kullanılır.  

- İki saklı yordamlar adlı  **\<type_one\>\<type_two\>_ekle** ve  **\<type_one\>\<type_two \>_Sil** (örneğin, PostTag_Insert ve PostTag_Delete).  
- Parametreleri her türü için anahtar değerleri olacaktır. Her parametre olma adı **\<type_name\>_\<property_name\>** (örneğin, Post_PostId ve Tag_TagId).

Örnek İşte ekleme ve saklı yordamları güncelleştirme.  

``` SQL
CREATE PROCEDURE [dbo].[PostTag_Insert]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  INSERT INTO [dbo].[Post_Tags] (Post_PostId, Tag_TagId)   
  VALUES (@Post_PostId, @Tag_TagId)
CREATE PROCEDURE [dbo].[PostTag_Delete]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  DELETE FROM [dbo].[Post_Tags]    
  WHERE Post_PostId = @Post_PostId AND Tag_TagId = @Tag_TagId
```  

### <a name="overriding-the-defaults"></a>Varsayılanları geçersiz kılma  

Yordam ve parametre adları, varlık saklı yordamlar için benzer bir şekilde yapılandırılabilir.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.HasName("add_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id"))  
     .Delete(d => d.HasName("remove_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id")));
```  
