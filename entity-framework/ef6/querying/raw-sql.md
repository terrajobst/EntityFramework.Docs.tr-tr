---
title: Ham SQL sorguları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 168aee67186535bf2a50705e86bfc5a88147e369
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417097"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="84d52-102">Ham SQL Sorguları</span><span class="sxs-lookup"><span data-stu-id="84d52-102">Raw SQL Queries</span></span>
<span data-ttu-id="84d52-103">Entity Framework, varlık sınıflarınız ile LINQ kullanarak sorgulama yapmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="84d52-103">Entity Framework allows you to query using LINQ with your entity classes.</span></span> <span data-ttu-id="84d52-104">Ancak, ham SQL kullanarak doğrudan veritabanına karşı sorguları çalıştırmak istediğiniz zamanlar olabilir.</span><span class="sxs-lookup"><span data-stu-id="84d52-104">However, there may be times that you want to run queries using raw SQL directly against the database.</span></span> <span data-ttu-id="84d52-105">Bu, saklı yordamların çağrılmasını içerir. Bu, şu anda saklı yordamlara eşlemeyi desteklemeyen Code First modeller için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="84d52-105">This includes calling stored procedures, which can be helpful for Code First models that currently do not support mapping to stored procedures.</span></span> <span data-ttu-id="84d52-106">Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="84d52-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="writing-sql-queries-for-entities"></a><span data-ttu-id="84d52-107">Varlıklar için SQL sorguları yazma</span><span class="sxs-lookup"><span data-stu-id="84d52-107">Writing SQL queries for entities</span></span>  

<span data-ttu-id="84d52-108">DbSet üzerindeki SqlQuery metodu, varlık örnekleri döndürecek ham bir SQL sorgusunun yazılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="84d52-108">The SqlQuery method on DbSet allows a raw SQL query to be written that will return entity instances.</span></span> <span data-ttu-id="84d52-109">Döndürülen nesneler, LINQ sorgusu tarafından döndürüldüler olduğu gibi bağlam tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="84d52-109">The returned objects will be tracked by the context just as they would be if they were returned by a LINQ query.</span></span> <span data-ttu-id="84d52-110">Örnek:</span><span class="sxs-lookup"><span data-stu-id="84d52-110">For example:</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="84d52-111">LINQ sorguları için olduğu gibi, sonuçlar numaralandırılıncaya kadar sorgu yürütülmez — Yukarıdaki örnekte, ToList çağrısıyla yapılır.</span><span class="sxs-lookup"><span data-stu-id="84d52-111">Note that, just as for LINQ queries, the query is not executed until the results are enumerated—in the example above this is done with the call to ToList.</span></span>  

<span data-ttu-id="84d52-112">Her iki nedenden dolayı ham SQL sorguları yazıldığında dikkatli olunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="84d52-112">Care should be taken whenever raw SQL queries are written for two reasons.</span></span> <span data-ttu-id="84d52-113">İlk olarak, yalnızca istenen türden gerçekten varlık döndüren varlıkların döndürüldüğünden emin olmak için sorgunun yazılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="84d52-113">First, the query should be written to ensure that it only returns entities that are really of the requested type.</span></span> <span data-ttu-id="84d52-114">Örneğin, devralma gibi özellikleri kullanırken yanlış CLR türünde varlıklar oluşturacak bir sorgu yazmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="84d52-114">For example, when using features such as inheritance it is easy to write a query that will create entities that are of the wrong CLR type.</span></span>  

<span data-ttu-id="84d52-115">İkinci olarak, bazı ham SQL sorgusu türleri, özellikle SQL ekleme saldırıları etrafında olası güvenlik riskleri sunar.</span><span class="sxs-lookup"><span data-stu-id="84d52-115">Second, some types of raw SQL query expose potential security risks, especially around SQL injection attacks.</span></span> <span data-ttu-id="84d52-116">Bu tür saldırılara karşı koruma sağlamak için Sorgunuzdaki parametreleri doğru şekilde kullandığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="84d52-116">Make sure that you use parameters in your query in the correct way to guard against such attacks.</span></span>  

