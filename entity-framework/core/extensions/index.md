---
title: Araçlar & Uzantılar-EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: bab725afffe1fbf9f8c0abeef58579ac9dc842d2
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502088"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="c1ac2-102">EF Core araçları & uzantıları</span><span class="sxs-lookup"><span data-stu-id="c1ac2-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="c1ac2-103">Bu araçlar ve uzantılar, Entity Framework Core 2,1 ve üzeri için ek işlevler sağlar.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="c1ac2-104">Uzantılar çeşitli kaynaklardan oluşturulmuştur ve Entity Framework Core projenin bir parçası olarak korunmaz.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="c1ac2-105">Üçüncü taraf uzantısını değerlendirirken, gereksinimlerinizi karşıladığından emin olmak için kalite, lisanslama, uyumluluk, destek vb. değerlendirdiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="c1ac2-106">Özellikle, EF Core eski bir sürümü için oluşturulmuş bir uzantının en son sürümlerle çalışmadan önce güncelleştirilmesi gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="c1ac2-107">Araçları</span><span class="sxs-lookup"><span data-stu-id="c1ac2-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="c1ac2-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="c1ac2-108">LLBLGen Pro</span></span>

<span data-ttu-id="c1ac2-109">LLBLGen Pro, Entity Framework ve Entity Framework Core desteğiyle bir varlık modelleme çözümüdür.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="c1ac2-110">Bu, varlık modelinizi kolayca tanımlamanızı ve veritabanını ilk veya modeli kullanarak veritabanınıza eşlemenizi sağlar, böylece sorguları hemen yazmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="c1ac2-111">EF Core için: 2.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-111">For EF Core: 2.</span></span>

