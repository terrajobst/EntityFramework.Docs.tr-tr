---
title: Entity Framework Core 5,0 planlaması
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: c5b7300c61c2f668b6f9393ae51bf9ebddf330a7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417878"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="6061f-102">Entity Framework Core 5,0 planlaması</span><span class="sxs-lookup"><span data-stu-id="6061f-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="6061f-103">[Planlama sürecinde](../release-planning.md)açıklandığı gibi, hissedarlardan EF Core 5,0 sürümü için geçici bir plana toplanan girişleri topladık.</span><span class="sxs-lookup"><span data-stu-id="6061f-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="6061f-104">Bu plan hala devam eden bir çalışmadır.</span><span class="sxs-lookup"><span data-stu-id="6061f-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="6061f-105">Taahhüt aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="6061f-105">Nothing here is a commitment.</span></span> <span data-ttu-id="6061f-106">Bu plan, daha fazla öğrendiğimiz için geliştireceğiz bir başlangıç noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="6061f-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="6061f-107">5,0 için şu anda planlanmayan bazı şeyler, çekime alabilir.</span><span class="sxs-lookup"><span data-stu-id="6061f-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="6061f-108">5,0 için şu anda planlanmış bazı şeyler kullanıma hazır olabilir.</span><span class="sxs-lookup"><span data-stu-id="6061f-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="6061f-109">Sürüm numarası ve sürüm tarihi.</span><span class="sxs-lookup"><span data-stu-id="6061f-109">Version number and release date.</span></span>

