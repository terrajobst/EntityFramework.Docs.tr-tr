---
title: Entity Framework Core yükleniyor
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: db1b83a9222e00a5e226a134085b18247b31f29a
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306479"
---
# <a name="installing-entity-framework-core"></a>Entity Framework Core yükleniyor

## <a name="prerequisites"></a>Önkoşullar

* EF Core bir [.NET Standard 2,0](/dotnet/standard/net-standard) kitaplığı. EF Core .NET Standard 2,0 ' i destekleyen bir .NET uygulamasının çalışmasını gerektirir. EF Core diğer .NET Standard 2,0 kitaplıkları tarafından da başvurulabilirler. 

* Örneğin, .NET Core 'u hedefleyen uygulamalar geliştirmek için EF Core kullanabilirsiniz. .NET Core uygulamaları oluşturmak için [.NET Core SDK](https://dotnet.microsoft.com/download)gerekir. İsteğe bağlı olarak, Visual Studio, Mac için Visual Studio veya Visual Studio Code gibi bir geliştirme ortamı da kullanabilirsiniz. Daha fazla bilgi için [.NET Core Ile çalışmaya](/dotnet/core/get-started)başlama konusuna bakın.

* Visual Studio 'Yu kullanarak Windows 'da .NET Framework 4.6.1 veya üstünü hedefleyen uygulamalar geliştirmek için EF Core kullanabilirsiniz. Visual Studio 'nun en son sürümü önerilir. Visual Studio 2015 gibi eski bir sürümü kullanmak istiyorsanız, NuGet istemcisini, .NET Standard 2,0 kitaplıklarıyla çalışmak üzere [3.6.0 sürümüne yükseltdiğinizden](https://www.nuget.org/downloads) emin olun.

* EF Core, Xamarin ve .NET Native gibi diğer .NET uygulamalarında çalıştırılabilir. Ancak uygulamada bu uygulamalar, EF Core uygulamanızın ne kadar iyi çalıştığını etkileyebilecek çalışma zamanı kısıtlamalarına sahiptir. Daha fazla bilgi için bkz. [EF Core tarafından desteklenen .NET uygulamaları](xref:core/platforms/index).

* Son olarak, farklı veritabanı sağlayıcıları belirli veritabanı altyapısı sürümleri, .NET uygulamaları veya işletim sistemleri gerektirebilir. Uygulamanız için doğru ortamı destekleyen bir [EF Core veritabanı sağlayıcısının](xref:core/providers/index) bulunduğundan emin olun.

## <a name="get-the-entity-framework-core-runtime"></a>Entity Framework Core çalışma zamanını al

Bir uygulamaya EF Core eklemek için, kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yükleyebilirsiniz.

ASP.NET Core uygulaması oluşturuyorsanız, bellek içi ve SQL Server sağlayıcılarını yüklemeniz gerekmez. Bu sağlayıcılar, EF Core çalışma zamanının yanı sıra geçerli ASP.NET Core sürümlerine dahil edilmiştir.  

NuGet paketlerini yüklemek veya güncelleştirmek için, .NET Core komut satırı arabirimini (CLı), Visual Studio Paket Yöneticisi Iletişim kutusunu veya Visual Studio Paket Yöneticisi konsolunu kullanabilirsiniz.

### <a name="net-core-cli"></a>.NET Core CLI

* EF Core SQL Server sağlayıcısını yüklemek veya güncelleştirmek için işletim sisteminin komut satırından aşağıdaki .NET Core CLI komutunu kullanın:

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* Değiştiriciyi`-v` kullanarak `dotnet add package` komutta belirli bir sürümü belirtebilirsiniz. Örneğin, EF Core 2.2.0 paketlerini yüklemek için komuta ekleyin `-v 2.2.0` .

Daha fazla bilgi için bkz. [.NET komut satırı arabirimi (CLI) araçları](/dotnet/core/tools/).

### <a name="visual-studio-nuget-package-manager-dialog"></a>Visual Studio NuGet Paket Yöneticisi Iletişim kutusu

* Visual Studio menüsünden **proje > NuGet Paketlerini Yönet** ' i seçin.

* **Gözatmaya** veya **güncelleştirmeler** sekmesine tıklayın

* SQL Server sağlayıcıyı yüklemek veya güncelleştirmek için, `Microsoft.EntityFrameworkCore.SqlServer` paketi seçin ve onaylayın.

