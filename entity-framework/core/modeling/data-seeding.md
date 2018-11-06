---
title: Veri üretme - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 791f7afff36aac52fe2ffdc16ab580db22011b99
ms.sourcegitcommit: 082946dcaa1ee5174e692dbfe53adeed40609c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51028102"
---
# <a name="data-seeding"></a><span data-ttu-id="b6b02-102">Veri çekirdeği oluşturma</span><span class="sxs-lookup"><span data-stu-id="b6b02-102">Data Seeding</span></span>

<span data-ttu-id="b6b02-103">Veri üretme ilk bir veri kümesi veritabanıyla doldurma işlemidir.</span><span class="sxs-lookup"><span data-stu-id="b6b02-103">Data seeding is the process of populating a database with an initial set of data.</span></span>

<span data-ttu-id="b6b02-104">EF Core gerçekleştirilebilir birkaç yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="b6b02-104">There are several ways this can be accomplished in EF Core:</span></span>
* <span data-ttu-id="b6b02-105">Çekirdek veri modeli</span><span class="sxs-lookup"><span data-stu-id="b6b02-105">Model seed data</span></span>
* <span data-ttu-id="b6b02-106">El ile geçiş özelleştirme</span><span class="sxs-lookup"><span data-stu-id="b6b02-106">Manual migration customization</span></span>
* <span data-ttu-id="b6b02-107">Özel başlatma mantığı</span><span class="sxs-lookup"><span data-stu-id="b6b02-107">Custom initialization logic</span></span>

## <a name="model-seed-data"></a><span data-ttu-id="b6b02-108">Çekirdek veri modeli</span><span class="sxs-lookup"><span data-stu-id="b6b02-108">Model seed data</span></span>

> [!NOTE]
> <span data-ttu-id="b6b02-109">Bu özellik, EF Core 2.1 içinde yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="b6b02-109">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="b6b02-110">Aksine, EF Core EF6 veri dengeli dağıtım modeli yapılandırmasının bir parçası olarak bir varlık türü ile ilişkili olabilir.</span><span class="sxs-lookup"><span data-stu-id="b6b02-110">Unlike in EF6, in EF Core, seeding data can be associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="b6b02-111">Ardından EF Core [geçişler](xref:core/managing-schemas/migrations/index) ne ekleme, güncelleştirme veya silme işlemleri veritabanı modelin yeni bir sürüme yükseltirken uygulanmasına gerek işlem otomatik olarak.</span><span class="sxs-lookup"><span data-stu-id="b6b02-111">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

> [!NOTE]
> <span data-ttu-id="b6b02-112">Geçişler yalnızca göz önünde bulundurur model değişiklikleri belirlerken hangi işlem çekirdek veri istenen duruma getirmek için gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="b6b02-112">Migrations only considers model changes when determining what operation should be performed to get the seed data into the desired state.</span></span> <span data-ttu-id="b6b02-113">Bu nedenle geçişleri dışında yapılan değişiklikler bir kayıp veya hataya neden.</span><span class="sxs-lookup"><span data-stu-id="b6b02-113">Thus any changes to the data performed outside of migrations might be lost or cause an error.</span></span>

