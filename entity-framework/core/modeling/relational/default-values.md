---
title: Varsayılan değerler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655910"
---
# <a name="default-values"></a>Varsayılan Değerler

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

Bir sütunun varsayılan değeri, yeni bir satır eklenirse, ancak sütun için hiçbir değer belirtilmemişse eklenecek değerdir.

## <a name="conventions"></a>Kurallar

Kurala göre, varsayılan bir değer yapılandırılmaz.

## <a name="data-annotations"></a>Veri Açıklamaları

Veri ek açıklamalarını kullanarak varsayılan bir değer ayarlayamazsınız.

## <a name="fluent-api"></a>Akıcı API

Bir özellik için varsayılan değeri belirtmek üzere Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

Varsayılan değeri hesaplamak için kullanılan bir SQL parçası de belirtebilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
