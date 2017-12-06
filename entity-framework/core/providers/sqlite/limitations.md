---
title: "SQLite veritabanı sağlayıcısı - sınırlamalar - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a><span data-ttu-id="4b3bb-102">SQLite EF çekirdek veritabanı sağlayıcısı sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="4b3bb-102">SQLite EF Core Database Provider Limitations</span></span>

<span data-ttu-id="4b3bb-103">SQLite sağlayıcısı bir dizi geçişler sınırlama vardır.</span><span class="sxs-lookup"><span data-stu-id="4b3bb-103">The SQLite provider has a number of migrations limitations.</span></span> <span data-ttu-id="4b3bb-104">Bu sınırlamalara çoğu sınırlamaları temel SQLite Veritabanı Altyapısı'ndaki bir sonucudur ve EF için özel değildir.</span><span class="sxs-lookup"><span data-stu-id="4b3bb-104">Most of these limitations are a result of limitations in the underlying SQLite database engine and are not specific to EF.</span></span>

## <a name="modeling-limitations"></a><span data-ttu-id="4b3bb-105">Modelleme sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="4b3bb-105">Modeling limitations</span></span>

<span data-ttu-id="4b3bb-106">(Entity Framework ilişkisel veritabanı sağlayıcıları tarafından paylaşılan) ortak ilişkisel kitaplığı API'leri, çoğu ilişkisel veritabanı motoru için ortak olan kavramları modelleme için tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4b3bb-106">The common relational library (shared by Entity Framework relational database providers) defines APIs for modelling concepts that are common to most relational database engines.</span></span> <span data-ttu-id="4b3bb-107">Bu kavramlar birkaç SQLite sağlayıcı tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="4b3bb-107">A couple of these concepts are not supported by the SQLite provider.</span></span>

* <span data-ttu-id="4b3bb-108">Şemaları</span><span class="sxs-lookup"><span data-stu-id="4b3bb-108">Schemas</span></span>
* <span data-ttu-id="4b3bb-109">Diziler</span><span class="sxs-lookup"><span data-stu-id="4b3bb-109">Sequences</span></span>

## <a name="migrations-limitations"></a><span data-ttu-id="4b3bb-110">Geçiş sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="4b3bb-110">Migrations limitations</span></span>

