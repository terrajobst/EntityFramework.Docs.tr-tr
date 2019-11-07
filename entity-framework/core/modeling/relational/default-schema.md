---
title: Varsayılan şema-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655977"
---
# <a name="default-schema"></a><span data-ttu-id="35901-102">Varsayılan Şema</span><span class="sxs-lookup"><span data-stu-id="35901-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="35901-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="35901-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="35901-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="35901-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="35901-105">Varsayılan şema, bu nesne için bir şema açıkça yapılandırılmamışsa nesnelerin oluşturulacağı veritabanı şemadır.</span><span class="sxs-lookup"><span data-stu-id="35901-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="35901-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="35901-106">Conventions</span></span>

<span data-ttu-id="35901-107">Kural gereği, veritabanı sağlayıcısı en uygun varsayılan şemayı seçer.</span><span class="sxs-lookup"><span data-stu-id="35901-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="35901-108">Örneğin, Microsoft SQL Server `dbo` şemasını kullanır ve SQLite bir şema kullanmaz (çünkü şemalar SQLite ' de desteklenmez).</span><span class="sxs-lookup"><span data-stu-id="35901-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="35901-109">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="35901-109">Data Annotations</span></span>

<span data-ttu-id="35901-110">Veri ek açıklamalarını kullanarak varsayılan şemayı ayarlayamazsınız.</span><span class="sxs-lookup"><span data-stu-id="35901-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="35901-111">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="35901-111">Fluent API</span></span>

<span data-ttu-id="35901-112">Varsayılan bir şema belirtmek için Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="35901-112">You can use the Fluent API to specify a default schema.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
