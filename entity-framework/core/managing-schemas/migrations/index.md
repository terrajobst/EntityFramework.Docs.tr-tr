---
title: Geçişler-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: ffa9a34f13ab29f0ba93f9fd1f469398630604ce
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005454"
---
<a name="migrations"></a><span data-ttu-id="c4f7a-102">Geçişler</span><span class="sxs-lookup"><span data-stu-id="c4f7a-102">Migrations</span></span>
==========

<span data-ttu-id="c4f7a-103">Geliştirme sırasında bir veri modeli değişir ve veritabanı ile eşitlenmemiş olur.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="c4f7a-104">Veritabanını bırakabilir ve EF ile eşleşen yeni bir tane oluşturmasını sağlayabilirsiniz, ancak bu yordam veri kaybına neden olur.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="c4f7a-105">EF Core geçiş özelliği, veritabanı şemasını, veritabanında var olan verileri korurken uygulamanın veri modeliyle eşitlenmiş halde tutmak için artımlı olarak güncellemek üzere bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="c4f7a-106">Geçişler, aşağıdaki görevlerle yardım eden komut satırı araçlarını ve API 'Leri içerir:</span><span class="sxs-lookup"><span data-stu-id="c4f7a-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="c4f7a-107">[Geçiş oluşturun](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="c4f7a-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="c4f7a-108">Veritabanını bir model değişikliği kümesiyle eşitlenecek şekilde güncelleştiren bir kod oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="c4f7a-109">[Veritabanını güncelleştirin](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="c4f7a-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="c4f7a-110">Veritabanı şemasını güncelleştirmek için bekleyen geçişleri uygulayın.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="c4f7a-111">[Geçiş kodunu özelleştirin](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="c4f7a-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="c4f7a-112">Bazen oluşturulan kodun değiştirilmesi veya takıma alınması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="c4f7a-113">[Bir geçişi kaldırın](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="c4f7a-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="c4f7a-114">Oluşturulan kodu silin.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-114">Delete the generated code.</span></span>
* <span data-ttu-id="c4f7a-115">[Bir geçişi döndürür](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="c4f7a-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="c4f7a-116">Veritabanı değişikliklerini geri al.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-116">Undo the database changes.</span></span>
* <span data-ttu-id="c4f7a-117">[SQL betikleri oluşturun](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="c4f7a-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="c4f7a-118">Bir üretim veritabanını güncelleştirmek veya geçiş kodu sorunlarını gidermek için bir betiğe gerek duyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="c4f7a-119">[Çalışma zamanında geçişleri uygulayın](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="c4f7a-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="c4f7a-120">Tasarım zamanı güncelleştirmeleri ve çalıştırma betikleri en iyi seçenek olmadığında, `Migrate()` yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

> [!TIP]
> <span data-ttu-id="c4f7a-121">Başlangıç projesinden farklı bir derlemede [](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project) ise,hedefvebaşlangıçprojelerinipaketYöneticisikonsolaraçlarıveya.NETCoreCLIaraçları'ndaaçıkça`DbContext` belirtebilirsiniz. [](xref:core/miscellaneous/cli/powershell#target-and-startup-project)</span><span class="sxs-lookup"><span data-stu-id="c4f7a-121">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span></span>

<a name="install-the-tools"></a><span data-ttu-id="c4f7a-122">Araçları yükler</span><span class="sxs-lookup"><span data-stu-id="c4f7a-122">Install the tools</span></span>
-----------------

<span data-ttu-id="c4f7a-123">[Komut satırı araçlarını](xref:core/miscellaneous/cli/index)yükler:</span><span class="sxs-lookup"><span data-stu-id="c4f7a-123">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>
* <span data-ttu-id="c4f7a-124">Visual Studio için [Paket Yöneticisi konsol araçları](xref:core/miscellaneous/cli/powershell)önerilir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-124">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="c4f7a-125">Diğer geliştirme ortamları için [.NET Core CLI araçları](xref:core/miscellaneous/cli/dotnet)' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-125">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

<a name="create-a-migration"></a><span data-ttu-id="c4f7a-126">Geçiş oluşturma</span><span class="sxs-lookup"><span data-stu-id="c4f7a-126">Create a migration</span></span>
------------------

<span data-ttu-id="c4f7a-127">[İlk modelinizi tanımladıktan](xref:core/modeling/index)sonra, veritabanının oluşturulması zaman alabilir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-127">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="c4f7a-128">İlk geçiş eklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-128">To add an initial migration, run the following command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="c4f7a-129">**Geçiş** dizini altında projenize üç dosya eklenir:</span><span class="sxs-lookup"><span data-stu-id="c4f7a-129">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="c4f7a-130">**XXXXXXXXXXXXXX_InitialCreate. cs**--ana geçişler dosyası.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-130">**XXXXXXXXXXXXXX_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="c4f7a-131">Geçişi uygulamak için gereken işlemleri (içinde `Up()`) ve döndürür (içinde `Down()`).</span><span class="sxs-lookup"><span data-stu-id="c4f7a-131">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="c4f7a-132">**XXXXXXXXXXXXXX_InitialCreate. Designer. cs**--geçişler meta verileri dosyası.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-132">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="c4f7a-133">EF tarafından kullanılan bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-133">Contains information used by EF.</span></span>
* <span data-ttu-id="c4f7a-134">**MyContextModelSnapshot.cs**--geçerli modelinize ait bir anlık görüntü.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-134">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="c4f7a-135">Sonraki geçiş eklenirken nelerin değiştiğini belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-135">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="c4f7a-136">Dosya adında zaman damgası, değişikliklerin ilerlemesini görebileceğiniz şekilde kronolojik olarak sıralanmasına yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-136">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="c4f7a-137">Geçiş dosyalarını taşımak ve ad alanını değiştirmek ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-137">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="c4f7a-138">Yeni geçişler, son geçişin eşdüzey öğeleri olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-138">New migrations are created as siblings of the last migration.</span></span>

<a name="update-the-database"></a><span data-ttu-id="c4f7a-139">Veritabanını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="c4f7a-139">Update the database</span></span>
-------------------

<span data-ttu-id="c4f7a-140">Sonra, şemayı oluşturmak için geçiş veritabanını veritabanına uygulayın.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-140">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="customize-migration-code"></a><span data-ttu-id="c4f7a-141">Geçiş kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="c4f7a-141">Customize migration code</span></span>
------------------------

<span data-ttu-id="c4f7a-142">EF Core modelinizde değişiklik yaptıktan sonra veritabanı şeması eşitlenmemiş olabilir. Bunu güncel hale getirmek için başka bir geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-142">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="c4f7a-143">Geçiş adı, bir sürüm denetim sisteminde bir COMMIT iletisi gibi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-143">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="c4f7a-144">Örneğin, gözden geçirmeler için değişiklik yeni bir varlık sınıfı ise *Addproductincelemeleri* gibi bir ad seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-144">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="c4f7a-145">Geçiş, daha sonra (bunun için oluşturulan kod) bir kez alındıktan sonra, kodu doğruluk için gözden geçirin ve doğru uygulamak için gereken tüm işlemleri ekleyin, kaldırın veya değiştirin.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-145">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="c4f7a-146">Örneğin, bir geçiş aşağıdaki işlemleri içerebilir:</span><span class="sxs-lookup"><span data-stu-id="c4f7a-146">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="c4f7a-147">Bu işlemler veritabanı şemasını uyumlu hale getirir, ancak mevcut müşteri adlarını korumaz.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-147">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="c4f7a-148">Daha iyi hale getirmek için aşağıdaki gibi yeniden yazın.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-148">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="c4f7a-149">Geçiş yapı iskelesi işlemi, bir işlem veri kaybına neden olabileceği zaman uyarır (bir sütunu bırakma gibi).</span><span class="sxs-lookup"><span data-stu-id="c4f7a-149">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="c4f7a-150">Uyarı görürseniz, özellikle de geçişler için geçiş kodunu gözden geçirdiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-150">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="c4f7a-151">Uygun komutu kullanarak geçişi veritabanına uygulayın.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-151">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a><span data-ttu-id="c4f7a-152">Boş geçişler</span><span class="sxs-lookup"><span data-stu-id="c4f7a-152">Empty migrations</span></span>

<span data-ttu-id="c4f7a-153">Bazen herhangi bir model değişikliği yapmadan bir geçiş eklemek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-153">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="c4f7a-154">Bu durumda, yeni bir geçiş eklemek boş sınıflarla kod dosyaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-154">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="c4f7a-155">Bu geçişi, EF Core modeliyle doğrudan bağlantılı olmayan işlemleri gerçekleştirmek için özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-155">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="c4f7a-156">Bu şekilde yönetmek isteyebileceğiniz bazı şeyler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="c4f7a-156">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="c4f7a-157">Tam metin arama</span><span class="sxs-lookup"><span data-stu-id="c4f7a-157">Full-Text Search</span></span>
* <span data-ttu-id="c4f7a-158">İşlevler</span><span class="sxs-lookup"><span data-stu-id="c4f7a-158">Functions</span></span>
* <span data-ttu-id="c4f7a-159">Saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="c4f7a-159">Stored procedures</span></span>
* <span data-ttu-id="c4f7a-160">Tetikleyiciler</span><span class="sxs-lookup"><span data-stu-id="c4f7a-160">Triggers</span></span>
* <span data-ttu-id="c4f7a-161">Görünümler</span><span class="sxs-lookup"><span data-stu-id="c4f7a-161">Views</span></span>

<a name="remove-a-migration"></a><span data-ttu-id="c4f7a-162">Geçiş kaldırma</span><span class="sxs-lookup"><span data-stu-id="c4f7a-162">Remove a migration</span></span>
------------------
<span data-ttu-id="c4f7a-163">Bazen bir geçiş ekleyip EF Core modelinizde uygulamadan önce ek değişiklikler yapmanız gerektiğini fark edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-163">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="c4f7a-164">Son geçişi kaldırmak için bu komutu kullanın.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-164">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="c4f7a-165">Geçiş kaldırıldıktan sonra, ek model değişiklikleri yapıp yeniden ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-165">After removing the migration, you can make the additional model changes and add it again.</span></span>

<a name="revert-a-migration"></a><span data-ttu-id="c4f7a-166">Geçişi döndürür</span><span class="sxs-lookup"><span data-stu-id="c4f7a-166">Revert a migration</span></span>
------------------
<span data-ttu-id="c4f7a-167">Veritabanına zaten bir geçiş (veya birkaç geçiş) uyguladıysanız ancak geri döndürmeniz gerekiyorsa, geçişleri uygulamak için aynı komutu kullanabilirsiniz, ancak geri almak istediğiniz geçişin adını belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-167">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="generate-sql-scripts"></a><span data-ttu-id="c4f7a-168">SQL betikleri oluştur</span><span class="sxs-lookup"><span data-stu-id="c4f7a-168">Generate SQL scripts</span></span>
--------------------
<span data-ttu-id="c4f7a-169">Geçişlerinizi hata ayıkladığınızda veya bir üretim veritabanına dağıttığınızda, bir SQL betiği oluşturmak yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-169">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="c4f7a-170">Betik daha sonra doğruluk açısından daha fazla incelenebilir ve bir üretim veritabanının ihtiyaçlarına uyacak şekilde ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-170">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="c4f7a-171">Betik Ayrıca bir dağıtım teknolojisiyle birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-171">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="c4f7a-172">Temel komut aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-172">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="c4f7a-173">Bu komut için birkaç seçenek vardır.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-173">There are several options to this command.</span></span>

<span data-ttu-id="c4f7a-174">Komut dosyası çalıştırılmadan önce geçiş **işleminden** önce veritabanına uygulanan son geçiş yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-174">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="c4f7a-175">Hiçbir geçiş uygulanmışsa, (bu varsayılandır) `0` belirtin.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-175">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="c4f7a-176">**-** Geçiş, komut dosyası çalıştırıldıktan sonra veritabanına uygulanacak son geçiştir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-176">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="c4f7a-177">Bu, varsayılan olarak projenizdeki son geçişe göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-177">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="c4f7a-178">İsteğe bağlı olarak **ıdempotent** betiği oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-178">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="c4f7a-179">Bu betik yalnızca, veritabanına henüz uygulanmadıklarında geçişler uygular.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-179">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="c4f7a-180">Bu, veritabanına en son geçişin ne olduğunu tam olarak bilmiyorsanız veya her biri farklı bir geçişte olabilecek birden çok veritabanına dağıtım yapıyorsanız kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-180">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="apply-migrations-at-runtime"></a><span data-ttu-id="c4f7a-181">Çalışma zamanında geçişleri Uygula</span><span class="sxs-lookup"><span data-stu-id="c4f7a-181">Apply migrations at runtime</span></span>
---------------------------
<span data-ttu-id="c4f7a-182">Bazı uygulamalar, başlangıç veya ilk çalıştırma sırasında çalışma zamanında geçiş uygulamak isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-182">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="c4f7a-183">`Migrate()` Yöntemini kullanarak bunu yapın.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-183">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="c4f7a-184">Bu yöntem, daha gelişmiş senaryolar için `IMigrator` kullanılabilen hizmetin üst kısmında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-184">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="c4f7a-185">Erişmek `myDbContext.GetInfrastructure().GetService<IMigrator>()` için kullanın.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-185">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> * <span data-ttu-id="c4f7a-186">Bu yaklaşım herkes için değildir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-186">This approach isn't for everyone.</span></span> <span data-ttu-id="c4f7a-187">Yerel bir veritabanına sahip uygulamalar için harika olsa da, çoğu uygulama SQL betikleri oluşturma gibi daha güçlü dağıtım stratejisi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-187">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="c4f7a-188">Daha önce `EnsureCreated()` `Migrate()`çağırmayın.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-188">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="c4f7a-189">`EnsureCreated()`Şemayı oluşturmak için geçişleri atlar, bu da başarısız `Migrate()` olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-189">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

<a name="next-steps"></a><span data-ttu-id="c4f7a-190">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="c4f7a-190">Next steps</span></span>
----------

<span data-ttu-id="c4f7a-191">Daha fazla bilgi için bkz. <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="c4f7a-191">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>
