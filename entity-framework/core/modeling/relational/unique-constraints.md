---
title: EF Core için alternatif anahtarlar (benzersiz kısıtlamalar)-
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994197"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="61596-102">Alternatif anahtarlar (benzersiz kısıtlamalar)</span><span class="sxs-lookup"><span data-stu-id="61596-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="61596-103">Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="61596-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="61596-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="61596-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="61596-105">Modeldeki her alternatif anahtar için benzersiz kısıtlama tanıtılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="61596-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="61596-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="61596-106">Conventions</span></span>

<span data-ttu-id="61596-107">Kural gereği, dizin ve alternatif anahtar için sunulan kısıtlaması adlandırılacağını `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="61596-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="61596-108">Bileşik alternatif anahtarlar için `<property name>` özellik adlarının bir alt çizgi ayrılmış listesi olur.</span><span class="sxs-lookup"><span data-stu-id="61596-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="61596-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="61596-109">Data Annotations</span></span>

<span data-ttu-id="61596-110">Benzersiz kısıtlamalar veri ek açıklamalarını kullanma yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="61596-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="61596-111">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="61596-111">Fluent API</span></span>

<span data-ttu-id="61596-112">Fluent API'si alternatif anahtar dizini ve kısıtlama adı yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="61596-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
