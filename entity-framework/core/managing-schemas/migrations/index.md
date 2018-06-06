---
title: Geçişler - EF çekirdek
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: dd164125c053497af94773011127853ad10d27a6
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754515"
---
<a name="migrations"></a><span data-ttu-id="570eb-102">Geçişleri</span><span class="sxs-lookup"><span data-stu-id="570eb-102">Migrations</span></span>
==========
<span data-ttu-id="570eb-103">Geçişler veritabanında var olan verileri korurken EF çekirdek modeli ile eşitlenmiş tutmak için veritabanının artımlı olarak şema değişiklikleri uygulamak için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="570eb-103">Migrations provide a way to incrementally apply schema changes to the database to keep it in sync with your EF Core model while preserving existing data in the database.</span></span>

<a name="creating-the-database"></a><span data-ttu-id="570eb-104">Veritabanı oluşturuluyor</span><span class="sxs-lookup"><span data-stu-id="570eb-104">Creating the database</span></span>
---------------------
<span data-ttu-id="570eb-105">Sonra [ilk modelinizi tanımlanan][1], veritabanı oluşturmak için zamanı.</span><span class="sxs-lookup"><span data-stu-id="570eb-105">After you've [defined your initial model][1], it's time to create the database.</span></span> <span data-ttu-id="570eb-106">Bunu yapmak için bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="570eb-106">To do this, add an initial migration.</span></span>
<span data-ttu-id="570eb-107">Yükleme [EF çekirdek Araçları] [ 2] ve uygun komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="570eb-107">Install the [EF Core Tools][2] and run the appropriate command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="570eb-108">Üç dosya projenizi altına eklenir **geçişler** dizini:</span><span class="sxs-lookup"><span data-stu-id="570eb-108">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="570eb-109">**00000000000000_InitialCreate.cs**--ana geçişler dosya.</span><span class="sxs-lookup"><span data-stu-id="570eb-109">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="570eb-110">Geçiş uygulamak gerekli işlemleri içerir (içinde `Up()`) ve dönmek için (içinde `Down()`).</span><span class="sxs-lookup"><span data-stu-id="570eb-110">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="570eb-111">**00000000000000_InitialCreate.Designer.cs**--geçişler meta veri dosyası.</span><span class="sxs-lookup"><span data-stu-id="570eb-111">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="570eb-112">EF tarafından kullanılan bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="570eb-112">Contains information used by EF.</span></span>
* <span data-ttu-id="570eb-113">**MyContextModelSnapshot.cs**--anlık görüntüsünü, geçerli model.</span><span class="sxs-lookup"><span data-stu-id="570eb-113">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="570eb-114">Sonraki geçiş eklenirken nelerin değiştiğini belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="570eb-114">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="570eb-115">Dosya adında zaman damgası, bunları değişiklikleri ilerleyişini görebilmeleri kronolojik olarak sıralanan tutmaya yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="570eb-115">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="570eb-116">Geçişler dosyaları taşıyın ve kendi ad alanı değiştirmek boş.</span><span class="sxs-lookup"><span data-stu-id="570eb-116">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="570eb-117">Yeni geçişler son geçiş eşdüzeyi olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="570eb-117">New migrations are created as siblings of the last migration.</span></span>

<span data-ttu-id="570eb-118">Ardından, geçiş şema oluşturmak için veritabanına uygulayın.</span><span class="sxs-lookup"><span data-stu-id="570eb-118">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a><span data-ttu-id="570eb-119">Başka bir geçiş ekleme</span><span class="sxs-lookup"><span data-stu-id="570eb-119">Adding another migration</span></span>
------------------------
<span data-ttu-id="570eb-120">EF çekirdek modelinizi değişiklikleri yaptıktan sonra veritabanı şemasını eşitlenmemiş olacaktır. Güncel duruma getirmek için başka bir geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="570eb-120">After making changes to your EF Core model, the database schema will be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="570eb-121">Geçiş adı gibi bir yürütme ileti bir sürüm denetim sisteminde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="570eb-121">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="570eb-122">Müşteri incelemelerini ürünlerin kaydedilecek değişiklik yaptıysanız, örneğin, aşağıdakine benzer seçmem *AddProductReviews*.</span><span class="sxs-lookup"><span data-stu-id="570eb-122">For example, if I made changes to save customer reviews of products, I might choose something like *AddProductReviews*.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="570eb-123">Geçiş iskele kurulmuş sonra doğruluk için gözden geçirin ve doğru şekilde uygulamak için gerekli herhangi bir ek işlem ekleyin.</span><span class="sxs-lookup"><span data-stu-id="570eb-123">Once the migration is scaffolded, you should review it for accuracy and add any additional operations required to apply it correctly.</span></span> <span data-ttu-id="570eb-124">Örneğin, geçişinizi aşağıdaki işlemleri içerebilir:</span><span class="sxs-lookup"><span data-stu-id="570eb-124">For example, your migration might contain the following operations:</span></span>

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

