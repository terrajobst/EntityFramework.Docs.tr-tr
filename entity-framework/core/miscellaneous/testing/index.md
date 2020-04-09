---
title: EF Core - EF Core kullanarak bileşenleri test edin
description: EF Core kullanan uygulamaları test etmek için farklı yaklaşımlar
author: ajcvickers
ms.date: 03/23/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: b1ab37ebb0a3aae09d5d5b225f746cf83dfba170
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634245"
---
# <a name="testing-code-that-uses-ef-core"></a><span data-ttu-id="ce830-103">EF Core kullanan test kodu</span><span class="sxs-lookup"><span data-stu-id="ce830-103">Testing code that uses EF Core</span></span>

<span data-ttu-id="ce830-104">Veritabanına erişen sınama kodu şunları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="ce830-104">Testing code that accesses a database requires either:</span></span>
* <span data-ttu-id="ce830-105">Üretimde kullanılan aynı veritabanı sistemine karşı sorguları ve güncelleştirmeleri çalıştırma.</span><span class="sxs-lookup"><span data-stu-id="ce830-105">Running queries and updates against the same database system used in production.</span></span>
* <span data-ttu-id="ce830-106">Sorguları ve güncelleştirmeleri yönetmek daha kolay olan diğer bazı veritabanı sistemine karşı çalıştırmak.</span><span class="sxs-lookup"><span data-stu-id="ce830-106">Running queries and updates against some other easier to manage database system.</span></span>
* <span data-ttu-id="ce830-107">Bir veritabanını kullanmaktan kaçınmak için test çiftlerini veya başka bir mekanizmayı kullanma.</span><span class="sxs-lookup"><span data-stu-id="ce830-107">Using test doubles or some other mechanism to avoid using a database at all.</span></span>

<span data-ttu-id="ce830-108">Bu belge, bu seçeneklerin her birinde yer alan dengeleri özetler ve EF Core'un her yaklaşımda nasıl kullanılabileceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="ce830-108">This document outlines the trade offs involved in each of these choices and shows how EF Core can be used with each approach.</span></span>  

## <a name="all-database-providers-are-not-equal"></a><span data-ttu-id="ce830-109">Tüm veritabanı sağlayıcıları eşit değildir</span><span class="sxs-lookup"><span data-stu-id="ce830-109">All database providers are not equal</span></span>

<span data-ttu-id="ce830-110">EF Core'un temel veritabanı sisteminin her yönünü özetlemek için tasarlanmadığını anlamak çok önemlidir.</span><span class="sxs-lookup"><span data-stu-id="ce830-110">It is very important to understand that EF Core is not designed to abstract every aspect of the underlying database system.</span></span>
<span data-ttu-id="ce830-111">Bunun yerine, EF Core herhangi bir veritabanı sistemi ile kullanılabilecek desenler ve kavramlar ortak bir kümesidir.</span><span class="sxs-lookup"><span data-stu-id="ce830-111">Instead, EF Core is a common set of patterns and concepts that can be used with any database system.</span></span>
<span data-ttu-id="ce830-112">EF Core veritabanı sağlayıcıları daha sonra veritabanına özgü davranış ve işlevleri bu ortak çerçeve üzerinde katman.</span><span class="sxs-lookup"><span data-stu-id="ce830-112">EF Core database providers then layer database-specific behavior and functionality over this common framework.</span></span>
<span data-ttu-id="ce830-113">Bu, her veritabanı sisteminin, uygun olduğu durumlarda diğer veritabanı sistemleriyle ortak lığı korurken en iyi yaptığı şeyi yapmasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="ce830-113">This allows each database system to do what it does best while still maintaining commonality, where appropriate, with other database systems.</span></span> 

