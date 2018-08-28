---
title: Geçişleri - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 4a5d6f3798c7af7597f95cebea1aeb9e5e58d277
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996528"
---
<a name="migrations"></a><span data-ttu-id="df7cc-102">Geçişleri</span><span class="sxs-lookup"><span data-stu-id="df7cc-102">Migrations</span></span>
==========
<span data-ttu-id="df7cc-103">Geçişler, EF Core modeliniz veritabanındaki mevcut verileri korurken eşitleyin veritabanına artımlı olarak şema değişiklikleri uygulamak için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="df7cc-103">Migrations provide a way to incrementally apply schema changes to the database to keep it in sync with your EF Core model while preserving existing data in the database.</span></span>

<a name="creating-the-database"></a><span data-ttu-id="df7cc-104">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="df7cc-104">Creating the database</span></span>
---------------------
<span data-ttu-id="df7cc-105">Sonra [ilk modelinizi tanımlanan][1], veritabanı oluşturma zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="df7cc-105">After you've [defined your initial model][1], it's time to create the database.</span></span> <span data-ttu-id="df7cc-106">Bunu yapmak için bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="df7cc-106">To do this, add an initial migration.</span></span>
<span data-ttu-id="df7cc-107">Yükleme [EF Core Araçları] [ 2] ve uygun komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="df7cc-107">Install the [EF Core Tools][2] and run the appropriate command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="df7cc-108">Üç dosyayı projenize altında eklenir **geçişler** dizini:</span><span class="sxs-lookup"><span data-stu-id="df7cc-108">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="df7cc-109">**00000000000000_InitialCreate.cs**--ana geçişleri dosya.</span><span class="sxs-lookup"><span data-stu-id="df7cc-109">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="df7cc-110">Geçiş uygulamak gerekli işlemleri içerir (içinde `Up()`) ve dönmek için (içinde `Down()`).</span><span class="sxs-lookup"><span data-stu-id="df7cc-110">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="df7cc-111">**00000000000000_InitialCreate.Designer.cs**--geçişleri meta veri dosyası.</span><span class="sxs-lookup"><span data-stu-id="df7cc-111">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="df7cc-112">EF tarafından kullanılan bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="df7cc-112">Contains information used by EF.</span></span>
* <span data-ttu-id="df7cc-113">**MyContextModelSnapshot.cs**--bir anlık görüntü geçerli modelinizin.</span><span class="sxs-lookup"><span data-stu-id="df7cc-113">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="df7cc-114">Sonraki geçiş eklerken nelerin değiştiğini belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="df7cc-114">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="df7cc-115">Zaman damgası dosya değişiklikleri ilerleyişini gördüğünüz şekilde kronolojik olarak sıralanan kalmalarını yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="df7cc-115">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="df7cc-116">Geçişleri dosyalarını taşıma ve kendi ad alanı değiştirmek ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="df7cc-116">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="df7cc-117">Yeni geçiş son geçiş bir eşdüzeyi olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="df7cc-117">New migrations are created as siblings of the last migration.</span></span>

<span data-ttu-id="df7cc-118">Ardından, geçiş veritabanına şema oluşturmak için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="df7cc-118">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a><span data-ttu-id="df7cc-119">Başka bir geçiş ekleniyor</span><span class="sxs-lookup"><span data-stu-id="df7cc-119">Adding another migration</span></span>
------------------------
<span data-ttu-id="df7cc-120">EF Core modelinizi değişiklikleri yaptıktan sonra veritabanı şemasını eşitlenmemiş olacaktır. Bu en güncel duruma getirmek için başka bir geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="df7cc-120">After making changes to your EF Core model, the database schema will be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="df7cc-121">Geçiş adı gibi bir işleme iletisi, bir sürüm denetim sisteminde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="df7cc-121">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="df7cc-122">Ben müşteri incelemeleri ürünlerin kaydedilecek değişiklikler yaptıysanız, örneğin, aşağıdaki gibi seçmem *AddProductReviews*.</span><span class="sxs-lookup"><span data-stu-id="df7cc-122">For example, if I made changes to save customer reviews of products, I might choose something like *AddProductReviews*.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="df7cc-123">Geçiş iskele kurulmuş sonra doğruluk gözden geçirmeli ve doğru bir şekilde uygulamak için gerekli olan herhangi bir ek işlem ekleyin.</span><span class="sxs-lookup"><span data-stu-id="df7cc-123">Once the migration is scaffolded, you should review it for accuracy and add any additional operations required to apply it correctly.</span></span> <span data-ttu-id="df7cc-124">Örneğin, geçişinizi aşağıdaki işlemleri içerebilir:</span><span class="sxs-lookup"><span data-stu-id="df7cc-124">For example, your migration might contain the following operations:</span></span>

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

