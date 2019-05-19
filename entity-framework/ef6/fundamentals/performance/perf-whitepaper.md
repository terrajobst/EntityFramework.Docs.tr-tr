---
title: EF4 EF5 ve EF6 için performans konuları
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: f8fa1001c85366e169cf50e89efdb65bd92b671e
ms.sourcegitcommit: f277883a5ed28eba57d14aaaf17405bc1ae9cf94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/18/2019
ms.locfileid: "65874607"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a><span data-ttu-id="33c5f-102">EF 6 4 ve 5 için performans konuları</span><span class="sxs-lookup"><span data-stu-id="33c5f-102">Performance considerations for EF 4, 5, and 6</span></span>
<span data-ttu-id="33c5f-103">David Obando, Eric Dettinger ve diğerleri</span><span class="sxs-lookup"><span data-stu-id="33c5f-103">By David Obando, Eric Dettinger and others</span></span>

<span data-ttu-id="33c5f-104">Yayım Tarihi: Nisan 2012</span><span class="sxs-lookup"><span data-stu-id="33c5f-104">Published: April 2012</span></span>

<span data-ttu-id="33c5f-105">Son güncelleme tarihi: Mayıs 2014</span><span class="sxs-lookup"><span data-stu-id="33c5f-105">Last updated: May 2014</span></span>

------------------------------------------------------------------------

## <a name="1-introduction"></a><span data-ttu-id="33c5f-106">1. Giriş</span><span class="sxs-lookup"><span data-stu-id="33c5f-106">1. Introduction</span></span>

<span data-ttu-id="33c5f-107">Nesne İlişkisel eşleme çerçeveleri, nesne yönelimli bir uygulamada veri erişimi için bir soyutlamayı sağlamak için kullanışlı bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-107">Object-Relational Mapping frameworks are a convenient way to provide an abstraction for data access in an object-oriented application.</span></span> <span data-ttu-id="33c5f-108">.NET uygulamaları için Microsoft'un O/RM Entity Framework önerilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-108">For .NET applications, Microsoft's recommended O/RM is Entity Framework.</span></span> <span data-ttu-id="33c5f-109">Herhangi bir soyutlama ile yine de önemli performans hale gelebilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-109">With any abstraction though, performance can become a concern.</span></span>

<span data-ttu-id="33c5f-110">Bu teknik incelemede, geliştiriciler performansını etkileyebilir Entity Framework iç algoritmaları hakkında fikir verir ve araştırma için ipuçları sağlamak için Entity Framework kullanan uygulamalar geliştirirken performansla ilgili önemli noktalar göstermek için yazılmıştır ve Entity Framework kullanan uygulamalarında performans iyileştirme.</span><span class="sxs-lookup"><span data-stu-id="33c5f-110">This whitepaper was written to show the performance considerations when developing applications using Entity Framework, to give developers an idea of the Entity Framework internal algorithms that can affect performance, and to provide tips for investigation and improving performance in their applications that use Entity Framework.</span></span> <span data-ttu-id="33c5f-111">Kullanılabilir birkaç performans üzerinde iyi konuların zaten web üzerinde ve size mümkün olduğunda bu kaynakları şuraya işaret eden çalıştınız.</span><span class="sxs-lookup"><span data-stu-id="33c5f-111">There are a number of good topics on performance already available on the web, and we've also tried pointing to these resources where possible.</span></span>

<span data-ttu-id="33c5f-112">Performans karmaşık bir konudur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-112">Performance is a tricky topic.</span></span> <span data-ttu-id="33c5f-113">Bu teknik incelemede bir kaynak olarak yardımcı olmak amacıyla performans yaptığınız hazırlanmıştır ilgili kararları Entity Framework kullanan uygulamalarınız için.</span><span class="sxs-lookup"><span data-stu-id="33c5f-113">This whitepaper is intended as a resource to help you make performance related decisions for your applications that use Entity Framework.</span></span> <span data-ttu-id="33c5f-114">Performans göstermek için bazı test ölçüm ekledik ancak bu ölçümlerin mutlak uygulamanızda göreceğiniz performans göstergelerini olarak sağlanmasıyla.</span><span class="sxs-lookup"><span data-stu-id="33c5f-114">We have included some test metrics to demonstrate performance, but these metrics aren't intended as absolute indicators of the performance you will see in your application.</span></span>

<span data-ttu-id="33c5f-115">Pratik amaçlar için bu belgede, Entity Framework 4, .NET 4.0 ve Entity Framework 5 altında çalıştırılır ve 6 .NET 4.5 altında çalışan varsayar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-115">For practical purposes, this document assumes Entity Framework 4 is run under .NET 4.0 and Entity Framework 5 and 6 are run under .NET 4.5.</span></span> <span data-ttu-id="33c5f-116">Entity Framework 5 için yapılan performans geliştirmelerin çoğunu, .NET 4.5 ile birlikte gelen temel bileşenleri içinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-116">Many of the performance improvements made for Entity Framework 5 reside within the core components that ship with .NET 4.5.</span></span>

<span data-ttu-id="33c5f-117">Entity Framework 6 bir bant sürüm olduğundan ve .NET ile birlikte gelen Entity Framework bileşenleri, bağlı değildir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-117">Entity Framework 6 is an out of band release and does not depend on the Entity Framework components that ship with .NET.</span></span> <span data-ttu-id="33c5f-118">Entity Framework 6 hem .NET 4.0 hem de .NET 4.5 üzerinde çalışır ve bir büyük performans avantajı adresinden .NET 4.0 yükseltme henüz ancak kendi son Entity Framework BITS isteyen kişiler sunabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-118">Entity Framework 6 work on both .NET 4.0 and .NET 4.5, and can offer a big performance benefit to those who haven’t upgraded from .NET 4.0 but want the latest Entity Framework bits in their application.</span></span> <span data-ttu-id="33c5f-119">Bu belge, Entity Framework 6 bahsetmeleri, bu makalenin yazıldığı sırada kullanılabilir en son sürüme başvuran: 6.1.0 sürümü.</span><span class="sxs-lookup"><span data-stu-id="33c5f-119">When this document mentions Entity Framework 6, it refers to the latest version available at the time of this writing: version 6.1.0.</span></span>

## <a name="2-cold-vs-warm-query-execution"></a><span data-ttu-id="33c5f-120">2. Soğuk vs. Orta Gecikmeli sorgu yürütme</span><span class="sxs-lookup"><span data-stu-id="33c5f-120">2. Cold vs. Warm Query Execution</span></span>

<span data-ttu-id="33c5f-121">Herhangi bir sorgu, belirli bir model karşı yapılan ilk kez Entity Framework birçok iş yüklemek ve model doğrulamak için arka planda gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-121">The very first time any query is made against a given model, the Entity Framework does a lot of work behind the scenes to load and validate the model.</span></span> <span data-ttu-id="33c5f-122">Biz bu ilk sorgu için "soğuk" sorgu olarak sık bakın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-122">We frequently refer to this first query as a "cold" query.</span></span><span data-ttu-id="33c5f-123">  Daha önceden yüklenmiş bir modeli sorguları "sıcak" sorgu olarak bilinir ve çok daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-123">  Further queries against an already loaded model are known as "warm" queries, and are much faster.</span></span>

<span data-ttu-id="33c5f-124">Şimdi Entity Framework kullanarak bir sorgu yürütülürken zaman nerede harcandığını bir üst düzey görünümü yararlanın ve şeyler Entity Framework 6'da burada geliştirdiğinizi bakın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-124">Let’s take a high-level view of where time is spent when executing a query using Entity Framework, and see where things are improving in Entity Framework 6.</span></span>

<span data-ttu-id="33c5f-125">**İlk sorgu yürütme – soğuk sorgu**</span><span class="sxs-lookup"><span data-stu-id="33c5f-125">**First Query Execution – cold query**</span></span>

| <span data-ttu-id="33c5f-126">Kod kullanıcı yazma</span><span class="sxs-lookup"><span data-stu-id="33c5f-126">Code User Writes</span></span>                                                                                     | <span data-ttu-id="33c5f-127">Eylem</span><span class="sxs-lookup"><span data-stu-id="33c5f-127">Action</span></span>                    | <span data-ttu-id="33c5f-128">EF4 Performans etkisi</span><span class="sxs-lookup"><span data-stu-id="33c5f-128">EF4 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                        | <span data-ttu-id="33c5f-129">EF5 Performans etkisi</span><span class="sxs-lookup"><span data-stu-id="33c5f-129">EF5 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                    | <span data-ttu-id="33c5f-130">EF6 Performans etkisi</span><span class="sxs-lookup"><span data-stu-id="33c5f-130">EF6 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | <span data-ttu-id="33c5f-131">Bağlam oluşturma</span><span class="sxs-lookup"><span data-stu-id="33c5f-131">Context creation</span></span>          | <span data-ttu-id="33c5f-132">Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-132">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                        | <span data-ttu-id="33c5f-133">Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-133">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | <span data-ttu-id="33c5f-134">Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-134">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | <span data-ttu-id="33c5f-135">Sorgu ifadesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="33c5f-135">Query expression creation</span></span> | <span data-ttu-id="33c5f-136">Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-136">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="33c5f-137">Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-137">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="33c5f-138">Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-138">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | <span data-ttu-id="33c5f-139">LINQ Sorgu yürütme</span><span class="sxs-lookup"><span data-stu-id="33c5f-139">LINQ query execution</span></span>      | <span data-ttu-id="33c5f-140">-Meta verileri yükleniyor: Önbelleğe alınmış ancak yüksek</span><span class="sxs-lookup"><span data-stu-id="33c5f-140">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="33c5f-141">-Görünüm oluşturma: Önbelleğe alınmış ancak potansiyel olarak çok yüksek</span><span class="sxs-lookup"><span data-stu-id="33c5f-141">- View generation: Potentially very high but cached</span></span> <br/> <span data-ttu-id="33c5f-142">-Değerlendirme parametresi: Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-142">- Parameter evaluation: Medium</span></span> <br/> <span data-ttu-id="33c5f-143">-Sorgu çevirisi: Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-143">- Query translation: Medium</span></span> <br/> <span data-ttu-id="33c5f-144">-Materializer oluşturma: Önbelleğe alınmış ancak orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-144">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="33c5f-145">-Veritabanı sorgusu yürütme: Büyük olasılıkla yüksek</span><span class="sxs-lookup"><span data-stu-id="33c5f-145">- Database query execution: Potentially high</span></span> <br/> <span data-ttu-id="33c5f-146">+ Connection.Open</span><span class="sxs-lookup"><span data-stu-id="33c5f-146">+ Connection.Open</span></span> <br/> <span data-ttu-id="33c5f-147">+ Command.ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="33c5f-147">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="33c5f-148">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="33c5f-148">+ DataReader.Read</span></span> <br/> <span data-ttu-id="33c5f-149">Nesne gerçekleştirme: Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-149">Object materialization: Medium</span></span> <br/> <span data-ttu-id="33c5f-150">-İdentity arama: Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-150">- Identity lookup: Medium</span></span> | <span data-ttu-id="33c5f-151">-Meta verileri yükleniyor: Önbelleğe alınmış ancak yüksek</span><span class="sxs-lookup"><span data-stu-id="33c5f-151">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="33c5f-152">-Görünüm oluşturma: Önbelleğe alınmış ancak potansiyel olarak çok yüksek</span><span class="sxs-lookup"><span data-stu-id="33c5f-152">- View generation: Potentially very high but cached</span></span> <br/> <span data-ttu-id="33c5f-153">-Değerlendirme parametresi: Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-153">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="33c5f-154">-Sorgu çevirisi: Önbelleğe alınmış ancak orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-154">- Query translation: Medium but cached</span></span> <br/> <span data-ttu-id="33c5f-155">-Materializer oluşturma: Önbelleğe alınmış ancak orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-155">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="33c5f-156">-Veritabanı sorgusu yürütme: Yüksek (bazı durumlarda sorguların daha iyi)</span><span class="sxs-lookup"><span data-stu-id="33c5f-156">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="33c5f-157">+ Connection.Open</span><span class="sxs-lookup"><span data-stu-id="33c5f-157">+ Connection.Open</span></span> <br/> <span data-ttu-id="33c5f-158">+ Command.ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="33c5f-158">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="33c5f-159">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="33c5f-159">+ DataReader.Read</span></span> <br/> <span data-ttu-id="33c5f-160">Nesne gerçekleştirme: Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-160">Object materialization: Medium</span></span> <br/> <span data-ttu-id="33c5f-161">-İdentity arama: Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-161">- Identity lookup: Medium</span></span> | <span data-ttu-id="33c5f-162">-Meta verileri yükleniyor: Önbelleğe alınmış ancak yüksek</span><span class="sxs-lookup"><span data-stu-id="33c5f-162">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="33c5f-163">-Görünüm oluşturma: Önbelleğe alınmış ancak orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-163">- View generation: Medium but cached</span></span> <br/> <span data-ttu-id="33c5f-164">-Değerlendirme parametresi: Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-164">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="33c5f-165">-Sorgu çevirisi: Önbelleğe alınmış ancak orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-165">- Query translation: Medium but cached</span></span> <br/> <span data-ttu-id="33c5f-166">-Materializer oluşturma: Önbelleğe alınmış ancak orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-166">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="33c5f-167">-Veritabanı sorgusu yürütme: Yüksek (bazı durumlarda sorguların daha iyi)</span><span class="sxs-lookup"><span data-stu-id="33c5f-167">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="33c5f-168">+ Connection.Open</span><span class="sxs-lookup"><span data-stu-id="33c5f-168">+ Connection.Open</span></span> <br/> <span data-ttu-id="33c5f-169">+ Command.ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="33c5f-169">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="33c5f-170">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="33c5f-170">+ DataReader.Read</span></span> <br/> <span data-ttu-id="33c5f-171">Nesne gerçekleştirme: Orta (EF5 hızlı)</span><span class="sxs-lookup"><span data-stu-id="33c5f-171">Object materialization: Medium (Faster than EF5)</span></span> <br/> <span data-ttu-id="33c5f-172">-İdentity arama: Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-172">- Identity lookup: Medium</span></span> |
| `}`                                                                                                  | <span data-ttu-id="33c5f-173">Connection.Close</span><span class="sxs-lookup"><span data-stu-id="33c5f-173">Connection.Close</span></span>          | <span data-ttu-id="33c5f-174">Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-174">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="33c5f-175">Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-175">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="33c5f-176">Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-176">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


<span data-ttu-id="33c5f-177">**İkinci sorgu yürütmenin – Orta Gecikmeli sorgu**</span><span class="sxs-lookup"><span data-stu-id="33c5f-177">**Second Query Execution – warm query**</span></span>

| <span data-ttu-id="33c5f-178">Kod kullanıcı yazma</span><span class="sxs-lookup"><span data-stu-id="33c5f-178">Code User Writes</span></span>                                                                                     | <span data-ttu-id="33c5f-179">Eylem</span><span class="sxs-lookup"><span data-stu-id="33c5f-179">Action</span></span>                    | <span data-ttu-id="33c5f-180">EF4 Performans etkisi</span><span class="sxs-lookup"><span data-stu-id="33c5f-180">EF4 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | <span data-ttu-id="33c5f-181">EF5 Performans etkisi</span><span class="sxs-lookup"><span data-stu-id="33c5f-181">EF5 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | <span data-ttu-id="33c5f-182">EF6 Performans etkisi</span><span class="sxs-lookup"><span data-stu-id="33c5f-182">EF6 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | <span data-ttu-id="33c5f-183">Bağlam oluşturma</span><span class="sxs-lookup"><span data-stu-id="33c5f-183">Context creation</span></span>          | <span data-ttu-id="33c5f-184">Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-184">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | <span data-ttu-id="33c5f-185">Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-185">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | <span data-ttu-id="33c5f-186">Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-186">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | <span data-ttu-id="33c5f-187">Sorgu ifadesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="33c5f-187">Query expression creation</span></span> | <span data-ttu-id="33c5f-188">Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-188">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | <span data-ttu-id="33c5f-189">Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-189">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | <span data-ttu-id="33c5f-190">Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-190">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | <span data-ttu-id="33c5f-191">LINQ Sorgu yürütme</span><span class="sxs-lookup"><span data-stu-id="33c5f-191">LINQ query execution</span></span>      | <span data-ttu-id="33c5f-192">-Metadata ~~Yükleniyor~~ arama: ~~Yüksek, ancak önbelleğe alınmış~~ düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-192">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="33c5f-193">-Görüntüleme ~~nesil~~ arama: ~~Potansiyel olarak çok yüksek ancak önbelleğe alınmış~~ düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-193">- View ~~generation~~ lookup: ~~Potentially very high but cached~~ Low</span></span> <br/> <span data-ttu-id="33c5f-194">-Değerlendirme parametresi: Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-194">- Parameter evaluation: Medium</span></span> <br/> <span data-ttu-id="33c5f-195">-Sorgu ~~çeviri~~ arama: Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-195">- Query ~~translation~~ lookup: Medium</span></span> <br/> <span data-ttu-id="33c5f-196">-Materializer ~~nesil~~ arama: ~~Orta ancak önbelleğe alınmış~~ düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-196">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="33c5f-197">-Veritabanı sorgusu yürütme: Büyük olasılıkla yüksek</span><span class="sxs-lookup"><span data-stu-id="33c5f-197">- Database query execution: Potentially high</span></span> <br/> <span data-ttu-id="33c5f-198">+ Connection.Open</span><span class="sxs-lookup"><span data-stu-id="33c5f-198">+ Connection.Open</span></span> <br/> <span data-ttu-id="33c5f-199">+ Command.ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="33c5f-199">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="33c5f-200">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="33c5f-200">+ DataReader.Read</span></span> <br/> <span data-ttu-id="33c5f-201">Nesne gerçekleştirme: Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-201">Object materialization: Medium</span></span> <br/> <span data-ttu-id="33c5f-202">-İdentity arama: Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-202">- Identity lookup: Medium</span></span> | <span data-ttu-id="33c5f-203">-Metadata ~~Yükleniyor~~ arama: ~~Yüksek, ancak önbelleğe alınmış~~ düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-203">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="33c5f-204">-Görüntüleme ~~nesil~~ arama: ~~Potansiyel olarak çok yüksek ancak önbelleğe alınmış~~ düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-204">- View ~~generation~~ lookup: ~~Potentially very high but cached~~ Low</span></span> <br/> <span data-ttu-id="33c5f-205">-Değerlendirme parametresi: Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-205">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="33c5f-206">-Sorgu ~~çeviri~~ arama: ~~Orta ancak önbelleğe alınmış~~ düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-206">- Query ~~translation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="33c5f-207">-Materializer ~~nesil~~ arama: ~~Orta ancak önbelleğe alınmış~~ düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-207">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="33c5f-208">-Veritabanı sorgusu yürütme: Yüksek (bazı durumlarda sorguların daha iyi)</span><span class="sxs-lookup"><span data-stu-id="33c5f-208">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="33c5f-209">+ Connection.Open</span><span class="sxs-lookup"><span data-stu-id="33c5f-209">+ Connection.Open</span></span> <br/> <span data-ttu-id="33c5f-210">+ Command.ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="33c5f-210">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="33c5f-211">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="33c5f-211">+ DataReader.Read</span></span> <br/> <span data-ttu-id="33c5f-212">Nesne gerçekleştirme: Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-212">Object materialization: Medium</span></span> <br/> <span data-ttu-id="33c5f-213">-İdentity arama: Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-213">- Identity lookup: Medium</span></span> | <span data-ttu-id="33c5f-214">-Metadata ~~Yükleniyor~~ arama: ~~Yüksek, ancak önbelleğe alınmış~~ düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-214">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="33c5f-215">-Görüntüleme ~~nesil~~ arama: ~~Orta ancak önbelleğe alınmış~~ düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-215">- View ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="33c5f-216">-Değerlendirme parametresi: Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-216">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="33c5f-217">-Sorgu ~~çeviri~~ arama: ~~Orta ancak önbelleğe alınmış~~ düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-217">- Query ~~translation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="33c5f-218">-Materializer ~~nesil~~ arama: ~~Orta ancak önbelleğe alınmış~~ düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-218">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="33c5f-219">-Veritabanı sorgusu yürütme: Yüksek (bazı durumlarda sorguların daha iyi)</span><span class="sxs-lookup"><span data-stu-id="33c5f-219">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="33c5f-220">+ Connection.Open</span><span class="sxs-lookup"><span data-stu-id="33c5f-220">+ Connection.Open</span></span> <br/> <span data-ttu-id="33c5f-221">+ Command.ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="33c5f-221">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="33c5f-222">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="33c5f-222">+ DataReader.Read</span></span> <br/> <span data-ttu-id="33c5f-223">Nesne gerçekleştirme: Orta (EF5 hızlı)</span><span class="sxs-lookup"><span data-stu-id="33c5f-223">Object materialization: Medium (Faster than EF5)</span></span> <br/> <span data-ttu-id="33c5f-224">-İdentity arama: Orta</span><span class="sxs-lookup"><span data-stu-id="33c5f-224">- Identity lookup: Medium</span></span> |
| `}`                                                                                                  | <span data-ttu-id="33c5f-225">Connection.Close</span><span class="sxs-lookup"><span data-stu-id="33c5f-225">Connection.Close</span></span>          | <span data-ttu-id="33c5f-226">Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-226">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | <span data-ttu-id="33c5f-227">Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-227">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | <span data-ttu-id="33c5f-228">Düşük</span><span class="sxs-lookup"><span data-stu-id="33c5f-228">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


<span data-ttu-id="33c5f-229">Soğuk ve orta Gecikmeli sorgular performans maliyetini azaltmak için birkaç yolu vardır ve biz şu aşağıdaki bölümdeki göz atacağız.</span><span class="sxs-lookup"><span data-stu-id="33c5f-229">There are several ways to reduce the performance cost of both cold and warm queries, and we'll take a look at these in the following section.</span></span> <span data-ttu-id="33c5f-230">Sorunlar performans görünümü oluşturma sırasında deneyimli çözmenize yardımcı önceden üretilmiş görünümleri kullanarak soğuk sorgularda yükleme modeli maliyetini azaltmayı özellikle inceleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-230">Specifically, we'll look at reducing the cost of model loading in cold queries by using pre-generated views, which should help alleviate performance pains experienced during view generation.</span></span> <span data-ttu-id="33c5f-231">Orta Gecikmeli sorgular için biz sorgu planını önbelleğe alma, izleme sorgu yok ve farklı bir sorgu yürütme seçenekleri ele alacağız.</span><span class="sxs-lookup"><span data-stu-id="33c5f-231">For warm queries, we'll cover query plan caching, no tracking queries, and different query execution options.</span></span>

### <a name="21-what-is-view-generation"></a><span data-ttu-id="33c5f-232">2.1 görünüm oluşturma nedir?</span><span class="sxs-lookup"><span data-stu-id="33c5f-232">2.1 What is View Generation?</span></span>

<span data-ttu-id="33c5f-233">Nesil olduğundan hangi görünümü anlamak için önce "Eşlemesi Görünüm" nedir anlıyoruz gerekir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-233">In order to understand what view generation is, we must first understand what “Mapping Views” are.</span></span> <span data-ttu-id="33c5f-234">Eşleme, her varlık kümesi ve ilişkilendirme belirtilen dönüşümleri yürütülebilir temsillerini görünümleridir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-234">Mapping Views are executable representations of the transformations specified in the mapping for each entity set and association.</span></span> <span data-ttu-id="33c5f-235">Dahili olarak, bu eşleme görünümler CQTs (kurallı sorgu ağaçları) şeklini alır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-235">Internally, these mapping views take the shape of CQTs (canonical query trees).</span></span> <span data-ttu-id="33c5f-236">Eşleme görünümleri iki tür vardır:</span><span class="sxs-lookup"><span data-stu-id="33c5f-236">There are two types of mapping views:</span></span>

-   <span data-ttu-id="33c5f-237">Sorgu görünümleri: Bunlar veritabanı şema kavramsal modeline gitmek gerekli dönüşümü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="33c5f-237">Query views: these represent the transformation necessary to go from the database schema to the conceptual model.</span></span>
-   <span data-ttu-id="33c5f-238">Görünümleri güncelleştirme: Bunlar veritabanı şemasına kavramsal modelden gitmek gerekli dönüşümü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="33c5f-238">Update views: these represent the transformation necessary to go from the conceptual model to the database schema.</span></span>

<span data-ttu-id="33c5f-239">Kavramsal model veritabanı şemasını çeşitli şekillerde farklılık gösterebilir göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="33c5f-239">Keep in mind that the conceptual model might differ from the database schema in various ways.</span></span> <span data-ttu-id="33c5f-240">Örneğin, tek bir tablo, iki farklı varlık türü için verileri depolamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-240">For example, one single table might be used to store the data for two different entity types.</span></span> <span data-ttu-id="33c5f-241">Devralma ve önemsiz olmayan eşlemeleri eşleme görünümleri karmaşıklığı içinde bir rol oynar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-241">Inheritance and non-trivial mappings play a role in the complexity of the mapping views.</span></span>

<span data-ttu-id="33c5f-242">Eşleme belirtimine göre bu görünümler bilgi işlem görünüm oluşturma dediğimiz işlemidir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-242">The process of computing these views based on the specification of the mapping is what we call view generation.</span></span> <span data-ttu-id="33c5f-243">Görünüm oluşturma ya da dinamik olarak bir modeli yüklendiğinde veya derleme zamanında "önceden üretilmiş görünümleri"; kullanarak gerçekleştirilebilir İkinci bir C varlık SQL deyimlerini biçiminde serileştirilme şeklini\# veya VB dosyası.</span><span class="sxs-lookup"><span data-stu-id="33c5f-243">View generation can either take place dynamically when a model is loaded, or at build time, by using "pre-generated views"; the latter are serialized in the form of Entity SQL statements to a C\# or VB file.</span></span>

<span data-ttu-id="33c5f-244">Görünüm oluşturulduğunda, aynı zamanda doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-244">When views are generated, they are also validated.</span></span> <span data-ttu-id="33c5f-245">Performans açısından, varlıklar arasındaki bağlantıları mantıklı ve desteklenen tüm işlemler için doğru kardinalite sahip sağlayan gerçekten doğrulanması görünümlerinin görünüm oluşturma maliyetini büyük çoğunluğu oluşur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-245">From a performance standpoint, the vast majority of the cost of view generation is actually the validation of the views which ensures that the connections between the entities make sense and have the correct cardinality for all the supported operations.</span></span>

<span data-ttu-id="33c5f-246">Bir varlık kümesi üzerindeki sorgu yürütüldüğünde, sorgu karşılık gelen sorgu görünümü ile birleştirilir ve bu bileşim sonucunu yedekleme deposu anlamak için sorgu gösterimini oluşturmak için plan derleyicide çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-246">When a query over an entity set is executed, the query is combined with the corresponding query view, and the result of this composition is run through the plan compiler to create the representation of the query that the backing store can understand.</span></span> <span data-ttu-id="33c5f-247">SQL Server için bu derleme sonucunu bir T-SQL SELECT deyimi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-247">For SQL Server, the final result of this compilation will be a T-SQL SELECT statement.</span></span> <span data-ttu-id="33c5f-248">Bir güncelleştirme bir varlık kümesi üzerinde gerçekleştirilir, ilk kez güncelleştirme görünüm hedef veritabanı için DML deyimleri dönüştürmek için benzer bir süreç aracılığıyla çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-248">The first time an update over an entity set is performed, the update view is run through a similar process to transform it into DML statements for the target database.</span></span>

### <a name="22-factors-that-affect-view-generation-performance"></a><span data-ttu-id="33c5f-249">2.2 görünüm oluşturma performansı etkileyen faktörler</span><span class="sxs-lookup"><span data-stu-id="33c5f-249">2.2 Factors that affect View Generation performance</span></span>

<span data-ttu-id="33c5f-250">Performans görünümü oluşturma adım yalnızca modelinizi boyutu aynı zamanda modelin nasıl birbirine bağlı olduğu bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-250">The performance of view generation step not only depends on the size of your model but also on how interconnected the model is.</span></span> <span data-ttu-id="33c5f-251">İki varlık devralma zincirini veya ilişkilendirme aracılığıyla birbirine bağlandıysa bağlandığı söylenir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-251">If two Entities are connected via an inheritance chain or an Association, they are said to be connected.</span></span> <span data-ttu-id="33c5f-252">Benzer şekilde iki tablo yabancı anahtar birbirine bağlandıysa, bağlı.</span><span class="sxs-lookup"><span data-stu-id="33c5f-252">Similarly if two tables are connected via a foreign key, they are connected.</span></span> <span data-ttu-id="33c5f-253">Bağlı varlıkları ve tablolar, şemalar sayısı arttıkça, görünüm oluşturma arttıkça maliyet.</span><span class="sxs-lookup"><span data-stu-id="33c5f-253">As the number of connected Entities and tables in your schemas increase, the view generation cost increases.</span></span>

<span data-ttu-id="33c5f-254">Oluşturmak ve görünümleri doğrulamak için kullanırız algoritması en kötü durumda, üstel, ancak bazı iyileştirmeler bu geliştirmek için kullanırız.</span><span class="sxs-lookup"><span data-stu-id="33c5f-254">The algorithm that we use to generate and validate views is exponential in the worst case, though we do use some optimizations to improve this.</span></span> <span data-ttu-id="33c5f-255">Performansı olumsuz şekilde etkileyebilir görünen büyük faktörler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="33c5f-255">The biggest factors that seem to negatively affect performance are:</span></span>

-   <span data-ttu-id="33c5f-256">Varlıkların sayısı ve bu varlıklar arasında ilişkilendirmeler miktarı söz konusu model boyutu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-256">Model size, referring to the number of entities and the amount of associations between these entities.</span></span>
-   <span data-ttu-id="33c5f-257">Model karmaşıklığı, özellikle çok sayıda türleri içeren devralma.</span><span class="sxs-lookup"><span data-stu-id="33c5f-257">Model complexity, specifically inheritance involving a large number of types.</span></span>
-   <span data-ttu-id="33c5f-258">Bağımsız ilişkilendirmeleri, yabancı anahtar ilişkilerini yerine kullanma.</span><span class="sxs-lookup"><span data-stu-id="33c5f-258">Using Independent Associations, instead of Foreign Key Associations.</span></span>

<span data-ttu-id="33c5f-259">Küçük, basit modelleri için maliyet önceden üretilmiş görünümleri kullanarak rahatsız değil için küçük olabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-259">For small, simple models the cost may be small enough to not bother using pre-generated views.</span></span> <span data-ttu-id="33c5f-260">Model boyutu ve karmaşıklığı arttıkça görünümü oluşturma ve doğrulama maliyetini azaltmak için kullanılabilecek birkaç seçenek vardır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-260">As model size and complexity increase, there are several options available to reduce the cost of view generation and validation.</span></span>

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a><span data-ttu-id="33c5f-261">2.3 kullanarak Pre-Generated modeli azaltmak için görünümleri yükleme süresi</span><span class="sxs-lookup"><span data-stu-id="33c5f-261">2.3 Using Pre-Generated Views to decrease model load time</span></span>

<span data-ttu-id="33c5f-262">Entity Framework 6 üzerinde önceden üretilmiş görünümleri kullanma hakkında ayrıntılı bilgi için ziyaret [Pre-Generated eşleme görünümleri](~/ef6/fundamentals/performance/pre-generated-views.md)</span><span class="sxs-lookup"><span data-stu-id="33c5f-262">For detailed information on how to use pre-generated views on Entity Framework 6 visit [Pre-Generated Mapping Views](~/ef6/fundamentals/performance/pre-generated-views.md)</span></span>

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools-community-edition"></a><span data-ttu-id="33c5f-263">2.3.1 Entity Framework güç araçları Community Edition'ı kullanarak önceden üretilmiş görünümleri</span><span class="sxs-lookup"><span data-stu-id="33c5f-263">2.3.1 Pre-Generated views using the Entity Framework Power Tools Community Edition</span></span>

