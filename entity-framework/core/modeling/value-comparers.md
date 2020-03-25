---
title: Değer Karşılaştırıcılar-EF Core
description: EF Core özellik değerlerini nasıl karşılaştırdığı denetlemek için değer Karşılaştırıcılar kullanma
author: ajcvickers
ms.date: 03/20/2020
uid: core/modeling/value-comparers
ms.openlocfilehash: 9dfed7b7ef8163f4f5c94a0c81c510807c53c13d
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80148273"
---
# <a name="value-comparers"></a>Değer Karşılaştırıcılar

> [!NOTE]  
> Bu özellik EF Core 3,0 ' de yenidir.

> [!TIP]  
> Bu belgedeki kod, GitHub 'da bir [runbir örnek](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/)olarak bulunabilir.

## <a name="background"></a>Arka plan

EF Core, şu durumlarda özellik değerlerini karşılaştırması gerekir:

* [Güncelleştirme değişikliklerinin algılanmasının](xref:core/saving/basic) parçası olarak bir özelliğin değiştirilip değiştirilmediğini belirleme
* İlişkiler çözümlenirken iki anahtar değerin aynı olup olmadığını belirleme 

Bu, int, bool, DateTime, vb. gibi yaygın temel türler için otomatik olarak işlenir.

Daha karmaşık türler için, karşılaştırmanın nasıl yapılacağı gibi seçimler yapılmalıdır.
Örneğin, bir bayt dizisi karşılaştırılabilir:

* Başvuruya göre yalnızca yeni bir bayt dizisi kullanılıyorsa farklılık tespit edilir
* Derin karşılaştırmaya göre, dizideki baytları algılaması algılandı

Varsayılan olarak EF Core, anahtar olmayan bayt dizileri için bu yaklaşımların ilkini kullanır.
Diğer bir deyişle, yalnızca başvurular karşılaştırılır ve bir değişiklik yalnızca mevcut bir bayt dizisi yenisiyle değiştirildiğinde algılanır.
Bu, SaveChanges 'ı yürütürken birçok büyük bayt dizilerinin derin karşılaştırmayı önleyen kolay bir karardır.
Ancak, farklı bir görüntüye sahip bir resim olan bir görüntü iyi bir şekilde işlenir.

Öte yandan, ikili anahtarları temsil etmek için bayt dizileri kullanıldığında başvuru eşitliği çalışmaz.
Bir FK özelliğinin, karşılaştırılması gereken bir PK özelliği ile _aynı örneğe_ ayarlanmış olması çok düşüktür.
Bu nedenle EF Core, anahtar olarak davranan bayt dizileri için derin karşılaştırmalar kullanır.
İkili anahtarlar genellikle kısaysa bu yana büyük bir performans isabetinin olması çok düşüktür.

### <a name="snapshots"></a>Anlık Görüntüler

Kesilebilir türlerde derin karşılaştırmalar, EF Core Özellik değeri için derin bir "Snapshot" oluşturma olanağına ihtiyaç duymasıdır.
Bunun yerine başvuruyu kopyalamak, _aynı nesne_olduklarından hem geçerli değerin hem de anlık görüntünün değiştirilmesine neden olur.
Bu nedenle, değişebilir türlerde derin karşılaştırmalar kullanıldığında derin anlık görüntüyle de gereklidir.

## <a name="properties-with-value-converters"></a>Değer Dönüştürücülerine sahip özellikler

Yukarıdaki durumda EF Core, bayt dizileri için yerel eşleme desteğine sahiptir ve bu nedenle uygun Varsayılanları otomatik olarak seçebilirler.
Ancak, özellik bir [değer Dönüştürücüsü](xref:core/modeling/value-conversions)aracılığıyla eşlenmişse EF Core her zaman kullanılacak uygun karşılaştırmayı belirleyemez.
Bunun yerine EF Core her zaman özelliğin türü tarafından tanımlanan varsayılan eşitlik karşılaştırmasını kullanır.
Bu genellikle doğrudur, ancak daha karmaşık türler eşlerken geçersiz kılınmalıdır.

### <a name="simple-immutable-classes"></a>Basit sabit sınıflar

Basit, sabit bir sınıfı eşlemek için bir değer Dönüştürücüsü kullanan bir özelliği düşünün.

[!code-csharp[SimpleImmutableClass](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=SimpleImmutableClass)]

[!code-csharp[ConfigureImmutableClassProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=ConfigureImmutableClassProperty)]

Bu türün özelliklerine özel karşılaştırmalar veya anlık görüntü gerekmez, çünkü:
* Farklı örneklerin doğru şekilde karşılaştırılabilmesi için eşitlik geçersiz kılındı
* Tür sabittir, bu nedenle bir anlık görüntü değeri değiştirici yoktur

Bu durumda, EF Core varsayılan davranışı olduğu kadar iyidir.

### <a name="simple-immutable-structs"></a>Basit sabit yapılar

Basit yapılar için eşleme de basittir ve özel Karşılaştırıcılar veya anlık görüntüyle gerekli değildir.

[!code-csharp[SimpleImmutableStruct](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=SimpleImmutableStruct)]

[!code-csharp[ConfigureImmutableStructProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=ConfigureImmutableStructProperty)]

