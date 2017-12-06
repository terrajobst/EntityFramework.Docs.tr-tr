---
title: "SQLite - EF çekirdek test etme"
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
---
# <a name="testing-with-sqlite"></a>SQLite ile test etme

SQLite gerçek veritabanı işlemleri yükü olmadan bir ilişkisel veritabanı karşı testleri yazmak SQLite kullanmanıza olanak sağlayan bir bellek içi modu vardır.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) github'da

## <a name="example-testing-scenario"></a>Örnek Test senaryosu

Uygulama kodu Bloglara ilgili bazı işlemlerini gerçekleştirmek imkan tanıyan aşağıdaki hizmet göz önünde bulundurun. Dahili olarak kullanan bir `DbContext` bir SQL Server veritabanına bağlar. Böylece biz kodunu değiştirmek zorunda kalmadan bu hizmet için verimli testleri yazmak veya bir test oluşturmak için iş çok yapmak bir bellek içi SQLite veritabanına bağlanmak için bu bağlamda takas yararlı bağlamının çift.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>İçeriğiniz hazırlanın

### <a name="avoid-configuring-two-database-providers"></a>İki veritabanı sağlayıcısı yapılandırmamak

Testlerinizde dışarıdan bağlamın Inmemory sağlayıcıyı kullanacak şekilde yapılandırmak için adımıdır. Veritabanı sağlayıcısı geçersiz kılarak yapılandırıyorsanız `OnConfiguring` içeriğiniz, ardından, bir değil zaten yapılandırılmışsa, yalnızca veritabanı sağlayıcısı yapılandırdığınız emin olmak için koşullu biraz kod eklemeniz gerekir.

> [!TIP]  
> ASP.NET Core kullanıyorsanız, veritabanı sağlayıcısı (bağlamında haline) dışında yapılandırıldıktan sonra sonra bu kod gerekir.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Test etmek için bir oluşturucu ekleyin

Kabul eden bir oluşturucu kullanıma sunmak için bağlamı değiştirmek için farklı bir veritabanına karşı test etkinleştirmek için en basit yolu olan bir `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`bağlam bağlanmak için hangi veritabanı gibi ayarlarından tümünü söyler. Bu, bağlamda OnConfiguring yöntemi çalıştırarak yerleşik aynı nesnesidir.

## <a name="writing-tests"></a>Testleri yazma

Bu sağlayıcı ile test için SQLite kullanın ve bellek içi veritabanı kapsamını denetlemek için bağlamı söyleyin olanağı anahtardır. Veritabanı kapsamı açarak ve bağlantı kesiliyor denetlenir. Veritabanı bağlantı açıldıktan süresi kapsamlıdır. Genellikle her test yöntemi için temiz bir veritabanı istiyor.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
