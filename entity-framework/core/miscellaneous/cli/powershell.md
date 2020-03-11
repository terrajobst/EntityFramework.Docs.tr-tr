---
title: EF Core araçları başvurusu (Paket Yöneticisi konsolu)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: a9ce6d5b5f36a72e3715a9de787f1f00e989a58c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416720"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a>Visual Studio 'da Entity Framework Core araçları başvurusu-Paket Yöneticisi konsolu

Entity Framework Core tasarım zamanı geliştirme görevlerini gerçekleştirmek için Paket Yöneticisi Konsolu (PMC) araçları. Örneğin, [geçişler](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0)oluşturur, geçişleri uygular ve var olan bir veritabanını temel alan bir model için kod oluşturur. Komutlar, Visual Studio içinde [Paket Yöneticisi konsolu](/nuget/tools/package-manager-console)kullanılarak çalıştırılır. Bu araçlar hem .NET Framework hem de .NET Core projeleriyle çalışır.

Visual Studio kullanmıyorsanız bunun yerine [EF Core komut satırı araçlarının](dotnet.md) kullanılması önerilir. CLı araçları platformlar arası ve komut istemi içinde çalıştırılır.

## <a name="installing-the-tools"></a>Araçları yükleme

Araçları yükleme ve güncelleştirme yordamları ASP.NET Core 2.1 + ile önceki sürümler veya diğer proje türleri arasında farklılık gösterir.

### <a name="aspnet-core-version-21-and-later"></a>ASP.NET Core sürüm 2,1 ve üzeri

`Microsoft.EntityFrameworkCore.Tools` paketi [Microsoft. AspNetCore. app metapackage](/aspnet/core/fundamentals/metapackage-app)içinde yer aldığı için Araçlar ASP.NET Core 2.1 + projesine otomatik olarak eklenir.

Bu nedenle, araçları yüklemek için herhangi bir şey yapmanız gerekmez, ancak şunları yapmanız gerekir:

* Yeni bir projedeki araçları kullanmadan önce paketleri geri yükleyin.
* Araçları daha yeni bir sürüme güncelleştirmek için bir paket yükler.

Araçların en son sürümünü aldığınızdan emin olmak için aşağıdaki adımı da yapmanızı öneririz:

* *. Csproj* dosyanızı düzenleyin ve [Microsoft. entityframeworkcore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) paketinin en son sürümünü belirten bir satır ekleyin. Örneğin, *. csproj* dosyası şuna benzer bir `ItemGroup` içerebilir:

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

Aşağıdaki örnekte olduğu gibi bir ileti aldığınızda araçları güncelleştirin:

> EF Core araçları sürümü ' 2.1.1-RTM-30846 ', ' 2.1.3-RTM-32065 ' çalışma zamanından daha eski. En son özellikler ve hata düzeltmeleri için araçları güncelleştirin.

Araçları güncelleştirmek için:

* En son .NET Core SDK yükler.
* Visual Studio 'Yu en son sürüme güncelleştirin.
* *. Csproj* dosyasını, daha önce gösterildiği gibi en son araçlar paketine bir paket başvurusu içerecek şekilde düzenleyin.

### <a name="other-versions-and-project-types"></a>Diğer sürümler ve proje türleri

Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırarak Paket Yöneticisi konsol araçlarını **yükler:**

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

**Paket Yöneticisi konsolunda**aşağıdaki komutu çalıştırarak araçları güncelleştirin.

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a>Yüklemeyi doğrulama

Şu komutu çalıştırarak araçların yüklendiğini doğrulayın:

``` powershell
Get-Help about_EntityFrameworkCore
```

Çıktı şuna benzer (hangi araçların hangi sürümünü kullandığınızı söylemez):

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

## <a name="using-the-tools"></a>Araçları kullanma

Araçları kullanmadan önce:

* Hedef ve başlangıç projesi arasındaki farkı anlayın.
* .NET Standard sınıf kitaplıklarıyla araçları nasıl kullanacağınızı öğrenin.
* ASP.NET Core projeler için ortamı ayarlayın.

### <a name="target-and-startup-project"></a>Hedef ve başlangıç projesi

Komutlar bir *projeye* ve bir *başlangıç projesine*başvurur.

* Ayrıca, komutların dosya eklemesi veya kaldırması nedeniyle *Proje* *hedef proje* olarak da bilinir. Varsayılan olarak, **Paket Yöneticisi konsolunda** seçilen **varsayılan proje** hedef projem tir. <nobr>`--project`</nobr> seçeneğini kullanarak, hedef proje olarak farklı bir proje belirtebilirsiniz.

* *Başlangıç projesi* , araçların oluşturup çalıştırdığı bir. Araçlar, veritabanı bağlantı dizesi ve modelin yapılandırması gibi proje hakkında bilgi almak için tasarım zamanında uygulama kodu yürütmeniz gerekir. Varsayılan olarak, Çözüm Gezgini **Başlangıç projesi** başlangıç projem ' dir. <nobr>`--startup-project`</nobr> seçeneğini kullanarak, başlangıç projesi olarak farklı bir proje belirtebilirsiniz.

