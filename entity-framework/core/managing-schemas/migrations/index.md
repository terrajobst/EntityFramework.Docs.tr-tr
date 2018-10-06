---
title: Geçişleri - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 5ae06a4342a556936dc44c5bf6622814eaad4733
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834753"
---
<a name="migrations"></a><span data-ttu-id="447d1-102">Geçişleri</span><span class="sxs-lookup"><span data-stu-id="447d1-102">Migrations</span></span>
==========

<span data-ttu-id="447d1-103">Bir veri modeli, geliştirme sırasında değiştirir ve veritabanı ile eşitlenmemiş alır.</span><span class="sxs-lookup"><span data-stu-id="447d1-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="447d1-104">Veritabanını bırakın ve EF modeli eşleşen yeni bir tane oluşturun sağlar, ancak bu yordamı veri kaybıyla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="447d1-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="447d1-105">EF Core geçişleri özelliği, veritabanındaki mevcut verileri korurken uygulamanın veri modeli ile eşitlenmiş saklamak için veritabanı şemasına artımlı olarak güncelleştirmek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="447d1-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="447d1-106">Geçişler, komut satırı araçları ve aşağıdaki görevlerde size yardımcı API'leri içerir:</span><span class="sxs-lookup"><span data-stu-id="447d1-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="447d1-107">[Bir geçiş oluşturmak](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="447d1-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="447d1-108">Bir model değişiklik kümesini eşitlemek için veritabanını güncellemek kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="447d1-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="447d1-109">[Veritabanını güncellemek](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="447d1-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="447d1-110">Veritabanı şemasını güncelleştirmek için geçişler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="447d1-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="447d1-111">[Geçiş kodu özelleştirme](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="447d1-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="447d1-112">Bazen takıma veya değiştirilecek oluşturulan kodu gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="447d1-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="447d1-113">[Bir geçiş Kaldır](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="447d1-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="447d1-114">Oluşturulan kodun silin.</span><span class="sxs-lookup"><span data-stu-id="447d1-114">Delete the generated code.</span></span>
* <span data-ttu-id="447d1-115">[Bir geçiş geri](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="447d1-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="447d1-116">Veritabanı değişiklikleri geri al.</span><span class="sxs-lookup"><span data-stu-id="447d1-116">Undo the database changes.</span></span>
* <span data-ttu-id="447d1-117">[SQL komut dosyaları üret](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="447d1-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="447d1-118">Bir üretim veritabanını güncelleştirmek için veya geçiş kodu sorunlarını gidermek için bir komut dosyası gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="447d1-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="447d1-119">[Geçişler, çalışma zamanında uygulama](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="447d1-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="447d1-120">Tasarım zamanı güncelleştirmeleri ve betiklerin çalıştırılmasını en iyi seçenekleri değilken çağrı `Migrate()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="447d1-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

<a name="install-the-tools"></a><span data-ttu-id="447d1-121">Araçları yükleme</span><span class="sxs-lookup"><span data-stu-id="447d1-121">Install the tools</span></span>
-----------------

<span data-ttu-id="447d1-122">Yükleme [komut satırı araçları](xref:core/miscellaneous/cli/index):</span><span class="sxs-lookup"><span data-stu-id="447d1-122">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>
* <span data-ttu-id="447d1-123">Visual Studio için öneririz [Paket Yöneticisi konsolu Araçları](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="447d1-123">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="447d1-124">Diğer geliştirme ortamlarında seçin [.NET Core CLI Araçları](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="447d1-124">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

<a name="create-a-migration"></a><span data-ttu-id="447d1-125">Bir geçiş oluşturun</span><span class="sxs-lookup"><span data-stu-id="447d1-125">Create a migration</span></span>
------------------

<span data-ttu-id="447d1-126">Sonra [ilk modelinizi tanımlanan](xref:core/modeling/index), veritabanı oluşturma zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="447d1-126">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="447d1-127">Bir başlangıç geçiş eklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="447d1-127">To add an initial migration, run the following command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="447d1-128">Üç dosyayı projenize altında eklenir **geçişler** dizini:</span><span class="sxs-lookup"><span data-stu-id="447d1-128">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="447d1-129">**00000000000000_InitialCreate.cs**--ana geçişleri dosya.</span><span class="sxs-lookup"><span data-stu-id="447d1-129">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="447d1-130">Geçiş uygulamak gerekli işlemleri içerir (içinde `Up()`) ve dönmek için (içinde `Down()`).</span><span class="sxs-lookup"><span data-stu-id="447d1-130">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="447d1-131">**00000000000000_InitialCreate.Designer.cs**--geçişleri meta veri dosyası.</span><span class="sxs-lookup"><span data-stu-id="447d1-131">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="447d1-132">EF tarafından kullanılan bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="447d1-132">Contains information used by EF.</span></span>
* <span data-ttu-id="447d1-133">**MyContextModelSnapshot.cs**--bir anlık görüntü geçerli modelinizin.</span><span class="sxs-lookup"><span data-stu-id="447d1-133">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="447d1-134">Sonraki geçiş eklerken nelerin değiştiğini belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="447d1-134">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="447d1-135">Zaman damgası dosya değişiklikleri ilerleyişini gördüğünüz şekilde kronolojik olarak sıralanan kalmalarını yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="447d1-135">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="447d1-136">Geçişleri dosyalarını taşıma ve kendi ad alanı değiştirmek ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="447d1-136">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="447d1-137">Yeni geçiş son geçiş bir eşdüzeyi olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="447d1-137">New migrations are created as siblings of the last migration.</span></span>

<a name="update-the-database"></a><span data-ttu-id="447d1-138">Veritabanını Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="447d1-138">Update the database</span></span>
-------------------

<span data-ttu-id="447d1-139">Ardından, geçiş veritabanına şema oluşturmak için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="447d1-139">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="customize-migration-code"></a><span data-ttu-id="447d1-140">Geçiş kodu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="447d1-140">Customize migration code</span></span>
------------------------

<span data-ttu-id="447d1-141">EF Core modelinizi değişiklikleri yaptıktan sonra veritabanı şemasını eşitlenmemiş olabilir. Bu en güncel duruma getirmek için başka bir geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="447d1-141">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="447d1-142">Geçiş adı gibi bir işleme iletisi, bir sürüm denetim sisteminde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="447d1-142">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="447d1-143">Örneğin, bir ad gibi seçebilir *AddProductReviews* incelemeleri için yeni bir varlık sınıfı değişiklikse.</span><span class="sxs-lookup"><span data-stu-id="447d1-143">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="447d1-144">(Bunun için oluşturulan iskele kurulmuş kodu) geçiş başladıktan sonra doğruluğunu kodu gözden geçirin ve eklemek, kaldırmak veya doğru uygulamak için gerekli işlemleri değiştirin.</span><span class="sxs-lookup"><span data-stu-id="447d1-144">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="447d1-145">Örneğin, bir geçiş aşağıdaki işlemleri içerebilir:</span><span class="sxs-lookup"><span data-stu-id="447d1-145">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="447d1-146">Bu işlemler veritabanı şeması uyumlu yaparken, mevcut müşteri adları korumak yok.</span><span class="sxs-lookup"><span data-stu-id="447d1-146">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="447d1-147">Daha iyi hale getirmek için şu şekilde yeniden.</span><span class="sxs-lookup"><span data-stu-id="447d1-147">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="447d1-148">Bir işlem (bir sütunun düşürülmesine gibi) veri kaybına neden olabilir, geçiş iskele oluşturma işlemi konusunda sizi uyarır.</span><span class="sxs-lookup"><span data-stu-id="447d1-148">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="447d1-149">Bu uyarıyı görürseniz, doğruluk geçiş kodunu gözden geçirmek özellikle emin olun.</span><span class="sxs-lookup"><span data-stu-id="447d1-149">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="447d1-150">Geçiş uygun komutu kullanılarak veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="447d1-150">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a><span data-ttu-id="447d1-151">Boş geçişleri</span><span class="sxs-lookup"><span data-stu-id="447d1-151">Empty migrations</span></span>

<span data-ttu-id="447d1-152">Bazen model değişiklik yapmadan bir geçiş eklemek yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="447d1-152">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="447d1-153">Bu durumda, yeni bir geçiş ekleme kodu dosyaları ile boş sınıflar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="447d1-153">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="447d1-154">EF Core modele doğrudan ilişkili olmayan işlemleri gerçekleştirmek için bu geçiş özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="447d1-154">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="447d1-155">Bu şekilde yönetmek için isteyebileceğiniz bazı işlemler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="447d1-155">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="447d1-156">Tam metin araması</span><span class="sxs-lookup"><span data-stu-id="447d1-156">Full-Text Search</span></span>
* <span data-ttu-id="447d1-157">İşlevler</span><span class="sxs-lookup"><span data-stu-id="447d1-157">Functions</span></span>
* <span data-ttu-id="447d1-158">Saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="447d1-158">Stored procedures</span></span>
* <span data-ttu-id="447d1-159">Tetikleyiciler</span><span class="sxs-lookup"><span data-stu-id="447d1-159">Triggers</span></span>
* <span data-ttu-id="447d1-160">Görünümler</span><span class="sxs-lookup"><span data-stu-id="447d1-160">Views</span></span>

<a name="remove-a-migration"></a><span data-ttu-id="447d1-161">Bir geçiş Kaldır</span><span class="sxs-lookup"><span data-stu-id="447d1-161">Remove a migration</span></span>
------------------
<span data-ttu-id="447d1-162">Bazen bir geçiş ekleyin ve uygulamadan önce EF Core modelinizi ek değişiklikler yapmanız gerekiyorsa farkında olun.</span><span class="sxs-lookup"><span data-stu-id="447d1-162">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="447d1-163">Son geçiş kaldırmak için bu komutu kullanın.</span><span class="sxs-lookup"><span data-stu-id="447d1-163">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="447d1-164">Yükseltme kaldırdıktan sonra ek model değişiklikleri yapın ve yeniden ekleyin.</span><span class="sxs-lookup"><span data-stu-id="447d1-164">After removing the migration, you can make the additional model changes and add it again.</span></span>

<a name="revert-a-migration"></a><span data-ttu-id="447d1-165">Bir geçiş geri döndür</span><span class="sxs-lookup"><span data-stu-id="447d1-165">Revert a migration</span></span>
------------------
<span data-ttu-id="447d1-166">Zaten bir geçiş (veya birkaç geçişler) veritabanı, ancak gereksinim dönmek için uyguladığınız, geçişleri geçerli, ancak geri almak istediğiniz geçiş adını belirtmek için aynı komutu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="447d1-166">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="generate-sql-scripts"></a><span data-ttu-id="447d1-167">SQL komut dosyaları üret</span><span class="sxs-lookup"><span data-stu-id="447d1-167">Generate SQL scripts</span></span>
--------------------
<span data-ttu-id="447d1-168">Ne zaman geçiş hata ayıklama veya bunları üretim veritabanı için dağıtma, bir SQL betiği oluşturmak kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="447d1-168">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="447d1-169">Betik daha sonra daha fazla doğruluk gözden ve bir üretim veritabanının gereksinimlerinize uyacak şekilde ayarlanmış.</span><span class="sxs-lookup"><span data-stu-id="447d1-169">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="447d1-170">Betik, bir dağıtım teknolojisi ile birlikte de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="447d1-170">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="447d1-171">Temel komut aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="447d1-171">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="447d1-172">Bu komut için birkaç seçenek vardır.</span><span class="sxs-lookup"><span data-stu-id="447d1-172">There are several options to this command.</span></span>

<span data-ttu-id="447d1-173">**Gelen** geçiş betiği çalıştırmadan önce veritabanına uygulanan son geçiş olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="447d1-173">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="447d1-174">Hiçbir geçiş uygulandıysa belirtin `0` (varsayılan değer budur).</span><span class="sxs-lookup"><span data-stu-id="447d1-174">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="447d1-175">**İçin** geçiş için veritabanı, betiği çalıştırdıktan sonra uygulanacak son geçiş olur.</span><span class="sxs-lookup"><span data-stu-id="447d1-175">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="447d1-176">Bu, son geçiş, projenizdeki varsayar.</span><span class="sxs-lookup"><span data-stu-id="447d1-176">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="447d1-177">Bir **ıdempotent** betik isteğe bağlı olarak oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="447d1-177">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="447d1-178">Bunlar zaten veritabanına sorgularınızda uygulanmamış bu betik yalnızca geçişleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="447d1-178">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="447d1-179">Ne son geçiş veritabanına uygulanan tam olarak emin değilseniz yaradı veya her farklı bir geçiş olabilir. birden fazla veritabanına dağıtıyorsanız, budur.</span><span class="sxs-lookup"><span data-stu-id="447d1-179">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="apply-migrations-at-runtime"></a><span data-ttu-id="447d1-180">Çalışma zamanında geçişleri Uygula</span><span class="sxs-lookup"><span data-stu-id="447d1-180">Apply migrations at runtime</span></span>
---------------------------
<span data-ttu-id="447d1-181">Bazı uygulamalar, geçişleri çalışma zamanında başlatma sırasında uygulama veya ilk çalıştırma isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="447d1-181">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="447d1-182">Bunu yapmak `Migrate()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="447d1-182">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="447d1-183">Bu yöntem, üst kısmındaki yapılar `IMigrator` daha Gelişmiş senaryolar için kullanılan hizmet.</span><span class="sxs-lookup"><span data-stu-id="447d1-183">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="447d1-184">Kullanım `DbContext.GetService<IMigrator>()` erişmek için.</span><span class="sxs-lookup"><span data-stu-id="447d1-184">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> * <span data-ttu-id="447d1-185">Bu yaklaşım herkese uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="447d1-185">This approach isn't for everyone.</span></span> <span data-ttu-id="447d1-186">Yerel veritabanı içeren uygulamalar için mükemmel olmakla birlikte, çoğu uygulama SQL betikleri oluşturma gibi daha güçlü Dağıtım stratejisi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="447d1-186">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="447d1-187">Remove() çağırmayın `EnsureCreated()` önce `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="447d1-187">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="447d1-188">`EnsureCreated()` neden olan şema oluşturmaya geçişleri atlar `Migrate()` başarısız.</span><span class="sxs-lookup"><span data-stu-id="447d1-188">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

<a name="next-steps"></a><span data-ttu-id="447d1-189">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="447d1-189">Next steps</span></span>
----------

<span data-ttu-id="447d1-190">Daha fazla bilgi için bkz. <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="447d1-190">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>