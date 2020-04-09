---
title: Göçler - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 190057daed61c58c1f89ee8d775913458e413a50
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136205"
---
# <a name="migrations"></a><span data-ttu-id="8190a-102">Geçişler</span><span class="sxs-lookup"><span data-stu-id="8190a-102">Migrations</span></span>

<span data-ttu-id="8190a-103">Bir veri modeli geliştirme sırasında değişir ve veritabanı ile eşitlenmemiş olur.</span><span class="sxs-lookup"><span data-stu-id="8190a-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="8190a-104">Veritabanını bırakabilir ve EF'nin modelle eşleşen yeni bir veritabanı oluşturmasına izin verebilirsiniz, ancak bu yordam veri kaybına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="8190a-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="8190a-105">EF Core'daki geçişler özelliği, veritabanındaki varolan verileri korurken uygulamanın veri modeliyle eşit tutmak için veritabanı şemasını aşamalı olarak güncelleştirmenin bir yolunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="8190a-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="8190a-106">Geçişler, aşağıdaki görevlere yardımcı olan komut satırı araçlarını ve API'leri içerir:</span><span class="sxs-lookup"><span data-stu-id="8190a-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="8190a-107">[Geçiş oluşturun.](#create-a-migration)</span><span class="sxs-lookup"><span data-stu-id="8190a-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="8190a-108">Veritabanını bir model değişikliği kümesiyle eşitlemek için güncelleştirebilecek kod oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8190a-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="8190a-109">[Veritabanını güncelleştirin.](#update-the-database)</span><span class="sxs-lookup"><span data-stu-id="8190a-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="8190a-110">Veritabanı şemasını güncelleştirmek için bekleyen geçişleri uygulayın.</span><span class="sxs-lookup"><span data-stu-id="8190a-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="8190a-111">[Geçiş kodunu özelleştirin.](#customize-migration-code)</span><span class="sxs-lookup"><span data-stu-id="8190a-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="8190a-112">Bazen oluşturulan kodun değiştirilmesi veya desteklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8190a-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="8190a-113">[Geçişkaldırma.](#remove-a-migration)</span><span class="sxs-lookup"><span data-stu-id="8190a-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="8190a-114">Oluşturulan kodu silin.</span><span class="sxs-lookup"><span data-stu-id="8190a-114">Delete the generated code.</span></span>
* <span data-ttu-id="8190a-115">[Geçişi geri çevirin.](#revert-a-migration)</span><span class="sxs-lookup"><span data-stu-id="8190a-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="8190a-116">Veritabanı değişikliklerini geri ala.</span><span class="sxs-lookup"><span data-stu-id="8190a-116">Undo the database changes.</span></span>
* <span data-ttu-id="8190a-117">[SQL komut dosyaları oluşturun.](#generate-sql-scripts)</span><span class="sxs-lookup"><span data-stu-id="8190a-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="8190a-118">Üretim veritabanını güncelleştirmek veya geçiş kodunu gidermek için bir komut dosyasına ihtiyacınız olabilir.</span><span class="sxs-lookup"><span data-stu-id="8190a-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="8190a-119">[Geçişleri çalışma zamanında uygulayın.](#apply-migrations-at-runtime)</span><span class="sxs-lookup"><span data-stu-id="8190a-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="8190a-120">Tasarım zamanı güncelleştirmeleri ve çalışan komut dosyaları en iyi `Migrate()` seçenek olmadığında, yöntemi arayın.</span><span class="sxs-lookup"><span data-stu-id="8190a-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

> [!TIP]
> <span data-ttu-id="8190a-121">Başlangıç `DbContext` projesinden farklı bir derlemedeyse, [Paket Yöneticisi Konsolu araçlarında](xref:core/miscellaneous/cli/powershell#target-and-startup-project) veya [.NET Core CLI araçlarında](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project)hedef ve başlangıç projelerini açıkça belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8190a-121">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span></span>

## <a name="install-the-tools"></a><span data-ttu-id="8190a-122">Araçları yükleme</span><span class="sxs-lookup"><span data-stu-id="8190a-122">Install the tools</span></span>

<span data-ttu-id="8190a-123">Komut [satırı araçlarını](xref:core/miscellaneous/cli/index)yükleyin:</span><span class="sxs-lookup"><span data-stu-id="8190a-123">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>

* <span data-ttu-id="8190a-124">Visual Studio için [Paket Yöneticisi Konsolu araçlarını](xref:core/miscellaneous/cli/powershell)öneririz.</span><span class="sxs-lookup"><span data-stu-id="8190a-124">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="8190a-125">Diğer geliştirme ortamları için [.NET Core CLI araçlarını](xref:core/miscellaneous/cli/dotnet)seçin.</span><span class="sxs-lookup"><span data-stu-id="8190a-125">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

## <a name="create-a-migration"></a><span data-ttu-id="8190a-126">Geçiş oluşturma</span><span class="sxs-lookup"><span data-stu-id="8190a-126">Create a migration</span></span>

<span data-ttu-id="8190a-127">[İlk modelinizi tanımladıktan](xref:core/modeling/index)sonra veritabanını oluşturma nın zamanı doldu.</span><span class="sxs-lookup"><span data-stu-id="8190a-127">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="8190a-128">İlk geçiş eklemek için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8190a-128">To add an initial migration, run the following command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="8190a-129">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8190a-129">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate
```

### <a name="visual-studio"></a>[<span data-ttu-id="8190a-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8190a-130">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration InitialCreate
```

***

<span data-ttu-id="8190a-131">**Geçişler** dizininde projenize üç dosya eklenir:</span><span class="sxs-lookup"><span data-stu-id="8190a-131">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="8190a-132">**XXXXXXXXXXXXXX_InitialCreate.cs**--Ana geçişler dosyası.</span><span class="sxs-lookup"><span data-stu-id="8190a-132">**XXXXXXXXXXXXXX_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="8190a-133">Geçiş (in) `Up()`uygulamak ve geri almak için (içinde) `Down()`gerekli işlemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="8190a-133">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="8190a-134">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--Geçişler meta veri dosyası.</span><span class="sxs-lookup"><span data-stu-id="8190a-134">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="8190a-135">EF tarafından kullanılan bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="8190a-135">Contains information used by EF.</span></span>
* <span data-ttu-id="8190a-136">**MyContextModelSnapshot.cs**-- Geçerli modelinizin anlık görüntüsü.</span><span class="sxs-lookup"><span data-stu-id="8190a-136">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="8190a-137">Sonraki geçiş eklenince nelerin değiştiğini belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8190a-137">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="8190a-138">Dosya adındaki zaman damgası, değişikliklerin ilerlemesini görebilmeniz için kronolojik olarak sıralı kalmalarına yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="8190a-138">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="8190a-139">Geçişler dosyalarını taşımak ve ad alanlarını değiştirmek ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="8190a-139">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="8190a-140">Yeni geçişler son geçişin kardeşleri olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8190a-140">New migrations are created as siblings of the last migration.</span></span>

## <a name="update-the-database"></a><span data-ttu-id="8190a-141">Veritabanını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="8190a-141">Update the database</span></span>

<span data-ttu-id="8190a-142">Ardından, şemayı oluşturmak için geçiş işimdi veritabanına uygulayın.</span><span class="sxs-lookup"><span data-stu-id="8190a-142">Next, apply the migration to the database to create the schema.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="8190a-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8190a-143">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[<span data-ttu-id="8190a-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8190a-144">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

## <a name="customize-migration-code"></a><span data-ttu-id="8190a-145">Geçiş kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="8190a-145">Customize migration code</span></span>

<span data-ttu-id="8190a-146">EF Core modelinizde değişiklik yaptıktan sonra, veritabanı şeması eşitlenmemiş olabilir. Güncel getirmek için başka bir geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8190a-146">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="8190a-147">Geçiş adı, sürüm denetim sisteminde bir ileti iletisi gibi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8190a-147">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="8190a-148">Örneğin, değişiklik incelemeler için yeni bir varlık sınıfıysa *AddProductReviews* gibi bir ad seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8190a-148">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="8190a-149">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8190a-149">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddProductReviews
```

### <a name="visual-studio"></a>[<span data-ttu-id="8190a-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8190a-150">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration AddProductReviews
```

***

<span data-ttu-id="8190a-151">Geçiş iskelesi (bunun için oluşturulan kod) oluşturulduktan sonra, doğruluk için kodu gözden geçirin ve doğru uygulamak için gereken işlemleri ekleyin, kaldırın veya değiştirin.</span><span class="sxs-lookup"><span data-stu-id="8190a-151">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="8190a-152">Örneğin, bir geçiş aşağıdaki işlemleri içerebilir:</span><span class="sxs-lookup"><span data-stu-id="8190a-152">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="8190a-153">Bu işlemler veritabanı şema uyumlu hale getirmekle birlikte, varolan müşteri adlarını korumaz.</span><span class="sxs-lookup"><span data-stu-id="8190a-153">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="8190a-154">Daha iyi hale getirmek için aşağıdaki gibi yeniden yazın.</span><span class="sxs-lookup"><span data-stu-id="8190a-154">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="8190a-155">Geçiş iskelesi işlemi, bir işlemin veri kaybına (sütun düşürme gibi) neden olabileceği konusunda uyarır.</span><span class="sxs-lookup"><span data-stu-id="8190a-155">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="8190a-156">Bu uyarıyı görürseniz, doğruluk için geçişler kodunu gözden geçirdiğinizden özellikle emin olun.</span><span class="sxs-lookup"><span data-stu-id="8190a-156">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="8190a-157">Geçişi uygun komutu kullanarak veritabanına uygulayın.</span><span class="sxs-lookup"><span data-stu-id="8190a-157">Apply the migration to the database using the appropriate command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="8190a-158">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8190a-158">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[<span data-ttu-id="8190a-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8190a-159">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

### <a name="empty-migrations"></a><span data-ttu-id="8190a-160">Boş geçişler</span><span class="sxs-lookup"><span data-stu-id="8190a-160">Empty migrations</span></span>

<span data-ttu-id="8190a-161">Bazen herhangi bir model değişikliği yapmadan bir geçiş eklemek yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="8190a-161">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="8190a-162">Bu durumda, yeni bir geçiş eklemek boş sınıfları olan kod dosyaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8190a-162">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="8190a-163">Bu geçişi, EF Core modeliyle doğrudan ilişkili olmayan işlemleri gerçekleştirmek için özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8190a-163">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="8190a-164">Bu şekilde yönetmek isteyebileceğin bazı şeyler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="8190a-164">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="8190a-165">Tam Metin Arama</span><span class="sxs-lookup"><span data-stu-id="8190a-165">Full-Text Search</span></span>
* <span data-ttu-id="8190a-166">İşlevler</span><span class="sxs-lookup"><span data-stu-id="8190a-166">Functions</span></span>
* <span data-ttu-id="8190a-167">Saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="8190a-167">Stored procedures</span></span>
* <span data-ttu-id="8190a-168">Tetikleyiciler</span><span class="sxs-lookup"><span data-stu-id="8190a-168">Triggers</span></span>
* <span data-ttu-id="8190a-169">Görünümler</span><span class="sxs-lookup"><span data-stu-id="8190a-169">Views</span></span>

## <a name="remove-a-migration"></a><span data-ttu-id="8190a-170">Geçişi kaldırma</span><span class="sxs-lookup"><span data-stu-id="8190a-170">Remove a migration</span></span>

<span data-ttu-id="8190a-171">Bazen bir geçiş ekler ve uygulamadan önce EF Core modelinizde ek değişiklikler yapmanız gerektiğini fark elersiniz.</span><span class="sxs-lookup"><span data-stu-id="8190a-171">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="8190a-172">Son geçişi kaldırmak için bu komutu kullanın.</span><span class="sxs-lookup"><span data-stu-id="8190a-172">To remove the last migration, use this command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="8190a-173">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8190a-173">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations remove
```

### <a name="visual-studio"></a>[<span data-ttu-id="8190a-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8190a-174">Visual Studio</span></span>](#tab/vs)

``` powershell
Remove-Migration
```

***

<span data-ttu-id="8190a-175">Geçişi kaldırdıktan sonra, ek model değişiklikleri yapabilir ve yeniden ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8190a-175">After removing the migration, you can make the additional model changes and add it again.</span></span>

## <a name="revert-a-migration"></a><span data-ttu-id="8190a-176">Geçişi geri almak</span><span class="sxs-lookup"><span data-stu-id="8190a-176">Revert a migration</span></span>

<span data-ttu-id="8190a-177">Veritabanına zaten bir geçiş (veya birkaç geçiş) uyguladıysanız, ancak geri almanız gerekiyorsa, geçişleri uygulamak için aynı komutu kullanabilirsiniz, ancak geri almak istediğiniz geçişin adını belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8190a-177">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="8190a-178">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8190a-178">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update LastGoodMigration
```

### <a name="visual-studio"></a>[<span data-ttu-id="8190a-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8190a-179">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database LastGoodMigration
```

***

## <a name="generate-sql-scripts"></a><span data-ttu-id="8190a-180">SQL komut dosyaları oluşturma</span><span class="sxs-lookup"><span data-stu-id="8190a-180">Generate SQL scripts</span></span>

<span data-ttu-id="8190a-181">Geçişlerinizin hata ayıklanması veya bunları bir üretim veritabanına dağıtırken, bir SQL komut dosyası oluşturmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="8190a-181">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="8190a-182">Komut dosyası daha sonra doğruluk açısından daha fazla gözden geçirilebilir ve bir üretim veritabanının gereksinimlerine uyacak şekilde ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="8190a-182">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="8190a-183">Komut dosyası, dağıtım teknolojisiyle birlikte de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8190a-183">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="8190a-184">Temel komut aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="8190a-184">The basic command is as follows.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="8190a-185">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8190a-185">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

#### <a name="basic-usage"></a><span data-ttu-id="8190a-186">Temel Kullanım</span><span class="sxs-lookup"><span data-stu-id="8190a-186">Basic Usage</span></span>
```dotnetcli
dotnet ef migrations script
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="8190a-187">From ile (zımni olarak)</span><span class="sxs-lookup"><span data-stu-id="8190a-187">With From (to implied)</span></span>
<span data-ttu-id="8190a-188">Bu, bu geçişten en son geçişe kadar bir SQL komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8190a-188">This will generate a SQL script from this migration to the latest migration.</span></span>
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="8190a-189">From ve To ile</span><span class="sxs-lookup"><span data-stu-id="8190a-189">With From and To</span></span>
<span data-ttu-id="8190a-190">Bu, `from` geçişten belirtilen `to` geçişe kadar bir SQL komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8190a-190">This will generate a SQL script from the `from` migration to the specified `to` migration.</span></span>
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
<span data-ttu-id="8190a-191">Geri alma `from` komut dosyası oluşturmak `to` için daha yeni bir komut dosyası kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8190a-191">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span> <span data-ttu-id="8190a-192">*Lütfen olası veri kaybı senaryolarına dikkat edin.*</span><span class="sxs-lookup"><span data-stu-id="8190a-192">*Please take note of potential data loss scenarios.*</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="8190a-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8190a-193">Visual Studio</span></span>](#tab/vs)

#### <a name="basic-usage"></a><span data-ttu-id="8190a-194">Temel Kullanım</span><span class="sxs-lookup"><span data-stu-id="8190a-194">Basic Usage</span></span>
``` powershell
Script-Migration
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="8190a-195">From ile (zımni olarak)</span><span class="sxs-lookup"><span data-stu-id="8190a-195">With From (to implied)</span></span>
<span data-ttu-id="8190a-196">Bu, bu geçişten en son geçişe kadar bir SQL komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8190a-196">This will generate a SQL script from this migration to the latest migration.</span></span>
```powershell
Script-Migration 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="8190a-197">From ve To ile</span><span class="sxs-lookup"><span data-stu-id="8190a-197">With From and To</span></span>
<span data-ttu-id="8190a-198">Bu, `from` geçişten belirtilen `to` geçişe kadar bir SQL komut dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8190a-198">This will generate a SQL script from the `from` migration to the specified `to` migration.</span></span>
```powershell
Script-Migration 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
<span data-ttu-id="8190a-199">Geri alma `from` komut dosyası oluşturmak `to` için daha yeni bir komut dosyası kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8190a-199">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span> <span data-ttu-id="8190a-200">*Lütfen olası veri kaybı senaryolarına dikkat edin.*</span><span class="sxs-lookup"><span data-stu-id="8190a-200">*Please take note of potential data loss scenarios.*</span></span>

***

<span data-ttu-id="8190a-201">Bu komutun birkaç seçeneği vardır.</span><span class="sxs-lookup"><span data-stu-id="8190a-201">There are several options to this command.</span></span>

<span data-ttu-id="8190a-202">**Geçiş,** komut dosyasını çalıştırmadan önce veritabanına uygulanan son geçiş olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8190a-202">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="8190a-203">Geçiş uygulanmadıysa, belirtin `0` (bu varsayılandır).</span><span class="sxs-lookup"><span data-stu-id="8190a-203">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="8190a-204">**Geçiş,** komut dosyasını çalıştırdıktan sonra veritabanına uygulanacak son geçiştir.</span><span class="sxs-lookup"><span data-stu-id="8190a-204">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="8190a-205">Bu, projenizdeki son geçiş için varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="8190a-205">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="8190a-206">İsteğe bağlı olarak **bir idempotent** komut dosyası oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="8190a-206">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="8190a-207">Bu komut dosyası, yalnızca veritabanına zaten uygulanmamışsa geçişleri uygular.</span><span class="sxs-lookup"><span data-stu-id="8190a-207">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="8190a-208">Veritabanına uygulanan son geçişin ne olduğunu tam olarak bilmiyorsanız veya her biri farklı bir geçişte olabilecek birden çok veritabanına dağıtıyorsanız bu kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="8190a-208">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

## <a name="apply-migrations-at-runtime"></a><span data-ttu-id="8190a-209">Geçişleri çalışma zamanında uygulama</span><span class="sxs-lookup"><span data-stu-id="8190a-209">Apply migrations at runtime</span></span>

<span data-ttu-id="8190a-210">Bazı uygulamalar, başlangıç veya ilk çalıştırma sırasında çalışma zamanında geçişuygulamak isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="8190a-210">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="8190a-211">Yöntemi kullanarak `Migrate()` bunu yapın.</span><span class="sxs-lookup"><span data-stu-id="8190a-211">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="8190a-212">Bu yöntem, daha gelişmiş `IMigrator` senaryolar için kullanılabilecek hizmetin üstüne oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8190a-212">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="8190a-213">Erişmek `myDbContext.GetInfrastructure().GetService<IMigrator>()` için kullan.</span><span class="sxs-lookup"><span data-stu-id="8190a-213">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * <span data-ttu-id="8190a-214">Bu yaklaşım herkes için değil.</span><span class="sxs-lookup"><span data-stu-id="8190a-214">This approach isn't for everyone.</span></span> <span data-ttu-id="8190a-215">Yerel veritabanına sahip uygulamalar için harika olsa da, çoğu uygulama SQL komut dosyası oluşturma gibi daha sağlam dağıtım stratejisi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="8190a-215">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="8190a-216">Daha önce `Migrate()` `EnsureCreated()` arama.</span><span class="sxs-lookup"><span data-stu-id="8190a-216">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="8190a-217">`EnsureCreated()`başarısız olmaya neden `Migrate()` olan şema oluşturmak için Geçişleri atlar.</span><span class="sxs-lookup"><span data-stu-id="8190a-217">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8190a-218">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="8190a-218">Next steps</span></span>

<span data-ttu-id="8190a-219">Daha fazla bilgi için bkz. <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="8190a-219">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>
