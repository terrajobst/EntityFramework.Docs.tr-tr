---
title: "Alternatif anahtarları (benzersiz kısıtlamalar) - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="80241-102">Alternatif anahtarları (benzersiz kısıtlamalar)</span><span class="sxs-lookup"><span data-stu-id="80241-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="80241-103">Bu bölümdeki yapılandırma genel ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="80241-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="80241-104">İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir olacağı (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).</span><span class="sxs-lookup"><span data-stu-id="80241-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="80241-105">Model diğer her anahtar için benzersiz bir kısıtlama sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="80241-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="80241-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="80241-106">Conventions</span></span>

<span data-ttu-id="80241-107">Kurala göre indeks ve için alternatif bir anahtar sunulan kısıtlaması adlandırılacağını `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="80241-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="80241-108">Bileşik alternatif anahtarları için `<property name>` özellik adları alt çizgi ayrılmış listesini olur.</span><span class="sxs-lookup"><span data-stu-id="80241-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="80241-109">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="80241-109">Data Annotations</span></span>

<span data-ttu-id="80241-110">Benzersiz kısıtlamalar veri ek açıklamaları kullanılarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="80241-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="80241-111">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="80241-111">Fluent API</span></span>

<span data-ttu-id="80241-112">Fluent API, alternatif bir anahtar dizini ve kısıtlama adı yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="80241-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
