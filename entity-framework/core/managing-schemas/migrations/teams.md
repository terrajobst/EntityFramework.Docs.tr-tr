---
title: Takım ortamlarında geçişler-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: 6c17c56277821159962884aef72d46c624442e20
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655537"
---
# <a name="migrations-in-team-environments"></a><span data-ttu-id="dd0e8-102">Takım Ortamlarında Geçişler</span><span class="sxs-lookup"><span data-stu-id="dd0e8-102">Migrations in Team Environments</span></span>

<span data-ttu-id="dd0e8-103">Ekip ortamlarında geçişlerle çalışırken, model anlık görüntü dosyasına fazladan dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="dd0e8-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="dd0e8-104">Bu dosya, takımınızın geçiş işleminden sizinkiyle düzgün bir şekilde birleşiyorsa ya da bir çakışmayı paylaşmadan önce yeniden oluşturarak bir çakışmayı çözmeniz gerekiyorsa size bildirir.</span><span class="sxs-lookup"><span data-stu-id="dd0e8-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

## <a name="merging"></a><span data-ttu-id="dd0e8-105">Birleştirme</span><span class="sxs-lookup"><span data-stu-id="dd0e8-105">Merging</span></span>

<span data-ttu-id="dd0e8-106">Takım mateklarından geçişleri birleştirdiğinizde, model anlık görüntü dosyanızda çakışmalar alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dd0e8-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="dd0e8-107">Her iki değişiklik de ilgisiz ise birleştirme basit olur ve iki geçiş birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dd0e8-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="dd0e8-108">Örneğin, müşteri varlık türü yapılandırmasında şuna benzer bir birleştirme çakışması alabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="dd0e8-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

``` output
<<<<<<< Mine
b.Property<bool>("Deactivated");
=======
b.Property<int>("LoyaltyPoints");
>>>>>>> Theirs
```

<span data-ttu-id="dd0e8-109">Bu özelliklerin her ikisinin de son modelde bulunması gerektiğinden, her iki özelliği de ekleyerek birleştirmeyi doldurun.</span><span class="sxs-lookup"><span data-stu-id="dd0e8-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="dd0e8-110">Çoğu durumda, sürüm denetimi sisteminiz sizin için bu değişiklikleri otomatik olarak birleştirebiliriz.</span><span class="sxs-lookup"><span data-stu-id="dd0e8-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="dd0e8-111">Bu gibi durumlarda geçişiniz ve ekip Mate geçişiniz birbirinden bağımsızdır.</span><span class="sxs-lookup"><span data-stu-id="dd0e8-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="dd0e8-112">Bunlardan biri önce uygulanamadığından, ekibinizle paylaşmadan önce geçişinize ek değişiklik yapmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="dd0e8-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

## <a name="resolving-conflicts"></a><span data-ttu-id="dd0e8-113">Çakışmaları çözme</span><span class="sxs-lookup"><span data-stu-id="dd0e8-113">Resolving conflicts</span></span>

<span data-ttu-id="dd0e8-114">Bazen model anlık görüntü modeli birleştirilirken doğru bir çakışma ile karşılaşırsınız.</span><span class="sxs-lookup"><span data-stu-id="dd0e8-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="dd0e8-115">Örneğin, sizin ve takımlarınızın her biri aynı özelliği yeniden adlandırmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="dd0e8-115">For example, you and your teammate may each have renamed the same property.</span></span>

``` output
<<<<<<< Mine
b.Property<string>("Username");
=======
b.Property<string>("Alias");
>>>>>>> Theirs
```

<span data-ttu-id="dd0e8-116">Bu tür çakışmayla karşılaşırsanız, geçişinizi yeniden oluşturarak çözümleyin.</span><span class="sxs-lookup"><span data-stu-id="dd0e8-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="dd0e8-117">Aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="dd0e8-117">Follow these steps:</span></span>

1. <span data-ttu-id="dd0e8-118">Birleştirme işleminden önce birleştirme ve çalışma dizininize geri alma işlemini iptal edin</span><span class="sxs-lookup"><span data-stu-id="dd0e8-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="dd0e8-119">Geçişinizi kaldırma (ancak model değişiklerinizi koru)</span><span class="sxs-lookup"><span data-stu-id="dd0e8-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="dd0e8-120">Ekipte yapılan değişiklikleri çalışma dizininiz ile birleştirin</span><span class="sxs-lookup"><span data-stu-id="dd0e8-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="dd0e8-121">Geçişinizi yeniden ekleyin</span><span class="sxs-lookup"><span data-stu-id="dd0e8-121">Re-add your migration</span></span>

<span data-ttu-id="dd0e8-122">Bunu yaptıktan sonra, iki geçiş doğru sırada uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="dd0e8-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="dd0e8-123">Önce geçişi uygulanır, sütunu *diğer ad*olarak yeniden adlandırır, bundan sonra geçişiniz bunu *Kullanıcı adı*olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="dd0e8-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="dd0e8-124">Geçişiniz, takımın geri kalanı ile güvenli bir şekilde paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="dd0e8-124">Your migration can safely be shared with the rest of the team.</span></span>
