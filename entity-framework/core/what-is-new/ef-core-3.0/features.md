---
title: EF Core 3,0 ' deki yeni özellikler EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: d61fa884f4669daa220ffc96ae59dd63518e6d5a
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921683"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a>EF Core 3,0 ' de bulunan yeni özellikler (Şu anda önizlemede)

> [!IMPORTANT]
> Gelecekteki sürümlerin özellik kümelerinin ve zamanlamalarının her zaman değişikliğe tabi olduğunu ve bu sayfayı güncel tutmaya deneydiğimiz halde, her zaman en son planlarımızı yansıtmadığımızdan emin olun.

Aşağıdaki listede EF Core 3,0 için planlanmış olan başlıca yeni özellikler yer almaktadır.
Bu özelliklerin çoğu geçerli önizlemeye dahil edilmez, ancak RTM 'ye doğru ilerleme yaptığımız için kullanılabilir hale gelir.

Bunun nedeni, yayının başlangıcında planlanmış [son değişiklikleri](xref:core/what-is-new/ef-core-3.0/breaking-changes)uygulamaya odaklanmaktadır.
Bu önemli değişikliklerden çoğu, kendi EF Core geliştirmelerdir.
Diğer iyileştirmeler, daha fazla geliştirmelerin engelini kaldırmak için gereklidir. 

Hata düzeltmelerinin ve geliştirmelerin tüm listesi için [Bu sorguyu sorun izleyicimizde](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc)görebilirsiniz.

## <a name="linq-improvements"></a>LINQ geliştirmeleri 

