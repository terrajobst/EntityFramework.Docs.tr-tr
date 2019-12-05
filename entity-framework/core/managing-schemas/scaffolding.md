---
title: Tersine mühendislik-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 1ba9352d261f1c131b0d70f8cdad2128d9afaefe
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824459"
---
# <a name="reverse-engineering"></a>Tersine mühendislik

Tersine mühendislik, yapı iskelesi varlık türü sınıfları ve veritabanı şemasını temel alan bir DbContext sınıfı işlemidir. Bu, EF Core Paket Yöneticisi Konsolu (PMC) araçlarının `Scaffold-DbContext` komutu veya .NET komut satırı arabirimi (CLı) araçlarının `dotnet ef dbcontext scaffold` komutu kullanılarak gerçekleştirilebilir.

## <a name="installing"></a>Yükleme

Ters mühendisden önce, [PMC araçları](xref:core/miscellaneous/cli/powershell) 'nı (yalnızca Visual Studio) veya [CLI araçlarını](xref:core/miscellaneous/cli/dotnet)yüklemeniz gerekir. Ayrıntılar için bkz. bağlantılar.

Ayrıca, ters mühendislik uygulamak istediğiniz veritabanı şeması için uygun bir [veritabanı sağlayıcısı](xref:core/providers/index) yüklemeniz gerekir.

## <a name="connection-string"></a>Bağlantı dizesi

Komutun ilk bağımsız değişkeni veritabanına yönelik bir bağlantı dizesidir. Araçlar, veritabanı şemasını okumak için bu bağlantı dizesini kullanır.

Bağlantı dizesini nasıl teklifiniz ve kaçış, komutu yürütmek için kullandığınız kabuğa bağlıdır. Ayrıntılar için kabuğunuzun belgelerine bakın. Örneğin, PowerShell `$` karakterden kaçar, ancak `\`içermemelidir.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

```dotnetcli
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>Yapılandırma ve Kullanıcı gizli dizileri

Bir ASP.NET Core projeniz varsa, yapılandırmadan bağlantı dizesini okumak için `Name=<connection-string>` sözdizimini kullanabilirsiniz.

Bu, veritabanı parolanızı kod tabanınızdan ayrı tutmak için [gizli dizi yöneticisi aracıyla](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) iyi bir şekilde çalışacaktır.

```dotnetcli
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>Sağlayıcı adı

İkinci bağımsız değişken sağlayıcı adıdır. Sağlayıcı adı, genellikle sağlayıcının NuGet paket adıyla aynıdır.

## <a name="specifying-tables"></a>Tabloları belirtme

Veritabanı şemasındaki tüm tablolar varsayılan olarak varlık türlerine ters mühendislik yapılır. Şemaları ve tabloları belirterek hangi tabloların tersine mühendislik uygulanabilir olduğunu sınırlayabilirsiniz.

PMC 'deki `-Schemas` parametresi ve CLı 'daki `--schema` seçeneği bir şema içindeki her tabloyu dahil etmek için kullanılabilir.

`-Tables` (PMC) ve `--table` (CLı), belirli tabloları dahil etmek için kullanılabilir.

PMC 'de birden çok tablo eklemek için bir dizi kullanın.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

CLı 'ya birden çok tablo eklemek için, seçeneği birden çok kez belirtin.

```dotnetcli
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>Adları koruma

Tablo ve sütun adları, varsayılan olarak türler ve özellikler için .NET adlandırma kurallarıyla daha iyi eşleşecek şekilde düzeltilir. `-UseDatabaseNames` anahtarının PMC 'de belirtilmesi veya CLı 'daki `--use-database-names` seçeneğinde, özgün veritabanı adlarını mümkün olduğunca koruyan bu davranış devre dışı bırakılır. Geçersiz .NET tanımlayıcıları düzeltilmeye devam eder ve gezinme özellikleri gibi birleştirilmiş adlar yine de .NET adlandırma kurallarıyla uyumlu olacaktır.

## <a name="fluent-api-or-data-annotations"></a>Akıcı API veya veri ek açıklamaları

Varlık türleri, varsayılan olarak akıcı API kullanılarak yapılandırılır. Bunun yerine, mümkün olduğunda veri ek açıklamalarını kullanmak için `-DataAnnotations` (PMC) veya `--data-annotations` (CLı) belirtin.

Örneğin, akıcı API 'YI kullanmak bu şekilde ele alınacaktır:

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

Veri ek açıklamalarını kullanırken bu işlem aşağıdaki şekilde ele alınacaktır:

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>DbContext adı

Scafkatlı DbContext sınıfı adı, varsayılan olarak *bağlam* ile düzeltilen veritabanı adının adı olacaktır. Farklı bir tane belirtmek için, `-Context` PMC ve `--context` CLı içinde kullanın.

## <a name="directories-and-namespaces"></a>Dizinler ve ad alanları

Varlık sınıfları ve bir DbContext sınıfı, projenin kök dizinine yönelik yapı ve projenin varsayılan ad alanını kullanır. Sınıfların `-OutputDir` (PMC) veya `--output-dir` (CLı) kullanarak tanılanlardaki dizini belirtebilirsiniz. Ad alanı, kök ad alanı ve projenin kök dizini altındaki tüm alt dizinlerin adları olacaktır.

DbContext sınıfını varlık türü sınıflarından ayrı bir dizine dönüştürmek için `-ContextDir` (PMC) ve `--context-dir` (CLı) de kullanabilirsiniz.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

