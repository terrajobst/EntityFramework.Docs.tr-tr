---
title: Varlık Çerçeve Çekirdek 5.0 Planı
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 8b4ca32524869019c04d5a4d4d55967f68181cd7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136234"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="deed0-102">Varlık Çerçeve Çekirdek 5.0 Planı</span><span class="sxs-lookup"><span data-stu-id="deed0-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="deed0-103">[Planlama sürecinde](../release-planning.md)açıklandığı gibi, paydaşlardan EF Core 5.0 sürümü için geçici bir plan için girdi topladık.</span><span class="sxs-lookup"><span data-stu-id="deed0-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="deed0-104">Bu plan hala devam etmekte olan bir çalışmadır.</span><span class="sxs-lookup"><span data-stu-id="deed0-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="deed0-105">Buradaki hiçbir şey bağlılık değildir.</span><span class="sxs-lookup"><span data-stu-id="deed0-105">Nothing here is a commitment.</span></span> <span data-ttu-id="deed0-106">Bu plan, biz daha fazla bilgi olarak gelişecek bir başlangıç noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="deed0-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="deed0-107">Şu anda 5.0 için planlanmamış bazı şeyler çekilebilir.</span><span class="sxs-lookup"><span data-stu-id="deed0-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="deed0-108">Şu anda 5.0 için planlanan bazı şeyler dışarı punted alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="deed0-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="deed0-109">Sürüm numarası ve çıkış tarihi.</span><span class="sxs-lookup"><span data-stu-id="deed0-109">Version number and release date.</span></span>

