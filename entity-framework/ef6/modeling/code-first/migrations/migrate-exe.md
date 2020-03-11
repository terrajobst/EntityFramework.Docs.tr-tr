---
title: Migrate. exe-EF6 kullanma
author: divega
ms.date: 10/23/2016
ms.assetid: 989ea862-e936-4c85-926a-8cfbef5df5b8
ms.openlocfilehash: 34866ddbbf81f090a064af148a612dd354ae9401
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418961"
---
# <a name="using-migrateexe"></a>Migrate. exe kullanma
Code First Migrations, Visual Studio 'nun içinden bir veritabanını güncelleştirmek için kullanılabilir, ancak komut satırı aracı migrate. exe aracılığıyla da çalıştırılabilir. Bu sayfa, bir veritabanına karşı geçişleri yürütmek için migrate. exe ' yi kullanma hakkında hızlı bir genel bakış sunar.

> [!NOTE]
> Bu makalede temel senaryolarda Code First Migrations kullanmayı bildiğiniz varsayılmaktadır. Bunu yapmazsanız devam etmeden önce [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) okumanız gerekir.

## <a name="copy-migrateexe"></a>Copy migrate. exe

Entity Framework yüklediğinizde, NuGet migrate. exe ' yi kullanarak karşıdan yüklenen paketin araçlar klasörünün içinde olur. &lt;proje klasörü&gt;\\paketleri EntityFramework '\\.&lt;sürüm&gt;\\araçları

. Exe dosyasını geçirdikten sonra, geçişlerinizi içeren derlemenin konumuna kopyalamanız gerekir.

Uygulamanız 4,5 değil .NET 4 ' i hedefliyorsa, **yeniden yönlendirme. config** dosyasını da konuma kopyalamanız ve bunu **migrate. exe. config**olarak yeniden adlandırmanız gerekir. Bu, migrate. exe ' nin Entity Framework derlemesini bulabilmek için doğru bağlama yeniden yönlendirmelerini almasını sağlar.

| .NET 4.5                                      | .NET 4,0                                      |
|:----------------------------------------------|:----------------------------------------------|
| ![.NET 4,5 dosyaları](~/ef6/media/net45files.png) | ![.NET 4,0 dosyaları](~/ef6/media/net40files.png) |

> [!NOTE]
> migrate. exe x64 derlemelerini desteklemez.

Migrate. exe dosyasını doğru klasöre taşıdıktan sonra veritabanına karşı geçişleri yürütmek için onu kullanabilirsiniz. Tüm yardımcı programları, geçişleri yürütmek için tasarlanmıştır. Geçiş oluşturamaz veya bir SQL betiği oluşturamaz.

## <a name="see-options"></a>Bkz. Seçenekler

``` console
Migrate.exe /?
```

Yukarıdaki işlem, bu yardımcı program ile ilişkili Yardım sayfasını görüntüler, bu işlemin çalışması için, ' ın migrate. exe ' yi çalıştırmakta olduğunuz konumda EntityFramework. dll ' ye sahip olmanız gerektiğini unutmayın.

## <a name="migrate-to-the-latest-migration"></a>En son geçişe geçiş

``` console
Migrate.exe MyMvcApplication.dll /startupConfigurationFile="..\\web.config"
```

Migrate. exe ' yi çalıştırırken, çalıştırmayı denediğiniz geçişleri içeren derleme olan tek zorunlu parametre, ancak yapılandırma dosyasını belirtmezseniz tüm kural tabanlı ayarları kullanacaktır.

## <a name="migrate-to-a-specific-migration"></a>Belirli bir geçişe geçiş

``` console
Migrate.exe MyApp.exe /startupConfigurationFile="MyApp.exe.config" /targetMigration="AddTitle"
```

Geçişleri belirli bir geçişe çalıştırmak istiyorsanız, geçişin adını belirtebilirsiniz. Bu, belirtilen geçişe gelene kadar tüm önceki geçişleri gereken şekilde çalıştırır.

## <a name="specify-working-directory"></a>Çalışma dizinini belirtin

``` console
Migrate.exe MyApp.exe /startupConfigurationFile="MyApp.exe.config" /startupDirectory="c:\\MyApp"
```

Derleme bağımlılıkları varsa veya çalışma diziniyle ilişkili dosyaları okuyorsa, startupDirectory 'yi ayarlamanız gerekir.

## <a name="specify-migration-configuration-to-use"></a>Kullanılacak geçiş yapılandırmasını belirtin

``` console
Migrate.exe MyAssembly CustomConfig /startupConfigurationFile="..\\web.config"
```

Birden çok geçiş yapılandırma sınıfınız varsa, DbMigrationConfiguration 'dan devralan sınıflar varsa, bu yürütme için kullanılacak olan sınıfları belirtmeniz gerekir. Bu, yukarıdaki gibi anahtar olmadan isteğe bağlı ikinci parametre sağlanarak belirtilir.

## <a name="provide-connection-string"></a>Bağlantı dizesi sağla

