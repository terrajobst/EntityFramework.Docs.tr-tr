---
title: EF Core - veri sorgulama
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: 447f48b780bc48b7a79153d17dcc1b8ef0cc508c
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949281"
---
# <a name="querying-data"></a><span data-ttu-id="3a434-102">Veri Sorgulama</span><span class="sxs-lookup"><span data-stu-id="3a434-102">Querying Data</span></span>

<span data-ttu-id="3a434-103">Entity Framework Core dil tümleşik sorgu (LINQ) veritabanından verileri sorgulamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="3a434-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="3a434-104">LINQ C# (veya tercih ettiğiniz .NET dilinizi) türetilen bağlamını ve varlık sınıflarında temel alan türü kesin belirlenmiş sorgular yazmak için kullanmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="3a434-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="3a434-105">LINQ sorgusu gösterimi (örneğin, ilişkisel bir veritabanı için SQL) veritabanına özel sorgu dilinde çevrilmesi için veritabanı sağlayıcısı geçirilir.</span><span class="sxs-lookup"><span data-stu-id="3a434-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (for example, SQL for a relational database).</span></span> <span data-ttu-id="3a434-106">Bir sorgunun nasıl işleneceği hakkında daha ayrıntılı bilgi için bkz: [nasıl sorgu çalışır](overview.md).</span><span class="sxs-lookup"><span data-stu-id="3a434-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
