---
title: Veri sorgulama-EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 009235c3673a414e06d1a64f9877b60e7cde97b0
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181925"
---
# <a name="querying-data"></a>Veri Sorgulama

Entity Framework Core, veritabanındaki verileri sorgulamak için dil ile tümleşik sorgu (LINQ) kullanır. LINQ, kesin tür belirtilmiş C# sorgular yazmak için (veya .net dilinizi tercih ettiğiniz) kullanmanıza olanak tanır. Veritabanı nesnelerine başvurmak için türetilmiş bağlam ve varlık sınıflarınızı kullanır. EF Core, LINQ sorgusunun bir gösterimini veritabanı sağlayıcısına geçirir. İçindeki veritabanı sağlayıcıları sırasıyla veritabanına özgü sorgu diline (örneğin, bir ilişkisel veritabanı için SQL) çevirir.

> [!TIP]
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.

Aşağıdaki kod parçacıklarında Entity Framework Core ortak görevlerin nasıl elde edilebilmesi için birkaç örnek gösterilmektedir.

## <a name="loading-all-data"></a>Tüm veriler yükleniyor

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a>Tek bir varlık yükleme

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a>Filtreleme

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a>Daha fazla okuma

- [LINQ sorgu ifadeleri](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations) hakkında daha fazla bilgi edinin
- EF Core bir sorgunun nasıl işlendiği hakkında daha ayrıntılı bilgi için bkz. [sorgunun nasıl çalıştığı](xref:core/querying/how-query-works).
