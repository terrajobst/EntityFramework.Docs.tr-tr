---
title: Günlük sağlayıcısı etkileyen değişiklikler - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: ee73940e3c0030b76e73438b1852cc29ebeadb45
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998364"
---
# <a name="provider-impacting-changes"></a><span data-ttu-id="e4ea3-102">Sağlayıcı etkileyen değişiklikler</span><span class="sxs-lookup"><span data-stu-id="e4ea3-102">Provider-impacting changes</span></span>

<span data-ttu-id="e4ea3-103">Bu sayfa, çekme isteklerine tepki vermek için diğer veritabanı sağlayıcıları yazarları gerektirebilecek EF Core depoda yapılan bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="e4ea3-103">This page contains links to pull requests made on the EF Core repo that may require authors of other database providers to react.</span></span> <span data-ttu-id="e4ea3-104">Amaç bir başlangıç noktası mevcut üçüncü taraf veritabanı sağlayıcıları yazarları için sağlayıcının yeni bir sürüme güncelleştirirken sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="e4ea3-104">The intention is to provide a starting point for authors of existing third-party database providers when updating their provider to a new version.</span></span>

<span data-ttu-id="e4ea3-105">2.2 2.1 değişikliklerle bu günlük başlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="e4ea3-105">We are starting this log with changes from 2.1 to 2.2.</span></span> <span data-ttu-id="e4ea3-106">2.1 önce kullandığımız [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) ve [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) bizim sorunlar ve çekme istekleri etiketleri.</span><span class="sxs-lookup"><span data-stu-id="e4ea3-106">Prior to 2.1 we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our issues and pull requests.</span></span>

### <a name="21-----22"></a><span data-ttu-id="e4ea3-107">2.1 2.2---></span><span class="sxs-lookup"><span data-stu-id="e4ea3-107">2.1 ---> 2.2</span></span>

#### <a name="test-only-changes"></a><span data-ttu-id="e4ea3-108">Yalnızca test değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="e4ea3-108">Test-only changes</span></span>

* <span data-ttu-id="e4ea3-109">https://github.com/aspnet/EntityFrameworkCore/pull/12057 -Özelleştirilebilir SQL sınırlayıcı testlerinde izin ver</span><span class="sxs-lookup"><span data-stu-id="e4ea3-109">https://github.com/aspnet/EntityFrameworkCore/pull/12057 - Allow customizable SQL delimeters in tests</span></span>
  * <span data-ttu-id="e4ea3-110">Katı olmayan kayan nokta karşılaştırmalar BuiltInDataTypesTestBase içinde izin değişiklikleri test etmek</span><span class="sxs-lookup"><span data-stu-id="e4ea3-110">Test changes that allow non-strict floating point comparisons in BuiltInDataTypesTestBase</span></span>
  * <span data-ttu-id="e4ea3-111">Farklı bir SQL sınırlayıcı ile yeniden kullanılmasının gerekmesi sorgu testlerine izin vermek test değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="e4ea3-111">Test changes that allow query tests to be re-used with different SQL delimeters</span></span>
* <span data-ttu-id="e4ea3-112">https://github.com/aspnet/EntityFrameworkCore/pull/12072 -İlişkisel belirtimi test DbFunction testleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e4ea3-112">https://github.com/aspnet/EntityFrameworkCore/pull/12072 - Add DbFunction tests to the relational specification tests</span></span>
  * <span data-ttu-id="e4ea3-113">Örneğin bu testleri tüm veritabanı sağlayıcıları karşı çalıştırabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="e4ea3-113">Such that these tests can be run against all database providers</span></span>
* <span data-ttu-id="e4ea3-114">https://github.com/aspnet/EntityFrameworkCore/pull/12362 -Zaman uyumsuz test temizleme</span><span class="sxs-lookup"><span data-stu-id="e4ea3-114">https://github.com/aspnet/EntityFrameworkCore/pull/12362 - Async test cleanup</span></span>
  * <span data-ttu-id="e4ea3-115">Kaldırma `Wait` çağrıları, zaman uyumsuz kapatın ve bazı test yöntemlerini yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="e4ea3-115">Remove `Wait` calls, unneeded async, and renamed some test methods</span></span>
* <span data-ttu-id="e4ea3-116">https://github.com/aspnet/EntityFrameworkCore/pull/12666 -Günlük kaydı ve test altyapısı birleştirin</span><span class="sxs-lookup"><span data-stu-id="e4ea3-116">https://github.com/aspnet/EntityFrameworkCore/pull/12666 - Unify logging test infrastructure</span></span>
  * <span data-ttu-id="e4ea3-117">Eklenen `CreateListLoggerFactory` ve tepki vermek için bu testleri kullanarak sağlayıcılarının gerektiren bazı önceki günlük altyapısı kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="e4ea3-117">Added `CreateListLoggerFactory` and removed some previous logging infrastructure, which will require providers using these tests to react</span></span>
