---
title: Temel Kaydet - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="basic-save"></a><span data-ttu-id="67780-102">Temel Kaydet</span><span class="sxs-lookup"><span data-stu-id="67780-102">Basic Save</span></span>

<span data-ttu-id="67780-103">Ekleme, değiştirme ve bağlamını ve varlık sınıflarını kullanarak verileri kaldırma öğrenin.</span><span class="sxs-lookup"><span data-stu-id="67780-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="67780-104">Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) github'da.</span><span class="sxs-lookup"><span data-stu-id="67780-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="67780-105">Veri ekleme</span><span class="sxs-lookup"><span data-stu-id="67780-105">Adding Data</span></span>

<span data-ttu-id="67780-106">Kullanım *DbSet.Add* varlık sınıflarınızı yeni örneklerini ekleme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="67780-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="67780-107">Veritabanında veri çağırdığınızda eklenir *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="67780-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="67780-108">Tüm çalışma tam grafiğin varlıkların Ekle, ekleme ve güncelleştirme yöntemleri geçirilen, kendilerine, açıklandığı gibi [ilgili verileri](related-data.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="67780-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="67780-109">Alternatif olarak, EntityEntry.State özelliği yalnızca tek bir varlık durumunu ayarlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="67780-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="67780-110">Örneğin, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="67780-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="67780-111">Verileri güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="67780-111">Updating Data</span></span>

<span data-ttu-id="67780-112">EF bağlam tarafından izlenen var olan bir varlığa yapılan değişiklikleri otomatik olarak algılar.</span><span class="sxs-lookup"><span data-stu-id="67780-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="67780-113">Bu, yük/sorgu veritabanından varlıkları ve önceden eklendi ve veritabanına kaydedilen varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="67780-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="67780-114">Yalnızca özelliklerine atanan değerlerini değiştirin ve ardından çağıran *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="67780-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="67780-115">Verileri silme</span><span class="sxs-lookup"><span data-stu-id="67780-115">Deleting Data</span></span>

<span data-ttu-id="67780-116">Kullanım *DbSet.Remove* , sınıflar örneklerini silmek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="67780-116">Use the *DbSet.Remove* method to delete instances of you entity classes.</span></span>

<span data-ttu-id="67780-117">Varlık veritabanında zaten varsa, sırasında silinecek *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="67780-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="67780-118">Varlık (yani, izlenir eklenen) veritabanı henüz kaydedilmedi varsa bağlamdan kaldırılır ve artık ne zaman eklenen *SaveChanges* olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="67780-118">If the entity has not yet been saved to the database (i.e. it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="67780-119">Tek bir SaveChanges birden çok işlemleri</span><span class="sxs-lookup"><span data-stu-id="67780-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="67780-120">Tek bir çağrı birden çok güncelleştirme/Ekle/Kaldır işlemlere birleştirebilirsiniz *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="67780-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="67780-121">Çoğu veritabanı sağlayıcısı için *SaveChanges* işlem.</span><span class="sxs-lookup"><span data-stu-id="67780-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="67780-122">Bunun anlamı tüm işlemlerin başarılı başarısız veya ve işlemleri hiçbir zaman sol kısmen uygulanır.</span><span class="sxs-lookup"><span data-stu-id="67780-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
