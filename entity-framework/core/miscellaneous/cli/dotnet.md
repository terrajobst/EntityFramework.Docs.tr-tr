---
title: EF Core araçları başvurusu (.NET CLI) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/20/2018
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: a280aad0344a89c41c30be27a249df3c28c44c70
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447176"
---
# <a name="entity-framework-core-tools-reference---net-cli"></a>Entity Framework Core başvuru - .NET CLI araçları

Entity Framework Core için komut satırı arabirimi (CLI) araçları tasarım zamanı geliştirme görevlerini gerçekleştirin. Örneğin, oluşturdukları [geçişler](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), geçişler uygulamak ve var olan bir veritabanını temel alan bir modeli için kod oluşturur. Platformlar arası bir uzantısı olan komutlardır [dotnet](/dotnet/core/tools) parçası olan komut, [.NET Core SDK'sı](https://www.microsoft.com/net/core). Bu araçlar, .NET Core projeleriyle çalışır.

Öneririz Visual Studio kullanıyorsanız, [Paket Yöneticisi konsolu Araçları](powershell.md) bunun yerine:
* Otomatik olarak seçilen geçerli proje ile çalıştıkları **Paket Yöneticisi Konsolu** gerektirmeden dizinleri el ile değiştirin.
* Bunlar, komut tamamlandıktan sonra bir komut tarafından üretilen dosyalar otomatik olarak açılır.

## <a name="installing-the-tools"></a>Araçları yükleme

Proje türü ve sürümü yükleme yordamını bağlıdır:

* ASP.NET Core sürüm 2.1 ve üzeri
* EF Core 2.x
* EF Core 1.x

### <a name="aspnet-core-21"></a>ASP.NET Core 2.1 +

* Geçerli yükleme [.NET Core SDK'sı](https://www.microsoft.com/net/download/core). SDK, Visual Studio 2017'in en son sürümü olsa bile yüklenmesi gerekir.

  Tüm için ASP.NET Core 2.1 + için gereken budur `Microsoft.EntityFrameworkCore.Design` paket dahil [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).

### <a name="ef-core-2x-not-aspnet-core"></a>EF Core 2.x (ASP.NET Core değil)

`dotnet ef` Komutları, .NET Core SDK'sı dahil edilir, ancak yüklemeniz gereken komutları etkinleştirmek için `Microsoft.EntityFrameworkCore.Design` paket.

* Geçerli yükleme [.NET Core SDK'sı](https://www.microsoft.com/net/download/core). SDK, Visual Studio 2017'in en son sürümü olsa bile yüklenmesi gerekir.

* En son kararlı yükleme `Microsoft.EntityFrameworkCore.Design` paket. 

  ``` Console   
  dotnet add package Microsoft.EntityFrameworkCore.Design   
  ```

### <a name="ef-core-1x"></a>EF Core 1.x

* .NET Core SDK'sı sürüm 2.1.200 yükleyin. Sonraki sürümlerde EF Core 1.0 ve 1.1 için CLI araçları ile uyumlu değildir.

* Değiştirerek kullanın 2.1.200 SDK sürümü için uygulama yapılandırma kendi [global.json](/dotnet/core/tools/global-json) dosya. Normalde bu dosya çözüm dizini (bir proje üzerinde) dahildir. 

* Proje dosyasını düzenleyin ve ekleyin `Microsoft.EntityFrameworkCore.Tools.DotNet` olarak bir `DotNetCliToolReference` öğesi. En son 1.x sürümü belirtin, örneğin: 1.1.6. Bu bölümün sonunda proje dosyası örneğe bakın.

* En son 1.x sürümünü `Microsoft.EntityFrameworkCore.Design` paketini, örneğin:

  ```console
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 1.1.6
  ```

  Eklenen her iki paket başvuruları proje dosyası şuna benzer:

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

  Bir paket başvurusu ile `PrivateAssets="All"` bu projeye başvuran projeler için açık değil. Bu kısıtlama, genellikle yalnızca geliştirme sırasında kullanılan paketler için özellikle yararlıdır.

### <a name="verify-installation"></a>Yüklemesini doğrulama

EF Core CLI araçları düzgün yüklendiğini doğrulamak için aşağıdaki komutları çalıştırın:

  ``` Console
  dotnet restore
  dotnet ef
  ```

Komut çıktısı, kullanılan araçları sürümünü tanımlar:

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

## <a name="using-the-tools"></a>Araçlarını kullanma

Araçları'nı kullanmadan önce bir başlangıç projesi oluşturun veya ortamı gerekebilir.

### <a name="target-project-and-startup-project"></a>Hedef proje ve başlangıç projesi

Komutlar başvurduğu bir *proje* ve *başlangıç projesi*.

* *Proje* olarak da bilinen *hedef proje* olduğundan burada komutlar dosyası ekleyebilir veya kaldırabilirsiniz. Varsayılan olarak, hedef proje geçerli dizinde projedir. Hedef projesi olarak farklı bir proje kullanarak belirtebilirsiniz <nobr> `--project` </nobr> seçeneği.

* *Başlangıç projesi* araçları derlemek ve çalıştırmak sunucudur. Veritabanı bağlantı dizesi ve modelin yapılandırması gibi proje hakkında bilgi almak için tasarım zamanında uygulama kodu yürütmek araçlar vardır. Varsayılan olarak, başlangıç projesi geçerli dizinde projedir. Başlangıç projesi olarak farklı bir proje kullanarak belirtebilirsiniz <nobr> `--startup-project` </nobr> seçeneği.

Hedef proje ve başlangıç projesi çoğunlukla aynı proje olur. Ayrı projeler oldukları tipik bir senaryo olduğunda:

* EF Core bağlamını ve varlık içinde bir .NET Core sınıf kitaplığı sınıflardır.
* Sınıf kitaplığı bir .NET Core konsol uygulaması veya web uygulamasına başvuruyor.

Ayrıca filtrelenebilir [EF Core bağlamdan ayrı bir sınıf kitaplığı'nda geçiş kodu koyun](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Diğer hedef çerçeveleri

CLI araçları, .NET Core projeleri ve .NET Framework projeleri ile çalışır. .NET Core veya .NET Framework projesi bir .NET standart sınıf kitaplığında EF Core modeli olan uygulamalara olmayabilir. Örneğin, bu Xamarin ve evrensel Windows platformu uygulamaları geçerlidir. Böyle durumlarda, araçları için başlangıç projesi olarak davranmak üzere tek amacı olan bir .NET Core konsol uygulaması projesi oluşturabilirsiniz. Proje Gerçek kod olmadan işlevsiz bir proje olabilir &mdash; yalnızca bir hedef için bir araç sağlamak için gereklidir.

Neden gerekli işlevsiz bir proje mi? Daha önce bahsedildiği gibi tasarım zamanında uygulama kodu yürütmek araçlar vardır. Bunu yapmak için .NET Core çalışma zamanı kullanmak gerekir. EF Core model .NET Core veya .NET Framework hedefleyen bir proje içinde olduğunda, EF Core Araçları çalışma zamanı'projesinden ödünç alın. EF Core modeli bir .NET standart sınıf kitaplığında ise bunlar, yapamazsınız. .NET Standard gerçek bir .NET uygulaması değil; .NET uygulamaları desteklemelidir API'leri kümesinin bir özelliğidir. Bu nedenle .NET standart EF Core araçları için uygulama kodu yürütmek yeterli değil. Başlangıç projesi olarak kullanmak için oluşturduğunuz işlevsiz proje içine .NET Standard sınıf kitaplığı araçları yükleyebilir bir somut hedef platformu sağlar. 

### <a name="aspnet-core-environment"></a>ASP.NET Core ortamı

ASP.NET Core projeleri için ortamını belirtmek için ayarlanmış **ASPNETCORE_ENVIRONMENT** komutları çalıştırmadan önce ortam değişkeni.

## <a name="common-options"></a>Sık kullanılan seçenekler

|                   | Seçenek                             | Açıklama                                                                                                                                                                                                                                                   |
|-------------------|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   | `--json`                           | JSON çıktıyı gösterir.                                                                                                                                                                                                                                             |
| <nobr>`-c`</nobr> | `--context <DBCONTEXT>`            | `DbContext` Kullanılacak sınıfı. Yalnızca ya da ad alanları ile tam sınıf adı.  Bu seçeneği atlanırsa, bağlam sınıfını EF Core bulabilirsiniz. Birden çok bağlamı sınıfları varsa, bu seçenek gereklidir.                                            |
| `-p`              | `--project <PROJECT>`              | Hedef proje proje klasörünün göreli yolu.  Varsayılan değer geçerli bir klasördür.                                                                                                                                                              |
| `-s`              | `--startup-project <PROJECT>`      | Başlangıç projesi, proje klasöründen göreli yolu. Varsayılan değer geçerli bir klasördür.                                                                                                                                                              |
|                   | `--framework <FRAMEWORK>`          | [Hedef çerçeve adı](/dotnet/standard/frameworks#supported-target-framework-versions) için [hedef Framework'ü](/dotnet/standard/frameworks).  Birden çok hedef çerçeve proje dosyasını belirtir ve bunlardan birini seçmek istediğinizde bu seçeneği kullanın. |
|                   | `--configuration <CONFIGURATION>`  | Derleme yapılandırmasını, örneğin: `Debug` veya `Release`.                                                                                                                                                                                                   |
|                   | `--runtime <IDENTIFIER>`           | Paketleri geri yüklemek için hedef çalışma zamanı tanımlayıcısı. Çalışma zamanı tanımlayıcılarının (RID'ler) bir listesi için bkz. [RID Kataloğu](/dotnet/core/rid-catalog).                                                                                                      |
| `-h`              | `--help`                           | Yardım bilgilerini gösterir.                                                                                                                                                                                                                                        |
| `-v`              | `--verbose`                        | Ayrıntılı çıktıyı gösterir.                                                                                                                                                                                                                                          |
|                   | `--no-color`                       | Çıkış renklendirmeye yok.                                                                                                                                                                                                                                        |
|                   | `--prefix-output`                  | Düzeyiyle çıkış öneki.                                                                                                                                                                                                                                     |

## <a name="dotnet-ef-database-drop"></a>DotNet ef veritabanını bırak

Veritabanı bırakır.

Seçenekler:

|                   | Seçenek                   | Açıklama                                                |
|-------------------|--------------------------|------------------------------------------------------------|
| <nobr>`-f`</nobr> | <nobr>`--force`</nobr>   | Onay isteme.                                             |
|                   | <nobr>`--dry-run`</nobr> | Hangi veritabanı bırakılan ancak açılan yoksa gösterir.   |

## <a name="dotnet-ef-database-update"></a>DotNet ef veritabanı güncelleştirmesi

Son geçiş veya belirtilen bir geçiş için veritabanını güncelleştirir.

Bağımsız değişkenleri:

| Bağımsız Değişken       | Açıklama                                                                                                                                                                                                                                                     |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<MIGRATION>`  | Hedef geçiş. Geçişler, ada veya kimliğe göre tanımlanan Sayısı 0 anlamına gelen bir özel durumdur *ilk geçişten önce* ve tüm geçişler döndürülmesi neden olur. Geçiş belirtilmişse komutu son geçiş varsayılan olarak kullanır. |

Aşağıdaki örnekler, belirtilen bir geçiş için veritabanı güncelleştirin. İlk geçiş adı ve ikinci geçiş kimliği kullanır:

```console
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## <a name="dotnet-ef-dbcontext-info"></a>DotNet ef dbcontext bilgileri

Bilgi alır bir `DbContext` türü.

## <a name="dotnet-ef-dbcontext-list"></a>DotNet ef dbcontext listesi

Kullanılabilen listelerini `DbContext` türleri.

## <a name="dotnet-ef-dbcontext-scaffold"></a>DotNet ef dbcontext iskele

İçin kod oluşturur bir `DbContext` ve varlık türleri için bir veritabanı.

Bağımsız değişkenleri:

| Bağımsız Değişken        | Açıklama                                                                                                                                                                                                             |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<CONNECTION>`  | Veritabanı bağlantı dizesi. ASP.NET Core 2.x projeleri için bir değer olabilir *adı =\<bağlantı dizesi adı >*. Bu durumda proje için ayarladığınız yapılandırma kaynaklarını adı gelir. |
| `<PROVIDER>`    | Kullanılacak sağlayıcı. Genellikle bu NuGet paketi, örneğin adıdır: `Microsoft.EntityFrameworkCore.SqlServer`.                                                                                           |

Seçenekler:

|                   | Seçenek                                    | Açıklama                                                                                                                                                                    |
|-------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr>   | `--data-annotations`                      | Öznitelikler, model (mümkünse) yapılandırmak için kullanın. Bu seçenek belirtilmezse, yalnızca fluent API'si kullanılır.                                                                |
| `-c`              | `--context <NAME>`                        | Adını `DbContext` oluşturmak için sınıf.                                                                                                                                 |
|                   | `--context-dir <PATH>`                    | Yerleştirileceği dizin `DbContext` sınıf dosyasında. Proje dizinine göreli yollardır. Ad alanları klasörü adlarından türetilir.                                 |
| `-f`              | `--force`                                 | Varolan dosyaların üzerine yaz.                                                                                                                                                      |
| `-o`              | `--output-dir <PATH>`                     | Varlık sınıf dosyaları yerleştirmek için dizin. Proje dizinine göreli yollardır.                                                                                       |
|                   | <nobr>`--schema <SCHEMA_NAME>...`</nobr>  | Şemaları için varlık türleri oluşturmak için tablo. Birden çok şema belirtmek için yineleyin `--schema` her biri için. Bu seçenek belirtilmezse, tüm şemalar dahil edilir.          |
| `-t`              | `--table <TABLE_NAME>`...                 | Varlık türleri için oluşturmak üzere tablolara. Birden çok tablo belirtmek için yineleyin `-t` veya `--table` her biri için. Bu seçenek belirtilmezse, tüm tabloları dahil edilir.                |
|                   | `--use-database-names`                    | Veritabanında tam olarak göründükleri gibi tablo ve sütun adları kullanın. Bu seçenek belirtilmezse, daha yakından C# ad stil kurallarına uymak için veritabanı adları değiştirildi. |

Aşağıdaki örnek, tüm şemaları ve tabloları iskele oluşturulduğunu ve yeni dosyaları yerleştirir *modelleri* klasör.

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

Aşağıdaki örnek, yalnızca seçilen tabloları iskele oluşturulduğunu ve ayrı bir klasörde belirtilen ada sahip bir bağlam oluşturur:

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models -t Blog -t Post --context-dir Context -c BlogContext
```

## <a name="dotnet-ef-migrations-add"></a>DotNet ef geçişleri Ekle

Yeni bir geçiş ekler.

Bağımsız değişkenleri:

| Bağımsız Değişken  | Açıklama                  |
|-----------|------------------------------|
| `<NAME>`  | Geçiş adı.   |

Seçenekler:

|                   | Seçenek                              | Açıklama                                                                                                        |
|-------------------|-------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| <nobr>`-o`</nobr> | <nobr>`--output-dir <PATH>`</nobr>  | Kullanılacak dizin (ve alt ad alanı). Proje dizinine göreli yollardır. Varsayılan olarak "Geçişler".   |

## <a name="dotnet-ef-migrations-list"></a>DotNet ef geçişleri listesi

Kullanılabilir geçişleri listeler.

## <a name="dotnet-ef-migrations-remove"></a>DotNet ef geçişleri Kaldır

(Geçiş için yapıldığını kod değişiklikleri geri alır) son geçiş kaldırır. 

Seçenekler:

|                   | Seçenek    | Açıklama                                                                        |
|-------------------|-----------|------------------------------------------------------------------------------------|
| <nobr>`-f`</nobr> | `--force` | Geçişi geri döndürme (veritabanına uygulanan değişiklikleri geri alma).    |

## <a name="dotnet-ef-migrations-script"></a>DotNet ef geçişleri betiği

Bir SQL betiği geçişleri oluşturur.

Bağımsız değişkenleri:

| Bağımsız Değişken  | Açıklama                                                                                                                                                   |
|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<FROM>`  | Başlangıç geçiş. Geçişler, ada veya kimliğe göre tanımlanan Sayısı 0 anlamına gelen bir özel durumdur *ilk geçişten önce*. Varsayılan olarak 0. |
| `<TO>`    | Bitiş geçiş. Son varsayılan olarak geçiş.                                                                                                         |

Seçenekler:

|                   | Seçenek             | Açıklama                                                          |
|-------------------|--------------------|----------------------------------------------------------------------|
| <nobr>`-o`</nobr> | `--output <FILE>`  | Komut dosyası yazmak için dosya.                                     |
| `-i`              | `--idempotent`     | Bir veritabanı herhangi bir geçiş sırasında kullanılan bir komut dosyası oluşturur.   |

Aşağıdaki örnek, InitialCreate geçiş için bir komut dosyası oluşturur:

```console
dotnet ef migrations script 0 InitialCreate
```

Aşağıdaki örnek, InitialCreate geçişten sonra tüm geçişler için bir komut dosyası oluşturur.

```console
dotnet ef migrations script 20180904195021_InitialCreate
```