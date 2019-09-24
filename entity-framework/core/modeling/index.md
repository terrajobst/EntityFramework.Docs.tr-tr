---
title: Model oluşturma ve yapılandırma-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 5b886226b16b5b1a1f01e6040e58d92ae8678d29
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197313"
---
# <a name="creating-and-configuring-a-model"></a>Model oluşturma ve yapılandırma

Entity Framework, varlık sınıflarınızın şekline göre bir model oluşturmak için bir kural kümesi kullanır. Kurala göre keşfedildiğini ve/veya geçersiz kılmak için ek yapılandırma belirtebilirsiniz.

Bu makale, herhangi bir veri deposunu hedefleyen ve herhangi bir ilişkisel veritabanını hedeflerken uygulanabilen bir modele uygulanabilen yapılandırmayı ele alır. Sağlayıcılar, belirli bir veri deposuna özgü yapılandırmayı da etkinleştirebilir. Sağlayıcıya özgü yapılandırmayla ilgili belgeler için, [veritabanı sağlayıcıları](../providers/index.md) bölümüne bakın.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) GitHub ' da görebilirsiniz.

## <a name="use-fluent-api-to-configure-a-model"></a>Model yapılandırmak için Fluent API kullanma

Türetilmiş bağlamdaki yöntemi `OnModelCreating`geçersiz kılabilir ve modelinizi yapılandırmakiçinkullanabilirsiniz. `ModelBuilder API` Bu, yapılandırmanın en güçlü yöntemidir ve varlık sınıflarınızda değişiklik yapılmadan yapılandırmanın belirtilmesini sağlar. Akıcı API yapılandırması en yüksek önceliğe sahiptir ve kuralları ve veri ek açıklamalarını geçersiz kılar.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a>Model yapılandırmak için veri ek açıklamalarını kullanma

Ayrıca, sınıflarınıza ve özelliklerine öznitelikler (veri ek açıklamaları olarak bilinir) uygulayabilirsiniz. Veri ek açıklamaları kuralları geçersiz kılar, ancak akıcı API yapılandırması tarafından geçersiz kılınır.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]