<span data-ttu-id="33c5f-264">Kullanabileceğiniz [Entity Framework 6 güç araçları Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) EDMX ve Code First modelleri, görünümleri model sınıfı dosyasını sağ tıklatın ve "Görünümleri Oluştur" seçmek için Entity Framework menüsü kullanılarak oluşturulacak.</span><span class="sxs-lookup"><span data-stu-id="33c5f-264">You can use the [Entity Framework 6 Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) to generate views of EDMX and Code First models by right-clicking the model class file and using the Entity Framework menu to select “Generate Views”.</span></span> <span data-ttu-id="33c5f-265">Entity Framework güç araçları Community Edition yalnızca DbContext türetilmiş bağlamları üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-265">The Entity Framework Power Tools Community Edition work only on DbContext-derived contexts.</span></span>

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a><span data-ttu-id="33c5f-266">2.3.2 nasıl EDMGen tarafından oluşturulan bir model ile önceden üretilmiş görünümleri kullanma</span><span class="sxs-lookup"><span data-stu-id="33c5f-266">2.3.2 How to use Pre-generated views with a model created by EDMGen</span></span>

<span data-ttu-id="33c5f-267">EDMGen Entity Framework 4 ve 5, ancak değil, Entity Framework 6 ile çalışır ve .NET ile birlikte gelen bir yardımcı programdır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-267">EDMGen is a utility that ships with .NET and works with Entity Framework 4 and 5, but not with Entity Framework 6.</span></span> <span data-ttu-id="33c5f-268">EDMGen komut satırından bir model dosyası, nesne katmanı ve görünümleri oluşturmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-268">EDMGen allows you to generate a model file, the object layer and the views from the command line.</span></span> <span data-ttu-id="33c5f-269">Çıktıları birini VB veya C istediğiniz dilde görünümleri dosyasında olacaktır\#.</span><span class="sxs-lookup"><span data-stu-id="33c5f-269">One of the outputs will be a Views file in your language of choice, VB or C\#.</span></span> <span data-ttu-id="33c5f-270">Her varlık kümesinin varlık SQL kod parçacıkları içeren bir kod dosyası budur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-270">This is a code file containing Entity SQL snippets for each entity set.</span></span> <span data-ttu-id="33c5f-271">Önceden üretilmiş görünümleri etkinleştirmek için yalnızca dosyasını projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="33c5f-271">To enable pre-generated views, you simply include the file in your project.</span></span>

<span data-ttu-id="33c5f-272">Model için şema dosyalarını el ile yaptığınız düzenlemeler, görünümleri dosyanın yeniden oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-272">If you manually make edits to the schema files for the model, you will need to re-generate the views file.</span></span> <span data-ttu-id="33c5f-273">EDMGen ile çalıştırarak bunu yapabilirsiniz **/mode:ViewGeneration** bayrağı.</span><span class="sxs-lookup"><span data-stu-id="33c5f-273">You can do this by running EDMGen with the **/mode:ViewGeneration** flag.</span></span>

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a><span data-ttu-id="33c5f-274">2.3.3 nasıl Pre-Generated görünümlerini içeren bir EDMX dosyası kullanma</span><span class="sxs-lookup"><span data-stu-id="33c5f-274">2.3.3 How to use Pre-Generated Views with an EDMX file</span></span>

<span data-ttu-id="33c5f-275">EDMGen görünümleri bir EDMX dosyası oluşturmak için de kullanabilirsiniz - daha önce başvurulan MSDN konusu - Bunu yapmak için bir derleme öncesi olay eklemeyi açıklar ancak bu karmaşık bir işlemdir ve burada mümkün olmayan bazı durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-275">You can also use EDMGen to generate views for an EDMX file - the previously referenced MSDN topic describes how to add a pre-build event to do this - but this is complicated and there are some cases where it isn't possible.</span></span> <span data-ttu-id="33c5f-276">Genellikle, model bir edmx dosyası olduğunda görünümleri oluşturmak için T4 şablonu kullanmak daha kolaydır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-276">It's generally easier to use a T4 template to generate the views when your model is in an edmx file.</span></span>

<span data-ttu-id="33c5f-277">Görünümü oluşturmak için T4 şablonu kullanmayı açıklayan bir gönderi ADO.NET ekibi blogu vardır ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>).</span><span class="sxs-lookup"><span data-stu-id="33c5f-277">The ADO.NET team blog has a post that describes how to use a T4 template for view generation ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>).</span></span> <span data-ttu-id="33c5f-278">Bu gönderinin indirilir ve projenize eklediğiniz bir şablonu içerir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-278">This post includes a template that can be downloaded and added to your project.</span></span> <span data-ttu-id="33c5f-279">Entity Framework'ün en son sürümleri ile çalışmak için garantili olmayan şekilde şablonu, Entity Framework'ü ilk sürümü için yazılmıştır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-279">The template was written for the first version of Entity Framework, so they aren’t guaranteed to work with the latest versions of Entity Framework.</span></span> <span data-ttu-id="33c5f-280">Ancak, daha güncel bir görünüm oluşturma şablon yelpazesine Entity Framework 4 ve 5from için Visual Studio Galerisi indirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="33c5f-280">However, you can download a more up-to-date set of view generation templates for Entity Framework 4 and 5from the Visual Studio Gallery:</span></span>

-   <span data-ttu-id="33c5f-281">VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d></span><span class="sxs-lookup"><span data-stu-id="33c5f-281">VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d></span></span>
-   <span data-ttu-id="33c5f-282">C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d></span><span class="sxs-lookup"><span data-stu-id="33c5f-282">C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d></span></span>

<span data-ttu-id="33c5f-283">Entity Framework 6 kullanıyorsanız, görünüm oluşturma T4 şablonlarını konumundaki Visual Studio Galerisi'nden alabileceğiniz \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.</span><span class="sxs-lookup"><span data-stu-id="33c5f-283">If you’re using Entity Framework 6 you can get the view generation T4 templates from the Visual Studio Gallery at \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.</span></span>

### <a name="24-reducing-the-cost-of-view-generation"></a><span data-ttu-id="33c5f-284">2.4 azaltarak maliyet görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="33c5f-284">2.4 Reducing the cost of view generation</span></span>

<span data-ttu-id="33c5f-285">Yükleme modelinden görünüm oluşturma (çalışma zamanı) maliyeti, önceden üretilmiş görünümleri kullanarak tasarım zamanına taşır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-285">Using pre-generated views moves the cost of view generation from model loading (run time) to design time.</span></span> <span data-ttu-id="33c5f-286">Bu çalışma zamanında başlatma performansını artırır, ancak, geliştirirken hala görünüm oluşturma sorunları yaşar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-286">While this improves startup performance at runtime, you will still experience the pain of view generation while you are developing.</span></span> <span data-ttu-id="33c5f-287">Görünüm oluşturma, hem derleme ve çalıştırma maliyetini azaltmaya yardımcı olabilecek birkaç ek püf noktaları vardır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-287">There are several additional tricks that can help reduce the cost of view generation, both at compile time and run time.</span></span>

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a><span data-ttu-id="33c5f-288">2.4.1 görünüm oluşturma maliyetini azaltmak için yabancı anahtar ilişkilerini kullanma</span><span class="sxs-lookup"><span data-stu-id="33c5f-288">2.4.1 Using Foreign Key Associations to reduce view generation cost</span></span>

<span data-ttu-id="33c5f-289">Görünüm oluşturma işleminde harcanan süreyi önemli ölçüde ilişkilendirmeleri bağımsız ilişkileri modelden yabancı anahtar ilişkileri de geçiş burada geliştirilmiş çalışmalarını bir dizi gördük.</span><span class="sxs-lookup"><span data-stu-id="33c5f-289">We have seen a number of cases where switching the associations in the model from Independent Associations to Foreign Key Associations dramatically improved the time spent in view generation.</span></span>

<span data-ttu-id="33c5f-290">Bu geliştirme göstermek için biz Navision modeli iki sürümünü EDMGen kullanarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-290">To demonstrate this improvement, we generated two versions of the Navision model by using EDMGen.</span></span> <span data-ttu-id="33c5f-291">*Not: Ek C Navision modelin bir açıklaması için bkz.*</span><span class="sxs-lookup"><span data-stu-id="33c5f-291">*Note: see appendix C for a description of the Navision model.*</span></span> <span data-ttu-id="33c5f-292">Varlıklar ve aralarındaki ilişkiler, çok büyük miktarda nedeniyle bu alıştırma için ilginç Navision modelidir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-292">The Navision model is interesting for this exercise due to its very large amount of entities and relationships between them.</span></span>

<span data-ttu-id="33c5f-293">Bu çok büyük bir modelin bir sürümü yabancı anahtar ilişkilerini oluşturuldu ve diğer bağımsız ilişkilerini oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-293">One version of this very large model was generated with Foreign Keys Associations and the other was generated with Independent Associations.</span></span> <span data-ttu-id="33c5f-294">Biz sonra görünümlerin her model için ne kadar sürdüğünü zaman aşımına uğradı.</span><span class="sxs-lookup"><span data-stu-id="33c5f-294">We then timed how long it took to generate the views for each model.</span></span> <span data-ttu-id="33c5f-295">Entity Framework 5 test sınıfı EntityViewGenerator GenerateViews() yöntemden Entity Framework 6 test sınıfı StorageMappingItemCollection GenerateViews() yöntemden kullanılabilir ancak görünümleri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-295">Entity Framework 5 test used the GenerateViews() method from class EntityViewGenerator to generate the views, while the Entity Framework 6 test used the GenerateViews() method from class StorageMappingItemCollection.</span></span> <span data-ttu-id="33c5f-296">Bu kod Entity Framework 6 kod temelinde oluşan alanlarını yeniden yapılandırma nedeniyle.</span><span class="sxs-lookup"><span data-stu-id="33c5f-296">This due to code restructuring that occurred in the Entity Framework 6 codebase.</span></span>

<span data-ttu-id="33c5f-297">Entity Framework 5 kullanılarak, görünüm oluşturma yabancı anahtarlara sahip olan model için bir laboratuvar makinede 65 dakika sürdü.</span><span class="sxs-lookup"><span data-stu-id="33c5f-297">Using Entity Framework 5, view generation for the model with Foreign Keys took 65 minutes in a lab machine.</span></span> <span data-ttu-id="33c5f-298">Cihazın bilinmeyen ne kadar bağımsız ilişkilendirmeleri kullandığınız modeline görünümleri oluşturmak için alacağı.</span><span class="sxs-lookup"><span data-stu-id="33c5f-298">It's unknown how long it would have taken to generate the views for the model that used independent associations.</span></span> <span data-ttu-id="33c5f-299">Size sunduğumuz laboratuvarda aylık güncelleştirmeleri yüklemek için makine yeniden başlatılmış önce bir ay üzerinden çalışan test kalmadı.</span><span class="sxs-lookup"><span data-stu-id="33c5f-299">We left the test running for over a month before the machine was rebooted in our lab to install monthly updates.</span></span>

<span data-ttu-id="33c5f-300">Entity Framework 6 kullanarak, yabancı anahtarlara sahip olan model için Görünüm oluşturma aynı Laboratuvar makinede 28 saniye sürdü.</span><span class="sxs-lookup"><span data-stu-id="33c5f-300">Using Entity Framework 6, view generation for the model with Foreign Keys took 28 seconds in the same lab machine.</span></span> <span data-ttu-id="33c5f-301">Bağımsız ilişkilerini kullanan model için Görünüm oluşturma 58 saniye sürdü.</span><span class="sxs-lookup"><span data-stu-id="33c5f-301">View generation for the model that uses Independent Associations took 58 seconds.</span></span> <span data-ttu-id="33c5f-302">Entity Framework 6 için kendi görünüm oluşturma kodunu yapılan iyileştirmeler, çok sayıda proje daha hızlı başlangıç süreleri elde etmek için önceden üretilmiş görünümleri gerekmez anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-302">The improvements done to Entity Framework 6 on its view generation code mean that many projects won’t need pre-generated views to obtain faster startup times.</span></span>

<span data-ttu-id="33c5f-303">Entity Framework 4 ve 5'teki görünümleri önceden oluşturma EDMGen veya Entity Framework güç araçları ile yapılabilir açıklama önemlidir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-303">It’s important to remark that pre-generating views in Entity Framework 4 and 5 can be done with EDMGen or the Entity Framework Power Tools.</span></span> <span data-ttu-id="33c5f-304">Entity Framework güç araçları aracılığıyla ya da açıklandığı gibi program aracılığıyla oluşturma için Entity Framework 6 görünümü yapılabilir [Pre-Generated eşleme görünümleri](~/ef6/fundamentals/performance/pre-generated-views.md).</span><span class="sxs-lookup"><span data-stu-id="33c5f-304">For Entity Framework 6 view generation can be done via the Entity Framework Power Tools or programmatically as described in [Pre-Generated Mapping Views](~/ef6/fundamentals/performance/pre-generated-views.md).</span></span>

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a><span data-ttu-id="33c5f-305">2.4.1.1 nasıl yerine bağımsız ilişkilendirmeleri yabancı anahtarlar kullanın</span><span class="sxs-lookup"><span data-stu-id="33c5f-305">2.4.1.1 How to use Foreign Keys instead of Independent Associations</span></span>

<span data-ttu-id="33c5f-306">Visual Studio'da EDMGen veya varlık Tasarımcısı kullanarak, varsayılan olarak FKs alın ve yalnızca IAS FKs arasında geçiş yapmak için tek bir onay kutusunun veya komut satırı bayrağı alır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-306">When using EDMGen or the Entity Designer in Visual Studio, you get FKs by default, and it only takes a single checkbox or command line flag to switch between FKs and IAs.</span></span>

<span data-ttu-id="33c5f-307">Büyük bir Code First modeli varsa, bağımsız ilişkilerini kullanarak görünümü oluşturma hakkında aynı etkiye sahip.</span><span class="sxs-lookup"><span data-stu-id="33c5f-307">If you have a large Code First model, using Independent Associations will have the same effect on view generation.</span></span> <span data-ttu-id="33c5f-308">Bazı geliştiriciler kendi nesne modeli kirletmesini için bu düşünür ancak sınıflarda, bağımlı nesneler için yabancı anahtar özellikleri ekleyerek bu etkiyi önleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-308">You can avoid this impact by including Foreign Key properties on the classes for your dependent objects, though some developers will consider this to be polluting their object model.</span></span> <span data-ttu-id="33c5f-309">Bu konu hakkında daha fazla bilgi bulabilirsiniz \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.</span><span class="sxs-lookup"><span data-stu-id="33c5f-309">You can find more information on this subject in \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.</span></span>

| <span data-ttu-id="33c5f-310">Kullanırken</span><span class="sxs-lookup"><span data-stu-id="33c5f-310">When using</span></span>      | <span data-ttu-id="33c5f-311">Bunu yapın</span><span class="sxs-lookup"><span data-stu-id="33c5f-311">Do this</span></span>                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="33c5f-312">Varlık Tasarımcısı</span><span class="sxs-lookup"><span data-stu-id="33c5f-312">Entity Designer</span></span> | <span data-ttu-id="33c5f-313">İki varlık arasında bir ilişki eklendikten sonra bir başvuru kısıtlaması olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="33c5f-313">After adding an association between two entities, make sure you have a referential constraint.</span></span> <span data-ttu-id="33c5f-314">Başvuru kısıtlamalarını yerine bağımsız ilişkilendirmeleri yabancı anahtarları kullanmak için Entity Framework söyleyin.</span><span class="sxs-lookup"><span data-stu-id="33c5f-314">Referential constraints tell Entity Framework to use Foreign Keys instead of Independent Associations.</span></span> <span data-ttu-id="33c5f-315">Ek ayrıntılar için ziyaret edin \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>.</span><span class="sxs-lookup"><span data-stu-id="33c5f-315">For additional details visit \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>.</span></span> |
| <span data-ttu-id="33c5f-316">EDMGen</span><span class="sxs-lookup"><span data-stu-id="33c5f-316">EDMGen</span></span>          | <span data-ttu-id="33c5f-317">EDMGen veritabanından dosyaları oluşturmak için kullanılırken, yabancı anahtarlar dikkate ve bu nedenle modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-317">When using EDMGen to generate your files from the database, your Foreign Keys will be respected and added to the model as such.</span></span> <span data-ttu-id="33c5f-318">EDMGen tarafından kullanıma sunulan farklı seçenekler hakkında daha fazla bilgi için ziyaret [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx).</span><span class="sxs-lookup"><span data-stu-id="33c5f-318">For more information on the different options exposed by EDMGen visit [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx).</span></span>                           |
| <span data-ttu-id="33c5f-319">İlk kod</span><span class="sxs-lookup"><span data-stu-id="33c5f-319">Code First</span></span>      | <span data-ttu-id="33c5f-320">"İlişkisi kuralı" bölümüne bakın [kod öncelikli kurallar](~/ef6/modeling/code-first/conventions/built-in.md) Code First kullanarak bağımlı nesneler üzerinde yabancı anahtar özelliklerini eklemek hakkında daha fazla bilgi için konu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-320">See the "Relationship Convention" section of the [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) topic for information on how to include foreign key properties on dependent objects when using Code First.</span></span>                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a><span data-ttu-id="33c5f-321">2.4.2 modeliniz için ayrı bir derleme taşıma</span><span class="sxs-lookup"><span data-stu-id="33c5f-321">2.4.2 Moving your model to a separate assembly</span></span>

<span data-ttu-id="33c5f-322">Proje yeniden olduğunda bile modeli değişti değildi modelinizi doğrudan uygulamanızın projeye eklenir ve derleme öncesi olay veya T4 şablonu üzerinden görünümlerini oluşturmak, görünüm oluşturma ve doğrulama gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-322">When your model is included directly in your application's project and you generate views through a pre-build event or a T4 template, view generation and validation will take place whenever the project is rebuilt, even if the model wasn't changed.</span></span> <span data-ttu-id="33c5f-323">Model için ayrı bir derleme taşıyın ve uygulamanızın proje başvurusu, diğer uygulamanıza modeli içeren projeyi yeniden dağıtmaya gerek kalmadan değişiklik yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-323">If you move the model to a separate assembly and reference it from your application's project, you can make other changes to your application without needing to rebuild the project containing the model.</span></span>

<span data-ttu-id="33c5f-324">*Not:*  derlemeleri ayırmak için modelinizi taşırken istemci projesinin uygulama yapılandırma dosyasına modeli için bağlantı dizelerini kopyalamayı unutmayın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-324">*Note:*  when moving your model to separate assemblies remember to copy the connection strings for the model into the application configuration file of the client project.</span></span>

#### <a name="243-disable-validation-of-an-edmx-based-model"></a><span data-ttu-id="33c5f-325">2.4.3 edmx tabanlı bir modeli doğrulaması devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="33c5f-325">2.4.3 Disable validation of an edmx-based model</span></span>

<span data-ttu-id="33c5f-326">Model değiştirilmemiş olsa bile, derleme zamanında EDMX modelleri doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-326">EDMX models are validated at compile time, even if the model is unchanged.</span></span> <span data-ttu-id="33c5f-327">Modelinizi zaten doğrulandı, "Doğrulama üzerinde derleme" özelliği false olarak ayarlayarak Özellikler penceresinde derleme zamanında doğrulama gösterilmemesini sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-327">If your model has already been validated, you can suppress validation at compile time by setting the "Validate on Build" property to false in the properties window.</span></span> <span data-ttu-id="33c5f-328">Eşleme veya model değiştirdiğinizde, geçici olarak yaptığınız değişiklikleri doğrulamak için doğrulama yeniden etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-328">When you change your mapping or model, you can temporarily re-enable validation to verify your changes.</span></span>

<span data-ttu-id="33c5f-329">Performans geliştirmeleri için Entity Framework Designer için Entity Framework 6 yapılan ve "doğrula"derlemede maliyeti çok daha kısa designer'ın önceki sürümlerinde olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-329">Note that performance improvements were made to the Entity Framework Designer for Entity Framework 6, and the cost of the “Validate on Build” is much lower than in previous versions of the designer.</span></span>

## <a name="3-caching-in-the-entity-framework"></a><span data-ttu-id="33c5f-330">3 önbelleğe alma varlık Çerçevesi'nde</span><span class="sxs-lookup"><span data-stu-id="33c5f-330">3 Caching in the Entity Framework</span></span>

<span data-ttu-id="33c5f-331">Entity Framework, aşağıdaki biçimlerden önbelleğe alma yerleşik sahiptir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-331">Entity Framework has the following forms of caching built-in:</span></span>

1.  <span data-ttu-id="33c5f-332">Önbelleğe alma nesnesi – ObjectContext örneği oluşturulan ObjectStateManager izleme, bu örneği kullanılarak alınan nesneleri bellekte tutar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-332">Object caching – the ObjectStateManager built into an ObjectContext instance keeps track in memory of the objects that have been retrieved using that instance.</span></span> <span data-ttu-id="33c5f-333">Birinci düzey önbellek olarak da bilinen budur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-333">This is also known as first-level cache.</span></span>
2.  <span data-ttu-id="33c5f-334">Sorgu planı Caching - sorguda birden çok kez çalıştırıldığında oluşturulan depo komutu yeniden kullanılıyor.</span><span class="sxs-lookup"><span data-stu-id="33c5f-334">Query Plan Caching - reusing the generated store command when a query is executed more than once.</span></span>
3.  <span data-ttu-id="33c5f-335">Önbelleğe alma - bir model için meta veriler aynı modelin farklı bağlantıları arasında paylaşım meta verileri.</span><span class="sxs-lookup"><span data-stu-id="33c5f-335">Metadata caching - sharing the metadata for a model across different connections to the same model.</span></span>

<span data-ttu-id="33c5f-336">ADO.NET veri sağlayıcısının sarmalama sağlayıcısı, veritabanından alınan sonuçları için bir önbelleği ile Entity Framework genişletmek için de kullanılabilir olarak bilinen özel bir tür ilk günden EF sağlayan önbellekler yanı sıra da ikinci düzey önbelleğe alma olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-336">Besides the caches that EF provides out of the box, a special kind of ADO.NET data provider known as a wrapping provider can also be used to extend Entity Framework with a cache for the results retrieved from the database, also known as second-level caching.</span></span>

### <a name="31-object-caching"></a><span data-ttu-id="33c5f-337">3.1 önbelleğe alma nesnesi</span><span class="sxs-lookup"><span data-stu-id="33c5f-337">3.1 Object Caching</span></span>

<span data-ttu-id="33c5f-338">Bir varlığın bir sorgunun sonuçları döndürdüğünde varsayılan olarak aynı anahtara sahip bir varlık, ObjectStateManager zaten yüklü değilse, yalnızca EF gerçekleştiren önce ObjectContext denetler.</span><span class="sxs-lookup"><span data-stu-id="33c5f-338">By default when an entity is returned in the results of a query, just before EF materializes it, the ObjectContext will check if an entity with the same key has already been loaded into its ObjectStateManager.</span></span> <span data-ttu-id="33c5f-339">EF aynı anahtarlara sahip bir varlık zaten varsa, sorgu sonuçlarında içerecektir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-339">If an entity with the same keys is already present EF will include it in the results of the query.</span></span> <span data-ttu-id="33c5f-340">EF veritabanında sorgu hala verecek olsa da, bu davranışı çok varlığın birden çok kez gerçekleştirilmesini maliyeti atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-340">Although EF will still issue the query against the database, this behavior can bypass much of the cost of materializing the entity multiple times.</span></span>

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a><span data-ttu-id="33c5f-341">3.1.1 varlıkları DbContext Bul kullanarak nesneyi önbellekten alma</span><span class="sxs-lookup"><span data-stu-id="33c5f-341">3.1.1 Getting entities from the object cache using DbContext Find</span></span>

