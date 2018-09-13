---
title: Araçlar ve uzantılar - EF Core
author: ErikEJ
ms.date: 07/03/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 1edc7a20f54b2d26f899c93e98dfaf6d62c29f86
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490733"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="c892b-102">EF Core Araçlar ve uzantılar</span><span class="sxs-lookup"><span data-stu-id="c892b-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="c892b-103">Araçlar ve uzantılar için Entity Framework Core ek işlevsellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="c892b-103">Tools and extensions provide additional functionality for Entity Framework Core.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="c892b-104">Uzantıları çeşitli kaynaklardan tarafından oluşturulur ve Entity Framework Core projesinin bir parçası korunur değil.</span><span class="sxs-lookup"><span data-stu-id="c892b-104">Extensions are built by a variety of sources and not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="c892b-105">Bir üçüncü taraf uzantı dikkate alındığında, kalite, lisanslama, uyumluluk, destek vb. gereksinimlerinizi karşıladıklarından emin olmak için değerlendirilecek emin olun.</span><span class="sxs-lookup"><span data-stu-id="c892b-105">When considering a third party extension, be sure to evaluate quality, licensing, compatibility, support, etc. to ensure they meet your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="c892b-106">Araçlar</span><span class="sxs-lookup"><span data-stu-id="c892b-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="c892b-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="c892b-107">LLBLGen Pro</span></span>

<span data-ttu-id="c892b-108">LLBLGen Pro çözüm Entity Framework ve Entity Framework Core desteğiyle birlikte modeling bir varlıktır.</span><span class="sxs-lookup"><span data-stu-id="c892b-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="c892b-109">Kolayca varlık modelinizi tanımlayın ve veritabanınıza ilk kullanarak veritabanı eşleme sağlar veya sorgu hemen yazma başlangıç yapmanıza yardımcı olacak ilk olarak, model.</span><span class="sxs-lookup"><span data-stu-id="c892b-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="c892b-110">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="c892b-110">website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="c892b-111">Devart varlık Geliştirici</span><span class="sxs-lookup"><span data-stu-id="c892b-111">Devart Entity Developer</span></span>

<span data-ttu-id="c892b-112">Varlık, ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access ve LINQ to SQL için güçlü bir ORM Tasarımcısı geliştiricidir.</span><span class="sxs-lookup"><span data-stu-id="c892b-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="c892b-113">Model-First kullanabilirsiniz ve ORM modelinizi tasarlamak ve C# veya Visual Basic .NET kodunu oluşturmak için veritabanı ilk yaklaşır.</span><span class="sxs-lookup"><span data-stu-id="c892b-113">You can use  Model-First and Database-First approaches to design your ORM model and generate C# or Visual Basic .NET code for it.</span></span> <span data-ttu-id="c892b-114">ORM modelleri oluşturmaya yönelik yeni bir yaklaşım sunar, üretkenliği artırıyor ve veritabanı uygulamaların geliştirilmesini kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="c892b-114">It introduces new approaches for designing ORM models, boosts productivity, and facilitates the development of database applications.</span></span>

