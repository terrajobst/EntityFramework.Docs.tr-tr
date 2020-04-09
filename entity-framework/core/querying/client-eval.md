---
title: İstemci ve Sunucu Değerlendirmesi - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: e01bd146c4dfe7a8d36b641cb52ae366fddd8239
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417762"
---
# <a name="client-vs-server-evaluation"></a>İstemci ve Sunucu Değerlendirmesi

Genel bir kural olarak, Entity Framework Core sunucudaki bir sorguyu mümkün olduğunca değerlendirmeye çalışır. EF Core, sorgunun bazı bölümlerini istemci tarafında değerlendirebileceği parametrelere dönüştürür. Sorgunun geri kalanı (oluşturulan parametrelerle birlikte) sunucuda değerlendirilecek eşdeğer veritabanı sorgusunu belirlemek üzere veritabanı sağlayıcısına verilir. EF Core, üst düzey projeksiyonda kısmi istemci değerlendirmesini destekler `Select()`(aslında, son çağrı). Sorgudaki üst düzey projeksiyon sunucuya çevrilemezse, EF Core sunucudan gerekli verileri alır ve istemcide sorgunun kalan bölümlerini değerlendirir. EF Core, sunucuya çevrilemez üst düzey projeksiyon dışında herhangi bir yerde bir ifade algılarsa, o zaman bir çalışma zamanı özel durum atar. EF Core'un sunucuya çevrilemeyecek leri nasıl belirlediğini anlamak için [sorgunun nasıl çalıştığını](xref:core/querying/how-query-works) görün.

> [!NOTE]
> Sürüm 3.0'dan önce, Entity Framework Core sorgunun herhangi bir yerindeki istemci değerlendirmesini destekledi. Daha fazla bilgi için [önceki sürümler bölümüne](#previous-versions)bakın.

> [!TIP]
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub'da görüntüleyebilirsiniz.

## <a name="client-evaluation-in-the-top-level-projection"></a>Üst düzey projeksiyonda müşteri değerlendirmesi

Aşağıdaki örnekte, SQL Server veritabanından döndürülen bloglar için URL'leri standartlaştırmak için yardımcı yöntemi kullanılır. SQL Server sağlayıcısının bu yöntemin nasıl uygulandığına dair bir içgörüsü olmadığından, bu yöntemin SQL'e çevrilmesi mümkün değildir. Sorgunun diğer tüm yönleri veritabanında değerlendirilir, ancak `URL` bu yöntem üzerinden döndürülen istemci üzerinde yapılır.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientMethod)]

## <a name="unsupported-client-evaluation"></a>Desteklenmeyen istemci değerlendirmesi

İstemci değerlendirmesi yararlı olsa da, bazen düşük performansa neden olabilir. Yardımcı yönteminin artık bir where filtresinde kullanıldığı aşağıdaki sorguyu göz önünde bulundurun. Filtre veritabanında uygulanamadığından, istemciye filtre uygulamak için tüm verilerin belleğe çekilmesi gerekir. Filtreye ve sunucudaki veri miktarına bağlı olarak, istemci değerlendirmesi düşük performansa neden olabilir. Yani Entity Framework Core bu tür istemci değerlendirmesini engeller ve çalışma zamanı özel bir durum atar.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientWhere)]

## <a name="explicit-client-evaluation"></a>Açık istemci değerlendirmesi

Aşağıdaki gibi bazı durumlarda açıkça müşteri değerlendirmesine zorlamanız gerekebilir

- Veri miktarı küçüktür, böylece istemci üzerinde değerlendirme büyük bir performans cezasına neden olmaz.
- Kullanılan LINQ işlecinin sunucu tarafı çevirisi yoktur.

Bu gibi durumlarda, `AsEnumerable` gibi veya `ToList` (veya`AsAsyncEnumerable` `ToListAsync` async) gibi yöntemleri arayarak açıkça istemci değerlendirmesini tercih edebilirsiniz. Kullanarak `AsEnumerable` sonuçları akışı olurdu, ancak `ToList` kullanarak da ek bellek alır bir liste oluşturarak arabelleğe alma neden olur. Birden çok kez sıralama ediyorsanız, veritabanında yalnızca bir sorgu olduğundan sonuçları bir listede depolamak daha fazla yardımcı olur. Belirli kullanıma bağlı olarak, hangi yöntemin servis talebi için daha yararlı olduğunu değerlendirmelisiniz.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ExplicitClientEval)]