* <span data-ttu-id="e4ea3-118">https://github.com/aspnet/EntityFrameworkCore/pull/12500 -Zaman uyumlu ve zaman uyumsuz olarak daha fazla sorgu testleri çalıştırma</span><span class="sxs-lookup"><span data-stu-id="e4ea3-118">https://github.com/aspnet/EntityFrameworkCore/pull/12500 - Run more query tests both synchronously and asynchronously</span></span>
  * <span data-ttu-id="e4ea3-119">Test adları ve hesaba katacak şekilde değişti, hangi tepki vermek için bu testleri kullanarak sağlayıcılarının gerektirir</span><span class="sxs-lookup"><span data-stu-id="e4ea3-119">Test names and factoring has changed, which will require providers using these tests to react</span></span>
* <span data-ttu-id="e4ea3-120">https://github.com/aspnet/EntityFrameworkCore/pull/12766 -Gezintiler ComplexNavigations modelinde yeniden adlandırma</span><span class="sxs-lookup"><span data-stu-id="e4ea3-120">https://github.com/aspnet/EntityFrameworkCore/pull/12766 - Renaming navigations in the ComplexNavigations model</span></span>
  * <span data-ttu-id="e4ea3-121">Bu testleri kullanarak sağlayıcılarının react gerekebilir</span><span class="sxs-lookup"><span data-stu-id="e4ea3-121">Providers using these tests may need to react</span></span>
* <span data-ttu-id="e4ea3-122">https://github.com/aspnet/EntityFrameworkCore/pull/12141 -Laboratuvardaki işlevsel testlere disposing yerine havuza bağlamını döndürür</span><span class="sxs-lookup"><span data-stu-id="e4ea3-122">https://github.com/aspnet/EntityFrameworkCore/pull/12141 - Return the context to the pool instead of disposing in functional tests</span></span>
  * <span data-ttu-id="e4ea3-123">Bu değişiklik, bazı test tepki vermek sağlayıcıları gerektirebilecek yeniden düzenleme içerir</span><span class="sxs-lookup"><span data-stu-id="e4ea3-123">This change includes some test refactoring which may require providers to react</span></span>


#### <a name="test-and-product-code-changes"></a><span data-ttu-id="e4ea3-124">Test ve ürün kodu değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="e4ea3-124">Test and product code changes</span></span>

* <span data-ttu-id="e4ea3-125">https://github.com/aspnet/EntityFrameworkCore/pull/12109 -RelationalTypeMapping.Clone yöntemleri birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e4ea3-125">https://github.com/aspnet/EntityFrameworkCore/pull/12109 - Consolidate RelationalTypeMapping.Clone methods</span></span>
  * <span data-ttu-id="e4ea3-126">2.1 bir basitleştirme türetilmiş sınıflarda için izin verilen RelationalTypeMapping için değişiklikler.</span><span class="sxs-lookup"><span data-stu-id="e4ea3-126">Changes in 2.1 to the RelationalTypeMapping allowed for a simplification in derived classes.</span></span> <span data-ttu-id="e4ea3-127">İnanıyoruz yok sağlayıcıları bu bozucu, ancak sağlayıcıları yararlanabilir bu değişiklik, türetilen türde de eşleme sınıfları.</span><span class="sxs-lookup"><span data-stu-id="e4ea3-127">We don't believe this was breaking to providers, but providers can take advantage of this change in their derived type mapping classes.</span></span>
* <span data-ttu-id="e4ea3-128">https://github.com/aspnet/EntityFrameworkCore/pull/12069 -Etiketli veya adlandırılmış sorguları</span><span class="sxs-lookup"><span data-stu-id="e4ea3-128">https://github.com/aspnet/EntityFrameworkCore/pull/12069 - Tagged or named queries</span></span>
  * <span data-ttu-id="e4ea3-129">LINQ sorguları etiketleme ve bu etiketleri SQL açıklamaları olarak göstermek zorunda için altyapı ekler.</span><span class="sxs-lookup"><span data-stu-id="e4ea3-129">Adds infrastructure for tagging LINQ queries and having those tags show up as comments in the SQL.</span></span> <span data-ttu-id="e4ea3-130">Bu SQL oluşturma tepki vermek sağlayıcıları gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="e4ea3-130">This may require providers to react in SQL generation.</span></span>