Başlangıç projesi ve hedef proje genellikle aynı projem. Farklı projeler oldukları tipik bir senaryo şunlardır:

* EF Core bağlamı ve varlık sınıfları bir .NET Core sınıf kitaplığında bulunur.
* .NET Core konsol uygulaması veya Web uygulaması, sınıf kitaplığına başvurur.

[Geçiş kodunu, EF Core bağlamından ayrı bir sınıf kitaplığına yerleştirmek](xref:core/managing-schemas/migrations/projects)de mümkündür.

### <a name="other-target-frameworks"></a>Diğer hedef çerçeveler

Paket Yöneticisi konsol araçları, .NET Core veya .NET Framework projeleriyle çalışır. .NET Standard Sınıf kitaplığındaki EF Core modeli olan uygulamalarda .NET Core veya .NET Framework projesi bulunmayabilir. Örneğin, bu, Xamarin ve Evrensel Windows Platformu uygulamaları için geçerlidir. Bu gibi durumlarda, yalnızca amacı araçlar için başlangıç projesi olarak davranacak olan bir .NET Core veya .NET Framework konsol uygulaması projesi oluşturabilirsiniz. Proje, gerçek kod içermeyen bir kukla proje olabilir &mdash; yalnızca araç için bir hedef sağlamanız gerekir.

İşlevsiz bir proje neden gereklidir? Daha önce belirtildiği gibi, araçların uygulama kodunu tasarım zamanında yürütmesi gerekir. Bunu yapmak için, .NET Core veya .NET Framework çalışma zamanını kullanmaları gerekir. EF Core modeli .NET Core veya .NET Framework hedefleyen bir projede olduğunda, EF Core araçları projeden çalışma zamanını ödünç. EF Core modeli .NET Standard bir sınıf kitaplığınlarsa bunu yapamazlar. .NET Standard gerçek bir .NET uygulamasını değil; .NET uygulamalarının desteklemesi gereken bir API kümesine yönelik bir belirtimdir. Bu nedenle .NET Standard uygulama kodunu yürütmek için EF Core araçları yeterli değildir. Başlangıç projesi olarak kullanmak için oluşturduğunuz kukla proje, araçların .NET Standard sınıf kitaplığını yükleyebileceği somut bir hedef platform sağlar.

### <a name="aspnet-core-environment"></a>ASP.NET Core ortamı

ASP.NET Core projelerine yönelik ortamı belirtmek için, komutları çalıştırmadan önce **env: ASPNETCORE_ENVIRONMENT** ayarlayın.

## <a name="common-parameters"></a>Ortak Parametreler

Aşağıdaki tabloda tüm EF Core komutlarında ortak olan parametreler gösterilmektedir:

| Parametre                 | Açıklama                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Context \<dize >        | Kullanılacak `DbContext` sınıfı. Yalnızca sınıf adı veya ad alanları ile tam nitelikli.  Bu parametre atlanırsa, EF Core bağlam sınıfını bulur. Birden çok bağlam sınıfı varsa, bu parametre gereklidir. |
| -Proje \<dize >        | Hedef proje. Bu parametre atlanırsa, hedef proje olarak **Package Manager konsolunun** **varsayılan projesi** kullanılır.                                                                             |
| -StartupProject \<dize > | Başlangıç projesi. Bu parametre atlanırsa, **çözüm özelliklerindeki** **Başlangıç projesi** hedef proje olarak kullanılır.                                                                                 |
| -Ayrıntılı                  | Ayrıntılı çıktıyı göster.                                                                                                                                                                                                 |

Bir komutla ilgili yardım bilgilerini göstermek için PowerShell 'in `Get-Help` komutunu kullanın.

> [!TIP]
> Bağlam, proje ve StartupProject parametreleri sekme genişletmeyi destekler.

## <a name="add-migration"></a>Geçiş Ekle

Yeni bir geçiş ekler.

Parametreler:

| Parametre                         | Açıklama                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <nobr>ad \<dize ><nobr>       | Geçişin adı. Bu bir Konumsal parametredir ve gereklidir.                                              |
| <nobr>-OutputDir \<dize ></nobr> | Kullanılacak dizin (ve alt ad alanı). Yollar, hedef proje dizini ile ilişkilidir. Varsayılan olarak "geçişler" olur. |

## <a name="drop-database"></a>Veritabanını bırak

Veritabanını bırakır.

Parametreler:

| Parametre | Açıklama                                              |
|:----------|:---------------------------------------------------------|
| -WhatIf   | Hangi veritabanının bırakılacağını gösterir, ancak onları düşürüyor. |

## <a name="get-dbcontext"></a>Get-DbContext

`DbContext` türü hakkında bilgi alır.

## <a name="remove-migration"></a>Geçişi Kaldır

Son geçişi kaldırır (geçiş için yapılan kod değişikliklerini geri kaydeder).

Parametreler:

| Parametre | Açıklama                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| -Force    | Geçişi geri alma (veritabanına uygulanan değişiklikleri geri alın). |

