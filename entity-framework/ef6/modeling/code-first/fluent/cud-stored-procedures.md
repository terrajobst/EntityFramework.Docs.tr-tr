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
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="fa16e-102">Code First saklı yordamları ekleme, güncelleştirme ve silme</span><span class="sxs-lookup"><span data-stu-id="fa16e-102">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="fa16e-103">**Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="fa16e-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="fa16e-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="fa16e-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="fa16e-105">Code First, varsayılan olarak, tüm varlıkları doğrudan tablo erişimini kullanarak INSERT, Update ve DELETE komutlarını gerçekleştirecek şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="fa16e-105">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="fa16e-106">EF6 ' den başlayarak, modelinizdeki bazı veya tüm varlıklar için Code First modelinizi saklı yordamlar kullanacak şekilde yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa16e-106">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="fa16e-107">Temel varlık eşleme</span><span class="sxs-lookup"><span data-stu-id="fa16e-107">Basic Entity Mapping</span></span>  

<span data-ttu-id="fa16e-108">Akıcı API kullanarak INSERT, Update ve delete için saklı yordamları kullanmayı tercih edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa16e-108">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="fa16e-109">Bunu yapmak Code First, saklı yordamların veritabanında beklenen şeklini oluşturmak için bazı kuralları kullanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="fa16e-109">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="fa16e-110">**\<type_name\>_Insert**, **\<** type_name\>_Update ve **\<** type_name\>_Delete (örneğin, Blog_Insert, Blog_Update ve Blog_Delete) adlı üç saklı yordam.</span><span class="sxs-lookup"><span data-stu-id="fa16e-110">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (for example, Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="fa16e-111">Parametre adları, özellik adlarına karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="fa16e-111">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="fa16e-112">Verilen bir özelliğin sütununu yeniden adlandırmak için Hasccolumnname () veya Column özniteliğini kullanırsanız, bu ad, özellik adı yerine parametreler için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fa16e-112">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="fa16e-113">**Ekle saklı yordamı** her özellik için bir parametreye sahip olur, örneğin, oluşturulan depo olarak işaretlenmiş (kimlik veya hesaplanan).</span><span class="sxs-lookup"><span data-stu-id="fa16e-113">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="fa16e-114">Saklı yordam, her mağaza tarafından oluşturulan özellik için bir sütun içeren bir sonuç kümesi döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="fa16e-114">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="fa16e-115">**Güncelleştirme saklı yordamı** , bir depo tarafından oluşturulan ' hesaplanan ' bir örüntüle işaretlenenler hariç her özellik için bir parametreye sahip olacaktır.</span><span class="sxs-lookup"><span data-stu-id="fa16e-115">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="fa16e-116">Bazı eşzamanlılık belirteçleri özgün değer için bir parametre gerektirir, Ayrıntılar için aşağıdaki *eşzamanlılık belirteçleri* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="fa16e-116">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="fa16e-117">Saklı yordam, hesaplanan her özellik için bir sütun içeren bir sonuç kümesi döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="fa16e-117">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="fa16e-118">**Delete saklı yordamı** , varlığın anahtar değeri için bir parametreye sahip olmalıdır (veya varlık bir bileşik anahtar içeriyorsa birden fazla parametre).</span><span class="sxs-lookup"><span data-stu-id="fa16e-118">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="fa16e-119">Ayrıca, silme yordamının aynı zamanda hedef tablodaki bağımsız Association yabancı anahtarları için parametreler olması gerekir (varlık içinde bildirildiği karşılık gelen yabancı anahtar özelliklerine sahip olmayan ilişkiler).</span><span class="sxs-lookup"><span data-stu-id="fa16e-119">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="fa16e-120">Bazı eşzamanlılık belirteçleri özgün değer için bir parametre gerektirir, Ayrıntılar için aşağıdaki *eşzamanlılık belirteçleri* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="fa16e-120">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="fa16e-121">Örnek olarak aşağıdaki sınıfı kullanma:</span><span class="sxs-lookup"><span data-stu-id="fa16e-121">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="fa16e-122">Varsayılan saklı yordamlar şöyle olacaktır:</span><span class="sxs-lookup"><span data-stu-id="fa16e-122">The default stored procedures would be:</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="fa16e-123">Varsayılanları geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="fa16e-123">Overriding the Defaults</span></span>  

