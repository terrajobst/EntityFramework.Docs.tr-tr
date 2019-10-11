---
title: İstemci ile Sunucu değerlendirmesi-EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: 3d70324f0b57a0ea9b165b5140a2154001c326f4
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181909"
---
# <a name="client-vs-server-evaluation"></a>İstemci ile Sunucunun Karşılaştırmalı Değerlendirmesi

Genel bir kural olarak, Entity Framework Core sunucu üzerinde mümkün olduğunca bir sorguyu değerlendirmeye çalışır. EF Core sorgunun bölümlerini, istemci tarafında değerlendirebileceği parametrelere dönüştürür. Sorgunun geri kalanı (oluşturulan parametrelerle birlikte), sunucuda değerlendirilecek eşdeğer veritabanı sorgusunun belirlenmesi için veritabanı sağlayıcısına verilir. EF Core, üst düzey projeksiyonda (temelde, `Select()` ' a yapılan son çağrı) kısmi istemci değerlendirmesini destekler. Sorgudaki en üst düzey projeksiyon sunucuya çevrilemediği takdirde, EF Core sunucudan gerekli verileri alır ve sorgunun kalan bölümlerini değerlendirir. EF Core, üst düzey projeksiyon dışında herhangi bir yerde, sunucuya çevrilemeyen bir ifade algılarsa, çalışma zamanı özel durumu oluşturur. Sorgunun ne EF Core sunucuya çevrilemeyecek olduğunu anlamak için [sorgunun nasıl çalıştığını](xref:core/querying/how-query-works) görün.

> [!NOTE]
> Sürüm 3,0 ' den önce, sorgudaki her yerde desteklenen istemci değerlendirmesi Entity Framework Core. Daha fazla bilgi için [önceki sürümler bölümüne](#previous-versions)bakın.

## <a name="client-evaluation-in-the-top-level-projection"></a>Üst düzey projeksiyde istemci değerlendirmesi

> [!TIP]
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.

Aşağıdaki örnekte, bir SQL Server veritabanından döndürülen blogların URL 'Lerini standartlaştırmak için bir yardımcı yöntem kullanılır. SQL Server sağlayıcının bu yöntemin nasıl uygulandığı hakkında öngörü olmadığından, SQL 'e çevirmek mümkün değildir. Sorgunun diğer tüm yönleri veritabanında değerlendirilir, ancak döndürülen `URL` ' ı bu yöntem aracılığıyla istemciye geçirme işlemi istemcide yapılır.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientMethod)]

## <a name="unsupported-client-evaluation"></a>Desteklenmeyen istemci değerlendirmesi

İstemci değerlendirmesi yararlı olsa da, bazı durumlarda düşük performansa neden olabilir. Aşağıdaki sorguyu göz önünde bulundurun. Bu, yardımcı yönteminin Şu anda bir WHERE filtresinde kullanıldığı bir yerdir. Filtre veritabanına uygulanamadığından, filtrenin istemciye uygulanması için tüm verilerin belleğe çekililmesi gerekir. Filtre ve sunucudaki veri miktarına bağlı olarak, istemci değerlendirmesi kötü performansa neden olabilir. Entity Framework Core, bu tür istemci değerlendirmesini engeller ve bir çalışma zamanı özel durumu oluşturur.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientWhere)]

## <a name="explicit-client-evaluation"></a>Açık istemci değerlendirmesi

Aşağıdaki gibi belirli durumlarda istemci değerlendirmesi için açıkça zorlamanız gerekebilir

- Verilerin miktarı küçüktür. böylece, istemcide değerlendirmek büyük bir performans cezası olmaz.
- Kullanılan LINQ işlecinin sunucu tarafı çevirisi yok.

Böyle durumlarda, `AsEnumerable` veya `ToList` (`AsAsyncEnumerable` veya `ToListAsync` zaman uyumsuz) gibi yöntemleri çağırarak istemci değerlendirmesini açıkça tercih edebilirsiniz. @No__t kullanarak, sonuçları akışla, ancak `ToList` ' i kullanmak, ek bellek de alan bir liste oluşturarak arabelleğe alma işlemine neden olur. Birden çok kez numaralandırdıysanız, sonuçları bir listede depolamak, veritabanında yalnızca bir sorgu olduğundan daha fazlasına yardımcı olur. Belirli bir kullanıma bağlı olarak, hangi yöntemin durum için daha yararlı olduğunu değerlendirmeniz gerekir.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ExplicitClientEval)]

