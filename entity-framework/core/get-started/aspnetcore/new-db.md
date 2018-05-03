---
title: ASP.NET Core - yeni veritabanı - EF Çekirdeğinde Başlarken
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 80477ca57b8b3df6de8ba3595c9056c6b8412040
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>ASP.NET Core EF Çekirdeğinde ile yeni bir veritabanı ile çalışmaya başlama

Bu kılavuzda, Entity Framework Çekirdek kullanarak temel veri erişimi gerçekleştirdiği bir ASP.NET Core MVC uygulaması oluşturacaksınız. EF çekirdek modelden veritabanı oluşturmak için geçişleri kullanır. Bkz: [ek kaynaklar](#additional-resources) daha fazla Entity Framework Çekirdek öğreticileri.

Bu öğretici gerektirir:
* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) bu iş yükleri ile:
  * **ASP.NET ve web geliştirme** (altında **Web ve bulut**)
  * **.NET core platformlar arası geliştirme** (altında **diğer Toolsets**)
* [.NET 2.0 SDK çekirdek](https://www.microsoft.com/net/download/core).

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) github'da.

## <a name="create-a-new-project-in-visual-studio-2017"></a>Visual Studio 2017 içinde yeni bir proje oluşturun

* **Dosya > Yeni > Proje**
* Sol menüden seçin **yüklü > şablonları > Visual C# > .NET Core**.
* Seçin **ASP.NET Core Web uygulaması**.
* Girin **EFGetStarted.AspNetCore.NewDb** tıklayın ve ad için **Tamam**.
* İçinde **çekirdek yeni bir ASP.NET Web uygulaması** iletişim:
  * Seçenekleri olun **.NET Core** ve **ASP.NET Core 2.0** açılan listeleri seçilir
  * Seçin **Web uygulaması (Model-View-Controller)** proje şablonu
  * Emin **kimlik doğrulaması** ayarlanır **doğrulaması yok**
  * **Tamam**’a tıklayın.

Uyarı: kullanırsanız **tek tek kullanıcı hesaplarını** yerine **hiçbiri** için **kimlik doğrulaması** bir Entity Framework Çekirdek modeli projenizdeeklenirsonra`Models\IdentityModel.cs`. Bu kılavuzda öğreneceksiniz teknikleri kullanarak, ikinci bir model ekleyin ya da varlık sınıflarınızı içerecek şekilde bu varolan modelini seçebilirsiniz.

## <a name="install-entity-framework-core"></a>Entity Framework Çekirdek yükleyin

Hedeflemek istediğiniz EF çekirdek veritabanı sağlayıcı(lar) için paketini yükleyin. Bu kılavuz, SQL Server kullanır. Kullanılabilir sağlayıcılar listesi için bkz: [veritabanı sağlayıcıları](../../providers/index.md).

* **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**

* `Install-Package Microsoft.EntityFrameworkCore.SqlServer`'i çalıştırın.

EF çekirdek modelinizin dışında bir veritabanı oluşturmak için size bazı Entity Framework Çekirdek araçları kullanarak. Böylece biz de araçları paketini yükleyecek:

* `Install-Package Microsoft.EntityFrameworkCore.Tools`'i çalıştırın.

Daha sonra denetleyicileri ve görünümleri oluşturmak için size bazı ASP.NET Core İskele araçlarını kullanma. Böylece biz de bu tasarım paket yükleyecek:

* `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`'i çalıştırın.

## <a name="create-the-model"></a>Model oluşturma

Modeli olun bağlamını ve varlık sınıflarını tanımlayın:

* Sağ **modelleri** klasörü ve select **Ekle > sınıfı**.
* Girin **Model.cs** tıklatın ve adı olarak **Tamam**.
* Dosyasının içeriğini aşağıdaki kodla değiştirin:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

Not: Gerçek bir uygulamada, genellikle her sınıf ayrı bir dosyada, modelden koyabilirsiniz. Basitleştirmek amacıyla, biz tüm sınıflar bir dosyada Bu öğretici için koyduğunuz.

## <a name="register-your-context-with-dependency-injection"></a>İçeriğiniz bağımlılık ekleme ile kaydetme

Hizmetler (gibi `BloggingContext`) ile kayıtlı [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) uygulama başlatma sırasında. Bu Hizmetleri (örneğin, MVC denetleyicileri) gerektiren bileşenler daha sonra bu hizmetlere Oluşturucu parametreleri veya özellikleri yoluyla sağlanır.

Bizim MVC denetleyicileri yapmak için sırayla kullanımı `BloggingContext` sizi bir hizmet olarak kaydeder.

* Açık **haline**
* Aşağıdakileri ekleyin `using` deyimleri:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Ekleme `AddDbContext` yöntemi bir hizmet olarak kaydetmek için:

* Aşağıdaki kodu ekleyin `ConfigureServices` yöntemi:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

Not: Gerçek bir uygulama yapılandırma dosyasında bağlantı dizesini genellikle koyabilirsiniz. Basitleştirmek amacıyla, biz bu kodda tanımlıyorsanız. Bkz: [bağlantı dizeleri](../../miscellaneous/connection-strings.md) daha fazla bilgi için.

## <a name="create-your-database"></a>Veritabanı oluşturma

Bir modeli eğittikten sonra kullanabileceğiniz [geçişler](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) bir veritabanı oluşturmak için.

* PMC açın:

  **Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu**
* Çalıştırma `Add-Migration InitialCreate` tabloları modeliniz için ilk kümesi oluşturmak için bir geçiş için iskele kurmak. Belirten bir hata alırsanız `The term 'add-migration' is not recognized as the name of a cmdlet`kapatın ve Visual Studio'yu yeniden açın.
* Çalıştırma `Update-Database` veritabanına yeni geçiş uygulanacak. Bu komut, geçişler uygulamadan önce veritabanı oluşturur.

## <a name="create-a-controller"></a>Bir denetleyici oluşturma

Yapı iskelesi projesinde etkinleştirin:

* Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > denetleyicisi**.
* Seçin **en az bağımlılıkları** tıklatıp **Ekle**.
* Yoksay veya silme *ScaffoldingReadMe.txt* dosya.

Askılama özelliğinin etkinleştirildiğinden, bir denetleyici için iskele `Blog` varlık.

* Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle > denetleyicisi**.
* Seçin **görünümleri ile MVC Entity Framework kullanarak denetleyicisi** tıklatıp **Tamam**.
* Ayarlama **Model sınıfı** için **Blog** ve **veri bağlamı sınıfı** için **BloggingContext**.
* **Ekle**'yi tıklatın.


## <a name="run-the-application"></a>Uygulamayı çalıştırın

Ve uygulamayı test çalıştırmak için F5 tuşuna basın.

* Gidin `/Blogs`
* Bazı Web günlüğü girişleri oluşturmak için Oluştur bağlantısını kullanın. Sınama ayrıntıları ve bağlantıları silin.

![görüntü](_static/create.png)

![görüntü](_static/index-new-db.png)

## <a name="additional-resources"></a>Ek Kaynaklar

* [EF - yeni veritabanı SQLite ile](xref:core/get-started/netcore/new-db-sqlite) -platformlar arası konsol EF öğretici.
* [ASP.NET Core Mac veya Linux MVC giriş](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [ASP.NET Core Visual Studio ile MVC giriş](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Visual Studio kullanarak ASP.NET Core ve Entity Framework Core ile çalışmaya başlama](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
