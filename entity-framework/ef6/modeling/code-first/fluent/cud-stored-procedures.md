---
title: İlk kod ekleme, güncelleştirme ve saklı yordamlar - EF6 silme
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
caps.latest.revision: 3
ms.openlocfilehash: 1f100ed888abd98df83c80d0de2086cfb1ba7b4f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949085"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="8aca8-102">İlk kod ekleme, güncelleştirme ve saklı yordamlar silme</span><span class="sxs-lookup"><span data-stu-id="8aca8-102">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="8aca8-103">**EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="8aca8-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="8aca8-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="8aca8-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="8aca8-105">Varsayılan olarak, Code First INSERT işlemi, güncelleştirme ve silme komutları doğrudan Tablo erişimini kullanarak tüm varlıklar yapılandıracaksınız.</span><span class="sxs-lookup"><span data-stu-id="8aca8-105">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="8aca8-106">EF6 ', başlatma, Code First modeli modelinizdeki bazı veya tüm varlıklar için saklı yordamları kullanmak için yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8aca8-106">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="8aca8-107">Temel varlık eşleme</span><span class="sxs-lookup"><span data-stu-id="8aca8-107">Basic Entity Mapping</span></span>  

<span data-ttu-id="8aca8-108">INSERT saklı yordamlar kullanmayı seçen, güncelleştirme ve Fluent API'sini kullanarak silin.</span><span class="sxs-lookup"><span data-stu-id="8aca8-108">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="8aca8-109">Bunun yapılması beklenen şeklini veritabanında saklı yordamları oluşturmak için bazı kurallar kullanmak Code First neden olur.</span><span class="sxs-lookup"><span data-stu-id="8aca8-109">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="8aca8-110">Üç saklı yordamlar adlı  **\<type_name\>_ekle**,  **\<type_name\>_güncelleştirme** ve  **\<type_ adı\>_sil** (örneğin, Blog_Insert, Blog_Update ve Blog_Delete).</span><span class="sxs-lookup"><span data-stu-id="8aca8-110">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (for example, Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="8aca8-111">Parametre adları, özellik adlara karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="8aca8-111">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="8aca8-112">Belirli bir özellik için bir sütunu yeniden adlandırmak için HasColumnName() veya sütun özniteliğini kullanırsanız, bu ad özelliği adı yerine parametreleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8aca8-112">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="8aca8-113">**INSERT saklı yordamı** oluşturulan depolama işaretlenmiş hariç her bir özellik için bir parametre gerekir (kimlik veya hesaplanan).</span><span class="sxs-lookup"><span data-stu-id="8aca8-113">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="8aca8-114">Saklı yordamı bir sonuç deposu oluşturulan her bir özellik için bir sütun kümesi döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="8aca8-114">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="8aca8-115">**Update saklı yordamı** bir 'Computed' oluşturulan depolama deseniyle işaretlenenler dışında her bir özellik için bir parametreye sahip.</span><span class="sxs-lookup"><span data-stu-id="8aca8-115">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="8aca8-116">Bazı eşzamanlılık belirteçleri ilk değeri bir parametre gerekli, bkz: *eşzamanlılık belirteçleri* ayrıntılarını bölümü altında.</span><span class="sxs-lookup"><span data-stu-id="8aca8-116">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="8aca8-117">Saklı yordam her hesaplanan bir özellik için bir sütun kümesi bir sonuç döndürmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8aca8-117">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="8aca8-118">**Delete saklı yordamı** varlık (veya varlık bir bileşik anahtarı varsa, birden çok parametre) anahtar değeri için bir parametre olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8aca8-118">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="8aca8-119">Ayrıca, delete yordamı hedef tablodaki (varlık içinde bildirilen, karşılık gelen, yabancı anahtar özelliklerini olmayan ilişkiler) da bağımsız ilişkilendirme yabancı anahtarları için parametreleri olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8aca8-119">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="8aca8-120">Bazı eşzamanlılık belirteçleri ilk değeri bir parametre gerekli, bkz: *eşzamanlılık belirteçleri* ayrıntılarını bölümü altında.</span><span class="sxs-lookup"><span data-stu-id="8aca8-120">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="8aca8-121">Aşağıdaki sınıftan bir örnek olarak kullanma:</span><span class="sxs-lookup"><span data-stu-id="8aca8-121">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="8aca8-122">Varsayılan saklı yordamlar şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="8aca8-122">The default stored procedures would be:</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="8aca8-123">Varsayılanları geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="8aca8-123">Overriding the Defaults</span></span>  

