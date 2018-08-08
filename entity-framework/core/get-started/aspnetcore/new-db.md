---
title: Üzerinde ASP.NET Core - yeni veritabanı - EF Core kullanmaya başlama
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 08/03/2018
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 9e86bc9cff028ad9791f23cbb45f0a93110c0064
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614356"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>EF çekirdekli ASP.NET Core üzerinde yeni bir veritabanı ile çalışmaya başlama

Bu öğreticide, Entity Framework Core kullanarak basit veri erişimi gerçekleştirdiği bir ASP.NET Core MVC uygulaması oluşturun. EF Core modelinizden veritabanı oluşturmaya geçişleri kullanın.

[Bu makaledeki örnek Github'da görüntüle](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yazılımları yükleyin:

* [Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) bu iş yükleri ile:
  * **ASP.NET ve web geliştirme** (altında **Web ve bulut**)
  * **.NET core çoklu platform geliştirme** (altında **diğer araç takımları**)
* [.NET core SDK'sını 2.1](https://www.microsoft.com/net/download/core).

## <a name="create-a-new-project-in-visual-studio-2017"></a>Visual Studio 2017'de yeni bir proje oluşturun

* Açık Visual Studio 2017
* **Dosya > Yeni > Proje**
* Sol menüden **yüklü > Visual C# > .NET Core**.
* Seçin **ASP.NET Core Web uygulaması**.
* Girin **EFGetStarted.AspNetCore.NewDb** tıklayın ve ad için **Tamam**.
* İçinde **yeni ASP.NET Core Web uygulaması** iletişim:
  * Seçenekleri sağlamak **.NET Core** ve **ASP.NET Core 2.1** açılan listeleri seçili
  * Seçin **Web uygulaması (Model-View-Controller)** proje şablonu
  * Emin **kimlik doğrulaması** ayarlanır **kimlik doğrulaması yok**
  * **Tamam**’a tıklayın.

Uyarı: kullanırsanız **bireysel kullanıcı hesapları** yerine **hiçbiri** için **kimlik doğrulaması** projenizdebirEntityFrameworkCoremodeliekleneceksonra`Models\IdentityModel.cs`. Bu öğreticide şunların tekniklerini kullanarak ikinci bir model eklemek veya var olan bu modeli, varlık sınıfları içeren genişletmek seçebilirsiniz.

## <a name="install-entity-framework-core"></a>Entity Framework Core yükleme

EF Core yüklemek için hedeflemek istediğiniz EF Core veritabanı sağlayıcı(lar) için paketi yükleyin. Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md). 

Bu öğreticide, öğretici, SQL Server kullandığından sağlayıcı paketi yüklemeniz gerekmez. SQL Server sağlayıcı paketi eklenmiştir [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

## <a name="create-the-model"></a>Model oluşturma

Bağlam sınıfı ve modelini yapmak bir varlık sınıfları tanımlayın:

* Sağ **modelleri** klasörü ve select **Ekle > sınıfı**.
* Girin **Model.cs** tıklayın ve adı olarak **Tamam**.
* Dosyanın içeriğini aşağıdaki kodla değiştirin:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

Gerçek bir uygulamada her sınıf genellikle ayrı bir dosyada modelinizdeki koyabilirsiniz. Basitleştirmek amacıyla, bu öğreticide tüm sınıflar tek dosyada koyar.

## <a name="register-your-context-with-dependency-injection"></a>Bağlamınızı bağımlılık ekleme ile kaydetme

Hizmetler (gibi `BloggingContext`) ile kaydedilen [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) uygulama başlatma sırasında. Bu hizmetler (örneğin, MVC denetleyicileri) gerektiren bileşenler sonra hizmetlerin Oluşturucu parametresi veya özellikleri aracılığıyla sağlanır.

Yapmak `BloggingContext` MVC denetleyicileri için kullanılabilir bir hizmet olarak kaydedin.

* Açık **Startup.cs**
* Aşağıdaki `using` ifadeleri:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Çağrı `AddDbContext` bağlamı bir hizmet olarak kaydetmek için yöntemi.

* Aşağıdaki vurgulanmış kodu ekleyin `ConfigureServices` yöntemi:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

Not: Gerçek bir uygulamada genellikle bir yapılandırma dosyası veya ortam değişkenine bağlantı dizesini koyabilirsiniz. Basitleştirmek amacıyla, bu öğreticide kod tanımlar. Bkz: [bağlantı dizeleri](../../miscellaneous/connection-strings.md) daha fazla bilgi için.

## <a name="create-the-database"></a>Veritabanı oluşturma

Bir model açtıktan sonra kullanabileceğiniz [geçişler](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) bir veritabanı oluşturmak için.

* **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**
* Çalıştırma `Add-Migration InitialCreate` tablolar, modelinize için başlangıç kümesi oluşturmak için bir geçiş iskele. Belirten bir hata alırsanız `The term 'add-migration' is not recognized as the name of a cmdlet`kapatın ve Visual Studio'yu yeniden açın.
* Çalıştırma `Update-Database` veritabanına yeni geçiş uygulamak için. Bu komut, veritabanı geçişleri uygulamadan önce oluşturur.

## <a name="create-a-controller"></a>Bir denetleyici oluşturma

Bir denetleyici ve görünüm için iskele `Blog` varlık.

* Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > denetleyicisi**.
* Seçin **MVC denetleyici Entity Framework kullanarak görünümler ile** tıklatıp **Ekle**.
* Ayarlama **Model sınıfı** için **Blog** ve **veri bağlamı sınıfının** için **BloggingContext**.
* **Ekle**'yi tıklatın.


## <a name="run-the-application"></a>Uygulamayı çalıştırın

Uygulamayı test etmek ve çalıştırmak için F5 tuşuna basın.

* Gidin `/Blogs`
* Bazı blog girişleri oluşturmak için Oluştur bağlantısını kullanın. Test ayrıntıları ve bağlantılarını silin.

![görüntü](_static/create.png)

![görüntü](_static/index-new-db.png)

## <a name="additional-resources"></a>Ek Kaynaklar

* [EF - yeni veritabanı SQLite ile](xref:core/get-started/netcore/new-db-sqlite) -platformlar arası konsol EF öğretici.
* [MVC Mac veya Linux'ta ASP.NET Core'a giriş](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Visual Studio ile MVC ASP.NET Core'a giriş](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Visual Studio kullanarak ASP.NET Core ve Entity Framework Core ile çalışmaya başlama](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
