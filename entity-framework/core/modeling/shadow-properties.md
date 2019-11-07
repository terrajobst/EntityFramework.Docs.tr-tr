---
title: Gölge özellikleri-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: ab57358dd247e32c4ca0f57d07b4cb98f2b85d29
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655961"
---
# <a name="shadow-properties"></a>Gölge Özellikler

Gölge özellikleri, .NET varlık sınıfınızda tanımlanmayan ancak EF Core modelinde bu varlık türü için tanımlanan özelliklerdir. Bu özelliklerin değeri ve durumu yalnızca değişiklik Izleyicide korunur.

Veritabanında, eşlenmiş varlık türlerinde gösterilmemesi gereken veriler olduğunda, gölge özellikleri faydalıdır. Bunlar, iki varlık arasındaki ilişkinin veritabanındaki bir yabancı anahtar değeri ile temsil edildiği, ancak ilişki varlık türleri arasında, varlık türleri arasında yönetildiğinde, yabancı anahtar özellikleri için en sık kullanılır.

Gölge özellik değerleri `ChangeTracker` API aracılığıyla elde edilebilir ve değiştirilebilir.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

`EF.Property` static yöntemi aracılığıyla, LINQ sorgularında gölge özelliklerine başvurulabilir.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Kurallar

Bir ilişki keşfedildiğinde, ancak bağımlı varlık sınıfında yabancı anahtar özelliği bulunamadığında, bu kural tarafından gölge özellikleri oluşturulabilir. Bu durumda, bir gölge yabancı anahtar özelliği tanıtılacaktır. Gölge yabancı anahtar özelliği `<navigation property name><principal key property name>` olarak adlandırılır (birincil varlığa işaret eden, bu, adlandırma için kullanılır). Asıl anahtar özellik adı Gezinti özelliğinin adını içeriyorsa, ad yalnızca `<principal key property name>`olur. Bağımlı varlık üzerinde hiçbir gezinti özelliği yoksa, asıl tür adı bunun yerine kullanılır.

Örneğin, aşağıdaki kod listesi, `Post` varlığa `BlogId` bir gölge özelliği tanıtılmasıyla sonuçlanır.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions)]

## <a name="data-annotations"></a>Veri Açıklamaları

Gölge özellikleri, veri ek açıklamalarıyla oluşturulamaz.

## <a name="fluent-api"></a>Akıcı API

Gölge özelliklerini yapılandırmak için Floent API 'sini kullanabilirsiniz. `Property` dize aşırı yüklemesini çağırdıktan sonra, diğer özellikler için yaptığınız yapılandırma çağrılarının herhangi birini zincirleyebilirsiniz.

`Property` yöntemi için sağlanan ad, var olan bir özelliğin adı (bir gölge özellik veya varlık sınıfında tanımlanmış bir) ile eşleşiyorsa, kod yeni bir gölge özellik tanıtımı yerine mevcut özelliği yapılandırır.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]