<span data-ttu-id="ce830-114">Temelde, bu veritabanı sağlayıcısı nı değiştirmenin EF Core davranışını değiştireceği ve uygulamanın davranıştaki tüm farklılıkları açıkça hesaba ketmediği sürece doğru çalışması beklenmediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ce830-114">Fundamentally, this means that switching out the database provider will change EF Core behavior and the application can't be expected to function correctly unless it explicitly accounts for all differences in behavior.</span></span>
<span data-ttu-id="ce830-115">Bu söyleniyor, birçok durumda ilişkisel veritabanları arasında ortak yüksek derecede olduğu için bu iş çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="ce830-115">That being said, in many cases doing this will work because there is a high degree of commonality amongst relational databases.</span></span>
<span data-ttu-id="ce830-116">Bu iyi ve kötü.</span><span class="sxs-lookup"><span data-stu-id="ce830-116">This is good and bad.</span></span>
<span data-ttu-id="ce830-117">Veritabanları arasında taşıma nispeten kolay olabilir, çünkü iyi.</span><span class="sxs-lookup"><span data-stu-id="ce830-117">Good because moving between databases can be relatively easy.</span></span>
<span data-ttu-id="ce830-118">Uygulama yeni veritabanı sistemine karşı tam olarak sınanmazsa yanlış bir güvenlik hissi verebilir çünkü kötü.</span><span class="sxs-lookup"><span data-stu-id="ce830-118">Bad because it can give a false sense of security if the application is not fully tested against the new database system.</span></span>  

## <a name="approach-1-production-database-system"></a><span data-ttu-id="ce830-119">Yaklaşım 1: Üretim veritabanı sistemi</span><span class="sxs-lookup"><span data-stu-id="ce830-119">Approach 1: Production database system</span></span>

<span data-ttu-id="ce830-120">Önceki bölümde açıklandığı gibi, üretimde ne çalışır sınama emin olmak için tek yolu aynı veritabanı sistemini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="ce830-120">As described in the previous section, the only way to be sure you are testing what runs in production is to use the same database system.</span></span>
<span data-ttu-id="ce830-121">Örneğin, dağıtılan uygulama SQL Azure kullanıyorsa, sql azure'a karşı da sınama yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ce830-121">For example, if the deployed application uses SQL Azure, then testing should also be done against SQL Azure.</span></span>

<span data-ttu-id="ce830-122">Ancak, kod üzerinde etkin olarak çalışırken her geliştiricinin SQL Azure'a karşı testler çalıştırması hem yavaş hem de pahalı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ce830-122">However, having every developer run tests against SQL Azure while actively working on the code would be both slow and expensive.</span></span>
<span data-ttu-id="ce830-123">Bu, bu yaklaşımlar boyunca ilgili ana ticareti göstermektedir: test verimliliğini artırmak için üretim veritabanı sisteminden sapmak ne zaman uygundur?</span><span class="sxs-lookup"><span data-stu-id="ce830-123">This illustrates the main trade off involved throughout these approaches: when is it appropriate to deviate from the production database system so as to improve test efficiency?</span></span>

<span data-ttu-id="ce830-124">Neyse ki, bu durumda cevap oldukça kolaydır: geliştirici testi için yerel veya şirket içi SQL Server kullanın.</span><span class="sxs-lookup"><span data-stu-id="ce830-124">Luckily, in this case the answer is quite easy: use local or on-premises SQL Server for developer testing.</span></span>
<span data-ttu-id="ce830-125">SQL Azure ve SQL Server son derece benzer, bu nedenle SQL Server karşı test genellikle makul bir ticaret kapalıdır.</span><span class="sxs-lookup"><span data-stu-id="ce830-125">SQL Azure and SQL Server are extremely similar, so testing against SQL Server is usually a reasonable trade off.</span></span>
<span data-ttu-id="ce830-126">Bununla ilgili olarak, üretime geçmeden önce SQL Azure'a karşı testler çalıştırmak hala akıllıca olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ce830-126">That being said, it is still wise to run tests against SQL Azure itself before going into production.</span></span>
 
### <a name="localdb"></a><span data-ttu-id="ce830-127">Localdb</span><span class="sxs-lookup"><span data-stu-id="ce830-127">LocalDb</span></span> 