<span data-ttu-id="33c5f-342">Normal bir sorgu olan DB (EF 4.1 ilk kez dahil edilen API'ler) bulma yöntemi bir arama bellekte veritabanında sorgu dahi vermeden önce gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-342">Unlike a regular query, the Find method in DbSet (APIs included for the first time in EF 4.1) will perform a search in memory before even issuing the query against the database.</span></span> <span data-ttu-id="33c5f-343">İki farklı ObjectContext örneği ayrı bir nesne önbellekler sahip oldukları anlamına gelir, iki farklı ObjectStateManager örnekleri olduğunu unutmamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-343">It’s important to note that two different ObjectContext instances will have two different ObjectStateManager instances, meaning that they have separate object caches.</span></span>

<span data-ttu-id="33c5f-344">Bul, bağlam tarafından izlenen bir varlığın bulmaya için birincil anahtar değeri kullanır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-344">Find uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="33c5f-345">Varlık bağlamı değilse, bir sorgu yürütülen sonra veritabanına karşı değerlendirilir ve varlık bağlamı veya veritabanında bulunmazsa null döndürülür.</span><span class="sxs-lookup"><span data-stu-id="33c5f-345">If the entity is not in the context then a query will be executed and evaluated against the database, and null is returned if the entity is not found in the context or in the database.</span></span> <span data-ttu-id="33c5f-346">Bul bağlamına eklenmiş ancak veritabanı henüz kaydedilmedi varlıklar döndürdüğüne dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="33c5f-346">Note that Find also returns entities that have been added to the context but have not yet been saved to the database.</span></span>

<span data-ttu-id="33c5f-347">Bul kullanırken gerçekleştirilecek bir performans artışı yoktur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-347">There is a performance consideration to be taken when using Find.</span></span> <span data-ttu-id="33c5f-348">Varsayılan olarak bu yöntem çağrılarına tamamlama veritabanı hala bekleyen değişikliklerini algılamak için nesne önbelleği doğrulanması tetikler.</span><span class="sxs-lookup"><span data-stu-id="33c5f-348">Invocations to this method by default will trigger a validation of the object cache in order to detect changes that are still pending commit to the database.</span></span> <span data-ttu-id="33c5f-349">Bu işlem, nesne önbelleği veya nesne önbelleğe eklenen bir büyük nesne grafiği nesnelerin çok büyük bir sayı ise çok pahalı olabilir, ancak bunu ayrıca devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-349">This process can be very expensive if there are a very large number of objects in the object cache or in a large object graph being added to the object cache, but it can also be disabled.</span></span> <span data-ttu-id="33c5f-350">Bazı durumlarda, büyüklük kertesinde otomatik devre dışı bıraktığınızda yöntemi algılamak Bul çağırma farkının üzerinden değişiklikleri algılayabileceğini dikkatle.</span><span class="sxs-lookup"><span data-stu-id="33c5f-350">In certain cases, you may perceive over an order of magnitude of difference in calling the Find method when you disable auto detect changes.</span></span> <span data-ttu-id="33c5f-351">Henüz veritabanından alınacak nesne sahip nesne aslında karşı önbelleğinde olduğunda ve ikinci bir büyüklük algılanır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-351">Yet a second order of magnitude is perceived when the object actually is in the cache versus when the object has to be retrieved from the database.</span></span> <span data-ttu-id="33c5f-352">5000 varlıkların bir yük, milisaniye cinsinden, bizim microbenchmarks bazılarını kullanarak gerçekleştirilen ölçümleri ile örnek bir grafik aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-352">Here is an example graph with measurements taken using some of our microbenchmarks, expressed in milliseconds, with a load of 5000 entities:</span></span>

<span data-ttu-id="33c5f-353">![.NET 4.5 Logaritmik ölçek](~/ef6/media/net45logscale.png ".NET 4.5 - Logaritmik ölçek")</span><span class="sxs-lookup"><span data-stu-id="33c5f-353">![.NET 4.5 logarithmic scale](~/ef6/media/net45logscale.png ".NET 4.5 - logarithmic scale")</span></span>

<span data-ttu-id="33c5f-354">Devre dışı auto-detect değişikliklerle örnek bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="33c5f-354">Example of Find with auto-detect changes disabled:</span></span>

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

<span data-ttu-id="33c5f-355">Find yöntemi kullanırken dikkate alınması gereken sahip aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-355">What you have to consider when using the Find method is:</span></span>

1.  <span data-ttu-id="33c5f-356">Nesne önbellekte değilse Bul avantajlarını tasarruflarını, ancak bir sorgu anahtarı tarafından hala basit bir sözdizimidir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-356">If the object is not in the cache the benefits of Find are negated, but the syntax is still simpler than a query by key.</span></span>
2.  <span data-ttu-id="33c5f-357">Değişiklikleri otomatik algıla durumunda etkin maliyetini bir bulma yöntemi bir büyüklük sırası veya nesne önbelleğinizi varlıklarda miktarı ve modelinizi karmaşıklığına bağlı olarak daha da artırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-357">If auto detect changes is enabled the cost of the Find method may increase by one order of magnitude, or even more depending on the complexity of your model and the amount of entities in your object cache.</span></span>

<span data-ttu-id="33c5f-358">Ayrıca, yalnızca Bul etkilenebileceğini aradığınız varlık döndürür ve değil zaten nesne önbellekte olmaları durumunda bunu otomatik olarak ilişkili varlıkları yükleri yapar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-358">Also, keep in mind that Find only returns the entity you are looking for and it does not automatically loads its associated entities if they are not already in the object cache.</span></span> <span data-ttu-id="33c5f-359">İlişkili varlıklar almanız gerekiyorsa, anahtar ile istekli yükleme tarafından bir sorgu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-359">If you need to retrieve associated entities, you can use a query by key with eager loading.</span></span> <span data-ttu-id="33c5f-360">Daha fazla bilgi için **8.1 vs yavaş yükleniyor. İstekli yükleme**.</span><span class="sxs-lookup"><span data-stu-id="33c5f-360">For more information see **8.1 Lazy Loading vs. Eager Loading**.</span></span>

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a><span data-ttu-id="33c5f-361">3.1.2 nesne önbelleği birçok varlığın olduğunda performans sorunları</span><span class="sxs-lookup"><span data-stu-id="33c5f-361">3.1.2 Performance issues when the object cache has many entities</span></span>

<span data-ttu-id="33c5f-362">Nesne önbelleği, Entity Framework'ün genel yanıt hızını artırmak için yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-362">The object cache helps to increase the overall responsiveness of Entity Framework.</span></span> <span data-ttu-id="33c5f-363">Ancak, nesne önbelleği çok büyük miktarda varlıklar ekleme gibi belirli işlemlerini etkileyebilir yüklü olduğunda, Kaldır, Bul, giriş, SaveChanges ve daha fazlası.</span><span class="sxs-lookup"><span data-stu-id="33c5f-363">However, when the object cache has a very large amount of entities loaded it may affect certain operations such as Add, Remove, Find, Entry, SaveChanges and more.</span></span> <span data-ttu-id="33c5f-364">Özellikle, DetectChanges çağrısı tetikleyen işlemler tarafından çok büyük nesne önbellekler olumsuz etkilenir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-364">In particular, operations that trigger a call to DetectChanges will be negatively affected by very large object caches.</span></span> <span data-ttu-id="33c5f-365">DetectChanges Nesne grafiğini doğrudan Nesne grafiğini boyutu tarafından belirlenir, performans edecek ve Nesne Durum Yöneticisi ile eşitler.</span><span class="sxs-lookup"><span data-stu-id="33c5f-365">DetectChanges synchronizes the object graph with the object state manager and its performance will determined directly by the size of the object graph.</span></span> <span data-ttu-id="33c5f-366">DetectChanges hakkında daha fazla bilgi için bkz: [POCO varlık değişiklikleri izleme](https://msdn.microsoft.com/library/dd456848.aspx).</span><span class="sxs-lookup"><span data-stu-id="33c5f-366">For more information about DetectChanges, see [Tracking Changes in POCO Entities](https://msdn.microsoft.com/library/dd456848.aspx).</span></span>

<span data-ttu-id="33c5f-367">Entity Framework 6 kullanırken, geliştiricilerin AddRange işlemi ve RemoveRange üzerindeki bir koleksiyona yineler ve Ekle örnek başına bir kez çağırmak yerine doğrudan bir olan DB, çağırma olanağına sahip olursunuz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-367">When using Entity Framework 6, developers are able to call AddRange and RemoveRange directly on a DbSet, instead of iterating on a collection and calling Add once per instance.</span></span> <span data-ttu-id="33c5f-368">Aralık yöntemleri kullanmanın avantajı DetectChanges maliyetini yalnızca bir kez eklenen her varlık başına bir kez yerine varlıkların tamamını için ödenen emin olan.</span><span class="sxs-lookup"><span data-stu-id="33c5f-368">The advantage of using the range methods is that the cost of DetectChanges is only paid once for the entire set of entities as opposed to once per each added entity.</span></span>

### <a name="32-query-plan-caching"></a><span data-ttu-id="33c5f-369">3.2 sorgu planı'önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="33c5f-369">3.2 Query Plan Caching</span></span>

<span data-ttu-id="33c5f-370">İlk kez bir sorgu yürütülür, bu depolama komutu (örneğin, T-SQL Server karşı çalıştırdığınızda yürütülür, SQL) kavramsal sorgu küçültmesini iç planı derleyici geçer.</span><span class="sxs-lookup"><span data-stu-id="33c5f-370">The first time a query is executed, it goes through the internal plan compiler to translate the conceptual query into the store command (for example, the T-SQL which is executed when run against SQL Server).</span></span><span data-ttu-id="33c5f-371">  Sorgu planını önbelleğe alma etkinse, sonraki açışınızda sorgu deposu yürütülen komut yürütme planı derleyici atlama için sorgu planı önbellek doğrudan alınır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-371">  If query plan caching is enabled, the next time the query is executed the store command is retrieved directly from the query plan cache for execution, bypassing the plan compiler.</span></span>

<span data-ttu-id="33c5f-372">Sorgu planını önbelleğe aynı AppDomain içinde ObjectContext örnekleri arasında paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-372">The query plan cache is shared across ObjectContext instances within the same AppDomain.</span></span> <span data-ttu-id="33c5f-373">Sorgu planını önbelleğe alma yararlanmak için bir ObjectContext örneği tutun gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-373">You don't need to hold onto an ObjectContext instance to benefit from query plan caching.</span></span>

#### <a name="321-some-notes-about-query-plan-caching"></a><span data-ttu-id="33c5f-374">3.2.1 bazı sorgu planı önbelleğe alma ile ilgili notlar</span><span class="sxs-lookup"><span data-stu-id="33c5f-374">3.2.1 Some notes about Query Plan Caching</span></span>

-   <span data-ttu-id="33c5f-375">Tüm sorgu türleri için sorgu planı önbellek paylaşıldı: Entity SQL, LINQ to Entities ve CompiledQuery nesneleri.</span><span class="sxs-lookup"><span data-stu-id="33c5f-375">The query plan cache is shared for all query types: Entity SQL, LINQ to Entities, and CompiledQuery objects.</span></span>
-   <span data-ttu-id="33c5f-376">Varsayılan olarak, sorgu planını önbelleğe alma varlık SQL sorgularında ObjectQuery veya bir EntityCommand aracılığıyla yürütülen olmadığını etkin.</span><span class="sxs-lookup"><span data-stu-id="33c5f-376">By default, query plan caching is enabled for Entity SQL queries, whether executed through an EntityCommand or through an ObjectQuery.</span></span> <span data-ttu-id="33c5f-377">Ayrıca varsayılan için LINQ to Entities sorgularında Entity Framework, .NET 4.5 ve Entity Framework 6 etkin</span><span class="sxs-lookup"><span data-stu-id="33c5f-377">It is also enabled by default for LINQ to Entities queries in Entity Framework on .NET 4.5, and in Entity Framework 6</span></span>
    -   <span data-ttu-id="33c5f-378">Sorgu planını önbelleğe alma (şirket EntityCommand veya ObjectQuery) EnablePlanCaching özelliği false olarak ayarlayarak devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-378">Query plan caching can be disabled by setting the EnablePlanCaching property (on EntityCommand or ObjectQuery) to false.</span></span> <span data-ttu-id="33c5f-379">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="33c5f-379">For example:</span></span>
``` csharp
                    var query = from customer in context.Customer
                                where customer.CustomerId == id
                                select new
                                {
                                    customer.CustomerId,
                                    customer.Name
                                };
                    ObjectQuery oQuery = query as ObjectQuery;
                    oQuery.EnablePlanCaching = false;
```
-   <span data-ttu-id="33c5f-380">Parametreli sorgular için parametrenin değerini değiştirerek önbelleğe alınmış sorgu hala ulaşırsınız.</span><span class="sxs-lookup"><span data-stu-id="33c5f-380">For parameterized queries, changing the parameter's value will still hit the cached query.</span></span> <span data-ttu-id="33c5f-381">Ancak, bir parametrenin facets (örneğin, boyutu, duyarlık veya Ölçek) değiştirerek farklı bir önbellek girdisi ulaşırsınız.</span><span class="sxs-lookup"><span data-stu-id="33c5f-381">But changing a parameter's facets (for example, size, precision, or scale) will hit a different entry in the cache.</span></span>
-   <span data-ttu-id="33c5f-382">Entity SQL kullanılırken, sorgu dizesi anahtarı bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-382">When using Entity SQL, the query string is part of the key.</span></span> <span data-ttu-id="33c5f-383">Sorgu hiç değiştirilmesi, sorguları işlevsel olarak eşdeğerdir olsa bile farklı bir önbellek girişlerinde neden olacak.</span><span class="sxs-lookup"><span data-stu-id="33c5f-383">Changing the query at all will result in different cache entries, even if the queries are functionally equivalent.</span></span> <span data-ttu-id="33c5f-384">Bu, büyük/küçük harf veya boşluk değişiklikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-384">This includes changes to casing or whitespace.</span></span>
-   <span data-ttu-id="33c5f-385">LINQ kullanırken, bir anahtarın parçası oluşturmak için sorguyu işlenir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-385">When using LINQ, the query is processed to generate a part of the key.</span></span> <span data-ttu-id="33c5f-386">LINQ ifadesi değiştirilmesi, bu nedenle farklı bir anahtar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-386">Changing the LINQ expression will therefore generate a different key.</span></span>
-   <span data-ttu-id="33c5f-387">Başka bir teknik kısıtlamalar geçerli olabilir; Autocompiled sorguları daha fazla ayrıntı için bkz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-387">Other technical limitations may apply; see Autocompiled Queries for more details.</span></span>

#### <a name="322-cache-eviction-algorithm"></a><span data-ttu-id="33c5f-388">3.2.2 önbellek çıkarma algoritması</span><span class="sxs-lookup"><span data-stu-id="33c5f-388">3.2.2      Cache eviction algorithm</span></span>

<span data-ttu-id="33c5f-389">İç algoritmaya works etkinleştirebilir veya devre dışı bırakma sorgu planını önbelleğe alma, anlayabilir nasıl yardımcı olabileceğini anlama.</span><span class="sxs-lookup"><span data-stu-id="33c5f-389">Understanding how the internal algorithm works will help you figure out when to enable or disable query plan caching.</span></span> <span data-ttu-id="33c5f-390">Temizleme algoritması aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-390">The cleanup algorithm is as follows:</span></span>

1.  <span data-ttu-id="33c5f-391">Biz, önbellek girdileri (800) ayarlama sayısını içeren sonra düzenli aralıklarla (bir kez dakikada) önbelleği temizleyen, Zamanlayıcı başlatın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-391">Once the cache contains a set number of entries (800), we start a timer that periodically (once-per-minute) sweeps the cache.</span></span>
2.  <span data-ttu-id="33c5f-392">Önbellek süpürmeleri sırasında bir LFRU (sık – son kullanılan az) üzerinde önbellekten girdiler kaldırılır temel.</span><span class="sxs-lookup"><span data-stu-id="33c5f-392">During cache sweeps, entries are removed from the cache on a LFRU (Least frequently – recently used) basis.</span></span> <span data-ttu-id="33c5f-393">Bu algoritma hem isabet sayısı ve yaş hangi girişlerin çıkarıldı karar verirken dikkate alır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-393">This algorithm takes both hit count and age into account when deciding which entries are ejected.</span></span>
3.  <span data-ttu-id="33c5f-394">Her önbellek Süpürme sonunda, önbelleği yeniden 800 girişler içeriyor.</span><span class="sxs-lookup"><span data-stu-id="33c5f-394">At the end of each cache sweep, the cache again contains 800 entries.</span></span>

<span data-ttu-id="33c5f-395">Tüm önbellek girişlerinin, çıkarmak için hangi girişlerin belirlerken eşit olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-395">All cache entries are treated equally when determining which entries to evict.</span></span> <span data-ttu-id="33c5f-396">Bu depolama komutu bir CompiledQuery için çıkarma varlık SQL sorgusu için depolama komutu olarak aynı şansı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-396">This means the store command for a CompiledQuery has the same chance of eviction as the store command for an Entity SQL query.</span></span>

<span data-ttu-id="33c5f-397">Önbellek çıkarma Zamanlayıcı önbellekte 800 varlık vardır, ancak önbellek yalnızca 60 saniye sonra bu Zamanlayıcı başlatıldığında gözden geçirilmiştir devreye girdi olmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-397">Note that the cache eviction timer is kicked in when there are 800 entities in the cache, but the cache is only swept 60 seconds after this timer is started.</span></span> <span data-ttu-id="33c5f-398">Bu, çok büyük 60 saniye için önbelleğinizi büyüme anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-398">That means that for up to 60 seconds your cache may grow to be quite large.</span></span>

#### <a name="323-test-metrics-demonstrating-query-plan-caching-performance"></a><span data-ttu-id="33c5f-399">3.2.3 sorgu planını önbelleğe alma performans gösteren ölçümleri test</span><span class="sxs-lookup"><span data-stu-id="33c5f-399">3.2.3       Test Metrics demonstrating query plan caching performance</span></span>

<span data-ttu-id="33c5f-400">Sorgu planını önbelleğe alma, uygulamanızın performansı etkisini göstermek için bir test ediyoruz Navision model Entity SQL sorguları bir dizi yürütüldüğü gerçekleştirdiğimiz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-400">To demonstrate the effect of query plan caching on your application's performance, we performed a test where we executed a number of Entity SQL queries against the Navision model.</span></span> <span data-ttu-id="33c5f-401">Ek Navision modeli ve yürütüldü sorguları türde bir açıklaması için bkz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-401">See the appendix for a description of the Navision model and the types of queries which were executed.</span></span> <span data-ttu-id="33c5f-402">Bu test ediyoruz önce sorguları listesi boyunca yineleme yapmak ve her bir kez (önbelleğe alma etkinse) bunları önbelleğine eklemek için çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-402">In this test, we first iterate through the list of queries and execute each one once to add them to the cache (if caching is enabled).</span></span> <span data-ttu-id="33c5f-403">Bu adım untimed bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-403">This step is untimed.</span></span> <span data-ttu-id="33c5f-404">Ardından, ana iş parçacığı gerçekleşmesi için üst düzey önbellek izin vermek tekrar 60 saniye için uyku; Son olarak, önbelleğe alınan sorgularını yürütmek için 2. bir liste zaman yineleme.</span><span class="sxs-lookup"><span data-stu-id="33c5f-404">Next, we sleep the main thread for over 60 seconds to allow cache sweeping to take place; finally, we iterate through the list a 2nd time to execute the cached queries.</span></span> <span data-ttu-id="33c5f-405">Ayrıca, böylece doğru bir şekilde elde zamanları sorgu planı önbelleği tarafından verilen avantajı sorgular kümelerine yürütülmeden önce SQL Server planı önbelleği temizlendi.</span><span class="sxs-lookup"><span data-stu-id="33c5f-405">Additionally, the SQL Server plan cache is flushed before each set of queries is executed so that the times we obtain accurately reflect the benefit given by the query plan cache.</span></span>

##### <a name="3231-test-results"></a><span data-ttu-id="33c5f-406">3.2.3.1 test sonuçları</span><span class="sxs-lookup"><span data-stu-id="33c5f-406">3.2.3.1       Test Results</span></span>

| <span data-ttu-id="33c5f-407">Test</span><span class="sxs-lookup"><span data-stu-id="33c5f-407">Test</span></span>                                                                   | <span data-ttu-id="33c5f-408">EF5 önbellek yok</span><span class="sxs-lookup"><span data-stu-id="33c5f-408">EF5 no cache</span></span> | <span data-ttu-id="33c5f-409">Önbelleğe alınmış EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-409">EF5 cached</span></span> | <span data-ttu-id="33c5f-410">EF6 önbellek yok</span><span class="sxs-lookup"><span data-stu-id="33c5f-410">EF6 no cache</span></span> | <span data-ttu-id="33c5f-411">Önbelleğe alınmış EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-411">EF6 cached</span></span> |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| <span data-ttu-id="33c5f-412">Tüm 18723 sorguları numaralandırma</span><span class="sxs-lookup"><span data-stu-id="33c5f-412">Enumerating all 18723 queries</span></span>                                          | <span data-ttu-id="33c5f-413">124</span><span class="sxs-lookup"><span data-stu-id="33c5f-413">124</span></span>          | <span data-ttu-id="33c5f-414">125.4</span><span class="sxs-lookup"><span data-stu-id="33c5f-414">125.4</span></span>      | <span data-ttu-id="33c5f-415">124.3</span><span class="sxs-lookup"><span data-stu-id="33c5f-415">124.3</span></span>        | <span data-ttu-id="33c5f-416">125.3</span><span class="sxs-lookup"><span data-stu-id="33c5f-416">125.3</span></span>      |
| <span data-ttu-id="33c5f-417">Gözden geçirme (yalnızca ilk 800 sorgular, karmaşıklığı bağımsız olarak) önleme</span><span class="sxs-lookup"><span data-stu-id="33c5f-417">Avoiding sweep (just the first 800 queries, regardless of complexity)</span></span>  | <span data-ttu-id="33c5f-418">41.7</span><span class="sxs-lookup"><span data-stu-id="33c5f-418">41.7</span></span>         | <span data-ttu-id="33c5f-419">5.5</span><span class="sxs-lookup"><span data-stu-id="33c5f-419">5.5</span></span>        | <span data-ttu-id="33c5f-420">40.5</span><span class="sxs-lookup"><span data-stu-id="33c5f-420">40.5</span></span>         | <span data-ttu-id="33c5f-421">5,4</span><span class="sxs-lookup"><span data-stu-id="33c5f-421">5.4</span></span>        |
| <span data-ttu-id="33c5f-422">Yalnızca AggregatingSubtotals sorgular (hangi Süpürme önler 178 toplam -)</span><span class="sxs-lookup"><span data-stu-id="33c5f-422">Just the AggregatingSubtotals queries (178 total - which avoids sweep)</span></span> | <span data-ttu-id="33c5f-423">39.5</span><span class="sxs-lookup"><span data-stu-id="33c5f-423">39.5</span></span>         | <span data-ttu-id="33c5f-424">4,5</span><span class="sxs-lookup"><span data-stu-id="33c5f-424">4.5</span></span>        | <span data-ttu-id="33c5f-425">38.1</span><span class="sxs-lookup"><span data-stu-id="33c5f-425">38.1</span></span>         | <span data-ttu-id="33c5f-426">4.6</span><span class="sxs-lookup"><span data-stu-id="33c5f-426">4.6</span></span>        |

<span data-ttu-id="33c5f-427">*Her zaman saniyelerle.*</span><span class="sxs-lookup"><span data-stu-id="33c5f-427">*All times in seconds.*</span></span>

<span data-ttu-id="33c5f-428">Moral - yürütme birçok olduğunda (örneğin sorguları dinamik olarak oluşturulan), ayrı sorguların önbelleğe alma için yardımcı olmaz ve sonuçta elde edilen önbelleğini temizlemeye gerçekten kullanımından planı en önbelleğe alınan avantaj elde edecektir sorguları tutabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-428">Moral - when executing lots of distinct queries (for example,  dynamically created queries), caching doesn't help and the resulting flushing of the cache can keep the queries that would benefit the most from plan caching from actually using it.</span></span>

<span data-ttu-id="33c5f-429">AggregatingSubtotals sorgular ile test edilen sorguların en karmaşık kullanılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-429">The AggregatingSubtotals queries are the most complex of the queries we tested with.</span></span> <span data-ttu-id="33c5f-430">Daha karmaşık olan sorguyu sorgu planını önbelleğe alma görürsünüz daha fazla avantaj beklendiği gibi.</span><span class="sxs-lookup"><span data-stu-id="33c5f-430">As expected, the more complex the query is, the more benefit you will see from query plan caching.</span></span>

<span data-ttu-id="33c5f-431">Bir CompiledQuery gerçekten bir LINQ Sorgu önbelleğe kendi planına sahip olduğundan, eşdeğer varlık SQL sorgusu karşı bir CompiledQuery karşılaştırması benzer sonuçlar olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-431">Because a CompiledQuery is really a LINQ query with its plan cached, the comparison of a CompiledQuery versus the equivalent Entity SQL query should have similar results.</span></span> <span data-ttu-id="33c5f-432">Bir uygulamanın dinamik Entity SQL sorguları çok sayıda varsa, aslında, önbellek sorgularla doldurma da verimli bir şekilde "önbellekten Temizlenen olduğunda derlemeyi" CompiledQueries neden olur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-432">In fact, if an app has lots of dynamic Entity SQL queries, filling the cache with queries will also effectively cause CompiledQueries to “decompile” when they are flushed from the cache.</span></span> <span data-ttu-id="33c5f-433">Bu senaryoda CompiledQueries önceliğini belirlemek için dinamik sorgular üzerinde önbelleğe alma devre dışı bırakarak performans artırılabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-433">In this scenario, performance may be improved by disabling caching on the dynamic queries to prioritize the CompiledQueries.</span></span> <span data-ttu-id="33c5f-434">Üstelik, parametreli sorgular yerine dinamik sorgular kullanmak için uygulamayı yeniden olacaktır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-434">Better yet, of course, would be to rewrite the app to use parameterized queries instead of dynamic queries.</span></span>

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a><span data-ttu-id="33c5f-435">3.3 LINQ sorguları ile performansı CompiledQuery kullanma</span><span class="sxs-lookup"><span data-stu-id="33c5f-435">3.3 Using CompiledQuery to improve performance with LINQ queries</span></span>

<span data-ttu-id="33c5f-436">Testlerimiz CompiledQuery kullanarak bir yararı % 7, autocompiled LINQ sorguları getirebilirsiniz olduğunu gösterir; Bu, daha az zaman Entity Framework yığından kod yürütülürken % 7 geçireceksiniz anlamına gelir; Uygulamanızı %7 daha hızlı olacak gelmez.</span><span class="sxs-lookup"><span data-stu-id="33c5f-436">Our tests indicate that using CompiledQuery can bring a benefit of 7% over autocompiled LINQ queries; this means that you’ll spend 7% less time executing code from the Entity Framework stack; it does not mean your application will be 7% faster.</span></span> <span data-ttu-id="33c5f-437">Genel olarak bakıldığında, yazma ve EF 5.0 CompiledQuery nesneleri bakım maliyeti avantaj karşılaştırıldığında sorun değer olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-437">Generally speaking, the cost of writing and maintaining CompiledQuery objects in EF 5.0 may not be worth the trouble when compared to the benefits.</span></span> <span data-ttu-id="33c5f-438">Mesafe farklı olduğundan, ek anında iletme projeniz gerektiriyorsa, bu nedenle bu seçeneği alıştırma.</span><span class="sxs-lookup"><span data-stu-id="33c5f-438">Your mileage may vary, so exercise this option if your project requires the extra push.</span></span> <span data-ttu-id="33c5f-439">CompiledQueries yalnızca ObjectContext türetilmiş modellerle uyumlu ve DbContext türetilmiş modellerle uyumlu olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-439">Note that CompiledQueries are only compatible with ObjectContext-derived models, and not compatible with DbContext-derived models.</span></span>

<span data-ttu-id="33c5f-440">Oluşturma ve bir CompiledQuery çağırma hakkında daha fazla bilgi için bkz. [derlenmiş sorgular (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).</span><span class="sxs-lookup"><span data-stu-id="33c5f-440">For more information on creating and invoking a CompiledQuery, see [Compiled Queries (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).</span></span>

<span data-ttu-id="33c5f-441">CompiledQuery, sahip oldukları composability ile statik örnekleri ve sorunları kullanma özelliği gereksinimi kullanırken yapmanız gereken iki önemli noktalar vardır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-441">There are two considerations you have to take when using a CompiledQuery, namely the requirement to use static instances and the problems they have with composability.</span></span> <span data-ttu-id="33c5f-442">Bu iki noktalar ayrıntılı bir açıklamasını buraya izler.</span><span class="sxs-lookup"><span data-stu-id="33c5f-442">Here follows an in-depth explanation of these two considerations.</span></span>

#### <a name="331-use-static-compiledquery-instances"></a><span data-ttu-id="33c5f-443">3.3.1 statik CompiledQuery örnekleri kullan</span><span class="sxs-lookup"><span data-stu-id="33c5f-443">3.3.1       Use static CompiledQuery instances</span></span>

<span data-ttu-id="33c5f-444">LINQ sorgusu derleme zaman alan bir işlem olduğundan, biz bunu yapmanın veritabanından veri getirme ihtiyacımız her zaman istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-444">Since compiling a LINQ query is a time-consuming process, we don’t want to do it every time we need to fetch data from the database.</span></span> <span data-ttu-id="33c5f-445">Bir kez derlemek ve birden çok kez çalıştırmak CompiledQuery örnekleri izin, ancak dikkatli olması ve aynı CompiledQuery örneği üzerinde yeniden derlemek yerine her zaman yeniden kullanmak için tedarik edin.</span><span class="sxs-lookup"><span data-stu-id="33c5f-445">CompiledQuery instances allow you to compile once and run multiple times, but you have to be careful and procure to re-use the same CompiledQuery instance every time instead of compiling it over and over again.</span></span> <span data-ttu-id="33c5f-446">Statik üyeleri CompiledQuery örneklerini depolamak için kullanımını gerekli hale gelir; Aksi takdirde, hiçbir avantajı görmezsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-446">The use of static members to store the CompiledQuery instances becomes necessary; otherwise you won’t see any benefit.</span></span>

<span data-ttu-id="33c5f-447">Örneğin, seçilen kategori ürünleri görüntüleme işlemek için aşağıdaki yöntem gövdesini sayfanız olduğunu varsayın:</span><span class="sxs-lookup"><span data-stu-id="33c5f-447">For example, suppose your page has the following method body to handle displaying the products for the selected category:</span></span>

``` csharp
    // Warning: this is the wrong way of using CompiledQuery
    using (NorthwindEntities context = new NorthwindEntities())
    {
        string selectedCategory = this.categoriesList.SelectedValue;

        var productsForCategory = CompiledQuery.Compile<NorthwindEntities, string, IQueryable<Product>>(
            (NorthwindEntities nwnd, string category) =>
                nwnd.Products.Where(p => p.Category.CategoryName == category)
        );

        this.productsGrid.DataSource = productsForCategory.Invoke(context, selectedCategory).ToList();
        this.productsGrid.DataBind();
    }

    this.productsGrid.Visible = true;
```

<span data-ttu-id="33c5f-448">Bu durumda, yöntem her çağrıldığında, çalışma sırasında yeni bir CompiledQuery örneği oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="33c5f-448">In this case, you will create a new CompiledQuery instance on the fly every time the method is called.</span></span> <span data-ttu-id="33c5f-449">Sorgu planı önbellekten depo komutu alarak performans avantajlarının görmenin yerine, yeni bir örneği oluşturulduğunda CompiledQuery planı derleyici geçer.</span><span class="sxs-lookup"><span data-stu-id="33c5f-449">Instead of seeing performance benefits by retrieving the store command from the query plan cache, the CompiledQuery will go through the plan compiler every time a new instance is created.</span></span> <span data-ttu-id="33c5f-450">Yöntem her çağrıldığında aslında, sorgu planı önbelleğinizi yeni CompiledQuery girdisi ile kirletmesini.</span><span class="sxs-lookup"><span data-stu-id="33c5f-450">In fact, you will be polluting your query plan cache with a new CompiledQuery entry every time the method is called.</span></span>

<span data-ttu-id="33c5f-451">Bunun yerine, yöntem her çağrıldığında aynı derlenmiş sorgu çağırdığınız şekilde derlenmiş sorgu statik bir örneğini oluşturmak istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-451">Instead, you want to create a static instance of the compiled query, so you are invoking the same compiled query every time the method is called.</span></span> <span data-ttu-id="33c5f-452">Yollarından biri bunu nesne Bağlamınızı bir üyesi olarak CompiledQuery örneği ekleyerek, bu nedenle.</span><span class="sxs-lookup"><span data-stu-id="33c5f-452">One way to so this is by adding the CompiledQuery instance as a member of your object context.</span></span><span data-ttu-id="33c5f-453">  Öğeleri küçük temizleyici CompiledQuery bir yardımcı yöntem aracılığıyla erişerek daha sonra yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="33c5f-453">  You can then make things a little cleaner by accessing the CompiledQuery through a helper method:</span></span>

``` csharp
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IEnumerable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
            );

        public IEnumerable<Product> GetProductsForCategory(string categoryName)
        {
            return productsForCategoryCQ.Invoke(this, categoryName).ToList();
        }
```

<span data-ttu-id="33c5f-454">Bu yardımcı yöntem şu şekilde çağrılması:</span><span class="sxs-lookup"><span data-stu-id="33c5f-454">This helper method would be invoked as follows:</span></span>

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-composing-over-a-compiledquery"></a><span data-ttu-id="33c5f-455">3.3.2 bir CompiledQuery oluşturma</span><span class="sxs-lookup"><span data-stu-id="33c5f-455">3.3.2       Composing over a CompiledQuery</span></span>

<span data-ttu-id="33c5f-456">Özelliği herhangi bir LINQ sorgu oluşturmak için son derece kullanışlıdır. Bunu yapmak için yalnızca bir yöntem sonra Iqueryable gibi çağırma *Skip()* veya *Count()*.</span><span class="sxs-lookup"><span data-stu-id="33c5f-456">The ability to compose over any LINQ query is extremely useful; to do this, you simply invoke a method after the IQueryable such as *Skip()* or *Count()*.</span></span> <span data-ttu-id="33c5f-457">Ancak, girebiliyorsunuz yapılması, yeni bir Iqueryable nesnesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="33c5f-457">However, doing so essentially returns a new IQueryable object.</span></span> <span data-ttu-id="33c5f-458">Yeni bir Iqueryable nesne oluşturmayı, neden olacak, böylece varken bir CompiledQuery oluşturma gelen teknik olarak durdurmak için hiçbir şey, planı derleyici ölçeklendirilebilirlikten yeniden gerektirir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-458">While there’s nothing to stop you technically from composing over a CompiledQuery, doing so will cause the generation of a new IQueryable object that requires passing through the plan compiler again.</span></span>

<span data-ttu-id="33c5f-459">Bazı bileşenler oluşan Iqueryable kullanın hale getirecek gelişmiş işlevselliği etkinleştirmek için nesneleri.</span><span class="sxs-lookup"><span data-stu-id="33c5f-459">Some components will make use of composed IQueryable objects to enable advanced functionality.</span></span> <span data-ttu-id="33c5f-460">Örneğin, ASP. NET GridView veri-Iqueryable nesneye SelectMethod özelliği üzerinden bağlanmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-460">For example, ASP.NET’s GridView can be data-bound to an IQueryable object via the SelectMethod property.</span></span> <span data-ttu-id="33c5f-461">GridView veri modeli üzerinde sayfalama ve sıralama izin vermek için bu Iqueryable nesne üzerinde compose.</span><span class="sxs-lookup"><span data-stu-id="33c5f-461">The GridView will then compose over this IQueryable object to allow sorting and paging over the data model.</span></span> <span data-ttu-id="33c5f-462">Gördüğünüz gibi bir CompiledQuery için GridView kullanarak derlenmiş sorgu isabet değil ancak yeni bir autocompiled sorgu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-462">As you can see, using a CompiledQuery for the GridView would not hit the compiled query but would generate a new autocompiled query.</span></span>

<span data-ttu-id="33c5f-463">Bir sorgu için aşamalı filtreler ekleyerek bu burada çalışabilir tek bir yerde andır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-463">One place where you may run into this is when adding progressive filters to a query.</span></span> <span data-ttu-id="33c5f-464">Örneğin, çeşitli açılan listeleri için isteğe bağlı filtreler (örneğin, ülke ve OrdersCount) müşteriler sayfasıyla sahip olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="33c5f-464">For example, suppose you had a Customers page with several drop-down lists for optional filters (for example, Country and OrdersCount).</span></span> <span data-ttu-id="33c5f-465">Bir CompiledQuery Iqueryable sonuçları üzerinde bu filtreler oluşturabilirsiniz ancak bunun yapılması, bu nedenle planı Derleyici bunu her zaman giderek yeni sorguda neden olur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-465">You can compose these filters over the IQueryable results of a CompiledQuery, but doing so will result in the new query going through the plan compiler every time you execute it.</span></span>

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployee();

        if (this.orderCountFilterList.SelectedItem.Value != defaultFilterText)
        {
            int orderCount = int.Parse(orderCountFilterList.SelectedValue);
            myCustomers = myCustomers.Where(c => c.Orders.Count > orderCount);
        }

        if (this.countryFilterList.SelectedItem.Value != defaultFilterText)
        {
            myCustomers = myCustomers.Where(c => c.Address.Country == countryFilterList.SelectedValue);
        }

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 <span data-ttu-id="33c5f-466">Bu yeniden derleme kaçınmak için mümkün filtreler dikkate almanız CompiledQuery yazabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="33c5f-466">To avoid this re-compilation, you can rewrite the CompiledQuery to take the possible filters into account:</span></span>

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

<span data-ttu-id="33c5f-467">Olduğu gibi kullanıcı arabiriminde çağrıldığı:</span><span class="sxs-lookup"><span data-stu-id="33c5f-467">Which would be invoked in the UI like:</span></span>

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        int? countFilter = (this.orderCountFilterList.SelectedIndex == 0) ?
            (int?)null :
            int.Parse(this.orderCountFilterList.SelectedValue);

        string countryFilter = (this.countryFilterList.SelectedIndex == 0) ?
            null :
            this.countryFilterList.SelectedValue;

        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployeeWithFilters(
                countFilter, countryFilter);

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 <span data-ttu-id="33c5f-468">Oluşturulan depo komutu her zaman null denetimleri filtrelerle olacaktır, ancak bunlar veritabanı sunucusu için en iyi duruma getirmeyi oldukça basit olmalıdır bir tradeoff burada verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-468">A tradeoff here is the generated store command will always have the filters with the null checks, but these should be fairly simple for the database server to optimize:</span></span>

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a><span data-ttu-id="33c5f-469">3.4 meta verileri önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="33c5f-469">3.4 Metadata caching</span></span>

<span data-ttu-id="33c5f-470">Entity Framework meta verileri önbelleğe almayı da destekler.</span><span class="sxs-lookup"><span data-stu-id="33c5f-470">The Entity Framework also supports Metadata caching.</span></span> <span data-ttu-id="33c5f-471">Bu temelde tür bilgileri ve tür veritabanı eşleme bilgileri aynı modelin farklı bağlantıları arasında önbelleğe alma.</span><span class="sxs-lookup"><span data-stu-id="33c5f-471">This is essentially caching of type information and type-to-database mapping information across different connections to the same model.</span></span> <span data-ttu-id="33c5f-472">Meta veri önbelleği AppDomain benzersiz değil.</span><span class="sxs-lookup"><span data-stu-id="33c5f-472">The Metadata cache is unique per AppDomain.</span></span>

#### <a name="341-metadata-caching-algorithm"></a><span data-ttu-id="33c5f-473">3.4.1 meta verileri önbelleğe alma algoritması</span><span class="sxs-lookup"><span data-stu-id="33c5f-473">3.4.1 Metadata Caching algorithm</span></span>

1.  <span data-ttu-id="33c5f-474">Bir model için meta veri bilgileri için her bir EntityConnection bir ItemCollection içinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-474">Metadata information for a model is stored in an ItemCollection for each EntityConnection.</span></span>
    -   <span data-ttu-id="33c5f-475">Yan Not olarak modelin farklı bölümleri için farklı ItemCollection nesnesi vardır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-475">As a side note, there are different ItemCollection objects for different parts of the model.</span></span> <span data-ttu-id="33c5f-476">Örneğin, StoreItemCollections veritabanı modeli hakkında bilgi içerir. ObjectItemCollection veri modeli hakkında bilgi içerir. Edmıtemcollection kavramsal modelle ilgili bilgiler içerir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-476">For example, StoreItemCollections contains the information about the database model; ObjectItemCollection contains information about the data model; EdmItemCollection contains information about the conceptual model.</span></span>

2.  <span data-ttu-id="33c5f-477">İki bağlantı aynı bağlantı dizesi kullanıyorsanız, bunlar aynı ItemCollection örneği paylaşır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-477">If two connections use the same connection string, they will share the same ItemCollection instance.</span></span>
3.  <span data-ttu-id="33c5f-478">Eşdeğer işleve sahiptir ancak metin içeriğini eklemek farklı bağlantı dizeleri farklı meta veri önbelleği neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-478">Functionally equivalent but textually different connection strings may result in different metadata caches.</span></span> <span data-ttu-id="33c5f-479">Biz bağlantı dizelerini, bu nedenle yalnızca belirteçleri sırasını değiştirmek paylaşılan meta verilerinde sonuçlanmalıdır simgeleştirin.</span><span class="sxs-lookup"><span data-stu-id="33c5f-479">We do tokenize connection strings, so simply changing the order of the tokens should result in shared metadata.</span></span> <span data-ttu-id="33c5f-480">Ancak, işlevsel olarak aynı görünen iki bağlantı dizesi aynı sonra simgeleştirme değerlendirilmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-480">But two connection strings that seem functionally the same may not be evaluated as identical after tokenization.</span></span>
4.  <span data-ttu-id="33c5f-481">ItemCollection kullanım için düzenli olarak denetlenir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-481">The ItemCollection is periodically checked for use.</span></span> <span data-ttu-id="33c5f-482">Bir çalışma alanı yakın zamanda eriştiğini değil belirlenirse sonraki önbellek Süpürme üzerinde Temizleme için işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-482">If it is determined that a workspace has not been accessed recently, it will be marked for cleanup on the next cache sweep.</span></span>
5.  <span data-ttu-id="33c5f-483">Yalnızca bir EntityConnection oluşturma (bağlantı açılıncaya kadar içindeki öğe koleksiyonlarını başlatılmayacak rağmen) oluşturulması için bir meta veri önbelleği neden olur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-483">Merely creating an EntityConnection will cause a metadata cache to be created (though the item collections in it will not be initialized until the connection is opened).</span></span> <span data-ttu-id="33c5f-484">Bu çalışma alanı bellek içi ermesine "kullanımda" değil, önbelleğe alma algoritmasını belirler.</span><span class="sxs-lookup"><span data-stu-id="33c5f-484">This workspace will remain in-memory until the caching algorithm determines it is not “in use”.</span></span>

<span data-ttu-id="33c5f-485">Müşteri danışma ekibi, "kullanımdan kaldırma" büyük modellerin kullanırken önlemek için bir ItemCollection bir başvuru tutan açıklayan bir blog gönderisi yazmıştır: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.</span><span class="sxs-lookup"><span data-stu-id="33c5f-485">The Customer Advisory Team has written a blog post that describes holding a reference to an ItemCollection in order to avoid "deprecation" when using large models: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.</span></span>

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a><span data-ttu-id="33c5f-486">3.4.2 arasındaki ilişkiyi meta verileri önbelleğe alma ve sorgu planı önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="33c5f-486">3.4.2 The relationship between Metadata Caching and Query Plan Caching</span></span>

<span data-ttu-id="33c5f-487">Sorgu planı önbellek örneği depolama türlerinin MetadataWorkspace'nın ItemCollection yaşar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-487">The query plan cache instance lives in the MetadataWorkspace's ItemCollection of store types.</span></span> <span data-ttu-id="33c5f-488">Bu, önbelleğe alınan depolama komutları sorguları kullanarak belirli bir MetadataWorkspace örneği her bağlam için kullanılacak anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-488">This means that cached store commands will be used for queries against any context instantiated using a given MetadataWorkspace.</span></span> <span data-ttu-id="33c5f-489">Ayrıca, biraz farklıdır ve belirteç oluşturma sonrasında eşleşmeyen iki bağlantı dizeleri varsa, önbellek örnekleri plan farklı sorgu olacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-489">It also means that if you have two connections strings that are slightly different and don't match after tokenizing, you will have different query plan cache instances.</span></span>

### <a name="35-results-caching"></a><span data-ttu-id="33c5f-490">3.5 sonuçları önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="33c5f-490">3.5 Results caching</span></span>

<span data-ttu-id="33c5f-491">Önbelleğe alma sonuçları (diğer adıyla "ikinci düzey önbelleğe alma") ile sorguların sonuçlarını bir yerel önbellek üzerinde tutun.</span><span class="sxs-lookup"><span data-stu-id="33c5f-491">With results caching (also known as "second-level caching"), you keep the results of queries in a local cache.</span></span> <span data-ttu-id="33c5f-492">Bir sorgu verme ilk sonuçları, sorgu deposu ile karşılaştırarak önce yerel olarak kullanılabilir olup olmadığını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-492">When issuing a query, you first see if the results are available locally before you query against the store.</span></span> <span data-ttu-id="33c5f-493">Sonuçları önbelleğe alma, Entity Framework tarafından doğrudan desteklenmiyor olsa da bir sarmalama sağlayıcısını kullanarak ikinci bir düzey önbellek eklemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="33c5f-493">While results caching isn't directly supported by Entity Framework, it's possible to add a second level cache by using a wrapping provider.</span></span> <span data-ttu-id="33c5f-494">Bir örnek kaydırma, ikinci düzey önbellek Alachisoft'ın sağlayıcısıdır [Entity Framework ikinci düzey önbellek üzerinde NCache tabanlı](http://www.alachisoft.com/ncache/entity-framework.html).</span><span class="sxs-lookup"><span data-stu-id="33c5f-494">An example wrapping provider with a second-level cache is Alachisoft's [Entity Framework Second Level Cache based on NCache](http://www.alachisoft.com/ncache/entity-framework.html).</span></span>

<span data-ttu-id="33c5f-495">Bu uygulama, ikinci düzey önbelleğe alma ve sorgu yürütme planı hesaplanan ya da birinci düzey önbellekten LINQ ifadesi değerlendirildikten sonra bir yerde (ve funcletized) eklenen bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-495">This implementation of second-level caching is an injected functionality that takes place after the LINQ expression has been evaluated (and funcletized) and the query execution plan is computed or retrieved from the first-level cache.</span></span> <span data-ttu-id="33c5f-496">Materialization ardışık düzen yine de daha sonra yürütür için ikinci düzey önbellek sonra yalnızca ham veritabanı sonuçlarını depolar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-496">The second-level cache will then store only the raw database results, so the materialization pipeline still executes afterwards.</span></span>

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a><span data-ttu-id="33c5f-497">3.5.1 sonuçlar sarmalama sağlayıcısında önbelleğe alma için ek başvurular</span><span class="sxs-lookup"><span data-stu-id="33c5f-497">3.5.1 Additional references for results caching with the wrapping provider</span></span>

-   <span data-ttu-id="33c5f-498">Windows Server AppFabric önbelleğe almayı kullanmak üzere örnek sarmalama sağlayıcı güncelleştirme içeren bir "İkinci düzey önbelleğe alma, Entity Framework ve Windows Azure" MSDN makalesi Julie Lerman yazmıştır: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)</span><span class="sxs-lookup"><span data-stu-id="33c5f-498">Julie Lerman has written a "Second-Level Caching in Entity Framework and Windows Azure" MSDN article that includes how to update the sample wrapping provider to use Windows Server AppFabric caching: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)</span></span>
-   <span data-ttu-id="33c5f-499">Entity Framework 5 ile çalışıyorsanız, önbelleğe alma sağlayıcısı için Entity Framework 5 ile çalışan gerçekleştirmeyi açıklayan bir gönderi ekibi blogu vardır: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>.</span><span class="sxs-lookup"><span data-stu-id="33c5f-499">If you are working with Entity Framework 5, the team blog has a post that describes how to get things running with the caching provider for Entity Framework 5: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>.</span></span> <span data-ttu-id="33c5f-500">2. düzey projenize önbelleğe almanın eklenmesi otomatikleştirmeye yardımcı olmak için T4 şablonu da içerir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-500">It also includes a T4 template to help automate adding the 2nd-level caching to your project.</span></span>

## <a name="4-autocompiled-queries"></a><span data-ttu-id="33c5f-501">4 Autocompiled sorguları</span><span class="sxs-lookup"><span data-stu-id="33c5f-501">4 Autocompiled Queries</span></span>

<span data-ttu-id="33c5f-502">Entity Framework kullanarak bir veritabanında bir sorgu verildiğinde, bir dizi adım ile gerçekten sonuçları düzeniyle önce gitmelidir; Böyle bir adım sorgu derlemesi ' dir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-502">When a query is issued against a database using Entity Framework, it must go through a series of steps before actually materializing the results; one such step is Query Compilation.</span></span> <span data-ttu-id="33c5f-503">Entity SQL sorguları otomatik olarak önbelleğe gibi ikinci veya üçüncü kez planı derleyici atlayın ve bunun yerine önbellekteki plan kullanın aynı sorgu yürütmek için iyi bir performans sağlamak için bilinirdi.</span><span class="sxs-lookup"><span data-stu-id="33c5f-503">Entity SQL queries were known to have good performance as they are automatically cached, so the second or third time you execute the same query it can skip the plan compiler and use the cached plan instead.</span></span>

<span data-ttu-id="33c5f-504">Entity Framework 5 otomatik LINQ için de Entities sorgularında önbelleğe kullanıma sunuldu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-504">Entity Framework 5 introduced automatic caching for LINQ to Entities queries as well.</span></span> <span data-ttu-id="33c5f-505">Bu, LINQ Entities sorgusunda önbelleğe alınabilir hale getirir Entity Framework hızlandırmak için bir CompiledQuery oluşturma önceki sürümlerinde yaygın bir uygulama performansınızı kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="33c5f-505">In past editions of Entity Framework creating a CompiledQuery to speed your performance was a common practice, as this would make your LINQ to Entities query cacheable.</span></span> <span data-ttu-id="33c5f-506">Önbelleğe alma artık otomatik olarak bir CompiledQuery kullanmadan yapılır olduğundan, "autocompiled sorguları" Bu özellik diyoruz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-506">Since caching is now done automatically without the use of a CompiledQuery, we call this feature “autocompiled queries”.</span></span> <span data-ttu-id="33c5f-507">Sorgu planını önbelleğe alma sorgu planı önbellek ve onun mekanizması hakkında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-507">For more information about the query plan cache and its mechanics, see Query Plan Caching.</span></span>

<span data-ttu-id="33c5f-508">Entity Framework derlenmesi için bir sorgu gerektirdiğinde algılar ve sorgu çağrıldığında, önce derlenen olsanız bile bunu yapar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-508">Entity Framework detects when a query requires to be recompiled, and does so when the query is invoked even if it had been compiled before.</span></span> <span data-ttu-id="33c5f-509">Derlenmesi için sorguyu neden olan genel koşullar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="33c5f-509">Common conditions that cause the query to be recompiled are:</span></span>

-   <span data-ttu-id="33c5f-510">Sorgunuz için ilişkili MergeOption değiştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="33c5f-510">Changing the MergeOption associated to your query.</span></span> <span data-ttu-id="33c5f-511">Önbelleğe alınmış sorgu kullanılmayacak, bunun yerine planı derleyici yeniden çalıştırın ve yeni oluşturulan plan önbelleğe.</span><span class="sxs-lookup"><span data-stu-id="33c5f-511">The cached query will not be used, instead the plan compiler will run again and the newly created plan gets cached.</span></span>
-   <span data-ttu-id="33c5f-512">ContextOptions.UseCSharpNullComparisonBehavior değerinin değiştirilmesi.</span><span class="sxs-lookup"><span data-stu-id="33c5f-512">Changing the value of ContextOptions.UseCSharpNullComparisonBehavior.</span></span> <span data-ttu-id="33c5f-513">MergeOption değiştirme aynı etkiye sahip olursunuz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-513">You get the same effect as changing the MergeOption.</span></span>

<span data-ttu-id="33c5f-514">Diğer koşullar, sorgu önbelleği kullanılmasını önleyebilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-514">Other conditions can prevent your query from using the cache.</span></span> <span data-ttu-id="33c5f-515">Ortak örnekler verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-515">Common examples are:</span></span>

-   <span data-ttu-id="33c5f-516">IEnumerable kullanarak&lt;T&gt;. İçeren&lt;&gt;(T değeri).</span><span class="sxs-lookup"><span data-stu-id="33c5f-516">Using IEnumerable&lt;T&gt;.Contains&lt;&gt;(T value).</span></span>
-   <span data-ttu-id="33c5f-517">Sabitler ile sorguları oluşturan işlevleri'ni kullanarak.</span><span class="sxs-lookup"><span data-stu-id="33c5f-517">Using functions that produce queries with constants.</span></span>
-   <span data-ttu-id="33c5f-518">Eşlenen olmayan bir nesne özelliklerini kullanma.</span><span class="sxs-lookup"><span data-stu-id="33c5f-518">Using the properties of a non-mapped object.</span></span>
-   <span data-ttu-id="33c5f-519">Sorgunuzu yeniden derlenmesi gerektiren başka bir sorgu bağlama.</span><span class="sxs-lookup"><span data-stu-id="33c5f-519">Linking your query to another query that requires to be recompiled.</span></span>

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a><span data-ttu-id="33c5f-520">4.1 IEnumerable kullanarak&lt;T&gt;. İçeren&lt;T&gt;(T değeri)</span><span class="sxs-lookup"><span data-stu-id="33c5f-520">4.1 Using IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value)</span></span>

