---
title: Veri Sorgulama - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 0e1e50d1a3f647d65301552d0a447f9fcae81438
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417689"
---
# <a name="querying-data"></a>Veri Sorgulama

Entity Framework Core, veritabanındaki verileri sorgulamak için Dil Tümleşik Sorgusu 'nu (LINQ) kullanır. LINQ, güçlü bir şekilde yazılan sorguları yazmak için C# (veya seçtiğiniz .NET dilini) kullanmanıza olanak tanır. Veritabanı nesnelerine başvurmak için türetilmiş bağlam ve varlık sınıflarınızı kullanır. EF Core, LINQ sorgusunun bir temsilini veritabanı sağlayıcısına geçirir. Veritabanı sağlayıcıları da veritabanına özgü sorgu diline (örneğin, ilişkisel bir veritabanı için SQL) çevirir.

> [!TIP]
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub'da görüntüleyebilirsiniz.

Aşağıdaki parçacıklar, Entity Framework Core ile ortak görevlerin nasıl başarılalalabildiğini gösteren birkaç örnek gösterir.

## <a name="loading-all-data"></a>Tüm verileri yükleme

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a>Tek bir varlığı yükleme

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a>Filtreleme

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a>Diğer okumalar

- [LINQ sorgu ifadeleri](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations) hakkında daha fazla bilgi edinin
- Bir sorgunun EF Core'da nasıl işlendiği hakkında daha ayrıntılı bilgi için sorgunun [nasıl çalıştığına](xref:core/querying/how-query-works)bakın.
