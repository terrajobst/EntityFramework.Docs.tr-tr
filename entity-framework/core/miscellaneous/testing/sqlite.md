---
title: SQLite - EF Core ile test etme
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: e8ff204a09d50064b4f0d4376f02b05c8681ac25
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562539"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="96404-102">SQLite ile test etme</span><span class="sxs-lookup"><span data-stu-id="96404-102">Testing with SQLite</span></span>

<span data-ttu-id="96404-103">SQLite gerçek veritabanı işlemleri yükü olmadan ilişkisel bir veritabanında testler yazmak için SQLite kullanmanıza olanak sağlayan bir bellek içi modda sahiptir.</span><span class="sxs-lookup"><span data-stu-id="96404-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="96404-104">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) github'da</span><span class="sxs-lookup"><span data-stu-id="96404-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="96404-105">Örnek Test senaryosu</span><span class="sxs-lookup"><span data-stu-id="96404-105">Example testing scenario</span></span>

<span data-ttu-id="96404-106">Uygulama kodunun Bloglarda ilgili bazı işlemlerini gerçekleştirmenize izin veren aşağıdaki hizmet göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="96404-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="96404-107">Dahili olarak kullandığı bir `DbContext` bir SQL Server veritabanına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="96404-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="96404-108">Bu bağlam, böylece biz kodu değiştirmek zorunda kalmadan bu hizmet için verimli testleri yazmak veya birçok test oluşturmak için iş yapmak bir bellek içi SQLite veritabanına bağlanmak için takas etmek yararlı olabilecek çift bağlam.</span><span class="sxs-lookup"><span data-stu-id="96404-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="96404-109">Bağlamınızı hazırlanın</span><span class="sxs-lookup"><span data-stu-id="96404-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="96404-110">İki veritabanı sağlayıcısı yapılandırmamak</span><span class="sxs-lookup"><span data-stu-id="96404-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="96404-111">Testlerinizde dışarıdan bağlamı Inmemory sağlayıcıyı kullanacak şekilde yapılandırmak için yükleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="96404-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="96404-112">Veritabanı sağlayıcısı geçersiz kılarak yapılandırıyorsanız `OnConfiguring` Bağlamınızı, ardından, bir değil zaten yapılandırılmışsa, yalnızca veritabanı sağlayıcısı yapılandırdığınız emin olmak için bazı koşullu kodu eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="96404-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="96404-113">ASP.NET Core kullanıyorsanız, veritabanı sağlayıcınız bağlamında (Startup.cs) dışında yapılandırıldığından daha sonra bu kod gerek.</span><span class="sxs-lookup"><span data-stu-id="96404-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="96404-114">Test etmek için bir oluşturucu ekleyin</span><span class="sxs-lookup"><span data-stu-id="96404-114">Add a constructor for testing</span></span>

<span data-ttu-id="96404-115">Kabul eden bir oluşturucu kullanıma sunmak için Bağlamınızı değiştirmek için farklı bir veritabanına karşı test etkinleştirmek için en kolay yolu olan bir `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="96404-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="96404-116">`DbContextOptions<TContext>` bağlam tüm bağlanmak için hangi veritabanı gibi ilişkili ayarları belirtir.</span><span class="sxs-lookup"><span data-stu-id="96404-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="96404-117">Bağlamınızı içinde OnConfiguring yöntemi çalıştırarak oluşturulan aynı nesne budur.</span><span class="sxs-lookup"><span data-stu-id="96404-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="96404-118">Testleri yazma</span><span class="sxs-lookup"><span data-stu-id="96404-118">Writing tests</span></span>

<span data-ttu-id="96404-119">Bu sağlayıcıyla sınaması için SQLite kullanın ve bellek içi veritabanına kapsamını denetlemek için bağlam olanağından anahtardır.</span><span class="sxs-lookup"><span data-stu-id="96404-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="96404-120">Veritabanı kapsamı, açma ve kapatma bağlantı denetlenir.</span><span class="sxs-lookup"><span data-stu-id="96404-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="96404-121">Veritabanı bağlantı açıldıktan süre kapsamlıdır.</span><span class="sxs-lookup"><span data-stu-id="96404-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="96404-122">Genellikle her bir test yöntemi için temiz bir veritabanı istersiniz.</span><span class="sxs-lookup"><span data-stu-id="96404-122">Typically you want a clean database for each test method.</span></span>

>[!TIP]
> <span data-ttu-id="96404-123">Kullanılacak `SqliteConnection()` ve `.UseSqlite()` genişletme yöntemi, NuGet paketi başvurusu [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="96404-123">To use `SqliteConnection()` and the `.UseSqlite()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
