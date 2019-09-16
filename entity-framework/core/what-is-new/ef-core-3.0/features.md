---
title: EF Core 3,0 ' deki yeni özellikler EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 528733d6eec33de2c9538541a6ed5be704b9d433
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005564"
---
# <a name="new-features-included-in-ef-core-30"></a><span data-ttu-id="0f248-102">EF Core 3,0 ' de bulunan yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="0f248-102">New features included in EF Core 3.0</span></span>

<span data-ttu-id="0f248-103">Aşağıdaki listede EF Core 3,0 için planlanmış olan başlıca yeni özellikler yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="0f248-103">The following list includes the major new features planned for EF Core 3.0.</span></span>

<span data-ttu-id="0f248-104">EF Core 3,0 önemli bir sürümdür ve ayrıca mevcut uygulamalarda olumsuz etkileri olabilecek API geliştirmeleri olan çok sayıda [Son değişiklik](xref:core/what-is-new/ef-core-3.0/breaking-changes)içerir.</span><span class="sxs-lookup"><span data-stu-id="0f248-104">EF Core 3.0 is a major release and also contains numerous [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-improvements"></a><span data-ttu-id="0f248-105">LINQ geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="0f248-105">LINQ improvements</span></span> 

<span data-ttu-id="0f248-106">LINQ, IntelliSense ve derleme zamanı tür denetimi sunmak için zengin tür bilgilerinin avantajlarından yararlanarak, seçtiğiniz dilden çıkmadan veritabanı sorguları yazmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="0f248-106">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="0f248-107">Ancak LINQ, rastgele ifadeler (Yöntem çağrıları veya işlemler) içeren sınırsız sayıda karmaşık sorgu yazmanızı de sağlar.</span><span class="sxs-lookup"><span data-stu-id="0f248-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="0f248-108">Bu kombinasyonların yönetilmesi, LINQ sağlayıcıları için her zaman önemli bir zorluk olmuştur.</span><span class="sxs-lookup"><span data-stu-id="0f248-108">Handling all those combinations has always been a significant challenge for LINQ providers.</span></span>
<span data-ttu-id="0f248-109">EF Core 3,0 ' de, LINQ uygulamamız SQL 'e daha fazla ifade çevirmeyi etkinleştirmek, daha fazla durumda verimli sorgular oluşturmak, verimsiz sorguların algılanmasını engellemek ve yeni sorguyu aşamalı olarak kolayca tanıtmasını sağlamak için yeniden yazılmıştır Mevcut uygulamalar ve veri sağlayıcılarının özellikleri ve performansı improvementswithout.</span><span class="sxs-lookup"><span data-stu-id="0f248-109">In EF Core 3.0, we've rewritten our LINQ implementation to enable translating more expressions into SQL, to generate efficient queries in more cases, to prevent inefficient queries from going undetected, and to make it easier for us to gradually introduce new query capabilities and performance improvementswithout breaking existing applications and data providers.</span></span>

### <a name="client-evaluation"></a><span data-ttu-id="0f248-110">İstemci değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="0f248-110">Client evaluation</span></span>

<span data-ttu-id="0f248-111">EF Core 3,0 ' deki ana tasarım değişikliğinin SQL veya parametrelere çevrilemez LINQ ifadelerini nasıl işleyeceğinden ilgili olması vardır:</span><span class="sxs-lookup"><span data-stu-id="0f248-111">The main design change in EF Core 3.0 has to do with how it handles LINQ expressions that it cannot translate to SQL or parameters:</span></span>

<span data-ttu-id="0f248-112">İlk birkaç sürümde, bir sorgunun yalnızca bir kısmının SQL 'e çevrilebilmesinin ve istemcinin geri kalanının yürütüldüğü EF Core.</span><span class="sxs-lookup"><span data-stu-id="0f248-112">In the first few versions, EF Core simply figured out what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="0f248-113">Bu tür istemci tarafı yürütme bazı durumlarda istenebilir, ancak çoğu durumda verimsiz sorgulara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="0f248-113">This type of client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>
<span data-ttu-id="0f248-114">Örneğin, EF Core 2,2 bir `Where()` çağrı içindeki bir koşulu çeviremez, filtre olmadan bir SQL ifadesini yürütür, veritabanındaki tüm satırları okuyabilir ve sonra bunları bellek içinde filtreledi.</span><span class="sxs-lookup"><span data-stu-id="0f248-114">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed a SQL statement without a filter, read all all the rows from the database, and then filtered them in-memory.</span></span>
<span data-ttu-id="0f248-115">Bu, veritabanı az sayıda satır içeriyorsa kabul edilebilir, ancak veritabanı büyük bir sayı veya satır içeriyorsa önemli performans sorunlarına veya hatta uygulama hatasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="0f248-115">That may be acceptable if the database contains a small number of rows, but can result in significant performance issues or even application failure if the database contains a large number or rows.</span></span>
<span data-ttu-id="0f248-116">EF Core 3,0 ' de, istemci değerlendirmesinden yalnızca en üst düzey projeksiyonde (son yapılan `Select()`çağrı) yönelik olduğunu sınırlandırdık.</span><span class="sxs-lookup"><span data-stu-id="0f248-116">In EF Core 3.0 we have restricted client evaluation to only happen on the top-level projection (the last call to `Select()`).</span></span>
<span data-ttu-id="0f248-117">EF Core 3,0, sorguda başka herhangi bir yere çevrilemeyen ifadeler algıladığında, çalışma zamanı özel durumu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0f248-117">When EF Core 3.0 detects expressions that cannot be translated anywhere else in the query, it throws a runtime exception.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="0f248-118">Cosmos DB desteği</span><span class="sxs-lookup"><span data-stu-id="0f248-118">Cosmos DB support</span></span> 

<span data-ttu-id="0f248-119">EF Core için Cosmos DB sağlayıcısı, EF programlama modeliyle tanıdık geliştiricilerin, uygulama veritabanı olarak Azure Cosmos DB kolayca hedeflemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="0f248-119">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="0f248-120">Amaç, küresel dağıtım, "her zaman açık" kullanılabilirlik, elastik ölçeklenebilirlik ve düşük gecikme süresi gibi Cosmos DB avantajlarından bazılarını, hatta .NET geliştiricilerine daha erişilebilir hale getirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0f248-120">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="0f248-121">Sağlayıcı, Cosmos DB içindeki SQL API 'sine karşı otomatik değişiklik izleme, LINQ ve değer dönüştürmeleri gibi EF Core özelliklerinin çoğunu etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="0f248-121">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="0f248-122">C#8,0 desteği</span><span class="sxs-lookup"><span data-stu-id="0f248-122">C# 8.0 support</span></span>

<span data-ttu-id="0f248-123">EF Core 3,0, 8,0 ' deki C# bazı yeni özelliklerden yararlanır:</span><span class="sxs-lookup"><span data-stu-id="0f248-123">EF Core 3.0 takes advantage of some of the new features in C# 8.0:</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="0f248-124">Zaman uyumsuz akışlar</span><span class="sxs-lookup"><span data-stu-id="0f248-124">Asynchronous streams</span></span>

<span data-ttu-id="0f248-125">Zaman uyumsuz sorgu sonuçları artık yeni standart `IAsyncEnumerable<T>` arabirim kullanılarak kullanıma sunulmuştur ve kullanılarak `await foreach`tüketilebilir.</span><span class="sxs-lookup"><span data-stu-id="0f248-125">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Proccess(o);
} 
```

### <a name="nullable-reference-types"></a><span data-ttu-id="0f248-126">Boş değer atanabilir başvuru türleri</span><span class="sxs-lookup"><span data-stu-id="0f248-126">Nullable reference types</span></span> 

<span data-ttu-id="0f248-127">Kodunuzda bu yeni özellik etkinleştirildiğinde EF Core, veritabanındaki sütunların ve ilişkilerin null olma durumunu belirlemek için refrence türlerinin (dize veya gezinti özellikleri gibi basit türlerden biri) null olma koşullarına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="0f248-127">When this new feature is enabled in your code, EF Core can reason about the nullability of properties of refrence types (either of primitive types like string or navigation properties) to decide the nullability of columns and relationships in the database.</span></span>

## <a name="interception"></a><span data-ttu-id="0f248-128">Haldedir</span><span class="sxs-lookup"><span data-stu-id="0f248-128">Interception</span></span>

<span data-ttu-id="0f248-129">EF Core 3,0 ' deki yeni yakama API 'SI, bağlantı açma, işlemleri başlatma ve komutları yürütme gibi normal EF Core işleminin bir parçası olarak oluşan düşük düzeyli veritabanı işlemlerinin bir parçası olarak ortaya çıkan, program aracılığıyla gözlemlenmesini ve değiştirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="0f248-129">The new interception API in EF Core 3.0 allows programatically observing and modifying the outcome of low-level database operations that occur as part of the normal operation of EF Core, such as opening connections, initating transactions, and executing commands.</span></span> 

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="0f248-130">Veritabanı görünümlerinin tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="0f248-130">Reverse engineering of database views</span></span>

<span data-ttu-id="0f248-131">Anahtarsız varlık türleri (daha önce [sorgu türleri](xref:core/modeling/query-types)olarak bilinirdi), veritabanından okunabilecek verileri temsil eder, ancak güncelleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="0f248-131">Entity types without keys (previously known as [query types](xref:core/modeling/query-types)) represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="0f248-132">Bu özellik, Çoğu senaryoda veritabanı görünümlerini eşlemek için mükemmel bir uyum sağlar. bu nedenle, ters mühendislik veritabanı görünümlerinde, anahtar olmadan varlık türlerinin oluşturulmasını otomatik hale aldık.</span><span class="sxs-lookup"><span data-stu-id="0f248-132">This characteristic makes them an excellent fit for mapping database views in most scenarios, so we automated the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="0f248-133">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="0f248-133">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="0f248-134">EF Core 3,0 ' den başlayarak, `OrderDetails` aynı tabloya `Order` ait veya açıkça eşlenmiş ise, birincil anahtar ile eşleştirilecektir, ancak tüm özellikler olmadan `Order` `OrderDetails` bir eklenebilir. `OrderDetails` null yapılabilir sütunlar.</span><span class="sxs-lookup"><span data-stu-id="0f248-134">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="0f248-135">Sorgulama yaparken, gerekli özelliklerinden herhangi `OrderDetails` birinin `null` bir değeri yoksa veya birincil `null`anahtar ve tüm Özellikler ' in yanı sıra gerekli özellikleri yoksa EF Core olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="0f248-135">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a><span data-ttu-id="0f248-136">.NET Core üzerinde EF 6,3</span><span class="sxs-lookup"><span data-stu-id="0f248-136">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="0f248-137">Mevcut birçok uygulamanın önceki EF sürümlerini kullandığını anladık ve yalnızca .NET Core 'un avantajlarından yararlanmak için EF Core 'e taşıma işlemleri bazen önemli bir çaba gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="0f248-137">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="0f248-138">Bu nedenle, EF 6 ' nın newewst sürümünü .NET Core 3,0 ' de çalışacak şekilde etkinleştirdik.</span><span class="sxs-lookup"><span data-stu-id="0f248-138">For that reason, we have enabled the newewst version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="0f248-139">Bazı sınırlamalar vardır, örneğin:</span><span class="sxs-lookup"><span data-stu-id="0f248-139">There are some limitations, for example:</span></span>
- <span data-ttu-id="0f248-140">.NET Core üzerinde çalışması için yeni sağlayıcılar gerekir</span><span class="sxs-lookup"><span data-stu-id="0f248-140">New providers are required to work on .NET Core</span></span>
- <span data-ttu-id="0f248-141">SQL Server ile uzamsal destek etkinleştirilmeyecek</span><span class="sxs-lookup"><span data-stu-id="0f248-141">Spatial support with SQL Server won't be enabled</span></span>

## <a name="postponed-features"></a><span data-ttu-id="0f248-142">Ertelenmiş Özellikler</span><span class="sxs-lookup"><span data-stu-id="0f248-142">Postponed features</span></span>

<span data-ttu-id="0f248-143">EF Core 3,0 için başlangıçta planlanmış bazı özellikler gelecek sürümlere ertelendi:</span><span class="sxs-lookup"><span data-stu-id="0f248-143">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span> 

- <span data-ttu-id="0f248-144">[#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725)tarafından izlenen geçişlerde bir modelin parçalarını alma özelliği.</span><span class="sxs-lookup"><span data-stu-id="0f248-144">Ability to ingore parts of a model in migrations, tracked by [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="0f248-145">İki ayrı sorun tarafından izlenen özellik paketi varlıkları: paylaşılan türdeki varlıklar hakkında [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) ve dizinli özellik eşleme desteği hakkında [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) .</span><span class="sxs-lookup"><span data-stu-id="0f248-145">Property bag entities, tracked by two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
