---
title: Entiy Framework Core yükleme
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 7831e6a54e885cf0b89ef3eef2cd81a9292df606
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250328"
---
# <a name="installing-entity-framework-core"></a>Entity Framework Core yükleme

## <a name="prerequisites"></a>Önkoşullar

* .NET Core 2.1'i hedefleyen uygulamalar geliştirmek için yükleme [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core). SDK, Visual Studio 2017'in en son sürümü olsa bile yüklenmesi gerekir.

* Visual Studio .NET Core 2.1 hedefleyen uygulamalar geliştirilmesi için kullanılacak Visual Studio 2017 sürüm 15.7 veya sonraki bir sürümü yükleyin.

* Entity Framework 2.1 ASP.NET Core uygulamaları kullanmak için ASP.NET Core 2.1 kullanın. ASP.NET Core önceki sürümlerini kullanan uygulamaları 2.1 için güncelleştirilmesi gerekir.

* Visual Studio 2015, .NET Framework 4.6.1'i hedefleyen uygulamalar için kullanabilirsiniz veya üzeri. Ancak, uyumlu çerçeveleri ve .NET Standard 2.0 ile uyumlu olan NuGet sürümü gerekir. Visual Studio 2015'te bu alınacağı [3.6.0 sürümüne NuGet istemci yükseltme](https://www.nuget.org/downloads).

## <a name="get-the-entity-framework-core-runtime"></a>Entity Framework Core çalışma zamanı Al

EF Core çalışma zamanı kitaplıkları bir uygulamaya eklemek için kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yükleyin. Desteklenen sağlayıcılar ve NuGet paket adlarının bir listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).

NuGet paketlerini güncelleştirmek veya yüklemek için .NET Core CLI, Visual Studio Paket Yöneticisi iletişim kutusu ya da Visual Studio Paket Yöneticisi konsolu kullanın.

Ayrı olarak yüklemeniz gerekmez. Bu nedenle ASP.NET Core 2.1 uygulamaları için SQL Server sağlayıcıları ve bellek içi otomatik olarak eklenir.

> [!TIP]  
> Bir üçüncü taraf veritabanı sağlayıcısı kullanan bir uygulamayı güncelleştirmek gerekiyorsa, kullanmak istediğiniz EF Core sürümüyle uyumlu sağlayıcısını güncelleştirmesi için her zaman denetleyin. Örneğin, önceki sürümler için veritabanı sağlayıcıları EF Core runtime'nın 2.1 sürümü ile uyumlu değildir.  

### <a name="net-core-cli"></a>.NET core CLI

Aşağıdaki .NET Core CLI komutunu yükler veya SQL Server sağlayıcısını güncelleştirir:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Belirli bir sürümde belirtebilir `dotnet add package` komutu `-v` değiştiricisi. Örneğin, EF Core 2.1.0 paketleri yüklemek için URL'ye `-v 2.1.0` komutu.

### <a name="visual-studio-nuget-package-manager-dialog"></a>Visual Studio NuGet Paket Yöneticisi iletişim kutusu

* Menüden **Proje > NuGet paketlerini Yönet**

* Tıklayarak **Gözat** veya **güncelleştirmeleri** sekmesi

* SQL Server sağlayıcısı güncelleştirmek veya yüklemek için seçin `Microsoft.EntityFrameworkCore.SqlServer` paketini ve onaylayın.