<span data-ttu-id="33c5f-521">Entity Framework IEnumerable çağırma sorguları önbelleğe almaz&lt;T&gt;. İçeren&lt;T&gt;(T değeri) karşı değerleri koleksiyonun geçici olarak kabul edilir bu yana bir bellek içi koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-521">Entity Framework does not cache queries that invoke IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value) against an in-memory collection, since the values of the collection are considered volatile.</span></span> <span data-ttu-id="33c5f-522">Her zaman planı derleyici tarafından işlenmesi için aşağıdaki örnek sorguda, önbelleğe alınacak değil:</span><span class="sxs-lookup"><span data-stu-id="33c5f-522">The following example query will not be cached, so it will always be processed by the plan compiler:</span></span>

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var query = context.MyEntities
                    .Where(entity => ids.Contains(entity.Id));

    var results = query.ToList();
    ...
}
```

<span data-ttu-id="33c5f-523">Hangi içerir karşı IEnumerable boyutunu yürütülen ne kadar hızlı ya da nasıl yavaş sorgunuzu derlenir belirler unutmayın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-523">Note that the size of the IEnumerable against which Contains is executed determines how fast or how slow your query is compiled.</span></span> <span data-ttu-id="33c5f-524">Önemli ölçüde yukarıdaki örnekte gösterildiği gibi büyük koleksiyonlar kullanırken performans olumsuz etkilenebilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-524">Performance can suffer significantly when using large collections such as the one shown in the example above.</span></span>

<span data-ttu-id="33c5f-525">Entity Framework 6 içeren iyileştirmeleri IEnumerable şekilde&lt;T&gt;. İçeren&lt;T&gt;sorgu yürütüldüğünde (T değeri) çalışır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-525">Entity Framework 6 contains optimizations to the way IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value) works when queries are executed.</span></span> <span data-ttu-id="33c5f-526">Oluşturulan SQL kodu oluşturmak için çok daha hızlı ve daha okunabilir ve çoğu durumda, ayrıca daha hızlı sunucuda yürütür.</span><span class="sxs-lookup"><span data-stu-id="33c5f-526">The SQL code that is generated is much faster to produce and more readable, and in most cases it also executes faster in the server.</span></span>

### <a name="42-using-functions-that-produce-queries-with-constants"></a><span data-ttu-id="33c5f-527">4.2 sabitleri sorgularla üreten işlevlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="33c5f-527">4.2 Using functions that produce queries with constants</span></span>

<span data-ttu-id="33c5f-528">Skip(), Take() Contains() ve DefautIfEmpty() LINQ işleçleri parametreleri içeren SQL sorguları üretmez, ancak bunun yerine bunları sabitleri geçirilen değerleri koyun.</span><span class="sxs-lookup"><span data-stu-id="33c5f-528">The Skip(), Take(), Contains() and DefautIfEmpty() LINQ operators do not produce SQL queries with parameters but instead put the values passed to them as constants.</span></span> <span data-ttu-id="33c5f-529">Bu nedenle, aksi takdirde sorgu kirletmesini yukarı aynı uç olabilecek sorguları EF yığını ve veritabanı sunucusu önbellek, planlayın ve aynı sabit bir sonraki sorgu yürütme kullanılmadığı sürece reutilized değil.</span><span class="sxs-lookup"><span data-stu-id="33c5f-529">Because of this, queries that might otherwise be identical end up polluting the query plan cache, both on the EF stack and on the database server, and do not get reutilized unless the same constants are used in a subsequent query execution.</span></span> <span data-ttu-id="33c5f-530">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="33c5f-530">For example:</span></span>

``` csharp
var id = 10;
...
using (var context = new MyContext())
{
    var query = context.MyEntities.Select(entity => entity.Id).Contains(id);

    var results = query.ToList();
    ...
}
```

<span data-ttu-id="33c5f-531">Bu örnekte, bu sorgu kimliği sorgu için farklı bir değerle yürütülen her zaman yeni bir plan ile derlenir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-531">In this example, each time this query is executed with a different value for id the query will be compiled into a new plan.</span></span>

<span data-ttu-id="33c5f-532">Belirli dikkat edin atlama ve disk belleği yaparken Al.</span><span class="sxs-lookup"><span data-stu-id="33c5f-532">In particular pay attention to the use of Skip and Take when doing paging.</span></span> <span data-ttu-id="33c5f-533">EF6 çünkü EF bu yönteme geçirilen değişkenleri yakalamak ve bunları çevirmesine SQLparameters için önbelleğe alınmış sorgu planı etkili bir şekilde yeniden kullanılabilir hale getirir bir lambda aşırı bu yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-533">In EF6 these methods have a lambda overload that effectively makes the cached query plan reusable because EF can capture variables passed to these methods and translate them to SQLparameters.</span></span> <span data-ttu-id="33c5f-534">Bu da aksi takdirde, her sorgu atlama ve alma için farklı bir sabit ile kendi sorgu planı önbellek girişi elde edebileceğiniz bu yana önbellek temiz tutmaya yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-534">This also helps keep the cache cleaner since otherwise each query with a different constant for Skip and Take would get its own query plan cache entry.</span></span>

<span data-ttu-id="33c5f-535">Yetersiz ancak yalnızca bu sınıf sorgu exemplify yönelik aşağıdaki kodu düşünün:</span><span class="sxs-lookup"><span data-stu-id="33c5f-535">Consider the following code, which is suboptimal but is only meant to exemplify this class of queries:</span></span>

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

<span data-ttu-id="33c5f-536">Bir lambda ile Atla çağırma aynı bu kodu daha hızlı bir sürümünü içerir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-536">A faster version of this same code would involve calling Skip with a lambda:</span></span>

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

<span data-ttu-id="33c5f-537">Aynı sorgu planı kullanıldığından sorgu, CPU süresi kaydeder ve sorgu önbelleği kirletmesini önler her çalıştırıldığında, ikinci kod parçacığı %11 kadar daha hızlı çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-537">The second snippet may run up to 11% faster because the same query plan is used every time the query is run, which saves CPU time and avoids polluting the query cache.</span></span> <span data-ttu-id="33c5f-538">İçinde bir kapanış atla parametresi olduğu için Ayrıca, kodu de artık şuna benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-538">Furthermore, because the parameter to Skip is in a closure the code might as well look like this now:</span></span>

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a><span data-ttu-id="33c5f-539">4.3 eşlenen olmayan bir nesne özelliklerini kullanma</span><span class="sxs-lookup"><span data-stu-id="33c5f-539">4.3 Using the properties of a non-mapped object</span></span>

<span data-ttu-id="33c5f-540">Ne zaman bir parametre sonra sorgu önbelleğe şekilde sorgu olmayan eşlenmiş nesne türünün özelliklerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-540">When a query uses the properties of a non-mapped object type as a parameter then the query will not get cached.</span></span> <span data-ttu-id="33c5f-541">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="33c5f-541">For example:</span></span>

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();

    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myObject.MyProperty)
                select entity;

   var results = query.ToList();
    ...
}
```

<span data-ttu-id="33c5f-542">Bu örnekte, sınıf NonMappedType varlık modeli parçası olmadığını varsayar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-542">In this example, assume that class NonMappedType is not part of the Entity model.</span></span> <span data-ttu-id="33c5f-543">Bu sorgu, eşlenen olmayan bir tür değil ve bunun yerine sorgu parametresi olarak bir yerel değişken kullanmak kolayca değiştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-543">This query can easily be changed to not use a non-mapped type and instead use a local variable as the parameter to the query:</span></span>

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();
    var myValue = myObject.MyProperty;
    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myValue)
                select entity;

    var results = query.ToList();
    ...
}
```

<span data-ttu-id="33c5f-544">Bu durumda, sorgu önbelleğe mümkün olacaktır ve sorgu planı önbellekten Kurum için avantaj sağlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-544">In this case, the query will be able to get cached and will benefit from the query plan cache.</span></span>

### <a name="44-linking-to-queries-that-require-recompiling"></a><span data-ttu-id="33c5f-545">4.4 yeniden derlemeden gerektiren sorguları bağlama</span><span class="sxs-lookup"><span data-stu-id="33c5f-545">4.4 Linking to queries that require recompiling</span></span>

<span data-ttu-id="33c5f-546">Derlenmesi gereken bir sorguya dayalı ikinci bir sorgu varsa yukarıdakilerle aynı örneği izleyerek, ayrıca tüm ikinci sorgunuzu derlenecek.</span><span class="sxs-lookup"><span data-stu-id="33c5f-546">Following the same example as above, if you have a second query that relies on a query that needs to be recompiled, your entire second query will also be recompiled.</span></span> <span data-ttu-id="33c5f-547">Bu senaryoyu göstermek üzere bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-547">Here’s an example to illustrate this scenario:</span></span>

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var firstQuery = from entity in context.MyEntities
                        where ids.Contains(entity.Id)
                        select entity;

    var secondQuery = from entity in context.MyEntities
                        where firstQuery.Any(otherEntity => otherEntity.Id == entity.Id)
                        select entity;

    var results = secondQuery.ToList();
    ...
}
```

<span data-ttu-id="33c5f-548">Örnek geneldir ancak nasıl, firstQuery için bağlama secondQuery önbelleğe başlatılamamasına neden olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-548">The example is generic, but it illustrates how linking to firstQuery is causing secondQuery to be unable to get cached.</span></span> <span data-ttu-id="33c5f-549">FirstQuery yeniden derlemeden gerektiren bir sorgu değil olsaydı, ardından secondQuery önbelleğe alınan.</span><span class="sxs-lookup"><span data-stu-id="33c5f-549">If firstQuery had not been a query that requires recompiling, then secondQuery would have been cached.</span></span>

## <a name="5-notracking-queries"></a><span data-ttu-id="33c5f-550">5 NoTracking sorguları</span><span class="sxs-lookup"><span data-stu-id="33c5f-550">5 NoTracking Queries</span></span>

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a><span data-ttu-id="33c5f-551">5.1 Durum Yönetim yükünü azaltmak için değişiklik devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="33c5f-551">5.1 Disabling change tracking to reduce state management overhead</span></span>

<span data-ttu-id="33c5f-552">Salt okunur bir senaryoda ve yüklenen nesneler Objectstatemanager'da ek yükü ortadan kaldırmak istiyorsanız, "No izleme" sorgu iletebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-552">If you are in a read-only scenario and want to avoid the overhead of loading the objects into the ObjectStateManager, you can issue "No Tracking" queries.</span></span><span data-ttu-id="33c5f-553">  Değişiklik izleme sorgu düzeyinde devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-553">  Change tracking can be disabled at the query level.</span></span>

<span data-ttu-id="33c5f-554">Ancak, değişiklik, izleme devre dışı bırakarak etkin nesne önbelleği devre dışı açıyorsunuz olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-554">Note though that by disabling change tracking you are effectively turning off the object cache.</span></span> <span data-ttu-id="33c5f-555">Bir varlık için sorguladığınızda, biz ObjectStateManager daha önce gerçekleştirilmiş sorgu sonuçları çekerek materialization atlayamazsınız.</span><span class="sxs-lookup"><span data-stu-id="33c5f-555">When you query for an entity, we can't skip materialization by pulling the previously-materialized query results from the ObjectStateManager.</span></span> <span data-ttu-id="33c5f-556">Tekrar tekrar aynı içerik üzerinde aynı varlıklar için sorgu oluşturuyorsanız, değişiklik izleme kaldırmadan yararlanabilecek bir performans gerçekten görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-556">If you are repeatedly querying for the same entities on the same context, you might actually see a performance benefit from enabling change tracking.</span></span>

<span data-ttu-id="33c5f-557">ObjectContext kullanarak sorgulanırken ObjectQuery ve ObjectSet örnekleri ayarlanır ve bunlar üzerinde oluşan sorguları ana sorgunun etkili MergeOption devralır sonra bir MergeOption hatırlanır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-557">When querying using ObjectContext, ObjectQuery and ObjectSet instances will remember a MergeOption once it is set, and queries that are composed on them will inherit the effective MergeOption of the parent query.</span></span> <span data-ttu-id="33c5f-558">DbContext kullanırken, izleme üzerinde olan DB AsNoTracking() değiştiricisi çağırarak devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-558">When using DbContext, tracking can be disabled by calling the AsNoTracking() modifier on the DbSet.</span></span>

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a><span data-ttu-id="33c5f-559">5.1.1 değişiklik DbContext kullanırken bir sorgu için izlemeyi devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="33c5f-559">5.1.1 Disabling change tracking for a query when using DbContext</span></span>

<span data-ttu-id="33c5f-560">Sorgudaki AsNoTracking() yönteme bir çağrı zinciri tarafından bir sorgu modunu NoTracking için geçiş yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-560">You can switch the mode of a query to NoTracking by chaining a call to the AsNoTracking() method in the query.</span></span> <span data-ttu-id="33c5f-561">ObjectQuery DbContext API olan DB ve DbQuery sınıfları için MergeOption değişebilir bir özellik yok.</span><span class="sxs-lookup"><span data-stu-id="33c5f-561">Unlike ObjectQuery, the DbSet and DbQuery classes in the DbContext API don’t have a mutable property for the MergeOption.</span></span>

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a><span data-ttu-id="33c5f-562">5.1.2 değişiklik izleme ObjectContext kullanarak sorgu düzeyinde devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="33c5f-562">5.1.2 Disabling change tracking at the query level using ObjectContext</span></span>

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a><span data-ttu-id="33c5f-563">5.1.3 değişiklik izleme ObjectContext kullanarak tüm varlık için devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="33c5f-563">5.1.3 Disabling change tracking for an entire entity set using ObjectContext</span></span>

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a><span data-ttu-id="33c5f-564">5.2 NoTracking sorguların performans avantajı gösteren test ölçümleri</span><span class="sxs-lookup"><span data-stu-id="33c5f-564">5.2 Test Metrics demonstrating the performance benefit of NoTracking queries</span></span>

<span data-ttu-id="33c5f-565">Bu sınamada Navision modelin NoTracking sorguları izleme karşılaştırarak ObjectStateManager doldurma karşılığında bakacağız.</span><span class="sxs-lookup"><span data-stu-id="33c5f-565">In this test we look at the cost of filling the ObjectStateManager by comparing Tracking to NoTracking queries for the Navision model.</span></span> <span data-ttu-id="33c5f-566">Ek Navision modeli ve yürütüldü sorguları türde bir açıklaması için bkz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-566">See the appendix for a description of the Navision model and the types of queries which were executed.</span></span> <span data-ttu-id="33c5f-567">Bu test sorguları listesi boyunca yineleme yapmak ve her birini bir kere yürütülen.</span><span class="sxs-lookup"><span data-stu-id="33c5f-567">In this test, we iterate through the list of queries and execute each one once.</span></span> <span data-ttu-id="33c5f-568">Test, bir kez NoTracking sorgularla de iki çeşidi "AppendOnly" varsayılan birleştirme seçeneği ile karşılaştık.</span><span class="sxs-lookup"><span data-stu-id="33c5f-568">We ran two variations of the test, once with NoTracking queries and once with the default merge option of "AppendOnly".</span></span> <span data-ttu-id="33c5f-569">Biz, her 3 kez çalıştırıldı ve çalıştırmalar ortalama değerini alın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-569">We ran each variation 3 times and take the mean value of the runs.</span></span> <span data-ttu-id="33c5f-570">Testleri arasında SQL Server'da sorgu önbelleği temizlemek ve aşağıdaki komutları çalıştırarak tempdb Daralt:</span><span class="sxs-lookup"><span data-stu-id="33c5f-570">Between the tests we clear the query cache on the SQL Server and shrink the tempdb by running the following commands:</span></span>

1.  <span data-ttu-id="33c5f-571">DBCC DROPCLEANBUFFERS</span><span class="sxs-lookup"><span data-stu-id="33c5f-571">DBCC DROPCLEANBUFFERS</span></span>
2.  <span data-ttu-id="33c5f-572">DBCC FREEPROCCACHE</span><span class="sxs-lookup"><span data-stu-id="33c5f-572">DBCC FREEPROCCACHE</span></span>
3.  <span data-ttu-id="33c5f-573">DBCC SHRINKDATABASE (tempdb, 0)</span><span class="sxs-lookup"><span data-stu-id="33c5f-573">DBCC SHRINKDATABASE (tempdb, 0)</span></span>

<span data-ttu-id="33c5f-574">Test sonuçları, ORTANCA 3 çalışır:</span><span class="sxs-lookup"><span data-stu-id="33c5f-574">Test Results, median over 3 runs:</span></span>

|                        | <span data-ttu-id="33c5f-575">İZLEME – ÇALIŞMA KÜMESİ</span><span class="sxs-lookup"><span data-stu-id="33c5f-575">NO TRACKING – WORKING SET</span></span> | <span data-ttu-id="33c5f-576">İZLEME YOK-ZAMAN</span><span class="sxs-lookup"><span data-stu-id="33c5f-576">NO TRACKING – TIME</span></span> | <span data-ttu-id="33c5f-577">ÇALIŞMA KÜMESİ YALNIZCA – EKLEME</span><span class="sxs-lookup"><span data-stu-id="33c5f-577">APPEND ONLY – WORKING SET</span></span> | <span data-ttu-id="33c5f-578">YALNIZCA – ZAMAN EKLE</span><span class="sxs-lookup"><span data-stu-id="33c5f-578">APPEND ONLY – TIME</span></span> |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| <span data-ttu-id="33c5f-579">**Entity Framework 5**</span><span class="sxs-lookup"><span data-stu-id="33c5f-579">**Entity Framework 5**</span></span> | <span data-ttu-id="33c5f-580">460361728</span><span class="sxs-lookup"><span data-stu-id="33c5f-580">460361728</span></span>                 | <span data-ttu-id="33c5f-581">1163536 ms</span><span class="sxs-lookup"><span data-stu-id="33c5f-581">1163536 ms</span></span>         | <span data-ttu-id="33c5f-582">596545536</span><span class="sxs-lookup"><span data-stu-id="33c5f-582">596545536</span></span>                 | <span data-ttu-id="33c5f-583">1273042 ms</span><span class="sxs-lookup"><span data-stu-id="33c5f-583">1273042 ms</span></span>         |
| <span data-ttu-id="33c5f-584">**Entity Framework 6**</span><span class="sxs-lookup"><span data-stu-id="33c5f-584">**Entity Framework 6**</span></span> | <span data-ttu-id="33c5f-585">647127040</span><span class="sxs-lookup"><span data-stu-id="33c5f-585">647127040</span></span>                 | <span data-ttu-id="33c5f-586">190228 ms</span><span class="sxs-lookup"><span data-stu-id="33c5f-586">190228 ms</span></span>          | <span data-ttu-id="33c5f-587">832798720</span><span class="sxs-lookup"><span data-stu-id="33c5f-587">832798720</span></span>                 | <span data-ttu-id="33c5f-588">195521 ms</span><span class="sxs-lookup"><span data-stu-id="33c5f-588">195521 ms</span></span>          |

