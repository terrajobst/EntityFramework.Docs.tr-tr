---
title: Geçişler-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: bf9aa32dd731b60d2985a9fe8bebd703af4af03b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655565"
---
# <a name="migrations"></a><span data-ttu-id="f58cb-102">Geçişler</span><span class="sxs-lookup"><span data-stu-id="f58cb-102">Migrations</span></span>

<span data-ttu-id="f58cb-103">Geliştirme sırasında bir veri modeli değişir ve veritabanı ile eşitlenmemiş olur.</span><span class="sxs-lookup"><span data-stu-id="f58cb-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="f58cb-104">Veritabanını bırakabilir ve EF ile eşleşen yeni bir tane oluşturmasını sağlayabilirsiniz, ancak bu yordam veri kaybına neden olur.</span><span class="sxs-lookup"><span data-stu-id="f58cb-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="f58cb-105">EF Core geçiş özelliği, veritabanı şemasını, veritabanında var olan verileri korurken uygulamanın veri modeliyle eşitlenmiş halde tutmak için artımlı olarak güncellemek üzere bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="f58cb-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="f58cb-106">Geçişler, aşağıdaki görevlerle yardım eden komut satırı araçlarını ve API 'Leri içerir:</span><span class="sxs-lookup"><span data-stu-id="f58cb-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="f58cb-107">[Geçiş oluşturun](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="f58cb-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="f58cb-108">Veritabanını bir model değişikliği kümesiyle eşitlenecek şekilde güncelleştiren bir kod oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f58cb-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="f58cb-109">[Veritabanını güncelleştirin](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="f58cb-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="f58cb-110">Veritabanı şemasını güncelleştirmek için bekleyen geçişleri uygulayın.</span><span class="sxs-lookup"><span data-stu-id="f58cb-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="f58cb-111">[Geçiş kodunu özelleştirin](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="f58cb-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="f58cb-112">Bazen oluşturulan kodun değiştirilmesi veya takıma alınması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="f58cb-113">[Bir geçişi kaldırın](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="f58cb-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="f58cb-114">Oluşturulan kodu silin.</span><span class="sxs-lookup"><span data-stu-id="f58cb-114">Delete the generated code.</span></span>
* <span data-ttu-id="f58cb-115">[Bir geçişi döndürür](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="f58cb-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="f58cb-116">Veritabanı değişikliklerini geri al.</span><span class="sxs-lookup"><span data-stu-id="f58cb-116">Undo the database changes.</span></span>
* <span data-ttu-id="f58cb-117">[SQL betikleri oluşturun](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="f58cb-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="f58cb-118">Bir üretim veritabanını güncelleştirmek veya geçiş kodu sorunlarını gidermek için bir betiğe gerek duyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f58cb-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="f58cb-119">[Çalışma zamanında geçişleri uygulayın](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="f58cb-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="f58cb-120">Tasarım zamanı güncelleştirmeleri ve çalıştırma betikleri en iyi seçenek olmadığından, `Migrate()` yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="f58cb-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

> [!TIP]
> <span data-ttu-id="f58cb-121">`DbContext` başlangıç projesinden farklı bir derlemede yer alıyorsa, hedef ve başlangıç projelerini [Paket Yöneticisi konsol araçları](xref:core/miscellaneous/cli/powershell#target-and-startup-project) veya [.NET Core CLI araçları](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project)'nda açıkça belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f58cb-121">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span></span>

## <a name="install-the-tools"></a><span data-ttu-id="f58cb-122">Araçları yükler</span><span class="sxs-lookup"><span data-stu-id="f58cb-122">Install the tools</span></span>

<span data-ttu-id="f58cb-123">[Komut satırı araçlarını](xref:core/miscellaneous/cli/index)yükler:</span><span class="sxs-lookup"><span data-stu-id="f58cb-123">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>

* <span data-ttu-id="f58cb-124">Visual Studio için [Paket Yöneticisi konsol araçları](xref:core/miscellaneous/cli/powershell)önerilir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-124">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="f58cb-125">Diğer geliştirme ortamları için [.NET Core CLI araçları](xref:core/miscellaneous/cli/dotnet)' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="f58cb-125">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

## <a name="create-a-migration"></a><span data-ttu-id="f58cb-126">Geçiş oluşturma</span><span class="sxs-lookup"><span data-stu-id="f58cb-126">Create a migration</span></span>

<span data-ttu-id="f58cb-127">[İlk modelinizi tanımladıktan](xref:core/modeling/index)sonra, veritabanının oluşturulması zaman alabilir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-127">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="f58cb-128">İlk geçiş eklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f58cb-128">To add an initial migration, run the following command.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="f58cb-129">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f58cb-129">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` Console
dotnet ef migrations add InitialCreate
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="f58cb-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f58cb-130">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration InitialCreate
```

***

<span data-ttu-id="f58cb-131">**Geçiş** dizini altında projenize üç dosya eklenir:</span><span class="sxs-lookup"><span data-stu-id="f58cb-131">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="f58cb-132">**XXXXXXXXXXXXXX_InitialCreate. cs**--ana geçişler dosyası.</span><span class="sxs-lookup"><span data-stu-id="f58cb-132">**XXXXXXXXXXXXXX_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="f58cb-133">Geçişi uygulamak için gerekli işlemleri içerir (`Up()`) ve döndürür (`Down()`).</span><span class="sxs-lookup"><span data-stu-id="f58cb-133">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="f58cb-134">**XXXXXXXXXXXXXX_InitialCreate. Designer. cs**--geçişler meta verileri dosyası.</span><span class="sxs-lookup"><span data-stu-id="f58cb-134">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="f58cb-135">EF tarafından kullanılan bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-135">Contains information used by EF.</span></span>
* <span data-ttu-id="f58cb-136">**MyContextModelSnapshot.cs**--geçerli modelinize ait bir anlık görüntü.</span><span class="sxs-lookup"><span data-stu-id="f58cb-136">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="f58cb-137">Sonraki geçiş eklenirken nelerin değiştiğini belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f58cb-137">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="f58cb-138">Dosya adında zaman damgası, değişikliklerin ilerlemesini görebileceğiniz şekilde kronolojik olarak sıralanmasına yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="f58cb-138">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="f58cb-139">Geçiş dosyalarını taşımak ve ad alanını değiştirmek ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-139">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="f58cb-140">Yeni geçişler, son geçişin eşdüzey öğeleri olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f58cb-140">New migrations are created as siblings of the last migration.</span></span>

## <a name="update-the-database"></a><span data-ttu-id="f58cb-141">Veritabanını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="f58cb-141">Update the database</span></span>

<span data-ttu-id="f58cb-142">Sonra, şemayı oluşturmak için geçiş veritabanını veritabanına uygulayın.</span><span class="sxs-lookup"><span data-stu-id="f58cb-142">Next, apply the migration to the database to create the schema.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="f58cb-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f58cb-143">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` Console
dotnet ef database update
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="f58cb-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f58cb-144">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

## <a name="customize-migration-code"></a><span data-ttu-id="f58cb-145">Geçiş kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="f58cb-145">Customize migration code</span></span>

<span data-ttu-id="f58cb-146">EF Core modelinizde değişiklik yaptıktan sonra veritabanı şeması eşitlenmemiş olabilir. Bunu güncel hale getirmek için başka bir geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f58cb-146">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="f58cb-147">Geçiş adı, bir sürüm denetim sisteminde bir COMMIT iletisi gibi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-147">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="f58cb-148">Örneğin, gözden geçirmeler için değişiklik yeni bir varlık sınıfı ise *Addproductincelemeleri* gibi bir ad seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f58cb-148">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="f58cb-149">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f58cb-149">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` Console
dotnet ef migrations add AddProductReviews
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="f58cb-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f58cb-150">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration AddProductReviews
```

***

<span data-ttu-id="f58cb-151">Geçiş, daha sonra (bunun için oluşturulan kod) bir kez alındıktan sonra, kodu doğruluk için gözden geçirin ve doğru uygulamak için gereken tüm işlemleri ekleyin, kaldırın veya değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f58cb-151">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="f58cb-152">Örneğin, bir geçiş aşağıdaki işlemleri içerebilir:</span><span class="sxs-lookup"><span data-stu-id="f58cb-152">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="f58cb-153">Bu işlemler veritabanı şemasını uyumlu hale getirir, ancak mevcut müşteri adlarını korumaz.</span><span class="sxs-lookup"><span data-stu-id="f58cb-153">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="f58cb-154">Daha iyi hale getirmek için aşağıdaki gibi yeniden yazın.</span><span class="sxs-lookup"><span data-stu-id="f58cb-154">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="f58cb-155">Geçiş yapı iskelesi işlemi, bir işlem veri kaybına neden olabileceği zaman uyarır (bir sütunu bırakma gibi).</span><span class="sxs-lookup"><span data-stu-id="f58cb-155">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="f58cb-156">Uyarı görürseniz, özellikle de geçişler için geçiş kodunu gözden geçirdiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="f58cb-156">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="f58cb-157">Uygun komutu kullanarak geçişi veritabanına uygulayın.</span><span class="sxs-lookup"><span data-stu-id="f58cb-157">Apply the migration to the database using the appropriate command.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="f58cb-158">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f58cb-158">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` Console
dotnet ef database update
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="f58cb-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f58cb-159">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

### <a name="empty-migrations"></a><span data-ttu-id="f58cb-160">Boş geçişler</span><span class="sxs-lookup"><span data-stu-id="f58cb-160">Empty migrations</span></span>

<span data-ttu-id="f58cb-161">Bazen herhangi bir model değişikliği yapmadan bir geçiş eklemek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-161">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="f58cb-162">Bu durumda, yeni bir geçiş eklemek boş sınıflarla kod dosyaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f58cb-162">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="f58cb-163">Bu geçişi, EF Core modeliyle doğrudan bağlantılı olmayan işlemleri gerçekleştirmek için özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f58cb-163">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="f58cb-164">Bu şekilde yönetmek isteyebileceğiniz bazı şeyler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f58cb-164">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="f58cb-165">Tam metin arama</span><span class="sxs-lookup"><span data-stu-id="f58cb-165">Full-Text Search</span></span>
* <span data-ttu-id="f58cb-166">İşlevler</span><span class="sxs-lookup"><span data-stu-id="f58cb-166">Functions</span></span>
* <span data-ttu-id="f58cb-167">Saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="f58cb-167">Stored procedures</span></span>
* <span data-ttu-id="f58cb-168">Tetikleyiciler</span><span class="sxs-lookup"><span data-stu-id="f58cb-168">Triggers</span></span>
* <span data-ttu-id="f58cb-169">Görünümler</span><span class="sxs-lookup"><span data-stu-id="f58cb-169">Views</span></span>

## <a name="remove-a-migration"></a><span data-ttu-id="f58cb-170">Geçiş kaldırma</span><span class="sxs-lookup"><span data-stu-id="f58cb-170">Remove a migration</span></span>

<span data-ttu-id="f58cb-171">Bazen bir geçiş ekleyip EF Core modelinizde uygulamadan önce ek değişiklikler yapmanız gerektiğini fark edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f58cb-171">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="f58cb-172">Son geçişi kaldırmak için bu komutu kullanın.</span><span class="sxs-lookup"><span data-stu-id="f58cb-172">To remove the last migration, use this command.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="f58cb-173">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f58cb-173">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` Console
dotnet ef migrations remove
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="f58cb-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f58cb-174">Visual Studio</span></span>](#tab/vs)

``` powershell
Remove-Migration
```

***

<span data-ttu-id="f58cb-175">Geçiş kaldırıldıktan sonra, ek model değişiklikleri yapıp yeniden ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f58cb-175">After removing the migration, you can make the additional model changes and add it again.</span></span>

## <a name="revert-a-migration"></a><span data-ttu-id="f58cb-176">Geçişi döndürür</span><span class="sxs-lookup"><span data-stu-id="f58cb-176">Revert a migration</span></span>

<span data-ttu-id="f58cb-177">Veritabanına zaten bir geçiş (veya birkaç geçiş) uyguladıysanız ancak geri döndürmeniz gerekiyorsa, geçişleri uygulamak için aynı komutu kullanabilirsiniz, ancak geri almak istediğiniz geçişin adını belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f58cb-177">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="f58cb-178">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f58cb-178">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` Console
dotnet ef database update LastGoodMigration
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="f58cb-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f58cb-179">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database LastGoodMigration
```

***

## <a name="generate-sql-scripts"></a><span data-ttu-id="f58cb-180">SQL betikleri oluştur</span><span class="sxs-lookup"><span data-stu-id="f58cb-180">Generate SQL scripts</span></span>

<span data-ttu-id="f58cb-181">Geçişlerinizi hata ayıkladığınızda veya bir üretim veritabanına dağıttığınızda, bir SQL betiği oluşturmak yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="f58cb-181">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="f58cb-182">Betik daha sonra doğruluk açısından daha fazla incelenebilir ve bir üretim veritabanının ihtiyaçlarına uyacak şekilde ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-182">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="f58cb-183">Betik Ayrıca bir dağıtım teknolojisiyle birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-183">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="f58cb-184">Temel komut aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-184">The basic command is as follows.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="f58cb-185">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f58cb-185">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` Console
dotnet ef migrations script
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="f58cb-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f58cb-186">Visual Studio</span></span>](#tab/vs)

``` powershell
Script-Migration
```

***

<span data-ttu-id="f58cb-187">Bu komut için birkaç seçenek vardır.</span><span class="sxs-lookup"><span data-stu-id="f58cb-187">There are several options to this command.</span></span>

<span data-ttu-id="f58cb-188">Komut dosyası çalıştırılmadan önce geçiş **işleminden** önce veritabanına uygulanan son geçiş yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f58cb-188">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="f58cb-189">Hiçbir geçiş uygulanmışsa, `0` belirtin (bu varsayılandır).</span><span class="sxs-lookup"><span data-stu-id="f58cb-189">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="f58cb-190">**-** Geçiş, komut dosyası çalıştırıldıktan sonra veritabanına uygulanacak son geçiştir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-190">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="f58cb-191">Bu, varsayılan olarak projenizdeki son geçişe göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="f58cb-191">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="f58cb-192">İsteğe bağlı olarak **ıdempotent** betiği oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-192">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="f58cb-193">Bu betik yalnızca, veritabanına henüz uygulanmadıklarında geçişler uygular.</span><span class="sxs-lookup"><span data-stu-id="f58cb-193">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="f58cb-194">Bu, veritabanına en son geçişin ne olduğunu tam olarak bilmiyorsanız veya her biri farklı bir geçişte olabilecek birden çok veritabanına dağıtım yapıyorsanız kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="f58cb-194">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

## <a name="apply-migrations-at-runtime"></a><span data-ttu-id="f58cb-195">Çalışma zamanında geçişleri Uygula</span><span class="sxs-lookup"><span data-stu-id="f58cb-195">Apply migrations at runtime</span></span>

<span data-ttu-id="f58cb-196">Bazı uygulamalar, başlangıç veya ilk çalıştırma sırasında çalışma zamanında geçiş uygulamak isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-196">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="f58cb-197">`Migrate()` yöntemini kullanarak bunu yapın.</span><span class="sxs-lookup"><span data-stu-id="f58cb-197">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="f58cb-198">Bu yöntem, daha gelişmiş senaryolar için kullanılabilen `IMigrator` hizmetinin üst kısmında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f58cb-198">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="f58cb-199">Erişmek için `myDbContext.GetInfrastructure().GetService<IMigrator>()` kullanın.</span><span class="sxs-lookup"><span data-stu-id="f58cb-199">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * <span data-ttu-id="f58cb-200">Bu yaklaşım herkes için değildir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-200">This approach isn't for everyone.</span></span> <span data-ttu-id="f58cb-201">Yerel bir veritabanına sahip uygulamalar için harika olsa da, çoğu uygulama SQL betikleri oluşturma gibi daha güçlü dağıtım stratejisi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f58cb-201">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="f58cb-202">`Migrate()`önce `EnsureCreated()` çağırmayın.</span><span class="sxs-lookup"><span data-stu-id="f58cb-202">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="f58cb-203">`EnsureCreated()` şemayı oluşturmak için geçişleri atlar, bu da `Migrate()` başarısız olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="f58cb-203">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f58cb-204">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="f58cb-204">Next steps</span></span>

<span data-ttu-id="f58cb-205">Daha fazla bilgi için bkz. <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="f58cb-205">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>
