---
title: Varlıkları Sorgulama ve Bulma - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417170"
---
# <a name="querying-and-finding-entities"></a><span data-ttu-id="ada85-102">Varlıkları Sorgulama ve Bulma</span><span class="sxs-lookup"><span data-stu-id="ada85-102">Querying and Finding Entities</span></span>
<span data-ttu-id="ada85-103">Bu konu, LINQ ve Bul yöntemi de dahil olmak üzere Varlık Çerçevesi'ni kullanarak veri sorgulayabildiğiniz çeşitli yolları kapsar.</span><span class="sxs-lookup"><span data-stu-id="ada85-103">This topic covers the various ways you can query for data using Entity Framework, including LINQ and the Find method.</span></span> <span data-ttu-id="ada85-104">Bu konuda gösterilen teknikler, Code First ve EF Designer ile oluşturulan modeller için eşit olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ada85-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="finding-entities-using-a-query"></a><span data-ttu-id="ada85-105">Sorgu kullanarak varlıkları bulma</span><span class="sxs-lookup"><span data-stu-id="ada85-105">Finding entities using a query</span></span>  

<span data-ttu-id="ada85-106">DbSet ve IDbSet iQueryable uygulamak ve böylece veritabanına karşı bir LINQ sorgu yazmak için başlangıç noktası olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ada85-106">DbSet and IDbSet implement IQueryable and so can be used as the starting point for writing a LINQ query against the database.</span></span> <span data-ttu-id="ada85-107">Bu LINQ derinlemesine bir tartışma için uygun bir yer değildir, ancak burada basit örnekler bir çift şunlardır:</span><span class="sxs-lookup"><span data-stu-id="ada85-107">This is not the appropriate place for an in-depth discussion of LINQ, but here are a couple of simple examples:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs with names starting with B
    var blogs = from b in context.Blogs
                   where b.Name.StartsWith("B")
                   select b;

    // Query for the Blog named ADO.NET Blog
    var blog = context.Blogs
                    .Where(b => b.Name == "ADO.NET Blog")
                    .FirstOrDefault();
}
```  

<span data-ttu-id="ada85-108">DbSet ve IDbSet'in her zaman veritabanına karşı sorgular oluşturduğunu ve döndürülen varlıklar bağlamında zaten mevcut olsa bile veritabanına gidiş-dönüş içereceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ada85-108">Note that DbSet and IDbSet always create queries against the database and will always involve a round trip to the database even if the entities returned already exist in the context.</span></span> <span data-ttu-id="ada85-109">Aşağıdaki saatlerde veritabanına karşı bir sorgu yürütülür:</span><span class="sxs-lookup"><span data-stu-id="ada85-109">A query is executed against the database when:</span></span>  

- <span data-ttu-id="ada85-110">Bir **foreach** (C#) veya **For Each** (Visual Basic) deyimi ile numaralandırılır.</span><span class="sxs-lookup"><span data-stu-id="ada85-110">It is enumerated by a **foreach** (C#) or **For Each** (Visual Basic) statement.</span></span>  
- <span data-ttu-id="ada85-111">[ToArray,](https://msdn.microsoft.com/library/bb298736) [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary)veya [ToList](https://msdn.microsoft.com/library/bb342261)gibi bir toplama işlemi tarafından numaralandırılır.</span><span class="sxs-lookup"><span data-stu-id="ada85-111">It is enumerated by a collection operation such as [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), or [ToList](https://msdn.microsoft.com/library/bb342261).</span></span>  
- <span data-ttu-id="ada85-112">[İlk](https://msdn.microsoft.com/library/bb291976) veya [Any](https://msdn.microsoft.com/library/bb337697) gibi LINQ işleçleri sorgunun en dış kısmında belirtilir.</span><span class="sxs-lookup"><span data-stu-id="ada85-112">LINQ operators such as [First](https://msdn.microsoft.com/library/bb291976) or [Any](https://msdn.microsoft.com/library/bb337697) are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="ada85-113">Aşağıdaki yöntemler denir: DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)ve Database.ExecuteSqlCommand yük [uzantısı](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ada85-113">The following methods are called: the [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) extension method on a DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx), and Database.ExecuteSqlCommand.</span></span>  

<span data-ttu-id="ada85-114">Sonuçlar veritabanından döndürüldüğünde, bağlamiçinde bulunmayan nesneler içeriğe eklenir.</span><span class="sxs-lookup"><span data-stu-id="ada85-114">When results are returned from the database, objects that do not exist in the context are attached to the context.</span></span> <span data-ttu-id="ada85-115">Bir nesne zaten bağlam içindeyse, varolan nesne döndürülür (nesnenin girişteki özelliklerinin geçerli ve özgün değerleri veritabanı değerleriyle birlikte üzerine **yazılmaz).**</span><span class="sxs-lookup"><span data-stu-id="ada85-115">If an object is already in the context, the existing object is returned (the current and original values of the object's properties in the entry are **not** overwritten with database values).</span></span>  

<span data-ttu-id="ada85-116">Bir sorgu gerçekleştirdiğinizde, içeriğe eklenen ancak henüz veritabanına kaydedilmemiş varlıklar, sonuç kümesinin bir parçası olarak döndürülmez.</span><span class="sxs-lookup"><span data-stu-id="ada85-116">When you perform a query, entities that have been added to the context but have not yet been saved to the database are not returned as part of the result set.</span></span> <span data-ttu-id="ada85-117">Bağlamdaki verileri almak için [Bkz. Yerel Veriler.](~/ef6/querying/local-data.md)</span><span class="sxs-lookup"><span data-stu-id="ada85-117">To get the data that is in the context, see [Local Data](~/ef6/querying/local-data.md).</span></span>  

<span data-ttu-id="ada85-118">Bir sorgu veritabanından satır döndürmezse, sonuç **null**yerine boş bir koleksiyon olur.</span><span class="sxs-lookup"><span data-stu-id="ada85-118">If a query returns no rows from the database, the result will be an empty collection, rather than **null**.</span></span>  

## <a name="finding-entities-using-primary-keys"></a><span data-ttu-id="ada85-119">Birincil anahtarları kullanarak varlıkları bulma</span><span class="sxs-lookup"><span data-stu-id="ada85-119">Finding entities using primary keys</span></span>  

<span data-ttu-id="ada85-120">DbSet'te Bul yöntemi, bağlam tarafından izlenen bir varlığı bulmaya çalışmak için birincil anahtar değerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="ada85-120">The Find method on DbSet uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="ada85-121">Varlık bağlamda bulunmazsa, veritabanına bir sorgu gönderilir ve orada bulunan varlığı bulur.</span><span class="sxs-lookup"><span data-stu-id="ada85-121">If the entity is not found in the context then a query will be sent to the database to find the entity there.</span></span> <span data-ttu-id="ada85-122">Varlık bağlamda veya veritabanında bulunmazsa null döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ada85-122">Null is returned if the entity is not found in the context or in the database.</span></span>  

<span data-ttu-id="ada85-123">Bul, sorgukullanmanın iki önemli şekilde kullanılmasından farklıdır:</span><span class="sxs-lookup"><span data-stu-id="ada85-123">Find is different from using a query in two significant ways:</span></span>  

- <span data-ttu-id="ada85-124">Veritabanına gidiş-dönüş yalnızca verilen anahtara sahip varlık bağlamında bulunmazsa yapılır.</span><span class="sxs-lookup"><span data-stu-id="ada85-124">A round-trip to the database will only be made if the entity with the given key is not found in the context.</span></span>  
- <span data-ttu-id="ada85-125">Find, Eklenen durumdaki varlıkları döndürecek.</span><span class="sxs-lookup"><span data-stu-id="ada85-125">Find will return entities that are in the Added state.</span></span> <span data-ttu-id="ada85-126">Diğer bir zamanda, Bul, içeriğe eklenen ancak henüz veritabanına kaydedilmemiş varlıkları döndürecektir.</span><span class="sxs-lookup"><span data-stu-id="ada85-126">That is, Find will return entities that have been added to the context but have not yet been saved to the database.</span></span>  
### <a name="finding-an-entity-by-primary-key"></a><span data-ttu-id="ada85-127">Birincil anahtara göre bir varlık bulma</span><span class="sxs-lookup"><span data-stu-id="ada85-127">Finding an entity by primary key</span></span>  

<span data-ttu-id="ada85-128">Aşağıdaki kod Bul'un bazı kullanımlarını gösterir:</span><span class="sxs-lookup"><span data-stu-id="ada85-128">The following code shows some uses of Find:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Will hit the database
    var blog = context.Blogs.Find(3);

    // Will return the same instance without hitting the database
    var blogAgain = context.Blogs.Find(3);

    context.Blogs.Add(new Blog { Id = -1 });

    // Will find the new blog even though it does not exist in the database
    var newBlog = context.Blogs.Find(-1);

    // Will find a User which has a string primary key
    var user = context.Users.Find("johndoe1987");
}
```  

