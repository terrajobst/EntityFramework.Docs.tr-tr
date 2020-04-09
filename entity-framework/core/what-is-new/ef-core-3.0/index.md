---
title: Entity Framework Core 3.0'daki yeni özellikler - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/index
ms.openlocfilehash: ebc676930ffc396aa70bb8afb91cf5a0cd43e04d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417944"
---
# <a name="new-features-in-entity-framework-core-30"></a>Entity Framework Core 3.0'daki yeni özellikler

Aşağıdaki liste, EF Core 3.0'daki başlıca yeni özellikleri içerir.

Önemli bir sürüm olarak, EF Core 3.0 da varolan uygulamalar üzerinde olumsuz etkisi olabilir API iyileştirmeleri olan birkaç [kesme değişiklikleri](xref:core/what-is-new/ef-core-3.0/breaking-changes)içerir.  

## <a name="linq-overhaul"></a>LINQ revizyonu

LINQ, intelliSense sunmak ve derleme zamanı türü denetimi sunmak için zengin tür bilgilerinden yararlanarak seçtiğiniz .NET dilini kullanarak veritabanı sorguları yazmanızı sağlar.
Ancak LINQ, rasgele ifadeler (yöntem çağrıları veya işlemler) içeren sınırsız sayıda karmaşık sorgu yazmanızı da sağlar.
Tüm bu kombinasyonların nasıl işleyeceğinilinq sağlayıcıları için en büyük zorluk.

EF Core 3.0'da, DAHA fazla sorgu deseninin SQL'e çevrilmesini, daha fazla durumda verimli sorgular oluşturmasını ve verimsiz sorguların algılanmamasını önlemek için LINQ sağlayıcımızı yeniden yeniden biçimlendirdik. Yeni LINQ sağlayıcısı, mevcut uygulamaları ve veri sağlayıcılarını bozmadan gelecek sürümlerde yeni sorgu yetenekleri ve performans iyileştirmeleri sunabileceğimiz temeldir.

### <a name="restricted-client-evaluation"></a>Kısıtlı istemci değerlendirmesi

En önemli tasarım değişikliği, parametrelere dönüştürülemeyen veya SQL'e çevrilemeyen LINQ ifadelerini nasıl işlediğimizle ilgilidir.

Önceki sürümlerde, EF Core sorgunun hangi bölümlerinin SQL'e çevrilebileceğini belirlemiş ve sorgunun geri kalanını istemciüzerinde yürütmüştür.
İstemci tarafı yürütme bu tür bazı durumlarda arzu edilir, ancak diğer birçok durumda verimsiz sorguları neden olabilir.

Örneğin, EF Core 2.2 bir `Where()` çağrıda bir yüklemi çeviremiyorsa, filtresiz bir SQL deyimi yürüttü, tüm satırları veritabanından aktardı ve sonra bunları bellekte filtreledi:

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

Veritabanı az sayıda satır içeriyorsa, ancak veritabanı çok sayıda satır içeriyorsa önemli performans sorunlarına ve hatta uygulama hatasına neden olabilir.

EF Core 3.0'da, istemci değerlendirmesini yalnızca üst düzey projeksiyonda (aslında son `Select()`çağrı) gerçekleşmesi için kısıtladık.
EF Core 3.0 sorguda başka hiçbir yerde çevrilemez ifadeler algılar, bir çalışma zamanı özel durum atar.

Önceki örnekte olduğu gibi istemci üzerinde bir yüklem koşulunu değerlendirmek için, geliştiricilerin artık sorgunun değerlendirmesini linq'e nesnelere açıkça değiştirmeleri gerekir:

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n))
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

Bunun varolan uygulamaları nasıl etkileyebileceği hakkında daha fazla ayrıntı için [sondaki değişiklikler belgelerine](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) bakın.

### <a name="single-sql-statement-per-linq-query"></a>LINQ sorgusu başına tek SQL deyimi

