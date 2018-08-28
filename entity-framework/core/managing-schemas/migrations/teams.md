---
title: Ekip ortamlarında - EF Core geçişleri
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: e8ff7f468d5ab6dbd6285f1abf9199e413288d10
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997701"
---
<a name="migrations-in-team-environments"></a>Ekip ortamlarında geçişleri
===============================
Ekip ortamlarında Migrations ile çalışırken, ek model anlık görüntü dosyası dikkat edin. Bu dosya, teammate's geçiş türlerinizle düzgün bir şekilde birleştirir veya yeniden paylaşımı önce geçişinizi oluşturarak çakışmayı gidermeniz gerekiyorsa söyleyebilirsiniz.

<a name="merging"></a>Birleştirme
-------
Kodunuza geçişleri birleştirdiğinizde çakışmaları modeli anlık görüntü dosyanızda alabilirsiniz. Her iki değişiklik ilgisiz ise birleştirme önemsiz ve iki geçiş bulunabilir. Örneğin, şuna benzeyen müşteri varlık türü yapılandırmasındaki bir birleştirme çakışması karşılaşabilirsiniz:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Bu özelliklerin her ikisi de son modelde mevcut olması gerektiğinden, birleştirme, her iki özellik ekleyerek tamamlayın. Çoğu durumda, sürüm denetimi sisteminiz otomatik olarak bu tür değişiklikleri sizin için birleştirme.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

Bu gibi durumlarda, geçiş ve, teammate's geçiş birbirinden bağımsızdır. Bunlardan birini ilk uygulanabilir olduğundan, takımınızla paylaşmayı önce geçişinizi ek değişiklik gerekmez.

<a name="resolving-conflicts"></a>Çakışmaları çözümleme
-------------------
Bazen model anlık görüntü modeli birleştirilirken gerçek bir çakışma ortaya. Örneğin, siz ve, teammate her aynı özelliğe yeniden adlandırmış olabilir.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Bu tür bir çakışma karşılaşırsanız geçişinizi yeniden oluşturarak çözümleyin. Aşağıdaki adımları uygulayın:

1. Birleştirme ve birleştirme önce çalışma dizininize geri alma iptal
2. Geçişinizi Kaldır (ancak modeli değişikliklerinizi saklamak)
3. Çalışma dizininize, teammate's değişiklikleri Birleştir
4. Geçişinizi yeniden ekleyin

Bunu yaptıktan sonra iki geçiş doğru sırayla uygulanabilir. Geçişini, ilk olarak sütunu yeniden adlandırma uygulanır *diğer*, bundan sonra geçiş için yeniden adlandırsa *Username*.

Geçişinizi güvenle takımın geri kalanı ile paylaşılabilir.