<span data-ttu-id="df7cc-125">Bu işlemler veritabanı şeması uyumlu yaparken, mevcut müşteri adları korumak yok.</span><span class="sxs-lookup"><span data-stu-id="df7cc-125">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="df7cc-126">Daha iyi hale getirmek için şu şekilde yeniden.</span><span class="sxs-lookup"><span data-stu-id="df7cc-126">To make it better, rewrite it as follows.</span></span>

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> <span data-ttu-id="df7cc-127">Bir işlem iskele kurulmuş zaman yeni bir geçiş ekleme (bir sütunun düşürülmesine gibi) veri kaybına neden olabilir sizi uyarır.</span><span class="sxs-lookup"><span data-stu-id="df7cc-127">Adding a new migration warns when an operation is scaffolded that may result in data loss (like dropping a column).</span></span> <span data-ttu-id="df7cc-128">Özellikle bu geçişler için doğruluk gözden geçirdiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="df7cc-128">Be sure to especially review these migrations for accuracy.</span></span>

<span data-ttu-id="df7cc-129">Geçiş uygun komutu kullanılarak veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="df7cc-129">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a><span data-ttu-id="df7cc-130">Bir geçiş kaldırılıyor</span><span class="sxs-lookup"><span data-stu-id="df7cc-130">Removing a migration</span></span>
--------------------
<span data-ttu-id="df7cc-131">Bazen bir geçiş ekleyin ve uygulamadan önce EF Core modelinizi ek değişiklikler yapmanız gerekiyorsa farkında olun.</span><span class="sxs-lookup"><span data-stu-id="df7cc-131">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span>
<span data-ttu-id="df7cc-132">Son geçiş kaldırmak için bu komutu kullanın.</span><span class="sxs-lookup"><span data-stu-id="df7cc-132">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="df7cc-133">Kaldırdıktan sonra ek model değişiklikleri yapın ve yeniden ekleyin.</span><span class="sxs-lookup"><span data-stu-id="df7cc-133">After removing it, you can make the additional model changes and add it again.</span></span>

<a name="reverting-a-migration"></a><span data-ttu-id="df7cc-134">Bir geçiş döndürülüyor</span><span class="sxs-lookup"><span data-stu-id="df7cc-134">Reverting a migration</span></span>
---------------------
<span data-ttu-id="df7cc-135">Zaten bir geçiş (veya birkaç geçişler) veritabanı, ancak gereksinim dönmek için uyguladığınız, geçişleri geçerli, ancak geri almak istediğiniz geçiş adını belirtmek için aynı komutu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="df7cc-135">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a><span data-ttu-id="df7cc-136">Boş geçişleri</span><span class="sxs-lookup"><span data-stu-id="df7cc-136">Empty migrations</span></span>
----------------
<span data-ttu-id="df7cc-137">Bazen model değişiklik yapmadan bir geçiş eklemek yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="df7cc-137">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="df7cc-138">Bu durumda, yeni bir geçiş ekleniyor, boş bir tane oluşturur.</span><span class="sxs-lookup"><span data-stu-id="df7cc-138">In this case, adding a new migration creates an empty one.</span></span> <span data-ttu-id="df7cc-139">EF Core modele doğrudan ilişkili olmayan işlemleri gerçekleştirmek için bu geçiş özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="df7cc-139">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span>
<span data-ttu-id="df7cc-140">Bu şekilde yönetmek için isteyebileceğiniz bazı işlemler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="df7cc-140">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="df7cc-141">Tam metin araması</span><span class="sxs-lookup"><span data-stu-id="df7cc-141">Full-Text Search</span></span>
* <span data-ttu-id="df7cc-142">İşlevler</span><span class="sxs-lookup"><span data-stu-id="df7cc-142">Functions</span></span>
* <span data-ttu-id="df7cc-143">Saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="df7cc-143">Stored procedures</span></span>
* <span data-ttu-id="df7cc-144">Tetikleyiciler</span><span class="sxs-lookup"><span data-stu-id="df7cc-144">Triggers</span></span>
* <span data-ttu-id="df7cc-145">Görünümler</span><span class="sxs-lookup"><span data-stu-id="df7cc-145">Views</span></span>
* <span data-ttu-id="df7cc-146">VS.</span><span class="sxs-lookup"><span data-stu-id="df7cc-146">etc.</span></span>

