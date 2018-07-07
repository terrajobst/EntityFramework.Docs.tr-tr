---
title: Inmemory - EF Core ile test etme
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: f814c8955e155688bb5e8d34b9c9f6d24dcc6601
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900268"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="d9f91-102">Inmemory ile test etme</span><span class="sxs-lookup"><span data-stu-id="d9f91-102">Testing with InMemory</span></span>

<span data-ttu-id="d9f91-103">Inmemory sağlayıcısı bileşenlerini gerçek veritabanı işlemleri yükü olmadan gerçek veritabanına bağlanıyor benzeyen bir şey kullanarak test etmek istediğiniz yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="d9f91-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="d9f91-104">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="d9f91-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="d9f91-105">Inmemory ilişkisel bir veritabanı değil.</span><span class="sxs-lookup"><span data-stu-id="d9f91-105">InMemory is not a relational database</span></span>

<span data-ttu-id="d9f91-106">EF Core veritabanı sağlayıcıları ilişkisel veritabanları olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="d9f91-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="d9f91-107">Inmemory test etmek için bir genel amaçlı veritabanı olacak şekilde tasarlanmıştır ve bir ilişkisel veritabanı taklit etmek üzere tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="d9f91-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="d9f91-108">Bu bazı örnekler:</span><span class="sxs-lookup"><span data-stu-id="d9f91-108">Some examples of this include:</span></span>

* <span data-ttu-id="d9f91-109">Inmemory ilişkisel bir veritabanındaki başvurusal bütünlük kısıtlamalarını ihlal ediyor veri kaydetmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="d9f91-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="d9f91-110">Modelinizde bir özellik için DefaultValueSql(string) kullanırsanız, bu bir ilişkisel veritabanı API'si ve Inmemory karşı çalışan hiçbir etkisi olmaz.</span><span class="sxs-lookup"><span data-stu-id="d9f91-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="d9f91-111">[Zaman damgası/satır sürümü aracılığıyla eşzamanlılık](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` veya `IsRowVersion`) desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="d9f91-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="d9f91-112">Hayır [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) bir güncelleştirme yapıldığında oluşturulan eski bir eşzamanlılık belirteci kullanarak.</span><span class="sxs-lookup"><span data-stu-id="d9f91-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="d9f91-113">Birçok test amaçları için bu farklılıkları önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="d9f91-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="d9f91-114">Daha doğru bir ilişkisel veritabanı gibi davranan bir şey karşı test etmek istediğiniz, ancak ardından kullanmayı [SQLite bellek içi modda](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="d9f91-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="d9f91-115">Örnek Test senaryosu</span><span class="sxs-lookup"><span data-stu-id="d9f91-115">Example testing scenario</span></span>

<span data-ttu-id="d9f91-116">Uygulama kodunun Bloglarda ilgili bazı işlemlerini gerçekleştirmenize izin veren aşağıdaki hizmet göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="d9f91-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="d9f91-117">Dahili olarak kullandığı bir `DbContext` bir SQL Server veritabanına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="d9f91-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="d9f91-118">Bu bağlam, böylece biz kodu değiştirmek zorunda kalmadan bu hizmet için verimli testleri yazmak veya birçok test oluşturmak için iş yapmak bir Inmemory veritabanına bağlanmak için takas etmek yararlı olabilecek çift bağlam.</span><span class="sxs-lookup"><span data-stu-id="d9f91-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="d9f91-119">Bağlamınızı hazırlanın</span><span class="sxs-lookup"><span data-stu-id="d9f91-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="d9f91-120">İki veritabanı sağlayıcısı yapılandırmamak</span><span class="sxs-lookup"><span data-stu-id="d9f91-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="d9f91-121">Testlerinizde dışarıdan bağlamı Inmemory sağlayıcıyı kullanacak şekilde yapılandırmak için yükleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="d9f91-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="d9f91-122">Veritabanı sağlayıcısı geçersiz kılarak yapılandırıyorsanız `OnConfiguring` Bağlamınızı, ardından, bir değil zaten yapılandırılmışsa, yalnızca veritabanı sağlayıcısı yapılandırdığınız emin olmak için bazı koşullu kodu eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d9f91-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="d9f91-123">ASP.NET Core kullanıyorsanız, veritabanı sağlayıcınız bağlamında (Startup.cs) dışında yapılandırıldığından sonra bu kod gerek.</span><span class="sxs-lookup"><span data-stu-id="d9f91-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="d9f91-124">Test etmek için bir oluşturucu ekleyin</span><span class="sxs-lookup"><span data-stu-id="d9f91-124">Add a constructor for testing</span></span>

<span data-ttu-id="d9f91-125">Kabul eden bir oluşturucu kullanıma sunmak için Bağlamınızı değiştirmek için farklı bir veritabanına karşı test etkinleştirmek için en kolay yolu olan bir `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="d9f91-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="d9f91-126">`DbContextOptions<TContext>` bağlam tüm bağlanmak için hangi veritabanı gibi ilişkili ayarları belirtir.</span><span class="sxs-lookup"><span data-stu-id="d9f91-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="d9f91-127">Bağlamınızı içinde OnConfiguring yöntemi çalıştırarak oluşturulan aynı nesne budur.</span><span class="sxs-lookup"><span data-stu-id="d9f91-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="d9f91-128">Testleri yazma</span><span class="sxs-lookup"><span data-stu-id="d9f91-128">Writing tests</span></span>

<span data-ttu-id="d9f91-129">Bu sağlayıcıyla sınaması için olanağından ve Inmemory sağlayıcıyı kullanmak ve bellek içi veritabanına kapsamını denetlemek için bağlam anahtardır.</span><span class="sxs-lookup"><span data-stu-id="d9f91-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="d9f91-130">Genellikle her bir test yöntemi için temiz bir veritabanı istersiniz.</span><span class="sxs-lookup"><span data-stu-id="d9f91-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="d9f91-131">Inmemory veritabanı kullanan bir test sınıfı örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d9f91-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="d9f91-132">Her test yönteminin her yöntemi kendi Inmemory veritabanı olduğu anlamına gelen bir benzersiz bir veritabanı adını belirtir.</span><span class="sxs-lookup"><span data-stu-id="d9f91-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="d9f91-133">Kullanılacak `.UseInMemoryDatabase()` genişletme yöntemi, NuGet paketi başvurusu `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="d9f91-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
