---
title: EF Core - veri sorgulama
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 51aaa5de11d3fe38b4fba82db8dcb5658088cc27
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993540"
---
# <a name="querying-data"></a><span data-ttu-id="60efc-102">Veri Sorgulama</span><span class="sxs-lookup"><span data-stu-id="60efc-102">Querying Data</span></span>

<span data-ttu-id="60efc-103">Entity Framework Core dil tümleşik sorgu (LINQ) veritabanından verileri sorgulamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="60efc-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="60efc-104">LINQ C# (veya tercih ettiğiniz .NET dilinizi) türetilen bağlamını ve varlık sınıflarında temel alan türü kesin belirlenmiş sorgular yazmak için kullanmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="60efc-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="60efc-105">LINQ sorgusu gösterimi (örneğin, ilişkisel bir veritabanı için SQL) veritabanına özel sorgu dilinde çevrilmesi için veritabanı sağlayıcısı geçirilir.</span><span class="sxs-lookup"><span data-stu-id="60efc-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (for example, SQL for a relational database).</span></span> <span data-ttu-id="60efc-106">Bir sorgunun nasıl işleneceği hakkında daha ayrıntılı bilgi için bkz: [nasıl sorgu çalışır](overview.md).</span><span class="sxs-lookup"><span data-stu-id="60efc-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
