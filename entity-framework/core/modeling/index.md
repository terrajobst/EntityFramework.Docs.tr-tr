---
title: Model oluşturma ve yapılandırma-EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416417"
---
# <a name="creating-and-configuring-a-model"></a>Model oluşturma ve yapılandırma

Entity Framework, varlık sınıflarınızın şekline göre bir model oluşturmak için bir kural kümesi kullanır. Kurala göre keşfedildiğini ve/veya geçersiz kılmak için ek yapılandırma belirtebilirsiniz.

Bu makale, herhangi bir veri deposunu hedefleyen ve herhangi bir ilişkisel veritabanını hedeflerken uygulanabilen bir modele uygulanabilen yapılandırmayı ele alır. Sağlayıcılar, belirli bir veri deposuna özgü yapılandırmayı da etkinleştirebilir. Sağlayıcıya özgü yapılandırmayla ilgili belgelerde, [veritabanı sağlayıcıları](../providers/index.md) bölümüne bakın.

> [!TIP]  
> Bu makalenin [örnek](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) GitHub ' da görüntüleyebilirsiniz.

## <a name="use-fluent-api-to-configure-a-model"></a>Model yapılandırmak için Fluent API kullanma

Türetilmiş içerikinizdeki `OnModelCreating` yöntemini geçersiz kılabilir ve modelinizi yapılandırmak için `ModelBuilder API` kullanabilirsiniz. Bu, yapılandırmanın en güçlü yöntemidir ve varlık sınıflarınızda değişiklik yapılmadan yapılandırmanın belirtilmesini sağlar. Akıcı API yapılandırması en yüksek önceliğe sahiptir ve kuralları ve veri ek açıklamalarını geçersiz kılar.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a>Model yapılandırmak için veri ek açıklamalarını kullanma

Ayrıca, sınıflarınıza ve özelliklerine öznitelikler (veri ek açıklamaları olarak bilinir) uygulayabilirsiniz. Veri ek açıklamaları kuralları geçersiz kılar, ancak akıcı API yapılandırması tarafından geçersiz kılınır.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
