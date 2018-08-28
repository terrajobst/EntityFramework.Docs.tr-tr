---
title: Temel kaydetme - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 23e0e4611f642d59048fca5a808d0782b22caa1e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994807"
---
# <a name="basic-save"></a><span data-ttu-id="b3793-102">Temel kaydetme</span><span class="sxs-lookup"><span data-stu-id="b3793-102">Basic Save</span></span>

<span data-ttu-id="b3793-103">Eklemek, değiştirmek ve bağlam ve varlık sınıfları kullanarak verileri kaldırma hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="b3793-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="b3793-104">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="b3793-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="b3793-105">Veri ekleme</span><span class="sxs-lookup"><span data-stu-id="b3793-105">Adding Data</span></span>

<span data-ttu-id="b3793-106">Kullanım *DbSet.Add* yeni örnekleri, varlık sınıfları eklemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b3793-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="b3793-107">Çağırdığınızda veriler veritabanında eklenir *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="b3793-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="b3793-108">Varlıkların tam grafikte tüm iş Ekle, ekleme ve güncelleştirme yöntemlerini geçirilen, açıklandığı [ilgili verileri](related-data.md) bölümü.</span><span class="sxs-lookup"><span data-stu-id="b3793-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="b3793-109">Alternatif olarak, EntityEntry.State özelliği yalnızca tek bir varlık durumunu ayarlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3793-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="b3793-110">Örneğin, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="b3793-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="b3793-111">Verileri güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="b3793-111">Updating Data</span></span>

<span data-ttu-id="b3793-112">EF bağlam tarafından izlenen var olan bir varlığa yapılan değişiklikleri otomatik olarak algılar.</span><span class="sxs-lookup"><span data-stu-id="b3793-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="b3793-113">Bu, yük/sorgu veritabanından varlıklar ve daha önce eklediğiniz ve veritabanına kaydedilen varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="b3793-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="b3793-114">Yalnızca özelliklerine atanan değerleri değiştirin ve sonra çağrı *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="b3793-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="b3793-115">Verileri silme</span><span class="sxs-lookup"><span data-stu-id="b3793-115">Deleting Data</span></span>

<span data-ttu-id="b3793-116">Kullanım *DbSet.Remove* , varlık sınıflarının örneklerini silmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b3793-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="b3793-117">Varlık veritabanında zaten varsa sırasında silinecek *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="b3793-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="b3793-118">Varlık henüz veritabanına kaydedilmedi varsa (diğer bir deyişle, eklenen izlenen) bağlamdan kaldırılacak ve artık ne zaman eklenen *SaveChanges* çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b3793-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="b3793-119">Tek bir SaveChanges içinde birden çok işlem</span><span class="sxs-lookup"><span data-stu-id="b3793-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="b3793-120">Tek bir çağrı ile birden fazla ekleme/güncelleştirme/kaldırma işlemi birleştirebilirsiniz *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="b3793-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="b3793-121">Çoğu veritabanı sağlayıcısı için *SaveChanges* işlem.</span><span class="sxs-lookup"><span data-stu-id="b3793-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="b3793-122">Başka bir deyişle, tüm işlemleri başarılı başarısız veya ve işlemleri hiçbir zaman sola kısmen uygulanır.</span><span class="sxs-lookup"><span data-stu-id="b3793-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