<span data-ttu-id="33c5f-589">Entity Framework 5, Entity Framework 6 daha bir daha küçük bellek Ayak izi çalıştırma sonunda sahip olur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-589">Entity Framework 5 will have a smaller memory footprint at the end of the run than Entity Framework 6 does.</span></span> <span data-ttu-id="33c5f-590">Entity Framework 6 tarafından kullanılan ek bellek ek bellek yapılar ve yeni özellikleri ve daha iyi performans sağlayan kod sonucudur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-590">The additional memory consumed by Entity Framework 6 is the result of additional memory structures and code that enable new features and better performance.</span></span>

<span data-ttu-id="33c5f-591">Aynı zamanda ObjectStateManager kullanırken aynı zamanda bellek Ayak izi NET bir fark yoktur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-591">There’s also a clear difference in memory footprint when using the ObjectStateManager.</span></span> <span data-ttu-id="33c5f-592">Entity Framework 5, Ayak izi biz veritabanından gerçekleştirilmiş tüm varlıkları izler, 30 oranında artırıldı.</span><span class="sxs-lookup"><span data-stu-id="33c5f-592">Entity Framework 5 increased its footprint by 30% when keeping track of all the entities we materialized from the database.</span></span> <span data-ttu-id="33c5f-593">Entity Framework 6 yapıldığında, Ayak izi % 28'artırdık.</span><span class="sxs-lookup"><span data-stu-id="33c5f-593">Entity Framework 6 increased its footprint by 28% when doing so.</span></span>

<span data-ttu-id="33c5f-594">Zaman açısından, Entity Framework 6 Entity Framework 5 bu test büyük bir kenar boşluğu ile çok daha iyi.</span><span class="sxs-lookup"><span data-stu-id="33c5f-594">In terms of time, Entity Framework 6 outperforms Entity Framework 5 in this test by a large margin.</span></span> <span data-ttu-id="33c5f-595">Entity Framework 6 yaklaşık %16 Entity Framework 5 tarafından tüketilen sürenin test tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="33c5f-595">Entity Framework 6 completed the test in roughly 16% of the time consumed by Entity Framework 5.</span></span> <span data-ttu-id="33c5f-596">Ayrıca, Entity Framework 5 ObjectStateManager kullanıldığında tamamlamak için %9 daha fazla zaman alır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-596">Additionally, Entity Framework 5 takes 9% more time to complete when the ObjectStateManager is being used.</span></span> <span data-ttu-id="33c5f-597">Buna karşılık, Entity Framework 6 %3 ObjectStateManager kullanırken daha fazla zaman kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="33c5f-597">In comparison, Entity Framework 6 is using 3% more time when using the ObjectStateManager.</span></span>

## <a name="6-query-execution-options"></a><span data-ttu-id="33c5f-598">6 sorgu yürütme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="33c5f-598">6 Query Execution Options</span></span>

<span data-ttu-id="33c5f-599">Entity Framework, sorgu için çeşitli yollar sunar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-599">Entity Framework offers several different ways to query.</span></span> <span data-ttu-id="33c5f-600">Biz aşağıdaki seçenekleri göz atın, Artıları ve eksileri her karşılaştırın ve performans özelliklerini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="33c5f-600">We'll take a look at the following options, compare the pros and cons of each, and examine their performance characteristics:</span></span>

-   <span data-ttu-id="33c5f-601">LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="33c5f-601">LINQ to Entities.</span></span>
-   <span data-ttu-id="33c5f-602">İzleme yok LINQ to Entities'de.</span><span class="sxs-lookup"><span data-stu-id="33c5f-602">No Tracking LINQ to Entities.</span></span>
-   <span data-ttu-id="33c5f-603">Bir ObjectQuery üzerinden SQL varlık.</span><span class="sxs-lookup"><span data-stu-id="33c5f-603">Entity SQL over an ObjectQuery.</span></span>
-   <span data-ttu-id="33c5f-604">Varlık bir EntityCommand üzerinden SQL.</span><span class="sxs-lookup"><span data-stu-id="33c5f-604">Entity SQL over an EntityCommand.</span></span>
-   <span data-ttu-id="33c5f-605">ExecuteStoreQuery.</span><span class="sxs-lookup"><span data-stu-id="33c5f-605">ExecuteStoreQuery.</span></span>
-   <span data-ttu-id="33c5f-606">SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="33c5f-606">SqlQuery.</span></span>
-   <span data-ttu-id="33c5f-607">CompiledQuery.</span><span class="sxs-lookup"><span data-stu-id="33c5f-607">CompiledQuery.</span></span>

### <a name="61-linq-to-entities-queries"></a><span data-ttu-id="33c5f-608">6.1 LINQ to Entities sorgularında</span><span class="sxs-lookup"><span data-stu-id="33c5f-608">6.1       LINQ to Entities queries</span></span>

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="33c5f-609">**Uzmanları**</span><span class="sxs-lookup"><span data-stu-id="33c5f-609">**Pros**</span></span>

-   <span data-ttu-id="33c5f-610">CUD işlemleri için uygundur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-610">Suitable for CUD operations.</span></span>
-   <span data-ttu-id="33c5f-611">Tam olarak gerçekleştirilmiş nesneleri.</span><span class="sxs-lookup"><span data-stu-id="33c5f-611">Fully materialized objects.</span></span>
-   <span data-ttu-id="33c5f-612">En basit söz dizimi ile yazmak programlama dilinde yerleşik.</span><span class="sxs-lookup"><span data-stu-id="33c5f-612">Simplest to write with syntax built into the programming language.</span></span>
-   <span data-ttu-id="33c5f-613">İyi bir performans.</span><span class="sxs-lookup"><span data-stu-id="33c5f-613">Good performance.</span></span>

<span data-ttu-id="33c5f-614">**Simgeler**</span><span class="sxs-lookup"><span data-stu-id="33c5f-614">**Cons**</span></span>

-   <span data-ttu-id="33c5f-615">Bazı teknik kısıtlamalar gibi:</span><span class="sxs-lookup"><span data-stu-id="33c5f-615">Certain technical restrictions, such as:</span></span>
    -   <span data-ttu-id="33c5f-616">OUTER JOIN varlık SQL deyimlerinde basit değerinden daha karmaşık sorgular için sorguların OUTER JOIN DefaultIfEmpty kullanarak desenlerini sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-616">Patterns using DefaultIfEmpty for OUTER JOIN queries result in more complex queries than simple OUTER JOIN statements in Entity SQL.</span></span>
    -   <span data-ttu-id="33c5f-617">Hala kullanamazsınız genel desen eşleştirme ile benzer.</span><span class="sxs-lookup"><span data-stu-id="33c5f-617">You still can’t use LIKE with general pattern matching.</span></span>

### <a name="62-no-tracking-linq-to-entities-queries"></a><span data-ttu-id="33c5f-618">6.2 hiçbir ' % s'izleme LINQ to Entities sorgularında</span><span class="sxs-lookup"><span data-stu-id="33c5f-618">6.2       No Tracking LINQ to Entities queries</span></span>

<span data-ttu-id="33c5f-619">Ne zaman bağlamı ObjectContext türetilir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-619">When the context derives ObjectContext:</span></span>

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="33c5f-620">Ne zaman bağlamı DbContext türetilir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-620">When the context derives DbContext:</span></span>

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="33c5f-621">**Uzmanları**</span><span class="sxs-lookup"><span data-stu-id="33c5f-621">**Pros**</span></span>

-   <span data-ttu-id="33c5f-622">Normal LINQ sorguları içinde performansı İyileştirildi.</span><span class="sxs-lookup"><span data-stu-id="33c5f-622">Improved performance over regular LINQ queries.</span></span>
-   <span data-ttu-id="33c5f-623">Tam olarak gerçekleştirilmiş nesneleri.</span><span class="sxs-lookup"><span data-stu-id="33c5f-623">Fully materialized objects.</span></span>
-   <span data-ttu-id="33c5f-624">En basit söz dizimi ile yazmak programlama dilinde yerleşik.</span><span class="sxs-lookup"><span data-stu-id="33c5f-624">Simplest to write with syntax built into the programming language.</span></span>

<span data-ttu-id="33c5f-625">**Simgeler**</span><span class="sxs-lookup"><span data-stu-id="33c5f-625">**Cons**</span></span>

-   <span data-ttu-id="33c5f-626">CUD işlemleri için uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-626">Not suitable for CUD operations.</span></span>
-   <span data-ttu-id="33c5f-627">Bazı teknik kısıtlamalar gibi:</span><span class="sxs-lookup"><span data-stu-id="33c5f-627">Certain technical restrictions, such as:</span></span>
    -   <span data-ttu-id="33c5f-628">OUTER JOIN varlık SQL deyimlerinde basit değerinden daha karmaşık sorgular için sorguların OUTER JOIN DefaultIfEmpty kullanarak desenlerini sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-628">Patterns using DefaultIfEmpty for OUTER JOIN queries result in more complex queries than simple OUTER JOIN statements in Entity SQL.</span></span>
    -   <span data-ttu-id="33c5f-629">Hala kullanamazsınız genel desen eşleştirme ile benzer.</span><span class="sxs-lookup"><span data-stu-id="33c5f-629">You still can’t use LIKE with general pattern matching.</span></span>

<span data-ttu-id="33c5f-630">Proje skaler özellikleri sorguları NoTracking belirtilmemiş olsa bile izlenmez unutmayın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-630">Note that queries that project scalar properties are not tracked even if the NoTracking is not specified.</span></span> <span data-ttu-id="33c5f-631">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="33c5f-631">For example:</span></span>

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

<span data-ttu-id="33c5f-632">Bu belirli bir sorgu NoTracking olan açıkça belirtmeyen ancak değil düzeniyle beri gerçekleştirilmiş sonuç nesnesi durum Yöneticisi ardından bilinen tür değil izlenir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-632">This particular query doesn’t explicitly specify being NoTracking, but since it’s not materializing a type that’s known to the object state manager then the materialized result is not tracked.</span></span>

### <a name="63-entity-sql-over-an-objectquery"></a><span data-ttu-id="33c5f-633">6.3 varlık ObjectQuery üzerinden SQL</span><span class="sxs-lookup"><span data-stu-id="33c5f-633">6.3       Entity SQL over an ObjectQuery</span></span>

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

<span data-ttu-id="33c5f-634">**Uzmanları**</span><span class="sxs-lookup"><span data-stu-id="33c5f-634">**Pros**</span></span>

-   <span data-ttu-id="33c5f-635">CUD işlemleri için uygundur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-635">Suitable for CUD operations.</span></span>
-   <span data-ttu-id="33c5f-636">Tam olarak gerçekleştirilmiş nesneleri.</span><span class="sxs-lookup"><span data-stu-id="33c5f-636">Fully materialized objects.</span></span>
-   <span data-ttu-id="33c5f-637">Destekler planını önbelleğe alma sorgulayın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-637">Supports query plan caching.</span></span>

<span data-ttu-id="33c5f-638">**Simgeler**</span><span class="sxs-lookup"><span data-stu-id="33c5f-638">**Cons**</span></span>

-   <span data-ttu-id="33c5f-639">Kullanıcı daha fazla hataya daha sorgu yapıları dilinde yerleşik olan metinsel sorgu dizelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-639">Involves textual query strings which are more prone to user error than query constructs built into the language.</span></span>

### <a name="64-entity-sql-over-an-entity-command"></a><span data-ttu-id="33c5f-640">6.4 varlık varlığın komut üzerinden SQL</span><span class="sxs-lookup"><span data-stu-id="33c5f-640">6.4       Entity SQL over an Entity Command</span></span>

``` csharp
EntityCommand cmd = eConn.CreateCommand();
cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
{
    while (reader.Read())
    {
        // manually 'materialize' the product
    }
}
```

<span data-ttu-id="33c5f-641">**Uzmanları**</span><span class="sxs-lookup"><span data-stu-id="33c5f-641">**Pros**</span></span>

-   <span data-ttu-id="33c5f-642">Plan .NET 4.0 (planını önbelleğe alma .NET 4.5 diğer tüm sorgu türleri tarafından desteklenir) önbelleğe almayı destekler sorgulayın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-642">Supports query plan caching in .NET 4.0 (plan caching is supported by all other query types in .NET 4.5).</span></span>

<span data-ttu-id="33c5f-643">**Simgeler**</span><span class="sxs-lookup"><span data-stu-id="33c5f-643">**Cons**</span></span>

-   <span data-ttu-id="33c5f-644">Kullanıcı daha fazla hataya daha sorgu yapıları dilinde yerleşik olan metinsel sorgu dizelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-644">Involves textual query strings which are more prone to user error than query constructs built into the language.</span></span>
-   <span data-ttu-id="33c5f-645">CUD işlemleri için uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-645">Not suitable for CUD operations.</span></span>
-   <span data-ttu-id="33c5f-646">Sonuçları değil otomatik olarak gerçekleştirilmiş ve veri okuyucusundan okunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-646">Results are not automatically materialized, and must be read from the data reader.</span></span>

### <a name="65-sqlquery-and-executestorequery"></a><span data-ttu-id="33c5f-647">6.5 SqlQuery ve ExecuteStoreQuery</span><span class="sxs-lookup"><span data-stu-id="33c5f-647">6.5       SqlQuery and ExecuteStoreQuery</span></span>

<span data-ttu-id="33c5f-648">Veritabanında SqlQuery:</span><span class="sxs-lookup"><span data-stu-id="33c5f-648">SqlQuery on Database:</span></span>

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

<span data-ttu-id="33c5f-649">SqlQuery on DbSet:</span><span class="sxs-lookup"><span data-stu-id="33c5f-649">SqlQuery on DbSet:</span></span>

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

<span data-ttu-id="33c5f-650">ExecyteStoreQuery:</span><span class="sxs-lookup"><span data-stu-id="33c5f-650">ExecyteStoreQuery:</span></span>

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

<span data-ttu-id="33c5f-651">**Uzmanları**</span><span class="sxs-lookup"><span data-stu-id="33c5f-651">**Pros**</span></span>

-   <span data-ttu-id="33c5f-652">Plan derleyici atlanır beri genellikle en hızlı performansı.</span><span class="sxs-lookup"><span data-stu-id="33c5f-652">Generally fastest performance since plan compiler is bypassed.</span></span>
-   <span data-ttu-id="33c5f-653">Tam olarak gerçekleştirilmiş nesneleri.</span><span class="sxs-lookup"><span data-stu-id="33c5f-653">Fully materialized objects.</span></span>
-   <span data-ttu-id="33c5f-654">Olan DB kullanıldığında CUD işlemleri için uygundur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-654">Suitable for CUD operations when used from the DbSet.</span></span>

<span data-ttu-id="33c5f-655">**Simgeler**</span><span class="sxs-lookup"><span data-stu-id="33c5f-655">**Cons**</span></span>

-   <span data-ttu-id="33c5f-656">Sorgu metinsel ve hataya açık alanlardır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-656">Query is textual and error prone.</span></span>
-   <span data-ttu-id="33c5f-657">Sorgu deposu semantiği yerine kavramsal semantiği kullanarak belirli bir arka uca bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-657">Query is tied to a specific backend by using store semantics instead of conceptual semantics.</span></span>
-   <span data-ttu-id="33c5f-658">Devralma mevcut olduğunda, sorgu hale talep türü için eşleme koşulları için hesap gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="33c5f-658">When inheritance is present, handcrafted query needs to account for mapping conditions for the type requested.</span></span>

### <a name="66-compiledquery"></a><span data-ttu-id="33c5f-659">6.6 CompiledQuery</span><span class="sxs-lookup"><span data-stu-id="33c5f-659">6.6       CompiledQuery</span></span>

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

<span data-ttu-id="33c5f-660">**Uzmanları**</span><span class="sxs-lookup"><span data-stu-id="33c5f-660">**Pros**</span></span>

-   <span data-ttu-id="33c5f-661">En fazla % 7 performans iyileştirmesi normal LINQ sorguları sağlar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-661">Provides up to a 7% performance improvement over regular LINQ queries.</span></span>
-   <span data-ttu-id="33c5f-662">Tam olarak gerçekleştirilmiş nesneleri.</span><span class="sxs-lookup"><span data-stu-id="33c5f-662">Fully materialized objects.</span></span>
-   <span data-ttu-id="33c5f-663">CUD işlemleri için uygundur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-663">Suitable for CUD operations.</span></span>

<span data-ttu-id="33c5f-664">**Simgeler**</span><span class="sxs-lookup"><span data-stu-id="33c5f-664">**Cons**</span></span>

-   <span data-ttu-id="33c5f-665">Karmaşıklık ve ek yükü programlama artırdık.</span><span class="sxs-lookup"><span data-stu-id="33c5f-665">Increased complexity and programming overhead.</span></span>
-   <span data-ttu-id="33c5f-666">Performans iyileştirmesi üzerinde derlenmiş bir sorgu oluştururken kaybolur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-666">The performance improvement is lost when composing on top of a compiled query.</span></span>
-   <span data-ttu-id="33c5f-667">Bazı LINQ sorguları bir CompiledQuery - Örneğin, anonim tür projeksiyonları yazılamaz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-667">Some LINQ queries can't be written as a CompiledQuery - for example, projections of anonymous types.</span></span>

### <a name="67-performance-comparison-of-different-query-options"></a><span data-ttu-id="33c5f-668">6.7 farklı bir sorgu seçenekleri performans karşılaştırması</span><span class="sxs-lookup"><span data-stu-id="33c5f-668">6.7       Performance Comparison of different query options</span></span>

<span data-ttu-id="33c5f-669">Burada içerik oluşturma değil uğradı basit microbenchmarks test yerleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-669">Simple microbenchmarks where the context creation was not timed were put to the test.</span></span> <span data-ttu-id="33c5f-670">Size bir dizi önbelleğe alınmamış varlık denetimli bir ortamda 5000 kez sorgulama ölçülür.</span><span class="sxs-lookup"><span data-stu-id="33c5f-670">We measured querying 5000 times for a set of non-cached entities in a controlled environment.</span></span> <span data-ttu-id="33c5f-671">Bu uyarı ile gerçekleştirilecek sayılardır: bir uygulama tarafından üretilen gerçek sayılar yansıtmaz, ancak bunun yerine bir performans farkı sorgulanırken farklı seçenekler karşılaştırıldığında yoktur ne kadar çok doğru ölçümü olur elma-için-yeni bir bağlam maliyetini hariç elma.</span><span class="sxs-lookup"><span data-stu-id="33c5f-671">These numbers are to be taken with a warning: they do not reflect actual numbers produced by an application, but instead they are a very accurate measurement of how much of a performance difference there is when different querying options are compared apples-to-apples, excluding the cost of creating a new context.</span></span>

| <span data-ttu-id="33c5f-672">EF</span><span class="sxs-lookup"><span data-stu-id="33c5f-672">EF</span></span>  | <span data-ttu-id="33c5f-673">Test</span><span class="sxs-lookup"><span data-stu-id="33c5f-673">Test</span></span>                                 | <span data-ttu-id="33c5f-674">Süre (ms)</span><span class="sxs-lookup"><span data-stu-id="33c5f-674">Time (ms)</span></span> | <span data-ttu-id="33c5f-675">Bellek</span><span class="sxs-lookup"><span data-stu-id="33c5f-675">Memory</span></span>   |
|:----|:-------------------------------------|:----------|:---------|
| <span data-ttu-id="33c5f-676">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-676">EF5</span></span> | <span data-ttu-id="33c5f-677">ObjectContext ESQL</span><span class="sxs-lookup"><span data-stu-id="33c5f-677">ObjectContext ESQL</span></span>                   | <span data-ttu-id="33c5f-678">2414</span><span class="sxs-lookup"><span data-stu-id="33c5f-678">2414</span></span>      | <span data-ttu-id="33c5f-679">38801408</span><span class="sxs-lookup"><span data-stu-id="33c5f-679">38801408</span></span> |
| <span data-ttu-id="33c5f-680">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-680">EF5</span></span> | <span data-ttu-id="33c5f-681">ObjectContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="33c5f-681">ObjectContext Linq Query</span></span>             | <span data-ttu-id="33c5f-682">2692</span><span class="sxs-lookup"><span data-stu-id="33c5f-682">2692</span></span>      | <span data-ttu-id="33c5f-683">38277120</span><span class="sxs-lookup"><span data-stu-id="33c5f-683">38277120</span></span> |
| <span data-ttu-id="33c5f-684">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-684">EF5</span></span> | <span data-ttu-id="33c5f-685">DbContext LINQ Sorgu izleme yok</span><span class="sxs-lookup"><span data-stu-id="33c5f-685">DbContext Linq Query No Tracking</span></span>     | <span data-ttu-id="33c5f-686">2818</span><span class="sxs-lookup"><span data-stu-id="33c5f-686">2818</span></span>      | <span data-ttu-id="33c5f-687">41840640</span><span class="sxs-lookup"><span data-stu-id="33c5f-687">41840640</span></span> |
| <span data-ttu-id="33c5f-688">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-688">EF5</span></span> | <span data-ttu-id="33c5f-689">DbContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="33c5f-689">DbContext Linq Query</span></span>                 | <span data-ttu-id="33c5f-690">2930</span><span class="sxs-lookup"><span data-stu-id="33c5f-690">2930</span></span>      | <span data-ttu-id="33c5f-691">41771008</span><span class="sxs-lookup"><span data-stu-id="33c5f-691">41771008</span></span> |
| <span data-ttu-id="33c5f-692">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-692">EF5</span></span> | <span data-ttu-id="33c5f-693">ObjectContext LINQ Sorgu izleme yok</span><span class="sxs-lookup"><span data-stu-id="33c5f-693">ObjectContext Linq Query No Tracking</span></span> | <span data-ttu-id="33c5f-694">3013</span><span class="sxs-lookup"><span data-stu-id="33c5f-694">3013</span></span>      | <span data-ttu-id="33c5f-695">38412288</span><span class="sxs-lookup"><span data-stu-id="33c5f-695">38412288</span></span> |
|     |                                      |           |          |
| <span data-ttu-id="33c5f-696">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-696">EF6</span></span> | <span data-ttu-id="33c5f-697">ObjectContext ESQL</span><span class="sxs-lookup"><span data-stu-id="33c5f-697">ObjectContext ESQL</span></span>                   | <span data-ttu-id="33c5f-698">2059</span><span class="sxs-lookup"><span data-stu-id="33c5f-698">2059</span></span>      | <span data-ttu-id="33c5f-699">46039040</span><span class="sxs-lookup"><span data-stu-id="33c5f-699">46039040</span></span> |
| <span data-ttu-id="33c5f-700">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-700">EF6</span></span> | <span data-ttu-id="33c5f-701">ObjectContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="33c5f-701">ObjectContext Linq Query</span></span>             | <span data-ttu-id="33c5f-702">3074</span><span class="sxs-lookup"><span data-stu-id="33c5f-702">3074</span></span>      | <span data-ttu-id="33c5f-703">45248512</span><span class="sxs-lookup"><span data-stu-id="33c5f-703">45248512</span></span> |
| <span data-ttu-id="33c5f-704">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-704">EF6</span></span> | <span data-ttu-id="33c5f-705">DbContext LINQ Sorgu izleme yok</span><span class="sxs-lookup"><span data-stu-id="33c5f-705">DbContext Linq Query No Tracking</span></span>     | <span data-ttu-id="33c5f-706">3125</span><span class="sxs-lookup"><span data-stu-id="33c5f-706">3125</span></span>      | <span data-ttu-id="33c5f-707">47575040</span><span class="sxs-lookup"><span data-stu-id="33c5f-707">47575040</span></span> |
| <span data-ttu-id="33c5f-708">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-708">EF6</span></span> | <span data-ttu-id="33c5f-709">DbContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="33c5f-709">DbContext Linq Query</span></span>                 | <span data-ttu-id="33c5f-710">3420</span><span class="sxs-lookup"><span data-stu-id="33c5f-710">3420</span></span>      | <span data-ttu-id="33c5f-711">47652864</span><span class="sxs-lookup"><span data-stu-id="33c5f-711">47652864</span></span> |
| <span data-ttu-id="33c5f-712">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-712">EF6</span></span> | <span data-ttu-id="33c5f-713">ObjectContext LINQ Sorgu izleme yok</span><span class="sxs-lookup"><span data-stu-id="33c5f-713">ObjectContext Linq Query No Tracking</span></span> | <span data-ttu-id="33c5f-714">3593</span><span class="sxs-lookup"><span data-stu-id="33c5f-714">3593</span></span>      | <span data-ttu-id="33c5f-715">45260800</span><span class="sxs-lookup"><span data-stu-id="33c5f-715">45260800</span></span> |

![EF5 mikro kıyaslamaları, 5000 sıcak yinelemeler](~/ef6/media/ef5micro5000warm.png)

![EF6 mikro kıyaslamaları, 5000 sıcak yinelemeler](~/ef6/media/ef6micro5000warm.png)

<span data-ttu-id="33c5f-718">Microbenchmarks çok kodu küçük değişikliklere duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-718">Microbenchmarks are very sensitive to small changes in the code.</span></span> <span data-ttu-id="33c5f-719">Bu durumda, Entity Framework 5 maliyetlerini ve Entity Framework 6 arasındaki farkı olan eklenmesi nedeniyle [durdurma](~/ef6/fundamentals/logging-and-interception.md) ve [işlem geliştirmeleri](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="33c5f-719">In this case, the difference between the costs of Entity Framework 5 and Entity Framework 6 are due to the addition of [interception](~/ef6/fundamentals/logging-and-interception.md) and [transactional improvements](~/ef6/saving/transactions.md).</span></span> <span data-ttu-id="33c5f-720">Bu microbenchmarks numaraları, ancak, yükseltilmiş bir işleme Entity Framework yapar, çok küçük bir parça halinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-720">These microbenchmarks numbers, however, are an amplified vision into a very small fragment of what Entity Framework does.</span></span> <span data-ttu-id="33c5f-721">Orta Gecikmeli sorgular, gerçek hayat senaryolarında, Entity Framework 6 için Entity Framework 5'ten yükseltme yaparken performans regresyon açmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-721">Real-world scenarios of warm queries should not see a performance regression when upgrading from Entity Framework 5 to Entity Framework 6.</span></span>

<span data-ttu-id="33c5f-722">Farklı bir sorgu seçeneklerini gerçek performansını karşılaştırmak için burada farklı sorgu seçeneği kategori adı "İçecekler" olan tüm ürünleri seçmek için kullanırız 5 ayrı test çeşitlemeleri oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="33c5f-722">To compare the real-world performance of the different query options, we created 5 separate test variations where we use a different query option to select all products whose category name is "Beverages".</span></span> <span data-ttu-id="33c5f-723">Her yineleme, bağlam maliyetini ve tüm döndürülen varlıkları düzeniyle maliyetini içerir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-723">Each iteration includes the cost of creating the context, and the cost of materializing all returned entities.</span></span> <span data-ttu-id="33c5f-724">10 yinelemeden zaman aşımına 1000 yineleme toplamını almadan önce untimed çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-724">10 iterations are run untimed before taking the sum of 1000 timed iterations.</span></span> <span data-ttu-id="33c5f-725">Gösterilen sonuçları geçen her test 5 çalıştırmalardan ORTANCA Çalıştır ' dir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-725">The results shown are the median run taken from 5 runs of each test.</span></span> <span data-ttu-id="33c5f-726">Daha fazla bilgi için test kodunu içeren bir ek B bakın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-726">For more information, see Appendix B which includes the code for the test.</span></span>

