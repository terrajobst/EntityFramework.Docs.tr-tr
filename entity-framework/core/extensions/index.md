---
title: Araçlar ve uzantılar - EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: d432ca36c166f7baf71253709bf58b1f5428a11a
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562513"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="0a143-102">EF Core Araçlar ve uzantılar</span><span class="sxs-lookup"><span data-stu-id="0a143-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="0a143-103">Bu araçlar ve uzantılar için Entity Framework Core 2.0 ve daha sonra ek işlevsellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="0a143-103">These tools and extensions provide additional functionality for Entity Framework Core 2.0 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="0a143-104">Uzantılar, çeşitli kaynaklardan tarafından oluşturulur ve Entity Framework Core projesinin bir parçası korunmaz.</span><span class="sxs-lookup"><span data-stu-id="0a143-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="0a143-105">Bir üçüncü taraf uzantı dikkate alındığında, lisanslama, uyumluluk, destek, gereksinimlerinizi karşıladığından emin olmak için vb. kalitesini değerlendirmek emin olun.</span><span class="sxs-lookup"><span data-stu-id="0a143-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="0a143-106">Araçlar</span><span class="sxs-lookup"><span data-stu-id="0a143-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="0a143-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="0a143-107">LLBLGen Pro</span></span>

<span data-ttu-id="0a143-108">LLBLGen Pro çözüm Entity Framework ve Entity Framework Core desteğiyle birlikte modeling bir varlıktır.</span><span class="sxs-lookup"><span data-stu-id="0a143-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="0a143-109">Kolayca varlık modelinizi tanımlayın ve veritabanınıza ilk kullanarak veritabanı eşleme sağlar veya sorgu hemen yazma başlangıç yapmanıza yardımcı olacak ilk olarak, model.</span><span class="sxs-lookup"><span data-stu-id="0a143-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="0a143-110">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="0a143-110">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="0a143-111">Devart varlık Geliştirici</span><span class="sxs-lookup"><span data-stu-id="0a143-111">Devart Entity Developer</span></span>

<span data-ttu-id="0a143-112">Varlık, ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access ve LINQ to SQL için güçlü bir ORM Tasarımcısı geliştiricidir.</span><span class="sxs-lookup"><span data-stu-id="0a143-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="0a143-113">Bu görsel olarak EF Core modelleri tasarlama destekler, ilk modelini kullanarak veya veritabanı ilk yaklaşır, ve C# veya Visual Basic kod oluşturma.</span><span class="sxs-lookup"><span data-stu-id="0a143-113">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> 

