---
title: UWP - yeni veritabanı - EF Core üzerinde çalışmaya başlama
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996916"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Yeni bir veritabanı ile Evrensel Windows Platformu (UWP) üzerinde EF Core ile çalışmaya başlama

Bu öğreticide, Entity Framework Core kullanan yerel bir SQLite veritabanından karşı temel veri erişimi gerçekleştirdiği bir evrensel Windows Platformu (UWP) uygulaması oluşturun.

[Bu makaledeki örnek Github'da görüntüle](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Önkoşullar

* [Windows 10 Fall Creators Update (10.0; 16299 yapı) veya üzeri](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 sürüm 15.7 veya üzeri](https://www.visualstudio.com/downloads/) ile **Evrensel Windows platformu geliştirme** iş yükü.

* [.NET core 2.1 SDK veya üzeri](https://www.microsoft.com/net/core) veya üzeri.

## <a name="create-a-model-project"></a>Bir model projesi oluşturma

> [!IMPORTANT]
> Şekilde sınırlamaları nedeniyle modeli geçişleri komutları çalıştırmak için bir UWP olmayan proje yerleştirilmesi gerekir UWP projeleri .NET Core araçları etkileşim **Paket Yöneticisi Konsolu** (PMC)

* Visual Studio'yu Aç

* **Dosya > Yeni > Proje**

* Sol menüden **yüklü > Visual C# > .NET Standard**.

* Seçin **sınıf kitaplığı (.NET Standard)** şablonu.

* Projeyi adlandırın *Blogging.Model*.

* Çözüm adı *blog*.

* **Tamam**'ı tıklatın.

## <a name="install-entity-framework-core"></a>Entity Framework Core yükleme

EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin. Bu öğreticide SQLite kullanılır. Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).

* **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.

* `Install-Package Microsoft.EntityFrameworkCore.Sqlite`'i çalıştırın.

Veritabanını korumak için bu öğreticinin ilerleyen bölümlerinde bazı Entity Framework Core araçları kullanacaksınız. Bu nedenle de araçları paketini yükleyin.

* `Install-Package Microsoft.EntityFrameworkCore.Tools`'i çalıştırın.

## <a name="create-the-model"></a>Model oluşturma

Artık modeli oluşturan bir bağlam ve varlık sınıflarını tanımlamak için zamanı geldi.

* Silme *Class1.cs*.

* Oluşturma *Model.cs* aşağıdaki kod ile:

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Yeni bir UWP projesi oluşturma

* İçinde **Çözüm Gezgini**, çözüme sağ tıklayın ve ardından **Ekle > Yeni proje**.

* Sol menüden **yüklü > Visual C# > Windows Evrensel**.

* Seçin **boş uygulama (Evrensel Windows)** proje şablonu.

* Projeyi adlandırın *Blogging.UWP*, tıklatıp **Tamam**

* En az bir hedef ve en düşük sürüm kümesine **Windows 10 Fall Creators Update (10.0; derleme 16299.0)**.

## <a name="create-the-initial-migration"></a>İlk geçiş oluştur

Bir modeliniz olduğuna göre ilk çalıştığında bir veritabanı oluşturmak için uygulamasını ayarlama. Bu bölümde, ilk geçiş oluşturun. Aşağıdaki bölümde, uygulama başlatıldığında, bu geçiş uygulayan kodu ekleyin.

Geçiş Araçları, UWP başlangıç projesi gerektirir, bu nedenle öncelikle oluşturun.

* İçinde **Çözüm Gezgini**, çözüme sağ tıklayın ve ardından **Ekle > Yeni proje**.

* Sol menüden **yüklü > Visual C# > .NET Core**.

* Seçin **konsol uygulaması (.NET Core)** proje şablonu.

* Projeyi adlandırın *Blogging.Migrations.Startup*, tıklatıp **Tamam**.

* Proje Başvuru Ekle *Blogging.Migrations.Startup* için proje *Blogging.Model* proje.

Şimdi ilk geçiş oluşturabilirsiniz.

* **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**

* Seçin *Blogging.Model* proje olarak **varsayılan proje**.

* İçinde **Çözüm Gezgini**ayarlayın *Blogging.Migrations.Startup* projeyi başlangıç projesi olarak.

* Çalıştırma `Add-Migration InitialCreate`.

  Bu komut, tablolar, modelinize için başlangıç kümesi oluşturan bir geçiş iskele oluşturulduğunu.

## <a name="create-the-database-on-app-startup"></a>Uygulama başlangıcında veritabanı oluşturma

Uygulamanın üzerinde çalıştığı cihazda oluşturulacak veritabanının istediğinden, uygulama başlangıcından yerel veritabanında bekleyen tüm geçişler uygulamak için kod ekleyin. Uygulama çalışır, ilk kez bu yerel veritabanı oluşturmayı ilgileniriz.

* Proje Başvuru Ekle *Blogging.UWP* için proje *Blogging.Model* proje.

* Açık *App.xaml.cs*.

* Bekleyen tüm geçişler uygulamak için vurgulanmış kodu ekleyin.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> Modelinizi değiştirmek kullanırsanız `Add-Migration` karşılık gelen uygulamak için yeni bir geçiş iskele komut, veritabanına değiştirir. Uygulama başladığında bekleyen tüm geçişlerde, her cihazda yerel veritabanına uygulanır.
>
>EF kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulanmış izlemek için veritabanı tablosunda.

## <a name="use-the-model"></a>Kullanım modeli

Şimdi, veri erişimi gerçekleştirdiği modeli kullanabilirsiniz.

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
