---
title: Araçlar & Uzantıları - EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e3806f7161fecfe66450d3e08f97caf3d2c84cf3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634233"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="ab0bd-102">EF Temel Araçlar & Uzantıları</span><span class="sxs-lookup"><span data-stu-id="ab0bd-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="ab0bd-103">Bu araçlar ve uzantılar, Entity Framework Core 2.1 ve sonraki etkinlikler için ek işlevler sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="ab0bd-104">Uzantılar çeşitli kaynaklar tarafından oluşturulur ve Entity Framework Core projesinin bir parçası olarak sürdürülemez.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="ab0bd-105">Bir üçüncü taraf uzantısı nı düşünürken, gereksinimlerinizi karşıladığından emin olmak için kalitesini, lisansını, uyumluluğunu, desteğini vb. değerlendirdiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="ab0bd-106">Özellikle, EF Core'un eski bir sürümü için oluşturulmuş bir uzantının en son sürümlerle çalışmadan önce güncelleştirilmesi gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="ab0bd-107">Araçlar</span><span class="sxs-lookup"><span data-stu-id="ab0bd-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="ab0bd-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="ab0bd-108">LLBLGen Pro</span></span>

<span data-ttu-id="ab0bd-109">LLBLGen Pro, Entity Framework ve Entity Framework Core desteğine sahip bir varlık modelleme çözümüdür.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="ab0bd-110">Önce veritabanını veya modeli kullanarak varlık modelinizi kolayca tanımlamanızı ve veritabanınızla eşlenizi belirlemenizi sağlar, böylece sorguları hemen yazmaya başlarsınız.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="ab0bd-111">EF Çekirdek için: 2.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-111">For EF Core: 2.</span></span>

