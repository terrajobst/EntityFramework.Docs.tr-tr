---
title: EF Core 3,0 ' deki yeni özellikler EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: a71aa01e81d9830d7b9e6cb01c200851100a15df
ms.sourcegitcommit: 87e72899d17602f7526d6ccd22f3c8ee844145df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69628423"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="5c331-102">EF Core 3,0 ' de bulunan yeni özellikler (Şu anda önizlemede)</span><span class="sxs-lookup"><span data-stu-id="5c331-102">New features included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5c331-103">Gelecekteki sürümlerin özellik kümelerinin ve zamanlamalarının her zaman değişikliğe tabi olduğunu ve bu sayfayı güncel tutmaya deneydiğimiz halde, her zaman en son planlarımızı yansıtmadığımızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="5c331-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="5c331-104">Aşağıdaki listede EF Core 3,0 için planlanmış olan başlıca yeni özellikler yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="5c331-104">The following list includes the major new features planned for EF Core 3.0.</span></span>
<span data-ttu-id="5c331-105">Bu özelliklerin çoğu geçerli önizlemeye dahil edilmez, ancak RTM 'ye doğru ilerleme yaptığımız için kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="5c331-105">Most of these features are not included in the current preview, but will become available as we make progress towards RTM.</span></span>

<span data-ttu-id="5c331-106">Bunun nedeni, yayının başlangıcında planlanmış [son değişiklikleri](xref:core/what-is-new/ef-core-3.0/breaking-changes)uygulamaya odaklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5c331-106">The reason is that at the beginning of the release we are focusing on implementing planned [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span></span>
<span data-ttu-id="5c331-107">Bu önemli değişikliklerden çoğu, kendi EF Core geliştirmelerdir.</span><span class="sxs-lookup"><span data-stu-id="5c331-107">Many of these breaking changes are improvements to EF Core on their own.</span></span>
<span data-ttu-id="5c331-108">Diğer iyileştirmeler, daha fazla geliştirmelerin engelini kaldırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="5c331-108">Many others are required to unblock further improvements.</span></span> 

<span data-ttu-id="5c331-109">Hata düzeltmelerinin ve geliştirmelerin tüm listesi için [Bu sorguyu sorun izleyicimizde](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc)görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5c331-109">For a complete list of bug fixes and enhancements underway, you can see [this query in our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span></span>

## <a name="linq-improvements"></a><span data-ttu-id="5c331-110">LINQ geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="5c331-110">LINQ improvements</span></span> 