| <span data-ttu-id="33c5f-727">EF</span><span class="sxs-lookup"><span data-stu-id="33c5f-727">EF</span></span>  | <span data-ttu-id="33c5f-728">Test</span><span class="sxs-lookup"><span data-stu-id="33c5f-728">Test</span></span>                                        | <span data-ttu-id="33c5f-729">Süre (ms)</span><span class="sxs-lookup"><span data-stu-id="33c5f-729">Time (ms)</span></span> | <span data-ttu-id="33c5f-730">Bellek</span><span class="sxs-lookup"><span data-stu-id="33c5f-730">Memory</span></span>   |
|:----|:--------------------------------------------|:----------|:---------|
| <span data-ttu-id="33c5f-731">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-731">EF5</span></span> | <span data-ttu-id="33c5f-732">ObjectContext varlık komutu</span><span class="sxs-lookup"><span data-stu-id="33c5f-732">ObjectContext Entity Command</span></span>                | <span data-ttu-id="33c5f-733">621</span><span class="sxs-lookup"><span data-stu-id="33c5f-733">621</span></span>       | <span data-ttu-id="33c5f-734">39350272</span><span class="sxs-lookup"><span data-stu-id="33c5f-734">39350272</span></span> |
| <span data-ttu-id="33c5f-735">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-735">EF5</span></span> | <span data-ttu-id="33c5f-736">DbContext veritabanında Sql sorgusu</span><span class="sxs-lookup"><span data-stu-id="33c5f-736">DbContext Sql Query on Database</span></span>             | <span data-ttu-id="33c5f-737">825</span><span class="sxs-lookup"><span data-stu-id="33c5f-737">825</span></span>       | <span data-ttu-id="33c5f-738">37519360</span><span class="sxs-lookup"><span data-stu-id="33c5f-738">37519360</span></span> |
| <span data-ttu-id="33c5f-739">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-739">EF5</span></span> | <span data-ttu-id="33c5f-740">ObjectContext Store sorgu</span><span class="sxs-lookup"><span data-stu-id="33c5f-740">ObjectContext Store Query</span></span>                   | <span data-ttu-id="33c5f-741">878</span><span class="sxs-lookup"><span data-stu-id="33c5f-741">878</span></span>       | <span data-ttu-id="33c5f-742">39460864</span><span class="sxs-lookup"><span data-stu-id="33c5f-742">39460864</span></span> |
| <span data-ttu-id="33c5f-743">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-743">EF5</span></span> | <span data-ttu-id="33c5f-744">ObjectContext LINQ Sorgu izleme yok</span><span class="sxs-lookup"><span data-stu-id="33c5f-744">ObjectContext Linq Query No Tracking</span></span>        | <span data-ttu-id="33c5f-745">969</span><span class="sxs-lookup"><span data-stu-id="33c5f-745">969</span></span>       | <span data-ttu-id="33c5f-746">38293504</span><span class="sxs-lookup"><span data-stu-id="33c5f-746">38293504</span></span> |
| <span data-ttu-id="33c5f-747">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-747">EF5</span></span> | <span data-ttu-id="33c5f-748">Nesne sorgusu kullanarak ObjectContext varlık Sql</span><span class="sxs-lookup"><span data-stu-id="33c5f-748">ObjectContext Entity Sql using Object Query</span></span> | <span data-ttu-id="33c5f-749">1089</span><span class="sxs-lookup"><span data-stu-id="33c5f-749">1089</span></span>      | <span data-ttu-id="33c5f-750">38981632</span><span class="sxs-lookup"><span data-stu-id="33c5f-750">38981632</span></span> |
| <span data-ttu-id="33c5f-751">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-751">EF5</span></span> | <span data-ttu-id="33c5f-752">ObjectContext derlenmiş sorgu</span><span class="sxs-lookup"><span data-stu-id="33c5f-752">ObjectContext Compiled Query</span></span>                | <span data-ttu-id="33c5f-753">1099</span><span class="sxs-lookup"><span data-stu-id="33c5f-753">1099</span></span>      | <span data-ttu-id="33c5f-754">38682624</span><span class="sxs-lookup"><span data-stu-id="33c5f-754">38682624</span></span> |
| <span data-ttu-id="33c5f-755">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-755">EF5</span></span> | <span data-ttu-id="33c5f-756">ObjectContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="33c5f-756">ObjectContext Linq Query</span></span>                    | <span data-ttu-id="33c5f-757">1152</span><span class="sxs-lookup"><span data-stu-id="33c5f-757">1152</span></span>      | <span data-ttu-id="33c5f-758">38178816</span><span class="sxs-lookup"><span data-stu-id="33c5f-758">38178816</span></span> |
| <span data-ttu-id="33c5f-759">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-759">EF5</span></span> | <span data-ttu-id="33c5f-760">DbContext LINQ Sorgu izleme yok</span><span class="sxs-lookup"><span data-stu-id="33c5f-760">DbContext Linq Query No Tracking</span></span>            | <span data-ttu-id="33c5f-761">1208</span><span class="sxs-lookup"><span data-stu-id="33c5f-761">1208</span></span>      | <span data-ttu-id="33c5f-762">41803776</span><span class="sxs-lookup"><span data-stu-id="33c5f-762">41803776</span></span> |
| <span data-ttu-id="33c5f-763">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-763">EF5</span></span> | <span data-ttu-id="33c5f-764">DbContext olan DB Sql sorgusu</span><span class="sxs-lookup"><span data-stu-id="33c5f-764">DbContext Sql Query on DbSet</span></span>                | <span data-ttu-id="33c5f-765">1414</span><span class="sxs-lookup"><span data-stu-id="33c5f-765">1414</span></span>      | <span data-ttu-id="33c5f-766">37982208</span><span class="sxs-lookup"><span data-stu-id="33c5f-766">37982208</span></span> |
| <span data-ttu-id="33c5f-767">EF5</span><span class="sxs-lookup"><span data-stu-id="33c5f-767">EF5</span></span> | <span data-ttu-id="33c5f-768">DbContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="33c5f-768">DbContext Linq Query</span></span>                        | <span data-ttu-id="33c5f-769">1574</span><span class="sxs-lookup"><span data-stu-id="33c5f-769">1574</span></span>      | <span data-ttu-id="33c5f-770">41738240</span><span class="sxs-lookup"><span data-stu-id="33c5f-770">41738240</span></span> |
|     |                                             |           |          |
| <span data-ttu-id="33c5f-771">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-771">EF6</span></span> | <span data-ttu-id="33c5f-772">ObjectContext varlık komutu</span><span class="sxs-lookup"><span data-stu-id="33c5f-772">ObjectContext Entity Command</span></span>                | <span data-ttu-id="33c5f-773">480</span><span class="sxs-lookup"><span data-stu-id="33c5f-773">480</span></span>       | <span data-ttu-id="33c5f-774">47247360</span><span class="sxs-lookup"><span data-stu-id="33c5f-774">47247360</span></span> |
| <span data-ttu-id="33c5f-775">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-775">EF6</span></span> | <span data-ttu-id="33c5f-776">ObjectContext Store sorgu</span><span class="sxs-lookup"><span data-stu-id="33c5f-776">ObjectContext Store Query</span></span>                   | <span data-ttu-id="33c5f-777">493</span><span class="sxs-lookup"><span data-stu-id="33c5f-777">493</span></span>       | <span data-ttu-id="33c5f-778">46739456</span><span class="sxs-lookup"><span data-stu-id="33c5f-778">46739456</span></span> |
| <span data-ttu-id="33c5f-779">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-779">EF6</span></span> | <span data-ttu-id="33c5f-780">DbContext veritabanında Sql sorgusu</span><span class="sxs-lookup"><span data-stu-id="33c5f-780">DbContext Sql Query on Database</span></span>             | <span data-ttu-id="33c5f-781">614</span><span class="sxs-lookup"><span data-stu-id="33c5f-781">614</span></span>       | <span data-ttu-id="33c5f-782">41607168</span><span class="sxs-lookup"><span data-stu-id="33c5f-782">41607168</span></span> |
| <span data-ttu-id="33c5f-783">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-783">EF6</span></span> | <span data-ttu-id="33c5f-784">ObjectContext LINQ Sorgu izleme yok</span><span class="sxs-lookup"><span data-stu-id="33c5f-784">ObjectContext Linq Query No Tracking</span></span>        | <span data-ttu-id="33c5f-785">684</span><span class="sxs-lookup"><span data-stu-id="33c5f-785">684</span></span>       | <span data-ttu-id="33c5f-786">46333952</span><span class="sxs-lookup"><span data-stu-id="33c5f-786">46333952</span></span> |
| <span data-ttu-id="33c5f-787">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-787">EF6</span></span> | <span data-ttu-id="33c5f-788">Nesne sorgusu kullanarak ObjectContext varlık Sql</span><span class="sxs-lookup"><span data-stu-id="33c5f-788">ObjectContext Entity Sql using Object Query</span></span> | <span data-ttu-id="33c5f-789">767</span><span class="sxs-lookup"><span data-stu-id="33c5f-789">767</span></span>       | <span data-ttu-id="33c5f-790">48865280</span><span class="sxs-lookup"><span data-stu-id="33c5f-790">48865280</span></span> |
| <span data-ttu-id="33c5f-791">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-791">EF6</span></span> | <span data-ttu-id="33c5f-792">ObjectContext derlenmiş sorgu</span><span class="sxs-lookup"><span data-stu-id="33c5f-792">ObjectContext Compiled Query</span></span>                | <span data-ttu-id="33c5f-793">788</span><span class="sxs-lookup"><span data-stu-id="33c5f-793">788</span></span>       | <span data-ttu-id="33c5f-794">48467968</span><span class="sxs-lookup"><span data-stu-id="33c5f-794">48467968</span></span> |
| <span data-ttu-id="33c5f-795">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-795">EF6</span></span> | <span data-ttu-id="33c5f-796">DbContext LINQ Sorgu izleme yok</span><span class="sxs-lookup"><span data-stu-id="33c5f-796">DbContext Linq Query No Tracking</span></span>            | <span data-ttu-id="33c5f-797">878</span><span class="sxs-lookup"><span data-stu-id="33c5f-797">878</span></span>       | <span data-ttu-id="33c5f-798">47554560</span><span class="sxs-lookup"><span data-stu-id="33c5f-798">47554560</span></span> |
| <span data-ttu-id="33c5f-799">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-799">EF6</span></span> | <span data-ttu-id="33c5f-800">ObjectContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="33c5f-800">ObjectContext Linq Query</span></span>                    | <span data-ttu-id="33c5f-801">953</span><span class="sxs-lookup"><span data-stu-id="33c5f-801">953</span></span>       | <span data-ttu-id="33c5f-802">47632384</span><span class="sxs-lookup"><span data-stu-id="33c5f-802">47632384</span></span> |
| <span data-ttu-id="33c5f-803">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-803">EF6</span></span> | <span data-ttu-id="33c5f-804">DbContext olan DB Sql sorgusu</span><span class="sxs-lookup"><span data-stu-id="33c5f-804">DbContext Sql Query on DbSet</span></span>                | <span data-ttu-id="33c5f-805">1023</span><span class="sxs-lookup"><span data-stu-id="33c5f-805">1023</span></span>      | <span data-ttu-id="33c5f-806">41992192</span><span class="sxs-lookup"><span data-stu-id="33c5f-806">41992192</span></span> |
| <span data-ttu-id="33c5f-807">EF6</span><span class="sxs-lookup"><span data-stu-id="33c5f-807">EF6</span></span> | <span data-ttu-id="33c5f-808">DbContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="33c5f-808">DbContext Linq Query</span></span>                        | <span data-ttu-id="33c5f-809">1290</span><span class="sxs-lookup"><span data-stu-id="33c5f-809">1290</span></span>      | <span data-ttu-id="33c5f-810">47529984</span><span class="sxs-lookup"><span data-stu-id="33c5f-810">47529984</span></span> |


![EF5 sıcak sorgu 1000 yineleme](~/ef6/media/ef5warmquery1000.png)

![EF6 sıcak sorgu 1000 yineleme](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> <span data-ttu-id="33c5f-813">Eksiksiz olması için burada biz bir varlık SQL sorgusu üzerinde bir EntityCommand yürütme bir değişim ekledik.</span><span class="sxs-lookup"><span data-stu-id="33c5f-813">For completeness, we included a variation where we execute an Entity SQL query on an EntityCommand.</span></span> <span data-ttu-id="33c5f-814">Sonuçları gibi sorgular için gerçekleştirilmiş değil, ancak karşılaştırma olmak zorunda değildir elma elma.</span><span class="sxs-lookup"><span data-stu-id="33c5f-814">However, because results are not materialized for such queries, the comparison isn't necessarily apples-to-apples.</span></span> <span data-ttu-id="33c5f-815">Test karşılaştırması fairer yaparken denemek düzeniyle için bir Kapat yaklaşık içerir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-815">The test includes a close approximation to materializing to try making the comparison fairer.</span></span>

<span data-ttu-id="33c5f-816">Bu uçtan uca durumda Entity Framework 6 Entity Framework 5 çeşitli parçaları çok açık DbContext başlatma ve hızlı MetadataCollection dahil yığını üzerinde yapılan performans iyileştirmeleri nedeniyle çok daha iyi&lt;T&gt; Arama.</span><span class="sxs-lookup"><span data-stu-id="33c5f-816">In this end-to-end case, Entity Framework 6 outperforms Entity Framework 5 due to performance improvements made on several parts of the stack, including a much lighter DbContext initialization and faster MetadataCollection&lt;T&gt; lookups.</span></span>

## <a name="7-design-time-performance-considerations"></a><span data-ttu-id="33c5f-817">7 tasarım zamanı performans konuları</span><span class="sxs-lookup"><span data-stu-id="33c5f-817">7 Design time performance considerations</span></span>

### <a name="71-inheritance-strategies"></a><span data-ttu-id="33c5f-818">7.1 devralma stratejileri</span><span class="sxs-lookup"><span data-stu-id="33c5f-818">7.1       Inheritance Strategies</span></span>

<span data-ttu-id="33c5f-819">Entity Framework kullanarak başka bir performans artışı, kullandığınız devralma stratejisidir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-819">Another performance consideration when using Entity Framework is the inheritance strategy you use.</span></span> <span data-ttu-id="33c5f-820">Entity Framework, devralma ve bunların bileşimleri 3 temel türlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="33c5f-820">Entity Framework supports 3 basic types of inheritance and their combinations:</span></span>

-   <span data-ttu-id="33c5f-821">Tablo başına hiyerarşi (her devralma haritalar hiyerarşideki belirli hangi tür belirtmek için bir ayrıştırıcı sütunu içeren bir tabloya ayarlandığı TPH) – satırda gösterilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-821">Table per Hierarchy (TPH) – where each inheritance set maps to a table with a discriminator column to indicate which particular type in the hierarchy is being represented in the row.</span></span>
-   <span data-ttu-id="33c5f-822">Tablo başına tür (her türü kendi tablo veritabanına sahip olduğu TPT) –; alt tablolar yalnızca üst tablo içermiyor sütunları tanımlar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-822">Table per Type (TPT) – where each type has its own table in the database; the child tables only define the columns that the parent table doesn’t contain.</span></span>
-   <span data-ttu-id="33c5f-823">Tablo başına sınıfı (her türü kendi tam tablo veritabanına sahip olduğu TPC) –; alt tablolar üst türlerinde tanımlanan dahil olmak üzere tüm bunların alanları tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-823">Table per Class (TPC) – where each type has its own full table in the database; the child tables define all their fields, including those defined in parent types.</span></span>

<span data-ttu-id="33c5f-824">Modelinizi TPT devralma kullanıyorsa, oluşturulan sorgular artık yürütme süresi Store'daki neden diğer devralma stratejileri ile oluşturulan olandan daha karmaşık olacaktır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-824">If your model uses TPT inheritance, the queries which are generated will be more complex than those that are generated with the other inheritance strategies, which may result on longer execution times on the store.</span></span><span data-ttu-id="33c5f-825">  Genellikle TPT modeli üzerinde sorgular oluşturun ve elde edilen nesnelerini gerçekleştirmek için daha uzun sürer.</span><span class="sxs-lookup"><span data-stu-id="33c5f-825">  It will generally take longer to generate queries over a TPT model, and to materialize the resulting objects.</span></span>

<span data-ttu-id="33c5f-826">"TPT (tablo başına tür) devralma varlık Çerçevesi'nde kullanırken performans konuları" Bkz MSDN blog gönderisi: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.</span><span class="sxs-lookup"><span data-stu-id="33c5f-826">See the "Performance Considerations when using TPT (Table per Type) Inheritance in the Entity Framework" MSDN blog post: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.</span></span>

#### <a name="711-avoiding-tpt-in-model-first-or-code-first-applications"></a><span data-ttu-id="33c5f-827">7.1.1 TPT modeli ilk ya da Code First uygulamaları engelleme</span><span class="sxs-lookup"><span data-stu-id="33c5f-827">7.1.1       Avoiding TPT in Model First or Code First applications</span></span>

<span data-ttu-id="33c5f-828">TPT şemaya sahip mevcut bir veritabanı üzerinde bir modeli oluşturduğunuzda, pek çok seçenek yok.</span><span class="sxs-lookup"><span data-stu-id="33c5f-828">When you create a model over an existing database that has a TPT schema, you don't have many options.</span></span> <span data-ttu-id="33c5f-829">Ancak, Model ilk ya da Code First kullanarak bir uygulama oluştururken, performans endişelerini TPT devralınmasını kaçınmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-829">But when creating an application using Model First or Code First, you should avoid TPT inheritance for performance concerns.</span></span>

<span data-ttu-id="33c5f-830">Varlık Tasarımcısı Sihirbazı'nda kullandığınız Model ilk zaman modelinize için herhangi bir devralma TPT alırsınız.</span><span class="sxs-lookup"><span data-stu-id="33c5f-830">When you use Model First in the Entity Designer Wizard, you will get TPT for any inheritance in your model.</span></span> <span data-ttu-id="33c5f-831">TPH devralma strateji ile ilk Model geçmek istiyorsanız, Visual Studio Gallery'den "varlık Tasarımcısı veritabanı oluşturma Power paketi" kullanıma kullanabilirsiniz ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).</span><span class="sxs-lookup"><span data-stu-id="33c5f-831">If you want to switch to a TPH inheritance strategy with Model First, you can use the "Entity Designer Database Generation Power Pack" available from the Visual Studio Gallery ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).</span></span>

<span data-ttu-id="33c5f-832">Devralma ile bir modelin eşlemeyi yapılandırmak için Code First kullanarak EF TPH varsayılan olarak kullanır, bu nedenle devralma hiyerarşisindeki tüm varlıkları aynı tablonun eşleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="33c5f-832">When using Code First to configure the mapping of a model with inheritance, EF will use TPH by default, therefore all entities in the inheritance hierarchy will be mapped to the same table.</span></span> <span data-ttu-id="33c5f-833">MSDN magazine'de "Kod ilk olarak varlığın Framework4.1" makale "Eşleme ile Fluent API'si" bölümüne bakın ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="33c5f-833">See the "Mapping with the Fluent API" section of the "Code First in Entity Framework4.1" article in MSDN Magazine ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) for more details.</span></span>

### <a name="72-upgrading-from-ef4-to-improve-model-generation-time"></a><span data-ttu-id="33c5f-834">7.2 model oluşturma geliştirmek için EF4 yükseltme zamanı</span><span class="sxs-lookup"><span data-stu-id="33c5f-834">7.2       Upgrading from EF4 to improve model generation time</span></span>

<span data-ttu-id="33c5f-835">Visual Studio 2010 SP1 yüklü olduğunda depolama katmanı (SSDL) oluşturan algoritma modelinin bir SQL Server'a özgü geliştirmeyi Entity Framework 5 ve 6 ve Entity Framework 4 için güncelleştirme olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-835">A SQL Server-specific improvement to the algorithm that generates the store-layer (SSDL) of the model is available in Entity Framework 5 and 6, and as an update to Entity Framework 4 when Visual Studio 2010 SP1 is installed.</span></span> <span data-ttu-id="33c5f-836">Bu çok büyük bir model oluşturma durumda olduğunda Navision model geliştirme aşağıdaki test sonuçlarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-836">The following test results demonstrate the improvement when generating a very big model, in this case the Navision model.</span></span> <span data-ttu-id="33c5f-837">Ek C ilgili daha fazla ayrıntı için bkz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-837">See Appendix C for more details about it.</span></span>

<span data-ttu-id="33c5f-838">1005 varlık setleri ve ilişki Setleri 4227 modeli içerir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-838">The model contains 1005 entity sets and 4227 association sets.</span></span>

| <span data-ttu-id="33c5f-839">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="33c5f-839">Configuration</span></span>                              | <span data-ttu-id="33c5f-840">Dökümü harcanan süre</span><span class="sxs-lookup"><span data-stu-id="33c5f-840">Breakdown of time consumed</span></span>                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="33c5f-841">Visual Studio 2010, Entity Framework 4</span><span class="sxs-lookup"><span data-stu-id="33c5f-841">Visual Studio 2010, Entity Framework 4</span></span>     | <span data-ttu-id="33c5f-842">SSDL oluşturma: 2 saat 27 dk</span><span class="sxs-lookup"><span data-stu-id="33c5f-842">SSDL Generation: 2 hr 27 min</span></span> <br/> <span data-ttu-id="33c5f-843">Eşleme oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-843">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-844">CSDL oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-844">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-845">ObjectLayer oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-845">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-846">Görünüm oluşturma: 2 saat 14 dakika</span><span class="sxs-lookup"><span data-stu-id="33c5f-846">View Generation: 2 h 14 min</span></span> |
| <span data-ttu-id="33c5f-847">Visual Studio 2010 SP1, Entity Framework 4</span><span class="sxs-lookup"><span data-stu-id="33c5f-847">Visual Studio 2010 SP1, Entity Framework 4</span></span> | <span data-ttu-id="33c5f-848">SSDL oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-848">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-849">Eşleme oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-849">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-850">CSDL oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-850">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-851">ObjectLayer oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-851">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-852">Görünüm oluşturma: 1 SA 53 dk</span><span class="sxs-lookup"><span data-stu-id="33c5f-852">View Generation: 1 hr 53 min</span></span>   |
| <span data-ttu-id="33c5f-853">Visual Studio 2013, Entity Framework 5</span><span class="sxs-lookup"><span data-stu-id="33c5f-853">Visual Studio 2013, Entity Framework 5</span></span>     | <span data-ttu-id="33c5f-854">SSDL oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-854">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-855">Eşleme oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-855">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-856">CSDL oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-856">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-857">ObjectLayer oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-857">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-858">Görünüm oluşturma: 65 dakika</span><span class="sxs-lookup"><span data-stu-id="33c5f-858">View Generation: 65 minutes</span></span>    |
| <span data-ttu-id="33c5f-859">Visual Studio 2013, Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="33c5f-859">Visual Studio 2013, Entity Framework 6</span></span>     | <span data-ttu-id="33c5f-860">SSDL oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-860">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-861">Eşleme oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-861">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-862">CSDL oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-862">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-863">ObjectLayer oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="33c5f-863">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="33c5f-864">Görünüm oluşturma: 28 saniye.</span><span class="sxs-lookup"><span data-stu-id="33c5f-864">View Generation: 28 seconds.</span></span>   |


<span data-ttu-id="33c5f-865">Bu istemci geliştirme makinesi beklerken SSDL oluştururken, yükü neredeyse tamamen SQL Server üzerinde harcadığı sunucudan geri dönmeniz sonuçları için boşta çarpmaktadır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-865">It's worth noting that when generating the SSDL, the load is almost entirely spent on the SQL Server, while the client development machine is waiting idle for results to come back from the server.</span></span> <span data-ttu-id="33c5f-866">Dba'lar, özellikle bu geliştirme yönetilmesinde.</span><span class="sxs-lookup"><span data-stu-id="33c5f-866">DBAs should particularly appreciate this improvement.</span></span> <span data-ttu-id="33c5f-867">Ayrıca, temelde tüm maliyet modeli oluşturma görünüm oluşturma işlemi artık gerçekleştirilir hatalarının ayıklanabileceğini belirtmekte yarar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-867">It's also worth noting that essentially the entire cost of model generation takes place in View Generation now.</span></span>

### <a name="73-splitting-large-models-with-database-first-and-model-first"></a><span data-ttu-id="33c5f-868">7.3 veritabanı ile büyük modeller ilk bölme ve ilk Model</span><span class="sxs-lookup"><span data-stu-id="33c5f-868">7.3       Splitting Large Models with Database First and Model First</span></span>

<span data-ttu-id="33c5f-869">Model boyutu arttıkça, Tasarımcı yüzeyine anlaşılamayacak ve kullanmak daha zor hale gelir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-869">As model size increases, the designer surface becomes cluttered and difficult to use.</span></span> <span data-ttu-id="33c5f-870">Biz genellikle bir model Tasarımcısı etkili bir şekilde kullanmak için çok büyük olacak şekilde 300'den fazla varlıklarla göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="33c5f-870">We typically consider a model with more than 300 entities to be too large to effectively use the designer.</span></span> <span data-ttu-id="33c5f-871">Büyük modellerin bölmek için çeşitli seçenekler şu blog gönderisinde açıklanmaktadır: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.</span><span class="sxs-lookup"><span data-stu-id="33c5f-871">The following blog post describes several options for splitting large models: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.</span></span>

<span data-ttu-id="33c5f-872">Post Entity Framework'ün ilk sürümü için yazılmıştır, ancak adımlar hala geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-872">The post was written for the first version of Entity Framework, but the steps still apply.</span></span>

### <a name="74-performance-considerations-with-the-entity-data-source-control"></a><span data-ttu-id="33c5f-873">7.4 varlık veri kaynak denetimi ile performans konuları</span><span class="sxs-lookup"><span data-stu-id="33c5f-873">7.4       Performance considerations with the Entity Data Source Control</span></span>

<span data-ttu-id="33c5f-874">Çok iş parçacıklı performans ve stres testleri durumlarda burada EntityDataSource denetimi kullanarak bir web uygulaması performansını önemli ölçüde deteriorates gördük.</span><span class="sxs-lookup"><span data-stu-id="33c5f-874">We've seen cases in multi-threaded performance and stress tests where the performance of a web application using the EntityDataSource Control deteriorates significantly.</span></span> <span data-ttu-id="33c5f-875">EntityDataSource art arda MetadataWorkspace.LoadFromAssembly varlıklar olarak kullanılacak türlerini bulmak için Web uygulaması tarafından başvurulan derlemeler üzerinde ProcessOrder temel nedeni.</span><span class="sxs-lookup"><span data-stu-id="33c5f-875">The underlying cause is that the EntityDataSource repeatedly calls MetadataWorkspace.LoadFromAssembly on the assemblies referenced by the Web application to discover the types to be used as entities.</span></span>

<span data-ttu-id="33c5f-876">Çözüm, kendi türetilmiş Objectcontext'e sınıfınızı tür adını EntityDataSource, ContextTypeName ayarlamaktır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-876">The solution is to set the ContextTypeName of the EntityDataSource to the type name of your derived ObjectContext class.</span></span> <span data-ttu-id="33c5f-877">Bu, varlık türleri, tüm başvurulan derlemelerin tarar mekanizması kapatır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-877">This turns off the mechanism that scans all referenced assemblies for entity types.</span></span>

<span data-ttu-id="33c5f-878">ContextTypeName alanını ayarlamak, işlevsel bir sorun olduğu yansıma yoluyla bir derlemeden bir tür yüklenemiyor, .NET 4.0 EntityDataSource bir ReflectionTypeLoadException oluşturur engeller.</span><span class="sxs-lookup"><span data-stu-id="33c5f-878">Setting the ContextTypeName field also prevents a functional problem where the EntityDataSource in .NET 4.0 throws a ReflectionTypeLoadException when it can't load a type from an assembly via reflection.</span></span> <span data-ttu-id="33c5f-879">Bu sorun, .NET 4.5 içinde düzeltilmiştir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-879">This issue has been fixed in .NET 4.5.</span></span>

### <a name="75-poco-entities-and-change-tracking-proxies"></a><span data-ttu-id="33c5f-880">7.5 POCO varlık ve değişiklik izleme proxy'ler</span><span class="sxs-lookup"><span data-stu-id="33c5f-880">7.5       POCO entities and change tracking proxies</span></span>

<span data-ttu-id="33c5f-881">Varlık çerçevesi veri sınıfları için herhangi bir değişiklik yapmadan özel veri sınıfları, veri modeli ile birlikte kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-881">Entity Framework enables you to use custom data classes together with your data model without making any modifications to the data classes themselves.</span></span> <span data-ttu-id="33c5f-882">Başka bir deyişle, "düz eski" CLR nesnelerine (POCO), veri modelinizle var olan etki alanı nesnelerini gibi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-882">This means that you can use "plain-old" CLR objects (POCO), such as existing domain objects, with your data model.</span></span> <span data-ttu-id="33c5f-883">Bir veri modelinde tanımlanan varlıklara eşlenmesi bu POCO veri sınıfları (olarak da bilinen Kalıcılık ignorant nesneler), aynı sorgu çoğunu destekler, ekleme, güncelleştirme ve varlık veri modeli araçlarının ürettiği varlık türleri olarak davranışları silme.</span><span class="sxs-lookup"><span data-stu-id="33c5f-883">These POCO data classes (also known as persistence-ignorant objects), which are mapped to entities that are defined in a data model, support most of the same query, insert, update, and delete behaviors as entity types that are generated by the Entity Data Model tools.</span></span>

<span data-ttu-id="33c5f-884">Entity Framework, yavaş yükleniyor ve otomatik değişiklik izleme POCO varlıklarda gibi özellikleri etkinleştirmek istediğinizde, kullanılan POCO türlerinden türetilmiş proxy sınıfları da oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-884">Entity Framework can also create proxy classes derived from your POCO types, which are used when you want to enable features such as lazy loading and automatic change tracking on POCO entities.</span></span> <span data-ttu-id="33c5f-885">POCO sınıflarınızı proxy'ler kullanmanız Entity Framework, burada açıklandığı şekilde izin vermek için belirli gereksinimleri karşılamalıdır: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).</span><span class="sxs-lookup"><span data-stu-id="33c5f-885">Your POCO classes must meet certain requirements to allow Entity Framework to use proxies, as described here: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).</span></span>

<span data-ttu-id="33c5f-886">Fırsat izleme proxy'leri varlıklarınızın özelliklerinden herhangi birini sahip değerine değiştirildi, Entity Framework, her zaman gerçek varlıklarınızın durumunu bilmesi için her zaman nesne durum Yöneticisi bildirir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-886">Chance tracking proxies will notify the object state manager each time any of the properties of your entities has its value changed, so Entity Framework knows the actual state of your entities all the time.</span></span> <span data-ttu-id="33c5f-887">Bu, ayarlayıcı yöntemlerinden birini özelliklerinizi gövdesine bildirim olaylar ekleme ve bu tür olayların işlenmesini nesne durum Yöneticisi sahip gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-887">This is done by adding notification events to the body of the setter methods of your properties, and having the object state manager processing such events.</span></span> <span data-ttu-id="33c5f-888">Proxy oluşturma varlık, genellikle görüntüler olmayan proxy'si POCO varlık eklenen Entity Framework tarafından oluşturulan olaylar nedeniyle oluşturmaya kıyasla daha pahalı unutmayın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-888">Note that creating a proxy entity will typically be more expensive than creating a non-proxy POCO entity due to the added set of events created by Entity Framework.</span></span>

<span data-ttu-id="33c5f-889">Değişiklik izleme proxy POCO varlık sahip olmadığında, değişiklikleri varlıklarınızı bir kopyasını bir önceki kaydedilen bir duruma karşı içeriğini karşılaştırarak bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="33c5f-889">When a POCO entity does not have a change tracking proxy, changes are found by comparing the contents of your entities against a copy of a previous saved state.</span></span> <span data-ttu-id="33c5f-890">En son karşılaştırmanın atıldıktan sonra bunların hiçbiri değişmiş olsa bile bu ayrıntılı karşılaştırma uzun süren bir işlem birçok varlığın, bağlam içinde olduğunda ya da özellikler, çok büyük miktarda varlıklarınızı sahip olur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-890">This deep comparison will become a lengthy process when you have many entities in your context, or when your entities have a very large amount of properties, even if none of them changed since the last comparison took place.</span></span>

<span data-ttu-id="33c5f-891">Özet: değişiklik izleme proxy oluşturulurken isabet bir performans ödersiniz, ancak değişiklik izleme yardımcı olacak birçok özelliği ya da modelinizde birçok varlığın olduğunda varlıklarınızı sahip olduğunuzda değişiklik algılama sürecini hızlandırmak.</span><span class="sxs-lookup"><span data-stu-id="33c5f-891">In summary: you’ll pay a performance hit when creating the change tracking proxy, but change tracking will help you speed up the change detection process when your entities have many properties or when you have many entities in your model.</span></span> <span data-ttu-id="33c5f-892">Varlıkları miktarı çok fazla büyüdüğü değil özelliklerinin küçük bir sayı ile varlıklar için çok yararı, değişiklik izleme proxy'leri sahip olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-892">For entities with a small number of properties where the amount of entities doesn’t grow too much, having change tracking proxies may not be of much benefit.</span></span>

## <a name="8-loading-related-entities"></a><span data-ttu-id="33c5f-893">İlgili varlıkları 8 yükleme</span><span class="sxs-lookup"><span data-stu-id="33c5f-893">8 Loading Related Entities</span></span>

### <a name="81-lazy-loading-vs-eager-loading"></a><span data-ttu-id="33c5f-894">8.1 yavaş yükleme vs. İstekli yükleme</span><span class="sxs-lookup"><span data-stu-id="33c5f-894">8.1 Lazy Loading vs. Eager Loading</span></span>

<span data-ttu-id="33c5f-895">Entity Framework, hedef varlıkla ilgili varlıkları yükleme için çeşitli yollar sunar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-895">Entity Framework offers several different ways to load the entities that are related to your target entity.</span></span> <span data-ttu-id="33c5f-896">Örneğin, ürünleri için sorgulama yapıldığında, ilgili Siparişler yüklenecek farklı yol vardır nesne durumu Manager'a.</span><span class="sxs-lookup"><span data-stu-id="33c5f-896">For example, when you query for Products, there are different ways that the related Orders will be loaded into the Object State Manager.</span></span> <span data-ttu-id="33c5f-897">Performans açısından, ilgili varlıkları yüklenirken dikkate alınması gereken büyük soru yavaş yükleniyor veya istekli yükleme olacaktır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-897">From a performance standpoint, the biggest question to consider when loading related entities will be whether to use Lazy Loading or Eager Loading.</span></span>

<span data-ttu-id="33c5f-898">İstekli yükleme kullanırken, ilgili varlıkları, hedef varlık kümesi ile birlikte yüklenir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-898">When using Eager Loading, the related entities are loaded along with your target entity set.</span></span> <span data-ttu-id="33c5f-899">Sorgunuzda INCLUDE deyimi, ilgili varlıkları almak için istediğiniz belirtmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-899">You use an Include statement in your query to indicate which related entities you want to bring in.</span></span>

<span data-ttu-id="33c5f-900">Yavaş yükleniyor kullanırken ilk sorgunuzu yalnızca hedef varlık kümesinde getirir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-900">When using Lazy Loading, your initial query only brings in the target entity set.</span></span> <span data-ttu-id="33c5f-901">Ancak, bir gezinti özelliği eriştiğinizde, başka bir sorgu ilgili varlık yüklemek için depo verilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-901">But whenever you access a navigation property, another query is issued against the store to load the related entity.</span></span>

<span data-ttu-id="33c5f-902">Bir varlık yüklendikten sonra yavaş yükleniyor veya istekli yükleme kullanıp kullanmadığınızı varlık için daha fazla sorgu, nesne durumu Yöneticisi'nden doğrudan yükleyin.</span><span class="sxs-lookup"><span data-stu-id="33c5f-902">Once an entity has been loaded, any further queries for the entity will load it directly from the Object State Manager, whether you are using lazy loading or eager loading.</span></span>

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a><span data-ttu-id="33c5f-903">8.2 yavaş yükleniyor ve istekli yükleme arasında seçim yapma</span><span class="sxs-lookup"><span data-stu-id="33c5f-903">8.2 How to choose between Lazy Loading and Eager Loading</span></span>