<span data-ttu-id="4b3bb-111">SQLite veritabanı altyapısı diğer ilişkisel veritabanları çoğunluğu tarafından desteklenen şema işlemlerinin sayısı desteklemez.</span><span class="sxs-lookup"><span data-stu-id="4b3bb-111">The SQLite database engine does not support a number of schema operations that are supported by the majority of other relational databases.</span></span> <span data-ttu-id="4b3bb-112">Desteklenmeyen işlemlerden biri, bir SQLite veritabanı için geçerli çalışırsanız sonra bir `NotSupportedException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4b3bb-112">If you attempt to apply one of the unsupported operations to a SQLite database then a `NotSupportedException` will be thrown.</span></span>

| <span data-ttu-id="4b3bb-113">İşlemi</span><span class="sxs-lookup"><span data-stu-id="4b3bb-113">Operation</span></span>            | <span data-ttu-id="4b3bb-114">Destekleniyor mu?</span><span class="sxs-lookup"><span data-stu-id="4b3bb-114">Supported?</span></span> |
| -------------------- | ---------- |
| <span data-ttu-id="4b3bb-115">AddColumn</span><span class="sxs-lookup"><span data-stu-id="4b3bb-115">AddColumn</span></span>            | <span data-ttu-id="4b3bb-116">✔</span><span class="sxs-lookup"><span data-stu-id="4b3bb-116">✔</span></span>          |
| <span data-ttu-id="4b3bb-117">AddForeignKey</span><span class="sxs-lookup"><span data-stu-id="4b3bb-117">AddForeignKey</span></span>        | <span data-ttu-id="4b3bb-118">✗</span><span class="sxs-lookup"><span data-stu-id="4b3bb-118">✗</span></span>          |
| <span data-ttu-id="4b3bb-119">AddPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="4b3bb-119">AddPrimaryKey</span></span>        | <span data-ttu-id="4b3bb-120">✗</span><span class="sxs-lookup"><span data-stu-id="4b3bb-120">✗</span></span>          |
| <span data-ttu-id="4b3bb-121">AddUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="4b3bb-121">AddUniqueConstraint</span></span>  | <span data-ttu-id="4b3bb-122">✗</span><span class="sxs-lookup"><span data-stu-id="4b3bb-122">✗</span></span>          |
| <span data-ttu-id="4b3bb-123">AlterColumn</span><span class="sxs-lookup"><span data-stu-id="4b3bb-123">AlterColumn</span></span>          | <span data-ttu-id="4b3bb-124">✗</span><span class="sxs-lookup"><span data-stu-id="4b3bb-124">✗</span></span>          |
| <span data-ttu-id="4b3bb-125">CreateIndex</span><span class="sxs-lookup"><span data-stu-id="4b3bb-125">CreateIndex</span></span>          | <span data-ttu-id="4b3bb-126">✔</span><span class="sxs-lookup"><span data-stu-id="4b3bb-126">✔</span></span>          |
| <span data-ttu-id="4b3bb-127">CreateTable</span><span class="sxs-lookup"><span data-stu-id="4b3bb-127">CreateTable</span></span>          | <span data-ttu-id="4b3bb-128">✔</span><span class="sxs-lookup"><span data-stu-id="4b3bb-128">✔</span></span>          |
| <span data-ttu-id="4b3bb-129">DropColumn</span><span class="sxs-lookup"><span data-stu-id="4b3bb-129">DropColumn</span></span>           | <span data-ttu-id="4b3bb-130">✗</span><span class="sxs-lookup"><span data-stu-id="4b3bb-130">✗</span></span>          |
| <span data-ttu-id="4b3bb-131">DropForeignKey</span><span class="sxs-lookup"><span data-stu-id="4b3bb-131">DropForeignKey</span></span>       | <span data-ttu-id="4b3bb-132">✗</span><span class="sxs-lookup"><span data-stu-id="4b3bb-132">✗</span></span>          |
| <span data-ttu-id="4b3bb-133">DropIndex</span><span class="sxs-lookup"><span data-stu-id="4b3bb-133">DropIndex</span></span>            | <span data-ttu-id="4b3bb-134">✔</span><span class="sxs-lookup"><span data-stu-id="4b3bb-134">✔</span></span>          |
| <span data-ttu-id="4b3bb-135">DropPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="4b3bb-135">DropPrimaryKey</span></span>       | <span data-ttu-id="4b3bb-136">✗</span><span class="sxs-lookup"><span data-stu-id="4b3bb-136">✗</span></span>          |
| <span data-ttu-id="4b3bb-137">DropTable</span><span class="sxs-lookup"><span data-stu-id="4b3bb-137">DropTable</span></span>            | <span data-ttu-id="4b3bb-138">✔</span><span class="sxs-lookup"><span data-stu-id="4b3bb-138">✔</span></span>          |
| <span data-ttu-id="4b3bb-139">DropUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="4b3bb-139">DropUniqueConstraint</span></span> | <span data-ttu-id="4b3bb-140">✗</span><span class="sxs-lookup"><span data-stu-id="4b3bb-140">✗</span></span>          |
| <span data-ttu-id="4b3bb-141">RenameColumn</span><span class="sxs-lookup"><span data-stu-id="4b3bb-141">RenameColumn</span></span>         | <span data-ttu-id="4b3bb-142">✗</span><span class="sxs-lookup"><span data-stu-id="4b3bb-142">✗</span></span>          |
| <span data-ttu-id="4b3bb-143">RenameIndex</span><span class="sxs-lookup"><span data-stu-id="4b3bb-143">RenameIndex</span></span>          | <span data-ttu-id="4b3bb-144">✗</span><span class="sxs-lookup"><span data-stu-id="4b3bb-144">✗</span></span>          |
| <span data-ttu-id="4b3bb-145">RenameTable</span><span class="sxs-lookup"><span data-stu-id="4b3bb-145">RenameTable</span></span>          | <span data-ttu-id="4b3bb-146">✔</span><span class="sxs-lookup"><span data-stu-id="4b3bb-146">✔</span></span>          |

## <a name="migrations-limitations-workaround"></a><span data-ttu-id="4b3bb-147">Geçiş sınırlamaları geçici çözüm</span><span class="sxs-lookup"><span data-stu-id="4b3bb-147">Migrations limitations workaround</span></span>

<span data-ttu-id="4b3bb-148">Geçici çözüm bazı yapabilecekleriniz sınırlamalara tablo gerçekleştirmek için geçiş kodu el ile yazarak, yeniden oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4b3bb-148">You can workaround some of these limitations by manually writing code in your migrations to perform a table rebuild.</span></span> <span data-ttu-id="4b3bb-149">Bir tablo yeniden oluşturma, varolan bir tabloyu yeniden adlandırma, yeni bir tablo oluşturma, yeni tabloya veri kopyalama ve eski tablo bırakma içerir.</span><span class="sxs-lookup"><span data-stu-id="4b3bb-149">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="4b3bb-150">Kullanmanız gerekecektir `Sql(string)` bazı adımları gerçekleştirmek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="4b3bb-150">You will need to use the `Sql(string)` method to perform some of these steps.</span></span>

<span data-ttu-id="4b3bb-151">Bkz: [yapmadan diğer türleri, tablo şema değişiklikleri](http://sqlite.org/lang_altertable.html#otheralter) daha fazla ayrıntı için SQLite belgelerinde.</span><span class="sxs-lookup"><span data-stu-id="4b3bb-151">See [Making Other Kinds Of Table Schema Changes](http://sqlite.org/lang_altertable.html#otheralter) in the SQLite documentation for more details.</span></span>

<span data-ttu-id="4b3bb-152">Gelecekte, EF bazı işlemlerini kapsar altında tablo yeniden yaklaşımı kullanarak destekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="4b3bb-152">In the future, EF may support some of these operations by using the table rebuild approach under the covers.</span></span> <span data-ttu-id="4b3bb-153">Yapabilecekleriniz [GitHub Projemizin bu özellik izlemek](https://github.com/aspnet/EntityFramework/issues/329).</span><span class="sxs-lookup"><span data-stu-id="4b3bb-153">You can [track this feature on our GitHub project](https://github.com/aspnet/EntityFramework/issues/329).</span></span>