[<span data-ttu-id="0a143-114">Web sitesi</span><span class="sxs-lookup"><span data-stu-id="0a143-114">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="0a143-115">EF Core güç araçları</span><span class="sxs-lookup"><span data-stu-id="0a143-115">EF Core Power Tools</span></span>

<span data-ttu-id="0a143-116">EF Core Power Tools çeşitli EF Core tasarım zamanı görevleri basit bir kullanıcı arabirimi kullanıma sunan bir Visual Studio 2017 uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="0a143-116">EF Core Power Tools is a Visual Studio 2017 extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="0a143-117">DbContext ve varlık sınıflarını mevcut veritabanlarının tersine mühendisliğini içerir ve [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), veritabanı geçişlerini ve model görselleştirmeler yönetimi.</span><span class="sxs-lookup"><span data-stu-id="0a143-117">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span>

[<span data-ttu-id="0a143-118">GitHub wiki</span><span class="sxs-lookup"><span data-stu-id="0a143-118">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="0a143-119">Entity Framework Visual Düzenleyicisi</span><span class="sxs-lookup"><span data-stu-id="0a143-119">Entity Framework Visual Editor</span></span>

<span data-ttu-id="0a143-120">Entity Framework Visual Düzenleyicisi bir görsel tasarım EF 6 ve EF Core sınıfları için ORM Tasarımcısı ekleyen bir Visual Studio uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="0a143-120">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="0a143-121">Kod, her türlü ihtiyacı karşılamak için özelleştirilebilir için T4 şablonlarını kullanarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0a143-121">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="0a143-122">Bu, devralma, tek yönlü ve çift yönlü ilişkilendirmeleri, numaralandırmalar ve sınıflarınızı renk kodu ve metin blokları ekleme olanağı tasarımınızı potansiyel olarak karıştıran bölümlerini açıklamak için destekler.</span><span class="sxs-lookup"><span data-stu-id="0a143-122">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span>

[<span data-ttu-id="0a143-123">Market</span><span class="sxs-lookup"><span data-stu-id="0a143-123">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="0a143-124">CatFactory</span><span class="sxs-lookup"><span data-stu-id="0a143-124">CatFactory</span></span>

<span data-ttu-id="0a143-125">CatFactory DbContext sınıfları, varlıklar, eşleme yapılandırmalarını ve bir SQL Server veritabanı sınıflarından depo oluşturulmasını otomatik hale getirebilirsiniz .NET Core için yapı iskelesi altyapısıdır.</span><span class="sxs-lookup"><span data-stu-id="0a143-125">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span>

[<span data-ttu-id="0a143-126">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-126">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="0a143-127">LoreSoft'ın Entity Framework Core Oluşturucusu</span><span class="sxs-lookup"><span data-stu-id="0a143-127">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="0a143-128">Entity Framework Core Generator (efg), EF Core modelleri çok gibi varolan bir veritabanından oluşturabilen bir .NET Core CLI aracı `dotnet ef dbcontext scaffold`, ancak güvenli kod da destekler [anahtarınızın yeniden oluşturulması](https://efg.loresoft.com/en/latest/regeneration/) bölge değiştirme veya aracılığıyla ayrıştırma eşleme dosyaları.</span><span class="sxs-lookup"><span data-stu-id="0a143-128">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="0a143-129">Bu araç, görünüm modelleri oluşturma ve doğrulama nesne Eşleyici kodu destekler.</span><span class="sxs-lookup"><span data-stu-id="0a143-129">This tool supports generating view models, validation, and object mapper code.</span></span> 

<span data-ttu-id="0a143-130">[Öğretici](http://www.loresoft.com/Generate-ASP-NET-Web-API)
[belgeleri](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="0a143-130">[Tutorial](http://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="0a143-131">Uzantıları</span><span class="sxs-lookup"><span data-stu-id="0a143-131">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="0a143-132">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="0a143-132">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="0a143-133">Bir eklenti kitaplık, EF Core tarafından gerçekleştirilen bir geçmiş tablosuna veri değişikliklerini kaydı otomatik olarak etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="0a143-133">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span>

[<span data-ttu-id="0a143-134">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-134">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="0a143-135">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="0a143-135">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="0a143-136">.NET Core / .NET standart bağlantı noktası, EF Core ile zaman uyumsuz desteği içeren System.Linq.Dynamic.</span><span class="sxs-lookup"><span data-stu-id="0a143-136">A .NET Core / .NET Standard port of System.Linq.Dynamic that includes async support with EF Core.</span></span>
<span data-ttu-id="0a143-137">System.Linq.Dynamic dinamik olarak gelen kod yerine dize ifadeleri LINQ sorgularının nasıl oluşturulacağını gösteren bir Microsoft örnek olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0a143-137">System.Linq.Dynamic originated as a Microsoft sample that shows how to construct LINQ queries dynamically from string expressions rather than code.</span></span>

[<span data-ttu-id="0a143-138">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-138">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="0a143-139">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="0a143-139">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="0a143-140">Aynı sorgu sonraki yürütmeleri veritabanına erişirken önlemek ve doğrudan önbellekten veri almak, ikinci düzey önbelleğine, EF Core sorguların sonuçlarını depolamak sağlayan uzantısı.</span><span class="sxs-lookup"><span data-stu-id="0a143-140">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span>

[<span data-ttu-id="0a143-141">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-141">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="0a143-142">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="0a143-142">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="0a143-143">Bu kitaplık (bileşik anahtarlar dahil) birincil anahtar değerlerini alınırken bir sözlük olarak herhangi bir varlık izin verir.</span><span class="sxs-lookup"><span data-stu-id="0a143-143">This library allows retrieving the values of primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="0a143-144">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-144">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="0a143-145">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="0a143-145">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="0a143-146">Bu kitaplık, varlık özelliklerini öğesinin özgün değerleri kesin türü belirtilmiş erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="0a143-146">This library enables strongly typed access to the original values of entity properties.</span></span> 

[<span data-ttu-id="0a143-147">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-147">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="0a143-148">Geco</span><span class="sxs-lookup"><span data-stu-id="0a143-148">Geco</span></span>

<span data-ttu-id="0a143-149">Geco (Oluşturucusu Konsolu) olan .NET Core üzerinde çalışır ve kullanan bir konsol projesi dayalı bir basit Kod Oluşturucu C# Ara değerli dizeler için kod oluşturma.</span><span class="sxs-lookup"><span data-stu-id="0a143-149">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="0a143-150">Geco ters model Oluşturucu EF Core için çoğullaştırma singularization ve düzenlenebilir şablonlar için destek içerir.</span><span class="sxs-lookup"><span data-stu-id="0a143-150">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="0a143-151">Ayrıca bir çekirdek veri betiği Oluşturucu, bir betik Çalıştırıcı ve bir veritabanı Temizleyicisi sağlar.</span><span class="sxs-lookup"><span data-stu-id="0a143-151">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span>

[<span data-ttu-id="0a143-152">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-152">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="0a143-153">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="0a143-153">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="0a143-154">LinqKit.Microsoft.EntityFrameworkCore bir LINQKit kitaplığı EF Core ile uyumlu sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="0a143-154">LinqKit.Microsoft.EntityFrameworkCore is an EF Core-compatible version of the LINQKit library.</span></span> <span data-ttu-id="0a143-155">LINQKit LINQ için uzantıları SQL ve Entity Framework ileri kullanıcılar için ücretsiz bir kümesidir.</span><span class="sxs-lookup"><span data-stu-id="0a143-155">LINQKit is a free set of extensions for LINQ to SQL and Entity Framework power users.</span></span> <span data-ttu-id="0a143-156">Bu, dinamik koşul ifadeleri oluşturmaya ve sorgularda ifade değişkenleri kullanma gibi gelişmiş işlevleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="0a143-156">It enables advanced functionality like dynamic building of predicate expressions, and using expression variables in subqueries.</span></span>  

[<span data-ttu-id="0a143-157">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-157">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="0a143-158">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="0a143-158">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="0a143-159">NeinLinq yeniden kullanma işlevleri etkinleştirmek için Entity Framework gibi sorguları yeniden yazmak ve çevrilebilir doğrulamaları ve Seçici kullanarak dinamik sorgular oluşturma LINQ sağlayıcıları genişletir.</span><span class="sxs-lookup"><span data-stu-id="0a143-159">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="0a143-160">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-160">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="0a143-161">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="0a143-161">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="0a143-162">Havuz, iş birimi desenleri ve birden çok veritabanı ile birlikte dağıtılmış işlem desteklenen desteklemek Microsoft.EntityFrameworkCore bir eklenti.</span><span class="sxs-lookup"><span data-stu-id="0a143-162">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span>

[<span data-ttu-id="0a143-163">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-163">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="0a143-164">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="0a143-164">EFCore.BulkExtensions</span></span>

<span data-ttu-id="0a143-165">EF Core uzantıları toplu işlemleri (INSERT, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="0a143-165">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="0a143-166">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-166">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="0a143-167">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="0a143-167">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="0a143-168">EF Core için tasarım zamanı çoğullaştırma ekler.</span><span class="sxs-lookup"><span data-stu-id="0a143-168">Adds design-time pluralization to EF Core.</span></span>

[<span data-ttu-id="0a143-169">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-169">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a><span data-ttu-id="0a143-170">PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql</span><span class="sxs-lookup"><span data-stu-id="0a143-170">PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql</span></span>

<span data-ttu-id="0a143-171">Basit senaryoda belirli bir LINQ Sorgu için SQL deyimi EF Core alır bir basit bir genişletme yöntemi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0a143-171">A simple extension method that obtains the SQL statement EF Core would generate for a given LINQ query in simple scenarios.</span></span> <span data-ttu-id="0a143-172">EF Core tek bir LINQ Sorgu için birden fazla SQL deyimi ve parametre değerlerini bağlı olarak farklı SQL deyimleri oluşturabileceğinden ToSql yöntemi basit senaryolar için sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="0a143-172">The ToSql method is limited to simple scenarios because EF Core can generate more than one SQL statement for a single LINQ query, and different SQL statements depending on parameter values.</span></span>

[<span data-ttu-id="0a143-173">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-173">GitHub repository</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="0a143-174">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="0a143-174">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="0a143-175">(Uzantılı modeli oluşturmak için) EF Core için revival [dizin] özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0a143-175">Revival of [Index] attribute for EF Core (with extension for model building).</span></span>

[<span data-ttu-id="0a143-176">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-176">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="0a143-177">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="0a143-177">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="0a143-178">EF Core bellek içi veritabanı sağlayıcısı çevresinde bir sarmalayıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="0a143-178">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="0a143-179">Daha fazla ilişkisel bir sağlayıcı gibi davranmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="0a143-179">Makes it act more like a relational provider.</span></span>

[<span data-ttu-id="0a143-180">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-180">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="0a143-181">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="0a143-181">EFCore.TemporalSupport</span></span>

<span data-ttu-id="0a143-182">EF Core için zamana bağlı desteği uygulaması.</span><span class="sxs-lookup"><span data-stu-id="0a143-182">An implementation of temporal support for EF Core.</span></span>

[<span data-ttu-id="0a143-183">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-183">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="0a143-184">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="0a143-184">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="0a143-185">EF Core için yüksek performanslı ikinci düzey sorgu önbelleği.</span><span class="sxs-lookup"><span data-stu-id="0a143-185">A high-performance second-level query cache for EF Core.</span></span>

[<span data-ttu-id="0a143-186">GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="0a143-186">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)
