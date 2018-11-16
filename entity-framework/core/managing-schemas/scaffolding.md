---
title: EF Core tersine mühendislik-
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: ef729c0c26d5a1f57099f339eb51cda7e83289df
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688686"
---
# <a name="reverse-engineering"></a>Tersine mühendislik

Tersine mühendislik varlık türü sınıfları ve veritabanı şemasını temel alan bir DbContext sınıfı iskele kurma özelliği işlemidir. Kullanılarak gerçekleştirilebilir `Scaffold-DbContext` EF Core Paket Yöneticisi Konsolu (PMC) Araçları'nın komutunu veya `dotnet ef dbcontext scaffold` .NET komut satırı arabirimi (CLI) araçlarını komutu.

## <a name="installing"></a>Yükleme

Tersine mühendislik önce ya da yüklemeniz gerekir [PMC Araçları](xref:core/miscellaneous/cli/powershell) (yalnızca Visual Studio) veya [CLI Araçları](xref:core/miscellaneous/cli/dotnet). Ayrıntılar için bağlantılara bakın.

Uygun bir yüklemek gerekecektir [veritabanı sağlayıcısı](xref:core/providers/index) tersine mühendislik istediğiniz veritabanı şeması.

## <a name="connection-string"></a>Bağlantı dizesi

İlk bağımsız değişkeni komut, veritabanına bir bağlantı dizesidir. Araçları, veritabanı şemasını okumak için bu bağlantı dizesini kullanır.

