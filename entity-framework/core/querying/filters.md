---
title: Genel Sorgu Filtreleri - EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417731"
---
# <a name="global-query-filters"></a>Genel Sorgu Filtreleri

> [!NOTE]
> Bu özellik EF Core 2.0 ile tanıtıldı.

Genel sorgu filtreleri LINQ sorgu yüklemleridir (genellikle *LINQ'ya* geçirilen bir boolean ifadesi sorgu işleci) meta veri modelinde Varlık Türleri'ne uygulanır (genellikle *OnModelOluşturma'da).* Bu tür filtreler, Dolaylı olarak başvurulan Varlık Türleri de dahil olmak üzere, bu Varlık Türlerini içeren tüm LINQ sorgularına (Örneğin veya doğrudan gezinme özelliği başvuruları nın kullanımı yoluyla) otomatik olarak uygulanır. Bu özelliğin bazı yaygın uygulamaları şunlardır:

* **Yumuşak silme** - Varlık Türü *Silinmiş* bir özelliği tanımlar.
* **Çoklu kira -** Varlık Türü *Kiracı Kimliği* özelliğini tanımlar.

## <a name="example"></a>Örnek

Aşağıdaki örnek, basit bir bloglama modelinde yumuşak silme ve çoklu kira sorgu davranışlarını uygulamak için Global Sorgu Filtreleri'nin nasıl kullanılacağını gösterir.

> [!TIP]
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) GitHub'da görüntüleyebilirsiniz.

İlk olarak, varlıkları tanımlayın:

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

_Blog_ varlığındaki _kiracı Kimliği_ alanının bildirimine dikkat edin. Bu, her blog örneğini belirli bir kiracıyla ilişkilendirmek için kullanılır. Ayrıca, _Post_ varlık türünde bir _Silinmiş_ özelliği tanımlanır. Bu, _Bir Gönderi_ örneğinin "yumuşak silinip silinmediğini" izlemek için kullanılır. Diğer bir zamanda, örnek, temel verileri fiziksel olarak kaldırmadan silinmiş olarak işaretlenir.

Ardından, `HasQueryFilter` API'yi kullanarak _OnModelOluşturma'daki_ sorgu filtrelerini yapılandırın.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

_HasQueryFilter_ çağrılarına geçirilen yüklem ifadeleri artık bu türler için linq sorgularına otomatik olarak uygulanır.

> [!TIP]
> Geçerli kiracıyı ayarlamak için `_tenantId` kullanılan DbContext örnek düzeyi alanının kullanımına dikkat edin. Model düzeyindefiltreler doğru bağlam örneğindeki değeri kullanır (diğer bir şekilde, sorguyu yürüten örnek).

> [!NOTE]
> Şu anda aynı varlık üzerinde birden çok sorgu filtresi tanımlamak mümkün değildir - yalnızca sonuncusu uygulanır. Ancak, mantıksal _VE_ işleci[ `&&` (C# )](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)kullanarak birden çok koşula sahip tek bir filtre tanımlayabilirsiniz.

## <a name="disabling-filters"></a>Filtreleri Devre Dışı Bırakma

`IgnoreQueryFilters()` Filtreler, işleci kullanarak tek tek LINQ sorguları için devre dışı bırakılmış olabilir.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Sınırlamalar

Genel sorgu filtreleri aşağıdaki sınırlamalara sahiptir:

* Filtreler yalnızca devralma hiyerarşisinin kök Varlık Türü için tanımlanabilir.
