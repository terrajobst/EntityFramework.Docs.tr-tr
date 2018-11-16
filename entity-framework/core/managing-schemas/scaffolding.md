---
title: EF Core tersine mühendislik-
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: ef729c0c26d5a1f57099f339eb51cda7e83289df
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688686"
---
# <a name="reverse-engineering"></a><span data-ttu-id="afb60-102">Tersine mühendislik</span><span class="sxs-lookup"><span data-stu-id="afb60-102">Reverse Engineering</span></span>

<span data-ttu-id="afb60-103">Tersine mühendislik varlık türü sınıfları ve veritabanı şemasını temel alan bir DbContext sınıfı iskele kurma özelliği işlemidir.</span><span class="sxs-lookup"><span data-stu-id="afb60-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="afb60-104">Kullanılarak gerçekleştirilebilir `Scaffold-DbContext` EF Core Paket Yöneticisi Konsolu (PMC) Araçları'nın komutunu veya `dotnet ef dbcontext scaffold` .NET komut satırı arabirimi (CLI) araçlarını komutu.</span><span class="sxs-lookup"><span data-stu-id="afb60-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="afb60-105">Yükleme</span><span class="sxs-lookup"><span data-stu-id="afb60-105">Installing</span></span>

<span data-ttu-id="afb60-106">Tersine mühendislik önce ya da yüklemeniz gerekir [PMC Araçları](xref:core/miscellaneous/cli/powershell) (yalnızca Visual Studio) veya [CLI Araçları](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="afb60-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="afb60-107">Ayrıntılar için bağlantılara bakın.</span><span class="sxs-lookup"><span data-stu-id="afb60-107">See links for details.</span></span>

<span data-ttu-id="afb60-108">Uygun bir yüklemek gerekecektir [veritabanı sağlayıcısı](xref:core/providers/index) tersine mühendislik istediğiniz veritabanı şeması.</span><span class="sxs-lookup"><span data-stu-id="afb60-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="afb60-109">Bağlantı dizesi</span><span class="sxs-lookup"><span data-stu-id="afb60-109">Connection string</span></span>

<span data-ttu-id="afb60-110">İlk bağımsız değişkeni komut, veritabanına bir bağlantı dizesidir.</span><span class="sxs-lookup"><span data-stu-id="afb60-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="afb60-111">Araçları, veritabanı şemasını okumak için bu bağlantı dizesini kullanır.</span><span class="sxs-lookup"><span data-stu-id="afb60-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="afb60-112">Nasıl teklif ve bağlantı dizesini kaçış hangi Kabuk komutu yürütmek için kullandığınız bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="afb60-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="afb60-113">Özellikleri için kabuğunuzun belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="afb60-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="afb60-114">Örneğin, PowerShell, kaçış gerektirir `$` karakter ancak `\`.</span><span class="sxs-lookup"><span data-stu-id="afb60-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="afb60-115">Yapılandırma ve kullanıcı parolaları</span><span class="sxs-lookup"><span data-stu-id="afb60-115">Configuration and User Secrets</span></span>

<span data-ttu-id="afb60-116">Bir ASP.NET Core projesi varsa, kullanabileceğiniz `Name=<connection-string>` yapılandırmasından bağlantı dizesini okumayı söz dizimi.</span><span class="sxs-lookup"><span data-stu-id="afb60-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="afb60-117">Bu şununla düzgün çalışır [gizli dizi Yöneticisi aracını](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) veritabanı parolanızı kod temelinizde ayrı tutmak için.</span><span class="sxs-lookup"><span data-stu-id="afb60-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="afb60-118">Sağlayıcı adı</span><span class="sxs-lookup"><span data-stu-id="afb60-118">Provider name</span></span>

<span data-ttu-id="afb60-119">İkinci bağımsız değişkeni sağlayıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="afb60-119">The second argument is the provider name.</span></span> <span data-ttu-id="afb60-120">Sağlayıcı adı genellikle sağlayıcının NuGet paket adı ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="afb60-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="afb60-121">Tabloları belirleme</span><span class="sxs-lookup"><span data-stu-id="afb60-121">Specifying tables</span></span>

<span data-ttu-id="afb60-122">Veritabanı şema tüm tabloları, varsayılan olarak varlık türleriyle mühendislik ters.</span><span class="sxs-lookup"><span data-stu-id="afb60-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="afb60-123">Tabloları ters mühendislik şemaları ve tabloları belirterek sınırlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="afb60-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="afb60-124">`-Schemas` PMC parametresinde ve `--schema` CLI seçeneğinde, bir şema içinde her tablo eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="afb60-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="afb60-125">`-Tables` (PMC) ve `--table` (CLI), belirli tablolar eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="afb60-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="afb60-126">Birden çok tablo PMC'de dahil etmek için bir dizi kullanın.</span><span class="sxs-lookup"><span data-stu-id="afb60-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="afb60-127">Birden çok tablo CLI'daki dahil etmek için birden çok kez seçeneğini belirtin.</span><span class="sxs-lookup"><span data-stu-id="afb60-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="afb60-128">Adları koruma</span><span class="sxs-lookup"><span data-stu-id="afb60-128">Preserving names</span></span>

<span data-ttu-id="afb60-129">Tablo ve sütun adları türleri ve özellikleri için .NET adlandırma kuralları daha iyi eşleşecek şekilde varsayılan olarak sabit.</span><span class="sxs-lookup"><span data-stu-id="afb60-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="afb60-130">Belirtme `-UseDatabaseNames` geçiş PMC'de veya `--use-database-names` CLI seçeneğinde özgün veritabanı adları mümkün olduğunca koruyarak bu davranışı devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="afb60-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="afb60-131">Geçersiz .NET tanımlayıcıları yine de sabit ve Sentezlenen adları Gezinti özellikleri gibi .NET adlandırma kuralları için yine de uyumlu olacaktır.</span><span class="sxs-lookup"><span data-stu-id="afb60-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="afb60-132">Fluent API'si veya veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="afb60-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="afb60-133">Varlık türleri Fluent API'sini kullanarak, varsayılan olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="afb60-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="afb60-134">Belirtin `-DataAnnotations` (PMC) veya `--data-annotations` (Bunun yerine, mümkün olduğunda veri ek açıklamaları kullanmak için CLI).</span><span class="sxs-lookup"><span data-stu-id="afb60-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="afb60-135">Örneğin, Fluent API'sini kullanarak bu iskelesini.</span><span class="sxs-lookup"><span data-stu-id="afb60-135">For example, using the Fluent API will scaffold the this.</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="afb60-136">Veri ek açıklamaları kullanırken bu iskelesini.</span><span class="sxs-lookup"><span data-stu-id="afb60-136">While using Data Annotations will scaffold this.</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="afb60-137">DbContext adı</span><span class="sxs-lookup"><span data-stu-id="afb60-137">DbContext name</span></span>

<span data-ttu-id="afb60-138">İskele kurulmuş DbContext sınıfı adı ile ve sonra veritabanının adı olacaktır *bağlam* varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="afb60-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="afb60-139">Farklı bir belirtmek için kullanın `-Context` PMC içinde ve `--context` CLI.</span><span class="sxs-lookup"><span data-stu-id="afb60-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="afb60-140">Dizinleri ve ad alanları</span><span class="sxs-lookup"><span data-stu-id="afb60-140">Directories and namespaces</span></span>

<span data-ttu-id="afb60-141">Varlık sınıfları ve bir DbContext sınıfı projenin kök dizinine başladınız ve projenin varsayılan ad alanı kullanın.</span><span class="sxs-lookup"><span data-stu-id="afb60-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="afb60-142">Dizini belirtebilirsiniz sınıfları kullanarak iskele kurulmuş burada `-OutputDir` (PMC) veya `--output-dir` (CLI).</span><span class="sxs-lookup"><span data-stu-id="afb60-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="afb60-143">Ad alanı, kök ad alanlarının herhangi bir alt dizini proje kök dizininin altında adlarını olacaktır.</span><span class="sxs-lookup"><span data-stu-id="afb60-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="afb60-144">Ayrıca `-ContextDir` (PMC) ve `--context-dir` varlık türü sınıflardan ayrı bir dizine DbContext sınıfı iskelesini kurmak (CLI).</span><span class="sxs-lookup"><span data-stu-id="afb60-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="afb60-145">Nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="afb60-145">How it works</span></span>

<span data-ttu-id="afb60-146">Tersine mühendislik, veritabanı şemasını okuyarak başlatır.</span><span class="sxs-lookup"><span data-stu-id="afb60-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="afb60-147">Bu, tablolar, sütunlar, kısıtlamalar ve dizinler ilişkin bilgileri okur.</span><span class="sxs-lookup"><span data-stu-id="afb60-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="afb60-148">Ardından, EF Core modeli oluşturmak için şema bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="afb60-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="afb60-149">Tablolar, varlık türleri oluşturmak için kullanılır; sütun özellikleri oluşturmak için kullanılır; ve yabancı anahtarlar ilişkiler oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="afb60-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="afb60-150">Son olarak, model, kodu oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="afb60-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="afb60-151">İlgili varlık türü sınıfları, Fluent API'si ve veri ek açıklamaları, aynı modelden uygulamanızı yeniden oluşturmak için başladınız.</span><span class="sxs-lookup"><span data-stu-id="afb60-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="what-doesnt-work"></a><span data-ttu-id="afb60-152">İşe yaramazsa</span><span class="sxs-lookup"><span data-stu-id="afb60-152">What doesn't work</span></span>

<span data-ttu-id="afb60-153">Her şey hakkında bir model kullanarak veritabanı şemasını temsil edilebilir.</span><span class="sxs-lookup"><span data-stu-id="afb60-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="afb60-154">Örneğin, hakkında bilgi **devralma hiyerarşilerini**, **türlerine ait**, ve **tablo bölme** veritabanı şemasında mevcut değildir.</span><span class="sxs-lookup"><span data-stu-id="afb60-154">For example, information about **inheritance hierarchies**, **owned types**, and **table splitting** are not present in the database schema.</span></span> <span data-ttu-id="afb60-155">Bu nedenle, bu yapıları hiçbir zaman olması ters mühendislik uygulanan.</span><span class="sxs-lookup"><span data-stu-id="afb60-155">Because of this, these constructs will never be reverse engineered.</span></span>

<span data-ttu-id="afb60-156">Ayrıca, **bazı sütun türleri** EF Core sağlayıcı tarafından desteklenmiyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="afb60-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="afb60-157">Bu sütunlar, modele dahil edilmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="afb60-157">These columns won't be included in the model.</span></span>

<span data-ttu-id="afb60-158">EF Core anahtara sahip her varlık türü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="afb60-158">EF Core requires every entity type to have a key.</span></span> <span data-ttu-id="afb60-159">Tablolar, ancak birincil anahtarın belirtilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="afb60-159">Tables, however, aren't required to specify a primary key.</span></span> <span data-ttu-id="afb60-160">**Birincil anahtarı olmayan tablolar** olan şu an ters mühendislik.</span><span class="sxs-lookup"><span data-stu-id="afb60-160">**Tables without a primary key** are currently not reverse engineered.</span></span>

<span data-ttu-id="afb60-161">Tanımlayabileceğiniz **eşzamanlılık belirteçleri** iki kullanıcı aynı anda aynı varlık güncelleştirmesini engellemek için bir EF Core modelinde.</span><span class="sxs-lookup"><span data-stu-id="afb60-161">You can define **concurrency tokens** in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="afb60-162">Bazı veritabanları, sütun (örneğin, SQL Server'da rowversion) bu tür, bu durumda biz ters mühendislik bu bilgileri temsil etmek için özel bir türüne sahip; Ancak, diğer eşzamanlılık belirteçleri değil olması ters mühendislik uygulanan.</span><span class="sxs-lookup"><span data-stu-id="afb60-162">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="afb60-163">Model özelleştirme</span><span class="sxs-lookup"><span data-stu-id="afb60-163">Customizing the model</span></span>

<span data-ttu-id="afb60-164">EF Core tarafından oluşturulan kodu, kodudur.</span><span class="sxs-lookup"><span data-stu-id="afb60-164">The code generated by EF Core is your code.</span></span> <span data-ttu-id="afb60-165">Bunu değiştirmek çekinmeyin.</span><span class="sxs-lookup"><span data-stu-id="afb60-165">Feel free to change it.</span></span> <span data-ttu-id="afb60-166">Yalnızca, aynı modelin yeniden tersine mühendislik, oluşturulacak.</span><span class="sxs-lookup"><span data-stu-id="afb60-166">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="afb60-167">İskele kurulan kodu temsil eden *bir* veritabanı, ancak erişmek için kullanılan model değil kesinlikle *yalnızca* kullanılabilmesi için modeli.</span><span class="sxs-lookup"><span data-stu-id="afb60-167">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="afb60-168">Varlık türü sınıfları ve DbContext sınıfı kendi gereksinimlerinize uyacak şekilde özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="afb60-168">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="afb60-169">Örneğin, türleri ve özellikleri yeniden adlandır, devralma hiyerarşilerini tanıtmak veya birden fazla varlık için bir tabloya bölme tercih edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="afb60-169">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="afb60-170">Benzersiz olmayan dizinleri, kullanılmayan dizileri ve gezinti özellikleri, isteğe bağlı bir skaler özellikler ve kısıtlama adları modelden kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="afb60-170">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="afb60-171">Ayrıca ekleyebileceğiniz ek Oluşturucular, yöntemler, özellikler, vb.</span><span class="sxs-lookup"><span data-stu-id="afb60-171">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="afb60-172">ayrı bir dosyada başka bir kısmi sınıf kullanarak.</span><span class="sxs-lookup"><span data-stu-id="afb60-172">using another partial class in a separate file.</span></span> <span data-ttu-id="afb60-173">Bu yaklaşım ters mühendislik modeli yeniden istiyorsanız bile, çalışır.</span><span class="sxs-lookup"><span data-stu-id="afb60-173">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="afb60-174">Bir modeli güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="afb60-174">Updating the model</span></span>

<span data-ttu-id="afb60-175">Veritabanına değişiklikleri yaptıktan sonra EF Core modelinizi bu değişiklikleri yansıtacak şekilde güncelleştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="afb60-175">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="afb60-176">Veritabanı değişikliklerini basittir, yalnızca değişiklikleri EF Core modelinizi el ile yapmak kolay olabilir.</span><span class="sxs-lookup"><span data-stu-id="afb60-176">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="afb60-177">Örneğin, bir tabloyu veya sütunu yeniden adlandırma, bir sütunu kaldırarak veya bir sütunun türünü güncelleştirme Önemsiz kodlarında değişiklik olur.</span><span class="sxs-lookup"><span data-stu-id="afb60-177">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="afb60-178">Önemli değişiklikler, ancak kolay olun el ile değildir.</span><span class="sxs-lookup"><span data-stu-id="afb60-178">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="afb60-179">Veritabanını yeniden kullanarak modeli bir ortak iş akışı, ters çevirmek için mühendislik `-Force` (PMC) veya `--force` varolan modeli güncelleştirilmiş bir olanın üzerine yazmak (CLI).</span><span class="sxs-lookup"><span data-stu-id="afb60-179">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="afb60-180">Başka bir yaygın olarak istenen özellik yeniden adlandırma, tür hiyerarşileri, vb. özelleştirme korurken veritabanından modeli güncelleştirmek için yeteneğidir. Sorunu kullanın [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) bu özelliğin ilerlemesini izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="afb60-180">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="afb60-181">Veritabanı modeli yeniden, ters mühendislik, dosyalarda yaptığınız tüm değişiklikler kaybolacak.</span><span class="sxs-lookup"><span data-stu-id="afb60-181">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
