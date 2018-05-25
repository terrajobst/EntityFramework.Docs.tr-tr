---
title: Genel sorgu filtreleri - EF çekirdek
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a>Genel sorgu filtreleri

Genel sorgu filtreleri şunlardır LINQ Sorgu koşulları (Boole ifadesi genellikle LINQ to geçirilen *nerede* sorgu işleci) meta veri modeli varlık türlerine uygulanan (genellikle *OnModelCreating*). Bu tür filtreleri, varlık türleri INCLUDE kullanımıyla dolaylı olarak gibi başvurulan veya doğrudan gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ Sorgu otomatik olarak uygulanır. Bu özellik, bazı ortak uygulamalar şunlardır:

* **Yumuşak silme** -bir varlık türünü tanımlayan bir *IsDeleted* özelliği.
* **Çoklu kiracı** -bir varlık türünü tanımlayan bir *Tenantıd* özelliği.

## <a name="example"></a>Örnek

Aşağıdaki örnek genel sorgu filtreleri basit blog modelinde yumuşak silebilir ve çoklu kiracı sorgu davranışlarını uygulamak için nasıl kullanılacağını gösterir.

> [!TIP]
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) github'da.

İlk olarak, varlıkları tanımlayın:

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

Bir _ bildirimi Not_Tenantıd_ alanını _Blog_ varlık. Bu, her bir Blog örneği belirli bir kiracı ile ilişkilendirmek için kullanılır. Ayrıca tanımlı olan bir _IsDeleted_ özelliği _Post_ varlık türü. Bu kullanılır olup izlenmesi için bir _Post_ örneği "geçici olarak silinen" açıldı. Yani Örnek temel alınan verileri fiziksel olarak kaldırma silinen withouth işaretlenir.

Ardından, sorgu filtreleri yapılandırma _OnModelCreating_ kullanarak ```HasQueryFilter``` API.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

Karşılaştırma ifadeleri geçirilen _HasQueryFilter_ çağrıları artık otomatik olarak uygulanacak bu türleri için herhangi bir LINQ Sorgu.

> [!TIP]
> DbContext örnek düzeyi alanı kullanımına dikkat edin: ```_tenantId``` geçerli Kiracı ayarlamak için kullanılır. Model düzeyindeki filtreleri doğru bağlamı örneğinden değeri kullanır. Yani Sorgu yürütülürken örneği.

## <a name="disabling-filters"></a>Filtreleri devre dışı bırakma

Filtreleri devre dışı bırakılabilir için tek tek LINQ sorgularını kullanarak ```IgnoreQueryFilters()``` işleci.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Sınırlamalar

Genel sorgu filtreleri aşağıdaki sınırlamalara sahiptir:

* Filtreler, gezinti özellikleri için başvuru içeremez.
* Filtreler yalnızca varlık türü bir devralma hiyerarşisini kök için tanımlanabilir.