3.0'da önemli ölçüde değişen tasarımın bir diğer yönü de, artık LINQ sorgusu başına her zaman tek bir SQL deyimi oluşturmamızdır. Önceki sürümlerde, belirli durumlarda birden çok SQL deyimi, koleksiyon gezinti özellikleri yle ilgili çevrilmiş `Include()` çağrılar ve alt sorgularla belirli desenleri izleyen çevrilmiş sorgular oluşturmak için kullanılırdık. Bu bazı durumlarda uygun olmasına `Include()` rağmen ve hatta tel üzerinden gereksiz veri göndermemek yardımcı oldu, uygulama karmaşık tı ve bazı son derece verimsiz davranışlar (N +1 sorguları) sonuçlandı. Birden çok sorguda döndürülen verilerin tutarsız olabileceği durumlar vardı.

İstemci değerlendirmesine benzer şekilde, EF Core 3.0 bir LINQ sorgusunu tek bir SQL deyimine çeviremezse, çalışma zamanı özel bir durum oluşturur. Ancak EF Core'u, birden çok sorguyu JO'larla tek bir sorguya oluşturmak için kullanılan ortak desenlerin çoğunu çevirebilme özelliğine sahip kıldık.

## <a name="cosmos-db-support"></a>Cosmos DB desteği

EF Core için Cosmos DB sağlayıcısı, EF programlama modeline aşina olan geliştiricilerin Azure Cosmos DB'yi uygulama veritabanı olarak kolayca hedeflemesini sağlar. Amaç, küresel dağıtım gibi Cosmos DB'nin bazı avantajlarını ,.her zaman açık, elastik ölçeklenebilirlik ve düşük gecikme süresini ,.NET geliştiricileri için daha da erişilebilir hale getirmektir. Sağlayıcı, Cosmos DB'deki SQL API'ye karşı otomatik değişiklik izleme, LINQ ve değer dönüşümleri gibi çoğu EF Core özelliğine olanak tanır.

Daha fazla ayrıntı için [Cosmos DB sağlayıcı belgelerine](xref:core/providers/cosmos/index) bakın.

## <a name="c-80-support"></a>C# 8.0 desteği

EF Core 3.0 C # [8.0 yeni özellikler](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8)bir çift yararlanır:

### <a name="asynchronous-streams"></a>Asenkron akarsular

Asynchronous sorgu sonuçları artık yeni `IAsyncEnumerable<T>` standart arabirim kullanılarak `await foreach`açıklanır ve .

``` csharp
var orders =
    from o in context.Orders
    where o.Status == OrderStatus.Pending
    select o;

await foreach(var o in orders.AsAsyncEnumerable())
{
    Process(o);
}
```

