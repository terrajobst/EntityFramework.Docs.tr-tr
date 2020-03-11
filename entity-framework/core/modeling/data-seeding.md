---
title: Veri dengeli dağıtımı-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 5c056c600f696ad1443ddb7b8c95c4b0ead06d21
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417228"
---
# <a name="data-seeding"></a>Veri Çekirdeği Oluşturma

Veri dengeli dağıtımı, bir veritabanını ilk veri kümesiyle doldurma işlemidir.

EF Core ' de gerçekleştirebilmenin birkaç yolu vardır:

* Temel verileri modelleyin
* El ile geçiş özelleştirmesi
* Özel başlatma mantığı

## <a name="model-seed-data"></a>Temel verileri modelleyin

> [!NOTE]
> Bu özellik EF Core 2,1 ' de yenidir.

EF6 'in aksine, EF Core, dengeli dağıtım verileri model yapılandırmasının bir parçası olarak bir varlık türüyle ilişkilendirilebilir. Daha sonra EF Core [geçişler](xref:core/managing-schemas/migrations/index) , veritabanının yeni bir sürümüne yükseltilirken ekleme, güncelleştirme veya silme işlemlerinin uygulanması gerektiğini otomatik olarak hesaplar.

> [!NOTE]
> Geçişler yalnızca, çekirdek verileri istenen duruma getirmek için hangi işlemin gerçekleştirilmesi gerektiğini belirlerken model değişikliklerini kabul eder. Bu nedenle, geçiş dışında gerçekleştirilen verilerde yapılan değişiklikler kaybolabilir veya hataya neden olabilir.

Örnek olarak, bu, `OnModelCreating`bir `Blog` için çekirdek verileri yapılandırır:

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

İlişkiye sahip varlıkları eklemek için yabancı anahtar değerleri belirtilmelidir:

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Varlık türünün gölge durumunda herhangi bir özelliği varsa, değerleri sağlamak için anonim bir sınıf kullanılabilir:

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

Sahip olan varlık türleri benzer bir biçimde çalıştırılabilir:

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

Daha fazla bağlam için bkz. [tam örnek proje](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) .

Veriler modele eklendikten sonra, değişiklikleri uygulamak için [geçişler](xref:core/managing-schemas/migrations/index) kullanılmalıdır.

> [!TIP]
> Bir otomatik dağıtımın parçası olarak geçişler uygulamanız gerekiyorsa, yürütmeden önce önizlenebilir [BIR SQL betiği oluşturabilirsiniz](xref:core/managing-schemas/migrations/index#generate-sql-scripts) .

Alternatif olarak, çekirdek verileri içeren yeni bir veritabanı oluşturmak için `context.Database.EnsureCreated()` kullanabilirsiniz (örneğin, bir test veritabanı veya bellek içi sağlayıcıyı veya hiçbir ilişki dışı veritabanını kullanırken). Veritabanı zaten varsa `EnsureCreated()` şemayı veya veritabanındaki çekirdek verileri güncelleştirdiğini unutmayın. Geçiş kullanmayı planlıyorsanız, ilişkisel veritabanları için `EnsureCreated()` Çağırmamanız gerekir.

### <a name="limitations-of-model-seed-data"></a>Model tohum verilerinin sınırlamaları

Bu tür çekirdek verileri geçişlerle yönetilir ve veritabanında zaten var olan verileri güncelleştirmek için betik veritabanına bağlanmadan oluşturulmalıdır. Bu, bazı kısıtlamalar getirir:

* Birincil anahtar değerinin, genellikle veritabanı tarafından oluşturulsa bile belirtilmesi gerekir. Geçişler arasındaki veri değişikliklerini algılamak için kullanılacaktır.
* Önceden sağlanan veriler, birincil anahtar herhangi bir şekilde değiştirilirse kaldırılır.

Bu nedenle, bu özellik, geçiş dışında değiştirilmesi beklenen statik veriler için en yararlı seçenektir ve örneğin ZIP kodları gibi veritabanında başka herhangi bir şeye bağlı değildir.

Senaryonuz aşağıdakilerden birini içeriyorsa, son bölümde açıklanan özel başlatma mantığını kullanmanız önerilir:

* Test için geçici veriler
* Veritabanı durumuna bağlı veriler
* Kimlik olarak alternatif anahtarlar kullanan varlıklar dahil olmak üzere, veritabanı tarafından oluşturulacak anahtar değerlere ihtiyacı olan veriler
* Bazı Parola karması gibi özel dönüşüm gerektiren ( [değer dönüştürmeleri](xref:core/modeling/value-conversions)tarafından işlenmemiş) veriler
* ASP.NET Core kimlik rolleri ve Kullanıcı oluşturma gibi dış API çağrıları gerektiren veriler

## <a name="manual-migration-customization"></a>El ile geçiş özelleştirmesi

Bir geçiş eklendiğinde `HasData` ile belirtilen verilere yapılan değişiklikler `InsertData()`, `UpdateData()`ve `DeleteData()`çağrılarına dönüştürülür. `HasData` bazı sınırlamalara geçici olarak çalışmanın bir yolu, bunun yerine bu çağrıları veya [özel işlemleri](xref:core/managing-schemas/migrations/operations) geçişe el ile eklemektir.

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a>Özel başlatma mantığı

Veri dengeli dağıtımı yapmanın basit ve güçlü bir yolu, ana uygulama mantığı yürütmeye başlamadan önce [`DbContext.SaveChanges()`](xref:core/saving/index) kullanmaktır.

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> Dağıtım kodu, birden çok örnek çalışırken eşzamanlılık sorunlarına yol açacağından ve ayrıca uygulamanın veritabanı şemasını değiştirme izni olmasını gerektirdiğinde, normal uygulama yürütmesinin parçası olmamalıdır.

Dağıtımınızın kısıtlamalarına bağlı olarak, başlatma kodu farklı yollarla yürütülebilir:

* Başlatma uygulamasını yerel olarak çalıştırma
* Başlangıç uygulamasını ana uygulamayla dağıtma, başlatma yordamını çağırarak ve başlatma uygulamasını devre dışı bırakma veya kaldırma.

Bu, genellikle [Yayımlama profilleri](/aspnet/core/host-and-deploy/visual-studio-publish-profiles)kullanılarak otomatikleştirilebilir.
