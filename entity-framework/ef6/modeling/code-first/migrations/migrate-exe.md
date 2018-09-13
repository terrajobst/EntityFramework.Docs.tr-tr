---
title: Migrate.exe - EF6 kullanarak
author: divega
ms.date: 10/23/2016
ms.assetid: 989ea862-e936-4c85-926a-8cfbef5df5b8
ms.openlocfilehash: 6e9880523bbcf2fe55390a447241e59723a0967f
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490232"
---
# <a name="using-migrateexe"></a>Migrate.exe kullanma
Code First geçişleri, bir veritabanında güncelleştirmek için kullanılabilir visual studio içinde ancak komut satırı aracı migrate.exe da yürütülebilir. Bu sayfa bir veritabanında geçişleri yürütülecek migrate.exe kullanma hakkında hızlı bir genel bakış sunar.

> [!NOTE]
> Bu makalede, temel senaryolarda Code First Migrations nasıl kullanılacağını varsayar. Sizin sonra okumak ihtiyacınız olacak [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) devam etmeden önce.

## <a name="copy-migrateexe"></a>Migrate.exe kopyalayın

NuGet kullanarak Entity Framework yüklediğinizde migrate.exe indirilen paketi Araçlar klasöründen olacaktır. İçinde &lt;proje klasörü&gt;\\paketleri\\EntityFramework.&lt; Sürüm&gt;\\araçları

Migrate.exe sonra geçiş içeren derlemenin konumuna kopyalamanız gerekir.

Uygulamanız .NET 4 hedefliyor ve 4.5 değil, ardından kopyalamak ihtiyacınız olacak **Redirect.config** konuma olarak yanı sıra ve yeniden adlandırmak **migrate.exe.config**. Bu, böylelikle Migrate.exe Entity Framework derlemeyi bulabilir olması için doğru bağlama yeniden yönlendirmeleri alır.

| .NET 4.5                                   | .NET 4.0                                   |
|:-------------------------------------------|:-------------------------------------------|
| ![.NET 4.5 dosyaları](~/ef6/media/net45files.png)  | ![.NET 4.0 dosyaları](~/ef6/media/net40files.png)  |

> [!NOTE]
> migrate.exe x64 desteklemiyor derlemeler.

Ardından doğru klasöre migrate.exe taşıdığınızda geçişleri veritabanında yürütmek için kullanabilmek için olmalıdır. Yardımcı program yapmak için tasarlanmış olan geçişleri yürütün. Geçişleri oluşturmak veya bir SQL betiği oluşturun.

## <a name="see-options"></a>Bkz. seçenekleri

``` console
Migrate.exe /?
```

Yukarıdaki bu yardımcı programı, EntityFramework.dll migrate.exe Bunun çalışması sırayla çalıştırdığınız aynı konumda olması gerekecektir not ile ilişkili Yardım sayfası görüntülenir.

## <a name="migrate-to-the-latest-migration"></a>En son geçiş için geçiş

``` console
Migrate.exe MyMvcApplication.dll /startupConfigurationFile=”..\\web.config”
```

Yapılandırma dosyası belirtmezseniz, ne zaman migrate.exe yalnızca zorunlu bir parametre çalıştırmayı denediğiniz geçişleri içeren derlemenin, derleme çalışıyor ancak tüm kuralı kullanacağı ayarları temel.

## <a name="migrate-to-a-specific-migration"></a>Belirli bir geçiş için geçiş

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /targetMigration=”AddTitle”
```

Ardından belirli bir geçiş kadar geçiş çalıştırmak istiyorsanız, geçiş adını belirtebilirsiniz. Bu tüm önceki geçişler gerektiği gibi çalışır belirtilen geçiş alma kadar.

## <a name="specify-working-directory"></a>Çalışma dizini belirtin

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /startupDirectory=”c:\\MyApp”
```

Ardından, derleme bağımlılıkları içeriyorsa ya da çalışma dizinine göre dosyalarını okur startupDirectory ayarlamak almanız gerekir.

## <a name="specify-migration-configuration-to-use"></a>Kullanmak için geçiş yapılandırmasını belirtin

``` console
Migrate.exe MyAssembly CustomConfig /startupConfigurationFile=”..\\web.config”
```

Birden fazla geçiş yapılandırma sınıfınız varsa, DbMigrationConfiguration devralan sınıflar sonra bu yürütme için kullanılacak olan belirtmeniz gerekir. Bu, isteğe bağlı ikinci parametre bir anahtar olmadan sağlayarak belirtilir.

## <a name="provide-connection-string"></a>Bağlantı dizesini belirtin