[<span data-ttu-id="c1ac2-112">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="c1ac2-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="c1ac2-113">Varlık geliştiriciyi kaldırma</span><span class="sxs-lookup"><span data-stu-id="c1ac2-113">Devart Entity Developer</span></span>

<span data-ttu-id="c1ac2-114">Varlık geliştiricisi, ADO.NET Entity Framework, Nhazırda beklet, LinqConnect, Telerik veri erişimi ve LINQ to SQL için güçlü bir ORM tasarlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="c1ac2-115">EF Core modellerinin görsel olarak tasarlanmasını, ilk veya veritabanı ilk yaklaşımı ve C# ya da kod üretimini Visual Basic destekler.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="c1ac2-116">EF Core için: 2.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-116">For EF Core: 2.</span></span>

[<span data-ttu-id="c1ac2-117">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="c1ac2-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="c1ac2-118">EF Core güç araçları</span><span class="sxs-lookup"><span data-stu-id="c1ac2-118">EF Core Power Tools</span></span>

<span data-ttu-id="c1ac2-119">EF Core güç araçları, basit bir kullanıcı arabiriminde çeşitli EF Core tasarım zamanı görevleri sunan bir Visual Studio uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-119">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="c1ac2-120">Bu, var olan veritabanlarından DbContext ve varlık sınıflarının tersine mühendislik ve [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), veritabanı geçişleri yönetimi ve model görselleştirmeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-120">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="c1ac2-121">EF Core için: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-121">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c1ac2-122">GitHub wiki</span><span class="sxs-lookup"><span data-stu-id="c1ac2-122">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="c1ac2-123">Entity Framework görsel düzenleyici</span><span class="sxs-lookup"><span data-stu-id="c1ac2-123">Entity Framework Visual Editor</span></span>

<span data-ttu-id="c1ac2-124">Entity Framework Visual Düzenleyicisi, EF 6 ve EF Core sınıflarının görsel tasarımı için bir ORM Tasarımcısı ekleyen bir Visual Studio uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-124">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="c1ac2-125">Kod, T4 şablonları kullanılarak oluşturulur, bu nedenle, herhangi bir gereksinimlerinize uyacak şekilde özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-125">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="c1ac2-126">Devralma, tek yönlü ve çift yönlü ilişkilendirmeler, numaralandırmalar ve sınıflarınızın renk Kodlayabilme ve tasarımınızın potansiyel olarak büyük bir kısmını açıklamak için metin blokları ekleme imkanını destekler.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-126">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="c1ac2-127">EF Core için: 2.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-127">For EF Core: 2.</span></span>

[<span data-ttu-id="c1ac2-128">Marketplace</span><span class="sxs-lookup"><span data-stu-id="c1ac2-128">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="c1ac2-129">Katfactory</span><span class="sxs-lookup"><span data-stu-id="c1ac2-129">CatFactory</span></span>

<span data-ttu-id="c1ac2-130">CatFactory, bir SQL Server veritabanından DbContext sınıflarının, varlıkların, eşleme yapılandırmalarının ve depo sınıflarının oluşturulmasını otomatik hale getirebilen .NET Core için bir yapı iskelesi altyapısıdır.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-130">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="c1ac2-131">EF Core için: 2.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-131">For EF Core: 2.</span></span>

[<span data-ttu-id="c1ac2-132">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-132">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="c1ac2-133">LoreSoft 'ın Entity Framework Core Oluşturucu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-133">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="c1ac2-134">Entity Framework Core Generator (EFG), `dotnet ef dbcontext scaffold`gibi var olan bir veritabanından EF Core modeller oluşturabilen, ancak bölge değişikliği aracılığıyla veya eşleme dosyalarını ayrıştırarak güvenli kod yeniden [oluşturmayı](https://efg.loresoft.com/en/latest/regeneration/) da destekleyen bir .NET Core CLI aracıdır.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-134">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="c1ac2-135">Bu araç, görünüm modellerinin, doğrulamanın ve nesne Eşleyici kodunun oluşturulmasını destekler.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-135">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="c1ac2-136">EF Core için: 2.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-136">For EF Core: 2.</span></span>

<span data-ttu-id="c1ac2-137">[Eğitim](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[belgeleri](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="c1ac2-137">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="c1ac2-138">Uzantıları</span><span class="sxs-lookup"><span data-stu-id="c1ac2-138">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="c1ac2-139">Microsoft. EntityFrameworkCore. oto geçmişi</span><span class="sxs-lookup"><span data-stu-id="c1ac2-139">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="c1ac2-140">EF Core tarafından gerçekleştirilen veri değişikliklerinin bir geçmiş tablosuna otomatik olarak kaydedilmesini sağlayan bir eklenti kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-140">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="c1ac2-141">EF Core için: 2.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-141">For EF Core: 2.</span></span>

[<span data-ttu-id="c1ac2-142">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-142">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="c1ac2-143">EFSecondLevelCache. Core</span><span class="sxs-lookup"><span data-stu-id="c1ac2-143">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="c1ac2-144">Aynı sorguların sonraki yürütmelerinin veritabanına erişmeyi ve verileri doğrudan önbellekten almasını önlemek için, EF Core sorgularının sonuçlarının ikinci düzey bir önbellekte depolanmasını sağlayan bir uzantı.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-144">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="c1ac2-145">EF Core için: 2.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-145">For EF Core: 2.</span></span>

[<span data-ttu-id="c1ac2-146">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-146">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="c1ac2-147">Geco</span><span class="sxs-lookup"><span data-stu-id="c1ac2-147">Geco</span></span>

<span data-ttu-id="c1ac2-148">Geco (Oluşturucu konsolu), .NET Core üzerinde çalışan ve kod oluşturma için enterpolasyonlu dizeler kullanan C# bir konsol projesine dayalı basit bir kod Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-148">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="c1ac2-149">Geco, plurmaya, sinleştirme ve düzenlenebilir şablonlara yönelik desteğe sahip EF Core için bir ters model Oluşturucu içerir.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-149">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="c1ac2-150">Ayrıca, bir çekirdek veri betik Oluşturucu, bir komut dosyası Çalıştırıcısı ve bir veritabanı temizleyicisi de sağlar.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-150">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="c1ac2-151">EF Core için: 2.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-151">For EF Core: 2.</span></span>

[<span data-ttu-id="c1ac2-152">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-152">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="c1ac2-153">EntityFrameworkCore. Scafkatlama. Handleçubuklar</span><span class="sxs-lookup"><span data-stu-id="c1ac2-153">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="c1ac2-154">Handleçubuklar şablonlarıyla Entity Framework Core toolzincirini kullanarak var olan bir veritabanından ters mühendislik uygulanmış sınıfların özelleştirilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-154">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="c1ac2-155">EF Core için: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-155">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c1ac2-156">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-156">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="c1ac2-157">Newınlınq. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="c1ac2-157">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="c1ac2-158">Neınlinq, işlevleri yeniden kullanmayı, sorguları yeniden yazmayı ve çevrilebilir koşullara ve seçicileri kullanarak dinamik sorgular oluşturmayı etkinleştirmek için Entity Framework gibi LINQ sağlayıcılarını genişletir.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-158">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="c1ac2-159">EF Core için: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-159">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c1ac2-160">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-160">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="c1ac2-161">Microsoft. EntityFrameworkCore. UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="c1ac2-161">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="c1ac2-162">Microsoft. EntityFrameworkCore 'un depoyu, iş biçimlerini ve dağıtılmış işlem desteklenen birden çok veritabanını desteklemesi için bir eklentisi.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-162">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="c1ac2-163">EF Core için: 2.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-163">For EF Core: 2.</span></span>

[<span data-ttu-id="c1ac2-164">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-164">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="c1ac2-165">EFCore. BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="c1ac2-165">EFCore.BulkExtensions</span></span>

<span data-ttu-id="c1ac2-166">Toplu işlemler için EF Core uzantıları (INSERT, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="c1ac2-166">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="c1ac2-167">EF Core için: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-167">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c1ac2-168">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-168">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="c1ac2-169">Brıcelam. EntityFrameworkCore. Pluralizer</span><span class="sxs-lookup"><span data-stu-id="c1ac2-169">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="c1ac2-170">Tasarım zamanı plurlaması ekler.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-170">Adds design-time pluralization.</span></span> <span data-ttu-id="c1ac2-171">EF Core için: 2.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-171">For EF Core: 2.</span></span>

[<span data-ttu-id="c1ac2-172">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-172">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="c1ac2-173">Toolbandı. EntityFrameworkCore. ındexattribute</span><span class="sxs-lookup"><span data-stu-id="c1ac2-173">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="c1ac2-174">[Index] özniteliğinin (model oluşturma uzantısı ile) bir önceki örneği.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-174">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="c1ac2-175">EF Core için: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-175">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c1ac2-176">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-176">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="c1ac2-177">EfCore. ınmemoryyardımcıları</span><span class="sxs-lookup"><span data-stu-id="c1ac2-177">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="c1ac2-178">Bellek Içi veritabanı sağlayıcısı EF Core etrafında bir sarmalayıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-178">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="c1ac2-179">İlişkisel bir sağlayıcı gibi çalışır hale getirir.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-179">Makes it act more like a relational provider.</span></span> <span data-ttu-id="c1ac2-180">EF Core için: 2.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-180">For EF Core: 2.</span></span>

[<span data-ttu-id="c1ac2-181">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-181">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="c1ac2-182">EFCore. TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="c1ac2-182">EFCore.TemporalSupport</span></span>

<span data-ttu-id="c1ac2-183">Zamana bağlı destek uygulama.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-183">An implementation of temporal support.</span></span> <span data-ttu-id="c1ac2-184">EF Core için: 2.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-184">For EF Core: 2.</span></span>

[<span data-ttu-id="c1ac2-185">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-185">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="c1ac2-186">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="c1ac2-186">EfCoreTemporalTable</span></span>

<span data-ttu-id="c1ac2-187">Sunulan uzantı yöntemlerini kullanarak, sık kullanılan veritabanınızda kolayca zamana bağlı sorgular gerçekleştirin: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-187">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="c1ac2-188">EF Core için: 3.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-188">For EF Core: 3.</span></span>

[<span data-ttu-id="c1ac2-189">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-189">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="c1ac2-190">EFCore. TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="c1ac2-190">EFCore.TimeTraveler</span></span>

<span data-ttu-id="c1ac2-191">Zaten tanımlamış olduğunuz EF Core kodu, varlıkları ve eşleştirmeleri kullanarak, isteğe bağlı [SQL Server geçmişle](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) karşı tam özellikli Entity Framework Core sorgulara izin verin.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-191">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="c1ac2-192">Kodunuzu `using (TemporalQuery.AsOf(targetDateTime)) {...}`içine sarmalayarak zaman içinde hareket edin.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-192">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="c1ac2-193">EF Core için: 3.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-193">For EF Core: 3.</span></span>

[<span data-ttu-id="c1ac2-194">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-194">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="c1ac2-195">EntityFrameworkCore. TemporalTables</span><span class="sxs-lookup"><span data-stu-id="c1ac2-195">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="c1ac2-196">SQL Server kullanan geliştiricilerin kolayca zamana bağlı tabloları kullanmasına izin veren Entity Framework Core için uzantı kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-196">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="c1ac2-197">EF Core için: 2.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-197">For EF Core: 2.</span></span>

[<span data-ttu-id="c1ac2-198">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-198">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="c1ac2-199">EntityFrameworkCore. önbelleklenebilir</span><span class="sxs-lookup"><span data-stu-id="c1ac2-199">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="c1ac2-200">Yüksek performanslı ikinci düzey bir sorgu önbelleği.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-200">A high-performance second-level query cache.</span></span> <span data-ttu-id="c1ac2-201">EF Core için: 2.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-201">For EF Core: 2.</span></span>

[<span data-ttu-id="c1ac2-202">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="c1ac2-202">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="c1ac2-203">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="c1ac2-203">Entity Framework Plus</span></span>

<span data-ttu-id="c1ac2-204">DbContext dosyanızı, şunlar gibi özelliklerle genişletir: filtre, denetim, önbelleğe alma, sorgu gelecekteki, toplu silme, toplu güncelleştirme ve daha fazlasını Içerir.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-204">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="c1ac2-205">EF Core için: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-205">For EF Core: 2, 3.</span></span>

<span data-ttu-id="c1ac2-206">[Web sitesi](https://entityframework-plus.net/)
[GitHub deposu](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="c1ac2-206">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="c1ac2-207">Entity Framework uzantıları</span><span class="sxs-lookup"><span data-stu-id="c1ac2-207">Entity Framework Extensions</span></span>

<span data-ttu-id="c1ac2-208">DbContext uygulamanızı yüksek performanslı toplu işlemlerle genişletir: BulkSaveChanges, Bulkınsert, BulkUpdate, BulkDelete, BulkMerge ve daha fazlası.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-208">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="c1ac2-209">EF Core için: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c1ac2-209">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c1ac2-210">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="c1ac2-210">Website</span></span>](https://entityframework-extensions.net/)
