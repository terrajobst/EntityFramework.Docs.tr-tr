---
title: EF çekirdek yükleniyor
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 31b96ebd0ae282b88be98988eff6263084dc5dd5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
ms.locfileid: "26054648"
---
# <a name="installing-ef-core"></a>EF çekirdek yükleniyor

## <a name="prerequisites"></a>Önkoşullar

(.NET Core hedef ASP.NET Core 2.0 uygulamalar da dahil) .NET Core 2.0 uygulamaları geliştirmek için bir sürümünü karşıdan yükleyip gerekir [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) platformunuz için uygun olan. **Visual Studio 2017 sürüm 15.3 yüklü olsa bile bu geçerlidir.**

EF çekirdek 2.0 veya herhangi bir .NET standart 2.0 kitaplığı .NET Core 2.0 yanı sıra .NET platformları ile kullanmak için (örneğin, .NET Framework 4.6.1 ile veya daha büyük) .NET standart 2.0 ve uyumlu çerçeveleri farkındadır NuGet sürümü gerekir. Bu edinebilirsiniz birkaç şekilde şunlardır:

* Visual Studio 2017 sürüm 15.3 yükleyin
* Visual Studio 2015 kullanıyorsanız [indirin ve NuGet İstemcisi sürüm 3.6.0 yükseltin](https://www.nuget.org/downloads)

Visual Studio ve .NET Framework'ü hedefleme önceki sürümleriyle oluşturulan projeleri .NET standart 2.0 kitaplıkları ile uyumlu olması için diğer tüm değişiklikleri gerekebilir:

* Proje dosyasını düzenleyin ve aşağıdaki girdiyi başlangıç özellik grubunda göründüğünden emin olun:
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* Test projeleri için ayrıca şu girdiyi bulunduğundan emin olun:
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a>BITS alma
EF çekirdek çalışma zamanı kitaplıkları bir uygulamaya eklemek için önerilen yol, bir EF çekirdek veritabanı sağlayıcısı Nuget'ten yüklemektir.

Çalışma zamanı kitaplıkları yanı sıra daha kolay oluşturma ve geçişler uygulama ve var olan bir veritabanını temel alan bir model oluşturma gibi tasarım zamanında projenizde birkaç EF çekirdek ile ilgili görevleri gerçekleştirmeniz için Araçlar yükleyebilirsiniz.

> [!TIP]  
> Bir üçüncü taraf veritabanı sağlayıcısı kullanarak bir uygulamayı güncelleştirmek gerekiyorsa, her zaman bir güncelleştirme EF kullanmak istediğiniz çekirdek sürümü ile uyumlu olan sağlayıcının denetleyin. Örneğin Önceki sürümler için veritabanı sağlayıcısı EF çekirdeği çalışma zamanı 2.0 sürümü ile uyumlu değil.  

> [!TIP]  
> ASP.NET Core 2.0 hedefleme uygulamaları EF çekirdek 2.0 üçüncü taraf veritabanı sağlayıcıları yanı sıra ek bağımlılıklar olmadan kullanabilir. EF çekirdek 2.0 kullanmak için ASP.NET Core 2.0 sürümüne yükseltmek ASP.NET Core önceki sürümlerini hedefleyen uygulama gerekir.

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a>.NET Core komut satırı arabirimi (CLI) kullanarak platformlar arası geliştirme

Hedefleyen uygulamalar geliştirmek için [.NET Core](https://www.microsoft.com/net/download/core) kullanmayı tercih edebileceğiniz [ `dotnet` CLI komutları](https://docs.microsoft.com/dotnet/core/tools/) , sık kullandığınız metin düzenleyiciyi ya da bir tümleşik geliştirme ortamı (IDE) bu tür ile birlikte Visual Studio, Visual Studio için Mac veya Visual Studio Code.

> [!IMPORTANT]  
> Visual Studio belirli sürümlerini .NET Core hedefleyen uygulamalar gerektirir, .NET Core 2.0 geliştirme Visual Studio 2017 sürüm 15.3 gerektirse .NET Core 1.x geliştirme Visual Studio 2017, örneğin gerektirir.

Yüklemek veya platformlar arası .NET Core uygulama SQL Server sağlayıcısında yükseltmek için uygulamanın dizinine geçin ve bir komut satırında aşağıdaki komutu çalıştırın:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Belirli bir sürümünü yüklemek için belirtebilir `dotnet add package` komutunu `-v` değiştiricisi. Örneğin EF çekirdek 2.0 paketleri yüklemek için append `-v 2.0.0` komutu.

EF çekirdek içeren bir dizi [için ek komutlar `dotnet` CLI](../../miscellaneous/cli/dotnet.md)sürümünden itibaren `dotnet ef`. Kullanmak için `dotnet ef` CLI komutları, uygulamanızın `.csproj` dosya şu girdiyi içermesi gerekiyor:

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

EF çekirdek için .NET Core CLI araçlarını da Microsoft.EntityFrameworkCore.Design adlı ayrı bir paket gerektirir. Yalnızca bu proje kullanmaya ekleyebilirsiniz:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> Her zaman ana sürümüne yönelik çalışma zamanı paketleri eşleşen araçları paketleri sürümlerini kullanın.

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a>Visual Studio geliştirme

Birçok farklı türde uygulamayı hedefleyen .NET Core, .NET Framework ve Visual Studio kullanarak EF çekirdek tarafından desteklenen diğer platformlar geliştirebilirsiniz.

Visual Studio'da, uygulamanızdaki bir EF çekirdek veritabanı sağlayıcısı yükleyebilirsiniz iki yolu vardır:

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a>NuGet kullanarak [Paket Yöneticisi kullanıcı arabirimi](https://docs.microsoft.com/nuget/tools/package-manager-ui)

* Select menüsünde **Proje > NuGet paketlerini Yönet**

* Tıklayın **Gözat** veya **güncelleştirmeleri** sekmesi

* Seçin `Microsoft.EntityFrameworkCore.SqlServer` paket ve istediğiniz sürümü ve onaylayın

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a>NuGet kullanarak [Paket Yöneticisi Konsolu (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)

* Select menüsünde **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**

* Yazın ve PMC aşağıdaki komutu çalıştırın:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Kullanabileceğiniz `Update-Package` daha yeni bir sürümü zaten yüklü olan bir paketi güncelleştirmek için bunun yerine komutu

* Belirli bir sürüm belirtmek için kullanabilirsiniz `-Version` değiştiricisi, örneğin EF çekirdek 2.0 paketleri yüklemek için append `-Version 2.0.0` komutları

#### <a name="tools"></a>Araçlar

Ayrıca bir PowerShell sürümü olan [EF çekirdek komutlar hangi çalışma PMC içinde](../../miscellaneous/cli/powershell.md) benzer özelliklere sahip Visual Studio'da `dotnet ef` komutları. Bunlar kullanmak üzere yükleyin `Microsoft.EntityFrameworkCore.Tools` Paket Yöneticisi kullanıcı Arabirimi veya PMC kullanarak paket.

> [!IMPORTANT]  
> Her zaman ana sürümüne yönelik çalışma zamanı paketleri eşleşen araçları paketleri sürümlerini kullanın.

> [!TIP]  
> Kullanmak mümkün olmakla `dotnet ef` komutları Visual Studio'da PMC gelen PowerShell sürümü kullanmak daha kullanışlı olduğu:
> * Bunlar otomatik olarak dizinler el ile geçiş gerek kalmadan PMC seçili geçerli proje ile çalışır.  
> * Bunlar, komut tamamlandıktan sonra Visual Studio komutları tarafından üretilen dosyalar otomatik olarak açılır.

> [!IMPORTANT]  
> **Kullanım dışı EF çekirdek 2.0 paketlerinde:** EF çekirdek 2.0 varolan bir uygulamaya yükseltme yapıyorsanız, eski EF temel paketler bazı başvuruları el ile kaldırılması gerekebilir. Özellikle, sağlayıcı tasarım zamanı paketleri gibi veritabanı `Microsoft.EntityFrameworkCore.SqlServer.Design` artık gerekli veya EF çekirdek 2. 0'desteklenir, ancak otomatik olarak diğer paketleri yükseltirken kaldırılmaz.
