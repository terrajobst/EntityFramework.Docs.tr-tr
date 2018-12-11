---
title: Entity Framework Core yükleme
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 58c79d477d590eea355a922b3e1233bbecb305cc
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53181987"
---
# <a name="installing-entity-framework-core"></a>Entity Framework Core yükleme

## <a name="prerequisites"></a>Önkoşullar

* EF Core bir [.NET Standard 2.0](/dotnet/standard/net-standard) kitaplığı. Bu nedenle EF Core çalıştırmak için .NET Standard 2.0 destekleyen bir .NET uygulaması gerektirir. EF Core, ayrıca diğer .NET Standard 2.0 kitaplıkları tarafından başvurulabilir. 

* Örneğin, EF Core, .NET Core'u hedefleyen uygulamalar geliştirmek için kullanabilirsiniz. .NET Core uygulamaları oluşturma gerektirir [.NET Core SDK'sı](https://dotnet.microsoft.com/download). İsteğe bağlı olarak, Visual Studio, Visual Studio gibi bir geliştirme ortamı Mac ya da Visual Studio Code için kullanabilirsiniz. Daha fazla bilgi için kontrol [.NET Core ile çalışmaya başlama](/dotnet/core/get-started).

* EF Core, Visual Studio kullanarak Windows, .NET Framework 4.6.1 veya sonraki bir sürümü hedefleyen uygulamalar geliştirmek için kullanabilirsiniz. Visual Studio'nun en son sürümü önerilir. Visual Studio 2015 gibi eski bir sürümünü kullanmak istiyorsanız, emin [3.6.0 sürümüne NuGet istemci yükseltme](https://www.nuget.org/downloads) .NET Standard 2.0 kitaplıkları ile çalışmak için.

* EF Core, Xamarin ve .NET Native gibi diğer .NET uygulamaları üzerinde çalıştırabilirsiniz. Ancak, uygulamada söz konusu uygulamaları EF Core uygulamanızı nasıl çalıştığı etkileyebilecek çalışma zamanı sınırlamaları vardır. Daha fazla bilgi için [EF Core tarafından desteklenen .NET uygulamalarıyla](xref:core/platforms/index).

* Son olarak, farklı veritabanı sağlayıcıları belirli veritabanı altyapısı sürümleri, .NET uygulamaları veya işletim sistemi gerektirebilir. Emin bir [EF Core veritabanı sağlayıcısı](xref:core/providers/index) olan kullanılabilir, doğru ortamda uygulamanız için destekler.

## <a name="get-the-entity-framework-core-runtime"></a>Entity Framework Core çalışma zamanı Al

EF Core bir uygulamaya eklemek için kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yükleyin.

Bir ASP.NET Core uygulaması derliyorsanız, bellek ve SQL Server sağlayıcıları yüklemek gerekmez. Sağlayıcılar, ASP.NET Core, geçerli sürümlerinde EF Core çalışma zamanı ile birlikte eklenir.  

NuGet paketlerini güncelleştirmek veya yüklemek için [.NET Core komut satırı arabirimi (CLI), Visual Studio Paket Yöneticisi iletişim kutusu veya Visual Studio Paket Yöneticisi konsolu. kullanabilirsiniz.

### <a name="net-core-cli"></a>.NET core CLI

* Yükleme veya güncelleştirme EF Core SQL Server sağlayıcısı için işletim sistemi komut satırından aşağıdaki .NET Core CLI komutunu kullanın:

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* Belirli bir sürümde belirtebilir `dotnet add package` komutu `-v` değiştiricisi. Örneğin, EF Core 2.2.0 paketleri yüklemek için URL'ye `-v 2.2.0` komutu.

Daha fazla bilgi için [.NET komut satırı arabirimi (CLI) araçlarını](/dotnet/core/tools/).

### <a name="visual-studio-nuget-package-manager-dialog"></a>Visual Studio NuGet Paket Yöneticisi iletişim kutusu

* Visual Studio menüden **Proje > NuGet paketlerini Yönet**

* Tıklayarak **Gözat** veya **güncelleştirmeleri** sekmesi

* SQL Server sağlayıcısı güncelleştirmek veya yüklemek için seçin `Microsoft.EntityFrameworkCore.SqlServer` paketini ve onaylayın.

Daha fazla bilgi için [NuGet Paket Yöneticisi iletişim](/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Visual Studio, NuGet Paket Yöneticisi Konsolu

* Visual Studio menüden **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**

* SQL Server sağlayıcıyı yüklemek için Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Sağlayıcı güncelleştirmek için `Update-Package` komutu.

* Belirli bir sürümünü belirtmek için kullanın `-Version` değiştiricisi. Örneğin, EF Core 2.2.0 paketleri yüklemek için URL'ye `-Version 2.2.0` komutlar

Daha fazla bilgi için [Paket Yöneticisi Konsolu](/nuget/tools/package-manager-console).

## <a name="get-the-entity-framework-core-tools"></a>Entity Framework Core araçları edinin

EF Core ile ilgili görevleri oluşturmak ve veritabanı geçişlerini uygulama veya varolan bir veritabanını temel alan bir EF Core modeli oluşturma gibi projenizdeki yürütmek için Araçlar yükleyebilirsiniz.

İki araç vardır:

* [.NET Core komut satırı arabirimi (CLI) araçlarını](xref:core/miscellaneous/cli/dotnet) Windows, Linux veya Macos'ta kullanılabilir. Bu komutlar şununla `dotnet ef`. 

* [Paket Yöneticisi Konsolu (PMC) araçları](xref:core/miscellaneous/cli/powershell) Windows üzerinde Visual Studio'da çalıştırın. Bu komutlar, örneğin bir fiili ile başlatın `Add-Migration`, `Update-Database`.

De kullanabilirsiniz ancak `dotnet ef` komutları Paket Yöneticisi konsolunda, Visual Studio kullanıyorsanız, Paket Yöneticisi konsolu araçlarını kullanmak için önerilir:

* Bunlar otomatik olarak geçerli proje dizinleri el ile geçiş gerek kalmadan Visual Studio'da PMC'yi seçili çalışın.  

* Bunlar, komut tamamlandıktan sonra Visual Studio'da komutlar tarafından üretilen dosyalar otomatik olarak açılır.

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a>.NET Core CLI araçları edinin

.NET core CLI araçları, daha önce bahsedilen .NET Core SDK gerektirir [önkoşulları](#prerequisites).

`dotnet ef` Komutları, .NET Core SDK'ın geçerli sürümlerinde dahil edilir, ancak yüklemeniz gereken komutları belirli bir projede etkinleştirmek için `Microsoft.EntityFrameworkCore.Design` paket:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

ASP.NET Core uygulamaları için bu paket otomatik olarak dahil edilir.

> [!IMPORTANT]      
> Her zaman ana sürümü çalışma zamanı paketlerini eşleşen araçları paket sürümünü kullanın.

### <a name="get-the-package-manager-console-tools"></a>Paket Yöneticisi konsolu araçları edinin

Paket Yöneticisi konsolu EF Core için araçları almak için yükleyin `Microsoft.EntityFrameworkCore.Tools` paket. Örneğin, Visual Studio'dan:

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

ASP.NET Core uygulamaları için bu paket otomatik olarak dahil edilir.

## <a name="upgrading-to-the-latest-ef-core"></a>En son EF Core için yükseltme

* Herhangi bir zamanda EF Core yeni bir sürümünü yayınlayabilir, ayrıca sağlayıcıları gibi Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, EF Core projenin bir parçası olan yeni bir sürümünü yayınlayabilir ve Microsoft.EntityFrameworkCore.InMemory. Yalnızca tüm geliştirmeleri almak için sağlayıcı yeni sürüme yükseltebilirsiniz. 

* SQL Server ve bellek içi sağlayıcıları ile birlikte EF çekirdekli ASP.NET Core'nın geçerli sürümlerinde dahil edilir. Mevcut bir ASP.NET Core uygulaması EF Core daha yeni bir sürüme yükseltmek için her zaman ASP.NET Core sürümünü yükseltin.

* Bir üçüncü taraf veritabanı sağlayıcısı kullanan bir uygulamayı güncelleştirmek gerekiyorsa, kullanmak istediğiniz EF Core sürümüyle uyumlu sağlayıcısını güncelleştirmesi için her zaman denetleyin. Örneğin, önceki sürümler için veritabanı sağlayıcıları EF Core çalışma zamanı 2.0 sürümü ile uyumlu değildir.

* Genellikle üçüncü taraf sağlayıcılar EF Core için düzeltme eki sürümleri yanında EF Core çalışma zamanı sürüm yok. Bir üçüncü taraf sağlayıcı için bir düzeltme eki sürümü EF Core kullanan bir uygulamayı yükseltmeye Microsoft.EntityFrameworkCore ve Microsoft.EntityFrameworkCore.Relational gibi bireysel EF Core çalışma zamanı bileşenleri için doğrudan bir başvuru eklemeniz gerekebilir.

* Var olan uygulamanın EF Core en son sürümüne yükseltme yapıyorsanız, eski EF Core paketleri bazı başvuruları el ile kaldırılması gerekebilir:

  * Sağlayıcı tasarım zamanı paketleri gibi veritabanı `Microsoft.EntityFrameworkCore.SqlServer.Design` artık gerekli ve üzeri veya EF Core 2.0 desteklenir, ancak diğer paketleri yükseltme sırasında otomatik olarak kaldırılmaz.

  * Bu paket başvurusu proje dosyasından kaldırılabilmesi için .NET CLI araçları 2.1 sürümünden itibaren .NET SDK'yı dahil edilmiştir:

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

* .NET Framework'ü hedefleyen uygulamalar .NET Standard 2.0 kitaplıkları ile çalışmak için değişiklikleri gerekebilir:

  * Proje dosyasını düzenleyin ve şu girişi ilk özellik grubunda göründüğünden emin olun:

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Test projeleri için şu girdiyi mevcut olduğundan emin olun:

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
