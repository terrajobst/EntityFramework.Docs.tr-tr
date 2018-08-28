---
title: Araçlar ve uzantılar - EF Core
author: ErikEJ
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e9f9a6cbbceeb0379ddb5588b564b0d2a962795f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995519"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="39c45-102">EF Core Araçlar ve uzantılar</span><span class="sxs-lookup"><span data-stu-id="39c45-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="39c45-103">Araçlar ve uzantılar için Entity Framework Core ek işlevsellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="39c45-103">Tools and extensions provide additional functionality for Entity Framework Core.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="39c45-104">Uzantıları çeşitli kaynaklardan tarafından oluşturulur ve Entity Framework Core projesinin bir parçası korunur değil.</span><span class="sxs-lookup"><span data-stu-id="39c45-104">Extensions are built by a variety of sources and not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="39c45-105">Bir üçüncü taraf uzantı dikkate alındığında, kalite, lisanslama, uyumluluk, destek vb. gereksinimlerinizi karşıladıklarından emin olmak için değerlendirilecek emin olun.</span><span class="sxs-lookup"><span data-stu-id="39c45-105">When considering a third party extension, be sure to evaluate quality, licensing, compatibility, support, etc. to ensure they meet your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="39c45-106">Araçlar</span><span class="sxs-lookup"><span data-stu-id="39c45-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="39c45-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="39c45-107">LLBLGen Pro</span></span>

<span data-ttu-id="39c45-108">LLBLGen Pro çözüm Entity Framework ve Entity Framework Core desteğiyle birlikte modeling bir varlıktır.</span><span class="sxs-lookup"><span data-stu-id="39c45-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="39c45-109">Kolayca varlık modelinizi tanımlayın ve veritabanınıza ilk kullanarak veritabanı eşleme sağlar veya sorgu hemen yazma başlangıç yapmanıza yardımcı olacak ilk olarak, model.</span><span class="sxs-lookup"><span data-stu-id="39c45-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="39c45-110">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="39c45-110">website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="39c45-111">Devart varlık Geliştirici</span><span class="sxs-lookup"><span data-stu-id="39c45-111">Devart Entity Developer</span></span>

<span data-ttu-id="39c45-112">Varlık, ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access ve LINQ to SQL için güçlü bir ORM Tasarımcısı geliştiricidir.</span><span class="sxs-lookup"><span data-stu-id="39c45-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="39c45-113">Model-First kullanabilirsiniz ve ORM modelinizi tasarlamak ve C# veya Visual Basic .NET kodunu oluşturmak için veritabanı ilk yaklaşır.</span><span class="sxs-lookup"><span data-stu-id="39c45-113">You can use  Model-First and Database-First approaches to design your ORM model and generate C# or Visual Basic .NET code for it.</span></span> <span data-ttu-id="39c45-114">ORM modelleri oluşturmaya yönelik yeni bir yaklaşım sunar, üretkenliği artırıyor ve veritabanı uygulamaların geliştirilmesini kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="39c45-114">It introduces new approaches for designing ORM models, boosts productivity, and facilitates the development of database applications.</span></span>

