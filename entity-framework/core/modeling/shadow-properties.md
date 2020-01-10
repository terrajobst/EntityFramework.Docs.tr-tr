---
title: Gölge özellikleri-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 229cfd83f75b01dff9ac9ad30ee55c7cc727c19e
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781189"
---
# <a name="shadow-properties"></a>Gölge Özellikler

Gölge özellikleri, .NET varlık sınıfınızda tanımlanmayan ancak EF Core modelinde bu varlık türü için tanımlanan özelliklerdir. Bu özelliklerin değeri ve durumu yalnızca değişiklik Izleyicide korunur. Veritabanında, eşlenmiş varlık türlerinde gösterilmemesi gereken veriler olduğunda, gölge özellikleri faydalıdır.

## <a name="foreign-key-shadow-properties"></a>Yabancı anahtar gölge özellikleri

Gölge Özellikler çoğu zaman, iki varlık arasındaki ilişkinin veritabanındaki bir yabancı anahtar değeriyle temsil edildiği yabancı anahtar özellikleri için kullanılır, ancak ilişki varlık türlerinde şirket arasındaki gezinti özellikleri kullanılarak yönetilir türü. Kurala göre, bir ilişki keşfedildiğinde, ancak bağımlı varlık sınıfında yabancı anahtar özelliği bulunamadığında EF bir Shadow özelliği ortaya çıkaracak.

Özellik `<navigation property name><principal key property name>` olarak adlandırılır (birincil varlığa işaret eden, bu, adlandırma için kullanılır). Asıl anahtar özellik adı Gezinti özelliğinin adını içeriyorsa, ad yalnızca `<principal key property name>`olur. Bağımlı varlık üzerinde hiçbir gezinti özelliği yoksa, asıl tür adı bunun yerine kullanılır.

Örneğin, aşağıdaki kod listesi, `Post` varlığa `BlogId` bir gölge özelliği tanıtılmasıyla sonuçlanır:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a>Gölge özelliklerini yapılandırma

Gölge özelliklerini yapılandırmak için Floent API 'sini kullanabilirsiniz. `Property`dize aşırı yüklemesini çağırdıktan sonra, diğer özellikler için yaptığınız yapılandırma çağrılarının herhangi birini zincirleyebilirsiniz. Aşağıdaki örnekte, `Blog` `LastUpdated`adlı CLR özelliği olmadığından, bir Shadow özelliği oluşturulur:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

`Property` yöntemi için sağlanan ad, var olan bir özelliğin adı (bir gölge özellik veya varlık sınıfında tanımlanmış bir) ile eşleşiyorsa, kod yeni bir gölge özellik tanıtımı yerine mevcut özelliği yapılandırır.

## <a name="accessing-shadow-properties"></a>Gölge özelliklerine erişme

Gölge özellik değerleri `ChangeTracker` API aracılığıyla elde edilebilir ve değiştirilebilir:

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

`EF.Property` statik yöntemi aracılığıyla, LINQ sorgularında gölge özelliklerine başvurulabilir:

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```