## <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Bir veritabanı için `DbContext` ve varlık türleri için kod üretir. `Scaffold-DbContext` bir varlık türü oluşturmak için veritabanı tablosunun birincil anahtarı olmalıdır.

Parametreler:

| Parametre                          | Açıklama                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Bağlantı \<dize ></nobr> | Veritabanına bağlantı dizesi. ASP.NET Core 2. x projeleri için değer *=\<bağlantı dizesi >* adı olabilir. Bu durumda, ad proje için ayarlanan yapılandırma kaynaklarından gelir. Bu bir Konumsal parametredir ve gereklidir. |
| <nobr>-Sağlayıcı \<dize ></nobr>   | Kullanılacak sağlayıcı. Genellikle bu, NuGet paketinin adıdır, örneğin: `Microsoft.EntityFrameworkCore.SqlServer`. Bu bir Konumsal parametredir ve gereklidir.                                                                                           |
| -OutputDir \<dize >               | Dosyaları içine koyabileceğiniz dizin. Yollar proje dizinine göredir.                                                                                                                                                                                             |
| -ContextDir \<dize >              | `DbContext` dosyasının içine yerleştirilecek dizin. Yollar proje dizinine göredir.                                                                                                                                                                              |
| -Context \<dize >                 | Oluşturulacak `DbContext` sınıfın adı.                                                                                                                                                                                                                          |
| -Şema \<dize [] >               | İçin varlık türleri oluşturulacak tablo şemaları. Bu parametre atlanırsa, tüm şemalar dahil edilir.                                                                                                                                                             |
| -Tables \<dize [] >                | İçin varlık türleri oluşturulacak tablolar. Bu parametre atlanırsa, tüm tablolar dahil edilir.                                                                                                                                                                         |
| -Datanot açıklamaları                   | Modeli yapılandırmak için öznitelikleri kullanın (mümkün olduğunda). Bu parametre atlanırsa yalnızca Fluent API kullanılır.                                                                                                                                                      |
| -UseDatabaseNames                  | Tablo ve sütun adlarını tam olarak veritabanında göründükleri gibi kullanın. Bu parametre atlanırsa, veritabanı adları C# ad stili kurallarıyla daha yakından uyumlu olacak şekilde değiştirilir.                                                                                       |
| -Force                             | Varolan dosyaların üzerine yaz.                                                                                                                                                                                                                                               |

Örnek:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Yalnızca seçili tabloları oluşturan ve bağlamı belirtilen bir adla ayrı bir klasörde oluşturan örnek:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a>Betik geçişi

Seçilen bir geçişin tüm değişikliklerini seçili başka bir geçişe uygulayan bir SQL betiği oluşturur.

Parametreler:

| Parametre                | Açıklama                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *-* \<dizeden >        | Geçiş başlatılıyor. Geçişler, ada veya KIMLIĞE göre tanımlanabilir. 0 sayısı, *ilk geçişten önceki*anlamına gelen özel bir durumdur. Varsayılan ayar: 0.                                                              |
| *-* \<dize >          | Son geçiş. Son geçişin varsayılan değeri.                                                                                                                                                                      |
| <nobr>-Idempotent</nobr> | Herhangi bir geçişte veritabanında kullanılabilecek bir betik oluşturun.                                                                                                                                                         |
| -Output \<dize >        | Sonucun yazılacağı dosya. Bu parametre atlanırsa dosya, uygulamanın çalışma zamanı dosyaları oluşturulduğu klasörde oluşturulmuş bir adla oluşturulur, örneğin: */obj/Debug/netcoreapp2,/ghbkztfz.exe*. |

> [!TIP]
> To, from ve OUTPUT parametreleri sekme genişletmeyi destekler.

Aşağıdaki örnek, geçiş adı kullanılarak, ınitialcreate geçişi için bir komut dosyası oluşturur.

```powershell
Script-Migration -To InitialCreate
```

Aşağıdaki örnek, geçiş KIMLIĞI kullanılarak, ınitialcreate geçişinden sonra tüm geçişler için bir komut dosyası oluşturur.

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a>Güncelleştirme-veritabanı

Veritabanını son geçişe veya belirtilen bir geçişe güncelleştirir.

| Parametre                           | Açıklama                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr> *-Geçiş* \<dize ></nobr> | Hedef geçişi. Geçişler, ada veya KIMLIĞE göre tanımlanabilir. 0 sayısı, *ilk geçişten önceki* ve tüm geçişlerin geri alınmasına neden olan özel bir durumdur. Hiçbir geçiş belirtilmemişse, komut en son geçişe varsayılan olarak ayarlanır. |

> [!TIP]
> Geçiş parametresi sekme genişletmeyi destekler.

Aşağıdaki örnek tüm geçişleri geri döndürür.

```powershell
Update-Database -Migration 0
```

Aşağıdaki örnekler veritabanını belirtilen bir geçişe güncelleştirir. İlki geçiş adını, ikincisi ise geçiş KIMLIĞINI kullanır:

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Ek kaynaklar

* [Geçişler](xref:core/managing-schemas/migrations/index)
* [Tersine mühendislik](xref:core/managing-schemas/scaffolding)
