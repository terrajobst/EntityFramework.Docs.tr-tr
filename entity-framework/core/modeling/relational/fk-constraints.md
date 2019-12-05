---
title: Yabancı anahtar kısıtlamaları-EF Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 2855137adf2ba3c9edaabd15a05f7a209f00f685
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824582"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="44cdf-102">Yabancı Anahtar Kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="44cdf-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="44cdf-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="44cdf-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="44cdf-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="44cdf-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="44cdf-105">Modeldeki her ilişki için bir yabancı anahtar kısıtlaması tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="44cdf-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="44cdf-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="44cdf-106">Conventions</span></span>

<span data-ttu-id="44cdf-107">Kurala göre, yabancı anahtar kısıtlamaları `FK_<dependent type name>_<principal type name>_<foreign key property name>`olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="44cdf-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="44cdf-108">Bileşik yabancı anahtarlar için `<foreign key property name>` yabancı anahtar özellik adlarının alt çizgiyle ayrılmış bir listesi haline gelir.</span><span class="sxs-lookup"><span data-stu-id="44cdf-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="44cdf-109">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="44cdf-109">Data Annotations</span></span>

<span data-ttu-id="44cdf-110">Yabancı anahtar kısıtlama adları, veri açıklamaları kullanılarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="44cdf-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="44cdf-111">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="44cdf-111">Fluent API</span></span>

<span data-ttu-id="44cdf-112">Bir ilişkinin yabancı anahtar kısıtlama adını yapılandırmak için akıcı API 'YI kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="44cdf-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