### <a name="finding-an-entity-by-composite-primary-key"></a><span data-ttu-id="ada85-129">Bileşik birincil anahtarla bir varlık bulma</span><span class="sxs-lookup"><span data-stu-id="ada85-129">Finding an entity by composite primary key</span></span>  

<span data-ttu-id="ada85-130">Entity Framework, varlıklarınızın bileşik anahtarlara sahip olmasını sağlar - bu birden fazla özellikten oluşan bir anahtardır.</span><span class="sxs-lookup"><span data-stu-id="ada85-130">Entity Framework allows your entities to have composite keys - that's a key that is made up of more than one property.</span></span> <span data-ttu-id="ada85-131">Örneğin, belirli bir blogun kullanıcı ayarlarını temsil eden bir BlogAyarları kuruluşunuz olabilir.</span><span class="sxs-lookup"><span data-stu-id="ada85-131">For example, you could have a BlogSettings entity that represents a users settings for a particular blog.</span></span> <span data-ttu-id="ada85-132">Çünkü bir kullanıcı sadece hiç her blog için bir BlogSettings olurdu BlogAyarlar birincil anahtarı BlogId ve Kullanıcı Adı bir arada yapmak için seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ada85-132">Because a user would only ever have one BlogSettings for each blog you could chose to make the primary key of BlogSettings a combination of BlogId and Username.</span></span> <span data-ttu-id="ada85-133">Aşağıdaki kod BlogAyarları BlogId = 3 ve Kullanıcı Adı = "johndoe1987" ile bulmaya çalışır:</span><span class="sxs-lookup"><span data-stu-id="ada85-133">The following code attempts to find the BlogSettings with BlogId = 3 and Username = "johndoe1987":</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

<span data-ttu-id="ada85-134">Bileşik tuşlarınız olduğunda, bileşik anahtarın özellikleri için bir sıralama belirtmek için ColumnAttribute veya akıcı API kullanmanız gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ada85-134">Note that when you have composite keys you need to use ColumnAttribute or the fluent API to specify an ordering for the properties of the composite key.</span></span> <span data-ttu-id="ada85-135">Anahtarı oluşturan değerleri belirtirken Bul çağrısı bu sırayı kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ada85-135">The call to Find must use this order when specifying the values that form the key.</span></span>  
