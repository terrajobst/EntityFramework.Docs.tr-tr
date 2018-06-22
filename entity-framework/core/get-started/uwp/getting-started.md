---
title: UWP - yeni veritabanı - EF Çekirdeğinde Başlarken
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
ms.locfileid: "26054816"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Evrensel Windows Platformu (UWP) yeni bir veritabanı EF Çekirdeğinde ile çalışmaya başlama

> [!NOTE]
> Bu öğretici EF çekirdek 2.0.1 (ASP.NET Core ve .NET Core SDK 2.0.3 yayımlanan) kullanır. EF çekirdek 2.0.0 iyi bir UWP deneyimi için gereken bazı önemli hata düzeltmeleri eksik.

Bu kılavuzda, Entity Framework kullanarak yerel bir SQLite veritabanı karşı temel veri erişimi gerçekleştirdiği bir evrensel Windows Platformu (UWP) uygulaması oluşturacaksınız.

> [!IMPORTANT]
> **Anonim türler UWP LINQ sorgularında önleme göz önünde bulundurun**. Bir UWP uygulamasını Uygulama mağazası dağıtılması .NET yerel ile derlenecek uygulamanız gerekir. Anonim türler sorgularıyla daha zayıf bir performans yerel .NET vardır.

> [!TIP]
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) github'da.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki öğeler bu yönlendirmeyi tamamlamak için gereklidir:

* [Windows 10 sonbaharda oluşturucuları güncelleştirmesi](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)

* [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.4 veya ile sonraki bir sürümü **Evrensel Windows platformu geliştirme** iş yükü.

## <a name="create-a-new-model-project"></a>Yeni bir modeli projesi oluşturma

> [!WARNING]
> Model Paket Yöneticisi konsolunda geçişler komutları çalıştırılabilmesi için UWP olmayan projesinde yerleştirilmesi gerekir UWP projeleri etkileşimde şekilde .NET Core araçlarında sınırlamaları nedeniyle

* Açık Visual Studio

* Dosya > Yeni > Proje...

* Sol menüden şablonları seçin > Visual C#

* Seçin **sınıf kitaplığı (.NET standart)** proje şablonu

* Proje bir ad verin ve tıklatın **Tamam**

## <a name="install-entity-framework"></a>Entity Framework'ü yükleme

EF çekirdek kullanmak için hedeflemek istediğiniz veritabanı sağlayıcı(lar) için paketini yükleyin. Bu kılavuzda SQLite kullanılır. Kullanılabilir sağlayıcılar listesi için bkz: [veritabanı sağlayıcıları](../../providers/index.md).

* Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu

* Çalıştırma`Install-Package Microsoft.EntityFrameworkCore.Sqlite`

Bu kılavuzda daha sonra biz de bazı Entity Framework Araçları veritabanını korumak için kullanır. Böylece biz de araçları paketini yükler.

* Çalıştırma`Install-Package Microsoft.EntityFrameworkCore.Tools`

* .Csproj dosyasını düzenleyin ve değiştirme `<TargetFramework>netstandard2.0</TargetFramework>` ile`<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`

## <a name="create-your-model"></a>Model oluşturma

Şimdi modelinizi yapmak bağlamını ve varlık sınıflarını tanımlamak için zaman yapılır.

* Proje > sınıfı Ekle...

* Girin *Model.cs* tıklatın ve adı olarak **Tamam**

* Dosyasının içeriğini aşağıdaki kodla değiştirin

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Yeni bir UWP projesi oluşturma

* Açık Visual Studio

* Dosya > Yeni > Proje...

* Sol menüden şablonları seçin > Visual C# > Windows Evrensel

* Seçin **boş uygulama (Evrensel Windows)** proje şablonu

* Proje bir ad verin ve tıklatın **Tamam**

* Hedef ve en düşük sürümler için en az ayarlayın`Windows 10 Fall Creators Update (10.0; build 16299.0)`

## <a name="create-your-database"></a>Veritabanı oluşturma

Bir model sahip olduğunuza göre sizin için bir veritabanı oluşturmak için geçiş kullanabilirsiniz.

* Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu

* Modeli projesi varsayılan proje olarak seçin ve başlangıç projesi olarak ayarla

* Çalıştırma `Add-Migration MyFirstMigration` tabloları modeliniz için ilk kümesi oluşturmak için bir geçiş için iskele kurmak.

Uygulamanın çalıştığı cihaz üzerinde oluşturulacak veritabanına istiyoruz olduğundan, uygulama başlatma yerel veritabanında bekleyen tüm geçişleri uygulamak için bazı kod ekleyeceğiz. Uygulama çalıştığında, ilk kez bu bize için yerel veritabanı oluşturmayı ilgilenebilmek.

* Sağ **App.xaml** içinde **Çözüm Gezgini** seçip **görünümü kodu**

* Vurgulanan kullanma dosyasının başlangıcına ekleyin

* Bekleyen tüm geçişleri uygulamak için vurgulanmış kodu ekleyin

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> Modelinize gelecekteki değişiklikler yapmak isterseniz, kullanabileceğiniz `Add-Migration` karşılık gelen uygulamak için yeni bir geçiş için iskele kurmak komut veritabanına değiştirir. Uygulama başladığında bekleyen tüm geçişleri her cihazda yerel veritabanı uygulanır.
>
>EF kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulandı izlemek için veritabanı tablosunda.

## <a name="use-your-model"></a>Modelinizi kullanın

Veri erişimi gerçekleştirdiği modelinizi artık kullanabilirsiniz.

* Açık *MainPage.xaml*

* Aşağıda vurgulanan UI içerik ve sayfa yükleme işleyici ekleme

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

UI veritabanıyla kablo kod artık ekleyeceğiz

* Sağ **MainPage.xaml** içinde **Çözüm Gezgini** seçip **görünümü kodu**

* Aşağıdaki listeden vurgulanmış kodu ekleyin

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

Şimdi eylemde görmek için uygulamayı çalıştırabilirsiniz.

* Hata ayıklama > hata ayıklama olmadan Başlat

* Uygulama oluşturma ve başlatma

* Bir URL girin ve tıklayın **Ekle** düğmesi

![görüntü](_static/create.png)

![görüntü](_static/list.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!TIP]
> `SaveChanges()`Performans geliştirilmiş uygulayarak [ `INotifyPropertyChanged` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [ `INotifyPropertyChanging` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [ `INotifyCollectionChanged` ](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) , varlık türleri ve kullanarak `ChangeTrackingStrategy.ChangingAndChangedNotifications`.

Tada! Entity Framework çalışan basit bir UWP uygulamasında artık sahipsiniz.

Entity Framework'ün özellikler hakkında daha fazla bilgi edinmek için bu belgeleri içindeki diğer makalelere göz atın.
