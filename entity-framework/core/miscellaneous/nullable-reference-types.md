---
title: Null yapılabilir başvuru türleriyle çalışma-EF Core
author: roji
ms.date: 09/09/2019
ms.assetid: bde4e0ee-fba3-4813-a849-27049323d301
uid: core/miscellaneous/nullable-reference-types
ms.openlocfilehash: c16a475c363320cd18804a4efe78ccae1ae22f0d
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124359"
---
# <a name="working-with-nullable-reference-types"></a>Null yapılabilir başvuru türleriyle çalışma

C#8 null [yapılabilir başvuru türleri](/dotnet/csharp/tutorials/nullable-reference-types)adlı yeni bir özellik tanıtılan ve başvuru türlerine açıklama eklenip eklenmeyeceğini belirten, bunların null içermesini sağlar. Bu özellik için yeni bir özelliktir, C# belgeleri okuyarak bunu öğrenmeniz önerilir.

Bu sayfa, EF Core null yapılabilir başvuru türleri desteğini tanıtır ve bunlarla çalışmak için en iyi yöntemleri açıklar.

## <a name="required-and-optional-properties"></a>Gerekli ve isteğe bağlı özellikler

Gerekli ve isteğe bağlı özellikler hakkındaki ana belgeler ve bunların null yapılabilir başvuru türleriyle etkileşimi, [gerekli ve Isteğe bağlı özellikler](xref:core/modeling/entity-properties#required-and-optional-properties) sayfasıdır. Önce bu sayfayı okuyarak başlatmanız önerilir.

> [!NOTE]
> Mevcut bir projede null yapılabilir başvuru türlerini etkinleştirirken dikkatli olun: daha önce isteğe bağlı olarak yapılandırılmış olan başvuru türü özellikleri, açıkça null olarak açıklanmadığı sürece artık gerekli olarak yapılandırılır. İlişkisel bir veritabanı şemasını yönetirken, bu, veritabanı sütununun null değer alabilme durumu belirleyen geçişlerin oluşturulmasına neden olabilir.

## <a name="dbcontext-and-dbset"></a>DbContext ve DbSet

Null yapılabilir başvuru türleri etkinleştirildiğinde, C# derleyici null değer içereceklerde hiçbir başlatılmamış null yapılamayan özelliği için uyarı yayar. Sonuç olarak, bir bağlamda null yapılamayan `DbSet` tanımlamanın ortak yöntemi artık bir uyarı oluşturur. Ancak, EF Core her zaman DbContext ile türetilmiş türler üzerinde tüm `DbSet` özelliklerini başlatır, bu nedenle derleyici bunun farkında olmasa bile hiçbir zaman null olmamaları garanti edilir. Bu nedenle, `DbSet` özelliklerinin null yapılamayan olarak tutulması önerilir. bu sayede, null denetimleri olmadan bunlara erişip onları null olarak ayarlayarak derleyici uyarılarını da null olarak ayarlayarak boş olarak ayarlayabilirsiniz (!):

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/NullableReferenceTypesContext.cs?name=Context&highlight=3-4)]

## <a name="non-nullable-properties-and-initialization"></a>Null yapılamayan Özellikler ve başlatma

Başlatılmamış null yapılamayan başvuru türleri için derleyici uyarıları ayrıca varlık türlerinizin normal özellikleri için bir sorundur. Yukarıdaki örnekte, null olamayan özelliklerle sorunsuz bir şekilde çalışacak, bu uyarıların her zaman başlatılmış olduğundan emin olmak için [Oluşturucu bağlamayı](xref:core/modeling/constructors)kullanarak bu uyarıları önliyoruz. Ancak, bazı senaryolarda Oluşturucu bağlamasında bir seçenek olmaz: gezinti özellikleri, örneğin, bu şekilde başlatılamaz.

Gerekli gezinti özellikleri ek bir zorluk sunsun: belirli bir sorumlu için her zaman bağımlı bir değere sahip olsa da, programda o noktadaki gereksinimlere bağlı olarak belirli bir sorgu tarafından yüklenemeyebilir veya yüklenmeyebilir ([bkz. veri yükleme için farklı desenler](xref:core/querying/related-data)). Aynı zamanda, gerekli olsa bile, bu özelliklerin null olup olmadığını denetlemek için tüm erişimleri zorlamadıklarından, bu özellikleri null yapılabilir hale getirmek de mümkün değildir.

Bu senaryolarla başa çıkmak için bir yol, null yapılabilir bir [yedekleme alanı](xref:core/modeling/backing-field)olan null atanamaz bir özelliğe sahiptir:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Order.cs?range=10-17)]

