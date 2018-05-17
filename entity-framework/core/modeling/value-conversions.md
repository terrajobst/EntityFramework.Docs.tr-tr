---
title: Değer dönüşümler - EF çekirdek
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 3e97c05a87ad9b4817c03f446031ea6c74704f5b
ms.sourcegitcommit: 605e42232854ce44bae09624a6eebc35b8e2473b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="value-conversions"></a><span data-ttu-id="9d30c-102">Değer dönüşümler</span><span class="sxs-lookup"><span data-stu-id="9d30c-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="9d30c-103">Bu özelliği EF çekirdek 2.1 içinde yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="9d30c-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="9d30c-104">Değer dönüştürücüler okuma veya yazma veritabanına dönüştürülecek özellik değerlerini izin verin.</span><span class="sxs-lookup"><span data-stu-id="9d30c-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="9d30c-105">Bu dönüştürme diğerine (örneğin, şifreleme dizeleri) aynı türde bir değer veya türünde bir değer tek bir değer için başka bir tür (örneğin, dönüştürme enum değerleri için ve veritabanı dizelerini.) olabilir</span><span class="sxs-lookup"><span data-stu-id="9d30c-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="9d30c-106">Temeller</span><span class="sxs-lookup"><span data-stu-id="9d30c-106">Fundamentals</span></span>

<span data-ttu-id="9d30c-107">Değer dönüştürücüler cinsinden belirtilen bir `ModelClrType` ve `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="9d30c-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="9d30c-108">Model türü varlık türü özelliğinde .NET türüdür.</span><span class="sxs-lookup"><span data-stu-id="9d30c-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="9d30c-109">Sağlayıcı türü sağlayıcısı tarafından anlaşılan .NET türüdür.</span><span class="sxs-lookup"><span data-stu-id="9d30c-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="9d30c-110">Örneğin veritabanında dize olarak numaralandırmaları kaydetmek için model türü enum türünde ve sağlayıcı türü `String`.</span><span class="sxs-lookup"><span data-stu-id="9d30c-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="9d30c-111">Bu iki tür aynı olabilir.</span><span class="sxs-lookup"><span data-stu-id="9d30c-111">These two types can be the same.</span></span>

<span data-ttu-id="9d30c-112">Dönüşümleri, iki kullanılarak tanımlanır `Func` ifade ağaçları: birinden `ModelClrType` için `ProviderClrType` ve diğer `ProviderClrType` için `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="9d30c-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="9d30c-113">İfade ağaçları verimli dönüştürmeleri için veritabanı erişim koda derlenebilir için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9d30c-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="9d30c-114">Karmaşık dönüştürmeleri için ifade ağacına basit bir dönüştürme gerçekleştirdiği bir yöntem çağrısı olabilir.</span><span class="sxs-lookup"><span data-stu-id="9d30c-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="9d30c-115">Değer dönüştürücüsü yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9d30c-115">Configuring a value converter</span></span>

<span data-ttu-id="9d30c-116">Değer dönüşümler, DbContext OnModelCreating özelliklerinde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="9d30c-116">Value conversions are defined on properties in the OnModelCreating of your DbContext.</span></span> <span data-ttu-id="9d30c-117">Örneğin, bir enum ve varlık türü olarak tanımlanmış göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="9d30c-117">For example, consider an enum and entity type defined as:</span></span>
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
<span data-ttu-id="9d30c-118">Ardından dönüştürmeleri enum değerleri dize olarak (örneğin depolamak için OnModelCreating içinde tanımlanmış olması "Donkey", "Mule",...) Veritabanı:</span><span class="sxs-lookup"><span data-stu-id="9d30c-118">Then conversions can be defined in OnModelCreating to store the enum values as strings (e.g. "Donkey", "Mule", ...) in the database:</span></span>
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
> <span data-ttu-id="9d30c-119">A `null` değeri için bir değer dönüştürücüsü hiçbir zaman geçirilecektir.</span><span class="sxs-lookup"><span data-stu-id="9d30c-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="9d30c-120">Bu dönüşümleri uygulama kolaylaştırır ve bunları boş değer atanabilir ve null özellikleri arasında paylaşılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="9d30c-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="9d30c-121">ValueConverter sınıfı</span><span class="sxs-lookup"><span data-stu-id="9d30c-121">The ValueConverter class</span></span>

