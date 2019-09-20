---
title: Veri türleri-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: d667cbcb821e321faed36d097b531c7c55b81248
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149162"
---
# <a name="data-types"></a><span data-ttu-id="4d73b-102">Veri Türleri</span><span class="sxs-lookup"><span data-stu-id="4d73b-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="4d73b-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4d73b-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="4d73b-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="4d73b-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="4d73b-105">Veri türü, bir özelliğin eşlendiği sütunun veritabanına özgü türünü ifade eder.</span><span class="sxs-lookup"><span data-stu-id="4d73b-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="4d73b-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="4d73b-106">Conventions</span></span>

<span data-ttu-id="4d73b-107">Kural gereği, veritabanı sağlayıcısı özelliğin .NET türüne göre bir veri türü seçer.</span><span class="sxs-lookup"><span data-stu-id="4d73b-107">By convention, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="4d73b-108">Ayrıca, yapılandırılmış [Maksimum uzunluk](../max-length.md), özelliğin birincil bir anahtarın parçası olup olmadığı gibi diğer meta veriler de hesaba girer.</span><span class="sxs-lookup"><span data-stu-id="4d73b-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="4d73b-109">Örneğin, SQL Server `DateTime` özellikler için `datetime2(7)` `nvarchar(max)` ve `string` özellikler için (veya `nvarchar(450)` anahtar olarak kullanılan özellikler `string` için) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4d73b-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4d73b-110">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="4d73b-110">Data Annotations</span></span>

<span data-ttu-id="4d73b-111">Bir sütun için tam bir veri türü belirtmek için veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4d73b-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="4d73b-112">Örneğin, aşağıdaki kod, en `Url` fazla `200` uzunluğu olan Unicode olmayan bir dize olarak ve `Rating` kesinliği `5` ve ölçeği `2`ile ondalık olarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="4d73b-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="4d73b-113">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="4d73b-113">Fluent API</span></span>

<span data-ttu-id="4d73b-114">Ayrıca, sütunlar için aynı veri türlerini belirtmek üzere Floent API 'sini de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4d73b-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