<span data-ttu-id="6061f-110">EF Core 5,0 şu anda sürüm için [.net 5,0 ile aynı zamanda](https://devblogs.microsoft.com/dotnet/introducing-net-5/)zamanlandı.</span><span class="sxs-lookup"><span data-stu-id="6061f-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="6061f-111">.NET 5,0 ile hizalamak için "5,0" sürümü seçildi.</span><span class="sxs-lookup"><span data-stu-id="6061f-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="6061f-112">Desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="6061f-112">Supported platforms</span></span>

<span data-ttu-id="6061f-113">EF Core 5,0, [Bu platformların .NET Core 'a yakınsamasını](https://devblogs.microsoft.com/dotnet/introducing-net-5/)temel alan herhangi bir .NET 5,0 platformunda çalıştırılmak üzere planlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="6061f-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="6061f-114">Bu .NET Standard, ne anlama gelir ve kullanılan gerçek tfd, hala TBD 'dir.</span><span class="sxs-lookup"><span data-stu-id="6061f-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="6061f-115">EF Core 5,0 .NET Framework çalıştırmayacak.</span><span class="sxs-lookup"><span data-stu-id="6061f-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="6061f-116">Yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="6061f-116">Breaking changes</span></span>

<span data-ttu-id="6061f-117">EF Core 5,0 bazı önemli değişiklikler içerir, ancak bu durum EF Core 3,0 ' den çok daha az önem altına alınır.</span><span class="sxs-lookup"><span data-stu-id="6061f-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="6061f-118">Hedefimiz, uygulamanın büyük çoğunluğunun bozmadan güncelleştirilmesine izin vermektir.</span><span class="sxs-lookup"><span data-stu-id="6061f-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="6061f-119">Veritabanı sağlayıcılarının, özellikle de TPT desteğinin çevresinde bazı önemli değişiklikler olacağı için bu değer beklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="6061f-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="6061f-120">Bununla birlikte, 5,0 için bir sağlayıcıyı güncelleştirme işinin 3,0 için güncelleştirilmesi gerekenden daha az olacağını umyoruz.</span><span class="sxs-lookup"><span data-stu-id="6061f-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="6061f-121">Temalar</span><span class="sxs-lookup"><span data-stu-id="6061f-121">Themes</span></span>

<span data-ttu-id="6061f-122">EF Core 5,0 ' de büyük yatırımların temelini oluşturacak birkaç önemli alanı veya temaları ayıkladık.</span><span class="sxs-lookup"><span data-stu-id="6061f-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="6061f-123">Çoka çok gezinti özellikleri (bir. k. a "gezintilerini atla")</span><span class="sxs-lookup"><span data-stu-id="6061f-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="6061f-124">Lider geliştiricileri: @smitpatel ve @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="6061f-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="6061f-125">[#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="6061f-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="6061f-126">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="6061f-126">T-shirt size: L</span></span>

<span data-ttu-id="6061f-127">Durum: devam ediyor</span><span class="sxs-lookup"><span data-stu-id="6061f-127">Status: In-progress</span></span>

<span data-ttu-id="6061f-128">Birden çok-çok, GitHub biriktirme listesindeki [en çok istenen özelliktir](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~ 407 oylardır).</span><span class="sxs-lookup"><span data-stu-id="6061f-128">Many-to-many is the [most requested feature](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 votes) on the GitHub backlog.</span></span>

<span data-ttu-id="6061f-129">Tam olarak çok-çok ilişkilerini destekler [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508)olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="6061f-129">Support for many-to-many relationships in their entirety is tracked as [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span></span> <span data-ttu-id="6061f-130">Bu, üç ana alana ayrılabilir:</span><span class="sxs-lookup"><span data-stu-id="6061f-130">This can be broken down into three major areas:</span></span>

* <span data-ttu-id="6061f-131">Gezinme özelliklerini atlayın.</span><span class="sxs-lookup"><span data-stu-id="6061f-131">Skip navigation properties.</span></span> <span data-ttu-id="6061f-132">Bunlar, modelin, temel alınan JOIN tablosu varlığına başvurmaksızın sorgular, vb. için kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="6061f-132">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span> <span data-ttu-id="6061f-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span><span class="sxs-lookup"><span data-stu-id="6061f-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span></span>
* <span data-ttu-id="6061f-134">Özellik paketi varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="6061f-134">Property-bag entity types.</span></span> <span data-ttu-id="6061f-135">Bunlar, varlık örnekleri için, her varlık türü için açık bir CLR türü gerektirmeyen standart bir CLR türüne (ör. `Dictionary`) olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="6061f-135">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span> <span data-ttu-id="6061f-136">(5,0 için uzat: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span><span class="sxs-lookup"><span data-stu-id="6061f-136">(Stretch for 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span></span>
* <span data-ttu-id="6061f-137">Çoka çok ilişkilerin kolay yapılandırması için cukr.</span><span class="sxs-lookup"><span data-stu-id="6061f-137">Sugar for easy configuration of many-to-many relationships.</span></span> <span data-ttu-id="6061f-138">(5,0 için uzatın.)</span><span class="sxs-lookup"><span data-stu-id="6061f-138">(Stretch for 5.0.)</span></span>

<span data-ttu-id="6061f-139">Çok-çok desteği olan en önemli engelleyici, JOIN tablosuna başvurulmadan, sorgular gibi iş mantığından "doğal" ilişkileri kullanmadığımızı düşüntik.</span><span class="sxs-lookup"><span data-stu-id="6061f-139">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="6061f-140">JOIN tablosu varlık türü yine de mevcut olabilir, ancak iş mantığı gibi kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="6061f-140">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="6061f-141">Bu nedenle 5,0 için gezinme özelliklerini atla ' nın üstesinden gelmeyi seçtik.</span><span class="sxs-lookup"><span data-stu-id="6061f-141">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="6061f-142">Şu anda, çoktan çoğa diğer bölümleri EF Core 5,0 için bir Esnetme hedefi olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="6061f-142">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="6061f-143">Bu, şu anda 5,0 için planda olmadıkları anlamına gelir, ancak şeyler ' ın içinde çekmesini umuyoruz.</span><span class="sxs-lookup"><span data-stu-id="6061f-143">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="6061f-144">Tablo/tür (TPT) devralma eşleme</span><span class="sxs-lookup"><span data-stu-id="6061f-144">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="6061f-145">Lider geliştiricisi: @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="6061f-145">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="6061f-146">[#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="6061f-146">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="6061f-147">Tişörlü Boyut: XL</span><span class="sxs-lookup"><span data-stu-id="6061f-147">T-shirt size: XL</span></span>

<span data-ttu-id="6061f-148">Durum: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="6061f-148">Status: Not started</span></span>

<span data-ttu-id="6061f-149">Yüksek düzeyde istenmiş bir Özellik (~ 254 oy ve 3. Genel) olduğundan ve genel .NET 5 planının temel yapısı için uygun olan bazı düşük düzey değişiklikler gerektirdiğinden TPT yapıyoruz.</span><span class="sxs-lookup"><span data-stu-id="6061f-149">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="6061f-150">Bunun, 3,0 için gereken değişikliklerden çok daha az ciddi olmasına rağmen, bu, veritabanı sağlayıcılarının değişikliklere neden olduğunu umyoruz.</span><span class="sxs-lookup"><span data-stu-id="6061f-150">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="6061f-151">Filtrelenen ekleme</span><span class="sxs-lookup"><span data-stu-id="6061f-151">Filtered Include</span></span>

<span data-ttu-id="6061f-152">Lider geliştiricisi: @maumar</span><span class="sxs-lookup"><span data-stu-id="6061f-152">Lead developer: @maumar</span></span>

<span data-ttu-id="6061f-153">[#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="6061f-153">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="6061f-154">Tişörlü Boyut: a</span><span class="sxs-lookup"><span data-stu-id="6061f-154">T-shirt size: M</span></span>

<span data-ttu-id="6061f-155">Durum: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="6061f-155">Status: Not started</span></span>

<span data-ttu-id="6061f-156">Filtrelenmiş dahil, çok büyük bir iş miktarı olmayan ve şu anda model düzeyi filtreler veya daha karmaşık sorgular gerektiren çok sayıda senaryonun engellemesini veya daha kolay olduğunu düşünmemiz için yüksek düzeyde istenmiş bir özelliktir (~ 317 oy; 2. Genel).</span><span class="sxs-lookup"><span data-stu-id="6061f-156">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="6061f-157">Dtiontotable, ToQuery, ToView, FromSql vb.</span><span class="sxs-lookup"><span data-stu-id="6061f-157">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="6061f-158">Lider geliştiricileri: @maumar ve @smitpatel</span><span class="sxs-lookup"><span data-stu-id="6061f-158">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="6061f-159">[#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="6061f-159">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="6061f-160">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="6061f-160">T-shirt size: L</span></span>

<span data-ttu-id="6061f-161">Durum: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="6061f-161">Status: Not started</span></span>

<span data-ttu-id="6061f-162">Ham SQL, anahtarsız türlerini ve ilgili alanı desteklemeye yönelik önceki sürümlerde ilerleme yaptık.</span><span class="sxs-lookup"><span data-stu-id="6061f-162">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="6061f-163">Ancak, her şeyin bir bütün olarak birlikte çalıştıkları şekilde hem boşluklar hem de tutarsızlıklar vardır.</span><span class="sxs-lookup"><span data-stu-id="6061f-163">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="6061f-164">5,0 için amaç bunları çözmektir ve farklı varlık türlerini ve bunlarla ilişkili sorguları ve veritabanı yapılarını tanımlamak, geçirmek ve kullanmak için iyi bir deneyim oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="6061f-164">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="6061f-165">Bu, derlenmiş sorgu API 'SI güncelleştirmelerini de içerebilir.</span><span class="sxs-lookup"><span data-stu-id="6061f-165">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="6061f-166">Bu öğe, şu anda kişilerin hata Pits 'e hızlı bir şekilde yol açabileceğini sağlayan çok fazla izin olduğundan, bu öğenin bazı uygulama düzeyi değişikliklere neden olabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="6061f-166">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="6061f-167">Bunun yerine ne yapacaklarla ilgili rehberlik ile bu işlevlerden bazılarını engellemeyi büyük olasılıkla yapacağız.</span><span class="sxs-lookup"><span data-stu-id="6061f-167">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="6061f-168">Genel sorgu geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="6061f-168">General query enhancements</span></span>

<span data-ttu-id="6061f-169">Lider geliştiricileri: @smitpatel ve @maumar</span><span class="sxs-lookup"><span data-stu-id="6061f-169">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="6061f-170">[5,0 kilometre taşında `area-query` etiketli sorunlar](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="6061f-170">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="6061f-171">Tişörlü Boyut: XL</span><span class="sxs-lookup"><span data-stu-id="6061f-171">T-shirt size: XL</span></span>

<span data-ttu-id="6061f-172">Durum: devam ediyor</span><span class="sxs-lookup"><span data-stu-id="6061f-172">Status: In-progress</span></span>

<span data-ttu-id="6061f-173">Sorgu çevirisi kodu EF Core 3,0 için kapsamlı olarak yeniden yazıldı.</span><span class="sxs-lookup"><span data-stu-id="6061f-173">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="6061f-174">Sorgu kodu genellikle bu nedenle çok daha sağlam durumda.</span><span class="sxs-lookup"><span data-stu-id="6061f-174">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="6061f-175">5,0 için, TPT 'yi desteklemek ve gezinme özelliklerini atlamak için gerekenlerden büyük sorgu değişiklikleri yapmayı planlamadık.</span><span class="sxs-lookup"><span data-stu-id="6061f-175">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="6061f-176">Bununla birlikte, 3,0 fazla yerine kalan bazı teknik borcu gidermek için hala önemli bir iş vardır.</span><span class="sxs-lookup"><span data-stu-id="6061f-176">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="6061f-177">Ayrıca, genel sorgu deneyimini geliştirmek için birçok hatayı gidermeyi planlıyoruz ve küçük geliştirmeler uygulayacağız.</span><span class="sxs-lookup"><span data-stu-id="6061f-177">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="6061f-178">Geçişler ve dağıtım deneyimi</span><span class="sxs-lookup"><span data-stu-id="6061f-178">Migrations and deployment experience</span></span>

<span data-ttu-id="6061f-179">Lider geliştiricileri: @bricelam</span><span class="sxs-lookup"><span data-stu-id="6061f-179">Lead developers: @bricelam</span></span>

<span data-ttu-id="6061f-180">[#19587](https://github.com/dotnet/efcore/issues/19587) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="6061f-180">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="6061f-181">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="6061f-181">T-shirt size: L</span></span>

<span data-ttu-id="6061f-182">Durum: devam ediyor</span><span class="sxs-lookup"><span data-stu-id="6061f-182">Status: In-progress</span></span>

<span data-ttu-id="6061f-183">Şu anda birçok geliştirici, veritabanlarını uygulama başlatma zamanına geçirecektir.</span><span class="sxs-lookup"><span data-stu-id="6061f-183">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="6061f-184">Bu kolay ancak önerilmez, çünkü:</span><span class="sxs-lookup"><span data-stu-id="6061f-184">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="6061f-185">Birden çok iş parçacığı/işlem/sunucu veritabanını aynı anda geçirmeye çalışabilir</span><span class="sxs-lookup"><span data-stu-id="6061f-185">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="6061f-186">Uygulamalar, bu durumdayken tutarsız duruma erişmeye çalışabilir</span><span class="sxs-lookup"><span data-stu-id="6061f-186">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="6061f-187">Genellikle Şemayı değiştirme veritabanı izinleri uygulama yürütmesi için verilmemelidir</span><span class="sxs-lookup"><span data-stu-id="6061f-187">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="6061f-188">Bir sorun varsa temiz bir duruma geri dönmek zordur</span><span class="sxs-lookup"><span data-stu-id="6061f-188">It's hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="6061f-189">Veritabanını dağıtım zamanında geçirmek için kolay bir yol sağlayan daha iyi bir deneyim sunmak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="6061f-189">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="6061f-190">Şunları yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="6061f-190">This should:</span></span>

* <span data-ttu-id="6061f-191">Linux, Mac ve Windows üzerinde çalışma</span><span class="sxs-lookup"><span data-stu-id="6061f-191">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="6061f-192">Komut satırında iyi bir deneyim olun</span><span class="sxs-lookup"><span data-stu-id="6061f-192">Be a good experience on the command line</span></span>
* <span data-ttu-id="6061f-193">Kapsayıcılarla destek senaryoları</span><span class="sxs-lookup"><span data-stu-id="6061f-193">Support scenarios with containers</span></span>
* <span data-ttu-id="6061f-194">Yaygın olarak kullanılan gerçek dünya dağıtım araçlarıyla ve akışlarla çalışma</span><span class="sxs-lookup"><span data-stu-id="6061f-194">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="6061f-195">En az Visual Studio ile tümleştirin</span><span class="sxs-lookup"><span data-stu-id="6061f-195">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="6061f-196">Sonuçta, yalnızca EF 'in ötesinde çok sayıda küçük EF Core geliştirmeler (örneğin, SQLite üzerinde daha iyi geçişler), diğer ekiplere yönelik kılavuzlarla daha fazla geçiş ve daha uzun süreli İşbirlikten yararlanmaktır.</span><span class="sxs-lookup"><span data-stu-id="6061f-196">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="6061f-197">EF Core platformları deneyimi</span><span class="sxs-lookup"><span data-stu-id="6061f-197">EF Core platforms experience</span></span> 

<span data-ttu-id="6061f-198">Lider geliştiricileri: @roji ve @bricelam</span><span class="sxs-lookup"><span data-stu-id="6061f-198">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="6061f-199">[#19588](https://github.com/dotnet/efcore/issues/19588) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="6061f-199">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="6061f-200">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="6061f-200">T-shirt size: L</span></span>

<span data-ttu-id="6061f-201">Durum: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="6061f-201">Status: Not started</span></span>

<span data-ttu-id="6061f-202">Geleneksel MVC benzeri Web uygulamalarında EF Core kullanmak için iyi bir kılavuzluk sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="6061f-202">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="6061f-203">Diğer platformlar ve uygulama modelleriyle ilgili rehberlik eksik ya da güncel değil.</span><span class="sxs-lookup"><span data-stu-id="6061f-203">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="6061f-204">EF Core 5,0 için, ile EF Core kullanma deneyimini araştırmaya, iyileştirmenize ve belgeleyecek şekilde planlıyoruz:</span><span class="sxs-lookup"><span data-stu-id="6061f-204">For EF Core 5.0, we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="6061f-205">Blazor</span><span class="sxs-lookup"><span data-stu-id="6061f-205">Blazor</span></span>
* <span data-ttu-id="6061f-206">AOT/bağlayıcı hikayesini kullanma dahil Xamarin</span><span class="sxs-lookup"><span data-stu-id="6061f-206">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="6061f-207">WinForms/WPF/WinUI ve muhtemelen diğer U.I.</span><span class="sxs-lookup"><span data-stu-id="6061f-207">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="6061f-208">çerçeveleri</span><span class="sxs-lookup"><span data-stu-id="6061f-208">frameworks</span></span>

<span data-ttu-id="6061f-209">Bu, EF Core çok küçük geliştirmeler, diğer ekiplere yönelik kılavuz ve uzun süreli ortak çalışmalarla birlikte, yalnızca EF 'in ötesine geçen uçtan uca deneyimler geliştirmeye olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="6061f-209">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="6061f-210">Göz atadığımız belirli bölgeler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="6061f-210">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="6061f-211">Geçiş için gibi EF araçları kullanma deneyimini de içeren dağıtım</span><span class="sxs-lookup"><span data-stu-id="6061f-211">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="6061f-212">Xamarin ve Blazor dahil olmak üzere uygulama modelleri ve büyük olasılıkla diğerleri</span><span class="sxs-lookup"><span data-stu-id="6061f-212">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="6061f-213">Uzamsal deneyim ve tablo da dahil olmak üzere SQLite deneyimler</span><span class="sxs-lookup"><span data-stu-id="6061f-213">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="6061f-214">AOT ve bağlama deneyimleri</span><span class="sxs-lookup"><span data-stu-id="6061f-214">AOT and linking experiences</span></span>
* <span data-ttu-id="6061f-215">Performans sayaçlarını içeren tanılama tümleştirmesi</span><span class="sxs-lookup"><span data-stu-id="6061f-215">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="6061f-216">Performans</span><span class="sxs-lookup"><span data-stu-id="6061f-216">Performance</span></span>

<span data-ttu-id="6061f-217">Lider geliştiricisi: @roji</span><span class="sxs-lookup"><span data-stu-id="6061f-217">Lead developer: @roji</span></span>

<span data-ttu-id="6061f-218">[5,0 kilometre taşında `area-perf` etiketli sorunlar](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="6061f-218">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="6061f-219">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="6061f-219">T-shirt size: L</span></span>

<span data-ttu-id="6061f-220">Durum: devam ediyor</span><span class="sxs-lookup"><span data-stu-id="6061f-220">Status: In-progress</span></span>

<span data-ttu-id="6061f-221">EF Core için, performans kıyaslamamızı iyileştirmemiz ve çalışma zamanında yönlendirilmiş performans geliştirmeleri yapmanız planlanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="6061f-221">For EF Core, we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="6061f-222">Ayrıca, 3,0 yayın çevrimi sırasında prototip oluşturulan yeni ADO.NET toplu işlem API 'sini tamamlamayı planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="6061f-222">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="6061f-223">Ayrıca, ADO.NET katmanında Npgsql sağlayıcısında ek performans geliştirmeleri planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="6061f-223">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="6061f-224">Bu çalışmanın bir parçası olarak, uygun şekilde ADO.NET/EF Core performans sayaçlarını ve diğer tanılamayı eklemeyi de planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="6061f-224">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="6061f-225">Mimari/katkıda bulunan belgeleri</span><span class="sxs-lookup"><span data-stu-id="6061f-225">Architectural/contributor documentation</span></span>

<span data-ttu-id="6061f-226">Müşteri adayı belge girme: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="6061f-226">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="6061f-227">[#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="6061f-227">Tracked by [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="6061f-228">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="6061f-228">T-shirt size: L</span></span>

<span data-ttu-id="6061f-229">Durum: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="6061f-229">Status: Not started</span></span>

<span data-ttu-id="6061f-230">Buradaki fikir, EF Core iç yapıları hakkında daha kolay anlaşılır hale getirmek için.</span><span class="sxs-lookup"><span data-stu-id="6061f-230">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="6061f-231">Bu, EF Core kullanan herkes için yararlı olabilir, ancak birincil mosyon, dış kişilerin şunları yapmasını kolaylaştırır:</span><span class="sxs-lookup"><span data-stu-id="6061f-231">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="6061f-232">EF Core koduna katkıda bulunun</span><span class="sxs-lookup"><span data-stu-id="6061f-232">Contribute to the EF Core code</span></span>
* <span data-ttu-id="6061f-233">Veritabanı sağlayıcıları oluşturma</span><span class="sxs-lookup"><span data-stu-id="6061f-233">Create database providers</span></span>
* <span data-ttu-id="6061f-234">Diğer uzantıları oluşturma</span><span class="sxs-lookup"><span data-stu-id="6061f-234">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="6061f-235">Microsoft. Data. SQLite belgeleri</span><span class="sxs-lookup"><span data-stu-id="6061f-235">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="6061f-236">Müşteri adayı belge girme: @bricelam</span><span class="sxs-lookup"><span data-stu-id="6061f-236">Lead documenter: @bricelam</span></span>

<span data-ttu-id="6061f-237">[#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="6061f-237">Tracked by [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="6061f-238">Tişörlü Boyut: a</span><span class="sxs-lookup"><span data-stu-id="6061f-238">T-shirt size: M</span></span>

<span data-ttu-id="6061f-239">Durum: tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="6061f-239">Status: Completed.</span></span> <span data-ttu-id="6061f-240">Yeni belgeler [Microsoft docs sitesinde canlı](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="6061f-240">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="6061f-241">EF ekibi, Microsoft. Data. SQLite ADO.NET sağlayıcısına de sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6061f-241">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="6061f-242">Bu sağlayıcıyı 5,0 sürümünün bir parçası olarak tam olarak belgelemek planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="6061f-242">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="6061f-243">Genel belgeler</span><span class="sxs-lookup"><span data-stu-id="6061f-243">General documentation</span></span>

<span data-ttu-id="6061f-244">Müşteri adayı belge girme: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="6061f-244">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="6061f-245">[5,0 kilometre taşında docs deposunda bulunan sorunlara](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+) göre izleniyor</span><span class="sxs-lookup"><span data-stu-id="6061f-245">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="6061f-246">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="6061f-246">T-shirt size: L</span></span>

<span data-ttu-id="6061f-247">Durum: devam ediyor</span><span class="sxs-lookup"><span data-stu-id="6061f-247">Status: In-progress</span></span>

<span data-ttu-id="6061f-248">3,0 ve 3,1 sürümleri için belgeleri güncelleştirme sürecimiz zaten var.</span><span class="sxs-lookup"><span data-stu-id="6061f-248">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="6061f-249">Ayrıca şu şekilde çalışıyoruz:</span><span class="sxs-lookup"><span data-stu-id="6061f-249">We are also working on:</span></span>
  * <span data-ttu-id="6061f-250">Daha fazla ulaşılabilir/daha kolay hale getirmek için kullanmaya başlama belgelerine yönelik fazla mesafe</span><span class="sxs-lookup"><span data-stu-id="6061f-250">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="6061f-251">Çapraz başvuruları bulmayı ve eklemeyi kolaylaştırmak için belgeleri yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="6061f-251">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="6061f-252">Mevcut docs için daha fazla ayrıntı ve ayrıntılı açıklamalar ekleme</span><span class="sxs-lookup"><span data-stu-id="6061f-252">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="6061f-253">Örnekleri güncelleştirme ve daha fazla örnek ekleme</span><span class="sxs-lookup"><span data-stu-id="6061f-253">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="6061f-254">Hataları düzeltme</span><span class="sxs-lookup"><span data-stu-id="6061f-254">Fixing bugs</span></span>

<span data-ttu-id="6061f-255">[5,0 kilometre taşında `type-bug` etiketli sorunlar](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="6061f-255">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="6061f-256">Geliştiriciler: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="6061f-256">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="6061f-257">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="6061f-257">T-shirt size: L</span></span>

<span data-ttu-id="6061f-258">Durum: devam ediyor</span><span class="sxs-lookup"><span data-stu-id="6061f-258">Status: In-progress</span></span>

<span data-ttu-id="6061f-259">Yazma sırasında, 5,0 sürümünde (zaten düzeltilmiş olan 62) düzeltilen 135 hata değerlendirildi, ancak yukarıdaki _genel sorgu geliştirmeleri_ bölümünde önemli bir çakışma var.</span><span class="sxs-lookup"><span data-stu-id="6061f-259">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="6061f-260">Gelen ücret (bir kilometre taşında çalışan olarak biten sorunlar) 3,0 sürümü boyunca ayda yaklaşık 23 sorun olmuştur.</span><span class="sxs-lookup"><span data-stu-id="6061f-260">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="6061f-261">Bunların tümünün 5,0 ' de düzeltilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="6061f-261">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="6061f-262">Kabaca bir tahmin olarak 5,0 zaman çerçevesinde ek 150 sorunları gidermeyi planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="6061f-262">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="6061f-263">Küçük geliştirmeler</span><span class="sxs-lookup"><span data-stu-id="6061f-263">Small enhancements</span></span>

<span data-ttu-id="6061f-264">[5,0 kilometre taşında `type-enhancement` etiketli sorunlar](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="6061f-264">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="6061f-265">Geliştiriciler: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="6061f-265">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="6061f-266">Tişörlü Boyut: L</span><span class="sxs-lookup"><span data-stu-id="6061f-266">T-shirt size: L</span></span>

<span data-ttu-id="6061f-267">Durum: devam ediyor</span><span class="sxs-lookup"><span data-stu-id="6061f-267">Status: In-progress</span></span>

<span data-ttu-id="6061f-268">Yukarıda özetlenen daha büyük özelliklere ek olarak, 5,0 "kağıt-keser" düzeltmesini sağlamak için zamanlanan çok sayıda daha küçük geliştirmeler de sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="6061f-268">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="6061f-269">Bu geliştirmelerin birçoğu yukarıda özetlenen daha genel temalar tarafından da ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="6061f-269">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="6061f-270">Satır altı</span><span class="sxs-lookup"><span data-stu-id="6061f-270">Below-the-line</span></span>

<span data-ttu-id="6061f-271">[`consider-for-next-release`etiketli sorunlar](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release) tarafından izleniyor</span><span class="sxs-lookup"><span data-stu-id="6061f-271">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="6061f-272">Bunlar, şu anda 5,0 sürümü için zamanlanmamış **olan hata** düzeltmeleri ve geliştirmeleridir, ancak yukarıdaki iş üzerinde yapılan ilerlemeye bağlı olarak, Esnetme hedefleri olarak bakacağız.</span><span class="sxs-lookup"><span data-stu-id="6061f-272">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="6061f-273">Ayrıca, planlama sırasında [en fazla oylanan sorunları](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) her zaman göz önünde bulundurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6061f-273">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="6061f-274">Bu sorunlardan herhangi birini bir yayından kesmek her zaman çok önemlidir, ancak sahip olduğumuz kaynaklar için gerçekçi bir plana ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="6061f-274">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="6061f-275">Geri Bildirim</span><span class="sxs-lookup"><span data-stu-id="6061f-275">Feedback</span></span>

<span data-ttu-id="6061f-276">Planlamaya ilişkin geri bildiriminiz önemlidir.</span><span class="sxs-lookup"><span data-stu-id="6061f-276">Your feedback on planning is important.</span></span> <span data-ttu-id="6061f-277">Bir sorunun önemini belirtmenin en iyi yolu GitHub 'da söz konusu sorundan oylanmanız (thumbs-up).</span><span class="sxs-lookup"><span data-stu-id="6061f-277">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="6061f-278">Bu veriler daha sonra bir sonraki sürüm için [planlama işlemine](../release-planning.md) akış eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="6061f-278">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
