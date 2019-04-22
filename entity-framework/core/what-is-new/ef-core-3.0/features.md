---
title: EF Core 3.0 - EF Core yenilikleri
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 7501a806271c9734e85e31845f260f2d512da077
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58867963"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="132a8-102">EF Core 3. 0 ' (şu anda Önizleme aşamasında) dahil olan yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="132a8-102">New features included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="132a8-103">Özellik kümeleri ve zamanlamaları gelecek sürümlerin her zaman değişikliğe tabi olduğu ve bu sayfada en güncel, en son planlarımızı hiç yansıtmayabilir tutmak deneyeceğiz rağmen zaman lütfen unutmayın.</span><span class="sxs-lookup"><span data-stu-id="132a8-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="132a8-104">Aşağıdaki liste, EF Core 3.0 için planlanan önemli yeni özellikler içerir.</span><span class="sxs-lookup"><span data-stu-id="132a8-104">The following list includes the major new features planned for EF Core 3.0.</span></span>
<span data-ttu-id="132a8-105">Bu özelliklerin çoğu geçerli Önizleme sürümünde yer almaz, ancak RTM ilerlemeyi vermiyoruz olarak kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="132a8-105">Most of these features are not included in the current preview, but will become available as we make progress towards RTM.</span></span>

<span data-ttu-id="132a8-106">Biz odaklanarak uygulamaya planlanan yayın başındaki olan nedeni [bozucu değişiklikler](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span><span class="sxs-lookup"><span data-stu-id="132a8-106">The reason is that at the beginning of the release we are focusing on implementing planned [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span></span>
<span data-ttu-id="132a8-107">En son değişikliklerin birçoğu EF core'a kendi geliştirmeleridir.</span><span class="sxs-lookup"><span data-stu-id="132a8-107">Many of these breaking changes are improvements to EF Core on their own.</span></span>
<span data-ttu-id="132a8-108">Diğer birçok ek geliştirmeler engelini kaldırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="132a8-108">Many others are required to unblock further improvements.</span></span> 

<span data-ttu-id="132a8-109">Hata düzeltmeleri ve geliştirmeleri tam listesi için devam ettiği, gördüğünüz [bu bizim sorun İzleyicisi sorguda](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="132a8-109">For a complete list of bug fixes and enhancements underway, you can see [this query in our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span></span>

## <a name="linq-improvements"></a><span data-ttu-id="132a8-110">LINQ geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="132a8-110">LINQ improvements</span></span> 

[<span data-ttu-id="132a8-111">İzleme sorun #12795</span><span class="sxs-lookup"><span data-stu-id="132a8-111">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

<span data-ttu-id="132a8-112">Bu özellik iş başlatıldı ancak geçerli önizlemede dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="132a8-112">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="132a8-113">LINQ sayesinde, istediğiniz dilde çıkmadan veritabanı sorguları yazma yararlanarak zengin IntelliSense ve derleme zamanı tür denetimi alma bilgilerini yazın.</span><span class="sxs-lookup"><span data-stu-id="132a8-113">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to get IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="132a8-114">Ancak LINQ, sınırsız sayıda karmaşık sorgular yazmaya olanak tanır ve her zaman LINQ sağlayıcıları için çok zor olmuştur.</span><span class="sxs-lookup"><span data-stu-id="132a8-114">But LINQ also enables you to write an unlimited number of complicated queries, and that has always been a huge challenge for LINQ providers.</span></span>
<span data-ttu-id="132a8-115">EF Core birkaç ilk sürümlerinde, biz, kısmi çıkış bir sorgu kısımlarını olabilir hesaplayarak Çözüldü SQL ve istemci üzerindeki bellekte yürütülecek sorgu geri kalanı sağlayarak çevrilir.</span><span class="sxs-lookup"><span data-stu-id="132a8-115">In the first few versions of EF Core, we solved that in part by figuring out what portions of a query could be translated to SQL, and then by allowing the rest of the query to execute in memory on the client.</span></span>
<span data-ttu-id="132a8-116">Bazı durumlarda bu istemci-tarafı yürütme istenebilir ancak diğer birçok durumda, bir uygulama üretime dağıtıldığında kadar belirtilmemiş olabilecek verimsiz sorguları neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="132a8-116">This client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries that may not be identified until an application is deployed to production.</span></span>
<span data-ttu-id="132a8-117">EF Core 3.0 sürümünde, çok büyük değişiklikler LINQ kararlılığımızın nasıl çalıştığını ve bunu nasıl test yapmak planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="132a8-117">In EF Core 3.0, we're planning to make profound changes to how our LINQ implementation works, and how we test it.</span></span>
<span data-ttu-id="132a8-118">Hedefleridir (örneğin sorguları düzeltme eki sürümlerde bozmayı önlemek için), daha sağlam hale getirmek için SQL, daha fazla durumda verimli sorgular oluşturun ve verimsiz sorguları algılanmayan gitmesini önlemek için doğru olarak daha fazla ifade çevirme etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="132a8-118">The goals are to make it more robust (for example, to avoid breaking queries in patch releases), to enable translating more expressions correctly into SQL, to generate efficient queries in more cases, and to prevent inefficient queries from going undetected.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="132a8-119">Cosmos DB desteği</span><span class="sxs-lookup"><span data-stu-id="132a8-119">Cosmos DB support</span></span> 

[<span data-ttu-id="132a8-120">İzleme sorun #8443</span><span class="sxs-lookup"><span data-stu-id="132a8-120">Tracking Issue #8443</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

<span data-ttu-id="132a8-121">Bu özellik geçerli Önizleme'de dahil, ancak henüz tamamlanmadı.</span><span class="sxs-lookup"><span data-stu-id="132a8-121">This feature is included in the current preview, but isn't complete yet.</span></span> 

<span data-ttu-id="132a8-122">Bir Cosmos DB sağlayıcısı için bir uygulama veritabanının olarak Azure Cosmos DB kolayca hedeflemek geliştiricilerin EF ölçeklenebilirliğinden modeli için EF Core üzerinde çalışıyoruz.</span><span class="sxs-lookup"><span data-stu-id="132a8-122">We're working on a Cosmos DB provider for EF Core, to enable developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="132a8-123">Hedef, Cosmos DB, küresel dağıtım gibi avantajlarından bazıları "her zaman açık" olmasını sağlamaktır kullanılabilirlik, esnek ölçeklenebilirlik ve düşük gecikme süresi, .NET geliştiricileri için daha erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="132a8-123">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="132a8-124">EF Core gibi özelliklerin çoğu, Cosmos DB SQL API karşı değer dönüştürmeleri otomatik değişiklik izleme ve LINQ sağlayıcı etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="132a8-124">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>
<span data-ttu-id="132a8-125">EF Core 2.2 önce bu çalışmaların Başladık ve [bazı yaptık Önizleme sürümleri kullanılabilir sağlayıcısının](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span><span class="sxs-lookup"><span data-stu-id="132a8-125">We started this effort before EF Core 2.2, and [we have made some preview versions of the provider available](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span></span>
<span data-ttu-id="132a8-126">Yeni plan EF Core 3.0 yanı sıra sağlayıcı geliştirmeye devam sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="132a8-126">The new plan is to continue developing the provider alongside EF Core 3.0.</span></span> 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="132a8-127">Bağımlı varlıkları tablo sorumlusuyla paylaşımı artık isteğe bağlıdır</span><span class="sxs-lookup"><span data-stu-id="132a8-127">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="132a8-128">İzleme sorun #9005</span><span class="sxs-lookup"><span data-stu-id="132a8-128">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="132a8-129">EF Core 3.0-preview 4 sürümünde bu özellik sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="132a8-129">This feature will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="132a8-130">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="132a8-130">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

<span data-ttu-id="132a8-131">EF Core 3.0 ile başlatılmasının `OrderDetails` tarafından sahip olunan `Order` veya eklenmesi mümkün olacaktır aynı tabloya açıkça eşleştirilmiş bir `Order` olmadan bir `OrderDetails` ve tüm `OrderDetails` için özellikleri birincil anahtar dışındaki eşleştirilir boş değer atanabilir sütun.</span><span class="sxs-lookup"><span data-stu-id="132a8-131">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties except the primary key will be mapped to nullable columns.</span></span>
<span data-ttu-id="132a8-132">EF Core sorgulama ayarlandığında `OrderDetails` için `null` gerekli özelliklerinden birini yoksa, bir değer veya birincil anahtarı yanı sıra gereken özellikleri yoktur ve tüm özellikleri `null`.</span><span class="sxs-lookup"><span data-stu-id="132a8-132">When querying EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="132a8-133">C#8.0 desteği</span><span class="sxs-lookup"><span data-stu-id="132a8-133">C# 8.0 support</span></span>

<span data-ttu-id="132a8-134">[Sorun #12047 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[sorun #10347 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span><span class="sxs-lookup"><span data-stu-id="132a8-134">[Tracking Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Tracking Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span></span>

<span data-ttu-id="132a8-135">Bu özellik iş başlatıldı ancak geçerli önizlemede dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="132a8-135">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="132a8-136">Bazı yararlanmak için müşterilerimizin istiyoruz [gelen yeni özellikler C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) istediğiniz zaman uyumsuz akışlar (dahil olmak üzere `await foreach`) ve EF Core kullanırken null başvuru türleri.</span><span class="sxs-lookup"><span data-stu-id="132a8-136">We want our customers to take advantage of some of the [new features coming in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) like async streams (including `await foreach`) and nullable reference types while using EF Core.</span></span>

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="132a8-137">Tersine mühendislik veritabanı görünümü</span><span class="sxs-lookup"><span data-stu-id="132a8-137">Reverse engineering of database views</span></span>

[<span data-ttu-id="132a8-138">İzleme sorun #1679</span><span class="sxs-lookup"><span data-stu-id="132a8-138">Tracking Issue #1679</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

<span data-ttu-id="132a8-139">Bu özellik geçerli Önizleme sürümünde yer almıyor.</span><span class="sxs-lookup"><span data-stu-id="132a8-139">This feature isn't included in the current preview.</span></span>

<span data-ttu-id="132a8-140">[Sorgu türü](xref:core/modeling/query-types), EF Core 2.1 içinde tanıtılan ve kabul EF Core 3.0, veritabanından okunan ancak güncelleştirilemiyor temsil veri anahtarları olmadan varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="132a8-140">[Query types](xref:core/modeling/query-types), introduced in EF Core 2.1 and considered entity types without keys in EF Core 3.0, represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="132a8-141">Varlık türleri geriye doğru olduğunda veritabanı görünümleri mühendislik anahtarları olmadan oluşturulmasını otomatik hale getirmek planlıyoruz için bu özellik, bunların veritabanı görünümleri için mükemmel bir uyum Çoğu senaryoda sağlar.</span><span class="sxs-lookup"><span data-stu-id="132a8-141">This characteristic makes them an excellent fit for database views in most scenarios, so we plan to automate the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="property-bag-entities"></a><span data-ttu-id="132a8-142">Özellik paketi varlıklar</span><span class="sxs-lookup"><span data-stu-id="132a8-142">Property bag entities</span></span>

<span data-ttu-id="132a8-143">[Sorun #13610 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/13610) ve [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span><span class="sxs-lookup"><span data-stu-id="132a8-143">[Tracking Issue #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) and [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span></span>

<span data-ttu-id="132a8-144">Bu özellik iş başlatıldı ancak geçerli önizlemede dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="132a8-144">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="132a8-145">Dizinli Özellikler normal özellikleri yerine verileri depolayan varlıkları etkinleştirme ve ayrıca aynı .NET sınıf örnekleri kullanabilmek için hakkındaki bu özellik, (büyük olasılıkla bir şey kadar basit bir `Dictionary<string, object>`) farklı varlık türlerini temsil etmek için aynı EF Core modelinde.</span><span class="sxs-lookup"><span data-stu-id="132a8-145">This feature is about enabling entities that store data in indexed properties instead of regular properties, and also about being able to use instances of the same .NET class (potentially something as simple as a `Dictionary<string, object>`) to represent different entity types in the same EF Core model.</span></span>
<span data-ttu-id="132a8-146">Bu özellik, birleştirme varlık olmadan çok-çok ilişkileri desteklemek için bir atlama taşı ([sorun #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), EF Core için en çok istenen geliştirmeleri birinde.</span><span class="sxs-lookup"><span data-stu-id="132a8-146">This feature is a stepping stone to support many-to-many relationships without a join entity ([issue #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), which is one of the most requested improvements for EF Core.</span></span>

## <a name="ef-63-on-net-core"></a><span data-ttu-id="132a8-147">.NET core'da EF 6.3</span><span class="sxs-lookup"><span data-stu-id="132a8-147">EF 6.3 on .NET Core</span></span>

[<span data-ttu-id="132a8-148">Sorunu EF6 izleme #271</span><span class="sxs-lookup"><span data-stu-id="132a8-148">Tracking Issue EF6#271</span></span>](https://github.com/aspnet/EntityFramework6/issues/271)

<span data-ttu-id="132a8-149">Bu özellik iş başlatıldı ancak geçerli önizlemede dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="132a8-149">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="132a8-150">Birçok mevcut uygulamaları EF'ın önceki sürümlerini kullanın ve bunları yalnızca .NET Core yararlanmak için EF Core'a taşıma bazen önemli bir efor gerektirebilir olduğunu biliyoruz.</span><span class="sxs-lookup"><span data-stu-id="132a8-150">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="132a8-151">Bu nedenle, biz .NET Core 3.0 üzerinde çalıştırılacak EF 6'ın sonraki sürümü adapte.</span><span class="sxs-lookup"><span data-stu-id="132a8-151">For that reason, we will be adapting the next version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="132a8-152">Biz bunu çok az değişiklikle taşıma mevcut uygulamaları kolaylaştırmak için yapıyor.</span><span class="sxs-lookup"><span data-stu-id="132a8-152">We are doing this to facilitate porting existing applications with minimal changes.</span></span>
<span data-ttu-id="132a8-153">Olan oraya giden bazı sınırlamalar olacak.</span><span class="sxs-lookup"><span data-stu-id="132a8-153">There are going to be some limitations.</span></span> <span data-ttu-id="132a8-154">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="132a8-154">For example:</span></span>
- <span data-ttu-id="132a8-155">Diğer veritabanlarını dahil edilen SQL Server destek yanı sıra .NET Core üzerinde çalışmak yeni sağlayıcılar gerekir</span><span class="sxs-lookup"><span data-stu-id="132a8-155">It will require new providers to work with other databases besides the included SQL Server support on .NET Core</span></span>
- <span data-ttu-id="132a8-156">SQL Server uzamsal desteği etkinleştirilemez</span><span class="sxs-lookup"><span data-stu-id="132a8-156">Spatial support with SQL Server won't be enabled</span></span>

<span data-ttu-id="132a8-157">Ayrıca, bu noktada EF 6 için planlanan yeni özellik olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="132a8-157">Note also that there are no new features planned for EF 6 at this point.</span></span>
