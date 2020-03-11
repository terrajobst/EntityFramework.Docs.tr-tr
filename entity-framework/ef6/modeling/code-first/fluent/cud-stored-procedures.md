---
title: Code First saklı yordamları ekleme, güncelleştirme ve silme-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: bfc56671814aec1965ac054ff901297e5cdbbecb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419089"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>Code First saklı yordamları ekleme, güncelleştirme ve silme
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.  

Code First, varsayılan olarak, tüm varlıkları doğrudan tablo erişimini kullanarak INSERT, Update ve DELETE komutlarını gerçekleştirecek şekilde yapılandırır. EF6 ' den başlayarak, modelinizdeki bazı veya tüm varlıklar için Code First modelinizi saklı yordamlar kullanacak şekilde yapılandırabilirsiniz.  

## <a name="basic-entity-mapping"></a>Temel varlık eşleme  

Akıcı API kullanarak INSERT, Update ve delete için saklı yordamları kullanmayı tercih edebilirsiniz.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

Bunu yapmak Code First, saklı yordamların veritabanında beklenen şeklini oluşturmak için bazı kuralları kullanmasına neden olur.  

- **\<type_name\>_Insert**, **\<** type_name\>_Update ve **\<** type_name\>_Delete (örneğin, Blog_Insert, Blog_Update ve Blog_Delete) adlı üç saklı yordam.  
- Parametre adları, özellik adlarına karşılık gelir.  
  > [!NOTE]
  > Verilen bir özelliğin sütununu yeniden adlandırmak için Hasccolumnname () veya Column özniteliğini kullanırsanız, bu ad, özellik adı yerine parametreler için kullanılır.  
- **Ekle saklı yordamı** her özellik için bir parametreye sahip olur, örneğin, oluşturulan depo olarak işaretlenmiş (kimlik veya hesaplanan). Saklı yordam, her mağaza tarafından oluşturulan özellik için bir sütun içeren bir sonuç kümesi döndürmelidir.  
- **Güncelleştirme saklı yordamı** , bir depo tarafından oluşturulan ' hesaplanan ' bir örüntüle işaretlenenler hariç her özellik için bir parametreye sahip olacaktır. Bazı eşzamanlılık belirteçleri özgün değer için bir parametre gerektirir, Ayrıntılar için aşağıdaki *eşzamanlılık belirteçleri* bölümüne bakın. Saklı yordam, hesaplanan her özellik için bir sütun içeren bir sonuç kümesi döndürmelidir.  
- **Delete saklı yordamı** , varlığın anahtar değeri için bir parametreye sahip olmalıdır (veya varlık bir bileşik anahtar içeriyorsa birden fazla parametre). Ayrıca, silme yordamının aynı zamanda hedef tablodaki bağımsız Association yabancı anahtarları için parametreler olması gerekir (varlık içinde bildirildiği karşılık gelen yabancı anahtar özelliklerine sahip olmayan ilişkiler). Bazı eşzamanlılık belirteçleri özgün değer için bir parametre gerektirir, Ayrıntılar için aşağıdaki *eşzamanlılık belirteçleri* bölümüne bakın.  

Örnek olarak aşağıdaki sınıfı kullanma:  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

Varsayılan saklı yordamlar şöyle olacaktır:  

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

Varsayılan olarak yapılandırılmış olan bölümü veya tümünü geçersiz kılabilirsiniz.  

Bir veya daha fazla saklı yordamın adını değiştirebilirsiniz. Bu örnek yalnızca Update saklı yordamını yeniden adlandırır.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

Bu örnek, üç saklı yordamın hepsini yeniden adlandırır.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

Bu örneklerde çağrılar birlikte zincirleme, ancak lambda blok sözdizimini de kullanabilirsiniz.  

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

Bu örnek, Update saklı yordamındaki blogID özelliğinin parametresini yeniden adlandırır.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

Bu çağrılar, tüm chainable ve birleştirilebilir. Aşağıda, üç saklı yordamın ve bunların parametrelerinin yeniden adlandırdıkları bir örnek verilmiştir.  

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

Sonuç kümesinde, veritabanı tarafından oluşturulan değerleri içeren sütunların adını da değiştirebilirsiniz.  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>Sınıfta yabancı anahtar olmadan ilişkiler (bağımsız Ilişkilendirmeler)  

Bir yabancı anahtar özelliği sınıf tanımına dahil edildiğinde, karşılık gelen parametre diğer tüm özellikler ile aynı şekilde yeniden adlandırılabilir. Sınıfında yabancı anahtar özelliği olmadan bir ilişki varsa, varsayılan parametre adı **\<navigation_property_name\>_\<primary_key_name\>** .  

