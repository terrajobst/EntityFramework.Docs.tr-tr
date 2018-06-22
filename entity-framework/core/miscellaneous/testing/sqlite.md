---
title: SQLite - EF çekirdek test etme
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054174"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="84947-102">SQLite ile test etme</span><span class="sxs-lookup"><span data-stu-id="84947-102">Testing with SQLite</span></span>

<span data-ttu-id="84947-103">SQLite gerçek veritabanı işlemleri yükü olmadan bir ilişkisel veritabanı karşı testleri yazmak SQLite kullanmanıza olanak sağlayan bir bellek içi modu vardır.</span><span class="sxs-lookup"><span data-stu-id="84947-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="84947-104">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) github'da</span><span class="sxs-lookup"><span data-stu-id="84947-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="84947-105">Örnek Test senaryosu</span><span class="sxs-lookup"><span data-stu-id="84947-105">Example testing scenario</span></span>

<span data-ttu-id="84947-106">Uygulama kodu Bloglara ilgili bazı işlemlerini gerçekleştirmek imkan tanıyan aşağıdaki hizmet göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="84947-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="84947-107">Dahili olarak kullanan bir `DbContext` bir SQL Server veritabanına bağlar.</span><span class="sxs-lookup"><span data-stu-id="84947-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="84947-108">Böylece biz kodunu değiştirmek zorunda kalmadan bu hizmet için verimli testleri yazmak veya bir test oluşturmak için iş çok yapmak bir bellek içi SQLite veritabanına bağlanmak için bu bağlamda takas yararlı bağlamının çift.</span><span class="sxs-lookup"><span data-stu-id="84947-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="84947-109">İçeriğiniz hazırlanın</span><span class="sxs-lookup"><span data-stu-id="84947-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="84947-110">İki veritabanı sağlayıcısı yapılandırmamak</span><span class="sxs-lookup"><span data-stu-id="84947-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="84947-111">Testlerinizde dışarıdan bağlamın Inmemory sağlayıcıyı kullanacak şekilde yapılandırmak için adımıdır.</span><span class="sxs-lookup"><span data-stu-id="84947-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="84947-112">Veritabanı sağlayıcısı geçersiz kılarak yapılandırıyorsanız `OnConfiguring` içeriğiniz, ardından, bir değil zaten yapılandırılmışsa, yalnızca veritabanı sağlayıcısı yapılandırdığınız emin olmak için koşullu biraz kod eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="84947-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="84947-113">ASP.NET Core kullanıyorsanız, veritabanı sağlayıcısı (bağlamında haline) dışında yapılandırıldıktan sonra sonra bu kod gerekir.</span><span class="sxs-lookup"><span data-stu-id="84947-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="84947-114">Test etmek için bir oluşturucu ekleyin</span><span class="sxs-lookup"><span data-stu-id="84947-114">Add a constructor for testing</span></span>

<span data-ttu-id="84947-115">Kabul eden bir oluşturucu kullanıma sunmak için bağlamı değiştirmek için farklı bir veritabanına karşı test etkinleştirmek için en basit yolu olan bir `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="84947-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="84947-116">`DbContextOptions<TContext>`bağlam bağlanmak için hangi veritabanı gibi ayarlarından tümünü söyler.</span><span class="sxs-lookup"><span data-stu-id="84947-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="84947-117">Bu, bağlamda OnConfiguring yöntemi çalıştırarak yerleşik aynı nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="84947-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="84947-118">Testleri yazma</span><span class="sxs-lookup"><span data-stu-id="84947-118">Writing tests</span></span>

<span data-ttu-id="84947-119">Bu sağlayıcı ile test için SQLite kullanın ve bellek içi veritabanı kapsamını denetlemek için bağlamı söyleyin olanağı anahtardır.</span><span class="sxs-lookup"><span data-stu-id="84947-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="84947-120">Veritabanı kapsamı açarak ve bağlantı kesiliyor denetlenir.</span><span class="sxs-lookup"><span data-stu-id="84947-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="84947-121">Veritabanı bağlantı açıldıktan süresi kapsamlıdır.</span><span class="sxs-lookup"><span data-stu-id="84947-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="84947-122">Genellikle her test yöntemi için temiz bir veritabanı istiyor.</span><span class="sxs-lookup"><span data-stu-id="84947-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