<span data-ttu-id="fa16e-124">Varsayılan olarak yapılandırılmış olan bölümü veya tümünü geçersiz kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa16e-124">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="fa16e-125">Bir veya daha fazla saklı yordamın adını değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa16e-125">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="fa16e-126">Bu örnek yalnızca Update saklı yordamını yeniden adlandırır.</span><span class="sxs-lookup"><span data-stu-id="fa16e-126">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="fa16e-127">Bu örnek, üç saklı yordamın hepsini yeniden adlandırır.</span><span class="sxs-lookup"><span data-stu-id="fa16e-127">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="fa16e-128">Bu örneklerde çağrılar birlikte zincirleme, ancak lambda blok sözdizimini de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa16e-128">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

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

<span data-ttu-id="fa16e-129">Bu örnek, Update saklı yordamındaki blogID özelliğinin parametresini yeniden adlandırır.</span><span class="sxs-lookup"><span data-stu-id="fa16e-129">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="fa16e-130">Bu çağrılar, tüm chainable ve birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="fa16e-130">These calls are all chainable and composable.</span></span> <span data-ttu-id="fa16e-131">Aşağıda, üç saklı yordamın ve bunların parametrelerinin yeniden adlandırdıkları bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fa16e-131">Here is an example that renames all three stored procedures and their parameters.</span></span>  

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

<span data-ttu-id="fa16e-132">Sonuç kümesinde, veritabanı tarafından oluşturulan değerleri içeren sütunların adını da değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa16e-132">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="fa16e-133">Sınıfta yabancı anahtar olmadan ilişkiler (bağımsız Ilişkilendirmeler)</span><span class="sxs-lookup"><span data-stu-id="fa16e-133">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="fa16e-134">Bir yabancı anahtar özelliği sınıf tanımına dahil edildiğinde, karşılık gelen parametre diğer tüm özellikler ile aynı şekilde yeniden adlandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="fa16e-134">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="fa16e-135">Sınıfında yabancı anahtar özelliği olmadan bir ilişki varsa, varsayılan parametre adı **\<navigation_property_name\>_\<primary_key_name\>** .</span><span class="sxs-lookup"><span data-stu-id="fa16e-135">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="fa16e-136">Örneğin, aşağıdaki sınıf tanımları, gönderimler eklemek ve güncelleştirmek için saklı yordamlarda beklenen bir Blog_BlogId parametresi oluşmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="fa16e-136">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="fa16e-137">Varsayılanları geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="fa16e-137">Overriding the Defaults</span></span>  

<span data-ttu-id="fa16e-138">Parametre yöntemine birincil anahtar özelliğinin yolunu sağlayarak sınıfına dahil olmayan yabancı anahtarların parametrelerini değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa16e-138">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="fa16e-139">Bağımlı varlık üzerinde bir gezinti özelliği yoksa (ör.</span><span class="sxs-lookup"><span data-stu-id="fa16e-139">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="fa16e-140">Post. blog özelliği yok) ilişki yöntemini kullanarak ilişkinin diğer sonunu tanımlayabilir ve ardından her anahtar özelliğin (ler) öğesine karşılık gelen parametreleri yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa16e-140">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="fa16e-141">Eşzamanlılık Belirteçleri</span><span class="sxs-lookup"><span data-stu-id="fa16e-141">Concurrency Tokens</span></span>  

