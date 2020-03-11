---
title: EF Core 2,1 ' deki yenilikler-EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: ba3a26bcd76cd0b9615b13f32456e7280afe533a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417484"
---
# <a name="new-features-in-ef-core-21"></a>EF Core 2,1 ' deki yeni özellikler

Çok sayıda hata düzeltmesi ve küçük işlevsellik ve performans geliştirmelerinin yanı sıra EF Core 2,1 bazı etkileyici yeni özellikler içerir:

## <a name="lazy-loading"></a>geç yükleme

EF Core artık, herhangi bir kişinin gezinti özelliklerini isteğe bağlı olarak yükleyebilen varlık sınıfları yazmak için gerekli yapı taşlarını içerir. Ayrıca, en düşük düzeyde değiştirilen varlık sınıflarına (örneğin, sanal gezinti özelliklerine sahip sınıflar) göre yavaş yükleme proxy sınıfları oluşturmak için bu yapı taşlarından yararlanan yeni bir Microsoft. EntityFrameworkCore. proxy paketi oluşturduk.

Bu konu hakkında daha fazla bilgi için, [yavaş yükleme bölümündeki bölümü](xref:core/querying/related-data#lazy-loading) okuyun.

## <a name="parameters-in-entity-constructors"></a>Varlık oluşturuculardaki parametreler

Yavaş yükleme için gereken yapı taşlarından biri olarak, kurucularında parametre alan varlıkların oluşturulmasını etkinleştirdik. Özellik değerlerini eklemek, yavaş yükleme temsilcileri ve hizmetleri eklemek için parametreleri kullanabilirsiniz.

Bu konu hakkında daha fazla bilgi için, [parametrelere sahip varlık Oluşturucu bölümünü](xref:core/modeling/constructors) okuyun.

## <a name="value-conversions"></a>Değer dönüştürmeleri

Artık EF Core, yalnızca temel alınan veritabanı sağlayıcısı tarafından desteklenen türlerin özelliklerini eşleyebilir. Değerler, herhangi bir dönüştürme olmadan sütunlar ve özellikler arasında geri ve ileri kopyalandı. EF Core 2,1 ' den başlayarak, değerlere uygulanmadan önce sütunlardan elde edilen değerleri dönüştürmek için değer dönüştürmeleri uygulanabilir ve tam tersi de geçerlidir. Kural ve özellikler arasında özel dönüştürmeleri kaydetmeye izin veren bir açık yapılandırma API 'SI de olmak üzere kurala göre uygulanabilen birkaç dönüştürmemiz vardır. Bu özelliğin bazı uygulamaları şunlardır:

- Numaralandırmaların dizeler olarak depolanması
- İşaretsiz tamsayılar SQL Server ile eşleme
- Özellik değerlerinin otomatik şifrelenmesi ve şifresinin çözülmesi

Bu konu hakkında daha fazla bilgi için [değer dönüştürmeleriyle ilgili bölümü](xref:core/modeling/value-conversions) okuyun.  

## <a name="linq-groupby-translation"></a>LINQ GroupBy çevirisi

Sürüm 2,1 ' den önce, EF Core GroupBy LINQ operatörü her zaman bellekte değerlendirilir. Artık en yaygın durumlarda bunu SQL GROUP BY yan tümcesine çevirmeyi destekliyoruz.

Bu örnek, çeşitli toplama işlevlerini hesaplamak için kullanılan GroupBy ile bir sorgu gösterir:

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

Karşılık gelen SQL çevirisi şuna benzer:

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a>Veri Çekirdeği Oluşturma

Yeni sürüm ile bir veritabanını doldurmak için ilk verileri sağlamak mümkün olacaktır. EF6 'in aksine, dengeli dağıtım verileri model yapılandırmasının bir parçası olarak bir varlık türü ile ilişkilendirilir. Daha sonra EF Core geçişler, veritabanının yeni bir sürümüne yükseltilirken ekleme, güncelleştirme veya silme işlemlerinin uygulanması gerektiğini otomatik olarak hesaplar.

Örnek olarak bunu, `OnModelCreating`bir gönderi için çekirdek verileri yapılandırmak üzere kullanabilirsiniz:

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

Bu konu hakkında daha fazla bilgi için [veri dengeli dağıtım bölümünü](xref:core/modeling/data-seeding) okuyun.  

## <a name="query-types"></a>Sorgu türleri

EF Core modeli artık sorgu türlerini içerebilir. Varlık türlerinden farklı olarak, sorgu türlerinde tanımlanmış anahtarlar yoktur ve eklenemez, silinemez veya güncelleştirilemez (yani, salt okunurdur), ancak sorgular tarafından doğrudan döndürülebilecek. Sorgu türleri için kullanım senaryolarından bazıları şunlardır:

- Birincil anahtarlar olmadan görünümlere eşleme
- Birincil anahtarlar olmadan tablolarla eşleme
- Modelde tanımlanan sorgularla eşleme
- `FromSql()` sorguları için dönüş türü olarak sunma

Bu konu hakkında daha fazla bilgi için [sorgu türleri bölümünü](xref:core/modeling/keyless-entity-types) okuyun.

## <a name="include-for-derived-types"></a>Türetilmiş türler için bulundur

Artık `Include` yöntemi için ifadeler yazılırken türetilmiş türlerde tanımlanan gezinti özelliklerini belirtmek mümkün olacaktır. `Include`türü kesin belirlenmiş olan sürümü için, açık bir dönüştürme veya `as` işleci kullanmayı destekliyoruz. Ayrıca artık `Include`dize sürümündeki türetilmiş türlerde tanımlanan gezinti özelliği adlarına başvurmayı de destekliyoruz:

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Bu konu hakkında daha fazla bilgi için [türetilmiş türlerle birlikte ekle bölümündeki bölümü](xref:core/querying/related-data#include-on-derived-types) okuyun.

## <a name="systemtransactions-support"></a>System. Transactions desteği

TransactionScope gibi System. Transactions özellikleriyle çalışma yeteneği ekledik. Bu, bunu destekleyen veritabanı sağlayıcıları kullanılırken hem .NET Framework hem de .NET Core üzerinde çalışır.

Bu konu hakkında daha fazla bilgi için [System. Transactions bölümündeki bölümü](xref:core/saving/transactions#using-systemtransactions) okuyun.

## <a name="better-column-ordering-in-initial-migration"></a>İlk geçişte daha iyi sütun sıralaması

Müşteri geri bildirimlerine bağlı olarak, Özellikler sınıflarda bildirildiği sırada tablolar için ilk olarak sütunlar oluşturmak üzere geçişleri güncelleştirdik. İlk tablo oluşturulduktan sonra yeni üyeler eklendiğinde EF Core sırayı değiştiremediğini unutmayın.

## <a name="optimization-of-correlated-subqueries"></a>Bağıntılı alt sorgular iyileştirmesi

"N + 1" SQL sorgularını, projeksiyondaki bir gezinti özelliğinin kullanımının, kök sorgudan bağıntılı bir alt sorgudan verilerle katılmasını sağlayan çok sayıda yaygın senaryoda yürütmemek için geliştirdik. İyileştirme, sorgunun sonuçlarını arabelleğe almayı gerektirir ve yeni davranışı kabul etmek için sorguyu değiştirmenizi gerektirir.

Örnek olarak, aşağıdaki sorgu normal olarak müşteriler için tek bir sorguya çevrilir, ek olarak N ("N" döndürülen müşterilerin sayısı) siparişlerin ayrı sorgularını alır:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

`ToList()` doğru yere ekleyerek, arabelleğe alma işleminin, en iyi duruma getirme olanağı sağlayan siparişlere uygun olduğunu belirtirsiniz:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Bu sorgunun yalnızca iki SQL sorgusuna çevrildiğine ve siparişler için bir diğeri müşterilere yönelik bir sonrakine sahip olduğunu unutmayın.

## <a name="owned-attribute"></a>[Sahipli] özniteliği

Artık türü `[Owned]` ve ardından sahip varlığın modele eklendiğinden emin olmak için, sahip olan [varlık türlerini](xref:core/modeling/owned-entities) yapılandırmak mümkün olacaktır:

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

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a>Komut satırı aracı DotNet-.NET Core SDK içinde yer alan EF

_DotNet-EF_ komutları artık .NET Core SDK bir parçasıdır. bu nedenle, geçişleri kullanabilmeniz veya var olan bir veritabanından bir DbContext 'i kullanmak için artık projede Dotnetclientoolreference kullanılması gerekli olmayacaktır.

Farklı .NET Core SDK ve EF Core sürümleri için komut satırı araçlarının nasıl etkinleştirileceği hakkında daha fazla bilgi için [araçları yükleme](xref:core/miscellaneous/cli/dotnet#installing-the-tools) bölümüne bakın.

## <a name="microsoftentityframeworkcoreabstractions-package"></a>Microsoft. EntityFrameworkCore. soyutlamalar paketi

Yeni paket, bir bütün olarak EF Core bağımlılığı almadan EF Core özellikleri açmak için projelerinizde kullanabileceğiniz öznitelikleri ve arabirimleri içerir. Örneğin, [sahip] özniteliği ve ılazyloader arabirimi burada bulunur.

## <a name="state-change-events"></a>Durum değişikliği olayları

`ChangeTracker` yeni `Tracked` ve `StateChanged` olayları, DbContext ' i girerek veya durumlarını değiştirerek varlıklara yeniden davranan mantığı yazmak için kullanılabilir.

## <a name="raw-sql-parameter-analyzer"></a>Ham SQL parametre Çözümleyicisi

`FromSql` veya `ExecuteSqlCommand`gibi ham-SQL API 'lerimizin güvensiz kullanımlarını algılayan EF Core yeni bir kod çözümleyici bulunur. Örneğin, aşağıdaki sorgu için bir uyarı görürsünüz çünkü _Minage_ parametreli değil:

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a>Veritabanı sağlayıcısı uyumluluğu

EF Core 2,1 ' i güncelleştirilmiş veya en az test edilmiş EF Core 2,1 ile çalışacak şekilde kullanmanız önerilir.

> [!TIP]
> Yeni özelliklerde beklenmeyen bir uyumsuzluk veya herhangi bir sorun bulursanız veya bunlarla ilgili geri bildiriminiz varsa, lütfen [sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues/new)'ni kullanarak bildirin.
