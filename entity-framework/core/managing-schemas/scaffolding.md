---
title: Tersine mühendislik-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 775a929982b9f4fb10aad9cd43bbb555ce632ad1
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149016"
---
# <a name="reverse-engineering"></a><span data-ttu-id="ef934-102">Tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="ef934-102">Reverse Engineering</span></span>

<span data-ttu-id="ef934-103">Tersine mühendislik, yapı iskelesi varlık türü sınıfları ve veritabanı şemasını temel alan bir DbContext sınıfı işlemidir.</span><span class="sxs-lookup"><span data-stu-id="ef934-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="ef934-104">EF Core Paket Yöneticisi Konsolu (PMC `Scaffold-DbContext` ) araçlarının komutu `dotnet ef dbcontext scaffold` veya .NET komut satırı arabirimi (CLI) araçlarının komutu kullanılarak gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ef934-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="ef934-105">Yüklemenin</span><span class="sxs-lookup"><span data-stu-id="ef934-105">Installing</span></span>

<span data-ttu-id="ef934-106">Ters mühendisden önce, [PMC araçları](xref:core/miscellaneous/cli/powershell) 'nı (yalnızca Visual Studio) veya [CLI araçlarını](xref:core/miscellaneous/cli/dotnet)yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="ef934-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="ef934-107">Ayrıntılar için bkz. bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="ef934-107">See links for details.</span></span>

<span data-ttu-id="ef934-108">Ayrıca, ters mühendislik uygulamak istediğiniz veritabanı şeması için uygun bir [veritabanı sağlayıcısı](xref:core/providers/index) yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="ef934-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="ef934-109">Bağlantı dizesi</span><span class="sxs-lookup"><span data-stu-id="ef934-109">Connection string</span></span>

