---
title: SQLite ile test etme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417304"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="69dab-102">SQLite ile test etme</span><span class="sxs-lookup"><span data-stu-id="69dab-102">Testing with SQLite</span></span>

<span data-ttu-id="69dab-103">SQLite, gerçek veritabanı işlemlerinin ek yükü olmadan, bir ilişkisel veritabanına karşı testler yazmak için SQLite kullanmanıza olanak tanıyan bellek içi bir moda sahiptir.</span><span class="sxs-lookup"><span data-stu-id="69dab-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="69dab-104">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) GitHub 'da görüntüleyebilirsiniz</span><span class="sxs-lookup"><span data-stu-id="69dab-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="69dab-105">Örnek test senaryosu</span><span class="sxs-lookup"><span data-stu-id="69dab-105">Example testing scenario</span></span>

<span data-ttu-id="69dab-106">Uygulama kodunun bloglarla ilgili bazı işlemleri gerçekleştirmesini sağlayan aşağıdaki hizmeti göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="69dab-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="69dab-107">Dahili olarak, bir SQL Server veritabanına bağlanan `DbContext` kullanır.</span><span class="sxs-lookup"><span data-stu-id="69dab-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="69dab-108">Bu bağlamı, kodu değiştirmek zorunda kalmadan bu hizmete yönelik etkili testler yazabilmeniz veya içeriğin bir test Double 'u oluşturmak için çok fazla iş yapmak için bellek içi bir SQLite veritabanına bağlanmak üzere takas etmek yararlı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="69dab-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="69dab-109">Bağlamınızı hazırlayın</span><span class="sxs-lookup"><span data-stu-id="69dab-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="69dab-110">İki veritabanı sağlayıcısı yapılandırmaktan kaçının</span><span class="sxs-lookup"><span data-stu-id="69dab-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="69dab-111">Testlerinizde, InMemory sağlayıcısını kullanmak için bağlamını dışarıdan yapılandıracaksınız.</span><span class="sxs-lookup"><span data-stu-id="69dab-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="69dab-112">Bir veritabanı sağlayıcısını, bağlamınızda `OnConfiguring` geçersiz kılarak yapılandırıyorsanız, yalnızca bir tane yapılandırılmışsa veritabanı sağlayıcısını yapılandırdığınızdan emin olmak için bazı koşullu kodlar eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="69dab-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="69dab-113">ASP.NET Core kullanıyorsanız, veritabanı sağlayıcınız bağlam dışında yapılandırıldıktan sonra bu koda ihtiyacınız olmaz (Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="69dab-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="69dab-114">Test için bir Oluşturucu ekleyin</span><span class="sxs-lookup"><span data-stu-id="69dab-114">Add a constructor for testing</span></span>

<span data-ttu-id="69dab-115">Farklı bir veritabanına karşı test etkinleştirmenin en basit yolu, `DbContextOptions<TContext>`kabul eden bir oluşturucuyu göstermek için bağlamını değiştirmektir.</span><span class="sxs-lookup"><span data-stu-id="69dab-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="69dab-116">`DbContextOptions<TContext>`, tüm ayarlarını, örneğin hangi veritabanına bağlanılacağını söyler.</span><span class="sxs-lookup"><span data-stu-id="69dab-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="69dab-117">Bu, bağlamınızın Onyapılandırıyor yöntemi çalıştırılarak oluşturulan nesnedir.</span><span class="sxs-lookup"><span data-stu-id="69dab-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="69dab-118">Testleri yazma</span><span class="sxs-lookup"><span data-stu-id="69dab-118">Writing tests</span></span>

<span data-ttu-id="69dab-119">Bu sağlayıcı ile test etmek için kullanılan anahtar, bağlamın SQLite kullanmasını söylemek ve bellek içi veritabanının kapsamını denetleyebilmesidir.</span><span class="sxs-lookup"><span data-stu-id="69dab-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="69dab-120">Veritabanının kapsamı, bağlantı açılarak ve kapatılırken denetlenir.</span><span class="sxs-lookup"><span data-stu-id="69dab-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="69dab-121">Veritabanı, bağlantının açık olduğu süreye göre kapsamlandırılır.</span><span class="sxs-lookup"><span data-stu-id="69dab-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="69dab-122">Genellikle her test yöntemi için temiz bir veritabanı istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="69dab-122">Typically you want a clean database for each test method.</span></span>

>[!TIP]
> <span data-ttu-id="69dab-123">`SqliteConnection()` ve `.UseSqlite()` uzantısı metodunu kullanmak için [Microsoft. EntityFrameworkCore. SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)NuGet paketine başvurun.</span><span class="sxs-lookup"><span data-stu-id="69dab-123">To use `SqliteConnection()` and the `.UseSqlite()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
