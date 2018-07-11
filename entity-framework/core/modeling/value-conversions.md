---
title: Değer dönüştürmeleri - EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 5bfb6111ac450db91f3f1a7074a924a1c8400ce7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949098"
---
# <a name="value-conversions"></a><span data-ttu-id="00b6a-102">Değer dönüştürmeleri</span><span class="sxs-lookup"><span data-stu-id="00b6a-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="00b6a-103">Bu özellik, EF Core 2.1 içinde yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="00b6a-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="00b6a-104">Değer dönüştürücüler, veritabanına yazma veya okuma dönüştürülecek özellik değerlerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="00b6a-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="00b6a-105">Bu dönüştürme diğerine (örneğin, şifreleme dizeleri) aynı türde bir değer veya başka bir tür (örneğin, dönüştürme sabit listesi değerleri dizeleri veritabanındaki gelen ve giden.) bir değer için bir tür değeri olabilir.</span><span class="sxs-lookup"><span data-stu-id="00b6a-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="00b6a-106">Temeller</span><span class="sxs-lookup"><span data-stu-id="00b6a-106">Fundamentals</span></span>

<span data-ttu-id="00b6a-107">Değer dönüştürücüler açısından belirtilen bir `ModelClrType` ve `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="00b6a-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="00b6a-108">Model türü varlık türü özelliği .NET türüdür.</span><span class="sxs-lookup"><span data-stu-id="00b6a-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="00b6a-109">Sağlayıcı türü, veritabanı sağlayıcısı tarafından anlaşılan .NET türüdür.</span><span class="sxs-lookup"><span data-stu-id="00b6a-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="00b6a-110">Örneğin, numaralandırmalar veritabanında dize olarak kaydetmek için model türü sabit listesi türüdür ve sağlayıcı türüdür `String`.</span><span class="sxs-lookup"><span data-stu-id="00b6a-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="00b6a-111">Bu iki tür aynı olabilir.</span><span class="sxs-lookup"><span data-stu-id="00b6a-111">These two types can be the same.</span></span>

<span data-ttu-id="00b6a-112">Dönüştürme, iki kullanılarak tanımlanır `Func` ifade ağaçları: birinden `ModelClrType` için `ProviderClrType` ve diğer `ProviderClrType` için `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="00b6a-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="00b6a-113">İfade ağaçları verimli dönüştürmeler için veritabanı erişim kodun içine derlenebilir için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="00b6a-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="00b6a-114">İfade ağacı, karmaşık dönüştürmeleri için basit bir dönüştürme uygulayan bir yöntem çağrısı olabilir.</span><span class="sxs-lookup"><span data-stu-id="00b6a-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="00b6a-115">Bir değer dönüştürücü yapılandırma</span><span class="sxs-lookup"><span data-stu-id="00b6a-115">Configuring a value converter</span></span>

<span data-ttu-id="00b6a-116">Değer dönüştürmeleri, DbContext, OnModelCreating özelliklerinde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="00b6a-116">Value conversions are defined on properties in the OnModelCreating of your DbContext.</span></span> <span data-ttu-id="00b6a-117">Örneğin, bir sabit listesi ve varlık türü olarak tanımlanmış göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="00b6a-117">For example, consider an enum and entity type defined as:</span></span>
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
<span data-ttu-id="00b6a-118">Ardından dönüştürmeleri OnModelCreating enum değerlerinden, dize (örneğin, "Donkey", "Mule",...) veritabanı olarak depolamak için tanımlanabilir:</span><span class="sxs-lookup"><span data-stu-id="00b6a-118">Then conversions can be defined in OnModelCreating to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>
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
> <span data-ttu-id="00b6a-119">A `null` değeri için bir değer dönüştürücü hiçbir zaman geçirilir.</span><span class="sxs-lookup"><span data-stu-id="00b6a-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="00b6a-120">Bu dönüştürmeler uygulamasını kolaylaştırır ve boş değer atanabilir ve NULL olmayan özellikler arasında paylaşılmasına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="00b6a-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="00b6a-121">ValueConverter sınıfı</span><span class="sxs-lookup"><span data-stu-id="00b6a-121">The ValueConverter class</span></span>