[<span data-ttu-id="39c45-115">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="39c45-115">website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="39c45-116">EF Core güç araçları</span><span class="sxs-lookup"><span data-stu-id="39c45-116">EF Core Power Tools</span></span>

<span data-ttu-id="39c45-117">Visual Studio 2017 + uzantısı.</span><span class="sxs-lookup"><span data-stu-id="39c45-117">Visual Studio 2017+ extension.</span></span> <span data-ttu-id="39c45-118">Varolan bir veritabanına veya SQL Server veritabanı projesi tersine mühendislik DbContext ve POCO sınıfların görselleştirin ve çeşitli yollarla, DbContext inceleyin.</span><span class="sxs-lookup"><span data-stu-id="39c45-118">You can reverse engineer of DbContext and POCO classes from an existing database or SQL Server Database project, and visualize and inspect your DbContext in various ways.</span></span>

[<span data-ttu-id="39c45-119">GitHub wiki</span><span class="sxs-lookup"><span data-stu-id="39c45-119">GitHub wiki</span></span>](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

## <a name="extensions"></a><span data-ttu-id="39c45-120">Uzantıları</span><span class="sxs-lookup"><span data-stu-id="39c45-120">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="39c45-121">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="39c45-121">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="39c45-122">Bir eklenti için otomatik olarak veri kaydını desteklemek Microsoft.EntityFrameworkCore geçmişi değiştirir.</span><span class="sxs-lookup"><span data-stu-id="39c45-122">A plugin for Microsoft.EntityFrameworkCore to support automatically recording data changes history.</span></span>

[<span data-ttu-id="39c45-123">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-123">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="39c45-124">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="39c45-124">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="39c45-125">Zaman uyumsuz destek ekleyen Microsoft.EntityFrameworkCore için dinamik LINQ uzantıları</span><span class="sxs-lookup"><span data-stu-id="39c45-125">Dynamic Linq extensions for Microsoft.EntityFrameworkCore which adds Async support</span></span>

 [<span data-ttu-id="39c45-126">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-126">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a><span data-ttu-id="39c45-127">EFCore.Practices</span><span class="sxs-lookup"><span data-stu-id="39c45-127">EFCore.Practices</span></span>

<span data-ttu-id="39c45-128">Test destekleyen bir API – N + 1 sorgular için taramak için küçük bir çerçeve dahil olmak üzere bazı iyi ya da en iyi uygulamaları yakalamak çalışır.</span><span class="sxs-lookup"><span data-stu-id="39c45-128">Attempt to capture some good or best practices in an API that supports testing – including a small framework to scan for N+1 queries.</span></span>

[<span data-ttu-id="39c45-129">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-129">GitHub repository</span></span>](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="39c45-130">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="39c45-130">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="39c45-131">İkinci düzey önbelleğe alma kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="39c45-131">Second Level Caching Library.</span></span> <span data-ttu-id="39c45-132">İkinci düzey önbelleğe alma sorgu bir önbellektir.</span><span class="sxs-lookup"><span data-stu-id="39c45-132">Second level caching is a query cache.</span></span> <span data-ttu-id="39c45-133">Böylece aynı EF komutları, verileri alır, önbellek yerine yeniden veritabanına karşı yürütme EF komutlarının sonuçlarını önbellekte depolanır.</span><span class="sxs-lookup"><span data-stu-id="39c45-133">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span>

[<span data-ttu-id="39c45-134">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-134">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a><span data-ttu-id="39c45-135">Detached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="39c45-135">Detached.EntityFramework</span></span>

<span data-ttu-id="39c45-136">Yükler ve tüm ayrılmış varlık grafikleri (alt varlıklar ve liste varlığı) kaydeder.</span><span class="sxs-lookup"><span data-stu-id="39c45-136">Loads and saves entire detached entity graphs (the entity with their child entities and lists).</span></span> <span data-ttu-id="39c45-137">İlham [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span><span class="sxs-lookup"><span data-stu-id="39c45-137">Inspired by [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span></span> <span data-ttu-id="39c45-138">Ayrıca bazı eklentiler için simplificate denetim ve sayfalandırma gibi bazı yinelenen görevleri ekleyin olur.</span><span class="sxs-lookup"><span data-stu-id="39c45-138">The idea is also add some plugins to simplificate some repetitive tasks, like auditing and pagination.</span></span>

[<span data-ttu-id="39c45-139">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-139">GitHub repository</span></span>](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="39c45-140">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="39c45-140">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="39c45-141">Birincil anahtarı (bileşik anahtarlar dahil) herhangi bir varlığa bir sözlük olarak alın.</span><span class="sxs-lookup"><span data-stu-id="39c45-141">Retrieve the primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="39c45-142">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-142">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a><span data-ttu-id="39c45-143">EntityFrameworkCore.Rx</span><span class="sxs-lookup"><span data-stu-id="39c45-143">EntityFrameworkCore.Rx</span></span>

<span data-ttu-id="39c45-144">Sık erişimli gözlemlenenler Entity Framework varlık için reaktif uzantısı sarmalayıcıları.</span><span class="sxs-lookup"><span data-stu-id="39c45-144">Reactive extension wrappers for hot observables of Entity Framework entities.</span></span>

[<span data-ttu-id="39c45-145">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-145">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a><span data-ttu-id="39c45-146">EntityFrameworkCore.Triggers</span><span class="sxs-lookup"><span data-stu-id="39c45-146">EntityFrameworkCore.Triggers</span></span>

<span data-ttu-id="39c45-147">Ekleme, güncelleştirme ve olayları Sil içeren varlıklarınız için Tetikleyiciler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="39c45-147">Add triggers to your entities with insert, update, and delete events.</span></span> <span data-ttu-id="39c45-148">Her üç olaylar vardır: önce sonra ve başarısızlık durumunda.</span><span class="sxs-lookup"><span data-stu-id="39c45-148">There are three events for each: before, after, and upon failure.</span></span>

[<span data-ttu-id="39c45-149">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-149">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="39c45-150">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="39c45-150">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="39c45-151">Türü belirlenmiş OriginalValue, varlık özelliklerini erişin.</span><span class="sxs-lookup"><span data-stu-id="39c45-151">Get typed access to the OriginalValue of your entity properties.</span></span> <span data-ttu-id="39c45-152">Basit ve karmaşık özellikler desteklenir, gezinti ve koleksiyonların değildir.</span><span class="sxs-lookup"><span data-stu-id="39c45-152">Simple and complex properties are supported, navigation/collections are not.</span></span>

[<span data-ttu-id="39c45-153">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-153">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="39c45-154">Geco</span><span class="sxs-lookup"><span data-stu-id="39c45-154">Geco</span></span>

<span data-ttu-id="39c45-155">Geco bir ters Model Oluşturucu Çoğullaştırma/Singularization ve C# 6.0 ilişkilendirilmiş dizelerle tabanlı ve.Net Core üzerinde çalışan düzenlenebilir şablonlar için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="39c45-155">Geco provides a Reverse Model generator with support for Pluralization/Singularization and editable templates based on C# 6.0 interpolated strings and running on .Net Core.</span></span> <span data-ttu-id="39c45-156">Ayrıca bir çekirdek betiği Oluşturucu SQL birleştirme betikleri ve bir betik Çalıştırıcısı ile sağlar.</span><span class="sxs-lookup"><span data-stu-id="39c45-156">It also provides an Seed script generator with SQL Merge scripts and an script runner.</span></span>

[<span data-ttu-id="39c45-157">Github deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-157">Github repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="39c45-158">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="39c45-158">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="39c45-159">LinqKit.Microsoft.EntityFrameworkCore LINQ için uzantıları SQL ve EntityFrameworkCore ileri kullanıcılar için ücretsiz bir kümesidir.</span><span class="sxs-lookup"><span data-stu-id="39c45-159">LinqKit.Microsoft.EntityFrameworkCore is a free set of extensions for LINQ to SQL and EntityFrameworkCore power users.</span></span> <span data-ttu-id="39c45-160">İle Include(...) ve IDbAsync desteği.</span><span class="sxs-lookup"><span data-stu-id="39c45-160">With Include(...) and IDbAsync support.</span></span>

[<span data-ttu-id="39c45-161">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-161">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="39c45-162">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="39c45-162">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="39c45-163">NeinLinq.EntityFrameworkCore İşlevler, bunları null için güvenli ve yapı dinamik sorgular yapma sorguları, yeniden yazma yeniden yalnızca küçük bir alt kümesini .NET işlevleri destekleyen LINQ sağlayıcıları gibi Entity Framework kullanarak için faydalı uzantılar sağlar. çevrilebilir doğrulamaları ve Seçici kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="39c45-163">NeinLinq.EntityFrameworkCore provides helpful extensions for using LINQ providers such as Entity Framework that support only a minor subset of .NET functions, reusing functions, rewriting queries, even making them null-safe, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="39c45-164">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-164">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="39c45-165">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="39c45-165">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="39c45-166">Havuz, iş birimi desenleri ve birden çok veritabanı ile birlikte dağıtılmış işlem desteklenen desteklemek Microsoft.EntityFrameworkCore bir eklenti.</span><span class="sxs-lookup"><span data-stu-id="39c45-166">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple database with distributed transaction supported.</span></span>

[<span data-ttu-id="39c45-167">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-167">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a><span data-ttu-id="39c45-168">EntityFramework.LazyLoading</span><span class="sxs-lookup"><span data-stu-id="39c45-168">EntityFramework.LazyLoading</span></span>

<span data-ttu-id="39c45-169">EF Core 1.1 yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="39c45-169">Lazy Loading for EF Core 1.1</span></span>

[<span data-ttu-id="39c45-170">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-170">GitHub repository</span></span>](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a><span data-ttu-id="39c45-171">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="39c45-171">EFCore.BulkExtensions</span></span>

<span data-ttu-id="39c45-172">Toplu işlemleri (INSERT, Update, Delete) EntityFrameworkCore uzantıları.</span><span class="sxs-lookup"><span data-stu-id="39c45-172">EntityFrameworkCore extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="39c45-173">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-173">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="39c45-174">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="39c45-174">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="39c45-175">EF Core için tasarım zamanı çoğullaştırma ekler.</span><span class="sxs-lookup"><span data-stu-id="39c45-175">Adds design-time pluralization to EF Core.</span></span>

[<span data-ttu-id="39c45-176">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="39c45-176">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)