### <a name="loading-entities-from-stored-procedures"></a><span data-ttu-id="84d52-117">Saklı yordamlardan varlık yükleme</span><span class="sxs-lookup"><span data-stu-id="84d52-117">Loading entities from stored procedures</span></span>  

<span data-ttu-id="84d52-118">Saklı yordamın sonuçlarından varlıkları yüklemek için DbSet. SqlQuery ' i kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84d52-118">You can use DbSet.SqlQuery to load entities from the results of a stored procedure.</span></span> <span data-ttu-id="84d52-119">Örneğin, aşağıdaki kod dbo 'yı çağırır. Veritabanında Getbloglar yordamı:</span><span class="sxs-lookup"><span data-stu-id="84d52-119">For example, the following code calls the dbo.GetBlogs procedure in the database:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

<span data-ttu-id="84d52-120">Ayrıca, aşağıdaki sözdizimini kullanarak parametreleri saklı yordama geçirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="84d52-120">You can also pass parameters to a stored procedure using the following syntax:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a><span data-ttu-id="84d52-121">Varlık dışı türler için SQL sorguları yazma</span><span class="sxs-lookup"><span data-stu-id="84d52-121">Writing SQL queries for non-entity types</span></span>  

<span data-ttu-id="84d52-122">İlkel türler de dahil olmak üzere herhangi bir türün örnek döndüren bir SQL sorgusu, veritabanı sınıfında SqlQuery yöntemi kullanılarak oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="84d52-122">A SQL query returning instances of any type, including primitive types, can be created using the SqlQuery method on the Database class.</span></span> <span data-ttu-id="84d52-123">Örnek:</span><span class="sxs-lookup"><span data-stu-id="84d52-123">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="84d52-124">Nesneler bir varlık türünün örnekleri olsa bile, veritabanındaki SqlQuery 'den döndürülen sonuçlar hiçbir şekilde bağlam tarafından izlenmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="84d52-124">The results returned from SqlQuery on Database will never be tracked by the context even if the objects are instances of an entity type.</span></span>  

## <a name="sending-raw-commands-to-the-database"></a><span data-ttu-id="84d52-125">Ham komutları veritabanına gönderme</span><span class="sxs-lookup"><span data-stu-id="84d52-125">Sending raw commands to the database</span></span>  

<span data-ttu-id="84d52-126">Sorgu olmayan komutlar veritabanı üzerinde ExecuteSqlCommand yöntemi kullanılarak veritabanına gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="84d52-126">Non-query commands can be sent to the database using the ExecuteSqlCommand method on Database.</span></span> <span data-ttu-id="84d52-127">Örnek:</span><span class="sxs-lookup"><span data-stu-id="84d52-127">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

<span data-ttu-id="84d52-128">Veritabanında veri yüklenmeden veya yeniden yüklenene kadar, ExecuteSqlCommand kullanılarak veritabanındaki verilerde yapılan tüm değişikliklerin bağlam halinde donuk olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="84d52-128">Note that any changes made to data in the database using ExecuteSqlCommand are opaque to the context until entities are loaded or reloaded from the database.</span></span>  

### <a name="output-parameters"></a><span data-ttu-id="84d52-129">Çıktı Parametreleri</span><span class="sxs-lookup"><span data-stu-id="84d52-129">Output Parameters</span></span>  

<span data-ttu-id="84d52-130">Çıkış parametreleri kullanılıyorsa, sonuçlar tamamen okunana kadar bu değerler kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="84d52-130">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="84d52-131">Bu, DbDataReader 'ın temel davranışının nedeni, daha fazla ayrıntı için [DataReader kullanarak veri alma](https://go.microsoft.com/fwlink/?LinkID=398589) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="84d52-131">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>  