<span data-ttu-id="00b6a-122">Çağırma `HasConversion` oluşturacaktır yukarıda gösterildiği gibi bir `ValueConverter` örneği ve özelliği ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="00b6a-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="00b6a-123">`ValueConverter` Yerine açıkça oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="00b6a-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="00b6a-124">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="00b6a-124">For example:</span></span>
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="00b6a-125">Birden çok özellik aynı dönüştürme kullandığınızda bu yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="00b6a-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="00b6a-126">Şu anda tek bir yerde, her özellik, belirli bir türde aynı değer dönüştürücü kullanması gerektiğini belirtmek için bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="00b6a-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="00b6a-127">Bu özellik, gelecekteki sürümlerde sunulması kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="00b6a-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="00b6a-128">Yerleşik dönüştürücüler</span><span class="sxs-lookup"><span data-stu-id="00b6a-128">Built-in converters</span></span>

<span data-ttu-id="00b6a-129">EF Core yüklenebilen bir dizi önceden tanımlanmış `ValueConverter` bulunan sınıfları, `Microsoft.EntityFrameworkCore.Storage.ValueConversion` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="00b6a-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="00b6a-130">Bunlar:</span><span class="sxs-lookup"><span data-stu-id="00b6a-130">These are:</span></span>
* <span data-ttu-id="00b6a-131">`BoolToZeroOneConverter` -Sıfır ve Bool</span><span class="sxs-lookup"><span data-stu-id="00b6a-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="00b6a-132">`BoolToStringConverter` -"Y" ve "N" gibi dizelere Bool</span><span class="sxs-lookup"><span data-stu-id="00b6a-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="00b6a-133">`BoolToTwoValuesConverter` -Her iki değer için Bool</span><span class="sxs-lookup"><span data-stu-id="00b6a-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="00b6a-134">`BytesToStringConverter` -Base64 ile kodlanmış dizeye bayt dizisi</span><span class="sxs-lookup"><span data-stu-id="00b6a-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="00b6a-135">`CastingConverter` -Yalnızca bir Csharp tür dönüştürme gerektiren dönüştürmeler</span><span class="sxs-lookup"><span data-stu-id="00b6a-135">`CastingConverter` - Conversions that require only a Csharp cast</span></span>
* <span data-ttu-id="00b6a-136">`CharToStringConverter` -Tek bir karakter dizesindeki bir karakter</span><span class="sxs-lookup"><span data-stu-id="00b6a-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="00b6a-137">`DateTimeOffsetToBinaryConverter` -İkili Kodlanmış 64-bit değere DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="00b6a-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="00b6a-138">`DateTimeOffsetToBytesConverter` -Bayt dizisine DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="00b6a-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="00b6a-139">`DateTimeOffsetToStringConverter` -DateTimeOffset dizeye</span><span class="sxs-lookup"><span data-stu-id="00b6a-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="00b6a-140">`DateTimeToBinaryConverter` -64-bit değere DateTimeKind dahil olmak üzere tarih saat</span><span class="sxs-lookup"><span data-stu-id="00b6a-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="00b6a-141">`DateTimeToStringConverter` -Dizeye tarih saat</span><span class="sxs-lookup"><span data-stu-id="00b6a-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="00b6a-142">`DateTimeToTicksConverter` -Saat döngüsü için tarih saat</span><span class="sxs-lookup"><span data-stu-id="00b6a-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="00b6a-143">`EnumToNumberConverter` -Temel alınan sayı sabit listesi</span><span class="sxs-lookup"><span data-stu-id="00b6a-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="00b6a-144">`EnumToStringConverter` -Dize sabit listesi</span><span class="sxs-lookup"><span data-stu-id="00b6a-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="00b6a-145">`GuidToBytesConverter` -Bayt dizisine GUID</span><span class="sxs-lookup"><span data-stu-id="00b6a-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="00b6a-146">`GuidToStringConverter` -GUID dizesi</span><span class="sxs-lookup"><span data-stu-id="00b6a-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="00b6a-147">`NumberToBytesConverter` -Bayt dizisine herhangi bir sayısal değer</span><span class="sxs-lookup"><span data-stu-id="00b6a-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="00b6a-148">`NumberToStringConverter` -Herhangi bir sayısal değer dizesi</span><span class="sxs-lookup"><span data-stu-id="00b6a-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="00b6a-149">`StringToBytesConverter` -UTF8 baytı dize</span><span class="sxs-lookup"><span data-stu-id="00b6a-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="00b6a-150">`TimeSpanToStringConverter` -Dize zaman aralığı</span><span class="sxs-lookup"><span data-stu-id="00b6a-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="00b6a-151">`TimeSpanToTicksConverter` -TimeSpan işaretleri</span><span class="sxs-lookup"><span data-stu-id="00b6a-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="00b6a-152">Dikkat `EnumToStringConverter` bu listede bulunuyor.</span><span class="sxs-lookup"><span data-stu-id="00b6a-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="00b6a-153">Başka bir deyişle, dönüştürme açıkça, yukarıda gösterildiği gibi belirtmek için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="00b6a-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="00b6a-154">Bunun yerine, yalnızca yerleşik dönüştürücü kullanın:</span><span class="sxs-lookup"><span data-stu-id="00b6a-154">Instead, just use the built-in converter:</span></span>
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="00b6a-155">Tüm yerleşik dönüştürücüler durum bilgisiz olduğundan ve bu nedenle tek bir örnek güvenli bir şekilde birden çok özellik tarafından paylaşılabilir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="00b6a-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="00b6a-156">Önceden tanımlı dönüştürmeler</span><span class="sxs-lookup"><span data-stu-id="00b6a-156">Pre-defined conversions</span></span>