[<span data-ttu-id="5c331-111">Sorun izleniyor #12795</span><span class="sxs-lookup"><span data-stu-id="5c331-111">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

<span data-ttu-id="5c331-112">Bu özellik üzerinde çalışma başlatıldı, ancak geçerli önizlemeye dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="5c331-112">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="5c331-113">LINQ, IntelliSense ve derleme zamanı tür denetimi almak için zengin tür bilgilerinizin avantajlarından yararlanarak, seçtiğiniz dilden çıkmadan veritabanı sorguları yazmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="5c331-113">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to get IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="5c331-114">Ancak LINQ Ayrıca sınırsız sayıda karmaşık sorgu yazmanızı sağlar ve bu, LINQ sağlayıcıları için her zaman çok büyük bir zorluk olmuştur.</span><span class="sxs-lookup"><span data-stu-id="5c331-114">But LINQ also enables you to write an unlimited number of complicated queries, and that has always been a huge challenge for LINQ providers.</span></span>
<span data-ttu-id="5c331-115">EF Core ilk birkaç sürümünde, bir sorgunun hangi bölümlerinin SQL 'e çevrilebileceğini öğrenerek ve sonra sorgunun geri kalanının istemci üzerinde bellekte yürütülmesine izin vererek bu bölümü çözeceğiz.</span><span class="sxs-lookup"><span data-stu-id="5c331-115">In the first few versions of EF Core, we solved that in part by figuring out what portions of a query could be translated to SQL, and then by allowing the rest of the query to execute in memory on the client.</span></span>
<span data-ttu-id="5c331-116">Bu istemci tarafı yürütme bazı durumlarda istenebilir, ancak çoğu durumda, bir uygulama üretime dağıtılana kadar belirlenemeyebilir verimsiz sorgulara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="5c331-116">This client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries that may not be identified until an application is deployed to production.</span></span>
<span data-ttu-id="5c331-117">EF Core 3,0 ' de, LINQ uygulamanızın nasıl çalıştığı ve nasıl test ettiğimiz üzerinde yapılan değişiklikleri yapmayı planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="5c331-117">In EF Core 3.0, we're planning to make profound changes to how our LINQ implementation works, and how we test it.</span></span>
<span data-ttu-id="5c331-118">Hedefler daha sağlam hale gelir (örneğin, düzeltme eki sürümlerindeki sorguların kesilmesini önlemek için), daha fazla ifadeyi SQL 'e doğru çevirmeyi etkinleştirmek, daha fazla durumda etkili sorgular oluşturmak ve verimsiz sorguların algılanmasını önlemek için.</span><span class="sxs-lookup"><span data-stu-id="5c331-118">The goals are to make it more robust (for example, to avoid breaking queries in patch releases), to enable translating more expressions correctly into SQL, to generate efficient queries in more cases, and to prevent inefficient queries from going undetected.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="5c331-119">Cosmos DB desteği</span><span class="sxs-lookup"><span data-stu-id="5c331-119">Cosmos DB support</span></span> 

[<span data-ttu-id="5c331-120">Sorun izleniyor #8443</span><span class="sxs-lookup"><span data-stu-id="5c331-120">Tracking Issue #8443</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

<span data-ttu-id="5c331-121">Bu özellik geçerli önizlemeye dahildir, ancak henüz tamamlanmadı.</span><span class="sxs-lookup"><span data-stu-id="5c331-121">This feature is included in the current preview, but isn't complete yet.</span></span> 

<span data-ttu-id="5c331-122">EF Core için bir Cosmos DB sağlayıcı üzerinde çalışıyoruz, böylece bir uygulama veritabanı olarak Azure Cosmos DB kolayca hedeflemek için EF programlama modeliyle tanıdık geliştiricilerin kolayca hedeflemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="5c331-122">We're working on a Cosmos DB provider for EF Core, to enable developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="5c331-123">Amaç, küresel dağıtım, "her zaman açık" kullanılabilirlik, elastik ölçeklenebilirlik ve düşük gecikme süresi gibi Cosmos DB avantajlarından bazılarını, hatta .NET geliştiricilerine daha erişilebilir hale getirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5c331-123">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="5c331-124">Sağlayıcı, Cosmos DB içindeki SQL API 'sine karşı otomatik değişiklik izleme, LINQ ve değer dönüştürmeleri gibi EF Core özelliklerinin çoğunu etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="5c331-124">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>
<span data-ttu-id="5c331-125">Bu çabadan önce 2,2 EF Core önce başladık ve [sağlayıcının bazı önizleme sürümlerini kullanıma](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/)sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="5c331-125">We started this effort before EF Core 2.2, and [we have made some preview versions of the provider available](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span></span>
<span data-ttu-id="5c331-126">Yeni plan, sağlayıcıyı EF Core 3,0 ' de geliştirmeye devam etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5c331-126">The new plan is to continue developing the provider alongside EF Core 3.0.</span></span> 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="5c331-127">Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="5c331-127">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="5c331-128">Sorun izleniyor #9005</span><span class="sxs-lookup"><span data-stu-id="5c331-128">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="5c331-129">Bu özellik EF Core 3,0-Preview 4 ' te tanıtılacaktır.</span><span class="sxs-lookup"><span data-stu-id="5c331-129">This feature will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5c331-130">Aşağıdaki modeli göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5c331-130">Consider the following model:</span></span>
```C#
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

<span data-ttu-id="5c331-131">EF Core 3,0 ' den başlayarak, `OrderDetails` aynı tabloya `Order` ait veya açıkça eşlenmiş ise, birincil anahtar ile eşleştirilecektir, ancak tüm özellikler olmadan `Order` `OrderDetails` bir eklenebilir. `OrderDetails` null yapılabilir sütunlar.</span><span class="sxs-lookup"><span data-stu-id="5c331-131">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="5c331-132">Sorgulama yaparken, gerekli özelliklerinden herhangi `OrderDetails` birinin `null` bir değeri yoksa veya birincil `null`anahtar ve tüm Özellikler ' in yanı sıra gerekli özellikleri yoksa EF Core olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="5c331-132">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="5c331-133">C#8,0 desteği</span><span class="sxs-lookup"><span data-stu-id="5c331-133">C# 8.0 support</span></span>

<span data-ttu-id="5c331-134">[](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
Sorun izleme #12047 izleme sorunu[#10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span><span class="sxs-lookup"><span data-stu-id="5c331-134">[Tracking Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Tracking Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span></span>

<span data-ttu-id="5c331-135">Bu özellik üzerinde çalışma başlatıldı, ancak geçerli önizlemeye dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="5c331-135">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="5c331-136">Müşterilerimizin, EF Core kullanılırken zaman uyumsuz akışlar (dahil `await foreach`) ve null yapılabilir başvuru türleri gibi [8,0 ' de C# gelen yeni özelliklerden](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) yararlanabilmeleri istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="5c331-136">We want our customers to take advantage of some of the [new features coming in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) like async streams (including `await foreach`) and nullable reference types while using EF Core.</span></span>

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="5c331-137">Veritabanı görünümlerinin tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="5c331-137">Reverse engineering of database views</span></span>

[<span data-ttu-id="5c331-138">Sorun izleniyor #1679</span><span class="sxs-lookup"><span data-stu-id="5c331-138">Tracking Issue #1679</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

<span data-ttu-id="5c331-139">Bu özellik geçerli önizlemeye dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="5c331-139">This feature isn't included in the current preview.</span></span>

<span data-ttu-id="5c331-140">EF Core 2,1 ' de tanıtılan ve EF Core 3,0 ' de anahtarlar olmadan varlık türlerini kabul eden [sorgu türleri](xref:core/modeling/query-types), veritabanından okunabilecek verileri temsil eder, ancak güncelleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="5c331-140">[Query types](xref:core/modeling/query-types), introduced in EF Core 2.1 and considered entity types without keys in EF Core 3.0, represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="5c331-141">Bu özellik, Çoğu senaryoda veritabanı görünümlerine mükemmel bir uyum sağlar. bu nedenle, tersine mühendislik veritabanı görünümlerinde, anahtar olmadan varlık türlerinin oluşturulmasını otomatikleştirmeyi planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="5c331-141">This characteristic makes them an excellent fit for database views in most scenarios, so we plan to automate the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="property-bag-entities"></a><span data-ttu-id="5c331-142">Özellik paketi varlıkları</span><span class="sxs-lookup"><span data-stu-id="5c331-142">Property bag entities</span></span>

<span data-ttu-id="5c331-143">[Sorun #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) ve [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) izleniyor</span><span class="sxs-lookup"><span data-stu-id="5c331-143">[Tracking Issue #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) and [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span></span>

<span data-ttu-id="5c331-144">Bu özellik üzerinde çalışma başlatıldı, ancak geçerli önizlemeye dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="5c331-144">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="5c331-145">Bu özellik, verileri düzenli özellikler yerine dizini oluşturulmuş özelliklerde depolayan varlıkların etkinleştirilmesi ve ayrıca, farklı varlık türlerini temsil etmek için aynı .net sınıfının (potansiyel olarak basit gibi `Dictionary<string, object>`) örneklerini kullanabilmekle ilgilidir. aynı EF Core modelinde.</span><span class="sxs-lookup"><span data-stu-id="5c331-145">This feature is about enabling entities that store data in indexed properties instead of regular properties, and also about being able to use instances of the same .NET class (potentially something as simple as a `Dictionary<string, object>`) to represent different entity types in the same EF Core model.</span></span>
<span data-ttu-id="5c331-146">Bu özellik, EF Core için en çok istenen geliştirmelerden biri olan bir JOIN varlığı ([sorun #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)) olmadan çok-çok ilişkilerini desteklemeye yönelik bir atlama Stone ' dir.</span><span class="sxs-lookup"><span data-stu-id="5c331-146">This feature is a stepping stone to support many-to-many relationships without a join entity ([issue #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), which is one of the most requested improvements for EF Core.</span></span>

## <a name="ef-63-on-net-core"></a><span data-ttu-id="5c331-147">.NET Core üzerinde EF 6,3</span><span class="sxs-lookup"><span data-stu-id="5c331-147">EF 6.3 on .NET Core</span></span>

[<span data-ttu-id="5c331-148">Sorun izleme EF6 # 271</span><span class="sxs-lookup"><span data-stu-id="5c331-148">Tracking Issue EF6#271</span></span>](https://github.com/aspnet/EntityFramework6/issues/271)

<span data-ttu-id="5c331-149">Bu özellik üzerinde çalışma başlatıldı, ancak geçerli önizlemeye dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="5c331-149">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="5c331-150">Mevcut birçok uygulamanın önceki EF sürümlerini kullandığını anladık ve yalnızca .NET Core 'un avantajlarından yararlanmak için EF Core 'e taşıma işlemleri bazen önemli bir çaba gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="5c331-150">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="5c331-151">Bu nedenle, .NET Core 3,0 ' de çalışacak EF 6 ' nın bir sonraki sürümünü uyarlarız.</span><span class="sxs-lookup"><span data-stu-id="5c331-151">For that reason, we will be adapting the next version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="5c331-152">Bunu, mevcut uygulamaların en az değişiklikle yapılmasını kolaylaştırmak için yapıyoruz.</span><span class="sxs-lookup"><span data-stu-id="5c331-152">We are doing this to facilitate porting existing applications with minimal changes.</span></span>
<span data-ttu-id="5c331-153">Bazı sınırlamalar olacak.</span><span class="sxs-lookup"><span data-stu-id="5c331-153">There are going to be some limitations.</span></span> <span data-ttu-id="5c331-154">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5c331-154">For example:</span></span>
- <span data-ttu-id="5c331-155">.NET Core 'da eklenen SQL Server desteğinin yanı sıra yeni sağlayıcıların diğer veritabanlarıyla çalışması gerekir</span><span class="sxs-lookup"><span data-stu-id="5c331-155">It will require new providers to work with other databases besides the included SQL Server support on .NET Core</span></span>
- <span data-ttu-id="5c331-156">SQL Server ile uzamsal destek etkinleştirilmeyecek</span><span class="sxs-lookup"><span data-stu-id="5c331-156">Spatial support with SQL Server won't be enabled</span></span>

<span data-ttu-id="5c331-157">Ayrıca, bu noktada EF 6 için planlanmış yeni bir özellik olmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="5c331-157">Note also that there are no new features planned for EF 6 at this point.</span></span>
