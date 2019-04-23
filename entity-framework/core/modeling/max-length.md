---
title: En fazla uzunluk - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: 3220518cb0a409b6e802d2f3a98acdb949ffbf56
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929855"
---
# <a name="maximum-length"></a>En fazla uzunluk

En fazla yapılandırma, belirli bir özellik için kullanılmak üzere uygun veri türü hakkında veri deposunda bir ipucu sağlar. En fazla uzunluk yalnızca geçerli dizi veri türleri için gibi `string` ve `byte[]`.

> [!NOTE]  
> Varlık çerçevesi sağlayıcısına verileri geçirmeden önce tüm doğrulama en fazla uzunluğu yapmaz. Buna uygun doğrulamak için sağlayıcısı veya veri deposuna kadar var. Örneğin, en büyük uzunluğu aşan SQL Server'ı hedefleyen bir özel durum temel alınan sütunun veri türü olarak neden olması durumunda depolanacak fazlalık veri izin vermez.

## <a name="conventions"></a>Kurallar

Kural gereği, bunu bir uygun veri türü özellikleri seçmek için veritabanı sağlayıcısı kadar bırakılır. Bir uzunluğa sahip özellikler için veritabanı sağlayıcısı uzun veri uzunluğu için izin veren bir veri türü genellikle seçersiniz. Örneğin, Microsoft SQL Server kullanacak `nvarchar(max)` için `string` özelliklerini (veya `nvarchar(450)` sütunu bir anahtar olarak kullanılıyorsa).

## <a name="data-annotations"></a>Veri ek açıklamaları

Bir özellik için en fazla uzunluk yapılandırmak için veri ek açıklamaları kullanabilirsiniz. Bu örnekte, SQL Server'ın bu içinde sonuçlanır hedefleyen `nvarchar(500)` veri türü.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si, bir özellik için en fazla uzunluk yapılandırmak için kullanabilirsiniz. Bu örnekte, SQL Server'ın bu içinde sonuçlanır hedefleyen `nvarchar(500)` veri türü.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=11-13)]