<span data-ttu-id="33c5f-904">Önemli olan, böylece uygulamanız için doğru seçim yapabileceğiniz yavaş yükleniyor ve istekli yükleme arasındaki farkı anlamak olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-904">The important thing is that you understand the difference between Lazy Loading and Eager Loading so that you can make the correct choice for your application.</span></span> <span data-ttu-id="33c5f-905">Bu, büyük bir yükü içerebilir, tek bir istek karşı veritabanında birden çok istek etmekten değerlendirmenize yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-905">This will help you evaluate the tradeoff between multiple requests against the database versus a single request that may contain a large payload.</span></span> <span data-ttu-id="33c5f-906">Uygulamanız bazı bölümlerinde istekli yükleme ve yavaş yükleniyor diğer bölümlerinde kullanmak uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-906">It may be appropriate to use eager loading in some parts of your application and lazy loading in other parts.</span></span>

<span data-ttu-id="33c5f-907">Başlık altında neler olduğunu ilişkin bir örnek olarak, Birleşik Krallık ve bunların sırası sayısını yaşayan müşterilerin sorgulamak istediğiniz varsayalım.</span><span class="sxs-lookup"><span data-stu-id="33c5f-907">As an example of what's happening under the hood, suppose you want to query for the customers who live in the UK and their order count.</span></span>

<span data-ttu-id="33c5f-908">**İstekli yükleme kullanma**</span><span class="sxs-lookup"><span data-stu-id="33c5f-908">**Using Eager Loading**</span></span>

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

<span data-ttu-id="33c5f-909">**Kullanılarak yavaş yükleniyor**</span><span class="sxs-lookup"><span data-stu-id="33c5f-909">**Using Lazy Loading**</span></span>

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    context.ContextOptions.LazyLoadingEnabled = true;

    //Notice that the Include method call is missing in the query
    var ukCustomers = context.Customers.Where(c => c.Address.Country == "UK");

    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

<span data-ttu-id="33c5f-910">İstekli yükleme kullanırken, tüm müşteriler döndüren tek bir sorgu sorunu ve tüm siparişleri.</span><span class="sxs-lookup"><span data-stu-id="33c5f-910">When using eager loading, you'll issue a single query that returns all customers and all orders.</span></span> <span data-ttu-id="33c5f-911">Depo komutu şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="33c5f-911">The store command looks like:</span></span>

``` SQL
SELECT
[Project1].[C1] AS [C1],
[Project1].[CustomerID] AS [CustomerID],
[Project1].[CompanyName] AS [CompanyName],
[Project1].[ContactName] AS [ContactName],
[Project1].[ContactTitle] AS [ContactTitle],
[Project1].[Address] AS [Address],
[Project1].[City] AS [City],
[Project1].[Region] AS [Region],
[Project1].[PostalCode] AS [PostalCode],
[Project1].[Country] AS [Country],
[Project1].[Phone] AS [Phone],
[Project1].[Fax] AS [Fax],
[Project1].[C2] AS [C2],
[Project1].[OrderID] AS [OrderID],
[Project1].[CustomerID1] AS [CustomerID1],
[Project1].[EmployeeID] AS [EmployeeID],
[Project1].[OrderDate] AS [OrderDate],
[Project1].[RequiredDate] AS [RequiredDate],
[Project1].[ShippedDate] AS [ShippedDate],
[Project1].[ShipVia] AS [ShipVia],
[Project1].[Freight] AS [Freight],
[Project1].[ShipName] AS [ShipName],
[Project1].[ShipAddress] AS [ShipAddress],
[Project1].[ShipCity] AS [ShipCity],
[Project1].[ShipRegion] AS [ShipRegion],
[Project1].[ShipPostalCode] AS [ShipPostalCode],
[Project1].[ShipCountry] AS [ShipCountry]
FROM ( SELECT
      [Extent1].[CustomerID] AS [CustomerID],
       [Extent1].[CompanyName] AS [CompanyName],
       [Extent1].[ContactName] AS [ContactName],
       [Extent1].[ContactTitle] AS [ContactTitle],
       [Extent1].[Address] AS [Address],
       [Extent1].[City] AS [City],
       [Extent1].[Region] AS [Region],
       [Extent1].[PostalCode] AS [PostalCode],
       [Extent1].[Country] AS [Country],
       [Extent1].[Phone] AS [Phone],
       [Extent1].[Fax] AS [Fax],
      1 AS [C1],
       [Extent2].[OrderID] AS [OrderID],
       [Extent2].[CustomerID] AS [CustomerID1],
       [Extent2].[EmployeeID] AS [EmployeeID],
       [Extent2].[OrderDate] AS [OrderDate],
       [Extent2].[RequiredDate] AS [RequiredDate],
       [Extent2].[ShippedDate] AS [ShippedDate],
       [Extent2].[ShipVia] AS [ShipVia],
       [Extent2].[Freight] AS [Freight],
       [Extent2].[ShipName] AS [ShipName],
       [Extent2].[ShipAddress] AS [ShipAddress],
       [Extent2].[ShipCity] AS [ShipCity],
       [Extent2].[ShipRegion] AS [ShipRegion],
       [Extent2].[ShipPostalCode] AS [ShipPostalCode],
       [Extent2].[ShipCountry] AS [ShipCountry],
      CASE WHEN ([Extent2].[OrderID] IS NULL) THEN CAST(NULL AS int) ELSE 1 END AS [C2]
      FROM  [dbo].[Customers] AS [Extent1]
      LEFT OUTER JOIN [dbo].[Orders] AS [Extent2] ON [Extent1].[CustomerID] = [Extent2].[CustomerID]
      WHERE N'UK' = [Extent1].[Country]
)  AS [Project1]
ORDER BY [Project1].[CustomerID] ASC, [Project1].[C2] ASC
```

<span data-ttu-id="33c5f-912">Yavaş yükleniyor kullanırken, başlangıçta aşağıdaki sorguyu yürütün:</span><span class="sxs-lookup"><span data-stu-id="33c5f-912">When using lazy loading, you'll issue the following query initially:</span></span>

``` SQL
SELECT
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[CompanyName] AS [CompanyName],
[Extent1].[ContactName] AS [ContactName],
[Extent1].[ContactTitle] AS [ContactTitle],
[Extent1].[Address] AS [Address],
[Extent1].[City] AS [City],
[Extent1].[Region] AS [Region],
[Extent1].[PostalCode] AS [PostalCode],
[Extent1].[Country] AS [Country],
[Extent1].[Phone] AS [Phone],
[Extent1].[Fax] AS [Fax]
FROM [dbo].[Customers] AS [Extent1]
WHERE N'UK' = [Extent1].[Country]
```

<span data-ttu-id="33c5f-913">Ve her bir müşterinin siparişleri gezinme özelliğini eriştiğinizde, depo aşağıdaki gibi başka bir sorgu verilir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-913">And each time you access the Orders navigation property of a customer another query like the following is issued against the store:</span></span>

``` SQL
exec sp_executesql N'SELECT
[Extent1].[OrderID] AS [OrderID],
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[EmployeeID] AS [EmployeeID],
[Extent1].[OrderDate] AS [OrderDate],
[Extent1].[RequiredDate] AS [RequiredDate],
[Extent1].[ShippedDate] AS [ShippedDate],
[Extent1].[ShipVia] AS [ShipVia],
[Extent1].[Freight] AS [Freight],
[Extent1].[ShipName] AS [ShipName],
[Extent1].[ShipAddress] AS [ShipAddress],
[Extent1].[ShipCity] AS [ShipCity],
[Extent1].[ShipRegion] AS [ShipRegion],
[Extent1].[ShipPostalCode] AS [ShipPostalCode],
[Extent1].[ShipCountry] AS [ShipCountry]
FROM [dbo].[Orders] AS [Extent1]
WHERE [Extent1].[CustomerID] = @EntityKeyValue1',N'@EntityKeyValue1 nchar(5)',@EntityKeyValue1=N'AROUT'
```

