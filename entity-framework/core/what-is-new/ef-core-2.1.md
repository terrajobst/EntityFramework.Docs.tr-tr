---
title: EF Core 2.1'deki yenilikler - EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: ba3a26bcd76cd0b9615b13f32456e7280afe533a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417484"
---
# <a name="new-features-in-ef-core-21"></a>EF Core 2.1'deki yeni özellikler

Çok sayıda hata düzeltmeleri ve küçük fonksiyonel ve performans geliştirmeleri yanı sıra, EF Core 2.1 bazı zorlayıcı yeni özellikler içerir:

## <a name="lazy-loading"></a>Tembel yükleme

EF Core artık, navigasyon özelliklerini isteğe bağlı olarak yükleyebilen varlık sınıflarını yazar herkes için gerekli yapı taşlarını içerir. Ayrıca, en az modifiye edilmiş varlık sınıflarına (örneğin, sanal gezinme özelliklerine sahip sınıflar) dayalı tembel yükleme proxy sınıfları üretmek için bu yapı taşlarından yararlanan microsoft.entityframeworkcore.Proxies( microsoft.entityframeworkcore.Proxies) olarak da yeni bir paket oluşturduk.

Bu konu hakkında daha fazla bilgi için [tembel yükleme bölümünü](xref:core/querying/related-data#lazy-loading) okuyun.

## <a name="parameters-in-entity-constructors"></a>Varlık yapıcılarında parametreler

Tembel yükleme için gerekli yapı taşlarından biri olarak, kendi yapıcıları parametreleri almak varlıkların oluşturulmasını sağladı. Özellik değerlerini, tembel yükleme temsilcilerini ve hizmetleri enjekte etmek için parametreleri kullanabilirsiniz.

Bu konu hakkında daha fazla bilgi için [parametreleri ile varlık oluşturucu bölümü](xref:core/modeling/constructors) okuyun.

## <a name="value-conversions"></a>Değer dönüşümleri

Şimdiye kadar, EF Core yalnızca temel veritabanı sağlayıcısı tarafından desteklenen türlerin özelliklerini eşlenebiliyordu. Değerler, sütunlar ve özellikler arasında herhangi bir dönüşüm olmaksızın ileri geri kopyalandı. EF Core 2.1 ile başlayarak, sütunlardan elde edilen değerleri özelliklere uygulanmadan önce dönüştürmek için değer dönüştürmeleri uygulanabilir ve bunun tersi de geçerlidir. Gerektiğinde kuralla uygulanabilen bir dizi dönüşüme ve sütunlar ve özellikler arasında özel dönüşümlerin kaydedilmesine olanak tanıyan açık bir yapılandırma API'miz vardır. Bu özelliğin bazı uygulamaları şunlardır:

- Enumları dizeler olarak depolama
- SQL Server ile imzasız tümsavarları eşleme
- Özellik değerlerinin otomatik şifrelemesi ve şifreçözmesi

Bu konu hakkında daha fazla bilgi için [değer dönüşümleri bölümünü](xref:core/modeling/value-conversions) okuyun.  

## <a name="linq-groupby-translation"></a>LINQ GroupBy çevirisi

Sürüm 2.1'den önce, EF Core'da GroupBy LINQ işleci her zaman bellekte değerlendirilir. Artık çoğu durumda SQL GROUP BY yan tümcesine çevrilmesini destekliyoruz.

Bu örnek, çeşitli toplu işlevleri hesaplamak için groupby ile kullanılan bir sorgu gösterir:

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => o.Amount)
        });
