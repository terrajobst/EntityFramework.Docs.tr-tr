---
title: Veri dengeli dağıtımı-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 5c056c600f696ad1443ddb7b8c95c4b0ead06d21
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417228"
---
# <a name="data-seeding"></a><span data-ttu-id="56610-102">Veri Çekirdeği Oluşturma</span><span class="sxs-lookup"><span data-stu-id="56610-102">Data Seeding</span></span>

<span data-ttu-id="56610-103">Veri dengeli dağıtımı, bir veritabanını ilk veri kümesiyle doldurma işlemidir.</span><span class="sxs-lookup"><span data-stu-id="56610-103">Data seeding is the process of populating a database with an initial set of data.</span></span>

<span data-ttu-id="56610-104">EF Core ' de gerçekleştirebilmenin birkaç yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="56610-104">There are several ways this can be accomplished in EF Core:</span></span>

* <span data-ttu-id="56610-105">Temel verileri modelleyin</span><span class="sxs-lookup"><span data-stu-id="56610-105">Model seed data</span></span>
* <span data-ttu-id="56610-106">El ile geçiş özelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="56610-106">Manual migration customization</span></span>
* <span data-ttu-id="56610-107">Özel başlatma mantığı</span><span class="sxs-lookup"><span data-stu-id="56610-107">Custom initialization logic</span></span>

## <a name="model-seed-data"></a><span data-ttu-id="56610-108">Temel verileri modelleyin</span><span class="sxs-lookup"><span data-stu-id="56610-108">Model seed data</span></span>

> [!NOTE]
> <span data-ttu-id="56610-109">Bu özellik EF Core 2,1 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="56610-109">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="56610-110">EF6 'in aksine, EF Core, dengeli dağıtım verileri model yapılandırmasının bir parçası olarak bir varlık türüyle ilişkilendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="56610-110">Unlike in EF6, in EF Core, seeding data can be associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="56610-111">Daha sonra EF Core [geçişler](xref:core/managing-schemas/migrations/index) , veritabanının yeni bir sürümüne yükseltilirken ekleme, güncelleştirme veya silme işlemlerinin uygulanması gerektiğini otomatik olarak hesaplar.</span><span class="sxs-lookup"><span data-stu-id="56610-111">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

> [!NOTE]
> <span data-ttu-id="56610-112">Geçişler yalnızca, çekirdek verileri istenen duruma getirmek için hangi işlemin gerçekleştirilmesi gerektiğini belirlerken model değişikliklerini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="56610-112">Migrations only considers model changes when determining what operation should be performed to get the seed data into the desired state.</span></span> <span data-ttu-id="56610-113">Bu nedenle, geçiş dışında gerçekleştirilen verilerde yapılan değişiklikler kaybolabilir veya hataya neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="56610-113">Thus any changes to the data performed outside of migrations might be lost or cause an error.</span></span>

<span data-ttu-id="56610-114">Örnek olarak, bu, `OnModelCreating`bir `Blog` için çekirdek verileri yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="56610-114">As an example, this will configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="56610-115">İlişkiye sahip varlıkları eklemek için yabancı anahtar değerleri belirtilmelidir:</span><span class="sxs-lookup"><span data-stu-id="56610-115">To add entities that have a relationship the foreign key values need to be specified:</span></span>

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="56610-116">Varlık türünün gölge durumunda herhangi bir özelliği varsa, değerleri sağlamak için anonim bir sınıf kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="56610-116">If the entity type has any properties in shadow state an anonymous class can be used to provide the values:</span></span>

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

<span data-ttu-id="56610-117">Sahip olan varlık türleri benzer bir biçimde çalıştırılabilir:</span><span class="sxs-lookup"><span data-stu-id="56610-117">Owned entity types can be seeded in a similar fashion:</span></span>

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

<span data-ttu-id="56610-118">Daha fazla bağlam için bkz. [tam örnek proje](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) .</span><span class="sxs-lookup"><span data-stu-id="56610-118">See the [full sample project](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) for more context.</span></span>

<span data-ttu-id="56610-119">Veriler modele eklendikten sonra, değişiklikleri uygulamak için [geçişler](xref:core/managing-schemas/migrations/index) kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="56610-119">Once the data has been added to the model, [migrations](xref:core/managing-schemas/migrations/index) should be used to apply the changes.</span></span>

