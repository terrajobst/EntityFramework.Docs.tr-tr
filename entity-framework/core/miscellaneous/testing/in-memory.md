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
# <a name="testing-with-inmemory"></a>InMemory ile test etme

Gerçek veritabanı işlemlerinin ek yükü olmadan, gerçek veritabanına bağlanan bir şeyi kullanarak bileşenleri test etmek istediğinizde InMemory sağlayıcısı faydalıdır.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) GitHub ' da görebilirsiniz.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory bir ilişkisel veritabanı değil

EF Core veritabanı sağlayıcılarının ilişkisel veritabanları olması gerekmez. InMemory, test için genel amaçlı bir veritabanı olacak şekilde tasarlanmıştır ve ilişkisel bir veritabanını taklit etmek üzere tasarlanmamıştır.

Buna örnek olarak şunlar verilebilir:

* InMemory, ilişkisel bir veritabanında bilgi tutarlılığı kısıtlamalarını ihlal eden verileri kaydetmenizi sağlar.
* Modelinizdeki bir özellik için DefaultValueSql (String) kullanırsanız, bu ilişkisel bir veritabanı API 'sidir ve InMemory üzerinde çalışırken hiçbir etkiye sahip olmayacaktır.
* [Zaman damgası/satır sürümü](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` veya `IsRowVersion`) aracılığıyla eşzamanlılık desteklenmez. Bir güncelleştirme eski bir eşzamanlılık belirteci kullanılarak yapıldığında hiçbir [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) oluşturulmaz.

> [!TIP]  
> Birçok test amacıyla bu farklılıklar bu farklılıklara göre farklılık görmez. Bununla birlikte, doğru bir ilişkisel veritabanı gibi davranan bir şeye karşı test etmek isterseniz, [SQLite bellek içi modu](sqlite.md)kullanmayı düşünün.

## <a name="example-testing-scenario"></a>Örnek test senaryosu

Uygulama kodunun bloglarla ilgili bazı işlemleri gerçekleştirmesini sağlayan aşağıdaki hizmeti göz önünde bulundurun. Dahili olarak, bir SQL Server veritabanına bağlanan `DbContext` kullanır. Kodu değiştirmek zorunda kalmadan bu hizmete yönelik etkili testler yazabilmeniz veya bağlamın bir test Double 'u oluşturmak için çok fazla iş yapmanız gerekmeden, bu bağlamı bir InMemory veritabanına bağlanmak üzere değiştirmek yararlı olacaktır.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Bağlamınızı hazırlayın

### <a name="avoid-configuring-two-database-providers"></a>İki veritabanı sağlayıcısı yapılandırmaktan kaçının

Testlerinizde, InMemory sağlayıcısını kullanmak için bağlamını dışarıdan yapılandıracaksınız. Bir veritabanı sağlayıcısını, bağlamınızda `OnConfiguring` geçersiz kılarak yapılandırıyorsanız, yalnızca bir tane yapılandırılmışsa veritabanı sağlayıcısını yapılandırdığınızdan emin olmak için bazı koşullu kodlar eklemeniz gerekir.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> ASP.NET Core kullanıyorsanız, veritabanı sağlayıcınız zaten bağlam dışında (Startup.cs) yapılandırılmış olduğundan bu koda ihtiyacınız olmaz.

### <a name="add-a-constructor-for-testing"></a>Test için bir Oluşturucu ekleyin

Farklı bir veritabanına karşı test etkinleştirmenin en basit yolu, `DbContextOptions<TContext>`kabul eden bir oluşturucuyu göstermek için bağlamını değiştirmektir.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`, tüm ayarlarını, örneğin hangi veritabanına bağlanılacağını söyler. Bu, bağlamınızın Onyapılandırıyor yöntemi çalıştırılarak oluşturulan nesnedir.

## <a name="writing-tests"></a>Testleri yazma

Bu sağlayıcı ile test etmek için anahtar, bağlamı InMemory sağlayıcısını kullanma ve bellek içi veritabanının kapsamını denetleme olanağıdır. Genellikle her test yöntemi için temiz bir veritabanı istiyorsunuz.

InMemory veritabanını kullanan bir test sınıfına bir örnek aşağıda verilmiştir. Her test yöntemi benzersiz bir veritabanı adı belirtir, yani her yöntemin kendi InMemory veritabanına sahip olduğu anlamına gelir.

>[!TIP]
> `.UseInMemoryDatabase()` uzantısı yöntemini kullanmak için [Microsoft. EntityFrameworkCore. InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)NuGet paketine başvurun.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
