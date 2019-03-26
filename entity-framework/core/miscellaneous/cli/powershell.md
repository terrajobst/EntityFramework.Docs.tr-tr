---
title: EF Core araçları başvurusu (Paket Yöneticisi Konsolu) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: cb05e3fb66adf96f8a6778711a76520d0be24c71
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419776"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a>Entity Framework Core başvuru - Visual Studio'da Paket Yöneticisi konsolu araçları

Entity Framework Core için Paket Yöneticisi Konsolu (PMC) araçları tasarım zamanı geliştirme görevlerini gerçekleştirin. Örneğin, oluşturdukları [geçişler](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), geçişler uygulamak ve var olan bir veritabanını temel alan bir modeli için kod oluşturur. Visual Studio kullanarak içindeki komutları çalıştırmanız [Paket Yöneticisi Konsolu](/nuget/tools/package-manager-console). Bu araçlar, .NET Framework hem de .NET Core projeleriyle çalışır.

Öneririz Visual Studio, kullanmadığınız, [EF Core komut satırı araçları](dotnet.md) yerine. CLI araçları, platformlar arası ve bir komut istemi içinde çalıştırın.

## <a name="installing-the-tools"></a>Araçları yükleme

Yükleme ve güncelleştirme araçları yordamları, ASP.NET Core 2.1 + ve önceki sürümleri veya diğer proje türleri arasında farklılık gösterir.

### <a name="aspnet-core-version-21-and-later"></a>ASP.NET Core sürüm 2.1 ve üzeri

Çünkü araçları otomatik olarak bir ASP.NET Core 2.1 + projeye eklenir `Microsoft.EntityFrameworkCore.Tools` paket dahil [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).

Bu nedenle, araçları yüklemek için herhangi bir şey yapmanız gerekmez, ancak göstermesi gerekmez:
* Yeni bir projede Araçları'nı kullanmadan önce paketleri geri yükle.
* Araçlar, yeni bir sürüme güncelleştirme paketini yükleyin.

Araçların en son sürümü aldığınızdan emin olmak için aşağıdaki adımı da yapmanız önerilir:

* Düzenleme, *.csproj* dosya ve en son sürümünü belirten bir satır ekleyin [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) paket. Örneğin, *.csproj* dosya içerebilir bir `ItemGroup` aşağıdakine benzer:

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

Aşağıdaki örnekte olduğu gibi bir ileti aldığınızda Araçları'nı güncelleştirin:

> EF Core Araçları '2.1.1-rtm-30846' çalışma zamanı '2.1.3-rtm-32065' eski sürümüdür. En son özellikler ve hata düzeltmeleri için araçları güncelleştirin.

Araçları güncelleştirmek için:
* En son .NET Core SDK'sını yükleyin.
* Visual Studio, en son sürüme güncelleştirin.
* Düzen *.csproj* daha önce gösterildiği gibi bir paket başvurusu için en son araçları paketini içeren dosya.

### <a name="other-versions-and-project-types"></a>Diğer sürümler ve proje türleri

Aşağıdaki komutu çalıştırarak Paket Yöneticisi konsolu Araçları'nı yükleme **Paket Yöneticisi Konsolu**:

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Aşağıdaki komutu çalıştırarak araçları güncelleştirme **Paket Yöneticisi Konsolu**.

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a>Yüklemeyi doğrulama

Bu komutu çalıştırarak araçların yüklendiğini doğrulayın:

``` powershell
Get-Help about_EntityFrameworkCore
```

