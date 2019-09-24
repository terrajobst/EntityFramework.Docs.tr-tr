---
title: Sütun eşleme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197206"
---
# <a name="column-mapping"></a><span data-ttu-id="74f02-102">Sütun Eşleme</span><span class="sxs-lookup"><span data-stu-id="74f02-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="74f02-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="74f02-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="74f02-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="74f02-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="74f02-105">Sütun eşleme, hangi sütun verilerinin sorgulandığını ve veritabanında veritabanına kaydedileceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="74f02-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="74f02-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="74f02-106">Conventions</span></span>

<span data-ttu-id="74f02-107">Kurala göre, her bir özellik, özelliği ile aynı ada sahip bir sütuna eşlenecek şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="74f02-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="74f02-108">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="74f02-108">Data Annotations</span></span>

<span data-ttu-id="74f02-109">Bir özelliğin eşlendiği sütunu yapılandırmak için, veri açıklamalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="74f02-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="74f02-110">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="74f02-110">Fluent API</span></span>

<span data-ttu-id="74f02-111">Bir özelliğin eşlendiği sütunu yapılandırmak için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="74f02-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
