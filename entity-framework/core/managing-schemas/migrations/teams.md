---
title: "Takım ortamlarda - EF çekirdek geçişleri"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
<a name="migrations-in-team-environments"></a>Takım ortamlarda geçişleri
===============================
Takım ortamlarda geçişler ile çalışırken, fazladan modeli anlık görüntü dosyası dikkat edin. Bu dosya paylaşımı önce geçiş işleminizi yeniden oluşturarak bir çakışmayı gerekiyorsa teammate ait geçiş düzgün bir şekilde size birleştirir olmadığını söyleyebilir.

<a name="merging"></a>Birleştirme
-------
Kodunuza geçişler birleştirdiğinizde, model anlık görüntü dosyanızda çakışmaları alabilirsiniz. Her iki değişiklik ilgisiz, birleştirme önemsiz ve iki geçiş bulunabilir. Örneğin, şöyle müşteri varlık türü yapılandırmasında bir birleştirme çakışması alabilirsiniz:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Bu özelliklerin her ikisi de son modelinde mevcut gerektiği, her iki özellik ekleyerek birleştirme işlemini tamamlayın. Çoğu durumda, sürüm denetim sisteminiz otomatik olarak bu değişiklikleri sizin için birleştirme.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

Bu durumlarda, geçişinizi ve teammate's geçiş birbirinden bağımsızdır. Bunlardan birini ilk uygulanabilir beri ekibinizle paylaşımı önce geçiş işleminizi ek değişiklikler yapmak gerekmez.

<a name="resolving-conflicts"></a>Çakışmalarını çözme
-------------------
Bazen, doğru bir çakışma modeli anlık görüntü modeli birleştirilirken karşılaşırsınız. Örneğin, siz ve, teammate her aynı özelliğe adlandırdınız.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Bu tür bir çakışma karşılaşırsanız, geçişinizi yeniden oluşturarak çözün. Aşağıdaki adımları izleyin:

1. Birleştirme ve geri alma çalışma dizinine birleştirme önce iptal
2. Geçişinizi kaldırın (ancak modeli değişikliklerinizi korumak)
3. Çalışma dizininizi teammate's değişiklikleri birleştirme
4. Geçişinizi yeniden ekleyin

Bunu yaptıktan sonra iki geçiş doğru sırayla uygulanabilir. Kendi geçiş için sütunu yeniden adlandırma ilk olarak, uygulanan *diğer*, bundan sonra geçiş için yeniden adlandırır *kullanıcıadı*.

Geçişinizi takımın geri kalanı ile güvenle paylaşılabilir.
