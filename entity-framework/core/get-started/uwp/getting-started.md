---
title: UWP - yeni veritabanı - EF Core üzerinde çalışmaya başlama
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 456967a0dc981053919064a19cc9c98bf7309865
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022382"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Yeni bir veritabanı ile Evrensel Windows Platformu (UWP) üzerinde EF Core ile çalışmaya başlama

Bu öğreticide, Entity Framework Core kullanan yerel bir SQLite veritabanından karşı temel veri erişimi gerçekleştirdiği bir evrensel Windows Platformu (UWP) uygulaması oluşturun.

[Bu makaledeki örnek Github'da görüntüle](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Önkoşullar

* [Windows 10 Fall Creators Update (10.0; 16299 yapı) veya üzeri](https://support.microsoft.com/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 sürüm 15.7 veya üzeri](https://www.visualstudio.com/downloads/) ile **Evrensel Windows platformu geliştirme** iş yükü.

* [.NET core 2.1 SDK veya üzeri](https://www.microsoft.com/net/core) veya üzeri.

> [!IMPORTANT]
> Bu öğreticide, Entity Framework Core kullanan [geçişler](xref:core/managing-schemas/migrations/index) oluşturmak ve veritabanı şeması güncelleştirmek için komutları.
> Bu komutlar, doğrudan UWP projeleri ile çalışmaz.
> Bu nedenle, uygulamanın veri modelini bir paylaşılan kitaplığı projesinde yerleştirilir ve ayrı bir .NET Core konsol uygulaması komutları çalıştırmak için kullanılır.

## <a name="create-a-library-project-to-hold-the-data-model"></a>Veri modelinde tutmak için bir kitaplık projesi oluşturun

* Visual Studio'yu Aç

* **Dosya > Yeni > Proje**

* Sol menüden **yüklü > Visual C# > .NET Standard**.

* Seçin **sınıf kitaplığı (.NET Standard)** şablonu.

* Projeyi adlandırın *Blogging.Model*.

* Çözüm adı *blog*.

* **Tamam**'ı tıklatın.

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a>Veri modeli projede Entity Framework Core runtime'ı yükleyin

EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin. Bu öğreticide SQLite kullanılır. Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).

* **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.

* Emin olun kitaplık projesi *Blogging.Model* olarak seçilen **varsayılan proje** Paket Yöneticisi konsolunda.

* `Install-Package Microsoft.EntityFrameworkCore.Sqlite`'i çalıştırın.

## <a name="create-the-data-model"></a>Veri modeli oluşturma

Tanımlama artık *DbContext* ve modelini yapmak bir varlık sınıfları.

* Silme *Class1.cs*.

* Oluşturma *Model.cs* aşağıdaki kod ile:

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a>Geçişleri komutlarını çalıştırmak için yeni bir konsol projesi oluşturma

* İçinde **Çözüm Gezgini**, çözüme sağ tıklayın ve ardından **Ekle > Yeni proje**.

* Sol menüden **yüklü > Visual C# > .NET Core**.

* Seçin **konsol uygulaması (.NET Core)** proje şablonu.

* Projeyi adlandırın *Blogging.Migrations.Startup*, tıklatıp **Tamam**.

* Proje Başvuru Ekle *Blogging.Migrations.Startup* için proje *Blogging.Model* proje.

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a>Geçişleri başlangıç projede Entity Framework Core Araçları'nı yükleyin

Paket Yöneticisi konsolu EF Core geçiş komutları etkinleştirmek için konsol uygulamasında EF Core araçları paketini yükleyin.

* **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**

* `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`'i çalıştırın.

## <a name="create-the-initial-migration"></a>İlk geçiş oluştur

 Konsol uygulaması başlangıç projesi olarak belirterek ilk geçiş oluşturun.

* `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`'i çalıştırın.

Bu komut, ilk veritabanı tabloları veri modeliniz için kümesini oluşturan bir geçiş iskele oluşturulduğunu.

## <a name="create-the-uwp-project"></a>UWP projesi oluşturma

* İçinde **Çözüm Gezgini**, çözüme sağ tıklayın ve ardından **Ekle > Yeni proje**.

* Sol menüden **yüklü > Visual C# > Windows Evrensel**.

* Seçin **boş uygulama (Evrensel Windows)** proje şablonu.

* Projeyi adlandırın *Blogging.UWP*, tıklatıp **Tamam**

> [!IMPORTANT]
> En az bir hedef ve en düşük sürüm kümesine **Windows 10 Fall Creators Update (10.0; derleme 16299.0)**.
> Entity Framework Core tarafından gerekli olan .NET Standard 2.0, Windows 10 'un önceki sürümlerini desteklemez.

## <a name="add-code-to-create-the-database-on-application-startup"></a>Uygulama başlangıcında veritabanını oluşturmak için kod ekleyin

Uygulamanın üzerinde çalıştığı cihazda oluşturulacak veritabanının istediğinden, uygulama başlangıcından yerel veritabanında bekleyen tüm geçişler uygulamak için kod ekleyin. Uygulama çalışır, ilk kez bu yerel veritabanı oluşturmayı ilgileniriz.

* Proje Başvuru Ekle *Blogging.UWP* için proje *Blogging.Model* proje.

* Açık *App.xaml.cs*.

* Bekleyen tüm geçişler uygulamak için vurgulanmış kodu ekleyin.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> Modelinizi değiştirmek kullanırsanız `Add-Migration` karşılık gelen uygulamak için yeni bir geçiş iskele komut, veritabanına değiştirir. Uygulama başladığında bekleyen tüm geçişlerde, her cihazda yerel veritabanına uygulanır.
>
>EF Core kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulanmış izlemek için veritabanı tablosunda.

## <a name="use-the-data-model"></a>Veri modelini kullanın

EF Core, veri erişimi gerçekleştirdiği artık kullanabilirsiniz.

* Açık *MainPage.xaml*.

* UI içerik aşağıda belirtildiği ve sayfa yükleme işleyicisi ekleyin

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

Şimdi veritabanı ile kullanıcı arabirimini'kurmak wire için kod ekleyin

* Açık *MainPage.xaml.cs*.

* Aşağıdaki listeden vurgulanmış kodu ekleyin:

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

Şimdi nasıl çalıştığını görmek için uygulamayı çalıştırabilirsiniz.

* İçinde **Çözüm Gezgini**, sağ *Blogging.UWP* proje ve ardından **Dağıt**.

* Ayarlama *Blogging.UWP* başlangıç projesi olarak.

* **Hata ayıklama > hata ayıklama olmadan Başlat**

  Uygulamayı derler ve çalıştırır.

* Bir URL girin ve tıklayın **Ekle** düğmesi

  ![görüntü](_static/create.png)

  ![görüntü](_static/list.png)

  Tada! Entity Framework Core çalıştıran basit bir UWP uygulamasında artık var.

## <a name="next-steps"></a>Sonraki adımlar

EF Core ile UWP kullanırken bilmeniz gereken uyumluluk ve performans için bilgi [EF Core tarafından desteklenen .NET uygulamalarıyla](../../platforms/index.md#universal-windows-platform).

Entity Framework Core özellikler hakkında daha fazla bilgi edinmek için bu belgeleri içindeki diğer makalelere göz atın.
