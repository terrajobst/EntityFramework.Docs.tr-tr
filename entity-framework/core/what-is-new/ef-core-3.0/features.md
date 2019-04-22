---
title: EF Core 3.0 - EF Core yenilikleri
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 7501a806271c9734e85e31845f260f2d512da077
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58867963"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a>EF Core 3. 0 ' (şu anda Önizleme aşamasında) dahil olan yeni özellikler

> [!IMPORTANT]
> Özellik kümeleri ve zamanlamaları gelecek sürümlerin her zaman değişikliğe tabi olduğu ve bu sayfada en güncel, en son planlarımızı hiç yansıtmayabilir tutmak deneyeceğiz rağmen zaman lütfen unutmayın.

Aşağıdaki liste, EF Core 3.0 için planlanan önemli yeni özellikler içerir.
Bu özelliklerin çoğu geçerli Önizleme sürümünde yer almaz, ancak RTM ilerlemeyi vermiyoruz olarak kullanılabilir hale gelir.

Biz odaklanarak uygulamaya planlanan yayın başındaki olan nedeni [bozucu değişiklikler](xref:core/what-is-new/ef-core-3.0/breaking-changes).
En son değişikliklerin birçoğu EF core'a kendi geliştirmeleridir.
Diğer birçok ek geliştirmeler engelini kaldırmak için gereklidir. 

Hata düzeltmeleri ve geliştirmeleri tam listesi için devam ettiği, gördüğünüz [bu bizim sorun İzleyicisi sorguda](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).

## <a name="linq-improvements"></a>LINQ geliştirmeleri 

