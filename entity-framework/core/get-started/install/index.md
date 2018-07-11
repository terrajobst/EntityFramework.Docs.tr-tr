---
title: EF Core'u yükleme
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 7bb2ee11940a4fd5736c7a23c16533ef53018f7b
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949198"
---
# <a name="installing-ef-core"></a>EF Core'u yükleme

## <a name="prerequisites"></a>Önkoşullar

(.NET Core hedefleyen ASP.NET Core 2.0 uygulamaları da dahil) .NET Core 2.0 uygulamaları geliştirmek için bir sürümünü karşıdan yükleyip gerekir [.NET Core 2.0 SDK'sını](https://www.microsoft.com/net/download/core) platformunuz için uygun olan. **Visual Studio 2017 sürüm 15.3 yüklü olsa bile bu geçerlidir.**

EF Core 2.0 veya herhangi bir .NET Standard 2.0 kitaplığı bir .NET Core 2.0 yanı sıra .NET platformları ile kullanmak için (örneğin, .NET Framework 4.6.1 ile veya üzeri) tanımaz ve bunun uyumlu çerçeveleri .NET Standard 2.0 NuGet sürümü gerekir. Bunu elde edebilirsiniz birkaç yolu vardır:

* Visual Studio 2017 sürüm 15.3 yükleyin
* Visual Studio 2015 kullanıyorsanız [indirin ve NuGet istemci 3.6.0 sürümüne yükseltme](https://www.nuget.org/downloads)

Visual Studio ve .NET Framework'ü hedefleyen önceki sürümleriyle oluşturulan projeleri .NET Standard 2.0 kitaplıklarla uyumlu olması için ek değişiklikleri gerekebilir:

* Proje dosyasını düzenleyin ve şu girişi ilk özellik grubunda göründüğünden emin olun:
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* Test projeleri için şu girdiyi mevcut olduğundan emin olun:
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a>BITS alma
EF Core çalışma zamanı kitaplıkları bir uygulamaya eklemek için önerilen yol, bir EF Core veritabanı sağlayıcısı Nuget'ten yüklemektir.

Çalışma zamanı kitaplıklarının yanı sıra, oluşturma ve geçişler uygulama ve var olan bir veritabanını temel alan bir model oluşturma gibi tasarım zamanında projenizde EF Core ile ilgili çeşitli görevleri gerçekleştirmek kolaylaştıran Araçlar yükleyebilirsiniz.

> [!TIP]  
> Bir üçüncü taraf veritabanı sağlayıcısı kullanan bir uygulamayı güncelleştirmek gerekiyorsa, kullanmak istediğiniz EF Core sürümüyle uyumlu sağlayıcısını güncelleştirmesi için her zaman denetleyin. Örneğin, önceki sürümler için veritabanı sağlayıcıları EF Core çalışma zamanı 2.0 sürümü ile uyumlu değildir.  

> [!TIP]  
> ASP.NET Core 2.0 hedefleyen uygulamalar, üçüncü taraf veritabanı sağlayıcıları yanı sıra ek bağımlılıkları olmadan EF Core 2.0 kullanabilirsiniz. ASP.NET Core'nın önceki sürümlerini hedefleyen uygulamalar EF Core 2.0 kullanmak için ASP.NET Core 2. 0'ı yükseltmeniz gerekir.

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a>.NET Core komut satırı arabirimi (CLI) kullanarak platformlar arası geliştirme

Hedefleyen uygulamalar geliştirmek için [.NET Core](https://www.microsoft.com/net/download/core) kullanmayı da tercih edebilirsiniz [ `dotnet` CLI komutları](https://docs.microsoft.com/dotnet/core/tools/) sevdiğiniz bir metin düzenleyici ya da bir tümleşik geliştirme ortamı (IDE) tür ile birlikte Visual Studio, Visual Studio Mac veya Visual Studio Code için.

> [!IMPORTANT]  
> .NET Core hedefleyen uygulamalar, Visual Studio'nun belirli sürümlerini gerektirir. Örneğin, .NET Core 2.0 geliştirme Visual Studio 2017 sürüm 15.3 gerekirken Visual Studio 2017, .NET Core 1.x geliştirme gerektirir.

Platformlar arası .NET Core uygulamasında SQL Server sağlayıcıyı yükseltin veya yüklemek için uygulamanın dizinine geçin ve komut satırında aşağıdaki komutu çalıştırın:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Belirli bir sürümünü yükleme, belirtebilir `dotnet add package` komutu `-v` değiştiricisi. Örneğin, EF Core 2.0 paketleri yüklemek için URL'ye `-v 2.0.0` komutu.

EF Core içeren bir dizi [ek komutlar için `dotnet` CLI](../../miscellaneous/cli/dotnet.md)ile başlayan `dotnet ef`. Kullanmak için `dotnet ef` CLI komutları, uygulamanızın `.csproj` dosya şu girdiyi içermesi gerekiyor:

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

EF Core için .NET Core CLI araçları, ayrıca Microsoft.EntityFrameworkCore.Design adlı ayrı bir paketi gerektirir. Basitçe bunu kullanarak projenize ekleyebilirsiniz:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> Her zaman ana sürümü çalışma zamanı paketlerini eşleşen araçları paketleri sürümlerini kullanın.

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a>Visual Studio geliştirme

Hedefleyen .NET Core, .NET Framework ve Visual Studio kullanarak EF Core tarafından desteklenen diğer platformlarda farklı türlerde uygulamalar geliştirebilirsiniz.

Uygulamanızı Visual Studio'dan bir EF Core veritabanı sağlayıcısı yüklemeden iki yolu vardır:

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a>NuGet kullanarak [Paket Yöneticisi kullanıcı arabirimi](https://docs.microsoft.com/nuget/tools/package-manager-ui)

* Seçim menüsünde **Proje > NuGet paketlerini Yönet**

* Tıklayarak **Gözat** veya **güncelleştirmeleri** sekmesi

* Seçin `Microsoft.EntityFrameworkCore.SqlServer` paket ve istenen sürüm ve onaylayın

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a>NuGet kullanarak [Paket Yöneticisi Konsolu (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)

* Seçim menüsünde **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**

* Yazın ve PMC'de aşağıdaki komutu çalıştırın:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Kullanabileceğiniz `Update-Package` daha yeni bir sürümü zaten yüklü olan bir paketi güncelleştirmek için bunun yerine komutu

* Belirli bir sürümünü belirlemek için kullanabileceğiniz `-Version` değiştiricisi. Örneğin, EF Core 2.0 paketleri yüklemek için URL'ye `-Version 2.0.0` komutlar

#### <a name="tools"></a>Araçlar

Bir PowerShell sürümü de mevcuttur [EF Core komutlar hangi çalışma PMC'yi içinde](../../miscellaneous/cli/powershell.md) için benzer özelliklere sahip, Visual Studio'da `dotnet ef` komutları. Bunları kullanmak için yükleme `Microsoft.EntityFrameworkCore.Tools` Paket Yöneticisi kullanıcı Arabirimi veya PMC'yi kullanarak paket.

> [!IMPORTANT]  
> Her zaman ana sürümü çalışma zamanı paketlerini eşleşen araçları paketleri sürümlerini kullanın.

> [!TIP]  
> Kullanılması mümkün olsa da `dotnet ef` komutları Visual Studio'da PMC'yi gelen PowerShell sürümü kullanmak çok daha kullanışlı olduğu:
> * Bunlar otomatik olarak geçerli proje dizinleri el ile geçiş gerek kalmadan PMC'de seçili çalışın.  
> * Bunlar, komut tamamlandıktan sonra Visual Studio'da komutlar tarafından üretilen dosyalar otomatik olarak açılır.

> [!IMPORTANT]  
> **EF Core 2.0 paketlerinde kullanım dışı:** EF Core 2.0 varolan bir uygulamaya yükseltme yapıyorsanız, eski EF Core paketleri bazı başvuruları el ile kaldırılması gerekir. Özellikle, sağlayıcı tasarım zamanı paketleri gibi veritabanı `Microsoft.EntityFrameworkCore.SqlServer.Design` artık gerekli ve EF Core 2.0 sürümünde desteklenir, ancak otomatik olarak diğer paketleri yükseltirken kaldırılmaz.
