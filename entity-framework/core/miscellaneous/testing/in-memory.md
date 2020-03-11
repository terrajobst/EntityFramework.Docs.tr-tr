---
title: InMemory ile test etme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 18641677098c20d9172136b07868dcb647d189c6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416511"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="00560-102">InMemory ile test etme</span><span class="sxs-lookup"><span data-stu-id="00560-102">Testing with InMemory</span></span>

<span data-ttu-id="00560-103">Gerçek veritabanı işlemlerinin ek yükü olmadan, gerçek veritabanına bağlanan bir şeyi kullanarak bileşenleri test etmek istediğinizde InMemory sağlayıcısı faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="00560-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="00560-104">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) GitHub ' da görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="00560-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="00560-105">InMemory bir ilişkisel veritabanı değil</span><span class="sxs-lookup"><span data-stu-id="00560-105">InMemory is not a relational database</span></span>

<span data-ttu-id="00560-106">EF Core veritabanı sağlayıcılarının ilişkisel veritabanları olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="00560-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="00560-107">InMemory, test için genel amaçlı bir veritabanı olacak şekilde tasarlanmıştır ve ilişkisel bir veritabanını taklit etmek üzere tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="00560-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="00560-108">Buna örnek olarak şunlar verilebilir:</span><span class="sxs-lookup"><span data-stu-id="00560-108">Some examples of this include:</span></span>

* <span data-ttu-id="00560-109">InMemory, ilişkisel bir veritabanında bilgi tutarlılığı kısıtlamalarını ihlal eden verileri kaydetmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="00560-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="00560-110">Modelinizdeki bir özellik için DefaultValueSql (String) kullanırsanız, bu ilişkisel bir veritabanı API 'sidir ve InMemory üzerinde çalışırken hiçbir etkiye sahip olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="00560-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="00560-111">[Zaman damgası/satır sürümü](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` veya `IsRowVersion`) aracılığıyla eşzamanlılık desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="00560-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="00560-112">Bir güncelleştirme eski bir eşzamanlılık belirteci kullanılarak yapıldığında hiçbir [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="00560-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="00560-113">Birçok test amacıyla bu farklılıklar bu farklılıklara göre farklılık görmez.</span><span class="sxs-lookup"><span data-stu-id="00560-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="00560-114">Bununla birlikte, doğru bir ilişkisel veritabanı gibi davranan bir şeye karşı test etmek isterseniz, [SQLite bellek içi modu](sqlite.md)kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="00560-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="00560-115">Örnek test senaryosu</span><span class="sxs-lookup"><span data-stu-id="00560-115">Example testing scenario</span></span>

<span data-ttu-id="00560-116">Uygulama kodunun bloglarla ilgili bazı işlemleri gerçekleştirmesini sağlayan aşağıdaki hizmeti göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="00560-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="00560-117">Dahili olarak, bir SQL Server veritabanına bağlanan `DbContext` kullanır.</span><span class="sxs-lookup"><span data-stu-id="00560-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="00560-118">Kodu değiştirmek zorunda kalmadan bu hizmete yönelik etkili testler yazabilmeniz veya bağlamın bir test Double 'u oluşturmak için çok fazla iş yapmanız gerekmeden, bu bağlamı bir InMemory veritabanına bağlanmak üzere değiştirmek yararlı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="00560-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="00560-119">Bağlamınızı hazırlayın</span><span class="sxs-lookup"><span data-stu-id="00560-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="00560-120">İki veritabanı sağlayıcısı yapılandırmaktan kaçının</span><span class="sxs-lookup"><span data-stu-id="00560-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="00560-121">Testlerinizde, InMemory sağlayıcısını kullanmak için bağlamını dışarıdan yapılandıracaksınız.</span><span class="sxs-lookup"><span data-stu-id="00560-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="00560-122">Bir veritabanı sağlayıcısını, bağlamınızda `OnConfiguring` geçersiz kılarak yapılandırıyorsanız, yalnızca bir tane yapılandırılmışsa veritabanı sağlayıcısını yapılandırdığınızdan emin olmak için bazı koşullu kodlar eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="00560-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="00560-123">ASP.NET Core kullanıyorsanız, veritabanı sağlayıcınız zaten bağlam dışında (Startup.cs) yapılandırılmış olduğundan bu koda ihtiyacınız olmaz.</span><span class="sxs-lookup"><span data-stu-id="00560-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="00560-124">Test için bir Oluşturucu ekleyin</span><span class="sxs-lookup"><span data-stu-id="00560-124">Add a constructor for testing</span></span>

<span data-ttu-id="00560-125">Farklı bir veritabanına karşı test etkinleştirmenin en basit yolu, `DbContextOptions<TContext>`kabul eden bir oluşturucuyu göstermek için bağlamını değiştirmektir.</span><span class="sxs-lookup"><span data-stu-id="00560-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="00560-126">`DbContextOptions<TContext>`, tüm ayarlarını, örneğin hangi veritabanına bağlanılacağını söyler.</span><span class="sxs-lookup"><span data-stu-id="00560-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="00560-127">Bu, bağlamınızın Onyapılandırıyor yöntemi çalıştırılarak oluşturulan nesnedir.</span><span class="sxs-lookup"><span data-stu-id="00560-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="00560-128">Testleri yazma</span><span class="sxs-lookup"><span data-stu-id="00560-128">Writing tests</span></span>

<span data-ttu-id="00560-129">Bu sağlayıcı ile test etmek için anahtar, bağlamı InMemory sağlayıcısını kullanma ve bellek içi veritabanının kapsamını denetleme olanağıdır.</span><span class="sxs-lookup"><span data-stu-id="00560-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="00560-130">Genellikle her test yöntemi için temiz bir veritabanı istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="00560-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="00560-131">InMemory veritabanını kullanan bir test sınıfına bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="00560-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="00560-132">Her test yöntemi benzersiz bir veritabanı adı belirtir, yani her yöntemin kendi InMemory veritabanına sahip olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="00560-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="00560-133">`.UseInMemoryDatabase()` uzantısı yöntemini kullanmak için [Microsoft. EntityFrameworkCore. InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)NuGet paketine başvurun.</span><span class="sxs-lookup"><span data-stu-id="00560-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