Daha fazla bilgi için bkz. [NuGet Paket Yöneticisi Iletişim kutusu](/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Visual Studio NuGet Paket Yöneticisi konsolu

* Visual Studio menüsünden **araçlar > NuGet paket yöneticisi > Paket Yöneticisi konsolu** ' nu seçin.

* SQL Server sağlayıcıyı yüklemek için, Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Sağlayıcıyı güncelleştirmek için `Update-Package` komutunu kullanın.

* Belirli bir sürümü belirtmek için `-Version` değiştiricisini kullanın. Örneğin, EF Core 2.2.0 paketlerini yüklemek için, komutlara ekleyin `-Version 2.2.0`

Daha fazla bilgi için bkz. [Paket Yöneticisi konsolu](/nuget/tools/package-manager-console).

## <a name="get-the-entity-framework-core-tools"></a>Entity Framework Core araçlarını al

Projenizde, veritabanı geçişleri oluşturma ve uygulama ya da var olan bir veritabanını temel alan bir EF Core modeli oluşturma gibi EF Core ilgili görevleri yürütmek için araçlar yükleyebilirsiniz.

İki araç kümesi mevcuttur:

* [.NET Core komut satırı arabirimi (CLI) araçları](xref:core/miscellaneous/cli/dotnet) Windows, Linux veya MacOS 'ta kullanılabilir. Bu komutlar ile `dotnet ef`başlar. 

* [Paket Yöneticisi Konsolu (PMC) araçları](xref:core/miscellaneous/cli/powershell) , Windows üzerinde Visual Studio 'da çalışır. Bu komutlar, örneğin `Add-Migration`, `Update-Database`bir fiil ile başlar.

Ayrıca, paket yöneticisi konsolundan `dotnet ef` komutları da kullanabilirsiniz, ancak Visual Studio 'yu kullanırken Paket Yöneticisi konsol araçlarının kullanılması önerilir:

* Bunlar, el ile geçiş yapmak zorunda kalmadan, Visual Studio 'da PMC 'de seçilen geçerli projeyle otomatik olarak çalışır.  

* Komut tamamlandıktan sonra Visual Studio 'da komutlar tarafından oluşturulan dosyaları otomatik olarak açar.

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a>.NET Core CLI araçlarını al

.NET Core CLI araçlar, [önkoşullardan](#prerequisites)daha önce bahsedilen .NET Core SDK gerektirir.

Komutlar .NET Core SDK geçerli sürümlerine dahil edilmiştir, ancak belirli bir projedeki komutları etkinleştirmek için `Microsoft.EntityFrameworkCore.Design` paketini yüklemelisiniz: `dotnet ef`

``` Console 
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

ASP.NET Core uygulamalar için bu paket otomatik olarak eklenir.

> [!IMPORTANT]      
> Her zaman çalışma zamanı paketlerinin ana sürümü ile eşleşen Araçlar paketinin sürümünü kullanın.

### <a name="get-the-package-manager-console-tools"></a>Paket Yöneticisi konsol araçlarını al

EF Core için Paket Yöneticisi konsol araçları 'nı almak için `Microsoft.EntityFrameworkCore.Tools` paketini yükledikten sonra. Örneğin, Visual Studio 'dan:

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

ASP.NET Core uygulamalar için bu paket otomatik olarak eklenir.

## <a name="upgrading-to-the-latest-ef-core"></a>En son EF Core yükseltme

* EF Core yeni bir sürümünü yayınlıyoruz, Microsoft. EntityFrameworkCore. SqlServer, Microsoft. EntityFrameworkCore. SQLite gibi EF Core projenin parçası olan sağlayıcıların yeni bir sürümünü de yayınlarız. Microsoft. EntityFrameworkCore. InMemory. Tüm geliştirmeleri almak için yalnızca sağlayıcının yeni sürümüne yükseltebilirsiniz. 

* EF Core, SQL Server ve bellek içi sağlayıcılar ASP.NET Core güncel sürümlerine dahildir. Mevcut bir ASP.NET Core uygulamasını EF Core daha yeni bir sürüme yükseltmek için ASP.NET Core sürümünü her zaman yükseltin.

* Üçüncü taraf veritabanı sağlayıcısı kullanan bir uygulamayı güncelleştirmeniz gerekiyorsa, kullanmak istediğiniz EF Core sürümü ile uyumlu bir sağlayıcının güncelleştirilmesini her zaman denetleyin. Örneğin, önceki sürümlere ait veritabanı sağlayıcıları EF Core çalışma zamanının 2,0 sürümü ile uyumlu değildir.

* EF Core için üçüncü taraf sağlayıcılar genellikle düzeltme eki sürümlerini EF Core çalışma zamanına göre serbest bırakmaz. Üçüncü taraf sağlayıcıyı kullanan bir uygulamayı EF Core bir düzeltme eki sürümüne yükseltmek için, Microsoft. EntityFrameworkCore ve Microsoft. EntityFrameworkCore. Ilikisel gibi bireysel EF Core çalışma zamanı bileşenlerine doğrudan başvuru eklemeniz gerekebilir.

* Mevcut bir uygulamayı EF Core en son sürümüne yükseltiyorsanız, eski EF Core paketlerine yapılan bazı başvuruların el ile kaldırılması gerekebilir:

  * Veritabanı sağlayıcısı tasarım zamanı paketleri `Microsoft.EntityFrameworkCore.SqlServer.Design` , gibi EF Core 2,0 ve sonraki sürümlerde artık gerekli değildir veya desteklenmemektedir, ancak diğer paketler yükseltilirken otomatik olarak kaldırılmaz.

  * .NET CLı araçları, sürüm 2,1 ' den beri .NET SDK 'da bulunur, bu nedenle bu pakete yönelik başvuru proje dosyasından kaldırılabilir:

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

* .NET Framework hedef uygulamaların .NET Standard 2,0 kitaplıklarıyla çalışması için değişiklikler gerekebilir:

  * Proje dosyasını düzenleyin ve aşağıdaki girdinin ilk özellik grubunda göründüğünden emin olun:

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Test projeleri için aşağıdaki girişin mevcut olduğundan da emin olun:

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
