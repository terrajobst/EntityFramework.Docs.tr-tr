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
# <a name="querying-data"></a><span data-ttu-id="0435a-102">Veri Sorgulama</span><span class="sxs-lookup"><span data-stu-id="0435a-102">Querying Data</span></span>

<span data-ttu-id="0435a-103">Entity Framework Core, veritabanındaki verileri sorgulamak için Dil Tümleşik Sorgusu 'nu (LINQ) kullanır.</span><span class="sxs-lookup"><span data-stu-id="0435a-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="0435a-104">LINQ, güçlü bir şekilde yazılan sorguları yazmak için C# (veya seçtiğiniz .NET dilini) kullanmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="0435a-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries.</span></span> <span data-ttu-id="0435a-105">Veritabanı nesnelerine başvurmak için türetilmiş bağlam ve varlık sınıflarınızı kullanır.</span><span class="sxs-lookup"><span data-stu-id="0435a-105">It uses your derived context and entity classes to reference database objects.</span></span> <span data-ttu-id="0435a-106">EF Core, LINQ sorgusunun bir temsilini veritabanı sağlayıcısına geçirir.</span><span class="sxs-lookup"><span data-stu-id="0435a-106">EF Core passes a representation of the LINQ query to the database provider.</span></span> <span data-ttu-id="0435a-107">Veritabanı sağlayıcıları da veritabanına özgü sorgu diline (örneğin, ilişkisel bir veritabanı için SQL) çevirir.</span><span class="sxs-lookup"><span data-stu-id="0435a-107">Database providers in turn translate it to database-specific query language (for example, SQL for a relational database).</span></span>

> [!TIP]
> <span data-ttu-id="0435a-108">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub'da görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0435a-108">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

<span data-ttu-id="0435a-109">Aşağıdaki parçacıklar, Entity Framework Core ile ortak görevlerin nasıl başarılalalabildiğini gösteren birkaç örnek gösterir.</span><span class="sxs-lookup"><span data-stu-id="0435a-109">The following snippets show a few examples of how to achieve common tasks with Entity Framework Core.</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="0435a-110">Tüm verileri yükleme</span><span class="sxs-lookup"><span data-stu-id="0435a-110">Loading all data</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a><span data-ttu-id="0435a-111">Tek bir varlığı yükleme</span><span class="sxs-lookup"><span data-stu-id="0435a-111">Loading a single entity</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a><span data-ttu-id="0435a-112">Filtreleme</span><span class="sxs-lookup"><span data-stu-id="0435a-112">Filtering</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a><span data-ttu-id="0435a-113">Diğer okumalar</span><span class="sxs-lookup"><span data-stu-id="0435a-113">Further readings</span></span>

- <span data-ttu-id="0435a-114">[LINQ sorgu ifadeleri](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations) hakkında daha fazla bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="0435a-114">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
- <span data-ttu-id="0435a-115">Bir sorgunun EF Core'da nasıl işlendiği hakkında daha ayrıntılı bilgi için sorgunun [nasıl çalıştığına](xref:core/querying/how-query-works)bakın.</span><span class="sxs-lookup"><span data-stu-id="0435a-115">For more detailed information on how a query is processed in EF Core, see [How Query Works](xref:core/querying/how-query-works).</span></span>
