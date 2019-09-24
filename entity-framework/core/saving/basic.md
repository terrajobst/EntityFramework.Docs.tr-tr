---
title: Temel kaydetme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 6f72458504a9dbe99038af7cfd23b6991258f6b8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197782"
---
# <a name="basic-save"></a><span data-ttu-id="6c8fe-102">Temel Kaydetme</span><span class="sxs-lookup"><span data-stu-id="6c8fe-102">Basic Save</span></span>

<span data-ttu-id="6c8fe-103">Bağlam ve varlık sınıflarınızı kullanarak verileri ekleme, değiştirme ve kaldırma hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="6c8fe-104">Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="6c8fe-105">Veri ekleme</span><span class="sxs-lookup"><span data-stu-id="6c8fe-105">Adding Data</span></span>

<span data-ttu-id="6c8fe-106">Varlık sınıflarınızın yeni örneklerini eklemek için *Dbset. Add* metodunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="6c8fe-107">*SaveChanges*' i çağırdığınızda veriler veritabanına eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="6c8fe-108">Add, Attach ve Update yöntemleri, [Ilişkili veri](related-data.md) bölümünde açıklandığı gibi, bunlara geçirilen varlıkların tam grafiğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="6c8fe-109">Alternatif olarak, EntityEntry. State özelliği yalnızca tek bir varlığın durumunu ayarlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="6c8fe-110">Örneğin, uygulamasında yönetilen Hyper-V konakları olarak eklemek için aşağıdaki yordamı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="6c8fe-111">Verileri güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="6c8fe-111">Updating Data</span></span>

<span data-ttu-id="6c8fe-112">EF, bağlam tarafından izlenen mevcut bir varlıkta yapılan değişiklikleri otomatik olarak algılar.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="6c8fe-113">Bu, veritabanından yüklediğiniz/sorgulayan varlıkları ve daha önce eklenen ve veritabanına kaydedilen varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="6c8fe-114">Yalnızca özelliklere atanan değerleri değiştirin ve ardından *SaveChanges*öğesini çağırın.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="6c8fe-115">Verileri silme</span><span class="sxs-lookup"><span data-stu-id="6c8fe-115">Deleting Data</span></span>

<span data-ttu-id="6c8fe-116">Varlık sınıflarınızın örneklerini silmek için *Dbset. Remove* metodunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="6c8fe-117">Varlık veritabanında zaten mevcutsa, *SaveChanges*sırasında silinir.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="6c8fe-118">Varlık henüz veritabanına kaydedilmediyse (yani, eklenen olarak izleniyorsa), içerikten kaldırılır ve *SaveChanges* çağrıldığında artık eklenmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="6c8fe-119">Tek bir SaveChanges içindeki birden çok Işlem</span><span class="sxs-lookup"><span data-stu-id="6c8fe-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="6c8fe-120">Birden çok ekleme/güncelleştirme/kaldırma işlemini tek bir *SaveChanges*çağrısıyla birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="6c8fe-121">Çoğu veritabanı sağlayıcısı için *SaveChanges* işlem.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="6c8fe-122">Bu, tüm işlemlerin başarılı veya başarısız olduğu anlamına gelir ve işlemler hiçbir durumda kısmen uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="6c8fe-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