``` console
Migrate.exe BlogDemo.dll /connectionString="Data Source=localhost;Initial Catalog=BlogDemo;Integrated Security=SSPI" /connectionProviderName="System.Data.SqlClient"
```

Komut satırında bir bağlantı dizesi belirtmek isterseniz, sağlayıcı adını da belirtmeniz gerekir. Sağlayıcı adının belirtilmemesine özel bir durum neden olur.

## <a name="common-problems"></a>Yaygın sorunlar

| Hata İletisi                                                                                                                                                                                                                                                                                                                      | Çözüm                                                                                                                                                                                                                                                                                             |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| İşlenmeyen özel durum: System. ıO. FileLoadException: dosya veya derleme ' EntityFramework, Version = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 ' veya bağımlılıklarından biri yüklenemedi. Konumlandırılan derlemenin bildirim tanımı derleme başvurusuyla eşleşmiyor. (HRESULT özel durumu: 0x80131040)         | Bu genellikle, Redirect. config dosyası olmadan bir .NET 4 uygulaması çalıştırdığınız anlamına gelir. Redirect. config dosyasını migrate. exe ile aynı konuma kopyalamanız ve. exe. config dosyasını geçirmek için yeniden adlandırmanız gerekir.                                                                                       |
| İşlenmeyen özel durum: System. ıO. FileLoadException: dosya veya derleme ' EntityFramework, Version = 4.4.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 ' veya bağımlılıklarından biri yüklenemedi. Konumlandırılan derlemenin bildirim tanımı derleme başvurusuyla eşleşmiyor. (HRESULT özel durumu: 0x80131040)          | Bu özel durum, migrate. exe konumuna kopyaladığınız Redirect. config dosyasına sahip bir .NET 4,5 uygulaması çalıştırdığınız anlamına gelir. Uygulamanız .NET 4,5 ise, içindeki yeniden yönlendirmeleri yapılandırma dosyasına sahip olmanız gerekmez. Migrate. exe. config dosyasını silin.                                    |
| Hata: bekleyen değişiklikler olduğu ve otomatik geçiş devre dışı olduğu için veritabanı geçerli modelle eşleşecek şekilde güncelleştirilemiyor. Bekleyen model değişikliklerini kod tabanlı bir geçişe yazın ya da otomatik geçişi etkinleştirin. Otomatik geçişi etkinleştirmek için DbMigrationsConfiguration. AutomaticMigrationsEnabled değerini true olarak ayarlayın. | Bu hata, modelde yapılan değişikliklerle buluta bir geçiş oluşturmadıysanız ve veritabanı modeliyle eşleşmediğinden geçiş işlemi çalıştırıldığında oluşur. Bir model sınıfına özellik eklemek ve ardından veritabanını yükseltmek için geçiş oluşturmadan migrate. exe ' yi çalıştırmak bunun bir örneğidir. |
| Hata: ' System. Data. Entity. geçişler. Design. Toolingfaon + UpdateRunner, EntityFramework, Version = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 ' üyesi için tür çözümlenemedi.                                                                                                                                       | Hatalı bir başlangıç dizini belirtilerek bu hataya neden olmuş olabilir. Bu, migrate. exe ' nin konumu olmalıdır                                                                                                                                                                                      |
| İşlenmeyen özel durum: System. NullReferenceException: nesne başvurusu bir nesnenin örneğine ayarlanmadı. <br/>   System. Data. Entity. geçişler. Console. program. Main (String [] args) konumunda                                                                                                                                             | Bunun nedeni, kullanmakta olduğunuz bir senaryo için gerekli bir parametre belirtmemelidir. Örneğin, sağlayıcı adını belirtmeden bir bağlantı dizesi belirtin.                                                                                                                        |
| Hata: ' ClassLibrary1 ' derlemesinde birden fazla geçiş yapılandırması türü bulundu. Kullanılacak birinin adını belirtin.                                                                                                                                                                                                  | Hata durumunda, verilen derlemede birden fazla yapılandırma sınıfı vardır. Hangisinin kullanılacağını belirtmek için/configurationType anahtarını kullanmanız gerekir.                                                                                                                                           |
| Hata: '&lt;assemblyName&gt;' dosyası veya bütünleştirilmiş kodu ya da bağımlılıklarından biri yüklenemedi. Verilen derleme adı veya kod temeli geçersizdi. (HRESULT özel durumu: 0x80131047)                                                                                                                                                    | Bunun nedeni, derleme adı yanlış belirtilerek veya olmayan                                                                                                                                                                                                                          |
| Hata: '&lt;assemblyName&gt;' dosyası veya bütünleştirilmiş kodu ya da bağımlılıklarından biri yüklenemedi. Bir programı hatalı biçimde yükleme girişiminde bulunuldu.                                                                                                                                                                          | Bu, migrate. exe ' yi bir x64 uygulamasına karşı çalıştırmaya çalışıyorsanız meydana gelir. EF 5,0 ve aşağıdaki, yalnızca x86 üzerinde çalışır.                                                                                                                                                                                |
