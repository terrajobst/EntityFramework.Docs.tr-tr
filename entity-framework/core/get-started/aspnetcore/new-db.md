---
title: Üzerinde ASP.NET Core - yeni veritabanı - EF Core kullanmaya başlama
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 878478099878e4a0bc65c44fef0609d28f39f2b8
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834779"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>EF çekirdekli ASP.NET Core üzerinde yeni bir veritabanı ile çalışmaya başlama

Bu öğreticide, Entity Framework Core kullanarak basit veri erişimi gerçekleştirdiği bir ASP.NET Core MVC uygulaması oluşturun. Öğreticide geçişleri veri modeli veritabanını oluşturmak için kullanılır.

Windows üzerinde Visual Studio 2017 kullanılarak veya Windows, macOS veya Linux üzerinde .NET Core CLI kullanarak öğreticiyi izleyebilirsiniz.

Bu makaledeki örnek Github'da görüntüleyin:
* [SQL Server ile Visual Studio 2017](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* [.NET core CLI SQLite ile](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yazılımları yükleyin:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 sürüm 15.7 veya üzeri](https://www.visualstudio.com/downloads/) bu iş yükleri ile:
  * **ASP.NET ve web geliştirme** (altında **Web ve bulut**)
  * **.NET core çoklu platform geliştirme** (altında **diğer araç takımları**)
* [.NET core SDK'sını 2.1](https://www.microsoft.com/net/download/core).

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* [.NET core SDK'sını 2.1](https://www.microsoft.com/net/download/core).

---

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Açık Visual Studio 2017
* **Dosya > Yeni > Proje**
* Sol menüden **yüklü > Visual C# > .NET Core**.
* Seçin **ASP.NET Core Web uygulaması**.
* Girin **EFGetStarted.AspNetCore.NewDb** tıklayın ve ad için **Tamam**.
* İçinde **yeni ASP.NET Core Web uygulaması** iletişim:
  * Emin olun **.NET Core** ve **ASP.NET Core 2.1** seçildiğinden açılan listeler
  * Seçin **Web uygulaması (Model-View-Controller)** proje şablonu
  * Emin olun **kimlik doğrulaması** ayarlanır **kimlik doğrulaması yok**
  * **Tamam**’a tıklayın.

Uyarı: kullanırsanız **bireysel kullanıcı hesapları** yerine **hiçbiri** için **kimlik doğrulaması** projedeEntityFrameworkCoremodeliekleneceksonra`Models\IdentityModel.cs`. Bu öğreticide şunların tekniklerini kullanarak ikinci bir model eklemek veya var olan bu modeli, varlık sınıfları içeren genişletmek seçebilirsiniz.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* Bir MVC projesi oluşturmak için aşağıdaki komutu çalıştırın:

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* Proje dizinine değiştirin. Yeni projeyi karşı çalıştırmak girdiğiniz komutları gerekir.

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a>Entity Framework Core yükleme

EF Core yüklemek için hedeflemek istediğiniz EF Core veritabanı sağlayıcı(lar) için paketi yükleyin. Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Bu öğreticide, öğretici, SQL Server kullandığından sağlayıcı paketi yüklemeniz gerekmez. SQL Server sağlayıcı paketi eklenmiştir [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Bu öğreticide, .NET Core destekleyen tüm platformlarda çalıştığı için SQLite kullanılır.

* SQLite sağlayıcısını yüklemek için aşağıdaki komutu çalıştırın:

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a>Model oluşturma

Bağlam sınıfı ve modelini olun varlık sınıfı tanımlar.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Sağ **modelleri** klasörü ve select **Ekle > sınıfı**.
* Girin **Model.cs** tıklayın ve adı olarak **Tamam**.
* Dosyanın içeriğini aşağıdaki kodla değiştirin:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* İçinde **modelleri** klasör oluşturma **Model.cs** aşağıdaki kod ile:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

Bir üretim uygulaması, genellikle ayrı bir dosyada her sınıf koyabilirsiniz. Basitleştirmek amacıyla, bu Öğreticide bu sınıfların tek bir dosyaya koyar.

## <a name="register-the-context-with-dependency-injection"></a>Bağımlılık ekleme bağlam kaydı

Hizmetler (gibi `BloggingContext`) ile kaydedilen [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) uygulama başlatma sırasında. Bu hizmetler (örneğin, MVC denetleyicileri) gerektiren bileşenler bu hizmetleri Oluşturucu parametresi veya özellikleri aracılığıyla sağlanır.

Yapmak `BloggingContext` MVC denetleyicileri için kullanılabilir bir hizmet olarak kaydedin.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* İçinde **Startup.cs** aşağıdaki `using` ifadeleri:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* Aşağıdaki vurgulanmış kodu ekleyin `ConfigureServices` yöntemi:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* İçinde **Startup.cs** aşağıdaki `using` ifadeleri:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* Aşağıdaki vurgulanmış kodu ekleyin `ConfigureServices` yöntemi:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

Bir üretim uygulaması, genellikle bir yapılandırma dosyası veya ortam değişkenine bağlantı dizesini koyabilirsiniz. Basitleştirmek amacıyla, bu öğreticide kod tanımlar. Bkz: [bağlantı dizeleri](../../miscellaneous/connection-strings.md) daha fazla bilgi için.

## <a name="create-the-database"></a>Veritabanı oluşturma

Aşağıdaki adımları kullanın [geçişler](xref:core/managing-schemas/migrations/index) bir veritabanı oluşturmak için.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**
* Aşağıdaki komutları çalıştırın:

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  Belirten bir hata alırsanız `The term 'add-migration' is not recognized as the name of a cmdlet`kapatın ve Visual Studio'yu yeniden açın.

  `Add-Migration` Komut iskele oluşturulduğunu tablo modeli için başlangıç kümesi oluşturmak için bir geçiş. `Update-Database` Komutu veritabanı oluşturur ve yeni bir geçiş uygular.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* Aşağıdaki komutları çalıştırın:

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  `migrations` Komut iskele oluşturulduğunu tablo modeli için başlangıç kümesi oluşturmak için bir geçiş. `database update` Komutu veritabanı oluşturur ve yeni bir geçiş uygular.

---

## <a name="create-a-controller"></a>Bir denetleyici oluşturma

Bir denetleyici ve görünüm için iskele `Blog` varlık.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > denetleyicisi**.
* Seçin **MVC denetleyici Entity Framework kullanarak görünümler ile** tıklatıp **Ekle**.
* Ayarlama **Model sınıfı** için **Blog** ve **veri bağlamı sınıfının** için **BloggingContext**.
* **Ekle**'yi tıklatın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* Aşağıdaki komutları çalıştırın:

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  `tool install` Ve `add package` komutları denetleyicileri ve görünümleri iskelesini oluşturabilirsiniz araçları yükleyin. `restore` Komut, tüm projenin paketlerin yüklenmesi, sağlar ve `aspnet-codegenerator` komutu iskele yapar.
---

Yapı iskelesi altyapısı aşağıdaki dosyaları oluşturur:

* Bir denetleyici (*Controllers/BlogsController.cs*)
* Oluşturma, silme, Ayrıntılar, düzenleme ve dizin sayfaları için Razor görünümleri (_Views/Movies/*.cshtml_)

## <a name="run-the-application"></a>Uygulamayı çalıştırın

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Hata ayıklama** > **hata ayıklama olmadan Başlat**

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet run
```
---

* Gidin `/Blogs`

* Kullanım **Yeni Oluştur** bağlantı bazı blog girişleri oluşturulamıyor.

  ![sayfası oluşturma](_static/create.png)

* Test **ayrıntıları**, **Düzenle**, ve **Sil** bağlantıları.

  ![Dizin Sayfası](_static/index-new-db.png)

## <a name="additional-resources"></a>Ek Kaynaklar

* [Öğretici: .NET core'da EF Core ile SQLite kullanarak yeni bir veritabanı ile çalışmaya başlama](xref:core/get-started/netcore/new-db-sqlite)
* [Öğretici: ASP.NET Core Razor sayfaları kullanmaya başlayın](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [Öğretici: ASP.NET Core, Entity Framework Core ile Razor sayfaları](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
