---
title: İzleme ve İzleme Sorgusu Yok - EF Core
author: smitpatel
ms.date: 10/10/2019
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: a6c71c12f429f1324abe91d1b2cef96312bec051
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417651"
---
# <a name="tracking-vs-no-tracking-queries"></a>İzleme ve İzleme Yok Sorguları

Entity Framework Core, değişiklik izleyicisinde bir varlık örneği hakkında bilgi tutacaksa davranış denetimlerini izleme. Bir varlık izlenirse, varlıkta algılanan tüm değişiklikler `SaveChanges()`veritabanında kalıcı olarak süre. EF Core ayrıca, izleme sorgusu sonucundaki varlıklar la değişiklik izleyicisi içinde bulunan varlıklar arasındaki gezinti özelliklerini de düzeltecektir.

> [!NOTE]
> [Anahtarsız varlık türleri](xref:core/modeling/keyless-entity-types) hiçbir zaman izlenmez. Bu makalede varlık türleri nerede belirtilirse belirtilsin, anahtar tanımlanmış varlık türlerini ifade eder.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub'da görüntüleyebilirsiniz.

## <a name="tracking-queries"></a>Sorguları izleme

Varsayılan olarak, varlık türlerini döndüren sorgular izlenir. Bu da, bu varlık örneklerinde değişiklik yapabileceğiniz ve `SaveChanges()`bu değişiklikleri kalıcı hale getirebileceğiniz anlamına gelir. Aşağıdaki örnekte, bloglar derecelendirmesinde yapılan değişiklik algılanır ve veritabanına devam eder. `SaveChanges()`

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#Tracking)]

## <a name="no-tracking-queries"></a>Sorguları izlememe

Sonuçlar salt okunur senaryoda kullanıldığında hiçbir izleme sorgusu yararlıdır. Değişiklik izleme bilgilerini ayarlamaya gerek olmadığından daha hızlı yürütülürler. Veritabanından alınan varlıkları güncelleştirmeniz gerekmiyorsa, izleme sorgusu kullanılmamalıdır. Tek bir sorguyu izlememek için değiştirebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#NoTracking)]

Bağlam örneği düzeyinde varsayılan izleme davranışını da değiştirebilirsiniz:

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ContextDefaultTrackingBehavior)]

## <a name="identity-resolution"></a>Kimlik çözünürlüğü

Bir izleme sorgusu değişiklik izleyicisini kullandığından, EF Core bir izleme sorgusunda kimlik çözümlemesi yapar. Bir varlığı somutlaştırırken, EF Core zaten izleniyorsa aynı varlık örneğini değişim izleyicisinden döndürecektir. Sonuç aynı varlığı birden çok kez içeriyorsa, her oluşum için aynı örneği geri alırsınız. İzleme olmayan sorgular değişiklik izleyicisini kullanmaz ve kimlik çözümlemesi yapmaz. Böylece, aynı varlık sonucu birden çok kez içerse bile yeni varlık örneğini geri alırsınız. Bu davranış, EF Core 3.0'dan önceki sürümlerde farklıydı, [önceki sürümler](#previous-versions)bakın.

## <a name="tracking-and-custom-projections"></a>İzleme ve özel projeksiyonlar

Sorgunun sonuç türü bir varlık türü olmasa bile, EF Core varsayılan olarak sonuçta bulunan varlık türlerini izlemeye devam eder. Anonim bir türü döndüren aşağıdaki `Blog` sorguda, sonuç kümesindeki örnekler izlenir.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection1)]

Sonuç kümesi LINQ kompozisyonundan çıkan varlık türlerini içeriyorsa, EF Core bunları izler.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

Sonuç kümesi herhangi bir varlık türü içermiyorsa, izleme yapılmaz. Aşağıdaki sorguda, varlıktan bazı değerlerle anonim bir tür döndürür (ancak gerçek varlık türünün örnekleri yoktur). Sorgudan gelen izlenen varlıklar yok.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection3)]

 EF Core, üst düzey projeksiyonda müşteri değerlendirmesini desteklemektedir. EF Core, istemci değerlendirmesi için bir varlık örneğini somutlaştırırsa, izlenir. Burada, varlıkları istemci `blog` yöntemine `StandardizeURL`aktardığımız için, EF Core blog örneklerini de izleyecektir.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientMethod)]

EF Core, sonuçta yer alan anahtarsız varlık örneklerini izlemez. Ancak EF Core, yukarıdaki kurallara göre anahtarla varlık türlerinin diğer tüm örneklerini izler.

Yukarıdaki kurallardan bazıları EF Core 3.0'dan önce farklı şekilde çalıştı. Daha fazla bilgi için [önceki sürümler'e](#previous-versions)bakın.

## <a name="previous-versions"></a>Önceki sürümler

Sürüm 3.0'dan önce, EF Core izlemenin nasıl yapıldığı konusunda bazı farklılıklar vardı. Önemli farklılıklar şunlardır:

- [İstemci vs Sunucu Değerlendirmesi](xref:core/querying/client-eval) sayfasında açıklandığı gibi, EF Core sürüm 3.0'dan önce sorgunun herhangi bir bölümünde istemci değerlendirmesini desteklemişti. İstemci değerlendirmesi, sonucun bir parçası olmayan varlıkların somutlaşmasına neden oldu. Böylece EF Core ne izlenir bulmak için sonucu analiz etti. Bu tasarım aşağıdaki gibi bazı farklılıklar vardı:
  - Projeksiyondaki müşteri değerlendirmesi, maddeleşmeye neden oldu ancak maddeleşmiş varlık örneğini döndürmedi izlenmedi. Aşağıdaki örnek varlıkları izlemedi. `blog`
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

  - EF Core bazı durumlarda LINQ bileşiminden çıkan nesneleri izlemedi. Aşağıdaki örnek izlemedi. `Post`
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

- Sorgu sonuçları anahtarsız varlık türleri içerse, tüm sorgu izleme denmeye niçin yapılırdı. Bu, sonuç olarak anahtarlı varlık türlerinin de izlenmediği anlamına gelir.
- EF Core, izleme sorgusunda kimlik çözümlemesi yaptı. Zaten döndürülmüş olan varlıkları izlemek için zayıf başvurular kullanılır. Bu nedenle, bir sonuç kümesi aynı varlığın katları kez içeriyorsa, her oluşum için aynı örneği alırsınız. Aynı kimliğe sahip önceki bir sonuç kapsam dışına çıkmış ve çöp toplanmış olsa da, EF Core yeni bir örnek döndürdü.
