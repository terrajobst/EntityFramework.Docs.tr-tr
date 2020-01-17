---
title: Entity Framework Core 5,0 planlaması
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 0472841fdcd105ec8ea38db062c6768510b8735d
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125384"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="02d1e-102">Entity Framework Core 5,0 planlaması</span><span class="sxs-lookup"><span data-stu-id="02d1e-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="02d1e-103">[Planlama sürecinde](../release-planning.md)açıklandığı gibi, hissedarlardan EF Core 5,0 sürümü için geçici bir plana toplanan girişleri topladık.</span><span class="sxs-lookup"><span data-stu-id="02d1e-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="02d1e-104">Bu plan hala devam eden bir çalışmadır.</span><span class="sxs-lookup"><span data-stu-id="02d1e-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="02d1e-105">Taahhüt aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="02d1e-105">Nothing here is a commitment.</span></span> <span data-ttu-id="02d1e-106">Bu plan, daha fazla öğrendiğimiz için geliştireceğiz bir başlangıç noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="02d1e-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="02d1e-107">5,0 için şu anda planlanmayan bazı şeyler, çekime alabilir.</span><span class="sxs-lookup"><span data-stu-id="02d1e-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="02d1e-108">5,0 için şu anda planlanmış bazı şeyler kullanıma hazır olabilir.</span><span class="sxs-lookup"><span data-stu-id="02d1e-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="02d1e-109">Sürüm numarası ve sürüm tarihi.</span><span class="sxs-lookup"><span data-stu-id="02d1e-109">Version number and release date.</span></span>