<span data-ttu-id="570eb-125">Bu işlemler veritabanı şeması uyumlu yaparken, mevcut müşteri adları korumak yok.</span><span class="sxs-lookup"><span data-stu-id="570eb-125">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="570eb-126">Daha iyi hale getirmek için aşağıdaki gibi yeniden.</span><span class="sxs-lookup"><span data-stu-id="570eb-126">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="570eb-127">Bir işlem iskele kurulmuş zaman yeni bir geçiş ekleme (bir sütunun düşürülmesine gibi) veri kaybı ile sonuçlanabilir sizi uyarır.</span><span class="sxs-lookup"><span data-stu-id="570eb-127">Adding a new migration warns when an operation is scaffolded that may result in data loss (like dropping a column).</span></span> <span data-ttu-id="570eb-128">Özellikle bu geçişler doğruluğu için gözden geçirdiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="570eb-128">Be sure to especially review these migrations for accuracy.</span></span>

<span data-ttu-id="570eb-129">Geçiş uygun komutu kullanılarak veritabanına uygulayın.</span><span class="sxs-lookup"><span data-stu-id="570eb-129">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a><span data-ttu-id="570eb-130">Bir geçiş kaldırma</span><span class="sxs-lookup"><span data-stu-id="570eb-130">Removing a migration</span></span>
--------------------
<span data-ttu-id="570eb-131">Bazen, bir geçiş ekleyebilir ve uygulamadan önce EF çekirdek modelinizi ek değişiklikler yapmak gereksinim unutmayın.</span><span class="sxs-lookup"><span data-stu-id="570eb-131">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span>
<span data-ttu-id="570eb-132">Son geçiş kaldırmak için bu komutu kullanın.</span><span class="sxs-lookup"><span data-stu-id="570eb-132">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="570eb-133">Kaldırıldıktan sonra ek model değişiklikleri yapın ve yeniden ekleyin.</span><span class="sxs-lookup"><span data-stu-id="570eb-133">After removing it, you can make the additional model changes and add it again.</span></span>

<a name="reverting-a-migration"></a><span data-ttu-id="570eb-134">Bir geçiş dönüştürme</span><span class="sxs-lookup"><span data-stu-id="570eb-134">Reverting a migration</span></span>
---------------------
<span data-ttu-id="570eb-135">Zaten bir geçiş (ya da birkaç geçişler) veritabanı ancak dönmek için gerek uyguladıysanız, geçişleri uygulayın, ancak geri almak istediğiniz geçiş adını belirtmek için aynı komutu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="570eb-135">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a><span data-ttu-id="570eb-136">Boş geçişleri</span><span class="sxs-lookup"><span data-stu-id="570eb-136">Empty migrations</span></span>
----------------
<span data-ttu-id="570eb-137">Bazen bir geçiş modeli değişiklik yapmadan eklemek yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="570eb-137">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="570eb-138">Bu durumda, yeni bir geçiş ekleme boş bir tane oluşturur.</span><span class="sxs-lookup"><span data-stu-id="570eb-138">In this case, adding a new migration creates an empty one.</span></span> <span data-ttu-id="570eb-139">EF çekirdek modeli doğrudan ilişkili olmayan işlemleri gerçekleştirmek için bu geçiş özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="570eb-139">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span>
<span data-ttu-id="570eb-140">Bu şekilde yönetmek isteyebilirsiniz bazı noktalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="570eb-140">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="570eb-141">Tam metin araması</span><span class="sxs-lookup"><span data-stu-id="570eb-141">Full-Text Search</span></span>
* <span data-ttu-id="570eb-142">İşlevler</span><span class="sxs-lookup"><span data-stu-id="570eb-142">Functions</span></span>
* <span data-ttu-id="570eb-143">Saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="570eb-143">Stored procedures</span></span>
* <span data-ttu-id="570eb-144">Tetikleyiciler</span><span class="sxs-lookup"><span data-stu-id="570eb-144">Triggers</span></span>
* <span data-ttu-id="570eb-145">Görünümler</span><span class="sxs-lookup"><span data-stu-id="570eb-145">Views</span></span>
* <span data-ttu-id="570eb-146">VS.</span><span class="sxs-lookup"><span data-stu-id="570eb-146">etc.</span></span>