<span data-ttu-id="33c5f-914">Daha fazla bilgi için [ilgili nesneler Yükleniyor](https://msdn.microsoft.com/library/bb896272.aspx).</span><span class="sxs-lookup"><span data-stu-id="33c5f-914">For more information, see the [Loading Related Objects](https://msdn.microsoft.com/library/bb896272.aspx).</span></span>

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a><span data-ttu-id="33c5f-915">8.2.1 yavaş yükleme karşılaştırması istekli yükleme kopya kağıdı</span><span class="sxs-lookup"><span data-stu-id="33c5f-915">8.2.1 Lazy Loading versus Eager Loading cheat sheet</span></span>

<span data-ttu-id="33c5f-916">İstekli yükleme yavaş yükleniyor karşılaştırması seçmek için gönderilemez bir BT'ye yoktur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-916">There’s no such thing as a one-size-fits-all to choosing eager loading versus lazy loading.</span></span> <span data-ttu-id="33c5f-917">Her iki stratejileri de bilinçli bir karar yapabilmesi arasındaki farkları anlamak önce deneyin; Ayrıca, kodunuzu herhangi aşağıdaki senaryolar için uygun olmadığını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="33c5f-917">Try first to understand the differences between both strategies so you can do a well informed decision; also, consider if your code fits to any of the following scenarios:</span></span>

| <span data-ttu-id="33c5f-918">Senaryo</span><span class="sxs-lookup"><span data-stu-id="33c5f-918">Scenario</span></span>                                                                    | <span data-ttu-id="33c5f-919">Bizim önerisi</span><span class="sxs-lookup"><span data-stu-id="33c5f-919">Our Suggestion</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="33c5f-920">Çok sayıda Gezinti özelliklerine getirilen varlıkların erişim gerekiyor mu?</span><span class="sxs-lookup"><span data-stu-id="33c5f-920">Do you need to access many navigation properties from the fetched entities?</span></span> | <span data-ttu-id="33c5f-921">**Hayır** -seçeneklerin muhtemelen kullanıyordur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-921">**No** - Both options will probably do.</span></span> <span data-ttu-id="33c5f-922">Ancak, sorgunuzu getirme yükü performans avantajlarının daha az ihtiyacınız olacaktır istekli yükleme kullanarak karşılaşabileceğiniz çok büyük ise uyguladığından, nesnelerini gerçekleştirmek için ağ.</span><span class="sxs-lookup"><span data-stu-id="33c5f-922">However, if the payload your query is bringing is not too big, you may experience performance benefits by using Eager loading as it’ll require less network round trips to materialize your objects.</span></span> <br/> <br/> <span data-ttu-id="33c5f-923">**Evet** -çok sayıda Gezinti özellikleri, varlıkların erişmeniz gerekiyorsa, birden fazla deyimleri ile istekli yükleme sorgunuzda içerdiğini yaptığınız.</span><span class="sxs-lookup"><span data-stu-id="33c5f-923">**Yes** -  If you need to access many navigation properties from the entities, you’d do that by using multiple include statements in your query with Eager loading.</span></span> <span data-ttu-id="33c5f-924">Daha fazla varlık dahil, daha büyük yük sorgunuzu döndürür.</span><span class="sxs-lookup"><span data-stu-id="33c5f-924">The more entities you include, the bigger the payload your query will return.</span></span> <span data-ttu-id="33c5f-925">Üç veya daha fazla varlık sorgunuzu dahil sonra yükleme Lazy için göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="33c5f-925">Once you include three or more entities into your query, consider switching to Lazy loading.</span></span> |
| <span data-ttu-id="33c5f-926">Tam olarak hangi verilerin çalışma zamanında gerekli olacağını biliyor musunuz?</span><span class="sxs-lookup"><span data-stu-id="33c5f-926">Do you know exactly what data will be needed at run time?</span></span>                   | <span data-ttu-id="33c5f-927">**Hayır** -yavaş yükleniyor sizin için daha iyi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-927">**No** - Lazy loading will be better for you.</span></span> <span data-ttu-id="33c5f-928">Aksi takdirde, değil gerekir veri sorgulama yukarı sonlandırabiliriz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-928">Otherwise, you may end up querying for data that you will not need.</span></span> <br/> <br/> <span data-ttu-id="33c5f-929">**Evet** - istekli yükleme büyük olasılıkla, en iyi sonucu; tüm ayarlar daha hızlı yükleme yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-929">**Yes** - Eager loading is probably your best bet; it will help loading entire sets faster.</span></span> <span data-ttu-id="33c5f-930">Sorgunuz çok büyük miktarda veri getirilirken gerektirir ve bu çok düşük olur, bunun yerine yüklenirken Lazy deneyin.</span><span class="sxs-lookup"><span data-stu-id="33c5f-930">If your query requires fetching a very large amount of data, and this becomes too slow, then try Lazy loading instead.</span></span>                                                                                                                                                                                                                                                       |
| <span data-ttu-id="33c5f-931">Kodunuzu veritabanınızı gölgeden uzak yürütüyor?</span><span class="sxs-lookup"><span data-stu-id="33c5f-931">Is your code executing far from your database?</span></span> <span data-ttu-id="33c5f-932">(daha fazla ağ gecikmesi)</span><span class="sxs-lookup"><span data-stu-id="33c5f-932">(increased network latency)</span></span>  | <span data-ttu-id="33c5f-933">**Hayır** - ağ gecikme süresi bir sorun olmadığında kullanılarak yavaş yükleniyor kodunuzu basitleştirin.</span><span class="sxs-lookup"><span data-stu-id="33c5f-933">**No** - When the network latency is not an issue, using Lazy loading may simplify your code.</span></span> <span data-ttu-id="33c5f-934">Uygulamanızın topolojisini, verilen için veritabanı yakınlık yakalayana şekilde değiştirebileceğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="33c5f-934">Remember that the topology of your application may change, so don’t take database proximity for granted.</span></span> <br/> <br/> <span data-ttu-id="33c5f-935">**Evet** - ağ ne senaryonuz için daha iyi uyduğunu karar yalnızca bir sorun olduğunda.</span><span class="sxs-lookup"><span data-stu-id="33c5f-935">**Yes** - When the network is a problem, only you can decide what fits better for your scenario.</span></span> <span data-ttu-id="33c5f-936">Genellikle daha az sayıda gidiş dönüş gerektirdiğinden istekli yükleme daha iyi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-936">Typically Eager loading will be better because it requires fewer round trips.</span></span>                                                                                                                                                                                                      |


#### <a name="822-performance-concerns-with-multiple-includes"></a><span data-ttu-id="33c5f-937">8.2.2 performans endişelerini ile birden çok içerir</span><span class="sxs-lookup"><span data-stu-id="33c5f-937">8.2.2       Performance concerns with multiple Includes</span></span>

<span data-ttu-id="33c5f-938">Biz sunucu yanıt süresi sorunları ilgili performans sorular duyduğunuzda, sorunun sık birden çok içerik deyimleri sorgularla kaynağıdır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-938">When we hear performance questions that involve server response time problems, the source of the issue is frequently queries with multiple Include statements.</span></span> <span data-ttu-id="33c5f-939">Bir sorguda ilgili varlıkları dahil olmak üzere güçlü olsa da, bu işlem arka planda neler olduğunu anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-939">While including related entities in a query is powerful, it's important to understand what's happening under the covers.</span></span>

<span data-ttu-id="33c5f-940">İçindeki depolama komutu oluşturmak için iç planı derleyicimiz gitmek için bir sorgu ile birden çok içerik deyimleri oldukça uzun bir süre sürer.</span><span class="sxs-lookup"><span data-stu-id="33c5f-940">It takes a relatively long time for a query with multiple Include statements in it to go through our internal plan compiler to produce the store command.</span></span> <span data-ttu-id="33c5f-941">Bu süre çoğu, sonuçta elde edilen sorgu iyileştirmeye çalışıyor harcanır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-941">The majority of this time is spent trying to optimize the resulting query.</span></span> <span data-ttu-id="33c5f-942">Oluşturulan depolama komutu, eşleştirme bağlı olarak her dahil etme için Outer JOIN veya birleşim içerecektir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-942">The generated store command will contain an Outer Join or Union for each Include, depending on your mapping.</span></span> <span data-ttu-id="33c5f-943">Bu gibi sorguların büyük bağlı grafikler, özellikle yedeklilik (örneğin INCLUDE birden fazla düzeyde geçirmek için kullanıldığında, yükte çok fazla olduğunda, bant genişliği sorunları acerbate olan tek bir yükte veritabanınızdan Getir ilişkileri bir-çok yönde).</span><span class="sxs-lookup"><span data-stu-id="33c5f-943">Queries like this will bring in large connected graphs from your database in a single payload, which will acerbate any bandwidth issues, especially when there is a lot of redundancy in the payload (for example, when multiple levels of Include are used to traverse associations in the one-to-many direction).</span></span>

<span data-ttu-id="33c5f-944">Burada, sorgularınızı aşırı büyük yükleri ToTraceString kullanarak ve yükü boyutu görmek için SQL Server Management Studio içinde depolama komutu yürütülürken temel alınan bir TSQL sorgusu için erişerek iade ettiğiniz durumlarda kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-944">You can check for cases where your queries are returning excessively large payloads by accessing the underlying TSQL for the query by using ToTraceString and executing the store command in SQL Server Management Studio to see the payload size.</span></span> <span data-ttu-id="33c5f-945">Azaltmak için yapabileceğiniz gibi durumlarda içerik deyimleri yalnızca sorgunuzda sayısını ihtiyacınız verilerinizi getirin.</span><span class="sxs-lookup"><span data-stu-id="33c5f-945">In such cases you can try to reduce the number of Include statements in your query to just bring in the data you need.</span></span> <span data-ttu-id="33c5f-946">Veya örneğin alt sorgularda, daha küçük dizisine sorgunuzu bölüneceği mümkün olabilir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-946">Or you may be able to break your query into a smaller sequence of subqueries, for example:</span></span>

<span data-ttu-id="33c5f-947">**Sorgu kesmeden önce:**</span><span class="sxs-lookup"><span data-stu-id="33c5f-947">**Before breaking the query:**</span></span>

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var customers = from c in context.Customers.Include(c => c.Orders)
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

<span data-ttu-id="33c5f-948">**Sorgu sonu sonra:**</span><span class="sxs-lookup"><span data-stu-id="33c5f-948">**After breaking the query:**</span></span>

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var orders = from o in context.Orders
                 where o.Customer.LastName.StartsWith(lastNameParameter)
                 select o;

    orders.Load();

    var customers = from c in context.Customers
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

<span data-ttu-id="33c5f-949">Yapıyoruz gibi bu yalnızca izlenen sorgulara çalışır bağlamı sahip kimlik çözümlemesi ve ilişki düzeltmesi otomatik olarak gerçekleştirmek için özelliği kullanımı.</span><span class="sxs-lookup"><span data-stu-id="33c5f-949">This will work only on tracked queries, as we are making use of the ability the context has to perform identity resolution and association fixup automatically.</span></span>

<span data-ttu-id="33c5f-950">Gecikmeli yükleme gibi karşılıklı avantaj ve dezavantajlarını daha fazla sorgular için daha küçük yüklerini sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-950">As with lazy loading, the tradeoff will be more queries for smaller payloads.</span></span> <span data-ttu-id="33c5f-951">Yalnızca ihtiyacınız olan verileri her bir varlıktaki açıkça seçmek için ayrı ayrı özellikler yansıtmaların de kullanabilirsiniz ancak, varlıklar bu durumda yüklenmemesi ve güncelleştirmeleri desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="33c5f-951">You can also use projections of individual properties to explicitly select only the data you need from each entity, but you will not be loading entities in this case, and updates will not be supported.</span></span>

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a><span data-ttu-id="33c5f-952">8.2.3 geçici özelliklerinin yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="33c5f-952">8.2.3 Workaround to get lazy loading of properties</span></span>

<span data-ttu-id="33c5f-953">Entity Framework, şu anda skaler veya karmaşık özellikler yavaş yüklenmesini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="33c5f-953">Entity Framework currently doesn’t support lazy loading of scalar or complex properties.</span></span> <span data-ttu-id="33c5f-954">Ancak, bir BLOB gibi büyük bir nesne içeren bir tablo olduğu durumlarda, tablo bölme büyük özelliklerin ayrı bir varlığa ayırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-954">However, in cases where you have a table that includes a large object such as a BLOB, you can use table splitting to separate the large properties into a separate entity.</span></span> <span data-ttu-id="33c5f-955">Örneğin, varbinary fotoğraf sütunu içeren bir ürün tablo olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="33c5f-955">For example, suppose you have a Product table that includes a varbinary photo column.</span></span> <span data-ttu-id="33c5f-956">Sık sorgularınızı bu özellikte erişmeye ihtiyacınız yoksa, yalnızca, normalde varlığa bölümlerinde getirmek için bölme tablo kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-956">If you don't frequently need to access this property in your queries, you can use table splitting to bring in only the parts of the entity that you normally need.</span></span> <span data-ttu-id="33c5f-957">Ürün fotoğrafı temsil eden varlık yalnızca açıkça gerektiğinde yüklenecek.</span><span class="sxs-lookup"><span data-stu-id="33c5f-957">The entity representing the product photo will only be loaded when you explicitly need it.</span></span>

<span data-ttu-id="33c5f-958">Gil Fink'ın "Tablo bölme, Entity Framework" blog gönderisine tablo bölme etkinleştirme gösteren makaleden faydalanabilirsiniz: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.</span><span class="sxs-lookup"><span data-stu-id="33c5f-958">A good resource that shows how to enable table splitting is Gil Fink's "Table Splitting in Entity Framework" blog post: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.</span></span>

## <a name="9-other-considerations"></a><span data-ttu-id="33c5f-959">9 diğer konular</span><span class="sxs-lookup"><span data-stu-id="33c5f-959">9 Other considerations</span></span>

### <a name="91-server-garbage-collection"></a><span data-ttu-id="33c5f-960">9.1 sunucu çöp toplama</span><span class="sxs-lookup"><span data-stu-id="33c5f-960">9.1      Server Garbage Collection</span></span>

<span data-ttu-id="33c5f-961">Bazı kullanıcılar, çöp toplayıcının düzgün şekilde yapılandırılmadığında, bunlar görmeyi paralellik sınırlar kaynak çekişmesini karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-961">Some users might experience resource contention that limits the parallelism they are expecting when the Garbage Collector is not properly configured.</span></span> <span data-ttu-id="33c5f-962">EF birden çok iş parçacıklı bir senaryoda kullanılan veya herhangi bir uygulamada, bir sunucu tarafı sistemi benzer olduğunda, sunucu Çöp toplamayı etkinleştirmek emin olun.</span><span class="sxs-lookup"><span data-stu-id="33c5f-962">Whenever EF is used in a multithreaded scenario, or in any application that resembles a server-side system, make sure to enable Server Garbage Collection.</span></span> <span data-ttu-id="33c5f-963">Bu, basit bir ayar, uygulama yapılandırma dosyasında aracılığıyla gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-963">This is done via a simple setting in your application config file:</span></span>

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

<span data-ttu-id="33c5f-964">Bu, iş parçacığı çekişmeyi azaltmak ve % 30 oranında e doygun CPU senaryolarda aktarım hızınızı artırın gerekir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-964">This should decrease your thread contention and increase your throughput by up to 30% in CPU saturated scenarios.</span></span> <span data-ttu-id="33c5f-965">Genel koşullarını nasıl yanı sıra sunucu çöp toplama (Bu, daha iyi kullanıcı Arabirimi ve istemci tarafı senaryolar için ayarlanmıştır) Klasik çöp toplama kullanarak uygulamanızın davranışını her zaman test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-965">In general terms, you should always test how your application behaves using the classic Garbage Collection (which is better tuned for UI and client side scenarios) as well as the Server Garbage Collection.</span></span>

### <a name="92-autodetectchanges"></a><span data-ttu-id="33c5f-966">9.2 AutoDetectChanges</span><span class="sxs-lookup"><span data-stu-id="33c5f-966">9.2      AutoDetectChanges</span></span>

<span data-ttu-id="33c5f-967">Daha önce belirtildiği gibi Entity Framework, nesne önbelleği birçok varlığın sahip olduğunda performans sorunlarını gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-967">As mentioned earlier, Entity Framework might show performance issues when the object cache has many entities.</span></span> <span data-ttu-id="33c5f-968">Ekle, Kaldır, bulma, giriş ve SaveChanges, gibi bazı işlemleri, büyük miktarda CPU ne kadar büyük nesne önbelleği haline gelmiştir üzerinde tabanlı tüketebilir DetectChanges çağrıları tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="33c5f-968">Certain operations, such as Add, Remove, Find, Entry and SaveChanges, trigger calls to DetectChanges which might consume a large amount of CPU based on how large the object cache has become.</span></span> <span data-ttu-id="33c5f-969">Bunun nedeni, nesne önbelleği ve Nesne Durum Yöneticisi'ni deneyin olarak kalır, böylece üretilen veri çeşit senaryo altında doğru olması garanti bir bağlam için gerçekleştirilen her işlem üzerinde mümkün olduğunca eşitlendiğini ' dir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-969">The reason for this is that the object cache and the object state manager try to stay as synchronized as possible on each operation performed to a context so that the produced data is guaranteed to be correct under a wide array of scenarios.</span></span>

<span data-ttu-id="33c5f-970">Entity Framework'ün otomatik değişiklik algılama ömrü, uygulamanız için etkin tutulacak genellikle iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-970">It is generally a good practice to leave Entity Framework’s automatic change detection enabled for the entire life of your application.</span></span> <span data-ttu-id="33c5f-971">Senaryonuz olumsuz yüksek CPU kullanımına göre etkilenmesini ve profillerinizi sabah DetectChanges çağrısı belirtmek, geçici olarak devre dışı AutoDetectChanges kodunuzun hassas bölümünde kapatma göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="33c5f-971">If your scenario is being negatively affected by high CPU usage and your profiles indicate that the culprit is the call to DetectChanges, consider temporarily turning off AutoDetectChanges in the sensitive portion of your code:</span></span>

``` csharp
try
{
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    ...
}
finally
{
    context.Configuration.AutoDetectChangesEnabled = true;
}
```

<span data-ttu-id="33c5f-972">AutoDetectChanges kapatmadan önce bu Entity Framework, gerçekleşirken varlıklar üzerinde değişiklikler hakkındaki belirli bilgileri izlemek için güncelleyebileceği kaybetmesine neden olabilir anlamak uygundur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-972">Before turning off AutoDetectChanges, it’s good to understand that this might cause Entity Framework to lose its ability to track certain information about the changes that are taking place on the entities.</span></span> <span data-ttu-id="33c5f-973">Hatalı olarak işlenir, bu uygulama veri tutarsızlığına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-973">If handled incorrectly, this might cause data inconsistency on your application.</span></span> <span data-ttu-id="33c5f-974">AutoDetectChanges kapatarak daha fazla bilgi için okuma \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.</span><span class="sxs-lookup"><span data-stu-id="33c5f-974">For more information on turning off AutoDetectChanges, read \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.</span></span>

### <a name="93-context-per-request"></a><span data-ttu-id="33c5f-975">9.3 istek başına bağlamı</span><span class="sxs-lookup"><span data-stu-id="33c5f-975">9.3      Context per request</span></span>

<span data-ttu-id="33c5f-976">Entity Framework'ün bağlamları en iyi performans sağlamak için kısa süreli örnekleri deneyimi gibi kullanılmaya yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-976">Entity Framework’s contexts are meant to be used as short-lived instances in order to provide the most optimal performance experience.</span></span> <span data-ttu-id="33c5f-977">Bağlamları bekleniyor kısa olması beklenir ve atılır ve bu nedenle basit ve meta verileri mümkün olduğunca reutilize uygulanmıştır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-977">Contexts are expected to be short lived and discarded, and as such have been implemented to be very lightweight and reutilize metadata whenever possible.</span></span> <span data-ttu-id="33c5f-978">Web senaryolarında bunu aklınızda bulundurun ve tek bir isteğin süresinden daha fazla bilgi için bir bağlam yok önemlidir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-978">In web scenarios it’s important to keep this in mind and not have a context for more than the duration of a single request.</span></span> <span data-ttu-id="33c5f-979">Benzer şekilde, web olmayan senaryolarda bağlam göre varlık Çerçevesi'nde önbelleğe alma düzeylerini anlama atılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-979">Similarly, in non-web scenarios, context should be discarded based on your understanding of the different levels of caching in the Entity Framework.</span></span> <span data-ttu-id="33c5f-980">Genel olarak bakıldığında, bir uygulama hem de iş parçacığı başına bağlamları ve statik içerikleri ömrü boyunca bir bağlam örneği kaçınmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-980">Generally speaking, one should avoid having a context instance throughout the life of the application, as well as contexts per thread and static contexts.</span></span>

### <a name="94-database-null-semantics"></a><span data-ttu-id="33c5f-981">9.4 sürümünden veritabanı null semantikler</span><span class="sxs-lookup"><span data-stu-id="33c5f-981">9.4      Database null semantics</span></span>

<span data-ttu-id="33c5f-982">Varsayılan olarak Entity Framework C olan SQL kodu üretir\# null karşılaştırma semantiği.</span><span class="sxs-lookup"><span data-stu-id="33c5f-982">Entity Framework by default will generate SQL code that has C\# null comparison semantics.</span></span> <span data-ttu-id="33c5f-983">Aşağıdaki örnek sorgu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="33c5f-983">Consider the following example query:</span></span>

``` csharp
            int? categoryId = 7;
            int? supplierId = 8;
            decimal? unitPrice = 0;
            short? unitsInStock = 100;
            short? unitsOnOrder = 20;
            short? reorderLevel = null;

            var q = from p incontext.Products
                    wherep.Category.CategoryName == "Beverages"
                          || (p.CategoryID == categoryId
                                || p.SupplierID == supplierId
                                || p.UnitPrice == unitPrice
                                || p.UnitsInStock == unitsInStock
                                || p.UnitsOnOrder == unitsOnOrder
                                || p.ReorderLevel == reorderLevel)
                    select p;

            var r = q.ToList();
```

<span data-ttu-id="33c5f-984">Bu örnekte biz SupplierID ve UnitPrice gibi varlık boş değer atanabilir özellikte karşı boş değer atanabilir değişkenleri sayısı senedini karşılaştırdığınızı.</span><span class="sxs-lookup"><span data-stu-id="33c5f-984">In this example, we’re comparing a number of nullable variables against nullable properties on the entity, such as SupplierID and UnitPrice.</span></span> <span data-ttu-id="33c5f-985">Bu sorgu için oluşturulan SQL sütun değeri olarak aynı parametre değeri ise ya da hem parametrenin hem de sütun değerlerini null sorar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-985">The generated SQL for this query will ask if the parameter value is the same as the column value, or if both the parameter and the column values are null.</span></span> <span data-ttu-id="33c5f-986">Bu veritabanı sunucusu, null değerlere işleme gizler ve tutarlı bir C sağlayacak\# deneyimi arasında farklı veritabanı satıcılarının null.</span><span class="sxs-lookup"><span data-stu-id="33c5f-986">This will hide the way the database server handles nulls and will provide a consistent C\# null experience across different database vendors.</span></span> <span data-ttu-id="33c5f-987">Öte yandan, oluşturulan kod biraz karışık ve iyi olduğunda çalışmayabilir karşılaştırmalar miktarını where sorgu deyimi için çok sayıda büyür.</span><span class="sxs-lookup"><span data-stu-id="33c5f-987">On the other hand, the generated code is a bit convoluted and may not perform well when the amount of comparisons in the where statement of the query grows to a large number.</span></span>

<span data-ttu-id="33c5f-988">Bu durumu düzeltmek için bir veritabanı null semantikler kullanarak yoludur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-988">One way to deal with this situation is by using database null semantics.</span></span> <span data-ttu-id="33c5f-989">Bu büyük olasılıkla farklı C davranabilir Not\# Entity Framework veritabanı altyapısı, boş değerler işleme kullanıma sunan basit SQL oluşturacak artık bu yana semantiği null.</span><span class="sxs-lookup"><span data-stu-id="33c5f-989">Note that this might potentially behave differently to the C\# null semantics since now Entity Framework will generate simpler SQL that exposes the way the database engine handles null values.</span></span> <span data-ttu-id="33c5f-990">Veritabanı null semantikler etkinleştirilmiş başına bağlam bağlam içi yapılandırma karşı tek tek yapılandırma satır içeren olabilir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-990">Database null semantics can be activated per-context with one single configuration line against the context configuration:</span></span>

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

<span data-ttu-id="33c5f-991">Küçük ve orta ölçekli sorguları algılanabilir performans iyileştirmesi veritabanı null semantikler kullanılırken görüntülenmez, ancak fark sorguları ile çok sayıda olası null karşılaştırmalar daha belirgin hale gelir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-991">Small to medium sized queries will not display a perceptible performance improvement when using database null semantics, but the difference will become more noticeable on queries with a large number of potential null comparisons.</span></span>

<span data-ttu-id="33c5f-992">Yukarıdaki örnek sorguda bir performans farkı küçüktür %2 denetimli bir ortamda çalışan bir microbenchmark içinde oluştu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-992">In the example query above, the performance difference was less than 2% in a microbenchmark running in a controlled environment.</span></span>

### <a name="95-async"></a><span data-ttu-id="33c5f-993">9.5 zaman uyumsuz</span><span class="sxs-lookup"><span data-stu-id="33c5f-993">9.5      Async</span></span>

<span data-ttu-id="33c5f-994">.NET 4.5 veya sonraki sürümlerde çalışan zaman uyumsuz işlemler Entity Framework 6 sunulan desteği.</span><span class="sxs-lookup"><span data-stu-id="33c5f-994">Entity Framework 6 introduced support of async operations when running on .NET 4.5 or later.</span></span> <span data-ttu-id="33c5f-995">Çoğunlukla, g/ç uygulamaları Çekişme ilgili en çok zaman uyumsuz sorgu kullanma avantajını yakalayabilirler ve kaydetme işlemleri.</span><span class="sxs-lookup"><span data-stu-id="33c5f-995">For the most part, applications that have IO related contention will benefit the most from using asynchronous query and save operations.</span></span> <span data-ttu-id="33c5f-996">Uygulamanızın g/ç kms'den kaynaklanan çakışmayı saptanmamış, zaman uyumsuz kullanımını en iyi durumda zaman uyumlu olarak çalışacak ve sonuç aynı süre içinde zaman uyumlu bir çağrı olarak ya da en kötü durumda, yalnızca zaman uyumsuz bir görev için yürütme ertele ve ek tim Ekle tamamlandığında, senaryonuz e.</span><span class="sxs-lookup"><span data-stu-id="33c5f-996">If your application does not suffer from IO contention, the use of async will, in the best cases, run synchronously and return the result in the same amount of time as a synchronous call, or in the worst case, simply defer execution to an asynchronous task and add extra time to the completion of your scenario.</span></span>

<span data-ttu-id="33c5f-997">Zaman uyumsuz bir uygulamanızın performansını artıracak karar yardımcı olacak ne zaman uyumsuz programlama iş ziyaret bilgi [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx).</span><span class="sxs-lookup"><span data-stu-id="33c5f-997">For information on how asynchronous programming work that will help you deciding if async will improve the performance of your application visit [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx).</span></span> <span data-ttu-id="33c5f-998">Entity Framework zaman uyumsuz işlemleri daha fazla bilgi için bkz. [zaman uyumsuz sorgu ve tasarruf](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="33c5f-998">For more information on the use of async operations on Entity Framework, see [Async Query and Save](~/ef6/fundamentals/async.md
).</span></span>

### <a name="96-ngen"></a><span data-ttu-id="33c5f-999">9.6 NGEN</span><span class="sxs-lookup"><span data-stu-id="33c5f-999">9.6      NGEN</span></span>

<span data-ttu-id="33c5f-1000">Entity Framework 6, .NET framework'ün varsayılan yüklemede gelmez.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1000">Entity Framework 6 does not come in the default installation of .NET framework.</span></span> <span data-ttu-id="33c5f-1001">Bu nedenle, Entity Framework derlemeleri Entity Framework kodunun herhangi bir MSIL derleme olarak aynı JIT'ing maliyetleri tabi olduğu anlamına gelir varsayılan NGEN 'D değildir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1001">As such, the Entity Framework assemblies are not NGEN’d by default which means that all the Entity Framework code is subject to the same JIT’ing costs as any other MSIL assembly.</span></span> <span data-ttu-id="33c5f-1002">Bu, geliştirme ve ayrıca, uygulamanızın üretim ortamlarında soğuk başlangıç F5 deneyimi düşebilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1002">This might degrade the F5 experience while developing and also the cold startup of your application in the production environments.</span></span> <span data-ttu-id="33c5f-1003">JIT'ing CPU ve bellek maliyetlerini azaltmak için Entity Framework uygun şekilde görüntüleri NGEN için tavsiye edilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1003">In order to reduce the CPU and memory costs of JIT’ing it is advisable to NGEN the Entity Framework images as appropriate.</span></span> <span data-ttu-id="33c5f-1004">Entity Framework 6 NGEN ile başlangıç performansını artırmak nasıl hakkında daha fazla bilgi için bkz. [NGen ile başlangıç performansı artırma](~/ef6/fundamentals/performance/ngen.md).</span><span class="sxs-lookup"><span data-stu-id="33c5f-1004">For more information on how to improve the startup performance of Entity Framework 6 with NGEN, see [Improving Startup Performance with NGen](~/ef6/fundamentals/performance/ngen.md).</span></span>

### <a name="97-code-first-versus-edmx"></a><span data-ttu-id="33c5f-1005">9.7 ilk EDMX karşı kodu</span><span class="sxs-lookup"><span data-stu-id="33c5f-1005">9.7      Code First versus EDMX</span></span>

<span data-ttu-id="33c5f-1006">Entity Framework nedeniyle nesne yönelimli programlama ve bir bellek içi temsillerinin kavramsal model (nesneler), depolama şemanın (veritabanı) ve arasında bir eşleme tarafından ilişkisel veritabanları arasında empedans uyuşmazlığı sorunu hakkında iki.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1006">Entity Framework reasons about the impedance mismatch problem between object oriented programming and relational databases by having an in-memory representation of the conceptual model (the objects), the storage schema (the database) and a mapping between the two.</span></span> <span data-ttu-id="33c5f-1007">Bu meta veriler için kısa bir varlık veri modeli veya EDM çağrılır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1007">This metadata is called an Entity Data Model, or EDM for short.</span></span> <span data-ttu-id="33c5f-1008">Bu EDM Entity Framework görünümleri gidiş dönüş verileri veritabanına bellekte nesnelerin türetilir ve yedekleyin.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1008">From this EDM, Entity Framework will derive the views to roundtrip data from the objects in memory to the database and back.</span></span>

<span data-ttu-id="33c5f-1009">Entity Framework, resmi bir EDMX dosyası ile kullanıldığında, kavramsal model ve depolama şema eşleme belirtir, ardından EDM doğru olduğunu doğrulamak yalnızca model yükleme aşaması vardır (örneğin, hiçbir eşlemesi eksik olduğundan emin olun), ardından görünümlerin, görünümleri doğrulamak ve kullanıma hazır bu meta veriler bulunur.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1009">When Entity Framework is used with an EDMX file that formally specifies the conceptual model, the storage schema, and the mapping, then the model loading stage only has to validate that the EDM is correct (for example, make sure that no mappings are missing), then generate the views, then validate the views and have this metadata ready for use.</span></span> <span data-ttu-id="33c5f-1010">Yalnızca sonra can bir sorgu yürütülür veya yeni verileri veri deposuna kaydedilmiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1010">Only then can a query be executed or new data be saved to the data store.</span></span>

<span data-ttu-id="33c5f-1011">Code First, temelinde, Gelişmiş bir varlık veri modeli Oluşturucu bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1011">The Code First approach is, at its heart, a sophisticated Entity Data Model generator.</span></span> <span data-ttu-id="33c5f-1012">Bir EDM sağlanan kod üretmek Entity Framework vardır; Bunu kuralları uygulamak ve modeli Fluent API'si aracılığıyla yapılandırma modeli katılan sınıflar çözümleme yapar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1012">The Entity Framework has to produce an EDM from the provided code; it does so by analyzing the classes involved in the model, applying conventions and configuring the model via the Fluent API.</span></span> <span data-ttu-id="33c5f-1013">EDM oluşturulduktan sonra şekilde olduğu gibi projede yüklenmemiş bir EDMX dosyasının varlık çerçevesi temelde aynı şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1013">After the EDM is built, the Entity Framework essentially behaves the same way as it would had an EDMX file been present in the project.</span></span> <span data-ttu-id="33c5f-1014">Bu nedenle, Code First modeli oluşturma, bir EDMX bulunması karşılaştırıldığında Entity Framework için daha yavaş bir başlangıç zamanına çevirir fazladan karmaşıklık ekler.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1014">Thus, building the model from Code First adds extra complexity that translates into a slower startup time for the Entity Framework when compared to having an EDMX.</span></span> <span data-ttu-id="33c5f-1015">Maliyet tamamen boyutu ve oluşturulmakta olan modelin karmaşıklığını bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1015">The cost is completely dependent on the size and complexity of the model that’s being built.</span></span>

<span data-ttu-id="33c5f-1016">Code First karşı EDMX kullanmayı seçerken, Code First tarafından sunulan esnekliği modeli ilk kez oluşturma maliyetini artırır bilmek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1016">When choosing to use EDMX versus Code First, it’s important to know that the flexibility introduced by Code First increases the cost of building the model for the first time.</span></span> <span data-ttu-id="33c5f-1017">Uygulamanız bu ilk kez yük maliyetini dayanabilir verirse genellikle Code First gitmek için tercih edilen yol olacaktır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1017">If your application can withstand the cost of this first-time load then typically Code First will be the preferred way to go.</span></span>

## <a name="10-investigating-performance"></a><span data-ttu-id="33c5f-1018">10 performans araştırma</span><span class="sxs-lookup"><span data-stu-id="33c5f-1018">10 Investigating Performance</span></span>

### <a name="101-using-the-visual-studio-profiler"></a><span data-ttu-id="33c5f-1019">10.1 Visual Studio Profiler kullanma</span><span class="sxs-lookup"><span data-stu-id="33c5f-1019">10.1 Using the Visual Studio Profiler</span></span>

<span data-ttu-id="33c5f-1020">Entity Framework ile performans sorunları yaşıyorsanız, uygulamanızı kendi zaman harcadığı burada görmek için Visual Studio'da yerleşik olanlar gibi bir profil oluşturucu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1020">If you are having performance issues with the Entity Framework, you can use a profiler like the one built into Visual Studio to see where your application is spending its time.</span></span> <span data-ttu-id="33c5f-1021">Bu "Keşfetme - bölüm 1 ADO.NET Entity Framework performansının" blog gönderisinde pasta grafikler oluşturmak için kullandığımız aracıdır ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) Entity Framework, süre boyunca soğuk ve orta Gecikmeli sorgular nerede geçirdiği göster.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1021">This is the tool we used to generate the pie charts in the “Exploring the Performance of the ADO.NET Entity Framework - Part 1” blog post ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) that show where Entity Framework spends its time during cold and warm queries.</span></span>

<span data-ttu-id="33c5f-1022">Profil Oluşturucu performans sorunu araştırmak için almaları bir gerçek örnek veri ve modelleme Müşteri danışma ekibi tarafından yazılan "profil oluşturma Entity Framework kullanarak Visual Studio 2010 Profiler" blog gönderisine gösterir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1022">The "Profiling Entity Framework using the Visual Studio 2010 Profiler" blog post written by the Data and Modeling Customer Advisory Team shows a real-world example of how they used the profiler to investigate a performance problem.</span></span><span data-ttu-id="33c5f-1023">  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1023">  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>.</span></span> <span data-ttu-id="33c5f-1024">Bu gönderi, windows uygulaması için yazılmıştır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1024">This post was written for a windows application.</span></span> <span data-ttu-id="33c5f-1025">Bir web uygulamasının profilini çıkarmak gerekiyorsa Windows Performans kaydedici (WPR) ve Windows Performans Çözümleyicisi (WPA) araçları Visual Studio'dan çalışma daha iyi çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1025">If you need to profile a web application the Windows Performance Recorder (WPR) and Windows Performance Analyzer (WPA) tools may work better than working from Visual Studio.</span></span> <span data-ttu-id="33c5f-1026">Windows değerlendirme ve Dağıtım Seti ile dahil olan Windows Performans araç bir parçası olan WBT ve WPA ( [ http://www.microsoft.com/download/details.aspx?id=39982 ](https://www.microsoft.com/download/details.aspx?id=39982)).</span><span class="sxs-lookup"><span data-stu-id="33c5f-1026">WPR and WPA are part of the Windows Performance Toolkit which is included with the Windows Assessment and Deployment Kit ( [http://www.microsoft.com/download/details.aspx?id=39982](https://www.microsoft.com/download/details.aspx?id=39982)).</span></span>

### <a name="102-applicationdatabase-profiling"></a><span data-ttu-id="33c5f-1027">10.2 uygulama/veritabanı profil oluşturma</span><span class="sxs-lookup"><span data-stu-id="33c5f-1027">10.2 Application/Database profiling</span></span>

<span data-ttu-id="33c5f-1028">Visual Studio'da yerleşik olarak bulunan profil oluşturucu gibi araçları uygulamanızın zaman harcadığı yerleri burada söyleyin.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1028">Tools like the profiler built into Visual Studio tell you where your application is spending time.</span></span><span data-ttu-id="33c5f-1029">  Profil Oluşturucu başka türde kullanılabilir çalışan uygulamanızı, üretim veya üretim öncesi gereksinimlerine bağlı olarak dinamik analizini yapar ve yaygın görülen tehlikeleri ve veritabanı erişim ters desenler için arar.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1029">  Another type of profiler is available that performs dynamic analysis of your running application, either in production or pre-production depending on needs, and looks for common pitfalls and anti-patterns of database access.</span></span>

<span data-ttu-id="33c5f-1030">Piyasadaki iki profil oluşturucular olan Entity Framework Profiler ( \< http://efprof.com>) ve ORMProfiler ( \< http://ormprofiler.com>).</span><span class="sxs-lookup"><span data-stu-id="33c5f-1030">Two commercially available profilers are the Entity Framework Profiler ( \<http://efprof.com>) and ORMProfiler ( \<http://ormprofiler.com>).</span></span>

<span data-ttu-id="33c5f-1031">Uygulamanız Code First kullanarak bir MVC uygulaması ise, StackExchange'nın MiniProfiler kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1031">If your application is an MVC application using Code First, you can use StackExchange's MiniProfiler.</span></span> <span data-ttu-id="33c5f-1032">Scott Hanselman blog bu araç açıklar: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1032">Scott Hanselman describes this tool in his blog at: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.</span></span>

<span data-ttu-id="33c5f-1033">Uygulamanızın veritabanı etkinliğini, Julie Lerman'ın MSDN Magazine makalesini başlıklı bkz: profil oluşturma hakkında daha fazla bilgi için [profil oluşturma, veritabanı etkinliğini içindeki varlık çerçevesi](https://msdn.microsoft.com/magazine/gg490349.aspx).</span><span class="sxs-lookup"><span data-stu-id="33c5f-1033">For more information on profiling your application's database activity, see Julie Lerman's MSDN Magazine article titled [Profiling Database Activity in the Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).</span></span>

### <a name="103-database-logger"></a><span data-ttu-id="33c5f-1034">10.3 veritabanı Günlükçü</span><span class="sxs-lookup"><span data-stu-id="33c5f-1034">10.3 Database logger</span></span>

<span data-ttu-id="33c5f-1035">Entity Framework 6 kullanıyorsanız de yerleşik günlük kaydetme işlevini kullanarak göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1035">If you are using Entity Framework 6 also consider using the built-in logging functionality.</span></span> <span data-ttu-id="33c5f-1036">Bağlam veritabanı özelliği, etkinlik basit bir satır yapılandırma yoluyla oturum sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-1036">The Database property of the context can be instructed to log its activity via a simple one-line configuration:</span></span>

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

<span data-ttu-id="33c5f-1037">Bu örnekte, veritabanı etkinliğini konsolda günlüğe kaydedilir, ancak herhangi bir eylemi çağırmak için günlük özelliği yapılandırılabilir&lt;dize&gt; temsilci.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1037">In this example the database activity will be logged to the console, but the Log property can be configured to call any Action&lt;string&gt; delegate.</span></span>

<span data-ttu-id="33c5f-1038">Etkinleştirmek istiyorsanız veritabanı günlüğü olmadan yeniden derlemeden ve Entity Framework 6.1 kullanarak veya daha sonra uygulamanızın web.config veya app.config dosyasında bir dinleyiciyi ekleyerek bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1038">If you want to enable database logging without recompiling, and you are using Entity Framework 6.1 or later, you can do so by adding an interceptor in the web.config or app.config file of your application.</span></span>

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

<span data-ttu-id="33c5f-1039">Git yeniden derlemeye gerek kalmadan günlüğe kaydetme ekleme hakkında daha fazla bilgi için \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1039">For more information on how to add logging without recompiling go to \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.</span></span>

## <a name="11-appendix"></a><span data-ttu-id="33c5f-1040">11 ek</span><span class="sxs-lookup"><span data-stu-id="33c5f-1040">11 Appendix</span></span>

### <a name="111-a-test-environment"></a><span data-ttu-id="33c5f-1041">11.1 A. Test Ortamı</span><span class="sxs-lookup"><span data-stu-id="33c5f-1041">11.1 A. Test Environment</span></span>

<span data-ttu-id="33c5f-1042">Bu ortam, ayrı bir makineye istemci uygulamadan alınan veritabanı ile 2 makine Kurulum kullanır.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1042">This environment uses a 2-machine setup with the database on a separate machine from the client application.</span></span> <span data-ttu-id="33c5f-1043">Ağ gecikme süresi düşük, ancak daha gerçekçi bir tek makineli ortamından, bu nedenle aynı rafa makinelerdir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1043">Machines are in the same rack, so network latency is relatively low, but more realistic than a single-machine environment.</span></span>

#### <a name="1111-app-server"></a><span data-ttu-id="33c5f-1044">11.1.1 uygulama sunucusu</span><span class="sxs-lookup"><span data-stu-id="33c5f-1044">11.1.1       App Server</span></span>

##### <a name="11111-software-environment"></a><span data-ttu-id="33c5f-1045">11.1.1.1 yazılım ortamı</span><span class="sxs-lookup"><span data-stu-id="33c5f-1045">11.1.1.1      Software Environment</span></span>

-   <span data-ttu-id="33c5f-1046">Entity Framework 4 yazılım ortamı</span><span class="sxs-lookup"><span data-stu-id="33c5f-1046">Entity Framework 4 Software Environment</span></span>
    -   <span data-ttu-id="33c5f-1047">İşletim sistemi adı: Windows Server 2008 R2 Enterprise SP1.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1047">OS Name: Windows Server 2008 R2 Enterprise SP1.</span></span>
    -   <span data-ttu-id="33c5f-1048">Visual Studio 2010 – Ultimate.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1048">Visual Studio 2010 – Ultimate.</span></span>
    -   <span data-ttu-id="33c5f-1049">Visual Studio 2010 SP1 (yalnızca bazı karşılaştırmalar için).</span><span class="sxs-lookup"><span data-stu-id="33c5f-1049">Visual Studio 2010 SP1 (only for some comparisons).</span></span>
-   <span data-ttu-id="33c5f-1050">Entity Framework 5 ve 6 yazılım ortamı</span><span class="sxs-lookup"><span data-stu-id="33c5f-1050">Entity Framework 5 and 6 Software Environment</span></span>
    -   <span data-ttu-id="33c5f-1051">İşletim sistemi adı: Windows 8.1 Enterprise</span><span class="sxs-lookup"><span data-stu-id="33c5f-1051">OS Name: Windows 8.1 Enterprise</span></span>
    -   <span data-ttu-id="33c5f-1052">Visual Studio 2013 – Ultimate.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1052">Visual Studio 2013 – Ultimate.</span></span>

##### <a name="11112-hardware-environment"></a><span data-ttu-id="33c5f-1053">11.1.1.2 donanım ortamı</span><span class="sxs-lookup"><span data-stu-id="33c5f-1053">11.1.1.2      Hardware Environment</span></span>

-   <span data-ttu-id="33c5f-1054">Çift işlemci:     Intel(r) Xeon(R) CPU L5520 W3530 2.27 GHz @, 2261 Mhz8 GHz, 4 çekirdek, 84 mantıksal işlemci.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1054">Dual Processor:     Intel(R) Xeon(R) CPU L5520 W3530 @ 2.27GHz, 2261 Mhz8 GHz, 4 Core(s), 84 Logical Processor(s).</span></span>
-   <span data-ttu-id="33c5f-1055">2412 GB RamRAM.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1055">2412 GB RamRAM.</span></span>
-   <span data-ttu-id="33c5f-1056">4 bölüme bölme 136 GB SCSI250GB SATA 7200 rpm 3 GB/sn sürücüsü.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1056">136 GB SCSI250GB SATA 7200 rpm 3GB/s drive split into 4 partitions.</span></span>

#### <a name="1112-db-server"></a><span data-ttu-id="33c5f-1057">11.1.2 DB sunucusu</span><span class="sxs-lookup"><span data-stu-id="33c5f-1057">11.1.2       DB server</span></span>

##### <a name="11121-software-environment"></a><span data-ttu-id="33c5f-1058">11.1.2.1 yazılım ortamı</span><span class="sxs-lookup"><span data-stu-id="33c5f-1058">11.1.2.1      Software Environment</span></span>

-   <span data-ttu-id="33c5f-1059">İşletim sistemi adı: Windows Server 2008 R28.1 Enterprise SP1.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1059">OS Name: Windows Server 2008 R28.1 Enterprise SP1.</span></span>
-   <span data-ttu-id="33c5f-1060">SQL Server 2008 R22012.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1060">SQL Server 2008 R22012.</span></span>

##### <a name="11122-hardware-environment"></a><span data-ttu-id="33c5f-1061">11.1.2.2 donanım ortamı</span><span class="sxs-lookup"><span data-stu-id="33c5f-1061">11.1.2.2      Hardware Environment</span></span>

-   <span data-ttu-id="33c5f-1062">Tek bir işlemcinin: Intel(r) Xeon(R) CPU L5520 2.27 GHz @, 2261 MhzES-1620 0 @ 3.60 GHz, 4 çekirdek, 8 mantıksal işlemci.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1062">Single Processor: Intel(R) Xeon(R) CPU L5520  @ 2.27GHz, 2261 MhzES-1620 0 @ 3.60GHz, 4 Core(s), 8 Logical Processor(s).</span></span>
-   <span data-ttu-id="33c5f-1063">824 GB RamRAM.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1063">824 GB RamRAM.</span></span>
-   <span data-ttu-id="33c5f-1064">4 bölüme bölme 465 GB ATA500GB SATA 7200 rpm 6 GB/sn sürücüsü.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1064">465 GB ATA500GB SATA 7200 rpm 6GB/s drive split into 4 partitions.</span></span>

### <a name="112-b-query-performance-comparison-tests"></a><span data-ttu-id="33c5f-1065">11.2 sorgu b testleri karşılaştırma</span><span class="sxs-lookup"><span data-stu-id="33c5f-1065">11.2      B. Query performance comparison tests</span></span>

<span data-ttu-id="33c5f-1066">Northwind modeli, bu testleri yürütmek için kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1066">The Northwind model was used to execute these tests.</span></span> <span data-ttu-id="33c5f-1067">Bu, Entity Framework designer kullanarak veritabanı oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1067">It was generated from the database using the Entity Framework designer.</span></span> <span data-ttu-id="33c5f-1068">Ardından, aşağıdaki kod, sorgu yürütme seçeneklerini performansını karşılaştırmak için kullanılan:</span><span class="sxs-lookup"><span data-stu-id="33c5f-1068">Then, the following code was used to compare the performance of the query execution options:</span></span>

``` csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Common;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Data.Objects;
using System.Linq;

namespace QueryComparison
{
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
                );

        public IQueryable<Product> InvokeProductsForCategoryCQ(string categoryName)
        {
            return productsForCategoryCQ(this, categoryName);
        }
    }

    public class QueryTypePerfComparison
    {
        private static string entityConnectionStr = @"metadata=res://*/Northwind.csdl|res://*/Northwind.ssdl|res://*/Northwind.msl;provider=System.Data.SqlClient;provider connection string='data source=.;initial catalog=Northwind;integrated security=True;multipleactiveresultsets=True;App=EntityFramework'";

        public void LINQIncludingContextCreation()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTracking()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                context.Products.MergeOption = MergeOption.NoTracking;

                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void CompiledQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                var q = context.InvokeProductsForCategoryCQ("Beverages");
                q.ToList();
            }
        }

        public void ObjectQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
                products.ToList();
            }
        }

        public void EntityCommand()
        {
            using (EntityConnection eConn = new EntityConnection(entityConnectionStr))
            {
                eConn.Open();
                EntityCommand cmd = eConn.CreateCommand();
                cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

                using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
                {
                    List<Product> productsList = new List<Product>();
                    while (reader.Read())
                    {
                        DbDataRecord record = (DbDataRecord)reader.GetValue(0);

                        // 'materialize' the product by accessing each field and value. Because we are materializing products, we won't have any nested data readers or records.
                        int fieldCount = record.FieldCount;

                        // Treat all products as Product, even if they are the subtype DiscontinuedProduct.
                        Product product = new Product();  

                        product.ProductID = record.GetInt32(0);
                        product.ProductName = record.GetString(1);
                        product.SupplierID = record.GetInt32(2);
                        product.CategoryID = record.GetInt32(3);
                        product.QuantityPerUnit = record.GetString(4);
                        product.UnitPrice = record.GetDecimal(5);
                        product.UnitsInStock = record.GetInt16(6);
                        product.UnitsOnOrder = record.GetInt16(7);
                        product.ReorderLevel = record.GetInt16(8);
                        product.Discontinued = record.GetBoolean(9);

                        productsList.Add(product);
                    }
                }
            }
        }

        public void ExecuteStoreQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectResult<Product> beverages = context.ExecuteStoreQuery<Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Database.SqlQuery\<QueryComparison.DbC.Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbSet()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Products.SqlQuery(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void LINQIncludingContextCreationDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTrackingDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var q = context.Products.AsNoTracking().Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }
    }
}
```

### <a name="113-c-navision-model"></a><span data-ttu-id="33c5f-1069">11,3 c Navision modeli</span><span class="sxs-lookup"><span data-stu-id="33c5f-1069">11.3 C. Navision Model</span></span>

<span data-ttu-id="33c5f-1070">Microsoft Dynamics – NAV tanıtım için kullanılan büyük bir veritabanı Navision veritabanıdır</span><span class="sxs-lookup"><span data-stu-id="33c5f-1070">The Navision database is a large database used to demo Microsoft Dynamics – NAV.</span></span> <span data-ttu-id="33c5f-1071">Oluşturulan kavramsal model 1005 varlık setleri ve ilişki Setleri 4227 içerir.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1071">The generated conceptual model contains 1005 entity sets and 4227 association sets.</span></span> <span data-ttu-id="33c5f-1072">Testte kullanılan model "Düz" – hiçbir devralma için eklendi.</span><span class="sxs-lookup"><span data-stu-id="33c5f-1072">The model used in the test is “flat” – no inheritance has been added to it.</span></span>

#### <a name="1131-queries-used-for-navision-tests"></a><span data-ttu-id="33c5f-1073">11.3.1 Navision testler için kullanılan sorguları</span><span class="sxs-lookup"><span data-stu-id="33c5f-1073">11.3.1 Queries used for Navision tests</span></span>

<span data-ttu-id="33c5f-1074">Navision modeliyle kullanılan sorguları listesi Entity SQL sorguları 3 kategorilerini içerir:</span><span class="sxs-lookup"><span data-stu-id="33c5f-1074">The queries list used with the Navision model contains 3 categories of Entity SQL queries:</span></span>

##### <a name="11311-lookup"></a><span data-ttu-id="33c5f-1075">11.3.1.1 arama</span><span class="sxs-lookup"><span data-stu-id="33c5f-1075">11.3.1.1 Lookup</span></span>

<span data-ttu-id="33c5f-1076">Basit arama sorgusuyla toplama</span><span class="sxs-lookup"><span data-stu-id="33c5f-1076">A simple lookup query with no aggregations</span></span>

-   <span data-ttu-id="33c5f-1077">Sayısı: 16232</span><span class="sxs-lookup"><span data-stu-id="33c5f-1077">Count: 16232</span></span>
-   <span data-ttu-id="33c5f-1078">Örnek:</span><span class="sxs-lookup"><span data-stu-id="33c5f-1078">Example:</span></span>

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312singleaggregating"></a><span data-ttu-id="33c5f-1079">11.3.1.2 SingleAggregating</span><span class="sxs-lookup"><span data-stu-id="33c5f-1079">11.3.1.2 SingleAggregating</span></span>

<span data-ttu-id="33c5f-1080">Normal bir BI sorgu birden çok toplama işlemi, ancak hiçbir alt toplamlar (tek sorgu)</span><span class="sxs-lookup"><span data-stu-id="33c5f-1080">A normal BI query with multiple aggregations, but no subtotals (single query)</span></span>

-   <span data-ttu-id="33c5f-1081">Sayısı: 2313</span><span class="sxs-lookup"><span data-stu-id="33c5f-1081">Count: 2313</span></span>
-   <span data-ttu-id="33c5f-1082">Örnek:</span><span class="sxs-lookup"><span data-stu-id="33c5f-1082">Example:</span></span>

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

<span data-ttu-id="33c5f-1083">Burada MDF\_SessionLogin\_zaman\_Max() model olarak tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="33c5f-1083">Where MDF\_SessionLogin\_Time\_Max() is defined in the model as:</span></span>

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313aggregatingsubtotals"></a><span data-ttu-id="33c5f-1084">11.3.1.3 AggregatingSubtotals</span><span class="sxs-lookup"><span data-stu-id="33c5f-1084">11.3.1.3 AggregatingSubtotals</span></span>

<span data-ttu-id="33c5f-1085">Bir BI sorgu toplamalar ve alt toplamlar (aracılığıyla tüm birleşimi)</span><span class="sxs-lookup"><span data-stu-id="33c5f-1085">A BI query with aggregations and subtotals (via union all)</span></span>

-   <span data-ttu-id="33c5f-1086">Sayısı: 178</span><span class="sxs-lookup"><span data-stu-id="33c5f-1086">Count: 178</span></span>
-   <span data-ttu-id="33c5f-1087">Örnek:</span><span class="sxs-lookup"><span data-stu-id="33c5f-1087">Example:</span></span>

``` xml
  <Query complexity="AggregatingSubtotals">
    <CommandText>
using NavisionFK;
function AmountConsumed(entities Collection([CRONUS_International_Ltd__Zone])) as
(
    Edm.Sum(select value N.Block_Movement FROM entities as E, E.CRONUS_International_Ltd__Bin as N)
)
function AmountConsumed(P1 Edm.Int32) as
(
    AmountConsumed(select value e from NavisionFKContext.CRONUS_International_Ltd__Zone as e where e.Zone_Ranking = P1)
)
----------------------------------------------------------------------------------------------------------------------
(
    select top(10) Zone_Ranking, Cross_Dock_Bin_Zone, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking, E.Cross_Dock_Bin_Zone
)
union all
(
    select top(10) Zone_Ranking, Cast(null as Edm.Byte) as P2, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking
)
union all
{
    Row(Cast(null as Edm.Int32) as P1, Cast(null as Edm.Byte) as P2, AmountConsumed(select value E
                                                                         from NavisionFKContext.CRONUS_International_Ltd__Zone as E
                                                                         where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed))
}</CommandText>
    <Parameters>
      <Parameter Name="MinAmountConsumed" DbType="Int32" Value="10000" />
    </Parameters>
  </Query>
```
