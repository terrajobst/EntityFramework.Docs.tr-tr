---
title: Genel sorgu filtreleri - EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 6b7a4069917c93015a218c131ff0d0a3920fb69d
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388409"
---
# <a name="global-query-filters"></a>Genel sorgu filtreleri

Genel sorgu filtreleri olan LINQ Sorgu koşullarına (bir Boole ifadesi, LINQ to genellikle geçirilen *burada* sorgu işleci) meta veri modeli varlık türlerine uygulanır (genellikle *OnModelCreating*). Bu filtreler, varlık türleri Ekle kullanarak dolaylı olarak gibi başvurulan veya doğrudan bir gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ sorguları için otomatik olarak uygulanır. Bu özelliğin bazı ortak uygulamalar şunlardır:

* **Geçici silme** -bir varlık türü tanımlayan bir *IsDeleted* özelliği.
* **Çok kiracılılık** -bir varlık türü tanımlayan bir *Tenantıd* özelliği.

## <a name="example"></a>Örnek

Aşağıdaki örnek, basit bir blog oluşturma modelinde geçici silmeyi ve çok kiracılılık sorgu davranışları uygulamak için genel sorgu filtreleri kullanmayı gösterir.

> [!TIP]
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters) GitHub üzerinde.

İlk olarak, varlıklar tanımlayın:

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Entities)]

Bir _ bildirimi Not_Tenantıd_ alanını _Blog_ varlık. Bu, her Blog örneği belirli bir kiracı ile ilişkilendirmek için kullanılır. Ayrıca tanımlı olan bir _IsDeleted_ özelliği _Post_ varlık türü. Bu izlemenin olup için kullanılan bir _Post_ örneği "geçici silinen". Diğer bir deyişle, örnek, temel alınan verileri fiziksel olarak kaldırmadan silindi olarak işaretlenir.

Ardından, sorgu filtreleri yapılandırma _OnModelCreating_ kullanarak ```HasQueryFilter``` API.

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Configuration)]

Koşul ifadeleri geçirilen _HasQueryFilter_ çağrıları artık otomatik olarak uygulanacak LINQ sorguları bu türleri için.

> [!TIP]
> DbContext örnek düzeyinde bir alanı kullanımına dikkat edin: ```_tenantId``` geçerli Kiracı ayarlamak için kullanılır. Model düzeyi filtreleri (diğer bir deyişle, sorguyu yürüten örnek) doğru bağlam örneğinin değerini kullanır.

## <a name="disabling-filters"></a>Filtreleri devre dışı bırakma

Filtreleri devre dışı bırakılabilir için tek tek LINQ sorgularını kullanarak ```IgnoreQueryFilters()``` işleci.

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Sınırlamalar

Genel sorgu filtreleri aşağıdaki sınırlamalara sahiptir:

* Filtreler, gezinti özellikleri başvurular içeremez.
* Filtreler yalnızca varlık türü devralma hiyerarşisinin kökü için tanımlanabilir.
