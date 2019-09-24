---
title: Maksimum uzunluk-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197216"
---
# <a name="maximum-length"></a><span data-ttu-id="e0185-102">En Fazla Uzunluk</span><span class="sxs-lookup"><span data-stu-id="e0185-102">Maximum Length</span></span>

<span data-ttu-id="e0185-103">En büyük uzunluk yapılandırması, belirli bir özellik için kullanılacak uygun veri türü hakkında veri deposuna bir ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="e0185-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="e0185-104">Maksimum uzunluk yalnızca, `string` ve `byte[]`gibi dizi veri türleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="e0185-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="e0185-105">Entity Framework, verileri sağlayıcıya geçirmeden önce en fazla uzunluk doğrulaması yapmaz.</span><span class="sxs-lookup"><span data-stu-id="e0185-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="e0185-106">Uygun olup olmadığını doğrulamak için sağlayıcıya veya veri deposuna kadar olur.</span><span class="sxs-lookup"><span data-stu-id="e0185-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="e0185-107">Örneğin, SQL Server hedeflenirken, en fazla uzunluğu aşarak temel alınan sütunun veri türü fazla verilerin depolanmasına izin vermez.</span><span class="sxs-lookup"><span data-stu-id="e0185-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="e0185-108">Kurallar</span><span class="sxs-lookup"><span data-stu-id="e0185-108">Conventions</span></span>

<span data-ttu-id="e0185-109">Kurala göre, özellikler için uygun bir veri türü seçmek üzere veritabanı sağlayıcısına ayrıldınız.</span><span class="sxs-lookup"><span data-stu-id="e0185-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="e0185-110">Uzunluğu olan özellikler için, veritabanı sağlayıcısı genellikle en uzun veri uzunluğuna izin veren bir veri türü seçer.</span><span class="sxs-lookup"><span data-stu-id="e0185-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="e0185-111">Örneğin, Microsoft SQL Server `nvarchar(max)` özellikler için `string` (veya `nvarchar(450)` sütun anahtar olarak kullanılıyorsa) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e0185-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e0185-112">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="e0185-112">Data Annotations</span></span>

<span data-ttu-id="e0185-113">Bir özellik için bir maksimum uzunluk yapılandırmak üzere veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0185-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="e0185-114">Bu örnekte, SQL Server hedefleme bu `nvarchar(500)` veri türünün kullanılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="e0185-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="e0185-115">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="e0185-115">Fluent API</span></span>

<span data-ttu-id="e0185-116">Bir özellik için maksimum uzunluğu yapılandırmak üzere akıcı API 'YI kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0185-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="e0185-117">Bu örnekte, SQL Server hedefleme bu `nvarchar(500)` veri türünün kullanılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="e0185-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