<span data-ttu-id="8aca8-124">Kısmını veya tamamını varsayılan olarak yapılandırılmış geçersiz kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8aca8-124">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="8aca8-125">Bir veya daha fazla saklı yordamlar adını değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8aca8-125">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="8aca8-126">Bu örnekte, yalnızca update kayıtlı yordamı yeniden adlandırır.</span><span class="sxs-lookup"><span data-stu-id="8aca8-126">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="8aca8-127">Bu örnekte, tüm üç saklı yordamlar yeniden adlandırır.</span><span class="sxs-lookup"><span data-stu-id="8aca8-127">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="8aca8-128">Bu örneklerde çağrıları birlikte Zincirli olan, ancak lambda blok söz dizimi de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8aca8-128">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

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

<span data-ttu-id="8aca8-129">Bu örnekte parametre update kayıtlı yordamı BlogId özelliği için yeniden adlandırır.</span><span class="sxs-lookup"><span data-stu-id="8aca8-129">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="8aca8-130">Bu, tüm chainable ve birleştirilebilir çağrılarıdır.</span><span class="sxs-lookup"><span data-stu-id="8aca8-130">These calls are all chainable and composable.</span></span> <span data-ttu-id="8aca8-131">Üç tüm saklı yordamları ve parametreleri yeniden adlandırır bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8aca8-131">Here is an example that renames all three stored procedures and their parameters.</span></span>  

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

<span data-ttu-id="8aca8-132">Oluşturulan veritabanı değerlerini içeren bir sonuç kümesi sütun adını da değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8aca8-132">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="8aca8-133">Olmadan (bağımsız ilişkileri) sınıfında yabancı anahtar ilişkileri</span><span class="sxs-lookup"><span data-stu-id="8aca8-133">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="8aca8-134">Bir yabancı anahtar özelliği, sınıf tanımında dahil edilirse, karşılık gelen parametre tıpkı diğer herhangi bir özelliği olarak adlandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="8aca8-134">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="8aca8-135">Bir yabancı anahtar özellik sınıfında olmadan bir ilişki mevcut olduğunda, varsayılan parametre adı.  **\<navigation_property_name\>_\<primary_key_name\>**.</span><span class="sxs-lookup"><span data-stu-id="8aca8-135">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="8aca8-136">Örneğin, aşağıdaki sınıf tanımları eklemek ve iletileri güncelleştirmek için saklı yordamları beklenen Blog_BlogId parametresinde neden olur.</span><span class="sxs-lookup"><span data-stu-id="8aca8-136">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="8aca8-137">Varsayılanları geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="8aca8-137">Overriding the Defaults</span></span>  

<span data-ttu-id="8aca8-138">Birincil anahtar özelliği parametre yönteme yolunu sağlayarak sınıfında yer almayan yabancı anahtarlar parametrelerini değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8aca8-138">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="8aca8-139">Bağımlı varlık (yani bir gezinti özelliği yoksa</span><span class="sxs-lookup"><span data-stu-id="8aca8-139">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="8aca8-140">hiçbir Post.Blog özelliği) ilişkilendirme yöntemi, ilişkinin diğer ucundaki belirleyin ve ardından her anahtar özellik karşılık gelen parametreleri yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8aca8-140">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="8aca8-141">Eşzamanlılık belirteçleri</span><span class="sxs-lookup"><span data-stu-id="8aca8-141">Concurrency Tokens</span></span>  

