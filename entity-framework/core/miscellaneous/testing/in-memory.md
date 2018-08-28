---
title: Inmemory - EF Core ile test etme
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 2754d1deba98fcee0eb88669293b2197545c8874
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997898"
---
# <a name="testing-with-inmemory"></a>Inmemory ile test etme

Inmemory sağlayıcısı bileşenlerini gerçek veritabanı işlemleri yükü olmadan gerçek veritabanına bağlanıyor benzeyen bir şey kullanarak test etmek istediğiniz yararlı olur.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) GitHub üzerinde.

## <a name="inmemory-is-not-a-relational-database"></a>Inmemory ilişkisel bir veritabanı değil.

EF Core veritabanı sağlayıcıları ilişkisel veritabanları olması gerekmez. Inmemory test etmek için bir genel amaçlı veritabanı olacak şekilde tasarlanmıştır ve bir ilişkisel veritabanı taklit etmek üzere tasarlanmamıştır.

Bu bazı örnekler:

* Inmemory ilişkisel bir veritabanındaki başvurusal bütünlük kısıtlamalarını ihlal ediyor veri kaydetmenize izin verir.
* Modelinizde bir özellik için DefaultValueSql(string) kullanırsanız, bu bir ilişkisel veritabanı API'si ve Inmemory karşı çalışan hiçbir etkisi olmaz.
* [Zaman damgası/satır sürümü aracılığıyla eşzamanlılık](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` veya `IsRowVersion`) desteklenmiyor. Hayır [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) bir güncelleştirme yapıldığında oluşturulan eski bir eşzamanlılık belirteci kullanarak.

> [!TIP]  
> Birçok test amaçları için bu farklılıkları önemli değildir. Daha doğru bir ilişkisel veritabanı gibi davranan bir şey karşı test etmek istediğiniz, ancak ardından kullanmayı [SQLite bellek içi modda](sqlite.md).

## <a name="example-testing-scenario"></a>Örnek Test senaryosu

Uygulama kodunun Bloglarda ilgili bazı işlemlerini gerçekleştirmenize izin veren aşağıdaki hizmet göz önünde bulundurun. Dahili olarak kullandığı bir `DbContext` bir SQL Server veritabanına bağlanır. Bu bağlam, böylece biz kodu değiştirmek zorunda kalmadan bu hizmet için verimli testleri yazmak veya birçok test oluşturmak için iş yapmak bir Inmemory veritabanına bağlanmak için takas etmek yararlı olabilecek çift bağlam.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Bağlamınızı hazırlanın

### <a name="avoid-configuring-two-database-providers"></a>İki veritabanı sağlayıcısı yapılandırmamak

Testlerinizde dışarıdan bağlamı Inmemory sağlayıcıyı kullanacak şekilde yapılandırmak için yükleyeceksiniz. Veritabanı sağlayıcısı geçersiz kılarak yapılandırıyorsanız `OnConfiguring` Bağlamınızı, ardından, bir değil zaten yapılandırılmışsa, yalnızca veritabanı sağlayıcısı yapılandırdığınız emin olmak için bazı koşullu kodu eklemeniz gerekir.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> ASP.NET Core kullanıyorsanız, veritabanı sağlayıcınız bağlamında (Startup.cs) dışında yapılandırıldığından sonra bu kod gerek.

### <a name="add-a-constructor-for-testing"></a>Test etmek için bir oluşturucu ekleyin

Kabul eden bir oluşturucu kullanıma sunmak için Bağlamınızı değiştirmek için farklı bir veritabanına karşı test etkinleştirmek için en kolay yolu olan bir `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` bağlam tüm bağlanmak için hangi veritabanı gibi ilişkili ayarları belirtir. Bağlamınızı içinde OnConfiguring yöntemi çalıştırarak oluşturulan aynı nesne budur.

## <a name="writing-tests"></a>Testleri yazma

Bu sağlayıcıyla sınaması için olanağından ve Inmemory sağlayıcıyı kullanmak ve bellek içi veritabanına kapsamını denetlemek için bağlam anahtardır. Genellikle her bir test yöntemi için temiz bir veritabanı istersiniz.

Inmemory veritabanı kullanan bir test sınıfı örneği aşağıda verilmiştir. Her test yönteminin her yöntemi kendi Inmemory veritabanı olduğu anlamına gelen bir benzersiz bir veritabanı adını belirtir.

>[!TIP]
> Kullanılacak `.UseInMemoryDatabase()` genişletme yöntemi, NuGet paketi başvurusu `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
