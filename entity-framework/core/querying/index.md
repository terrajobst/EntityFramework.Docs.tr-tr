---
title: Data - EF çekirdek sorgulama
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: a2dd830b25c64b007a881c105a87b5c631b00266
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="querying-data"></a><span data-ttu-id="70b04-102">Veri sorgulama</span><span class="sxs-lookup"><span data-stu-id="70b04-102">Querying Data</span></span>

<span data-ttu-id="70b04-103">Entity Framework Çekirdek dil tümleştirmek sorgu (LINQ) veritabanından verileri sorgulamak kullanır.</span><span class="sxs-lookup"><span data-stu-id="70b04-103">Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="70b04-104">LINQ, C# (veya .NET dilinizi tercih) türetilen bağlamını ve varlık sınıfları esas alarak kesin türü belirtilmiş sorguları yazmaya kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="70b04-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="70b04-105">LINQ Sorgu gösterimini veritabanına özel sorgu dili (örneğin bir ilişkisel veritabanı için SQL) çevrilmesi için veritabanı sağlayıcısı geçirilir.</span><span class="sxs-lookup"><span data-stu-id="70b04-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (e.g. SQL for a relational database).</span></span> <span data-ttu-id="70b04-106">Bir sorgu nasıl işlendiği hakkında daha ayrıntılı bilgi için bkz: [nasıl sorgu çalışır](overview.md).</span><span class="sxs-lookup"><span data-stu-id="70b04-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