<span data-ttu-id="ef934-110">Komutun ilk bağımsız değişkeni veritabanına yönelik bir bağlantı dizesidir.</span><span class="sxs-lookup"><span data-stu-id="ef934-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="ef934-111">Araçlar, veritabanı şemasını okumak için bu bağlantı dizesini kullanır.</span><span class="sxs-lookup"><span data-stu-id="ef934-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="ef934-112">Bağlantı dizesini nasıl teklifiniz ve kaçış, komutu yürütmek için kullandığınız kabuğa bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="ef934-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="ef934-113">Ayrıntılar için kabuğunuzun belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="ef934-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="ef934-114">Örneğin, PowerShell `$` karakterden kaçar, ancak içermemelidir `\`.</span><span class="sxs-lookup"><span data-stu-id="ef934-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="ef934-115">Yapılandırma ve Kullanıcı gizli dizileri</span><span class="sxs-lookup"><span data-stu-id="ef934-115">Configuration and User Secrets</span></span>

<span data-ttu-id="ef934-116">Bir ASP.NET Core projeniz varsa, yapılandırmadan bağlantı dizesini okumak için `Name=<connection-string>` söz dizimini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef934-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="ef934-117">Bu, veritabanı parolanızı kod tabanınızdan ayrı tutmak için [gizli dizi yöneticisi aracıyla](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) iyi bir şekilde çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="ef934-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="ef934-118">Sağlayıcı adı</span><span class="sxs-lookup"><span data-stu-id="ef934-118">Provider name</span></span>

<span data-ttu-id="ef934-119">İkinci bağımsız değişken sağlayıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="ef934-119">The second argument is the provider name.</span></span> <span data-ttu-id="ef934-120">Sağlayıcı adı, genellikle sağlayıcının NuGet paket adıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="ef934-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="ef934-121">Tabloları belirtme</span><span class="sxs-lookup"><span data-stu-id="ef934-121">Specifying tables</span></span>

<span data-ttu-id="ef934-122">Veritabanı şemasındaki tüm tablolar varsayılan olarak varlık türlerine ters mühendislik yapılır.</span><span class="sxs-lookup"><span data-stu-id="ef934-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="ef934-123">Şemaları ve tabloları belirterek hangi tabloların tersine mühendislik uygulanabilir olduğunu sınırlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef934-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="ef934-124">`--schema` PMC içindeki `-Schemas` parametresi ve CLI 'daki seçeneği, bir şema içindeki her tabloyu dahil etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef934-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="ef934-125">`-Tables`(PMC) ve `--table` (CLI), belirli tabloları dahil etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef934-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="ef934-126">PMC 'de birden çok tablo eklemek için bir dizi kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef934-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="ef934-127">CLı 'ya birden çok tablo eklemek için, seçeneği birden çok kez belirtin.</span><span class="sxs-lookup"><span data-stu-id="ef934-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="ef934-128">Adları koruma</span><span class="sxs-lookup"><span data-stu-id="ef934-128">Preserving names</span></span>

<span data-ttu-id="ef934-129">Tablo ve sütun adları, varsayılan olarak türler ve özellikler için .NET adlandırma kurallarıyla daha iyi eşleşecek şekilde düzeltilir.</span><span class="sxs-lookup"><span data-stu-id="ef934-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="ef934-130">Anahtarın PMC 'de `--use-database-names` veya CLI 'deki seçeneğinde belirtilmesi, özgün veritabanı adlarını mümkün olduğunca korumak için bu davranışı devre dışı bırakır. `-UseDatabaseNames`</span><span class="sxs-lookup"><span data-stu-id="ef934-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="ef934-131">Geçersiz .NET tanımlayıcıları düzeltilmeye devam eder ve gezinme özellikleri gibi birleştirilmiş adlar yine de .NET adlandırma kurallarıyla uyumlu olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ef934-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="ef934-132">Akıcı API veya veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="ef934-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="ef934-133">Varlık türleri, varsayılan olarak akıcı API kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ef934-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="ef934-134">Bunun `-DataAnnotations` yerine, mümkün olduğunda `--data-annotations` veri ek açıklamalarını kullanmak için (PMC) veya (CLI) belirtin.</span><span class="sxs-lookup"><span data-stu-id="ef934-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="ef934-135">Örneğin, akıcı API 'YI kullanmak bu şekilde ele alınacaktır:</span><span class="sxs-lookup"><span data-stu-id="ef934-135">For example, using the Fluent API will scaffold this:</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="ef934-136">Veri ek açıklamalarını kullanırken bu işlem aşağıdaki şekilde ele alınacaktır:</span><span class="sxs-lookup"><span data-stu-id="ef934-136">While using Data Annotations will scaffold this:</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="ef934-137">DbContext adı</span><span class="sxs-lookup"><span data-stu-id="ef934-137">DbContext name</span></span>

<span data-ttu-id="ef934-138">Scafkatlı DbContext sınıfı adı, varsayılan olarak *bağlam* ile düzeltilen veritabanı adının adı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ef934-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="ef934-139">Farklı bir tane belirtmek için, PMC `-Context` 'de ve `--context` CLI içinde kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef934-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="ef934-140">Dizinler ve ad alanları</span><span class="sxs-lookup"><span data-stu-id="ef934-140">Directories and namespaces</span></span>

<span data-ttu-id="ef934-141">Varlık sınıfları ve bir DbContext sınıfı, projenin kök dizinine yönelik yapı ve projenin varsayılan ad alanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="ef934-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="ef934-142">Sınıfların (PMC) veya `-OutputDir` `--output-dir` (CLI) kullanarak yapı iskelesi olduğu dizini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef934-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="ef934-143">Ad alanı, kök ad alanı ve projenin kök dizini altındaki tüm alt dizinlerin adları olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ef934-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="ef934-144">DbContext sınıfını varlık `-ContextDir` türü sınıflarından ayrı bir `--context-dir` dizine dönüştürmek için (PMC) ve (CLI) de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef934-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="ef934-145">Nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="ef934-145">How it works</span></span>

<span data-ttu-id="ef934-146">Tersine mühendislik, veritabanı şemasını okuyarak başlar.</span><span class="sxs-lookup"><span data-stu-id="ef934-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="ef934-147">Tablolar, sütunlar, kısıtlamalar ve dizinler hakkında bilgi okur.</span><span class="sxs-lookup"><span data-stu-id="ef934-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="ef934-148">Daha sonra, şema bilgilerini bir EF Core modeli oluşturmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="ef934-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="ef934-149">Tablolar varlık türleri oluşturmak için kullanılır; sütunları özellik oluşturmak için kullanılır; ve yabancı anahtarlar ilişki oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ef934-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="ef934-150">Son olarak, model kod oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ef934-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="ef934-151">Aynı modeli uygulamanızdan yeniden oluşturmak için karşılık gelen varlık türü sınıfları, akıcı API ve veri ek açıklamaları yapı iskelesi yapılır.</span><span class="sxs-lookup"><span data-stu-id="ef934-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="what-doesnt-work"></a><span data-ttu-id="ef934-152">Ne işe yaramıyor</span><span class="sxs-lookup"><span data-stu-id="ef934-152">What doesn't work</span></span>

<span data-ttu-id="ef934-153">Bir modelle ilgili her şey, bir veritabanı şeması kullanılarak gösterilebilir.</span><span class="sxs-lookup"><span data-stu-id="ef934-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="ef934-154">Örneğin, [**Devralma hiyerarşileri**](../modeling/inheritance.md), [**sahip olunan türler**](../modeling/owned-entities.md)ve [**tablo bölme**](../modeling/table-splitting.md) hakkında bilgiler veritabanı şemasında yok.</span><span class="sxs-lookup"><span data-stu-id="ef934-154">For example, information about [**inheritance hierarchies**](../modeling/inheritance.md), [**owned types**](../modeling/owned-entities.md), and [**table splitting**](../modeling/table-splitting.md) are not present in the database schema.</span></span> <span data-ttu-id="ef934-155">Bu nedenle, bu yapılar hiçbir şekilde tersine mühendislik olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="ef934-155">Because of this, these constructs will never be reverse engineered.</span></span>

<span data-ttu-id="ef934-156">Ayrıca, **bazı sütun türleri** EF Core sağlayıcısı tarafından desteklenmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="ef934-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="ef934-157">Bu sütunlar modele eklenmeyecek.</span><span class="sxs-lookup"><span data-stu-id="ef934-157">These columns won't be included in the model.</span></span>

<span data-ttu-id="ef934-158">Aynı anda iki kullanıcının aynı varlığı güncelleştirmesini engellemek için bir EF Core modelinde [**eşzamanlılık belirteçleri**](../modeling/concurrency.md)tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef934-158">You can define [**concurrency tokens**](../modeling/concurrency.md), in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="ef934-159">Bazı veritabanlarının bu tür bir sütunu (örneğin, SQL Server) temsil etmesi için özel bir türü vardır; bu durumda bu bilgilere ters mühendislik uygulanabilir. Ancak, diğer eşzamanlılık belirteçleri tersine mühendislik uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="ef934-159">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="ef934-160">Modeli özelleştirme</span><span class="sxs-lookup"><span data-stu-id="ef934-160">Customizing the model</span></span>

<span data-ttu-id="ef934-161">EF Core tarafından oluşturulan kod kodunuz.</span><span class="sxs-lookup"><span data-stu-id="ef934-161">The code generated by EF Core is your code.</span></span> <span data-ttu-id="ef934-162">Bunu değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef934-162">Feel free to change it.</span></span> <span data-ttu-id="ef934-163">Yalnızca aynı modele yeniden mühendislik uygulamanız durumunda yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ef934-163">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="ef934-164">Yapı iskelesi kodu, veritabanına erişmek için kullanılabilecek *bir* modeli temsil eder, ancak *yalnızca kullanılabilecek tek* model değildir.</span><span class="sxs-lookup"><span data-stu-id="ef934-164">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="ef934-165">Varlık türü sınıflarını ve DbContext sınıfını gereksinimlerinize uyacak şekilde özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="ef934-165">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="ef934-166">Örneğin, türleri ve özellikleri yeniden adlandırmayı, devralma hiyerarşileri tanıtmanızı veya bir tabloyu birden çok varlığa bölmeyi seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef934-166">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="ef934-167">Ayrıca, benzersiz olmayan dizinleri, kullanılmayan dizileri ve gezinti özelliklerini, isteğe bağlı skaler özellikleri ve modelden kısıtlama adlarını da kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef934-167">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="ef934-168">Ayrıca ek oluşturucular, Yöntemler, özellikler vb. ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef934-168">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="ef934-169">ayrı bir dosyada başka bir kısmi sınıf kullanılıyor.</span><span class="sxs-lookup"><span data-stu-id="ef934-169">using another partial class in a separate file.</span></span> <span data-ttu-id="ef934-170">Bu yaklaşım, modele geri dönmek istediğinizde bile geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ef934-170">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="ef934-171">Model güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="ef934-171">Updating the model</span></span>

<span data-ttu-id="ef934-172">Veritabanında değişiklik yaptıktan sonra, EF Core modelinizi bu değişiklikleri yansıtacak şekilde güncelleştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="ef934-172">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="ef934-173">Veritabanı değişiklikleri basittir, değişiklikleri EF Core modelinizde el ile yapmak en kolay olabilir.</span><span class="sxs-lookup"><span data-stu-id="ef934-173">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="ef934-174">Örneğin, bir tabloyu veya sütunu yeniden adlandırma, bir sütunu kaldırma veya bir sütunun türünü güncelleştirme, kodda yapılacak basit değişikliklerdir.</span><span class="sxs-lookup"><span data-stu-id="ef934-174">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="ef934-175">Ancak, daha önemli değişiklikler, el ile hızlı bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="ef934-175">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="ef934-176">Yaygın olarak kullanılan bir iş akışı, var olan modeli güncelleştirilmiş bir dosyanın üzerine `-Force` yazmak için (PMC `--force` ) veya (CLI) kullanarak modele geri doğru mühendislik kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="ef934-176">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="ef934-177">Diğer bir yaygın olarak istenen özellik, yeniden adlandırmalar, tür hiyerarşileri vs. gibi özelleştirmeyi korurken modeli veritabanından güncelleştirebilme yeteneğidir. Bu özelliğin ilerlemesini izlemek için sorun [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef934-177">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="ef934-178">Modele yeniden veritabanından geri dönmek isterseniz, dosyalarda yaptığınız tüm değişiklikler kaybedilir.</span><span class="sxs-lookup"><span data-stu-id="ef934-178">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
