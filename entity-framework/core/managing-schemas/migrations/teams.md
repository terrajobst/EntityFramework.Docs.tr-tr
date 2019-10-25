---
title: Takım ortamlarında geçişler-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: e6a1b86761a201cbcae34cced7e64f11df37a420
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811986"
---
# <a name="migrations-in-team-environments"></a>Takım Ortamlarında Geçişler

Ekip ortamlarında geçişlerle çalışırken, model anlık görüntü dosyasına fazladan dikkat edin. Bu dosya, takımınızın geçiş işleminden sizinkiyle düzgün bir şekilde birleşiyorsa ya da bir çakışmayı paylaşmadan önce yeniden oluşturarak bir çakışmayı çözmeniz gerekiyorsa size bildirir.

## <a name="merging"></a>Birleştirme

Takım mateklarından geçişleri birleştirdiğinizde, model anlık görüntü dosyanızda çakışmalar alabilirsiniz. Her iki değişiklik de ilgisiz ise birleştirme basit olur ve iki geçiş birlikte kullanılabilir. Örneğin, müşteri varlık türü yapılandırmasında şuna benzer bir birleştirme çakışması alabilirsiniz:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Bu özelliklerin her ikisinin de son modelde bulunması gerektiğinden, her iki özelliği de ekleyerek birleştirmeyi doldurun. Çoğu durumda, sürüm denetimi sisteminiz sizin için bu değişiklikleri otomatik olarak birleştirebiliriz.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

Bu gibi durumlarda geçişiniz ve ekip Mate geçişiniz birbirinden bağımsızdır. Bunlardan biri önce uygulanamadığından, ekibinizle paylaşmadan önce geçişinize ek değişiklik yapmanız gerekmez.

## <a name="resolving-conflicts"></a>Çakışmaları çözme

Bazen model anlık görüntü modeli birleştirilirken doğru bir çakışma ile karşılaşırsınız. Örneğin, sizin ve takımlarınızın her biri aynı özelliği yeniden adlandırmış olabilir.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Bu tür çakışmayla karşılaşırsanız, geçişinizi yeniden oluşturarak çözümleyin. Aşağıdaki adımları uygulayın:

1. Birleştirme işleminden önce birleştirme ve çalışma dizininize geri alma işlemini iptal edin
2. Geçişinizi kaldırma (ancak model değişiklerinizi koru)
3. Ekipte yapılan değişiklikleri çalışma dizininiz ile birleştirin
4. Geçişinizi yeniden ekleyin

Bunu yaptıktan sonra, iki geçiş doğru sırada uygulanabilir. Önce geçişi uygulanır, sütunu *diğer ad*olarak yeniden adlandırır, bundan sonra geçişiniz bunu *Kullanıcı adı*olarak değiştirir.

Geçişiniz, takımın geri kalanı ile güvenli bir şekilde paylaşılabilir.