Daha fazla ayrıntı için [C# belgelerindeki eşzamanlı akışlara](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) bakın.

### <a name="nullable-reference-types"></a>Boş değer atanabilir başvuru türleri

Bu yeni özellik kodunuzda etkinleştirildiğinde, EF Core başvuru türü özelliklerinin nullable'ını inceler ve veritabanındaki ilgili sütunlara ve ilişkilere uygular: `[Required]` nullable olmayan başvuru türlerinin özellikleri, veri ek açıklama özelliğine sahipmiş gibi değerlendirilir.

Örneğin, aşağıdaki sınıfta, tür `string?` olarak işaretlenmiş özellikler isteğe bağlı olarak `string` yapılandırılırken, gerektiği gibi yapılandırılacaktır:

``` csharp
public class Customer
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string? MiddleName { get; set; }
}
```

Daha fazla ayrıntı için EF Core belgelerinde [geçersiz başvuru türleri ile çalışma](xref:core/miscellaneous/nullable-reference-types) hakkında bilgi verilir.

## <a name="interception-of-database-operations"></a>Veritabanı işlemlerinin engellenmesi

EF Core 3.0'daki yeni durdurma API'si, EF Core'un normal çalışmasının bir parçası olarak düşük düzeyli veritabanı işlemleri gerçekleştiğinde özel mantığın otomatik olarak çağrılmasını sağlar. Örneğin, bağlantıları açarken, hareketler işlerken veya komutları yürüterken.

EF 6'da bulunan durdurma özelliklerine benzer şekilde, durdurucular da operasyonlar gerçekleşmeden önce veya sonra durdurmanızı sağlar. Bunlar olmadan önce onları yakaladığınızda, yürütmeyi by-pass ve durdurma mantığından alternatif sonuçlar sağlamanıza izin verilir.

Örneğin, komut metnini işlemek için aşağıdakileri `IDbCommandInterceptor`oluşturabilirsiniz:

``` csharp
public class HintCommandInterceptor : DbCommandInterceptor
{
    public override InterceptionResult ReaderExecuting(
        DbCommand command,
        CommandEventData eventData,
        InterceptionResult result)
    {
        // Manipulate the command text, etc. here...
        command.CommandText += " OPTION (OPTIMIZE FOR UNKNOWN)";
        return result;
    }
}
```

Ve ile kaydedin: `DbContext`

``` csharp
services.AddDbContext(b => b
    .UseSqlServer(connectionString)
    .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a>Veritabanı görünümlerinin ters mühendislik

Veritabanından okunabilen ancak güncelleştirilmeyen verileri temsil eden sorgu türleri [anahtarsız varlık türlerine](xref:core/modeling/keyless-entity-types)yeniden adlandırıldı.
Çoğu senaryoda veritabanı görünümlerini eşleme için mükemmel bir uyum sağladıklarından, EF Core artık veritabanı görünümlerini tersine çevirdiğinde anahtarsız varlık türleri oluşturur.

Örneğin, [dotnet ef komut satırı aracını](xref:core/miscellaneous/cli/dotnet) kullanarak yazabilirsiniz:

```dotnetcli
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

Ve araç artık anahtarsız görünümler ve tablolar için otomatik olarak iskele tipleri olacaktır:

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Names>(entity =>
    {
        entity.HasNoKey();
        entity.ToView("Names");
    });

    modelBuilder.Entity<Things>(entity =>
    {
        entity.HasNoKey();
    });
}
```

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Tabloyu anaparayla paylaşan bağımlı varlıklar artık isteğe bağlıdır

EF Core 3.0 ile `OrderDetails` başlayarak, `Order` sahip olunan veya açıkça aynı tabloya eşlenmişse, birincil anahtar hariç, bir `Order` özellik olmadan ve `OrderDetails` tüm `OrderDetails` özellikleri niçin geçersiz sütunlara eşlenecektir.

Sorgulanırken, EF Core `OrderDetails` `null` gerekli özelliklerinden herhangi birinin bir değeri yoksa veya birincil anahtar dışında gerekli özellikleri yoksa `null`ve tüm özellikleri.

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a>EF 6.3 üzerinde .NET Core

Bu gerçekten bir EF Core 3.0 özelliği değil, ama biz mevcut müşterilerinin çoğu için önemli olduğunu düşünüyorum.

Varolan birçok uygulamanın EF'nin önceki sürümlerini kullandığını ve bunları yalnızca .NET Core'dan yararlanmak için EF Core'a taşımanın önemli bir çaba gerektirebileceğini biliyoruz.
Bu nedenle, .NET Core 3.0 üzerinde çalıştırmak için EF 6 en yeni sürümünü bağlantı noktasına karar verdi.

Daha fazla bilgi [için, EF 6'daki yeniliklere](xref:ef6/what-is-new/index)bakın.

## <a name="postponed-features"></a>Ertelenen özellikler

Başlangıçta EF Core 3.0 için planlanan bazı özellikler gelecek sürümlere ertelendi:

- [Geçişlerde](https://github.com/aspnet/EntityFrameworkCore/issues/2725)bir modelin bölümlerini yok sayma yeteneği, #2725 olarak izlenir.
- Mülkiyet çanta varlıklar, iki ayrı konu olarak izlenen: paylaşılan tür varlıklar hakkında [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) ve endeksli özellik haritalama desteği hakkında [#13610.](https://github.com/aspnet/EntityFrameworkCore/issues/13610)