## <a name="potential-memory-leak-in-client-evaluation"></a>İstemci değerlendirmesinde olası bellek sızıntısı

Sorgu çevirisi ve derleme pahalı olduğundan, derlenmiş sorgu planını EF Core önbelleğe alır. Önbelleğe alınan temsilci, en üst düzey projeksiyonun istemci değerlendirmesi sırasında istemci kodunu kullanabilir. EF Core, ağacın istemci tarafından değerlendirilen parçaları için parametreler üretir ve parametre değerlerini değiştirerek sorgu planını yeniden kullanır. Ancak ifade ağacındaki bazı sabitler parametrelere dönüştürülemez. Önbelleğe alınmış temsilci bu tür sabitleri içeriyorsa, bu nesneler hala başvurulduğu için atık olarak toplanamaz. Böyle bir nesne, içindeki bir DbContext veya diğer hizmetleri içeriyorsa, uygulamanın bellek kullanımının zaman içinde büyümesine neden olabilir. Bu davranış genellikle Bellek sızıntısının bir imzadır. EF Core, geçerli veritabanı sağlayıcısı kullanılarak eşleştirilenemeyen bir türün sabitlerinde her geldiğinde bir özel durum oluşturur. Yaygın nedenler ve çözümleri aşağıdaki gibidir:

- **Örnek yöntemi kullanma**: Bir istemci projeksiyonundaki örnek yöntemleri kullanırken, ifade ağacı örneğin bir sabitini içerir. Yönteminiz örnekten herhangi bir veri kullanmıyorsa, yöntemi statik hale getirmeyi düşünün. Yöntem gövdesinde örnek veriye ihtiyacınız varsa, belirli verileri yöntemine bir bağımsız değişken olarak geçirin.
- **Yönteme sabit bağımsız değişkenler geçiliyor**: Bu durum genellikle istemci yöntemine yönelik bir bağımsız değişkende `this` kullanılarak yapılır. İçindeki bağımsız değişkenini, veritabanı sağlayıcısı tarafından eşlenmekte olan birden çok skaler bağımsız değişkene bölmeyi göz önünde bulundurun.
- **Diğer sabitler**: Başka herhangi bir durumda bir sabit geliyorsa, sabit işlemin işlenmek üzere gerekli olup olmadığını değerlendirebilirsiniz. Sabit olması gerekiyorsa veya yukarıdaki durumlardan bir çözüm kullanamıyoruz, değeri depolamak için yerel bir değişken oluşturun ve sorguda yerel değişkeni kullanın. EF Core, yerel değişkeni bir parametreye dönüştürecek.

## <a name="previous-versions"></a>Önceki sürümler

Aşağıdaki bölüm 3,0 öncesi EF Core sürümler için geçerlidir.

Daha eski EF Core, bir sorgunun herhangi bir bölümünde istemci değerlendirmesi desteklenir; yalnızca üst düzey projeksiyon değil. Bu nedenle, [Desteklenmeyen istemci değerlendirme](#unsupported-client-evaluation) bölümü altında gönderilen sorgular doğru bir şekilde çalışmıştır. Bu davranış fark edilmemiş performans sorunlarına neden olabileceği için EF Core istemci değerlendirme uyarısını günlüğe kaydetti. Günlüğe kaydetme çıkışını görüntüleme hakkında daha fazla bilgi için bkz. [Logging](xref:core/miscellaneous/logging).

İsteğe bağlı olarak EF Core, varsayılan davranışı bir özel durum oluşturmak veya istemci değerlendirmesi yaparken (projeksiyonu hariç) hiçbir şey yapmak üzere değiştirmenize izin verilir. Özel durum atma davranışı, 3,0 'deki davranışa benzer hale gelir. Davranışı değiştirmek için, bağlam için seçenekleri ayarlarken (genellikle `DbContext.OnConfiguring` ' da) veya ASP.NET Core kullanıyorsanız, `Startup.cs` ' de uyarı yapılandırmanız gerekir.

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