EF Core, yapı özelliklerinin derlenmiş, üye tabanlı karşılaştırmaları oluşturmak için yerleşik desteğe sahiptir.
Bu, yapıların EF için eşitlik geçersiz kılınmasına gerek duymadığı anlamına gelir, ancak bunu [başka nedenlerle](/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type)yapmak yine de tercih edebilirsiniz.
Ayrıca, yapılar sabit olduğundan ve her zaman üye tabanlı olarak kopyalandığı için özel anlık görüntüyle gerek yoktur.
(Aynı zamanda değişebilir yapılar için de geçerlidir, ancak [kesilebilir yapıların genel olarak kaçınılması gerekir](/dotnet/csharp/write-safe-efficient-code).)

### <a name="mutable-classes"></a>Kesilebilir sınıflar

Mümkün olduğunda değer dönüştürücülerle değişmez türler (sınıflar veya yapılar) kullanmanız önerilir.
Bu genellikle daha etkilidir ve kesilebilir bir tür kullanmaktan daha temiz anlambilim içerir.

Bununla birlikte, uygulamanın değiştiretiğimiz türlerin özelliklerinin kullanılması yaygındır.
Örneğin, bir sayı listesi içeren bir özelliği eşleme: 

[!code-csharp[ListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ListProperty)]

[`List<T>` sınıfı](/dotnet/api/system.collections.generic.list-1?view=netstandard-2.1):
* Başvuru eşitliği vardır; aynı değerleri içeren iki liste farklı olarak değerlendirilir.
* Değişebilir; Listedeki değerler eklenebilir ve kaldırılabilir.

Liste özelliğindeki tipik bir değer dönüştürme listeyi JSON öğesine ve öğesinden dönüştürebilir:

[!code-csharp[ConfigureListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListProperty)]

Bu daha sonra, bu dönüşümle doğru karşılaştırmaları kullanmak EF Core zorlamak için özelliği `ValueComparer<T>` ayarlamayı gerektirir:

[!code-csharp[ConfigureListPropertyComparer](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListPropertyComparer)]

> [!NOTE]  
> Bir değer karşılaştırıcısı ayarlamak için model Oluşturucu ("floent") API 'SI henüz uygulanmadı.
> Bunun yerine, yukarıdaki kod, oluşturucunun ' Metadata ' olarak kullanıma sunulan alt düzey ımutableproperty üzerinde SetValueComparer ' i çağırır.

`ValueComparer<T>` Oluşturucusu üç ifadeyi kabul eder:
* Kalite denetimi için bir ifade
* Karma kodu oluşturmak için bir ifade
* Bir değerin anlık görüntüsünü almak için bir ifade  

Bu durumda, sayı sıralarının aynı olup olmadığını denetleyerek karşılaştırma yapılır.

Benzer şekilde, karma kodu aynı sırayla oluşturulur.
(Bu, değişebilir değerler üzerinde bir karma kod olduğunu ve bu nedenle [sorunlara neden](https://ericlippert.com/2011/02/28/guidelines-and-rules-for-gethashcode/)olabileceğini unutmayın.
Mümkünse, bunun yerine sabit olun.)

Anlık görüntü, liste ToList ile kopyalanarak oluşturulur.
Bu, yalnızca listelerin değişikliğe girecek olması durumunda gereklidir.
Mümkünse, bunun yerine sabit olun. 

> [!NOTE]  
> Değer dönüştürücüler ve Karşılaştırıcılar basit temsilciler yerine ifadeler kullanılarak oluşturulur.
> Bunun nedeni, EF 'in bu ifadeleri daha sonra temsilci başına bir varlık içine derlenen çok daha karmaşık bir ifade ağacına eklemeleridir.
> Kavramsal olarak, bu derleyici içine benzer.
> Örneğin, basit bir dönüştürme, dönüştürme yapmak için başka bir yönteme yapılan bir çağrı yerine yalnızca atama içinde derlenmiş olabilir.    

### <a name="key-comparers"></a>Anahtar Karşılaştırıcılar

Arka plan bölümü, anahtar karşılaştırmaların neden özel anlambilim gerektirebileceğini ele alır.
Birincil, sorumlu veya yabancı anahtar özelliği üzerinde ayarlama sırasında anahtarlar için uygun bir karşılaştırıcı oluşturmayı unutmayın.

Aynı özellikte farklı semantik olması gereken nadir durumlarda [Setkeyvaluecomparer](/dotnet/api/microsoft.entityframeworkcore.mutablepropertyextensions.setkeyvaluecomparer?view=efcore-3.1) kullanın.

> [!NOTE]  
> SetStructuralComparer EF Core 5,0 ' de kullanımdan kaldırılmıştır.
> Bunun yerine SetKeyValueComparer kullanın.

### <a name="overriding-defaults"></a>Varsayılanları geçersiz kılma

Bazen EF Core tarafından kullanılan varsayılan karşılaştırma uygun olmayabilir.
Örneğin, bayt dizileri, varsayılan olarak EF Core içinde algılanır.
Bu, özellikte farklı bir karşılaştırıcı ayarlanarak geçersiz kılınabilir: 

[!code-csharp[OverrideComparer](../../../samples/core/Modeling/ValueConversions/OverridingByteArrayComparisons.cs?name=OverrideComparer)]

EF Core artık bayt dizilerini karşılaştırabilir ve bu nedenle bayt dizisi mutasyonları tespit edilir.
