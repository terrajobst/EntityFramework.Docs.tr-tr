---
title: Araçlar & Uzantılar-EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e70011b42818e4df1ec5b9b88d7adb9d36bb26f1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654795"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="dd2ed-102">EF Core araçları & uzantıları</span><span class="sxs-lookup"><span data-stu-id="dd2ed-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="dd2ed-103">Bu araçlar ve uzantılar, Entity Framework Core 2,0 ve üzeri için ek işlevler sağlar.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-103">These tools and extensions provide additional functionality for Entity Framework Core 2.0 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="dd2ed-104">Uzantılar çeşitli kaynaklardan oluşturulmuştur ve Entity Framework Core projenin bir parçası olarak korunmaz.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="dd2ed-105">Üçüncü taraf uzantısını değerlendirirken, gereksinimlerinizi karşıladığından emin olmak için kalite, lisanslama, uyumluluk, destek vb. değerlendirdiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="dd2ed-106">Araçlar</span><span class="sxs-lookup"><span data-stu-id="dd2ed-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="dd2ed-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="dd2ed-107">LLBLGen Pro</span></span>

<span data-ttu-id="dd2ed-108">LLBLGen Pro, Entity Framework ve Entity Framework Core desteğiyle bir varlık modelleme çözümüdür.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="dd2ed-109">Bu, varlık modelinizi kolayca tanımlamanızı ve veritabanını ilk veya modeli kullanarak veritabanınıza eşlemenizi sağlar, böylece sorguları hemen yazmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="dd2ed-110">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="dd2ed-110">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="dd2ed-111">Varlık geliştiriciyi kaldırma</span><span class="sxs-lookup"><span data-stu-id="dd2ed-111">Devart Entity Developer</span></span>

<span data-ttu-id="dd2ed-112">Varlık geliştiricisi, ADO.NET Entity Framework, Nhazırda beklet, LinqConnect, Telerik veri erişimi ve LINQ to SQL için güçlü bir ORM tasarlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="dd2ed-113">EF Core modellerinin görsel olarak tasarlanmasını, ilk veya veritabanı ilk yaklaşımı ve C# ya da kod üretimini Visual Basic destekler.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-113">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span>

