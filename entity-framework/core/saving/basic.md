---
title: Temel Kaydet - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 066d67d6104316832a33f5a3648f1f2fa6cc9c50
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417637"
---
# <a name="basic-save"></a><span data-ttu-id="d9a0c-102">Temel Kaydetme</span><span class="sxs-lookup"><span data-stu-id="d9a0c-102">Basic Save</span></span>

<span data-ttu-id="d9a0c-103">Bağlam ve varlık sınıflarınızı kullanarak veri eklemeyi, değiştirmeyi ve kaldırmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="d9a0c-104">Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) GitHub'da görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="d9a0c-105">Veri Ekleme</span><span class="sxs-lookup"><span data-stu-id="d9a0c-105">Adding Data</span></span>

<span data-ttu-id="d9a0c-106">Varlık sınıflarınızın yeni örneklerini eklemek için *DbSet.Add* yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="d9a0c-107">*SaveChanges'ı*aradiğinizde veriler veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="d9a0c-108">Ekle, Ekle ve Güncelleştir yöntemleri, [İlgili Veriler](related-data.md) bölümünde açıklandığı gibi, kendilerine geçen varlıkların tam grafiğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="d9a0c-109">Alternatif olarak, EntityEntry.State özelliği sadece tek bir varlığın durumunu ayarlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="d9a0c-110">Örneğin, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="d9a0c-111">Verileri Güncelleme</span><span class="sxs-lookup"><span data-stu-id="d9a0c-111">Updating Data</span></span>

<span data-ttu-id="d9a0c-112">EF, bağlam tarafından izlenen varolan bir varlıkta yapılan değişiklikleri otomatik olarak algılar.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="d9a0c-113">Bu, veritabanından yüklediğiniz/sorguladığınız varlıkları ve daha önce eklenen ve veritabanına kaydedilmiş varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="d9a0c-114">Yalnızca özelliklere atanan değerleri değiştirin ve ardından *SaveChanges'ı*arayın.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="d9a0c-115">Veri silme</span><span class="sxs-lookup"><span data-stu-id="d9a0c-115">Deleting Data</span></span>

<span data-ttu-id="d9a0c-116">Varlık sınıflarınızın örneklerini silmek için *DbSet.Remove* yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="d9a0c-117">Varlık veritabanında zaten varsa, *SaveChanges*sırasında silinir.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="d9a0c-118">Varlık henüz veritabanına kaydedilmemişse (yani eklendikçe izlenir) o zaman bağlamdan kaldırılır ve *SaveChanges* çağrıldığında artık eklenmez.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="d9a0c-119">Tek bir SaveChanges'ta Birden Çok İşlem</span><span class="sxs-lookup"><span data-stu-id="d9a0c-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="d9a0c-120">Birden çok Ekle/Güncelleştir/Kaldır işlemlerini *SaveChanges*için tek bir çağrıda birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="d9a0c-121">Çoğu veritabanı sağlayıcısı için *SaveChanges* işlemseldir.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="d9a0c-122">Bu, tüm işlemlerin başarılı olacağı veya başarısız olacağı ve işlemlerin hiçbir zaman kısmen uygulanamayacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="d9a0c-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
