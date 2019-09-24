---
title: Maksimum uzunluk-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197216"
---
# <a name="maximum-length"></a>En Fazla Uzunluk

En büyük uzunluk yapılandırması, belirli bir özellik için kullanılacak uygun veri türü hakkında veri deposuna bir ipucu sağlar. Maksimum uzunluk yalnızca, `string` ve `byte[]`gibi dizi veri türleri için geçerlidir.

> [!NOTE]  
> Entity Framework, verileri sağlayıcıya geçirmeden önce en fazla uzunluk doğrulaması yapmaz. Uygun olup olmadığını doğrulamak için sağlayıcıya veya veri deposuna kadar olur. Örneğin, SQL Server hedeflenirken, en fazla uzunluğu aşarak temel alınan sütunun veri türü fazla verilerin depolanmasına izin vermez.

## <a name="conventions"></a>Kurallar

Kurala göre, özellikler için uygun bir veri türü seçmek üzere veritabanı sağlayıcısına ayrıldınız. Uzunluğu olan özellikler için, veritabanı sağlayıcısı genellikle en uzun veri uzunluğuna izin veren bir veri türü seçer. Örneğin, Microsoft SQL Server `nvarchar(max)` özellikler için `string` (veya `nvarchar(450)` sütun anahtar olarak kullanılıyorsa) kullanılır.

## <a name="data-annotations"></a>Veri Açıklamaları

Bir özellik için bir maksimum uzunluk yapılandırmak üzere veri açıklamalarını kullanabilirsiniz. Bu örnekte, SQL Server hedefleme bu `nvarchar(500)` veri türünün kullanılmasına neden olur.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>Akıcı API

Bir özellik için maksimum uzunluğu yapılandırmak üzere akıcı API 'YI kullanabilirsiniz. Bu örnekte, SQL Server hedefleme bu `nvarchar(500)` veri türünün kullanılmasına neden olur.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
