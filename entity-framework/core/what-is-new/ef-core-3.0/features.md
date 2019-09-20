---
title: EF Core 3,0 ' deki yeni özellikler EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: d938f17daecd5031147951d0018602c5635de41d
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149101"
---
# <a name="new-features-included-in-ef-core-30"></a>EF Core 3,0 ' de bulunan yeni özellikler

Aşağıdaki listede EF Core 3,0 için planlanmış olan başlıca yeni özellikler yer almaktadır.

EF Core 3,0 önemli bir sürümdür ve ayrıca mevcut uygulamalarda olumsuz etkileri olabilecek API geliştirmeleri olan çok sayıda [Son değişiklik](xref:core/what-is-new/ef-core-3.0/breaking-changes)içerir.  

## <a name="linq-improvements"></a>LINQ geliştirmeleri 

LINQ, IntelliSense ve derleme zamanı tür denetimi sunmak için zengin tür bilgilerinin avantajlarından yararlanarak, seçtiğiniz dilden çıkmadan veritabanı sorguları yazmanızı sağlar.
Ancak LINQ, rastgele ifadeler (Yöntem çağrıları veya işlemler) içeren sınırsız sayıda karmaşık sorgu yazmanızı de sağlar.
Bu kombinasyonların yönetilmesi, LINQ sağlayıcıları için her zaman önemli bir zorluk olmuştur.
EF Core 3,0 ' de, LINQ uygulamamız SQL 'e daha fazla ifade çevirmeyi etkinleştirmek, daha fazla durumda verimli sorgular oluşturmak, verimsiz sorguların algılanmasını engellemek ve yeni sorguyu aşamalı olarak kolayca tanıtmasını sağlamak için yeniden yazılmıştır Mevcut uygulamalar ve veri sağlayıcılarının özellikleri ve performansı improvementswithout.

### <a name="client-evaluation"></a>İstemci değerlendirmesi

EF Core 3,0 ' deki ana tasarım değişikliğinin SQL veya parametrelere çevrilemez LINQ ifadelerini nasıl işleyeceğinden ilgili olması vardır:

İlk birkaç sürümde, bir sorgunun yalnızca bir kısmının SQL 'e çevrilebilmesinin ve istemcinin geri kalanının yürütüldüğü EF Core.
Bu tür istemci tarafı yürütme bazı durumlarda istenebilir, ancak çoğu durumda verimsiz sorgulara yol açabilir.
Örneğin, EF Core 2,2 bir `Where()` çağrı içindeki bir koşulu çeviremez, filtre olmadan bir SQL ifadesini yürütür, veritabanındaki tüm satırları okuyabilir ve sonra bunları bellek içinde filtreledi.
Bu, veritabanı az sayıda satır içeriyorsa kabul edilebilir, ancak veritabanı büyük bir sayı veya satır içeriyorsa önemli performans sorunlarına veya hatta uygulama hatasına neden olabilir.
EF Core 3,0 ' de, istemci değerlendirmesinden yalnızca en üst düzey projeksiyonde (son yapılan `Select()`çağrı) yönelik olduğunu sınırlandırdık.
EF Core 3,0, sorguda başka herhangi bir yere çevrilemeyen ifadeler algıladığında, çalışma zamanı özel durumu oluşturur.

## <a name="cosmos-db-support"></a>Cosmos DB desteği 

EF Core için Cosmos DB sağlayıcısı, EF programlama modeliyle tanıdık geliştiricilerin, uygulama veritabanı olarak Azure Cosmos DB kolayca hedeflemesini sağlar.
Amaç, küresel dağıtım, "her zaman açık" kullanılabilirlik, elastik ölçeklenebilirlik ve düşük gecikme süresi gibi Cosmos DB avantajlarından bazılarını, hatta .NET geliştiricilerine daha erişilebilir hale getirmek için kullanılır.
Sağlayıcı, Cosmos DB içindeki SQL API 'sine karşı otomatik değişiklik izleme, LINQ ve değer dönüştürmeleri gibi EF Core özelliklerinin çoğunu etkinleştirir.

## <a name="c-80-support"></a>C#8,0 desteği

EF Core 3,0, 8,0 ' deki C# bazı yeni özelliklerden yararlanır:

### <a name="asynchronous-streams"></a>Zaman uyumsuz akışlar

Zaman uyumsuz sorgu sonuçları artık yeni standart `IAsyncEnumerable<T>` arabirim kullanılarak kullanıma sunulmuştur ve kullanılarak `await foreach`tüketilebilir.

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Process(o);
} 
```

### <a name="nullable-reference-types"></a>Boş değer atanabilir başvuru türleri 

Kodunuzda bu yeni özellik etkinleştirildiğinde EF Core, veritabanındaki sütunların ve ilişkilerin null olma durumunu belirlemek için refrence türlerinin (dize veya gezinti özellikleri gibi basit türlerden biri) null olma koşullarına neden olabilir.

## <a name="interception"></a>Haldedir

EF Core 3,0 ' deki yeni yakama API 'SI, bağlantı açma, işlemleri başlatma ve komutları yürütme gibi normal EF Core işleminin bir parçası olarak oluşan düşük düzeyli veritabanı işlemlerinin bir parçası olarak ortaya çıkan, program aracılığıyla gözlemlenmesini ve değiştirilmesini sağlar. 

## <a name="reverse-engineering-of-database-views"></a>Veritabanı görünümlerinin tersine mühendislik

Anahtarsız varlık türleri (daha önce [sorgu türleri](xref:core/modeling/keyless-entity-types)olarak bilinirdi), veritabanından okunabilecek verileri temsil eder, ancak güncelleştirilemez.
Bu özellik, Çoğu senaryoda veritabanı görünümlerini eşlemek için mükemmel bir uyum sağlar. bu nedenle, ters mühendislik veritabanı görünümlerinde, anahtar olmadan varlık türlerinin oluşturulmasını otomatik hale aldık.

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

Mevcut birçok uygulamanın önceki EF sürümlerini kullandığını anladık ve yalnızca .NET Core 'un avantajlarından yararlanmak için EF Core 'e taşıma işlemleri bazen önemli bir çaba gerektirebilir.
Bu nedenle, EF 6 ' nın newewst sürümünü .NET Core 3,0 ' de çalışacak şekilde etkinleştirdik.
Bazı sınırlamalar vardır, örneğin:
- .NET Core üzerinde çalışması için yeni sağlayıcılar gerekir
- SQL Server ile uzamsal destek etkinleştirilmeyecek

## <a name="postponed-features"></a>Ertelenmiş Özellikler

EF Core 3,0 için başlangıçta planlanmış bazı özellikler gelecek sürümlere ertelendi: 

- Geçiş sırasında bir modelin parçalarını alma özelliği, [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725)olarak izlenir.
- İki ayrı sorun olarak izlenen özellik paketi varlıkları: paylaşılan türdeki varlıklar hakkında [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) ve dizinli özellik eşleme desteği hakkında [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) .
