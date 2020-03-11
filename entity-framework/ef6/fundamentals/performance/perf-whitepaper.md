---
title: EF4, EF5 ve EF6-EF6 için performans konuları
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: 07eb605f0d39f0c1bcfe781540525180f0dd0b22
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419449"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a><span data-ttu-id="22b0f-102">EF 4, 5 ve 6 için performans konuları</span><span class="sxs-lookup"><span data-stu-id="22b0f-102">Performance considerations for EF 4, 5, and 6</span></span>
<span data-ttu-id="22b0f-103">David OBANDO, Eric Dettu ve diğerleri</span><span class="sxs-lookup"><span data-stu-id="22b0f-103">By David Obando, Eric Dettinger and others</span></span>

<span data-ttu-id="22b0f-104">Yayımlandı: Nisan 2012</span><span class="sxs-lookup"><span data-stu-id="22b0f-104">Published: April 2012</span></span>

<span data-ttu-id="22b0f-105">Son güncelleme tarihi: Mayıs 2014</span><span class="sxs-lookup"><span data-stu-id="22b0f-105">Last updated: May 2014</span></span>

------------------------------------------------------------------------

## <a name="1-introduction"></a><span data-ttu-id="22b0f-106">1. Giriş</span><span class="sxs-lookup"><span data-stu-id="22b0f-106">1. Introduction</span></span>

<span data-ttu-id="22b0f-107">Nesne Ilişkisel eşleme çerçeveleri, nesne odaklı bir uygulamada veri erişimi için bir soyutlama sağlamanın kullanışlı bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-107">Object-Relational Mapping frameworks are a convenient way to provide an abstraction for data access in an object-oriented application.</span></span> <span data-ttu-id="22b0f-108">.NET uygulamaları için Microsoft 'un önerilen O/RM Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="22b0f-108">For .NET applications, Microsoft's recommended O/RM is Entity Framework.</span></span> <span data-ttu-id="22b0f-109">Ancak, herhangi bir soyutlama ile performans sorun oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-109">With any abstraction though, performance can become a concern.</span></span>

<span data-ttu-id="22b0f-110">Bu Teknik İnceleme, Entity Framework kullanarak uygulamalar geliştirirken, geliştiricilere performansı etkileyebilecek Entity Framework Dahili algoritmalar hakkında fikir vermek ve araştırma için ipuçları sağlamak üzere yazılmıştır. Entity Framework kullanan uygulamalarında performansı artırma.</span><span class="sxs-lookup"><span data-stu-id="22b0f-110">This whitepaper was written to show the performance considerations when developing applications using Entity Framework, to give developers an idea of the Entity Framework internal algorithms that can affect performance, and to provide tips for investigation and improving performance in their applications that use Entity Framework.</span></span> <span data-ttu-id="22b0f-111">Web 'de zaten performans üzerinde bulunan çok sayıda iyi konu vardır ve mümkün olduğunda bu kaynaklara işaret eden bir daha da çalıştık.</span><span class="sxs-lookup"><span data-stu-id="22b0f-111">There are a number of good topics on performance already available on the web, and we've also tried pointing to these resources where possible.</span></span>

<span data-ttu-id="22b0f-112">Performans, karmaşık bir konudur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-112">Performance is a tricky topic.</span></span> <span data-ttu-id="22b0f-113">Bu Teknik İnceleme, Entity Framework kullanan uygulamalarınız için performans ile ilgili kararlar almanıza yardımcı olacak bir kaynak olarak hazırlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-113">This whitepaper is intended as a resource to help you make performance related decisions for your applications that use Entity Framework.</span></span> <span data-ttu-id="22b0f-114">Performansı göstermek için bazı test ölçümleri ekledik, ancak bu ölçümler uygulamanızda göreceğiniz performansın mutlak göstergeleri olarak tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-114">We have included some test metrics to demonstrate performance, but these metrics aren't intended as absolute indicators of the performance you will see in your application.</span></span>

<span data-ttu-id="22b0f-115">Pratik amaçlarla, bu belge Entity Framework 4 ' ün .NET 4,0 altında çalıştırıldığını varsayar ve Entity Framework 5 ve 6 .NET 4,5 altında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-115">For practical purposes, this document assumes Entity Framework 4 is run under .NET 4.0 and Entity Framework 5 and 6 are run under .NET 4.5.</span></span> <span data-ttu-id="22b0f-116">Entity Framework 5 ' te yapılan performans geliştirmelerinden birçoğu, .NET 4,5 ile birlikte gelen çekirdek bileşenler içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-116">Many of the performance improvements made for Entity Framework 5 reside within the core components that ship with .NET 4.5.</span></span>

<span data-ttu-id="22b0f-117">Entity Framework 6 bant dışı bir sürümdür ve .NET ile birlikte gelen Entity Framework bileşenlerine bağlı değildir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-117">Entity Framework 6 is an out of band release and does not depend on the Entity Framework components that ship with .NET.</span></span> <span data-ttu-id="22b0f-118">Entity Framework 6 hem .NET 4,0 hem de .NET 4,5 üzerinde çalışır ve .NET 4,0 'den yükseltilmeyen ancak uygulamalarınızda en son Entity Framework bitleri istediğiniz büyük bir performans avantajı sunabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-118">Entity Framework 6 work on both .NET 4.0 and .NET 4.5, and can offer a big performance benefit to those who haven’t upgraded from .NET 4.0 but want the latest Entity Framework bits in their application.</span></span> <span data-ttu-id="22b0f-119">Bu belgede Entity Framework 6 ' dan bahsetme, bu yazma sırasında bulunan en son sürüme başvurur: sürüm 6.1.0.</span><span class="sxs-lookup"><span data-stu-id="22b0f-119">When this document mentions Entity Framework 6, it refers to the latest version available at the time of this writing: version 6.1.0.</span></span>

## <a name="2-cold-vs-warm-query-execution"></a><span data-ttu-id="22b0f-120">2. soğuk ve sıcak sorgu yürütme</span><span class="sxs-lookup"><span data-stu-id="22b0f-120">2. Cold vs. Warm Query Execution</span></span>

<span data-ttu-id="22b0f-121">Bazı sorgular belirli bir modele göre ilk kez yapıldığında Entity Framework, arka planda modeli yüklemek ve doğrulamak için çok sayıda iş yapar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-121">The very first time any query is made against a given model, the Entity Framework does a lot of work behind the scenes to load and validate the model.</span></span> <span data-ttu-id="22b0f-122">Bu ilk sorguya "soğuk" sorgusu olarak sıklıkla başvurduk.</span><span class="sxs-lookup"><span data-stu-id="22b0f-122">We frequently refer to this first query as a "cold" query.</span></span><span data-ttu-id="22b0f-123">  Önceden yüklenmiş bir modele yönelik daha fazla sorgu "normal" sorgular olarak bilinir ve çok daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-123">  Further queries against an already loaded model are known as "warm" queries, and are much faster.</span></span>

<span data-ttu-id="22b0f-124">Entity Framework kullanarak bir sorgu yürütürken nerede harcandığına ilişkin üst düzey bir görünüm atalım ve Entity Framework 6 ' da nesnelerin nerede geliştiğini görün.</span><span class="sxs-lookup"><span data-stu-id="22b0f-124">Let’s take a high-level view of where time is spent when executing a query using Entity Framework, and see where things are improving in Entity Framework 6.</span></span>

<span data-ttu-id="22b0f-125">**İlk sorgu yürütme – soğuk sorgu**</span><span class="sxs-lookup"><span data-stu-id="22b0f-125">**First Query Execution – cold query**</span></span>

