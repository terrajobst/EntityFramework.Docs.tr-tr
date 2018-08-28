---
title: Dizinleri - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993223"
---
# <a name="indexes"></a>Dizinleri

> [!NOTE]  
> Bu bölümdeki yapılandırma, genel olarak ilişkisel veritabanları için geçerlidir. İlişkisel veritabanı sağlayıcısı yüklediğinizde, burada gösterilen genişletme yöntemleri kullanılabilir hale gelir (paylaşılan nedeniyle *Microsoft.EntityFrameworkCore.Relational* paketi).

İlişkisel bir veritabanındaki bir dizin aynı dizin Entity Framework'ün temel kavram eşlenir.

## <a name="conventions"></a>Kurallar

Kural gereği, dizinleri adlandırılır `IX_<type name>_<property name>`. Bileşik dizinler için `<property name>` özellik adlarının bir alt çizgi ayrılmış listesi olur.

## <a name="data-annotations"></a>Veri ek açıklamaları

Veri ek açıklamalarını kullanma dizinleri yapılandırılamaz.

## <a name="fluent-api"></a>Fluent API'si

Fluent API'si, bir dizinin adını yapılandırmak için kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Ayrıca, bir filtre belirtebilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Bir 'Is NOT NULL' ve SQL Server sağlayıcıyı EF eklediğinde benzersiz bir dizinin parçası olan tüm null yapılabilir sütunlar için filtreleyin. Bu kuralı belirtebilirsiniz geçersiz kılmak için bir `null` değeri.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