Nasıl teklif ve bağlantı dizesini kaçış hangi Kabuk komutu yürütmek için kullandığınız bağlıdır. Özellikleri için kabuğunuzun belgelerine bakın. Örneğin, PowerShell, kaçış gerektirir `$` karakter ancak `\`.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>Yapılandırma ve kullanıcı parolaları

Bir ASP.NET Core projesi varsa, kullanabileceğiniz `Name=<connection-string>` yapılandırmasından bağlantı dizesini okumayı söz dizimi.

Bu şununla düzgün çalışır [gizli dizi Yöneticisi aracını](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) veritabanı parolanızı kod temelinizde ayrı tutmak için.

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>Sağlayıcı adı

İkinci bağımsız değişkeni sağlayıcı adıdır. Sağlayıcı adı genellikle sağlayıcının NuGet paket adı ile aynıdır.

## <a name="specifying-tables"></a>Tabloları belirleme

Veritabanı şema tüm tabloları, varsayılan olarak varlık türleriyle mühendislik ters. Tabloları ters mühendislik şemaları ve tabloları belirterek sınırlayabilirsiniz.

`-Schemas` PMC parametresinde ve `--schema` CLI seçeneğinde, bir şema içinde her tablo eklemek için kullanılabilir.

`-Tables` (PMC) ve `--table` (CLI), belirli tablolar eklemek için kullanılabilir.

Birden çok tablo PMC'de dahil etmek için bir dizi kullanın.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

Birden çok tablo CLI'daki dahil etmek için birden çok kez seçeneğini belirtin.

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>Adları koruma

Tablo ve sütun adları türleri ve özellikleri için .NET adlandırma kuralları daha iyi eşleşecek şekilde varsayılan olarak sabit. Belirtme `-UseDatabaseNames` geçiş PMC'de veya `--use-database-names` CLI seçeneğinde özgün veritabanı adları mümkün olduğunca koruyarak bu davranışı devre dışı bırakır. Geçersiz .NET tanımlayıcıları yine de sabit ve Sentezlenen adları Gezinti özellikleri gibi .NET adlandırma kuralları için yine de uyumlu olacaktır.

## <a name="fluent-api-or-data-annotations"></a>Fluent API'si veya veri ek açıklamaları

Varlık türleri Fluent API'sini kullanarak, varsayılan olarak yapılandırılır. Belirtin `-DataAnnotations` (PMC) veya `--data-annotations` (Bunun yerine, mümkün olduğunda veri ek açıklamaları kullanmak için CLI).

Örneğin, Fluent API'sini kullanarak bu iskelesini.

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

Veri ek açıklamaları kullanırken bu iskelesini.

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>DbContext adı

İskele kurulmuş DbContext sınıfı adı ile ve sonra veritabanının adı olacaktır *bağlam* varsayılan olarak. Farklı bir belirtmek için kullanın `-Context` PMC içinde ve `--context` CLI.

## <a name="directories-and-namespaces"></a>Dizinleri ve ad alanları

Varlık sınıfları ve bir DbContext sınıfı projenin kök dizinine başladınız ve projenin varsayılan ad alanı kullanın. Dizini belirtebilirsiniz sınıfları kullanarak iskele kurulmuş burada `-OutputDir` (PMC) veya `--output-dir` (CLI). Ad alanı, kök ad alanlarının herhangi bir alt dizini proje kök dizininin altında adlarını olacaktır.

Ayrıca `-ContextDir` (PMC) ve `--context-dir` varlık türü sınıflardan ayrı bir dizine DbContext sınıfı iskelesini kurmak (CLI).

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>Nasıl çalışır?

Tersine mühendislik, veritabanı şemasını okuyarak başlatır. Bu, tablolar, sütunlar, kısıtlamalar ve dizinler ilişkin bilgileri okur.

Ardından, EF Core modeli oluşturmak için şema bilgileri kullanır. Tablolar, varlık türleri oluşturmak için kullanılır; sütun özellikleri oluşturmak için kullanılır; ve yabancı anahtarlar ilişkiler oluşturmak için kullanılır.

Son olarak, model, kodu oluşturmak için kullanılır. İlgili varlık türü sınıfları, Fluent API'si ve veri ek açıklamaları, aynı modelden uygulamanızı yeniden oluşturmak için başladınız.

## <a name="what-doesnt-work"></a>İşe yaramazsa

Her şey hakkında bir model kullanarak veritabanı şemasını temsil edilebilir. Örneğin, hakkında bilgi **devralma hiyerarşilerini**, **türlerine ait**, ve **tablo bölme** veritabanı şemasında mevcut değildir. Bu nedenle, bu yapıları hiçbir zaman olması ters mühendislik uygulanan.

Ayrıca, **bazı sütun türleri** EF Core sağlayıcı tarafından desteklenmiyor olabilir. Bu sütunlar, modele dahil edilmeyecektir.

EF Core anahtara sahip her varlık türü gerektirir. Tablolar, ancak birincil anahtarın belirtilmesi gerekmez. **Birincil anahtarı olmayan tablolar** olan şu an ters mühendislik.

Tanımlayabileceğiniz **eşzamanlılık belirteçleri** iki kullanıcı aynı anda aynı varlık güncelleştirmesini engellemek için bir EF Core modelinde. Bazı veritabanları, sütun (örneğin, SQL Server'da rowversion) bu tür, bu durumda biz ters mühendislik bu bilgileri temsil etmek için özel bir türüne sahip; Ancak, diğer eşzamanlılık belirteçleri değil olması ters mühendislik uygulanan.

## <a name="customizing-the-model"></a>Model özelleştirme

EF Core tarafından oluşturulan kodu, kodudur. Bunu değiştirmek çekinmeyin. Yalnızca, aynı modelin yeniden tersine mühendislik, oluşturulacak. İskele kurulan kodu temsil eden *bir* veritabanı, ancak erişmek için kullanılan model değil kesinlikle *yalnızca* kullanılabilmesi için modeli.

Varlık türü sınıfları ve DbContext sınıfı kendi gereksinimlerinize uyacak şekilde özelleştirin. Örneğin, türleri ve özellikleri yeniden adlandır, devralma hiyerarşilerini tanıtmak veya birden fazla varlık için bir tabloya bölme tercih edebilirsiniz. Benzersiz olmayan dizinleri, kullanılmayan dizileri ve gezinti özellikleri, isteğe bağlı bir skaler özellikler ve kısıtlama adları modelden kaldırabilirsiniz.

Ayrıca ekleyebileceğiniz ek Oluşturucular, yöntemler, özellikler, vb. ayrı bir dosyada başka bir kısmi sınıf kullanarak. Bu yaklaşım ters mühendislik modeli yeniden istiyorsanız bile, çalışır.

## <a name="updating-the-model"></a>Bir modeli güncelleştirme

Veritabanına değişiklikleri yaptıktan sonra EF Core modelinizi bu değişiklikleri yansıtacak şekilde güncelleştirmeniz gerekebilir. Veritabanı değişikliklerini basittir, yalnızca değişiklikleri EF Core modelinizi el ile yapmak kolay olabilir. Örneğin, bir tabloyu veya sütunu yeniden adlandırma, bir sütunu kaldırarak veya bir sütunun türünü güncelleştirme Önemsiz kodlarında değişiklik olur.

Önemli değişiklikler, ancak kolay olun el ile değildir. Veritabanını yeniden kullanarak modeli bir ortak iş akışı, ters çevirmek için mühendislik `-Force` (PMC) veya `--force` varolan modeli güncelleştirilmiş bir olanın üzerine yazmak (CLI).

Başka bir yaygın olarak istenen özellik yeniden adlandırma, tür hiyerarşileri, vb. özelleştirme korurken veritabanından modeli güncelleştirmek için yeteneğidir. Sorunu kullanın [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) bu özelliğin ilerlemesini izleyebilirsiniz.

> [!WARNING]
> Veritabanı modeli yeniden, ters mühendislik, dosyalarda yaptığınız tüm değişiklikler kaybolacak.
