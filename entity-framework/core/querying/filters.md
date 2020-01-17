---
title: Genel sorgu filtreleri - EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: f4ee9b77411290249e763f9cb8492eea61803e91
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124398"
---
# <a name="global-query-filters"></a>Genel Sorgu Filtreleri

> [!NOTE]
> Bu özellik EF Core 2,0 ' de tanıtılmıştı.

Genel sorgu filtreleri olan LINQ Sorgu koşullarına (bir Boole ifadesi, LINQ to genellikle geçirilen *burada* sorgu işleci) meta veri modeli varlık türlerine uygulanır (genellikle *OnModelCreating*). Bu filtreler, varlık türleri Ekle kullanarak dolaylı olarak gibi başvurulan veya doğrudan bir gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ sorguları için otomatik olarak uygulanır. Bu özelliğin bazı ortak uygulamalar şunlardır:

* **Geçici silme** -bir varlık türü tanımlayan bir *IsDeleted* özelliği.
* **Çok kiracılılık** -bir varlık türü tanımlayan bir *Tenantıd* özelliği.

## <a name="example"></a>Örnek

Aşağıdaki örnek, basit bir blog oluşturma modelinde geçici silmeyi ve çok kiracılılık sorgu davranışları uygulamak için genel sorgu filtreleri kullanmayı gösterir.

> [!TIP]
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) GitHub üzerinde.

İlk olarak, varlıklar tanımlayın:

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

_Blog_ varlığındaki bir _tenantıd_ alanının bildirimine göz önünde varın. Bu, her Blog örneği belirli bir kiracı ile ilişkilendirmek için kullanılır. Ayrıca tanımlı olan bir _IsDeleted_ özelliği _Post_ varlık türü. Bu izlemenin olup için kullanılan bir _Post_ örneği "geçici silinen". Diğer bir deyişle, örnek, temel alınan verileri fiziksel olarak kaldırmadan silindi olarak işaretlenir.

Ardından, sorgu filtreleri yapılandırma _OnModelCreating_ kullanarak `HasQueryFilter` API.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

Koşul ifadeleri geçirilen _HasQueryFilter_ çağrıları artık otomatik olarak uygulanacak LINQ sorguları bu türleri için.

> [!TIP]
> DbContext örnek düzeyinde bir alanı kullanımına dikkat edin: `_tenantId` geçerli Kiracı ayarlamak için kullanılır. Model düzeyi filtreleri (diğer bir deyişle, sorguyu yürüten örnek) doğru bağlam örneğinin değerini kullanır.

> [!NOTE]
> Aynı varlık üzerinde birden çok sorgu filtresi tanımlamak mümkün değildir; yalnızca en son bir değer geçerli olur. Ancak, mantıksal _ve_ işlecini ([`&&` içinde C# ](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)) kullanarak birden çok koşuldan oluşan tek bir filtre tanımlayabilirsiniz.

## <a name="disabling-filters"></a>Filtreleri devre dışı bırakma

Filtreleri devre dışı bırakılabilir için tek tek LINQ sorgularını kullanarak `IgnoreQueryFilters()` işleci.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Sınırlamalar

Genel sorgu filtreleri aşağıdaki sınırlamalara sahiptir:

* Filtreler yalnızca varlık türü devralma hiyerarşisinin kökü için tanımlanabilir.
