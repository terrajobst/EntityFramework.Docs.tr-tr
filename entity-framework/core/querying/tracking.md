---
title: İzleme ve Izleme sorguları karşılaştırması-EF Core
author: smitpatel
ms.date: 10/10/2019
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 66988f936ab75e17620398c8f21e4a32bbc950bd
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445955"
---
# <a name="tracking-vs-no-tracking-queries"></a>İzleme ve Izleme sorguları karşılaştırması

Entity Framework Core değişiklik izleyicide bir varlık örneği hakkındaki bilgileri tutacağı davranış denetimlerini izleme. Bir varlık izleniyorsa, varlıkta algılanan tüm değişiklikler `SaveChanges()` sırasında veritabanında kalıcı hale getirilir. EF Core, bir izleme sorgusu sonucu ve değişiklik izleyicide bulunan varlıklar arasındaki gezinti özelliklerini de düzeltir.

> [!NOTE]
> [Keyless varlık türleri](xref:core/modeling/keyless-entity-types) hiçbir şekilde izlenmez. Bu makalede varlık türleri söz konusu olduğunda, tanımlı bir anahtarı olan varlık türleri anlamına gelir.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub ' da görebilirsiniz.

## <a name="tracking-queries"></a>Sorguları izleme

Varsayılan olarak, varlık türleri döndüren sorgular izliyor. Bu, söz konusu varlık örneklerinde değişiklik yapabileceğiniz ve bu değişiklikleri `SaveChanges()` ile kalıcı hale oluşturabileceğiniz anlamına gelir. Aşağıdaki örnekte, blogların derecelendirmesi değişikliği, `SaveChanges()` sırasında veritabanı üzerinde algılanır ve kalıcı hale getirilir.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#Tracking)]

## <a name="no-tracking-queries"></a>İzleme sorguları yok

Bir salt okuma senaryosunda sonuçlar kullanıldığında hiçbir izleme sorgusu yararlı değildir. Değişiklik izleme bilgilerini ayarlamaya gerek olmadığı için daha hızlı bir şekilde yürütüyoruz. Veritabanından alınan varlıkları güncelleştirmeniz gerekmiyorsa, bir izleme sorgusunun kullanılması gerekir. Tek bir sorguyu bir izleme olmayacak şekilde takas edebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#NoTracking)]

Bağlam örneği düzeyinde varsayılan izleme davranışını da değiştirebilirsiniz:

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ContextDefaultTrackingBehavior)]

## <a name="identity-resolution"></a>Kimlik çözümlemesi

İzleme sorgusu değişiklik izleyicisini kullandığından, EF Core izleme sorgusunda kimlik çözümlemesi yapılır. Bir varlığı oluştururken EF Core, zaten izleniyorsa değişiklik izleyicide aynı varlık örneğini döndürür. Sonuç aynı varlığı birden çok kez içeriyorsa, her oluşum için aynı örneği geri alırsınız. Hiçbir izleme sorgusu değişiklik izleyicisini kullanmaz ve kimlik çözümlemesi yapmayın. Bu nedenle, aynı varlık sonucun birden çok kez dahil edildiğinde bile yeni varlık örneğini geri alırsınız. Bu davranış EF Core 3,0 öncesi sürümlerde farklıydı, [önceki sürümlere](#previous-versions)bakın.

## <a name="tracking-and-custom-projections"></a>İzleme ve özel tahminler

Sorgunun sonuç türü bir varlık türü olmasa bile EF Core, varsayılan olarak sonuçta bulunan varlık türlerini izlemeye devam eder. Bir anonim tür döndüren aşağıdaki sorguda, sonuç kümesinde `Blog` örnekleri izlenir.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection1)]

Sonuç kümesi, LINQ kompozisyonunuzdan gelen varlık türlerini içeriyorsa, EF Core bunları izler.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

Sonuç kümesi herhangi bir varlık türü içermiyorsa, izleme yapılmaz. Aşağıdaki sorguda, varlıktan bazı değerler içeren anonim bir tür döndürüyoruz (ancak gerçek varlık türünün örneklerinden hiçbiri). Sorgudan gelen hiçbir izlenen varlık yok.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection3)]

 EF Core, en üst düzey projeksiyde istemci değerlendirmesi yapmak destekler. EF Core, istemci değerlendirmesi için bir varlık örneği içeriyorsa izlenir. Burada, `StandardizeURL` istemci yöntemine `blog` varlıklar geçirdiğimiz için, EF Core blog örneklerini de izler.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientMethod)]

EF Core, sonuçta yer alan anahtarsız varlık örneklerini izlemez. Ancak EF Core Yukarıdaki kurallara göre anahtar içeren tüm varlık türlerinin diğer örneklerini izler.

Yukarıdaki kurallardan bazıları EF Core 3,0 ' dan önce farklı şekilde çalıştı. Daha fazla bilgi için bkz. [önceki sürümler](#previous-versions).

## <a name="previous-versions"></a>Önceki sürümler

Sürüm 3,0 ' den önce, EF Core izlemenin nasıl yapıldığı konusunda bazı farklılıklar vardı. Önemli farklar şunlardır:

- [İstemci vs Server değerlendirmesi](xref:core/querying/client-eval) sayfasında açıklandığı gibi, 3,0 sürümünden önceki sorgunun herhangi bir bölümünde desteklenen istemci değerlendirmesi EF Core. İstemci değerlendirmesi, sonucun bir parçası olmayan varlıkların bir şekilde oluşturulmasına neden oldu. EF Core, nelerin izleneceğini algılamak için sonucu analiz eder. Bu tasarımda aşağıdaki gibi bazı farklılıklar vardı:
  - Yansıtmada oluşan, ancak gerçekleştirilmiş varlık örneğini döndürmeyen istemci değerlendirmesi izlenmiyor. Aşağıdaki örnek `blog` varlıklarını izlememedi.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

  - EF Core, bazı durumlarda LINQ kompozisyonunun geldiği nesneleri izlememedi. Aşağıdaki örnek `Post` ' i izlememedi.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

- Sorgu sonuçları, anahtarsız varlık türleri içerdiğinde, tüm sorgu izlenmesiz hale getirilir. Diğer bir deyişle, sonuçları bulunan anahtarlar içeren varlık türlerinin, her ikisi de izlenemez.
- EF Core, hiçbir izleme sorgusunda kimlik çözümlemesi gerçekleştirmedi. Daha önceden döndürülen varlıkların izlenmesini sağlamak için zayıf başvurular kullandı. Bu nedenle, bir sonuç kümesi aynı varlığı çarpan bir kez içeriyorsa, her oluşum için aynı örneği elde edersiniz. Aynı kimliğe sahip önceki bir sonuç kapsam dışında gelse ve atık toplanmışsa, EF Core yeni bir örnek döndürdü.
