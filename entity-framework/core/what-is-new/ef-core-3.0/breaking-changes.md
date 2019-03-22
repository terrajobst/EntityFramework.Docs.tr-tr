---
title: EF Core 3.0 - EF Core yeni değişiklikler
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 534ac95cccc03e9797ba766e601e2fe86eaf8061
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319224"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a>EF Core 3. 0 ' (şu anda Önizleme aşamasında) dahil edilen değişiklikler

> [!IMPORTANT]
> Özellik kümeleri ve zamanlamaları gelecek sürümlerin her zaman değişikliğe tabi olduğu ve bu sayfada en güncel, en son planlarımızı hiç yansıtmayabilir tutmak deneyeceğiz rağmen zaman lütfen unutmayın.

Aşağıdaki API ve davranışı değişiklikleri EF Core için geliştirilen uygulamaları ayırmak için olası sahip bunları 3.0.0 için yükseltirken 2.2.x.
Veritabanı sağlayıcıları yalnızca etkileyen bekliyoruz değişiklikleri altında belgelenir [sağlayıcısı değişiklikleri](../../providers/provider-log.md).
Bir 3.0 Önizlemesi'nden başka bir 3.0 Önizleme için kullanıma sunulan yeni özellikler sonlarını burada belgelenmemiş.

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>LINQ sorguları, artık istemcide değerlendirilir

