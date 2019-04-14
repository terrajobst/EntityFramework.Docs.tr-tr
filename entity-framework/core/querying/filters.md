---
title: Genel sorgu filtreleri - EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 4afc9fb0338d34845639d57013ac710445321940
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562448"
---
# <a name="global-query-filters"></a>Genel sorgu filtreleri

> [!NOTE]
> Bu özellik EF Core 2.0 sürümünde kullanıma sunulmuştur.

Genel sorgu filtreleri olan LINQ Sorgu koşullarına (bir Boole ifadesi, LINQ to genellikle geçirilen *burada* sorgu işleci) meta veri modeli varlık türlerine uygulanır (genellikle *OnModelCreating*). Bu filtreler, varlık türleri Ekle kullanarak dolaylı olarak gibi başvurulan veya doğrudan bir gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ sorguları için otomatik olarak uygulanır. Bu özelliğin bazı ortak uygulamalar şunlardır:

* **Geçici silme** -bir varlık türü tanımlayan bir *IsDeleted* özelliği.
* **Çok kiracılılık** -bir varlık türü tanımlayan bir *Tenantıd* özelliği.

## <a name="example"></a>Örnek

Aşağıdaki örnek, basit bir blog oluşturma modelinde geçici silmeyi ve çok kiracılılık sorgu davranışları uygulamak için genel sorgu filtreleri kullanmayı gösterir.

> [!TIP]
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) GitHub üzerinde.

İlk olarak, varlıklar tanımlayın:

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

Bir _ bildirimi Not_Tenantıd_ alanını _Blog_ varlık. Bu, her Blog örneği belirli bir kiracı ile ilişkilendirmek için kullanılır. Ayrıca tanımlı olan bir _IsDeleted_ özelliği _Post_ varlık türü. Bu izlemenin olup için kullanılan bir _Post_ örneği "geçici silinen". Diğer bir deyişle, örnek, temel alınan verileri fiziksel olarak kaldırmadan silindi olarak işaretlenir.

Ardından, sorgu filtreleri yapılandırma _OnModelCreating_ kullanarak ```HasQueryFilter``` API.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

Koşul ifadeleri geçirilen _HasQueryFilter_ çağrıları artık otomatik olarak uygulanacak LINQ sorguları bu türleri için.

> [!TIP]
> DbContext örnek düzeyinde bir alanı kullanımına dikkat edin: ```_tenantId``` geçerli Kiracı ayarlamak için kullanılır. Model düzeyi filtreleri (diğer bir deyişle, sorguyu yürüten örnek) doğru bağlam örneğinin değerini kullanır.

## <a name="disabling-filters"></a>Filtreleri devre dışı bırakma

Filtreleri devre dışı bırakılabilir için tek tek LINQ sorgularını kullanarak ```IgnoreQueryFilters()``` işleci.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Sınırlamalar

Genel sorgu filtreleri aşağıdaki sınırlamalara sahiptir:

* Filtreler, gezinti özellikleri başvurular içeremez.
* Filtreler yalnızca varlık türü devralma hiyerarşisinin kökü için tanımlanabilir.
