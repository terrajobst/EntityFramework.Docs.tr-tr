---
title: Değer dönüştürmeleri-EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 93774bc1bc3887f982faeac151825a6643c1107c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417204"
---
# <a name="value-conversions"></a>Değer Dönüştürmeleri

> [!NOTE]  
> Bu özellik EF Core 2,1 ' de yenidir.

Değer dönüştürücüler, veritabanından okuma veya yazma sırasında özellik değerlerinin dönüştürülmesine izin verir. Bu dönüştürme bir değerden diğerine (örneğin, şifreleme dizeleri) veya bir türden bir değerden başka bir türdeki bir değere (örneğin, Enum değerlerini veritabanındaki dizelerdeki veya dizeden dönüştürme) olabilir.

## <a name="fundamentals"></a>Temel Konular

Değer dönüştürücüler bir `ModelClrType` ve bir `ProviderClrType`bakımından belirtilmiştir. Model türü, varlık türündeki özelliğin .NET türüdür. Sağlayıcı türü, veritabanı sağlayıcısı tarafından anlaşılan .NET türüdür. Örneğin, numaralandırmaları veritabanına dizeler olarak kaydetmek için, model türü numaralandırmanın türüdür ve sağlayıcı türü `String`. Bu iki tür aynı olabilir.

Dönüştürmeler iki `Func` ifade ağacı kullanılarak tanımlanır: biri `ModelClrType` `ProviderClrType` ve diğeri `ProviderClrType` arasında `ModelClrType`. İfade ağaçları, verimli dönüştürmeler için veritabanı erişim koduna Derlenebilmeleri için kullanılır. Karmaşık dönüştürmeler için, ifade ağacı dönüştürmeyi gerçekleştiren bir yönteme yönelik basit bir çağrı olabilir.

## <a name="configuring-a-value-converter"></a>Değer dönüştürücüsünü yapılandırma

Değer dönüştürmeleri, `DbContext``OnModelCreating` özelliklerde tanımlanmıştır. Örneğin, şöyle tanımlanmış bir Enum ve varlık türü düşünün:

``` csharp
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

Ardından, veritabanında Enum değerlerini dizeler olarak depolamak için `OnModelCreating` tanımlanabilir (örneğin, "Donkey", "Mule",...):

``` csharp
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
> Bir `null` değeri hiçbir şekilde bir değer dönüştürücüsüne geçirilmeyecektir. Bu, dönüştürme uygulanmasını kolaylaştırır ve bunlara null yapılabilir ve null yapılamayan özellikler arasında paylaşılmasını sağlar.

## <a name="the-valueconverter-class"></a>ValueConverter sınıfı

Yukarıda gösterildiği gibi `HasConversion` çağırmak, bir `ValueConverter` örneği oluşturur ve bunu özelliğinde ayarlar. `ValueConverter` bunun yerine açık bir şekilde oluşturulabilir. Örnek:

``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

Bu, birden çok özellik aynı dönüştürmeyi kullanırken yararlı olabilir.

> [!NOTE]  
> Şu anda, belirli bir türün her özelliğinin aynı değer dönüştürücüsünü kullanması gerektiğini tek bir yerde belirtmenin bir yolu yoktur. Bu özellik gelecek bir sürüm için göz önünde bulundurulacaktır.

## <a name="built-in-converters"></a>Yerleşik dönüştürücüler

EF Core, `Microsoft.EntityFrameworkCore.Storage.ValueConversion` ad alanında bulunan önceden tanımlanmış `ValueConverter` sınıfları kümesiyle birlikte gelir. Bunlar:

* `BoolToZeroOneConverter`-bool ile sıfıra ve bir
* `BoolToStringConverter`-"Y" ve "N" gibi dizelere bool
* `BoolToTwoValuesConverter`-her iki değere bool
* `BytesToStringConverter` baytlık dizi ile Base64 kodlamalı dize
* yalnızca tür dönüştürme gerektiren `CastingConverter` dönüşümler
* `CharToStringConverter`-char-tek karakter dizesine
* `DateTimeOffsetToBinaryConverter`-DateTimeOffset ile ikili kodlu 64 bit değere
* `DateTimeOffsetToBytesConverter`-bayt dizisine DateTimeOffset
* `DateTimeOffsetToStringConverter`-dize olarak DateTimeOffset
* `DateTimeToBinaryConverter`-DateTime, DateTimeKind dahil 64 bitlik bir değere
* `DateTimeToStringConverter`-DateTime ile String
* `DateTimeToTicksConverter`-tarih/saat sonu
* `EnumToNumberConverter`-numaralandırma temel alınan sayıya
* `EnumToStringConverter`-dizeye Enum
* `GuidToBytesConverter`-GUID-Byte dizisine
* `GuidToStringConverter`-GUID-String
* `NumberToBytesConverter`-bayt dizisine herhangi bir sayısal değer
* `NumberToStringConverter`-dizeye herhangi bir sayısal değer
* `StringToBytesConverter`--UTF8 bayta dize
* `TimeSpanToStringConverter` zaman aralığı dizeye
* `TimeSpanToTicksConverter` TimeSpan-ticks

`EnumToStringConverter` bu listeye dahil edildiğini unutmayın. Bu, yukarıda gösterildiği gibi dönüştürme işleminin açıkça belirtilmesi gerekmediği anlamına gelir. Bunun yerine, yerleşik dönüştürücüyü kullanmanız yeterlidir:

``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

Tüm yerleşik dönüştürücülerin durumsuz olduğunu ve bu nedenle tek bir örneğin birden çok özellik tarafından güvenli bir şekilde paylaşılacağını unutmayın.

## <a name="pre-defined-conversions"></a>Önceden tanımlanmış dönüşümler

Yerleşik dönüştürücünün bulunduğu yaygın dönüştürmeler için dönüştürücüyü açıkça belirtmeye gerek yoktur. Bunun yerine, hangi sağlayıcı türünün kullanılması gerektiğini ve EF 'in uygun yerleşik dönüştürücüyü otomatik olarak kullanacağı yapılandırmanız yeterlidir. Dize dönüştürmelerinde Enum, yukarıdaki bir örnek olarak kullanılır, ancak sağlayıcı türü yapılandırılırsa EF bunu otomatik olarak olur:

``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

Aynı şey, açıkça sütun türünü belirterek elde edilebilir. Örneğin, varlık türü şöyle tanımlanmazsa:

``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

Sonra Enum değerleri, `OnModelCreating`başka bir yapılandırma olmadan veritabanına dizeler olarak kaydedilir.

## <a name="limitations"></a>Sınırlamalar

Değer dönüştürme sisteminin bazı bilinen geçerli sınırlamaları vardır:

* Yukarıda belirtildiği gibi, `null` dönüştürülemez.
* Şu anda, bir özelliğin birden çok sütuna dönüştürülmesini veya bunun tersini yapmanın bir yolu yoktur.
* Değer dönüştürmelerinden kullanımı, EF Core ifadeleri SQL 'e çevirebilme olanağını etkileyebilir. Bu gibi durumlarda bir uyarı kaydedilir.
Bu sınırlamaların kaldırılması gelecekteki bir sürüm için göz önünde bulundurulmaktadır.
