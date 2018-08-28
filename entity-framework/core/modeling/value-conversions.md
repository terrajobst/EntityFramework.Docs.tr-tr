---
title: Değer dönüştürmeleri - EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: d6b51a0a70ee527844b6fe995f39bec534dbaba8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996294"
---
# <a name="value-conversions"></a>Değer dönüştürmeleri

> [!NOTE]  
> Bu özellik, EF Core 2.1 içinde yeni bir özelliktir.

Değer dönüştürücüler, veritabanına yazma veya okuma dönüştürülecek özellik değerlerini sağlar. Bu dönüştürme diğerine (örneğin, şifreleme dizeleri) aynı türde bir değer veya başka bir tür (örneğin, dönüştürme sabit listesi değerleri dizeleri veritabanındaki gelen ve giden.) bir değer için bir tür değeri olabilir.

## <a name="fundamentals"></a>Temeller

Değer dönüştürücüler açısından belirtilen bir `ModelClrType` ve `ProviderClrType`. Model türü varlık türü özelliği .NET türüdür. Sağlayıcı türü, veritabanı sağlayıcısı tarafından anlaşılan .NET türüdür. Örneğin, numaralandırmalar veritabanında dize olarak kaydetmek için model türü sabit listesi türüdür ve sağlayıcı türüdür `String`. Bu iki tür aynı olabilir.

Dönüştürme, iki kullanılarak tanımlanır `Func` ifade ağaçları: birinden `ModelClrType` için `ProviderClrType` ve diğer `ProviderClrType` için `ModelClrType`. İfade ağaçları verimli dönüştürmeler için veritabanı erişim kodun içine derlenebilir için kullanılır. İfade ağacı, karmaşık dönüştürmeleri için basit bir dönüştürme uygulayan bir yöntem çağrısı olabilir.

## <a name="configuring-a-value-converter"></a>Bir değer dönüştürücü yapılandırma

Değer dönüştürmeleri özellikler tanımlanmıştır `OnModelCreating` , uygulamanızın `DbContext`. Örneğin, bir sabit listesi ve varlık türü olarak tanımlanmış göz önünde bulundurun:
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
Dönüştürme tanımlanabilir sonra `OnModelCreating` enum değerlerinden, dize (örneğin, "Donkey", "Mule",...) veritabanı olarak depolamak için:
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
> A `null` değeri için bir değer dönüştürücü hiçbir zaman geçirilir. Bu dönüştürmeler uygulamasını kolaylaştırır ve boş değer atanabilir ve NULL olmayan özellikler arasında paylaşılmasına olanak sağlar.

## <a name="the-valueconverter-class"></a>ValueConverter sınıfı

Çağırma `HasConversion` oluşturacaktır yukarıda gösterildiği gibi bir `ValueConverter` örneği ve özelliği ayarlayın. `ValueConverter` Yerine açıkça oluşturulabilir. Örneğin:
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Birden çok özellik aynı dönüştürme kullandığınızda bu yararlı olabilir.

> [!NOTE]  
> Şu anda tek bir yerde, her özellik, belirli bir türde aynı değer dönüştürücü kullanması gerektiğini belirtmek için bir yolu yoktur. Bu özellik, gelecekteki sürümlerde sunulması kabul edilir.

## <a name="built-in-converters"></a>Yerleşik dönüştürücüler

EF Core yüklenebilen bir dizi önceden tanımlanmış `ValueConverter` bulunan sınıfları, `Microsoft.EntityFrameworkCore.Storage.ValueConversion` ad alanı. Bunlar:
* `BoolToZeroOneConverter` -Sıfır ve Bool
* `BoolToStringConverter` -"Y" ve "N" gibi dizelere Bool
* `BoolToTwoValuesConverter` -Her iki değer için Bool
* `BytesToStringConverter` -Base64 ile kodlanmış dizeye bayt dizisi
* `CastingConverter` -Yalnızca bir Csharp tür dönüştürme gerektiren dönüştürmeler
* `CharToStringConverter` -Tek bir karakter dizesindeki bir karakter
* `DateTimeOffsetToBinaryConverter` -İkili Kodlanmış 64-bit değere DateTimeOffset
* `DateTimeOffsetToBytesConverter` -Bayt dizisine DateTimeOffset
* `DateTimeOffsetToStringConverter` -DateTimeOffset dizeye
* `DateTimeToBinaryConverter` -64-bit değere DateTimeKind dahil olmak üzere tarih saat
* `DateTimeToStringConverter` -Dizeye tarih saat
* `DateTimeToTicksConverter` -Saat döngüsü için tarih saat
* `EnumToNumberConverter` -Temel alınan sayı sabit listesi
* `EnumToStringConverter` -Dize sabit listesi
* `GuidToBytesConverter` -Bayt dizisine GUID
* `GuidToStringConverter` -GUID dizesi
* `NumberToBytesConverter` -Bayt dizisine herhangi bir sayısal değer
* `NumberToStringConverter` -Herhangi bir sayısal değer dizesi
* `StringToBytesConverter` -UTF8 baytı dize
* `TimeSpanToStringConverter` -Dize zaman aralığı
* `TimeSpanToTicksConverter` -TimeSpan işaretleri

Dikkat `EnumToStringConverter` bu listede bulunuyor. Başka bir deyişle, dönüştürme açıkça, yukarıda gösterildiği gibi belirtmek için gerek yoktur. Bunun yerine, yalnızca yerleşik dönüştürücü kullanın:
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Tüm yerleşik dönüştürücüler durum bilgisiz olduğundan ve bu nedenle tek bir örnek güvenli bir şekilde birden çok özellik tarafından paylaşılabilir unutmayın.

## <a name="pre-defined-conversions"></a>Önceden tanımlı dönüştürmeler

Yerleşik bir dönüştürücü bulunduğu ortak dönüştürmeleri için dönüştürücü açıkça belirtmek için gerek yoktur. Bunun yerine, hangi sağlayıcı türü kullanılmalıdır yapılandırılması ve EF uygun yerleşik dönüştürücü otomatik olarak kullanacak. Enum dize dönüştürme için yukarıdaki örnek olarak kullanılır, ancak bu sağlayıcı türü yapılandırılmışsa EF aslında bu otomatik olarak yapar:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
Sütun türünü açıkça belirterek aynı şeyi elde edilebilir. Örneğin, varlık türü tanımladıysanız gibi için:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Enum değerlerinden, dize içinde başka bir yapılandırma olmadan olarak kaydedilecek sonra `OnModelCreating`.

## <a name="limitations"></a>Sınırlamalar

Değer dönüştürme sistemi bazı bilinen geçerli sınırlamalar vardır:
* Yukarıda belirtildiği gibi `null` dönüştürülemez.
* Şu anda birden çok sütun veya tam tersi bir özelliğin dönüştürme yaymak için bir yolu yoktur.
* Değer dönüştürmeleri kullanımını EF Core için SQL deyimleri Çevir yeteneğini etkileyebilir. Böyle durumlarda, bir uyarı günlüğe kaydedilir.
Bu sınırlamalar kaldırılmasını gelecekteki sürümlerde sunulması kabul edilir.