[<span data-ttu-id="c892b-115">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="c892b-115">website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="c892b-116">EF Core güç araçları</span><span class="sxs-lookup"><span data-stu-id="c892b-116">EF Core Power Tools</span></span>

<span data-ttu-id="c892b-117">Visual Studio 2017 + uzantısı.</span><span class="sxs-lookup"><span data-stu-id="c892b-117">Visual Studio 2017+ extension.</span></span> <span data-ttu-id="c892b-118">Varolan bir veritabanına veya SQL Server veritabanı projesi tersine mühendislik DbContext ve POCO sınıfların görselleştirin ve çeşitli yollarla, DbContext inceleyin.</span><span class="sxs-lookup"><span data-stu-id="c892b-118">You can reverse engineer of DbContext and POCO classes from an existing database or SQL Server Database project, and visualize and inspect your DbContext in various ways.</span></span>

[<span data-ttu-id="c892b-119">GitHub wiki</span><span class="sxs-lookup"><span data-stu-id="c892b-119">GitHub wiki</span></span>](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="c892b-120">Entity Framework Visual Düzenleyicisi</span><span class="sxs-lookup"><span data-stu-id="c892b-120">Entity Framework Visual Editor</span></span>

<span data-ttu-id="c892b-121">Bir görsel tasarım sınıfların, Entity Framework 6, Core 2.0 ve Core 2.1 için ORM Tasarımcısı ekleyen bir Visual Studio 2017 uzantısı.</span><span class="sxs-lookup"><span data-stu-id="c892b-121">A Visual Studio 2017 extension that adds an ORM designer for visual design of Entity Framework 6, Core 2.0 and Core 2.1 classes.</span></span> <span data-ttu-id="c892b-122">Kod, her türlü ihtiyacı karşılamak için T4 şablonları tamamen özelleştirilebilir şekilde kullanarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c892b-122">Code is generated using T4 templates so can be completely customized to suit any needs.</span></span> <span data-ttu-id="c892b-123">Numaralandırmalar ve sınıflarınızı renk kodu ve metin blokları tasarımınızı potansiyel olarak karıştıran bölümlerini açıklamak için ekleme olanağı gibi devralma, tek yönlü ve çift yönlü ilişkilendirmeleri tümü, desteklenir.</span><span class="sxs-lookup"><span data-stu-id="c892b-123">Inheritance, unidirectional and bidirectional associations are all supported, as are enumerations and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span>

[<span data-ttu-id="c892b-124">Market</span><span class="sxs-lookup"><span data-stu-id="c892b-124">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

## <a name="extensions"></a><span data-ttu-id="c892b-125">Uzantıları</span><span class="sxs-lookup"><span data-stu-id="c892b-125">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="c892b-126">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="c892b-126">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="c892b-127">Bir eklenti için otomatik olarak veri kaydını desteklemek Microsoft.EntityFrameworkCore geçmişi değiştirir.</span><span class="sxs-lookup"><span data-stu-id="c892b-127">A plugin for Microsoft.EntityFrameworkCore to support automatically recording data changes history.</span></span>

[<span data-ttu-id="c892b-128">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-128">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="c892b-129">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="c892b-129">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="c892b-130">Zaman uyumsuz destek ekleyen Microsoft.EntityFrameworkCore için dinamik LINQ uzantıları</span><span class="sxs-lookup"><span data-stu-id="c892b-130">Dynamic Linq extensions for Microsoft.EntityFrameworkCore which adds Async support</span></span>

 [<span data-ttu-id="c892b-131">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-131">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a><span data-ttu-id="c892b-132">EFCore.Practices</span><span class="sxs-lookup"><span data-stu-id="c892b-132">EFCore.Practices</span></span>

<span data-ttu-id="c892b-133">Test destekleyen bir API – N + 1 sorgular için taramak için küçük bir çerçeve dahil olmak üzere bazı iyi ya da en iyi uygulamaları yakalamak çalışır.</span><span class="sxs-lookup"><span data-stu-id="c892b-133">Attempt to capture some good or best practices in an API that supports testing – including a small framework to scan for N+1 queries.</span></span>

[<span data-ttu-id="c892b-134">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-134">GitHub repository</span></span>](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="c892b-135">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="c892b-135">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="c892b-136">İkinci düzey önbelleğe alma kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="c892b-136">Second Level Caching Library.</span></span> <span data-ttu-id="c892b-137">İkinci düzey önbelleğe alma sorgu bir önbellektir.</span><span class="sxs-lookup"><span data-stu-id="c892b-137">Second level caching is a query cache.</span></span> <span data-ttu-id="c892b-138">Böylece aynı EF komutları, verileri alır, önbellek yerine yeniden veritabanına karşı yürütme EF komutlarının sonuçlarını önbellekte depolanır.</span><span class="sxs-lookup"><span data-stu-id="c892b-138">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span>

[<span data-ttu-id="c892b-139">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-139">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a><span data-ttu-id="c892b-140">Detached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="c892b-140">Detached.EntityFramework</span></span>

<span data-ttu-id="c892b-141">Yükler ve tüm ayrılmış varlık grafikleri (alt varlıklar ve liste varlığı) kaydeder.</span><span class="sxs-lookup"><span data-stu-id="c892b-141">Loads and saves entire detached entity graphs (the entity with their child entities and lists).</span></span> <span data-ttu-id="c892b-142">İlham [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span><span class="sxs-lookup"><span data-stu-id="c892b-142">Inspired by [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span></span> <span data-ttu-id="c892b-143">Ayrıca bazı eklentiler için simplificate denetim ve sayfalandırma gibi bazı yinelenen görevleri ekleyin olur.</span><span class="sxs-lookup"><span data-stu-id="c892b-143">The idea is also add some plugins to simplificate some repetitive tasks, like auditing and pagination.</span></span>

[<span data-ttu-id="c892b-144">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-144">GitHub repository</span></span>](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="c892b-145">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="c892b-145">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="c892b-146">Birincil anahtarı (bileşik anahtarlar dahil) herhangi bir varlığa bir sözlük olarak alın.</span><span class="sxs-lookup"><span data-stu-id="c892b-146">Retrieve the primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="c892b-147">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-147">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a><span data-ttu-id="c892b-148">EntityFrameworkCore.Rx</span><span class="sxs-lookup"><span data-stu-id="c892b-148">EntityFrameworkCore.Rx</span></span>

<span data-ttu-id="c892b-149">Sık erişimli gözlemlenenler Entity Framework varlık için reaktif uzantısı sarmalayıcıları.</span><span class="sxs-lookup"><span data-stu-id="c892b-149">Reactive extension wrappers for hot observables of Entity Framework entities.</span></span>

[<span data-ttu-id="c892b-150">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-150">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a><span data-ttu-id="c892b-151">EntityFrameworkCore.Triggers</span><span class="sxs-lookup"><span data-stu-id="c892b-151">EntityFrameworkCore.Triggers</span></span>

<span data-ttu-id="c892b-152">Ekleme, güncelleştirme ve olayları Sil içeren varlıklarınız için Tetikleyiciler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c892b-152">Add triggers to your entities with insert, update, and delete events.</span></span> <span data-ttu-id="c892b-153">Her üç olaylar vardır: önce sonra ve başarısızlık durumunda.</span><span class="sxs-lookup"><span data-stu-id="c892b-153">There are three events for each: before, after, and upon failure.</span></span>

[<span data-ttu-id="c892b-154">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-154">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="c892b-155">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="c892b-155">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="c892b-156">Türü belirlenmiş OriginalValue, varlık özelliklerini erişin.</span><span class="sxs-lookup"><span data-stu-id="c892b-156">Get typed access to the OriginalValue of your entity properties.</span></span> <span data-ttu-id="c892b-157">Basit ve karmaşık özellikler desteklenir, gezinti ve koleksiyonların değildir.</span><span class="sxs-lookup"><span data-stu-id="c892b-157">Simple and complex properties are supported, navigation/collections are not.</span></span>

[<span data-ttu-id="c892b-158">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-158">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="c892b-159">Geco</span><span class="sxs-lookup"><span data-stu-id="c892b-159">Geco</span></span>

<span data-ttu-id="c892b-160">Geco bir ters Model Oluşturucu Çoğullaştırma/Singularization ve C# 6.0 ilişkilendirilmiş dizelerle tabanlı ve.Net Core üzerinde çalışan düzenlenebilir şablonlar için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="c892b-160">Geco provides a Reverse Model generator with support for Pluralization/Singularization and editable templates based on C# 6.0 interpolated strings and running on .Net Core.</span></span> <span data-ttu-id="c892b-161">Ayrıca bir çekirdek betiği Oluşturucu SQL birleştirme betikleri ve bir betik Çalıştırıcısı ile sağlar.</span><span class="sxs-lookup"><span data-stu-id="c892b-161">It also provides an Seed script generator with SQL Merge scripts and an script runner.</span></span>

[<span data-ttu-id="c892b-162">Github deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-162">Github repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="c892b-163">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="c892b-163">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="c892b-164">LinqKit.Microsoft.EntityFrameworkCore LINQ için uzantıları SQL ve EntityFrameworkCore ileri kullanıcılar için ücretsiz bir kümesidir.</span><span class="sxs-lookup"><span data-stu-id="c892b-164">LinqKit.Microsoft.EntityFrameworkCore is a free set of extensions for LINQ to SQL and EntityFrameworkCore power users.</span></span> <span data-ttu-id="c892b-165">İle Include(...) ve IDbAsync desteği.</span><span class="sxs-lookup"><span data-stu-id="c892b-165">With Include(...) and IDbAsync support.</span></span>

[<span data-ttu-id="c892b-166">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-166">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="c892b-167">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="c892b-167">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="c892b-168">NeinLinq.EntityFrameworkCore İşlevler, bunları null için güvenli ve yapı dinamik sorgular yapma sorguları, yeniden yazma yeniden yalnızca küçük bir alt kümesini .NET işlevleri destekleyen LINQ sağlayıcıları gibi Entity Framework kullanarak için faydalı uzantılar sağlar. çevrilebilir doğrulamaları ve Seçici kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="c892b-168">NeinLinq.EntityFrameworkCore provides helpful extensions for using LINQ providers such as Entity Framework that support only a minor subset of .NET functions, reusing functions, rewriting queries, even making them null-safe, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="c892b-169">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-169">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="c892b-170">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="c892b-170">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="c892b-171">Havuz, iş birimi desenleri ve birden çok veritabanı ile birlikte dağıtılmış işlem desteklenen desteklemek Microsoft.EntityFrameworkCore bir eklenti.</span><span class="sxs-lookup"><span data-stu-id="c892b-171">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple database with distributed transaction supported.</span></span>

[<span data-ttu-id="c892b-172">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-172">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a><span data-ttu-id="c892b-173">EntityFramework.LazyLoading</span><span class="sxs-lookup"><span data-stu-id="c892b-173">EntityFramework.LazyLoading</span></span>

<span data-ttu-id="c892b-174">EF Core 1.1 yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="c892b-174">Lazy Loading for EF Core 1.1</span></span>

[<span data-ttu-id="c892b-175">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-175">GitHub repository</span></span>](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a><span data-ttu-id="c892b-176">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="c892b-176">EFCore.BulkExtensions</span></span>

<span data-ttu-id="c892b-177">Toplu işlemleri (INSERT, Update, Delete) EntityFrameworkCore uzantıları.</span><span class="sxs-lookup"><span data-stu-id="c892b-177">EntityFrameworkCore extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="c892b-178">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-178">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="c892b-179">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="c892b-179">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="c892b-180">EF Core için tasarım zamanı çoğullaştırma ekler.</span><span class="sxs-lookup"><span data-stu-id="c892b-180">Adds design-time pluralization to EF Core.</span></span>

[<span data-ttu-id="c892b-181">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c892b-181">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)