```dotnetcli
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>Nasıl çalışır?

Tersine mühendislik, veritabanı şemasını okuyarak başlar. Tablolar, sütunlar, kısıtlamalar ve dizinler hakkında bilgi okur.

Daha sonra, şema bilgilerini bir EF Core modeli oluşturmak için kullanır. Tablolar varlık türleri oluşturmak için kullanılır; sütunları özellik oluşturmak için kullanılır; ve yabancı anahtarlar ilişki oluşturmak için kullanılır.

Son olarak, model kod oluşturmak için kullanılır. Aynı modeli uygulamanızdan yeniden oluşturmak için karşılık gelen varlık türü sınıfları, akıcı API ve veri ek açıklamaları yapı iskelesi yapılır.

## <a name="limitations"></a>Sınırlamalar

* Bir modelle ilgili her şey, bir veritabanı şeması kullanılarak gösterilebilir. Örneğin, [**Devralma hiyerarşileri**](../modeling/inheritance.md), [**sahip olunan türler**](../modeling/owned-entities.md)ve [**tablo bölme**](../modeling/table-splitting.md) hakkında bilgiler veritabanı şemasında yok. Bu nedenle, bu yapılar hiçbir şekilde tersine mühendislik olmayacaktır.
* Ayrıca, **bazı sütun türleri** EF Core sağlayıcısı tarafından desteklenmeyebilir. Bu sütunlar modele eklenmeyecek.
* Aynı anda iki kullanıcının aynı varlığı güncelleştirmesini engellemek için bir EF Core modelinde [**eşzamanlılık belirteçleri**](../modeling/concurrency.md)tanımlayabilirsiniz. Bazı veritabanlarının bu tür bir sütunu (örneğin, SQL Server) temsil etmesi için özel bir türü vardır; bu durumda bu bilgilere ters mühendislik uygulanabilir. Ancak, diğer eşzamanlılık belirteçleri tersine mühendislik uygulanmaz.
* [8 C# Nullable başvuru türü özelliği](/dotnet/csharp/tutorials/nullable-reference-types) Şu anda ters mühendislik içinde desteklenmiyor: EF Core her zaman özelliğin C# devre dışı olduğunu varsayan kodu üretir. Örneğin, null yapılabilir metin sütunları, bir özelliğin gerekli olup olmadığını yapılandırmak için kullanılan akıcı API veya veri ek açıklamalarıyla `string?`değil, `string` türü olan bir özellik olarak tanılanacaktır. Yapı iskelesi kodunu düzenleyebilir ve bunları C# null yapılabilir ek açıklamalarla değiştirebilirsiniz. Null yapılabilir başvuru türleri için yapı iskelesi desteği [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520)soruna göre izlenir.

## <a name="customizing-the-model"></a>Modeli özelleştirme

EF Core tarafından oluşturulan kod kodunuz. Bunu değiştirebilirsiniz. Yalnızca aynı modele yeniden mühendislik uygulamanız durumunda yeniden oluşturulur. Yapı iskelesi kodu, veritabanına erişmek için kullanılabilecek *bir* modeli temsil eder, ancak *yalnızca kullanılabilecek tek* model değildir.

Varlık türü sınıflarını ve DbContext sınıfını gereksinimlerinize uyacak şekilde özelleştirin. Örneğin, türleri ve özellikleri yeniden adlandırmayı, devralma hiyerarşileri tanıtmanızı veya bir tabloyu birden çok varlığa bölmeyi seçebilirsiniz. Ayrıca, benzersiz olmayan dizinleri, kullanılmayan dizileri ve gezinti özelliklerini, isteğe bağlı skaler özellikleri ve modelden kısıtlama adlarını da kaldırabilirsiniz.

Ayrıca ek oluşturucular, Yöntemler, özellikler vb. ekleyebilirsiniz. ayrı bir dosyada başka bir kısmi sınıf kullanılıyor. Bu yaklaşım, modele geri dönmek istediğinizde bile geçerlidir.

## <a name="updating-the-model"></a>Model güncelleştiriliyor

Veritabanında değişiklik yaptıktan sonra, EF Core modelinizi bu değişiklikleri yansıtacak şekilde güncelleştirmeniz gerekebilir. Veritabanı değişiklikleri basittir, değişiklikleri EF Core modelinizde el ile yapmak en kolay olabilir. Örneğin, bir tabloyu veya sütunu yeniden adlandırma, bir sütunu kaldırma veya bir sütunun türünü güncelleştirme, kodda yapılacak basit değişikliklerdir.

Ancak, daha önemli değişiklikler, el ile hızlı bir şekilde yapılır. Yaygın olarak kullanılan bir iş akışı, var olan modeli güncelleştirilmiş bir dosyanın üzerine yazmak için `-Force` (PMC) veya `--force` (CLı) kullanarak modele geri doğru mühendislik kullanmaktır.

Diğer bir yaygın olarak istenen özellik, yeniden adlandırmalar, tür hiyerarşileri vs. gibi özelleştirmeyi korurken modeli veritabanından güncelleştirebilme yeteneğidir. Bu özelliğin ilerlemesini izlemek için sorun [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) kullanın.

> [!WARNING]
> Modele yeniden veritabanından geri dönmek isterseniz, dosyalarda yaptığınız tüm değişiklikler kaybedilir.
