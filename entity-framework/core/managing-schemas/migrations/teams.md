---
title: Takım ortamlarda - EF çekirdek geçişleri
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
ms.locfileid: "26054726"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="5f2fb-102">Takım ortamlarda geçişleri</span><span class="sxs-lookup"><span data-stu-id="5f2fb-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="5f2fb-103">Takım ortamlarda geçişler ile çalışırken, fazladan modeli anlık görüntü dosyası dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="5f2fb-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="5f2fb-104">Bu dosya paylaşımı önce geçiş işleminizi yeniden oluşturarak bir çakışmayı gerekiyorsa teammate ait geçiş düzgün bir şekilde size birleştirir olmadığını söyleyebilir.</span><span class="sxs-lookup"><span data-stu-id="5f2fb-104">This file can tell you if your teammate's migration merges cleanly with yours of if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="5f2fb-105">Birleştirme</span><span class="sxs-lookup"><span data-stu-id="5f2fb-105">Merging</span></span>
-------
<span data-ttu-id="5f2fb-106">Kodunuza geçişler birleştirdiğinizde, model anlık görüntü dosyanızda çakışmaları alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f2fb-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="5f2fb-107">Her iki değişiklik ilgisiz, birleştirme önemsiz ve iki geçiş bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="5f2fb-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="5f2fb-108">Örneğin, şöyle müşteri varlık türü yapılandırmasında bir birleştirme çakışması alabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5f2fb-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="5f2fb-109">Bu özelliklerin her ikisi de son modelinde mevcut gerektiği, her iki özellik ekleyerek birleştirme işlemini tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="5f2fb-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="5f2fb-110">Çoğu durumda, sürüm denetim sisteminiz otomatik olarak bu değişiklikleri sizin için birleştirme.</span><span class="sxs-lookup"><span data-stu-id="5f2fb-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="5f2fb-111">Bu durumlarda, geçişinizi ve teammate's geçiş birbirinden bağımsızdır.</span><span class="sxs-lookup"><span data-stu-id="5f2fb-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="5f2fb-112">Bunlardan birini ilk uygulanabilir beri ekibinizle paylaşımı önce geçiş işleminizi ek değişiklikler yapmak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5f2fb-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="5f2fb-113">Çakışmalarını çözme</span><span class="sxs-lookup"><span data-stu-id="5f2fb-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="5f2fb-114">Bazen, doğru bir çakışma modeli anlık görüntü modeli birleştirilirken karşılaşırsınız.</span><span class="sxs-lookup"><span data-stu-id="5f2fb-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="5f2fb-115">Örneğin, siz ve, teammate her aynı özelliğe adlandırdınız.</span><span class="sxs-lookup"><span data-stu-id="5f2fb-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="5f2fb-116">Bu tür bir çakışma karşılaşırsanız, geçişinizi yeniden oluşturarak çözün.</span><span class="sxs-lookup"><span data-stu-id="5f2fb-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="5f2fb-117">Aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="5f2fb-117">Follow these steps:</span></span>

1. <span data-ttu-id="5f2fb-118">Birleştirme ve geri alma çalışma dizinine birleştirme önce iptal</span><span class="sxs-lookup"><span data-stu-id="5f2fb-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="5f2fb-119">Geçişinizi kaldırın (ancak modeli değişikliklerinizi korumak)</span><span class="sxs-lookup"><span data-stu-id="5f2fb-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="5f2fb-120">Çalışma dizininizi teammate's değişiklikleri birleştirme</span><span class="sxs-lookup"><span data-stu-id="5f2fb-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="5f2fb-121">Geçişinizi yeniden ekleyin</span><span class="sxs-lookup"><span data-stu-id="5f2fb-121">Re-add your migration</span></span>

<span data-ttu-id="5f2fb-122">Bunu yaptıktan sonra iki geçiş doğru sırayla uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="5f2fb-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="5f2fb-123">Kendi geçiş için sütunu yeniden adlandırma ilk olarak, uygulanan *diğer*, bundan sonra geçiş için yeniden adlandırır *kullanıcıadı*.</span><span class="sxs-lookup"><span data-stu-id="5f2fb-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="5f2fb-124">Geçişinizi takımın geri kalanı ile güvenle paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="5f2fb-124">Your migration can safely be shared with the rest of the team.</span></span>