[<span data-ttu-id="ab0bd-112">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="ab0bd-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="ab0bd-113">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="ab0bd-113">Devart Entity Developer</span></span>

<span data-ttu-id="ab0bd-114">Entity Developer, ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access ve LINQ to SQL için güçlü bir ORM tasarımcısıdır.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="ab0bd-115">EF Core modellerinin görsel olarak tasarlanması, önce model veya veritabanı yaklaşımlarının kullanılması ve C# veya Visual Basic kod oluşturmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="ab0bd-116">EF Çekirdek için: 2.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-116">For EF Core: 2.</span></span>

[<span data-ttu-id="ab0bd-117">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="ab0bd-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a><span data-ttu-id="ab0bd-118">Varlık Çerçevesi için nHydrate ORM</span><span class="sxs-lookup"><span data-stu-id="ab0bd-118">nHydrate ORM for Entity Framework</span></span>

<span data-ttu-id="ab0bd-119">Varlık Çerçevesi için güçlü bir şekilde yazılmış, genişletilebilir sınıflar oluşturan bir ORM.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-119">An ORM that creates strongly-typed, extendable classes for Entity Framework.</span></span> <span data-ttu-id="ab0bd-120">Oluşturulan kod Varlık Framework Core'dur.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-120">The generated code is Entity Framework Core.</span></span> <span data-ttu-id="ab0bd-121">Aralarında herhangi bir fark yoktur.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-121">There is no difference.</span></span> <span data-ttu-id="ab0bd-122">Bu, EF veya özel bir ORM'un yerini almayacak.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-122">This is not a replacement for EF or a custom ORM.</span></span> <span data-ttu-id="ab0bd-123">Bir takımın karmaşık veritabanı şemalarını yönetmesine olanak tanıyan görsel, modelleme katmanıdır.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-123">It is a visual, modeling layer that allows a team to manage complex database schemas.</span></span> <span data-ttu-id="ab0bd-124">Git gibi SCM yazılımlarında iyi çalışır ve en az çakışma ile modelinize çok kullanıcılı erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-124">It works well with SCM software like Git, allowing multi-user access to your model with minimal conflicts.</span></span> <span data-ttu-id="ab0bd-125">Yükleyici model değişikliklerini izler ve yükseltme komut dosyaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-125">The installer tracks model changes and creates upgrade scripts.</span></span> <span data-ttu-id="ab0bd-126">EF Çekirdek için: 3.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-126">For EF Core: 3.</span></span>

[<span data-ttu-id="ab0bd-127">Github sitesi</span><span class="sxs-lookup"><span data-stu-id="ab0bd-127">Github site</span></span>](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a><span data-ttu-id="ab0bd-128">EF Çekirdek Elektrikli El Aletleri</span><span class="sxs-lookup"><span data-stu-id="ab0bd-128">EF Core Power Tools</span></span>

<span data-ttu-id="ab0bd-129">EF Core Power Tools, çeşitli EF Core tasarım zamanı görevlerini basit bir kullanıcı arabiriminde ortaya çıkaran bir Visual Studio uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-129">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="ab0bd-130">Varolan veritabanlarından ve [SQL Server DACPACs'dan](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications)DbContext ve varlık sınıflarının ters mühendislik, veritabanı geçişlerinin yönetimi ve model görselleştirmelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-130">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="ab0bd-131">EF Çekirdek için: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-131">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ab0bd-132">GitHub wiki</span><span class="sxs-lookup"><span data-stu-id="ab0bd-132">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="ab0bd-133">Entity Framework Görsel Editör</span><span class="sxs-lookup"><span data-stu-id="ab0bd-133">Entity Framework Visual Editor</span></span>

<span data-ttu-id="ab0bd-134">Entity Framework Visual Editor, EF 6'nın görsel tasarımı için bir ORM tasarımcısı ve EF Core sınıfları ekleyen bir Visual Studio uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-134">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="ab0bd-135">Kod, t4 şablonları kullanılarak oluşturulur, böylece her ihtiyaca uyacak şekilde özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-135">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="ab0bd-136">Devralma, tek yönlü ve çift yönlü çağrışımları, sayısallaştırmaları ve sınıflarınızı renk kodlama ve tasarımınızın olası esrarengiz kısımlarını açıklamak için metin blokları ekleme yeteneğini destekler.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-136">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="ab0bd-137">EF Çekirdek için: 2.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-137">For EF Core: 2.</span></span>

[<span data-ttu-id="ab0bd-138">Mağaza</span><span class="sxs-lookup"><span data-stu-id="ab0bd-138">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="ab0bd-139">CatFactory</span><span class="sxs-lookup"><span data-stu-id="ab0bd-139">CatFactory</span></span>

<span data-ttu-id="ab0bd-140">CatFactory, .NET Core için, DbContext sınıflarının, varlıkların, eşleme yapılandırmalarının ve depo sınıflarının bir SQL Server veritabanından üretimini otomatikleştirebilen bir iskele motorudur.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-140">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="ab0bd-141">EF Çekirdek için: 2.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-141">For EF Core: 2.</span></span>

[<span data-ttu-id="ab0bd-142">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-142">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="ab0bd-143">LoreSoft'un Varlık Çerçeve Çekirdek Jeneratörü</span><span class="sxs-lookup"><span data-stu-id="ab0bd-143">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="ab0bd-144">Entity Framework Core Generator (efg), varolan bir veritabanından EF Core modelleri oluşturabilen bir .NET Core CLI aracıdır, `dotnet ef dbcontext scaffold`ancak bölge değiştirme veya haritalama dosyalarını ayrıştırarak güvenli kod [yenilenmesini](https://efg.loresoft.com/en/latest/regeneration/) de destekler.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-144">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="ab0bd-145">Bu araç, görünüm modelleri, doğrulama ve nesne mapper kodu oluşturmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-145">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="ab0bd-146">EF Çekirdek için: 2.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-146">For EF Core: 2.</span></span>

<span data-ttu-id="ab0bd-147">[Öğretici](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Dokümantasyon](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="ab0bd-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="ab0bd-148">Uzantıları</span><span class="sxs-lookup"><span data-stu-id="ab0bd-148">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="ab0bd-149">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="ab0bd-149">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="ab0bd-150">EF Core tarafından gerçekleştirilen veri değişikliklerinin tarih tablosuna otomatik olarak kaydedilmesini sağlayan bir eklenti kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-150">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="ab0bd-151">EF Çekirdek için: 2.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-151">For EF Core: 2.</span></span>

[<span data-ttu-id="ab0bd-152">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-152">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="ab0bd-153">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="ab0bd-153">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="ab0bd-154">Aynı sorguların sonraki yürütmelerinin veritabanına erişmesini önleyebilmesi ve verileri doğrudan önbellekten alabilmeleri için EF Core sorgularının sonuçlarını ikinci düzey bir önbelleğe depolamayı sağlayan uzantı.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-154">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="ab0bd-155">EF Çekirdek için: 2.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-155">For EF Core: 2.</span></span>

[<span data-ttu-id="ab0bd-156">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-156">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="ab0bd-157">Geco</span><span class="sxs-lookup"><span data-stu-id="ab0bd-157">Geco</span></span>

<span data-ttu-id="ab0bd-158">Geco (Jeneratör Konsolu) .NET Core üzerinde çalışan ve kod oluşturma için C# enterpolasyonlu dizeleri kullanan bir konsol projesine dayalı basit bir kod üretecidir.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-158">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="ab0bd-159">Geco, EF Core için çoğullaştırma, tekilleştirme ve değiştirilebilir şablonlar için desteklenen bir ters model jeneratörü içerir.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-159">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="ab0bd-160">Ayrıca bir tohum veri komut dosyası jeneratör, bir komut dosyası koşucusu ve bir veritabanı temizleyici sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-160">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="ab0bd-161">EF Çekirdek için: 2.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-161">For EF Core: 2.</span></span>

[<span data-ttu-id="ab0bd-162">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-162">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="ab0bd-163">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="ab0bd-163">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="ab0bd-164">Handlebars şablonları ile Entity Framework Core araç zincirini kullanarak varolan bir veritabanından yeniden tasarlanmış sınıfların özelleştirmesine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-164">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="ab0bd-165">EF Çekirdek için: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-165">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ab0bd-166">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-166">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="ab0bd-167">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="ab0bd-167">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="ab0bd-168">NeinLinq, işlevleri yeniden kullanma, sorguları yeniden yazma ve translatable yüklemleri ve seçiciler kullanarak dinamik sorgular oluşturmayı etkinleştirmek için Entity Framework gibi LINQ sağlayıcılarını genişletir.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-168">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="ab0bd-169">EF Çekirdek için: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-169">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ab0bd-170">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-170">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="ab0bd-171">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="ab0bd-171">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="ab0bd-172">Microsoft.EntityFrameworkCore için depo, iş desenleri birimi ve dağıtılmış işlem desteklenen birden çok veritabanını destekleyen bir eklenti.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-172">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="ab0bd-173">EF Çekirdek için: 2.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-173">For EF Core: 2.</span></span>

[<span data-ttu-id="ab0bd-174">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-174">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="ab0bd-175">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="ab0bd-175">EFCore.BulkExtensions</span></span>

<span data-ttu-id="ab0bd-176">Toplu işlemler için EF Core uzantıları (Ekleme, Güncelleme, Silme).</span><span class="sxs-lookup"><span data-stu-id="ab0bd-176">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="ab0bd-177">EF Çekirdek için: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-177">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ab0bd-178">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-178">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="ab0bd-179">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="ab0bd-179">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="ab0bd-180">Tasarım zamanı çoğullaştırma ekler.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-180">Adds design-time pluralization.</span></span> <span data-ttu-id="ab0bd-181">EF Çekirdek için: 2.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-181">For EF Core: 2.</span></span>

[<span data-ttu-id="ab0bd-182">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-182">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="ab0bd-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="ab0bd-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="ab0bd-184">[Index] özniteliğinin canlandırılması (model oluşturma uzantısı ile).</span><span class="sxs-lookup"><span data-stu-id="ab0bd-184">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="ab0bd-185">EF Çekirdek için: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-185">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ab0bd-186">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-186">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="ab0bd-187">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="ab0bd-187">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="ab0bd-188">EF Core In-Memory Veritabanı Sağlayıcısı etrafında bir sarmalayıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-188">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="ab0bd-189">Daha çok ilişkisel bir sağlayıcı gibi davranması sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-189">Makes it act more like a relational provider.</span></span> <span data-ttu-id="ab0bd-190">EF Çekirdek için: 2.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-190">For EF Core: 2.</span></span>

[<span data-ttu-id="ab0bd-191">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-191">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="ab0bd-192">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="ab0bd-192">EFCore.TemporalSupport</span></span>

<span data-ttu-id="ab0bd-193">Zamansal desteğin uygulanması.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-193">An implementation of temporal support.</span></span> <span data-ttu-id="ab0bd-194">EF Çekirdek için: 2.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-194">For EF Core: 2.</span></span>

[<span data-ttu-id="ab0bd-195">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-195">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="ab0bd-196">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="ab0bd-196">EfCoreTemporalTable</span></span>

<span data-ttu-id="ab0bd-197">Tanıtılan uzantı yöntemlerini kullanarak favori veritabanınızda `AsTemporalAll()` `AsTemporalAsOf(date)`zamansal `AsTemporalBetween(startDate, endDate)` `AsTemporalContained(startDate, endDate)`sorguları kolayca gerçekleştirin: , , `AsTemporalFrom(startDate, endDate)`, .</span><span class="sxs-lookup"><span data-stu-id="ab0bd-197">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="ab0bd-198">EF Çekirdek için: 3.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-198">For EF Core: 3.</span></span>

[<span data-ttu-id="ab0bd-199">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-199">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="ab0bd-200">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="ab0bd-200">EFCore.TimeTraveler</span></span>

<span data-ttu-id="ab0bd-201">Daha önce tanımlamış olduğunuz EF Core kodunu, varlıkları ve eşleşmelerini kullanarak [SQL Server Geçici Geçmişi'](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) ne karşı tam özellikli Entity Framework Core sorgularına izin verin.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-201">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="ab0bd-202">Kodunuzu ' a `using (TemporalQuery.AsOf(targetDateTime)) {...}`sarmalayarak zamanda yolculuk</span><span class="sxs-lookup"><span data-stu-id="ab0bd-202">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="ab0bd-203">EF Çekirdek için: 3.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-203">For EF Core: 3.</span></span>

[<span data-ttu-id="ab0bd-204">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-204">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="ab0bd-205">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="ab0bd-205">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="ab0bd-206">SQL Server kullanan geliştiricilerin zamansal tabloları kolayca kullanmasına olanak tanıyan Entity Framework Core uzantılı kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-206">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="ab0bd-207">EF Çekirdek için: 2.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-207">For EF Core: 2.</span></span>

[<span data-ttu-id="ab0bd-208">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-208">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="ab0bd-209">EntityFrameworkCore.Önbelleğe</span><span class="sxs-lookup"><span data-stu-id="ab0bd-209">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="ab0bd-210">Yüksek performanslı ikinci düzey sorgu önbelleği.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-210">A high-performance second-level query cache.</span></span> <span data-ttu-id="ab0bd-211">EF Çekirdek için: 2.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-211">For EF Core: 2.</span></span>

[<span data-ttu-id="ab0bd-212">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-212">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="ab0bd-213">Varlık Çerçevesi Plus</span><span class="sxs-lookup"><span data-stu-id="ab0bd-213">Entity Framework Plus</span></span>

<span data-ttu-id="ab0bd-214">DbContext'ınızı Şu gibi özelliklerle genişletir: Filtre Ekle, Denetim, Önbelleğe Alma, Sorgula, Gelecek Sorgula, Toplu İş Silme, Toplu Güncelleme ve daha fazlası.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-214">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="ab0bd-215">EF Çekirdek için: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-215">For EF Core: 2, 3.</span></span>

<span data-ttu-id="ab0bd-216">[Web sitesi](https://entityframework-plus.net/)
[GitHub deposu](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="ab0bd-216">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="ab0bd-217">Varlık Çerçeve Uzantıları</span><span class="sxs-lookup"><span data-stu-id="ab0bd-217">Entity Framework Extensions</span></span>

<span data-ttu-id="ab0bd-218">Yüksek performanslı toplu işlemlerle DbContext'ınızı genişletir: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge ve daha fazlası.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-218">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="ab0bd-219">EF Çekirdek için: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-219">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ab0bd-220">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="ab0bd-220">Website</span></span>](https://entityframework-extensions.net/)

### <a name="expressionify"></a><span data-ttu-id="ab0bd-221">İfade</span><span class="sxs-lookup"><span data-stu-id="ab0bd-221">Expressionify</span></span>

<span data-ttu-id="ab0bd-222">Linq lambdas arama uzatma yöntemleri için destek ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-222">Add support for calling extension methods in linq lambdas.</span></span> <span data-ttu-id="ab0bd-223">EF Çekirdek için: 3.1</span><span class="sxs-lookup"><span data-stu-id="ab0bd-223">For EF Core: 3.1</span></span>

[<span data-ttu-id="ab0bd-224">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ab0bd-224">GitHub repository</span></span>](https://github.com/ClaveConsulting/Expressionify)

### <a name="xlinq"></a><span data-ttu-id="ab0bd-225">XLinq</span><span class="sxs-lookup"><span data-stu-id="ab0bd-225">XLinq</span></span>

<span data-ttu-id="ab0bd-226">İlişkisel veritabanları için Dil Tümleşik Sorgu (LINQ) teknolojisi.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-226">Language Integrated Query (LINQ) technology for relational databases.</span></span> <span data-ttu-id="ab0bd-227">Güçlü bir şekilde yazılan sorguları yazmak için C# kullanmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-227">It allows you to use C# to write strongly typed queries.</span></span> <span data-ttu-id="ab0bd-228">EF Çekirdek için: 3.1</span><span class="sxs-lookup"><span data-stu-id="ab0bd-228">For EF Core: 3.1</span></span>

- <span data-ttu-id="ab0bd-229">Sorgu oluşturma için tam C# desteği: lambda içinde birden fazla ifade, değişkenler, fonksiyonlar, vb.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-229">Full C# support for query creation: multiple statements inside lambda, variables, functions, etc.</span></span>
- <span data-ttu-id="ab0bd-230">SQL ile anlamsal boşluk yok.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-230">No semantic gap with SQL.</span></span> <span data-ttu-id="ab0bd-231">XLinq, xLinq, `SELECT` `FROM`xandiman, tip güvenliği ve yeniden düzenleme ile tanıdık sözdizimini birleştirerek SQL deyimlerini (, , `WHERE`) birinci sınıf C# yöntemleri olarak bildirir.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-231">XLinq declares SQL statements (like `SELECT`, `FROM`, `WHERE`) as first class C# methods, combining familiar syntax with intellisense, type safety and refactoring.</span></span>

<span data-ttu-id="ab0bd-232">Sonuç olarak SQL, API'sini yerel olarak ortaya çıkaran "başka" sınıf kitaplığı olur, tam anlamıyla *"Dil Tümleşik SQL"* olur.</span><span class="sxs-lookup"><span data-stu-id="ab0bd-232">As a result SQL becomes just "another" class library exposing its API locally, literally *"Language Integrated SQL"*.</span></span>

[<span data-ttu-id="ab0bd-233">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="ab0bd-233">Website</span></span>](http://xlinq.live/)
