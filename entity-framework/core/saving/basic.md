---
title: Temel kaydetme - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: ecf8f344a5baae37a5e7255a4affb1085f1b3ff3
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949404"
---
# <a name="basic-save"></a><span data-ttu-id="56d21-102">Temel kaydetme</span><span class="sxs-lookup"><span data-stu-id="56d21-102">Basic Save</span></span>

<span data-ttu-id="56d21-103">Eklemek, değiştirmek ve bağlam ve varlık sınıfları kullanarak verileri kaldırma hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="56d21-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="56d21-104">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="56d21-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="56d21-105">Veri ekleme</span><span class="sxs-lookup"><span data-stu-id="56d21-105">Adding Data</span></span>

<span data-ttu-id="56d21-106">Kullanım *DbSet.Add* yeni örnekleri, varlık sınıfları eklemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="56d21-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="56d21-107">Çağırdığınızda veriler veritabanında eklenir *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="56d21-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="56d21-108">Varlıkların tam grafikte tüm iş Ekle, ekleme ve güncelleştirme yöntemlerini geçirilen, açıklandığı [ilgili verileri](related-data.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="56d21-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="56d21-109">Alternatif olarak, EntityEntry.State özelliği yalnızca tek bir varlık durumunu ayarlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="56d21-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="56d21-110">Örneğin, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="56d21-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="56d21-111">Verileri güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="56d21-111">Updating Data</span></span>

<span data-ttu-id="56d21-112">EF bağlam tarafından izlenen var olan bir varlığa yapılan değişiklikleri otomatik olarak algılar.</span><span class="sxs-lookup"><span data-stu-id="56d21-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="56d21-113">Bu, yük/sorgu veritabanından varlıklar ve daha önce eklediğiniz ve veritabanına kaydedilen varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="56d21-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="56d21-114">Yalnızca özelliklerine atanan değerleri değiştirin ve sonra çağrı *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="56d21-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="56d21-115">Verileri silme</span><span class="sxs-lookup"><span data-stu-id="56d21-115">Deleting Data</span></span>

<span data-ttu-id="56d21-116">Kullanım *DbSet.Remove* , varlık sınıflarının örneklerini silmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="56d21-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="56d21-117">Varlık veritabanında zaten varsa sırasında silinecek *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="56d21-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="56d21-118">Varlık henüz veritabanına kaydedilmedi varsa (diğer bir deyişle, eklenen izlenen) bağlamdan kaldırılacak ve artık ne zaman eklenen *SaveChanges* çağrılır.</span><span class="sxs-lookup"><span data-stu-id="56d21-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="56d21-119">Tek bir SaveChanges içinde birden çok işlem</span><span class="sxs-lookup"><span data-stu-id="56d21-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="56d21-120">Tek bir çağrı ile birden fazla ekleme/güncelleştirme/kaldırma işlemi birleştirebilirsiniz *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="56d21-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="56d21-121">Çoğu veritabanı sağlayıcısı için *SaveChanges* işlem.</span><span class="sxs-lookup"><span data-stu-id="56d21-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="56d21-122">Başka bir deyişle, tüm işlemleri başarılı başarısız veya ve işlemleri hiçbir zaman sola kısmen uygulanır.</span><span class="sxs-lookup"><span data-stu-id="56d21-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
