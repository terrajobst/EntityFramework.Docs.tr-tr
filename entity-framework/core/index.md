---
title: Entity Framework Core genel bakış EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655618"
---
# <a name="entity-framework-core"></a><span data-ttu-id="554a1-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="554a1-102">Entity Framework Core</span></span>

<span data-ttu-id="554a1-103">Entity Framework (EF) Core, popüler Entity Framework veri erişim teknolojisinin basit, Genişletilebilir, [Açık kaynaklı](https://github.com/aspnet/EntityFrameworkCore) ve platformlar arası bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="554a1-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="554a1-104">EF Core, nesne ilişkisel Eşleyici (O/RM) olarak görev yapabilir, .NET geliştiricilerin .NET nesnelerini kullanarak bir veritabanıyla çalışmasını sağlar ve genellikle yazması gereken veri erişimi kodunun çoğunu ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="554a1-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="554a1-105">EF Core birçok veritabanı altyapısını destekler, Ayrıntılar için bkz. [veritabanı sağlayıcıları](providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="554a1-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="554a1-106">Model</span><span class="sxs-lookup"><span data-stu-id="554a1-106">The Model</span></span>

<span data-ttu-id="554a1-107">EF Core, veri erişimi bir model kullanılarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="554a1-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="554a1-108">Bir model, varlık sınıflarından ve veritabanı ile bir oturumu temsil eden bir bağlam nesnesinden oluşur ve verileri sorgulayıp kaydetmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="554a1-108">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="554a1-109">Daha fazla bilgi için bkz. [model oluşturma](modeling/index.md) .</span><span class="sxs-lookup"><span data-stu-id="554a1-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="554a1-110">Var olan bir veritabanından bir model oluşturabilir, bir modeli veritabanınızdan eşlemek üzere kodlayabilir veya modelinizden bir veritabanı oluşturmak için [EF geçişleri](managing-schemas/migrations/index.md) kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="554a1-110">You can generate a model from an existing database, hand code a model to match your database, or use [EF Migrations](managing-schemas/migrations/index.md) to create a database from your model, and then evolve it as your model changes over time.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a><span data-ttu-id="554a1-111">Oluştu</span><span class="sxs-lookup"><span data-stu-id="554a1-111">Querying</span></span>

<span data-ttu-id="554a1-112">Varlık sınıflarınızın örnekleri, dil ile tümleşik sorgu (LINQ) kullanılarak veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="554a1-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="554a1-113">Daha fazla bilgi için bkz. [veri sorgulama](querying/index.md) .</span><span class="sxs-lookup"><span data-stu-id="554a1-113">See [Querying Data](querying/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a><span data-ttu-id="554a1-114">Verileri Kaydetme</span><span class="sxs-lookup"><span data-stu-id="554a1-114">Saving Data</span></span>

<span data-ttu-id="554a1-115">Veri, varlık sınıflarınızın örnekleri kullanılarak oluşturulur, silinir ve değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="554a1-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="554a1-116">Daha fazla bilgi edinmek için [verileri kaydetme](saving/index.md) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="554a1-116">See [Saving Data](saving/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a><span data-ttu-id="554a1-117">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="554a1-117">Next steps</span></span>

<span data-ttu-id="554a1-118">Tanıtım öğreticileri için bkz. [Entity Framework Core kullanmaya](get-started/index.md)başlama.</span><span class="sxs-lookup"><span data-stu-id="554a1-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>
