---
title: "Inmemory - EF çekirdek test etme"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 33690e3424d0777930d3cb8167575fb0f4ddd8f7
ms.sourcegitcommit: d096484dcf9eff73d9943fa60db7a418b10ca0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
---
# <a name="testing-with-inmemory"></a>Inmemory ile test etme

Inmemory sağlayıcısı bileşenlerini gerçek veritabanı işlemleri yükü olmadan gerçek veritabanına bağlanma benzeyen bir şey kullanarak test etmek istediğiniz zaman yararlıdır.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) github'da.

## <a name="inmemory-is-not-a-relational-database"></a>Inmemory bir ilişkisel veritabanı değil

EF çekirdek veritabanı sağlayıcıları ilişkisel veritabanları olması gerekmez. Inmemory test etmek için bir genel amaçlı veritabanı olacak şekilde tasarlanmıştır ve ilişkisel veritabanı taklit etmek üzere tasarlanmamıştır.

Bu INCLUDE bazı örnekler:
* Inmemory ilişkisel bir veritabanındaki başvurusal bütünlük kısıtlamalarını ihlal ediyor veri kaydetmenize izin verir.

* Modelinizi bir özellik için DefaultValueSql(string) kullanırsanız, bu bir ilişkisel veritabanı API ve Inmemory karşı çalıştırılırken hiçbir etkisi olmaz.

> [!TIP]  
> Bu farklılıklar kadar test amaçları için önemli değil. Daha doğru bir ilişkisel veritabanı gibi davranan bir şey karşı test etmek isterseniz, ancak ardından kullanmayı [SQLite bellek içi modu](sqlite.md).

## <a name="example-testing-scenario"></a>Örnek Test senaryosu

Uygulama kodu Bloglara ilgili bazı işlemlerini gerçekleştirmek imkan tanıyan aşağıdaki hizmet göz önünde bulundurun. Dahili olarak kullanan bir `DbContext` bir SQL Server veritabanına bağlar. Böylece biz kodunu değiştirmek zorunda kalmadan bu hizmet için verimli testleri yazmak veya bir test oluşturmak için iş çok yapmak bir Inmemory veritabanına bağlanmak için bu bağlamda takas yararlı bağlamının çift.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>İçeriğiniz hazırlanın

### <a name="avoid-configuring-two-database-providers"></a>İki veritabanı sağlayıcısı yapılandırmamak

Testlerinizde dışarıdan bağlamın Inmemory sağlayıcıyı kullanacak şekilde yapılandırmak için adımıdır. Veritabanı sağlayıcısı geçersiz kılarak yapılandırıyorsanız `OnConfiguring` içeriğiniz, ardından, bir değil zaten yapılandırılmışsa, yalnızca veritabanı sağlayıcısı yapılandırdığınız emin olmak için koşullu biraz kod eklemeniz gerekir.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> ASP.NET Core kullanıyorsanız, veritabanı sağlayıcısı zaten bağlamda (haline) dışında yapılandırıldıktan sonra sonra bu kod gerekir.

### <a name="add-a-constructor-for-testing"></a>Test etmek için bir oluşturucu ekleyin

Kabul eden bir oluşturucu kullanıma sunmak için bağlamı değiştirmek için farklı bir veritabanına karşı test etkinleştirmek için en basit yolu olan bir `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`bağlam bağlanmak için hangi veritabanı gibi ayarlarından tümünü söyler. Bu, bağlamda OnConfiguring yöntemi çalıştırarak yerleşik aynı nesnesidir.

## <a name="writing-tests"></a>Testleri yazma

Bu sağlayıcı ile test için Inmemory Sağlayıcısı'nı kullanın ve bellek içi veritabanı kapsamını denetlemek için bağlamı söyleyin olanağı anahtardır. Genellikle her test yöntemi için temiz bir veritabanı istiyor.

Inmemory veritabanı kullanan bir test sınıfı bir örneği burada verilmiştir. Her yöntemin kendi Inmemory veritabanına olduğu anlamına gelen benzersiz bir veritabanı adı, her bir test yöntemi belirtir.

>[!TIP]
> Kullanılacak `.UseInMemoryDatabase()` genişletme yöntemi, başvuru NuGet paketi `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