<span data-ttu-id="9d30c-122">Çağırma `HasConversion` oluşturacak yukarıda gösterildiği gibi bir `ValueConverter` örneği ve özelliği ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="9d30c-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="9d30c-123">`ValueConverter` Yerine açıkça oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="9d30c-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="9d30c-124">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9d30c-124">For example:</span></span>
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="9d30c-125">Birden çok özellikleri aynı dönüştürme kullandığınızda bu yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="9d30c-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="9d30c-126">Şu anda tek bir yerde her özellik belirli bir türde aynı değer dönüştürücüsü kullanması gerektiğini belirtmek için bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="9d30c-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="9d30c-127">Bu özellik için gelecekteki bir sürüm olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="9d30c-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="9d30c-128">Yerleşik dönüştürücüler</span><span class="sxs-lookup"><span data-stu-id="9d30c-128">Built-in converters</span></span>

<span data-ttu-id="9d30c-129">EF çekirdek gelir bir dizi önceden tanımlanmış `ValueConverter` bulunan sınıflar, `Microsoft.EntityFrameworkCore.Storage.ValueConversion` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="9d30c-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="9d30c-130">Bunlar:</span><span class="sxs-lookup"><span data-stu-id="9d30c-130">These are:</span></span>
* <span data-ttu-id="9d30c-131">`BoolToZeroOneConverter` -Bool sıfır ve</span><span class="sxs-lookup"><span data-stu-id="9d30c-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="9d30c-132">`BoolToStringConverter` -"Y" ve "N" gibi dizelere Bool</span><span class="sxs-lookup"><span data-stu-id="9d30c-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="9d30c-133">`BoolToTwoValuesConverter` -Bool herhangi iki değer için</span><span class="sxs-lookup"><span data-stu-id="9d30c-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="9d30c-134">`BytesToStringConverter` -Base64 ile kodlanmış dizeye bayt dizisi</span><span class="sxs-lookup"><span data-stu-id="9d30c-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="9d30c-135">`CastingConverter` -Yalnızca Csharp cast gerektiren dönüşümleri</span><span class="sxs-lookup"><span data-stu-id="9d30c-135">`CastingConverter` - Conversions that require only a Csharp cast</span></span>
* <span data-ttu-id="9d30c-136">`CharToStringConverter` -Tek bir karakter dizesine char</span><span class="sxs-lookup"><span data-stu-id="9d30c-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="9d30c-137">`DateTimeOffsetToBinaryConverter` -DateTimeOffset ikili olarak kodlanmış 64-bit değeri</span><span class="sxs-lookup"><span data-stu-id="9d30c-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="9d30c-138">`DateTimeOffsetToBytesConverter` -DateTimeOffset Bayt dizisine</span><span class="sxs-lookup"><span data-stu-id="9d30c-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="9d30c-139">`DateTimeOffsetToStringConverter` -DateTimeOffset dizeye</span><span class="sxs-lookup"><span data-stu-id="9d30c-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="9d30c-140">`DateTimeToBinaryConverter` -DateTimeKind dahil olmak üzere 64-bit değerine tarih saat</span><span class="sxs-lookup"><span data-stu-id="9d30c-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="9d30c-141">`DateTimeToStringConverter` -Dizeye tarih saat</span><span class="sxs-lookup"><span data-stu-id="9d30c-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="9d30c-142">`DateTimeToTicksConverter` -Ticks için tarih saat</span><span class="sxs-lookup"><span data-stu-id="9d30c-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="9d30c-143">`EnumToNumberConverter` -Temel alınan sayı Enum</span><span class="sxs-lookup"><span data-stu-id="9d30c-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="9d30c-144">`EnumToStringConverter` -Enum dizeye</span><span class="sxs-lookup"><span data-stu-id="9d30c-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="9d30c-145">`GuidToBytesConverter` -Guid Bayt dizisine</span><span class="sxs-lookup"><span data-stu-id="9d30c-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="9d30c-146">`GuidToStringConverter` -GUID dize olarak</span><span class="sxs-lookup"><span data-stu-id="9d30c-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="9d30c-147">`NumberToBytesConverter` -Herhangi bir sayısal değer Bayt dizisine</span><span class="sxs-lookup"><span data-stu-id="9d30c-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="9d30c-148">`NumberToStringConverter` -Herhangi bir sayısal dize değerine</span><span class="sxs-lookup"><span data-stu-id="9d30c-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="9d30c-149">`StringToBytesConverter` -UTF8 bayt dize</span><span class="sxs-lookup"><span data-stu-id="9d30c-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="9d30c-150">`TimeSpanToStringConverter` -TimeSpan dizeye</span><span class="sxs-lookup"><span data-stu-id="9d30c-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="9d30c-151">`TimeSpanToTicksConverter` -Çizgilerine TimeSpan</span><span class="sxs-lookup"><span data-stu-id="9d30c-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="9d30c-152">Dikkat `EnumToStringConverter` bu listede yer.</span><span class="sxs-lookup"><span data-stu-id="9d30c-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="9d30c-153">Başka bir deyişle, dönüştürme yukarıda gösterildiği gibi açık olarak belirtmek için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="9d30c-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="9d30c-154">Bunun yerine, yalnızca yerleşik dönüştürücü kullanın:</span><span class="sxs-lookup"><span data-stu-id="9d30c-154">Instead, just use the built-in converter:</span></span>
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="9d30c-155">Tüm yerleşik dönüştürücüler durum bilgisiz ve bu nedenle tek bir örnek güvenli bir şekilde birden çok özellikleri tarafından paylaşılabilir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="9d30c-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="9d30c-156">Önceden tanımlanmış dönüşümleri</span><span class="sxs-lookup"><span data-stu-id="9d30c-156">Pre-defined conversions</span></span>

