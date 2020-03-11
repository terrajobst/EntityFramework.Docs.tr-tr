---
title: Veri sorgulama-EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 0e1e50d1a3f647d65301552d0a447f9fcae81438
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417689"
---
# <a name="querying-data"></a><span data-ttu-id="79ee8-102">Veri Sorgulama</span><span class="sxs-lookup"><span data-stu-id="79ee8-102">Querying Data</span></span>

<span data-ttu-id="79ee8-103">Entity Framework Core, veritabanındaki verileri sorgulamak için dil ile tümleşik sorgu (LINQ) kullanır.</span><span class="sxs-lookup"><span data-stu-id="79ee8-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="79ee8-104">LINQ, kesin tür belirtilmiş C# sorgular yazmak için (veya .net dilinizi tercih ettiğiniz) kullanmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="79ee8-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries.</span></span> <span data-ttu-id="79ee8-105">Veritabanı nesnelerine başvurmak için türetilmiş bağlam ve varlık sınıflarınızı kullanır.</span><span class="sxs-lookup"><span data-stu-id="79ee8-105">It uses your derived context and entity classes to reference database objects.</span></span> <span data-ttu-id="79ee8-106">EF Core, LINQ sorgusunun bir gösterimini veritabanı sağlayıcısına geçirir.</span><span class="sxs-lookup"><span data-stu-id="79ee8-106">EF Core passes a representation of the LINQ query to the database provider.</span></span> <span data-ttu-id="79ee8-107">İçindeki veritabanı sağlayıcıları sırasıyla veritabanına özgü sorgu diline (örneğin, bir ilişkisel veritabanı için SQL) çevirir.</span><span class="sxs-lookup"><span data-stu-id="79ee8-107">Database providers in turn translate it to database-specific query language (for example, SQL for a relational database).</span></span>

> [!TIP]
> <span data-ttu-id="79ee8-108">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub ' da görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79ee8-108">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

<span data-ttu-id="79ee8-109">Aşağıdaki kod parçacıklarında Entity Framework Core ortak görevlerin nasıl elde edilebilmesi için birkaç örnek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="79ee8-109">The following snippets show a few examples of how to achieve common tasks with Entity Framework Core.</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="79ee8-110">Tüm veriler yükleniyor</span><span class="sxs-lookup"><span data-stu-id="79ee8-110">Loading all data</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a><span data-ttu-id="79ee8-111">Tek bir varlık yükleme</span><span class="sxs-lookup"><span data-stu-id="79ee8-111">Loading a single entity</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a><span data-ttu-id="79ee8-112">Filtreleme</span><span class="sxs-lookup"><span data-stu-id="79ee8-112">Filtering</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a><span data-ttu-id="79ee8-113">Daha fazla okuma</span><span class="sxs-lookup"><span data-stu-id="79ee8-113">Further readings</span></span>

- <span data-ttu-id="79ee8-114">[LINQ sorgu ifadeleri](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations) hakkında daha fazla bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="79ee8-114">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
- <span data-ttu-id="79ee8-115">EF Core bir sorgunun nasıl işlendiği hakkında daha ayrıntılı bilgi için bkz. [sorgunun nasıl çalıştığı](xref:core/querying/how-query-works).</span><span class="sxs-lookup"><span data-stu-id="79ee8-115">For more detailed information on how a query is processed in EF Core, see [How Query Works](xref:core/querying/how-query-works).</span></span>
