---
title: Sütun eşlemesi - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: 22fcafbfcf9daf765c94e6ca9c42d7770d3e7d07
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929868"
---
# <a name="column-mapping"></a><span data-ttu-id="9dcbb-102">Sütun eşleme</span><span class="sxs-lookup"><span data-stu-id="9dcbb-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="9dcbb-103">Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="9dcbb-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="9dcbb-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="9dcbb-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="9dcbb-105">Sütun eşlemesi, hangi sütunun veri gelen sorgulanabilen ve veritabanında kaydedilmiş tanımlar.</span><span class="sxs-lookup"><span data-stu-id="9dcbb-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="9dcbb-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="9dcbb-106">Conventions</span></span>

<span data-ttu-id="9dcbb-107">Kural gereği, her bir özellik özelliğiyle aynı ada sahip bir sütun eşlemek için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="9dcbb-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="9dcbb-108">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="9dcbb-108">Data Annotations</span></span>

<span data-ttu-id="9dcbb-109">Bir özellik için eşlenen sütun yapılandırmak için veri ek açıklamaları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9dcbb-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="9dcbb-110">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="9dcbb-110">Fluent API</span></span>

<span data-ttu-id="9dcbb-111">Fluent API'si, bir özellik için eşlenen sütun yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9dcbb-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=11-13)]
