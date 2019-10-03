---
title: Entity Framework Core 3,0 ' deki yeni özellikler
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/index
ms.openlocfilehash: ce53d0fa21acfd52dc5e8b37911202cab02f00c8
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813472"
---
# <a name="new-features-in-entity-framework-core-30"></a>Entity Framework Core 3,0 ' deki yeni özellikler

Aşağıdaki listede EF Core 3,0 ' deki başlıca yeni özellikler yer almaktadır.

Büyük bir sürüm olarak EF Core 3,0, mevcut uygulamalar üzerinde olumsuz etkileri olabilecek API geliştirmeleri olan çok sayıda [Son değişiklik](xref:core/what-is-new/ef-core-3.0/breaking-changes)de içerir.  

## <a name="linq-overhaul"></a>LINQ fazla mesafe 

LINQ, IntelliSense ve derleme zamanı tür denetimi sunmak için zengin tür bilgilerinin avantajlarından yararlanarak tercih ettiğiniz .NET dilini kullanarak veritabanı sorguları yazmanızı sağlar.
Ancak LINQ, rastgele ifadeler (Yöntem çağrıları veya işlemler) içeren sınırsız sayıda karmaşık sorgu yazmanızı de sağlar.
Bu kombinasyonların nasıl işleneceği, LINQ sağlayıcıları için Ana zorluk dır.

EF Core 3,0 ' de, LINQ sağlayıcımız, daha fazla sorgu desenini SQL 'e çevirmeyi, daha fazla durumda verimli sorgular oluşturmayı ve verimsiz sorguların algılanışlanmasını engellemeyi sağlayan bir şekilde yeniden tasarlanmıştır. Yeni LINQ sağlayıcısı, mevcut uygulamaları ve veri sağlayıcılarını bozmadan gelecek sürümlerde yeni sorgu özellikleri ve performans iyileştirmeleri sunabilediğimiz temel bir temelidir.

### <a name="restricted-client-evaluation"></a>Kısıtlanmış istemci değerlendirmesi
En önemli tasarım değişikliği, parametrelere dönüştürülemeyen veya SQL 'e çevrilemeyen LINQ ifadelerini nasıl işleytiğimiz ile aynı olmalıdır.

Önceki sürümlerde, bir sorgunun hangi bölümlerinin SQL 'e çevrilebileceğini ve bu sorgunun geri kalanını istemcide yürütüldüğünü EF Core.
Bu tür istemci tarafı yürütme, bazı durumlarda istenebilir, ancak çoğu durumda verimsiz sorgulara yol açabilir.

Örneğin, EF Core 2,2 bir `Where()` çağrı içindeki bir koşulu çeviremediğinden, filtre olmadan bir SQL ifadesini yürütür, veritabanından tüm satırları aktardı ve sonra bunları bellek içinde filtreledi:

``` csharp
var specialCustomers = 
  context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

Bu, veritabanı az sayıda satır içeriyorsa ancak önemli performans sorunlarına yol açabilir, hatta veritabanı büyük bir sayı veya satır içeriyorsa uygulama başarısızlığından kaynaklanabilir.

EF Core 3,0 ' de, istemci değerlendirmesinin yalnızca en üst düzey projeksiyde (temelde, son çağrı `Select()`) gerçekleşmesini kısıtlarız.
EF Core 3,0, sorguda başka herhangi bir yere çevrilemeyen ifadeler algıladığında, çalışma zamanı özel durumu oluşturur.

Önceki örnekte olduğu gibi, istemci üzerindeki bir koşul koşulunu değerlendirmek için, geliştiricilerin artık sorgunun değerlendirmesini LINQ to Objects için açıkça geçiş yapması gerekir: 

``` csharp
var specialCustomers =
  context.Customers
    .Where(c => c.Name.StartsWith(n)) 
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

Bunun var olan uygulamaları nasıl etkileyebileceği hakkında daha fazla ayrıntı için bkz. [son değişiklikler belgeleri](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) .

### <a name="single-sql-statement-per-linq-query"></a>LINQ sorgusu başına tek SQL ekstresi

Tasarımın 3,0 ' de önemli ölçüde değiştiği başka bir yönü de her LINQ sorgusu için her zaman tek bir SQL ekstresi oluşturmamız. Önceki sürümlerde, bazı durumlarda birden çok SQL deyimi oluşturmak için kullanıldığımız gibi, koleksiyon gezinti özelliklerine `Include()` yapılan çağrıları çevirmek ve belirli desenleri izleyen sorguları alt sorgular halinde çevirmek gibi. Bu, bazı durumlarda kullanışlı olsa da, `Include()` hatta yedekli verileri kablo üzerinden göndermekten kaçınmaya yardımcı olsa da, uygulama karmaşıktır, son derece verimsiz davranışlar (N + 1 sorgu) ile sonuçlanır ve birden çok sorgu arasında döndürülen veriler tutarsız olabilir.

İstemci değerlendirmesine benzer şekilde, EF Core 3,0 bir LINQ sorgusunu tek bir SQL ifadesine çeviremiyorsa, çalışma zamanı özel durumu oluşturur. Ancak birleşimli tek bir sorgu için birden çok sorgu oluşturmak için kullanılan yaygın desenlerin çoğunu çevirdiğimiz EF Core yaptık.

## <a name="cosmos-db-support"></a>Cosmos DB desteği 

EF Core için Cosmos DB sağlayıcısı, EF programlama modeliyle tanıdık geliştiricilerin, uygulama veritabanı olarak Azure Cosmos DB kolayca hedeflemesini sağlar. Amaç, küresel dağıtım, "her zaman açık" kullanılabilirlik, elastik ölçeklenebilirlik ve düşük gecikme süresi gibi Cosmos DB avantajlarından bazılarını, hatta .NET geliştiricilerine daha erişilebilir hale getirmek için kullanılır. Sağlayıcı, Cosmos DB içindeki SQL API 'sine karşı otomatik değişiklik izleme, LINQ ve değer dönüştürmeleri gibi EF Core özelliklerinin çoğunu mümkün bir şekilde sunar.