[<span data-ttu-id="dd2ed-114">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="dd2ed-114">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="dd2ed-115">EF Core güç araçları</span><span class="sxs-lookup"><span data-stu-id="dd2ed-115">EF Core Power Tools</span></span>

<span data-ttu-id="dd2ed-116">EF Core güç araçları, basit bir kullanıcı arabiriminde çeşitli EF Core tasarım zamanı görevleri sunan bir Visual Studio 2017 uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-116">EF Core Power Tools is a Visual Studio 2017 extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="dd2ed-117">Bu, var olan veritabanlarından DbContext ve varlık sınıflarının tersine mühendislik ve [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), veritabanı geçişleri yönetimi ve model görselleştirmeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-117">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span>

[<span data-ttu-id="dd2ed-118">GitHub wiki</span><span class="sxs-lookup"><span data-stu-id="dd2ed-118">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="dd2ed-119">Entity Framework görsel düzenleyici</span><span class="sxs-lookup"><span data-stu-id="dd2ed-119">Entity Framework Visual Editor</span></span>

<span data-ttu-id="dd2ed-120">Entity Framework Visual Düzenleyicisi, EF 6 ve EF Core sınıflarının görsel tasarımı için bir ORM Tasarımcısı ekleyen bir Visual Studio uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-120">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="dd2ed-121">Kod, T4 şablonları kullanılarak oluşturulur, bu nedenle, herhangi bir gereksinimlerinize uyacak şekilde özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-121">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="dd2ed-122">Devralma, tek yönlü ve çift yönlü ilişkilendirmeler, numaralandırmalar ve sınıflarınızın renk Kodlayabilme ve tasarımınızın potansiyel olarak büyük bir kısmını açıklamak için metin blokları ekleme imkanını destekler.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-122">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span>

[<span data-ttu-id="dd2ed-123">'Nde</span><span class="sxs-lookup"><span data-stu-id="dd2ed-123">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="dd2ed-124">Katfactory</span><span class="sxs-lookup"><span data-stu-id="dd2ed-124">CatFactory</span></span>

<span data-ttu-id="dd2ed-125">CatFactory, bir SQL Server veritabanından DbContext sınıflarının, varlıkların, eşleme yapılandırmalarının ve depo sınıflarının oluşturulmasını otomatik hale getirebilen .NET Core için bir yapı iskelesi altyapısıdır.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-125">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span>

[<span data-ttu-id="dd2ed-126">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-126">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="dd2ed-127">LoreSoft 'ın Entity Framework Core Oluşturucu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-127">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="dd2ed-128">Entity Framework Core Generator (EFG), `dotnet ef dbcontext scaffold`gibi var olan bir veritabanından EF Core modeller oluşturabilen, ancak bölge değişikliği aracılığıyla veya eşleme dosyalarını ayrıştırarak güvenli kod yeniden [oluşturmayı](https://efg.loresoft.com/en/latest/regeneration/) da destekleyen bir .NET Core CLI aracıdır.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-128">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="dd2ed-129">Bu araç, görünüm modellerinin, doğrulamanın ve nesne Eşleyici kodunun oluşturulmasını destekler.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-129">This tool supports generating view models, validation, and object mapper code.</span></span>

<span data-ttu-id="dd2ed-130">[Eğitim](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[belgeleri](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="dd2ed-130">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="dd2ed-131">Uzantılar</span><span class="sxs-lookup"><span data-stu-id="dd2ed-131">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="dd2ed-132">Microsoft. EntityFrameworkCore. oto geçmişi</span><span class="sxs-lookup"><span data-stu-id="dd2ed-132">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="dd2ed-133">EF Core tarafından gerçekleştirilen veri değişikliklerinin bir geçmiş tablosuna otomatik olarak kaydedilmesini sağlayan bir eklenti kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-133">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span>

[<span data-ttu-id="dd2ed-134">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-134">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="dd2ed-135">Microsoft. EntityFrameworkCore. Dynamiclınq</span><span class="sxs-lookup"><span data-stu-id="dd2ed-135">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="dd2ed-136">EF Core ile zaman uyumsuz destek içeren System. LINQ. Dynamic .NET Core/.NET Standard bağlantı noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-136">A .NET Core / .NET Standard port of System.Linq.Dynamic that includes async support with EF Core.</span></span>
<span data-ttu-id="dd2ed-137">System. LINQ. Dynamic, kod yerine dize ifadelerinden dinamik olarak LINQ sorgularının nasıl oluşturulacağını gösteren bir Microsoft örneği olarak kaynaklı.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-137">System.Linq.Dynamic originated as a Microsoft sample that shows how to construct LINQ queries dynamically from string expressions rather than code.</span></span>

[<span data-ttu-id="dd2ed-138">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-138">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="dd2ed-139">EFSecondLevelCache. Core</span><span class="sxs-lookup"><span data-stu-id="dd2ed-139">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="dd2ed-140">Aynı sorguların sonraki yürütmelerinin veritabanına erişmeyi ve verileri doğrudan önbellekten almasını önlemek için, EF Core sorgularının sonuçlarının ikinci düzey bir önbellekte depolanmasını sağlayan bir uzantı.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-140">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span>

[<span data-ttu-id="dd2ed-141">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-141">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="dd2ed-142">EntityFrameworkCore. PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="dd2ed-142">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="dd2ed-143">Bu kitaplık, birincil anahtar (bileşik anahtarlar dahil) değerlerinin bir sözlük olarak herhangi bir varlıktan alınmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-143">This library allows retrieving the values of primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="dd2ed-144">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-144">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="dd2ed-145">EntityFrameworkCore. TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="dd2ed-145">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="dd2ed-146">Bu kitaplık, varlık özelliklerinin orijinal değerlerine kesin olarak belirlenmiş erişimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-146">This library enables strongly typed access to the original values of entity properties.</span></span>

[<span data-ttu-id="dd2ed-147">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-147">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="dd2ed-148">Geco</span><span class="sxs-lookup"><span data-stu-id="dd2ed-148">Geco</span></span>

<span data-ttu-id="dd2ed-149">Geco (Oluşturucu konsolu), .NET Core üzerinde çalışan ve kod oluşturma için enterpolasyonlu dizeler kullanan C# bir konsol projesine dayalı basit bir kod Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-149">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="dd2ed-150">Geco, plurmaya, sinleştirme ve düzenlenebilir şablonlara yönelik desteğe sahip EF Core için bir ters model Oluşturucu içerir.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-150">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="dd2ed-151">Ayrıca, bir çekirdek veri betik Oluşturucu, bir komut dosyası Çalıştırıcısı ve bir veritabanı temizleyicisi de sağlar.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-151">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span>

[<span data-ttu-id="dd2ed-152">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-152">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="dd2ed-153">LinqKit. Microsoft. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="dd2ed-153">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="dd2ed-154">LinqKit. Microsoft. EntityFrameworkCore, LINQKit kitaplığının EF Core uyumlu bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-154">LinqKit.Microsoft.EntityFrameworkCore is an EF Core-compatible version of the LINQKit library.</span></span> <span data-ttu-id="dd2ed-155">LINQKit, LINQ to SQL ve Power Users Entity Framework için ücretsiz bir uzantılar kümesidir.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-155">LINQKit is a free set of extensions for LINQ to SQL and Entity Framework power users.</span></span> <span data-ttu-id="dd2ed-156">Koşul ifadelerinin dinamik olarak oluşturulmasına ve alt sorgularda ifade değişkenlerinin kullanılmasına benzer gelişmiş işlevlere izin vermez.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-156">It enables advanced functionality like dynamic building of predicate expressions, and using expression variables in subqueries.</span></span>  

[<span data-ttu-id="dd2ed-157">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-157">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="dd2ed-158">Newınlınq. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="dd2ed-158">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="dd2ed-159">Neınlinq, işlevleri yeniden kullanmayı, sorguları yeniden yazmayı ve çevrilebilir koşullara ve seçicileri kullanarak dinamik sorgular oluşturmayı etkinleştirmek için Entity Framework gibi LINQ sağlayıcılarını genişletir.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-159">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="dd2ed-160">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-160">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="dd2ed-161">Microsoft. EntityFrameworkCore. UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="dd2ed-161">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="dd2ed-162">Microsoft. EntityFrameworkCore 'un depoyu, iş biçimlerini ve dağıtılmış işlem desteklenen birden çok veritabanını desteklemesi için bir eklentisi.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-162">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span>

[<span data-ttu-id="dd2ed-163">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-163">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="dd2ed-164">EFCore. BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="dd2ed-164">EFCore.BulkExtensions</span></span>

<span data-ttu-id="dd2ed-165">Toplu işlemler için EF Core uzantıları (INSERT, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="dd2ed-165">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="dd2ed-166">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-166">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="dd2ed-167">Brıcelam. EntityFrameworkCore. Pluralizer</span><span class="sxs-lookup"><span data-stu-id="dd2ed-167">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="dd2ed-168">EF Core için tasarım zamanı plurun ekler.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-168">Adds design-time pluralization to EF Core.</span></span>

[<span data-ttu-id="dd2ed-169">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-169">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a><span data-ttu-id="dd2ed-170">Pomelofons/Pomelo. EntityFrameworkCore. Extensions. ToSql</span><span class="sxs-lookup"><span data-stu-id="dd2ed-170">PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql</span></span>

<span data-ttu-id="dd2ed-171">Basit senaryolarda belirli bir LINQ sorgusu için oluşturulacak EF Core SQL ifadesini alan basit bir genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-171">A simple extension method that obtains the SQL statement EF Core would generate for a given LINQ query in simple scenarios.</span></span> <span data-ttu-id="dd2ed-172">EF Core, tek bir LINQ sorgusu için birden fazla SQL deyimi ve parametre değerlerine bağlı olarak farklı SQL deyimlerini oluşturabileceği için ToSql yöntemi basit senaryolarla sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-172">The ToSql method is limited to simple scenarios because EF Core can generate more than one SQL statement for a single LINQ query, and different SQL statements depending on parameter values.</span></span>

[<span data-ttu-id="dd2ed-173">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-173">GitHub repository</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="dd2ed-174">Toolbandı. EntityFrameworkCore. ındexattribute</span><span class="sxs-lookup"><span data-stu-id="dd2ed-174">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="dd2ed-175">EF Core (model oluşturma için uzantılı) için [Index] özniteliğinin ' den önceki değer.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-175">Revival of [Index] attribute for EF Core (with extension for model building).</span></span>

[<span data-ttu-id="dd2ed-176">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-176">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="dd2ed-177">EfCore. ınmemoryyardımcıları</span><span class="sxs-lookup"><span data-stu-id="dd2ed-177">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="dd2ed-178">Bellek Içi veritabanı sağlayıcısı EF Core etrafında bir sarmalayıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-178">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="dd2ed-179">İlişkisel bir sağlayıcı gibi çalışır hale getirir.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-179">Makes it act more like a relational provider.</span></span>

[<span data-ttu-id="dd2ed-180">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-180">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="dd2ed-181">EFCore. TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="dd2ed-181">EFCore.TemporalSupport</span></span>

<span data-ttu-id="dd2ed-182">EF Core için zamana bağlı destek bir uygulama.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-182">An implementation of temporal support for EF Core.</span></span>

[<span data-ttu-id="dd2ed-183">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-183">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="dd2ed-184">EntityFrameworkCore. önbelleklenebilir</span><span class="sxs-lookup"><span data-stu-id="dd2ed-184">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="dd2ed-185">EF Core için yüksek performanslı ikinci düzey sorgu önbelleği.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-185">A high-performance second-level query cache for EF Core.</span></span>

[<span data-ttu-id="dd2ed-186">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-186">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="dd2ed-187">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="dd2ed-187">Entity Framework Plus</span></span>

<span data-ttu-id="dd2ed-188">DbContext dosyanızı, şunlar gibi özelliklerle genişletir: filtre, denetim, önbelleğe alma, sorgu gelecekteki, toplu silme, toplu güncelleştirme ve daha fazlasını Içerir.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-188">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span>

<span data-ttu-id="dd2ed-189">[Web sitesi](https://entityframework-plus.net/)
[GitHub deposu](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="dd2ed-189">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="dd2ed-190">Entity Framework uzantıları</span><span class="sxs-lookup"><span data-stu-id="dd2ed-190">Entity Framework Extensions</span></span>

<span data-ttu-id="dd2ed-191">DbContext uygulamanızı yüksek performanslı toplu işlemlerle genişletir: BulkSaveChanges, Bulkınsert, BulkUpdate, BulkDelete, BulkMerge ve daha fazlası.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-191">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span>

[<span data-ttu-id="dd2ed-192">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="dd2ed-192">Website</span></span>](https://entityframework-extensions.net/)

### <a name="reconciler"></a><span data-ttu-id="dd2ed-193">Bağdaştıriler</span><span class="sxs-lookup"><span data-stu-id="dd2ed-193">Reconciler</span></span>

<span data-ttu-id="dd2ed-194">İlgili varlıkları ekleyerek, güncelleştirerek ve kaldırarak depodaki bir varlık grafiğini verilen bir tek güncelleştirme.</span><span class="sxs-lookup"><span data-stu-id="dd2ed-194">Update an entity graph in store to a given one by inserting, updating and removing the respective entities.</span></span>

[<span data-ttu-id="dd2ed-195">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="dd2ed-195">GitHub repository</span></span>](https://github.com/jtheisen/reconciler)