<span data-ttu-id="deed0-110">EF Core 5.0 şu anda [.NET 5.0 ile aynı anda](https://devblogs.microsoft.com/dotnet/introducing-net-5/)piyasaya sürülmesi planlanıyor.</span><span class="sxs-lookup"><span data-stu-id="deed0-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="deed0-111">"5.0" sürümü .NET 5.0 ile hizalamak için seçildi.</span><span class="sxs-lookup"><span data-stu-id="deed0-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="deed0-112">Desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="deed0-112">Supported platforms</span></span>

<span data-ttu-id="deed0-113">EF Core 5.0'ın [bu platformların .NET Core ile yakınsaması](https://devblogs.microsoft.com/dotnet/introducing-net-5/)temel alınabilmek için herhangi bir .NET 5.0 platformunda çalışması planlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="deed0-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="deed0-114">Bu .NET Standart ve kullanılan gerçek TFM açısından ne anlama geliyor hala TBD olduğunu.</span><span class="sxs-lookup"><span data-stu-id="deed0-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="deed0-115">EF Core 5.0 .NET Framework üzerinde çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="deed0-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="deed0-116">Yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="deed0-116">Breaking changes</span></span>

<span data-ttu-id="deed0-117">EF Core 5.0 bazı kırılma değişiklikleri içerecektir, ancak bu EF Core 3.0 için olduğu gibi çok daha az şiddetli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="deed0-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="deed0-118">Amacımız, uygulamaların büyük çoğunluğunun kırılmadan güncellemesine izin vermektir.</span><span class="sxs-lookup"><span data-stu-id="deed0-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="deed0-119">Özellikle TPT desteği çevresinde veritabanı sağlayıcıları için bazı kırılma değişiklikleri olması beklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="deed0-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="deed0-120">Ancak, 5.0 için bir sağlayıcı güncelleştirmek için çalışma 3.0 için güncelleştirmek için gerekli olandan daha az olacağını bekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="deed0-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="deed0-121">Temalar</span><span class="sxs-lookup"><span data-stu-id="deed0-121">Themes</span></span>

<span data-ttu-id="deed0-122">EF Core 5.0'daki büyük yatırımların temelini oluşturacak birkaç ana alan veya tema çıkardık.</span><span class="sxs-lookup"><span data-stu-id="deed0-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="deed0-123">Çok-çok navigasyon özellikleri (aka "gezintileri atlamak")</span><span class="sxs-lookup"><span data-stu-id="deed0-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="deed0-124">Müşteri adayı @smitpatel geliştiriciler: ve@AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="deed0-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="deed0-125">#19003 [tarafından](https://github.com/aspnet/EntityFrameworkCore/issues/19003) izlenir</span><span class="sxs-lookup"><span data-stu-id="deed0-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="deed0-126">T-shirt boyutu: L</span><span class="sxs-lookup"><span data-stu-id="deed0-126">T-shirt size: L</span></span>

<span data-ttu-id="deed0-127">Durum: Devam ediyor</span><span class="sxs-lookup"><span data-stu-id="deed0-127">Status: In-progress</span></span>

<span data-ttu-id="deed0-128">Çok-to-çok GitHub biriktirme listesien [çok istenen özellik](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~ 407 oy) olduğunu.</span><span class="sxs-lookup"><span data-stu-id="deed0-128">Many-to-many is the [most requested feature](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 votes) on the GitHub backlog.</span></span>

<span data-ttu-id="deed0-129">Onların bütünüyle çok-çok ilişkiler için destek [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508)olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="deed0-129">Support for many-to-many relationships in their entirety is tracked as [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span></span> <span data-ttu-id="deed0-130">Bu üç ana alana ayrılabilir:</span><span class="sxs-lookup"><span data-stu-id="deed0-130">This can be broken down into three major areas:</span></span>

* <span data-ttu-id="deed0-131">Gezinme özelliklerini atlayın.</span><span class="sxs-lookup"><span data-stu-id="deed0-131">Skip navigation properties.</span></span> <span data-ttu-id="deed0-132">Bunlar, altyapı birleştirme tablosu varlığına başvurmadan, modelin sorgular vb. için kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="deed0-132">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span> <span data-ttu-id="deed0-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span><span class="sxs-lookup"><span data-stu-id="deed0-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span></span>
* <span data-ttu-id="deed0-134">Özellik-çanta varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="deed0-134">Property-bag entity types.</span></span> <span data-ttu-id="deed0-135">Bunlar, standart bir CLR türünün `Dictionary`(örn. ) varlık örnekleri için kullanılmasına izin verir, böylece her varlık türü için açık bir CLR türü gerekmez.</span><span class="sxs-lookup"><span data-stu-id="deed0-135">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span> <span data-ttu-id="deed0-136">(5.0 için stretch: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span><span class="sxs-lookup"><span data-stu-id="deed0-136">(Stretch for 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span></span>
* <span data-ttu-id="deed0-137">Çok-çok ilişkilerin kolay yapılandırma için Şeker.</span><span class="sxs-lookup"><span data-stu-id="deed0-137">Sugar for easy configuration of many-to-many relationships.</span></span> <span data-ttu-id="deed0-138">(5.0 için streç.)</span><span class="sxs-lookup"><span data-stu-id="deed0-138">(Stretch for 5.0.)</span></span>

<span data-ttu-id="deed0-139">Çok-çok destek isteyenler için en önemli engelleyicinin, birleştirme tablosuna atıfta bulunmaksızın, sorgular gibi iş mantığıyla "doğal" ilişkileri kullanamamak olduğuna inanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="deed0-139">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="deed0-140">Birleştirme tablosu varlık türü hala var olabilir, ancak iş mantığının önüne girmemelidir.</span><span class="sxs-lookup"><span data-stu-id="deed0-140">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="deed0-141">Bu nedenle 5.0 için atlama navigasyon özellikleri ele seçtik.</span><span class="sxs-lookup"><span data-stu-id="deed0-141">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="deed0-142">Şu anda çok-çok diğer parçaları EF Core 5.0 için bir streç hedef olarak takip edilmektedir.</span><span class="sxs-lookup"><span data-stu-id="deed0-142">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="deed0-143">Bu da şu anda 5.0 için planda olmadıkları anlamına geliyor, ancak işler yolunda giderse onları içeri çekmeyi umuyoruz.</span><span class="sxs-lookup"><span data-stu-id="deed0-143">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="deed0-144">Tablo başına tip (TPT) kalıtım eşleme</span><span class="sxs-lookup"><span data-stu-id="deed0-144">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="deed0-145">Müşteri adayı geliştirici:@AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="deed0-145">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="deed0-146">İzlenen [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="deed0-146">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="deed0-147">T-shirt boyutu: XL</span><span class="sxs-lookup"><span data-stu-id="deed0-147">T-shirt size: XL</span></span>

<span data-ttu-id="deed0-148">Durum: Devam ediyor</span><span class="sxs-lookup"><span data-stu-id="deed0-148">Status: In-progress</span></span>

<span data-ttu-id="deed0-149">Hem çok istenen bir özellik (~254 oy; genel olarak 3.</span><span class="sxs-lookup"><span data-stu-id="deed0-149">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="deed0-150">Bunun veritabanı sağlayıcıları için kırılmaya neden olmasını bekliyoruz, ancak bunlar 3.0 için gereken değişikliklerden çok daha az ciddi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="deed0-150">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="deed0-151">Filtrelenmiş Ekle</span><span class="sxs-lookup"><span data-stu-id="deed0-151">Filtered Include</span></span>

<span data-ttu-id="deed0-152">Müşteri adayı geliştirici:@maumar</span><span class="sxs-lookup"><span data-stu-id="deed0-152">Lead developer: @maumar</span></span>

<span data-ttu-id="deed0-153">#1833 [tarafından](https://github.com/aspnet/EntityFrameworkCore/issues/1833) izlenir</span><span class="sxs-lookup"><span data-stu-id="deed0-153">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="deed0-154">T-shirt boyutu: M</span><span class="sxs-lookup"><span data-stu-id="deed0-154">T-shirt size: M</span></span>

<span data-ttu-id="deed0-155">Durum: Devam ediyor</span><span class="sxs-lookup"><span data-stu-id="deed0-155">Status: In-progress</span></span>

<span data-ttu-id="deed0-156">Filtreli Include, çok fazla istenen bir özelliktir (~317 oy; genel olarak 2.) ve şu anda model düzeyinde filtreler veya daha karmaşık sorgular gerektiren birçok senaryonun engelini kaldıracağına veya kolaylaştıracağına inandığımız bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="deed0-156">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="deed0-157">ToTable, ToQuery, ToView, FromSql, vb. rasyonalize edin</span><span class="sxs-lookup"><span data-stu-id="deed0-157">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="deed0-158">Müşteri adayı @maumar geliştiriciler: ve@smitpatel</span><span class="sxs-lookup"><span data-stu-id="deed0-158">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="deed0-159">İzlendi [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="deed0-159">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="deed0-160">T-shirt boyutu: L</span><span class="sxs-lookup"><span data-stu-id="deed0-160">T-shirt size: L</span></span>

<span data-ttu-id="deed0-161">Durum: Devam ediyor</span><span class="sxs-lookup"><span data-stu-id="deed0-161">Status: In-progress</span></span>

<span data-ttu-id="deed0-162">Ham SQL, anahtarsız türleri ve ilgili alanları destekleme yolunda önceki sürümlerde ilerleme kaydettik.</span><span class="sxs-lookup"><span data-stu-id="deed0-162">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="deed0-163">Ancak, her şeyin bir bütün olarak birlikte çalışması nda hem boşluklar hem de tutarsızlıklar vardır.</span><span class="sxs-lookup"><span data-stu-id="deed0-163">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="deed0-164">5.0 için amaç bunları düzeltmek ve farklı türde varlıkları ve bunların ilişkili sorgularını ve veritabanı yapılarını tanımlamak, geçiş yapmak ve kullanmak için iyi bir deneyim oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="deed0-164">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="deed0-165">Bu, derlenmiş sorgu API güncelleştirmelerini de içerebilir.</span><span class="sxs-lookup"><span data-stu-id="deed0-165">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="deed0-166">Şu anda sahip olduğumuz bazı işlevler insanları hızlı bir şekilde başarısızlık çukurlarına sokabilecek kadar izin verici olduğundan, bu öğenin bazı uygulama düzeyinde kırılmadeğişikliklerine neden olabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="deed0-166">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="deed0-167">Büyük olasılıkla bu işlevlerin bir kısmını, bunun yerine ne yapacağımıza ilişkin rehberlikle birlikte engelleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="deed0-167">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="deed0-168">Genel sorgu geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="deed0-168">General query enhancements</span></span>

<span data-ttu-id="deed0-169">Müşteri adayı @smitpatel geliştiriciler: ve@maumar</span><span class="sxs-lookup"><span data-stu-id="deed0-169">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="deed0-170">[5.0 `area-query` kilometre taşında etiketlenmiş sorunlarla](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+) izlenir</span><span class="sxs-lookup"><span data-stu-id="deed0-170">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="deed0-171">T-shirt boyutu: XL</span><span class="sxs-lookup"><span data-stu-id="deed0-171">T-shirt size: XL</span></span>

<span data-ttu-id="deed0-172">Durum: Devam ediyor</span><span class="sxs-lookup"><span data-stu-id="deed0-172">Status: In-progress</span></span>

<span data-ttu-id="deed0-173">Sorgu çeviri kodu EF Core 3.0 için kapsamlı olarak yeniden yazılmıştır.</span><span class="sxs-lookup"><span data-stu-id="deed0-173">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="deed0-174">Sorgu kodu genellikle bu nedenle çok daha sağlam bir durumdadır.</span><span class="sxs-lookup"><span data-stu-id="deed0-174">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="deed0-175">5.0 için, TPT'yi desteklemek ve gezinme özelliklerini atlamak için gerekenler dışında büyük sorgu değişiklikleri yapmayı planlamiyoruz.</span><span class="sxs-lookup"><span data-stu-id="deed0-175">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="deed0-176">Ancak 3,0'lık elden geçirmeden geriye kalan bazı teknik borçları düzeltmek için hala önemli çalışmalar gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="deed0-176">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="deed0-177">Ayrıca, genel sorgu deneyimini daha da geliştirmek için birçok hatayı düzeltmeyi ve küçük geliştirmeler uygulamayı planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="deed0-177">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="deed0-178">Geçişler ve dağıtım deneyimi</span><span class="sxs-lookup"><span data-stu-id="deed0-178">Migrations and deployment experience</span></span>

<span data-ttu-id="deed0-179">Müşteri adayı geliştiriciler:@bricelam</span><span class="sxs-lookup"><span data-stu-id="deed0-179">Lead developers: @bricelam</span></span>

<span data-ttu-id="deed0-180">[#19587](https://github.com/dotnet/efcore/issues/19587) tarafından izlenir</span><span class="sxs-lookup"><span data-stu-id="deed0-180">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="deed0-181">T-shirt boyutu: L</span><span class="sxs-lookup"><span data-stu-id="deed0-181">T-shirt size: L</span></span>

<span data-ttu-id="deed0-182">Durum: Devam ediyor</span><span class="sxs-lookup"><span data-stu-id="deed0-182">Status: In-progress</span></span>

<span data-ttu-id="deed0-183">Şu anda, birçok geliştirici uygulama başlangıç zamanında veritabanlarını geçirin.</span><span class="sxs-lookup"><span data-stu-id="deed0-183">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="deed0-184">Bu kolay, ancak önerilmez çünkü:</span><span class="sxs-lookup"><span data-stu-id="deed0-184">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="deed0-185">Birden çok iş parçacığı/işlem/sunucu veritabanını aynı anda geçirmeye çalışabilir</span><span class="sxs-lookup"><span data-stu-id="deed0-185">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="deed0-186">Bu olurken uygulamalar tutarsız duruma erişmeye çalışabilir</span><span class="sxs-lookup"><span data-stu-id="deed0-186">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="deed0-187">Genellikle şema değiştirmek için veritabanı izinleri uygulama yürütme için verilmemelidir</span><span class="sxs-lookup"><span data-stu-id="deed0-187">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="deed0-188">Bir şeyler ters giderse temiz bir duruma dönmek zordur.</span><span class="sxs-lookup"><span data-stu-id="deed0-188">It's hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="deed0-189">Dağıtım zamanında veritabanını geçirmenin kolay bir yolunu sağlayan daha iyi bir deneyim sunmak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="deed0-189">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="deed0-190">Bu gerekir:</span><span class="sxs-lookup"><span data-stu-id="deed0-190">This should:</span></span>

* <span data-ttu-id="deed0-191">Linux, Mac ve Windows üzerinde çalışma</span><span class="sxs-lookup"><span data-stu-id="deed0-191">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="deed0-192">Komut satırında iyi bir deneyim yaşayın</span><span class="sxs-lookup"><span data-stu-id="deed0-192">Be a good experience on the command line</span></span>
* <span data-ttu-id="deed0-193">Kapsayıcılarla destek senaryoları</span><span class="sxs-lookup"><span data-stu-id="deed0-193">Support scenarios with containers</span></span>
* <span data-ttu-id="deed0-194">Yaygın olarak kullanılan gerçek dünya dağıtım araçları/akışları ile çalışma</span><span class="sxs-lookup"><span data-stu-id="deed0-194">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="deed0-195">En azından Visual Studio'ya entegre edin</span><span class="sxs-lookup"><span data-stu-id="deed0-195">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="deed0-196">Sonuç, EF Core'da (örneğin, SQLite'deki daha iyi Göçler) ve sadece EF'nin ötesine geçen uçtan uca deneyimleri geliştirmek için diğer ekiplerle daha uzun vadeli işbirlikleri yle birlikte pek çok küçük iyileştirme olacaktır.</span><span class="sxs-lookup"><span data-stu-id="deed0-196">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="deed0-197">EF Core platformları deneyimi</span><span class="sxs-lookup"><span data-stu-id="deed0-197">EF Core platforms experience</span></span> 

<span data-ttu-id="deed0-198">Müşteri adayı @roji geliştiriciler: ve@bricelam</span><span class="sxs-lookup"><span data-stu-id="deed0-198">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="deed0-199">#19588 [tarafından](https://github.com/dotnet/efcore/issues/19588) izlenir</span><span class="sxs-lookup"><span data-stu-id="deed0-199">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="deed0-200">T-shirt boyutu: L</span><span class="sxs-lookup"><span data-stu-id="deed0-200">T-shirt size: L</span></span>

<span data-ttu-id="deed0-201">Durum: Başlamadı</span><span class="sxs-lookup"><span data-stu-id="deed0-201">Status: Not started</span></span>

<span data-ttu-id="deed0-202">Geleneksel MVC benzeri web uygulamalarında EF Core'u kullanmak için iyi bir kılavuzumuz vardır.</span><span class="sxs-lookup"><span data-stu-id="deed0-202">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="deed0-203">Diğer platformlar ve uygulama modelleri için kılavuz eksik veya güncel değil.</span><span class="sxs-lookup"><span data-stu-id="deed0-203">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="deed0-204">EF Core 5.0 için, EF Core'u aşağıdakilerle birlikte kullanma deneyimini araştırmayı, geliştirmeyi ve belgelemayı planlıyoruz:</span><span class="sxs-lookup"><span data-stu-id="deed0-204">For EF Core 5.0, we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="deed0-205">Blazor</span><span class="sxs-lookup"><span data-stu-id="deed0-205">Blazor</span></span>
* <span data-ttu-id="deed0-206">Xamarin, AOT/linker hikayesini kullanmak da dahil olmak üzere</span><span class="sxs-lookup"><span data-stu-id="deed0-206">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="deed0-207">WinForms/WPF/WinUI ve muhtemelen diğer U.I.</span><span class="sxs-lookup"><span data-stu-id="deed0-207">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="deed0-208">Çerçeve</span><span class="sxs-lookup"><span data-stu-id="deed0-208">frameworks</span></span>

<span data-ttu-id="deed0-209">Bu, EF Core'da, sadece EF'nin ötesine geçen uçtan uca deneyimleri geliştirmek için diğer ekiplerle rehberlik ve uzun vadeli işbirlikleriyle birlikte pek çok küçük iyileştirme olması muhtemeldir.</span><span class="sxs-lookup"><span data-stu-id="deed0-209">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="deed0-210">Bakmayı planladığımız belirli alanlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="deed0-210">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="deed0-211">Geçişler gibi EF takımlarını kullanma deneyimi de dahil olmak üzere dağıtım</span><span class="sxs-lookup"><span data-stu-id="deed0-211">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="deed0-212">Xamarin ve Blazor ve muhtemelen diğerleri de dahil olmak üzere uygulama modelleri,</span><span class="sxs-lookup"><span data-stu-id="deed0-212">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="deed0-213">Mekansal deneyim ve tablo yeniden de dahil olmak üzere SQLite deneyimleri,</span><span class="sxs-lookup"><span data-stu-id="deed0-213">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="deed0-214">AOT ve bağlantı deneyimleri</span><span class="sxs-lookup"><span data-stu-id="deed0-214">AOT and linking experiences</span></span>
* <span data-ttu-id="deed0-215">Perf sayaçları da dahil olmak üzere tanılama entegrasyonu</span><span class="sxs-lookup"><span data-stu-id="deed0-215">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="deed0-216">Performans</span><span class="sxs-lookup"><span data-stu-id="deed0-216">Performance</span></span>

<span data-ttu-id="deed0-217">Müşteri adayı geliştirici:@roji</span><span class="sxs-lookup"><span data-stu-id="deed0-217">Lead developer: @roji</span></span>

<span data-ttu-id="deed0-218">[5.0 `area-perf` kilometre taşında etiketlenmiş sorunlarla](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+) izlenir</span><span class="sxs-lookup"><span data-stu-id="deed0-218">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="deed0-219">T-shirt boyutu: L</span><span class="sxs-lookup"><span data-stu-id="deed0-219">T-shirt size: L</span></span>

<span data-ttu-id="deed0-220">Durum: Devam ediyor</span><span class="sxs-lookup"><span data-stu-id="deed0-220">Status: In-progress</span></span>

<span data-ttu-id="deed0-221">EF Core için, performans kriterleri paketimizi geliştirmeyi ve çalışma süresine yönelik performans iyileştirmeleri yapmayı planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="deed0-221">For EF Core, we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="deed0-222">Buna ek olarak, 3.0 sürüm döngüsü sırasında prototipedildi yeni ADO.NET toplu API tamamlamak için planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="deed0-222">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="deed0-223">Ayrıca ADO.NET katmanında, Npgsql sağlayıcısına ek performans iyileştirmeleri planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="deed0-223">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="deed0-224">Bu çalışmanın bir parçası olarak, uygun ADO.NET/EF Çekirdek performans sayaçları ve diğer tanılamaeklemeyi planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="deed0-224">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="deed0-225">Mimari/katkıda bulunan dokümantasyon</span><span class="sxs-lookup"><span data-stu-id="deed0-225">Architectural/contributor documentation</span></span>

<span data-ttu-id="deed0-226">Müşteri adayı belgeleyici:@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="deed0-226">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="deed0-227">İzlenen [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="deed0-227">Tracked by [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="deed0-228">T-shirt boyutu: L</span><span class="sxs-lookup"><span data-stu-id="deed0-228">T-shirt size: L</span></span>

<span data-ttu-id="deed0-229">Durum: Devam ediyor</span><span class="sxs-lookup"><span data-stu-id="deed0-229">Status: In-progress</span></span>

<span data-ttu-id="deed0-230">Buradaki fikir, EF Core'un iç lerinde neler olup bittiğini daha kolay anlamaktır.</span><span class="sxs-lookup"><span data-stu-id="deed0-230">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="deed0-231">Bu, EF Core kullanan herkes için yararlı olabilir, ancak birincil motivasyon, dış insanların aşağıdakileri yapmasını kolaylaştırmaktır:</span><span class="sxs-lookup"><span data-stu-id="deed0-231">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="deed0-232">EF Core koduna katkıda bulunun</span><span class="sxs-lookup"><span data-stu-id="deed0-232">Contribute to the EF Core code</span></span>
* <span data-ttu-id="deed0-233">Veritabanı sağlayıcıları oluşturma</span><span class="sxs-lookup"><span data-stu-id="deed0-233">Create database providers</span></span>
* <span data-ttu-id="deed0-234">Diğer uzantıları oluşturma</span><span class="sxs-lookup"><span data-stu-id="deed0-234">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="deed0-235">Microsoft.Data.Sqlite belgeleri</span><span class="sxs-lookup"><span data-stu-id="deed0-235">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="deed0-236">Müşteri adayı belgeleyici:@bricelam</span><span class="sxs-lookup"><span data-stu-id="deed0-236">Lead documenter: @bricelam</span></span>

<span data-ttu-id="deed0-237">İzlenen [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="deed0-237">Tracked by [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="deed0-238">T-shirt boyutu: M</span><span class="sxs-lookup"><span data-stu-id="deed0-238">T-shirt size: M</span></span>

<span data-ttu-id="deed0-239">Durum: Tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="deed0-239">Status: Completed.</span></span> <span data-ttu-id="deed0-240">Yeni belgeler [Microsoft dokümanlar sitesinde yayında.](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli)</span><span class="sxs-lookup"><span data-stu-id="deed0-240">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="deed0-241">EF Ekibi ayrıca Microsoft.Data.Sqlite ADO.NET sağlayıcısının da sahibidir.</span><span class="sxs-lookup"><span data-stu-id="deed0-241">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="deed0-242">Bu sağlayıcıyı 5.0 sürümün bir parçası olarak tam olarak belgeletmayı planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="deed0-242">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="deed0-243">Genel dokümantasyon</span><span class="sxs-lookup"><span data-stu-id="deed0-243">General documentation</span></span>

<span data-ttu-id="deed0-244">Müşteri adayı belgeleyici:@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="deed0-244">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="deed0-245">[5.0 kilometre taşındaki dokümanreposundaki sorunlar](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+) tarafından izlenir</span><span class="sxs-lookup"><span data-stu-id="deed0-245">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="deed0-246">T-shirt boyutu: L</span><span class="sxs-lookup"><span data-stu-id="deed0-246">T-shirt size: L</span></span>

<span data-ttu-id="deed0-247">Durum: Devam ediyor</span><span class="sxs-lookup"><span data-stu-id="deed0-247">Status: In-progress</span></span>

<span data-ttu-id="deed0-248">3.0 ve 3.1 sürümleri için belgeleri güncelleme sürecindeyiz.</span><span class="sxs-lookup"><span data-stu-id="deed0-248">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="deed0-249">Biz de üzerinde çalışıyoruz:</span><span class="sxs-lookup"><span data-stu-id="deed0-249">We are also working on:</span></span>
  * <span data-ttu-id="deed0-250">Daha ulaşılabilir/takip edilebilen daha kolay hale getirmek için dokümanların elden geçirilmesi</span><span class="sxs-lookup"><span data-stu-id="deed0-250">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="deed0-251">İşleri bulmayı kolaylaştırmak ve çapraz referanslar eklemek için dokümanların yeniden düzenlenmesi</span><span class="sxs-lookup"><span data-stu-id="deed0-251">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="deed0-252">Varolan dokümanlara daha fazla ayrıntı ve açıklama ekleme</span><span class="sxs-lookup"><span data-stu-id="deed0-252">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="deed0-253">Örnekleri güncelleme ve daha fazla örnek ekleme</span><span class="sxs-lookup"><span data-stu-id="deed0-253">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="deed0-254">Hataları düzeltme</span><span class="sxs-lookup"><span data-stu-id="deed0-254">Fixing bugs</span></span>

<span data-ttu-id="deed0-255">[5.0 `type-bug` kilometre taşında etiketlenmiş sorunlarla](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+) izlenir</span><span class="sxs-lookup"><span data-stu-id="deed0-255">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="deed0-256">Geliştiriciler: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, ,@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="deed0-256">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="deed0-257">T-shirt boyutu: L</span><span class="sxs-lookup"><span data-stu-id="deed0-257">T-shirt size: L</span></span>

<span data-ttu-id="deed0-258">Durum: Devam ediyor</span><span class="sxs-lookup"><span data-stu-id="deed0-258">Status: In-progress</span></span>

<span data-ttu-id="deed0-259">Yazma sırasında, 5.0 sürümünde (62'si zaten sabitlenmiş) düzeltilmesi gereken 135 hata var, ancak yukarıdaki _Genel sorgu geliştirmeleri_ bölümüyle önemli bir çakışma söz vardır.</span><span class="sxs-lookup"><span data-stu-id="deed0-259">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="deed0-260">Gelen oran (bir kilometre taşı çalışma olarak sona erer sorunları) 3.0 sürümü boyunca ayda yaklaşık 23 sorunları oldu.</span><span class="sxs-lookup"><span data-stu-id="deed0-260">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="deed0-261">Bunların hepsinin 5.0'da düzeltilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="deed0-261">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="deed0-262">Kaba bir tahmin olarak, 5,0 zaman diliminde 150 sorunu daha düzeltmeyi planlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="deed0-262">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="deed0-263">Küçük geliştirmeler</span><span class="sxs-lookup"><span data-stu-id="deed0-263">Small enhancements</span></span>

<span data-ttu-id="deed0-264">[5.0 `type-enhancement` kilometre taşında etiketlenmiş sorunlarla](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+) izlenir</span><span class="sxs-lookup"><span data-stu-id="deed0-264">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="deed0-265">Geliştiriciler: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, ,@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="deed0-265">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="deed0-266">T-shirt boyutu: L</span><span class="sxs-lookup"><span data-stu-id="deed0-266">T-shirt size: L</span></span>

<span data-ttu-id="deed0-267">Durum: Devam ediyor</span><span class="sxs-lookup"><span data-stu-id="deed0-267">Status: In-progress</span></span>

<span data-ttu-id="deed0-268">Yukarıda özetlenen büyük özelliklere ek olarak, biz de "kağıt kesim" düzeltmek için 5.0 için planlanan birçok küçük iyileştirmeler var.</span><span class="sxs-lookup"><span data-stu-id="deed0-268">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="deed0-269">Bu geliştirmelerin çoğunun yukarıda özetlenen daha genel temalar tarafından da karşılandığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="deed0-269">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="deed0-270">Satır Altı</span><span class="sxs-lookup"><span data-stu-id="deed0-270">Below-the-line</span></span>

<span data-ttu-id="deed0-271">[Etiketli sorunlartarafından `consider-for-next-release` ](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release) izlenen</span><span class="sxs-lookup"><span data-stu-id="deed0-271">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="deed0-272">Bunlar, **not** şu anda 5.0 sürümü için zamanlanmamış hata düzeltmeleri ve geliştirmelerdir, ancak yukarıdaki çalışmada kaydedilen ilerlemeye bağlı olarak streç hedefler olarak bakacağız.</span><span class="sxs-lookup"><span data-stu-id="deed0-272">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="deed0-273">Buna ek olarak, biz her zaman planlama [en çok oy sorunları](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) düşünün.</span><span class="sxs-lookup"><span data-stu-id="deed0-273">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="deed0-274">Bu sorunlardan herhangi birini bir sürümden kesmek her zaman acı vericidir, ancak sahip olduğumuz kaynaklar için gerçekçi bir plana ihtiyacımız vardır.</span><span class="sxs-lookup"><span data-stu-id="deed0-274">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="deed0-275">Geri Bildirim</span><span class="sxs-lookup"><span data-stu-id="deed0-275">Feedback</span></span>

<span data-ttu-id="deed0-276">Planlama hakkındaki görüşleriniz önemlidir.</span><span class="sxs-lookup"><span data-stu-id="deed0-276">Your feedback on planning is important.</span></span> <span data-ttu-id="deed0-277">Bir sorunun önemini belirtmenin en iyi yolu GitHub'da bu sorun için oy kullanmaktır (thumbs-up).</span><span class="sxs-lookup"><span data-stu-id="deed0-277">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="deed0-278">Bu veriler daha sonra bir sonraki sürüm için [planlama sürecine](../release-planning.md) beslenir.</span><span class="sxs-lookup"><span data-stu-id="deed0-278">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