```

İlgili SQL çevirisi aşağıdaki gibi görünür:

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a>Veri Çekirdeği Oluşturma

Yeni sürümle birlikte, bir veritabanıdoldurmak için ilk verileri sağlamak mümkün olacaktır. EF6'dan farklı olarak, tohumlama verileri model yapılandırmasının bir parçası olarak bir varlık türüyle ilişkilidir. Daha sonra EF Core geçişleri, veritabanını modelin yeni bir sürümüne yükseltirken hangi ekleme, güncelleştirme veya silme işlemlerinin uygulanması gerektiğini otomatik olarak hesaplayabilir.

Örnek olarak, bunu bir Gönderi için tohum verilerini `OnModelCreating`yapılandırmak için kullanabilirsiniz:

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

Bu konu hakkında daha fazla bilgi için [veri tohumlama bölümünü](xref:core/modeling/data-seeding) okuyun.  

## <a name="query-types"></a>Sorgu türleri

BIR EF Core modeli artık sorgu türlerini içerebilir. Varlık türlerinin aksine, sorgu türlerinde anahtarlar tanımlanmaz ve eklenemez, silinemez veya güncelleştirilemez (diğer bir deyişle, bunlar salt okunur) ancak doğrudan sorgularla döndürülebilir. Sorgu türleri için kullanım senaryolarından bazıları şunlardır:

- Birincil anahtarlar olmadan görünümlere eşleme
- Birincil anahtarlar olmadan tablolara eşleme
- Modelde tanımlanan sorgularla eşleme
- Sorgular için `FromSql()` iade türü olarak hizmet

Bu konu hakkında daha fazla bilgi için [sorgu türleri hakkındaki bölümü](xref:core/modeling/keyless-entity-types) okuyun.

## <a name="include-for-derived-types"></a>Türemiş türler için ekleme

Yöntem için `Include` ifadeler yazarken yalnızca türetilmiş türlerde tanımlanan gezinti özelliklerini belirtmek artık mümkün olacaktır. Güçlü bir şekilde yazılan `Include`sürümü için, açık bir döküm `as` veya işleç kullanarak destek. Ayrıca şimdi dize sürümünde türemiş türleri üzerinde tanımlanan gezinti `Include`özelliği adları başvurudestek:

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Bu konu hakkında daha fazla bilgi için [türemiş türleri ile Ekle bölümünü](xref:core/querying/related-data#include-on-derived-types) okuyun.

## <a name="systemtransactions-support"></a>System.Transactions desteği

TransactionScope gibi System.Transactions özellikleriyle çalışma özelliğini ekledik. Bu, destekleyen veritabanı sağlayıcılarını kullanırken hem .NET Framework hem de .NET Core üzerinde çalışır.

Bu konu hakkında daha fazla bilgi için [System.Transactions bölümünü](xref:core/saving/transactions#using-systemtransactions) okuyun.

## <a name="better-column-ordering-in-initial-migration"></a>İlk geçişte daha iyi sütun sırası

Müşteri geri bildirimlerine dayanarak, geçişleri, sınıflarda özellikler beyan edildiği sırada tablolar için sütunoluşturmak üzere güncelledik. Ef Core'un ilk tablo oluşturulduktan sonra yeni üyeler eklendiğinde düzeni değiştiremeyeceğini unutmayın.

## <a name="optimization-of-correlated-subqueries"></a>İlişkili alt sorguların optimizasyonu

Projeksiyonda bir gezinti özelliğinin kullanımının, ilişkili bir alt sorgudaki verilerle kök sorgusundan veri birletmeye yol açtığı birçok yaygın senaryoda "N + 1" SQL sorgularının yürütülmesini önlemek için sorgu çevirimizi geliştirdik. En iyi duruma getirilmesi alt sorgusonuçları arabelleğe gerektirir ve biz yeni davranışı devre dışı bırakmak için sorgu değiştirmenizi gerektirir.

Örnek olarak, aşağıdaki sorgu normalde Müşteriler için bir sorguya, artı N ("N" döndürülen müşteri sayısı) Siparişler için ayrı sorgular olarak çevrilmiş olur:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

Doğru yere `ToList()` ekleyerek, arabelleğe almanın Siparişler için uygun olduğunu ve bu sayede optimizasyonu sağlarsınız:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Bu sorgunun yalnızca iki SQL sorgusuna çevrileceğini unutmayın: Biri Müşteriler için, diğeri siparişler için.

## <a name="owned-attribute"></a>[Sahipolunan] öznitelik

Artık yalnızca türü açıklamaek `[Owned]` ve sonra sahibi varlığın modele eklenmesini sağlayarak sahip [olunan varlık türlerini](xref:core/modeling/owned-entities) yapılandırmak mümkündür:

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a>.NET Core SDK'da yer alan komut satırı aracı dotnet-ef

_Dotnet-ef_ komutları artık .NET Core SDK'nın bir parçasıdır, bu nedenle artık geçişleri kullanabilmek veya varolan bir veritabanından Bir DbContext'ı iskelelemek için projede DotNetCliToolReference'ı kullanmak gerekli olmayacaktır.

.NET Core SDK ve EF Core'un farklı sürümleri için komut satırı araçlarının nasıl etkinleştirilen hakkında daha fazla bilgi için [araçları yükleme](xref:core/miscellaneous/cli/dotnet#installing-the-tools) bölümüne bakın.

## <a name="microsoftentityframeworkcoreabstractions-package"></a>Microsoft.EntityFrameworkCore.Abstractions paketi

Yeni paket, EF Core'a bir bütün olarak bağımlılık yapmadan EF Core özelliklerini aydınlatmak için projelerinizde kullanabileceğiniz öznitelikler ve arabirimler içerir. Örneğin, [Owned] özniteliği ve ILazyLoader arabirimi burada bulunur.

## <a name="state-change-events"></a>Durum değişikliği olayları

Yeni `Tracked` `StateChanged` Ve `ChangeTracker` olaylar DbContext giren veya durum larını değiştiren varlıklar tepki mantık yazmak için kullanılabilir.

## <a name="raw-sql-parameter-analyzer"></a>Ham SQL parametre çözümleyicisi

EF Core'a yeni bir kod çözümleyicisi dahildir ve raw-SQL API'lerimizin `FromSql` `ExecuteSqlCommand`güvenli olmayan kullanımlarını algılar. Örneğin, aşağıdaki sorgu için, _minAge_ parametreli olmadığından bir uyarı görürsünüz:

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a>Veritabanı sağlayıcısı uyumluluğu

EF Core 2.1'i güncelleştirilmiş veya en azından EF Core 2.1 ile çalışmak üzere test edilmiş sağlayıcılarla kullanmanız önerilir.

> [!TIP]
> Yeni özelliklerde beklenmeyen bir uyumsuzluk veya herhangi bir sorun bulursanız veya bunlar hakkında geri bildirimde bulunduysanız, lütfen [sorun izleyicimizi](https://github.com/aspnet/EntityFrameworkCore/issues/new)kullanarak bildirin.