``` console
Migrate.exe BlogDemo.dll /connectionString=”Data Source=localhost;Initial Catalog=BlogDemo;Integrated Security=SSPI” /connectionProviderName=”System.Data.SqlClient”
```

Ardından bir bağlantı dizesi komut satırında belirtmek istiyorsanız sağlayıcı adı da sağlamanız gerekir. Sağlayıcı adı belirtmeden bir özel durum neden olur.

## <a name="common-problems"></a>Yaygın sorunlar

| Hata İletisi                                                                                                                                                                                                                                                                                                                      | Çözüm                                                                                                                                                                                                                                                                                             |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| İşlenmeyen özel durum: System.IO.FileLoadException: dosya veya derleme yüklenemedi ' EntityFramework, sürüm 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 ' veya bağımlılıklarından biri. Bildirim tanımı bulunduğu derlemenin, derleme başvurusunu eşleşmiyor. (HRESULT özel durum: 0x80131040)         | Bu, genellikle Redirect.config dosya bir .NET 4 uygulaması çalıştıran anlamına gelir. Redirect.config migrate.exe aynı konuma kopyalayın ve migrate.exe.config için yeniden adlandırmanız gerekir.                                                                                       |
| İşlenmeyen özel durum: System.IO.FileLoadException: dosya veya derleme yüklenemedi ' EntityFramework, sürüm 4.4.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 ' veya bağımlılıklarından biri. Bildirim tanımı bulunduğu derlemenin, derleme başvurusunu eşleşmiyor. (HRESULT özel durum: 0x80131040)          | Bu özel durumun Redirect.config uygulamayla migrate.exe konuma kopyalanan bir .NET 4.5 çalıştırdığınız anlamına gelir. Ardından uygulamanız .NET 4.5 ise yeniden yönlendirmeleri içinde ile yapılandırma dosyası olması gerekmez. Migrate.exe.config dosyasını silin.                                    |
| Hata: Bekleyen değişiklikleri ve otomatik geçişi devre dışı olduğundan geçerli model eşleştirilecek veritabanı güncelleştirilemedi. Kod tabanlı bir geçiş için bekleyen model değişiklikleri yazma ya da otomatik geçişi etkinleştirin. İçin DbMigrationsConfiguration.AutomaticMigrationsEnabled etkinleştir otomatik geçiş için true olarak ayarlayın. | Çalışan bir geçiş modelinde yapılan değişikliklerle başa oluşturmadıysanız ve veritabanı modeli eşleşmiyor, geçiş işlemi gerçekleştirirseniz bu hata oluşur. Sonra veritabanını yükseltmek için bir geçiş oluşturmadan migrate.exe çalıştırarak bir model sınıfı için bir özellik ekleme, bu bir örnektir. |
| Hata: Tür için üye çözümlenemedi ' System.Data.Entity.Migrations.Design.ToolingFacade+UpdateRunner,EntityFramework, sürüm 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 '.                                                                                                                                       | Bu hata yanlış başlangıç dizini belirterek neden olabilir. Bu migrate.exe konumunu olmalıdır                                                                                                                                                                                      |
| İşlenmeyen özel durum: System.NullReferenceException: nesne başvurusu bir nesnenin örneğine ayarlanmadı. <br/>   (String [] args) System.Data.Entity.Migrations.Console.Program.Main                                                                                                                                             | Bu, kullanmakta olduğunuz bir senaryo için gerekli bir parametre belirtmeden kaynaklanabilir. Örneğin bir bağlantı dizesi sağlayıcı adı belirtmeden belirtme.                                                                                                                        |
| Hata: birden fazla geçişleri yapılandırma türü 'ClassLibrary1' derlemesinde bulunamadı. Kullanılacak adı belirtin.                                                                                                                                                                                                  | Hata durumları gibi belirtilen derlemede birden fazla yapılandırma sınıfı yoktur. Kullanılacağını belirtmek için /configurationType anahtarını kullanmanız gerekir.                                                                                                                                           |
| Hata: dosya veya derleme yüklenemedi '&lt;assemblyName&gt;' veya bağımlılıklarından biri. Verilen derleme adı veya kod temeli geçersizdi. (HRESULT özel durum: 0x80131047)                                                                                                                                                    | Bu yanlış bir derleme adı belirterek veya olmaması neden olabilir                                                                                                                                                                                                                          |
| Hata: dosya veya derleme yüklenemedi '&lt;assemblyName&gt;' veya bağımlılıklarından biri. Bir programı hatalı biçimde yüklemek için girişimde bulunuldu.                                                                                                                                                                          | X x64 karşı migrate.exe çalıştırmayı denediğiniz Bu, uygulama. EF 5.0 ve aşağıda x86 yalnızca iş olacaktır.                                                                                                                                                                                |
