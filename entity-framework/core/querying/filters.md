---
title: Genel sorgu filtreleri - EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417731"
---
# <a name="global-query-filters"></a>Genel Sorgu Filtreleri

> [!NOTE]
> Bu özellik EF Core 2,0 ' de tanıtılmıştı.

Genel sorgu filtreleri LINQ sorgu koşullarına (genellikle LINQ *WHERE* sorgu işlecine geçirilen bir Boole ifadesi) meta veri modelindeki varlık türlerine (genellikle *onmodeloluþturma*içinde) uygulanan. Bu filtreler, varlık türleri Ekle kullanarak dolaylı olarak gibi başvurulan veya doğrudan bir gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ sorguları için otomatik olarak uygulanır. Bu özelliğin bazı ortak uygulamalar şunlardır:

* **Geçici silme** -bir varlık türü, *IsDeleted* özelliğini tanımlar.
* **Çok kiracılı** -varlık türü bir *tenantıd* özelliğini tanımlar.

## <a name="example"></a>Örnek

Aşağıdaki örnek, basit bir blog oluşturma modelinde geçici silmeyi ve çok kiracılılık sorgu davranışları uygulamak için genel sorgu filtreleri kullanmayı gösterir.

> [!TIP]
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) GitHub ' da görebilirsiniz.

İlk olarak, varlıklar tanımlayın:

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

_Blog_ varlığındaki bir _tenantıd_ alanının bildirimine göz önünde varın. Bu, her Blog örneği belirli bir kiracı ile ilişkilendirmek için kullanılır. Ayrıca, _Post_ varlık türünde bir _IsDeleted_ özelliği de tanımlanmıştır. Bu, bir _Post_ örneğinin "geçici olarak silinmiş" olup olmadığını izlemek için kullanılır. Diğer bir deyişle, örnek, temel alınan verileri fiziksel olarak kaldırmadan silindi olarak işaretlenir.

Sonra, `HasQueryFilter` API 'sini kullanarak _Onmodelyaratırken_ sorgu filtrelerini yapılandırın.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

_Hasqueryfilter_ çağrılarına geçirilen koşul ifadeleri artık bu türler için HERHANGI bir LINQ sorgusuna otomatik olarak uygulanır.

> [!TIP]
> DbContext örnek düzeyi alanının kullanımını, geçerli kiracıyı ayarlamak için kullanılan `_tenantId`. Model düzeyi filtreleri (diğer bir deyişle, sorguyu yürüten örnek) doğru bağlam örneğinin değerini kullanır.

> [!NOTE]
> Aynı varlık üzerinde birden çok sorgu filtresi tanımlamak mümkün değildir; yalnızca en son bir değer geçerli olur. Ancak, mantıksal _ve_ işlecini ([`&&` içinde C# ](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)) kullanarak birden çok koşuldan oluşan tek bir filtre tanımlayabilirsiniz.

## <a name="disabling-filters"></a>Filtreleri devre dışı bırakma

Filtreler, `IgnoreQueryFilters()` işleci kullanılarak tekil LINQ sorguları için devre dışı bırakılabilir.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Sınırlamalar

Genel sorgu filtreleri aşağıdaki sınırlamalara sahiptir:

* Filtreler yalnızca varlık türü devralma hiyerarşisinin kökü için tanımlanabilir.