[Sorun #14935 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[sorun #12795 Ayrıca bkz:](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.

**Eski davranışı**

EF Core, SQL veya bir parametre için bir sorgunun parçası olan bir ifade dönüştürülemedi, 3.0 önce bunu otomatik olarak istemcide ifadesi değerlendirilir.
Varsayılan olarak, istemci değerlendirmesi yüksek maliyetlere neden olabilecek bir ifade yalnızca bir uyarı tetiklenir.

**Yeni davranış**

3.0 ile başlayarak, EF Core yalnızca ifadeleri üst düzey projeksiyon izin verir (son `Select()` sorguda çağrısı) istemcide değerlendirilecek.
İfadeleri herhangi bir sorgunun parçası olarak SQL veya bir parametre olarak dönüştürülemez, bir özel durum oluşturulur.

**Neden**

Otomatik istemci değerlendirme sorguların bile bunları önemli bölümleri çevrilemez yürütülecek çok sayıda sorgu sağlar.
Bu davranış yalnızca üretimde yetkisiz değiştirmeye karşı korumalı hale gelebilir beklenmeyen ve zararlı davranışa neden olabilir.
Örneğin, bir koşulda bir `Where()` çevrilemez çağrısı, veritabanı sunucusu ve istemcide uygulanması için filtreyi aktarılacak tablosundan tüm satırları açabilir.
Tablo, geliştirme, ancak isabet sabit yalnızca birkaç satır içeriyorsa uygulama üretime burada tablo milyonlarca satır içerebilir taşındığında bu durum kolayca algılanmayabilir.
İstemci değerlendirme uyarılar ayrıca geliştirme sırasında göz ardı edilmesi çok kolay kanıtladılar.

Belirli ifadeler için iyileştirme sorgu çevirisi sürümler arasında istenmeyen bozucu değişiklikleri nedeniyle oluşan sorunları bu yanı sıra, otomatik istemci değerlendirme neden olabilir.

**Risk azaltma işlemleri**

Bir sorgu tam olarak çevrilemez sonra ya da çevrilebilir veya kullanan bir form sorguda yeniden `AsEnumerable()`, `ToList()`, veya açıkça verileri Burada, ardından daha büyük olabilir istemciye geri getirmek için benzer LINQ nesneleri kullanılarak işlenir.

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>Entity Framework Core artık ASP.NET Core paylaşılan framework'ün bir parçasıdır

[İzleme sorunu duyuruları #325](https://github.com/aspnet/Announcements/issues/325)

Bu değişiklik, ASP.NET Core 3.0 Önizleme 1 sunulmuştur. 

**Eski davranışı**

ASP.NET Core 3.0 Paketi başvurusu eklendiğinde önce `Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All`, EF Core içerir ve EF Core veri sağlayıcıları bazıları istediğiniz SQL Server sağlayıcısı.

**Yeni davranış**

3. 0'dan başlayarak, ASP.NET Core paylaşılan çerçeve EF Core veya tüm EF Core veri sağlayıcıları içermez.

**Neden**

Bu değişiklikten önce EF Core alma olup uygulama ASP.NET Core ve SQL Server veya hedeflenen bağlı olarak farklı adımlar gereklidir. Ayrıca, ASP.NET Core yükseltme EF Core ve SQL Server sağlayıcısı, her zaman tercih değildir yükseltmesini zorlandı.

Bu değişiklik, EF Core alma aynı tüm sağlayıcıları, desteklenen .NET uygulamaları ve uygulama türleri arasında deneyimidir.
Geliştiriciler, tam olarak EF Core ve EF Core veri sağlayıcıları yükseltildiğinde de denetleyebilirsiniz.

**Risk azaltma işlemleri**

EF çekirdekli ASP.NET Core 3.0 uygulama veya desteklenen başka bir uygulama içinde kullanmak için açıkça uygulamanızın kullanacağı EF Core veritabanı sağlayıcısı için bir paket başvurusu ekleyin.

## <a name="query-execution-is-logged-at-debug-level"></a>Sorgu yürütme hata ayıklama düzeyinde günlüğe kaydedilir

[İzleme sorun #14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce sorgu ve diğer komutları yürütme sırasında günlüğe kaydedilen `Info` düzeyi.

**Yeni davranış**

EF Core 3.0 ile başlayarak, komut/SQL yürütme günlüğe kaydetme sırasında işlemi `Debug` düzeyi.

**Neden**

Konumundaki paraziti azaltmak için bu değişiklik yapılmıştır `Info` günlük düzeyi.

**Risk azaltma işlemleri**

Bu günlük olayı tarafından tanımlanan `RelationalEventId.CommandExecuting` 20100 olay kimliği.
SQL oturum `Info` yeniden düzey, düzeyi açıkça yapılandırmanıza `OnConfiguring` veya `AddDbContext`.
Örneğin:
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>Geçici bir anahtar değere artık varlık örneklerini ayarlanır

[İzleme sorun #12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce veritabanı tarafından oluşturulan gerçek değer daha sonra sahip olabileceği tüm anahtar özellikleri geçici değerler atanmıştır.
Genellikle bu geçici değerleri büyük negatif sayılar yoktu.

**Yeni davranış**

3.0 ile başlayarak, EF Core, varlığın izleme bilgilerini bir parçası olarak geçici bir anahtar değeri depolar ve anahtar özelliği kendisini değiştirmeden bırakır.

**Neden**

Geçici bir anahtar değere deneyebileceğinizi daha önce bazı tarafından izlenen bir varlık kalıcı hale gelmesini önlemek için bu değişiklik yapılmıştır `DbContext` örneği farklı bir taşınır `DbContext` örneği. 

**Risk azaltma işlemleri**

Varlıklar arasında ilişkilendirmeler form üzerine yabancı anahtarlar birincil anahtar değerlerini atamak uygulamaları birincil anahtarları depoda üretilmiş ve varlıklara ait eski davranışı bağımlı `Added` durumu.
Tarafından önlenebilir:
* Depoda üretilmiş anahtarlar kullanmıyor.
* Yabancı anahtar değerleri ayarlamak yerine form ilişkiler için Gezinti özelliklerini ayarlama.
* Gerçek geçici anahtar değerlerinin varlığın izleme bilgileri edinin.
Örneğin, `context.Entry(blog).Property(e => e.Id).CurrentValue` geçici değer olsa bile iade `blog.Id` kendisini ayarlanmadı.

## <a name="detectchanges-honors-store-generated-key-values"></a>Depoda üretilmiş anahtar değerlerini DetectChanges geliştirir

[İzleme sorun #14616](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce izlenmeyen bir varlık tarafından bulunan `DetectChanges` olarak izlenmesi `Added` durum ve eklenen yeni bir olarak ne zaman satır `SaveChanges` çağrılır.

**Yeni davranış**

Oluşturulan anahtar değerlerini bir varlık kullanıyorsa EF Core 3.0 ile başlayan ve bazı anahtar değerini ayarlayın ve ardından varlığı olarak takip `Modified` durumu.
Bu varlık için bir satır var olduğu varsayılır ve bu anlamına gelir ne zaman güncelleştirilmiş `SaveChanges` çağrılır.
Anahtar değeri ayarlanmamış, veya varlık türü kullanmıyor oluşturulan anahtarları varsa yeni bir varlık olarak hala izlenecektir `Added` önceki sürümlerinde olduğu gibi.

**Neden**

Bu değişiklik, daha kolay ve bağlantısı kesilmiş varlık grafiklerle depoda üretilmiş anahtarlar kullanılırken çalışmak için daha tutarlı hale getirmek için yapılmıştır.

**Risk azaltma işlemleri**

Bu değişiklik, bir varlık türü için üretilmiş anahtarlar yapılandırılır, ancak anahtar değerlerine açıkça yeni örnekleri için ayarlanan uygulama bozulabilir.
Düzeltme üretilmiş değerleri anahtar özelliklerine açıkça yapılandırmaktır.
Örneğin, fluent API'si ile:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

Veya veri ek açıklamaları:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a>Art arda silme işlemi hemen hemen varsayılan olarak gerçekleşir

[İzleme sorun #10114](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.

**Eski davranışı**

SaveChanges çağrıldı kadar EF Core uygulanan basamaklı eylemlerinin (gerekli sorumlu silindiğinde veya ilişki gerekli sorumlusuna yazıyordunuz bağımlı varlıkları silmek) 3.0 önce gerçekleşmedi.

**Yeni davranış**

Tetikleme koşulu algılandığında hemen sonra 3.0 ile başlayarak, EF Core basamaklı eylemlerinin geçerlidir.
Örneğin, çağırma `context.Remove()` asıl bir varlığı silme tüm izlenen sonucu ilgili gerekli bağımlılıklar da ayarlanacak şekilde `Deleted` hemen.

**Neden**

Veri bağlama ve hangi varlıkları silinecek anlamanız önemli olduğu senaryolar denetimi deneyimini geliştirmek için bu değişiklik yapılmıştır _önce_ `SaveChanges` çağrılır.

**Risk azaltma işlemleri**

Davranış ayarları aracılığıyla geri yüklenebilir `context.ChangedTracker`.
Örneğin:

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a>Sorgu türleri varlık türleri ile birleştirilir.

[İzleme sorun #14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce [sorgu türü](xref:core/modeling/query-types) olan birincil anahtar, yapılandırılmış bir biçimde tanımlamıyor verileri sorgulamak için bir anlamına gelir.
Diğer bir deyişle, bir sorgu türü kullanıldı sırasında normal bir varlık anahtarları (büyük olasılıkla bir görünümden, ancak büyük olasılıkla bir tablodan) olmadan varlık türleriyle eşlemek için bir anahtar kullanılabilir (daha büyük olasılıkla bir tablodan ancak büyük olasılıkla bir görünümden) olduğunda türü kullanıldı.

**Yeni davranış**

Bir sorgu türü artık yalnızca birincil anahtarı olmayan bir varlık türü olur.
Anahtarsız varlık türleri, önceki sürümlerde sorgu türleri aynı işlevselliğe sahiptir.

**Neden**

Sorgu türleri amacı etrafında karışıklık azaltmak için bu değişiklik yapılmıştır.
Özellikle, anahtarsız varlık türleridir ve bu nedenle kendiliğinden salt okunur olduklarından, ancak yalnızca bir varlık türü salt okunur olması gerekir çünkü bunlar kullanılmamalıdır.
Benzer şekilde, genellikle görünümlerine eşlenmiş, ancak yalnızca görünümleri genellikle anahtarlar tanımlama yok olmasıdır.

**Risk azaltma işlemleri**

Aşağıdaki bölümleri API artık kullanılmıyor:
* **`ModelBuilder.Query<>()`** -Bunun yerine `ModelBuilder.Entity<>().HasNoKey()` hiçbir anahtarına sahip bir varlık türü işaretlemek için çağrılması gerekir.
Bu yine de bir birincil anahtar bekleniyordu, ancak kuralı eşleşmiyor, yanlış yapılandırma önlemek için kural olarak yapılandırılamadığı.
* **`DbQuery<>`** -Bunun yerine `DbSet<>` kullanılmalıdır.
* **`DbContext.Query<>()`** -Bunun yerine `DbContext.Set<>()` kullanılmalıdır.

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a>Sahip olunan tür ilişkileri için API yapılandırması değişti

[Sorun #12444 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sorun #9148 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[sorun #14153 izleme](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce hemen sonra yapılandırma sahibi ilişkinin gerçekleştirildi `OwnsOne` veya `OwnsMany` çağırın. 

**Yeni davranış**

EF Core 3.0 ile başlayarak, var. Şimdi fluent API'sini kullanarak sahibine bir gezinti özelliğini yapılandırmak için `WithOwner()`.
Örneğin:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

Yapılandırma sahibi arasındaki ilişkiyi ilgili ve ait hemen sonra zincirlenmesi gereken `WithOwner()` diğer ilişkileri nasıl yapılandırılacağını benzer şekilde.
Sahip olunan türü hala sonra zincirlenmesine için yapılandırma while `OwnsOne()/OwnsMany()`.
Örneğin:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

Ayrıca çağırma `Entity()`, `HasOne()`, veya `Set()` sahip olunan bir türü ile hedef şimdi bir özel durum oluşturmaz.

**Neden**

Sahip olunan türü yapılandırma arasında daha net bir ayrım oluşturmak için bu değişiklik yapılmıştır ve _ilişkisi_ ait türü.
Bu sırayla belirsizlik ve yöntemler gibi geçici karışıklık kaldırır `HasForeignKey`.

**Risk azaltma işlemleri**

Yukarıdaki örnekte gösterildiği gibi yeni bir API yüzeyi kullanmak için sahip olunan tür ilişkileri yapılandırmasını değiştirin.

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>Yabancı anahtar özellik kuralı asıl özelliğiyle aynı ada artık eşleşmiyor.

[İzleme sorun #13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.

**Eski davranışı**

Şu model göz önünde bulundurun:
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}

```
EF Core 3.0 önce `CustomerId` özelliği kullanılan Kural gereği için yabancı anahtarı.
Ancak, varsa `Order` bu da yapacağı sonra sahip olunan bir türü olan `CustomerId` birincil anahtar ve bu değilse genellikle beklentisi.

**Yeni davranış**

3.0 ile başlayarak, EF Core asıl özelliğiyle aynı ada sahip oldukları özellikleri yabancı anahtarlar için kural olarak kullanılacak denemez.
Asıl tür adı asıl özellik adıyla birleştirilmiş ve asıl özellik adı desenleri ile birleştirilmiş gezinme adı hala eşleştirilir.
Örneğin:

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**Neden**

Bu değişikliği yanlışlıkla bir birincil anahtar özelliği sahip olunan türüne tanımlamamaya özen gösterin çalışıldı.

**Risk azaltma işlemleri**

Yabancı anahtar olması ve bu nedenle birincil anahtarın sonra açıkça bölüm özelliği isteniyorsa bu şekilde yapılandırın.

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Her bir özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır.

[İzleme sorun #6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.

**Eski davranışı**

EF Core 3.0 önce tüm bellek içi tamsayı anahtar özellikleri için bir paylaşılan değer Oluşturucu kullanıldı.

**Yeni davranış**

EF Core 3.0 ile başlayarak, her bir tamsayı anahtar özellik değer Oluşturucusu kendi bellek içi veritabanı kullanılırken alır.
Ayrıca, veritabanı silinirse, anahtar oluşturma için tüm tabloları sıfırlanır.

**Neden**

Bellek içi anahtar oluşturma gerçek veritabanı anahtar oluşturma için daha yakından Hizala ve bellek içi veritabanı kullanılırken testler birbirinden ayırma özelliğini artırmak için bu değişiklik yapılmıştır.

**Risk azaltma işlemleri**

Bunu ayarlamak için belirli bellek içi anahtar değerlerine bağlı olan bir uygulama bozulabilir.
Göz önünde bulundurun yerine değil özel anahtar değerlerine bağlı olan veya yeni davranış eşleştirilecek güncelleştiriliyor.

## <a name="backing-fields-are-used-by-default"></a>Varsayılan olarak kullanılan destekleyen alanlar

[İzleme sorun #12430](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

3.0 önce bir özellik için destek alanı biliniyordu olsa bile, EF Core yine de varsayılan olarak okuma ve özellik alıcı ve ayarlayıcı yöntemleri kullanarak özellik değeri yazma.
Bunun özel durumu burada destek alanı doğrudan biliniyorsa ayarlamak sorgu yürütme oluştu.

**Yeni davranış**

Bir özellik için destek alanı biliniyorsa, EF Core 3.0 ile başlayan sonra her zaman okuyabilmesini ve yazma yedekleme alanını kullanarak bu özelliği.
Uygulama, alıcı veya ayarlayıcı yöntemleri kodlanmış ek davranış bağlı değilse bu bir uygulama sonu neden olabilir.

**Neden**

Bu değişiklik iş mantığı deneyebileceğinizi olarak tetikleyen EF Core önlemek için varsayılan olarak varlıkları içeren bir veritabanı işlemleri gerçekleştirirken yapılmıştır.

**Risk azaltma işlemleri**

ModelBuilder fluent API'si özellik erişim modunda yapılandırmasını aracılığıyla öncesi 3.0 davranış geri yüklenebilir.
Örneğin:

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Birden çok uyumlu yedekleme alan bulunamazsa throw

[İzleme sorun #12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.

**Eski davranışı**

Birden çok alan karşılarsa, EF Core 3.0 önce bazı öncelik sırası üzerinde bir alan seçilirdi sonra bir özelliğinin destek alanı bulma kurallarını temel.
Bu, belirsiz durumda kullanılacak yanlış alan neden olabilir.

**Yeni davranış**

Birden çok alan aynı özelliğe eşleşirse EF Core 3.0 ile başlayarak, bir özel durum oluşturulur.

**Neden**

Yalnızca biri doğru olduğunda sessiz bir şekilde bir alanı başka bir kullanmaktan kaçınmak için bu değişiklik yapılmıştır.

**Risk azaltma işlemleri**

Belirsiz destekleyen alanlarla özellikleri, açıkça belirtilen kullanılacak bir alan olması gerekir.
Örneğin, fluent API'sini kullanarak:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağırın

[İzleme sorun #14756](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.

**Eski davranışı**

EF Core 3.0, çağırma önce `AddDbContext` veya `AddDbContextPool` de günlüğe kaydetme ve bellek D.I hizmetleriyle yapılan çağrılar aracılığıyla önbelleğe kaydetmek [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

**Yeni davranış**

EF Core 3.0 ile başlayan `AddDbContext` ve `AddDbContextPool` artık kayıt hizmetlerin bağımlılık ekleme (dı) sahip olur.

**Neden**

EF Core 3.0, bu hizmetler, uygulamanın DI cotainer olduğunu gerektirmez. Ancak, varsa `ILoggerFactory` uygulamanın DI kapsayıcısında kayıtlı EF Core tarafından kullanılır.

**Risk azaltma işlemleri**

Uygulamanız bu hizmetlere ihtiyacı varsa, sonra bunları açıkça DI kullanarak kapsayıcı kayıt [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) veya [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext.Entry artık yerel DetectChanges gerçekleştirir

[İzleme sorun #13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.

**Eski davranışı**

EF Core 3.0, çağırma önce `DbContext.Entry` izlenen tüm varlıklar için algılanabilir değişiklik neden olabilir.
Bu durumu içinde kullanıma sunulan sağlamış `EntityEntry` güncel oluştu.

**Yeni davranış**

Çağırma EF Core ile 3.0, başlangıç `DbContext.Entry` ilgili asıl varlıkları yalnızca belirtilen varlık ve diğer değişikliklerini algılamak girişimi izlenen başlatılacak.
Başka bir yerde değiştirir Bunun anlamı etkileri uygulama durumu olabilir. Bu yöntemi çağrılarak algılandı değil.

Unutmayın `ChangeTracker.AutoDetectChangesEnabled` ayarlanır `false` sonra bile bu yerel değişiklik algılama devre dışı bırakılır.

Değişiklik algılama--örneğin neden diğer yöntemleri `ChangeTracker.Entries` ve `SaveChanges`--yine de tam neden `DetectChanges` tüm varlıkları izlenir.

**Neden**

Kullanmanın varsayılan performansını artırmak için bu değişiklik yapılmıştır `context.Entry`.

**Risk azaltma işlemleri**

Çağrı `ChgangeTracker.DetectChanges()` açıkça çağırmadan önce `Entry` öncesi 3.0 davranış sağlamak için.

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>Dize ve bayt dizisi anahtarlar varsayılan olarak istemci tarafından oluşturulan değildir

[İzleme sorun #14617](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.

**Eski davranışı**

EF Core 3.0 önce `string` ve `byte[]` açıkça null olmayan bir değer ayarlamadan anahtar özellikleri kullanılabilir.
Böyle bir durumda anahtar değeri istemcide bayt için seri hale getirilmiş bir GUID olarak oluşturulacak `byte[]`.

**Yeni davranış**

Bir özel durum EF Core 3.0 ile başlayarak, anahtar değer kümesi gösteren oluşturulur.

**Neden**

Bu değişiklik nedeniyle istemci tarafından oluşturulan yapılmıştır `string` / `byte[]` değerleri genellikle olmadığı kullanışlı ve varsayılan davranışını yaptığı sabit nedeni hakkında oluşturulan anahtar değerler için yaygın bir şekilde.

**Risk azaltma işlemleri**

Başka bir null olmayan değer ayarlarsanız anahtar özellikleri oluşturulan değerler kullanması gerektiğini açıkça belirterek öncesi 3.0 davranış elde edilebilir.
Örneğin, fluent API'si ile:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

Veya veri ek açıklamaları:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a>Kapsamlı bir hizmet Iloggerfactory sunulmuştur

[İzleme sorun #14698](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce `ILoggerFactory` tek bir hizmet kaydedildi.

**Yeni davranış**

EF Core 3.0 ile başlayan `ILoggerFactory` şimdi kapsamlı olarak kaydedilir.

**Neden**

Günlükçü ile ilişkisini izin vermek için bu değişiklik yapılmıştır bir `DbContext` diğer işlevleri sağlar ve bazı durumlarda pathological davranış gibi iç hizmet sağlayıcıları sayısındaki kaldıran örneği.

**Risk azaltma işlemleri**

Bu değişiklik, kaydetme ve EF Core iç hizmet sağlayıcısına özel hizmetler kullanarak sürece uygulama kodu etkilememesi.
Bu ortak değildir.
Bu durumlarda, bilgilerin çoğunu hala çalışır, ancak bağlı olarak herhangi bir tekil hizmeti olacak `ILoggerFactory` almak için değiştirilmesi gerekecektir `ILoggerFactory` farklı bir yolla.

Bu gibi durumlarda karşılaşırsanız lütfen sırasında bir sorun üzerinde dosya [EF Core GitHub sorun İzleyicisi](https://github.com/aspnet/EntityFrameworkCore/issues) nasıl kullandığınızı bize bildirmek `ILoggerFactory` sağlayacak şekilde biz bunu gelecekte kesmemesi nasıl daha iyi anlamak.

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a>IDbContextOptionsExtensionWithDebugInfo IDbContextOptionsExtension birleştirilir.

[İzleme sorun #13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.

**Eski davranışı**

`IDbContextOptionsExtensionWithDebugInfo` ek isteğe bağlı bir arabirim gelen genişletilmişse `IDbContextOptionsExtension` arabirimine 2.x sürüm döngüsü sırasında değiştirme bir hataya neden önlemek için.

**Yeni davranış**

Arabirimler artık birlikte birleştirilir `IDbContextOptionsExtension`.

**Neden**

Arabirimler kavramsal olarak biri olduğundan, bu değişiklik yapılmıştır.

**Risk azaltma işlemleri**

Tüm uygulamaları `IDbContextOptionsExtension` yeni üye destekleyecek şekilde güncelleştirilmesi gerekir.

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>Gezinti özellikleri tam olarak yüklü olduğundan yükleme Lazy proxy'leri artık varsayılır

[İzleme sorun #12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.

**Eski davranışı**

EF Core 3.0, bir kez önce bir `DbContext` bırakıldı veya bir belirtilen gezinme özelliğinin bir varlığı söz konusu bağlamdan elde tam yüklü ise, olduğunu bilmesinin imkanı yoktu.
Proxy'leri bunun yerine null olmayan bir değer varsa bir başvuru Gezinti yüklenir ve boş değilse, bir koleksiyon Gezinti yüklenen varsayılır.
Bu durumlarda, yavaş dışarıdan yüklemeyi deneyen bir İşlemsiz olacaktır.

**Yeni davranış**

EF Core 3.0 ile başlayarak, proxy'leri bir gezinti özelliğinin yüklü olup olmadığını izler.
Bu yüklenen Gezinti boş veya null olduğunda bile bağlamı bırakıldıktan sonra yüklenen bir gezinti özelliği erişmeye çalışan her zaman bir İşlemsiz anlamına gelir.
Buna karşılık, boş bir koleksiyon gezinme özelliği olsa bile bağlamı elden yüklü olmayan bir gezinti özelliği erişmeye çalışan bir özel durum oluşturur.
Bu durum ortaya çıkarsa, uygulama kodu, geçersiz bir zamanda yavaş yükleme kullanmak çalışıyor ve bunu yapmak için uygulama değiştirilmelidir anlamına gelir.

**Neden**

Bu değişiklik, bırakılmış yükü yavaş çalışırken davranışını tutarlı ve doğru hale yapılmıştır `DbContext` örneği.

**Risk azaltma işlemleri**

Uygulama kodunun yükleme yavaş bırakılmış bağlamla kullanmamanız güncelleştirin veya bu özel durum iletisini açıklandığı bir İşlemsiz olacak şekilde yapılandırın.

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>İç hizmet sağlayıcılarının aşırı oluşturma artık varsayılan olarak bir hata

[İzleme sorun #10236](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce bir uyarı iç hizmet sağlayıcıları pathological bir dizi oluşturmak için bir uygulama oturum açmış olmanız.

**Yeni davranış**

EF Core 3.0 ile başlayarak, bu uyarı artık olarak kabul edilir ve hata ve özel durum harekete geçirilir. 

**Neden**

Bu değişiklik, daha açık pathological bu durumda gösterme yoluyla uygulama kodu daha iyi desteklemek üzere yapılmıştır.

**Risk azaltma işlemleri**

Bu hata ile karşılaşma eylemde en uygun nedeni, kök nedenini anlamak ve çok sayıda iç hizmet sağlayıcıları oluşturmayı durdur oluşturmaktır.
Ancak, hata bir uyarı olarak geri dönüştürülür (veya yok sayıldı) aracılığıyla yapılandırma `DbContextOptionsBuilder`.
Örneğin:

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>Tek bir dize ile HasOne/çok ilişkin yeni davranış çağırılır

[İzleme sorun #9171](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

Bu değişiklik, EF Core 3.0-preview 4 sürümünde sunulacaktır.

**Eski davranışı**

EF Core 3.0 önce kod arama `HasOne` veya `HasMany` tek bir dize ile yorumlandığı kafa karıştırıcı bir şekilde oluştu.
Örneğin:
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

Kod ilgili Görülüyor `Samurai` bazı diğer varlık türü kullanarak `Entrance` özel gezinme özelliği.

Bazı varlık türü olarak adlandırılan bir ilişki oluşturmak bu kodu gerçekte çalışır `Entrance` ile gezinti özelliği yok.

**Yeni davranış**

EF Core 3.0 ile başlayarak, yukarıdaki kod artık önce yapmakta gibi hangi Aranan yapar.

**Neden**

Eski davranışı özellikle yapılandırma kodu okurken ve hatalarını arayarak çok karmaşık.

**Risk azaltma işlemleri**

Bu, yalnızca tür adları ve gezinme özelliğini açıkça belirtmeden dizeleriyle ilişkileri açıkça yapılandırmakta olduğunuz uygulamalar keser.
Bu yaygın değildir.
Önceki davranışı açıkça geçirme aracılığıyla alınabilir `null` gezinme özelliğinin adı.
Örneğin:

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>İlişkisel: TypeMapping ek açıklama yalnızca TypeMapping sunulmuştur

[İzleme sorun #9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

Bu değişiklik EF Core 3.0-preview 2 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

Tür eşlemesi ek açıklamaları için ek açıklama "İlişkisel: TypeMapping" adlandırılmıştı.

**Yeni davranış**

Ek açıklama türü eşleme ek açıklamalar için artık "TypeMapping" addır.

**Neden**

Türü eşlemeleri artık birden fazla yalnızca ilişkisel veritabanı sağlayıcıları için kullanılır.

**Risk azaltma işlemleri**

Bu, yalnızca tür eşlemesi ortak olmayan doğrudan bir ek açıklama, olarak erişen uygulamalar keser.
API yüzeyi için ek açıklama doğrudan kullanmak yerine erişim türü eşlemeleri düzeltmek için en uygun eylemi kullanmaktır.

## <a name="totable-on-a-derived-type-throws-an-exception"></a>Türetilmiş bir tür üzerinde ToTable bir özel durum oluşturur. 

[İzleme sorun #11811](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce `ToTable()` türetilmiş üzerinde adlı türü yoksayılan beri tek devralma eşleme stratejisi TPH burada bu geçerli değil. 

**Yeni davranış**

EF Core 3.0 ve sonraki bir sürümde TPT ve TPC desteği eklemek için hazırlık başlangıç `ToTable()` beklenmeyen eşleme değişiklik gelecekte önlemek için bir özel durum bir türetilmiş tür şimdi throw üzerinde çağrılır.

**Neden**

Şu anda farklı bir tabloya türetilmiş bir tür eşlemek için geçerli değil.
Bu değişiklik, gelecekte yapılacak geçerli bir şey olduğunda bozucu önler.

**Risk azaltma işlemleri**

Türetilen türlerin diğer tablolara eşleme girişimleri kaldırın.

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a>ForSqlServerHasIndex HasIndex ile değiştirildi 

[İzleme sorun #12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce `ForSqlServerHasIndex().ForSqlServerInclude()` ile kullanılan sütunları yapılandırmak üzere bir yöntem sağlayan `INCLUDE`.

**Yeni davranış**

EF Core 3.0 ile başlayarak, kullanarak `Include` dizin ilişkisel düzeyinde artık desteklenmektedir.
Kullanım `HasIndex().ForSqlServerInclude()`.

**Neden**

API ile dizin için birleştirmek için bu değişiklik yapılmıştır `Includes` tüm sağlayıcıları veritabanı için içine yerleştirin.

**Risk azaltma işlemleri**

Yukarıda da gösterildiği gibi yeni API'yi kullanın.

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>EF Core artık SQLite FK zorlama pragma gönderir

[İzleme sorun #12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

Bu değişiklik, EF Core 3.0-preview 3 sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce EF Core gönderir gibi `PRAGMA foreign_keys = 1` SQLite bağlantısı açıldığında.

**Yeni davranış**

EF Core 3.0 ile başlayarak, EF Core artık gönderir `PRAGMA foreign_keys = 1` SQLite bağlantısı açıldığında.

**Neden**

EF Core kullandığından bu değişiklik yapılmıştır `SQLitePCLRaw.bundle_e_sqlite3` varsayılan olarak, hangi sırayla anlamına gelir FK zorlama varsayılan olarak açık ve açıkça bir bağlantı her açıldığında etkinleştirilmesi gerekmez.

**Risk azaltma işlemleri**

Yabancı anahtarlar, varsayılan olarak EF Core için kullanılan SQLitePCLRaw.bundle_e_sqlite3 varsayılan olarak etkindir.
Diğer durumlarda, yabancı anahtarlar belirterek etkinleştirilebilir `Foreign Keys=True` bağlantı dizenizi içinde.

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a>Microsoft.EntityFrameworkCore.Sqlite üzerinde SQLitePCLRaw.bundle_e_sqlite3 artık bağlıdır.

**Eski davranışı**

EF Core 3.0 önce EF Core kullanılan `SQLitePCLRaw.bundle_green`.

**Yeni davranış**

EF Core 3.0 ile başlayarak, EF Core kullanan `SQLitePCLRaw.bundle_e_sqlite3`.

**Neden**

Diğer platformlar ile tutarlı ios'ta kullanılan SQLite sürümü, bu değişiklik yapılmıştır.

**Risk azaltma işlemleri**

İos'ta yerel bir SQLite sürümü kullanmak için yapılandırma `Microsoft.Data.Sqlite` farklı bir kullanılacak `SQLitePCLRaw` paket.

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Char değerleri artık SQLite metin olarak depolanır

[İzleme sorun #15020](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

Char değerleri daha önce SQLite tamsayı değerleri olarak sored. Örneğin, bir karakter değerini *A* 65 tamsayı değeri depolanmıştır.

**Yeni davranış**

Char değerleri sotred metin olarak sunulmuştur.

**Neden**

Değerlerin metin olarak depolanması daha doğal ve veritabanı diğer teknolojiler ile daha uyumlu olmasını sağlar.

**Risk azaltma işlemleri**

Aşağıdaki gibi SQL yürüterek veritabanlarında yeni biçime geçiş yapabilirsiniz.

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

EF Core de önceki davranışı configuirng bir değer dönüştürücü bu özellikleri kullanmaya devam edebilirsiniz.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

Microsoft.Data.Sqlite Ayrıca belirli senaryoları herhangi bir eylem gerekli değil şekilde karakter değerleri hem tamsayı hem de metin sütunları, okuma özellikli kalır.

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a>Geçiş kimlikleri, artık sabit kültürün takvimini kullanarak oluşturulur

[İzleme sorun #12978](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

Geçiş kimlikleri currret kültürün takvimini kullanarak oluşturulan inadvertantly yoktu.

**Yeni davranış**

Geçiş kimlikleri artık her zaman sabit kültürün takvimini (Gregoryen) kullanılarak oluşturulur.

**Neden**

Geçiş sırası önemlidir veritabanını güncelleştirmek ya da birleştirme çakışmalarını çözme. Sabit takvimi kullanılarak sıralama ekip üyelerinden farklı sistem takvimler sahip sonuçlanabilen sorunları önler.

**Risk azaltma işlemleri**

Bu değişiklik olmayan Gregoryen takvimdeki bir yılın Gregoryen takvimini (içeren Thai Budist takvimi gibi) daha büyük olduğu kullanan herkesin etkiler. Mevcut geçiş kimlikleri böylece mevcut sonra yeni geçişleri sıralı güncelleştirilmesi gerekiyor geçişler.

Geçiş kimliği geçiş özniteliğinde geçişler Tasarımcı dosyalarında bulunabilir.

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

Geçişleri geçmiş tablosu da güncelleştirilmesi gerekiyor.

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```
