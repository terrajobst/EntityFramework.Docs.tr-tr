---
title: "Değer dönüşümler - EF çekirdek"
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 50acba39cdec16caa9300fcaf47ab6242a4f69fb
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="value-conversions"></a>Değer dönüşümler

> [!NOTE]  
> Bu özelliği EF çekirdek 2.1 içinde yeni bir özelliktir.

Değer dönüştürücüler okuma veya yazma veritabanına dönüştürülecek özellik değerlerini izin verin. Bu dönüştürme diğerine (örneğin, şifreleme dizeleri) aynı türde bir değer veya türünde bir değer tek bir değer için başka bir tür (örneğin, dönüştürme enum değerleri için ve veritabanı dizelerini.) olabilir

## <a name="fundamentals"></a>Temeller

Değer dönüştürücüler cinsinden belirtilen bir `ModelClrType` ve `ProviderClrType`. Model türü varlık türü özelliğinde .NET türüdür. Sağlayıcı türü sağlayıcısı tarafından anlaşılan .NET türüdür. Örneğin veritabanında dize olarak numaralandırmaları kaydetmek için model türü enum türünde ve sağlayıcı türü `String`. Bu iki tür aynı olabilir.

Dönüşümleri, iki kullanılarak tanımlanır `Func` ifade ağaçları: birinden `ModelClrType` için `ProviderClrType` ve diğer `ProviderClrType` için `ModelClrType`. İfade ağaçları verimli dönüştürmeleri için veritabanı erişim koda derlenebilir için kullanılır. Karmaşık dönüştürmeleri için ifade ağacına basit bir dönüştürme gerçekleştirdiği bir yöntem çağrısı olabilir.

## <a name="configuring-a-value-converter"></a>Değer dönüştürücüsü yapılandırma

Değer dönüşümler, DbContext OnModelCreating özelliklerinde tanımlanır. Örneğin, bir enum ve varlık türü olarak tanımlanmış göz önünde bulundurun:
```Csharp
public class Rider
{
    public int Id { get; set; }
    public EquineBeast Mount { get; set; }
}

public enum EquineBeast
{
    Donkey,
    Mule,
    Horse,
    Unicorn
}
```
Ardından dönüştürmeleri enum değerleri dize olarak (örneğin depolamak için OnModelCreating içinde tanımlanmış olması "Donkey", "Mule",...) Veritabanı:
```Csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Rider>()
        .Property(e => e.Mount)
        .HasConversion(
            v => v.ToString(),
            v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));
}
```
> [!NOTE]  
> A `null` değeri için bir değer dönüştürücüsü hiçbir zaman geçirilecektir. Bu dönüşümleri uygulama kolaylaştırır ve bunları boş değer atanabilir ve null özellikleri arasında paylaşılmasına izin verir.

## <a name="the-valueconverter-class"></a>ValueConverter sınıfı

Çağırma `HasConversion` oluşturacak yukarıda gösterildiği gibi bir `ValueConverter` örneği ve özelliği ayarlayın. `ValueConverter` Yerine açıkça oluşturulabilir. Örneğin:
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Birden çok özellikleri aynı dönüştürme kullandığınızda bu yararlı olabilir.

> [!NOTE]  
> Şu anda tek bir yerde her özellik belirli bir türde aynı değer dönüştürücüsü kullanması gerektiğini belirtmek için bir yolu yoktur. Bu özellik için gelecekteki bir sürüm olarak kabul edilir.

## <a name="built-in-converters"></a>Yerleşik dönüştürücüler

EF çekirdek gelir bir dizi önceden tanımlanmış `ValueConverter` bulunan sınıflar, `Microsoft.EntityFrameworkCore.Storage.Converters` ad alanı. Bunlar:
* `BoolToZeroOneConverter` -Bool sıfır ve
* `BoolToStringConverter` -"Y" ve "N" gibi dizelere Bool
* `BoolToTwoValuesConverter` -Bool herhangi iki değer için
* `BytesToStringConverter` -Base64 ile kodlanmış dizeye bayt dizisi
* `CastingConverter` -Yalnızca Csharp cast gerektiren dönüşümleri
* `CharToStringConverter` -Tek bir karakter dizesine char
* `DateTimeOffsetToBinaryConverter` -DateTimeOffset ikili olarak kodlanmış 64-bit değeri
* `DateTimeOffsetToBytesConverter` -DateTimeOffset Bayt dizisine
* `DateTimeOffsetToStringConverter` -DateTimeOffset dizeye
* `DateTimeToBinaryConverter` -DateTimeKind dahil olmak üzere 64-bit değerine tarih saat
* `DateTimeToStringConverter` -Dizeye tarih saat
* `DateTimeToTicksConverter` -Ticks için tarih saat
* `EnumToNumberConverter` -Temel alınan sayı Enum
* `EnumToStringConverter` -Enum dizeye
* `GuidToBytesConverter` -Guid Bayt dizisine
* `GuidToStringConverter` -GUID dize olarak
* `NumberToBytesConverter` -Herhangi bir sayısal değer Bayt dizisine
* `NumberToStringConverter` -Herhangi bir sayısal dize değerine
* `StringToBytesConverter` -UTF8 bayt dize
* `TimeSpanToStringConverter` -TimeSpan dizeye
* `TimeSpanToTicksConverter` -Çizgilerine TimeSpan

Dikkat `EnumToStringConverter` bu listede yer. Başka bir deyişle, dönüştürme yukarıda gösterildiği gibi açık olarak belirtmek için gerek yoktur. Bunun yerine, yalnızca yerleşik dönüştürücü kullanın:
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Tüm yerleşik dönüştürücüler durum bilgisiz ve bu nedenle tek bir örnek güvenli bir şekilde birden çok özellikleri tarafından paylaşılabilir unutmayın.

## <a name="pre-defined-conversions"></a>Önceden tanımlanmış dönüşümleri

Yerleşik bir dönüştürücü bulunduğu ortak dönüştürmelerde dönüştürücü açıkça belirtmek için gerek yoktur. Bunun yerine, yalnızca hangi sağlayıcı türü kullanılmalıdır yapılandırın ve EF otomatik olarak uygun yapı dönüştürücü kullanır. Enum dize dönüştürmeleri için yukarıdaki örnek olarak kullanılır, ancak sağlayıcı türü yapılandırılmışsa EF gerçekte bu otomatik olarak yapar:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
Aynı şey açıkça sütun türü belirterek elde edilebilir. Örneğin, varlık türü tanımlanmışsa gibi şekilde:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Ardından enum değerleri, dize OnModelCreating içinde başka bir yapılandırma olmadan veritabanı olarak kaydedilir.

## <a name="limitations"></a>Sınırlamalar

Değer dönüştürmesi sistem birkaç bilinen geçerli sınırlamalar vardır:
* Yukarıda belirtildiği gibi `null` dönüştürülemiyor.
* Şu anda bir özellik dönüştürülmesi multuple sütunlara veya tam tersini yayılan mümkün değildir.
* Değer dönüşümler kullanımını EF çekirdek SQL ifadeleri Çevir yeteneğini etkileyebilir. Bir uyarı için bu gibi durumlarda günlüğe kaydedilir.
Bu sınırlamalara kaldırılması için gelecekteki bir sürüm kabul edilir.
