---
title: .NET Framework - yeni veritabanı - EF Core üzerinde çalışmaya başlama
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 088ac915041489242eb8090e7bf3a2bdc8036534
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614434"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>.NET Framework ile yeni bir veritabanı üzerinde EF Core ile çalışmaya başlama

Bu öğreticide, Entity Framework kullanarak bir Microsoft SQL Server veritabanında temel veri erişimi gerçekleştirdiği bir konsol uygulaması oluşturun. Bir modelden veritabanı oluşturmaya geçişleri kullanın.

[Bu makaledeki örnek Github'da görüntüle](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017 sürüm 15.7 veya üzeri](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

* Açık Visual Studio 2017

* **Dosya > Yeni > Proje...**

* Sol menüden **yüklü > Visual C# > Windows Masaüstü**

* Seçin **konsol uygulaması (.NET Framework)** proje şablonu

* Emin olun projenizin hedeflediği **.NET Framework 4.6.1** veya üzeri

* Projeyi adlandırın *ConsoleApp.NewDb* tıklatıp **Tamam**

## <a name="install-entity-framework"></a>Entity Framework'ü yükleme

EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin. Bu öğreticide, SQL Server kullanır. Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).

* Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu

* `Install-Package Microsoft.EntityFrameworkCore.SqlServer`'i çalıştırın.

Bu öğreticide daha sonra veritabanını korumak için bazı Entity Framework araçları kullanın. Bu nedenle de araçları paketini yükleyin.

* `Install-Package Microsoft.EntityFrameworkCore.Tools`'i çalıştırın.

## <a name="create-the-model"></a>Model oluşturma

Artık modeli oluşturan bir bağlam ve varlık sınıflarını tanımlamak için zamanı geldi.

* **Proje > sınıfı Ekle...**

* Girin *Model.cs* tıklayın ve adı olarak **Tamam**

* Dosyanın içeriğini aşağıdaki kodla değiştirin.

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> Gerçek bir uygulamada ayrı bir dosyada her sınıf koyun ve bağlantı dizesini bir yapılandırma dosyası veya ortam değişkeninde yerleştirin. Basitleştirmek amacıyla, her şey Bu öğretici için bir tek bir kod dosyasında tutmaktır.

## <a name="create-the-database"></a>Veritabanı oluşturma

Bir modeliniz olduğuna göre bir veritabanı oluşturmaya geçişleri kullanabilirsiniz.

* **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**

* Çalıştırma `Add-Migration InitialCreate` tablo modeli için başlangıç kümesi oluşturmak için bir geçiş iskele.

* Çalıştırma `Update-Database` veritabanına yeni geçiş uygulamak için. Veritabanı henüz mevcut olmadığından geçiş uygulanmadan önce oluşturulur.

> [!TIP]  
> Modele değişiklik yaparsanız, kullanabileceğiniz `Add-Migration` karşılık gelen şema yapmak için yeni bir geçiş iskele komut, veritabanına değiştirir. Gerekli iskele kurulmuş kod iade (ve gerekli değişiklikleri yaptıktan sonra), kullanabileceğiniz `Update-Database` veritabanına değişiklikleri uygulamak için komutu.
>
> EF kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulanmış izlemek için veritabanı tablosunda.

## <a name="use-the-model"></a>Kullanım modeli

Şimdi, veri erişimi gerçekleştirdiği modeli kullanabilirsiniz.

* Açık *Program.cs*

* Dosyanın içeriğini aşağıdaki kodla değiştirin.

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* **Hata ayıklama > hata ayıklama olmadan Başlat**

  Bir blog veritabanına kaydedilir ve daha sonra tüm blogları ayrıntılarını konsola yazdırılır görürsünüz.

  ![görüntü](_static/output-new-db.png)

## <a name="additional-resources"></a>Ek Kaynaklar

* [Mevcut bir veritabanı ile .NET Framework üzerinde EF Core](xref:core/get-started/full-dotnet/existing-db)
* [EF Core ile yeni bir veritabanı - SQLite .NET core'da](xref:core/get-started/netcore/new-db-sqlite) -platformlar arası konsol EF öğretici.