<span data-ttu-id="9d30c-157">Yerleşik bir dönüştürücü bulunduğu ortak dönüştürmelerde dönüştürücü açıkça belirtmek için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="9d30c-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="9d30c-158">Bunun yerine, yalnızca hangi sağlayıcı türü kullanılmalıdır yapılandırın ve EF otomatik olarak uygun yapı dönüştürücü kullanır.</span><span class="sxs-lookup"><span data-stu-id="9d30c-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate build-in converter.</span></span> <span data-ttu-id="9d30c-159">Enum dize dönüştürmeleri için yukarıdaki örnek olarak kullanılır, ancak sağlayıcı türü yapılandırılmışsa EF gerçekte bu otomatik olarak yapar:</span><span class="sxs-lookup"><span data-stu-id="9d30c-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="9d30c-160">Aynı şey açıkça sütun türü belirterek elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="9d30c-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="9d30c-161">Örneğin, varlık türü tanımlanmışsa gibi şekilde:</span><span class="sxs-lookup"><span data-stu-id="9d30c-161">For example, if the entity type is defined like so:</span></span>
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="9d30c-162">Ardından enum değerleri, dize OnModelCreating içinde başka bir yapılandırma olmadan veritabanı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="9d30c-162">Then the enum values will be saved as strings in the database without any further configuration in OnModelCreating.</span></span>

## <a name="limitations"></a><span data-ttu-id="9d30c-163">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="9d30c-163">Limitations</span></span>

<span data-ttu-id="9d30c-164">Değer dönüştürmesi sistem birkaç bilinen geçerli sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="9d30c-164">There are a few known current limitations of the value convertion system:</span></span>
* <span data-ttu-id="9d30c-165">Yukarıda belirtildiği gibi `null` dönüştürülemiyor.</span><span class="sxs-lookup"><span data-stu-id="9d30c-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="9d30c-166">Şu anda bir özellik dönüştürülmesi birden çok sütun ya da tam tersini yayılan mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="9d30c-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="9d30c-167">Değer dönüşümler kullanımını EF çekirdek SQL ifadeleri Çevir yeteneğini etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="9d30c-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="9d30c-168">Bir uyarı için bu gibi durumlarda günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="9d30c-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="9d30c-169">Bu sınırlamalara kaldırılması için gelecekteki bir sürüm kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="9d30c-169">Removal of these limitations is being considered for a future release.</span></span>
