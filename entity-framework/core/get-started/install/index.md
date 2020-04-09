---
title: Varlık Çerçeve Çekirdeğinin Yüklenmesi - EF Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 6575b1ac028f8b67b49ca7f4e49d6f19500be98f
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136178"
---
# <a name="installing-entity-framework-core"></a>Varlık Çerçeve Çekirdeğinin Yüklenmesi

## <a name="prerequisites"></a>Ön koşullar

* EF Core bir [.NET Standart 2.0](/dotnet/standard/net-standard) kitaplığıdır. Bu nedenle EF Core çalıştırmak için .NET Standart 2.0 destekleyen bir .NET uygulaması gerektirir. EF Core diğer .NET Standart 2.0 kitaplıkları tarafından da başvurulabilir.

* Örneğin, .NET Core'u hedefleyen uygulamalar geliştirmek için EF Core'u kullanabilirsiniz. .NET Core uygulamaları oluşturmak için [.NET Core SDK](https://dotnet.microsoft.com/download)gerekir. İsteğe bağlı olarak, ayrıca [Visual Studio](https://visualstudio.microsoft.com/vs)gibi bir geliştirme ortamı kullanabilirsiniz , Mac için [Visual Studio](https://visualstudio.microsoft.com/vs/mac), veya Visual [Studio Kodu](https://code.visualstudio.com). Daha fazla bilgi için [.NET Core ile Başlarken'i](/dotnet/core/get-started)kontrol edin.

* Visual Studio'yu kullanarak Windows'da uygulama geliştirmek için EF Core'u kullanabilirsiniz. [Visual Studio'nun](https://visualstudio.microsoft.com/vs) en son sürümü önerilir.

* EF Core, [Xamarin](https://dotnet.microsoft.com/apps/xamarin) ve .NET Native gibi diğer .NET uygulamalarında da kullanılabilir. Ancak uygulamada bu uygulamalar, EF Core'un uygulamanızda ne kadar iyi çalıştığını etkileyebilecek çalışma zamanı sınırlamalarına sahiptir. Daha fazla bilgi için [bkz.](xref:core/platforms/index)

* Son olarak, farklı veritabanı sağlayıcıları belirli veritabanı altyapısı sürümlerini, .NET uygulamalarını veya işletim sistemlerini gerektirebilir. Uygulamanız için doğru ortamı destekleyen bir [EF Core veritabanı sağlayıcısının](xref:core/providers/index) kullanılabilir olduğundan emin olun.

## <a name="get-the-entity-framework-core-runtime"></a>Varlık Framework Core çalışma süresini alın

Bir uygulamaya EF Core eklemek için, kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yükleyin.

Bir ASP.NET Core uygulaması oluşturuyorsanız, bellek ve SQL Server sağlayıcılarını yüklemeniz gerekmez. Bu sağlayıcılar, ASP.NET Core'un geçerli sürümlerinde, EF Core çalışma süresinin yanı sıra dahildir.  

NuGet paketlerini yüklemek veya güncellemek için .NET Core komut satırı arabirimini (CLI), Visual Studio Paket Yöneticisi İletişim Kutusunu veya Visual Studio Paket Yöneticisi Konsolunu kullanabilirsiniz.

### <a name="net-core-cli"></a>.NET Core CLI

* EF Core SQL Server sağlayıcısını yüklemek veya güncellemek için işletim sisteminin komut satırından aşağıdaki .NET Core CLI komutunu kullanın:

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* `-v` Değiştiriciyi kullanarak `dotnet add package` komutta belirli bir sürümü belirtebilirsiniz. Örneğin, EF Core 2.2.0 paketlerini `-v 2.2.0` yüklemek için komuta eklenir.

Daha fazla bilgi için [bkz.](/dotnet/core/tools/)

### <a name="visual-studio-nuget-package-manager-dialog"></a>Visual Studio NuGet Paket Yöneticisi İletişim

* Visual Studio menüsünden **NuGet Paketlerini Yönet> Proje'yi** seçin

* **Gözat'a** veya **Güncellemeler** sekmesine tıklayın

* SQL Server sağlayıcısını yüklemek veya `Microsoft.EntityFrameworkCore.SqlServer` güncelleştirmek için paketi seçin ve onaylayın.

Daha fazla bilgi için [NuGet Paket Yöneticisi İletişim Kutusu'na](/nuget/tools/package-manager-ui)bakın.

### <a name="visual-studio-nuget-package-manager-console"></a>Visual Studio NuGet Paket Yöneticisi Konsolu

* Visual Studio **menüsünden, NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu > Araçlar'ı** seçin

* SQL Server sağlayıcısını yüklemek için Paket Yöneticisi Konsolunda aşağıdaki komutu çalıştırın:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```

* Sağlayıcıyı güncelleştirmek için `Update-Package` komutu kullanın.

* Belirli bir sürümü belirtmek `-Version` için değiştiriciyi kullanın. Örneğin, EF Core 2.2.0 paketlerini `-Version 2.2.0` yüklemek için, komutlara ek

Daha fazla bilgi için [Paket Yöneticisi Konsolu'na](/nuget/tools/package-manager-console)bakın.

## <a name="get-the-entity-framework-core-tools"></a>Varlık Çerçeve Core araçlarını alın

Projenizde, veritabanı geçişleri oluşturma ve uygulama veya varolan bir veritabanını temel alan bir EF Core modeli oluşturma gibi EF Core ile ilgili görevleri yürütmek için araçlar yükleyebilirsiniz.

İki araç kümesi mevcuttur:

* [.NET Core komut satırı arabirimi (CLI) araçları](xref:core/miscellaneous/cli/dotnet) Windows, Linux veya macOS'ta kullanılabilir. Bu komutlar `dotnet ef`.

* [Paket Yöneticisi Konsolu (PMC) araçları](xref:core/miscellaneous/cli/powershell) Windows'daki Visual Studio'da çalışır. Bu komutlar bir fiille `Add-Migration`başlar, örneğin . `Update-Database`.

Paket Yöneticisi Konsolu'ndaki `dotnet ef` komutları da kullanabilirsiniz, ancak Visual Studio'yu kullanırken Paket Yöneticisi Konsolu araçlarını kullanmanız önerilir:

* Görsel Studio'daki PMC'de seçilen geçerli projeyle, dizinleri el ile değiştirmeye gerek kalmadan otomatik olarak çalışırlar.  

* Komut tamamlandıktan sonra Visual Studio'daki komutlar tarafından oluşturulan dosyaları otomatik olarak açarlar.

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a>.NET Core CLI araçlarını alın

.NET Core CLI araçları, daha önce [Önkoşullar'da](#prerequisites)belirtilen .NET Core SDK'yı gerektirir.

Komutlar `dotnet ef` .NET Core SDK'nın geçerli sürümlerinde yer almakla birlikte, belirli bir projedeki `Microsoft.EntityFrameworkCore.Design` komutları etkinleştirmek için paketi yüklemeniz gerekir:

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]
> Her zaman çalışma zamanı paketlerinin ana sürümüyle eşleşen araçlar paketinin sürümünü kullanın.

### <a name="get-the-package-manager-console-tools"></a>Paket Yöneticisi Konsol uçağını alma

EF Core için Package Manager Console araçlarını `Microsoft.EntityFrameworkCore.Tools` almak için paketi yükleyin. Örneğin, Visual Studio'dan:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

core uygulamaları ASP.NET için bu paket otomatik olarak dahildir.

## <a name="upgrading-to-the-latest-ef-core"></a>En son EF Çekirdeğine yükseltme

* EF Core'un yeni bir sürümünü yayınladığımız her zaman, Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite ve Microsoft.EntityFrameworkCore.InMemory gibi EF Core projesinin bir parçası olan sağlayıcıların yeni bir sürümünü de yayınlıyoruz. Tüm iyileştirmeleri almak için sağlayıcının yeni sürümüne yükseltebilirsiniz.

* EF Core, SQL Server ve bellek içi sağlayıcılarla birlikte ASP.NET Core'un geçerli sürümlerinde yer almaktadır. Varolan bir ASP.NET Core uygulamasını EF Core'un daha yeni bir sürümüne yükseltmek için, her zaman ASP.NET Core sürümünü yükseltin.

* Üçüncü taraf veritabanı sağlayıcısı nı kullanan bir uygulamayı güncelleştirmeniz gerekiyorsa, her zaman kullanmak istediğiniz EF Core sürümüyle uyumlu sağlayıcının güncelleştirmesini denetleyin. Örneğin, önceki sürümler için veritabanı sağlayıcıları EF Core çalışma zamanının 2.0 sürümüyle uyumlu değildir.

* EF Core için üçüncü taraf sağlayıcılar genellikle EF Core çalışma zamanının yanında yama sürümleri yayınlamaz. Bir üçüncü taraf sağlayıcıkullanan bir uygulamayı EF Core'un yama sürümüne yükseltmek için Microsoft.EntityFrameworkCore ve Microsoft.EntityFrameworkCore.Relational gibi tek tek EF Core çalışma zamanı bileşenlerine doğrudan başvuru eklemeniz gerekebilir.

* Varolan bir uygulamayı EF Core'un en son sürümüne yükseltiyorsanız, eski EF Core paketlerine yapılan bazı başvuruların el ile kaldırılması gerekebilir:

  * Veritabanı sağlayıcısı tasarım zamanı `Microsoft.EntityFrameworkCore.SqlServer.Design` paketleri artık GEREKLI değildir veya EF Core 2.0 ve sonraki düzeylerinden desteklenmez, ancak diğer paketleri yükseltirken otomatik olarak kaldırılmaz.

  * .NET CLI araçları sürüm 2.1'den beri .NET SDK'ya dahildir, böylece bu pakete yapılan başvuru proje dosyasından kaldırılabilir:

    ``` xml
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```
