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
# <a name="testing-with-sqlite"></a>SQLite ile test etme

SQLite gerçek veritabanı işlemleri yükü olmadan ilişkisel bir veritabanında testler yazmak için SQLite kullanmanıza olanak sağlayan bir bellek içi modda sahiptir.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) github'da

## <a name="example-testing-scenario"></a>Örnek Test senaryosu

Uygulama kodunun Bloglarda ilgili bazı işlemlerini gerçekleştirmenize izin veren aşağıdaki hizmet göz önünde bulundurun. Dahili olarak kullandığı bir `DbContext` bir SQL Server veritabanına bağlanır. Bu bağlam, böylece biz kodu değiştirmek zorunda kalmadan bu hizmet için verimli testleri yazmak veya birçok test oluşturmak için iş yapmak bir bellek içi SQLite veritabanına bağlanmak için takas etmek yararlı olabilecek çift bağlam.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Bağlamınızı hazırlanın

### <a name="avoid-configuring-two-database-providers"></a>İki veritabanı sağlayıcısı yapılandırmamak

Testlerinizde dışarıdan bağlamı Inmemory sağlayıcıyı kullanacak şekilde yapılandırmak için yükleyeceksiniz. Veritabanı sağlayıcısı geçersiz kılarak yapılandırıyorsanız `OnConfiguring` Bağlamınızı, ardından, bir değil zaten yapılandırılmışsa, yalnızca veritabanı sağlayıcısı yapılandırdığınız emin olmak için bazı koşullu kodu eklemeniz gerekir.

> [!TIP]  
> ASP.NET Core kullanıyorsanız, veritabanı sağlayıcınız bağlamında (Startup.cs) dışında yapılandırıldığından daha sonra bu kod gerek.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Test etmek için bir oluşturucu ekleyin

Kabul eden bir oluşturucu kullanıma sunmak için Bağlamınızı değiştirmek için farklı bir veritabanına karşı test etkinleştirmek için en kolay yolu olan bir `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` bağlam tüm bağlanmak için hangi veritabanı gibi ilişkili ayarları belirtir. Bağlamınızı içinde OnConfiguring yöntemi çalıştırarak oluşturulan aynı nesne budur.

## <a name="writing-tests"></a>Testleri yazma

Bu sağlayıcıyla sınaması için SQLite kullanın ve bellek içi veritabanına kapsamını denetlemek için bağlam olanağından anahtardır. Veritabanı kapsamı, açma ve kapatma bağlantı denetlenir. Veritabanı bağlantı açıldıktan süre kapsamlıdır. Genellikle her bir test yöntemi için temiz bir veritabanı istersiniz.

>[!TIP]
> Kullanılacak `SqliteConnection()` ve `.UseSqlite()` genişletme yöntemi, NuGet paketi başvurusu [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