<span data-ttu-id="8aca8-142">Update ve delete saklı yordamları eşzamanlılık ile dağıtılacak da gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="8aca8-142">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="8aca8-143">Varlık eşzamanlılık belirteçleri varsa, saklı yordam isteğe bağlı olarak güncelleştirilen/silinen satır (etkilenen satır) sayısını döndüren bir output parametresi olabilir.</span><span class="sxs-lookup"><span data-stu-id="8aca8-143">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="8aca8-144">Bu tür RowsAffectedParameter yöntemi kullanılarak yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8aca8-144">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="8aca8-145">Varsayılan olarak EF kaç satır etkilendiğini belirlemek için dönüş değeri ExecuteNonQuery kullanır.</span><span class="sxs-lookup"><span data-stu-id="8aca8-145">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="8aca8-146">Satırlardan etkilenen çıkış parametresi ExecuteNonQuery (EF'ın açısından) için yanlış olan dönüş değeri neden olacağından, sproc herhangi bir mantık gerçekleştirmek istiyorsanız, kullanışlı belirtme yürütme sonunda.</span><span class="sxs-lookup"><span data-stu-id="8aca8-146">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="8aca8-147">Her eşzamanlılık belirteci yok adlı bir parametre olacak  **\<property_name\>_Original** (örneğin, Timestamp_Original).</span><span class="sxs-lookup"><span data-stu-id="8aca8-147">For each concurrency token there will be a parameter named **\<property_name\>_Original** (for example, Timestamp_Original ).</span></span> <span data-ttu-id="8aca8-148">Bu özelliğin – veritabanından sorgulandığında değer özgün değeri geçirilir.</span><span class="sxs-lookup"><span data-stu-id="8aca8-148">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="8aca8-149">Zaman damgaları gibi– – veritabanı tarafından hesaplanan eşzamanlılık belirteçleri, yalnızca özgün bir değer parametresi sahip olur.</span><span class="sxs-lookup"><span data-stu-id="8aca8-149">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="8aca8-150">Eşzamanlılık belirteçleri ayarlanan olmayan hesaplanan özellikler, güncelleştirme yordamı da yeni bir değer için bir parametre gerekir.</span><span class="sxs-lookup"><span data-stu-id="8aca8-150">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="8aca8-151">Bu yeni değerleri için zaten ele adlandırma kurallarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="8aca8-151">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="8aca8-152">Böyle bir belirteç örneği bir Blog URL bir eşzamanlılık belirteci olarak kullanılmasına, kodunuzu (aksine, yalnızca bir veritabanı tarafından güncelleştirilen zaman damgası belirteç) tarafından yeni bir değer bu güncelleştirilebilir olduğundan yeni bir değer gereklidir.</span><span class="sxs-lookup"><span data-stu-id="8aca8-152">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="8aca8-153">Bir örnek sınıf ve saklı yordam zaman damgası eşzamanlı bir simge ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="8aca8-153">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

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

<span data-ttu-id="8aca8-154">İşte bir örnek sınıf ve saklı yordam hesaplanmayan eşzamanlılık belirteci ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="8aca8-154">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="8aca8-155">Varsayılanları geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="8aca8-155">Overriding the Defaults</span></span>  

<span data-ttu-id="8aca8-156">Satırlardan etkilenen parametresi isteğe bağlı olarak çıkarabilir.</span><span class="sxs-lookup"><span data-stu-id="8aca8-156">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="8aca8-157">Hesaplanan Veritabanı eşzamanlılık belirteçleri için – yalnızca özgün değer geçirildiği – yalnızca mekanizması yeniden adlandırma standart parametresi parametresi özgün değeri için yeniden adlandırmak için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8aca8-157">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="8aca8-158">– Nerede hem özgün hem yeni değeri geçirilir – hesaplanmayan eşzamanlılık belirteçleri için bir aşırı yüklemesini her parametre için bir ad sağlayın olanak tanıyan parametresini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8aca8-158">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="8aca8-159">Çoktan Çoğa İlişkiler</span><span class="sxs-lookup"><span data-stu-id="8aca8-159">Many to Many Relationships</span></span>  

<span data-ttu-id="8aca8-160">Bu bölümde örnek olarak aşağıdaki sınıflar kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="8aca8-160">We’ll use the following classes as an example in this section.</span></span>  

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

<span data-ttu-id="8aca8-161">Çok-çok ilişkileri saklı yordamlar aşağıdaki söz dizimi ile eşlenebilir.</span><span class="sxs-lookup"><span data-stu-id="8aca8-161">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="8aca8-162">Sonra başka bir yapılandırma belirtilirse aşağıdaki saklı yordamı şekil varsayılan olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8aca8-162">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="8aca8-163">İki saklı yordamlar adlı  **\<type_one\>\<type_two\>_ekle** ve  **\<type_one\>\<type_two \>_Sil** (örneğin, PostTag_Insert ve PostTag_Delete).</span><span class="sxs-lookup"><span data-stu-id="8aca8-163">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (for example, PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="8aca8-164">Parametreleri her türü için anahtar değerleri olacaktır.</span><span class="sxs-lookup"><span data-stu-id="8aca8-164">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="8aca8-165">Her parametre olma adı **\<type_name\>_\<property_name\>** (örneğin, Post_PostId ve Tag_TagId).</span><span class="sxs-lookup"><span data-stu-id="8aca8-165">The name of each parameter being **\<type_name\>_\<property_name\>** (for example, Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="8aca8-166">Örnek İşte ekleme ve saklı yordamları güncelleştirme.</span><span class="sxs-lookup"><span data-stu-id="8aca8-166">Here are example insert and update stored procedures.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="8aca8-167">Varsayılanları geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="8aca8-167">Overriding the Defaults</span></span>  

<span data-ttu-id="8aca8-168">Yordam ve parametre adları, varlık saklı yordamlar için benzer bir şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="8aca8-168">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

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