<span data-ttu-id="b6b02-114">Örnek olarak, bu için çekirdek veri yapılandıracak bir `Blog` içinde `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="b6b02-114">As an example, this will configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="b6b02-115">Bir ilişki, yabancı anahtar değerlerine sahip varlıklar eklemek için belirtilmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="b6b02-115">To add entities that have a relationship the foreign key values need to be specified:</span></span>

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="b6b02-116">Varlık türü anonim bir sınıf değerlerini sağlamak için kullanılabilir gölge durumda herhangi bir özellik varsa:</span><span class="sxs-lookup"><span data-stu-id="b6b02-116">If the entity type has any properties in shadow state an anonymous class can be used to provide the values:</span></span>

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

<span data-ttu-id="b6b02-117">Sahip olunan varlık türleri benzer bir biçimde sağlanmış:</span><span class="sxs-lookup"><span data-stu-id="b6b02-117">Owned entity types can be seeded in a similar fashion:</span></span>

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

<span data-ttu-id="b6b02-118">Bkz: [tam örnek proje](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) daha fazla bağlam için.</span><span class="sxs-lookup"><span data-stu-id="b6b02-118">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) for more context.</span></span>

<span data-ttu-id="b6b02-119">Veri modeline eklendikten sonra [geçişler](xref:core/managing-schemas/migrations/index) değişiklikleri uygulamak için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b02-119">Once the data has been added to the model, [migrations](xref:core/managing-schemas/migrations/index) should be used to apply the changes.</span></span>

> [!TIP]
> <span data-ttu-id="b6b02-120">Geçişleri otomatik bir dağıtımının bir parçası uygulamak gerekiyorsa [SQL komut dosyası oluşturma](xref:core/managing-schemas/migrations/index#generate-sql-scripts) , önizlemesi görüntülenemez yürütmeden önce.</span><span class="sxs-lookup"><span data-stu-id="b6b02-120">If you need to apply migrations as part of an automated deployment you can [create a SQL script](xref:core/managing-schemas/migrations/index#generate-sql-scripts) that can be previewed before execution.</span></span>

<span data-ttu-id="b6b02-121">Alternatif olarak, `context.Database.EnsureCreated()` bellek içi sağlayıcısı veya herhangi bir ilişkisi olmayan veritabanı kullanırken veya örneğin, bir test veritabanı için çekirdek veri içeren yeni bir veritabanı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="b6b02-121">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider or any non-relation database.</span></span> <span data-ttu-id="b6b02-122">Unutmayın veritabanı zaten var, `EnsureCreated()` ne ya da veritabanında çekirdek veri şemasını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="b6b02-122">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span> <span data-ttu-id="b6b02-123">İlişkisel veritabanları için çağrı olmamalıdır `EnsureCreated()` geçişler kullanmayı planlıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="b6b02-123">For relational databases you shouldn't call `EnsureCreated()` if you plan to use Migrations.</span></span>

<span data-ttu-id="b6b02-124">Bu tür verilerin çekirdek geçişleri tarafından yönetilir ve veritabanında zaten var olan verileri güncelleştirmek için komut veritabanına bağlanırken olmadan oluşturulması gereken.</span><span class="sxs-lookup"><span data-stu-id="b6b02-124">This type of seed data is managed by migrations and the script to update the data that's already in the database needs to be generated without connecting to the database.</span></span> <span data-ttu-id="b6b02-125">Bu, bazı kısıtlamaları getirir:</span><span class="sxs-lookup"><span data-stu-id="b6b02-125">This imposes some restrictions:</span></span>
* <span data-ttu-id="b6b02-126">Birincil anahtar değeri bile genellikle veritabanı tarafından oluşturulan belirtilmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="b6b02-126">The primary key value needs to be specified even if it's usually generated by the database.</span></span> <span data-ttu-id="b6b02-127">Geçişleri arasında veri değişikliklerini algılamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b6b02-127">It will be used to detect data changes between migrations.</span></span>
* <span data-ttu-id="b6b02-128">Birincil anahtarı herhangi bir şekilde değiştirilirse, daha önce çekirdeği oluşturulmuş veri kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="b6b02-128">Previously seeded data will be removed if the primary key is changed in any way.</span></span>

<span data-ttu-id="b6b02-129">Bu nedenle bu özellik, veritabanı, posta kodları gibi başka bir şey bağlı değildir ve geçişleri dışında değiştirmek beklenmiyor statik veriler için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b02-129">Therefore this feature is most useful for static data that's not expected to change outside of migrations and does not depend on anything else in the database, for example ZIP codes.</span></span>

<span data-ttu-id="b6b02-130">Senaryonuz aşağıdakilerden herhangi birini içeriyorsa, son bölümünde açıklanan özel başlatma mantığı kullanmak için önerilir:</span><span class="sxs-lookup"><span data-stu-id="b6b02-130">If your scenario includes any of the following it is recommended to use custom initialization logic described in the last section:</span></span>
* <span data-ttu-id="b6b02-131">Test etmek için geçici verileri</span><span class="sxs-lookup"><span data-stu-id="b6b02-131">Temporary data for testing</span></span>
* <span data-ttu-id="b6b02-132">Verileri veritabanı durumuna bağlıdır</span><span class="sxs-lookup"><span data-stu-id="b6b02-132">Data that depends on database state</span></span>
* <span data-ttu-id="b6b02-133">Alternatif anahtarlar kimlik olarak kullanmak varlıklar dahil olmak üzere veritabanı tarafından oluşturulması gereken anahtarı değerleri gereken verileri</span><span class="sxs-lookup"><span data-stu-id="b6b02-133">Data that needs key values to be generated by the database, including entities that use alternate keys as the identity</span></span>
* <span data-ttu-id="b6b02-134">Özel bir dönüştürme gerektiren verilerin (Bu işlenmez tarafından [değer dönüştürmeleri](xref:core/modeling/value-conversions)), gibi bazı parola karması</span><span class="sxs-lookup"><span data-stu-id="b6b02-134">Data that requires custom transformation (that is not handled by [value conversions](xref:core/modeling/value-conversions)), such as some password hashing</span></span>
* <span data-ttu-id="b6b02-135">ASP.NET Core kimliği roller ve kullanıcılar oluşturma gibi dış API çağrıları gerektiren</span><span class="sxs-lookup"><span data-stu-id="b6b02-135">Data that requires calls to external API, such as ASP.NET Core Identity roles and users creation</span></span>

## <a name="manual-migration-customization"></a><span data-ttu-id="b6b02-136">El ile geçiş özelleştirme</span><span class="sxs-lookup"><span data-stu-id="b6b02-136">Manual migration customization</span></span>

<span data-ttu-id="b6b02-137">Bir geçiş eklendiğinde değişiklikleri ile belirtilen verilerle `HasData` çağrıları dönüştürülür `InsertData()`, `UpdateData()`, ve `DeleteData()`.</span><span class="sxs-lookup"><span data-stu-id="b6b02-137">When a migration is added the changes to the data specified with `HasData` are transformed to calls to `InsertData()`, `UpdateData()`, and `DeleteData()`.</span></span> <span data-ttu-id="b6b02-138">Bazı sınırlamaları etrafında çalışma, tek yönlü `HasData` el ile bu çağrılar ekleyebilirsiniz veya [özel işlemler](xref:core/managing-schemas/migrations/operations) geçiş için bunun yerine.</span><span class="sxs-lookup"><span data-stu-id="b6b02-138">One way of working around some of the limitations of `HasData` is to manually add these calls or [custom operations](xref:core/managing-schemas/migrations/operations) to the migration instead.</span></span>

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a><span data-ttu-id="b6b02-139">Özel başlatma mantığı</span><span class="sxs-lookup"><span data-stu-id="b6b02-139">Custom initialization logic</span></span>

<span data-ttu-id="b6b02-140">Veri üretme gerçekleştirmek için basit ve güçlü bir yolu [ `DbContext.SaveChanges()` ](xref:core/saving/index) önce ana uygulama mantığı yürütmeyi başlatır.</span><span class="sxs-lookup"><span data-stu-id="b6b02-140">A straightforward and powerful way to perform data seeding is to use [`DbContext.SaveChanges()`](xref:core/saving/index) before the main application logic begins execution.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> <span data-ttu-id="b6b02-141">Birden çok örneği çalıştırıyorsanız ve veritabanı şeması değiştirme iznine sahip bir uygulama da gerektirecek bu eşzamanlılık sorunlarına yol açabileceğinden dengeli dağıtım kod normal uygulama yürütme parçası olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b02-141">The seeding code should not be part of the normal app execution as this can cause concurrency issues when multiple instances are running and would also require the app having permission to modify the database schema.</span></span>

<span data-ttu-id="b6b02-142">Dağıtımınızı kısıtlamalarına bağlı olarak başlatma kodu farklı yollarla Çalıştırılabilir:</span><span class="sxs-lookup"><span data-stu-id="b6b02-142">Depending on the constraints of your deployment the initialization code can be executed in different ways:</span></span>
* <span data-ttu-id="b6b02-143">Başlatma uygulamayı yerel olarak çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b6b02-143">Running the initialization app locally</span></span>
* <span data-ttu-id="b6b02-144">Başlatma yordamı çağırma ve devre dışı bırakma veya kaldırma başlatma uygulama ana uygulama başlatma uygulamayı dağıtma.</span><span class="sxs-lookup"><span data-stu-id="b6b02-144">Deploying the initialization app with the main app, invoking the initialization routine and disabling or removing the initialization app.</span></span>

<span data-ttu-id="b6b02-145">Bu genellikle kullanılarak otomatikleştirilebilir [yayımlama profilleri](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="b6b02-145">This can usually be automated by using [publish profiles](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span></span>
