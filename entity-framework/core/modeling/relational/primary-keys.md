---
title: Birincil anahtarlar-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: c195cc37859ea0689b5c6fbb84051564cf96c83a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656044"
---
# <a name="primary-keys"></a><span data-ttu-id="e26f0-102">Birincil Anahtarlar</span><span class="sxs-lookup"><span data-stu-id="e26f0-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="e26f0-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="e26f0-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="e26f0-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="e26f0-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="e26f0-105">Her varlık türünün anahtarı için bir birincil anahtar kısıtlaması tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e26f0-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="e26f0-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="e26f0-106">Conventions</span></span>

<span data-ttu-id="e26f0-107">Kurala göre, veritabanındaki birincil anahtar `PK_<type name>`olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="e26f0-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e26f0-108">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="e26f0-108">Data Annotations</span></span>

<span data-ttu-id="e26f0-109">Birincil anahtarın ilişkisel veritabanı özel yönleri, veri ek açıklamaları kullanılarak yapılandırılamaz.</span><span class="sxs-lookup"><span data-stu-id="e26f0-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e26f0-110">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="e26f0-110">Fluent API</span></span>

<span data-ttu-id="e26f0-111">Akıcı API 'YI, veritabanında birincil anahtar kısıtlamasının adını yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e26f0-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/KeyName.cs?name=KeyName&highlight=9)]