[Sorun izleniyor #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Bu özellik üzerinde çalışma başlatıldı, ancak geçerli önizlemeye dahil değildir.

LINQ, IntelliSense ve derleme zamanı tür denetimi almak için zengin tür bilgilerinizin avantajlarından yararlanarak, seçtiğiniz dilden çıkmadan veritabanı sorguları yazmanızı sağlar.
Ancak LINQ Ayrıca sınırsız sayıda karmaşık sorgu yazmanızı sağlar ve bu, LINQ sağlayıcıları için her zaman çok büyük bir zorluk olmuştur.
EF Core ilk birkaç sürümünde, bir sorgunun hangi bölümlerinin SQL 'e çevrilebileceğini öğrenerek ve sonra sorgunun geri kalanının istemci üzerinde bellekte yürütülmesine izin vererek bu bölümü çözeceğiz.
Bu istemci tarafı yürütme bazı durumlarda istenebilir, ancak çoğu durumda, bir uygulama üretime dağıtılana kadar belirlenemeyebilir verimsiz sorgulara yol açabilir.
EF Core 3,0 ' de, LINQ uygulamanızın nasıl çalıştığı ve nasıl test ettiğimiz üzerinde yapılan değişiklikleri yapmayı planlıyoruz.
Hedefler daha sağlam hale gelir (örneğin, düzeltme eki sürümlerindeki sorguların kesilmesini önlemek için), daha fazla ifadeyi SQL 'e doğru çevirmeyi etkinleştirmek, daha fazla durumda etkili sorgular oluşturmak ve verimsiz sorguların algılanmasını önlemek için.

## <a name="cosmos-db-support"></a>Cosmos DB desteği 

[Sorun izleniyor #8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

Bu özellik geçerli önizlemeye dahildir, ancak henüz tamamlanmadı. 

EF Core için bir Cosmos DB sağlayıcı üzerinde çalışıyoruz, böylece bir uygulama veritabanı olarak Azure Cosmos DB kolayca hedeflemek için EF programlama modeliyle tanıdık geliştiricilerin kolayca hedeflemesini sağlar.
Amaç, küresel dağıtım, "her zaman açık" kullanılabilirlik, elastik ölçeklenebilirlik ve düşük gecikme süresi gibi Cosmos DB avantajlarından bazılarını, hatta .NET geliştiricilerine daha erişilebilir hale getirmek için kullanılır.
Sağlayıcı, Cosmos DB içindeki SQL API 'sine karşı otomatik değişiklik izleme, LINQ ve değer dönüştürmeleri gibi EF Core özelliklerinin çoğunu etkinleştirir.
Bu çabadan önce 2,2 EF Core önce başladık ve [sağlayıcının bazı önizleme sürümlerini kullanıma](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/)sunuyoruz.
Yeni plan, sağlayıcıyı EF Core 3,0 ' de geliştirmeye devam etmek için kullanılır. 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Tabloyu sorumlu ile paylaşan bağımlı varlıklar artık isteğe bağlıdır

[Sorun izleniyor #9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

Bu özellik EF Core 3,0-Preview 4 ' te tanıtılacaktır.

Aşağıdaki modeli göz önünde bulundurun:
```C#
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

EF Core 3,0 ' den başlayarak, `OrderDetails` aynı tabloya `Order` ait veya açıkça eşlenmiş ise, birincil anahtar ile eşleştirilecektir, ancak tüm özellikler olmadan `Order` `OrderDetails` bir eklenebilir. `OrderDetails` null yapılabilir sütunlar.

Sorgulama yaparken, gerekli özelliklerinden herhangi `OrderDetails` birinin `null` bir değeri yoksa veya birincil `null`anahtar ve tüm Özellikler ' in yanı sıra gerekli özellikleri yoksa EF Core olarak ayarlanır.

## <a name="c-80-support"></a>C#8,0 desteği

[Sorun izleme #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[izleme sorunu#10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)

Bu özellik üzerinde çalışma başlatıldı, ancak geçerli önizlemeye dahil değildir.

Müşterilerimizin, EF Core kullanılırken zaman uyumsuz akışlar (dahil `await foreach`) ve null yapılabilir başvuru türleri gibi [8,0 ' de C# gelen yeni özelliklerden](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) yararlanabilmeleri istiyoruz.

## <a name="reverse-engineering-of-database-views"></a>Veritabanı görünümlerinin tersine mühendislik

[Sorun izleniyor #1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

Bu özellik geçerli önizlemeye dahil değildir.

EF Core 2,1 ' de tanıtılan ve EF Core 3,0 ' de anahtarlar olmadan varlık türlerini kabul eden [sorgu türleri](xref:core/modeling/query-types), veritabanından okunabilecek verileri temsil eder, ancak güncelleştirilemez.
Bu özellik, Çoğu senaryoda veritabanı görünümlerine mükemmel bir uyum sağlar. bu nedenle, tersine mühendislik veritabanı görünümlerinde, anahtar olmadan varlık türlerinin oluşturulmasını otomatikleştirmeyi planlıyoruz.

## <a name="ef-63-on-net-core"></a>.NET Core üzerinde EF 6,3

[Sorun izleme EF6 # 271](https://github.com/aspnet/EntityFramework6/issues/271)

Bu özellik üzerinde çalışma başlatıldı, ancak geçerli önizlemeye dahil değildir. 

Mevcut birçok uygulamanın önceki EF sürümlerini kullandığını anladık ve yalnızca .NET Core 'un avantajlarından yararlanmak için EF Core 'e taşıma işlemleri bazen önemli bir çaba gerektirebilir.
Bu nedenle, .NET Core 3,0 ' de çalışacak EF 6 ' nın bir sonraki sürümünü uyarlarız.
Bunu, mevcut uygulamaların en az değişiklikle yapılmasını kolaylaştırmak için yapıyoruz.
Bazı sınırlamalar olacak. Örneğin:
- .NET Core 'da eklenen SQL Server desteğinin yanı sıra yeni sağlayıcıların diğer veritabanlarıyla çalışması gerekir
- SQL Server ile uzamsal destek etkinleştirilmeyecek

Ayrıca, bu noktada EF 6 için planlanmış yeni bir özellik olmadığını unutmayın.
