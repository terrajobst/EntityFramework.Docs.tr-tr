---
title: Model oluşturma ve yapılandırma - EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416417"
---
# <a name="creating-and-configuring-a-model"></a>Model oluşturma ve yapılandırma

Varlık Çerçevesi, varlık sınıflarınızın şekline dayalı bir model oluşturmak için bir dizi kural kullanır. Sözleşme tarafından keşfedilenleri tamamlamak ve/veya geçersiz kılmak için ek yapılandırma belirtebilirsiniz.

Bu makalede, herhangi bir veri deposu hedefleyen bir modele uygulanabilecek ve herhangi bir ilişkisel veritabanıhedeflediğinde uygulanabilecek yapılandırma yı kapsar. Sağlayıcılar, belirli bir veri deposuna özgü yapılandırmayı da etkinleştirebilir. Sağlayıcıya özgü yapılandırmayla ilgili belgeler için [Veritabanı Sağlayıcıları](../providers/index.md) bölümüne bakın.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) GitHub'da görüntüleyebilirsiniz.

## <a name="use-fluent-api-to-configure-a-model"></a>Bir modeli yapılandırmak için akıcı API kullanın

Türemiş `OnModelCreating` bağlamınızdaki yöntemi geçersiz kılabilir ve modelinizi yapılandırmak için yöntemk `ModelBuilder API` kullanabilirsiniz. Bu yapılandırmanın en güçlü yöntemidir ve varlık sınıflarınızı değiştirmeden yapılandırmanın belirtilmesine olanak tanır. Akıcı API yapılandırması en yüksek önceliğe sahiptir ve kuralları ve veri ek açıklamalarını geçersiz kılar.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a>Bir modeli yapılandırmak için veri ek açıklamalarını kullanma

Sınıflarınıza ve özelliklerinize öznitelikleri (Veri Ek Açıklamaları olarak da bilinir) de uygulayabilirsiniz. Veri ek açıklamaları kuralları geçersiz kılar, ancak Akıcı API yapılandırması tarafından geçersiz kılınacaktır.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