<span data-ttu-id="fa16e-142">Saklı yordamları güncelleştir ve Sil eşzamanlılık ile de uğraşmak gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="fa16e-142">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="fa16e-143">Varlık eşzamanlılık belirteçleri içeriyorsa, saklı yordam isteğe bağlı olarak, güncellenen/silinen satır sayısını döndüren bir çıkış parametresine sahip olabilir (etkilenen satırlar).</span><span class="sxs-lookup"><span data-stu-id="fa16e-143">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="fa16e-144">Bu tür bir parametre RowsAffectedParameter yöntemi kullanılarak yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fa16e-144">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="fa16e-145">Varsayılan olarak, bir kaç satır etkilendiğini öğrenmek için ExecuteNonQuery 'den dönüş değeri kullanır.</span><span class="sxs-lookup"><span data-stu-id="fa16e-145">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="fa16e-146">Sproc üzerinde bir mantık gerçekleştirdiğinizde, bir satır için etkilenen çıktı parametresi, yürütme sonunda ExecuteNonQuery 'nin dönüş değeri yanlış (EF perspektifinden) ile sonuçlanacaktır.</span><span class="sxs-lookup"><span data-stu-id="fa16e-146">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="fa16e-147">Her eşzamanlılık belirteci için, **\<property_name\>_original** adlı bir parametre (örneğin, Timestamp_Original) olacaktır.</span><span class="sxs-lookup"><span data-stu-id="fa16e-147">For each concurrency token there will be a parameter named **\<property_name\>_Original** (for example, Timestamp_Original ).</span></span> <span data-ttu-id="fa16e-148">Bu, bu özelliğin orijinal değerine geçirilir: veritabanından sorgulandığında değeri.</span><span class="sxs-lookup"><span data-stu-id="fa16e-148">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="fa16e-149">Zaman damgaları gibi veritabanı tarafından hesaplanan eşzamanlılık belirteçleri yalnızca bir özgün değer parametresine sahip olur.</span><span class="sxs-lookup"><span data-stu-id="fa16e-149">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="fa16e-150">Eşzamanlılık belirteçleri olarak ayarlanan hesaplanamayan özellikler, Update yordamındaki yeni değer için de bir parametreye sahip olur.</span><span class="sxs-lookup"><span data-stu-id="fa16e-150">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="fa16e-151">Bu, yeni değerler için önceden tartıılan adlandırma kurallarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="fa16e-151">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="fa16e-152">Bu tür bir belirtece bir örnek, bir blog belirteci olarak blog URL 'SI kullanıyor olabilir. Bu, kodunuzun (yalnızca veritabanı tarafından güncelleştirilmiş bir zaman damgası belirtecinin aksine) yeni bir değere güncelleştirilemediğinden yeni değer gereklidir.</span><span class="sxs-lookup"><span data-stu-id="fa16e-152">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="fa16e-153">Bu örnek bir sınıftır ve saklı yordamı bir zaman damgası eşzamanlılık belirteci ile güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="fa16e-153">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

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

<span data-ttu-id="fa16e-154">Aşağıda örnek bir sınıf yer verilmiştir ve saklı yordam hesaplanmayan eşzamanlılık belirteci ile güncelleştirilsin.</span><span class="sxs-lookup"><span data-stu-id="fa16e-154">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="fa16e-155">Varsayılanları geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="fa16e-155">Overriding the Defaults</span></span>  

<span data-ttu-id="fa16e-156">İsteğe bağlı olarak etkilenen bir satır parametresi sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa16e-156">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="fa16e-157">Veritabanı tarafından hesaplanan eşzamanlılık belirteçleri – yalnızca özgün değerin geçirildiği yerlerde, özgün değerin parametresini yeniden adlandırmak için yalnızca standart parametre yeniden adlandırma mekanizmasını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa16e-157">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="fa16e-158">Hem özgün hem de yeni değerin geçirildiği, hesaplanmayan eşzamanlılık belirteçleri için – her bir parametre için bir ad vermenizi sağlayan bir parametresinin aşırı yüklemesini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa16e-158">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="fa16e-159">Çoktan Çoğa İlişkiler</span><span class="sxs-lookup"><span data-stu-id="fa16e-159">Many to Many Relationships</span></span>  

<span data-ttu-id="fa16e-160">Bu bölümde bir örnek olarak aşağıdaki sınıfları kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="fa16e-160">We’ll use the following classes as an example in this section.</span></span>  

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

<span data-ttu-id="fa16e-161">Birçok çoğa ilişki, aşağıdaki sözdizimi ile saklı yordamlara eşlenebilir.</span><span class="sxs-lookup"><span data-stu-id="fa16e-161">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="fa16e-162">Başka bir yapılandırma sağlanmıyorsa, varsayılan olarak aşağıdaki saklı yordam şekli kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fa16e-162">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="fa16e-163">**\<type_one\>\<type_two\>_Insert** ve\<type_one\> **\<type_two**\>_Delete adlı iki saklı yordam (örneğin, PostTag_Insert ve PostTag_Delete).</span><span class="sxs-lookup"><span data-stu-id="fa16e-163">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (for example, PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="fa16e-164">Parametreler, her tür için anahtar değer (ler) i olacaktır.</span><span class="sxs-lookup"><span data-stu-id="fa16e-164">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="fa16e-165">**\<type_name\>_\<property_name\>** olan her parametrenin adı (örneğin, Post_PostId ve Tag_TagId).</span><span class="sxs-lookup"><span data-stu-id="fa16e-165">The name of each parameter being **\<type_name\>_\<property_name\>** (for example, Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="fa16e-166">Aşağıda, saklı yordamlar ekleme ve güncelleştirme örnekleri verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fa16e-166">Here are example insert and update stored procedures.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="fa16e-167">Varsayılanları geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="fa16e-167">Overriding the Defaults</span></span>  

<span data-ttu-id="fa16e-168">Yordam ve parametre adları, varlık saklı yordamlarına benzer bir şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="fa16e-168">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

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