<a name="generating-a-sql-script"></a><span data-ttu-id="570eb-147">SQL komut dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="570eb-147">Generating a SQL script</span></span>
-----------------------
<span data-ttu-id="570eb-148">Ne zaman, geçişler hata ayıklama veya üretim veritabanına dağıtırken, bir SQL komut dosyası oluşturmak kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="570eb-148">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="570eb-149">Komut dosyası sonra daha doğruluk bakımından gözden ve üretim veritabanını gereksinimlerine uyacak şekilde ayarlanmış.</span><span class="sxs-lookup"><span data-stu-id="570eb-149">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="570eb-150">Komut dosyası da bir dağıtım teknolojisi ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="570eb-150">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="570eb-151">Temel komut aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="570eb-151">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="570eb-152">Bu komut için birkaç seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="570eb-152">There are several options to this command.</span></span>

<span data-ttu-id="570eb-153">**Gelen** geçiş komut dosyasını çalıştırmadan önce veritabanına uygulanan son geçiş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="570eb-153">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="570eb-154">Geçiş uygulanan belirtebilmeniz `0` (varsayılan değer budur).</span><span class="sxs-lookup"><span data-stu-id="570eb-154">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="570eb-155">**İçin** geçiş komut dosyasını çalıştırdıktan sonra veritabanına uygulanacak son geçiş olur.</span><span class="sxs-lookup"><span data-stu-id="570eb-155">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="570eb-156">Bu son geçiş projenizdeki varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="570eb-156">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="570eb-157">Bir **ıdempotent** betik isteğe bağlı olarak oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="570eb-157">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="570eb-158">Bunlar zaten veritabanına uygulanmamış bu betik yalnızca geçişler geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="570eb-158">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="570eb-159">Tam olarak ne son geçiş veritabanına uygulanan bilmiyorsanız, yararlı, ya da her farklı bir geçiş olabilir birden fazla veritabanı için dağıtıyorsanız budur.</span><span class="sxs-lookup"><span data-stu-id="570eb-159">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="applying-migrations-at-runtime"></a><span data-ttu-id="570eb-160">Çalışma zamanında uygulanan geçişleri</span><span class="sxs-lookup"><span data-stu-id="570eb-160">Applying migrations at runtime</span></span>
------------------------------
<span data-ttu-id="570eb-161">Bazı uygulamalar başlatma sırasında çalışma zamanında geçişleri uygulayın veya ilk çalıştırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="570eb-161">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="570eb-162">Kullanarak bunu `Migrate()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="570eb-162">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="570eb-163">Uyarı, bu yaklaşım herkes için değil.</span><span class="sxs-lookup"><span data-stu-id="570eb-163">Caution, this approach isn't for everyone.</span></span> <span data-ttu-id="570eb-164">Yerel bir veritabanı olan uygulamalar için harika olsa da, çoğu uygulamayı SQL komut dosyaları oluşturma gibi daha sağlam Dağıtım stratejisi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="570eb-164">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> <span data-ttu-id="570eb-165">Çağrı yok `EnsureCreated()` önce `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="570eb-165">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="570eb-166">`EnsureCreated()` neden şema oluşturmak için geçiş atlar `Migrate()` başarısız.</span><span class="sxs-lookup"><span data-stu-id="570eb-166">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

> [!NOTE]
> <span data-ttu-id="570eb-167">Bu yöntem üstünde derlemeler `IMigrator` daha Gelişmiş senaryolar için kullanılan hizmet.</span><span class="sxs-lookup"><span data-stu-id="570eb-167">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="570eb-168">Kullanım `DbContext.GetService<IMigrator>()` erişmek için.</span><span class="sxs-lookup"><span data-stu-id="570eb-168">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