<span data-ttu-id="ce830-128">Tüm büyük veritabanı sistemleri yerel sınama için "Developer Edition" bir çeşit var.</span><span class="sxs-lookup"><span data-stu-id="ce830-128">All the major database systems have some form of "Developer Edition" for local testing.</span></span>
<span data-ttu-id="ce830-129">SQL Server da [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15)adlı bir özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="ce830-129">SQL Server also also has a feature called [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span></span>
<span data-ttu-id="ce830-130">LocalDb'ın birincil avantajı, veritabanı örneğini isteğe bağlı olarak döndürmesidir.</span><span class="sxs-lookup"><span data-stu-id="ce830-130">The primary advantage of LocalDb is that it spins up the database instance on demand.</span></span>
<span data-ttu-id="ce830-131">Bu, testler çalıştırmıyorsanız bile makinenizde çalışan bir veritabanı hizmetiolmasını önler.</span><span class="sxs-lookup"><span data-stu-id="ce830-131">This avoids having a database service running on your machine even when you're not running tests.</span></span>

<span data-ttu-id="ce830-132">LocalDb sorunları olmadan değil:</span><span class="sxs-lookup"><span data-stu-id="ce830-132">LocalDb is not without it's issues:</span></span>
* <span data-ttu-id="ce830-133">[SQL Server Developer Edition'ın](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) yaptığı her şeyi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="ce830-133">It doesn't support everything that [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) does.</span></span>
* <span data-ttu-id="ce830-134">Linux'ta kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="ce830-134">It isn't available on Linux.</span></span>
* <span data-ttu-id="ce830-135">Hizmet büküldükçe ilk test çalışmasında gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="ce830-135">It can cause lag on first test run as the service is spun up.</span></span>

<span data-ttu-id="ce830-136">Şahsen, benim dev makine üzerinde çalışan bir veritabanı hizmeti olan bir sorun hiç bulamadım ve ben genellikle yerine Developer Edition kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="ce830-136">Personally, I've never found it a problem having a database service running on my dev machine and I would generally recommend using Developer Edition instead.</span></span>
<span data-ttu-id="ce830-137">Ancak, bazı insanlar için uygun olabilir, özellikle daha az güçlü dev makineleri.</span><span class="sxs-lookup"><span data-stu-id="ce830-137">However, it may be appropriate for some people, especially on less powerful dev machines.</span></span>  

## <a name="approach-2-sqlite"></a><span data-ttu-id="ce830-138">Yaklaşım 2: SQLite</span><span class="sxs-lookup"><span data-stu-id="ce830-138">Approach 2: SQLite</span></span>

<span data-ttu-id="ce830-139">EF Core, SQL Server sağlayıcısını öncelikle yerel bir SQL Server örneğine karşı çalıştırarak sınar.</span><span class="sxs-lookup"><span data-stu-id="ce830-139">EF Core tests the SQL Server provider primarily by running it against a local SQL Server instance.</span></span>
<span data-ttu-id="ce830-140">Bu testler, hızlı bir makinede birkaç dakika içinde on binlerce sorgu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ce830-140">These tests run tens of thousands of queries in a couple of minutes on a fast machine.</span></span>
<span data-ttu-id="ce830-141">Bu, gerçek veritabanı sistemini kullanmanın bir performans çözümü olabileceğini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="ce830-141">This illustrates that using the real database system can be a performant solution.</span></span>
<span data-ttu-id="ce830-142">Bazı hafif veritabanı kullanarak hızlı testleri çalıştırmak için tek yol olduğunu bir efsanedir.</span><span class="sxs-lookup"><span data-stu-id="ce830-142">It is a myth that using some lighter-weight database is the only way to run tests quickly.</span></span>

<span data-ttu-id="ce830-143">Bununla söyleniyor, ya her ne sebeple olursa olsun, üretim veritabanı sisteminize yakın bir şeye karşı testler çalıştıramıyorsanız?</span><span class="sxs-lookup"><span data-stu-id="ce830-143">That being said, what if for whatever reason you can't run tests against something close to your production database system?</span></span>
<span data-ttu-id="ce830-144">Bir sonraki en iyi seçenek benzer işlevsellik ile bir şey kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="ce830-144">The next best choice is to use something with similar functionality.</span></span>
<span data-ttu-id="ce830-145">Bu genellikle başka bir ilişkisel veritabanı anlamına gelir, hangi [SQLite](https://sqlite.org/index.html) bariz bir seçimdir.</span><span class="sxs-lookup"><span data-stu-id="ce830-145">This usually means another relational database, for which [SQLite](https://sqlite.org/index.html) is the obvious choice.</span></span>

<span data-ttu-id="ce830-146">SQLite iyi bir seçimdir çünkü:</span><span class="sxs-lookup"><span data-stu-id="ce830-146">SQLite is a good choice because:</span></span>
* <span data-ttu-id="ce830-147">Uygulamanızla birlikte çalışır ve böylece düşük ek yükü vardır.</span><span class="sxs-lookup"><span data-stu-id="ce830-147">It runs in-process with your application and so has low overhead.</span></span>
* <span data-ttu-id="ce830-148">Veritabanları için basit, otomatik olarak oluşturulan dosyaları kullanır ve bu nedenle veritabanı yönetimi gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="ce830-148">It uses simple, automatically created files for databases, and so doesn't require database management.</span></span>
* <span data-ttu-id="ce830-149">Dosya oluşturmayı bile önleyen bir bellek modu vardır.</span><span class="sxs-lookup"><span data-stu-id="ce830-149">It has an in-memory mode that avoids even the file creation.</span></span>

<span data-ttu-id="ce830-150">Ancak, şunu unutmayın:</span><span class="sxs-lookup"><span data-stu-id="ce830-150">However, remember that:</span></span>
* <span data-ttu-id="ce830-151">SQLite kaçınılmazlık üretim veritabanı sistemi yaptığı her şeyi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="ce830-151">SQLite inevitability doesn't support everything that your production database system does.</span></span>
* <span data-ttu-id="ce830-152">SQLite, bazı sorgular için üretim veritabanı sisteminizden farklı davranacaktır.</span><span class="sxs-lookup"><span data-stu-id="ce830-152">SQLite will behave differently than your production database system for some queries.</span></span>

<span data-ttu-id="ce830-153">Yani bazı test için SQLite kullanıyorsanız, aynı zamanda gerçek veritabanı sistemine karşı test emin olun.</span><span class="sxs-lookup"><span data-stu-id="ce830-153">So if you do use SQLite for some testing, make sure to also test against your real database system.</span></span>

<span data-ttu-id="ce830-154">EF Core özel kılavuzu için [SQLite ile Test'e](xref:core/miscellaneous/testing/sqlite) bakın.</span><span class="sxs-lookup"><span data-stu-id="ce830-154">See [Testing with SQLite](xref:core/miscellaneous/testing/sqlite) for EF Core specific guidance.</span></span> 

## <a name="approach-3-the-ef-core-in-memory-database"></a><span data-ttu-id="ce830-155">Yaklaşım 3: EF Core bellek veritabanı</span><span class="sxs-lookup"><span data-stu-id="ce830-155">Approach 3: The EF Core in-memory database</span></span>

<span data-ttu-id="ce830-156">EF Core, EF Core'un kendi iç testlerinde kullandığımız bir bellek içi veritabanıyla birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="ce830-156">EF Core comes with an in-memory database that we use for internal testing of EF Core itself.</span></span>
<span data-ttu-id="ce830-157">Bu veritabanı genel **olarak EF Core kullanan uygulamaları test etmek için uygun değildir.**</span><span class="sxs-lookup"><span data-stu-id="ce830-157">This database is in general **not suitable as a substitute for testing applications that use EF Core**.</span></span> <span data-ttu-id="ce830-158">Daha ayrıntılı şekilde belirtmek gerekirse:</span><span class="sxs-lookup"><span data-stu-id="ce830-158">Specifically:</span></span>
* <span data-ttu-id="ce830-159">İlişkisel bir veritabanı değildir</span><span class="sxs-lookup"><span data-stu-id="ce830-159">It is not a relational database</span></span>
* <span data-ttu-id="ce830-160">Hareketleri desteklemiyor</span><span class="sxs-lookup"><span data-stu-id="ce830-160">It doesn't support transactions</span></span>
* <span data-ttu-id="ce830-161">Performans için optimize edilmedi</span><span class="sxs-lookup"><span data-stu-id="ce830-161">It is not optimized for performance</span></span>

<span data-ttu-id="ce830-162">EF Core dahili lerini test ederken bunların hiçbiri çok önemli değildir, çünkü veritabanının testle alakasız olduğu yerlerde kullanırız.</span><span class="sxs-lookup"><span data-stu-id="ce830-162">None of this is very important when testing EF Core internals because we use it specifically where the database is irrelevant to the test.</span></span>
<span data-ttu-id="ce830-163">Öte yandan, bu şeyler EF Core kullanan bir uygulama test ederken çok önemli olma eğilimindedir.</span><span class="sxs-lookup"><span data-stu-id="ce830-163">On the other hand, these things tend to be very important when testing an application that uses EF Core.</span></span>

## <a name="unit-testing"></a><span data-ttu-id="ce830-164">Birim testi</span><span class="sxs-lookup"><span data-stu-id="ce830-164">Unit testing</span></span>

<span data-ttu-id="ce830-165">Veritabanından bazı verileri kullanması gerekebilecek, ancak veritabanı etkileşimlerini doğal olarak sınamayan bir iş mantığı parçasını sınamayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="ce830-165">Consider testing a piece of business logic that might need to use some data from a database, but is not inherently testing the database interactions.</span></span>
<span data-ttu-id="ce830-166">Bir seçenek sahte veya sahte gibi bir [test çift](https://en.wikipedia.org/wiki/Test_double) kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="ce830-166">One option is to use a [test double](https://en.wikipedia.org/wiki/Test_double) such as a mock or fake.</span></span>

<span data-ttu-id="ce830-167">EF Core'un dahili testleri için test çiftleri kullanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="ce830-167">We use test doubles for internal testing of EF Core.</span></span>
<span data-ttu-id="ce830-168">Ancak, biz DbContext veya IQueryable alay asla.</span><span class="sxs-lookup"><span data-stu-id="ce830-168">However, we never try to mock DbContext or IQueryable.</span></span>
<span data-ttu-id="ce830-169">Bunu yapmak zor, hantal ve kırılgandır.</span><span class="sxs-lookup"><span data-stu-id="ce830-169">Doing so is difficult, cumbersome, and fragile.</span></span>
<span data-ttu-id="ce830-170">**Bunu yapma.**</span><span class="sxs-lookup"><span data-stu-id="ce830-170">**Don't do it.**</span></span>

<span data-ttu-id="ce830-171">Bunun yerine, DbContext kullanan bir şeyi birim olarak sınarken bellek içi veritabanını kullanırız.</span><span class="sxs-lookup"><span data-stu-id="ce830-171">Instead we use the in-memory database when unit testing something that uses DbContext.</span></span>
<span data-ttu-id="ce830-172">Bu durumda, sınama veritabanı davranışına bağlı olmadığından bellek içi veritabanını kullanmak uygundur.</span><span class="sxs-lookup"><span data-stu-id="ce830-172">In this case using the in-memory database is appropriate because the test is not dependent on database behavior.</span></span>
<span data-ttu-id="ce830-173">Bunu gerçek veritabanı sorgularını veya güncelleştirmelerini sınamak için yapmayın.</span><span class="sxs-lookup"><span data-stu-id="ce830-173">Just don't do this to test actual database queries or updates.</span></span>   

<span data-ttu-id="ce830-174">Birim sınama için bellek içi veritabanını kullanma konusunda EF Core'a özgü kılavuz için [bellek içi sağlayıcıyla sınama](xref:core/miscellaneous/testing/in-memory) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="ce830-174">See [Testing with the in-memory provider](xref:core/miscellaneous/testing/in-memory) for EF Core specific guidance on using the in-memory database for unit testing.</span></span>