<a name="generating-a-sql-script"></a><span data-ttu-id="df7cc-147">SQL komut dosyası oluşturuluyor</span><span class="sxs-lookup"><span data-stu-id="df7cc-147">Generating a SQL script</span></span>
-----------------------
<span data-ttu-id="df7cc-148">Ne zaman geçiş hata ayıklama veya bunları üretim veritabanı için dağıtma, bir SQL betiği oluşturmak kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="df7cc-148">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="df7cc-149">Betik daha sonra daha fazla doğruluk gözden ve bir üretim veritabanının gereksinimlerinize uyacak şekilde ayarlanmış.</span><span class="sxs-lookup"><span data-stu-id="df7cc-149">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="df7cc-150">Betik, bir dağıtım teknolojisi ile birlikte de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="df7cc-150">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="df7cc-151">Temel komut aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="df7cc-151">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="df7cc-152">Bu komut için birkaç seçenek vardır.</span><span class="sxs-lookup"><span data-stu-id="df7cc-152">There are several options to this command.</span></span>

<span data-ttu-id="df7cc-153">**Gelen** geçiş betiği çalıştırmadan önce veritabanına uygulanan son geçiş olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="df7cc-153">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="df7cc-154">Hiçbir geçiş uygulandıysa belirtin `0` (varsayılan değer budur).</span><span class="sxs-lookup"><span data-stu-id="df7cc-154">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="df7cc-155">**İçin** geçiş için veritabanı, betiği çalıştırdıktan sonra uygulanacak son geçiş olur.</span><span class="sxs-lookup"><span data-stu-id="df7cc-155">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="df7cc-156">Bu, son geçiş, projenizdeki varsayar.</span><span class="sxs-lookup"><span data-stu-id="df7cc-156">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="df7cc-157">Bir **ıdempotent** betik isteğe bağlı olarak oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="df7cc-157">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="df7cc-158">Bunlar zaten veritabanına sorgularınızda uygulanmamış bu betik yalnızca geçişleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="df7cc-158">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="df7cc-159">Ne son geçiş veritabanına uygulanan tam olarak emin değilseniz yaradı veya her farklı bir geçiş olabilir. birden fazla veritabanına dağıtıyorsanız, budur.</span><span class="sxs-lookup"><span data-stu-id="df7cc-159">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="applying-migrations-at-runtime"></a><span data-ttu-id="df7cc-160">Zamanında uygulanan geçişleri</span><span class="sxs-lookup"><span data-stu-id="df7cc-160">Applying migrations at runtime</span></span>
------------------------------
<span data-ttu-id="df7cc-161">Bazı uygulamalar, geçişleri çalışma zamanında başlatma sırasında uygulama veya ilk çalıştırma isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="df7cc-161">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="df7cc-162">Bunu yapmak `Migrate()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="df7cc-162">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="df7cc-163">Uyarı, bu yaklaşım herkese uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="df7cc-163">Caution, this approach isn't for everyone.</span></span> <span data-ttu-id="df7cc-164">Yerel veritabanı içeren uygulamalar için mükemmel olmakla birlikte, çoğu uygulama SQL betikleri oluşturma gibi daha güçlü Dağıtım stratejisi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="df7cc-164">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> <span data-ttu-id="df7cc-165">Remove() çağırmayın `EnsureCreated()` önce `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="df7cc-165">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="df7cc-166">`EnsureCreated()` neden olan şema oluşturmaya geçişleri atlar `Migrate()` başarısız.</span><span class="sxs-lookup"><span data-stu-id="df7cc-166">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

> [!NOTE]
> <span data-ttu-id="df7cc-167">Bu yöntem, üst kısmındaki yapılar `IMigrator` daha Gelişmiş senaryolar için kullanılan hizmet.</span><span class="sxs-lookup"><span data-stu-id="df7cc-167">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="df7cc-168">Kullanım `DbContext.GetService<IMigrator>()` erişmek için.</span><span class="sxs-lookup"><span data-stu-id="df7cc-168">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
