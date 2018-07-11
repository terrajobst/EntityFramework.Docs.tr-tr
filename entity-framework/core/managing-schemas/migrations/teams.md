---
title: Ekip ortamlarında - EF Core geçişleri
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cf76df32099c25f33d5d94edf6bccec099cf714a
ms.sourcegitcommit: de491b0988eab91b84dcbd941b7551e597e9c051
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38228386"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="2ed35-102">Ekip ortamlarında geçişleri</span><span class="sxs-lookup"><span data-stu-id="2ed35-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="2ed35-103">Ekip ortamlarında Migrations ile çalışırken, ek model anlık görüntü dosyası dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2ed35-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="2ed35-104">Bu dosya, teammate's geçiş türlerinizle düzgün bir şekilde birleştirir veya yeniden paylaşımı önce geçişinizi oluşturarak çakışmayı gidermeniz gerekiyorsa söyleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2ed35-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="2ed35-105">Birleştirme</span><span class="sxs-lookup"><span data-stu-id="2ed35-105">Merging</span></span>
-------
<span data-ttu-id="2ed35-106">Kodunuza geçişleri birleştirdiğinizde çakışmaları modeli anlık görüntü dosyanızda alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2ed35-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="2ed35-107">Her iki değişiklik ilgisiz ise birleştirme önemsiz ve iki geçiş bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="2ed35-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="2ed35-108">Örneğin, şuna benzeyen müşteri varlık türü yapılandırmasındaki bir birleştirme çakışması karşılaşabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="2ed35-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="2ed35-109">Bu özelliklerin her ikisi de son modelde mevcut olması gerektiğinden, birleştirme, her iki özellik ekleyerek tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="2ed35-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="2ed35-110">Çoğu durumda, sürüm denetimi sisteminiz otomatik olarak bu tür değişiklikleri sizin için birleştirme.</span><span class="sxs-lookup"><span data-stu-id="2ed35-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="2ed35-111">Bu gibi durumlarda, geçiş ve, teammate's geçiş birbirinden bağımsızdır.</span><span class="sxs-lookup"><span data-stu-id="2ed35-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="2ed35-112">Bunlardan birini ilk uygulanabilir olduğundan, takımınızla paylaşmayı önce geçişinizi ek değişiklik gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2ed35-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="2ed35-113">Çakışmaları çözümleme</span><span class="sxs-lookup"><span data-stu-id="2ed35-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="2ed35-114">Bazen model anlık görüntü modeli birleştirilirken gerçek bir çakışma ortaya.</span><span class="sxs-lookup"><span data-stu-id="2ed35-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="2ed35-115">Örneğin, siz ve, teammate her aynı özelliğe yeniden adlandırmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="2ed35-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="2ed35-116">Bu tür bir çakışma karşılaşırsanız geçişinizi yeniden oluşturarak çözümleyin.</span><span class="sxs-lookup"><span data-stu-id="2ed35-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="2ed35-117">Aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="2ed35-117">Follow these steps:</span></span>

1. <span data-ttu-id="2ed35-118">Birleştirme ve birleştirme önce çalışma dizininize geri alma iptal</span><span class="sxs-lookup"><span data-stu-id="2ed35-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="2ed35-119">Geçişinizi Kaldır (ancak modeli değişikliklerinizi saklamak)</span><span class="sxs-lookup"><span data-stu-id="2ed35-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="2ed35-120">Çalışma dizininize, teammate's değişiklikleri Birleştir</span><span class="sxs-lookup"><span data-stu-id="2ed35-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="2ed35-121">Geçişinizi yeniden ekleyin</span><span class="sxs-lookup"><span data-stu-id="2ed35-121">Re-add your migration</span></span>

<span data-ttu-id="2ed35-122">Bunu yaptıktan sonra iki geçiş doğru sırayla uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="2ed35-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="2ed35-123">Geçişini, ilk olarak sütunu yeniden adlandırma uygulanır *diğer*, bundan sonra geçiş için yeniden adlandırsa *Username*.</span><span class="sxs-lookup"><span data-stu-id="2ed35-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="2ed35-124">Geçişinizi güvenle takımın geri kalanı ile paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="2ed35-124">Your migration can safely be shared with the rest of the team.</span></span>
