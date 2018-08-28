---
title: Ham SQL sorguları - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 99893ca1c634ce6f2e4cf9dcb70b1a1e43532c60
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995740"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="9ca42-102">Ham SQL sorguları</span><span class="sxs-lookup"><span data-stu-id="9ca42-102">Raw SQL Queries</span></span>
<span data-ttu-id="9ca42-103">Entity Framework, varlık sınıfları ile LINQ kullanarak sorgulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="9ca42-103">Entity Framework allows you to query using LINQ with your entity classes.</span></span> <span data-ttu-id="9ca42-104">Ancak, kullanarak doğrudan veritabanında ham SQL sorguları çalıştırmak istediğiniz zamanlar olabilir.</span><span class="sxs-lookup"><span data-stu-id="9ca42-104">However, there may be times that you want to run queries using raw SQL directly against the database.</span></span> <span data-ttu-id="9ca42-105">Bu, şu anda saklı yordamlar için eşleme desteklemeyen Code First modelleri için yararlı olabilir saklı yordam çağırma içerir.</span><span class="sxs-lookup"><span data-stu-id="9ca42-105">This includes calling stored procedures, which can be helpful for Code First models that currently do not support mapping to stored procedures.</span></span> <span data-ttu-id="9ca42-106">Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="9ca42-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="writing-sql-queries-for-entities"></a><span data-ttu-id="9ca42-107">Varlıklar için SQL sorguları yazma</span><span class="sxs-lookup"><span data-stu-id="9ca42-107">Writing SQL queries for entities</span></span>  

<span data-ttu-id="9ca42-108">Varlık örneklerini döndürür yazılacak ham bir SQL sorgusu olan DB SqlQuery yöntemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="9ca42-108">The SqlQuery method on DbSet allows a raw SQL query to be written that will return entity instances.</span></span> <span data-ttu-id="9ca42-109">Yalnızca bunlar bir LINQ Sorgu tarafından döndürülen durumunda olduğu gibi döndürülen nesneleri bağlam tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="9ca42-109">The returned objects will be tracked by the context just as they would be if they were returned by a LINQ query.</span></span> <span data-ttu-id="9ca42-110">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9ca42-110">For example:</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="9ca42-111">Sonuçları numaralandırılır kadar LINQ sorguları olduğu gibi sorgu yürütülmez ise, Not: Yukarıdaki örnekte, bu ToList çağrısıyla yapılır.</span><span class="sxs-lookup"><span data-stu-id="9ca42-111">Note that, just as for LINQ queries, the query is not executed until the results are enumerated—in the example above this is done with the call to ToList.</span></span>  

<span data-ttu-id="9ca42-112">Ham SQL sorguları iki nedenden dolayı yazılan her dikkatli olunması.</span><span class="sxs-lookup"><span data-stu-id="9ca42-112">Care should be taken whenever raw SQL queries are written for two reasons.</span></span> <span data-ttu-id="9ca42-113">İlk olarak, yalnızca gerçekten istenen türde olan varlıklar döndürüyor emin olmak için sorgu yazılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9ca42-113">First, the query should be written to ensure that it only returns entities that are really of the requested type.</span></span> <span data-ttu-id="9ca42-114">Örneğin, devralma gibi özellikleri kullanarak CLR türü yanlış olan varlıkları oluşturan bir sorgu yazmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="9ca42-114">For example, when using features such as inheritance it is easy to write a query that will create entities that are of the wrong CLR type.</span></span>  

<span data-ttu-id="9ca42-115">İkinci olarak, bazı türleri ham SQL sorgusunun özellikle geçici SQL ekleme saldırılarına karşı olası güvenlik risklerini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="9ca42-115">Second, some types of raw SQL query expose potential security risks, especially around SQL injection attacks.</span></span> <span data-ttu-id="9ca42-116">Bu tür saldırılara karşı korunmak için doğru şekilde sorgu parametreleri kullandığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="9ca42-116">Make sure that you use parameters in your query in the correct way to guard against such attacks.</span></span>  

### <a name="loading-entities-from-stored-procedures"></a><span data-ttu-id="9ca42-117">Saklı yordamlardan varlıklar yükleniyor</span><span class="sxs-lookup"><span data-stu-id="9ca42-117">Loading entities from stored procedures</span></span>  

<span data-ttu-id="9ca42-118">DbSet.SqlQuery varlıklar bir saklı yordam sonuçlarından yüklemek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9ca42-118">You can use DbSet.SqlQuery to load entities from the results of a stored procedure.</span></span> <span data-ttu-id="9ca42-119">Örneğin, aşağıdaki kod, dbo çağırır. Veritabanı yordamda GetBlogs:</span><span class="sxs-lookup"><span data-stu-id="9ca42-119">For example, the following code calls the dbo.GetBlogs procedure in the database:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

<span data-ttu-id="9ca42-120">Ayrıca, aşağıdaki söz dizimini kullanarak bir saklı yordam parametreleri geçirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9ca42-120">You can also pass parameters to a stored procedure using the following syntax:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a><span data-ttu-id="9ca42-121">Varlık olmayan türleri için SQL sorguları yazma</span><span class="sxs-lookup"><span data-stu-id="9ca42-121">Writing SQL queries for non-entity types</span></span>  

<span data-ttu-id="9ca42-122">İlkel türler dahil olmak üzere herhangi bir türü örneklerini döndüren bir SQL sorgusu üzerinde veritabanı sınıfı SqlQuery yöntemi kullanılarak oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="9ca42-122">A SQL query returning instances of any type, including primitive types, can be created using the SqlQuery method on the Database class.</span></span> <span data-ttu-id="9ca42-123">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9ca42-123">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="9ca42-124">Nesneleri bir varlık türü örneklerini olsa bile, SqlQuery döndürülen veritabanı sonuçları hiçbir zaman bağlam tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="9ca42-124">The results returned from SqlQuery on Database will never be tracked by the context even if the objects are instances of an entity type.</span></span>  

## <a name="sending-raw-commands-to-the-database"></a><span data-ttu-id="9ca42-125">Veritabanına ham komut gönderme</span><span class="sxs-lookup"><span data-stu-id="9ca42-125">Sending raw commands to the database</span></span>  

<span data-ttu-id="9ca42-126">Sorgu dışı komutları veritabanında ExecuteSqlCommand yöntemi kullanarak veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="9ca42-126">Non-query commands can be sent to the database using the ExecuteSqlCommand method on Database.</span></span> <span data-ttu-id="9ca42-127">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9ca42-127">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

<span data-ttu-id="9ca42-128">Varlıkları yüklenen veya veritabanından yeniden kadar ExecuteSqlCommand kullanarak veritabanındaki verileri yapılan tüm değişiklikler bağlamına donuk olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="9ca42-128">Note that any changes made to data in the database using ExecuteSqlCommand are opaque to the context until entities are loaded or reloaded from the database.</span></span>  

### <a name="output-parameters"></a><span data-ttu-id="9ca42-129">Çıktı Parametreleri</span><span class="sxs-lookup"><span data-stu-id="9ca42-129">Output Parameters</span></span>  

<span data-ttu-id="9ca42-130">Çıktı parametreleri kullandıysanız değerleri sonuçları tamamen okunana kadar kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="9ca42-130">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="9ca42-131">Bu dbdatareader öğesine dönüştürülemedi arka plandaki davranışı nedeniyle, bkz: [alma verileri kullanarak bir DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="9ca42-131">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>  