## <a name="potential-memory-leak-in-client-evaluation"></a>İstemci değerlendirmesinde olası bellek sızıntısı

Sorgu çevirisi ve derleme pahalı olduğundan, EF Core derlenmiş sorgu planını önbelleğe alır. Önbelleğe alınan temsilci, üst düzey projeksiyonun istemci değerlendirmesini yaparken istemci kodunu kullanabilir. EF Core, ağacın istemci tarafından değerlendirilen bölümleri için parametreler oluşturur ve parametre değerlerini değiştirerek sorgu planını yeniden kullanır. Ancak ifade ağacındaki bazı sabitler parametreye dönüştürülemez. Önbelleğe alınan temsilci bu tür sabitleri içeriyorsa, bu nesneler hala başvurulmakta oldukları için çöp olarak toplanabilir. Böyle bir nesne içinde bir DbContext veya başka hizmetler içeriyorsa, uygulamanın bellek kullanımının zaman içinde büyümesine neden olabilir. Bu davranış genellikle bir bellek sızıntısının bir işaretidir. EF Core, geçerli veritabanı sağlayıcısı kullanılarak eşlenemez bir tür sabitleri karşılaştığında bir özel durum atar. Ortak nedenleri ve çözümleri aşağıdaki gibidir:

- **Örnek yöntemi kullanma**: Bir istemci projeksiyonunda örnek yöntemlerini kullanırken, ifade ağacı örneğin sabitini içerir. Yönteminiz örnekten herhangi bir veri kullanmıyorsa, yöntemi statik hale getirmeyi düşünün. Yöntem gövdesinde örnek verilere ihtiyacınız varsa, belirli verileri bir bağımsız değişken olarak yönteme geçirin.
- **Sabit bağımsız değişkenleri yönteme geçirme**: `this` Bu durum genellikle bir bağımsız değişkende istemci yöntemi kullanılarak ortaya çıkar. Bağımsız değişkeni veritabanı sağlayıcısı tarafından eşlenen birden çok skaler bağımsız değişkene bölmeyi düşünün.
- **Diğer sabitler**: Başka bir durumda bir sabit rastlarsa, o zaman sabitin işlenmesinde gerekli olup olmadığını değerlendirebilirsiniz. Sabite sahip olmak gerekiyorsa veya yukarıdaki durumlardan bir çözüm kullanamıyorsanız, değeri depolamak ve sorguda yerel değişkeni kullanmak için yerel bir değişken oluşturun. EF Core yerel değişkeni bir parametreye dönüştürür.

## <a name="previous-versions"></a>Önceki sürümler

Aşağıdaki bölüm 3.0'dan önceki EF Core sürümleri için geçerlidir.

Eski EF Core sürümleri, yalnızca üst düzey projeksiyonu değil, sorgunun herhangi bir bölümünde istemci değerlendirmesini desteklenebilmiştir. Bu nedenle [Desteklenmeyen istemci değerlendirme](#unsupported-client-evaluation) bölümü altında deftere nakledilene benzer sorgular doğru çalıştı. Bu davranış fark edilmeyen performans sorunlarına neden olabileceğinden, EF Core bir istemci değerlendirme uyarısı günlüğe kaydetmiştir. Günlüğe kaydetme çıktısını görüntüleme hakkında daha fazla bilgi için [Günlük'](xref:core/miscellaneous/logging)e bakın.

İsteğe bağlı olarak EF Core, varsayılan davranışı bir özel durum atmak veya istemci değerlendirmesi yaparken hiçbir şey yapmamak için (projeksiyon hariç) değiştirmenize olanak sağladı. Özel durum atma davranışı 3.0'daki davranışa benzer hale getirecek. Davranışı değiştirmek için, bağlamınıziçin seçenekleri ayarlarken uyarıları yapılandırmanız gerekir `DbContext.OnConfiguring`- genellikle `Startup.cs` ASP.NET Core'da veya burada.

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
