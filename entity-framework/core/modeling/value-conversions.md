---
title: Değer dönüştürmeleri-EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 93774bc1bc3887f982faeac151825a6643c1107c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654788"
---
# <a name="value-conversions"></a><span data-ttu-id="ed858-102">Değer Dönüştürmeleri</span><span class="sxs-lookup"><span data-stu-id="ed858-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="ed858-103">Bu özellik EF Core 2,1 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="ed858-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="ed858-104">Değer dönüştürücüler, veritabanından okuma veya yazma sırasında özellik değerlerinin dönüştürülmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="ed858-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="ed858-105">Bu dönüştürme bir değerden diğerine (örneğin, şifreleme dizeleri) veya bir türden bir değerden başka bir türdeki bir değere (örneğin, Enum değerlerini veritabanındaki dizelerdeki veya dizeden dönüştürme) olabilir.</span><span class="sxs-lookup"><span data-stu-id="ed858-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="ed858-106">Temeller</span><span class="sxs-lookup"><span data-stu-id="ed858-106">Fundamentals</span></span>

<span data-ttu-id="ed858-107">Değer dönüştürücüler bir `ModelClrType` ve bir `ProviderClrType`bakımından belirtilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ed858-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="ed858-108">Model türü, varlık türündeki özelliğin .NET türüdür.</span><span class="sxs-lookup"><span data-stu-id="ed858-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="ed858-109">Sağlayıcı türü, veritabanı sağlayıcısı tarafından anlaşılan .NET türüdür.</span><span class="sxs-lookup"><span data-stu-id="ed858-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="ed858-110">Örneğin, numaralandırmaları veritabanına dizeler olarak kaydetmek için, model türü numaralandırmanın türüdür ve sağlayıcı türü `String`.</span><span class="sxs-lookup"><span data-stu-id="ed858-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="ed858-111">Bu iki tür aynı olabilir.</span><span class="sxs-lookup"><span data-stu-id="ed858-111">These two types can be the same.</span></span>