> [!TIP]
> <span data-ttu-id="56610-120">Bir otomatik dağıtımın parçası olarak geçişler uygulamanız gerekiyorsa, yürütmeden önce önizlenebilir [BIR SQL betiği oluşturabilirsiniz](xref:core/managing-schemas/migrations/index#generate-sql-scripts) .</span><span class="sxs-lookup"><span data-stu-id="56610-120">If you need to apply migrations as part of an automated deployment you can [create a SQL script](xref:core/managing-schemas/migrations/index#generate-sql-scripts) that can be previewed before execution.</span></span>

<span data-ttu-id="56610-121">Alternatif olarak, çekirdek verileri içeren yeni bir veritabanı oluşturmak için `context.Database.EnsureCreated()` kullanabilirsiniz (örneğin, bir test veritabanı veya bellek içi sağlayıcıyı veya hiçbir ilişki dışı veritabanını kullanırken).</span><span class="sxs-lookup"><span data-stu-id="56610-121">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider or any non-relation database.</span></span> <span data-ttu-id="56610-122">Veritabanı zaten varsa `EnsureCreated()` şemayı veya veritabanındaki çekirdek verileri güncelleştirdiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="56610-122">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor seed data in the database.</span></span> <span data-ttu-id="56610-123">Geçiş kullanmayı planlıyorsanız, ilişkisel veritabanları için `EnsureCreated()` Çağırmamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="56610-123">For relational databases you shouldn't call `EnsureCreated()` if you plan to use Migrations.</span></span>

### <a name="limitations-of-model-seed-data"></a><span data-ttu-id="56610-124">Model tohum verilerinin sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="56610-124">Limitations of model seed data</span></span>

<span data-ttu-id="56610-125">Bu tür çekirdek verileri geçişlerle yönetilir ve veritabanında zaten var olan verileri güncelleştirmek için betik veritabanına bağlanmadan oluşturulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="56610-125">This type of seed data is managed by migrations and the script to update the data that's already in the database needs to be generated without connecting to the database.</span></span> <span data-ttu-id="56610-126">Bu, bazı kısıtlamalar getirir:</span><span class="sxs-lookup"><span data-stu-id="56610-126">This imposes some restrictions:</span></span>

* <span data-ttu-id="56610-127">Birincil anahtar değerinin, genellikle veritabanı tarafından oluşturulsa bile belirtilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="56610-127">The primary key value needs to be specified even if it's usually generated by the database.</span></span> <span data-ttu-id="56610-128">Geçişler arasındaki veri değişikliklerini algılamak için kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="56610-128">It will be used to detect data changes between migrations.</span></span>
* <span data-ttu-id="56610-129">Önceden sağlanan veriler, birincil anahtar herhangi bir şekilde değiştirilirse kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="56610-129">Previously seeded data will be removed if the primary key is changed in any way.</span></span>

<span data-ttu-id="56610-130">Bu nedenle, bu özellik, geçiş dışında değiştirilmesi beklenen statik veriler için en yararlı seçenektir ve örneğin ZIP kodları gibi veritabanında başka herhangi bir şeye bağlı değildir.</span><span class="sxs-lookup"><span data-stu-id="56610-130">Therefore this feature is most useful for static data that's not expected to change outside of migrations and does not depend on anything else in the database, for example ZIP codes.</span></span>

<span data-ttu-id="56610-131">Senaryonuz aşağıdakilerden birini içeriyorsa, son bölümde açıklanan özel başlatma mantığını kullanmanız önerilir:</span><span class="sxs-lookup"><span data-stu-id="56610-131">If your scenario includes any of the following it is recommended to use custom initialization logic described in the last section:</span></span>

* <span data-ttu-id="56610-132">Test için geçici veriler</span><span class="sxs-lookup"><span data-stu-id="56610-132">Temporary data for testing</span></span>
* <span data-ttu-id="56610-133">Veritabanı durumuna bağlı veriler</span><span class="sxs-lookup"><span data-stu-id="56610-133">Data that depends on database state</span></span>
* <span data-ttu-id="56610-134">Kimlik olarak alternatif anahtarlar kullanan varlıklar dahil olmak üzere, veritabanı tarafından oluşturulacak anahtar değerlere ihtiyacı olan veriler</span><span class="sxs-lookup"><span data-stu-id="56610-134">Data that needs key values to be generated by the database, including entities that use alternate keys as the identity</span></span>
* <span data-ttu-id="56610-135">Bazı Parola karması gibi özel dönüşüm gerektiren ( [değer dönüştürmeleri](xref:core/modeling/value-conversions)tarafından işlenmemiş) veriler</span><span class="sxs-lookup"><span data-stu-id="56610-135">Data that requires custom transformation (that is not handled by [value conversions](xref:core/modeling/value-conversions)), such as some password hashing</span></span>
* <span data-ttu-id="56610-136">ASP.NET Core kimlik rolleri ve Kullanıcı oluşturma gibi dış API çağrıları gerektiren veriler</span><span class="sxs-lookup"><span data-stu-id="56610-136">Data that requires calls to external API, such as ASP.NET Core Identity roles and users creation</span></span>

## <a name="manual-migration-customization"></a><span data-ttu-id="56610-137">El ile geçiş özelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="56610-137">Manual migration customization</span></span>

<span data-ttu-id="56610-138">Bir geçiş eklendiğinde `HasData` ile belirtilen verilere yapılan değişiklikler `InsertData()`, `UpdateData()`ve `DeleteData()`çağrılarına dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="56610-138">When a migration is added the changes to the data specified with `HasData` are transformed to calls to `InsertData()`, `UpdateData()`, and `DeleteData()`.</span></span> <span data-ttu-id="56610-139">`HasData` bazı sınırlamalara geçici olarak çalışmanın bir yolu, bunun yerine bu çağrıları veya [özel işlemleri](xref:core/managing-schemas/migrations/operations) geçişe el ile eklemektir.</span><span class="sxs-lookup"><span data-stu-id="56610-139">One way of working around some of the limitations of `HasData` is to manually add these calls or [custom operations](xref:core/managing-schemas/migrations/operations) to the migration instead.</span></span>

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a><span data-ttu-id="56610-140">Özel başlatma mantığı</span><span class="sxs-lookup"><span data-stu-id="56610-140">Custom initialization logic</span></span>

<span data-ttu-id="56610-141">Veri dengeli dağıtımı yapmanın basit ve güçlü bir yolu, ana uygulama mantığı yürütmeye başlamadan önce [`DbContext.SaveChanges()`](xref:core/saving/index) kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="56610-141">A straightforward and powerful way to perform data seeding is to use [`DbContext.SaveChanges()`](xref:core/saving/index) before the main application logic begins execution.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> <span data-ttu-id="56610-142">Dağıtım kodu, birden çok örnek çalışırken eşzamanlılık sorunlarına yol açacağından ve ayrıca uygulamanın veritabanı şemasını değiştirme izni olmasını gerektirdiğinde, normal uygulama yürütmesinin parçası olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="56610-142">The seeding code should not be part of the normal app execution as this can cause concurrency issues when multiple instances are running and would also require the app having permission to modify the database schema.</span></span>

<span data-ttu-id="56610-143">Dağıtımınızın kısıtlamalarına bağlı olarak, başlatma kodu farklı yollarla yürütülebilir:</span><span class="sxs-lookup"><span data-stu-id="56610-143">Depending on the constraints of your deployment the initialization code can be executed in different ways:</span></span>

* <span data-ttu-id="56610-144">Başlatma uygulamasını yerel olarak çalıştırma</span><span class="sxs-lookup"><span data-stu-id="56610-144">Running the initialization app locally</span></span>
* <span data-ttu-id="56610-145">Başlangıç uygulamasını ana uygulamayla dağıtma, başlatma yordamını çağırarak ve başlatma uygulamasını devre dışı bırakma veya kaldırma.</span><span class="sxs-lookup"><span data-stu-id="56610-145">Deploying the initialization app with the main app, invoking the initialization routine and disabling or removing the initialization app.</span></span>

<span data-ttu-id="56610-146">Bu, genellikle [Yayımlama profilleri](/aspnet/core/host-and-deploy/visual-studio-publish-profiles)kullanılarak otomatikleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="56610-146">This can usually be automated by using [publish profiles](/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span></span>
