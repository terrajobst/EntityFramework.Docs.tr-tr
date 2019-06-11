---
title: EF Core 3.0 - EF Core yeni değişiklikler
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 9112d8d235237e68232aac54453d584af0edb524
ms.sourcegitcommit: b188194a1901f4d086d05765cbc5c9b8c9dc5eed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66829490"
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

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

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

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a>EF Core komut satırı aracı, dotnet ef artık .NET Core SDK'sının bir parçasıdır

[İzleme sorun #14016](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

Bu değişiklik EF Core 3.0-preview 4 ve .NET Core SDK'sını ilgili sürümü kullanıma sunulmuştur.

**Eski davranışı**

3.0 önce `dotnet ef` aracı, .NET Core SDK'yı eklenmiştir ve ek adımlar gerek kalmadan, herhangi bir projeyi komut satırından kullanmaya hazır. 

**Yeni davranış**

3. 0'dan başlayarak .NET SDK'sı yer almaz `dotnet ef` aracı kullanabilmeniz için önce açıkça yerel veya genel bir aracı yüklemek zorunda. 

**Neden**

Bu değişiklik dağıtmak ve güncelleştirmek sağlıyor `dotnet ef` bir düzenli .NET CLI aracı, NuGet üzerindeki EF Core 3.0 de her zaman bir NuGet paketi olarak dağıtılmış bir olgu ile tutarlı olarak.

**Risk azaltma işlemleri**

Geçişleri veya iskele yönetebilmek için bir `DbContext`, yükleme `dotnet-ef` kullanarak `dotnet tool install` komutu.
Örneğin, bir genel araç olarak yüklemek için şu komutu yazabilirsiniz:

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

Aletle şekillerini kullanan bağımlılık olarak bildiren bir proje bağımlılıklarını geri yüklediğinizde de, yerel aracı edinebilirsiniz bir [aracı bildirim dosyası](https://github.com/dotnet/cli/issues/10288).

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a>SQL ve ExecuteSql ExecuteSqlAsync yeniden adlandırıldı

[İzleme sorun #10996](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce bu yöntem adlarının bir normal bir dize ya da SQL ve parametreleri ilişkilendirilmiş bir dize ile çalışmak için aşırı yüklenmiş.

**Yeni davranış**

EF Core 3.0 ile başlayarak, kullanmaktadır `FromSqlRaw`, `ExecuteSqlRaw`, ve `ExecuteSqlRawAsync` burada parametreleri geçirilir ayrı olarak Sorgu dizesinden parametreli bir sorgu oluşturun.
Örneğin:

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

Kullanım `FromSqlInterpolated`, `ExecuteSqlInterpolated`, ve `ExecuteSqlInterpolatedAsync` burada parametreleri geçirilir ilişkilendirilmiş sorgu dizesinin parçası olarak parametreli bir sorgu oluşturun.
Örneğin:

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

Yukarıdaki sorgu her ikisi de aynı SQL parametrelere sahip aynı parametreli SQL üretecektir unutmayın.

**Neden**

Yöntem aşırı yüklemeleri böyle güvenmelidir yanı sıra ilişkilendirilmiş dize yöntemini çağırmak için hedefi olduğu zaman yanlışlıkla ham dize yöntemini çağırmak çok kolay hale getirir.
Bu, verilmiş olması, parametreli getirilemedi sorgularda neden olabilir.

**Risk azaltma işlemleri**

Yeni yöntem adları kullanılacak anahtar.

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a>SQL yöntemleri yalnızca sorgu kökleri üzerinde belirtilebilir

[İzleme sorun #15704](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

Bu değişiklik, EF Core 3.0-Önizleme 6 sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce `FromSql` yöntemi belirtilebilir herhangi bir sorgu.

**Yeni davranış**

EF Core 3.0 ile başlayan yeni `FromSqlRaw` ve `FromSqlInterpolated` yöntemleri (hangi Değiştir `FromSql`) yalnızca sorgu kökleri üzerinde yani doğrudan belirtilebilir `DbSet<>`. Başka bir yerde bunları denemek, bir derleme hatasına neden olur.

**Neden**

Belirtme `FromSql` herhangi bir yere dışında üzerinde bir `DbSet` hiçbir anlamı eklendi veya değer ve belirli senaryolarda karışıklığa neden olabilir.

**Risk azaltma işlemleri**

`FromSql` çağrıları doğrudan açık olmasını taşınması gereken `DbSet` hangi uygulanır.

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

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a>DeleteBehavior.Restrict temizleyici semantiğe sahip

[İzleme sorun #12661](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

Bu değişiklik, EF Core 3.0-preview 5'teki sunulmuştur.

**Eski davranışı**

3.0 önce `DeleteBehavior.Restrict` ile veritabanındaki yabancı anahtarlar oluşturulan `Restrict` semantiği, aynı zamanda açık olmayan bir şekilde değiştirilmiş iç düzeltme.

**Yeni davranış**

3.0 ile başlayan `DeleteBehavior.Restrict` yabancı anahtarlar ile oluşturulan sağlar `Restrict` semantiği--diğer bir deyişle, hiçbir basamaklar; throw EF iç düzeltme de etkilemeden kısıtlama ihlali üzerinde--.

**Neden**

Bu değişiklik deneyimini geliştirmek amacıyla kullanılarak yapıldığını `DeleteBehavior` beklenmeyen yan etkiler olmadan sezgisel bir şekilde.

**Risk azaltma işlemleri**

Kullanarak önceki davranış geri yüklenebilir `DeleteBehavior.ClientNoAction`.

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

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Bağımlı varlıkları tablo sorumlusuyla paylaşımı artık isteğe bağlıdır

[İzleme sorun #9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

Şu model göz önünde bulundurun:
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
3.0, önce EF Core `OrderDetails` tarafından sahip olunan `Order` veya aynı tabloya açıkça eşleştirilmiş bir `OrderDetails` örneği her zaman gerekli yeni bir eklerken `Order`.


**Yeni davranış**

3.0 ile başlayarak, EF Core eklemek için sağlayan bir `Order` olmadan bir `OrderDetails` ve tüm eşler `OrderDetails` özellikleri hariç null yapılabilir sütunlar için birincil anahtarı.
EF Core kümeleri sorgulanırken `OrderDetails` için `null` gerekli özelliklerinden birini yoksa, bir değer veya birincil anahtarı yanı sıra gereken özellikleri yoktur ve tüm özellikleri `null`.

**Risk azaltma işlemleri**

İsteğe bağlı tüm sütunlar eklenmiş bağımlı paylaşımı tablo modelinizi varken işaret Gezinti olması beklenmiyor `null` Gezinti olduğunda durumlarında uygulama değiştirilmelidir sonra `null`. Bu mümkün değilse, varlık türüne gerekli bir özellik eklenmesini veya en az bir özelliğine sahip olmayan bir`null` atanmış değer.

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a>Bir özelliğini eşleştirmek tüm varlıklar bir eşzamanlılık belirteci sütun içeren bir tablo paylaşımı sahip

[İzleme sorun #14154](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

Şu model göz önünde bulundurun:
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
3.0, önce EF Core `OrderDetails` tarafından sahip olunan `Order` veya yalnızca güncelleştirme aynı tabloya açıkça eşlenen `OrderDetails` değil güncelleştirecek `Version` değeri istemcisi ve İleri güncelleştirmeyi başarısız olur.


**Yeni davranış**

EF Core 3.0 ile başlayarak, yeni yayınlar `Version` değerini `Order` bunu sahipse `OrderDetails`. Aksi takdirde model doğrulama sırasında bir özel durum oluşturulur.

**Neden**

Eski eşzamanlılık belirteç değeri aynı tabloya eşlenen varlıkları yalnızca biri güncelleştirildiğinde önlemek için bu değişiklik yapılmıştır.

**Risk azaltma işlemleri**

Eşzamanlılık belirteci sütuna eşlenmiş bir özellik içerir tabloda paylaşımı tüm varlıklar gerekir. Mümkündür oluşturma bir gölge durumu:
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a>Tüm türetilmiş türler için tek bir sütun eşlenmemiş türlerden devralınan özellikler artık eşlenir

[İzleme sorun #13998](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

Şu model göz önünde bulundurun:
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

EF Core 3.0 önce `ShippingAddress` özelliği için sütunları ayırmak için eşlenebilir `BulkOrder` ve `Order` varsayılan olarak.

**Yeni davranış**

3.0 ile başlayarak, EF Core yalnızca bir sütun için oluşturur `ShippingAddress`.

**Neden**

Eski davranışı beklenmiyordu.

**Risk azaltma işlemleri**

Özellik sütunu türetilmiş türler üzerinde ayırmak için açıkça eşlenebilir:

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

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

3.0 ile başlayarak, EF Core asıl özelliğiyle aynı ada sahip oldukları özellikleri için yabancı anahtarlar kurala göre kullanırsanız dener.
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

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a>Veritabanı bağlantısı artık kapalı kullanılmazsa süresi TransactionScope tamamlanmadan önce artık

[İzleme sorun #14218](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

EF Core bağlam içinde bağlantısına açarsa, 3.0 önce bir `TransactionScope`, bağlantı sırasında geçerli açık kalır `TransactionScope` etkindir.

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

**Yeni davranış**

Kullanmaya tamamladıktan hemen sonra 3.0 ile başlayarak, EF Core bağlantıyı kapatır.

**Neden**

Birden fazla bağlamı aynı kullanmak için bu değişiklik sağlayan `TransactionScope`. Yeni davranış da EF6 eşleşir.

**Risk azaltma işlemleri**

Bağlantı açık çağrı açık kalması gerekiyorsa `OpenConnection()` EF Core, erken kapatmadığına emin olun:

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Her bir özellik bağımsız bellek içi tamsayı anahtar oluşturma kullanır.

[İzleme sorun #6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

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

Bir özellik için destek alanı biliniyorsa EF Core 3.0 ile başlayarak, ardından EF Core her zaman okuyabilmesini ve yazma yedekleme alanını kullanarak bu özelliği.
Uygulama, alıcı veya ayarlayıcı yöntemleri kodlanmış ek davranış bağlı değilse bu bir uygulama sonu neden olabilir.

**Neden**

Bu değişiklik iş mantığı deneyebileceğinizi olarak tetikleyen EF Core önlemek için varsayılan olarak varlıkları içeren bir veritabanı işlemleri gerçekleştirirken yapılmıştır.

**Risk azaltma işlemleri**

Öncesi 3.0 davranışı özellik erişim modu konfigürasyonuyla geri yüklenebilir `ModelBuilder`.
Örneğin:

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Birden çok uyumlu yedekleme alan bulunamazsa throw

[İzleme sorun #12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

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

## <a name="field-only-property-names-should-match-the-field-name"></a>Alan adı alanı-yalnızca özellik adlarının eşleşmesi gerekir

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce bir özellik bir dize değeri belirtilebilir ve bu ada sahip hiçbir özellik CLR türüne bulunduysa EF Core kuralı kurallarını kullanarak bir alan için eşleşen deneyin.
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

**Yeni davranış**

EF Core 3.0 ile başlayarak, bir alan özelliği salt okunur alan adı tam olarak eşleşmelidir.

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

**Neden**

Benzer şekilde adlı iki özellik için aynı alanı kullanmaktan kaçınmak için bu değişiklik yapılmıştır, ayrıca eşleşen kuralları yalnızca alan özellikleri için CLR eşleniyor özellikleri aynıdır kolaylaştırır.

**Risk azaltma işlemleri**

Yalnızca alan özellikleri aynı eşleştirildikleri alan olarak adlandırılmalıdır.
EF Core 3.0 daha yeni bir önizleme özelliği adından farklı bir alan adı açıkça yapılandırma yeniden etkinleştirmek planlıyoruz:

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool artık AddLogging ve AddMemoryCache çağırın

[İzleme sorun #14756](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

EF Core 3.0, çağırma önce `AddDbContext` veya `AddDbContextPool` de günlüğe kaydetme ve bellek D.I hizmetleriyle yapılan çağrılar aracılığıyla önbelleğe kaydetmek [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ve [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

**Yeni davranış**

EF Core 3.0 ile başlayan `AddDbContext` ve `AddDbContextPool` artık kayıt hizmetlerin bağımlılık ekleme (dı) sahip olur.

**Neden**

EF Core 3.0, bu hizmetler uygulamanın DI kapsayıcısında olduğunu gerektirmez. Ancak, varsa `ILoggerFactory` uygulamanın DI kapsayıcısında kayıtlı EF Core tarafından kullanılır.

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

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

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

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

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

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce kod arama `HasOne` veya `HasMany` tek bir dize ile kafa karıştırıcı bir şekilde yorumlanır.
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

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a>Birden çok zaman uyumsuz yöntemler için dönüş türü için ValueTask görevden değiştirildi

[İzleme sorun #15184](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

Aşağıdaki zaman uyumsuz yöntemler daha önce döndürülen bir `Task<T>`:

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()` (ve türetilen sınıflar)

**Yeni davranış**

Yukarıda sözü edilen yöntemleri artık döndürür bir `ValueTask<T>` aynı üzerinden `T` önceki gibi.

**Neden**

Bu değişiklik, yığın ayırmaları genel performansını iyileştirme, bu yöntemleri çağrılırken tahakkuk sayısını azaltır.

**Risk azaltma işlemleri**

Yalnızca yukarıdaki API'leri yalnızca bekleyen gerekir derlenmesi için - uygulamalar kaynak değişiklik gereklidir.
Daha karmaşık bir kullanım (örneğin döndürülen geçirme `Task` için `Task.WhenAny()`) genellikle gerektiren döndürülen `ValueTask<T>` dönüştürülmesi bir `Task<T>` çağırarak `AsTask()` üzerindeki.
Bu bu değişiklik getirir ayırma azaltma verilerek unutmayın.

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

API ile dizin için birleştirmek için bu değişiklik yapılmıştır `Include` tüm sağlayıcıları veritabanı için içine yerleştirin.

**Risk azaltma işlemleri**

Yukarıda da gösterildiği gibi yeni API'yi kullanın.

## <a name="metadata-api-changes"></a>Meta veri API'si değişiklikleri

[İzleme sorun #214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Yeni davranış**

Aşağıdaki özellikler için genişletme yöntemleri dönüştürüldü:

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

**Neden**

Bu değişiklik, yukarıda sözü edilen arabirimleri uygulamasını kolaylaştırır.

**Risk azaltma işlemleri**

Yeni uzantı yöntemlerini kullanın.

## <a name="provider-specific-metadata-api-changes"></a>Sağlayıcıya özgü meta veriler API'SİNDEKİ değişiklikler

[İzleme sorun #214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

Bu değişiklik, EF Core 3.0-Önizleme 6 sunulmuştur.

**Yeni davranış**

Sağlayıcıya özgü genişletme yöntemlerini düzleştirilmiş:

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

**Neden**

Bu değişiklik, yukarıda sözü edilen genişletme yöntemleri uygulamasını kolaylaştırır.

**Risk azaltma işlemleri**

Yeni uzantı yöntemlerini kullanın.

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

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a>GUID değerlerinin artık SQLite metin olarak depolanır

[İzleme sorun #15078](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

GUID değerleri, daha önce SQLite değerlerine BLOB olarak sored.

**Yeni davranış**

GUID değerleri, artık metin olarak depolanır.

**Neden**

İkili biçimi GUID'leri standartlaştırılmış değil. Değerlerin metin olarak depolanması veritabanı diğer teknolojiler ile daha uyumlu olmasını sağlar.

**Risk azaltma işlemleri**

Aşağıdaki gibi SQL yürüterek veritabanlarında yeni biçime geçiş yapabilirsiniz.

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

EF Core de bir değer dönüştürücü bu özelliklerini yapılandırarak önceki davranışı kullanarak devam edemedi.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

Microsoft.Data.Sqlite GUID değerlerinin hem BLOB hem de metin sütunları okuma özellikli kalır; Parametreler ve sabitleri için varsayılan biçimi değiştiğinden ancak büyük olasılıkla GUID'leri içeren çoğu senaryo için bir eylem yapmanız gerekir.

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Char değerleri artık SQLite metin olarak depolanır

[İzleme sorun #15020](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

Char değerleri daha önce SQLite tamsayı değerleri olarak sored. Örneğin, bir karakter değerini *A* 65 tamsayı değeri depolanmıştır.

**Yeni davranış**

Char değerleri artık metin olarak depolanır.

**Neden**

Değerlerin metin olarak depolanması daha doğal ve veritabanı diğer teknolojiler ile daha uyumlu olmasını sağlar.

**Risk azaltma işlemleri**

Aşağıdaki gibi SQL yürüterek veritabanlarında yeni biçime geçiş yapabilirsiniz.

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

EF Core de bir değer dönüştürücü bu özelliklerini yapılandırarak önceki davranışı kullanarak devam edemedi.

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

Geçiş kimlikleri, geçerli kültürün takvimini kullanarak yanlışlıkla üretildi.

**Yeni davranış**

Geçiş kimlikleri artık her zaman sabit kültürün takvimini (Gregoryen) kullanılarak oluşturulur.

**Neden**

Geçiş sırası önemlidir veritabanını güncelleştirmek ya da birleştirme çakışmalarını çözme. Sabit takvimi kullanılarak sıralama ekip üyelerinden farklı sistem takvimler sahip sonuçlanabilen sorunları önler.

**Risk azaltma işlemleri**

Bu değişiklik, olmayan-Gregoryen takvim yılı Gregoryen takvimini (içeren Thai Budist takvimi gibi) daha büyük olduğu kullanan herkesin etkiler. Mevcut geçiş kimlikleri böylece mevcut sonra yeni geçişleri sıralı güncelleştirilmesi gerekiyor geçişler.

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

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a>LogQueryPossibleExceptionWithAggregateOperator yeniden adlandırıldı

[İzleme sorun #10985](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Değişiklik**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` yeniden adlandırıldı `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.

**Neden**

Bu uyarı olayı tüm uyarı olayları ile adlandırma hizalar.

**Risk azaltma işlemleri**

Yeni bir ad kullanın. (Olay kimliği numarasını değiştirilmedi unutmayın.)

## <a name="clarify-api-for-foreign-key-constraint-names"></a>API açıklığa kavuşturmak için yabancı anahtar kısıtlaması adları

[İzleme sorun #10730](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

Bu değişiklik EF Core 3.0-preview 4 sürümünde kullanıma sunulmuştur.

**Eski davranışı**

EF Core 3.0 önce yabancı anahtar kısıtlaması adları için yalnızca "adı olarak" adı veriliyordu. Örneğin:

```C#
var constraintName = myForeignKey.Name;
```

**Yeni davranış**

EF Core 3.0 ile başlayarak, yabancı anahtar kısıtlaması adları şimdi de "kısıtlama adı" adlandırılır. Örneğin:

```C#
var constraintName = myForeignKey.ConstraintName;
```

**Neden**

Bu değişiklik, bu alanda adlandırma için tutarlılık getirir ve ayrıca bu yabancı anahtarı üzerinde tanımlanan yabancı anahtar kısıtlaması ve olmayan sütun veya özellik adı adı olduğunu açıklar.

**Risk azaltma işlemleri**

Yeni bir ad kullanın.