Örneğin, aşağıdaki sınıf tanımları, gönderimler eklemek ve güncelleştirmek için saklı yordamlarda beklenen bir Blog_BlogId parametresi oluşmasına neden olur.  

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

Parametre yöntemine birincil anahtar özelliğinin yolunu sağlayarak sınıfına dahil olmayan yabancı anahtarların parametrelerini değiştirebilirsiniz.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

Bağımlı varlık üzerinde bir gezinti özelliği yoksa (ör. Post. blog özelliği yok) ilişki yöntemini kullanarak ilişkinin diğer sonunu tanımlayabilir ve ardından her anahtar özelliğin (ler) öğesine karşılık gelen parametreleri yapılandırabilirsiniz.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>Eşzamanlılık Belirteçleri  

Saklı yordamları güncelleştir ve Sil eşzamanlılık ile de uğraşmak gerekebilir:  

- Varlık eşzamanlılık belirteçleri içeriyorsa, saklı yordam isteğe bağlı olarak, güncellenen/silinen satır sayısını döndüren bir çıkış parametresine sahip olabilir (etkilenen satırlar). Bu tür bir parametre RowsAffectedParameter yöntemi kullanılarak yapılandırılmalıdır.  
Varsayılan olarak, bir kaç satır etkilendiğini öğrenmek için ExecuteNonQuery 'den dönüş değeri kullanır. Sproc üzerinde bir mantık gerçekleştirdiğinizde, bir satır için etkilenen çıktı parametresi, yürütme sonunda ExecuteNonQuery 'nin dönüş değeri yanlış (EF perspektifinden) ile sonuçlanacaktır.  
- Her eşzamanlılık belirteci için, **\<property_name\>_original** adlı bir parametre (örneğin, Timestamp_Original) olacaktır. Bu, bu özelliğin orijinal değerine geçirilir: veritabanından sorgulandığında değeri.  
    - Zaman damgaları gibi veritabanı tarafından hesaplanan eşzamanlılık belirteçleri yalnızca bir özgün değer parametresine sahip olur.  
    - Eşzamanlılık belirteçleri olarak ayarlanan hesaplanamayan özellikler, Update yordamındaki yeni değer için de bir parametreye sahip olur. Bu, yeni değerler için önceden tartıılan adlandırma kurallarını kullanır. Bu tür bir belirtece bir örnek, bir blog belirteci olarak blog URL 'SI kullanıyor olabilir. Bu, kodunuzun (yalnızca veritabanı tarafından güncelleştirilmiş bir zaman damgası belirtecinin aksine) yeni bir değere güncelleştirilemediğinden yeni değer gereklidir.  

Bu örnek bir sınıftır ve saklı yordamı bir zaman damgası eşzamanlılık belirteci ile güncelleştirir.  

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

Aşağıda örnek bir sınıf yer verilmiştir ve saklı yordam hesaplanmayan eşzamanlılık belirteci ile güncelleştirilsin.  

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

İsteğe bağlı olarak etkilenen bir satır parametresi sağlayabilirsiniz.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

Veritabanı tarafından hesaplanan eşzamanlılık belirteçleri – yalnızca özgün değerin geçirildiği yerlerde, özgün değerin parametresini yeniden adlandırmak için yalnızca standart parametre yeniden adlandırma mekanizmasını kullanabilirsiniz.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

Hem özgün hem de yeni değerin geçirildiği, hesaplanmayan eşzamanlılık belirteçleri için – her bir parametre için bir ad vermenizi sağlayan bir parametresinin aşırı yüklemesini kullanabilirsiniz.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>Çoktan Çoğa İlişkiler  

Bu bölümde bir örnek olarak aşağıdaki sınıfları kullanacağız.  

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

Birçok çoğa ilişki, aşağıdaki sözdizimi ile saklı yordamlara eşlenebilir.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

Başka bir yapılandırma sağlanmıyorsa, varsayılan olarak aşağıdaki saklı yordam şekli kullanılır.  

- **\<type_one\>\<type_two\>_Insert** ve\<type_one\> **\<type_two**\>_Delete adlı iki saklı yordam (örneğin, PostTag_Insert ve PostTag_Delete).  
- Parametreler, her tür için anahtar değer (ler) i olacaktır. **\<type_name\>_\<property_name\>** olan her parametrenin adı (örneğin, Post_PostId ve Tag_TagId).

Aşağıda, saklı yordamlar ekleme ve güncelleştirme örnekleri verilmiştir.  

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

Yordam ve parametre adları, varlık saklı yordamlarına benzer bir şekilde yapılandırılabilir.  

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