<span data-ttu-id="00b6a-157">Yerleşik bir dönüştürücü bulunduğu ortak dönüştürmeleri için dönüştürücü açıkça belirtmek için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="00b6a-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="00b6a-158">Bunun yerine, hangi sağlayıcı türü kullanılmalıdır yapılandırılması ve EF uygun yapı dönüştürücü otomatik olarak kullanacak.</span><span class="sxs-lookup"><span data-stu-id="00b6a-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate build-in converter.</span></span> <span data-ttu-id="00b6a-159">Enum dize dönüştürme için yukarıdaki örnek olarak kullanılır, ancak bu sağlayıcı türü yapılandırılmışsa EF aslında bu otomatik olarak yapar:</span><span class="sxs-lookup"><span data-stu-id="00b6a-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="00b6a-160">Sütun türünü açıkça belirterek aynı şeyi elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="00b6a-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="00b6a-161">Örneğin, varlık türü tanımladıysanız gibi için:</span><span class="sxs-lookup"><span data-stu-id="00b6a-161">For example, if the entity type is defined like so:</span></span>
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="00b6a-162">Ardından enum değerlerinden, dize OnModelCreating içinde başka bir yapılandırma olmadan olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="00b6a-162">Then the enum values will be saved as strings in the database without any further configuration in OnModelCreating.</span></span>

## <a name="limitations"></a><span data-ttu-id="00b6a-163">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="00b6a-163">Limitations</span></span>

<span data-ttu-id="00b6a-164">Değer dönüştürmeyle sistemin bazı bilinen geçerli sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="00b6a-164">There are a few known current limitations of the value convertion system:</span></span>
* <span data-ttu-id="00b6a-165">Yukarıda belirtildiği gibi `null` dönüştürülemez.</span><span class="sxs-lookup"><span data-stu-id="00b6a-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="00b6a-166">Şu anda birden çok sütun veya tam tersi bir özelliğin dönüştürme yaymak için bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="00b6a-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="00b6a-167">Değer dönüştürmeleri kullanımını EF Core için SQL deyimleri Çevir yeteneğini etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="00b6a-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="00b6a-168">Böyle durumlarda, bir uyarı günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="00b6a-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="00b6a-169">Bu sınırlamalar kaldırılmasını gelecekteki sürümlerde sunulması kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="00b6a-169">Removal of these limitations is being considered for a future release.</span></span>