Daha fazla bilgi için [Cosmos DB sağlayıcı belgelerine](xref:core/providers/cosmos/index) bakın.

## <a name="c-80-support"></a>C#8,0 desteği

EF Core 3,0, [8,0 ' deki C# yeni özelliklerden](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8)faydalanır:

### <a name="asynchronous-streams"></a>Zaman uyumsuz akışlar

Zaman uyumsuz sorgu sonuçları artık yeni standart `IAsyncEnumerable<T>` arabirim kullanılarak kullanıma sunulmuştur ve kullanılarak `await foreach`tüketilebilir.

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

Daha fazla bilgi için [ C# belgelerindeki zaman uyumsuz akışlara](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) bakın.

### <a name="nullable-reference-types"></a>Boş değer atanabilir başvuru türleri 

Kodunuzda bu yeni özellik etkinleştirildiğinde EF Core, başvuru türü özelliklerinin null değer alabilme durumunu inceler ve veritabanındaki karşılık gelen sütunlara ve ilişkilere uygular: null yapılamayan başvurular türlerinin özellikleri, `[Required]` veri ek açıklaması özniteliği.

Örneğin, aşağıdaki sınıfta, türü `string?` olarak işaretlenen özellikler isteğe bağlı olarak yapılandırılır, ancak `string` gereken şekilde yapılandırılır:

``` csharp
public class Customer
{
  public int Id { get; set; }
  public string FirstName { get; set; }
  public string LastName { get; set; }
  public string? MiddleName { get; set; }
}
```

Daha fazla bilgi için EF Core belgelerinde [null yapılabilir başvuru türleriyle çalışma](xref:core/miscellaneous/nullable-reference-types) konusuna bakın.

## <a name="interception-of-database-operations"></a>Veritabanı işlemlerinin yakaçıkarılması

EF Core 3,0 ' deki yeni yakama API 'SI, EF Core normal işleminin bir parçası olarak düşük düzey veritabanı işlemleri gerçekleşdiğinde otomatik olarak çağrılması için özel mantık sağlamaya olanak sağlar. Örneğin, bağlantılar açılırken, işlemler uygulanırken veya komutları yürütürken. 

EF 6 ' da var olan ele geçirme özelliklerine benzer şekilde, kırıcılar, işlemleri gerçekleşmeden önce veya sonra ele aktarmanıza olanak tanır. Bunları gerçekleşmeden önce zaman geçitirsiniz, yürütmeye göre yürütme ve yedek mantığdan alternatif sonuçlar sağlama izni verilir. 

Örneğin, komut metnini işlemek için şunu oluşturabilirsiniz `IDbCommandInterceptor`:

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

Ve şu şekilde kaydolun `DbContext`:

``` csharp
services.AddDbContext(b => b
  .UseSqlServer(connectionString)
  .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a>Veritabanı görünümlerinin tersine mühendislik

Veritabanından okunabilecek ancak güncelleştirilmemiş verileri temsil eden sorgu türleri, [anahtarsız varlık türleri](xref:core/modeling/keyless-entity-types)olarak yeniden adlandırıldı. Çoğu senaryoda veritabanı görünümlerini eşlemek için mükemmel bir uyum olduğundan, EF Core artık tersine mühendislik veritabanı görünümlerinde otomatik olarak anahtarsız varlık türleri oluşturur.

Örneğin, [DotNet EF komut satırı aracını](xref:core/miscellaneous/cli/dotnet) kullanarak şunu yazabilirsiniz:

``` console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

Ve araç artık, anahtarlar olmadan görünümler ve tablolar için fkatlama türlerini otomatik olarak dolandırıcılardır:

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

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır

EF Core 3,0 ' den başlayarak, `OrderDetails` aynı tabloya `Order` ait veya açıkça eşlenmiş ise, birincil anahtar ile eşleştirilecektir, ancak tüm özellikler olmadan `Order` `OrderDetails` bir eklenebilir. `OrderDetails` null yapılabilir sütunlar.

Sorgulama yaparken, gerekli özelliklerinden herhangi `OrderDetails` birinin `null` bir değeri yoksa veya birincil `null`anahtar ve tüm Özellikler ' in yanı sıra gerekli özellikleri yoksa EF Core olarak ayarlanır.

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

## <a name="ef-63-on-net-core"></a>.NET Core üzerinde EF 6,3

Bu aslında EF Core 3,0 özelliği değildir, ancak geçerli müşterilerimizin birçoğu için önemli olduğunu düşündük. 

Mevcut birçok uygulamanın daha önceki EF sürümlerini kullandığını ve yalnızca .NET Core 'un avantajlarından yararlanmak için EF Core aktarmak için önemli bir çaba gerektirebilir. Bu nedenle, .NET Core 3,0 ' de çalıştırmak için en yeni EF 6 sürümü bağlantı noktasına karar verdik. 

Daha ayrıntılı bilgi için bkz. [EF 6 ' daki](xref:ef6/what-is-new/index)yenilikler.

## <a name="postponed-features"></a>Ertelenmiş Özellikler

EF Core 3,0 için başlangıçta planlanmış bazı özellikler gelecek sürümlere ertelendi:

- [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725)olarak izlenen, geçişler içindeki bir modelin parçalarını yok saybilme özelliği.
- İki ayrı sorun olarak izlenen özellik paketi varlıkları: paylaşılan türdeki varlıklar hakkında [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) ve dizinli özellik eşleme desteği hakkında [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) .
