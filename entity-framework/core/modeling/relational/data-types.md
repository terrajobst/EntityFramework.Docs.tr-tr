---
title: Veri türleri - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2017
ms.locfileid: "26054546"
---
# <a name="data-types"></a><span data-ttu-id="0b441-102">Veri Türleri</span><span class="sxs-lookup"><span data-stu-id="0b441-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="0b441-103">Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0b441-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="0b441-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="0b441-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="0b441-105">Veri türü, veritabanının belirli bir özellik eşlenmiş sütununun türüne başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="0b441-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="0b441-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="0b441-106">Conventions</span></span>

<span data-ttu-id="0b441-107">Kurala göre veritabanı sağlayıcısı özelliğin CLR türüne göre bir veri türünü seçer.</span><span class="sxs-lookup"><span data-stu-id="0b441-107">By convention, the database provider selects a data type based on the CLR type of the property.</span></span> <span data-ttu-id="0b441-108">Ayrıca dikkate alır yapılandırılmış gibi diğer meta verileri [en fazla uzunluk](../max-length.md), özelliğin bir birincil anahtar, vb. bir parçası olup.</span><span class="sxs-lookup"><span data-stu-id="0b441-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="0b441-109">Örneğin, SQL Server kullanır `datetime2(7)` için `DateTime` özelliklerini ve `nvarchar(max)` için `string` özellikleri (veya `nvarchar(450)` için `string` bir anahtar olarak kullanılan özellikleri).</span><span class="sxs-lookup"><span data-stu-id="0b441-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0b441-110">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="0b441-110">Data Annotations</span></span>

<span data-ttu-id="0b441-111">Bir sütun için bir tam veri türünü belirtmek için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0b441-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="0b441-112">Örneğin aşağıdaki kodu yapılandırır `Url` unicode olmayan dize uzunluğu en fazla ile olarak `200` ve `Rating` kesinliğini ile ondalık `5` ve, ölçeği `2`.</span><span class="sxs-lookup"><span data-stu-id="0b441-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="0b441-113">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="0b441-113">Fluent API</span></span>

<span data-ttu-id="0b441-114">Fluent API, sütunları için aynı veri türlerini belirtmek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0b441-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