[İzleme sorun #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Bu özellik iş başlatıldı ancak geçerli önizlemede dahil değildir.

LINQ sayesinde, istediğiniz dilde çıkmadan veritabanı sorguları yazma yararlanarak zengin IntelliSense ve derleme zamanı tür denetimi alma bilgilerini yazın.
Ancak LINQ, sınırsız sayıda karmaşık sorgular yazmaya olanak tanır ve her zaman LINQ sağlayıcıları için çok zor olmuştur.
EF Core birkaç ilk sürümlerinde, biz, kısmi çıkış bir sorgu kısımlarını olabilir hesaplayarak Çözüldü SQL ve istemci üzerindeki bellekte yürütülecek sorgu geri kalanı sağlayarak çevrilir.
Bazı durumlarda bu istemci-tarafı yürütme istenebilir ancak diğer birçok durumda, bir uygulama üretime dağıtıldığında kadar belirtilmemiş olabilecek verimsiz sorguları neden olabilir.
EF Core 3.0 sürümünde, çok büyük değişiklikler LINQ kararlılığımızın nasıl çalıştığını ve bunu nasıl test yapmak planlıyoruz.
Hedefleridir (örneğin sorguları düzeltme eki sürümlerde bozmayı önlemek için), daha sağlam hale getirmek için SQL, daha fazla durumda verimli sorgular oluşturun ve verimsiz sorguları algılanmayan gitmesini önlemek için doğru olarak daha fazla ifade çevirme etkinleştirmek için.

## <a name="cosmos-db-support"></a>Cosmos DB desteği 

[İzleme sorun #8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

Bu özellik geçerli Önizleme'de dahil, ancak henüz tamamlanmadı. 

Bir Cosmos DB sağlayıcısı için bir uygulama veritabanının olarak Azure Cosmos DB kolayca hedeflemek geliştiricilerin EF ölçeklenebilirliğinden modeli için EF Core üzerinde çalışıyoruz.
Hedef, Cosmos DB, küresel dağıtım gibi avantajlarından bazıları "her zaman açık" olmasını sağlamaktır kullanılabilirlik, esnek ölçeklenebilirlik ve düşük gecikme süresi, .NET geliştiricileri için daha erişilebilir.
EF Core gibi özelliklerin çoğu, Cosmos DB SQL API karşı değer dönüştürmeleri otomatik değişiklik izleme ve LINQ sağlayıcı etkinleştirir.
EF Core 2.2 önce bu çalışmaların Başladık ve [bazı yaptık Önizleme sürümleri kullanılabilir sağlayıcısının](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
Yeni plan EF Core 3.0 yanı sıra sağlayıcı geliştirmeye devam sağlamaktır. 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Bağımlı varlıkları tablo sorumlusuyla paylaşımı artık isteğe bağlıdır

[İzleme sorun #9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

EF Core 3.0-preview 4 sürümünde bu özellik sunulacaktır.

Şu model göz önünde bulundurun:
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

EF Core 3.0 ile başlatılmasının `OrderDetails` tarafından sahip olunan `Order` veya eklenmesi mümkün olacaktır aynı tabloya açıkça eşleştirilmiş bir `Order` olmadan bir `OrderDetails` ve tüm `OrderDetails` için özellikleri birincil anahtar dışındaki eşleştirilir boş değer atanabilir sütun.
EF Core sorgulama ayarlandığında `OrderDetails` için `null` gerekli özelliklerinden birini yoksa, bir değer veya birincil anahtarı yanı sıra gereken özellikleri yoktur ve tüm özellikleri `null`.

## <a name="c-80-support"></a>C#8.0 desteği

[Sorun #12047 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[sorun #10347 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/10347)

Bu özellik iş başlatıldı ancak geçerli önizlemede dahil değildir.

Bazı yararlanmak için müşterilerimizin istiyoruz [gelen yeni özellikler C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) istediğiniz zaman uyumsuz akışlar (dahil olmak üzere `await foreach`) ve EF Core kullanırken null başvuru türleri.

## <a name="reverse-engineering-of-database-views"></a>Tersine mühendislik veritabanı görünümü

[İzleme sorun #1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

Bu özellik geçerli Önizleme sürümünde yer almıyor.

[Sorgu türü](xref:core/modeling/query-types), EF Core 2.1 içinde tanıtılan ve kabul EF Core 3.0, veritabanından okunan ancak güncelleştirilemiyor temsil veri anahtarları olmadan varlık türleri.
Varlık türleri geriye doğru olduğunda veritabanı görünümleri mühendislik anahtarları olmadan oluşturulmasını otomatik hale getirmek planlıyoruz için bu özellik, bunların veritabanı görünümleri için mükemmel bir uyum Çoğu senaryoda sağlar.

## <a name="property-bag-entities"></a>Özellik paketi varlıklar

[Sorun #13610 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/13610) ve [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)

Bu özellik iş başlatıldı ancak geçerli önizlemede dahil değildir. 

Dizinli Özellikler normal özellikleri yerine verileri depolayan varlıkları etkinleştirme ve ayrıca aynı .NET sınıf örnekleri kullanabilmek için hakkındaki bu özellik, (büyük olasılıkla bir şey kadar basit bir `Dictionary<string, object>`) farklı varlık türlerini temsil etmek için aynı EF Core modelinde.
Bu özellik, birleştirme varlık olmadan çok-çok ilişkileri desteklemek için bir atlama taşı ([sorun #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), EF Core için en çok istenen geliştirmeleri birinde.

## <a name="ef-63-on-net-core"></a>.NET core'da EF 6.3

[Sorunu EF6 izleme #271](https://github.com/aspnet/EntityFramework6/issues/271)

Bu özellik iş başlatıldı ancak geçerli önizlemede dahil değildir. 

Birçok mevcut uygulamaları EF'ın önceki sürümlerini kullanın ve bunları yalnızca .NET Core yararlanmak için EF Core'a taşıma bazen önemli bir efor gerektirebilir olduğunu biliyoruz.
Bu nedenle, biz .NET Core 3.0 üzerinde çalıştırılacak EF 6'ın sonraki sürümü adapte.
Biz bunu çok az değişiklikle taşıma mevcut uygulamaları kolaylaştırmak için yapıyor.
Olan oraya giden bazı sınırlamalar olacak. Örneğin:
- Diğer veritabanlarını dahil edilen SQL Server destek yanı sıra .NET Core üzerinde çalışmak yeni sağlayıcılar gerekir
- SQL Server uzamsal desteği etkinleştirilemez

Ayrıca, bu noktada EF 6 için planlanan yeni özellik olduğunu unutmayın.
