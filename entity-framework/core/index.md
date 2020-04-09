---
title: Varlık Çerçeve Çekirdeğine Genel Bakış - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416848"
---
# <a name="entity-framework-core"></a><span data-ttu-id="3a838-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3a838-102">Entity Framework Core</span></span>

<span data-ttu-id="3a838-103">Entity Framework (EF) Core, popüler Entity Framework veri erişim teknolojisinin hafif, genişletilebilir, [açık kaynak](https://github.com/aspnet/EntityFrameworkCore) kodlu ve çapraz platformlu bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="3a838-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="3a838-104">EF Core, .NET geliştiricilerin .NET nesnelerini kullanarak bir veritabanıyla çalışmasını sağlayan ve genellikle yazmaları gereken veri erişim kodunun çoğuna olan gereksinimi ortadan kaldıran bir nesne ilişkisisel mapper (O/RM) olarak hizmet verebilir.</span><span class="sxs-lookup"><span data-stu-id="3a838-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="3a838-105">EF Core birçok veritabanı altyapılarını destekler, ayrıntılar için [Veritabanı Sağlayıcıları'na](providers/index.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="3a838-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="3a838-106">The Model</span><span class="sxs-lookup"><span data-stu-id="3a838-106">The Model</span></span>

<span data-ttu-id="3a838-107">EF Core ile veri erişimi bir model kullanılarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="3a838-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="3a838-108">Bir model, varlık sınıfları ve veritabanıyla bir oturumu temsil eden ve verileri sorgulayıp kaydetmenize olanak tanıyan bir bağlam nesnesi oluşur.</span><span class="sxs-lookup"><span data-stu-id="3a838-108">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="3a838-109">Bkz. Daha fazla bilgi edinmek için [Model Oluşturma.](modeling/index.md)</span><span class="sxs-lookup"><span data-stu-id="3a838-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="3a838-110">Varolan bir veritabanından bir model oluşturabilir, veritabanınızla eşleşecek bir modeli el koduyla kodlayabilir veya modelinizden bir veritabanı oluşturmak için [EF Geçişleri'ni](managing-schemas/migrations/index.md) kullanabilir ve modeliniz zaman içinde değiştikçe bu modeli geliştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3a838-110">You can generate a model from an existing database, hand code a model to match your database, or use [EF Migrations](managing-schemas/migrations/index.md) to create a database from your model, and then evolve it as your model changes over time.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a><span data-ttu-id="3a838-111">Sorgulama</span><span class="sxs-lookup"><span data-stu-id="3a838-111">Querying</span></span>

<span data-ttu-id="3a838-112">Varlık sınıflarınızın örnekleri, Language Integrated Query (LINQ) kullanılarak veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="3a838-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="3a838-113">Daha fazla bilgi edinmek için [Verileri Sorgulama'ya](querying/index.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="3a838-113">See [Querying Data](querying/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a><span data-ttu-id="3a838-114">Verileri Kaydetme</span><span class="sxs-lookup"><span data-stu-id="3a838-114">Saving Data</span></span>

<span data-ttu-id="3a838-115">Veriler, varlık sınıflarınızın örnekleri kullanılarak veritabanında oluşturulur, silinir ve değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="3a838-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="3a838-116">Daha fazla bilgi edinmek için [Verileri Kaydetme'ye](saving/index.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="3a838-116">See [Saving Data](saving/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a><span data-ttu-id="3a838-117">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="3a838-117">Next steps</span></span>

<span data-ttu-id="3a838-118">Giriş eğitimleri için Entity [Framework Core ile Başlarken'e](get-started/index.md)bakın.</span><span class="sxs-lookup"><span data-stu-id="3a838-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>