Gezinti özelliği null değer atanamaz olduğundan, gerekli bir gezinti yapılandırılır; gezinti düzgün şekilde yüklendiği sürece, bağımlı olacaktır, özelliği aracılığıyla erişilebilir olur. Ancak, özellikle ilgili varlığı doğru şekilde yüklemeden özelliğe erişildiğinde, API sözleşmesi yanlış kullanıldığı için bir InvalidOperationException atılır. Değer, ayarlanmadığında bile değeri okuyabildiğine bağlı olarak, özelliği değil, her zaman yedekleme alanına erişecek şekilde yapılandırılmalıdır. Bunun nasıl yapılacağı hakkındaki [alanları](xref:core/modeling/backing-field) ve yapılandırmanın doğru olduğundan emin olmak için `PropertyAccessMode.Field` belirtmeyi göz önünde bulundurun.

Bir Terme alternatifi olarak, null-forverme işlecinin (!) yardımı ile özelliği null olarak başlatmak mümkündür:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Order.cs?range=19)]

Bir programlama hatasının sonucu haricinde gerçek bir null değer hiçbir zaman gözlemlenmeyecektir, örn. ilgili varlığı önceden düzgün şekilde yüklemeden gezinti özelliğine erişim.

> [!NOTE]
> Birden çok ilgili varlığa başvurular içeren koleksiyon gezginlerinin her zaman null değer atanamaz olması gerekir. Boş bir koleksiyon, ilgili hiçbir varlık olmadığı anlamına gelir, ancak listenin kendisi hiçbir şekilde null olmamalıdır.

## <a name="navigating-and-including-nullable-relationships"></a>Null yapılabilir ilişkiler dahil ve gezinme

İsteğe bağlı ilişkilerle ilgilenirken, gerçek bir null başvuru özel durumunun imkansız olacağı Derleyici uyarıları ile karşılaşmak mümkündür. LINQ EF Core sorgularınızı çevirirken ve yürütürken, isteğe bağlı bir ilgili varlık yoksa, bunun için herhangi bir gezinti yapmak yerine yalnızca yok sayılacak olur. Ancak, derleyici bu EF Core garantisi hakkında farkında değildir ve LINQ sorgusu bellekte yürütülürse, LINQ to Objects birlikte uyarılar üretir. Sonuç olarak, derleyiciye gerçek bir null değerin mümkün olmadığını bildirmek için null-forverme işlecinin (!) kullanılması gerekir:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Program.cs?range=46)]

İsteğe bağlı gezinmeler arasında birden çok ilişki düzeyi dahil edildiğinde de benzer bir sorun oluşur:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Program.cs?range=36-39&highlight=2)]

Bunu çok fazla fark ederseniz ve söz konusu varlık türleri EF Core sorgularda kullanılan ağırlıklı (veya özel olarak) ise, gezinti özelliklerini null değer atanamaz hale getirmeyi ve bunları akıcı API veya veri ek açıklamaları aracılığıyla isteğe bağlı olarak yapılandırmayı düşünün. Bu, ilişkiyi isteğe bağlı tutarken tüm derleyici uyarılarını kaldırır; Ancak, varlıklarınız EF Core dışına çıkardığında, Özellikler null değer atanamaz olarak açıklansa da null değerler gözlemleyebilirsiniz.

## <a name="limitations"></a>Sınırlamalar

* Ters mühendislik Şu anda [ C# 8 Nullable başvuru türünü (nrts)](/dotnet/csharp/tutorials/nullable-reference-types)desteklememektedir: EF Core her zaman özelliğin C# kapalı olduğunu varsayan kodu üretir. Örneğin, null yapılabilir metin sütunları, bir özelliğin gerekli olup olmadığını yapılandırmak için kullanılan akıcı API veya veri ek açıklamalarıyla `string?`değil, `string` türü olan bir özellik olarak tanılanacaktır. Yapı iskelesi kodunu düzenleyebilir ve bunları C# null yapılabilir ek açıklamalarla değiştirebilirsiniz. Null yapılabilir başvuru türleri için yapı iskelesi desteği [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520)soruna göre izlenir.
* EF Core genel API 'SI yüzeyi, null olabilme için henüz açıklanmamıştır (ortak API "null-yükümlülüğü"), bu, bazen NRT özelliği açık olduğunda daha fazla zaman alabilir. Bu özellikle, [Firstordefaultasync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)gibi EF Core tarafından sunulan zaman uyumsuz LINQ işleçlerini içerir. Bu 5,0 sürümü için bu adresi ele almak için planlıyoruz.
