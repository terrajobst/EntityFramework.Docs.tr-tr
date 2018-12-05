---
title: Veri üretme - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 8f28dfea12461572ade8fbf3910ebd216dafb389
ms.sourcegitcommit: fa863883f1193d2118c2f9cee90808baa5e3e73e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52857435"
---
# <a name="data-seeding"></a>Veri çekirdeği oluşturma

Veri üretme ilk bir veri kümesi veritabanıyla doldurma işlemidir.

EF Core gerçekleştirilebilir birkaç yolu vardır:
* Çekirdek veri modeli
* El ile geçiş özelleştirme
* Özel başlatma mantığı

## <a name="model-seed-data"></a>Çekirdek veri modeli

> [!NOTE]
> Bu özellik, EF Core 2.1 içinde yeni bir özelliktir.

Aksine, EF Core EF6 veri dengeli dağıtım modeli yapılandırmasının bir parçası olarak bir varlık türü ile ilişkili olabilir. Ardından EF Core [geçişler](xref:core/managing-schemas/migrations/index) ne ekleme, güncelleştirme veya silme işlemleri veritabanı modelin yeni bir sürüme yükseltirken uygulanmasına gerek işlem otomatik olarak.

> [!NOTE]
> Geçişler yalnızca göz önünde bulundurur model değişiklikleri belirlerken hangi işlem çekirdek veri istenen duruma getirmek için gerçekleştirilmelidir. Bu nedenle geçişleri dışında yapılan değişiklikler bir kayıp veya hataya neden.

Örnek olarak, bu için çekirdek veri yapılandıracak bir `Blog` içinde `OnModelCreating`:

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Bir ilişki, yabancı anahtar değerlerine sahip varlıklar eklemek için belirtilmesi gerekir:

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Varlık türü anonim bir sınıf değerlerini sağlamak için kullanılabilir gölge durumda herhangi bir özellik varsa:

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

Sahip olunan varlık türleri benzer bir biçimde sağlanmış:

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

Bkz: [tam örnek proje](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) daha fazla bağlam için.

Veri modeline eklendikten sonra [geçişler](xref:core/managing-schemas/migrations/index) değişiklikleri uygulamak için kullanılmalıdır.

> [!TIP]
> Geçişleri otomatik bir dağıtımının bir parçası uygulamak gerekiyorsa [SQL komut dosyası oluşturma](xref:core/managing-schemas/migrations/index#generate-sql-scripts) , önizlemesi görüntülenemez yürütmeden önce.

Alternatif olarak, `context.Database.EnsureCreated()` bellek içi sağlayıcısı veya herhangi bir ilişkisi olmayan veritabanı kullanırken veya örneğin, bir test veritabanı için çekirdek veri içeren yeni bir veritabanı oluşturmak için. Unutmayın veritabanı zaten var, `EnsureCreated()` ne veritabanında şema veya çekirdek verileri güncelleştirir. İlişkisel veritabanları için çağrı olmamalıdır `EnsureCreated()` geçişler kullanmayı planlıyorsanız.

Bu tür verilerin çekirdek geçişleri tarafından yönetilir ve veritabanında zaten var olan verileri güncelleştirmek için komut veritabanına bağlanırken olmadan oluşturulması gereken. Bu, bazı kısıtlamaları getirir:
* Birincil anahtar değeri bile genellikle veritabanı tarafından oluşturulan belirtilmesi gerekiyor. Geçişleri arasında veri değişikliklerini algılamak için kullanılır.
* Birincil anahtarı herhangi bir şekilde değiştirilirse, daha önce çekirdeği oluşturulmuş veri kaldırılacak.

Bu nedenle bu özellik, veritabanı, posta kodları gibi başka bir şey bağlı değildir ve geçişleri dışında değiştirmek beklenmiyor statik veriler için kullanışlıdır.

Senaryonuz aşağıdakilerden herhangi birini içeriyorsa, son bölümünde açıklanan özel başlatma mantığı kullanmak için önerilir:
* Test etmek için geçici verileri
* Verileri veritabanı durumuna bağlıdır
* Alternatif anahtarlar kimlik olarak kullanmak varlıklar dahil olmak üzere veritabanı tarafından oluşturulması gereken anahtarı değerleri gereken verileri
* Özel bir dönüştürme gerektiren verilerin (Bu işlenmez tarafından [değer dönüştürmeleri](xref:core/modeling/value-conversions)), gibi bazı parola karması
* ASP.NET Core kimliği roller ve kullanıcılar oluşturma gibi dış API çağrıları gerektiren

## <a name="manual-migration-customization"></a>El ile geçiş özelleştirme

Bir geçiş eklendiğinde değişiklikleri ile belirtilen verilerle `HasData` çağrıları dönüştürülür `InsertData()`, `UpdateData()`, ve `DeleteData()`. Bazı sınırlamaları etrafında çalışma, tek yönlü `HasData` el ile bu çağrılar ekleyebilirsiniz veya [özel işlemler](xref:core/managing-schemas/migrations/operations) geçiş için bunun yerine.

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a>Özel başlatma mantığı

Veri üretme gerçekleştirmek için basit ve güçlü bir yolu [ `DbContext.SaveChanges()` ](xref:core/saving/index) önce ana uygulama mantığı yürütmeyi başlatır.

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> Birden çok örneği çalıştırıyorsanız ve veritabanı şeması değiştirme iznine sahip bir uygulama da gerektirecek bu eşzamanlılık sorunlarına yol açabileceğinden dengeli dağıtım kod normal uygulama yürütme parçası olmamalıdır.

Dağıtımınızı kısıtlamalarına bağlı olarak başlatma kodu farklı yollarla Çalıştırılabilir:
* Başlatma uygulamayı yerel olarak çalıştırma
* Başlatma yordamı çağırma ve devre dışı bırakma veya kaldırma başlatma uygulama ana uygulama başlatma uygulamayı dağıtma.

Bu genellikle kullanılarak otomatikleştirilebilir [yayımlama profilleri](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).