<span data-ttu-id="ed858-112">Dönüştürmeler iki `Func` ifade ağacı kullanılarak tanımlanır: biri `ModelClrType` `ProviderClrType` ve diğeri `ProviderClrType` arasında `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="ed858-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="ed858-113">İfade ağaçları, verimli dönüştürmeler için veritabanı erişim koduna Derlenebilmeleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ed858-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="ed858-114">Karmaşık dönüştürmeler için, ifade ağacı dönüştürmeyi gerçekleştiren bir yönteme yönelik basit bir çağrı olabilir.</span><span class="sxs-lookup"><span data-stu-id="ed858-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="ed858-115">Değer dönüştürücüsünü yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ed858-115">Configuring a value converter</span></span>

<span data-ttu-id="ed858-116">Değer dönüştürmeleri, `DbContext``OnModelCreating` özelliklerde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="ed858-116">Value conversions are defined on properties in the `OnModelCreating` of your `DbContext`.</span></span> <span data-ttu-id="ed858-117">Örneğin, şöyle tanımlanmış bir Enum ve varlık türü düşünün:</span><span class="sxs-lookup"><span data-stu-id="ed858-117">For example, consider an enum and entity type defined as:</span></span>

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

<span data-ttu-id="ed858-118">Ardından, veritabanında Enum değerlerini dizeler olarak depolamak için `OnModelCreating` tanımlanabilir (örneğin, "Donkey", "Mule",...):</span><span class="sxs-lookup"><span data-stu-id="ed858-118">Then conversions can be defined in `OnModelCreating` to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>

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
> <span data-ttu-id="ed858-119">Bir `null` değeri hiçbir şekilde bir değer dönüştürücüsüne geçirilmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="ed858-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="ed858-120">Bu, dönüştürme uygulanmasını kolaylaştırır ve bunlara null yapılabilir ve null yapılamayan özellikler arasında paylaşılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="ed858-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="ed858-121">ValueConverter sınıfı</span><span class="sxs-lookup"><span data-stu-id="ed858-121">The ValueConverter class</span></span>

<span data-ttu-id="ed858-122">Yukarıda gösterildiği gibi `HasConversion` çağırmak, bir `ValueConverter` örneği oluşturur ve bunu özelliğinde ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ed858-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="ed858-123">`ValueConverter` bunun yerine açık bir şekilde oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="ed858-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="ed858-124">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ed858-124">For example:</span></span>

``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

<span data-ttu-id="ed858-125">Bu, birden çok özellik aynı dönüştürmeyi kullanırken yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="ed858-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="ed858-126">Şu anda, belirli bir türün her özelliğinin aynı değer dönüştürücüsünü kullanması gerektiğini tek bir yerde belirtmenin bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="ed858-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="ed858-127">Bu özellik gelecek bir sürüm için göz önünde bulundurulacaktır.</span><span class="sxs-lookup"><span data-stu-id="ed858-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="ed858-128">Yerleşik dönüştürücüler</span><span class="sxs-lookup"><span data-stu-id="ed858-128">Built-in converters</span></span>

<span data-ttu-id="ed858-129">EF Core, `Microsoft.EntityFrameworkCore.Storage.ValueConversion` ad alanında bulunan önceden tanımlanmış `ValueConverter` sınıfları kümesiyle birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="ed858-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="ed858-130">Bunlar:</span><span class="sxs-lookup"><span data-stu-id="ed858-130">These are:</span></span>

* <span data-ttu-id="ed858-131">`BoolToZeroOneConverter`-bool ile sıfıra ve bir</span><span class="sxs-lookup"><span data-stu-id="ed858-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="ed858-132">`BoolToStringConverter`-"Y" ve "N" gibi dizelere bool</span><span class="sxs-lookup"><span data-stu-id="ed858-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="ed858-133">`BoolToTwoValuesConverter`-her iki değere bool</span><span class="sxs-lookup"><span data-stu-id="ed858-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="ed858-134">`BytesToStringConverter` baytlık dizi ile Base64 kodlamalı dize</span><span class="sxs-lookup"><span data-stu-id="ed858-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="ed858-135">yalnızca tür dönüştürme gerektiren `CastingConverter` dönüşümler</span><span class="sxs-lookup"><span data-stu-id="ed858-135">`CastingConverter` - Conversions that require only a type cast</span></span>
* <span data-ttu-id="ed858-136">`CharToStringConverter`-char-tek karakter dizesine</span><span class="sxs-lookup"><span data-stu-id="ed858-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="ed858-137">`DateTimeOffsetToBinaryConverter`-DateTimeOffset ile ikili kodlu 64 bit değere</span><span class="sxs-lookup"><span data-stu-id="ed858-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="ed858-138">`DateTimeOffsetToBytesConverter`-bayt dizisine DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="ed858-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="ed858-139">`DateTimeOffsetToStringConverter`-dize olarak DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="ed858-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="ed858-140">`DateTimeToBinaryConverter`-DateTime, DateTimeKind dahil 64 bitlik bir değere</span><span class="sxs-lookup"><span data-stu-id="ed858-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="ed858-141">`DateTimeToStringConverter`-DateTime ile String</span><span class="sxs-lookup"><span data-stu-id="ed858-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="ed858-142">`DateTimeToTicksConverter`-tarih/saat sonu</span><span class="sxs-lookup"><span data-stu-id="ed858-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="ed858-143">`EnumToNumberConverter`-numaralandırma temel alınan sayıya</span><span class="sxs-lookup"><span data-stu-id="ed858-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="ed858-144">`EnumToStringConverter`-dizeye Enum</span><span class="sxs-lookup"><span data-stu-id="ed858-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="ed858-145">`GuidToBytesConverter`-GUID-Byte dizisine</span><span class="sxs-lookup"><span data-stu-id="ed858-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="ed858-146">`GuidToStringConverter`-GUID-String</span><span class="sxs-lookup"><span data-stu-id="ed858-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="ed858-147">`NumberToBytesConverter`-bayt dizisine herhangi bir sayısal değer</span><span class="sxs-lookup"><span data-stu-id="ed858-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="ed858-148">`NumberToStringConverter`-dizeye herhangi bir sayısal değer</span><span class="sxs-lookup"><span data-stu-id="ed858-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="ed858-149">`StringToBytesConverter`--UTF8 bayta dize</span><span class="sxs-lookup"><span data-stu-id="ed858-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="ed858-150">`TimeSpanToStringConverter` zaman aralığı dizeye</span><span class="sxs-lookup"><span data-stu-id="ed858-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="ed858-151">`TimeSpanToTicksConverter` TimeSpan-ticks</span><span class="sxs-lookup"><span data-stu-id="ed858-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="ed858-152">`EnumToStringConverter` bu listeye dahil edildiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ed858-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="ed858-153">Bu, yukarıda gösterildiği gibi dönüştürme işleminin açıkça belirtilmesi gerekmediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ed858-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="ed858-154">Bunun yerine, yerleşik dönüştürücüyü kullanmanız yeterlidir:</span><span class="sxs-lookup"><span data-stu-id="ed858-154">Instead, just use the built-in converter:</span></span>

``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

<span data-ttu-id="ed858-155">Tüm yerleşik dönüştürücülerin durumsuz olduğunu ve bu nedenle tek bir örneğin birden çok özellik tarafından güvenli bir şekilde paylaşılacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ed858-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="ed858-156">Önceden tanımlanmış dönüşümler</span><span class="sxs-lookup"><span data-stu-id="ed858-156">Pre-defined conversions</span></span>

<span data-ttu-id="ed858-157">Yerleşik dönüştürücünün bulunduğu yaygın dönüştürmeler için dönüştürücüyü açıkça belirtmeye gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="ed858-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="ed858-158">Bunun yerine, hangi sağlayıcı türünün kullanılması gerektiğini ve EF 'in uygun yerleşik dönüştürücüyü otomatik olarak kullanacağı yapılandırmanız yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="ed858-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate built-in converter.</span></span> <span data-ttu-id="ed858-159">Dize dönüştürmelerinde Enum, yukarıdaki bir örnek olarak kullanılır, ancak sağlayıcı türü yapılandırılırsa EF bunu otomatik olarak olur:</span><span class="sxs-lookup"><span data-stu-id="ed858-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>

``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

<span data-ttu-id="ed858-160">Aynı şey, açıkça sütun türünü belirterek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="ed858-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="ed858-161">Örneğin, varlık türü şöyle tanımlanmazsa:</span><span class="sxs-lookup"><span data-stu-id="ed858-161">For example, if the entity type is defined like so:</span></span>

``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

<span data-ttu-id="ed858-162">Sonra Enum değerleri, `OnModelCreating`başka bir yapılandırma olmadan veritabanına dizeler olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ed858-162">Then the enum values will be saved as strings in the database without any further configuration in `OnModelCreating`.</span></span>

## <a name="limitations"></a><span data-ttu-id="ed858-163">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="ed858-163">Limitations</span></span>

<span data-ttu-id="ed858-164">Değer dönüştürme sisteminin bazı bilinen geçerli sınırlamaları vardır:</span><span class="sxs-lookup"><span data-stu-id="ed858-164">There are a few known current limitations of the value conversion system:</span></span>

* <span data-ttu-id="ed858-165">Yukarıda belirtildiği gibi, `null` dönüştürülemez.</span><span class="sxs-lookup"><span data-stu-id="ed858-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="ed858-166">Şu anda, bir özelliğin birden çok sütuna dönüştürülmesini veya bunun tersini yapmanın bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="ed858-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="ed858-167">Değer dönüştürmelerinden kullanımı, EF Core ifadeleri SQL 'e çevirebilme olanağını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="ed858-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="ed858-168">Bu gibi durumlarda bir uyarı kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ed858-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="ed858-169">Bu sınırlamaların kaldırılması gelecekteki bir sürüm için göz önünde bulundurulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ed858-169">Removal of these limitations is being considered for a future release.</span></span>
