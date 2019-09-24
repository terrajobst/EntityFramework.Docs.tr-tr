---
title: Alternatif anahtarlar (benzersiz kısıtlamalar)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197608"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="052d6-102">Alternatif Anahtarlar (Benzersiz Kısıtlamalar)</span><span class="sxs-lookup"><span data-stu-id="052d6-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="052d6-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="052d6-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="052d6-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="052d6-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="052d6-105">Modeldeki her alternatif anahtar için benzersiz bir kısıtlama getirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="052d6-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="052d6-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="052d6-106">Conventions</span></span>

<span data-ttu-id="052d6-107">Kurala göre, alternatif bir anahtar için tanıtılan dizin ve kısıtlama adlandıralınacaktır `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="052d6-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="052d6-108">Bileşik alternatif anahtarlar `<property name>` için özellik adlarının alt çizgiyle ayrılmış bir listesi haline gelir.</span><span class="sxs-lookup"><span data-stu-id="052d6-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="052d6-109">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="052d6-109">Data Annotations</span></span>

<span data-ttu-id="052d6-110">Benzersiz kısıtlamalar, veri ek açıklamaları kullanılarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="052d6-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="052d6-111">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="052d6-111">Fluent API</span></span>

<span data-ttu-id="052d6-112">Farklı bir anahtar için dizin ve kısıtlama adını yapılandırmak üzere Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="052d6-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
