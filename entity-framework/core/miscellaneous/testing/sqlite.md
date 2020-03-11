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
# <a name="testing-with-sqlite"></a>SQLite ile test etme

SQLite, gerçek veritabanı işlemlerinin ek yükü olmadan, bir ilişkisel veritabanına karşı testler yazmak için SQLite kullanmanıza olanak tanıyan bellek içi bir moda sahiptir.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) GitHub 'da görüntüleyebilirsiniz

## <a name="example-testing-scenario"></a>Örnek test senaryosu

Uygulama kodunun bloglarla ilgili bazı işlemleri gerçekleştirmesini sağlayan aşağıdaki hizmeti göz önünde bulundurun. Dahili olarak, bir SQL Server veritabanına bağlanan `DbContext` kullanır. Bu bağlamı, kodu değiştirmek zorunda kalmadan bu hizmete yönelik etkili testler yazabilmeniz veya içeriğin bir test Double 'u oluşturmak için çok fazla iş yapmak için bellek içi bir SQLite veritabanına bağlanmak üzere takas etmek yararlı olacaktır.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Bağlamınızı hazırlayın

### <a name="avoid-configuring-two-database-providers"></a>İki veritabanı sağlayıcısı yapılandırmaktan kaçının

Testlerinizde, InMemory sağlayıcısını kullanmak için bağlamını dışarıdan yapılandıracaksınız. Bir veritabanı sağlayıcısını, bağlamınızda `OnConfiguring` geçersiz kılarak yapılandırıyorsanız, yalnızca bir tane yapılandırılmışsa veritabanı sağlayıcısını yapılandırdığınızdan emin olmak için bazı koşullu kodlar eklemeniz gerekir.

> [!TIP]  
> ASP.NET Core kullanıyorsanız, veritabanı sağlayıcınız bağlam dışında yapılandırıldıktan sonra bu koda ihtiyacınız olmaz (Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Test için bir Oluşturucu ekleyin

Farklı bir veritabanına karşı test etkinleştirmenin en basit yolu, `DbContextOptions<TContext>`kabul eden bir oluşturucuyu göstermek için bağlamını değiştirmektir.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`, tüm ayarlarını, örneğin hangi veritabanına bağlanılacağını söyler. Bu, bağlamınızın Onyapılandırıyor yöntemi çalıştırılarak oluşturulan nesnedir.

## <a name="writing-tests"></a>Testleri yazma

Bu sağlayıcı ile test etmek için kullanılan anahtar, bağlamın SQLite kullanmasını söylemek ve bellek içi veritabanının kapsamını denetleyebilmesidir. Veritabanının kapsamı, bağlantı açılarak ve kapatılırken denetlenir. Veritabanı, bağlantının açık olduğu süreye göre kapsamlandırılır. Genellikle her test yöntemi için temiz bir veritabanı istiyorsunuz.

>[!TIP]
> `SqliteConnection()` ve `.UseSqlite()` uzantısı metodunu kullanmak için [Microsoft. EntityFrameworkCore. SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)NuGet paketine başvurun.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