| <span data-ttu-id="22b0f-126">Kod Kullanıcı yazmaları</span><span class="sxs-lookup"><span data-stu-id="22b0f-126">Code User Writes</span></span>                                                                                     | <span data-ttu-id="22b0f-127">Eylem</span><span class="sxs-lookup"><span data-stu-id="22b0f-127">Action</span></span>                    | <span data-ttu-id="22b0f-128">EF4 performans etkisi</span><span class="sxs-lookup"><span data-stu-id="22b0f-128">EF4 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                        | <span data-ttu-id="22b0f-129">EF5 performans etkisi</span><span class="sxs-lookup"><span data-stu-id="22b0f-129">EF5 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                    | <span data-ttu-id="22b0f-130">EF6 performans etkisi</span><span class="sxs-lookup"><span data-stu-id="22b0f-130">EF6 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | <span data-ttu-id="22b0f-131">Bağlam oluşturma</span><span class="sxs-lookup"><span data-stu-id="22b0f-131">Context creation</span></span>          | <span data-ttu-id="22b0f-132">Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-132">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                        | <span data-ttu-id="22b0f-133">Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-133">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | <span data-ttu-id="22b0f-134">Düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-134">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | <span data-ttu-id="22b0f-135">Sorgu ifadesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="22b0f-135">Query expression creation</span></span> | <span data-ttu-id="22b0f-136">Düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-136">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="22b0f-137">Düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-137">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="22b0f-138">Düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-138">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | <span data-ttu-id="22b0f-139">LINQ sorgu yürütme</span><span class="sxs-lookup"><span data-stu-id="22b0f-139">LINQ query execution</span></span>      | <span data-ttu-id="22b0f-140">-Meta veri yükleme: yüksek ancak önbelleğe alınmış</span><span class="sxs-lookup"><span data-stu-id="22b0f-140">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="22b0f-141">-Üretimi görüntüle: büyük olasılıkla çok yüksek ancak önbelleğe alınmış</span><span class="sxs-lookup"><span data-stu-id="22b0f-141">- View generation: Potentially very high but cached</span></span> <br/> <span data-ttu-id="22b0f-142">-Parametre değerlendirmesi: Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-142">- Parameter evaluation: Medium</span></span> <br/> <span data-ttu-id="22b0f-143">-Sorgu çevirisi: Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-143">- Query translation: Medium</span></span> <br/> <span data-ttu-id="22b0f-144">-Materializer oluşturma: Orta ancak önbelleğe alınmış</span><span class="sxs-lookup"><span data-stu-id="22b0f-144">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="22b0f-145">-Veritabanı sorgusu yürütme: büyük olasılıkla yüksek</span><span class="sxs-lookup"><span data-stu-id="22b0f-145">- Database query execution: Potentially high</span></span> <br/> <span data-ttu-id="22b0f-146">+ Connection. Open</span><span class="sxs-lookup"><span data-stu-id="22b0f-146">+ Connection.Open</span></span> <br/> <span data-ttu-id="22b0f-147">+ Command. ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="22b0f-147">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="22b0f-148">+ DataReader. Read</span><span class="sxs-lookup"><span data-stu-id="22b0f-148">+ DataReader.Read</span></span> <br/> <span data-ttu-id="22b0f-149">Nesne materialization: Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-149">Object materialization: Medium</span></span> <br/> <span data-ttu-id="22b0f-150">-Kimlik arama: Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-150">- Identity lookup: Medium</span></span> | <span data-ttu-id="22b0f-151">-Meta veri yükleme: yüksek ancak önbelleğe alınmış</span><span class="sxs-lookup"><span data-stu-id="22b0f-151">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="22b0f-152">-Üretimi görüntüle: büyük olasılıkla çok yüksek ancak önbelleğe alınmış</span><span class="sxs-lookup"><span data-stu-id="22b0f-152">- View generation: Potentially very high but cached</span></span> <br/> <span data-ttu-id="22b0f-153">-Parametre değerlendirmesi: düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-153">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="22b0f-154">-Sorgu çevirisi: Orta ancak önbelleğe alınmış</span><span class="sxs-lookup"><span data-stu-id="22b0f-154">- Query translation: Medium but cached</span></span> <br/> <span data-ttu-id="22b0f-155">-Materializer oluşturma: Orta ancak önbelleğe alınmış</span><span class="sxs-lookup"><span data-stu-id="22b0f-155">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="22b0f-156">-Veritabanı sorgusu yürütme: potansiyel yüksek (bazı durumlarda daha Iyi sorgular)</span><span class="sxs-lookup"><span data-stu-id="22b0f-156">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="22b0f-157">+ Connection. Open</span><span class="sxs-lookup"><span data-stu-id="22b0f-157">+ Connection.Open</span></span> <br/> <span data-ttu-id="22b0f-158">+ Command. ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="22b0f-158">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="22b0f-159">+ DataReader. Read</span><span class="sxs-lookup"><span data-stu-id="22b0f-159">+ DataReader.Read</span></span> <br/> <span data-ttu-id="22b0f-160">Nesne materialization: Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-160">Object materialization: Medium</span></span> <br/> <span data-ttu-id="22b0f-161">-Kimlik arama: Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-161">- Identity lookup: Medium</span></span> | <span data-ttu-id="22b0f-162">-Meta veri yükleme: yüksek ancak önbelleğe alınmış</span><span class="sxs-lookup"><span data-stu-id="22b0f-162">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="22b0f-163">-Üretimi görüntüle: Orta ancak önbelleğe alınmış</span><span class="sxs-lookup"><span data-stu-id="22b0f-163">- View generation: Medium but cached</span></span> <br/> <span data-ttu-id="22b0f-164">-Parametre değerlendirmesi: düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-164">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="22b0f-165">-Sorgu çevirisi: Orta ancak önbelleğe alınmış</span><span class="sxs-lookup"><span data-stu-id="22b0f-165">- Query translation: Medium but cached</span></span> <br/> <span data-ttu-id="22b0f-166">-Materializer oluşturma: Orta ancak önbelleğe alınmış</span><span class="sxs-lookup"><span data-stu-id="22b0f-166">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="22b0f-167">-Veritabanı sorgusu yürütme: potansiyel yüksek (bazı durumlarda daha Iyi sorgular)</span><span class="sxs-lookup"><span data-stu-id="22b0f-167">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="22b0f-168">+ Connection. Open</span><span class="sxs-lookup"><span data-stu-id="22b0f-168">+ Connection.Open</span></span> <br/> <span data-ttu-id="22b0f-169">+ Command. ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="22b0f-169">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="22b0f-170">+ DataReader. Read</span><span class="sxs-lookup"><span data-stu-id="22b0f-170">+ DataReader.Read</span></span> <br/> <span data-ttu-id="22b0f-171">Nesne materialization: Orta (EF5 'den daha hızlı)</span><span class="sxs-lookup"><span data-stu-id="22b0f-171">Object materialization: Medium (Faster than EF5)</span></span> <br/> <span data-ttu-id="22b0f-172">-Kimlik arama: Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-172">- Identity lookup: Medium</span></span> |
| `}`                                                                                                  | <span data-ttu-id="22b0f-173">Connection. Close</span><span class="sxs-lookup"><span data-stu-id="22b0f-173">Connection.Close</span></span>          | <span data-ttu-id="22b0f-174">Düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-174">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="22b0f-175">Düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-175">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="22b0f-176">Düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-176">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


<span data-ttu-id="22b0f-177">**İkinci sorgu yürütme – sıcak sorgu**</span><span class="sxs-lookup"><span data-stu-id="22b0f-177">**Second Query Execution – warm query**</span></span>

| <span data-ttu-id="22b0f-178">Kod Kullanıcı yazmaları</span><span class="sxs-lookup"><span data-stu-id="22b0f-178">Code User Writes</span></span>                                                                                     | <span data-ttu-id="22b0f-179">Eylem</span><span class="sxs-lookup"><span data-stu-id="22b0f-179">Action</span></span>                    | <span data-ttu-id="22b0f-180">EF4 performans etkisi</span><span class="sxs-lookup"><span data-stu-id="22b0f-180">EF4 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | <span data-ttu-id="22b0f-181">EF5 performans etkisi</span><span class="sxs-lookup"><span data-stu-id="22b0f-181">EF5 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | <span data-ttu-id="22b0f-182">EF6 performans etkisi</span><span class="sxs-lookup"><span data-stu-id="22b0f-182">EF6 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | <span data-ttu-id="22b0f-183">Bağlam oluşturma</span><span class="sxs-lookup"><span data-stu-id="22b0f-183">Context creation</span></span>          | <span data-ttu-id="22b0f-184">Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-184">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | <span data-ttu-id="22b0f-185">Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-185">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | <span data-ttu-id="22b0f-186">Düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-186">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | <span data-ttu-id="22b0f-187">Sorgu ifadesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="22b0f-187">Query expression creation</span></span> | <span data-ttu-id="22b0f-188">Düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-188">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | <span data-ttu-id="22b0f-189">Düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-189">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | <span data-ttu-id="22b0f-190">Düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-190">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | <span data-ttu-id="22b0f-191">LINQ sorgu yürütme</span><span class="sxs-lookup"><span data-stu-id="22b0f-191">LINQ query execution</span></span>      | <span data-ttu-id="22b0f-192">-Meta veri ~~yükleme~~ araması: ~~yüksek ancak önbelleğe alınan~~ düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-192">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="22b0f-193">- ~~Oluşturma~~ aramasını görüntüle: ~~büyük olasılıkla çok yüksek ancak önbelleğe alınmış~~ düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-193">- View ~~generation~~ lookup: ~~Potentially very high but cached~~ Low</span></span> <br/> <span data-ttu-id="22b0f-194">-Parametre değerlendirmesi: Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-194">- Parameter evaluation: Medium</span></span> <br/> <span data-ttu-id="22b0f-195">-Sorgu ~~çevirisi~~ arama: Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-195">- Query ~~translation~~ lookup: Medium</span></span> <br/> <span data-ttu-id="22b0f-196">-Materializer ~~oluşturma~~ araması: ~~Orta ancak düşük önbelleğe alınmış~~</span><span class="sxs-lookup"><span data-stu-id="22b0f-196">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="22b0f-197">-Veritabanı sorgusu yürütme: büyük olasılıkla yüksek</span><span class="sxs-lookup"><span data-stu-id="22b0f-197">- Database query execution: Potentially high</span></span> <br/> <span data-ttu-id="22b0f-198">+ Connection. Open</span><span class="sxs-lookup"><span data-stu-id="22b0f-198">+ Connection.Open</span></span> <br/> <span data-ttu-id="22b0f-199">+ Command. ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="22b0f-199">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="22b0f-200">+ DataReader. Read</span><span class="sxs-lookup"><span data-stu-id="22b0f-200">+ DataReader.Read</span></span> <br/> <span data-ttu-id="22b0f-201">Nesne materialization: Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-201">Object materialization: Medium</span></span> <br/> <span data-ttu-id="22b0f-202">-Kimlik arama: Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-202">- Identity lookup: Medium</span></span> | <span data-ttu-id="22b0f-203">-Meta veri ~~yükleme~~ araması: ~~yüksek ancak önbelleğe alınan~~ düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-203">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="22b0f-204">- ~~Oluşturma~~ aramasını görüntüle: ~~büyük olasılıkla çok yüksek ancak önbelleğe alınmış~~ düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-204">- View ~~generation~~ lookup: ~~Potentially very high but cached~~ Low</span></span> <br/> <span data-ttu-id="22b0f-205">-Parametre değerlendirmesi: düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-205">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="22b0f-206">-Sorgu ~~çevirisi~~ arama: ~~Orta ancak düşük önbelleğe alınmış~~</span><span class="sxs-lookup"><span data-stu-id="22b0f-206">- Query ~~translation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="22b0f-207">-Materializer ~~oluşturma~~ araması: ~~Orta ancak düşük önbelleğe alınmış~~</span><span class="sxs-lookup"><span data-stu-id="22b0f-207">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="22b0f-208">-Veritabanı sorgusu yürütme: potansiyel yüksek (bazı durumlarda daha Iyi sorgular)</span><span class="sxs-lookup"><span data-stu-id="22b0f-208">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="22b0f-209">+ Connection. Open</span><span class="sxs-lookup"><span data-stu-id="22b0f-209">+ Connection.Open</span></span> <br/> <span data-ttu-id="22b0f-210">+ Command. ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="22b0f-210">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="22b0f-211">+ DataReader. Read</span><span class="sxs-lookup"><span data-stu-id="22b0f-211">+ DataReader.Read</span></span> <br/> <span data-ttu-id="22b0f-212">Nesne materialization: Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-212">Object materialization: Medium</span></span> <br/> <span data-ttu-id="22b0f-213">-Kimlik arama: Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-213">- Identity lookup: Medium</span></span> | <span data-ttu-id="22b0f-214">-Meta veri ~~yükleme~~ araması: ~~yüksek ancak önbelleğe alınan~~ düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-214">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="22b0f-215">- ~~Oluşturma~~ aramasını görüntüle: ~~Orta ancak düşük önbelleğe alınmış~~</span><span class="sxs-lookup"><span data-stu-id="22b0f-215">- View ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="22b0f-216">-Parametre değerlendirmesi: düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-216">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="22b0f-217">-Sorgu ~~çevirisi~~ arama: ~~Orta ancak düşük önbelleğe alınmış~~</span><span class="sxs-lookup"><span data-stu-id="22b0f-217">- Query ~~translation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="22b0f-218">-Materializer ~~oluşturma~~ araması: ~~Orta ancak düşük önbelleğe alınmış~~</span><span class="sxs-lookup"><span data-stu-id="22b0f-218">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="22b0f-219">-Veritabanı sorgusu yürütme: potansiyel yüksek (bazı durumlarda daha Iyi sorgular)</span><span class="sxs-lookup"><span data-stu-id="22b0f-219">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="22b0f-220">+ Connection. Open</span><span class="sxs-lookup"><span data-stu-id="22b0f-220">+ Connection.Open</span></span> <br/> <span data-ttu-id="22b0f-221">+ Command. ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="22b0f-221">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="22b0f-222">+ DataReader. Read</span><span class="sxs-lookup"><span data-stu-id="22b0f-222">+ DataReader.Read</span></span> <br/> <span data-ttu-id="22b0f-223">Nesne materialization: Orta (EF5 'den daha hızlı)</span><span class="sxs-lookup"><span data-stu-id="22b0f-223">Object materialization: Medium (Faster than EF5)</span></span> <br/> <span data-ttu-id="22b0f-224">-Kimlik arama: Orta</span><span class="sxs-lookup"><span data-stu-id="22b0f-224">- Identity lookup: Medium</span></span> |
| `}`                                                                                                  | <span data-ttu-id="22b0f-225">Connection. Close</span><span class="sxs-lookup"><span data-stu-id="22b0f-225">Connection.Close</span></span>          | <span data-ttu-id="22b0f-226">Düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-226">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | <span data-ttu-id="22b0f-227">Düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-227">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | <span data-ttu-id="22b0f-228">Düşük</span><span class="sxs-lookup"><span data-stu-id="22b0f-228">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


<span data-ttu-id="22b0f-229">Hem soğuk hem de sıcak sorguların performans maliyetini düşürmenin birkaç yolu vardır ve aşağıdaki bölümde bunlara göz atalım.</span><span class="sxs-lookup"><span data-stu-id="22b0f-229">There are several ways to reduce the performance cost of both cold and warm queries, and we'll take a look at these in the following section.</span></span> <span data-ttu-id="22b0f-230">Özellikle, görünüm oluşturma sırasında karşılaşılan performans pakanları azaltmaya yardımcı olması gereken önceden oluşturulmuş görünümler kullanarak, soğuk sorgularda model yükleme maliyetini azaltmaya bakacağız.</span><span class="sxs-lookup"><span data-stu-id="22b0f-230">Specifically, we'll look at reducing the cost of model loading in cold queries by using pre-generated views, which should help alleviate performance pains experienced during view generation.</span></span> <span data-ttu-id="22b0f-231">Isınma sorguları için sorgu planı önbelleğe alma, izleme sorgusu yok ve farklı sorgu yürütme seçenekleri ele alınacaktır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-231">For warm queries, we'll cover query plan caching, no tracking queries, and different query execution options.</span></span>

### <a name="21-what-is-view-generation"></a><span data-ttu-id="22b0f-232">2,1 görünüm oluşturma nedir?</span><span class="sxs-lookup"><span data-stu-id="22b0f-232">2.1 What is View Generation?</span></span>

<span data-ttu-id="22b0f-233">Hangi görünüm oluşturmanın olduğunu anlamak için öncelikle "eşleme görünümlerini" ne olduğunu anlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-233">In order to understand what view generation is, we must first understand what “Mapping Views” are.</span></span> <span data-ttu-id="22b0f-234">Eşleme görünümleri, her bir varlık kümesi ve ilişkilendirmesi için eşlemede belirtilen dönüştürmelerin yürütülebilir temsilleridir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-234">Mapping Views are executable representations of the transformations specified in the mapping for each entity set and association.</span></span> <span data-ttu-id="22b0f-235">Dahili olarak, bu eşleme görünümleri CQTs (kurallı sorgu ağaçları) şeklini alır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-235">Internally, these mapping views take the shape of CQTs (canonical query trees).</span></span> <span data-ttu-id="22b0f-236">İki tür eşleme görünümü vardır:</span><span class="sxs-lookup"><span data-stu-id="22b0f-236">There are two types of mapping views:</span></span>

-   <span data-ttu-id="22b0f-237">Sorgu görünümleri: Bu, veritabanı şemasından kavramsal modele gitmek için gereken dönüşümü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="22b0f-237">Query views: these represent the transformation necessary to go from the database schema to the conceptual model.</span></span>
-   <span data-ttu-id="22b0f-238">Güncelleştirme görünümleri: Bu, kavramsal modelden veritabanı şemasına gitmek için gereken dönüşümü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="22b0f-238">Update views: these represent the transformation necessary to go from the conceptual model to the database schema.</span></span>

<span data-ttu-id="22b0f-239">Kavramsal modelin, veritabanı şemasından çeşitli yollarla farklı olabileceğini aklınızda bulundurun.</span><span class="sxs-lookup"><span data-stu-id="22b0f-239">Keep in mind that the conceptual model might differ from the database schema in various ways.</span></span> <span data-ttu-id="22b0f-240">Örneğin, bir tek tablo, verileri iki farklı varlık türü için depolamak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-240">For example, one single table might be used to store the data for two different entity types.</span></span> <span data-ttu-id="22b0f-241">Devralma ve önemsiz olmayan eşlemeler, eşleme görünümlerinin karmaşıklığından bir rol oynar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-241">Inheritance and non-trivial mappings play a role in the complexity of the mapping views.</span></span>

<span data-ttu-id="22b0f-242">Bu görünümleri eşlemenin belirtimine göre hesaplama işlemi, görünüm oluşturmayı çağırdığımız şeydir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-242">The process of computing these views based on the specification of the mapping is what we call view generation.</span></span> <span data-ttu-id="22b0f-243">Görünüm üretimi, bir model yüklendiğinde veya derleme zamanında "önceden oluşturulmuş görünümler" kullanılarak dinamik olarak yapılabilir; İkincisi, Entity SQL deyimleri biçiminde bir C\# veya VB dosyasına serileştirilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-243">View generation can either take place dynamically when a model is loaded, or at build time, by using "pre-generated views"; the latter are serialized in the form of Entity SQL statements to a C\# or VB file.</span></span>

<span data-ttu-id="22b0f-244">Görünümler oluşturulduğunda, bunlar da doğrulanmaz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-244">When views are generated, they are also validated.</span></span> <span data-ttu-id="22b0f-245">Performans açısından, görünüm oluşturma maliyetinin büyük bir bölümü aslında, varlıklar arasındaki bağlantıların anlamlı hale gelmesini ve desteklenen tüm işlemler için doğru kardinalite olmasını sağlayan görünümlerin doğrulanmasından oluşur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-245">From a performance standpoint, the vast majority of the cost of view generation is actually the validation of the views which ensures that the connections between the entities make sense and have the correct cardinality for all the supported operations.</span></span>

<span data-ttu-id="22b0f-246">Bir varlık kümesi üzerinde bir sorgu yürütüldüğünde, sorgu ilgili sorgu görünümüyle birleştirilir ve bu birleşimin sonucu, yedekleme deposunun anlayabileceği sorgunun gösterimini oluşturmak için plan derleyicisi aracılığıyla çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-246">When a query over an entity set is executed, the query is combined with the corresponding query view, and the result of this composition is run through the plan compiler to create the representation of the query that the backing store can understand.</span></span> <span data-ttu-id="22b0f-247">SQL Server için, bu derlemenin nihai sonucu bir T-SQL SELECT deyimidir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-247">For SQL Server, the final result of this compilation will be a T-SQL SELECT statement.</span></span> <span data-ttu-id="22b0f-248">Bir varlık kümesi üzerinde ilk kez güncelleştirme gerçekleştirildiğinde güncelleştirme görünümü, hedef veritabanı için DML deyimlerine dönüştürmek üzere benzer bir işlem aracılığıyla çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-248">The first time an update over an entity set is performed, the update view is run through a similar process to transform it into DML statements for the target database.</span></span>

### <a name="22-factors-that-affect-view-generation-performance"></a><span data-ttu-id="22b0f-249">Görünüm oluşturma performansını etkileyen 2,2 faktörleri</span><span class="sxs-lookup"><span data-stu-id="22b0f-249">2.2 Factors that affect View Generation performance</span></span>

<span data-ttu-id="22b0f-250">Görünüm oluşturma adımının performansı yalnızca modelinizin boyutuna ve ayrıca modelin nasıl bağlantılı olduğuna bağlı olarak farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-250">The performance of view generation step not only depends on the size of your model but also on how interconnected the model is.</span></span> <span data-ttu-id="22b0f-251">İki varlık bir devralma zinciri veya bir Ilişki aracılığıyla bağlandıysa, bağlandıkları söylenir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-251">If two Entities are connected via an inheritance chain or an Association, they are said to be connected.</span></span> <span data-ttu-id="22b0f-252">Benzer şekilde, iki tablo bir yabancı anahtar aracılığıyla bağlandıysa, bunlar bağlanır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-252">Similarly if two tables are connected via a foreign key, they are connected.</span></span> <span data-ttu-id="22b0f-253">Şemalarınızdaki bağlantılı varlıkların ve tabloların sayısı arttıkça, görünüm oluşturma maliyeti artar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-253">As the number of connected Entities and tables in your schemas increase, the view generation cost increases.</span></span>

<span data-ttu-id="22b0f-254">Görünümleri oluşturmak ve doğrulamak için kullandığımız algoritma en kötü durumda üstel olur, ancak bunu geliştirmek için bazı iyileştirmeler kullanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-254">The algorithm that we use to generate and validate views is exponential in the worst case, though we do use some optimizations to improve this.</span></span> <span data-ttu-id="22b0f-255">Performansı olumsuz yönde etkileyen en büyük faktörler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="22b0f-255">The biggest factors that seem to negatively affect performance are:</span></span>

-   <span data-ttu-id="22b0f-256">Model boyutu, varlıkların sayısına ve bu varlıklar arasındaki ilişkilerin miktarına işaret eder.</span><span class="sxs-lookup"><span data-stu-id="22b0f-256">Model size, referring to the number of entities and the amount of associations between these entities.</span></span>
-   <span data-ttu-id="22b0f-257">Model karmaşıklığı, özellikle de çok sayıda tür içeren devralma.</span><span class="sxs-lookup"><span data-stu-id="22b0f-257">Model complexity, specifically inheritance involving a large number of types.</span></span>
-   <span data-ttu-id="22b0f-258">Yabancı anahtar Ilişkilendirmeleri yerine bağımsız Ilişkilendirmeler kullanma.</span><span class="sxs-lookup"><span data-stu-id="22b0f-258">Using Independent Associations, instead of Foreign Key Associations.</span></span>

<span data-ttu-id="22b0f-259">Küçük ve basit modeller için maliyet, önceden üretilmiş görünümler kullanılarak oluşmayacak kadar küçük olabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-259">For small, simple models the cost may be small enough to not bother using pre-generated views.</span></span> <span data-ttu-id="22b0f-260">Model boyutu ve karmaşıklığı artdıkça, görünüm oluşturma ve doğrulama maliyetini azaltmak için çeşitli seçenekler mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-260">As model size and complexity increase, there are several options available to reduce the cost of view generation and validation.</span></span>

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a><span data-ttu-id="22b0f-261">2,3 model yükleme süresini azaltmak için önceden oluşturulmuş görünümleri kullanma</span><span class="sxs-lookup"><span data-stu-id="22b0f-261">2.3 Using Pre-Generated Views to decrease model load time</span></span>

<span data-ttu-id="22b0f-262">Entity Framework 6 ' da önceden oluşturulmuş görünümleri kullanma hakkında ayrıntılı bilgi için [önceden oluşturulmuş eşleme görünümlerini](~/ef6/fundamentals/performance/pre-generated-views.md) ziyaret edin</span><span class="sxs-lookup"><span data-stu-id="22b0f-262">For detailed information on how to use pre-generated views on Entity Framework 6 visit [Pre-Generated Mapping Views](~/ef6/fundamentals/performance/pre-generated-views.md)</span></span>

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools-community-edition"></a><span data-ttu-id="22b0f-263">Entity Framework Power Tools Community Edition 'ı kullanarak önceden üretilmiş Görünümler 2.3.1</span><span class="sxs-lookup"><span data-stu-id="22b0f-263">2.3.1 Pre-Generated views using the Entity Framework Power Tools Community Edition</span></span>

<span data-ttu-id="22b0f-264">Model sınıf dosyasına sağ tıklayıp "görünüm üret" i seçmek için Entity Framework menüsünü kullanarak EDMX ve Code First modellerinin görünümlerini oluşturmak için [Entity Framework 6 Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) 'ı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-264">You can use the [Entity Framework 6 Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) to generate views of EDMX and Code First models by right-clicking the model class file and using the Entity Framework menu to select “Generate Views”.</span></span> <span data-ttu-id="22b0f-265">Entity Framework Power Tools Community Edition yalnızca DbContext ile türetilmiş bağlamlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-265">The Entity Framework Power Tools Community Edition work only on DbContext-derived contexts.</span></span>

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a><span data-ttu-id="22b0f-266">2.3.2, EDMGen tarafından oluşturulan bir modelle önceden oluşturulmuş görünümleri nasıl kullanacağınızı</span><span class="sxs-lookup"><span data-stu-id="22b0f-266">2.3.2 How to use Pre-generated views with a model created by EDMGen</span></span>

<span data-ttu-id="22b0f-267">EDMGen, .NET ile birlikte gelen ve Entity Framework 4 ve 5 ile çalışan, ancak Entity Framework 6 ile birlikte çalışan bir yardımcı programdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-267">EDMGen is a utility that ships with .NET and works with Entity Framework 4 and 5, but not with Entity Framework 6.</span></span> <span data-ttu-id="22b0f-268">EDMGen, komut satırından bir model dosyası, nesne katmanı ve görünümler oluşturmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-268">EDMGen allows you to generate a model file, the object layer and the views from the command line.</span></span> <span data-ttu-id="22b0f-269">Çıktılardan biri, seçim, VB veya C\#dilinizdeki bir görünümler dosyası olacaktır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-269">One of the outputs will be a Views file in your language of choice, VB or C\#.</span></span> <span data-ttu-id="22b0f-270">Bu, her varlık kümesi için Entity SQL parçacıkları içeren bir kod dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-270">This is a code file containing Entity SQL snippets for each entity set.</span></span> <span data-ttu-id="22b0f-271">Önceden oluşturulmuş görünümleri etkinleştirmek için, dosyayı projenize eklemeniz yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-271">To enable pre-generated views, you simply include the file in your project.</span></span>

<span data-ttu-id="22b0f-272">Model için şema dosyalarında el ile düzenleme yaparsanız, görünümler dosyasını yeniden oluşturmanız gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-272">If you manually make edits to the schema files for the model, you will need to re-generate the views file.</span></span> <span data-ttu-id="22b0f-273">Bunu, **/Mode: ViewGeneration** bayrağıyla EDMGen çalıştırarak yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-273">You can do this by running EDMGen with the **/mode:ViewGeneration** flag.</span></span>

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a><span data-ttu-id="22b0f-274">2.3.3 bir EDMX dosyası ile önceden oluşturulmuş görünümleri kullanma</span><span class="sxs-lookup"><span data-stu-id="22b0f-274">2.3.3 How to use Pre-Generated Views with an EDMX file</span></span>

<span data-ttu-id="22b0f-275">Ayrıca, bir EDMX dosyasının görünümlerini oluşturmak için EDMGen ' yı kullanabilirsiniz. daha önce başvurulan MSDN konusu bunu yapmak için oluşturma öncesi bir olay eklemeyi açıklar; ancak bu karmaşıktır ve mümkün olmayan bazı durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-275">You can also use EDMGen to generate views for an EDMX file - the previously referenced MSDN topic describes how to add a pre-build event to do this - but this is complicated and there are some cases where it isn't possible.</span></span> <span data-ttu-id="22b0f-276">Modeliniz bir edmx dosyasında olduğunda görünümleri oluşturmak için bir T4 şablonu kullanmak genellikle daha kolay olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-276">It's generally easier to use a T4 template to generate the views when your model is in an edmx file.</span></span>

<span data-ttu-id="22b0f-277">ADO.NET ekip blogu, görünüm oluşturma (\<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>)için T4 şablonunun nasıl kullanılacağını açıklayan bir gönderisine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-277">The ADO.NET team blog has a post that describes how to use a T4 template for view generation ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>).</span></span> <span data-ttu-id="22b0f-278">Bu gönderi, indirilip projenize eklenebilen bir şablon içerir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-278">This post includes a template that can be downloaded and added to your project.</span></span> <span data-ttu-id="22b0f-279">Şablon, Entity Framework ilk sürümü için yazıldığı için, en son Entity Framework sürümleriyle çalışmayı garanti edilmez.</span><span class="sxs-lookup"><span data-stu-id="22b0f-279">The template was written for the first version of Entity Framework, so they aren’t guaranteed to work with the latest versions of Entity Framework.</span></span> <span data-ttu-id="22b0f-280">Ancak, Visual Studio galerisinden Entity Framework 4 ve 5 için daha güncel bir görünüm oluşturma şablonları yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="22b0f-280">However, you can download a more up-to-date set of view generation templates for Entity Framework 4 and 5from the Visual Studio Gallery:</span></span>

-   <span data-ttu-id="22b0f-281">VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d></span><span class="sxs-lookup"><span data-stu-id="22b0f-281">VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d></span></span>
-   <span data-ttu-id="22b0f-282">C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d></span><span class="sxs-lookup"><span data-stu-id="22b0f-282">C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d></span></span>

<span data-ttu-id="22b0f-283">Entity Framework 6 kullanıyorsanız, Visual Studio galerisindeki görünüm üretimi T4 şablonlarını \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-283">If you’re using Entity Framework 6 you can get the view generation T4 templates from the Visual Studio Gallery at \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.</span></span>

### <a name="24-reducing-the-cost-of-view-generation"></a><span data-ttu-id="22b0f-284">2,4 görüntü oluşturma maliyetini azaltma</span><span class="sxs-lookup"><span data-stu-id="22b0f-284">2.4 Reducing the cost of view generation</span></span>

<span data-ttu-id="22b0f-285">Önceden oluşturulmuş görünümleri kullanmak, görünüm oluşturma maliyetini model yüklemeden (çalışma zamanı) tasarım zamanına taşıdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-285">Using pre-generated views moves the cost of view generation from model loading (run time) to design time.</span></span> <span data-ttu-id="22b0f-286">Bu, çalışma zamanında başlangıç performansını artırırken, geliştirilirken görünüm oluşturma konusunda hala sorun yaşar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-286">While this improves startup performance at runtime, you will still experience the pain of view generation while you are developing.</span></span> <span data-ttu-id="22b0f-287">Hem derleme zamanında hem de çalışma zamanında görünüm oluşturma maliyetini azaltmaya yardımcı olabilecek çeşitli ek püf noktaları vardır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-287">There are several additional tricks that can help reduce the cost of view generation, both at compile time and run time.</span></span>

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a><span data-ttu-id="22b0f-288">2.4.1, görünüm oluşturma maliyetini azaltmak için yabancı anahtar Ilişkilendirmelerini kullanma</span><span class="sxs-lookup"><span data-stu-id="22b0f-288">2.4.1 Using Foreign Key Associations to reduce view generation cost</span></span>

<span data-ttu-id="22b0f-289">Modeldeki ilişkilerin bağımsız ilişkilerden yabancı anahtar Ilişkilendirmelerine geçişi, görünüm oluşturma sırasında harcanan sürenin önemli ölçüde iyileştirilmesinden çok sayıda durum gördük.</span><span class="sxs-lookup"><span data-stu-id="22b0f-289">We have seen a number of cases where switching the associations in the model from Independent Associations to Foreign Key Associations dramatically improved the time spent in view generation.</span></span>

<span data-ttu-id="22b0f-290">Bu geliştirmeyi göstermek için, EDMGen kullanarak Navision modelinin iki sürümünü oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="22b0f-290">To demonstrate this improvement, we generated two versions of the Navision model by using EDMGen.</span></span> <span data-ttu-id="22b0f-291">*Note: Navision modelinin açıklaması için bkz. Ek C.*</span><span class="sxs-lookup"><span data-stu-id="22b0f-291">*Note: see appendix C for a description of the Navision model.*</span></span> <span data-ttu-id="22b0f-292">Navision modeli, Bu alıştırmada aralarındaki çok büyük miktarda varlık ve ilişki olması nedeniyle bu alıştırma için ilginç.</span><span class="sxs-lookup"><span data-stu-id="22b0f-292">The Navision model is interesting for this exercise due to its very large amount of entities and relationships between them.</span></span>

<span data-ttu-id="22b0f-293">Bu çok büyük modelin bir sürümü yabancı anahtarlar Ilişkilendirmeleriyle oluşturulmuştur ve diğeri bağımsız Ilişkilendirmelerde oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-293">One version of this very large model was generated with Foreign Keys Associations and the other was generated with Independent Associations.</span></span> <span data-ttu-id="22b0f-294">Daha sonra her bir model için görünümleri oluşturma süresinin ne kadar sürdüğünü zaman aşımına uğradık.</span><span class="sxs-lookup"><span data-stu-id="22b0f-294">We then timed how long it took to generate the views for each model.</span></span> <span data-ttu-id="22b0f-295">Entity Framework 5 testi, görünümleri oluşturmak için EntityViewGenerator sınıfından GenerateViews () yöntemini kullandı, Entity Framework 6 testi StorageMappingItemCollection sınıfından GenerateViews () yöntemini kullandı.</span><span class="sxs-lookup"><span data-stu-id="22b0f-295">Entity Framework 5 test used the GenerateViews() method from class EntityViewGenerator to generate the views, while the Entity Framework 6 test used the GenerateViews() method from class StorageMappingItemCollection.</span></span> <span data-ttu-id="22b0f-296">Bu, Entity Framework 6 kod tabanında oluşan kod yeniden yapılandırma nedeniyle oluştu.</span><span class="sxs-lookup"><span data-stu-id="22b0f-296">This due to code restructuring that occurred in the Entity Framework 6 codebase.</span></span>

<span data-ttu-id="22b0f-297">Entity Framework 5 ' ü kullanarak, yabancı anahtarlarla model oluşturma, bir laboratuvar makinesinde 65 dakika sürdü.</span><span class="sxs-lookup"><span data-stu-id="22b0f-297">Using Entity Framework 5, view generation for the model with Foreign Keys took 65 minutes in a lab machine.</span></span> <span data-ttu-id="22b0f-298">Bağımsız İlişkilendirmelerin kullanıldığı modelin görünümlerini oluşturmak için ne kadar süre sürdüğünü bilinmiyor.</span><span class="sxs-lookup"><span data-stu-id="22b0f-298">It's unknown how long it would have taken to generate the views for the model that used independent associations.</span></span> <span data-ttu-id="22b0f-299">Aylık güncelleştirmeleri yüklemek için makineyi laboratuvarımızda yeniden başlatılmadan önce, çalıştırılan testi bir ay boyunca çalıştırdık.</span><span class="sxs-lookup"><span data-stu-id="22b0f-299">We left the test running for over a month before the machine was rebooted in our lab to install monthly updates.</span></span>

<span data-ttu-id="22b0f-300">Entity Framework 6 ' yı kullanarak, yabancı anahtarlarla model oluşturma işlemi aynı laboratuvar makinesinde 28 saniye sürdü.</span><span class="sxs-lookup"><span data-stu-id="22b0f-300">Using Entity Framework 6, view generation for the model with Foreign Keys took 28 seconds in the same lab machine.</span></span> <span data-ttu-id="22b0f-301">Bağımsız Ilişkilerin kullanıldığı model için görünüm üretimi 58 saniye sürdü.</span><span class="sxs-lookup"><span data-stu-id="22b0f-301">View generation for the model that uses Independent Associations took 58 seconds.</span></span> <span data-ttu-id="22b0f-302">Görünüm oluşturma kodu üzerinde 6 Entity Framework yapılan iyileştirmeler, birçok projenin daha hızlı başlangıç süreleri elde etmek için önceden oluşturulmuş görünümlere gerek duymayacağına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-302">The improvements done to Entity Framework 6 on its view generation code mean that many projects won’t need pre-generated views to obtain faster startup times.</span></span>

<span data-ttu-id="22b0f-303">Entity Framework 4 ve 5 ' teki önceden üretilen görünümlerin EDMGen veya Entity Framework güç araçlarıyla yapılabilirler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-303">It’s important to remark that pre-generating views in Entity Framework 4 and 5 can be done with EDMGen or the Entity Framework Power Tools.</span></span> <span data-ttu-id="22b0f-304">Entity Framework 6 görünüm üretimi için, [önceden oluşturulmuş eşleme görünümlerinde](~/ef6/fundamentals/performance/pre-generated-views.md)açıklandığı şekilde, Entity Framework güç araçları veya program aracılığıyla yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-304">For Entity Framework 6 view generation can be done via the Entity Framework Power Tools or programmatically as described in [Pre-Generated Mapping Views](~/ef6/fundamentals/performance/pre-generated-views.md).</span></span>

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a><span data-ttu-id="22b0f-305">2.4.1.1 bağımsız Ilişkilendirmeler yerine yabancı anahtarlar kullanma</span><span class="sxs-lookup"><span data-stu-id="22b0f-305">2.4.1.1 How to use Foreign Keys instead of Independent Associations</span></span>

<span data-ttu-id="22b0f-306">EDMGen veya Visual Studio 'da Entity Desisgner kullanırken, varsayılan olarak, FKs 'ler ve IAS arasında geçiş yapmak için yalnızca tek bir onay kutusu ya da komut satırı bayrağı alır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-306">When using EDMGen or the Entity Designer in Visual Studio, you get FKs by default, and it only takes a single checkbox or command line flag to switch between FKs and IAs.</span></span>

<span data-ttu-id="22b0f-307">Büyük bir Code First modeliniz varsa, bağımsız Ilişkilerin kullanılması görünüm üretimi üzerinde aynı etkiye sahip olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-307">If you have a large Code First model, using Independent Associations will have the same effect on view generation.</span></span> <span data-ttu-id="22b0f-308">Bağımlı nesnelerinizin sınıflarına yabancı anahtar özellikleri ekleyerek bu etkiden kaçınabilirsiniz, ancak bazı geliştiriciler bunu nesne modellerini yoklamak üzere kabul eder.</span><span class="sxs-lookup"><span data-stu-id="22b0f-308">You can avoid this impact by including Foreign Key properties on the classes for your dependent objects, though some developers will consider this to be polluting their object model.</span></span> <span data-ttu-id="22b0f-309">Bu konuyla ilgili daha fazla bilgiyi \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-309">You can find more information on this subject in \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.</span></span>

| <span data-ttu-id="22b0f-310">Kullanırken</span><span class="sxs-lookup"><span data-stu-id="22b0f-310">When using</span></span>      | <span data-ttu-id="22b0f-311">Bunu yapın</span><span class="sxs-lookup"><span data-stu-id="22b0f-311">Do this</span></span>                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="22b0f-312">Entity Desisgner</span><span class="sxs-lookup"><span data-stu-id="22b0f-312">Entity Designer</span></span> | <span data-ttu-id="22b0f-313">İki varlık arasında bir ilişki ekledikten sonra, bir başvuru kısıtlamasına sahip olduğunuzdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="22b0f-313">After adding an association between two entities, make sure you have a referential constraint.</span></span> <span data-ttu-id="22b0f-314">Başvurusal kısıtlamalar Entity Framework bağımsız Ilişkilendirmeler yerine yabancı anahtarlar kullanmasını söyler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-314">Referential constraints tell Entity Framework to use Foreign Keys instead of Independent Associations.</span></span> <span data-ttu-id="22b0f-315">Daha fazla bilgi için \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="22b0f-315">For additional details visit \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>.</span></span> |
| <span data-ttu-id="22b0f-316">EDMGen</span><span class="sxs-lookup"><span data-stu-id="22b0f-316">EDMGen</span></span>          | <span data-ttu-id="22b0f-317">Veritabanından dosyaları oluşturmak için EDMGen kullanıldığında, yabancı anahtarlarınız dikkate alınır ve bu şekilde modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-317">When using EDMGen to generate your files from the database, your Foreign Keys will be respected and added to the model as such.</span></span> <span data-ttu-id="22b0f-318">EDMGen tarafından sunulan farklı seçenekler hakkında daha fazla bilgi için [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx)ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="22b0f-318">For more information on the different options exposed by EDMGen visit [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx).</span></span>                           |
| <span data-ttu-id="22b0f-319">Code First</span><span class="sxs-lookup"><span data-stu-id="22b0f-319">Code First</span></span>      | <span data-ttu-id="22b0f-320">Code First kullanırken bağımlı nesnelere yabancı anahtar özellikleri ekleme hakkında bilgi için [Code First kuralları](~/ef6/modeling/code-first/conventions/built-in.md) konusunun "ilişki kuralı" bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-320">See the "Relationship Convention" section of the [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) topic for information on how to include foreign key properties on dependent objects when using Code First.</span></span>                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a><span data-ttu-id="22b0f-321">2.4.2 sections modelinizi ayrı bir derlemeye taşıma</span><span class="sxs-lookup"><span data-stu-id="22b0f-321">2.4.2 Moving your model to a separate assembly</span></span>

<span data-ttu-id="22b0f-322">Modeliniz doğrudan uygulamanızın projesine dahil edildiğinde ve oluşturma öncesi bir olay veya T4 şablonu aracılığıyla görünümler oluşturursanız, model değiştirilmese bile, oluşturma ve doğrulama proje yeniden oluşturulduğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-322">When your model is included directly in your application's project and you generate views through a pre-build event or a T4 template, view generation and validation will take place whenever the project is rebuilt, even if the model wasn't changed.</span></span> <span data-ttu-id="22b0f-323">Modeli ayrı bir derlemeye taşır ve uygulamanızın projesinden başvurmanız durumunda, modeli içeren projeyi yeniden derlemeye gerek kalmadan uygulamanızda başka değişiklikler yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-323">If you move the model to a separate assembly and reference it from your application's project, you can make other changes to your application without needing to rebuild the project containing the model.</span></span>

<span data-ttu-id="22b0f-324">*Note:* modelinizi ayrı derlemelere taşırken  , modelin bağlantı dizelerini istemci projesinin uygulama yapılandırma dosyasına kopyalamayı unutmayın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-324">*Note:*  when moving your model to separate assemblies remember to copy the connection strings for the model into the application configuration file of the client project.</span></span>

#### <a name="243-disable-validation-of-an-edmx-based-model"></a><span data-ttu-id="22b0f-325">2.4.3 bir edmx tabanlı modelin doğrulanmasını devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="22b0f-325">2.4.3 Disable validation of an edmx-based model</span></span>

<span data-ttu-id="22b0f-326">Model değiştirilmemiş olsa bile, EDMX modelleri derleme zamanında onaylanır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-326">EDMX models are validated at compile time, even if the model is unchanged.</span></span> <span data-ttu-id="22b0f-327">Modeliniz zaten doğrulanmışsa, Özellikler penceresinde "derleme üzerinde Doğrula" özelliğini ayarlayarak derleme zamanında doğrulamayı gizleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-327">If your model has already been validated, you can suppress validation at compile time by setting the "Validate on Build" property to false in the properties window.</span></span> <span data-ttu-id="22b0f-328">Eşlemenizi veya modelinizi değiştirdiğinizde, değişikliklerinizi doğrulamak için geçici olarak doğrulamayı yeniden etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-328">When you change your mapping or model, you can temporarily re-enable validation to verify your changes.</span></span>

<span data-ttu-id="22b0f-329">Entity Framework 6 için Entity Framework Designer performans geliştirmeleri yapılmıştır ve "derleme üzerinde Doğrula" maliyeti, tasarımcının önceki sürümlerinden çok daha düşüktür.</span><span class="sxs-lookup"><span data-stu-id="22b0f-329">Note that performance improvements were made to the Entity Framework Designer for Entity Framework 6, and the cost of the “Validate on Build” is much lower than in previous versions of the designer.</span></span>

## <a name="3-caching-in-the-entity-framework"></a><span data-ttu-id="22b0f-330">Entity Framework 3 önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="22b0f-330">3 Caching in the Entity Framework</span></span>

<span data-ttu-id="22b0f-331">Entity Framework, aşağıdaki önbelleğe alma yerleşik biçimlerine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="22b0f-331">Entity Framework has the following forms of caching built-in:</span></span>

1.  <span data-ttu-id="22b0f-332">Nesne önbelleğe alma – bir ObjectContext örneğine yerleşik olan ObjectStateManager, bu örneği kullanarak alınan nesnelerin belleğinde izler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-332">Object caching – the ObjectStateManager built into an ObjectContext instance keeps track in memory of the objects that have been retrieved using that instance.</span></span> <span data-ttu-id="22b0f-333">Bu, birinci düzey önbellek olarak da bilinir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-333">This is also known as first-level cache.</span></span>
2.  <span data-ttu-id="22b0f-334">Sorgu planı önbelleğe alma-bir sorgu birden çok kez yürütüldüğünde oluşturulan depo komutunu yeniden kullanma.</span><span class="sxs-lookup"><span data-stu-id="22b0f-334">Query Plan Caching - reusing the generated store command when a query is executed more than once.</span></span>
3.  <span data-ttu-id="22b0f-335">Meta veri önbelleğe alma-model için meta verileri aynı modele farklı bağlantılarda paylaşma.</span><span class="sxs-lookup"><span data-stu-id="22b0f-335">Metadata caching - sharing the metadata for a model across different connections to the same model.</span></span>

<span data-ttu-id="22b0f-336">EF 'in kullanıma sunduğu önbelleklerin yanı sıra, sarmalama sağlayıcısı olarak bilinen özel bir tür ADO.NET veri sağlayıcısı, veritabanından alınan sonuçlar için, ikinci düzey önbelleğe alma olarak da bilinen bir önbellekle Entity Framework genişletmek için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-336">Besides the caches that EF provides out of the box, a special kind of ADO.NET data provider known as a wrapping provider can also be used to extend Entity Framework with a cache for the results retrieved from the database, also known as second-level caching.</span></span>

### <a name="31-object-caching"></a><span data-ttu-id="22b0f-337">3,1 nesne önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="22b0f-337">3.1 Object Caching</span></span>

<span data-ttu-id="22b0f-338">Varsayılan olarak, bir varlık bir sorgunun sonuçlarında döndürüldüğünde, yalnızca EF 'i çalıştırmadan önce, ObjectContext 'e aynı anahtara sahip bir varlığın zaten yüklenmiş olup olmadığını kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="22b0f-338">By default when an entity is returned in the results of a query, just before EF materializes it, the ObjectContext will check if an entity with the same key has already been loaded into its ObjectStateManager.</span></span> <span data-ttu-id="22b0f-339">Aynı anahtarlara sahip bir varlık zaten varsa, bunu sorgunun sonuçlarına dahil eder.</span><span class="sxs-lookup"><span data-stu-id="22b0f-339">If an entity with the same keys is already present EF will include it in the results of the query.</span></span> <span data-ttu-id="22b0f-340">EF, sorguyu veritabanına karşı almaya devam edebilse de, bu davranış, varlığı birden çok kez gerçekleştirme maliyetinin çoğunu atlayabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-340">Although EF will still issue the query against the database, this behavior can bypass much of the cost of materializing the entity multiple times.</span></span>

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a><span data-ttu-id="22b0f-341">3.1.1 DbContext Find kullanarak nesne önbelleğinden varlık alma</span><span class="sxs-lookup"><span data-stu-id="22b0f-341">3.1.1 Getting entities from the object cache using DbContext Find</span></span>

<span data-ttu-id="22b0f-342">Normal bir sorgunun aksine, DbSet 'teki bul Yöntemi (EF 4,1 ' de ilk kez dahil edilen API 'Ler), sorguyu veritabanına karşı vermeden önce bellekte bir arama yapar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-342">Unlike a regular query, the Find method in DbSet (APIs included for the first time in EF 4.1) will perform a search in memory before even issuing the query against the database.</span></span> <span data-ttu-id="22b0f-343">İki farklı ObjectContext örneğinin iki farklı ObjectStateManager örneğine sahip olacağını, yani ayrı nesne önbellekleri olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-343">It’s important to note that two different ObjectContext instances will have two different ObjectStateManager instances, meaning that they have separate object caches.</span></span>

<span data-ttu-id="22b0f-344">Find, bağlam tarafından izlenen bir varlığı bulmayı denemek için birincil anahtar değerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-344">Find uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="22b0f-345">Varlık bağlamda değilse, bir sorgu yürütülür ve veritabanına göre değerlendirilir ve varlık bağlamda veya veritabanında bulunmazsa null değeri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="22b0f-345">If the entity is not in the context then a query will be executed and evaluated against the database, and null is returned if the entity is not found in the context or in the database.</span></span> <span data-ttu-id="22b0f-346">Bul 'un Ayrıca içeriğe eklenmiş ancak henüz veritabanına kaydedilmemiş olan varlıkları döndürdüğünü unutmayın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-346">Note that Find also returns entities that have been added to the context but have not yet been saved to the database.</span></span>

<span data-ttu-id="22b0f-347">Bul kullanılırken dikkate alınması gereken bir performans vardır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-347">There is a performance consideration to be taken when using Find.</span></span> <span data-ttu-id="22b0f-348">Bu yönteme varsayılan olarak yapılan çağrılar, hala veritabanına kaydedilmesi bekleyen değişiklikleri algılamak için nesne önbelleğinin doğrulanmasını tetikler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-348">Invocations to this method by default will trigger a validation of the object cache in order to detect changes that are still pending commit to the database.</span></span> <span data-ttu-id="22b0f-349">Nesne önbelleğinde çok fazla sayıda nesne varsa veya nesne önbelleğine eklenen büyük bir nesne grafiğinde bu işlem çok pahalı olabilir, ancak devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-349">This process can be very expensive if there are a very large number of objects in the object cache or in a large object graph being added to the object cache, but it can also be disabled.</span></span> <span data-ttu-id="22b0f-350">Belirli durumlarda, değişiklikleri otomatik olarak algılamayı devre dışı bıraktığınızda Find metodunu çağırma konusunda farklılık gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-350">In certain cases, you may perceive over an order of magnitude of difference in calling the Find method when you disable auto detect changes.</span></span> <span data-ttu-id="22b0f-351">İkinci Büyüklük sırası, nesne gerçekten önbellekte olduğunda, nesnenin veritabanından alınması gerektiğinde algılanır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-351">Yet a second order of magnitude is perceived when the object actually is in the cache versus when the object has to be retrieved from the database.</span></span> <span data-ttu-id="22b0f-352">Aşağıda, 5000 varlıkların bir yüküne göre milisaniye olarak ifade edilen mikro kıyaslamalarımızın bazıları kullanılarak alınan ölçümlere örnek bir grafik verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="22b0f-352">Here is an example graph with measurements taken using some of our microbenchmarks, expressed in milliseconds, with a load of 5000 entities:</span></span>

<span data-ttu-id="22b0f-353">![.NET 4,5 Logaritmik ölçek](~/ef6/media/net45logscale.png ".NET 4,5-Logaritmik ölçek")</span><span class="sxs-lookup"><span data-stu-id="22b0f-353">![.NET 4.5 logarithmic scale](~/ef6/media/net45logscale.png ".NET 4.5 - logarithmic scale")</span></span>

<span data-ttu-id="22b0f-354">Değişiklik otomatik algılama devre dışı olan bul örneği:</span><span class="sxs-lookup"><span data-stu-id="22b0f-354">Example of Find with auto-detect changes disabled:</span></span>

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

<span data-ttu-id="22b0f-355">Find metodunu kullanırken göz önünde bulundurmanız gerekenler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="22b0f-355">What you have to consider when using the Find method is:</span></span>

1.  <span data-ttu-id="22b0f-356">Nesne önbellekte değilse, bulma avantajları de daha basit olur, ancak sözdizimi anahtara göre sorgudan hala daha basittir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-356">If the object is not in the cache the benefits of Find are negated, but the syntax is still simpler than a query by key.</span></span>
2.  <span data-ttu-id="22b0f-357">Değişiklikleri otomatik algıla etkinleştirilirse, bulma yönteminin maliyeti tek bir büyüklük sırasıyla artabilir veya modelinizin karmaşıklığına ve nesne önbelleğinizin varlık miktarına bağlı olarak daha da fazla olabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-357">If auto detect changes is enabled the cost of the Find method may increase by one order of magnitude, or even more depending on the complexity of your model and the amount of entities in your object cache.</span></span>

<span data-ttu-id="22b0f-358">Ayrıca, bul 'un yalnızca aradığınız varlığı döndürdüğünü ve zaten nesne önbelleğinde değilse ilişkili varlıklarını otomatik olarak yüklemediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-358">Also, keep in mind that Find only returns the entity you are looking for and it does not automatically loads its associated entities if they are not already in the object cache.</span></span> <span data-ttu-id="22b0f-359">İlişkili varlıkları almanız gerekiyorsa, bir sorgu ile bir sorgu kullanarak bir sorgu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-359">If you need to retrieve associated entities, you can use a query by key with eager loading.</span></span> <span data-ttu-id="22b0f-360">Daha fazla bilgi için bkz. **8,1 yavaş yükleme vs. Eager yüklemesi**.</span><span class="sxs-lookup"><span data-stu-id="22b0f-360">For more information see **8.1 Lazy Loading vs. Eager Loading**.</span></span>

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a><span data-ttu-id="22b0f-361">Nesne önbelleğinde çok sayıda varlık olduğunda performans sorunları 3.1.2</span><span class="sxs-lookup"><span data-stu-id="22b0f-361">3.1.2 Performance issues when the object cache has many entities</span></span>

<span data-ttu-id="22b0f-362">Nesne önbelleği, Entity Framework genel yanıt hızını artırmaya yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-362">The object cache helps to increase the overall responsiveness of Entity Framework.</span></span> <span data-ttu-id="22b0f-363">Ancak, nesne önbelleğinde çok büyük miktarda varlık varsa, ekleme, kaldırma, bulma, giriş, SaveChanges ve daha fazlası gibi bazı işlemleri etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-363">However, when the object cache has a very large amount of entities loaded it may affect certain operations such as Add, Remove, Find, Entry, SaveChanges and more.</span></span> <span data-ttu-id="22b0f-364">Özellikle, DetectChanges çağrısı tetikleyen işlemler, çok büyük nesne önbellekler tarafından olumsuz etkilenir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-364">In particular, operations that trigger a call to DetectChanges will be negatively affected by very large object caches.</span></span> <span data-ttu-id="22b0f-365">DetectChanges, nesne grafiğini nesne durumu yöneticisiyle eşitler ve bu, performansı doğrudan nesne grafiğinin boyutuyla belirlenir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-365">DetectChanges synchronizes the object graph with the object state manager and its performance will determined directly by the size of the object graph.</span></span> <span data-ttu-id="22b0f-366">DetectChanges hakkında daha fazla bilgi için bkz. [POCO varlıklarındaki değişiklikleri izleme](https://msdn.microsoft.com/library/dd456848.aspx).</span><span class="sxs-lookup"><span data-stu-id="22b0f-366">For more information about DetectChanges, see [Tracking Changes in POCO Entities](https://msdn.microsoft.com/library/dd456848.aspx).</span></span>

<span data-ttu-id="22b0f-367">Entity Framework 6 kullanırken, geliştiriciler bir koleksiyon üzerinde yineleme yapmak yerine doğrudan bir DbSet üzerinde AddRange ve RemoveRange ' ı çağırabilir ve örnek başına Add bir kez çağrı yapabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-367">When using Entity Framework 6, developers are able to call AddRange and RemoveRange directly on a DbSet, instead of iterating on a collection and calling Add once per instance.</span></span> <span data-ttu-id="22b0f-368">Aralık yöntemlerini kullanmanın avantajı, DetectChanges maliyetinin yalnızca bir kez eklenen her varlık için bir kez olmak üzere tüm varlık kümesi için bir kez ödeneceği bir avantajdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-368">The advantage of using the range methods is that the cost of DetectChanges is only paid once for the entire set of entities as opposed to once per each added entity.</span></span>

### <a name="32-query-plan-caching"></a><span data-ttu-id="22b0f-369">3,2 sorgu planını önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="22b0f-369">3.2 Query Plan Caching</span></span>

<span data-ttu-id="22b0f-370">Sorgu ilk kez yürütüldüğünde, kavramsal sorguyu mağaza komutuna (örneğin, SQL Server karşı çalıştırıldığında yürütülen T-SQL) dönüştürmek için iç plan derleyicisinden geçer.</span><span class="sxs-lookup"><span data-stu-id="22b0f-370">The first time a query is executed, it goes through the internal plan compiler to translate the conceptual query into the store command (for example, the T-SQL which is executed when run against SQL Server).</span></span><span data-ttu-id="22b0f-371">  Sorgu planı önbelleğe alma etkinse, sorgu bir sonraki yürütüldüğünde, plan derleyicisini atlayarak yürütme için doğrudan sorgu planı önbelleğinden mağaza komutu alınır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-371">  If query plan caching is enabled, the next time the query is executed the store command is retrieved directly from the query plan cache for execution, bypassing the plan compiler.</span></span>

<span data-ttu-id="22b0f-372">Sorgu planı önbelleği aynı AppDomain içindeki ObjectContext örnekleri arasında paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-372">The query plan cache is shared across ObjectContext instances within the same AppDomain.</span></span> <span data-ttu-id="22b0f-373">Sorgu planı önbelleklemesi avantajlarından yararlanmak için bir ObjectContext örneğine sahip olmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="22b0f-373">You don't need to hold onto an ObjectContext instance to benefit from query plan caching.</span></span>

#### <a name="321-some-notes-about-query-plan-caching"></a><span data-ttu-id="22b0f-374">Sorgu planı önbelleğe alma hakkında bazı notları 3.2.1</span><span class="sxs-lookup"><span data-stu-id="22b0f-374">3.2.1 Some notes about Query Plan Caching</span></span>

-   <span data-ttu-id="22b0f-375">Sorgu planı önbelleği tüm sorgu türleri için paylaşılır: Entity SQL, LINQ to Entities ve CompiledQuery nesneleri.</span><span class="sxs-lookup"><span data-stu-id="22b0f-375">The query plan cache is shared for all query types: Entity SQL, LINQ to Entities, and CompiledQuery objects.</span></span>
-   <span data-ttu-id="22b0f-376">Varsayılan olarak, bir EntityCommand aracılığıyla veya bir ObjectQuery aracılığıyla yürütülüp yürütülmeksizin, sorgu planı önbelleğe alma, Entity SQL sorguları için etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-376">By default, query plan caching is enabled for Entity SQL queries, whether executed through an EntityCommand or through an ObjectQuery.</span></span> <span data-ttu-id="22b0f-377">Ayrıca, .NET 4,5 ve Entity Framework 6 ' da Entity Framework LINQ to Entities sorguları için varsayılan olarak etkinleştirilir</span><span class="sxs-lookup"><span data-stu-id="22b0f-377">It is also enabled by default for LINQ to Entities queries in Entity Framework on .NET 4.5, and in Entity Framework 6</span></span>
    -   <span data-ttu-id="22b0f-378">Sorgu planı önbelleğe alma, EnablePlanCaching özelliği (EntityCommand veya ObjectQuery üzerinde) false olarak ayarlanarak devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-378">Query plan caching can be disabled by setting the EnablePlanCaching property (on EntityCommand or ObjectQuery) to false.</span></span> <span data-ttu-id="22b0f-379">Örnek:</span><span class="sxs-lookup"><span data-stu-id="22b0f-379">For example:</span></span>
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
-   <span data-ttu-id="22b0f-380">Parametreli sorgular için, parametrenin değerini değiştirmek, önbelleğe alınmış sorguya yine de devam edecektir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-380">For parameterized queries, changing the parameter's value will still hit the cached query.</span></span> <span data-ttu-id="22b0f-381">Ancak parametrenin modellerini (örneğin, boyut, duyarlık veya ölçek) değiştirmek önbellekte farklı bir girdiye ulaşacaktır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-381">But changing a parameter's facets (for example, size, precision, or scale) will hit a different entry in the cache.</span></span>
-   <span data-ttu-id="22b0f-382">Entity SQL kullanırken sorgu dizesi anahtarın bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-382">When using Entity SQL, the query string is part of the key.</span></span> <span data-ttu-id="22b0f-383">Sorgu işlevsel olarak eşdeğer olsa bile sorgunun her seferinde değiştirilmesi farklı önbellek girişlerine neden olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-383">Changing the query at all will result in different cache entries, even if the queries are functionally equivalent.</span></span> <span data-ttu-id="22b0f-384">Bu, büyük küçük harf veya boşluk değişiklikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-384">This includes changes to casing or whitespace.</span></span>
-   <span data-ttu-id="22b0f-385">LINQ kullanılırken, anahtarın bir parçasını oluşturmak için sorgu işlenir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-385">When using LINQ, the query is processed to generate a part of the key.</span></span> <span data-ttu-id="22b0f-386">LINQ ifadesinin değiştirilmesi, bu nedenle farklı bir anahtar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-386">Changing the LINQ expression will therefore generate a different key.</span></span>
-   <span data-ttu-id="22b0f-387">Diğer teknik sınırlamalar da uygulanabilir. daha fazla ayrıntı için bkz. oto derlenmiş sorgular.</span><span class="sxs-lookup"><span data-stu-id="22b0f-387">Other technical limitations may apply; see Autocompiled Queries for more details.</span></span>

#### <a name="322-cache-eviction-algorithm"></a><span data-ttu-id="22b0f-388">3.2.2 önbellek çıkarma algoritması</span><span class="sxs-lookup"><span data-stu-id="22b0f-388">3.2.2      Cache eviction algorithm</span></span>

<span data-ttu-id="22b0f-389">İç algoritmanın nasıl çalıştığını anlamak, sorgu planının önbelleğe alınmasını ne zaman etkinleştirebileceğinizi veya devre dışı bırakacağınızı belirlemenize yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-389">Understanding how the internal algorithm works will help you figure out when to enable or disable query plan caching.</span></span> <span data-ttu-id="22b0f-390">Temizleme algoritması aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="22b0f-390">The cleanup algorithm is as follows:</span></span>

1.  <span data-ttu-id="22b0f-391">Önbellekte bir dizi girdi (800) varsa, düzenli aralıklarla (dakikada bir kez) bir süreölçeri, önbelleği başlatır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-391">Once the cache contains a set number of entries (800), we start a timer that periodically (once-per-minute) sweeps the cache.</span></span>
2.  <span data-ttu-id="22b0f-392">Önbellek uyumlu EPS 'ler sırasında, girdiler bir LFRU (en az sık kullanılan) temelinde önbellekten kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-392">During cache sweeps, entries are removed from the cache on a LFRU (Least frequently – recently used) basis.</span></span> <span data-ttu-id="22b0f-393">Hangi girişlerin çıkarılcağına karar verirken bu algoritma hem isabet sayısını hem de yaşı hesaba girer.</span><span class="sxs-lookup"><span data-stu-id="22b0f-393">This algorithm takes both hit count and age into account when deciding which entries are ejected.</span></span>
3.  <span data-ttu-id="22b0f-394">Her önbellek süpürme sonunda, önbellek 800 girdi içerir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-394">At the end of each cache sweep, the cache again contains 800 entries.</span></span>

<span data-ttu-id="22b0f-395">Hangi girişlerin çıkarılması belirlenirken tüm önbellek girişleri eşit olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-395">All cache entries are treated equally when determining which entries to evict.</span></span> <span data-ttu-id="22b0f-396">Bu, bir CompiledQuery için mağaza komutunun, bir Entity SQL sorgusunun depolama komutu olarak çıkarılması ihtimaline sahip olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-396">This means the store command for a CompiledQuery has the same chance of eviction as the store command for an Entity SQL query.</span></span>

<span data-ttu-id="22b0f-397">Önbellekte 800 varlık olduğunda önbellek çıkarma süreölçerinin çıkardığına, ancak önbelleğin yalnızca bu Zamanlayıcı başlatıldıktan sonra yalnızca 60 saniye olduğuna göz önünde kılandı.</span><span class="sxs-lookup"><span data-stu-id="22b0f-397">Note that the cache eviction timer is kicked in when there are 800 entities in the cache, but the cache is only swept 60 seconds after this timer is started.</span></span> <span data-ttu-id="22b0f-398">Yani, önbelleğinizin en fazla 60 saniyeye kadar büyüeceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-398">That means that for up to 60 seconds your cache may grow to be quite large.</span></span>

#### <a name="323-test-metrics-demonstrating-query-plan-caching-performance"></a><span data-ttu-id="22b0f-399">sorgu planını önbelleğe alma performansını gösteren 3.2.3 test ölçümleri</span><span class="sxs-lookup"><span data-stu-id="22b0f-399">3.2.3       Test Metrics demonstrating query plan caching performance</span></span>

<span data-ttu-id="22b0f-400">Sorgu planı önbelleğinin uygulamanızın performansına etkisini göstermek için, Navision modeline göre çok sayıda Entity SQL sorgusu yürütüyoruz bir test gerçekleştirdik.</span><span class="sxs-lookup"><span data-stu-id="22b0f-400">To demonstrate the effect of query plan caching on your application's performance, we performed a test where we executed a number of Entity SQL queries against the Navision model.</span></span> <span data-ttu-id="22b0f-401">Navision modelinin açıklaması ve yürütülen sorgu türleri için ek başlığına bakın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-401">See the appendix for a description of the Navision model and the types of queries which were executed.</span></span> <span data-ttu-id="22b0f-402">Bu testte, ilk olarak sorgu listesi boyunca yineliyoruz ve önbelleğe eklemek için her birini bir kez yürütüyoruz (önbelleğe alma etkinse).</span><span class="sxs-lookup"><span data-stu-id="22b0f-402">In this test, we first iterate through the list of queries and execute each one once to add them to the cache (if caching is enabled).</span></span> <span data-ttu-id="22b0f-403">Bu adım zaman aşımına uğradı.</span><span class="sxs-lookup"><span data-stu-id="22b0f-403">This step is untimed.</span></span> <span data-ttu-id="22b0f-404">Daha sonra, önbelleğin sürme durumunda 60 saniye boyunca ana iş parçacığını uykuya geçiririz; son olarak, önbelleğe alınmış sorguların yürütülmesi için ikinci kez listede yineleme yaptık.</span><span class="sxs-lookup"><span data-stu-id="22b0f-404">Next, we sleep the main thread for over 60 seconds to allow cache sweeping to take place; finally, we iterate through the list a 2nd time to execute the cached queries.</span></span> <span data-ttu-id="22b0f-405">Ayrıca, her sorgu kümesi, sorgu planı önbelleğinin verdiği avantajı doğru şekilde yansıttığından, her bir sorgu kümesi yürütülmeden önce SQL Server planı önbelleği temizlenir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-405">Additionally, the SQL Server plan cache is flushed before each set of queries is executed so that the times we obtain accurately reflect the benefit given by the query plan cache.</span></span>

##### <a name="3231-test-results"></a><span data-ttu-id="22b0f-406">3.2.3.1 Test Sonuçları</span><span class="sxs-lookup"><span data-stu-id="22b0f-406">3.2.3.1       Test Results</span></span>

| <span data-ttu-id="22b0f-407">Test etme</span><span class="sxs-lookup"><span data-stu-id="22b0f-407">Test</span></span>                                                                   | <span data-ttu-id="22b0f-408">EF5 önbellek yok</span><span class="sxs-lookup"><span data-stu-id="22b0f-408">EF5 no cache</span></span> | <span data-ttu-id="22b0f-409">EF5 önbelleğe alındı</span><span class="sxs-lookup"><span data-stu-id="22b0f-409">EF5 cached</span></span> | <span data-ttu-id="22b0f-410">EF6 önbellek yok</span><span class="sxs-lookup"><span data-stu-id="22b0f-410">EF6 no cache</span></span> | <span data-ttu-id="22b0f-411">EF6 önbelleğe alındı</span><span class="sxs-lookup"><span data-stu-id="22b0f-411">EF6 cached</span></span> |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| <span data-ttu-id="22b0f-412">Tüm 18723 sorguları numaralandırılıyor</span><span class="sxs-lookup"><span data-stu-id="22b0f-412">Enumerating all 18723 queries</span></span>                                          | <span data-ttu-id="22b0f-413">124</span><span class="sxs-lookup"><span data-stu-id="22b0f-413">124</span></span>          | <span data-ttu-id="22b0f-414">125,4</span><span class="sxs-lookup"><span data-stu-id="22b0f-414">125.4</span></span>      | <span data-ttu-id="22b0f-415">124,3</span><span class="sxs-lookup"><span data-stu-id="22b0f-415">124.3</span></span>        | <span data-ttu-id="22b0f-416">125,3</span><span class="sxs-lookup"><span data-stu-id="22b0f-416">125.3</span></span>      |
| <span data-ttu-id="22b0f-417">Süpürme (karmaşıklıktan bağımsız olarak yalnızca ilk 800 sorgu)</span><span class="sxs-lookup"><span data-stu-id="22b0f-417">Avoiding sweep (just the first 800 queries, regardless of complexity)</span></span>  | <span data-ttu-id="22b0f-418">41,7</span><span class="sxs-lookup"><span data-stu-id="22b0f-418">41.7</span></span>         | <span data-ttu-id="22b0f-419">5.5</span><span class="sxs-lookup"><span data-stu-id="22b0f-419">5.5</span></span>        | <span data-ttu-id="22b0f-420">40.5</span><span class="sxs-lookup"><span data-stu-id="22b0f-420">40.5</span></span>         | <span data-ttu-id="22b0f-421">5,4</span><span class="sxs-lookup"><span data-stu-id="22b0f-421">5.4</span></span>        |
| <span data-ttu-id="22b0f-422">Yalnızca Aggregatingalt toplamları sorguları (178 toplam, süpürme önlenir)</span><span class="sxs-lookup"><span data-stu-id="22b0f-422">Just the AggregatingSubtotals queries (178 total - which avoids sweep)</span></span> | <span data-ttu-id="22b0f-423">39,5</span><span class="sxs-lookup"><span data-stu-id="22b0f-423">39.5</span></span>         | <span data-ttu-id="22b0f-424">4,5</span><span class="sxs-lookup"><span data-stu-id="22b0f-424">4.5</span></span>        | <span data-ttu-id="22b0f-425">38,1</span><span class="sxs-lookup"><span data-stu-id="22b0f-425">38.1</span></span>         | <span data-ttu-id="22b0f-426">4.6</span><span class="sxs-lookup"><span data-stu-id="22b0f-426">4.6</span></span>        |

<span data-ttu-id="22b0f-427">*Saniyeler içinde her zaman.*</span><span class="sxs-lookup"><span data-stu-id="22b0f-427">*All times in seconds.*</span></span>

<span data-ttu-id="22b0f-428">Moral-çok sayıda farklı sorgu yürütürken (örneğin, dinamik olarak oluşturulan sorgular), önbelleğe alma işlemi yardım etmez ve önbelleğin ortaya çıkmasına yönelik olarak, gerçekten onu kullanarak plan önbelleğinin en iyi şekilde yararlanabileceği sorguları koruyabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-428">Moral - when executing lots of distinct queries (for example,  dynamically created queries), caching doesn't help and the resulting flushing of the cache can keep the queries that would benefit the most from plan caching from actually using it.</span></span>

<span data-ttu-id="22b0f-429">Aggregatingalt toplamları sorguları, test ettiğimiz sorguların en karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-429">The AggregatingSubtotals queries are the most complex of the queries we tested with.</span></span> <span data-ttu-id="22b0f-430">Beklenildiği gibi, sorgu daha karmaşık olduğunda sorgu planı önbelleğe alma işleminden daha fazla avantaj elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-430">As expected, the more complex the query is, the more benefit you will see from query plan caching.</span></span>

<span data-ttu-id="22b0f-431">Bir CompiledQuery, planı önbelleğe alınmış bir LINQ sorgusu olduğundan, bir CompiledQuery ile eşdeğer Entity SQL sorgusunun karşılaştırılması benzer sonuçlara sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-431">Because a CompiledQuery is really a LINQ query with its plan cached, the comparison of a CompiledQuery versus the equivalent Entity SQL query should have similar results.</span></span> <span data-ttu-id="22b0f-432">Aslında, bir uygulamanın çok sayıda dinamik Entity SQL sorgusu varsa, önbelleğin sorgular ile doldurulması, ön bellekten Temizlendiklerinde "derlemeyi parçalara ayırma" işlemi de etkili olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-432">In fact, if an app has lots of dynamic Entity SQL queries, filling the cache with queries will also effectively cause CompiledQueries to “decompile” when they are flushed from the cache.</span></span> <span data-ttu-id="22b0f-433">Bu senaryoda, dinamik sorgular üzerinde önbelleğe alma devre dışı bırakılarak, CompiledQueries önceliklendirilerek performans artırılabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-433">In this scenario, performance may be improved by disabling caching on the dynamic queries to prioritize the CompiledQueries.</span></span> <span data-ttu-id="22b0f-434">Daha iyi kuşkusuz, dinamik sorgular yerine parametreli sorguları kullanmak için uygulamayı yeniden yazmak olacaktır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-434">Better yet, of course, would be to rewrite the app to use parameterized queries instead of dynamic queries.</span></span>

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a><span data-ttu-id="22b0f-435">LINQ sorgularıyla performansı artırmak için CompiledQuery kullanma 3,3</span><span class="sxs-lookup"><span data-stu-id="22b0f-435">3.3 Using CompiledQuery to improve performance with LINQ queries</span></span>

<span data-ttu-id="22b0f-436">Sınamalarımız, CompiledQuery kullanmanın, oto derlenen LINQ sorgularının üzerinde %7 ' nin avantajlarından yararlanabileceğinizi gösteriyor; Bu, Entity Framework yığınından kod yürüten %7 daha az zaman harcamanız anlamına gelir; Uygulamanızın %7 daha hızlı olacağını ifade etmez.</span><span class="sxs-lookup"><span data-stu-id="22b0f-436">Our tests indicate that using CompiledQuery can bring a benefit of 7% over autocompiled LINQ queries; this means that you’ll spend 7% less time executing code from the Entity Framework stack; it does not mean your application will be 7% faster.</span></span> <span data-ttu-id="22b0f-437">Genel olarak, f 5,0 ' de CompiledQuery nesnelerini yazma ve korumanın maliyeti avantajlarla karşılaştırıldığında sorun için değer olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-437">Generally speaking, the cost of writing and maintaining CompiledQuery objects in EF 5.0 may not be worth the trouble when compared to the benefits.</span></span> <span data-ttu-id="22b0f-438">Gelinmeniz farklılık gösterebilir, bu nedenle projeniz fazladan gönderim gerektiriyorsa bu seçeneği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-438">Your mileage may vary, so exercise this option if your project requires the extra push.</span></span> <span data-ttu-id="22b0f-439">CompiledQueries 'in yalnızca ObjectContext ile türetilmiş modellerle uyumlu olduğunu ve DbContext ile türetilmiş modellerle uyumlu olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-439">Note that CompiledQueries are only compatible with ObjectContext-derived models, and not compatible with DbContext-derived models.</span></span>

<span data-ttu-id="22b0f-440">Bir CompiledQuery oluşturma ve çağırma hakkında daha fazla bilgi için bkz. [derlenmiş sorgular (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).</span><span class="sxs-lookup"><span data-stu-id="22b0f-440">For more information on creating and invoking a CompiledQuery, see [Compiled Queries (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).</span></span>

<span data-ttu-id="22b0f-441">Bir CompiledQuery kullanırken yapmanız gereken iki önemli noktalar vardır. Bu, statik örnekler ve bunların birlikte bulunan sorunları kullanma gereksinimidir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-441">There are two considerations you have to take when using a CompiledQuery, namely the requirement to use static instances and the problems they have with composability.</span></span> <span data-ttu-id="22b0f-442">Burada, bu iki önemli noktaların derinlemesine bir açıklaması verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-442">Here follows an in-depth explanation of these two considerations.</span></span>

#### <a name="331-use-static-compiledquery-instances"></a><span data-ttu-id="22b0f-443">3.3.1 statik CompiledQuery örnekleri kullan</span><span class="sxs-lookup"><span data-stu-id="22b0f-443">3.3.1       Use static CompiledQuery instances</span></span>

<span data-ttu-id="22b0f-444">LINQ sorgusunun derlenmesi zaman alan bir işlem olduğundan, veritabanından veri getirmeye gerek duyduğumuz her seferinde bunu yapmak istemedik.</span><span class="sxs-lookup"><span data-stu-id="22b0f-444">Since compiling a LINQ query is a time-consuming process, we don’t want to do it every time we need to fetch data from the database.</span></span> <span data-ttu-id="22b0f-445">CompiledQuery örnekleri bir kez derlemenize ve birden çok kez çalıştırmanıza izin verir, ancak aynı CompiledQuery örneğini tekrar tekrar derlemek yerine yeniden kullanmak için dikkatli olmanız ve temin etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-445">CompiledQuery instances allow you to compile once and run multiple times, but you have to be careful and procure to re-use the same CompiledQuery instance every time instead of compiling it over and over again.</span></span> <span data-ttu-id="22b0f-446">CompiledQuery örneklerini depolamak için statik üyelerin kullanımı gerekli hale gelir; Aksi takdirde hiçbir avantaj görmezsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-446">The use of static members to store the CompiledQuery instances becomes necessary; otherwise you won’t see any benefit.</span></span>

<span data-ttu-id="22b0f-447">Örneğin, sayfanızın seçili kategori için ürünleri görüntülemeyi işlemek üzere aşağıdaki yöntem gövdesine sahip olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="22b0f-447">For example, suppose your page has the following method body to handle displaying the products for the selected category:</span></span>

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

<span data-ttu-id="22b0f-448">Bu durumda, yöntemi her çağrıldığında anında, yeni bir CompiledQuery örneği oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="22b0f-448">In this case, you will create a new CompiledQuery instance on the fly every time the method is called.</span></span> <span data-ttu-id="22b0f-449">Sorgu planı önbelleğinden mağaza komutunu alarak performans avantajlarını görmek yerine, her yeni örnek oluşturulduğunda CompiledQuery plan derleyicisini alır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-449">Instead of seeing performance benefits by retrieving the store command from the query plan cache, the CompiledQuery will go through the plan compiler every time a new instance is created.</span></span> <span data-ttu-id="22b0f-450">Aslında, yöntem her çağrıldığında sorgu planı önbelleğinizi yeni bir CompiledQuery girişi ile yoklacaksınız.</span><span class="sxs-lookup"><span data-stu-id="22b0f-450">In fact, you will be polluting your query plan cache with a new CompiledQuery entry every time the method is called.</span></span>

<span data-ttu-id="22b0f-451">Bunun yerine, derlenmiş sorgunun statik bir örneğini oluşturmak istersiniz; bu nedenle, yöntemi her çağrıldığında aynı derlenmiş sorguyu çağırılır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-451">Instead, you want to create a static instance of the compiled query, so you are invoking the same compiled query every time the method is called.</span></span> <span data-ttu-id="22b0f-452">Bunu yapmanın bir yolu, CompiledQuery örneğini nesne bağlamınızın bir üyesi olarak eklemektir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-452">One way to so this is by adding the CompiledQuery instance as a member of your object context.</span></span><span data-ttu-id="22b0f-453">  Daha sonra bir yardımcı yöntemi aracılığıyla CompiledQuery 'e erişerek bir şeyler daha az temizleyici yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="22b0f-453">  You can then make things a little cleaner by accessing the CompiledQuery through a helper method:</span></span>

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

<span data-ttu-id="22b0f-454">Bu yardımcı yöntem aşağıdaki şekilde çağrılabilir:</span><span class="sxs-lookup"><span data-stu-id="22b0f-454">This helper method would be invoked as follows:</span></span>

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-composing-over-a-compiledquery"></a><span data-ttu-id="22b0f-455">bir CompiledQuery üzerinde 3.3.2 oluşturma</span><span class="sxs-lookup"><span data-stu-id="22b0f-455">3.3.2       Composing over a CompiledQuery</span></span>

<span data-ttu-id="22b0f-456">Herhangi bir LINQ sorgusunu oluşturma özelliği son derece yararlı olur; Bunu yapmak için, bir yöntemi, *Skip ()* veya *Count ()* gibi IQueryable 'dan sonra çağırmanız yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-456">The ability to compose over any LINQ query is extremely useful; to do this, you simply invoke a method after the IQueryable such as *Skip()* or *Count()*.</span></span> <span data-ttu-id="22b0f-457">Ancak, bunun yapılması aslında yeni bir IQueryable nesnesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="22b0f-457">However, doing so essentially returns a new IQueryable object.</span></span> <span data-ttu-id="22b0f-458">Bir CompiledQuery üzerinden oluşturmanız için teknik olarak herhangi bir şey yapmayın, ancak bunu yapmak plan derleyicisinin yeniden geçirilmesini gerektiren yeni bir IQueryable nesnesinin oluşturulmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-458">While there’s nothing to stop you technically from composing over a CompiledQuery, doing so will cause the generation of a new IQueryable object that requires passing through the plan compiler again.</span></span>

<span data-ttu-id="22b0f-459">Bazı bileşenler, gelişmiş işlevleri etkinleştirmek için oluşturulmuş IQueryable nesnelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-459">Some components will make use of composed IQueryable objects to enable advanced functionality.</span></span> <span data-ttu-id="22b0f-460">Örneğin, ASP. NET ' in GridView, bir IQueryable nesnesine, SelectMethod özelliği aracılığıyla veri ile bağlantılı olabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-460">For example, ASP.NET’s GridView can be data-bound to an IQueryable object via the SelectMethod property.</span></span> <span data-ttu-id="22b0f-461">GridView daha sonra veri modeli üzerinde sıralama ve sayfalama sağlamak için bu IQueryable nesnesini oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="22b0f-461">The GridView will then compose over this IQueryable object to allow sorting and paging over the data model.</span></span> <span data-ttu-id="22b0f-462">Gördüğünüz gibi, GridView için bir CompiledQuery kullanmak derlenen sorguya ulaşmaz, ancak yeni bir oto derlenmiş sorgu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-462">As you can see, using a CompiledQuery for the GridView would not hit the compiled query but would generate a new autocompiled query.</span></span>

<span data-ttu-id="22b0f-463">Bu, bir sorguya aşamalı filtreler eklenirken bir yer olabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-463">One place where you may run into this is when adding progressive filters to a query.</span></span> <span data-ttu-id="22b0f-464">Örneğin, isteğe bağlı filtreler (örneğin, ülke ve OrdersCount) için birkaç açılan liste içeren bir müşteriler sayfasına sahip olduğunuzu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="22b0f-464">For example, suppose you had a Customers page with several drop-down lists for optional filters (for example, Country and OrdersCount).</span></span> <span data-ttu-id="22b0f-465">Bu filtreleri bir CompiledQuery 'nin IQueryable sonuçları üzerinden oluşturabilirsiniz, ancak bunu yaptığınızda Yeni sorgunun plan derleyicisine her yürüttüğünüzde, bu filtreler oluşur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-465">You can compose these filters over the IQueryable results of a CompiledQuery, but doing so will result in the new query going through the plan compiler every time you execute it.</span></span>

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

 <span data-ttu-id="22b0f-466">Bu yeniden derlemeyi önlemek için, aşağıdaki olası filtreleri hesaba çekmek üzere CompiledQuery 'yi yeniden yazabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="22b0f-466">To avoid this re-compilation, you can rewrite the CompiledQuery to take the possible filters into account:</span></span>

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

<span data-ttu-id="22b0f-467">Şu şekilde Kullanıcı arabiriminde çağrılacak:</span><span class="sxs-lookup"><span data-stu-id="22b0f-467">Which would be invoked in the UI like:</span></span>

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

 <span data-ttu-id="22b0f-468">Burada oluşturulan depo komutu, her zaman null denetimleri olan filtrelere sahip olur, ancak bu, veritabanı sunucusunun en iyileştirmek için oldukça basittir:</span><span class="sxs-lookup"><span data-stu-id="22b0f-468">A tradeoff here is the generated store command will always have the filters with the null checks, but these should be fairly simple for the database server to optimize:</span></span>

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a><span data-ttu-id="22b0f-469">3,4 meta verileri önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="22b0f-469">3.4 Metadata caching</span></span>

<span data-ttu-id="22b0f-470">Entity Framework meta verileri önbelleğe almayı da destekler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-470">The Entity Framework also supports Metadata caching.</span></span> <span data-ttu-id="22b0f-471">Bu, temel olarak aynı modele farklı bağlantılarda tür bilgilerini ve tür-veritabanı eşleme bilgilerini önbelleğe alma işlemini sağlar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-471">This is essentially caching of type information and type-to-database mapping information across different connections to the same model.</span></span> <span data-ttu-id="22b0f-472">Meta veri önbelleği, AppDomain başına benzersizdir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-472">The Metadata cache is unique per AppDomain.</span></span>

#### <a name="341-metadata-caching-algorithm"></a><span data-ttu-id="22b0f-473">3.4.1 Metadata önbelleğe alma algoritması</span><span class="sxs-lookup"><span data-stu-id="22b0f-473">3.4.1 Metadata Caching algorithm</span></span>

1.  <span data-ttu-id="22b0f-474">Bir modelin meta veri bilgileri her EntityConnection için bir ItemCollection 'da depolanır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-474">Metadata information for a model is stored in an ItemCollection for each EntityConnection.</span></span>
    -   <span data-ttu-id="22b0f-475">Yan bir notta, modelin farklı parçaları için farklı ItemCollection nesneleri vardır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-475">As a side note, there are different ItemCollection objects for different parts of the model.</span></span> <span data-ttu-id="22b0f-476">Örneğin, Storeıtemcollections, veritabanı modeliyle ilgili bilgileri içerir; ObjectItemCollection, veri modeli hakkında bilgi içerir; EdmItemCollection, kavramsal model hakkında bilgi içerir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-476">For example, StoreItemCollections contains the information about the database model; ObjectItemCollection contains information about the data model; EdmItemCollection contains information about the conceptual model.</span></span>

2.  <span data-ttu-id="22b0f-477">İki bağlantı aynı bağlantı dizesini kullanıyorsa, aynı ItemCollection örneğini paylaşacağız.</span><span class="sxs-lookup"><span data-stu-id="22b0f-477">If two connections use the same connection string, they will share the same ItemCollection instance.</span></span>
3.  <span data-ttu-id="22b0f-478">İşlevsel eşdeğer ancak metin içeriğini eklemek farklı bağlantı dizeleri farklı meta veri önbelleklerine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-478">Functionally equivalent but textually different connection strings may result in different metadata caches.</span></span> <span data-ttu-id="22b0f-479">Bağlantı dizelerini Simgeleştir, bu nedenle belirteçlerin sırasını değiştirmenin paylaşılan meta verilerle sonuçlanmaları gerekir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-479">We do tokenize connection strings, so simply changing the order of the tokens should result in shared metadata.</span></span> <span data-ttu-id="22b0f-480">Ancak işlevsel olarak görünen iki bağlantı dizesi, simgeleştirme sonrasında özdeş olarak değerlendirilmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-480">But two connection strings that seem functionally the same may not be evaluated as identical after tokenization.</span></span>
4.  <span data-ttu-id="22b0f-481">ItemCollection düzenli olarak kullanım için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-481">The ItemCollection is periodically checked for use.</span></span> <span data-ttu-id="22b0f-482">Bir çalışma alanına son zamanlarda erişilmediğini tespit ediyorsanız, bir sonraki önbellek tarama işlemi üzerinde Temizleme için işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-482">If it is determined that a workspace has not been accessed recently, it will be marked for cleanup on the next cache sweep.</span></span>
5.  <span data-ttu-id="22b0f-483">Yalnızca bir EntityConnection oluşturmak, meta veri önbelleğinin oluşturulmasına neden olur (ancak, içindeki öğe koleksiyonları bağlantı açılıncaya kadar başlatılamaz).</span><span class="sxs-lookup"><span data-stu-id="22b0f-483">Merely creating an EntityConnection will cause a metadata cache to be created (though the item collections in it will not be initialized until the connection is opened).</span></span> <span data-ttu-id="22b0f-484">Bu çalışma alanı, önbelleğe alma algoritması "kullanımda" olmadığını belirlediğinde bellek içinde kalır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-484">This workspace will remain in-memory until the caching algorithm determines it is not “in use”.</span></span>

<span data-ttu-id="22b0f-485">Müşteri danışmanlığı ekibi, büyük modeller kullanırken "kullanımdan kaldırma" olmaması için bir ItemCollection 'ın başvurusunu tutan bir blog gönderisi yazdı: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.</span><span class="sxs-lookup"><span data-stu-id="22b0f-485">The Customer Advisory Team has written a blog post that describes holding a reference to an ItemCollection in order to avoid "deprecation" when using large models: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.</span></span>

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a><span data-ttu-id="22b0f-486">Meta veri önbelleği ve sorgu planı önbelleğe alma arasındaki ilişkiyi 3.4.2</span><span class="sxs-lookup"><span data-stu-id="22b0f-486">3.4.2 The relationship between Metadata Caching and Query Plan Caching</span></span>

<span data-ttu-id="22b0f-487">Sorgu planı önbellek örneği, MetadataWorkspace 'in depo türlerinin ItemCollection 'ı içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-487">The query plan cache instance lives in the MetadataWorkspace's ItemCollection of store types.</span></span> <span data-ttu-id="22b0f-488">Bu, önbelleğe alınan depo komutlarının, belirli bir MetadataWorkspace kullanılarak oluşturulan herhangi bir bağlamda sorgu için kullanılacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-488">This means that cached store commands will be used for queries against any context instantiated using a given MetadataWorkspace.</span></span> <span data-ttu-id="22b0f-489">Ayrıca, biraz farklı olan iki bağlantı dizeniz varsa ve Simgeleştirici sonrasında eşleşmezse, farklı sorgu planı önbellek örneklerine sahip olursunuz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-489">It also means that if you have two connections strings that are slightly different and don't match after tokenizing, you will have different query plan cache instances.</span></span>

### <a name="35-results-caching"></a><span data-ttu-id="22b0f-490">3,5 sonuçları önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="22b0f-490">3.5 Results caching</span></span>

<span data-ttu-id="22b0f-491">Sonuçları önbelleğe alma ("ikinci düzey önbelleğe alma" olarak da bilinir) sayesinde sorguların sonuçlarını yerel bir önbellekte saklayın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-491">With results caching (also known as "second-level caching"), you keep the results of queries in a local cache.</span></span> <span data-ttu-id="22b0f-492">Bir sorgu verirken, öncelikle depoya karşı sorgu yapmadan önce sonuçların yerel olarak kullanılabilir olup olmadığını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-492">When issuing a query, you first see if the results are available locally before you query against the store.</span></span> <span data-ttu-id="22b0f-493">Sonuçları önbelleğe alma, Entity Framework tarafından doğrudan desteklenirken, sarmalama sağlayıcısı kullanılarak ikinci düzey bir önbellek eklemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="22b0f-493">While results caching isn't directly supported by Entity Framework, it's possible to add a second level cache by using a wrapping provider.</span></span> <span data-ttu-id="22b0f-494">İkinci düzey önbellek içeren örnek bir sarmalama sağlayıcısı, [NCache 'e göre Alachisoft 'In Ikinci düzey önbelleği Entity Framework](https://www.alachisoft.com/ncache/entity-framework.html).</span><span class="sxs-lookup"><span data-stu-id="22b0f-494">An example wrapping provider with a second-level cache is Alachisoft's [Entity Framework Second Level Cache based on NCache](https://www.alachisoft.com/ncache/entity-framework.html).</span></span>

<span data-ttu-id="22b0f-495">İkinci düzey önbelleğe almanın bu uygulama, LINQ ifadesi hesaplandıktan sonra (ve komik) ve sorgu yürütme planı hesaplandıktan veya ilk düzey önbellekten alındıktan sonra gerçekleşen, eklenen bir işlevsellikten oluşur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-495">This implementation of second-level caching is an injected functionality that takes place after the LINQ expression has been evaluated (and funcletized) and the query execution plan is computed or retrieved from the first-level cache.</span></span> <span data-ttu-id="22b0f-496">İkinci düzey önbellek daha sonra yalnızca ham veritabanı sonuçlarını depolar, bu nedenle gerçekleştirmesi işlem hattı daha sonra yine de yürütülür.</span><span class="sxs-lookup"><span data-stu-id="22b0f-496">The second-level cache will then store only the raw database results, so the materialization pipeline still executes afterwards.</span></span>

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a><span data-ttu-id="22b0f-497">sarmalama sağlayıcısıyla önbelleğe alma sonuçları için 3.5.1 ek başvuruları</span><span class="sxs-lookup"><span data-stu-id="22b0f-497">3.5.1 Additional references for results caching with the wrapping provider</span></span>

-   <span data-ttu-id="22b0f-498">Julie Lerman, örnek sarmalama sağlayıcısını Windows Server AppFabric Önbelleği kullanmak üzere güncelleştirmeyi içeren bir "Entity Framework ve Windows Azure 'da Ikinci düzey önbelleğe alma" MSDN makalesinde yazılmıştır: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)</span><span class="sxs-lookup"><span data-stu-id="22b0f-498">Julie Lerman has written a "Second-Level Caching in Entity Framework and Windows Azure" MSDN article that includes how to update the sample wrapping provider to use Windows Server AppFabric caching: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)</span></span>
-   <span data-ttu-id="22b0f-499">Entity Framework 5 ile çalışıyorsanız, ekip bloguna Entity Framework 5: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>için önbelleğe alma sağlayıcısıyla birlikte çalışan şeyleri nasıl alacağınız açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-499">If you are working with Entity Framework 5, the team blog has a post that describes how to get things running with the caching provider for Entity Framework 5: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>.</span></span> <span data-ttu-id="22b0f-500">Ayrıca, projenize 2. düzey önbellek eklemeyi otomatik hale getirmeye yardımcı olan bir T4 şablonu da içerir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-500">It also includes a T4 template to help automate adding the 2nd-level caching to your project.</span></span>

## <a name="4-autocompiled-queries"></a><span data-ttu-id="22b0f-501">4 oto derlenmiş sorgular</span><span class="sxs-lookup"><span data-stu-id="22b0f-501">4 Autocompiled Queries</span></span>

<span data-ttu-id="22b0f-502">Bir sorgu Entity Framework kullanarak bir veritabanına karşı verildiğinde, sonuçların gerçekten çıkarılmadan önce bir dizi adımdan ilerlemesi gerekir; Bu tür bir adım sorgu derlemesi olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-502">When a query is issued against a database using Entity Framework, it must go through a series of steps before actually materializing the results; one such step is Query Compilation.</span></span> <span data-ttu-id="22b0f-503">Entity SQL sorguları otomatik olarak önbelleğe alındığından iyi performansa sahip oldukları bilindiğinden, ikinci veya üçüncü kez aynı sorguyu yürüttüğünüzde plan derleyicisini atlayabilir ve bunun yerine önbelleğe alınmış planı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-503">Entity SQL queries were known to have good performance as they are automatically cached, so the second or third time you execute the same query it can skip the plan compiler and use the cached plan instead.</span></span>

<span data-ttu-id="22b0f-504">Entity Framework 5, LINQ to Entities sorguları için otomatik önbelleğe alma özelliği de sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-504">Entity Framework 5 introduced automatic caching for LINQ to Entities queries as well.</span></span> <span data-ttu-id="22b0f-505">Entity Framework geçmiş sürümlerinde, performansı hızlandırmak için bir CompiledQuery oluşturmak, bu, LINQ to Entities sorgusunun önbelleklenebilir olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-505">In past editions of Entity Framework creating a CompiledQuery to speed your performance was a common practice, as this would make your LINQ to Entities query cacheable.</span></span> <span data-ttu-id="22b0f-506">Önbelleğe alma işlemi bir CompiledQuery kullanılmadan otomatik olarak yapıldığından, bu özelliği "otomatik derlenmiş sorgular" olarak çağırırız.</span><span class="sxs-lookup"><span data-stu-id="22b0f-506">Since caching is now done automatically without the use of a CompiledQuery, we call this feature “autocompiled queries”.</span></span> <span data-ttu-id="22b0f-507">Sorgu planı önbelleği ve mekanizması hakkında daha fazla bilgi için bkz. sorgu planı önbelleğe alma.</span><span class="sxs-lookup"><span data-stu-id="22b0f-507">For more information about the query plan cache and its mechanics, see Query Plan Caching.</span></span>

<span data-ttu-id="22b0f-508">Entity Framework, bir sorgunun yeniden derlenmesi gereken zaman algılar ve sorgu daha önce derlense bile çağrıldığında olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-508">Entity Framework detects when a query requires to be recompiled, and does so when the query is invoked even if it had been compiled before.</span></span> <span data-ttu-id="22b0f-509">Sorgunun yeniden derlenmesine neden olan genel koşullar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="22b0f-509">Common conditions that cause the query to be recompiled are:</span></span>

-   <span data-ttu-id="22b0f-510">Sorgunuzla ilişkili MergeOption değiştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="22b0f-510">Changing the MergeOption associated to your query.</span></span> <span data-ttu-id="22b0f-511">Önbelleğe alınmış sorgu kullanılmayacak, bunun yerine plan derleyicisi yeniden çalışır ve yeni oluşturulan plan önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-511">The cached query will not be used, instead the plan compiler will run again and the newly created plan gets cached.</span></span>
-   <span data-ttu-id="22b0f-512">ContextOptions. UseCSharpNullComparisonBehavior değeri değiştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="22b0f-512">Changing the value of ContextOptions.UseCSharpNullComparisonBehavior.</span></span> <span data-ttu-id="22b0f-513">Birleştirme seçeneğini değiştirme ile aynı etkiyi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="22b0f-513">You get the same effect as changing the MergeOption.</span></span>

<span data-ttu-id="22b0f-514">Diğer koşullar, sorgunuzun önbelleği kullanmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-514">Other conditions can prevent your query from using the cache.</span></span> <span data-ttu-id="22b0f-515">Sık karşılaşılan örnekler:</span><span class="sxs-lookup"><span data-stu-id="22b0f-515">Common examples are:</span></span>

-   <span data-ttu-id="22b0f-516">IEnumerable&lt;T&gt;kullanılıyor.&lt;&gt;(T değeri) içerir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-516">Using IEnumerable&lt;T&gt;.Contains&lt;&gt;(T value).</span></span>
-   <span data-ttu-id="22b0f-517">Sabitler ile sorgu üreten işlevleri kullanma.</span><span class="sxs-lookup"><span data-stu-id="22b0f-517">Using functions that produce queries with constants.</span></span>
-   <span data-ttu-id="22b0f-518">Eşlenmeyen bir nesnenin özelliklerini kullanma.</span><span class="sxs-lookup"><span data-stu-id="22b0f-518">Using the properties of a non-mapped object.</span></span>
-   <span data-ttu-id="22b0f-519">Sorgunuzu yeniden derlenmesi gereken başka bir sorguya bağlama.</span><span class="sxs-lookup"><span data-stu-id="22b0f-519">Linking your query to another query that requires to be recompiled.</span></span>

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a><span data-ttu-id="22b0f-520">IEnumerable&lt;T&gt;kullanılarak 4,1.&lt;T&gt;içerir (T değeri)</span><span class="sxs-lookup"><span data-stu-id="22b0f-520">4.1 Using IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value)</span></span>

<span data-ttu-id="22b0f-521">Entity Framework IEnumerable&lt;T&gt;çağıran sorguları önbelleğe almaz. Koleksiyonun değerleri geçici olarak değerlendirildiğinden, bellek içi bir koleksiyonda&lt;T&gt;(T değeri) bulunur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-521">Entity Framework does not cache queries that invoke IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value) against an in-memory collection, since the values of the collection are considered volatile.</span></span> <span data-ttu-id="22b0f-522">Aşağıdaki örnek sorgu önbelleğe alınmayacak, bu nedenle plan derleyicisi tarafından her zaman işlenir:</span><span class="sxs-lookup"><span data-stu-id="22b0f-522">The following example query will not be cached, so it will always be processed by the plan compiler:</span></span>

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

<span data-ttu-id="22b0f-523">Içerdiği IEnumerable 'ın, sorgunuzun ne kadar hızlı veya nasıl derlendiğini belirleyen bir boyut olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-523">Note that the size of the IEnumerable against which Contains is executed determines how fast or how slow your query is compiled.</span></span> <span data-ttu-id="22b0f-524">Yukarıdaki örnekte gösterildiği gibi büyük koleksiyonlar kullanılırken performans önemli ölçüde düşebilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-524">Performance can suffer significantly when using large collections such as the one shown in the example above.</span></span>

<span data-ttu-id="22b0f-525">Entity Framework 6, IEnumerable&lt;T&gt;yönteme iyileştirmeler içerir. Sorgular yürütüldüğünde&lt;T&gt;(T değeri) içerir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-525">Entity Framework 6 contains optimizations to the way IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value) works when queries are executed.</span></span> <span data-ttu-id="22b0f-526">Oluşturulan SQL kodu daha hızlıdır ve daha fazla okunabilir ve çoğu durumda sunucuda daha hızlı yürütülür.</span><span class="sxs-lookup"><span data-stu-id="22b0f-526">The SQL code that is generated is much faster to produce and more readable, and in most cases it also executes faster in the server.</span></span>

### <a name="42-using-functions-that-produce-queries-with-constants"></a><span data-ttu-id="22b0f-527">sabitler ile sorgu üreten işlevleri kullanarak 4,2</span><span class="sxs-lookup"><span data-stu-id="22b0f-527">4.2 Using functions that produce queries with constants</span></span>

<span data-ttu-id="22b0f-528">Skip (), take (), Contains () ve Defautıempty () LINQ işleçleri, parametreleri olan SQL sorguları oluşturmaz, ancak bunun yerine, bunlara sabitler olarak geçirilen değerleri yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-528">The Skip(), Take(), Contains() and DefautIfEmpty() LINQ operators do not produce SQL queries with parameters but instead put the values passed to them as constants.</span></span> <span data-ttu-id="22b0f-529">Bu nedenle, başka bir şekilde özdeş olabilecek sorgular, her ikisi de EF Stack ve veritabanı sunucusunda sorgu planı önbelleğini yoklamaya ve aynı sabitler sonraki bir sorgu yürütmesinde kullanılmadıkça yeniden kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-529">Because of this, queries that might otherwise be identical end up polluting the query plan cache, both on the EF stack and on the database server, and do not get reutilized unless the same constants are used in a subsequent query execution.</span></span> <span data-ttu-id="22b0f-530">Örnek:</span><span class="sxs-lookup"><span data-stu-id="22b0f-530">For example:</span></span>

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

<span data-ttu-id="22b0f-531">Bu örnekte, bu sorgu ID için farklı bir değerle yürütüldüğünde sorgu yeni bir plana derlenir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-531">In this example, each time this query is executed with a different value for id the query will be compiled into a new plan.</span></span>

<span data-ttu-id="22b0f-532">Özellikle de disk belleği alırken atlama ve alma işlemleri için dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="22b0f-532">In particular pay attention to the use of Skip and Take when doing paging.</span></span> <span data-ttu-id="22b0f-533">EF6 içinde bu yöntemlerin, bu yöntemlere geçirilen değişkenleri yakalayıp SQLparameters 'a çevirebildiğinden, önbelleğe alınmış sorgu planını etkin bir şekilde yeniden kullanılabilir hale getiren bir lambda aşırı yüklemesi vardır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-533">In EF6 these methods have a lambda overload that effectively makes the cached query plan reusable because EF can capture variables passed to these methods and translate them to SQLparameters.</span></span> <span data-ttu-id="22b0f-534">Bu Ayrıca, atlama ve alma için farklı bir sabite sahip her bir sorgu kendi sorgu planı önbellek girişini alacak şekilde önbelleğin temizleyiciyi tutmaya yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-534">This also helps keep the cache cleaner since otherwise each query with a different constant for Skip and Take would get its own query plan cache entry.</span></span>

<span data-ttu-id="22b0f-535">Aşağıdaki kodu göz önünde bulundurun, ancak bu, yalnızca bu sorgu sınıfını muaf tutmak amacıyla geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="22b0f-535">Consider the following code, which is suboptimal but is only meant to exemplify this class of queries:</span></span>

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

<span data-ttu-id="22b0f-536">Aynı kodun daha hızlı bir sürümü lambda ile Skip yöntemini çağırmayı içerir:</span><span class="sxs-lookup"><span data-stu-id="22b0f-536">A faster version of this same code would involve calling Skip with a lambda:</span></span>

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

<span data-ttu-id="22b0f-537">İkinci kod parçacığı, sorgu her çalıştırıldığında aynı sorgu planı kullanıldığından, bu, CPU süresini kaydeden ve sorgu önbelleğinin kirletmesini önleyen bir kez daha hızlı çalışıyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-537">The second snippet may run up to 11% faster because the same query plan is used every time the query is run, which saves CPU time and avoids polluting the query cache.</span></span> <span data-ttu-id="22b0f-538">Ayrıca, atlanacak parametre bir kapanış içinde olduğundan, kod şu anda şöyle de görünebilir:</span><span class="sxs-lookup"><span data-stu-id="22b0f-538">Furthermore, because the parameter to Skip is in a closure the code might as well look like this now:</span></span>

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a><span data-ttu-id="22b0f-539">4,3 eşlenmeyen bir nesnenin özelliklerini kullanma</span><span class="sxs-lookup"><span data-stu-id="22b0f-539">4.3 Using the properties of a non-mapped object</span></span>

<span data-ttu-id="22b0f-540">Bir sorgu, eşlenmiş olmayan bir nesne türünün özelliklerini parametre olarak kullandığında sorgu önbelleğe alınmaz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-540">When a query uses the properties of a non-mapped object type as a parameter then the query will not get cached.</span></span> <span data-ttu-id="22b0f-541">Örnek:</span><span class="sxs-lookup"><span data-stu-id="22b0f-541">For example:</span></span>

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

<span data-ttu-id="22b0f-542">Bu örnekte, yönetilmeyen türdeki sınıfın varlık modelinin parçası olmadığını varsayın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-542">In this example, assume that class NonMappedType is not part of the Entity model.</span></span> <span data-ttu-id="22b0f-543">Bu sorgu, eşlenmemiş bir tür kullanmadan kolayca değiştirilebilir ve bunun yerine sorgunun parametresi olarak yerel bir değişken kullanın:</span><span class="sxs-lookup"><span data-stu-id="22b0f-543">This query can easily be changed to not use a non-mapped type and instead use a local variable as the parameter to the query:</span></span>

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

<span data-ttu-id="22b0f-544">Bu durumda, sorgu önbelleğe alınabilecektir ve sorgu planı önbelleğinden faydalanır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-544">In this case, the query will be able to get cached and will benefit from the query plan cache.</span></span>

### <a name="44-linking-to-queries-that-require-recompiling"></a><span data-ttu-id="22b0f-545">4,4 yeniden derleme gerektiren sorgulara bağlama</span><span class="sxs-lookup"><span data-stu-id="22b0f-545">4.4 Linking to queries that require recompiling</span></span>

<span data-ttu-id="22b0f-546">Yukarıdaki örnekteki aynı örneği izleyerek, yeniden derlenmesi gereken bir sorguyu temel alan ikinci bir sorgunuz varsa ikinci sorgunuzun tamamı de yeniden derlenir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-546">Following the same example as above, if you have a second query that relies on a query that needs to be recompiled, your entire second query will also be recompiled.</span></span> <span data-ttu-id="22b0f-547">Bu senaryoyu göstermek için bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="22b0f-547">Here’s an example to illustrate this scenario:</span></span>

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

<span data-ttu-id="22b0f-548">Örnek geneldir, ancak firstQuery 'ye bağlanmanın, secondQuery 'nin önbelleğe alınmamasına neden olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-548">The example is generic, but it illustrates how linking to firstQuery is causing secondQuery to be unable to get cached.</span></span> <span data-ttu-id="22b0f-549">FirstQuery yeniden derleme gerektiren bir sorgu olsaydı, secondQuery önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-549">If firstQuery had not been a query that requires recompiling, then secondQuery would have been cached.</span></span>

## <a name="5-notracking-queries"></a><span data-ttu-id="22b0f-550">5 NoTracking sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-550">5 NoTracking Queries</span></span>

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a><span data-ttu-id="22b0f-551">5,1 durum yönetimi yükünü azaltmak için değişiklik izlemeyi devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="22b0f-551">5.1 Disabling change tracking to reduce state management overhead</span></span>

<span data-ttu-id="22b0f-552">Yalnızca bir salt okuma senaryosuyla karşılaşdıysanız ve nesneleri ObjectStateManager 'a yükleme yükünden kaçınmak istiyorsanız, "Izleme yok" sorguları gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-552">If you are in a read-only scenario and want to avoid the overhead of loading the objects into the ObjectStateManager, you can issue "No Tracking" queries.</span></span><span data-ttu-id="22b0f-553">  Değişiklik izleme, sorgu düzeyinde devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-553">  Change tracking can be disabled at the query level.</span></span>

<span data-ttu-id="22b0f-554">Değişiklik izlemeyi devre dışı bırakarak nesne önbelleğinin etkin bir şekilde kapatılmasını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-554">Note though that by disabling change tracking you are effectively turning off the object cache.</span></span> <span data-ttu-id="22b0f-555">Bir varlık için sorgulama yaptığınızda, ObjectStateManager 'dan daha önce gerçekleştirilmiş sorgu sonuçlarını çekerek gerçekleştirmesi 'ı atlayamıyoruz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-555">When you query for an entity, we can't skip materialization by pulling the previously-materialized query results from the ObjectStateManager.</span></span> <span data-ttu-id="22b0f-556">Aynı bağlamda aynı varlıkları tekrar tekrar sorguladıysanız değişiklik izlemeyi etkinleştirmenin bir performans avantajı görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-556">If you are repeatedly querying for the same entities on the same context, you might actually see a performance benefit from enabling change tracking.</span></span>

<span data-ttu-id="22b0f-557">ObjectContext kullanılarak sorgulama yaparken, ObjectQuery ve ObjectSet örnekleri, ayarlandıktan sonra bir MergeOption hatırlayacaktır ve üzerinde oluşturulan sorgular üst sorgunun etkin birleştirme seçeneğini miras alır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-557">When querying using ObjectContext, ObjectQuery and ObjectSet instances will remember a MergeOption once it is set, and queries that are composed on them will inherit the effective MergeOption of the parent query.</span></span> <span data-ttu-id="22b0f-558">DbContext kullanılırken izleme, DbSet üzerinde AsNoTracking () değiştiricisi çağırarak devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-558">When using DbContext, tracking can be disabled by calling the AsNoTracking() modifier on the DbSet.</span></span>

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a><span data-ttu-id="22b0f-559">DbContext kullanılırken bir sorgu için değişiklik izlemeyi devre dışı bırakma 5.1.1</span><span class="sxs-lookup"><span data-stu-id="22b0f-559">5.1.1 Disabling change tracking for a query when using DbContext</span></span>

<span data-ttu-id="22b0f-560">Sorgudaki AsNoTracking () yöntemine yapılan çağrıyı zincirleyerek bir sorgunun modunu NoTracking öğesine geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-560">You can switch the mode of a query to NoTracking by chaining a call to the AsNoTracking() method in the query.</span></span> <span data-ttu-id="22b0f-561">ObjectQuery 'den farklı olarak, DbContext API 'sindeki DbSet ve DbQuery sınıfları MergeOption için kesilebilir bir özelliğe sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-561">Unlike ObjectQuery, the DbSet and DbQuery classes in the DbContext API don’t have a mutable property for the MergeOption.</span></span>

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a><span data-ttu-id="22b0f-562">ObjectContext kullanarak sorgu düzeyinde değişiklik izlemeyi devre dışı bırakma 5.1.2</span><span class="sxs-lookup"><span data-stu-id="22b0f-562">5.1.2 Disabling change tracking at the query level using ObjectContext</span></span>

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a><span data-ttu-id="22b0f-563">ObjectContext kullanarak tüm varlık kümesi için değişiklik izlemeyi devre dışı bırakma 5.1.3</span><span class="sxs-lookup"><span data-stu-id="22b0f-563">5.1.3 Disabling change tracking for an entire entity set using ObjectContext</span></span>

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a><span data-ttu-id="22b0f-564">5,2, NoTracking sorgularının performans avantajını gösteren test ölçümleri</span><span class="sxs-lookup"><span data-stu-id="22b0f-564">5.2 Test Metrics demonstrating the performance benefit of NoTracking queries</span></span>

<span data-ttu-id="22b0f-565">Bu testte, Izlemeyi Navision modeli için NoTracking sorgularıyla karşılaştırarak ObjectStateManager 'ı doldurma maliyetine göz atacağız.</span><span class="sxs-lookup"><span data-stu-id="22b0f-565">In this test we look at the cost of filling the ObjectStateManager by comparing Tracking to NoTracking queries for the Navision model.</span></span> <span data-ttu-id="22b0f-566">Navision modelinin açıklaması ve yürütülen sorgu türleri için ek başlığına bakın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-566">See the appendix for a description of the Navision model and the types of queries which were executed.</span></span> <span data-ttu-id="22b0f-567">Bu testte sorgular listesinden yineleme yapılır ve her birini bir kez yürütüyoruz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-567">In this test, we iterate through the list of queries and execute each one once.</span></span> <span data-ttu-id="22b0f-568">Bir testin iki çeşitlemeyi, NoTracking sorgularıyla ve bir kez "AppendOnly" varsayılan birleştirme seçeneğiyle çalıştırdık.</span><span class="sxs-lookup"><span data-stu-id="22b0f-568">We ran two variations of the test, once with NoTracking queries and once with the default merge option of "AppendOnly".</span></span> <span data-ttu-id="22b0f-569">Her bir çeşitleme 3 kez çalıştık ve çalıştırmaların ortalama değerini alır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-569">We ran each variation 3 times and take the mean value of the runs.</span></span> <span data-ttu-id="22b0f-570">Testler arasında SQL Server sorgu önbelleğini temizler ve aşağıdaki komutları çalıştırarak tempdb 'yi küçülttik:</span><span class="sxs-lookup"><span data-stu-id="22b0f-570">Between the tests we clear the query cache on the SQL Server and shrink the tempdb by running the following commands:</span></span>

1.  <span data-ttu-id="22b0f-571">DBCC DROPCLEANARABELLEKLERI</span><span class="sxs-lookup"><span data-stu-id="22b0f-571">DBCC DROPCLEANBUFFERS</span></span>
2.  <span data-ttu-id="22b0f-572">DBCC FREEPROCCACHE</span><span class="sxs-lookup"><span data-stu-id="22b0f-572">DBCC FREEPROCCACHE</span></span>
3.  <span data-ttu-id="22b0f-573">DBCC SHRINKDATABASE (tempdb, 0)</span><span class="sxs-lookup"><span data-stu-id="22b0f-573">DBCC SHRINKDATABASE (tempdb, 0)</span></span>

<span data-ttu-id="22b0f-574">Test Sonuçları, 3 üzerinde ortanca çalışma:</span><span class="sxs-lookup"><span data-stu-id="22b0f-574">Test Results, median over 3 runs:</span></span>

|                        | <span data-ttu-id="22b0f-575">IZLEME YOK – ÇALıŞMA KÜMESI</span><span class="sxs-lookup"><span data-stu-id="22b0f-575">NO TRACKING – WORKING SET</span></span> | <span data-ttu-id="22b0f-576">IZLEME YOK – ZAMAN</span><span class="sxs-lookup"><span data-stu-id="22b0f-576">NO TRACKING – TIME</span></span> | <span data-ttu-id="22b0f-577">YALNıZCA APPEND – ÇALıŞMA KÜMESI</span><span class="sxs-lookup"><span data-stu-id="22b0f-577">APPEND ONLY – WORKING SET</span></span> | <span data-ttu-id="22b0f-578">YALNıZCA APPEND – SAAT</span><span class="sxs-lookup"><span data-stu-id="22b0f-578">APPEND ONLY – TIME</span></span> |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| <span data-ttu-id="22b0f-579">**Entity Framework 5**</span><span class="sxs-lookup"><span data-stu-id="22b0f-579">**Entity Framework 5**</span></span> | <span data-ttu-id="22b0f-580">460361728</span><span class="sxs-lookup"><span data-stu-id="22b0f-580">460361728</span></span>                 | <span data-ttu-id="22b0f-581">1163536 MS</span><span class="sxs-lookup"><span data-stu-id="22b0f-581">1163536 ms</span></span>         | <span data-ttu-id="22b0f-582">596545536</span><span class="sxs-lookup"><span data-stu-id="22b0f-582">596545536</span></span>                 | <span data-ttu-id="22b0f-583">1273042 MS</span><span class="sxs-lookup"><span data-stu-id="22b0f-583">1273042 ms</span></span>         |
| <span data-ttu-id="22b0f-584">**Entity Framework 6**</span><span class="sxs-lookup"><span data-stu-id="22b0f-584">**Entity Framework 6**</span></span> | <span data-ttu-id="22b0f-585">647127040</span><span class="sxs-lookup"><span data-stu-id="22b0f-585">647127040</span></span>                 | <span data-ttu-id="22b0f-586">190228 MS</span><span class="sxs-lookup"><span data-stu-id="22b0f-586">190228 ms</span></span>          | <span data-ttu-id="22b0f-587">832798720</span><span class="sxs-lookup"><span data-stu-id="22b0f-587">832798720</span></span>                 | <span data-ttu-id="22b0f-588">195521 MS</span><span class="sxs-lookup"><span data-stu-id="22b0f-588">195521 ms</span></span>          |

<span data-ttu-id="22b0f-589">Entity Framework 5 ' ün sonunda Entity Framework 6 ' dan daha küçük bir bellek parmak izi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-589">Entity Framework 5 will have a smaller memory footprint at the end of the run than Entity Framework 6 does.</span></span> <span data-ttu-id="22b0f-590">Entity Framework 6 tarafından tüketilen ek bellek, yeni özellikleri ve daha iyi performans sağlayan ek bellek yapılarının ve kodların sonucudur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-590">The additional memory consumed by Entity Framework 6 is the result of additional memory structures and code that enable new features and better performance.</span></span>

<span data-ttu-id="22b0f-591">Ayrıca, ObjectStateManager kullanılırken bellek parmak izde net bir farklılık vardır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-591">There’s also a clear difference in memory footprint when using the ObjectStateManager.</span></span> <span data-ttu-id="22b0f-592">Entity Framework 5, veritabanından gerçekleştirilmiş olan tüm varlıkların izini tutarken, %30 ' a kadar olan ayak izini artırmıştır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-592">Entity Framework 5 increased its footprint by 30% when keeping track of all the entities we materialized from the database.</span></span> <span data-ttu-id="22b0f-593">Entity Framework 6 ' nın parmak izi %28 ' i arttı.</span><span class="sxs-lookup"><span data-stu-id="22b0f-593">Entity Framework 6 increased its footprint by 28% when doing so.</span></span>

<span data-ttu-id="22b0f-594">Zaman içinde, Entity Framework 6 out, bu testte büyük bir kenar boşluğu ile Entity Framework 5 gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-594">In terms of time, Entity Framework 6 outperforms Entity Framework 5 in this test by a large margin.</span></span> <span data-ttu-id="22b0f-595">Entity Framework 6, testi yaklaşık %16 ' da Entity Framework 5 tarafından tüketilen zamanın %16 ' da tamamladı.</span><span class="sxs-lookup"><span data-stu-id="22b0f-595">Entity Framework 6 completed the test in roughly 16% of the time consumed by Entity Framework 5.</span></span> <span data-ttu-id="22b0f-596">Ayrıca, Entity Framework 5, ObjectStateManager kullanılırken tamamlanacak %9 daha fazla zaman alır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-596">Additionally, Entity Framework 5 takes 9% more time to complete when the ObjectStateManager is being used.</span></span> <span data-ttu-id="22b0f-597">Buna karşılık, Entity Framework 6, ObjectStateManager kullanırken 3 ' ü daha fazla kez kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="22b0f-597">In comparison, Entity Framework 6 is using 3% more time when using the ObjectStateManager.</span></span>

## <a name="6-query-execution-options"></a><span data-ttu-id="22b0f-598">6 sorgu yürütme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="22b0f-598">6 Query Execution Options</span></span>

<span data-ttu-id="22b0f-599">Entity Framework, sorgulamak için birkaç farklı yol sunar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-599">Entity Framework offers several different ways to query.</span></span> <span data-ttu-id="22b0f-600">Aşağıdaki seçeneklere göz atacağız, her birinin profesyonelleri ve dezavantajlarını karşılaştırıyoruz ve performans özelliklerini inceliyoruz:</span><span class="sxs-lookup"><span data-stu-id="22b0f-600">We'll take a look at the following options, compare the pros and cons of each, and examine their performance characteristics:</span></span>

-   <span data-ttu-id="22b0f-601">LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="22b0f-601">LINQ to Entities.</span></span>
-   <span data-ttu-id="22b0f-602">Izleme LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="22b0f-602">No Tracking LINQ to Entities.</span></span>
-   <span data-ttu-id="22b0f-603">Bir ObjectQuery üzerinde Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="22b0f-603">Entity SQL over an ObjectQuery.</span></span>
-   <span data-ttu-id="22b0f-604">Bir EntityCommand üzerinden Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="22b0f-604">Entity SQL over an EntityCommand.</span></span>
-   <span data-ttu-id="22b0f-605">ExecuteStoreQuery.</span><span class="sxs-lookup"><span data-stu-id="22b0f-605">ExecuteStoreQuery.</span></span>
-   <span data-ttu-id="22b0f-606">SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="22b0f-606">SqlQuery.</span></span>
-   <span data-ttu-id="22b0f-607">CompiledQuery.</span><span class="sxs-lookup"><span data-stu-id="22b0f-607">CompiledQuery.</span></span>

### <a name="61-linq-to-entities-queries"></a><span data-ttu-id="22b0f-608">6,1 LINQ to Entities sorguları</span><span class="sxs-lookup"><span data-stu-id="22b0f-608">6.1       LINQ to Entities queries</span></span>

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="22b0f-609">**Artıları**</span><span class="sxs-lookup"><span data-stu-id="22b0f-609">**Pros**</span></span>

-   <span data-ttu-id="22b0f-610">CUD işlemlerine uygun.</span><span class="sxs-lookup"><span data-stu-id="22b0f-610">Suitable for CUD operations.</span></span>
-   <span data-ttu-id="22b0f-611">Tam gerçekleştirilmiş nesneler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-611">Fully materialized objects.</span></span>
-   <span data-ttu-id="22b0f-612">Programlama diline yerleşik sözdizimi ile yazmak en basit.</span><span class="sxs-lookup"><span data-stu-id="22b0f-612">Simplest to write with syntax built into the programming language.</span></span>
-   <span data-ttu-id="22b0f-613">İyi performans.</span><span class="sxs-lookup"><span data-stu-id="22b0f-613">Good performance.</span></span>

<span data-ttu-id="22b0f-614">**Eksileri**</span><span class="sxs-lookup"><span data-stu-id="22b0f-614">**Cons**</span></span>

-   <span data-ttu-id="22b0f-615">Gibi belirli teknik kısıtlamalar:</span><span class="sxs-lookup"><span data-stu-id="22b0f-615">Certain technical restrictions, such as:</span></span>
    -   <span data-ttu-id="22b0f-616">Dış BIRLEŞIM sorguları için Defaultıempty kullanan desenler Entity SQL ' deki basit dış BIRLEŞIM deyimlerinden daha karmaşık sorgularla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-616">Patterns using DefaultIfEmpty for OUTER JOIN queries result in more complex queries than simple OUTER JOIN statements in Entity SQL.</span></span>
    -   <span data-ttu-id="22b0f-617">Hala genel kalıp eşleme ile gıbı kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="22b0f-617">You still can’t use LIKE with general pattern matching.</span></span>

### <a name="62-no-tracking-linq-to-entities-queries"></a><span data-ttu-id="22b0f-618">6,2 Izleme LINQ to Entities sorgu yok</span><span class="sxs-lookup"><span data-stu-id="22b0f-618">6.2       No Tracking LINQ to Entities queries</span></span>

<span data-ttu-id="22b0f-619">Bağlam ObjectContext 'e türetiliyor:</span><span class="sxs-lookup"><span data-stu-id="22b0f-619">When the context derives ObjectContext:</span></span>

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="22b0f-620">Bağlam DbContext türetiliyor:</span><span class="sxs-lookup"><span data-stu-id="22b0f-620">When the context derives DbContext:</span></span>

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="22b0f-621">**Artıları**</span><span class="sxs-lookup"><span data-stu-id="22b0f-621">**Pros**</span></span>

-   <span data-ttu-id="22b0f-622">Normal LINQ sorguları üzerinden geliştirilmiş performans.</span><span class="sxs-lookup"><span data-stu-id="22b0f-622">Improved performance over regular LINQ queries.</span></span>
-   <span data-ttu-id="22b0f-623">Tam gerçekleştirilmiş nesneler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-623">Fully materialized objects.</span></span>
-   <span data-ttu-id="22b0f-624">Programlama diline yerleşik sözdizimi ile yazmak en basit.</span><span class="sxs-lookup"><span data-stu-id="22b0f-624">Simplest to write with syntax built into the programming language.</span></span>

<span data-ttu-id="22b0f-625">**Eksileri**</span><span class="sxs-lookup"><span data-stu-id="22b0f-625">**Cons**</span></span>

-   <span data-ttu-id="22b0f-626">CUD işlemleri için uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-626">Not suitable for CUD operations.</span></span>
-   <span data-ttu-id="22b0f-627">Gibi belirli teknik kısıtlamalar:</span><span class="sxs-lookup"><span data-stu-id="22b0f-627">Certain technical restrictions, such as:</span></span>
    -   <span data-ttu-id="22b0f-628">Dış BIRLEŞIM sorguları için Defaultıempty kullanan desenler Entity SQL ' deki basit dış BIRLEŞIM deyimlerinden daha karmaşık sorgularla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-628">Patterns using DefaultIfEmpty for OUTER JOIN queries result in more complex queries than simple OUTER JOIN statements in Entity SQL.</span></span>
    -   <span data-ttu-id="22b0f-629">Hala genel kalıp eşleme ile gıbı kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="22b0f-629">You still can’t use LIKE with general pattern matching.</span></span>

<span data-ttu-id="22b0f-630">NoTracking belirtilmemiş olsa bile, proje skaler özelliklerinin izlenmediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-630">Note that queries that project scalar properties are not tracked even if the NoTracking is not specified.</span></span> <span data-ttu-id="22b0f-631">Örnek:</span><span class="sxs-lookup"><span data-stu-id="22b0f-631">For example:</span></span>

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

<span data-ttu-id="22b0f-632">Bu belirli sorgu açıkça NoTracking olarak belirtilmiyor, ancak nesne durumu Yöneticisi tarafından bilinen bir tür olmadığından gerçekleştirilmiş sonuç izlenmiyor.</span><span class="sxs-lookup"><span data-stu-id="22b0f-632">This particular query doesn’t explicitly specify being NoTracking, but since it’s not materializing a type that’s known to the object state manager then the materialized result is not tracked.</span></span>

### <a name="63-entity-sql-over-an-objectquery"></a><span data-ttu-id="22b0f-633">ObjectQuery üzerinde 6,3 Entity SQL</span><span class="sxs-lookup"><span data-stu-id="22b0f-633">6.3       Entity SQL over an ObjectQuery</span></span>

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

<span data-ttu-id="22b0f-634">**Artıları**</span><span class="sxs-lookup"><span data-stu-id="22b0f-634">**Pros**</span></span>

-   <span data-ttu-id="22b0f-635">CUD işlemlerine uygun.</span><span class="sxs-lookup"><span data-stu-id="22b0f-635">Suitable for CUD operations.</span></span>
-   <span data-ttu-id="22b0f-636">Tam gerçekleştirilmiş nesneler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-636">Fully materialized objects.</span></span>
-   <span data-ttu-id="22b0f-637">Sorgu planı önbelleğe almayı destekler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-637">Supports query plan caching.</span></span>

<span data-ttu-id="22b0f-638">**Eksileri**</span><span class="sxs-lookup"><span data-stu-id="22b0f-638">**Cons**</span></span>

-   <span data-ttu-id="22b0f-639">Dile yerleştirilmiş sorgu yapılarından daha fazla kullanıcı hatası ile daha fazla olan metinsel Sorgu dizelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-639">Involves textual query strings which are more prone to user error than query constructs built into the language.</span></span>

### <a name="64-entity-sql-over-an-entity-command"></a><span data-ttu-id="22b0f-640">bir varlık komutu üzerinden 6,4 Entity SQL</span><span class="sxs-lookup"><span data-stu-id="22b0f-640">6.4       Entity SQL over an Entity Command</span></span>

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

<span data-ttu-id="22b0f-641">**Artıları**</span><span class="sxs-lookup"><span data-stu-id="22b0f-641">**Pros**</span></span>

-   <span data-ttu-id="22b0f-642">.NET 4,0 ' de sorgu planı önbelleğe almayı destekler (plan önbelleği, .NET 4,5 ' deki diğer tüm sorgu türleri tarafından desteklenir).</span><span class="sxs-lookup"><span data-stu-id="22b0f-642">Supports query plan caching in .NET 4.0 (plan caching is supported by all other query types in .NET 4.5).</span></span>

<span data-ttu-id="22b0f-643">**Eksileri**</span><span class="sxs-lookup"><span data-stu-id="22b0f-643">**Cons**</span></span>

-   <span data-ttu-id="22b0f-644">Dile yerleştirilmiş sorgu yapılarından daha fazla kullanıcı hatası ile daha fazla olan metinsel Sorgu dizelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-644">Involves textual query strings which are more prone to user error than query constructs built into the language.</span></span>
-   <span data-ttu-id="22b0f-645">CUD işlemleri için uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-645">Not suitable for CUD operations.</span></span>
-   <span data-ttu-id="22b0f-646">Sonuçlar otomatik olarak gerçekleştirilmez ve veri okuyucudan okunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-646">Results are not automatically materialized, and must be read from the data reader.</span></span>

### <a name="65-sqlquery-and-executestorequery"></a><span data-ttu-id="22b0f-647">6,5 SqlQuery ve ExecuteStoreQuery</span><span class="sxs-lookup"><span data-stu-id="22b0f-647">6.5       SqlQuery and ExecuteStoreQuery</span></span>

<span data-ttu-id="22b0f-648">Veritabanında SqlQuery:</span><span class="sxs-lookup"><span data-stu-id="22b0f-648">SqlQuery on Database:</span></span>

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

<span data-ttu-id="22b0f-649">DbSet üzerinde SqlQuery:</span><span class="sxs-lookup"><span data-stu-id="22b0f-649">SqlQuery on DbSet:</span></span>

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

<span data-ttu-id="22b0f-650">ExecyteStoreQuery:</span><span class="sxs-lookup"><span data-stu-id="22b0f-650">ExecyteStoreQuery:</span></span>

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

<span data-ttu-id="22b0f-651">**Artıları**</span><span class="sxs-lookup"><span data-stu-id="22b0f-651">**Pros**</span></span>

-   <span data-ttu-id="22b0f-652">Plan derleyicisi atlandığından genellikle en hızlı performans.</span><span class="sxs-lookup"><span data-stu-id="22b0f-652">Generally fastest performance since plan compiler is bypassed.</span></span>
-   <span data-ttu-id="22b0f-653">Tam gerçekleştirilmiş nesneler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-653">Fully materialized objects.</span></span>
-   <span data-ttu-id="22b0f-654">DbSet 'ten kullanıldığında CUD işlemlerine uygun.</span><span class="sxs-lookup"><span data-stu-id="22b0f-654">Suitable for CUD operations when used from the DbSet.</span></span>

<span data-ttu-id="22b0f-655">**Eksileri**</span><span class="sxs-lookup"><span data-stu-id="22b0f-655">**Cons**</span></span>

-   <span data-ttu-id="22b0f-656">Sorgu metinsel ve hataya açıktır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-656">Query is textual and error prone.</span></span>
-   <span data-ttu-id="22b0f-657">Sorgu, kavramsal semantik yerine depo semantiğini kullanarak belirli bir arka uca bağlanır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-657">Query is tied to a specific backend by using store semantics instead of conceptual semantics.</span></span>
-   <span data-ttu-id="22b0f-658">Devralma mevcut olduğunda, el ile oluşturulmuş sorgunun istenen tür için eşleme koşullarını hesaba eklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-658">When inheritance is present, handcrafted query needs to account for mapping conditions for the type requested.</span></span>

### <a name="66-compiledquery"></a><span data-ttu-id="22b0f-659">6,6 CompiledQuery</span><span class="sxs-lookup"><span data-stu-id="22b0f-659">6.6       CompiledQuery</span></span>

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

<span data-ttu-id="22b0f-660">**Artıları**</span><span class="sxs-lookup"><span data-stu-id="22b0f-660">**Pros**</span></span>

-   <span data-ttu-id="22b0f-661">Normal LINQ sorguları üzerinde en fazla %7 performans geliştirmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-661">Provides up to a 7% performance improvement over regular LINQ queries.</span></span>
-   <span data-ttu-id="22b0f-662">Tam gerçekleştirilmiş nesneler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-662">Fully materialized objects.</span></span>
-   <span data-ttu-id="22b0f-663">CUD işlemlerine uygun.</span><span class="sxs-lookup"><span data-stu-id="22b0f-663">Suitable for CUD operations.</span></span>

<span data-ttu-id="22b0f-664">**Eksileri**</span><span class="sxs-lookup"><span data-stu-id="22b0f-664">**Cons**</span></span>

-   <span data-ttu-id="22b0f-665">Artan karmaşıklık ve programlama yükü.</span><span class="sxs-lookup"><span data-stu-id="22b0f-665">Increased complexity and programming overhead.</span></span>
-   <span data-ttu-id="22b0f-666">Derlenmiş bir sorgunun üzerine oluştururken performans iyileştirmesi kaybolur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-666">The performance improvement is lost when composing on top of a compiled query.</span></span>
-   <span data-ttu-id="22b0f-667">Bazı LINQ sorguları bir CompiledQuery olarak yazılamaz; Örneğin, anonim türlerin tahminleri.</span><span class="sxs-lookup"><span data-stu-id="22b0f-667">Some LINQ queries can't be written as a CompiledQuery - for example, projections of anonymous types.</span></span>

### <a name="67-performance-comparison-of-different-query-options"></a><span data-ttu-id="22b0f-668">6,7 farklı sorgu seçenekleri performans karşılaştırması</span><span class="sxs-lookup"><span data-stu-id="22b0f-668">6.7       Performance Comparison of different query options</span></span>

<span data-ttu-id="22b0f-669">Bağlam oluşturma işlemi zaman aşımına uğramayan basit mikro kıyaslamalar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-669">Simple microbenchmarks where the context creation was not timed were put to the test.</span></span> <span data-ttu-id="22b0f-670">Denetlenen bir ortamda, önbelleğe alınmamış varlıkların bir kümesi için 5000 kez sorgu ölçüleceğini ölçüyoruz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-670">We measured querying 5000 times for a set of non-cached entities in a controlled environment.</span></span> <span data-ttu-id="22b0f-671">Bu numaralar bir uyarı ile birlikte alınırlar: bir uygulama tarafından üretilen gerçek sayıları yansıtmaz, ancak bunun yerine farklı sorgulama seçenekleri karşılaştırıldığında performans farkının ne kadarının olacağını çok doğru bir ölçüdür Yeni bağlam oluşturma maliyeti hariç, elmalar ve elmalar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-671">These numbers are to be taken with a warning: they do not reflect actual numbers produced by an application, but instead they are a very accurate measurement of how much of a performance difference there is when different querying options are compared apples-to-apples, excluding the cost of creating a new context.</span></span>

| <span data-ttu-id="22b0f-672">AŞV</span><span class="sxs-lookup"><span data-stu-id="22b0f-672">EF</span></span>  | <span data-ttu-id="22b0f-673">Test etme</span><span class="sxs-lookup"><span data-stu-id="22b0f-673">Test</span></span>                                 | <span data-ttu-id="22b0f-674">Zaman (MS)</span><span class="sxs-lookup"><span data-stu-id="22b0f-674">Time (ms)</span></span> | <span data-ttu-id="22b0f-675">Bellek</span><span class="sxs-lookup"><span data-stu-id="22b0f-675">Memory</span></span>   |
|:----|:-------------------------------------|:----------|:---------|
| <span data-ttu-id="22b0f-676">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-676">EF5</span></span> | <span data-ttu-id="22b0f-677">ObjectContext ESQL</span><span class="sxs-lookup"><span data-stu-id="22b0f-677">ObjectContext ESQL</span></span>                   | <span data-ttu-id="22b0f-678">2414</span><span class="sxs-lookup"><span data-stu-id="22b0f-678">2414</span></span>      | <span data-ttu-id="22b0f-679">38801408</span><span class="sxs-lookup"><span data-stu-id="22b0f-679">38801408</span></span> |
| <span data-ttu-id="22b0f-680">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-680">EF5</span></span> | <span data-ttu-id="22b0f-681">ObjectContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-681">ObjectContext Linq Query</span></span>             | <span data-ttu-id="22b0f-682">2692</span><span class="sxs-lookup"><span data-stu-id="22b0f-682">2692</span></span>      | <span data-ttu-id="22b0f-683">38277120</span><span class="sxs-lookup"><span data-stu-id="22b0f-683">38277120</span></span> |
| <span data-ttu-id="22b0f-684">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-684">EF5</span></span> | <span data-ttu-id="22b0f-685">DbContext LINQ sorgusu Izleme yok</span><span class="sxs-lookup"><span data-stu-id="22b0f-685">DbContext Linq Query No Tracking</span></span>     | <span data-ttu-id="22b0f-686">2818</span><span class="sxs-lookup"><span data-stu-id="22b0f-686">2818</span></span>      | <span data-ttu-id="22b0f-687">41840640</span><span class="sxs-lookup"><span data-stu-id="22b0f-687">41840640</span></span> |
| <span data-ttu-id="22b0f-688">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-688">EF5</span></span> | <span data-ttu-id="22b0f-689">DbContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-689">DbContext Linq Query</span></span>                 | <span data-ttu-id="22b0f-690">2930</span><span class="sxs-lookup"><span data-stu-id="22b0f-690">2930</span></span>      | <span data-ttu-id="22b0f-691">41771008</span><span class="sxs-lookup"><span data-stu-id="22b0f-691">41771008</span></span> |
| <span data-ttu-id="22b0f-692">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-692">EF5</span></span> | <span data-ttu-id="22b0f-693">ObjectContext LINQ sorgusu Izleme yok</span><span class="sxs-lookup"><span data-stu-id="22b0f-693">ObjectContext Linq Query No Tracking</span></span> | <span data-ttu-id="22b0f-694">3013</span><span class="sxs-lookup"><span data-stu-id="22b0f-694">3013</span></span>      | <span data-ttu-id="22b0f-695">38412288</span><span class="sxs-lookup"><span data-stu-id="22b0f-695">38412288</span></span> |
|     |                                      |           |          |
| <span data-ttu-id="22b0f-696">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-696">EF6</span></span> | <span data-ttu-id="22b0f-697">ObjectContext ESQL</span><span class="sxs-lookup"><span data-stu-id="22b0f-697">ObjectContext ESQL</span></span>                   | <span data-ttu-id="22b0f-698">2059</span><span class="sxs-lookup"><span data-stu-id="22b0f-698">2059</span></span>      | <span data-ttu-id="22b0f-699">46039040</span><span class="sxs-lookup"><span data-stu-id="22b0f-699">46039040</span></span> |
| <span data-ttu-id="22b0f-700">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-700">EF6</span></span> | <span data-ttu-id="22b0f-701">ObjectContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-701">ObjectContext Linq Query</span></span>             | <span data-ttu-id="22b0f-702">3074</span><span class="sxs-lookup"><span data-stu-id="22b0f-702">3074</span></span>      | <span data-ttu-id="22b0f-703">45248512</span><span class="sxs-lookup"><span data-stu-id="22b0f-703">45248512</span></span> |
| <span data-ttu-id="22b0f-704">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-704">EF6</span></span> | <span data-ttu-id="22b0f-705">DbContext LINQ sorgusu Izleme yok</span><span class="sxs-lookup"><span data-stu-id="22b0f-705">DbContext Linq Query No Tracking</span></span>     | <span data-ttu-id="22b0f-706">3125</span><span class="sxs-lookup"><span data-stu-id="22b0f-706">3125</span></span>      | <span data-ttu-id="22b0f-707">47575040</span><span class="sxs-lookup"><span data-stu-id="22b0f-707">47575040</span></span> |
| <span data-ttu-id="22b0f-708">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-708">EF6</span></span> | <span data-ttu-id="22b0f-709">DbContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-709">DbContext Linq Query</span></span>                 | <span data-ttu-id="22b0f-710">3420</span><span class="sxs-lookup"><span data-stu-id="22b0f-710">3420</span></span>      | <span data-ttu-id="22b0f-711">47652864</span><span class="sxs-lookup"><span data-stu-id="22b0f-711">47652864</span></span> |
| <span data-ttu-id="22b0f-712">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-712">EF6</span></span> | <span data-ttu-id="22b0f-713">ObjectContext LINQ sorgusu Izleme yok</span><span class="sxs-lookup"><span data-stu-id="22b0f-713">ObjectContext Linq Query No Tracking</span></span> | <span data-ttu-id="22b0f-714">3593</span><span class="sxs-lookup"><span data-stu-id="22b0f-714">3593</span></span>      | <span data-ttu-id="22b0f-715">45260800</span><span class="sxs-lookup"><span data-stu-id="22b0f-715">45260800</span></span> |

![EF5 mikro kıyaslamalar, 5000 sıcak yineleme](~/ef6/media/ef5micro5000warm.png)

![EF6 mikro kıyaslamalar, 5000 sıcak yineleme](~/ef6/media/ef6micro5000warm.png)

<span data-ttu-id="22b0f-718">Mikro kıyaslamalar, koddaki küçük değişikliklere çok duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-718">Microbenchmarks are very sensitive to small changes in the code.</span></span> <span data-ttu-id="22b0f-719">Bu durumda, Entity Framework 5 ve Entity Framework 6 ' nın maliyetleri arasındaki fark, [ele](~/ef6/fundamentals/logging-and-interception.md) alma ve [işlem geliştirmelerinden](~/ef6/saving/transactions.md)kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-719">In this case, the difference between the costs of Entity Framework 5 and Entity Framework 6 are due to the addition of [interception](~/ef6/fundamentals/logging-and-interception.md) and [transactional improvements](~/ef6/saving/transactions.md).</span></span> <span data-ttu-id="22b0f-720">Bununla birlikte, bu mikro kıyaslamalar sayıları, Entity Framework ne yaptığını çok küçük parçalara ayırır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-720">These microbenchmarks numbers, however, are an amplified vision into a very small fragment of what Entity Framework does.</span></span> <span data-ttu-id="22b0f-721">Entity Framework 5 ' ten Entity Framework 6 ' ya yükseltirken, normal sorguların gerçek zamanlı senaryolarında bir performans gerileme görmemelidir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-721">Real-world scenarios of warm queries should not see a performance regression when upgrading from Entity Framework 5 to Entity Framework 6.</span></span>

<span data-ttu-id="22b0f-722">Farklı sorgu seçeneklerinin gerçek dünya performansını karşılaştırmak için, kategori adı "Içecek" olan tüm ürünleri seçmek üzere farklı bir sorgu seçeneği kullandığımız 5 ayrı test varyasyonunu oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="22b0f-722">To compare the real-world performance of the different query options, we created 5 separate test variations where we use a different query option to select all products whose category name is "Beverages".</span></span> <span data-ttu-id="22b0f-723">Her yineleme, bağlam oluşturma maliyetinin yanı sıra döndürülen tüm varlıkların nasıl bir ücret almakta olduğunu içerir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-723">Each iteration includes the cost of creating the context, and the cost of materializing all returned entities.</span></span> <span data-ttu-id="22b0f-724">10 yineleme, 1000 süreli yinelemelerin toplamı alınmadan önce zaman aşımına uğramadan çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-724">10 iterations are run untimed before taking the sum of 1000 timed iterations.</span></span> <span data-ttu-id="22b0f-725">Gösterilen sonuçlar, her testin 5 çalıştırmasından alınan ortanca çalışmadır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-725">The results shown are the median run taken from 5 runs of each test.</span></span> <span data-ttu-id="22b0f-726">Daha fazla bilgi için bkz. Ek B, test kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-726">For more information, see Appendix B which includes the code for the test.</span></span>

| <span data-ttu-id="22b0f-727">AŞV</span><span class="sxs-lookup"><span data-stu-id="22b0f-727">EF</span></span>  | <span data-ttu-id="22b0f-728">Test etme</span><span class="sxs-lookup"><span data-stu-id="22b0f-728">Test</span></span>                                        | <span data-ttu-id="22b0f-729">Zaman (MS)</span><span class="sxs-lookup"><span data-stu-id="22b0f-729">Time (ms)</span></span> | <span data-ttu-id="22b0f-730">Bellek</span><span class="sxs-lookup"><span data-stu-id="22b0f-730">Memory</span></span>   |
|:----|:--------------------------------------------|:----------|:---------|
| <span data-ttu-id="22b0f-731">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-731">EF5</span></span> | <span data-ttu-id="22b0f-732">ObjectContext varlık komutu</span><span class="sxs-lookup"><span data-stu-id="22b0f-732">ObjectContext Entity Command</span></span>                | <span data-ttu-id="22b0f-733">621</span><span class="sxs-lookup"><span data-stu-id="22b0f-733">621</span></span>       | <span data-ttu-id="22b0f-734">39350272</span><span class="sxs-lookup"><span data-stu-id="22b0f-734">39350272</span></span> |
| <span data-ttu-id="22b0f-735">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-735">EF5</span></span> | <span data-ttu-id="22b0f-736">Veritabanı üzerinde DbContext SQL sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-736">DbContext Sql Query on Database</span></span>             | <span data-ttu-id="22b0f-737">825</span><span class="sxs-lookup"><span data-stu-id="22b0f-737">825</span></span>       | <span data-ttu-id="22b0f-738">37519360</span><span class="sxs-lookup"><span data-stu-id="22b0f-738">37519360</span></span> |
| <span data-ttu-id="22b0f-739">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-739">EF5</span></span> | <span data-ttu-id="22b0f-740">ObjectContext mağaza sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-740">ObjectContext Store Query</span></span>                   | <span data-ttu-id="22b0f-741">878</span><span class="sxs-lookup"><span data-stu-id="22b0f-741">878</span></span>       | <span data-ttu-id="22b0f-742">39460864</span><span class="sxs-lookup"><span data-stu-id="22b0f-742">39460864</span></span> |
| <span data-ttu-id="22b0f-743">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-743">EF5</span></span> | <span data-ttu-id="22b0f-744">ObjectContext LINQ sorgusu Izleme yok</span><span class="sxs-lookup"><span data-stu-id="22b0f-744">ObjectContext Linq Query No Tracking</span></span>        | <span data-ttu-id="22b0f-745">969</span><span class="sxs-lookup"><span data-stu-id="22b0f-745">969</span></span>       | <span data-ttu-id="22b0f-746">38293504</span><span class="sxs-lookup"><span data-stu-id="22b0f-746">38293504</span></span> |
| <span data-ttu-id="22b0f-747">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-747">EF5</span></span> | <span data-ttu-id="22b0f-748">Nesne sorgusu kullanarak ObjectContext varlık SQL</span><span class="sxs-lookup"><span data-stu-id="22b0f-748">ObjectContext Entity Sql using Object Query</span></span> | <span data-ttu-id="22b0f-749">1089</span><span class="sxs-lookup"><span data-stu-id="22b0f-749">1089</span></span>      | <span data-ttu-id="22b0f-750">38981632</span><span class="sxs-lookup"><span data-stu-id="22b0f-750">38981632</span></span> |
| <span data-ttu-id="22b0f-751">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-751">EF5</span></span> | <span data-ttu-id="22b0f-752">ObjectContext derlenmiş sorgu</span><span class="sxs-lookup"><span data-stu-id="22b0f-752">ObjectContext Compiled Query</span></span>                | <span data-ttu-id="22b0f-753">1099</span><span class="sxs-lookup"><span data-stu-id="22b0f-753">1099</span></span>      | <span data-ttu-id="22b0f-754">38682624</span><span class="sxs-lookup"><span data-stu-id="22b0f-754">38682624</span></span> |
| <span data-ttu-id="22b0f-755">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-755">EF5</span></span> | <span data-ttu-id="22b0f-756">ObjectContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-756">ObjectContext Linq Query</span></span>                    | <span data-ttu-id="22b0f-757">1152</span><span class="sxs-lookup"><span data-stu-id="22b0f-757">1152</span></span>      | <span data-ttu-id="22b0f-758">38178816</span><span class="sxs-lookup"><span data-stu-id="22b0f-758">38178816</span></span> |
| <span data-ttu-id="22b0f-759">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-759">EF5</span></span> | <span data-ttu-id="22b0f-760">DbContext LINQ sorgusu Izleme yok</span><span class="sxs-lookup"><span data-stu-id="22b0f-760">DbContext Linq Query No Tracking</span></span>            | <span data-ttu-id="22b0f-761">1208</span><span class="sxs-lookup"><span data-stu-id="22b0f-761">1208</span></span>      | <span data-ttu-id="22b0f-762">41803776</span><span class="sxs-lookup"><span data-stu-id="22b0f-762">41803776</span></span> |
| <span data-ttu-id="22b0f-763">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-763">EF5</span></span> | <span data-ttu-id="22b0f-764">DbSet üzerinde DbContext SQL sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-764">DbContext Sql Query on DbSet</span></span>                | <span data-ttu-id="22b0f-765">1414</span><span class="sxs-lookup"><span data-stu-id="22b0f-765">1414</span></span>      | <span data-ttu-id="22b0f-766">37982208</span><span class="sxs-lookup"><span data-stu-id="22b0f-766">37982208</span></span> |
| <span data-ttu-id="22b0f-767">EF5</span><span class="sxs-lookup"><span data-stu-id="22b0f-767">EF5</span></span> | <span data-ttu-id="22b0f-768">DbContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-768">DbContext Linq Query</span></span>                        | <span data-ttu-id="22b0f-769">1574</span><span class="sxs-lookup"><span data-stu-id="22b0f-769">1574</span></span>      | <span data-ttu-id="22b0f-770">41738240</span><span class="sxs-lookup"><span data-stu-id="22b0f-770">41738240</span></span> |
|     |                                             |           |          |
| <span data-ttu-id="22b0f-771">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-771">EF6</span></span> | <span data-ttu-id="22b0f-772">ObjectContext varlık komutu</span><span class="sxs-lookup"><span data-stu-id="22b0f-772">ObjectContext Entity Command</span></span>                | <span data-ttu-id="22b0f-773">480</span><span class="sxs-lookup"><span data-stu-id="22b0f-773">480</span></span>       | <span data-ttu-id="22b0f-774">47247360</span><span class="sxs-lookup"><span data-stu-id="22b0f-774">47247360</span></span> |
| <span data-ttu-id="22b0f-775">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-775">EF6</span></span> | <span data-ttu-id="22b0f-776">ObjectContext mağaza sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-776">ObjectContext Store Query</span></span>                   | <span data-ttu-id="22b0f-777">493</span><span class="sxs-lookup"><span data-stu-id="22b0f-777">493</span></span>       | <span data-ttu-id="22b0f-778">46739456</span><span class="sxs-lookup"><span data-stu-id="22b0f-778">46739456</span></span> |
| <span data-ttu-id="22b0f-779">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-779">EF6</span></span> | <span data-ttu-id="22b0f-780">Veritabanı üzerinde DbContext SQL sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-780">DbContext Sql Query on Database</span></span>             | <span data-ttu-id="22b0f-781">614</span><span class="sxs-lookup"><span data-stu-id="22b0f-781">614</span></span>       | <span data-ttu-id="22b0f-782">41607168</span><span class="sxs-lookup"><span data-stu-id="22b0f-782">41607168</span></span> |
| <span data-ttu-id="22b0f-783">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-783">EF6</span></span> | <span data-ttu-id="22b0f-784">ObjectContext LINQ sorgusu Izleme yok</span><span class="sxs-lookup"><span data-stu-id="22b0f-784">ObjectContext Linq Query No Tracking</span></span>        | <span data-ttu-id="22b0f-785">684</span><span class="sxs-lookup"><span data-stu-id="22b0f-785">684</span></span>       | <span data-ttu-id="22b0f-786">46333952</span><span class="sxs-lookup"><span data-stu-id="22b0f-786">46333952</span></span> |
| <span data-ttu-id="22b0f-787">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-787">EF6</span></span> | <span data-ttu-id="22b0f-788">Nesne sorgusu kullanarak ObjectContext varlık SQL</span><span class="sxs-lookup"><span data-stu-id="22b0f-788">ObjectContext Entity Sql using Object Query</span></span> | <span data-ttu-id="22b0f-789">767</span><span class="sxs-lookup"><span data-stu-id="22b0f-789">767</span></span>       | <span data-ttu-id="22b0f-790">48865280</span><span class="sxs-lookup"><span data-stu-id="22b0f-790">48865280</span></span> |
| <span data-ttu-id="22b0f-791">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-791">EF6</span></span> | <span data-ttu-id="22b0f-792">ObjectContext derlenmiş sorgu</span><span class="sxs-lookup"><span data-stu-id="22b0f-792">ObjectContext Compiled Query</span></span>                | <span data-ttu-id="22b0f-793">788</span><span class="sxs-lookup"><span data-stu-id="22b0f-793">788</span></span>       | <span data-ttu-id="22b0f-794">48467968</span><span class="sxs-lookup"><span data-stu-id="22b0f-794">48467968</span></span> |
| <span data-ttu-id="22b0f-795">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-795">EF6</span></span> | <span data-ttu-id="22b0f-796">DbContext LINQ sorgusu Izleme yok</span><span class="sxs-lookup"><span data-stu-id="22b0f-796">DbContext Linq Query No Tracking</span></span>            | <span data-ttu-id="22b0f-797">878</span><span class="sxs-lookup"><span data-stu-id="22b0f-797">878</span></span>       | <span data-ttu-id="22b0f-798">47554560</span><span class="sxs-lookup"><span data-stu-id="22b0f-798">47554560</span></span> |
| <span data-ttu-id="22b0f-799">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-799">EF6</span></span> | <span data-ttu-id="22b0f-800">ObjectContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-800">ObjectContext Linq Query</span></span>                    | <span data-ttu-id="22b0f-801">953</span><span class="sxs-lookup"><span data-stu-id="22b0f-801">953</span></span>       | <span data-ttu-id="22b0f-802">47632384</span><span class="sxs-lookup"><span data-stu-id="22b0f-802">47632384</span></span> |
| <span data-ttu-id="22b0f-803">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-803">EF6</span></span> | <span data-ttu-id="22b0f-804">DbSet üzerinde DbContext SQL sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-804">DbContext Sql Query on DbSet</span></span>                | <span data-ttu-id="22b0f-805">1023</span><span class="sxs-lookup"><span data-stu-id="22b0f-805">1023</span></span>      | <span data-ttu-id="22b0f-806">41992192</span><span class="sxs-lookup"><span data-stu-id="22b0f-806">41992192</span></span> |
| <span data-ttu-id="22b0f-807">EF6</span><span class="sxs-lookup"><span data-stu-id="22b0f-807">EF6</span></span> | <span data-ttu-id="22b0f-808">DbContext LINQ sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-808">DbContext Linq Query</span></span>                        | <span data-ttu-id="22b0f-809">1290</span><span class="sxs-lookup"><span data-stu-id="22b0f-809">1290</span></span>      | <span data-ttu-id="22b0f-810">47529984</span><span class="sxs-lookup"><span data-stu-id="22b0f-810">47529984</span></span> |


![EF5 sıcak sorgu 1000 yinelemeleri](~/ef6/media/ef5warmquery1000.png)

![EF6 sıcak sorgu 1000 yinelemeleri](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> <span data-ttu-id="22b0f-813">Bu, bir EntityCommand üzerinde Entity SQL bir sorgu yürütüyoruz bir varyasyon ekledik.</span><span class="sxs-lookup"><span data-stu-id="22b0f-813">For completeness, we included a variation where we execute an Entity SQL query on an EntityCommand.</span></span> <span data-ttu-id="22b0f-814">Ancak, bu tür sorgular için sonuçlar gerçekleştirilmediğinden, karşılaştırma, her zaman, her ne kadar elmalar olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="22b0f-814">However, because results are not materialized for such queries, the comparison isn't necessarily apples-to-apples.</span></span> <span data-ttu-id="22b0f-815">Test, karşılaştırma fairer yapmayı denemek için bir kapanış tahmini içerir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-815">The test includes a close approximation to materializing to try making the comparison fairer.</span></span>

<span data-ttu-id="22b0f-816">Bu uçtan uca bu durumda Entity Framework 6 out, çok daha hafif bir DbContext başlatması ve daha hızlı MetadataCollection&lt;T&gt; aramaları dahil olmak üzere yığının çeşitli bölümlerinde yapılan performans geliştirmelerinden dolayı 5 ' i Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="22b0f-816">In this end-to-end case, Entity Framework 6 outperforms Entity Framework 5 due to performance improvements made on several parts of the stack, including a much lighter DbContext initialization and faster MetadataCollection&lt;T&gt; lookups.</span></span>

## <a name="7-design-time-performance-considerations"></a><span data-ttu-id="22b0f-817">7 tasarım zamanı performans konuları</span><span class="sxs-lookup"><span data-stu-id="22b0f-817">7 Design time performance considerations</span></span>

### <a name="71-inheritance-strategies"></a><span data-ttu-id="22b0f-818">7,1 devralma stratejileri</span><span class="sxs-lookup"><span data-stu-id="22b0f-818">7.1       Inheritance Strategies</span></span>

<span data-ttu-id="22b0f-819">Entity Framework kullanırken başka bir performans değerlendirmesi kullandığınız devralma stratejisidir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-819">Another performance consideration when using Entity Framework is the inheritance strategy you use.</span></span> <span data-ttu-id="22b0f-820">Entity Framework, 3 temel devralma türünü ve bunların birleşimlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="22b0f-820">Entity Framework supports 3 basic types of inheritance and their combinations:</span></span>

-   <span data-ttu-id="22b0f-821">Hiyerarşi başına tablo (TPH) – her devralma kümesi, hiyerarşide hangi tür türün temsil edileceğini belirtmek için bir Ayrıştırıcı sütunu olan bir tabloyla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-821">Table per Hierarchy (TPH) – where each inheritance set maps to a table with a discriminator column to indicate which particular type in the hierarchy is being represented in the row.</span></span>
-   <span data-ttu-id="22b0f-822">Tür başına tablo (TPT) – her türün veritabanında kendi tablosu vardır; alt tablolar yalnızca üst tablonun içermediği sütunları tanımlar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-822">Table per Type (TPT) – where each type has its own table in the database; the child tables only define the columns that the parent table doesn’t contain.</span></span>
-   <span data-ttu-id="22b0f-823">Sınıf başına tablo (TPC) – her türün veritabanında kendine ait tam tablosu vardır; alt tablolar, üst türlerde tanımlananlar da dahil olmak üzere tüm alanlarını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-823">Table per Class (TPC) – where each type has its own full table in the database; the child tables define all their fields, including those defined in parent types.</span></span>

<span data-ttu-id="22b0f-824">Modeliniz TPT devralmayı kullanıyorsa, oluşturulan sorgular diğer devralma stratejileriyle oluşturulanlardan daha karmaşık olacaktır, bu da depodaki yürütme sürelerinin uzun süreleriyle sonuçlanabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-824">If your model uses TPT inheritance, the queries which are generated will be more complex than those that are generated with the other inheritance strategies, which may result on longer execution times on the store.</span></span><span data-ttu-id="22b0f-825">  Genellikle bir TPT modeli üzerinde sorgu oluşturmak ve sonuçta elde edilen nesneleri faturalandırmak daha uzun sürer.</span><span class="sxs-lookup"><span data-stu-id="22b0f-825">  It will generally take longer to generate queries over a TPT model, and to materialize the resulting objects.</span></span>

<span data-ttu-id="22b0f-826">"MSDN blog gönderisi: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>Entity Framework, TPT (tür başına tablo) devralma ile Ilgili performans konuları bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-826">See the "Performance Considerations when using TPT (Table per Type) Inheritance in the Entity Framework" MSDN blog post: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.</span></span>

#### <a name="711-avoiding-tpt-in-model-first-or-code-first-applications"></a><span data-ttu-id="22b0f-827">Model First veya Code First uygulamalarında TPT 7.1.1 önleme</span><span class="sxs-lookup"><span data-stu-id="22b0f-827">7.1.1       Avoiding TPT in Model First or Code First applications</span></span>

<span data-ttu-id="22b0f-828">Bir TPT şemasına sahip var olan bir veritabanı üzerinde bir model oluşturduğunuzda çok sayıda seçeneğiniz yoktur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-828">When you create a model over an existing database that has a TPT schema, you don't have many options.</span></span> <span data-ttu-id="22b0f-829">Ancak Model First veya Code First kullanarak bir uygulama oluştururken performans sorunları için TPT devralmasından kaçının.</span><span class="sxs-lookup"><span data-stu-id="22b0f-829">But when creating an application using Model First or Code First, you should avoid TPT inheritance for performance concerns.</span></span>

<span data-ttu-id="22b0f-830">Entity Desisgner sihirbazında Model First kullandığınızda, modelinizde devralma için TPT alırsınız.</span><span class="sxs-lookup"><span data-stu-id="22b0f-830">When you use Model First in the Entity Designer Wizard, you will get TPT for any inheritance in your model.</span></span> <span data-ttu-id="22b0f-831">Model First ile bir TPH devralma stratejisine geçiş yapmak istiyorsanız, Visual Studio galerisindeki (\<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>)bulunan "Entity Desisgner veritabanı oluşturma Power Pack" kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-831">If you want to switch to a TPH inheritance strategy with Model First, you can use the "Entity Designer Database Generation Power Pack" available from the Visual Studio Gallery ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).</span></span>

<span data-ttu-id="22b0f-832">Bir modelin devralma ile eşlemesini yapılandırmak için Code First kullanırken EF, varsayılan olarak TPH kullanır, bu nedenle devralma hiyerarşisindeki tüm varlıklar aynı tabloyla eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-832">When using Code First to configure the mapping of a model with inheritance, EF will use TPH by default, therefore all entities in the inheritance hierarchy will be mapped to the same table.</span></span> <span data-ttu-id="22b0f-833">Daha fazla bilgi için MSDN [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)Magazine 'teki "Code First Entity Framework 4.1" makalesindeki "akıcı API ile eşleme" bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-833">See the "Mapping with the Fluent API" section of the "Code First in Entity Framework4.1" article in MSDN Magazine ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) for more details.</span></span>

### <a name="72-upgrading-from-ef4-to-improve-model-generation-time"></a><span data-ttu-id="22b0f-834">7,2 model oluşturma süresini geliştirmek için EF4 'ten yükseltme</span><span class="sxs-lookup"><span data-stu-id="22b0f-834">7.2       Upgrading from EF4 to improve model generation time</span></span>

<span data-ttu-id="22b0f-835">Modelin mağaza katmanını (SSDL) oluşturan algoritmaya yönelik SQL Server özgü bir geliştirme Entity Framework 5 ve 6 ' da ve Visual Studio 2010 SP1 yüklendiğinde Entity Framework 4 ' e bir güncelleştirme olarak sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-835">A SQL Server-specific improvement to the algorithm that generates the store-layer (SSDL) of the model is available in Entity Framework 5 and 6, and as an update to Entity Framework 4 when Visual Studio 2010 SP1 is installed.</span></span> <span data-ttu-id="22b0f-836">Aşağıdaki test sonuçları, çok büyük bir model oluştururken, bu örnekte Navision modelinde geliştirme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-836">The following test results demonstrate the improvement when generating a very big model, in this case the Navision model.</span></span> <span data-ttu-id="22b0f-837">Daha fazla ayrıntı için bkz. Ek C.</span><span class="sxs-lookup"><span data-stu-id="22b0f-837">See Appendix C for more details about it.</span></span>

<span data-ttu-id="22b0f-838">Model, 1005 varlık kümesi ve 4227 ilişkilendirme kümesi içerir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-838">The model contains 1005 entity sets and 4227 association sets.</span></span>

| <span data-ttu-id="22b0f-839">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="22b0f-839">Configuration</span></span>                              | <span data-ttu-id="22b0f-840">Tüketilen sürenin dökümü</span><span class="sxs-lookup"><span data-stu-id="22b0f-840">Breakdown of time consumed</span></span>                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="22b0f-841">Visual Studio 2010, Entity Framework 4</span><span class="sxs-lookup"><span data-stu-id="22b0f-841">Visual Studio 2010, Entity Framework 4</span></span>     | <span data-ttu-id="22b0f-842">SSDL oluşturma: 2 sa 27 dk</span><span class="sxs-lookup"><span data-stu-id="22b0f-842">SSDL Generation: 2 hr 27 min</span></span> <br/> <span data-ttu-id="22b0f-843">Eşleme oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-843">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-844">CSDL oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-844">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-845">ObjectLayer oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-845">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-846">Üretimi görüntüle: 2 h 14 dk</span><span class="sxs-lookup"><span data-stu-id="22b0f-846">View Generation: 2 h 14 min</span></span> |
| <span data-ttu-id="22b0f-847">Visual Studio 2010 SP1, Entity Framework 4</span><span class="sxs-lookup"><span data-stu-id="22b0f-847">Visual Studio 2010 SP1, Entity Framework 4</span></span> | <span data-ttu-id="22b0f-848">SSDL oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-848">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-849">Eşleme oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-849">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-850">CSDL oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-850">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-851">ObjectLayer oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-851">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-852">Üretimi görüntüle: 1 sa 53 dk</span><span class="sxs-lookup"><span data-stu-id="22b0f-852">View Generation: 1 hr 53 min</span></span>   |
| <span data-ttu-id="22b0f-853">Visual Studio 2013, Entity Framework 5</span><span class="sxs-lookup"><span data-stu-id="22b0f-853">Visual Studio 2013, Entity Framework 5</span></span>     | <span data-ttu-id="22b0f-854">SSDL oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-854">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-855">Eşleme oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-855">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-856">CSDL oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-856">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-857">ObjectLayer oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-857">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-858">Üretimi görüntüle: 65 dakika</span><span class="sxs-lookup"><span data-stu-id="22b0f-858">View Generation: 65 minutes</span></span>    |
| <span data-ttu-id="22b0f-859">Visual Studio 2013, Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="22b0f-859">Visual Studio 2013, Entity Framework 6</span></span>     | <span data-ttu-id="22b0f-860">SSDL oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-860">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-861">Eşleme oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-861">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-862">CSDL oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-862">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-863">ObjectLayer oluşturma: 1 saniye</span><span class="sxs-lookup"><span data-stu-id="22b0f-863">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="22b0f-864">Üretimi görüntüle: 28 saniye.</span><span class="sxs-lookup"><span data-stu-id="22b0f-864">View Generation: 28 seconds.</span></span>   |


<span data-ttu-id="22b0f-865">SSDL 'ı oluştururken yükün neredeyse tamamen SQL Server, istemci geliştirme makinesi sonuçların sunucudan geri gelmesi için boşta beklediği sırada yükün neredeyse tamamen harcandığını belirten bir değer.</span><span class="sxs-lookup"><span data-stu-id="22b0f-865">It's worth noting that when generating the SSDL, the load is almost entirely spent on the SQL Server, while the client development machine is waiting idle for results to come back from the server.</span></span> <span data-ttu-id="22b0f-866">DBAs özellikle bu geliştirmeyi bilmelidir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-866">DBAs should particularly appreciate this improvement.</span></span> <span data-ttu-id="22b0f-867">Ayrıca, tüm model oluşturma maliyetinin artık görünüm oluşturma bölümünde meydana gelir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-867">It's also worth noting that essentially the entire cost of model generation takes place in View Generation now.</span></span>

### <a name="73-splitting-large-models-with-database-first-and-model-first"></a><span data-ttu-id="22b0f-868">7,3 Database First ve Model First büyük modelleri bölme</span><span class="sxs-lookup"><span data-stu-id="22b0f-868">7.3       Splitting Large Models with Database First and Model First</span></span>

<span data-ttu-id="22b0f-869">Model boyutu arttıkça tasarımcı yüzeyi de karışık hale gelir ve kullanılması zordur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-869">As model size increases, the designer surface becomes cluttered and difficult to use.</span></span> <span data-ttu-id="22b0f-870">Genellikle, tasarımcı 'nın etkin bir şekilde kullanılması için 300 ' den fazla varlık içeren bir model düşünün.</span><span class="sxs-lookup"><span data-stu-id="22b0f-870">We typically consider a model with more than 300 entities to be too large to effectively use the designer.</span></span> <span data-ttu-id="22b0f-871">Aşağıdaki blog gönderisine büyük modelleri bölmek için çeşitli seçenekler açıklanmaktadır: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.</span><span class="sxs-lookup"><span data-stu-id="22b0f-871">The following blog post describes several options for splitting large models: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.</span></span>

<span data-ttu-id="22b0f-872">Gönderi Entity Framework ilk sürümü için yazıldı, ancak adımlar hala geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-872">The post was written for the first version of Entity Framework, but the steps still apply.</span></span>

### <a name="74-performance-considerations-with-the-entity-data-source-control"></a><span data-ttu-id="22b0f-873">Varlık veri kaynağı denetimiyle ilgili 7,4 performans konuları</span><span class="sxs-lookup"><span data-stu-id="22b0f-873">7.4       Performance considerations with the Entity Data Source Control</span></span>

<span data-ttu-id="22b0f-874">EntityDataSource denetimini kullanan bir Web uygulamasının performansının önemli ölçüde azalyacağı, çok iş parçacıklı performans ve stres testlerinde bu durumları gördük.</span><span class="sxs-lookup"><span data-stu-id="22b0f-874">We've seen cases in multi-threaded performance and stress tests where the performance of a web application using the EntityDataSource Control deteriorates significantly.</span></span> <span data-ttu-id="22b0f-875">Temeldeki neden, EntityDataSource 'un varlık olarak kullanılacak türleri bulması için Web uygulaması tarafından başvurulan derlemelerde MetadataWorkspace. LoadFromAssembly ' i tekrar tekrar çağırıyor olması olabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-875">The underlying cause is that the EntityDataSource repeatedly calls MetadataWorkspace.LoadFromAssembly on the assemblies referenced by the Web application to discover the types to be used as entities.</span></span>

<span data-ttu-id="22b0f-876">Çözüm, EntityDataSource 'un ContextTypeName öğesini türetilmiş ObjectContext sınıfınızın tür adıyla ayarlamaya yönelik olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-876">The solution is to set the ContextTypeName of the EntityDataSource to the type name of your derived ObjectContext class.</span></span> <span data-ttu-id="22b0f-877">Bu, varlık türleri için başvurulan tüm derlemeleri tarayan mekanizmayı kapatır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-877">This turns off the mechanism that scans all referenced assemblies for entity types.</span></span>

<span data-ttu-id="22b0f-878">ContextTypeName alanının ayarlanması ayrıca, .NET 4,0 ' deki EntityDataSource 'un yansıma aracılığıyla bir derlemeden bir tür yükleyebildiği bir ReflectionTypeLoadException oluşturduğu işlevsel bir sorunu önler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-878">Setting the ContextTypeName field also prevents a functional problem where the EntityDataSource in .NET 4.0 throws a ReflectionTypeLoadException when it can't load a type from an assembly via reflection.</span></span> <span data-ttu-id="22b0f-879">Bu sorun .NET 4,5 ' de düzeltilmiştir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-879">This issue has been fixed in .NET 4.5.</span></span>

### <a name="75-poco-entities-and-change-tracking-proxies"></a><span data-ttu-id="22b0f-880">7,5 POCO varlıkları ve değişiklik izleme proxy 'leri</span><span class="sxs-lookup"><span data-stu-id="22b0f-880">7.5       POCO entities and change tracking proxies</span></span>

<span data-ttu-id="22b0f-881">Entity Framework, veri sınıflarında herhangi bir değişiklik yapmadan veri modelinizle birlikte özel veri sınıflarını kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-881">Entity Framework enables you to use custom data classes together with your data model without making any modifications to the data classes themselves.</span></span> <span data-ttu-id="22b0f-882">Bu, mevcut etki alanı nesneleri gibi "düz eski" CLR nesnelerini (POCO) veri modelinizle kullanabileceğiniz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-882">This means that you can use "plain-old" CLR objects (POCO), such as existing domain objects, with your data model.</span></span> <span data-ttu-id="22b0f-883">Bu POCO veri sınıfları (Kalıcılık-Ignorant nesneleri olarak da bilinir), aynı sorgunun çoğunu destekler, Varlık Veri Modeli araçları tarafından oluşturulan varlık türleri olarak davranışları ekleyin, güncelleştirin ve silin.</span><span class="sxs-lookup"><span data-stu-id="22b0f-883">These POCO data classes (also known as persistence-ignorant objects), which are mapped to entities that are defined in a data model, support most of the same query, insert, update, and delete behaviors as entity types that are generated by the Entity Data Model tools.</span></span>

<span data-ttu-id="22b0f-884">Entity Framework, Poco varlıklarınızda elde edilen yavaş yükleme ve otomatik değişiklik izleme gibi özellikleri etkinleştirmek istediğinizde kullanılan, POCO türlerinizi türetilmiş proxy sınıfları da oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-884">Entity Framework can also create proxy classes derived from your POCO types, which are used when you want to enable features such as lazy loading and automatic change tracking on POCO entities.</span></span> <span data-ttu-id="22b0f-885">POCO sınıflarınız, aşağıda açıklandığı gibi Entity Framework proxy 'leri kullanmasına izin vermek için bazı gereksinimleri karşılamalıdır: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).</span><span class="sxs-lookup"><span data-stu-id="22b0f-885">Your POCO classes must meet certain requirements to allow Entity Framework to use proxies, as described here: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).</span></span>

<span data-ttu-id="22b0f-886">Varlık izleme proxy 'leri, varlıklarınızın özelliklerinden herhangi birinin değeri değiştiği her seferinde nesne durumu yöneticisini bilgilendirir, bu nedenle varlıklarınızın gerçek durumunu her zaman bilir Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="22b0f-886">Chance tracking proxies will notify the object state manager each time any of the properties of your entities has its value changed, so Entity Framework knows the actual state of your entities all the time.</span></span> <span data-ttu-id="22b0f-887">Bu işlem, özelliklerinizi ayarlarınızın ayarlayıcı yöntemlerinin gövdesine ekleyerek ve nesne durumu yöneticisinin bu gibi olayları işlemesini sağlamak için yapılır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-887">This is done by adding notification events to the body of the setter methods of your properties, and having the object state manager processing such events.</span></span> <span data-ttu-id="22b0f-888">Bir proxy varlık oluşturmanın genellikle Entity Framework tarafından oluşturulan eklenen olay kümesi nedeniyle proxy olmayan bir POCO varlığı oluşturmaktan daha pahalı olacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-888">Note that creating a proxy entity will typically be more expensive than creating a non-proxy POCO entity due to the added set of events created by Entity Framework.</span></span>

<span data-ttu-id="22b0f-889">Bir POCO varlığının değişiklik izleme proxy 'si olmadığında, varlıklarınızın içerikleri önceki kaydedilmiş bir kopyanın bir kopyasına göre karşılaştırılırken değişiklikler bulunur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-889">When a POCO entity does not have a change tracking proxy, changes are found by comparing the contents of your entities against a copy of a previous saved state.</span></span> <span data-ttu-id="22b0f-890">Bu derin karşılaştırma, ortamınızda çok fazla varlık olduğunda veya varlıklarınızda son karşılaştırma gerçekleşmesinden bu yana değiştirilmese bile çok büyük miktarda özelliğe sahip olduğunda uzun bir işlem olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-890">This deep comparison will become a lengthy process when you have many entities in your context, or when your entities have a very large amount of properties, even if none of them changed since the last comparison took place.</span></span>

<span data-ttu-id="22b0f-891">Özet: değişiklik izleme proxy 'si oluştururken bir performans isabeti ödetireceksiniz, ancak değişiklik izleme, varlıklarınızda birçok özellik olduğunda veya modelinizde birçok varlık olduğunda değişiklik algılama sürecini hızlandırmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-891">In summary: you’ll pay a performance hit when creating the change tracking proxy, but change tracking will help you speed up the change detection process when your entities have many properties or when you have many entities in your model.</span></span> <span data-ttu-id="22b0f-892">Varlık miktarının çok fazla büyümeyeceği az sayıda özelliği olan varlıklar için değişiklik izleme proxy 'lerinin olması çok avantajlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-892">For entities with a small number of properties where the amount of entities doesn’t grow too much, having change tracking proxies may not be of much benefit.</span></span>

## <a name="8-loading-related-entities"></a><span data-ttu-id="22b0f-893">8 Ilgili varlıkları yükleme</span><span class="sxs-lookup"><span data-stu-id="22b0f-893">8 Loading Related Entities</span></span>

### <a name="81-lazy-loading-vs-eager-loading"></a><span data-ttu-id="22b0f-894">8,1 yavaş yükleme vs. Eager yükleniyor</span><span class="sxs-lookup"><span data-stu-id="22b0f-894">8.1 Lazy Loading vs. Eager Loading</span></span>

<span data-ttu-id="22b0f-895">Entity Framework, hedef varlığınıza ilişkin varlıkları yüklemek için birkaç farklı yol sunar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-895">Entity Framework offers several different ways to load the entities that are related to your target entity.</span></span> <span data-ttu-id="22b0f-896">Örneğin, ürünler için sorgulama yaptığınızda ilgili siparişlerin nesne durumu yöneticisine yüklenebilmenin farklı yolları vardır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-896">For example, when you query for Products, there are different ways that the related Orders will be loaded into the Object State Manager.</span></span> <span data-ttu-id="22b0f-897">Bir performans açısından, ilgili varlıkların yüklenmesi sırasında göz önünde bulundurmanız gereken en büyük soru, geç yükleme veya Eager yükleme kullanılıp kullanılmayacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-897">From a performance standpoint, the biggest question to consider when loading related entities will be whether to use Lazy Loading or Eager Loading.</span></span>

<span data-ttu-id="22b0f-898">Eager yüklemesi kullanılırken, ilgili varlıklar hedef varlık kümesiyle birlikte yüklenir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-898">When using Eager Loading, the related entities are loaded along with your target entity set.</span></span> <span data-ttu-id="22b0f-899">Hangi ilgili varlıkların getirmek istediğinizi belirtmek için sorgunuzda bir Include ifadesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-899">You use an Include statement in your query to indicate which related entities you want to bring in.</span></span>

<span data-ttu-id="22b0f-900">Geç yükleme kullanılırken, ilk sorgunuz yalnızca hedef varlık kümesine getirir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-900">When using Lazy Loading, your initial query only brings in the target entity set.</span></span> <span data-ttu-id="22b0f-901">Ancak bir gezinti özelliğine her eriştiğinizde, ilgili varlığı yüklemek için mağazaya karşı başka bir sorgu verilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-901">But whenever you access a navigation property, another query is issued against the store to load the related entity.</span></span>

<span data-ttu-id="22b0f-902">Bir varlık yüklendikten sonra, yavaş yükleme veya Eager yükleme kullandığınızda varlık için başka sorgular doğrudan nesne durumu yöneticisinden yüklenir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-902">Once an entity has been loaded, any further queries for the entity will load it directly from the Object State Manager, whether you are using lazy loading or eager loading.</span></span>

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a><span data-ttu-id="22b0f-903">8,2 yavaş yükleme ve Eager yükleme arasında seçim yapma</span><span class="sxs-lookup"><span data-stu-id="22b0f-903">8.2 How to choose between Lazy Loading and Eager Loading</span></span>

<span data-ttu-id="22b0f-904">Önemli olan şey, uygulamanız için doğru seçimi yapabilmeniz için yavaş yükleme ve Eager yükleme arasındaki farkı anlamanızdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-904">The important thing is that you understand the difference between Lazy Loading and Eager Loading so that you can make the correct choice for your application.</span></span> <span data-ttu-id="22b0f-905">Bu, veritabanına yönelik birden çok istek ile büyük bir yük içerebilen tek bir istek arasındaki zorunluluğunu getirir değerlendirmenize yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-905">This will help you evaluate the tradeoff between multiple requests against the database versus a single request that may contain a large payload.</span></span> <span data-ttu-id="22b0f-906">Uygulamanızın bazı bölümlerinde ve başka bölümlere geç yüklemeye yönelik olarak yükleme kullanmak uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-906">It may be appropriate to use eager loading in some parts of your application and lazy loading in other parts.</span></span>

<span data-ttu-id="22b0f-907">Devlet kapsamında neler olduğuna ilişkin bir örnek olarak, UK 'de yaşayan müşterileri ve bunların sıra sayısını sorgulamak istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="22b0f-907">As an example of what's happening under the hood, suppose you want to query for the customers who live in the UK and their order count.</span></span>

<span data-ttu-id="22b0f-908">**Eager yükleme kullanma**</span><span class="sxs-lookup"><span data-stu-id="22b0f-908">**Using Eager Loading**</span></span>

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

<span data-ttu-id="22b0f-909">**Geç yükleme kullanma**</span><span class="sxs-lookup"><span data-stu-id="22b0f-909">**Using Lazy Loading**</span></span>

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

<span data-ttu-id="22b0f-910">Eager yükleme kullanılırken, tüm müşterileri ve tüm siparişleri döndüren tek bir sorgu oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="22b0f-910">When using eager loading, you'll issue a single query that returns all customers and all orders.</span></span> <span data-ttu-id="22b0f-911">Mağaza komutu şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="22b0f-911">The store command looks like:</span></span>

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

<span data-ttu-id="22b0f-912">Yavaş yükleme kullanırken başlangıçta aşağıdaki sorguyu verirsiniz:</span><span class="sxs-lookup"><span data-stu-id="22b0f-912">When using lazy loading, you'll issue the following query initially:</span></span>

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

<span data-ttu-id="22b0f-913">Aynı zamanda bir müşterinin Orders gezinti özelliğine her eriştiğinizde, mağazaya karşı aşağıdakiler gibi başka bir sorgu da verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="22b0f-913">And each time you access the Orders navigation property of a customer another query like the following is issued against the store:</span></span>

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

<span data-ttu-id="22b0f-914">Daha fazla bilgi için [Ilgili nesneleri yükleme](https://msdn.microsoft.com/library/bb896272.aspx)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-914">For more information, see the [Loading Related Objects](https://msdn.microsoft.com/library/bb896272.aspx).</span></span>

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a><span data-ttu-id="22b0f-915">8.2.1 geç yükleme, bir ekip yükleme sayfası</span><span class="sxs-lookup"><span data-stu-id="22b0f-915">8.2.1 Lazy Loading versus Eager Loading cheat sheet</span></span>

<span data-ttu-id="22b0f-916">Bu tür bir şey, bir veya daha fazla uygulamanın, yavaş yüklemeye karşı karşıya yüklemeyi tercih eden bir şeydir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-916">There’s no such thing as a one-size-fits-all to choosing eager loading versus lazy loading.</span></span> <span data-ttu-id="22b0f-917">İyi bilinçli bir karar vermek için, her iki strateji arasındaki farkları anlamak için deneyin; Ayrıca, kodunuzun aşağıdaki senaryolardan birine uygun olup olmadığını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="22b0f-917">Try first to understand the differences between both strategies so you can do a well informed decision; also, consider if your code fits to any of the following scenarios:</span></span>

| <span data-ttu-id="22b0f-918">Senaryo</span><span class="sxs-lookup"><span data-stu-id="22b0f-918">Scenario</span></span>                                                                    | <span data-ttu-id="22b0f-919">Önerimiz</span><span class="sxs-lookup"><span data-stu-id="22b0f-919">Our Suggestion</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="22b0f-920">Getirilen varlıklardan birçok gezinti özelliklerine erişmeniz gerekiyor mu?</span><span class="sxs-lookup"><span data-stu-id="22b0f-920">Do you need to access many navigation properties from the fetched entities?</span></span> | <span data-ttu-id="22b0f-921">**Hayır** -her iki seçenek de büyük olasılıkla yapılır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-921">**No** - Both options will probably do.</span></span> <span data-ttu-id="22b0f-922">Ancak, sorgunuzun yaptığı yük çok büyük değilse, nesnelerinizi bir araya getirmek için daha az ağ gidiş dönüşlerinin gerekli olduğu için, yükleme seçeneğini kullanarak performans avantajları yaşayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-922">However, if the payload your query is bringing is not too big, you may experience performance benefits by using Eager loading as it’ll require less network round trips to materialize your objects.</span></span> <br/> <br/> <span data-ttu-id="22b0f-923">**Evet** -varlıklardan çok sayıda gezinti özelliklerine erişmeniz gerekiyorsa, bunu sorgularınızda bulunan ve Eager yüklemesi ile birden çok Include deyimi kullanarak yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-923">**Yes** -  If you need to access many navigation properties from the entities, you’d do that by using multiple include statements in your query with Eager loading.</span></span> <span data-ttu-id="22b0f-924">Ne kadar çok varlık varsa, sorgunuzun döndürdüğü yükün daha büyük olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-924">The more entities you include, the bigger the payload your query will return.</span></span> <span data-ttu-id="22b0f-925">Sorgunuza üç veya daha fazla varlık eklediğinizde, geç yüklemeye geçmeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="22b0f-925">Once you include three or more entities into your query, consider switching to Lazy loading.</span></span> |
| <span data-ttu-id="22b0f-926">Çalışma zamanında tam olarak hangi verilerin gerekli olacağını tanıyor musunuz?</span><span class="sxs-lookup"><span data-stu-id="22b0f-926">Do you know exactly what data will be needed at run time?</span></span>                   | <span data-ttu-id="22b0f-927">**Hayır** -geç yükleme sizin için daha iyi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-927">**No** - Lazy loading will be better for you.</span></span> <span data-ttu-id="22b0f-928">Aksi takdirde, ihtiyaç duymayacak verilerin sorgulanmasını bitimeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-928">Otherwise, you may end up querying for data that you will not need.</span></span> <br/> <br/> <span data-ttu-id="22b0f-929">**Evet** -Eager yüklemesi büyük olasılıkla en iyi sonuç; tüm kümeleri daha hızlı yüklemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-929">**Yes** - Eager loading is probably your best bet; it will help loading entire sets faster.</span></span> <span data-ttu-id="22b0f-930">Sorgunuz çok büyük miktarda veri getirmeyi gerektiriyorsa ve bu çok yavaş hale gelirse bunun yerine yavaş yükleme yapmayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="22b0f-930">If your query requires fetching a very large amount of data, and this becomes too slow, then try Lazy loading instead.</span></span>                                                                                                                                                                                                                                                       |
| <span data-ttu-id="22b0f-931">Kodunuz veritabanınızdan mi çalışıyor?</span><span class="sxs-lookup"><span data-stu-id="22b0f-931">Is your code executing far from your database?</span></span> <span data-ttu-id="22b0f-932">(artan ağ gecikmesi)</span><span class="sxs-lookup"><span data-stu-id="22b0f-932">(increased network latency)</span></span>  | <span data-ttu-id="22b0f-933">**Hayır** -ağ gecikmesi bir sorun değilse, yavaş yükleme kullanmak kodunuzu basitleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-933">**No** - When the network latency is not an issue, using Lazy loading may simplify your code.</span></span> <span data-ttu-id="22b0f-934">Uygulamanızın topolojisinin değişeceğini unutmayın, bu nedenle verilen için veritabanı yakınlığını kaçırmayın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-934">Remember that the topology of your application may change, so don’t take database proximity for granted.</span></span> <br/> <br/> <span data-ttu-id="22b0f-935">**Evet** -ağ bir sorun olduğunda, senaryonuza ne kadar uygun olduğuna karar verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-935">**Yes** - When the network is a problem, only you can decide what fits better for your scenario.</span></span> <span data-ttu-id="22b0f-936">Genellikle, daha az gidiş dönüş gerektirdiğinden Eager yüklemesi daha iyi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-936">Typically Eager loading will be better because it requires fewer round trips.</span></span>                                                                                                                                                                                                      |


#### <a name="822-performance-concerns-with-multiple-includes"></a><span data-ttu-id="22b0f-937">birden çok Içerme ile 8.2.2 performans sorunları</span><span class="sxs-lookup"><span data-stu-id="22b0f-937">8.2.2       Performance concerns with multiple Includes</span></span>

<span data-ttu-id="22b0f-938">Sunucu yanıt süresi sorunlarını içeren performans sorularını duyduğumuz zaman, bu sorunun kaynağı birden çok Include deyimi ile sık kullanılan sorgulardır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-938">When we hear performance questions that involve server response time problems, the source of the issue is frequently queries with multiple Include statements.</span></span> <span data-ttu-id="22b0f-939">Bir sorgudaki ilgili varlıkları da dahil ederken, kapakların altında neler olduğunu anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-939">While including related entities in a query is powerful, it's important to understand what's happening under the covers.</span></span>

<span data-ttu-id="22b0f-940">İçinde birden fazla ekleme deyimi olan bir sorgu için, depo komutunu oluşturmak üzere iç plan derleyicimize gidebileceği görece uzun zaman alır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-940">It takes a relatively long time for a query with multiple Include statements in it to go through our internal plan compiler to produce the store command.</span></span> <span data-ttu-id="22b0f-941">Bu sürenin büyük bölümü, elde edilen sorguyu iyileştirmeye çalışırken harcanacak.</span><span class="sxs-lookup"><span data-stu-id="22b0f-941">The majority of this time is spent trying to optimize the resulting query.</span></span> <span data-ttu-id="22b0f-942">Oluşturulan depo komutu, eşlemelerinize bağlı olarak her bir ekleme için bir dış birleşim veya birleşim içerecektir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-942">The generated store command will contain an Outer Join or Union for each Include, depending on your mapping.</span></span> <span data-ttu-id="22b0f-943">Bunun gibi sorgular, özellikle de yük içinde çok fazla artıklık olduğunda (örneğin, çapraz geçiş için çok sayıda artım olduğunda) çok sayıda bant genişliği sorununu Acer, tek bir yükte veritabanınızdaki büyük bağlantılı grafikler sağlar. bire çok yönünde ilişkilendirmeler).</span><span class="sxs-lookup"><span data-stu-id="22b0f-943">Queries like this will bring in large connected graphs from your database in a single payload, which will acerbate any bandwidth issues, especially when there is a lot of redundancy in the payload (for example, when multiple levels of Include are used to traverse associations in the one-to-many direction).</span></span>

<span data-ttu-id="22b0f-944">Sorgu için ToTraceString kullanarak ve yük boyutunu görmek üzere SQL Server Management Studio ' de Store komutunu yürüterek, sorguların çok büyük yükleri döndüren durumları kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-944">You can check for cases where your queries are returning excessively large payloads by accessing the underlying TSQL for the query by using ToTraceString and executing the store command in SQL Server Management Studio to see the payload size.</span></span> <span data-ttu-id="22b0f-945">Bu gibi durumlarda, yalnızca ihtiyaç duyduğunuz verileri getirmek için sorguunuzdaki ekleme deyimlerinin sayısını azaltmayı deneyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-945">In such cases you can try to reduce the number of Include statements in your query to just bring in the data you need.</span></span> <span data-ttu-id="22b0f-946">Ya da sorgunuzu daha küçük bir alt sorgu dizisine bölebilir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="22b0f-946">Or you may be able to break your query into a smaller sequence of subqueries, for example:</span></span>

<span data-ttu-id="22b0f-947">**Sorguyu bozmadan önce:**</span><span class="sxs-lookup"><span data-stu-id="22b0f-947">**Before breaking the query:**</span></span>

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

<span data-ttu-id="22b0f-948">**Sorguyu kopardıktan sonra:**</span><span class="sxs-lookup"><span data-stu-id="22b0f-948">**After breaking the query:**</span></span>

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

<span data-ttu-id="22b0f-949">Bu, içeriğin kimlik çözümlemesi ve ilişkilendirme düzeltmesini otomatik olarak gerçekleştirme yeteneği kullandığımızda yalnızca izlenen sorgularda çalışır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-949">This will work only on tracked queries, as we are making use of the ability the context has to perform identity resolution and association fixup automatically.</span></span>

<span data-ttu-id="22b0f-950">Lazy yüklemesinde olduğu gibi, zorunluluğunu getirir daha küçük yük için daha fazla sorgu olacaktır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-950">As with lazy loading, the tradeoff will be more queries for smaller payloads.</span></span> <span data-ttu-id="22b0f-951">Ayrıca, her bir varlıktan yalnızca ihtiyaç duyduğunuz verileri açıkça seçmek için tek tek özelliklerin projeksiyonlarını da kullanabilirsiniz, ancak bu durumda varlıkları yüklemeyin ve güncelleştirmeler desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="22b0f-951">You can also use projections of individual properties to explicitly select only the data you need from each entity, but you will not be loading entities in this case, and updates will not be supported.</span></span>

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a><span data-ttu-id="22b0f-952">özelliklerin geç yüklenmesini sağlamak için 8.2.3 geçici çözüm</span><span class="sxs-lookup"><span data-stu-id="22b0f-952">8.2.3 Workaround to get lazy loading of properties</span></span>

<span data-ttu-id="22b0f-953">Entity Framework şu anda skaler veya karmaşık özelliklerin geç yüklemesini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="22b0f-953">Entity Framework currently doesn’t support lazy loading of scalar or complex properties.</span></span> <span data-ttu-id="22b0f-954">Ancak, BLOB gibi büyük bir nesne içeren bir tablonuz olduğu durumlarda, büyük özellikleri ayrı bir varlığa ayırmak için tablo bölmeyi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-954">However, in cases where you have a table that includes a large object such as a BLOB, you can use table splitting to separate the large properties into a separate entity.</span></span> <span data-ttu-id="22b0f-955">Örneğin, değişken fotoğraf sütunu içeren bir ürün tablonuz olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="22b0f-955">For example, suppose you have a Product table that includes a varbinary photo column.</span></span> <span data-ttu-id="22b0f-956">Sorgularda bu özelliğe sık sık erişmeniz gerekmiyorsa, yalnızca normal olarak ihtiyaç duyduğunuz varlığın parçalarını getirmek için tablo bölmeyi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-956">If you don't frequently need to access this property in your queries, you can use table splitting to bring in only the parts of the entity that you normally need.</span></span> <span data-ttu-id="22b0f-957">Ürün fotoğrafı temsil eden varlık yalnızca açıkça ihtiyacınız olduğunda yüklenecektir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-957">The entity representing the product photo will only be loaded when you explicitly need it.</span></span>

<span data-ttu-id="22b0f-958">Tablo bölmenin nasıl etkinleştirileceğini gösteren iyi bir kaynak, Entity Framework \<blog gönderisine "http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>tablo bölme"</span><span class="sxs-lookup"><span data-stu-id="22b0f-958">A good resource that shows how to enable table splitting is Gil Fink's "Table Splitting in Entity Framework" blog post: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.</span></span>

## <a name="9-other-considerations"></a><span data-ttu-id="22b0f-959">9 diğer konular</span><span class="sxs-lookup"><span data-stu-id="22b0f-959">9 Other considerations</span></span>

### <a name="91-server-garbage-collection"></a><span data-ttu-id="22b0f-960">9,1 sunucu çöp toplama</span><span class="sxs-lookup"><span data-stu-id="22b0f-960">9.1      Server Garbage Collection</span></span>

<span data-ttu-id="22b0f-961">Bazı kullanıcılar, Atık toplayıcısının düzgün yapılandırılmadığı durumlarda beklendikleri paralelliği sınırlayan kaynak çekişmesine karşılaşabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-961">Some users might experience resource contention that limits the parallelism they are expecting when the Garbage Collector is not properly configured.</span></span> <span data-ttu-id="22b0f-962">Çok iş parçacıklı bir senaryoda veya sunucu tarafı sistemine benzeyen herhangi bir uygulamada EF kullanıldığında, sunucu çöp toplamayı etkinleştirdiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="22b0f-962">Whenever EF is used in a multithreaded scenario, or in any application that resembles a server-side system, make sure to enable Server Garbage Collection.</span></span> <span data-ttu-id="22b0f-963">Bu, uygulama yapılandırma dosyanızdaki basit bir ayar aracılığıyla yapılır:</span><span class="sxs-lookup"><span data-stu-id="22b0f-963">This is done via a simple setting in your application config file:</span></span>

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

<span data-ttu-id="22b0f-964">Bu, iş parçacığı çekişmesini azaltmalı ve CPU doygun senaryolarda %30 ' a varan aktarım hızını artırmalıdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-964">This should decrease your thread contention and increase your throughput by up to 30% in CPU saturated scenarios.</span></span> <span data-ttu-id="22b0f-965">Genel koşullarda, her zaman uygulamanızın klasik çöp toplamayı (UI ve istemci tarafı senaryoları için daha iyi ayarlanmış) ve sunucu çöp toplama işlemini kullanarak nasıl davranacağını test etmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-965">In general terms, you should always test how your application behaves using the classic Garbage Collection (which is better tuned for UI and client side scenarios) as well as the Server Garbage Collection.</span></span>

### <a name="92-autodetectchanges"></a><span data-ttu-id="22b0f-966">9,2 Oto DetectChanges</span><span class="sxs-lookup"><span data-stu-id="22b0f-966">9.2      AutoDetectChanges</span></span>

<span data-ttu-id="22b0f-967">Daha önce belirtildiği gibi, nesne önbelleğinde çok sayıda varlık olduğunda Entity Framework performans sorunlarını gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-967">As mentioned earlier, Entity Framework might show performance issues when the object cache has many entities.</span></span> <span data-ttu-id="22b0f-968">Add, Remove, Find, Entry ve SaveChanges gibi bazı işlemler, nesne önbelleğinin ne kadar büyük bir CPU tüketilebileceğini anlamak için, DetectChanges 'a çağrıları tetikler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-968">Certain operations, such as Add, Remove, Find, Entry and SaveChanges, trigger calls to DetectChanges which might consume a large amount of CPU based on how large the object cache has become.</span></span> <span data-ttu-id="22b0f-969">Bunun nedeni, nesne önbelleğinin ve nesne durumu yöneticisinin, bir bağlam için gerçekleştirilen her işlem için mümkün olduğunca eşitlenmiş olarak kalmaya çalışacağı, üretilen verilerin çok sayıda senaryo kapsamında doğru olması garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-969">The reason for this is that the object cache and the object state manager try to stay as synchronized as possible on each operation performed to a context so that the produced data is guaranteed to be correct under a wide array of scenarios.</span></span>

<span data-ttu-id="22b0f-970">Uygulamanızın tüm ömrü için Entity Framework otomatik değişiklik algılamayı etkin bırakmak genellikle iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-970">It is generally a good practice to leave Entity Framework’s automatic change detection enabled for the entire life of your application.</span></span> <span data-ttu-id="22b0f-971">Senaryonuz yüksek CPU kullanımından olumsuz bir şekilde etkileniyorsa ve profilleriniz, SSIS 'nin DetectChanges çağrısı olduğunu gösteriyorsa, kodunuzun hassas bölümünde, geçici olarak yeniden gözden geçirin:</span><span class="sxs-lookup"><span data-stu-id="22b0f-971">If your scenario is being negatively affected by high CPU usage and your profiles indicate that the culprit is the call to DetectChanges, consider temporarily turning off AutoDetectChanges in the sensitive portion of your code:</span></span>

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

<span data-ttu-id="22b0f-972">Oto DetectChanges 'ı kapatmadan önce, bunun Entity Framework varlıklarda gerçekleşen değişikliklerle ilgili belirli bilgileri takip etme yeteneğini kaybetmesine neden olabileceğini anlamanız yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-972">Before turning off AutoDetectChanges, it’s good to understand that this might cause Entity Framework to lose its ability to track certain information about the changes that are taking place on the entities.</span></span> <span data-ttu-id="22b0f-973">Yanlış işlenirse, bu durum uygulamanızda veri tutarsızlığına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-973">If handled incorrectly, this might cause data inconsistency on your application.</span></span> <span data-ttu-id="22b0f-974">Oto DetectChanges 'i kapatma hakkında daha fazla bilgi için \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>okuyun.</span><span class="sxs-lookup"><span data-stu-id="22b0f-974">For more information on turning off AutoDetectChanges, read \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.</span></span>

### <a name="93-context-per-request"></a><span data-ttu-id="22b0f-975">istek başına 9,3 bağlam</span><span class="sxs-lookup"><span data-stu-id="22b0f-975">9.3      Context per request</span></span>

<span data-ttu-id="22b0f-976">Entity Framework bağlamlarının en iyi performans deneyimini sağlamak için kısa süreli örnekler olarak kullanılması amaçlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-976">Entity Framework’s contexts are meant to be used as short-lived instances in order to provide the most optimal performance experience.</span></span> <span data-ttu-id="22b0f-977">Bağlamların kısa süreli ve atılacak olması beklenir. bu nedenle, mümkün olduğunca çok hafif ve yeniden kullanmaya yönelik olarak uygulanırlar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-977">Contexts are expected to be short lived and discarded, and as such have been implemented to be very lightweight and reutilize metadata whenever possible.</span></span> <span data-ttu-id="22b0f-978">Web senaryolarında, tek bir isteğin süresinden daha uzun bir içeriğe sahip olmak için bunu göz önünde bulundurmanız önemlidir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-978">In web scenarios it’s important to keep this in mind and not have a context for more than the duration of a single request.</span></span> <span data-ttu-id="22b0f-979">Benzer şekilde, Web 'de olmayan senaryolarda bağlam, Entity Framework farklı önbelleğe alma seviyelerinin anlaşılmasına göre atılır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-979">Similarly, in non-web scenarios, context should be discarded based on your understanding of the different levels of caching in the Entity Framework.</span></span> <span data-ttu-id="22b0f-980">Genellikle, biri uygulamanın ömrü boyunca bağlam örneğine sahip olmanın yanı sıra iş parçacığı ve statik bağlamlara göre bağlamlar yapmaktan kaçınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-980">Generally speaking, one should avoid having a context instance throughout the life of the application, as well as contexts per thread and static contexts.</span></span>

### <a name="94-database-null-semantics"></a><span data-ttu-id="22b0f-981">9,4 veritabanı null semantiği</span><span class="sxs-lookup"><span data-stu-id="22b0f-981">9.4      Database null semantics</span></span>

<span data-ttu-id="22b0f-982">Varsayılan olarak Entity Framework, C\# null karşılaştırma semantiğinin bulunduğu SQL kodu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-982">Entity Framework by default will generate SQL code that has C\# null comparison semantics.</span></span> <span data-ttu-id="22b0f-983">Aşağıdaki örnek sorguyu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="22b0f-983">Consider the following example query:</span></span>

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

<span data-ttu-id="22b0f-984">Bu örnekte, bir dizi null yapılabilir değişkeni, varlığındaki null yapılabilir özelliklerle (SupplierID ve BirimFiyat gibi) karşılaştırıyoruz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-984">In this example, we’re comparing a number of nullable variables against nullable properties on the entity, such as SupplierID and UnitPrice.</span></span> <span data-ttu-id="22b0f-985">Bu sorgu için oluşturulan SQL, parametre değerinin sütun değeriyle aynı olup olmadığını veya hem parametre hem de sütun değerlerinin null olduğunu sorar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-985">The generated SQL for this query will ask if the parameter value is the same as the column value, or if both the parameter and the column values are null.</span></span> <span data-ttu-id="22b0f-986">Bu, veritabanı sunucusunun null değerleri işleme biçimini gizler ve farklı veritabanı satıcıları arasında tutarlı bir C\# null deneyimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-986">This will hide the way the database server handles nulls and will provide a consistent C\# null experience across different database vendors.</span></span> <span data-ttu-id="22b0f-987">Diğer taraftan, oluşturulan kod bir bit evdir ve sorgunun where deyimindeki karşılaştırma miktarı büyük bir sayıya büyüdükçe iyi şekilde gerçekleştirilemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-987">On the other hand, the generated code is a bit convoluted and may not perform well when the amount of comparisons in the where statement of the query grows to a large number.</span></span>

<span data-ttu-id="22b0f-988">Bu durumla başa çıkmak için bir yol veritabanı null semantiğini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-988">One way to deal with this situation is by using database null semantics.</span></span> <span data-ttu-id="22b0f-989">Entity Framework bundan sonra veritabanı altyapısının null değerleri işleme biçimini sunan daha basit bir SQL üretecağından, bu durum büyük olasılıkla C\# null semantiğinin farklı davranmasına neden olabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-989">Note that this might potentially behave differently to the C\# null semantics since now Entity Framework will generate simpler SQL that exposes the way the database engine handles null values.</span></span> <span data-ttu-id="22b0f-990">Veritabanı null semantiği bağlam yapılandırmasına karşı tek bir yapılandırma satırıyla bağlam başına etkinleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="22b0f-990">Database null semantics can be activated per-context with one single configuration line against the context configuration:</span></span>

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

<span data-ttu-id="22b0f-991">Küçük ve orta ölçekli sorgularda, veritabanı null semantiği kullanılırken bir algılanabilir performans geliştirmesi gösterilmez, ancak fark çok sayıda olası null karşılaştırmaları olan sorgularda daha belirgin olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-991">Small to medium sized queries will not display a perceptible performance improvement when using database null semantics, but the difference will become more noticeable on queries with a large number of potential null comparisons.</span></span>

<span data-ttu-id="22b0f-992">Yukarıdaki örnek sorguda performans farkı, denetlenen bir ortamda çalışan bir mikro kıyaslama için %2 ' den az idi.</span><span class="sxs-lookup"><span data-stu-id="22b0f-992">In the example query above, the performance difference was less than 2% in a microbenchmark running in a controlled environment.</span></span>

### <a name="95-async"></a><span data-ttu-id="22b0f-993">9,5 zaman uyumsuz</span><span class="sxs-lookup"><span data-stu-id="22b0f-993">9.5      Async</span></span>

<span data-ttu-id="22b0f-994">Entity Framework 6, .NET 4,5 veya üzeri sürümlerde çalışırken zaman uyumsuz işlemler desteği getirmiştir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-994">Entity Framework 6 introduced support of async operations when running on .NET 4.5 or later.</span></span> <span data-ttu-id="22b0f-995">Çoğu bölümde, GÇ ile ilgili çekişmeye sahip uygulamalar, zaman uyumsuz sorgu ve kaydetme işlemlerini kullanmanın en iyi avantajını kullanacaktır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-995">For the most part, applications that have IO related contention will benefit the most from using asynchronous query and save operations.</span></span> <span data-ttu-id="22b0f-996">Uygulamanız GÇ çekişme ile karşılaşmamışsa, zaman uyumsuz kullanılması en iyi şekilde çalışır ve sonucu zaman uyumlu bir çağrı ile aynı süre içinde döndürür ya da en kötü durumda yürütmeyi zaman uyumsuz bir göreve erteleyin ve ek Tim ekleyin Senaryonuzun tamamlanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-996">If your application does not suffer from IO contention, the use of async will, in the best cases, run synchronously and return the result in the same amount of time as a synchronous call, or in the worst case, simply defer execution to an asynchronous task and add extra time to the completion of your scenario.</span></span>

<span data-ttu-id="22b0f-997">Zaman uyumsuz programlama işinin, zaman uyumsuz olarak uygulamanızın performansını iyileştirecağına karar vermenize yardımcı olacak bilgiler için [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx)ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="22b0f-997">For information on how asynchronous programming work that will help you deciding if async will improve the performance of your application visit [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx).</span></span> <span data-ttu-id="22b0f-998">Entity Framework zaman uyumsuz işlemlerin kullanımı hakkında daha fazla bilgi için bkz. [Async Query ve Save](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="22b0f-998">For more information on the use of async operations on Entity Framework, see [Async Query and Save](~/ef6/fundamentals/async.md
).</span></span>

### <a name="96-ngen"></a><span data-ttu-id="22b0f-999">9,6 NGEN</span><span class="sxs-lookup"><span data-stu-id="22b0f-999">9.6      NGEN</span></span>

<span data-ttu-id="22b0f-1000">Entity Framework 6, .NET Framework 'ün varsayılan yüklemesinde gelmez.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1000">Entity Framework 6 does not come in the default installation of .NET framework.</span></span> <span data-ttu-id="22b0f-1001">Bu nedenle, Entity Framework derlemeleri varsayılan olarak NGEN değildir ve bu da tüm Entity Framework kodlarının diğer MSIL bütünleştirilmiş kodları ile aynı maliyet maliyetlerine tabidir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1001">As such, the Entity Framework assemblies are not NGEN’d by default which means that all the Entity Framework code is subject to the same JIT’ing costs as any other MSIL assembly.</span></span> <span data-ttu-id="22b0f-1002">Bu, uygulamanızı geliştirme sırasında ve ayrıca üretim ortamlarında uygulamanızın soğuk bir şekilde başlatılmasını sağlarken F5 deneyimini düşürebilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1002">This might degrade the F5 experience while developing and also the cold startup of your application in the production environments.</span></span> <span data-ttu-id="22b0f-1003">CPU ve bellek maliyetlerini azaltmak için, uygun şekilde Entity Framework görüntülerini NGEN önerilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1003">In order to reduce the CPU and memory costs of JIT’ing it is advisable to NGEN the Entity Framework images as appropriate.</span></span> <span data-ttu-id="22b0f-1004">Entity Framework 6 ' nın başlangıç performansını NGEN ile geliştirme hakkında daha fazla bilgi için bkz. [Ngen Ile başlangıç performansını artırma](~/ef6/fundamentals/performance/ngen.md).</span><span class="sxs-lookup"><span data-stu-id="22b0f-1004">For more information on how to improve the startup performance of Entity Framework 6 with NGEN, see [Improving Startup Performance with NGen](~/ef6/fundamentals/performance/ngen.md).</span></span>

### <a name="97-code-first-versus-edmx"></a><span data-ttu-id="22b0f-1005">9,7 Code First ve EDMX</span><span class="sxs-lookup"><span data-stu-id="22b0f-1005">9.7      Code First versus EDMX</span></span>

<span data-ttu-id="22b0f-1006">Kavramsal modelin bellek içi gösterimine (nesneler), depolama şemasına (veritabanı) ve arasında bir eşlemeye sahip olan nesne odaklı programlama ve ilişkisel veritabanları arasındaki empedance uyuşmazlığı sorunuyla ilgili nedenler Entity Framework. ikiye.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1006">Entity Framework reasons about the impedance mismatch problem between object oriented programming and relational databases by having an in-memory representation of the conceptual model (the objects), the storage schema (the database) and a mapping between the two.</span></span> <span data-ttu-id="22b0f-1007">Bu meta veriler Varlık Veri Modeli veya kısa için EDM olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1007">This metadata is called an Entity Data Model, or EDM for short.</span></span> <span data-ttu-id="22b0f-1008">Bu EDM Entity Framework, verileri bellekteki nesnelerden veritabanına ve geri dönüş için görünümleri türetecektir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1008">From this EDM, Entity Framework will derive the views to roundtrip data from the objects in memory to the database and back.</span></span>

<span data-ttu-id="22b0f-1009">Entity Framework kavramsal modeli, depolama şemasını ve eşlemeyi belirten bir EDMX dosyası ile kullanıldığında, model yükleme aşamasının yalnızca EDM 'nin doğru olduğunu doğrulaması gerekir (örneğin, hiçbir eşleme olmadığından emin olun), ardından görünümleri oluşturun, ardından görünümleri doğrulayın ve bu meta verilerin kullanıma hazırolmasını sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1009">When Entity Framework is used with an EDMX file that formally specifies the conceptual model, the storage schema, and the mapping, then the model loading stage only has to validate that the EDM is correct (for example, make sure that no mappings are missing), then generate the views, then validate the views and have this metadata ready for use.</span></span> <span data-ttu-id="22b0f-1010">Yalnızca bir sorgunun yürütülmesi veya veri deposuna yeni veri kaydedilmesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1010">Only then can a query be executed or new data be saved to the data store.</span></span>

<span data-ttu-id="22b0f-1011">Code First yaklaşım, bu noktada, gelişmiş bir Varlık Veri Modeli Oluşturucu olan.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1011">The Code First approach is, at its heart, a sophisticated Entity Data Model generator.</span></span> <span data-ttu-id="22b0f-1012">Entity Framework, belirtilen koddan bir EDM üretecek. Bu, modele dahil olan sınıfları çözümleyerek, kuralları uygulayarak ve modeli akıcı API aracılığıyla yapılandırarak yapar.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1012">The Entity Framework has to produce an EDM from the provided code; it does so by analyzing the classes involved in the model, applying conventions and configuring the model via the Fluent API.</span></span> <span data-ttu-id="22b0f-1013">EDM oluşturulduktan sonra, Entity Framework temelde, projede bir EDMX dosyası bulunduğu şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1013">After the EDM is built, the Entity Framework essentially behaves the same way as it would had an EDMX file been present in the project.</span></span> <span data-ttu-id="22b0f-1014">Bu nedenle Code First modelin oluşturulması, bir EDMX 'e kıyasla Entity Framework için daha yavaş bir başlangıç zamanına çeviren ek karmaşıklık ekler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1014">Thus, building the model from Code First adds extra complexity that translates into a slower startup time for the Entity Framework when compared to having an EDMX.</span></span> <span data-ttu-id="22b0f-1015">Maliyet, oluşturulmakta olan modelin boyutuna ve karmaşıklığına tamamen bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1015">The cost is completely dependent on the size and complexity of the model that’s being built.</span></span>

<span data-ttu-id="22b0f-1016">EDMX 'i Code First karşı kullanmayı seçerken, Code First tarafından tanıtılan esnekliğe, modelin ilk kez oluşturulmasına ilişkin maliyeti artırdığından emin olmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1016">When choosing to use EDMX versus Code First, it’s important to know that the flexibility introduced by Code First increases the cost of building the model for the first time.</span></span> <span data-ttu-id="22b0f-1017">Uygulamanız bu ilk kez yükleme maliyetini tercih edebilir, genellikle Code First gitmek için tercih edilen yol olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1017">If your application can withstand the cost of this first-time load then typically Code First will be the preferred way to go.</span></span>

## <a name="10-investigating-performance"></a><span data-ttu-id="22b0f-1018">10 araştırma performansı</span><span class="sxs-lookup"><span data-stu-id="22b0f-1018">10 Investigating Performance</span></span>

### <a name="101-using-the-visual-studio-profiler"></a><span data-ttu-id="22b0f-1019">10,1 Visual Studio Profiler 'ı kullanma</span><span class="sxs-lookup"><span data-stu-id="22b0f-1019">10.1 Using the Visual Studio Profiler</span></span>

<span data-ttu-id="22b0f-1020">Entity Framework performans sorunları yaşıyorsanız, uygulamanızın zamanını nerede harcadığını görmek için Visual Studio içinde yerleşik bir profil oluşturucu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1020">If you are having performance issues with the Entity Framework, you can use a profiler like the one built into Visual Studio to see where your application is spending its time.</span></span> <span data-ttu-id="22b0f-1021">Bu araç, "ADO.NET Entity Framework-Part 1" blog gönderisinin performansını keşfetme (\<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>), soğuk ve sıcak sorgular sırasında Entity Framework nereden zaman harcadığını gösteren bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1021">This is the tool we used to generate the pie charts in the “Exploring the Performance of the ADO.NET Entity Framework - Part 1” blog post ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) that show where Entity Framework spends its time during cold and warm queries.</span></span>

<span data-ttu-id="22b0f-1022">"Visual Studio 2010 Profiler kullanılarak profil oluşturma Entity Framework, veri ve modelleme müşteri danışmanlığı ekibi, bir performans sorununu araştırmak için profil oluşturucuyu nasıl kullandıklarından gerçek bir örnek gösterir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1022">The "Profiling Entity Framework using the Visual Studio 2010 Profiler" blog post written by the Data and Modeling Customer Advisory Team shows a real-world example of how they used the profiler to investigate a performance problem.</span></span><span data-ttu-id="22b0f-1023">  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1023">  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>.</span></span> <span data-ttu-id="22b0f-1024">Bu gönderi bir Windows uygulaması için yazılmıştır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1024">This post was written for a windows application.</span></span> <span data-ttu-id="22b0f-1025">Bir Web uygulaması profili oluşturmanız gerekiyorsa, Windows performans Kaydedicisi (WPR) ve Windows Performans Çözümleyicisi (WPA) araçları Visual Studio 'dan çalışmaktan daha iyi çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1025">If you need to profile a web application the Windows Performance Recorder (WPR) and Windows Performance Analyzer (WPA) tools may work better than working from Visual Studio.</span></span> <span data-ttu-id="22b0f-1026">WPR ve WPA, Windows değerlendirme ve dağıtım seti 'Nde ( [http://www.microsoft.com/download/details.aspx?id=39982](https://www.microsoft.com/download/details.aspx?id=39982)) bulunan Windows performans araç seti 'nin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1026">WPR and WPA are part of the Windows Performance Toolkit which is included with the Windows Assessment and Deployment Kit ( [http://www.microsoft.com/download/details.aspx?id=39982](https://www.microsoft.com/download/details.aspx?id=39982)).</span></span>

### <a name="102-applicationdatabase-profiling"></a><span data-ttu-id="22b0f-1027">10,2 uygulama/veritabanı profili oluşturma</span><span class="sxs-lookup"><span data-stu-id="22b0f-1027">10.2 Application/Database profiling</span></span>

<span data-ttu-id="22b0f-1028">Visual Studio 'da yerleşik olarak bulunan profil oluşturucu gibi araçlar, uygulamanızın ne zaman harcamış olduğunu söyler.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1028">Tools like the profiler built into Visual Studio tell you where your application is spending time.</span></span><span data-ttu-id="22b0f-1029">  Çalışan uygulamanızın dinamik analizini, gereksinimlerinize bağlı olarak üretim veya ön üretim aşamasında gerçekleştiren ve veritabanı erişiminin genel sınırları ve çapraz düzenlerini belirten başka bir profil oluşturucu türü vardır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1029">  Another type of profiler is available that performs dynamic analysis of your running application, either in production or pre-production depending on needs, and looks for common pitfalls and anti-patterns of database access.</span></span>

<span data-ttu-id="22b0f-1030">Entity Framework profil Oluşturucu (\<http://efprof.com>) ve ORMProfiler (\<http://ormprofiler.com>), ticari olarak kullanılabilir iki profil oluşturucular.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1030">Two commercially available profilers are the Entity Framework Profiler ( \<http://efprof.com>) and ORMProfiler ( \<http://ormprofiler.com>).</span></span>

<span data-ttu-id="22b0f-1031">Uygulamanız Code First kullanan bir MVC uygulaması ise, StackExchange 'in mini Profiler 'ı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1031">If your application is an MVC application using Code First, you can use StackExchange's MiniProfiler.</span></span> <span data-ttu-id="22b0f-1032">Scott Hanselman bu aracı blogda şu adreste açıklar: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1032">Scott Hanselman describes this tool in his blog at: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.</span></span>

<span data-ttu-id="22b0f-1033">Uygulamanızın veritabanı etkinliğinin profilini oluşturma hakkında daha fazla bilgi için, [Entity Framework profil oluşturma veritabanı etkinliği](https://msdn.microsoft.com/magazine/gg490349.aspx)başlıklı Julie Lerman 'ın MSDN Magazine makalesine bakın.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1033">For more information on profiling your application's database activity, see Julie Lerman's MSDN Magazine article titled [Profiling Database Activity in the Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).</span></span>

### <a name="103-database-logger"></a><span data-ttu-id="22b0f-1034">10,3 veritabanı günlükçüsü</span><span class="sxs-lookup"><span data-stu-id="22b0f-1034">10.3 Database logger</span></span>

<span data-ttu-id="22b0f-1035">Entity Framework 6 kullanıyorsanız, yerleşik günlüğe kaydetme işlevini de kullanmayı göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1035">If you are using Entity Framework 6 also consider using the built-in logging functionality.</span></span> <span data-ttu-id="22b0f-1036">Bağlam veritabanı özelliğine, etkinliklerini basit bir tek satır yapılandırması aracılığıyla günlüğe kaydetmek için de talimat verilir:</span><span class="sxs-lookup"><span data-stu-id="22b0f-1036">The Database property of the context can be instructed to log its activity via a simple one-line configuration:</span></span>

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

<span data-ttu-id="22b0f-1037">Bu örnekte, veritabanı etkinliği konsolunda günlüğe kaydedilir, ancak Log özelliği herhangi bir Işlem&lt;dize&gt; yetkisini çağırmak üzere yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1037">In this example the database activity will be logged to the console, but the Log property can be configured to call any Action&lt;string&gt; delegate.</span></span>

<span data-ttu-id="22b0f-1038">Veritabanı günlüğünü yeniden derlemeden etkinleştirmek istiyorsanız ve Entity Framework 6,1 veya sonraki bir sürümü kullanıyorsanız, uygulamanızın Web. config veya App. config dosyasına bir şifre ekleyerek bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1038">If you want to enable database logging without recompiling, and you are using Entity Framework 6.1 or later, you can do so by adding an interceptor in the web.config or app.config file of your application.</span></span>

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

<span data-ttu-id="22b0f-1039">Yeniden derleme olmadan günlüğe kaydetme ekleme hakkında daha fazla bilgi için \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>gidin.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1039">For more information on how to add logging without recompiling go to \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.</span></span>

## <a name="11-appendix"></a><span data-ttu-id="22b0f-1040">11 ek</span><span class="sxs-lookup"><span data-stu-id="22b0f-1040">11 Appendix</span></span>

### <a name="111-a-test-environment"></a><span data-ttu-id="22b0f-1041">11,1 A. test ortamı</span><span class="sxs-lookup"><span data-stu-id="22b0f-1041">11.1 A. Test Environment</span></span>

<span data-ttu-id="22b0f-1042">Bu ortam, istemci uygulamasından ayrı bir makine üzerinde veritabanıyla birlikte 2 makineli bir kurulum kullanır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1042">This environment uses a 2-machine setup with the database on a separate machine from the client application.</span></span> <span data-ttu-id="22b0f-1043">Makineler aynı rafta olduğundan ağ gecikmesi görece düşüktür, ancak tek makineli bir ortamdan daha gerçekçi olur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1043">Machines are in the same rack, so network latency is relatively low, but more realistic than a single-machine environment.</span></span>

#### <a name="1111-app-server"></a><span data-ttu-id="22b0f-1044">11.1.1 App Server</span><span class="sxs-lookup"><span data-stu-id="22b0f-1044">11.1.1       App Server</span></span>

##### <a name="11111-software-environment"></a><span data-ttu-id="22b0f-1045">11.1.1.1 yazılım ortamı</span><span class="sxs-lookup"><span data-stu-id="22b0f-1045">11.1.1.1      Software Environment</span></span>

-   <span data-ttu-id="22b0f-1046">Entity Framework 4 yazılım ortamı</span><span class="sxs-lookup"><span data-stu-id="22b0f-1046">Entity Framework 4 Software Environment</span></span>
    -   <span data-ttu-id="22b0f-1047">İşletim sistemi adı: Windows Server 2008 R2 Enterprise SP1.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1047">OS Name: Windows Server 2008 R2 Enterprise SP1.</span></span>
    -   <span data-ttu-id="22b0f-1048">Visual Studio 2010 – Ultimate.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1048">Visual Studio 2010 – Ultimate.</span></span>
    -   <span data-ttu-id="22b0f-1049">Visual Studio 2010 SP1 (yalnızca bazı karşılaştırmalar için).</span><span class="sxs-lookup"><span data-stu-id="22b0f-1049">Visual Studio 2010 SP1 (only for some comparisons).</span></span>
-   <span data-ttu-id="22b0f-1050">Entity Framework 5 ve 6 yazılım ortamı</span><span class="sxs-lookup"><span data-stu-id="22b0f-1050">Entity Framework 5 and 6 Software Environment</span></span>
    -   <span data-ttu-id="22b0f-1051">İşletim sistemi adı: Kurumsal Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="22b0f-1051">OS Name: Windows 8.1 Enterprise</span></span>
    -   <span data-ttu-id="22b0f-1052">Visual Studio 2013 – Ultimate.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1052">Visual Studio 2013 – Ultimate.</span></span>

##### <a name="11112-hardware-environment"></a><span data-ttu-id="22b0f-1053">11.1.1.2 donanım ortamı</span><span class="sxs-lookup"><span data-stu-id="22b0f-1053">11.1.1.2      Hardware Environment</span></span>

-   <span data-ttu-id="22b0f-1054">Çift Işlemci: Intel (R) Xeon (R) CPU L5520 W3530 @ 2.27 GHz, 2261 Mhz8 GHz, 4 çekirdek, 84 mantıksal Işlemci.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1054">Dual Processor:     Intel(R) Xeon(R) CPU L5520 W3530 @ 2.27GHz, 2261 Mhz8 GHz, 4 Core(s), 84 Logical Processor(s).</span></span>
-   <span data-ttu-id="22b0f-1055">2412 GB RamRAM.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1055">2412 GB RamRAM.</span></span>
-   <span data-ttu-id="22b0f-1056">136 GB SCSI250GB SATA 7200 RPM 3GB/s sürücü 4 bölüme bölünür.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1056">136 GB SCSI250GB SATA 7200 rpm 3GB/s drive split into 4 partitions.</span></span>

#### <a name="1112-db-server"></a><span data-ttu-id="22b0f-1057">11.1.2 DB sunucusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-1057">11.1.2       DB server</span></span>

##### <a name="11121-software-environment"></a><span data-ttu-id="22b0f-1058">11.1.2.1 yazılım ortamı</span><span class="sxs-lookup"><span data-stu-id="22b0f-1058">11.1.2.1      Software Environment</span></span>

-   <span data-ttu-id="22b0f-1059">İşletim sistemi adı: Windows Server 2008 R 28.1 Enterprise SP1.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1059">OS Name: Windows Server 2008 R28.1 Enterprise SP1.</span></span>
-   <span data-ttu-id="22b0f-1060">SQL Server 2008 R22012.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1060">SQL Server 2008 R22012.</span></span>

##### <a name="11122-hardware-environment"></a><span data-ttu-id="22b0f-1061">11.1.2.2 donanım ortamı</span><span class="sxs-lookup"><span data-stu-id="22b0f-1061">11.1.2.2      Hardware Environment</span></span>

-   <span data-ttu-id="22b0f-1062">Tek Işlemci: Intel (R) Xeon (R) CPU L5520 @ 2.27 GHz, 2261 Mhleştirir-1620 0 @ 3.60 GHz, 4 çekirdek, 8 mantıksal Işlemci.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1062">Single Processor: Intel(R) Xeon(R) CPU L5520  @ 2.27GHz, 2261 MhzES-1620 0 @ 3.60GHz, 4 Core(s), 8 Logical Processor(s).</span></span>
-   <span data-ttu-id="22b0f-1063">824 GB RamRAM.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1063">824 GB RamRAM.</span></span>
-   <span data-ttu-id="22b0f-1064">465 GB ATA500GB SATA 7200 RPM 6GB/s sürücü 4 bölüme bölünür.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1064">465 GB ATA500GB SATA 7200 rpm 6GB/s drive split into 4 partitions.</span></span>

### <a name="112-b-query-performance-comparison-tests"></a><span data-ttu-id="22b0f-1065">11,2 B. sorgu performansı karşılaştırma testleri</span><span class="sxs-lookup"><span data-stu-id="22b0f-1065">11.2      B. Query performance comparison tests</span></span>

<span data-ttu-id="22b0f-1066">Bu testleri yürütmek için Northwind modeli kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1066">The Northwind model was used to execute these tests.</span></span> <span data-ttu-id="22b0f-1067">Entity Framework Tasarımcısı kullanılarak veritabanından oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1067">It was generated from the database using the Entity Framework designer.</span></span> <span data-ttu-id="22b0f-1068">Ardından, sorgu yürütme seçeneklerinin performansını karşılaştırmak için aşağıdaki kod kullanılmıştır:</span><span class="sxs-lookup"><span data-stu-id="22b0f-1068">Then, the following code was used to compare the performance of the query execution options:</span></span>

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

### <a name="113-c-navision-model"></a><span data-ttu-id="22b0f-1069">11,3 C. Navision modeli</span><span class="sxs-lookup"><span data-stu-id="22b0f-1069">11.3 C. Navision Model</span></span>

<span data-ttu-id="22b0f-1070">Navision veritabanı, Microsoft Dynamics – NAV tanıtımı için kullanılan büyük bir veritabanıdır.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1070">The Navision database is a large database used to demo Microsoft Dynamics – NAV.</span></span> <span data-ttu-id="22b0f-1071">Oluşturulan kavramsal model 1005 varlık kümesi ve 4227 ilişkilendirme kümesi içerir.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1071">The generated conceptual model contains 1005 entity sets and 4227 association sets.</span></span> <span data-ttu-id="22b0f-1072">Testte kullanılan model "Flat" olarak belirlenmiştir. Bu, kendisine devralma eklenmedi.</span><span class="sxs-lookup"><span data-stu-id="22b0f-1072">The model used in the test is “flat” – no inheritance has been added to it.</span></span>

#### <a name="1131-queries-used-for-navision-tests"></a><span data-ttu-id="22b0f-1073">Navision testleri için kullanılan 11.3.1 sorguları</span><span class="sxs-lookup"><span data-stu-id="22b0f-1073">11.3.1 Queries used for Navision tests</span></span>

<span data-ttu-id="22b0f-1074">Navision modeliyle kullanılan sorgular listesi, Entity SQL sorgularının 3 kategorisini içerir:</span><span class="sxs-lookup"><span data-stu-id="22b0f-1074">The queries list used with the Navision model contains 3 categories of Entity SQL queries:</span></span>

##### <a name="11311-lookup"></a><span data-ttu-id="22b0f-1075">11.3.1.1 arama</span><span class="sxs-lookup"><span data-stu-id="22b0f-1075">11.3.1.1 Lookup</span></span>

<span data-ttu-id="22b0f-1076">Toplamaları olmayan basit bir arama sorgusu</span><span class="sxs-lookup"><span data-stu-id="22b0f-1076">A simple lookup query with no aggregations</span></span>

-   <span data-ttu-id="22b0f-1077">Sayı: 16232</span><span class="sxs-lookup"><span data-stu-id="22b0f-1077">Count: 16232</span></span>
-   <span data-ttu-id="22b0f-1078">Örnek:</span><span class="sxs-lookup"><span data-stu-id="22b0f-1078">Example:</span></span>

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312singleaggregating"></a><span data-ttu-id="22b0f-1079">11.3.1.2 Singletoplama</span><span class="sxs-lookup"><span data-stu-id="22b0f-1079">11.3.1.2 SingleAggregating</span></span>

<span data-ttu-id="22b0f-1080">Birden çok toplama içeren normal bir bı sorgusu, ancak alt toplamlar (tek sorgu)</span><span class="sxs-lookup"><span data-stu-id="22b0f-1080">A normal BI query with multiple aggregations, but no subtotals (single query)</span></span>

-   <span data-ttu-id="22b0f-1081">Sayı: 2313</span><span class="sxs-lookup"><span data-stu-id="22b0f-1081">Count: 2313</span></span>
-   <span data-ttu-id="22b0f-1082">Örnek:</span><span class="sxs-lookup"><span data-stu-id="22b0f-1082">Example:</span></span>

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

<span data-ttu-id="22b0f-1083">Burada MDF\_SessionLogin\_zaman\_Max () modelinde tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="22b0f-1083">Where MDF\_SessionLogin\_Time\_Max() is defined in the model as:</span></span>

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313aggregatingsubtotals"></a><span data-ttu-id="22b0f-1084">11.3.1.3 Aggregatingalt toplamları</span><span class="sxs-lookup"><span data-stu-id="22b0f-1084">11.3.1.3 AggregatingSubtotals</span></span>

<span data-ttu-id="22b0f-1085">Toplamaları ve alt toplamları olan bir bı sorgusu (UNION ALL aracılığıyla)</span><span class="sxs-lookup"><span data-stu-id="22b0f-1085">A BI query with aggregations and subtotals (via union all)</span></span>

-   <span data-ttu-id="22b0f-1086">Sayı: 178</span><span class="sxs-lookup"><span data-stu-id="22b0f-1086">Count: 178</span></span>
-   <span data-ttu-id="22b0f-1087">Örnek:</span><span class="sxs-lookup"><span data-stu-id="22b0f-1087">Example:</span></span>

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
