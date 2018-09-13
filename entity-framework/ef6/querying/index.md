---
title: Sorgulamak ve varlıkları - EF6 bulma
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489797"
---
# <a name="querying-and-finding-entities"></a><span data-ttu-id="1a880-102">Sorgulamak ve varlıkları bulma</span><span class="sxs-lookup"><span data-stu-id="1a880-102">Querying and Finding Entities</span></span>
<span data-ttu-id="1a880-103">Bu konu LINQ ve bulma yöntemini de dahil olmak üzere, Entity Framework kullanarak verileri sorgulamak farklı yöntemleri kapsar.</span><span class="sxs-lookup"><span data-stu-id="1a880-103">This topic covers the various ways you can query for data using Entity Framework, including LINQ and the Find method.</span></span> <span data-ttu-id="1a880-104">Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="1a880-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="finding-entities-using-a-query"></a><span data-ttu-id="1a880-105">Bir sorgu kullanarak varlıkları bulma</span><span class="sxs-lookup"><span data-stu-id="1a880-105">Finding entities using a query</span></span>  

<span data-ttu-id="1a880-106">Olan DB ve IDbSet Iqueryable'a uygulayın ve veritabanında bir LINQ sorgusunu yazmak için başlangıç noktası olarak bu nedenle kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1a880-106">DbSet and IDbSet implement IQueryable and so can be used as the starting point for writing a LINQ query against the database.</span></span> <span data-ttu-id="1a880-107">LINQ ilgili ayrıntılı bir tartışma için uygun bir yerde değil, ancak birkaç basit bir örnek aşağıdadır:</span><span class="sxs-lookup"><span data-stu-id="1a880-107">This is not the appropriate place for an in-depth discussion of LINQ, but here are a couple of simple examples:</span></span>  

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

<span data-ttu-id="1a880-108">Olan DB ve IDbSet her zaman veritabanına karşı sorgular oluşturun ve zaten döndürülen varlıklara bağlamında bulunsa bile her zaman veritabanına gidiş dönüş içerecektir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1a880-108">Note that DbSet and IDbSet always create queries against the database and will always involve a round trip to the database even if the entities returned already exist in the context.</span></span> <span data-ttu-id="1a880-109">Bir sorgu veritabanında yürütüldüğü zaman:</span><span class="sxs-lookup"><span data-stu-id="1a880-109">A query is executed against the database when:</span></span>  