<span data-ttu-id="02d1e-110">EF Core 5,0 şu anda sürüm için [.net 5,0 ile aynı zamanda](https://devblogs.microsoft.com/dotnet/introducing-net-5/)zamanlandı.</span><span class="sxs-lookup"><span data-stu-id="02d1e-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="02d1e-111">.NET 5,0 ile hizalamak için "5,0" sürümü seçildi.</span><span class="sxs-lookup"><span data-stu-id="02d1e-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="02d1e-112">Desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="02d1e-112">Supported platforms</span></span>

<span data-ttu-id="02d1e-113">EF Core 5,0, [Bu platformların .NET Core 'a yakınsamasını](https://devblogs.microsoft.com/dotnet/introducing-net-5/)temel alan herhangi bir .NET 5,0 platformunda çalıştırılmak üzere planlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="02d1e-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="02d1e-114">Bu .NET Standard, ne anlama gelir ve kullanılan gerçek tfd, hala TBD 'dir.</span><span class="sxs-lookup"><span data-stu-id="02d1e-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="02d1e-115">EF Core 5,0 .NET Framework çalıştırmayacak.</span><span class="sxs-lookup"><span data-stu-id="02d1e-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="02d1e-116">Yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="02d1e-116">Breaking changes</span></span>

<span data-ttu-id="02d1e-117">EF Core 5,0 bazı önemli değişiklikler içerir, ancak bu durum EF Core 3,0 ' den çok daha az önem altına alınır.</span><span class="sxs-lookup"><span data-stu-id="02d1e-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="02d1e-118">Hedefimiz, uygulamanın büyük çoğunluğunun bozmadan güncelleştirilmesine izin vermektir.</span><span class="sxs-lookup"><span data-stu-id="02d1e-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="02d1e-119">Veritabanı sağlayıcılarının, özellikle de TPT desteğinin çevresinde bazı önemli değişiklikler olacağı için bu değer beklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="02d1e-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="02d1e-120">Bununla birlikte, 5,0 için bir sağlayıcıyı güncelleştirme işinin 3,0 için güncelleştirilmesi gerekenden daha az olacağını umyoruz.</span><span class="sxs-lookup"><span data-stu-id="02d1e-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="02d1e-121">Temalar</span><span class="sxs-lookup"><span data-stu-id="02d1e-121">Themes</span></span>

<span data-ttu-id="02d1e-122">EF Core 5,0 ' de büyük yatırımların temelini oluşturacak birkaç önemli alanı veya temaları ayıkladık.</span><span class="sxs-lookup"><span data-stu-id="02d1e-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="02d1e-123">Çoka çok gezinti özellikleri (bir. k. a "gezintilerini atla")</span><span class="sxs-lookup"><span data-stu-id="02d1e-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="02d1e-124">Lider geliştiricileri: @smitpatel ve @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="02d1e-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="02d1e-125">[#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="02d1e-126">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="02d1e-126">T-shirt size: L</span></span>

<span data-ttu-id="02d1e-127">Durum: devam ediyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-127">Status: In-progress</span></span>

<span data-ttu-id="02d1e-128">Birden çok-çok, GitHub biriktirme listesindeki en çok istenen özelliktir (~ 407 oylardır).</span><span class="sxs-lookup"><span data-stu-id="02d1e-128">Many-to-many is the most requested feature (~407 votes) on the GitHub backlog.</span></span> <span data-ttu-id="02d1e-129">Çoktan çoğa ilişkiler için destek üç ana alana ayrılabilir:</span><span class="sxs-lookup"><span data-stu-id="02d1e-129">Support for many-to-many relationships can be broken down into three major areas:</span></span>

* <span data-ttu-id="02d1e-130">Gezinme özelliklerini atlayın.</span><span class="sxs-lookup"><span data-stu-id="02d1e-130">Skip navigation properties.</span></span> <span data-ttu-id="02d1e-131">Bunlar, modelin, temel alınan JOIN tablosu varlığına başvurmaksızın sorgular, vb. için kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="02d1e-131">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span>
* <span data-ttu-id="02d1e-132">Özellik paketi varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="02d1e-132">Property-bag entity types.</span></span> <span data-ttu-id="02d1e-133">Bunlar, varlık örnekleri için, her varlık türü için açık bir CLR türü gerektirmeyen standart bir CLR türüne (ör. `Dictionary`) olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="02d1e-133">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span>
* <span data-ttu-id="02d1e-134">Çoka çok ilişkilerin kolay yapılandırması için cukr.</span><span class="sxs-lookup"><span data-stu-id="02d1e-134">Sugar for easy configuration of many-to-many relationships.</span></span>

<span data-ttu-id="02d1e-135">Çok-çok desteği olan en önemli engelleyici, JOIN tablosuna başvurulmadan, sorgular gibi iş mantığından "doğal" ilişkileri kullanmadığımızı düşüntik.</span><span class="sxs-lookup"><span data-stu-id="02d1e-135">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="02d1e-136">JOIN tablosu varlık türü yine de mevcut olabilir, ancak iş mantığı gibi kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="02d1e-136">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="02d1e-137">Bu nedenle 5,0 için gezinme özelliklerini atla ' nın üstesinden gelmeyi seçtik.</span><span class="sxs-lookup"><span data-stu-id="02d1e-137">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="02d1e-138">Şu anda, çoktan çoğa diğer bölümleri EF Core 5,0 için bir Esnetme hedefi olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="02d1e-138">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="02d1e-139">Bu, şu anda 5,0 için planda olmadıkları anlamına gelir, ancak şeyler ' ın içinde çekmesini umuyoruz.</span><span class="sxs-lookup"><span data-stu-id="02d1e-139">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="02d1e-140">Tablo/tür (TPT) devralma eşleme</span><span class="sxs-lookup"><span data-stu-id="02d1e-140">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="02d1e-141">Lider geliştiricisi: @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="02d1e-141">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="02d1e-142">[#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-142">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="02d1e-143">Tişörlü Boyut: XL</span><span class="sxs-lookup"><span data-stu-id="02d1e-143">T-shirt size: XL</span></span>

<span data-ttu-id="02d1e-144">Durum: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="02d1e-144">Status: Not started</span></span>

<span data-ttu-id="02d1e-145">Yüksek düzeyde istenmiş bir Özellik (~ 254 oy ve 3. Genel) olduğundan ve genel .NET 5 planının temel yapısı için uygun olan bazı düşük düzey değişiklikler gerektirdiğinden TPT yapıyoruz.</span><span class="sxs-lookup"><span data-stu-id="02d1e-145">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="02d1e-146">Bunun, 3,0 için gereken değişikliklerden çok daha az ciddi olmasına rağmen, bu, veritabanı sağlayıcılarının değişikliklere neden olduğunu umyoruz.</span><span class="sxs-lookup"><span data-stu-id="02d1e-146">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="02d1e-147">Filtrelenen ekleme</span><span class="sxs-lookup"><span data-stu-id="02d1e-147">Filtered Include</span></span>

<span data-ttu-id="02d1e-148">Lider geliştiricisi: @maumar</span><span class="sxs-lookup"><span data-stu-id="02d1e-148">Lead developer: @maumar</span></span>

<span data-ttu-id="02d1e-149">[#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-149">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="02d1e-150">Tişörlü Boyut: a</span><span class="sxs-lookup"><span data-stu-id="02d1e-150">T-shirt size: M</span></span>

<span data-ttu-id="02d1e-151">Durum: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="02d1e-151">Status: Not started</span></span>

<span data-ttu-id="02d1e-152">Filtrelenmiş dahil, çok büyük bir iş miktarı olmayan ve şu anda model düzeyi filtreler veya daha karmaşık sorgular gerektiren çok sayıda senaryonun engellemesini veya daha kolay olduğunu düşünmemiz için yüksek düzeyde istenmiş bir özelliktir (~ 317 oy; 2. Genel).</span><span class="sxs-lookup"><span data-stu-id="02d1e-152">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="02d1e-153">Dtiontotable, ToQuery, ToView, FromSql vb.</span><span class="sxs-lookup"><span data-stu-id="02d1e-153">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="02d1e-154">Lider geliştiricileri: @maumar ve @smitpatel</span><span class="sxs-lookup"><span data-stu-id="02d1e-154">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="02d1e-155">[#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-155">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="02d1e-156">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="02d1e-156">T-shirt size: L</span></span>

<span data-ttu-id="02d1e-157">Durum: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="02d1e-157">Status: Not started</span></span>

<span data-ttu-id="02d1e-158">Ham SQL, anahtarsız türlerini ve ilgili alanı desteklemeye yönelik önceki sürümlerde ilerleme yaptık.</span><span class="sxs-lookup"><span data-stu-id="02d1e-158">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="02d1e-159">Ancak, her şeyin bir bütün olarak birlikte çalıştıkları şekilde hem boşluklar hem de tutarsızlıklar vardır.</span><span class="sxs-lookup"><span data-stu-id="02d1e-159">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="02d1e-160">5,0 için amaç bunları çözmektir ve farklı varlık türlerini ve bunlarla ilişkili sorguları ve veritabanı yapılarını tanımlamak, geçirmek ve kullanmak için iyi bir deneyim oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="02d1e-160">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="02d1e-161">Bu, derlenmiş sorgu API 'SI güncelleştirmelerini de içerebilir.</span><span class="sxs-lookup"><span data-stu-id="02d1e-161">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="02d1e-162">Bu öğe, şu anda kişilerin hata Pits 'e hızlı bir şekilde yol açabileceğini sağlayan çok fazla izin olduğundan, bu öğenin bazı uygulama düzeyi değişikliklere neden olabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="02d1e-162">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="02d1e-163">Bunun yerine ne yapacaklarla ilgili rehberlik ile bu işlevlerden bazılarını engellemeyi büyük olasılıkla yapacağız.</span><span class="sxs-lookup"><span data-stu-id="02d1e-163">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="02d1e-164">Genel sorgu geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="02d1e-164">General query enhancements</span></span>

<span data-ttu-id="02d1e-165">Lider geliştiricileri: @smitpatel ve @maumar</span><span class="sxs-lookup"><span data-stu-id="02d1e-165">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="02d1e-166">[5,0 kilometre taşında `area-query` etiketli sorunlar](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-166">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="02d1e-167">Tişörlü Boyut: XL</span><span class="sxs-lookup"><span data-stu-id="02d1e-167">T-shirt size: XL</span></span>

<span data-ttu-id="02d1e-168">Durum: devam ediyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-168">Status: In-progress</span></span>

<span data-ttu-id="02d1e-169">Sorgu çevirisi kodu EF Core 3,0 için kapsamlı olarak yeniden yazıldı.</span><span class="sxs-lookup"><span data-stu-id="02d1e-169">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="02d1e-170">Sorgu kodu genellikle bu nedenle çok daha sağlam durumda.</span><span class="sxs-lookup"><span data-stu-id="02d1e-170">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="02d1e-171">5,0 için, TPT 'yi desteklemek ve gezinme özelliklerini atlamak için gerekenlerden büyük sorgu değişiklikleri yapmayı planlamadık.</span><span class="sxs-lookup"><span data-stu-id="02d1e-171">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="02d1e-172">Bununla birlikte, 3,0 fazla yerine kalan bazı teknik borcu gidermek için hala önemli bir iş vardır.</span><span class="sxs-lookup"><span data-stu-id="02d1e-172">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="02d1e-173">Ayrıca, genel sorgu deneyimini geliştirmek için birçok hatayı gidermeyi planlıyoruz ve küçük geliştirmeler uygulayacağız.</span><span class="sxs-lookup"><span data-stu-id="02d1e-173">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="02d1e-174">Geçişler ve dağıtım deneyimi</span><span class="sxs-lookup"><span data-stu-id="02d1e-174">Migrations and deployment experience</span></span>

<span data-ttu-id="02d1e-175">Lider geliştiricileri: @bricelam</span><span class="sxs-lookup"><span data-stu-id="02d1e-175">Lead developers: @bricelam</span></span>

<span data-ttu-id="02d1e-176">[#19587](https://github.com/dotnet/efcore/issues/19587) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-176">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="02d1e-177">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="02d1e-177">T-shirt size: L</span></span>

<span data-ttu-id="02d1e-178">Durum: devam ediyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-178">Status: In-progress</span></span>

<span data-ttu-id="02d1e-179">Şu anda birçok geliştirici, veritabanlarını uygulama başlatma zamanına geçirecektir.</span><span class="sxs-lookup"><span data-stu-id="02d1e-179">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="02d1e-180">Bu kolay ancak önerilmez, çünkü:</span><span class="sxs-lookup"><span data-stu-id="02d1e-180">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="02d1e-181">Birden çok iş parçacığı/işlem/sunucu veritabanını aynı anda geçirmeye çalışabilir</span><span class="sxs-lookup"><span data-stu-id="02d1e-181">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="02d1e-182">Uygulamalar, bu durumdayken tutarsız duruma erişmeye çalışabilir</span><span class="sxs-lookup"><span data-stu-id="02d1e-182">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="02d1e-183">Genellikle Şemayı değiştirme veritabanı izinleri uygulama yürütmesi için verilmemelidir</span><span class="sxs-lookup"><span data-stu-id="02d1e-183">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="02d1e-184">Bir şeyler yanlış olursa temiz bir duruma geri dönme</span><span class="sxs-lookup"><span data-stu-id="02d1e-184">Its hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="02d1e-185">Veritabanını dağıtım zamanında geçirmek için kolay bir yol sağlayan daha iyi bir deneyim sunmak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="02d1e-185">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="02d1e-186">Şunları yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="02d1e-186">This should:</span></span>

* <span data-ttu-id="02d1e-187">Linux, Mac ve Windows üzerinde çalışma</span><span class="sxs-lookup"><span data-stu-id="02d1e-187">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="02d1e-188">Komut satırında iyi bir deneyim olun</span><span class="sxs-lookup"><span data-stu-id="02d1e-188">Be a good experience on the command line</span></span>
* <span data-ttu-id="02d1e-189">Kapsayıcılarla destek senaryoları</span><span class="sxs-lookup"><span data-stu-id="02d1e-189">Support scenarios with containers</span></span>
* <span data-ttu-id="02d1e-190">Yaygın olarak kullanılan gerçek dünya dağıtım araçlarıyla ve akışlarla çalışma</span><span class="sxs-lookup"><span data-stu-id="02d1e-190">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="02d1e-191">En az Visual Studio ile tümleştirin</span><span class="sxs-lookup"><span data-stu-id="02d1e-191">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="02d1e-192">Sonuçta, yalnızca EF 'in ötesinde çok sayıda küçük EF Core geliştirmeler (örneğin, SQLite üzerinde daha iyi geçişler), diğer ekiplere yönelik kılavuzlarla daha fazla geçiş ve daha uzun süreli İşbirlikten yararlanmaktır.</span><span class="sxs-lookup"><span data-stu-id="02d1e-192">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="02d1e-193">EF Core platformları deneyimi</span><span class="sxs-lookup"><span data-stu-id="02d1e-193">EF Core platforms experience</span></span> 

<span data-ttu-id="02d1e-194">Lider geliştiricileri: @roji ve @bricelam</span><span class="sxs-lookup"><span data-stu-id="02d1e-194">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="02d1e-195">[#19588](https://github.com/dotnet/efcore/issues/19588) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-195">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="02d1e-196">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="02d1e-196">T-shirt size: L</span></span>

<span data-ttu-id="02d1e-197">Durum: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="02d1e-197">Status: Not started</span></span>

<span data-ttu-id="02d1e-198">Geleneksel MVC benzeri Web uygulamalarında EF Core kullanmak için iyi bir kılavuzluk sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="02d1e-198">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="02d1e-199">Diğer platformlar ve uygulama modelleriyle ilgili rehberlik eksik ya da güncel değil.</span><span class="sxs-lookup"><span data-stu-id="02d1e-199">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="02d1e-200">EF Core 5,0 ' de, ile EF Core kullanma deneyimini araştırmaya, iyileştirmenize ve belgeleyecek şekilde planlıyoruz:</span><span class="sxs-lookup"><span data-stu-id="02d1e-200">For EF Core 5.0 we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="02d1e-201">Blazor</span><span class="sxs-lookup"><span data-stu-id="02d1e-201">Blazor</span></span>
* <span data-ttu-id="02d1e-202">AOT/bağlayıcı hikayesini kullanma dahil Xamarin</span><span class="sxs-lookup"><span data-stu-id="02d1e-202">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="02d1e-203">WinForms/WPF/WinUI ve muhtemelen diğer U.I.</span><span class="sxs-lookup"><span data-stu-id="02d1e-203">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="02d1e-204">çerçeveleri</span><span class="sxs-lookup"><span data-stu-id="02d1e-204">frameworks</span></span>

<span data-ttu-id="02d1e-205">Bu, EF Core çok küçük geliştirmeler, diğer ekiplere yönelik kılavuz ve uzun süreli ortak çalışmalarla birlikte, yalnızca EF 'in ötesine geçen uçtan uca deneyimler geliştirmeye olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="02d1e-205">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="02d1e-206">Göz atadığımız belirli bölgeler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="02d1e-206">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="02d1e-207">Geçiş için gibi EF araçları kullanma deneyimini de içeren dağıtım</span><span class="sxs-lookup"><span data-stu-id="02d1e-207">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="02d1e-208">Xamarin ve Blazor dahil olmak üzere uygulama modelleri ve büyük olasılıkla diğerleri</span><span class="sxs-lookup"><span data-stu-id="02d1e-208">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="02d1e-209">Uzamsal deneyim ve tablo da dahil olmak üzere SQLite deneyimler</span><span class="sxs-lookup"><span data-stu-id="02d1e-209">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="02d1e-210">AOT ve bağlama deneyimleri</span><span class="sxs-lookup"><span data-stu-id="02d1e-210">AOT and linking experiences</span></span>
* <span data-ttu-id="02d1e-211">Performans sayaçlarını içeren tanılama tümleştirmesi</span><span class="sxs-lookup"><span data-stu-id="02d1e-211">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="02d1e-212">Performans</span><span class="sxs-lookup"><span data-stu-id="02d1e-212">Performance</span></span>

<span data-ttu-id="02d1e-213">Lider geliştiricisi: @roji</span><span class="sxs-lookup"><span data-stu-id="02d1e-213">Lead developer: @roji</span></span>

<span data-ttu-id="02d1e-214">[5,0 kilometre taşında `area-perf` etiketli sorunlar](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-214">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="02d1e-215">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="02d1e-215">T-shirt size: L</span></span>

<span data-ttu-id="02d1e-216">Durum: devam ediyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-216">Status: In-progress</span></span>

<span data-ttu-id="02d1e-217">EF Core için performans değerlendirmeleri takımımızı geliştirmeyi ve çalışma zamanında yönlendirilmiş performans iyileştirmeleri yapmayı planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="02d1e-217">For EF Core we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="02d1e-218">Ayrıca, 3,0 yayın çevrimi sırasında prototip oluşturulan yeni ADO.NET toplu işlem API 'sini tamamlamayı planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="02d1e-218">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="02d1e-219">Ayrıca, ADO.NET katmanında Npgsql sağlayıcısında ek performans geliştirmeleri planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="02d1e-219">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="02d1e-220">Bu çalışmanın bir parçası olarak, uygun şekilde ADO.NET/EF Core performans sayaçlarını ve diğer tanılamayı eklemeyi de planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="02d1e-220">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="02d1e-221">Mimari/katkıda bulunan belgeleri</span><span class="sxs-lookup"><span data-stu-id="02d1e-221">Architectural/contributor documentation</span></span>

<span data-ttu-id="02d1e-222">Müşteri adayı belge girme: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="02d1e-222">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="02d1e-223">[#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-223">Tracked by [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="02d1e-224">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="02d1e-224">T-shirt size: L</span></span>

<span data-ttu-id="02d1e-225">Durum: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="02d1e-225">Status: Not started</span></span>

<span data-ttu-id="02d1e-226">Buradaki fikir, EF Core iç yapıları hakkında daha kolay anlaşılır hale getirmek için.</span><span class="sxs-lookup"><span data-stu-id="02d1e-226">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="02d1e-227">Bu, EF Core kullanan herkes için yararlı olabilir, ancak birincil mosyon, dış kişilerin şunları yapmasını kolaylaştırır:</span><span class="sxs-lookup"><span data-stu-id="02d1e-227">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="02d1e-228">EF Core koduna katkıda bulunun</span><span class="sxs-lookup"><span data-stu-id="02d1e-228">Contribute to the EF Core code</span></span>
* <span data-ttu-id="02d1e-229">Veritabanı sağlayıcıları oluşturma</span><span class="sxs-lookup"><span data-stu-id="02d1e-229">Create database providers</span></span>
* <span data-ttu-id="02d1e-230">Diğer uzantıları oluşturma</span><span class="sxs-lookup"><span data-stu-id="02d1e-230">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="02d1e-231">Microsoft. Data. SQLite belgeleri</span><span class="sxs-lookup"><span data-stu-id="02d1e-231">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="02d1e-232">Müşteri adayı belge girme: @bricelam</span><span class="sxs-lookup"><span data-stu-id="02d1e-232">Lead documenter: @bricelam</span></span>

<span data-ttu-id="02d1e-233">[#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-233">Tracked by [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="02d1e-234">Tişörlü Boyut: a</span><span class="sxs-lookup"><span data-stu-id="02d1e-234">T-shirt size: M</span></span>

<span data-ttu-id="02d1e-235">Durum: tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="02d1e-235">Status: Completed.</span></span> <span data-ttu-id="02d1e-236">Yeni belgeler [Microsoft docs sitesinde canlı](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="02d1e-236">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="02d1e-237">EF ekibi, Microsoft. Data. SQLite ADO.NET sağlayıcısına de sahiptir.</span><span class="sxs-lookup"><span data-stu-id="02d1e-237">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="02d1e-238">Bu sağlayıcıyı 5,0 sürümünün bir parçası olarak tam olarak belgelemek planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="02d1e-238">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="02d1e-239">Genel belgeler</span><span class="sxs-lookup"><span data-stu-id="02d1e-239">General documentation</span></span>

<span data-ttu-id="02d1e-240">Müşteri adayı belge girme: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="02d1e-240">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="02d1e-241">[5,0 kilometre taşında docs deposunda bulunan sorunlara](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+) göre izleniyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-241">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="02d1e-242">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="02d1e-242">T-shirt size: L</span></span>

<span data-ttu-id="02d1e-243">Durum: devam ediyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-243">Status: In-progress</span></span>

<span data-ttu-id="02d1e-244">3,0 ve 3,1 sürümleri için belgeleri güncelleştirme sürecimiz zaten var.</span><span class="sxs-lookup"><span data-stu-id="02d1e-244">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="02d1e-245">Ayrıca şu şekilde çalışıyoruz:</span><span class="sxs-lookup"><span data-stu-id="02d1e-245">We are also working on:</span></span>
  * <span data-ttu-id="02d1e-246">Daha fazla ulaşılabilir/daha kolay hale getirmek için kullanmaya başlama belgelerine yönelik fazla mesafe</span><span class="sxs-lookup"><span data-stu-id="02d1e-246">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="02d1e-247">Çapraz başvuruları bulmayı ve eklemeyi kolaylaştırmak için belgeleri yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="02d1e-247">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="02d1e-248">Mevcut docs için daha fazla ayrıntı ve ayrıntılı açıklamalar ekleme</span><span class="sxs-lookup"><span data-stu-id="02d1e-248">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="02d1e-249">Örnekleri güncelleştirme ve daha fazla örnek ekleme</span><span class="sxs-lookup"><span data-stu-id="02d1e-249">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="02d1e-250">Hataları düzeltme</span><span class="sxs-lookup"><span data-stu-id="02d1e-250">Fixing bugs</span></span>

<span data-ttu-id="02d1e-251">[5,0 kilometre taşında `type-bug` etiketli sorunlar](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-251">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="02d1e-252">Geliştiriciler: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="02d1e-252">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="02d1e-253">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="02d1e-253">T-shirt size: L</span></span>

<span data-ttu-id="02d1e-254">Durum: devam ediyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-254">Status: In-progress</span></span>

<span data-ttu-id="02d1e-255">Yazma sırasında, 5,0 sürümünde (zaten düzeltilmiş olan 62) düzeltilen 135 hata değerlendirildi, ancak yukarıdaki _genel sorgu geliştirmeleri_ bölümünde önemli bir çakışma var.</span><span class="sxs-lookup"><span data-stu-id="02d1e-255">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="02d1e-256">Gelen ücret (bir kilometre taşında çalışan olarak biten sorunlar) 3,0 sürümü boyunca ayda yaklaşık 23 sorun olmuştur.</span><span class="sxs-lookup"><span data-stu-id="02d1e-256">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="02d1e-257">Bunların tümünün 5,0 ' de düzeltilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="02d1e-257">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="02d1e-258">Kabaca bir tahmin olarak 5,0 zaman çerçevesinde ek 150 sorunları gidermeyi planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="02d1e-258">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="02d1e-259">Küçük geliştirmeler</span><span class="sxs-lookup"><span data-stu-id="02d1e-259">Small enhancements</span></span>

<span data-ttu-id="02d1e-260">[5,0 kilometre taşında `type-enhancement` etiketli sorunlar](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-260">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="02d1e-261">Geliştiriciler: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="02d1e-261">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="02d1e-262">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="02d1e-262">T-shirt size: L</span></span>

<span data-ttu-id="02d1e-263">Durum: devam ediyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-263">Status: In-progress</span></span>

<span data-ttu-id="02d1e-264">Yukarıda özetlenen daha büyük özelliklere ek olarak, 5,0 "kağıt-keser" düzeltmesini sağlamak için zamanlanan çok sayıda daha küçük geliştirmeler de sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="02d1e-264">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="02d1e-265">Bu geliştirmelerin birçoğu yukarıda özetlenen daha genel temalar tarafından da ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="02d1e-265">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="02d1e-266">Satır altı</span><span class="sxs-lookup"><span data-stu-id="02d1e-266">Below-the-line</span></span>

<span data-ttu-id="02d1e-267">[`consider-for-next-release`etiketli sorunlar](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="02d1e-267">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="02d1e-268">Bunlar, şu anda 5,0 sürümü için zamanlanmamış **olan hata** düzeltmeleri ve geliştirmeleridir, ancak yukarıdaki iş üzerinde yapılan ilerlemeye bağlı olarak, Esnetme hedefleri olarak bakacağız.</span><span class="sxs-lookup"><span data-stu-id="02d1e-268">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="02d1e-269">Ayrıca, planlama sırasında [en fazla oylanan sorunları](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) her zaman göz önünde bulundurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="02d1e-269">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="02d1e-270">Bu sorunlardan herhangi birini bir yayından kesmek her zaman çok önemlidir, ancak sahip olduğumuz kaynaklar için gerçekçi bir plana ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="02d1e-270">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="02d1e-271">Geribildirim</span><span class="sxs-lookup"><span data-stu-id="02d1e-271">Feedback</span></span>

<span data-ttu-id="02d1e-272">Planlamaya ilişkin geri bildiriminiz önemlidir.</span><span class="sxs-lookup"><span data-stu-id="02d1e-272">Your feedback on planning is important.</span></span> <span data-ttu-id="02d1e-273">Bir sorunun önemini belirtmenin en iyi yolu GitHub 'da söz konusu sorundan oylanmanız (thumbs-up).</span><span class="sxs-lookup"><span data-stu-id="02d1e-273">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="02d1e-274">Bu veriler daha sonra bir sonraki sürüm için [planlama işlemine](../release-planning.md) akış eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="02d1e-274">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
