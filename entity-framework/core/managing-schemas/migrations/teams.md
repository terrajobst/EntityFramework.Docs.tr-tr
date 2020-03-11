---
title: Takım ortamlarında geçişler-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: 6c17c56277821159962884aef72d46c624442e20
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416772"
---
# <a name="migrations-in-team-environments"></a>Takım Ortamlarında Geçişler

Ekip ortamlarında geçişlerle çalışırken, model anlık görüntü dosyasına fazladan dikkat edin. Bu dosya, takımınızın geçiş işleminden sizinkiyle düzgün bir şekilde birleşiyorsa ya da bir çakışmayı paylaşmadan önce yeniden oluşturarak bir çakışmayı çözmeniz gerekiyorsa size bildirir.

## <a name="merging"></a>Birleştirme

Takım mateklarından geçişleri birleştirdiğinizde, model anlık görüntü dosyanızda çakışmalar alabilirsiniz. Her iki değişiklik de ilgisiz ise birleştirme basit olur ve iki geçiş birlikte kullanılabilir. Örneğin, müşteri varlık türü yapılandırmasında şuna benzer bir birleştirme çakışması alabilirsiniz:

``` output
<<<<<<< Mine
b.Property<bool>("Deactivated");
=======
b.Property<int>("LoyaltyPoints");
>>>>>>> Theirs
```

Bu özelliklerin her ikisinin de son modelde bulunması gerektiğinden, her iki özelliği de ekleyerek birleştirmeyi doldurun. Çoğu durumda, sürüm denetimi sisteminiz sizin için bu değişiklikleri otomatik olarak birleştirebiliriz.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

Bu gibi durumlarda geçişiniz ve ekip Mate geçişiniz birbirinden bağımsızdır. Bunlardan biri önce uygulanamadığından, ekibinizle paylaşmadan önce geçişinize ek değişiklik yapmanız gerekmez.

## <a name="resolving-conflicts"></a>Çakışmaları çözme

Bazen model anlık görüntü modeli birleştirilirken doğru bir çakışma ile karşılaşırsınız. Örneğin, sizin ve takımlarınızın her biri aynı özelliği yeniden adlandırmış olabilir.

``` output
<<<<<<< Mine
b.Property<string>("Username");
=======
b.Property<string>("Alias");
>>>>>>> Theirs
```

Bu tür çakışmayla karşılaşırsanız, geçişinizi yeniden oluşturarak çözümleyin. Şu adımları uygulayın:

1. Birleştirme işleminden önce birleştirme ve çalışma dizininize geri alma işlemini iptal edin
2. Geçişinizi kaldırma (ancak model değişiklerinizi koru)
3. Ekipte yapılan değişiklikleri çalışma dizininiz ile birleştirin
4. Geçişinizi yeniden ekleyin

Bunu yaptıktan sonra, iki geçiş doğru sırada uygulanabilir. Önce geçişi uygulanır, sütunu *diğer ad*olarak yeniden adlandırır, bundan sonra geçişiniz bunu *Kullanıcı adı*olarak değiştirir.

Geçişiniz, takımın geri kalanı ile güvenli bir şekilde paylaşılabilir.