- <span data-ttu-id="1a880-110">Tarafından numaralandırılmış bir **foreach** (C#) veya **her** deyimi (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="1a880-110">It is enumerated by a **foreach** (C#) or **For Each** (Visual Basic) statement.</span></span>  
- <span data-ttu-id="1a880-111">Bir toplama işlemi tarafından gibi numaralandırılana [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), veya [ToList](https://msdn.microsoft.com/library/bb342261).</span><span class="sxs-lookup"><span data-stu-id="1a880-111">It is enumerated by a collection operation such as [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), or [ToList](https://msdn.microsoft.com/library/bb342261).</span></span>  
- <span data-ttu-id="1a880-112">Gibi LINQ işleçleri [ilk](https://msdn.microsoft.com/library/bb291976) veya [herhangi](https://msdn.microsoft.com/library/bb337697) en dıştaki sorgunun parçası belirtilir.</span><span class="sxs-lookup"><span data-stu-id="1a880-112">LINQ operators such as [First](https://msdn.microsoft.com/library/bb291976) or [Any](https://msdn.microsoft.com/library/bb337697) are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="1a880-113">Aşağıdaki yöntem de çağrıldığında: [yük](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) genişletme yöntemini bir olan DB [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)ve Database.ExecuteSqlCommand.</span><span class="sxs-lookup"><span data-stu-id="1a880-113">The following methods are called: the [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) extension method on a DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx), and Database.ExecuteSqlCommand.</span></span>  

<span data-ttu-id="1a880-114">Veritabanından sonuçları döndürdüğünde bağlamda mevcut değil bir nesne bağlamına eklenir.</span><span class="sxs-lookup"><span data-stu-id="1a880-114">When results are returned from the database, objects that do not exist in the context are attached to the context.</span></span> <span data-ttu-id="1a880-115">Bir nesne bağlamı ise, varolan nesne döndürülür (geçerli ve orijinal nesnenin özelliklerini giriş değerleri **değil** veritabanı değerlerle geçersiz kılınır).</span><span class="sxs-lookup"><span data-stu-id="1a880-115">If an object is already in the context, the existing object is returned (the current and original values of the object's properties in the entry are **not** overwritten with database values).</span></span>  

<span data-ttu-id="1a880-116">Bir sorgu gerçekleştirdiğinizde, bağlamına eklenmiş ancak veritabanı henüz kaydedilmedi varlıklar sonuç kümesinin bir parçası olarak döndürülmez.</span><span class="sxs-lookup"><span data-stu-id="1a880-116">When you perform a query, entities that have been added to the context but have not yet been saved to the database are not returned as part of the result set.</span></span> <span data-ttu-id="1a880-117">Bir bağlam içinde veri almak için bkz [yerel veri](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="1a880-117">To get the data that is in the context, see [Local Data](~/ef6/querying/local-data.md).</span></span>  

<span data-ttu-id="1a880-118">Bir sorgu, veritabanından satır döndürürse, sonuç boş bir koleksiyon olacaktır yerine **null**.</span><span class="sxs-lookup"><span data-stu-id="1a880-118">If a query returns no rows from the database, the result will be an empty collection, rather than **null**.</span></span>  

## <a name="finding-entities-using-primary-keys"></a><span data-ttu-id="1a880-119">Birincil anahtarlar kullanarak varlıkları bulma</span><span class="sxs-lookup"><span data-stu-id="1a880-119">Finding entities using primary keys</span></span>  

<span data-ttu-id="1a880-120">Bulma yöntemini olan DB bağlam tarafından izlenen bir varlığın bulmaya için birincil anahtar değeri kullanır.</span><span class="sxs-lookup"><span data-stu-id="1a880-120">The Find method on DbSet uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="1a880-121">Varlık bağlamı bulunamazsa sonra bir sorgu veritabanına varlık var. bulmak için gönderilir.</span><span class="sxs-lookup"><span data-stu-id="1a880-121">If the entity is not found in the context then a query will be sent to the database to find the entity there.</span></span> <span data-ttu-id="1a880-122">Varlık bağlamı veya veritabanında bulunmazsa null döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1a880-122">Null is returned if the entity is not found in the context or in the database.</span></span>  

<span data-ttu-id="1a880-123">Bulma, bir sorgu kullanarak iki önemli şekilde farklıdır:</span><span class="sxs-lookup"><span data-stu-id="1a880-123">Find is different from using a query in two significant ways:</span></span>  

- <span data-ttu-id="1a880-124">Bağlam içinde verilen anahtara sahip bir varlık bulunamıyorsa bir gidiş dönüş veritabanına yalnızca sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="1a880-124">A round-trip to the database will only be made if the entity with the given key is not found in the context.</span></span>  
- <span data-ttu-id="1a880-125">Bul Added durumda olan varlıklar döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a880-125">Find will return entities that are in the Added state.</span></span> <span data-ttu-id="1a880-126">Bu, bulabilir, bağlamına eklenmiş ancak veritabanı henüz kaydedilmedi varlıkları döndürür.</span><span class="sxs-lookup"><span data-stu-id="1a880-126">That is, Find will return entities that have been added to the context but have not yet been saved to the database.</span></span>  
### <a name="finding-an-entity-by-primary-key"></a><span data-ttu-id="1a880-127">Bir varlığın birincil anahtara göre bulma</span><span class="sxs-lookup"><span data-stu-id="1a880-127">Finding an entity by primary key</span></span>  

<span data-ttu-id="1a880-128">Aşağıdaki kod bulma bazı kullanımlarını gösterir:</span><span class="sxs-lookup"><span data-stu-id="1a880-128">The following code shows some uses of Find:</span></span>  

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

### <a name="finding-an-entity-by-composite-primary-key"></a><span data-ttu-id="1a880-129">Bir varlığın birincil bileşik anahtara göre bulma</span><span class="sxs-lookup"><span data-stu-id="1a880-129">Finding an entity by composite primary key</span></span>  

<span data-ttu-id="1a880-130">Entity Framework varlıklarınızı birden fazla özelliği oluşan bir anahtar kullanıcının bileşik anahtarlar - olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="1a880-130">Entity Framework allows your entities to have composite keys - that's a key that is made up of more than one property.</span></span> <span data-ttu-id="1a880-131">Örneğin, belirli bir blog için kullanıcı ayarlarını temsil eden bir BlogSettings varlık olabilir.</span><span class="sxs-lookup"><span data-stu-id="1a880-131">For example, you could have a BlogSettings entity that represents a users settings for a particular blog.</span></span> <span data-ttu-id="1a880-132">Bir kullanıcı her zaman sadece yeterli olacağından bir BlogSettings yapabilirsiniz her blog için birincil anahtarı olarak BlogSettings BlogId ve kullanıcı adı olun seçtiniz.</span><span class="sxs-lookup"><span data-stu-id="1a880-132">Because a user would only ever have one BlogSettings for each blog you could chose to make the primary key of BlogSettings a combination of BlogId and Username.</span></span> <span data-ttu-id="1a880-133">Aşağıdaki kod ile BlogId BlogSettings bulmayı dener 3 ve kullanıcı adı = "johndoe1987" =:</span><span class="sxs-lookup"><span data-stu-id="1a880-133">The following code attempts to find the BlogSettings with BlogId = 3 and Username = "johndoe1987":</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

<span data-ttu-id="1a880-134">Bileşik anahtarlar olduğunda bileşik anahtarın özelliklerini sıralama belirtmek için ColumnAttribute ya da fluent API'sini kullanmanız gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1a880-134">Note that when you have composite keys you need to use ColumnAttribute or the fluent API to specify an ordering for the properties of the composite key.</span></span> <span data-ttu-id="1a880-135">Bul çağrısı bu anahtarı form değerler belirtirken kullanmalısınız.</span><span class="sxs-lookup"><span data-stu-id="1a880-135">The call to Find must use this order when specifying the values that form the key.</span></span>  
