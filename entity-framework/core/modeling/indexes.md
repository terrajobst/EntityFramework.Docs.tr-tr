---
title: Dizinler-EF Core
author: roji
ms.date: 12/16/2019
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 810fccc0c6b035f515107601b245811f7b4118a6
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502142"
---
# <a name="indexes"></a>Dizinler

Dizinler birçok veri deposu genelinde ortak bir kavramdır. Veri deposundaki uygulamaları farklılık gösterebilir, ancak bir sütuna (veya sütun kümesine) göre aramalar yapmak için kullanılır.

Dizinler, veri açıklamaları kullanılarak oluşturulamaz. Tek bir sütunda dizin belirtmek için akıcı API 'yi aşağıdaki şekilde kullanabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=4)]

Ayrıca, birden fazla sütundan bir dizin belirtebilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=4)]

> [!NOTE]
> Kurala göre, yabancı anahtar olarak kullanılan her bir özellikte (veya özellik kümesinde) bir dizin oluşturulur.
>
> EF Core, farklı özellik kümesi başına yalnızca bir dizini destekler. Zaten bir dizini tanımlanmış, kural veya önceki yapılandırma ile tanımlanmış bir dizi özellik kümesi üzerinde bir dizin yapılandırmak için akıcı API kullanıyorsanız, bu dizinin tanımını değiştirmiş olursunuz. Kural tarafından oluşturulan bir dizini daha fazla yapılandırmak istiyorsanız bu yararlı olur.

## <a name="index-uniqueness"></a>Dizin benzersizliği

Varsayılan olarak, dizinler benzersiz değildir: birden çok satırın, dizinin sütun kümesi için aynı değere sahip olmasına izin verilir. Dizini aşağıdaki gibi benzersiz hale getirebilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=IndexUnique&highlight=5)]

Dizinin sütun kümesi için aynı değerlere sahip birden fazla varlık eklenmeye çalışıldığında bir özel durum oluşturulmasına neden olur.

## <a name="index-name"></a>Dizin adı

Kurala göre, ilişkisel bir veritabanında oluşturulan dizinlerin `IX_<type name>_<property name>`adı verilir. Bileşik dizinler için `<property name>`, özellik adlarının alt çizgiyle ayrılmış bir listesi haline gelir.

Veritabanında oluşturulan dizinin adını ayarlamak için Floent API 'sini kullanabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexName.cs?name=IndexName&highlight=5)]

## <a name="index-filter"></a>Dizin filtresi

Bazı ilişkisel veritabanları filtrelenmiş veya kısmi dizin belirtmenize olanak tanır. Bu, bir sütunun değerlerinin yalnızca bir alt kümesini dizinetmenize, dizin boyutunu azaltmanızı ve hem performans hem de disk alanı kullanımını iyileştirmeye olanak sağlar. Filtrelenmiş dizinler SQL Server hakkında daha fazla bilgi için [belgelerine bakın](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes).

Bir dizin üzerinde SQL ifadesi olarak sağlanmış bir filtre belirtmek için Floent API 'sini kullanabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexFilter.cs?name=IndexFilter&highlight=5)]

SQL Server sağlayıcısı EF kullanıldığında, benzersiz bir dizinin parçası olan tüm null yapılabilen sütunlar için bir `'IS NOT NULL'` filtresi ekler. Bu kuralı geçersiz kılmak için `null` bir değer sağlayabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexNoFilter.cs?name=IndexNoFilter&highlight=6)]

## <a name="included-columns"></a>Dahil edilen sütunlar

Bazı ilişkisel veritabanları, dizine dahil edilen ancak "Key" ın bir parçası olmayan bir sütun kümesini yapılandırmanızı sağlar. Bu, tabloya erişilmesi gerekmiyorsa sorgudaki tüm sütunlar anahtar veya anahtar olmayan sütunlar olarak dizine eklendiğinde sorgu performansını önemli ölçüde iyileştirebilir. İçerilen SQL Server sütunları hakkında daha fazla bilgi için [belgelerine bakın](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns).

Aşağıdaki örnekte, `Url` sütunu dizin anahtarının bir parçası olduğundan, bu sütunda herhangi bir sorgu filtrelemesi dizini kullanabilir. Ancak buna ek olarak, yalnızca `Title` ve `PublishedOn` sütunlarına erişen sorguların tabloya erişmesi ve daha verimli bir şekilde çalışması gerekecektir:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexInclude.cs?name=IndexInclude&highlight=5-9)]
