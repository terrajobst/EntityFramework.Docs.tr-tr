---
title: EF Core araçları başvurusu (.NET CLı)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 07/11/2019
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 29434c26a503fabb16b43ee8f0c36136a0b5b745
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811964"
---
# <a name="entity-framework-core-tools-reference---net-cli"></a>Entity Framework Core araçları başvurusu-.NET CLı

Entity Framework Core için komut satırı arabirimi (CLı) araçları tasarım zamanı geliştirme görevlerini gerçekleştirmeye yöneliktir. Örneğin, [geçişler](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0)oluşturur, geçişleri uygular ve var olan bir veritabanını temel alan bir model için kod oluşturur. Komutlar, [.NET Core SDK](https://www.microsoft.com/net/core)bir parçası olan platformlar arası [DotNet](/dotnet/core/tools) komutuna bir uzantıdır. Bu araçlar .NET Core projeleriyle birlikte çalışır.

Visual Studio kullanıyorsanız, bunun yerine [Paket Yöneticisi konsol araçları](powershell.md) önerilir:

* Bunlar, el ile dizin geçmeniz gerekmeden, **Paket Yöneticisi konsolunda** seçilen geçerli projeyle otomatik olarak çalışır.
* Komut tamamlandıktan sonra komut tarafından oluşturulan dosyaları otomatik olarak açar.

## <a name="installing-the-tools"></a>Araçları yükleme

Yükleme yordamı proje türüne ve sürümüne bağlıdır:

* EF Core 3. x
* ASP.NET Core sürüm 2,1 ve üzeri
* EF Core 2. x
* EF Core 1. x

### <a name="ef-core-3x"></a>EF Core 3. x

* `dotnet ef`, genel veya yerel bir araç olarak yüklenmelidir. Çoğu geliştirici, `dotnet ef` aşağıdaki komutla küresel bir araç olarak yükler:

  ``` console
  dotnet tool install --global dotnet-ef
  ```

  `dotnet ef` yerel araç olarak da kullanabilirsiniz. Bunu yerel bir araç olarak kullanmak için, bir [araç bildirim dosyası](https://github.com/dotnet/cli/issues/10288)kullanarak bunu araç bağımlılığı olarak bildiren bir projenin bağımlılıklarını geri yükleyin.

* [.NET Core SDK 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0)) yüklemesini yapın. Visual Studio 'nun en son sürümüne sahip olsanız bile SDK 'nın yüklenmesi gerekir.

* En son `Microsoft.EntityFrameworkCore.Design` paketini yükler.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="aspnet-core-21"></a>ASP.NET Core 2.1 +

* Geçerli [.NET Core SDK](https://www.microsoft.com/net/download/core)'yi yükler. Visual Studio 2017 ' nin en son sürümüne sahip olsanız bile SDK 'nın yüklenmesi gerekir.

  `Microsoft.EntityFrameworkCore.Design` paketi [Microsoft. AspNetCore. app metapackage](/aspnet/core/fundamentals/metapackage-app)içine eklendiğinden, bu ASP.NET Core 2.1 + için gereklidir.

### <a name="ef-core-2x-not-aspnet-core"></a>EF Core 2. x (ASP.NET Core değil)

`dotnet ef` komutları .NET Core SDK dahil edilir, ancak `Microsoft.EntityFrameworkCore.Design` paketini yüklemek için sahip olduğunuz komutları etkinleştirmek için.

* Geçerli [.NET Core SDK](https://www.microsoft.com/net/download/core)'yi yükler. Visual Studio 'nun en son sürümüne sahip olsanız bile SDK 'nın yüklenmesi gerekir.

* En son kararlı `Microsoft.EntityFrameworkCore.Design` paketini yükler.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="ef-core-1x"></a>EF Core 1. x

* .NET Core SDK sürümünü 2.1.200 ' ü yükler. Sonraki sürümler EF Core 1,0 ve 1,1 için CLı araçlarıyla uyumlu değildir.

* Uygulamayı, [genel. JSON](/dotnet/core/tools/global-json) dosyasını DEĞIŞTIREREK 2.1.200 SDK sürümünü kullanacak şekilde yapılandırın. Bu dosya normalde çözüm dizinine eklenir (projenin üzerinde bir tane).

* Proje dosyasını düzenleyin ve `DotNetCliToolReference` öğe olarak `Microsoft.EntityFrameworkCore.Tools.DotNet` ekleyin. En son 1. x sürümünü belirtin, örneğin: 1.1.6. Bu bölümün sonundaki proje dosyası örneğine bakın.

* `Microsoft.EntityFrameworkCore.Design` paketinin en son 1. x sürümünü yükler, örneğin:

  ```console
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 1.1.6
  ```

  Paket başvurularının her ikisi de eklendikçe, proje dosyası şuna benzer:

  ``` xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
      <OutputType>Exe</OutputType>
      <TargetFramework>netcoreapp1.1</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
      <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                        Version="1.1.6"
                         PrivateAssets="All" />
    </ItemGroup>
    <ItemGroup>
       <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                              Version="1.1.6" />
    </ItemGroup>
  </Project>
  ```

  `PrivateAssets="All"` olan bir paket başvurusu, bu projeye başvuran projelere gösterilmez. Bu kısıtlama özellikle yalnızca geliştirme sırasında kullanılan paketler için yararlıdır.

### <a name="verify-installation"></a>Yüklemeyi doğrula

EF Core CLı araçlarının düzgün yüklendiğini doğrulamak için aşağıdaki komutları çalıştırın:

  ``` Console
  dotnet restore
  dotnet ef
  ```

Komutun çıktısı kullanımdaki araçların sürümünü tanımlar:

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

Entity Framework Core .NET Command-line Tools 2.1.3-rtm-32065

<Usage documentation follows, not shown.>
```

## <a name="using-the-tools"></a>Araçları kullanma

Araçları kullanmadan önce, bir başlangıç projesi oluşturmanız veya ortamı ayarlamanız gerekebilir.

### <a name="target-project-and-startup-project"></a>Hedef proje ve başlangıç projesi

Komutlar bir *projeye* ve bir *başlangıç projesine*başvurur.

* Ayrıca, komutların dosya eklemesi veya kaldırması nedeniyle *Proje* *hedef proje* olarak da bilinir. Varsayılan olarak, geçerli dizindeki proje hedef projem tir. <nobr>`--project`</nobr> seçeneğini kullanarak, hedef proje olarak farklı bir proje belirtebilirsiniz.

* *Başlangıç projesi* , araçların oluşturup çalıştırdığı bir. Araçlar, veritabanı bağlantı dizesi ve modelin yapılandırması gibi proje hakkında bilgi almak için tasarım zamanında uygulama kodu yürütmeniz gerekir. Varsayılan olarak, geçerli dizindeki proje başlangıç projem ' dir. <nobr>`--startup-project`</nobr> seçeneğini kullanarak, başlangıç projesi olarak farklı bir proje belirtebilirsiniz.

Başlangıç projesi ve hedef proje genellikle aynı projem. Farklı projeler oldukları tipik bir senaryo şunlardır:

* EF Core bağlamı ve varlık sınıfları bir .NET Core sınıf kitaplığında bulunur.
* .NET Core konsol uygulaması veya Web uygulaması, sınıf kitaplığına başvurur.

[Geçiş kodunu, EF Core bağlamından ayrı bir sınıf kitaplığına yerleştirmek](xref:core/managing-schemas/migrations/projects)de mümkündür.

### <a name="other-target-frameworks"></a>Diğer hedef çerçeveler

CLı araçları .NET Core projeleriyle ve .NET Framework projeleriyle çalışır. .NET Standard Sınıf kitaplığındaki EF Core modeli olan uygulamalarda .NET Core veya .NET Framework projesi bulunmayabilir. Örneğin, bu, Xamarin ve Evrensel Windows Platformu uygulamaları için geçerlidir. Bu gibi durumlarda, yalnızca amacı araçlar için başlangıç projesi olarak davranacak bir .NET Core konsol uygulaması projesi oluşturabilirsiniz. Proje, gerçek kod içermeyen bir kukla proje olabilir &mdash; yalnızca araç için bir hedef sağlamanız gerekir.

İşlevsiz bir proje neden gereklidir? Daha önce belirtildiği gibi, araçların uygulama kodunu tasarım zamanında yürütmesi gerekir. Bunu yapmak için .NET Core çalışma zamanını kullanmaları gerekir. EF Core modeli .NET Core veya .NET Framework hedefleyen bir projede olduğunda, EF Core araçları projeden çalışma zamanını ödünç. EF Core modeli .NET Standard bir sınıf kitaplığınlarsa bunu yapamazlar. .NET Standard gerçek bir .NET uygulamasını değil; .NET uygulamalarının desteklemesi gereken bir API kümesine yönelik bir belirtimdir. Bu nedenle .NET Standard uygulama kodunu yürütmek için EF Core araçları yeterli değildir. Başlangıç projesi olarak kullanmak için oluşturduğunuz kukla proje, araçların .NET Standard sınıf kitaplığını yükleyebileceği somut bir hedef platform sağlar.

### <a name="aspnet-core-environment"></a>ASP.NET Core ortamı

ASP.NET Core projelerine yönelik ortamı belirtmek için, komutları çalıştırmadan önce **ASPNETCORE_ENVIRONMENT** ortam değişkenini ayarlayın.

## <a name="common-options"></a>Ortak seçenekler

|                   | Seçenek                            | Açıklama                                                                                                                                                                                                                                                   |
|:------------------|:----------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   | `--json`                          | JSON çıkışını göster.                                                                                                                                                                                                                                             |
| <nobr>`-c`</nobr> | `--context <DBCONTEXT>`           | Kullanılacak `DbContext` sınıfı. Yalnızca sınıf adı veya ad alanları ile tam nitelikli.  Bu seçenek atlanırsa, EF Core bağlam sınıfını bulur. Birden çok bağlam sınıfı varsa, bu seçenek gereklidir.                                            |
| `-p`              | `--project <PROJECT>`             | Hedef projenin proje klasörünün göreli yolu.  Varsayılan değer geçerli klasördür.                                                                                                                                                              |
| `-s`              | `--startup-project <PROJECT>`     | Başlangıç projesinin proje klasörünün göreli yolu. Varsayılan değer geçerli klasördür.                                                                                                                                                              |
|                   | `--framework <FRAMEWORK>`         | [Hedef çerçeve](/dotnet/standard/frameworks)Için [hedef çerçeve bilinen](/dotnet/standard/frameworks#supported-target-framework-versions) adı.  Proje dosyası birden çok hedef çerçeve belirttiğinde ve bunlardan birini seçmek istediğinizde kullanın. |
|                   | `--configuration <CONFIGURATION>` | Yapı yapılandırması, örneğin: `Debug` veya `Release`.                                                                                                                                                                                                   |
|                   | `--runtime <IDENTIFIER>`          | Paketlerinin geri yükleneceği hedef çalışma zamanının tanımlayıcısı. Çalışma zamanı tanımlayıcıları (RID 'Ler) listesi için bkz. [RID kataloğu](/dotnet/core/rid-catalog).                                                                                                      |
| `-h`              | `--help`                          | Yardım bilgilerini göster.                                                                                                                                                                                                                                        |
| `-v`              | `--verbose`                       | Ayrıntılı çıktıyı göster.                                                                                                                                                                                                                                          |
|                   | `--no-color`                      | Çıktıyı renklendirme.                                                                                                                                                                                                                                        |
|                   | `--prefix-output`                 | Düzeydeki ön ek çıkışı.                                                                                                                                                                                                                                     |

## <a name="dotnet-ef-database-drop"></a>DotNet EF veritabanı bırakma

Veritabanını bırakır.

Seçenekler:

|                   | Seçenek                   | Açıklama                                              |
|:------------------|:-------------------------|:---------------------------------------------------------|
| <nobr>`-f`</nobr> | <nobr>`--force`</nobr>   | Onaylama.                                           |
|                   | <nobr>`--dry-run`</nobr> | Hangi veritabanının bırakılacağını gösterir, ancak onları düşürüyor. |

## <a name="dotnet-ef-database-update"></a>DotNet EF veritabanı güncelleştirmesi

Veritabanını son geçişe veya belirtilen bir geçişe güncelleştirir.

Değişkenlerinden

| Bağımsız Değişken      | Açıklama                                                                                                                                                                                                                                                     |
|:--------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<MIGRATION>` | Hedef geçişi. Geçişler, ada veya KIMLIĞE göre tanımlanabilir. 0 sayısı, *ilk geçişten önceki* ve tüm geçişlerin geri alınmasına neden olan özel bir durumdur. Hiçbir geçiş belirtilmemişse, komut en son geçişe varsayılan olarak ayarlanır. |

Aşağıdaki örnekler veritabanını belirtilen bir geçişe güncelleştirir. İlki geçiş adını, ikincisi ise geçiş KIMLIĞINI kullanır:

```console
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## <a name="dotnet-ef-dbcontext-info"></a>DotNet EF DbContext bilgisi

`DbContext` türü hakkında bilgi alır.

## <a name="dotnet-ef-dbcontext-list"></a>DotNet EF DbContext listesi

Kullanılabilir `DbContext` türlerini listeler.

## <a name="dotnet-ef-dbcontext-scaffold"></a>DotNet EF DbContext iskele

Bir veritabanı için `DbContext` ve varlık türleri için kod üretir. Bu komutun bir varlık türü oluşturması için, veritabanı tablosunun bir birincil anahtarı olmalıdır.

Değişkenlerinden

| Bağımsız Değişken       | Açıklama                                                                                                                                                                                                             |
|:---------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<CONNECTION>` | Veritabanına bağlantı dizesi. ASP.NET Core 2. x projeleri için değer *=\<bağlantı dizesi >* adı olabilir. Bu durumda, ad proje için ayarlanan yapılandırma kaynaklarından gelir. |
| `<PROVIDER>`   | Kullanılacak sağlayıcı. Genellikle bu, NuGet paketinin adıdır, örneğin: `Microsoft.EntityFrameworkCore.SqlServer`.                                                                                           |

Seçenekler:

|                 | Seçenek                                   | Açıklama                                                                                                                                                                    |
|:----------------|:-----------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                     | Modeli yapılandırmak için öznitelikleri kullanın (mümkün olduğunda). Bu seçenek atlanırsa yalnızca Fluent API kullanılır.                                                                |
| `-c`            | `--context <NAME>`                       | Oluşturulacak `DbContext` sınıfın adı.                                                                                                                                 |
|                 | `--context-dir <PATH>`                   | `DbContext` sınıfı dosyasının içine yerleştirilecek dizin. Yollar proje dizinine göredir. Ad alanları, Klasör adlarından türetilir.                                 |
| `-f`            | `--force`                                | Varolan dosyaların üzerine yaz.                                                                                                                                                      |
| `-o`            | `--output-dir <PATH>`                    | İçinde varlık sınıfı dosyalarını yerleştirmek için dizin. Yollar proje dizinine göredir.                                                                                       |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | İçin varlık türleri oluşturulacak tablo şemaları. Birden çok şema belirtmek için `--schema` her biri için yineleyin. Bu seçenek atlanırsa, tüm şemalar dahil edilir.          |
| `-t`            | `--table <TABLE_NAME>`...                | İçin varlık türleri oluşturulacak tablolar. Birden çok tablo belirtmek için her biri için `-t` veya `--table` tekrarlayın. Bu seçenek atlanırsa, tüm tablolar dahil edilir.                |
|                 | `--use-database-names`                   | Tablo ve sütun adlarını tam olarak veritabanında göründükleri gibi kullanın. Bu seçenek atlanırsa, veritabanı adları C# ad stili kurallarıyla daha yakından uyumlu olacak şekilde değiştirilir. |

Aşağıdaki örnek, tüm şemaları ve tabloları uygular ve yeni dosyaları *modeller* klasörüne koyar.

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

Aşağıdaki örnek yalnızca seçili tabloları ve bağlamı belirtilen bir ada sahip ayrı bir klasörde oluşturur:

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models -t Blog -t Post --context-dir Context -c BlogContext
```

## <a name="dotnet-ef-migrations-add"></a>DotNet EF geçişleri ekleme

Yeni bir geçiş ekler.

Değişkenlerinden

| Bağımsız Değişken | Açıklama                |
|:---------|:---------------------------|
| `<NAME>` | Geçişin adı. |

Seçenekler:

|                   | Seçenek                             | Açıklama                                                                                                      |
|:------------------|:-----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>`-o`</nobr> | <nobr>`--output-dir <PATH>`</nobr> | Kullanılacak dizin (ve alt ad alanı). Yollar proje dizinine göredir. Varsayılan olarak "geçişler" olur. |

## <a name="dotnet-ef-migrations-list"></a>DotNet EF geçişleri listesi

Kullanılabilir geçişleri listeler.

## <a name="dotnet-ef-migrations-remove"></a>DotNet EF geçişleri kaldır

Son geçişi kaldırır (geçiş için yapılan kod değişikliklerini geri kaydeder).

Seçenekler:

|                   | Seçenek    | Açıklama                                                                     |
|:------------------|:----------|:--------------------------------------------------------------------------------|
| <nobr>`-f`</nobr> | `--force` | Geçişi geri alma (veritabanına uygulanan değişiklikleri geri alın). |

## <a name="dotnet-ef-migrations-script"></a>DotNet EF geçişleri betiği

Geçişlerden bir SQL betiği oluşturur.

Değişkenlerinden

| Bağımsız Değişken | Açıklama                                                                                                                                                   |
|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<FROM>` | Geçiş başlatılıyor. Geçişler, ada veya KIMLIĞE göre tanımlanabilir. 0 sayısı, *ilk geçişten önceki*anlamına gelen özel bir durumdur. Varsayılan değer 0 ' dır. |
| `<TO>`   | Son geçiş. Son geçişin varsayılan değeri.                                                                                                         |

Seçenekler:

|                   | Seçenek            | Açıklama                                                        |
|:------------------|:------------------|:-------------------------------------------------------------------|
| <nobr>`-o`</nobr> | `--output <FILE>` | Betiğe yazılacak dosya.                                   |
| `-i`              | `--idempotent`    | Herhangi bir geçişte veritabanında kullanılabilecek bir betik oluşturun. |

Aşağıdaki örnek, ınitialcreate geçişi için bir komut dosyası oluşturur:

```console
dotnet ef migrations script 0 InitialCreate
```

Aşağıdaki örnek, ınitialcreate geçişinden sonra tüm geçişler için bir komut dosyası oluşturur.

```console
dotnet ef migrations script 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Ek kaynaklar

* [Geçişler](xref:core/managing-schemas/migrations/index)
* [Tersine mühendislik](xref:core/managing-schemas/scaffolding)
