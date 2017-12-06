---
title: "Inmemory - EF çekirdek test etme"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: c5c48c575e9fd693d1f28d1a6d10eb83ebbc9d70
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2017
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="8b6cf-102">Inmemory ile test etme</span><span class="sxs-lookup"><span data-stu-id="8b6cf-102">Testing with InMemory</span></span>

<span data-ttu-id="8b6cf-103">Inmemory sağlayıcısı bileşenlerini gerçek veritabanı işlemleri yükü olmadan gerçek veritabanına bağlanma benzeyen bir şey kullanarak test etmek istediğiniz zaman yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="8b6cf-104">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) github'da.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="8b6cf-105">Inmemory bir ilişkisel veritabanı değil</span><span class="sxs-lookup"><span data-stu-id="8b6cf-105">InMemory is not a relational database</span></span>

<span data-ttu-id="8b6cf-106">EF çekirdek veritabanı sağlayıcıları ilişkisel veritabanları olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="8b6cf-107">Inmemory test etmek için bir genel amaçlı veritabanı olacak şekilde tasarlanmıştır ve ilişkisel veritabanı taklit etmek üzere tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="8b6cf-108">Bu INCLUDE bazı örnekler:</span><span class="sxs-lookup"><span data-stu-id="8b6cf-108">Some examples of this include:</span></span>
* <span data-ttu-id="8b6cf-109">Inmemory ilişkisel bir veritabanındaki başvurusal bütünlük kısıtlamalarını ihlal ediyor veri kaydetmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>

* <span data-ttu-id="8b6cf-110">Modelinizi bir özellik için DefaultValueSql(string) kullanırsanız, bu bir ilişkisel veritabanı API ve Inmemory karşı çalıştırılırken hiçbir etkisi olmaz.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>

> [!TIP]  
> <span data-ttu-id="8b6cf-111">Bu farklılıklar kadar test amaçları için önemli değil.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-111">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="8b6cf-112">Daha doğru bir ilişkisel veritabanı gibi davranan bir şey karşı test etmek isterseniz, ancak ardından kullanmayı [SQLite bellek içi modu](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="8b6cf-112">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="8b6cf-113">Örnek Test senaryosu</span><span class="sxs-lookup"><span data-stu-id="8b6cf-113">Example testing scenario</span></span>

<span data-ttu-id="8b6cf-114">Uygulama kodu Bloglara ilgili bazı işlemlerini gerçekleştirmek imkan tanıyan aşağıdaki hizmet göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-114">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="8b6cf-115">Dahili olarak kullanan bir `DbContext` bir SQL Server veritabanına bağlar.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-115">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="8b6cf-116">Böylece biz kodunu değiştirmek zorunda kalmadan bu hizmet için verimli testleri yazmak veya bir test oluşturmak için iş çok yapmak bir Inmemory veritabanına bağlanmak için bu bağlamda takas yararlı bağlamının çift.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-116">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="8b6cf-117">İçeriğiniz hazırlanın</span><span class="sxs-lookup"><span data-stu-id="8b6cf-117">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="8b6cf-118">İki veritabanı sağlayıcısı yapılandırmamak</span><span class="sxs-lookup"><span data-stu-id="8b6cf-118">Avoid configuring two database providers</span></span>

<span data-ttu-id="8b6cf-119">Testlerinizde dışarıdan bağlamın Inmemory sağlayıcıyı kullanacak şekilde yapılandırmak için adımıdır.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-119">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="8b6cf-120">Veritabanı sağlayıcısı geçersiz kılarak yapılandırıyorsanız `OnConfiguring` içeriğiniz, ardından, bir değil zaten yapılandırılmışsa, yalnızca veritabanı sağlayıcısı yapılandırdığınız emin olmak için koşullu biraz kod eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-120">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="8b6cf-121">ASP.NET Core kullanıyorsanız, veritabanı sağlayıcısı zaten bağlamda (haline) dışında yapılandırıldıktan sonra sonra bu kod gerekir.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-121">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="8b6cf-122">Test etmek için bir oluşturucu ekleyin</span><span class="sxs-lookup"><span data-stu-id="8b6cf-122">Add a constructor for testing</span></span>

<span data-ttu-id="8b6cf-123">Kabul eden bir oluşturucu kullanıma sunmak için bağlamı değiştirmek için farklı bir veritabanına karşı test etkinleştirmek için en basit yolu olan bir `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-123">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="8b6cf-124">`DbContextOptions<TContext>`bağlam bağlanmak için hangi veritabanı gibi ayarlarından tümünü söyler.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-124">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="8b6cf-125">Bu, bağlamda OnConfiguring yöntemi çalıştırarak yerleşik aynı nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-125">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="8b6cf-126">Testleri yazma</span><span class="sxs-lookup"><span data-stu-id="8b6cf-126">Writing tests</span></span>

<span data-ttu-id="8b6cf-127">Bu sağlayıcı ile test için Inmemory Sağlayıcısı'nı kullanın ve bellek içi veritabanı kapsamını denetlemek için bağlamı söyleyin olanağı anahtardır.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-127">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="8b6cf-128">Genellikle her test yöntemi için temiz bir veritabanı istiyor.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-128">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="8b6cf-129">Inmemory veritabanı kullanan bir test sınıfı bir örneği burada verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-129">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="8b6cf-130">Her yöntemin kendi Inmemory veritabanına olduğu anlamına gelen benzersiz bir veritabanı adı, her bir test yöntemi belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-130">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="8b6cf-131">Kullanılacak `.UseInMemoryDatabase()` genişletme yöntemi, başvuru Nuget paketi `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="8b6cf-131">To use the `.UseInMemoryDatabase()` extension method, reference the Nuget package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