(Bu, kullanmakta olduğunuz Araçlar'ın hangi sürümünün sunmayacaktır) çıktı şuna benzer:

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a>Araçlarını kullanma

Araçları'nı kullanmadan önce:
* Hedef ve başlangıç projesi arasındaki farkı anlama.
* Araçları ile .NET standart sınıf kitaplıkları kullanmayı öğrenin.
* ASP.NET Core projeleri için ortam ayarlayın.

### <a name="target-and-startup-project"></a>Hedef ve başlangıç projesi

Komutlar başvurduğu bir *proje* ve *başlangıç projesi*.

* *Proje* olarak da bilinen *hedef proje* olduğundan burada komutlar dosyası ekleyebilir veya kaldırabilirsiniz. Varsayılan olarak, **varsayılan proje** seçili **Paket Yöneticisi Konsolu** hedef projedir. Hedef projesi olarak farklı bir proje kullanarak belirtebilirsiniz <nobr> `--project` </nobr> seçeneği.

* *Başlangıç projesi* araçları derlemek ve çalıştırmak sunucudur. Veritabanı bağlantı dizesi ve modelin yapılandırması gibi proje hakkında bilgi almak için tasarım zamanında uygulama kodu yürütmek araçlar vardır. Varsayılan olarak, **başlangıç projesi** içinde **Çözüm Gezgini** başlangıç projedir. Başlangıç projesi olarak farklı bir proje kullanarak belirtebilirsiniz <nobr> `--startup-project` </nobr> seçeneği.

Hedef proje ve başlangıç projesi çoğunlukla aynı proje olur. Ayrı projeler oldukları tipik bir senaryo olduğunda:

* EF Core bağlamını ve varlık içinde bir .NET Core sınıf kitaplığı sınıflardır.
* Sınıf kitaplığı bir .NET Core konsol uygulaması veya web uygulamasına başvuruyor.

Ayrıca filtrelenebilir [EF Core bağlamdan ayrı bir sınıf kitaplığı'nda geçiş kodu koyun](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Diğer hedef çerçeveleri

Paket Yöneticisi konsolu Araçlar, .NET Core veya .NET Framework projeleriyle çalışır. .NET Core veya .NET Framework projesi bir .NET standart sınıf kitaplığında EF Core modeli olan uygulamalara olmayabilir. Örneğin, bu Xamarin ve evrensel Windows platformu uygulamaları geçerlidir. Böyle durumlarda, araçları için başlangıç projesi olarak davranmak üzere tek amacı olan bir .NET Core veya .NET Framework konsol uygulama projesi oluşturabilirsiniz. Proje Gerçek kod olmadan işlevsiz bir proje olabilir &mdash; yalnızca bir hedef için bir araç sağlamak için gereklidir.

Neden gerekli işlevsiz bir proje mi? Daha önce bahsedildiği gibi tasarım zamanında uygulama kodu yürütmek araçlar vardır. Bunu yapmak için .NET Core veya .NET Framework çalışma zamanı kullanmak gerekir. EF Core model .NET Core veya .NET Framework hedefleyen bir proje içinde olduğunda, EF Core Araçları çalışma zamanı'projesinden ödünç alın. EF Core modeli bir .NET standart sınıf kitaplığında ise bunlar, yapamazsınız. .NET Standard gerçek bir .NET uygulaması değil; .NET uygulamaları desteklemelidir API'leri kümesinin bir özelliğidir. Bu nedenle .NET standart EF Core araçları için uygulama kodu yürütmek yeterli değil. Başlangıç projesi olarak kullanmak için oluşturduğunuz işlevsiz proje içine .NET Standard sınıf kitaplığı araçları yükleyebilir bir somut hedef platformu sağlar.

### <a name="aspnet-core-environment"></a>ASP.NET Core ortamı

ASP.NET Core projeleri için ortamını belirtmek için ayarlanmış **env:ASPNETCORE_ENVIRONMENT** komutları çalıştırmadan önce.

## <a name="common-parameters"></a>Ortak parametreleri

Aşağıdaki tabloda, EF Core komutların tümü için ortak olan parametreler gösterilmektedir:

| Parametre                 | Açıklama                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Bağlam \<dizesi >        | `DbContext` Kullanılacak sınıfı. Yalnızca ya da ad alanları ile tam sınıf adı.  Bu parametre atlanırsa, bağlam sınıfını EF Core bulur. Birden çok bağlamı sınıfları varsa, bu parametre gereklidir. |
| -Project \<dizesi >        | Hedef proje. Bu parametre atlanırsa, **varsayılan proje** için **Paket Yöneticisi Konsolu** hedef projesi olarak kullanılır.                                                                             |
| -StartupProject \<dizesi > | Başlangıç projesi. Bu parametre atlanırsa, **başlangıç projesi** içinde **çözüm özellikleri** hedef projesi olarak kullanılır.                                                                                 |
| -Verbose                  | Ayrıntılı çıktıyı gösterir.                                                                                                                                                                                                 |

Bir komut hakkında Yardım bilgilerini görüntülemek için PowerShell'in kullanın `Get-Help` komutu.

> [!TIP]
> Bağlam, proje ve StartupProject parametreleri sekme genişletmeyi destekler.

## <a name="add-migration"></a>Geçiş Ekle

Yeni bir geçiş ekler.

Parametreler:

| Parametre                         | Açıklama                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Ad \<dizesi ><nobr>       | Geçiş adı. Bu, konumsal bir parametredir ve gereklidir.                                              |
| <nobr>-OutputDir \<dizesi ></nobr> | Kullanılacak dizin (ve alt ad alanı). Hedef proje dizinine göreli yollardır. Varsayılan olarak "Geçişler". |

## <a name="drop-database"></a>Veritabanını silme

Veritabanı bırakır.

Parametreler:

| Parametre | Açıklama                                              |
|:----------|:---------------------------------------------------------|
| -WhatIf   | Hangi veritabanı bırakılan ancak açılan yoksa gösterir. |

## <a name="get-dbcontext"></a>Get-DbContext

Bilgi alır bir `DbContext` türü.

## <a name="remove-migration"></a>Remove-geçiş

(Geçiş için yapıldığını kod değişiklikleri geri alır) son geçiş kaldırır.

Parametreler:

| Parametre | Açıklama                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| -Force    | Geçişi geri döndürme (veritabanına uygulanan değişiklikleri geri alma). |

## <a name="scaffold-dbcontext"></a>Scaffold-DbContext

İçin kod oluşturur bir `DbContext` ve varlık türleri için bir veritabanı. Sırayla `Scaffold-DbContext` bir varlık türü oluşturmak için veritabanı tablosunun birincil anahtarı olmalıdır.

Parametreler:

| Parametre                          | Açıklama                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Bağlantı \<dizesi ></nobr> | Veritabanı bağlantı dizesi. ASP.NET Core 2.x projeleri için bir değer olabilir *adı =\<bağlantı dizesi adı >*. Bu durumda proje için ayarladığınız yapılandırma kaynaklarını adı gelir. Bu, konumsal bir parametredir ve gereklidir. |
| <nobr>-Provider \<dizesi ></nobr>   | Kullanılacak sağlayıcı. Genellikle bu NuGet paketi, örneğin adıdır: `Microsoft.EntityFrameworkCore.SqlServer`. Bu, konumsal bir parametredir ve gereklidir.                                                                                           |
| -OutputDir \<dizesi >               | Dosyaları yerleştirmek için dizin. Proje dizinine göreli yollardır.                                                                                                                                                                                             |
| -ContextDir \<dizesi >              | Yerleştirileceği dizin `DbContext` dosyası. Proje dizinine göreli yollardır.                                                                                                                                                                              |
| -Bağlam \<dizesi >                 | Adını `DbContext` oluşturmak için sınıf.                                                                                                                                                                                                                          |
| -Schemas \<String[]>               | Şemaları için varlık türleri oluşturmak için tablo. Bu parametre atlanırsa, tüm şemalar dahil edilir.                                                                                                                                                             |
| -Tablolar \<String [] >                | Varlık türleri için oluşturmak üzere tablolara. Bu parametre atlanırsa, tüm tabloları dahil edilir.                                                                                                                                                                         |
| -DataAnnotations                   | Öznitelikler, model (mümkünse) yapılandırmak için kullanın. Bu parametre atlanırsa, yalnızca fluent API'si kullanılır.                                                                                                                                                      |
| -UseDatabaseNames                  | Veritabanında tam olarak göründükleri gibi tablo ve sütun adları kullanın. Bu parametre atlanırsa, veritabanı adları daha yakından C# ad stil kurallarına uygun şekilde değiştirilir.                                                                                       |
| -Force                             | Varolan dosyaların üzerine yaz.                                                                                                                                                                                                                                               |

Örnek:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Yalnızca seçilen tabloları iskele oluşturulduğunu ve ayrı bir klasörde belirtilen ada sahip bir bağlam oluşturur. örnek:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a>Geçiş betiği

Tüm değişiklikleri başka bir seçili geçiş için seçilen bir geçiş geçerli bir SQL betiği oluşturur.

Parametreler:

| Parametre                | Açıklama                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *-From* \<dizesi >        | Başlangıç geçiş. Geçişler, ada veya kimliğe göre tanımlanan Sayısı 0 anlamına gelen bir özel durumdur *ilk geçişten önce*. Varsayılan olarak 0.                                                              |
| *-Çok* \<dizesi >          | Bitiş geçiş. Son varsayılan olarak geçiş.                                                                                                                                                                      |
| <nobr>-Bir kez etkili</nobr> | Bir veritabanı herhangi bir geçiş sırasında kullanılan bir komut dosyası oluşturur.                                                                                                                                                         |
| -Çıkış \<dizesi >        | Yazma sonucu dosyası. Bu parametre ATLANIRSA, uygulamanın çalışma zamanı dosyalarını, örneğin oluşturuldukça dosyası ile aynı klasörde oluşturulan bir ad oluşturulur: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*. |

> [!TIP]
> Kime, ve çıkış parametreleri sekme genişletmeyi destekler.

Aşağıdaki örnek, taşıma adını kullanarak InitialCreate geçiş için bir komut dosyası oluşturur.

```powershell
Script-Migration -To InitialCreate
```

Aşağıdaki örnek, geçiş kimliğini kullanarak InitialCreate geçişten sonra tüm geçişler için bir komut dosyası oluşturur.

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a>Veritabanını Güncelleştir

Son geçiş veya belirtilen bir geçiş için veritabanını güncelleştirir.

| Parametre                           | Açıklama                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>*-Geçiş* \<dizesi ></nobr> | Hedef geçiş. Geçişler, ada veya kimliğe göre tanımlanan Sayısı 0 anlamına gelen bir özel durumdur *ilk geçişten önce* ve tüm geçişler döndürülmesi neden olur. Geçiş belirtilmişse komutu son geçiş varsayılan olarak kullanır. |

> [!TIP]
> Geçiş parametresi sekme genişletmeyi destekler.

Aşağıdaki örnek, tüm geçişler döner.

```powershell
Update-Database -Migration 0
```

Aşağıdaki örnekler, belirtilen bir geçiş için veritabanı güncelleştirin. İlk geçiş adı ve ikinci geçiş kimliği kullanır:

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Ek kaynaklar

* [Geçişler](xref:core/managing-schemas/migrations/index)
* [Tersine mühendislik](xref:core/managing-schemas/scaffolding)