Daha fazla bilgi için [NuGet Paket Yöneticisi iletişim](https://docs.microsoft.com/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Visual Studio, NuGet Paket Yöneticisi Konsolu

* Menüden **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**

* SQL Server sağlayıcıyı yüklemek için Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Sağlayıcı güncelleştirmek için `Update-Package` komutu.

* Belirli bir sürümünü belirtmek için kullanın `-Version` değiştiricisi. Örneğin, EF Core 2.1.0 paketleri yüklemek için URL'ye `-Version 2.1.0` komutlar

Daha fazla bilgi için [Paket Yöneticisi Konsolu](https://docs.microsoft.com/nuget/tools/package-manager-console).

## <a name="get-entity-framework-core-tools"></a>Entity Framework Core Araçları'nı edinin

Çalışma zamanı kitaplıklarının yanı sıra, EF Core ile ilgili bazı görevleri tasarım zamanında projenizde gerçekleştirebilirsiniz Araçlar yükleyebilirsiniz. Örneğin, geçişler oluşturmak, geçişler uygulamak ve var olan bir veritabanını temel alan bir model oluşturma.

İki araç vardır:
* .NET Core [komut satırı arabirimi (CLI) araçlarını](../../miscellaneous/cli/dotnet.md) Windows, Linux veya Macos'ta kullanılabilir. Bu komutlar şununla `dotnet ef`. 
* [Paket Yöneticisi konsolu Araçları](../../miscellaneous/cli/powershell.md) Visual Studio 2017'de Windows üzerinde çalıştırın. Bu komutlar, örneğin bir fiili ile başlatın `Add-Migration`, `Update-Database`.

Hizmetini kullanıyor olsanız da `dotnet ef` Paket Yöneticisi konsolundan çalıştığınızda, Visual Studio Paket Yöneticisi konsolu araçları kullanmak daha kullanışlı olduğu komutları:
* Bunlar Paket Yöneticisi konsolunda el ile dizin değiştirmeyi gerek kalmadan seçilen geçerli proje ile otomatik olarak çalışır.  
* Bunlar, komut tamamlandıktan sonra Visual Studio'da komutlar tarafından üretilen dosyalar otomatik olarak açılır.

<a name="cli"></a>

### <a name="get-the-cli-tools"></a>CLI araçları edinin

`dotnet ef` Komutları, .NET Core SDK'sı dahil edilir, ancak yüklemeniz gereken komutları etkinleştirmek için `Microsoft.EntityFrameworkCore.Design` paket:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

ASP.NET Core 2.1 uygulamaları için bu paket otomatik olarak dahil edilir.

Daha önce açıklandığı gibi [önkoşulları](#prerequisites), ayrıca .NET Core 2.1 SDK'yı yüklemeniz gerekir.

> [!IMPORTANT]      
> Her zaman ana sürümü çalışma zamanı paketlerini eşleşen araçları paket sürümünü kullanın.

### <a name="get-the-package-manager-console-tools"></a>Paket Yöneticisi konsolu araçları edinin

Paket Yöneticisi konsolu EF Core için araçları almak için yükleyin `Microsoft.EntityFrameworkCore.Tools` paket:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

ASP.NET Core 2.1 uygulamaları için bu paket otomatik olarak dahil edilir.

## <a name="upgrading-to-ef-core-21"></a>EF Core 2.1 yükseltme

EF Core 2.1 varolan bir uygulamaya yükseltme yapıyorsanız, eski EF Core paketleri bazı başvuruları el ile kaldırılması gerekebilir:

* Sağlayıcı tasarım zamanı paketleri gibi veritabanı `Microsoft.EntityFrameworkCore.SqlServer.Design` artık gerekli ve EF Core 2.1 içinde desteklenmez, ancak diğer paketleri yükseltme sırasında otomatik olarak kaldırılmaz.

* Bu paket başvurusu gelen kaldırılabilmesi için .NET CLI araçları artık .NET SDK'da dahil *.csproj* dosyası:

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

Visual Studio'nun önceki sürümleri tarafından oluşturulmuş ve .NET Framework'ü hedefleyen uygulamalar için .NET Standard 2.0 kitaplıkları ile uyumlu olduklarından emin olun:

  * Proje dosyasını düzenleyin ve şu girişi ilk özellik grubunda göründüğünden emin olun:

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Test projeleri için şu girdiyi mevcut olduğundan emin olun:

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
