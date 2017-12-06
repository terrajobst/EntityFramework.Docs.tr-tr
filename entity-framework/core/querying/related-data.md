---
title: "Yükleme ilgili verileri - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: cd26bd2e6f85083f73d97b1356d0ba38f53e0b8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="loading-related-data"></a>İlgili verileri yükleniyor

Entity Framework Çekirdek, gezinti özellikleri ilgili varlıklar yüklemek için modelinizde kullanmanıza olanak sağlar. İlgili verileri yüklemek için kullanılan üç ortak O/RM desenler vardır.
* **İstekli yükleme** ilgili verileri ilk sorgu bir parçası olarak veritabanından yüklenen anlamına gelir.
* **Açık yükleme** ilgili verileri veritabanından açıkça daha sonraki bir zamanda yüklenip yüklenmediğine anlamına gelir.
* **Yavaş Yükleniyor** gezinti özelliği erişildiğinde ilgili verileri veritabanından saydam yüklenen anlamına gelir. Yavaş yükleniyor henüz EF çekirdek ile mümkün değildir.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) github'da.

## <a name="eager-loading"></a>istekli yükleniyor

Kullanabileceğiniz `Include` yöntemi sorgu sonuçlarında dahil edilecek ilgili verileri belirtin. Aşağıdaki örnekte, döndürülen sonuçlarda bloglar olacaktır kendi `Posts` özelliği ile ilgili gönderileri doldurulur.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Çekirdek otomatik olarak düzeltme yukarı Gezinti özellikleri bağlam örneğine önceden yüklenmiş herhangi bir varlık için. Bu nedenle bir gezinme özelliği için veri açıkça içerme olsa bile, özellik hala bazıları doldurulmuş veya tüm ilgili varlıklar daha önce yüklenen.


Tek bir sorguda birden çok ilişki ilgili verileri içerebilir.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Birden çok düzeyi de dahil olmak üzere

Birden çok düzeyinden birini kullanarak ilgili verileri içerecek şekilde ilişkileri ayrıntıya girebilirsiniz `ThenInclude` yöntemi. Aşağıdaki örnek, tüm blogları, kendi ilgili gönderileri ve her posta yazarı yükler.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

Birden fazla çağrı zincir `ThenInclude` daha ilgili verileri düzeylerini dahil olmak üzere devam etmek için.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Bu aynı sorguda birden çok düzeyi ve birden çok kök ilgili verileri içerecek şekilde tüm birleştirebilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Dahil varlıklar biri için birden çok ilgili varlık eklemek isteyebilirsiniz. Örneğin, sorgulanırken `Blog`s, eklediğiniz `Posts` ve ardından her ikisi de dahil etmek istediğiniz `Author` ve `Tags` , `Posts`. Bunu yapmak için her dahil kökte başlayan yolunu belirtmeniz gerekir. Örneğin, `Blog -> Posts -> Author` ve `Blog -> Posts -> Tags`. Bu, çoğu durumda EF birleştirmek yedekli birleştirmeler SQL oluştururken birleştirmeler alırsınız gelmez.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a>Göz ardı içerir

Böylece artık sorgu ile başlamıştır varlık türünün örneklerini döndürür sorgu değiştirirseniz, INCLUDE işleçleri göz ardı edilir.

INCLUDE işleçleri aşağıdaki örnekte, temel alan `Blog`, ancak ardından `Select` işleci anonim bir tür döndürmek için sorguyu değiştirmek için kullanılır. Bu durumda, INCLUDE işleçleri hiçbir etkisi yoktur.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Varsayılan olarak, bir uyarı EF çekirdek oturum zaman dahil işleçleri yok sayılır. Bkz: [günlüğü](../miscellaneous/logging.md) günlük çıktısı görüntüleme hakkında daha fazla bilgi. Ekle işleci throw veya hiçbir şey yapma göz ardı davranışı değiştirebilirsiniz. Bu genellikle, içeriğiniz - seçeneklerini ayarlama zaman yapılır `DbContext.OnConfiguring`, veya `Startup.cs` ASP.NET Core kullanıyorsanız.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Açık yükleniyor

> [!NOTE]  
> Bu özellik EF çekirdek 1.1 sunulmuştur.

Bir gezinti özelliği aracılığıyla açıkça yükleyebilir `DbContext.Entry(...)` API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

İlgili varlıklar döndürüyor ayrı bir sorgu yürüterek bir gezinti özelliği de açıkça yükleyebilirsiniz. Değişiklik izleme etkinleştirildi, varsa bir varlık yüklenirken EF çekirdek otomatik olarak zaten yüklenmiş herhangi bir varlık başvurmak için yeni yüklenen entitiy Gezinti özelliklerini ayarlamak ve başvurmak için daha önce yüklenmiş varlıklar Gezinti özelliklerini ayarlama Yeni yüklenen varlık.

### <a name="querying-related-entities"></a>İlgili varlıklar sorgulama

Ayrıca bir gezinti özelliği içeriğini temsil eden bir LINQ Sorgu alabilirsiniz.

Bu belleğe yüklemeden ilgili varlıklar üzerinde bir toplama işleci çalıştırma gibi şeyler olanak sağlar.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Ayrıca, hangi ilgili varlıklar belleğe yüklenen filtre uygulayabilirsiniz.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>yavaş yükleniyor

Yavaş yükleniyor EF çekirdek tarafından henüz desteklenmiyor. Görüntüleyebileceğiniz [bizim biriktirme listesi üzerinde öğe yavaş yüklenirken](https://github.com/aspnet/EntityFramework/issues/3797) bu özellik izlemek için.

## <a name="related-data-and-serialization"></a>İlgili tüm verileri ve seri hale getirme

EF çekirdek işlem otomatik olarak düzeltme yukarı Gezinti özellikleri, döngü ile nesne grafiğinde düşebilir olduğundan. Örneğin, bir blog ve yükleme gönderileri gönderileri koleksiyonu başvuruda bulunan bir blog nesnesinde sonuçlanır ilişkilidir. Bu gönderileri her bir başvuru geri blog sahip olur.

Bazı serileştirme çerçeveler gibi döngüleri izin vermez. Örneğin, bir döngü encoutered ise Json.NET şu özel durum oluşturur.

> Newtonsoft.Json.JsonSerializationException: Kendi kendine başvuran döngü 'Blog' özelliği için 'MyApplication.Models.Blog' türüyle algılandı.

ASP.NET Core kullanıyorsanız, nesne grafiğinde bulduğu döngüleri yoksaymak için Json.NET yapılandırabilirsiniz. Bu yapılır `ConfigureServices(...)` yönteminde `Startup.cs`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
